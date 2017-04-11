<properties
    pageTitle="Automatske koncentratora Azure obavijesti sigurna"
    description="Saznajte kako slanje sigurne automatske obavijesti na aplikaciju iOS iz Azure. Primjere koda pisane cilj C i C#."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Automatske obavijesti Azure koncentratora sigurne

> [AZURE.SELECTOR]
- [Univerzalni za Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Pregled

Podrška za slanje obavijesti u Microsoft Azure omogućuje vam da biste pristupili infrastrukture jednostavno koristi multiplatform, skalirana izlaz automatske koji za komercijalni ispis olakšava implementacije proslijeđenih obavijesti za korisnika i enterprise aplikacije za mobilne platformama.

Zbog propisima ili sigurnosnih ograničenja, ponekad aplikacije možda htjeti uključiti nešto u obavijesti koja se ne možete prenositi putem Infrastruktura standardne automatske obavijesti. Pomoću ovog praktičnog vodiča opisuje način da biste postigli iste sučelje slanjem povjerljivih podataka putem sigurne, čija je autentičnost provjerena veze između klijentskog uređaja i pozadinska aplikacija.

Visoke razine tijeka je kako slijedi:

1. Aplikacija pozadinske:
    - Služi za pohranu sigurne tereta u pozadinskoj bazi podataka.
    - Šalje ID obavijest uređaj (sigurne ne šalju nikakvi podaci).
2. Aplikacija na uređaju, prilikom primanja obavijesti:
    - Uređaj kontakta pozadinske zahtijeva sigurnu opseg.
    - Aplikaciju možete prikazati opseg kao obavijesti na uređaju.

Važno Imajte na umu da u prethodnom tijeku (i u ovom ćete praktičnom vodiču), pretpostavimo da uređaj pohranjuje token za provjeru autentičnosti u lokalno spremište kada se korisnik prijavljuje je. Time će se jamčiti potpuno objedinjenog doživljaj, kao što je uređaja možete dohvatiti na obavijest sigurno tereta pomoću ovaj token. Ako aplikacija pohranite tokeni za provjeru autentičnosti na uređaj ili ako je istekao tokena aplikaciju uređaj nakon prima obavijest prikazati generički obavijesti postavljanja upita korisniku pokrenite aplikaciju. Aplikaciju pa potvrđuje korisnika i prikazuje opseg obavijesti.

Pomoću ovog praktičnog vodiča sigurne automatske pokazuje kako sigurno Pošalji automatske obavijesti. Vodič sastavlja na vodič [Obavijestiti korisnike](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) da bi se trebali biste najprije dovršite korake u tom praktičnom vodiču.

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča pretpostavlja da ste stvorili i konfigurirali koncentratora za obavijesti, kao što je opisano u [Početak rada s obavijesti koncentratora (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Izmjena projekta za iOS

Sad kad koju ste izmijenili vaše aplikacije pozadinskih da biste poslali samo *id* obavijest, morate promijeniti iOS aplikacije za tu obavijest, a pozvati vaše pozadinske dohvatiti sigurnu poruku da se prikazuje.

Da biste postigli cilja, imamo pisanje logike za dohvaćanje sigurnosti sadržaja iz aplikacije pozadinske.

1. U **AppDelegate.m**, provjerite je li Registri aplikacije za tihu obavijesti da bi se obrađuje id obavijesti šalje pozadinski. Mogućnost **UIRemoteNotificationTypeNewsstandContentAvailability** dodatka didFinishLaunchingWithOptions:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. U **AppDelegate.m** dodajte odjeljak implementaciju pri vrhu pomoću sljedećih izvješća:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Dodaj u odjeljku implementaciju sljedeći kod zamjenjujući rezervirano mjesto `{back-end endpoint}` s krajnja točka za vaše pozadinske nabavili prethodno:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Sad imamo obrađuje dolazne obavijesti i korištenje gore navedeni način za dohvaćanje sadržaja za prikaz. Najprije Trebamo da biste omogućili iOS aplikacije da biste pokrenuli u pozadini prilikom primanja automatske obavijesti. U **XCode**, odaberite aplikaciju projekta u lijevom oknu za, a zatim kliknite vaš cilj glavni aplikacije u odjeljku **ciljnih web-mjesta** iz okna središnje.

5. Zatim kliknite na kartici **Mogućnosti** pri vrhu okna središnje, a zatim potvrdite okvir **Udaljene obavijesti** .

    ![][IOS1]


6. U **AppDelegate.m** dodajte sljedeće način rukovanja automatske obavijesti:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Imajte na umu da se preporučuje za rukovanje slučajeva nedostaje svojstvo zaglavlju provjera autentičnosti ili odbijanje po pozadinske. Posebno rukovanje tim slučajevima ovise o tome uglavnom vaš cilj korisničkog sučelja. Jedan je prikazati obavijest s generički upit za korisnika za provjeru autentičnosti za dohvaćanje stvarni obavijesti.

## <a name="run-the-application"></a>Pokrenite aplikaciju

Da biste pokrenuli aplikaciju, učinite sljedeće:

1. U XCode, pokrenite aplikaciju na uređaju fizičke iOS (automatske obavijesti neće funkcionirati u simulator).

2. U aplikaciji za iOS korisničkog Sučelja unesite korisničko ime i lozinku. To može biti bilo koji niz znakova, ali moraju biti iste vrijednosti.

3. U aplikaciji za iOS korisničkog Sučelja kliknite **prijavite se**. Zatim kliknite **Pošalji automatske**. Trebali biste vidjeti sigurne obavijesti koja se prikazuje u centru za obavijesti.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
