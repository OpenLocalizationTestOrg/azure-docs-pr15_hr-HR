<properties
    pageTitle="Početak rada s Azure Mobile radnje za iOS u Swift | Microsoft Azure"
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
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Početak rada s Azure Mobile radnje za iOS aplikacije u Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi objašnjava da biste koristili Azure Mobile radnje za razumijevanje Upotreba aplikacije i automatske obavijesti poslati segmentirani korisnicima aplikaciju sustava iOS.
U ovom ćete praktičnom vodiču stvorite aplikaciju za prazna iOS koja prikuplja osnovne podatke i prima automatske obavijesti pomoću Apple automatske obavijesti sustava (APN-ove).

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ 8 XCode koje možete instalirati iz MAC App Store
+ [iOS Mobile radnje SDK]
+ Automatske obavijesti certifikat (.p12) možete dobiti na Razvojni centar za Apple

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi Swift verzije 3.0. 

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve radnje Mobile vodiče za iOS aplikacije.

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Postavljanje mobilnih radnje za aplikaciju za iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni Integracija", što je minimalnim potrebne za prikupljanje podataka i slanje automatske obavijesti. Potpuna Integracija dokumentaciju pronaći ćete u [Mobile radnje iOS SDK Integracija](mobile-engagement-ios-sdk-overview.md)

S XCode da bismo pokazali integraciju sa servisom ćemo će stvoriti osnovni aplikacije:

###<a name="create-a-new-ios-project"></a>Stvaranje novog projekta iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

1. Preuzimanje [Mobile radnje iOS SDK]
2. Izdvajanje na. tar.gz datoteku u mapu na računalu
3. Desnom tipkom miša kliknite projekt, a zatim odaberite "Dodavali da biste..."

    ![][1]

4. Dođite do mape u koju ste izdvojili SDK i odaberite na `EngagementSDK` mapu, a zatim kliknite u redu.

    ![][2]

5. Otvaranje u `Build Phases` kartica i na `Link Binary With Libraries` izbornika Dodavanje okviri kao što je prikazano u nastavku:

    ![][3]

8. Stvaranje Bridging zaglavlje da biste mogli koristiti u SDK cilj C API-ji tako da odaberete Datoteka > novo > datoteke > iOS > izvor > datoteka zaglavlja.

    ![][4]

9. Uređivanje datoteke da biste otkrili kod mobilnog radnje cilj-C da biste Swift šifru, dodajte sljedeće uvozi bridging zaglavlja:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. U odjeljku postavke za sastavljanje provjerite je li zaglavlje premošćivanja cilj C sastavljanje postavku u odjeljku Swift alata za Kompiliranje - generiranje koda sadrži put do tog zaglavlja. Evo primjera put: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (ovisno o put)**

    ![][6]

11. Vratite se u Azure portal na stranici vaše aplikacije *Veze informacije* i kopirajte niz za povezivanje

    ![][5]

12. Zalijepiti u nizu za povezivanje s `didFinishLaunchingWithOptions` delegiranje

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Omogućivanje nadzora u stvarnom vremenu

Da biste pokrenuli slanje podataka i provjera jesu li korisnici aktivna, pozadinski Mobile radnje mora poslati barem jedan zaslon (aktivnosti).

1. Otvorite datoteku **ViewController.swift** i zamijeni osnovni klasa **ViewController** biti **EngagementViewController**:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Radnje Mobile omogućuje interakciju i DOSEGNE s korisnicima s automatske obavijesti i razmjena poruka u aplikaciji u kontekstu kampanja. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
U sljedećim se odjeljcima će se instalacija aplikacije primati.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Omogućivanje aplikacije za primanje tihu automatske obavijesti

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Dodavanje biblioteke razgovor u projekt

1. Desnom tipkom miša kliknite projekta
2. Odaberite`Add file to ...`
3. Dođite do mape u koju ste izdvojili SDK-a
4. Odaberite na `EngagementReach` mape
5. Kliknite Dodaj
6. Uređivanje datoteke da biste otkrili Mobile radnje cilj C dosegne zaglavlja i dodajte sljedeće uvozi bridging zaglavlja:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Izmjena svog delegata aplikacije

1. Unutar na `didFinishLaunchingWithOptions` – stvaranje modula razgovor i prenesite na postojeći redak radnje pokretanje:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Omogućivanje aplikacije za primanje APN-ove automatske obavijesti
1. Dodajte na sljedeći redak u `didFinishLaunchingWithOptions` metoda:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Dodavanje u `didRegisterForRemoteNotificationsWithDeviceToken` metoda na sljedeći način:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Dodavanje u `didReceiveRemoteNotification:fetchCompletionHandler:` metoda na sljedeći način:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Korištenje mobilne iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
