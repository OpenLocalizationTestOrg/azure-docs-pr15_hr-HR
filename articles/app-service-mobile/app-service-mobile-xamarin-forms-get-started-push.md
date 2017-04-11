<properties
    pageTitle="Aplikaciju za Xamarin.Forms dodati automatske obavijesti | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure više platformu automatske obavijesti poslati Xamarin.Forms aplikacija."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Aplikaciju za Xamarin.Forms dodati automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Pregled

U ovom ćete praktičnom vodiču dodajte automatske obavijesti da biste svim projektima dobivena [Xamarin.Forms Brzi start](app-service-mobile-xamarin-forms-get-started.md) tako da se automatske se šalje obavijest da biste sve klijente različite platforme svaki put kada se umetne zapis.

Ako ne koristite project server preuzete brzi početak rada, morate paket za nastavak automatske obavijesti. Dodatne informacije potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Preduvjeti

* Za iOS, potrebno je [Program za razvojne inženjere za Apple članstva](https://developer.apple.com/programs/ios/) i uređaj fizički iOS jer [iOS simulator ne podržava slanje obavijesti](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Konfiguriranje obavijesti koncentratora

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Ažuriranje project server da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Neobavezno) Konfiguriranje i pokretanje Android projekta

Dovršite ovaj odjeljak da biste omogućili slanje obavijesti za projekt Xamarin.Forms Droid za Android.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Omogućivanje Firebase oblaka razmjenu poruka (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Konfiguriranje pozadinskog mobilnu aplikaciju slanje zahtjeva za automatske pomoću FCM

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Dodavanje automatske obavijesti Android projekt

S pozadinskom konfiguriran pomoću FCM, ne možemo možete dodati komponente kodovi za klijenta za registriranje FCM, Registrirajte se za slanje obavijesti s koncentratorima Azure obavijesti putem pozadinskog mobilne aplikacije i primanje obavijesti.

1. U programu project **Droid** desnom tipkom miša kliknite mapu **komponente** , kliknite **Dohvati dodatne komponente...**, potražite komponentu **Google Cloud poruka klijenta** i dodati u projekt. Komponenta podržava slanje obavijesti za projekt Xamarin Android.


2. Otvorite datoteku MainActivity.cs projekta i dodajte sljedeće pomoću izjave pri vrhu datoteke:

        using Gcm.Client;

3. Dodajte sljedeći kod metodu **OnCreate** nakon poziv na **LoadApplication**:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Nova metoda **CreateAndShowDialog** preglednika, dodajte na sljedeći način:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Dodajte sljedeći kod **MainActivity** klasa:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    To izlaže trenutnoj instanci **MainActivity** pa ćemo možete izvršiti na glavnom niti korisničkog Sučelja.

6. Pokretanje u `instance`, varijable na početku metodu **OnCreate** na sljedeći način.

        // Set the current instance of MainActivity.
        instance = this;

2. Dodajte novu datoteku klase **Droid** projekt pod nazivom `GcmService.cs`, te provjerite postoje sljedeće **pomoću** naredbe pri vrhu datoteke:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Pri vrhu datoteke, dodajte sljedeće zahtjeva za dozvole nakon **pomoću** naredbe i prije **prostora za naziv** izvješća.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Dodajte sljedeću definiciju klase naziva. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Zamijenite **< PROJECT_NUMBER >** vaš broj projekta ste ranije zabilježili.   

11. Zamijenite prazan klase **GcmService** sljedeći kod koji koristi novi emitiranje tekstnog okvira:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Dodajte sljedeći kod klase **GcmService** koji nadjačava rukovatelj događajima **OnRegistered** i implementira **registrirati** metode.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Dodajte sljedeći kod koji implementira **OnMessage**: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Obrađuje dolazne obavijesti i poslati ih upravitelju obavijesti da se prikazuje.

14. **GcmServiceBase** zahtijeva i implementirati rukovatelj metode **OnUnRegistered** i **OnError** koje možete učiniti na sljedeći način:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Sada ste spremni test automatske obavijesti u aplikaciji na uređaju sa sustavom Android ili na emulator.

###<a name="test-push-notifications-in-your-android-app"></a>Testiranje automatske obavijesti u aplikacija za Android

Prva dva koraka potrebni su samo kada testiranje na programa emulator.

1. Provjerite je li se za implementaciju u ili ispravljanje pogrešaka na virtualnog uređaja koji je Google API-ji Postavi kao cilj, kao što je prikazano u nastavku u upravitelju Android virtualne uređaja (AVD).

2. Dodajte Google račun Android uređaj tako da kliknete **aplikacije** > **Postavke** > **Dodavanje računa**, a zatim slijedite upute da biste koristili dodati postojeći račun za Google uređaj da biste stvorili novi.

1. U Visual Studio ili Xamarin Studio, desnom tipkom miša kliknite projekt **Droid** , a zatim kliknite **Postavi kao pokretanje projekta**.

2. Pritisnite gumb **Pokreni** omogućuje stvaranje projekta i pokretanje aplikacija na uređaju sa sustavom Android ili emulator.

3. U aplikaciji, upišite zadatak, a zatim kliknite znak plus (**+**) ikona.

4. Provjerite je li je primili obavijest kada dodate stavke.


##<a name="optional-configure-and-run-the-ios-project"></a>(Neobavezno) Konfiguriranje i pokretanje projekta za iOS

U ovom se odjeljku je za pokretanje projekta iOS Xamarin za uređaje sa sustavom iOS. Ako ne radite s uređajima sa sustavom iOS, preskočite ovaj odjeljak.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Konfiguriranje obavijesti koncentrator za APN-ove

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Sljedeće će konfigurirati postavke projekta iOS u Xamarin Studio ili Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Automatske obavijesti dodati aplikaciju za iOS

1. U programu project **iOS** otvorite AppDelegate.cs dodati naredbu za **pomoću** sljedeće na vrh kod datoteku.

        using Newtonsoft.Json.Linq;

4. U predmete **AppDelegate** dodajte na prekoračenje za događaj **RegisteredForRemoteNotifications** da biste registrirali za obavijesti:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. U **AppDelegate**i dodajte sljedeće nadjačavanje za rukovatelja događajima **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Ta metoda obrađuje dolazne obavijesti dok se izvodi aplikaciju.

2. U razredu **AppDelegate** dodati sljedeći kod **FinishedLaunching** metoda: 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    To omogućuje podrška za udaljene obavijesti i zahtjevi za automatske registracije.

Pokrenite aplikaciju sada ažurirati podržava automatske obavijesti.

####<a name="test-push-notifications-in-your-ios-app"></a>Testiranje automatske obavijesti u aplikaciju za iOS

1. Desnom tipkom miša kliknite projekt iOS, a zatim kliknite **Postavi kao StartPp projekta**.

2. Pritisnite gumb za **pokretanje** ili **F5** u Visual Studio omogućuje stvaranje projekta i pokretanje aplikacija na uređaju sa sustavom iOS, a zatim kliknite **u redu** da biste prihvatili automatske obavijesti.

    > [AZURE.NOTE] Morate izričito prihvatili automatske obavijesti iz aplikacije. Ovaj zahtjev pojavljuje se samo prvi put da se pokreće aplikaciju.

3. U aplikaciji, upišite zadatak, a zatim kliknite znak plus (**+**) ikona.

4. Provjerite je li se prima obavijest, a zatim **u redu** da biste odbacili obavijest.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Neobavezno) Konfiguriranje i pokrenite Windows projekata

