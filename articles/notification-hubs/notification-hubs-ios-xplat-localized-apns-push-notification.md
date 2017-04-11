<properties
    pageTitle="Obavijest o koncentratora lokalizirani najnovije vijesti Praktični vodič za iOS"
    description="Saznajte kako koristiti Azure servisa Bus obavijesti koncentratora za slanje obavijesti novosti lokalizirane prijelom (iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Koristite koncentratora obavijesti da biste poslali lokalizirane važnih vijesti za uređaje sa sustavom iOS

> [AZURE.SELECTOR]
- [C# iz Windows trgovine](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Pregled

U ovoj se temi objašnjava pomoću značajke [predložaka](notification-hubs-templates-cross-platform-push-messages.md) programa Azure obavijesti koncentratora za emitiranje prijelom novosti obavijesti koji imaju omogućeno lokalizirani jezika i uređaja. U ovom ćete praktičnom vodiču počet ćete aplikaciju iOS stvorene u [Koristi koncentratora obavijesti da biste poslali važnih vijesti]. Kada završi, moći ćete se registrirati za kategorije koje vas zanimaju, navedite jezik u kojem želite primati obavijesti i primati samo proslijeđenih obavijesti za odabrane kategorije na tom jeziku.


Postoje dva dijela scenarij:

- aplikaciju za iOS omogućuje klijentski uređaji za određivanje jezika, a da biste se pretplatili kategorije novosti različite prijelom;

- pozadinske odašilje obavijesti, korištenje **oznaka** i **predložak** feautres od koncentratora Azure obavijesti.



##<a name="prerequisites"></a>Preduvjeti

Mora već dovršite vodič za [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] , a kod dostupna, jer ovog praktičnog vodiča nadovezuje izravno kod.

Visual Studio 2012 ili noviji nije obavezno.



##<a name="template-concepts"></a>Predložak koncepti

U [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] ugrađeni aplikaciju koja koriste **oznake** Pretplaćivanje na obavijesti za kategorije različitih novosti.
Mnoge aplikacije, međutim, ciljani više tržištima i potrebna lokalizaciju. To znači da se sadržaj obavijesti sami morati lokalizirani i isporučuju pravilan skup uređaja.
U ovoj temi Pokazat ćemo kako koristiti **predložak** značajka obavijesti koncentratora jednostavno izlaganje obavijesti o novostima lokalizirane prijelom.

Napomena: jedan za slanje obavijesti lokalizirane tako da biste stvorili više verzija sustava svakoj oznaci. Ako, primjerice, za podršku engleski, francuski i Mandarinski, ne možemo potrebni tri različite oznake svijeta novosti: "world_en", "world_fr" i "world_ch". Zatim ćemo promijenile slanje lokalizirane verzije novosti svijeta za svaki od tih oznaka. U ovoj temi koristimo predlošci da biste izbjegli proliferation oznake i obavezne slanja više poruka.

Visoke razine Predlošci su način da biste odredili kako određeni uređaj trebale primiti obavijest. Predložak određuje oblik točno tereta tako da upućuju na svojstva koja su dio poruku poslao vaše aplikacije pozadinskih. U našem slučaju smo poslat će regionalnu shemu agnostic poruku koja sadrži sve jezike:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Zatim ćemo će osigurati uređaji registrirate predložak koji se odnosi na odgovarajuće svojstvo. Ako, primjerice, aplikaciju iOS koja želi da biste registrirali francuski novosti će registrirati sljedeće:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Predlošci su vrlo napredne značajke mogu saznati više o u našem članku [Predlošci](notification-hubs-templates-cross-platform-push-messages.md) .

##<a name="the-app-user-interface"></a>Korisničkom sučelju aplikacije

Ne možemo sada izmijeniti najnovije vijesti aplikaciju koju ste stvorili u članku [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] da biste poslali lokalizirane važnih vijesti pomoću predložaka.


U MainStoryboard_iPhone.storyboard, dodajte segmentiranog kontrole s tri jezika koji će se podržati: engleski, francuski i Mandarinski.

![][13]

Pazite da biste dodali programa IBOutlet u vašem ViewController.h kao što je prikazano u nastavku:

![][14]

##<a name="building-the-ios-app"></a>Stvaranje aplikacije za iOS


1. U vašem Notification.h dodajte metodu *retrieveLocale* i izmjena spremište i pretplata načine, kao što je prikazano u nastavku:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    U Notification.m, izmijenili metodu *storeCategoriesAndSubscribe* dodavanjem parametara regionalne sheme i spremanje u korisničke zadane vrijednosti:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Zatim izmijenite metodu *Pretplata* će se uvrstiti u regionalnim postavkama:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Imajte na umu kako ćemo sada koristite način *registerTemplateWithDeviceToken*umjesto *registerNativeWithDeviceToken*. Kada smo registrirate za predložak imamo pružaju json predloška i naziv za predložak (kao što je naša aplikacija preporučuje se da biste registrirali različite predloške). Provjerite da biste registrirali vaše kategorije kao oznake, kao što je želimo da biste bili sigurni da biste primali notifciations te novosti.

    Dodavanje načina za dohvaćanje regionalnu shemu iz zadane postavke korisnika:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Nakon što smo izmijenio naš predmete obavijesti, imamo provjerite je li naše ViewController omogućuje korištenje novog UISegmentControl. Dodajte sljedeći redak u način *viewDidLoad* da biste bili sigurni da bi se prikazala regionalnoj shemi koju trenutno odabrana:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Nakon toga u način *Pretplata* Promijeni poziva *storeCategoriesAndSubscribe* sljedeće:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Naposljetku, morate ažurirati način *didRegisterForRemoteNotificationsWithDeviceToken* u AppDelegate.m, tako da možete pravilno osvježiti svoju registraciju prilikom pokretanja aplikacije. Promjena poziva na način *Pretplata* obavijesti o sljedećim preporukama:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(neobavezno) Slanje obavijesti lokalizirane predloška iz aplikacije konzole za .NET.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(neobavezno) Slanje obavijesti lokalizirani predložak s uređaja

Ako nemate pristup Visual Studio, ili samo želite testirati slanja obavijesti lokalizirane predložak izravno iz aplikacije na uređaju.  Možete jednostavno dodati parametre lokalizirane predložak u `SendNotificationRESTAPI` metoda koje ste definirali u prethodnom izvješću.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju predložaka potražite u članku:

- [Obavijestiti korisnike s koncentratorima obavijest: platforme ASP.NET]
- [Obavijestiti korisnike s koncentratorima obavijest: mobilne usluge]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Obavijestiti korisnike s koncentratorima obavijest: platforme ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Obavijestiti korisnike s koncentratorima obavijest: mobilne usluge]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
