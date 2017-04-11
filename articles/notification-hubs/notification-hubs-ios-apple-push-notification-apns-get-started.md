<properties
    pageTitle="Automatske obavijesti da pošaljete iOS uz Azure obavijesti koncentratora | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču Saznajte kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti aplikacije sustava iOS."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="automatske obavijesti, slanje obavijesti ios automatske obavijesti"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Automatske obavijesti da pošaljete iOS uz Azure obavijesti koncentratora

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Pregled

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Pomoću ovog praktičnog vodiča pokazuje kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti aplikaciju iOS. Stvorit ćete aplikaciju za prazna iOS koja prima slanje obavijesti za [servis za Apple automatske obavijesti (APN-ove)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Kada završite, ćete moći koristiti koncentratora za obavijesti za emitiranje automatske obavijesti na svim uređajima s instaliranim programom aplikacije.

## <a name="before-you-begin"></a>Prije početka

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dovršeni kod za ovaj vodič možete pronaći [na GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ [Mobilna usluga iOS SDK verzija 1.2.4]
+ Najnoviju verziju [Xcode]
+ IOS 8 (ili novija verzija) – instaliranih uređaja
+ [Program za razvojne inženjere za Apple](https://developer.apple.com/programs/) članstvo.

   > [AZURE.NOTE] Zbog konfiguracije preduvjeti za slanje obavijesti, morate uvesti i testiranje automatske obavijesti na uređaju fizičke iOS (iPhone ili iPad) umjesto iOS Simulator.

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve obavijesti koncentratora vodiče za iOS aplikacije.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfiguriranje koncentratora za obavijesti za iOS automatske obavijesti

U ovom se odjeljku vodit će vas kroz stvaranje nove koncentrator obavijesti i konfiguriranje provjere autentičnosti s APN-ove pomoću certifikata automatske **.p12** koji ste stvorili. Ako želite koristiti obavijesti koncentratora koje ste već stvorili, možete preskočiti na 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Kliknite gumb <b>Servisa obavijesti</b> u plohu <b>Postavke</b> , a zatim odaberite <b>Apple (APN-ove)</b>. Kliknite <b>Prijenos certifikat</b> i odaberite <b>.p12</b> datoteku koju ste ranije izvezli. Provjerite je li navedete ispravnu lozinku.</p>
<p>Provjerite je li za odabir načinu rada <b>s memorijom za testiranje</b> Budući da je to za razvoj. <b>Radni</b> koristiti samo ako želite poslati slanje obavijesti za korisnike koji su kupili aplikacije iz trgovine.</p>
</li>
</ol>
&emsp;&emsp;![Konfiguriranje APNS Portal za Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Konfiguriranje certifikata za APN-ove portalu Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Koncentratora za obavijesti sada je konfiguriran za APN-ove, a imate nizove veze da biste registrirali aplikacije i Pošalji automatske obavijesti.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Povezivanje aplikaciju za iOS s koncentratorima obavijesti

1. U Xcode, stvorite novi projekt iOS i odaberite predložak **Jedan prikaz aplikacije** .

    ![Xcode - jednom prikazu aplikacije][8]

2. Kada postavite mogućnosti za novi projekt, provjerite je li koristiti isti **Naziv proizvoda** i **Identifikator tvrtke ili ustanove** koji ste koristili kada ste prethodno postavili ID paket portala za razvojne inženjere Apple.

    ![Xcode - mogućnosti projekta][11]

3. U odjeljku **ciljnih web-mjesta**, kliknite naziv projekta, kliknite karticu **Postavke sastavljanje** proširite **Identiteta potpisivanje koda**i zatim u odjeljku **ispravljanje pogrešaka**, postavite svoj identitet potpisivanje koda. Uključivanje/isključivanje **razine** od **jednostavnih** do **svih**i postavite **Profila za dodjelu resursa** za dodjelu resursa profila koji ste prethodno stvorili.

    Ako ne vidite novi profil za dodjelu resursa koji ste stvorili u Xcode, pokušajte osvježite profila za potpisivanje identitet. Na traci izbornika kliknite **Xcode** kliknite **Postavke**, kliknite karticu **račun** , kliknite gumb **Prikaz detalja** kliknite potpisivanja identiteta, a zatim gumb Osvježi u donjem desnom kutu.

    ![Xcode - profilu za dodjelu resursa][9]

4. Preuzmite [mobilne usluge iOS SDK verziju 1.2.4] i raspakirajte datoteku. U Xcode, desnom tipkom miša kliknite projekt, a zatim kliknite mogućnost **Dodavanje datoteke** da biste dodali mapu **WindowsAzureMessaging.framework** Xcode projekta. Odaberite **Kopiraj stavke po potrebi**, a zatim kliknite **Dodaj**.

    >[AZURE.NOTE] Obavijest o koncentratora SDK trenutno ne podržava bitcode na Xcode 7.  **Omogućivanje Bitcode** morate postaviti na **bez** u **Mogućnosti stvaranja** projekta.

    ![Raspakiraj Azure SDK][10]

5. Dodajte novu datoteku zaglavlja u projekt pod nazivom `HubInfo.h`. Datoteka će se spremati konstante za koncentratora za obavijesti.  Dodajte sljedeće definicije i zamjena rezerviranih mjesta za doslovni niz *koncentrator naziv* i *DefaultListenSharedAccessSignature* koju ste ranije zabilježili.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Otvaranje vaše `AppDelegate.h` datoteke dodajte sljedeće upute poslužitelja za uvoz:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. U vašem `AppDelegate.m file`, dodajte sljedeći kôd u na `didFinishLaunchingWithOptions` način ovisno o verziji sustava iOS. Kod registrira ručicu za uređaj s APN-ove:

    Za iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    Za iOS verzije prije 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. U istoj datoteci, dodajte na sljedeće načine. Kod povezuje koncentrator obavijesti koristeći podatke o vezi koju ste naveli u HubInfo.h. Zatim pruža token uređaj koncentrator obavijesti da bi središte obavijesti možete poslati obavijesti:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. U istu datoteku, dodajte sljedeći način za prikaz **UIAlert** ako obavijesti primitku dok je aktivna mogućnost aplikaciju:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Stvaranje i pokretanje aplikacija na uređaju da biste provjerili ima li bez pogrešaka.

## <a name="send-test-push-notifications"></a>Slanje test automatske obavijesti


Možete testirati primanje obavijesti u svojoj aplikaciji slanjem automatske obavijesti [Azure Portal] putem u odjeljku **Otklanjanje poteškoća** plohu koncentrator (koristite mogućnost *Testiranje slanje* ).

![Azure portala - Test Pošalji][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Neobavezno) Pošalji automatske obavijesti iz aplikacije

>[AZURE.IMPORTANT] U ovom se primjeru slanja obavijesti iz aplikacije klijenta je namijenjeno učenju samo. Budući da to je potrebno u `DefaultFullSharedAccessSignature` da u aplikaciju za klijenta, izlaže koncentratora za obavijesti da biste rizik da korisnik može ostvariti pristup da biste poslali neovlašteno obavijesti o klijentima.

Ako želite poslati automatske obavijesti iz aplikacije Ovo poglavlje sadrži primjera kako to učiniti pomoću sučelja OSTALE.

1. Xcode, otvorite `Main.storyboard` i iz biblioteke objekt da biste korisniku omogućuje slanje automatske obavijesti u aplikaciji dodajte sljedeće komponente korisničkog Sučelja:

    - Natpis s nikakav tekst natpisa. Će se koristiti pogrešaka u slanja obavijesti. Svojstvo **Reci** se treba postavljeno na **0** tako da će automatski veličina ograničeno na desnoj strani i prema lijevoj margini i vrhu prikaza.
    - Tekstno polje sa **zamjenskim** tekstom postavite **Unesite poruku**. Ograničiti polje neposredno ispod natpisa, kao što je prikazano u nastavku. Postavite prikaz kontroler kao delegat utičnicu.
    - Gumb pod naslovom **Slanje obavijesti** ograničeno neposredno ispod tekstno polje i u centru za vodoravno.

    Prikaz trebao bi izgledati ovako:

    ![Dizajner Xcode][32]


2. [Tvorničke trgovine dodavanje](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) natpisa i tekstnog polja povezani svoj prikaz i ažuriranje vaše `interface` definicija za podršku `UITextFieldDelegate` i `NSXMLParserDelegate`. Dodajte deklaracija tri svojstvo prikazano u nastavku da biste pomoći s podrškom za pozivanje REST API-JA i analize odgovor.

    ViewController.h datoteke trebao bi izgledati ovako:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Otvaranje `HubInfo.h` i dodavanje konstante koji će se koristiti za slanje obavijesti o vašem koncentrator. Doslovni niz rezerviranog mjesta zamijenite stvarni *DefaultFullSharedAccessSignature* nizu za povezivanje.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Dodajte sljedeće `#import` naredbi za vaše `ViewController.h` datoteku.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. U `ViewController.m` dodati sljedeći kod implementacije sučelja. Kod će raščlaniti niz za povezivanje *DefaultFullSharedAccessSignature* . Kao što je rečeno [REST API -JA referenca](http://msdn.microsoft.com/library/azure/dn495627.aspx), te rastavljeni informacije će se koristiti za generiranje SaS token za zaglavlje zahtjev za **provjeru autentičnosti** .

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. U `ViewController.m`, ažuriranje na `viewDidLoad` način raščlaniti niz za povezivanje prilikom učitavanja prikaza. Dodati utility načine, prikazano u nastavku, u implementaciju sučelja.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. U `ViewController.m`, dodajte sljedeći kôd u implementaciju sučelje za generiranje SaS ovlaštenja koje će se posluživati u zaglavlju **autorizacije** kao što je rečeno [REST API Reference](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + povucite s **Slanje obavijesti** gumb `ViewController.m` da biste dodali akciju pod nazivom **SendNotificationMessage** za događaj **Dodira dolje** . Ažurirati način sljedeći kod za slanje obavijesti pomoću REST API-JA.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. U `ViewController.m`, dodajte sljedeće delegat način za podršku zatvaranje tipkovnice za tekstno polje. CTRL + povucite s tekstno polje na ikonu prikaz kontroler u dizajneru sučelja da biste postavili kontrolerom prikaz kao delegat utičnicu.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. U `ViewController.m`, dodavanje sljedećih metoda delegata za podršku Raščlanjivanje odgovor pomoću `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Stvaranje projekta i provjerite postoje li bez pogrešaka.


> [AZURE.NOTE] Ako se pojavi pogreška Sastavi u Xcode7 o podršci za bitcode, trebali biste promijeniti **Postavke za sastavljanje** > **Omogućiti Bitcode (ENABLE_BITCODE)** na **bez** u Xcode. SDK koncentratora obavijesti trenutno ne podržava bitcode. 

Možete pronaći sve moguće obavijesti payloads u jabuka [lokalne i automatske obavijesti vodiču za programiranje].


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Provjera ako aplikacije primajte automatske obavijesti

Da biste testirali automatske obavijesti na iOS, morate implementirati aplikaciju na uređaj fizički iOS. Apple automatske obavijesti ne možete poslati pomoću iOS Simulator.

1. Pokrenite aplikaciju i potvrda uspješnog Registracija, a zatim pritisnite **u redu**.

    ![iOS aplikacije automatske obavijesti Registracija Test][33]

2. Automatske obavijesti test možete poslati s [Portala za Azure]prethodno opisan. Ako ste dodali kod za slanje automatskih obavijesti u aplikaciji, dodirnite unutar tekstnog polja za unos poruke s obavijesti. Pritisnite gumb **Pošalji** na tipkovnici ili gumb za **Slanje obavijesti** u prikazu za slanje poruke s obavijesti.

    ![iOS aplikacije automatske obavijesti poslati Test][34]

3. Automatske obavijesti se šalje na svim uređajima registrirane za primanje obavijesti iz centra za određeni obavijesti.

    ![iOS aplikacije automatske obavijesti primanje Test][35]


##<a name="next-steps"></a>Daljnji koraci

U ovom primjeru jednostavne raspoređena automatske obavijesti na svim uređajima registrirani iOS. Predlažemo da kao sljedeći korak u naučeno nastavite na [Azure obavijesti koncentratora obavijestite korisnike za iOS s pozadinskom .NET] Praktični vodič koji će vas voditi kroz stvaranje pozadinskog da biste poslali automatske obavijesti pomoću oznaka. 

Ako želite fazi korisnika po grupama kamata možete dodatno premjestiti na Praktični vodič za [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] . 

Opće informacije o obavijesti koncentratora potražite u članku [Smjernice koncentratora obavijesti].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobilna usluga iOS SDK verzija 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Upute za koncentratora obavijesti]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure obavijesti koncentratora obavijestite korisnike za iOS s pozadinskom .NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Lokalni i automatske obavijesti vodič za programiranje]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Portal za Azure]: https://portal.azure.com