<properties
    pageTitle="Automatske obogaćeni koncentratora za Azure obavijesti"
    description="Saznajte kako slanje obogaćenog automatske obavijesti na aplikaciju iOS iz Azure. Primjere koda pisane cilj C i C#."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Automatske obogaćeni koncentratora za Azure obavijesti


##<a name="overview"></a>Pregled

Da bi se sudjelovati korisnici s izravnim obogaćenim sadržaj aplikacije možda htjeti automatske osim običnog teksta. Ove obavijesti Promicanje interakcija korisnika i izlaganje sadržaja kao što su URL-ovi zvukova, slika/kupona, itd. Pomoću ovog praktičnog vodiča sastavlja o temi [Obavijestite korisnike](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , a služi za slanje automatske obavijesti uključivanje payloads (na primjer, slika).


Pomoću ovog praktičnog vodiča kompatibilan sa sustavom iOS 7 i 8.

  ![][IOS1]

Visoke razine:

1. Pozadinska aplikacija:
    - Sprema obogaćenog opseg (u ovom slučaju slika) u pozadinske baze podataka i lokalne prostora za pohranu
    - Šalje ID obogaćenog obavijest na uređaju
2. Aplikacija na uređaju:
    - Kontakti pozadinskog obogaćenog opseg s ID-om primi zahtjev
    - Šalje i obavijesti korisnici na uređaju kada dovršite dohvaćanje podataka i prikazuje opseg odmah nakon korisnika dodirnite da biste saznali više


## <a name="webapi-project"></a>WebAPI projekta

1. U Visual Studio, otvorite projekt **AppBackend** koji ste stvorili u ovom praktičnom vodiču [Obavijestite korisnike](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .
2. Nabavite sliku želite obavijestiti korisnike s te ih u mapi **img** u direktoriju projekta.
3. Kliknite **Prikaži sve datoteke** u pregledniku rješenja, a desnom tipkom miša kliknite mapu **uključiti u**projekt.
4. Sa slikom odaberete, promijenite njegov akciju sastavljanje u prozoru svojstva **Ugrađene resursa**.

    ![][IOS2]

5. U **Notifications.cs**, dodajte sljedeće pomoću naredbe:

        using System.Reflection;

6. Ažurirajte cijeli predmet **obavijesti** o sljedeći kod. Ne zaboravite rezerviranih mjesta zamijenite vaše vjerodajnice koncentrator obavijesti i naziv grafičke datoteke.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (neobavezno) Pogledajte [kako ugraditi i pristup resursima pomoću Visual C#](http://support.microsoft.com/kb/319292) dodatne informacije o dodavanju i dobiti resursima za projekt.

7. U **NotificationsController.cs**redefinirati njihove **NotificationsController** sa sljedećim isječci. Šalje ID-ove početne tihu obogaćenog obavijesti uređaj i omogućuje klijentsko dohvaćanja slike:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Sada ćemo će ponovno implementirati ovu aplikaciju programa Azure web-mjesta da bi ga čine pristupačnim na svim uređajima. Desnom tipkom miša kliknite na projektu **AppBackend** , a zatim odaberite **Objavi**.

9. Odaberite Azure web-mjesto kao vaš cilj Objavi. Prijavite se pomoću računa za Azure, odaberite na web-mjesto s postojeći ili novi i zabilježite **Odredišni URL** svojstva na kartici **veze** . Će nazivamo ovaj URL *pozadinskog krajnje točke* u nastavku ovog praktičnog vodiča. Kliknite **Objavi**.

## <a name="modify-the-ios-project"></a>Izmjena projekta za iOS

Sad kad ste izmijenili aplikacije pozadinskog sustava da biste poslali samo *id* obavijest, promijenit će se iOS aplikacije za tog ID-a, a dohvatiti obogaćenog poruku iz svoje pozadine.

1. Otvaranje projekta iOS i omogućite udaljene obavijesti tako da vaš cilj glavni aplikacije u odjeljku **ciljnih web-mjesta** .

2. Kliknite **Mogućnosti**, uključite **Načini pozadine**i potvrdite okvir **Udaljene obavijesti** .

    ![][IOS3]

3. Idite na **Main.storyboard**pa provjerite je li prikaz kontroleru (refered da biste kao Home prikaz kontroler ovog praktičnog vodiča) iz vodiča [Obavijesti korisnika](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .

4. Dodajte **Kontroler navigacije** na scenarija i kontrole povlačenjem Home kontroler prikaz da biste **Prikaz korijen** navigacije. Obavezno **Je početni prikaz kontroler** u atribute kontrola za navigaciju kontrolerom samo.

5. Dodajte **Kontroler prikaz** ploče scenarija i dodali **Prikaz slike**. To je stranica koje će korisnici vidjeti kada odabrati više klikom na u notifiication. Vaš scenarija trebao bi izgledati ovako:

    ![][IOS4]

6. Kliknite **Polazno prikaz kontroler** u scenarija i provjerite je li **homeViewController** kao njegov **Prilagođeno predmete** i **ID scenarija** je u odjeljku Provjera identiteta.

7. Isto učinite i za kontroler za prikaz slike kao **imageViewController**.

8. Zatim stvorite novi prikaz kontroler klase pod naslovom **imageViewController** učiniti korisničkog Sučelja koji ste upravo stvorili.

9. U **imageViewController.h**, dodajte sljedeće deklaracije s kontrolerom sučelja. Provjerite jeste li kontrola i povucite iz prikaza slike scenarija u tim svojstvima za povezivanje dva:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. U **imageViewController.m**na kraju **viewDidload**dodajte sljedeće:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. U **AppDelegate.m**uvesti kontrolerom slike koju ste stvorili:

        #import "imageViewController.h"

12. Dodajte odjeljak sučelja pomoću sljedećih izvješća:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. U **AppDelegate**, provjerite je li vaša aplikacija registrira za tihu obavijesti u **aplikacije: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Subsitute u sljedeće implementaciji za **aplikaciju: didRegisterForRemoteNotificationsWithDeviceToken** da bi se scenarija korisničkog Sučelja mijenja se u obzir:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Zatim dodajte na sljedeće načine **AppDelegate.m** Dohvati sliku iz na krajnjoj točki i poslali obavijest o lokalnom nakon dovršetka dohvaćanja. Provjerite jeste li zamijeniti rezervirano mjesto `{backend endpoint}` s na krajnjoj točki pozadinskog:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Rukovanje lokalne obavijesti iznad otvaranjem gore kontrolerom prikaz slika u **AppDelegate.m** pomoću sljedećih načina:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Pokrenite aplikaciju

1. U XCode, pokrenite aplikaciju na uređaju fizičke iOS (automatske obavijesti neće funkcionirati u simulator).

2. U aplikaciji za iOS korisničkog Sučelja unesite korisničko ime i lozinku za iste vrijednosti za provjeru autentičnosti i kliknite **Prijava**.

3. Kliknite **Pošalji automatske** i trebali biste vidjeti u aplikaciji upozorenja. Ako kliknete na **više**će biti uvezeni sa slikom odaberete da biste obuhvatili vaše pozadinska aplikacija.

4. Možete kliknuti i **Slanje automatskih** i odmah pritisnite gumb Polazno uređaja. U trenutak, primit ćete obavijest automatske. Ako je dodirnite ili kliknite više, će biti na aplikaciju i sadržaja obogaćenog slike.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
