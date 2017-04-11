<properties
    pageTitle="Početak rada s Azure Mobile radnje za iOS u cilj C | Microsoft Azure"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za iOS aplikacije."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Početak rada s Azure Mobile radnje za iOS aplikacije u cilj C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi objašnjava da biste koristili Azure Mobile radnje za razumijevanje Upotreba aplikacije i automatske obavijesti poslati segmentiranog korisnicima aplikaciju sustava iOS.
U ovom ćete praktičnom vodiču stvorite aplikaciju za prazna iOS koja prikuplja osnovne podatke i prima automatske obavijesti pomoću Apple automatske obavijesti sustava (APN-ove).

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ 8 XCode koje možete instalirati iz MAC App Store
+ [iOS Mobile radnje SDK]

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve radnje Mobile vodiče za iOS aplikacije.

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Postavljanje mobilnih radnje za aplikaciju za iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni Integracija", što je minimalnim potrebne za prikupljanje podataka i slanje automatske obavijesti. Potpuna Integracija dokumentaciju pronaći ćete u [Mobile radnje iOS SDK Integracija](mobile-engagement-ios-sdk-overview.md)

Ne možemo će stvoriti osnovni aplikacije s XCode da bismo pokazali integraciju sa servisom.

###<a name="create-a-new-ios-project"></a>Stvaranje novog projekta iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

1. Preuzmite [Mobile radnje iOS SDK].
2. Izdvajanje na. tar.gz datoteku u mapu na vašem računalu.
3. Desnom tipkom miša kliknite projekt, a zatim odaberite **Dodaj datoteke**.

    ![][1]

4. Dođite do mape u koju ste izdvojili SDK, odaberite na `EngagementSDK` mapu, a zatim pritisnite **u redu**.

    ![][2]

5. Otvorite karticu **Sastavljanje faze** pa na izborniku **Binarni s biblioteka veza** Dodaj okviri kao što je prikazano u nastavku:

    ![][3]

6. Vratite se u Azure portal na stranici vaše aplikacije **Veze informacije** i kopirajte niz za povezivanje.

    ![][4]

7. Dodajte sljedeći redak koda u datoteci **AppDelegate.m** .

        #import "EngagementAgent.h"

8. Zalijepiti u nizu za povezivanje s `didFinishLaunchingWithOptions` delegiranje.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`je na neobavezno iskaz koji omogućuje SDK zapisnika prepoznati problema. 

##<a id="monitor"></a>Omogućiti praćenje u stvarnom vremenu

Da biste pokrenuli slanje podataka i provjera jesu li korisnici aktivna, pozadinski Mobile radnje mora poslati barem jedan zaslon (aktivnosti).

1. Otvorite datoteku **ViewController.h** i uvoz **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Sada zamijeniti super klase sučelja **ViewController** po `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Radnje Mobile omogućuje interakciju s korisnicima i DOSEGNE s automatske obavijesti i u aplikaciji poruka u kontekstu kampanja. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
U sljedećim se odjeljcima Postavljanje aplikacije primati.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Omogućivanje aplikacije za primanje tihu automatske obavijesti

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>Dodavanje biblioteke razgovor u projekt

1. Desnom tipkom miša kliknite projekt.
2. Odaberite **Dodaj datoteku koju želite.**
3. Dođite do mape u koju ste izdvojili SDK-a.
4. Odaberite na `EngagementReach` mapu.
5. Kliknite **Dodaj**.

### <a name="modify-your-application-delegate"></a>Izmjena svog delegata aplikacije

1. Vratite se u **AppDeletegate.m** datoteka, Uvoz modul dosegne radnje.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Unutar na `application:didFinishLaunchingWithOptions` način, stvorite modul razgovor i prenesite na postojeći redak radnje pokretanje:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Omogućivanje aplikacije za primanje APN-ove automatske obavijesti

1. Dodajte na sljedeći redak u `application:didFinishLaunchingWithOptions` metoda:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Dodavanje u `application:didRegisterForRemoteNotificationsWithDeviceToken` metoda na sljedeći način:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Dodavanje u `didFailToRegisterForRemoteNotificationsWithError` metoda na sljedeći način:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Dodavanje u `didReceiveRemoteNotification:fetchCompletionHandler` metoda na sljedeći način:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Korištenje mobilne iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

