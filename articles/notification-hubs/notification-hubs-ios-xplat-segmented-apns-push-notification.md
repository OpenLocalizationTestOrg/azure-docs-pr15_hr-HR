<properties
    pageTitle="Obavijest o koncentratora prijelom novosti Praktični vodič – iOS"
    description="Saznajte kako koristiti Azure servisa Bus obavijesti koncentratora za slanje obavijesti o novostima prijelom za uređaje sa sustavom iOS."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Korištenje koncentratora obavijesti da biste poslali važnih vijesti

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Pregled

U ovoj se temi objašnjava da biste koristili Azure obavijesti koncentratora za emitiranje prijelom novosti obavijesti na aplikaciju iOS. Po dovršetku će moći Registrirajte se za najnovije vijesti kategorije koje vas zanimaju te primati samo slanje obavijesti za te kategorije. Taj se scenarij uobičajeni obrazac za mnoge aplikacije gdje se obavijesti imaju slati grupe korisnika koje ste prethodno deklarirane kamate na njima, npr RSS čitač, aplikacije za sporta glazbu, itd.

Scenariji za emitiranje omogućeni uključivanjem jednu ili više _oznaka_ prilikom stvaranja je registracija u središtu obavijesti. Kada se šalju obavijesti o oznaci, sve uređaje koje ste registrirali za oznaku će primiti obavijest. Budući da oznake jednostavno nizove, ne moraju biti unaprijed dodjeli. Dodatne informacije o oznake, pogledajte [usmjeravanje koncentratora obavijesti i oznaka izraza](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Preduvjeti

U ovoj se temi sastavlja u aplikaciju koju ste stvorili u [Početak rada s obavijesti koncentratora][get-started]. Prije pokretanja ovog praktičnog vodiča, morate već dovršite [Početak rada s obavijesti koncentratora][get-started].

##<a name="add-category-selection-to-the-app"></a>Dodavanje kategorija odabira u aplikaciju

U prvi je korak da biste dodali elemente korisničkog Sučelja u svoje postojeće scenarija koji korisniku omogućuju odaberite kategorije da biste registrirali. Kategorije koji je odabrao korisnik pohranjuju se na uređaj. Prilikom pokretanja aplikacije Registracija uređaja se stvara u koncentratora za obavijesti s odabrane kategorije kao oznake.

1. U vašem MainStoryboard_iPhone.storyboard dodajte sljedeće komponente iz biblioteka objekata:
    + Natpis "Najnovije vijesti" tekstom
    + Naljepnice, sa tekstova kategorija "Svijeta", "Politika", "Tvrtke", "Tehnologije", "Znanstvenog", "Sportovi"
    + Šest parametri jedna po kategoriji, postavite svaki parametar **Stanje** da bi **se po zadanom** .
    + Jedan gumb s oznakom "Pretplati"

    Vaš scenarija trebao bi izgledati ovako:

    ![][3]

2. U uređivaču Pomoćnik za stvaranje tvorničke trgovine za sve skretnice i poziva "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Stvaranje akcije na gumb pod nazivom "pretplata". Vaše ViewController.h mora sadržavati sljedeće:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Stvaranje nove **Predmete dodira Cocoa** pod nazivom `Notifications`. U odjeljku sučelja datoteke Notifications.h kopirajte sljedeći kod:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Dodajte sljedećih smjernica uvoz Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Kopirajte sljedeći kod u odjeljku implementaciju datoteke Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Klase koristi lokalno spremište za pohranu i dohvaćanje kategorije novosti koja će primiti ovaj uređaj. Osim toga, sadrži način da biste registrirali za te kategorije pomoću [predloška](notification-hubs-templates-cross-platform-push-messages.md) registracije.

7. U datoteci AppDelegate.h dodajte naredbu programa uvoza za Notifications.h i dodavanje svojstva za instanca klase obavijesti:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. U metodu **didFinishLaunchingWithOptions** AppDelegate.m dodavanje koda za inicijalizaciju instancu obavijesti na početku načina.  
 
    `HUBNAME`i `HUBLISTENACCESS` (definirano u hubinfo.h) mora već imate u `<hub name>` i `<connection string with listen access>` rezerviranih mjesta zamjenjuje naziva koncentrator obavijesti i niz za povezivanje za *DefaultListenSharedAccessSignature* koji ste nabavili neke starije verzije

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Budući da su vjerodajnice koje su distributed s klijentskom aplikacijom obično sigurne, samo distribucija ključ za pristup preslušavanja s aplikacijom klijenta. Poslušajte omogućuje pristup ne može se mijenjati aplikaciju za registraciju na obavijesti, ali postojeće registracije i ne može se obavijesti poslati. Puni pristup ključ se koristi u zaštićenim pozadinskog servisa za slanje obavijesti i promjene postojećih registracije.


9. U metodu **didRegisterForRemoteNotificationsWithDeviceToken** AppDelegate.m zamijenite kod u način sljedeći kod za prosljeđivanje token uređaj predmete obavijesti. Predmet obavijesti o će izvesti registracije za obavijesti s kategorijama. Ako korisnik promijeni odabire kategorija, ne možemo poziva na `subscribeWithCategories` metoda kao odgovor na gumb **Pretplata** da biste ih ažurirati.

    > [AZURE.NOTE] Budući da token uređaju dodijeljena tako da na Apple automatske obavijesti servisa (APN-ove) chance možete u bilo kojem trenutku, trebala bi se registrirati za obavijesti često da biste izbjegli pogreške obavijesti. U ovom se primjeru registrira za obavijesti prilikom svakog pokretanja aplikacije. Za aplikacije koje se često više puta dan, vjerojatno preskočite Registracija da biste sačuvali propusnosti Ako manji od jednog dana prošlo od prethodnog registracije.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Imajte na umu da sada postoje treba bez koda u način **didRegisterForRemoteNotificationsWithDeviceToken** .

10. Na sljedeće načine trebalo bi biti prisutne u AppDelegate.m dovršavanje [Početak rada s obavijesti koncentratora] [ get-started] vodič.  Ako nije, dodajte ih.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Ta metoda rukuje obavijesti primili kada aplikacija je pokrenut prikazom jednostavne **UIAlert**.

11. U ViewController.m, dodajte naredbu uvoza za AppDelegate.h i kopirajte sljedeći kod u način XCode generira **Pretplata** . Kod ažurirat će se Registracija obavijesti da biste koristili nove oznake kategorija korisnik odabere u korisničkom sučelju.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Ovaj postupak stvara **NSMutableArray** kategorija i koristi predmete **obavijesti** za pohranu na popisu u lokalno spremište i registrira odgovarajuće oznaka s koncentratora za obavijesti. Kada se mijenjaju se kategorije, Registracija je ponovno stvoriti pomoću novih kategorija.

12. U ViewController.m, dodajte sljedeći kod u **viewDidLoad** način da biste postavili korisničko sučelje na temelju prethodno spremljene kategorija.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Aplikaciju možete skup kategorije pohraniti u lokalno spremište uređaj koristi za registriranje središtu obavijesti pri svakom pokretanju aplikaciju.  Korisnik možete promijeniti odabir kategorije prilikom izvođenja i kliknite željeni način **Pretplata** da biste ažurirali Registracija uređaja. Nakon toga će se ažurirati aplikaciju za slanje obavijesti novosti prijelom izravno u samoj aplikaciji.


##<a name="optional-sending-tagged-notifications"></a>(neobavezno) Slanje obavijesti označenog

Ako nemate pristup Visual Studio, preskočite na sljedeći odjeljak i slanje obavijesti iz samoj aplikaciji. Možete slati obavijesti proper predložak s [Azure klasični Portal] putem kartice za ispravljanje pogrešaka za koncentratora za obavijesti. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(neobavezno) Slanje obavijesti s uređaja

Obično bi se obavijesti poslati u skladu s pozadinskom usluga, ali prijelom novosti obavijesti možete poslati izravno iz aplikacije. Da biste to ćemo ažurirati na `SendNotificationRESTAPI` način na koji ste definirali u [Početak rada s obavijesti koncentratora] [ get-started] vodič.


1. U ViewController.m ažuriranje na `SendNotificationRESTAPI` metodi kao slijedi tako da prihvaća parametar oznake kategorija i da bi šalje obavijest proper [predložak](notification-hubs-templates-cross-platform-push-messages.md) .

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

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



2. U ViewController.m ažurirati akcija **Slanje obavijesti** , kao što je prikazano u kodu koji slijedi. Da bi ga će slanje obavijesti pojedinačno pomoću svakoj oznaci i poslati više platformama.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Ponovno stvaranje projekta i provjerite zadovoljavate li bez pogrešaka Sastavi.


##<a name="run-the-app-and-generate-notifications"></a>Pokrenite aplikaciju i generiranje obavijesti

1. Pritisnite gumb Pokreni omogućuje stvaranje projekta i pokrenite aplikaciju. Odabir određene mogućnosti novosti prijelom se pretplatiti na i zatim pritisnite gumb **Pretplati se** . Prikazat će se dijaloški okvir koji označava obavijesti pretplaćenih.

    ![][1]

    Kad odaberete **pretplati**, aplikaciju pretvara odabrane kategorije u oznake i zahtjeve novi Registracija uređaja za odabrane oznake u središtu obavijesti.

2. Unesite poruku da biste se i poslati kao važnih vijesti, a zatim pritisnite gumb za **Slanje obavijesti** . Umjesto toga pokrenuti aplikaciju .NET konzole za generiranje obavijesti.

    ![][2]


3. Svakom uređaju pretplaćeni na najnovije vijesti će primiti obavijesti novosti prijelom ste upravo poslali.



## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču smo naučili kako emitiranje važnih vijesti po kategoriji. Imajte na umu dovršavanje neku od sljedeće vodiči za kojima se ističu druge napredne scenarije koncentratora obavijesti:

+ **[Korištenje obavijesti koncentratora za emitiranje lokalizirane važnih vijesti]**

    Saznajte kako da biste proširili aplikaciju prijelom novosti da biste omogućili slanje lokalizirane obavijesti.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Korištenje obavijesti koncentratora za emitiranje lokalizirane važnih vijesti]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure klasični Portal]: https://manage.windowsazure.com
