<properties
    pageTitle="Azure Mobile radnje iOS pregled SDK | Microsoft Azure"
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

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK za Azure Mobile radnje

Da biste vidjeli sve detalje o tome kako integrirati Azure Mobile radnje u aplikaciji iOS, počnite ovdje. Ako želite isprobajte sami prvo, provjerite je li to naš [vodič 15 minuta](mobile-engagement-ios-get-started.md).

Kliknite da biste vidjeli na [SDK sadržaja](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Integracija postupaka
1. Počnite ovdje: [integraciji korištenje mobilne aplikacije za iOS](mobile-engagement-ios-integrate-engagement.md)

2. Za obavijesti: [Integraciji razgovor (obavijesti) u aplikaciju za iOS](mobile-engagement-ios-integrate-engagement-reach.md)

3. Označavanje Planiranje implementacije: [kako koristiti dodatne radnje Mobile označavanje API-JA u aplikaciju za iOS](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Napomene

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   FIXED obavijesti ne actioned na uređajima sa sustavom iOS 10.
-   Zastarijevanje XCode 7.

Starije verzije potražite [Dovršeno napomene](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Nadogradnja postupaka

Ako već imate integrirali stariju verziju radnje u aplikaciji, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Možda ćete morati slijedite nekoliko postupaka ako Propušteni nekoliko verzija pogledajte SDK dovršavanje [Nadogradnje postupke](mobile-engagement-ios-upgrade-procedure.md).

Za svaku novu verziju SDK morate najprije zamijeniti (uklanjanje i ponovno uvezli u xcode) EngagementSDK i EngagementReach mape.

###<a name="from-300-to-400"></a>Iz 3.0.0 za 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 je obavezna počevši od verzije 4.0.0 SDK.

> [AZURE.NOTE] Ako ste doista ovise o XCode 7 možda koristi [iOS v3.2.4 SDK radnje](https://aka.ms/r6oouh). Ima li poznatih problema na razgovor modul ove prethodne verzije prilikom pokretanja na uređajima sa sustavom iOS 10: obavijesti sustava nisu actioned. Da biste to ispravili morat ćete provesti zastarjeli API `application:didReceiveRemoteNotification:` u svojoj aplikaciji delegatu na sljedeći način:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Ne preporučujemo to zaobilazno rješenje** kao takvo ponašanje možete promijeniti u verziju nadogradnje nadolazeće iOS (čak i pomoćne) jer je zastario iOS API-JA. Trebate prebaciti na XCode 8 čim.

#### <a name="usernotifications-framework"></a>UserNotifications framework
Najprije morate dodati na `UserNotifications` framework u vašem sastavljanje faze.

u programa project explorer mrežu projekta i odaberite odgovarajuće cilj. Zatim otvorite karticu **"Sastavljanje faze"** i na izborniku **"binarni s biblioteka veza"** dodavanje framework `UserNotifications.framework` – postavljanje kao veza`Optional`

#### <a name="application-push-capability"></a>Mogućnost automatske aplikacije
XCode 8 možete ponovno postaviti aplikacije automatske mogućnost, ponovno ga dvaput provjerite na `capability` kartica odabrani ciljani.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Dodavanje novog Registracija koda za iOS 10 obavijesti
Stariji koda da biste registrirali aplikaciju da biste obavijesti i dalje funkcionira, ali koristi zastarjeli API-ji prilikom pokretanja na iOS 10. 

Uvoz u `User Notification` okvir:

        #import <UserNotifications/UserNotifications.h>

U ovlaštenike aplikacije `application:didFinishLaunchingWithOptions` Zamijeni metoda:

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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Ako već imate UNUserNotificationCenterDelegate implementaciju

SDK ima vlastitu implementaciju UNUserNotificationCenterDelegate protokol. Koristi se tako da u SDK praćenje životnog ciklusa obavijesti o mogućnostima na uređajima sa sustavom na iOS 10 ili noviji. Ako u SDK otkrije svog delegata ga će koristiti vlastitu implementaciju jer se može biti samo jedna UNUserNotificationCenter delegat svaku aplikaciju. To znači da će imati logike radnje vlastite delegatu.

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

> [AZURE.NOTE] Možete odrediti hoće li obavijest dolazi s mogućnostima ili ne prosljeđivanjem njegov `userInfo` rječnik agenta `isEngagementPushPayload:` klase način.