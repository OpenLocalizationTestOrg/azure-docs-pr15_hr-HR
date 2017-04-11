<properties
    pageTitle="iOS automatske obavijesti s obavijesti koncentratora za aplikacije Xamarin | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču, Saznajte kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti aplikaciju za iOS Xamarin."
    services="notification-hubs"
    keywords="ios automatske obavijesti, slanje poruka, slanje obavijesti, slanje poruka"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS automatske obavijesti s obavijesti koncentratora za Xamarin aplikacije

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Pregled
> [AZURE.IMPORTANT] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Pomoću ovog praktičnog vodiča pokazuje kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti aplikaciju iOS.
Stvorit ćete prilagođenom Xamarin.iOS web-aplikacijom koja prima slanje obavijesti za [Servis za Apple automatske obavijesti (APN-ove)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). Kada završite, ćete moći koristiti koncentratora za obavijesti za emitiranje automatske obavijesti na svim uređajima s instaliranim programom aplikacije. Dovršeni kod nalazi se u [aplikaciju NotificationHubs] [ GitHub] uzorka.

Pomoću ovog praktičnog vodiča pokazuje jednostavne automatske scenarij emitiranje poruke s obavijesti koncentratora.

##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ [Xcode 6.0][Install Xcode]
+ IOS 7.0 (ili novija verzija) kompatibilan uređaj
+ iOS članstvo u Program za razvojne inženjere
+ [Xamarin Studio]

   > [AZURE.NOTE] Zbog konfiguracije preduvjeti za iOS automatske obavijesti, morate uvesti i testirali primjer aplikacije na uređaju fizičke iOS (iPhone ili iPad) umjesto u simulator.

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve obavijesti koncentratora vodiče za Xamarin iOS aplikacije.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Konfiguriranje koncentratora za obavijesti

U ovom se odjeljku vodit će vas kroz stvaranje nove koncentrator obavijesti i konfiguriranje provjere autentičnosti s APN-ove pomoću certifikata automatske **.p12** koji ste stvorili. Ako želite koristiti obavijesti koncentratora koje ste već stvorili, možete preskočiti na 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Kao želimo konfigurirati vezu za APN-ove na portalu za Azure, otvorite postavke obavijesti koncentrator, ande kliknite na <b>Servisima za obavijesti</b>, a zatim kliknite <b>Apple (APN-ove)</b> stavke na popisu. Kada završi, kliknite <b>Prijenos certifikat</b> i odaberite <b>.p12</b> certifikat koji ste ranije, izvezli kao i lozinka za potvrdu.</p>
<p>Provjerite je li za odabir načinu rada <b>s memorijom za testiranje</b> jer će slanje automatskih poruka u razvojno okruženje. Postavka <b>radnog</b> koristiti samo ako želite poslati slanje obavijesti za korisnike koji već kupili aplikacije iz trgovine.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Koncentratora za obavijesti sada je konfiguriran za APN-ove, a imate nizove veze da biste registrirali aplikacije i Pošalji automatske obavijesti.


##<a name="connect-your-app-to-the-notification-hub"></a>Povezivanje aplikacije koncentrator obavijesti

#### <a name="create-a-new-project"></a>Stvaranje novog projekta

1. U Xamarin Studio, stvorite novi projekt iOS i odaberite **Sjedinjeno komuniciranje API** > **Jedan prikaz aplikacije** predložak.

    ![Xamarin Studio – Vrsta odaberite aplikacije][31]

2. Dodavanje reference s komponentom Azure poruka. U prikazu rješenje desnom tipkom miša kliknite mapu **komponente** projekta, a zatim odaberite **Dobiti dodatne komponente**. Pretraživanje za komponentu **Azure poruka** i dodajte komponentu u projekt.

3. U **AppDelegate.cs**, dodajte sljedeće pomoću naredbe:

        using WindowsAzure.Messaging;

4. Deklariranje instance komponente **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. Stvaranje **Constants.cs** predmete s sljedeće varijable:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. U **AppDelegate.cs**, ažurirajte **FinishedLaunching()** da sadrže sljedeće:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Zanemarivanje metoda **RegisteredForRemoteNotifications()** u **AppDelegate.cs**:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Zanemarivanje metoda **ReceivedRemoteNotification()** u **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Stvoriti sljedeću metodu **ProcessNotification()** **AppDelegate.cs**:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Možete odabrati da biste nadjačali **FailedToRegisterForRemoteNotifications()** u raznim situacijama kao što su nema mrežne veze. To je osobito važno koju korisnik može pokrenuti aplikacije u izvanmrežnom načinu rada (primjerice aviona) i želite rukovati automatskih poruka scenariji specifične za aplikacije.


10. Pokrenite aplikaciju na uređaju.


## <a name="sending-push-notifications"></a>Slanje automatskih obavijesti


Možete testirati primanje automatske obavijesti u svojoj aplikaciji slanjem obavijesti [Azure Portal] putem **Testiranje slanje** mogućnost skup alata za **Otklanjanje poteškoća** , desno na stranici središte obavijesti kao što je prikazano na zaslonu u nastavku.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Automatske obavijesti obično se šalju putem servisa pozadinske kao što su mobilne usluge ili ASP.NET pomoću kompatibilne biblioteke. REST API-JA možete koristiti i izravno za slanje automatskih poruka ako biblioteci nije dostupna u vašem scenariju. 

