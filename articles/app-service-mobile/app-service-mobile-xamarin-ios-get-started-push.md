<properties
    pageTitle="Automatske obavijesti dodati aplikacije Xamarin.iOS sa servisom Azure aplikacije"
    description="Saznajte kako koristiti aplikacije servisa za Azure automatske obavijesti poslati Xamarin.iOS aplikacije"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Aplikaciju za Xamarin.iOS dodati automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Pregled

U ovom ćete praktičnom vodiču dodate automatske obavijesti projekt [Xamarin.iOS Brzi start](app-service-mobile-xamarin-ios-get-started.md) tako da se automatske obavijesti poslati na uređaju svaki put kada se umetne zapis.

Ako ne koristite project server preuzete brzi početak rada, morate paket za nastavak automatske obavijesti. Dodatne informacije potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Preduvjeti

* Dovršite vodič za [brzi početak rada Xamarin.iOS](app-service-mobile-xamarin-ios-get-started.md) .

* Uređaj fizički iOS. Automatske obavijesti ne podržava iOS simulator.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registracija aplikacije za slanje obavijesti na Appleova portala za razvojne inženjere

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Konfiguriranje mobilne aplikacije za slanje automatskih obavijesti

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Ažuriranje project server da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Konfiguriranje Xamarin.iOS projekta

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Automatske obavijesti dodati aplikacije

1. U **QSTodoService**, dodajte sljedeće svojstvo tako da se **AppDelegate** možete nabaviti mobilnu verziju klijentskog:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Dodajte sljedeće `using` izjava na vrh **AppDelegate.cs** datoteku.

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. U **AppDelegate**nadjačati **FinishedLaunching** događaja:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. U istoj datoteci nadjačati **RegisteredForRemoteNotifications** događaj. U kod registriranja za jednostavne predložak obavijesti koja će se slati preko sve podržane platforme poslužitelj.

    Dodatne informacije o predlošcima s koncentratorima obavijesti potražite u članku [Predlošci](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Nakon toga nadjačati **DidReceivedRemoteNotification** događaja:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Pokrenite aplikaciju sada ažurirati podržava automatske obavijesti.

## <a name="test"></a>Testiranje automatske obavijesti u aplikaciji

1. Pritisnite gumb **Pokreni** omogućuje stvaranje projekta i pokrenite aplikaciju na koje je moguće povezati uređaju sa sustavom iOS, a zatim kliknite **u redu** da biste prihvatili automatske obavijesti.

    > [AZURE.NOTE] Morate izričito prihvatili automatske obavijesti iz aplikacije. Ovaj zahtjev pojavljuje se samo prvi put da se pokreće aplikaciju.

2. U aplikaciji, upišite zadatak, a zatim kliknite znak plus (**+**) ikona.

3. Provjerite je li se prima obavijest, a zatim **u redu** da biste odbacili obavijest.

4. Ponovite korak 2 i odmah zatvorite aplikaciju, a zatim provjerite je li prikazuje se obavijest.

Uspješno ste dovršili ovog praktičnog vodiča.

<!-- Images. -->

<!-- URLs. -->



