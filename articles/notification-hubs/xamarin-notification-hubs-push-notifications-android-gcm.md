<properties
    pageTitle="Početak rada s obavijesti koncentratora za aplikacije Xamarin.Android | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču, Saznajte kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti s aplikacijom Xamarin Android."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Početak rada s koncentratorima obavijesti s Xamarin za Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča pokazuje kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti Xamarin.Android aplikacije.
Stvorit ćete prilagođenom Xamarin.Android web-aplikacijom koja prima slanje obavijesti putem razmjene poruka Google Cloud (GCM). Kada završite, ćete moći koristiti koncentratora za obavijesti za emitiranje automatske obavijesti na svim uređajima s instaliranim programom aplikacije. Dovršeni kod nalazi se u [aplikaciju NotificationHubs] [ GitHub] uzorka.

Pomoću ovog praktičnog vodiča pokazuje jednostavni scenarij emitiranje pomoću koncentratora obavijesti.


## <a name="before-you-begin"></a>Prije početka

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dovršeni kod za ovaj vodič možete pronaći na GitHub [ovdje](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ Visual Studio s Xamarin u sustavu Windows ili Xamarin Studio na Mac OS X. potpune upute za instalaciju nalaze se na [Postavljanje i instalirajte za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ Aktivni Google račun
+ [Azure komponenta za razmjenu poruka]
+ [Google Cloud poruka klijentske komponente]

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve obavijesti koncentratora vodiče za Xamarin.Android aplikacije.

> [AZURE.IMPORTANT] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Omogućivanje Google Cloud razmjene poruka

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Konfiguriranje koncentratora za obavijesti

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Kliknite karticu <b>Konfiguracija</b> pri vrhu, unesite <b>Ključ za API</b> vrijednost ste nabavili u prethodnom odjeljku i zatim kliknite <b>Spremi</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Koncentratora za obavijesti sada je konfiguriran za rad s GCM, a imate nizove veze i registrirati aplikacije da biste primali obavijesti i slanje automatskih obavijesti.

##<a name="connect-your-app-to-the-notification-hub"></a>Povezivanje aplikacije koncentrator obavijesti

###<a name="create-a-new-project"></a>Stvaranje novog projekta

1. U Xamarin Studio, kliknite **Novo rješenje**, kliknite **Aplikacija za Android**i kliknite **Dalje**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Unesite **Naziv aplikacije** i **identifikator**. Kliknite **Ciljnu Plaforms** želite podržava, a zatim kliknite **Dalje** i **Stvaranje**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Time se stvara novi projekt sa sustavom Android.

2. Da biste otvorili svojstva projekta, desnom tipkom miša kliknete novi projekt prikaz rješenja, a zatim odaberete **Mogućnosti**. Odaberite stavku **Aplikacija sa sustavom Android** u odjeljku **Sastavljanje** .

    Provjerite je li prvo slovo **Naziv paketa** mala slova.

    > [AZURE.IMPORTANT] Prvo slovo naziva paket mora biti mala slova. U suprotnom, primit ćete manifesta pogreške aplikacije kada registrirate **BroadcastReceiver** i **IntentFilter** za slanje obavijesti ispod.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Po želji, postavite **Android Minimalna verzija** na neku drugu razinu API-JA.

4. Po želji, postavljanje **cilj Android verzija** na neku drugu verziju API koju želite ciljani (mora biti API razinu 8 ili noviji).

Kliknite **u redu** i zatvorite dijaloški okvir Mogućnosti projekta.


###<a name="add-the-required-components-to-your-project"></a>Dodajte obavezne komponente u projektu

Google Cloud poruka klijentskog programa dostupna u trgovini komponente Xamarin pojednostavljuje postupak podrške automatske obavijesti u Xamarin.Android.

1. Desnom tipkom miša kliknite mapu komponente Xamarin.Android u aplikaciji, a zatim odaberite **Dobiti dodatne komponente**.

2. Traženje komponentu **Azure poruka** i dodajte ga u projekt.

3. Potražite komponentu **Google Cloud poruka klijenta** i dodati u projekt.


###<a name="set-up-notification-hubs-in-your-project"></a>Postavljanje obavijesti koncentratora u projektu

1. Prikupite sljedeće informacije za Android koncentrator aplikacije i obavijesti:

    - **GoogleProjectNumber**: tu vrijednost broj projekta zatražite od pregled aplikacije portala za razvojne inženjere za Google. Prilikom stvaranja aplikacije na portalu unesene bilješke te vrijednosti neke starije verzije.
    - **Slušanje niz za povezivanje**: na nadzornoj ploči za [Azure klasični Portal], kliknite **Prikaz nizu za povezivanje**. Kopirajte niz za povezivanje *DefaultListenSharedAccessSignature* za tu vrijednost.
    - **Koncentrator naziv**: to je naziv vaše središte s [Azure klasični Portal]. Na primjer, *mynotificationhub2*.

    Stvaranje **Constants.cs** klase Xamarin projekta i odrediti sljedeće konstante u klasu. Zamjena rezerviranih mjesta vrijednosti.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Dodajte sljedeće pomoću naredbe za **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. Dodavanje varijabli instancu da biste na `MainActivity` klase koja će se koristiti za prikaz upozorenja dijaloškog kada je pokrenut aplikaciju:

        public static MainActivity instance;


3. Klase **MainActivity** stvoriti na sljedeći način:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. U na `OnCreate` način **MainActivity.cs**pokrenuti na `instance` varijabla i dodajte poziv `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Stvorite novi klase **MyBroadcastReceiver**.

    > [AZURE.NOTE] Ne možemo će voditi kroz stvaranje **BroadcastReceiver** predmete ispočetka ispod. Brzi zamjena za ručno stvarati **MyBroadcastReceiver.cs** je da biste se pozvali **GcmService.cs** datoteke pronaći u programu project Xamarin.Android uzorka uključene [NotificationHubs uzoraka][GitHub]. Dupliciranje **GcmService.cs** i mijenjanje nazivi klasa može biti sjajno mjesto da biste pokrenuli kao i.

5. Dodajte sljedeće pomoću naredbe za **MyBroadcastReceiver.cs** (upućuju na komponente i koji ste prethodno dodali):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. U **MyBroadcastReceiver.cs**, dodajte sljedeće zahtjeva za dozvole između rečenica **pomoću** i deklariranje **prostor naziva** :

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. U **MyBroadcastReceiver.cs**, promijenite predmet **MyBroadcastReceiver** da sadrže sljedeće:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Dodajte drugi klase **MyBroadcastReceiver.cs** pod nazivom **PushHandlerService**, koji je rezultat **GcmServiceBase**. Provjerite je li da biste primijenili **servisa** atribut klase:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** implementira metode **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**i **OnError()**. Naš klase **PushHandlerService** implementaciju morate odbaciti ovih metoda i ovih metoda će pokrenuti u odgovoru interakcija s središtu obavijesti.


9. Nadjačati metoda **OnRegistered()** u **PushHandlerService** pomoću sljedeći kod:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] **OnRegistered()** kod iznad, imajte na umu mogućnost da biste odredili oznake da biste registrirali za određene razmjenu kanale.

10. Nadjačati metoda **OnMessage** u **PushHandlerService** pomoću sljedeći kod:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Dodavanje načina **createNotification** i **dialogNotify** **PushHandlerService** za obavješćivati korisnike kada je primili obavijest.

    >[AZURE.NOTE] Obavijest o dizajna u Android verzija 5.0 ili noviji predstavlja značajan odlaska iz prethodne verzije. Ako vam to provjerili na Android 5.0 ili noviji, aplikaciju morat ćete imati da biste primali obavijesti. Dodatne informacije potražite u članku [Android obavijesti](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Zanemarivanje Apstraktni članovi **OnUnRegistered()**, **OnRecoverableError()**i **OnError()** tako da se kompilira kod:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Pokretanje aplikacije u na emulator

Pokretanje aplikacije u na emulator, provjerite je li pomoću programa Android virtualne uređaja (AVD) koji podržava API-ji Google.

> [AZURE.IMPORTANT] Da bi se primajte automatske obavijesti, morate postaviti Google račun na uređaju sa sustavom virtualne. (U emulator, idite na **Postavke** pa kliknite **Dodaj račun**). Osim toga, provjerite je li u emulator povezani s Internetom.

>[AZURE.NOTE] Obavijest o dizajna u Android verzija 5.0 ili noviji predstavlja značajan odlaska iz prethodne verzije. Dodatne informacije potražite u članku [Android obavijesti](http://go.microsoft.com/fwlink/?LinkId=615880).


1. U odjeljku **Alati**kliknite **Otvorite upravitelj Emulator Android**, odaberite svoj uređaj, a zatim **Uređivanje**.

    ![][18]

2. Odaberite **Google API -ji** u **ciljnoj**, a zatim kliknite **u redu**.

    ![][19]

3. Na gornjoj alatnoj traci kliknite **Pokreni**, a zatim odaberite aplikaciju. Pokreće se emulator i pokreće aplikaciju.

  Aplikacija dohvaća *atributima registrationId* iz GCM i registrira s središtu obavijesti.

##<a name="send-notifications-from-your-backend"></a>Slanje obavijesti iz svoje pozadine


Možete testirati primanje obavijesti u svojoj aplikaciji slanjem obavijesti [Azure klasični Portal] putem kartice za ispravljanje pogrešaka u središtu obavijesti kao što je prikazano na zaslonu u nastavku.

![][30]


Automatske obavijesti obično se šalju u pozadinskog servis kao što je mobilnih usluga ili ASP.NET putem kompatibilne biblioteke. REST API-JA možete koristiti i izravno za slanje obavijesti o ako biblioteci nije dostupna za vaš pozadinskog.

Slijedi popis nekih vodiče koje možda želite pregledati za slanje obavijesti:

- ASP.NET: Potražite u članku [Korištenje obavijesti koncentratora za slanje obavijesti korisnicima].
- Azure Java koncentratora obavijesti SDK: Informirajte [se o korištenju koncentratora obavijesti iz Java](notification-hubs-java-push-notification-tutorial.md) za slanje obavijesti iz Java. To testirano u Eclipse za razvoj sa sustavom Android.
- PHP: Potražite [u](notification-hubs-php-push-notification-tutorial.md)članku korištenje koncentratora obavijesti iz PHP.


U na sljedeći Pododlomci koji vodiča, slanje obavijesti putem aplikacije konzole za .NET i servis za mobilne uređaje putem skripte čvor.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Neobavezno) Slanje obavijesti putem aplikacije .NET

U ovom ćete odjeljku smo poslat će obavijesti putem aplikacije konzole za .NET

1. Stvaranje nove Visual C# konzole aplikacije:

    ![][20]

2. U Visual Studio, kliknite **Alati**, kliknite **Upravitelj paketa NuGet**pa kliknite **Upravitelj paketa konzolu**.

    Prikazat će se konzole za Upravitelj paketa u Visual Studio.

3. U prozoru upravitelja paketima konzole postavite **zadane projekta** na novi projekt konzole za aplikaciju, a zatim u prozoru konzole, pokrenite sljedeću naredbu:

        Install-Package Microsoft.Azure.NotificationHubs

    Time se dodaje referenca SDK koncentratora obavijesti Azure pomoću značajke <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification koncentratora NuGet paketa</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Otvorite datoteku Program.cs i dodajte sljedeće `using` izjava:

        using Microsoft.Azure.NotificationHubs;

5. U vašem `Program` klase, dodajte na sljedeći način. Tekst rezerviranog mjesta ažurirati vašim imenom *DefaultFullSharedAccessSignature* veze niz i koncentrator s [Azure klasični Portal].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Dodajte sljedeće retke u način **glavne** :

         SendNotificationAsync();
         Console.ReadLine();

7. Da biste pokrenuli aplikaciju, pritisnite tipku F5. U aplikaciji trebale primiti obavijest.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Neobavezno) Slanje obavijesti pomoću servis za mobilne uređaje

1. Slijedite [Početak rada s mobilnih usluga].

1. Prijava na [Portal za klasični Azure]pa odaberite servis za mobilne uređaje.

2. Odaberite karticu **raspored** na vrhu.

    ![][22]

3. Stvaranje nove zakazani posao, umetanje naziv i odaberite **na zahtjev**.

    ![][23]

4. Kada se stvara zadatak, kliknite naziv zadatka. Zatim kliknite karticu **skripte** na gornjoj traci.

5. Umetnite sljedeću skriptu unutar funkcija raspored. Provjerite jeste li zamijenite rezervirana mjesta koncentratora naziva obavijesti i niz za povezivanje za *DefaultFullSharedAccessSignature* koji ste nabavili neke starije verzije. Kliknite **Spremi**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Na donjoj traci, kliknite **Pokretanje jednom** . Trebali biste dobiti skočnoj obavijesti.

##<a name="next-steps"></a>Daljnji koraci

U ovom primjeru jednostavne raspoređena obavijesti o svim svojim uređajima sa sustavom Android. Da biste ciljnu određene korisnike, pogledajte praktičnom vodiču [Pomoću obavijesti koncentratora za slanje obavijesti korisnicima]. Ako želite fazi korisnika po grupama kamatu, možete čitati [Korištenje koncentratora obavijesti da biste poslali važnih vijesti]. Dodatne informacije o načinu korištenja koncentratora obavijesti u [Obavijesti koncentratora upute] i na [Obavijesti koncentratora s uputama za Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Početak rada s mobilnih usluga]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure klasični Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Upute za koncentratora obavijesti]: http://msdn.microsoft.com/library/jj927170.aspx
[Upute za korištenje obavijesti koncentratora za Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Automatske obavijesti korisnicima pomoću obavijesti koncentratora]: /manage/services/notification-hubs/notify-users-aspnet
[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud poruka klijentske komponente]: http://components.xamarin.com/view/GCMClient/
[Azure komponenta za razmjenu poruka]: http://components.xamarin.com/view/azure-messaging