U ovom ćete praktičnom vodiču ćemo će jednostavnost i samo demonstrirati testiranje aplikacije klijenta slanjem obavijesti pomoću .NET SDK za obavijesti koncentratora u aplikaciji konzole umjesto pozadinskog servisa. Praktični vodič za [Korištenje obavijesti koncentratora za slanje obavijesti korisnicima](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) preporučujemo kao na sljedeći korak za slanje obavijesti iz programa ASP.NET pozadine. Međutim, sljedeće postupke mogu koristiti za slanje obavijesti:

* **OSTALE sučelja**: podržavaju automatske obavijesti na bilo kojoj platformi pozadinskog pomoću [sučelja OSTALE](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure obavijesti koncentratora .NET SDK**: U alatu Nuget Upravitelj paketa za Visual Studio, pokrenite [Instalaciju paketa Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [kako koristiti koncentratora obavijesti iz Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobilne aplikacije**: primjerice slanje obavijesti iz mobilne aplikacije za Azure aplikacija servisa pozadinskog sustava koji je integriran s koncentratorima obavijesti potražite u članku [Dodavanje slanje obavijesti za mobilne aplikacije](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: primjerice slanje automatskih obavijesti pomoću REST API-ji potražite u članku "da biste koristili koncentratora obavijesti iz Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Neobavezno) Pošalji automatske obavijesti iz aplikacije konzole za .NET

U ovom ćete odjeljku smo poslat će slanje obavijesti putem jednostavne aplikacije konzole za .NET. U svrhu u ovom primjeru, ne možemo će se prebaciti razvojno okruženje sustava Windows koji sadrži Visual Studio već instaliran.

1. U Visual Studio stvaranje nove Visual C# konzole aplikacije:

    ![Visual Studio – stvaranje nove aplikacije konzole][213]

2. U Visual Studio, kliknite **Alati**, kliknite **Upravitelj paketa NuGet**pa kliknite **Upravitelj paketa konzolu**.

    Konzola upravitelja paketa prikazivati Usidreni pri dnu radnog prostora za Visual Studio.

3. U prozoru upravitelja paketima konzole postavite **zadane projekta** na novi projekt konzole za aplikaciju, a zatim u prozoru konzole, pokrenite sljedeću naredbu:

        Install-Package Microsoft.Azure.NotificationHubs

    Time se dodaje referenca SDK koncentratora obavijesti Azure pomoću značajke <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification koncentratora NuGet paketa</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Otvaranje u `Program.cs` datoteku i dodajte sljedeće `using` naredbe, jamči da možete koristiti funkcije u svojoj učionici glavni i Azure klase:

        using Microsoft.Azure.NotificationHubs;

3. U vašem `Program` klase, dodajte na sljedeći način (ne zaboravite da biste zamijenili **niz za povezivanje** i **naziv koncentrator**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Dodajte sljedeće retke u vašem `Main` metoda:

         SendNotificationAsync();
         Console.ReadLine();

5. Da biste pokrenuli aplikaciju, pritisnite tipku F5. Sekundi, trebali biste vidjeti obavijest automatske pojavljuju na uređaju. Bez obzira koristite li Wi-Fi ili mobilnom podatkovnom mrežom, provjerite je li na uređaju imate aktivnu internetsku vezu.

Moguće payloads možete pronaći u jabuka [lokalne i automatske obavijesti vodiču za programiranje].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Neobavezno) Slanje obavijesti iz servis za mobilne uređaje

U ovom ćete odjeljku smo poslat će slanje obavijesti putem servisa za mobilne putem skripte čvor.

Da biste poslali obavijest putem mobilne usluge, slijedite [Početak rada s mobilnih usluga], a zatim:

1. Prijava na [Portal za klasični Azure]pa odaberite servis za mobilne uređaje.

2. Odaberite karticu **raspored** na vrhu.

    ![Azure klasični portala - rasporeda][215]

3. Stvaranje nove zakazani posao, umetanje naziv i odaberite **na zahtjev**.

    ![Azure Portal klasični – stvaranje novog zadatka][216]

4. Kada se stvara zadatak, kliknite naziv zadatka. Zatim kliknite karticu **skripte** na gornjoj traci.

5. Umetnite sljedeću skriptu unutar funkcija raspored. Provjerite jeste li zamijenite rezervirana mjesta koncentratora naziva obavijesti i niz za povezivanje za *DefaultFullSharedAccessSignature* koji ste nabavili neke starije verzije. Kliknite **Spremi**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Na donjoj traci, kliknite **Pokretanje jednom** . Trebali biste primali obavijesti na uređaju.

##<a name="next-steps"></a>Daljnji koraci

U ovom primjeru jednostavne raspoređena automatske obavijesti na svim uređajima sa sustavom iOS. Da biste ciljnu određene korisnike, pogledajte praktičnom vodiču [Pomoću obavijesti koncentratora za slanje obavijesti korisnicima]. Ako želite fazi korisnika po grupama kamatu, možete čitati [Korištenje koncentratora obavijesti da biste poslali važnih vijesti]. Dodatne informacije o načinu korištenja koncentratora obavijesti u [Obavijesti koncentratora upute] i na [Obavijesti koncentratora s uputama za iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Početak rada s mobilnih usluga]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure klasični Portal]: https://manage.windowsazure.com/
[Upute za koncentratora obavijesti]: http://msdn.microsoft.com/library/jj927170.aspx
[Obavijest o koncentratora s uputama za iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Automatske obavijesti korisnicima pomoću obavijesti koncentratora]: /manage/services/notification-hubs/notify-users-aspnet
[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: /manage/services/notification-hubs/breaking-news-dotnet

[Lokalni i automatske obavijesti vodič za programiranje]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Portal za Azure]: https://portal.azure.com
