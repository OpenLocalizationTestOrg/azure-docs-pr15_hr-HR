<properties
    pageTitle="Registriranje trenutnog korisnika za slanje obavijesti putem Web API | Microsoft Azure"
    description="Saznajte kako da biste zatražili automatske obavijesti registracija u aplikaciju iOS s koncentratorima Azure obavijesti kada je registeration obavlja API servisa platforme ASP.NET Web."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Dnevnik trenutnog korisnika za slanje obavijesti putem platforme ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Pregled

U ovoj se temi objašnjava da biste zatražili automatske obavijesti Registracija s koncentratorima Azure obavijesti prilikom registracije ne izvodi API servisa platforme ASP.NET Web. U ovoj se temi proširuje vodič [obavijesti korisnici s koncentratorima obavijesti]. Mora već dovršite potrebne korake u tom Praktični vodič da biste stvorili čija je autentičnost provjerena servis za mobilne uređaje. Dodatne informacije o korisnicima scenarij obavijesti potražite u članku [obavijesti korisnici s koncentratorima obavijesti].

##<a name="update-your-app"></a>Ažuriranje aplikacije  

1. U MainStoryboard_iPhone.storyboard, dodajte sljedeće komponente iz biblioteka objekata:

    + **Oznaka**: "Automatske korisniku s obavijesti koncentratora"
    + **Oznaka**: "InstallationId"
    + **Oznaka**: "Korisnik"
    + **Tekstno polje**: "Korisnik"
    + **Oznaka**: "Lozinku"
    + **Tekstno polje**: "Lozinku"
    + **Gumb**: "Prijavi"

    Sada na scenarija izgleda ovako:

    ![][0]

2. U uređivaču Pomoćnik za stvaranje tvorničke trgovine zamijenjeni kontrola i nazovite ih, povezivanje tekstna polja s kontrolerom prikaz (delegat) i stvaranje **Akcije** za gumb **Prijavi** .

    ![][1]

    Datoteku BreakingNewsViewController.h sada bi trebala sadržavati sljedeći kod:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Stvaranje klase pod nazivom **DeviceInfo**i kopirajte sljedeći kod u odjeljku sučelja datoteke DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. U odjeljku implementaciju DeviceInfo.m datoteke kopirajte sljedeći kod:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. U PushToUserAppDelegate.h, dodajte sljedeće jednočlana svojstvo:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. U metodu **didFinishLaunchingWithOptions** PushToUserAppDelegate.m dodati sljedeći kod:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    U prvom retku inicijalizira jednočlana **DeviceInfo** . U drugom retku pokreće registracije za automatsko prosljeđivanje obavijesti koja je već izlaganje su već obavili Praktični vodič za [Početak rada s koncentratorima obavijesti] .

9. U PushToUserAppDelegate.m, implementirati način **didRegisterForRemoteNotificationsWithDeviceToken** u vašem AppDelegate i dodati sljedeći kod:

        self.deviceInfo.deviceToken = deviceToken;

    Postavlja token uređaj za zahtjev.

    > [AZURE.NOTE] Sada ima ne smije biti ostale kodove u ovu metodu. Ako već imate poziv **registerNativeWithDeviceToken** način na koji je dodan kada završi Praktični vodič za [Početak rada s koncentratorima obavijesti](/manage/services/notification-hubs/get-started-notification-hubs-ios/) , morate iz komentara ili uklonite taj poziv.

10. U datoteci PushToUserAppDelegate.m dodajte sljedeće rukovatelj metoda:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Ta metoda prikazuje upozorenja u korisničkom Sučelju pri aplikacijom prima obavijesti o dok se izvodi.

9. Otvorite datoteku PushToUserViewController.m i vraćanje tipkovnice u sljedeće implementacije:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. U metodu **viewDidLoad** u datoteci PushToUserViewController.m Inicijalizacija oznaku installationId na sljedeći način:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. U sučelju u PushToUserViewController.m dodajte sljedeća svojstva:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Zatim dodajte sljedeće implementacije:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Kopirajte sljedeći kod u način rukovatelj **prijavu** stvorio XCode:

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Ovaj postupak dobiti poruku ID instalacije i kanal za slanje obavijesti i šalje, zajedno s vrste uređaja čija je autentičnost provjerena način Web API koji stvara u registracija u koncentratora obavijesti. Taj API Web je definirano u [obavijesti korisnici s koncentratorima obavijesti].

Sad kad aplikaciju klijent ažurirana, vratite se [obavijesti korisnici s koncentratorima obavijesti] i ažuriranje servis za slanje obavijesti putem koncentratora obavijesti za mobilne uređaje.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Obavijestiti korisnike s obavijesti koncentratora]: /manage/services/notification-hubs/notify-users-aspnet

[Početak rada s obavijesti koncentratora]: /manage/services/notification-hubs/get-started-notification-hubs-ios