U ovom se odjeljku je za pokretanje Xamarin.Forms WinApp i WinPhone81 projekata za uređaje sa sustavom Windows. Ove korake podržava i projektima univerzalni platforme Windows (UWP). Ako ne radite s uređaje sa sustavom Windows, preskočite ovaj odjeljak.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrirajte Windows aplikacije za slanje obavijesti s WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Konfiguriranje obavijesti koncentrator za WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Dodavanje automatske obavijesti aplikacija za Windows

1. U Visual Studio, otvorite **App.xaml.cs** u programu project za Windows i dodajte sljedeće **pomoću** naredbe.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Zamjena `<your_TodoItemManager_portable_class_namespace>` prostora za naziv prijenosni projekta koji sadrži na `TodoItemManager` predmete.
 

2. U App.xaml.cs dodajte sljedeće **InitNotificationsAsync** metoda: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Ovaj postupak dobiva kanal automatske obavijesti i registrira predložak da biste primali obavijesti predloška iz centra za obavijesti. Obavijest predložak koji podržava *messageParam* isporuku će se ovaj klijent.

3. U App.xaml.cs, ažurirati definiciju način rukovatelja događajima **OnLaunched** dodavanjem u `async` mijenjanje, zatim dodajte sljedeći redak koda na kraju metoda: 

        await InitNotificationsAsync();

    Time ćete obavijest o registraciji automatske stvorili ili osvježiti prilikom svakog pokretanja aplikacije. Važno je da to jamči da automatske kanala WNS uvijek je aktivna.  

4. U programu Explorer rješenja za Visual Studio, otvorite datoteku **Package.appxmanifest** i postavite **Skočnoj instaliranih** na **da** u odjeljku **obavijesti**.

5. Stvaranje aplikacije i provjere imate bez pogrešaka.  Klijent aplikacije sada trebala bi se registrirati za obavijesti predloška iz pozadine za mobilnu aplikaciju. U ovom se odjeljku ponovite za svaki projekt programa Windows u rješenje.


####<a name="test-push-notifications-in-your-windows-app"></a>Testiranje automatske obavijesti u aplikacija za Windows

1. U Visual Studio, desnom tipkom miša kliknite Windows projekta, a zatim kliknite **Postavi kao pokretanje projekta**.

2. Pritisnite gumb **Pokreni** omogućuje stvaranje projekta i pokrenite aplikaciju.

3. U aplikaciji, upišite naziv nove todoitem, a zatim kliknite znak plus (**+**) ikonu da biste je dodali.

4. Provjerite je li je primili obavijest prilikom dodavanja stavke.

##<a name="next-steps"></a>Daljnji koraci

Dodatne informacije o automatske obavijesti:

* [Dijagnosticiranje problema automatske obavijesti](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Postoje razni razloga zašto obavijesti možda se prekine ili prekid prema gore na uređajima. U ovoj se temi objašnjava za analizu i utvrđivanje uzrok neuspjeha automatske obavijesti. 

Imajte na umu nastavka na jedan od sljedećih vodiči za:

* [Dodavanje provjere autentičnosti za aplikaciju](app-service-mobile-xamarin-forms-get-started-users.md)  
Upute za provjeru autentičnosti korisnika aplikacije s davateljem identiteta.

* [Omogućivanje izvanmrežne sinkronizacije za aplikaciju](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Saznajte kako dodati aplikacije pomoću programa pozadinskog mobilnu aplikaciju Izvanmrežna podrška. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

