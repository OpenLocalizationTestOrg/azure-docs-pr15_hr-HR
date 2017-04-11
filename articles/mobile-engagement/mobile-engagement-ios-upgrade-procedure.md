<properties
    pageTitle="Azure Mobile radnje iOS postupak nadogradnje SDK | Microsoft Azure"
    description="Najnovija ažuriranja i postupke za iOS SDK za Azure Mobile radnje"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="upgrade-procedures"></a>Nadogradnja postupaka

Ako već imate integrirali stariju verziju radnje u aplikaciji, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Za svaku novu verziju SDK morate najprije zamijeniti (uklanjanje i ponovno uvezli u xcode) EngagementSDK i EngagementReach mape.

##<a name="from-300-to-400"></a>Iz 3.0.0 za 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 je obavezna počevši od verzije 4.0.0 SDK.

> [AZURE.NOTE] Ako ste doista ovise o XCode 7 možda koristi [iOS v3.2.4 SDK radnje](https://aka.ms/r6oouh). Ima li poznatih problema na razgovor modul ove prethodne verzije prilikom pokretanja na uređajima sa sustavom iOS 10: obavijesti sustava nisu actioned. Da biste to ispravili morat ćete provesti zastarjeli API `application:didReceiveRemoteNotification:` u svojoj aplikaciji delegatu na sljedeći način:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Ne preporučujemo to zaobilazno rješenje** kao takvo ponašanje možete promijeniti u verziju nadogradnje nadolazeće iOS (čak i pomoćne) jer je zastario iOS API-JA. Trebate prebaciti na XCode 8 čim.

### <a name="usernotifications-framework"></a>UserNotifications framework
Najprije morate dodati na `UserNotifications` framework u vašem sastavljanje faze.

u programa project explorer mrežu projekta i odaberite odgovarajuće cilj. Zatim otvorite karticu **"Sastavljanje faze"** i na izborniku **"binarni s biblioteka veza"** dodavanje framework `UserNotifications.framework` – postavljanje kao veza`Optional`

### <a name="application-push-capability"></a>Mogućnost automatske aplikacije
XCode 8 možete ponovno postaviti aplikacije automatske mogućnost, ponovno ga dvaput provjerite na `capability` kartica odabrani ciljani.

### <a name="add-the-new-ios-10-notification-registration-code"></a>Dodavanje novog Registracija koda za iOS 10 obavijesti
Stariji koda da biste registrirali aplikaciju da biste obavijesti i dalje funkcionira, ali koristi zastarjeli API-ji pokrenutim iOS 10.

Uvoz u `User Notification` okvir:

        #import <UserNotifications/UserNotifications.h> 

U vašem delegatu aplikacije `application:didFinishLaunchingWithOptions` Zamijeni metoda:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

po:

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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Ako već imate UNUserNotificationCenterDelegate implementaciju

SDK ima vlastitu implementaciju UNUserNotificationCenterDelegate protokol. Koristi se tako da u SDK praćenje životnog ciklusa obavijesti o mogućnostima na uređajima sa sustavom na iOS 10 ili noviji. Ako u SDK otkrije svog delegata ga će koristiti vlastitu implementaciju jer se može biti samo jedna UNUserNotificationCenter delegat svaku aplikaciju. To znači da ćete morati dodavanje logike radnje vlastite delegatu.

Da biste to postigli na dva načina.

Jednostavno po svog delegata za prosljeđivanje poziva SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Ili nasljeđuju na `AEUserNotificationHandler` klase

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Možete odrediti hoće li obavijest dolazi iz radnje ili ne prosljeđivanjem njegov `userInfo` rječnik agenta `isEngagementPushPayload:` klase način.

##<a name="from-200-to-300"></a>Iz 2.0.0 za 3.0.0
Podrška za iOS prekida 4.X. Počevši od ove verzije implementacije ciljne aplikacije mora biti najmanje iOS 6.

Ako koristite razgovor u aplikaciji, morate dodati `remote-notification` vrijednosti za na `UIBackgroundModes` polja u datoteci Info.plist da biste dobili udaljene obavijesti.

Način `application:didReceiveRemoteNotification:` treba zamijeniti `application:didReceiveRemoteNotification:fetchCompletionHandler:` u vašem delegatu aplikacije.

Ukinuta je "AEPushDelegate.h" sučelje i koje morate ukloniti sve reference. To obuhvaća uklanjanje `[[EngagementAgent shared] setPushDelegate:self]` i metode delegat iz svog delegata aplikacije:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Iz 1.16.0 za 2.0.0
Sljedeće opisuje kako migrirati programa SDK Integracija sa servisa Capptain u aplikaciju pokreće Azure Mobile radnje koje nudi Capptain SAS.
Ako premještate iz starije verzije, obratite se Capptain web-mjesta najprije migrirati 1.16, a zatim primijenite postupak u nastavku.

>[AZURE.IMPORTANT] Capptain i Mobile radnje nisu iste usluge i postupak dolje samo ističe migriranju aplikaciju klijenta. Migracija SDK u aplikaciji će nije moguće migrirati podatke s poslužitelja Capptain poslužiteljima Mobile radnje

### <a name="agent"></a>Agent

Način `registerApp:` zamijenjen je nov način `init:`. Vaš delegat aplikacije mora se ažurirati u skladu s tim i koristite niz za povezivanje:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Praćenje SmartAd uklonjeni su iz SDK samo morate ukloniti sve instance `AETrackModule` klase

### <a name="class-name-changes"></a>Promjena naziva klase

Kao dio sustava rebranding, postoji nekoliko nazivi klasa/datoteka koje je potrebno promijeniti.

Svi Tečajevi mjestu s "CP" preimenuju se s prefiksom "Lt".

Primjer:

-   `CPModule.h`preimenovana je u `AEModule.h`.

Svi Tečajevi mjestu s "Capptain" preimenuju se s prefiksom "Radnje".

Primjeri:

-   Klase `CapptainAgent` preimenovana je u `EngagementAgent`.
-   Klase `CapptainTableViewController` preimenovana je u `EngagementTableViewController`.
-   Klase `CapptainUtils` preimenovana je u `EngagementUtils`.
-   Klase `CapptainViewController` preimenovana je u `EngagementViewController`.
