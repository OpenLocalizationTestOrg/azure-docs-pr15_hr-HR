<properties
    pageTitle="Slanje automatske obavijesti s koncentratorima Azure obavijesti na uređaju Windows Phone | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču Saznajte kako koristiti Azure obavijesti koncentratora za slanje obavijesti za Windows Phone 8 ili Windows Phone 8.1 Silverlight aplikacije."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="automatske obavijesti, automatske obavijesti, automatske u sustavu windows phone"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Slanje automatskih obavijesti Azure koncentratora obavijesti na uređaju Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Pregled

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Pomoću ovog praktičnog vodiča pokazuje kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti za Windows Phone 8 ili Windows Phone 8.1 Silverlight aplikacije. Ako birate Windows Phone 8.1 (koji nisu Silverlight), zatim odnose se na [Windows univerzalni](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) verziju.
U ovom ćete praktičnom vodiču stvarate praznu aplikaciju za Windows Phone 8 koja prima automatske obavijesti pomoću Microsoft automatske obavijesti servisa (MPNS). Kada završite, ćete moći koristiti koncentratora za obavijesti za emitiranje automatske obavijesti na svim uređajima s instaliranim programom aplikacije.

> [AZURE.NOTE] Obavijest o koncentratora Windows Phone SDK ne podržava u sustavu Windows automatske obavijesti servisa (WNS) pomoću aplikacije za Windows Phone 8.1 Silverlight. Da biste koristili WNS (umjesto MPNS) s aplikacijama za Windows Phone 8.1 Silverlight, slijedite [Obavijest koncentratora – vodič za Windows Phone Silverlight], koja koristi REST API-ji.

Vodič pokazuje jednostavni scenarij emitiranje pomoću koncentratora obavijesti.

##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ [Visual Studio 2012 Express za Windows Phone]ili noviju verziju.

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve obavijesti koncentratora vodiče za aplikacije za Windows Phone 8.

##<a name="create-your-notification-hub"></a>Stvaranje koncentratora za obavijesti

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Kliknite sekciju <b>Obavijesti Services</b> (u odjeljku <i>Postavke</i>), kliknite na <b>Windows Phone (MPNS)</b> , a zatim potvrdite okvir <b>Omogući Neprovjereni automatske</b> .</p>
</li>
</ol>

&emsp;&emsp;![Azure portala - Omogući unathenticated automatske obavijesti](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Vaš koncentrator sada se stvara i konfiguriran za slanje Neprovjereni obavijesti za Windows Phone.

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi MPNS u načinu rada za Neprovjereni. MPNS Neprovjereni način dolazi s ograničenjima na obavijesti koje možete poslati svakog kanala. Obavijest o koncentratora podržava [MPNS autentičnost način](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) omogućujući vam da biste prenijeli certifikata.

##<a name="connecting-your-app-to-the-notification-hub"></a>Povezivanje aplikacije koncentrator obavijesti

1. U Visual Studio, stvorite novu aplikaciju Windows Phone 8.

    ![Aplikacija za Visual Studio – novi projekt – Windows Phone][13]

    U Visual Studio 2013 ažuriranje 2 ili noviju verziju, umjesto toga stvoriti aplikaciju za Windows Phone Silverlight.

    ![Visual Studio – novi projekt - praznu aplikaciju – Silverlight u sustavu Windows Phone][11]

2. U Visual Studio, desnom tipkom miša kliknite rješenje, a zatim **Upravljanje NuGet paketa**.

    Prikazat će se dijaloški okvir **Upravljanje NuGet paketa** .

3. Traženje `WindowsAzure.Messaging.Managed` i kliknite **Instaliraj**, a zatim prihvatite uvjete korištenja.

    ![Visual Studio - NuGet Upravitelj paketa][20]

    To preuzimanja instalira, a dodaje referencu u biblioteku Azure poruka za Windows pomoću <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet paketa</a>.

4. Otvorite datoteku App.xaml.cs i dodajte sljedeće `using` izvješća:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Pri vrhu metoda **Application_Launching** u App.xaml.cs, dodajte sljedeći kod:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] Vrijednost **MyPushChannel** je indeks koji se koristi za potražiti postojeće kanala u zbirci [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) . Ako nema nešto postoji, stvorite novu stavku s tim nazivom.
    
    Provjerite je li da biste umetnuli naziv vaše središte i niz za povezivanje naziva **DefaultListenSharedAccessSignature** koji ste nabavili u prethodnom odjeljku.
    Kod dohvaća kanala URI za aplikaciju iz MPNS, a zatim registrira kanala URI s koncentratora za obavijesti. To također jamčiti koji kanala URI registriran u koncentratora za obavijesti prilikom svakog pokretanja aplikacije.

    >[AZURE.NOTE]Pomoću ovog praktičnog vodiča šalje skočnoj obavijesti na uređaju. Prilikom slanja obavijesti pločica, umjesto toga morate pozvati metodu **BindToShellTile** na kanal. Da biste podržavaju oba skočnoj i pločica obavijesti, nazovite **BindToShellTile** i **BindToShellToast**.

6. U pregledniku rješenja proširite **Svojstva**, otvorite na `WMAppManifest.xml` datoteku, kliknite karticu **Mogućnosti** i provjerite je li isključena mogućnost **ID_CAP_PUSH_NOTIFICATION** .

    ![Visual Studio - mogućnosti aplikacije u sustavu Windows Phone][14]

    Na taj način aplikaciju možete primajte automatske obavijesti. Bez nje, sve da biste poslali automatske obavijesti aplikaciju neće uspjeti.

7. Pritisnite na `F5` ključ da biste pokrenuli aplikaciju.

    Registracija poruke prikazuje se u aplikaciju.

8. Zatvaranje aplikacije.  

   >[AZURE.NOTE] Da biste primali skočnoj automatske obavijesti, aplikacija ne mora biti pokrenut u prvom planu.

##<a name="send-push-notifications-from-your-backend"></a>Pošalji automatske obavijesti iz svoje pozadine

Automatske obavijesti možete poslati pomoću koncentratora obavijesti iz bilo kojeg pozadine putem javne <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">OSTALE sučelja</a>. U ovom ćete praktičnom vodiču Pošalji automatske obavijesti pomoću aplikacije konzole za .NET. 

Primjer Pošalji automatske obavijesti iz ASP.NET WebAPI pozadinskog sustava koji je integriran s koncentratorima obavijesti potražite u članku [Azure obavijesti koncentratora obavijestite korisnike s pozadinskom .NET](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Na primjer kako [REST API -ji](https://msdn.microsoft.com/library/azure/dn223264.aspx)slati automatske obavijesti, pogledajte [upute za korištenje koncentratora obavijesti iz Java](./notification-hubs-java-push-notification-tutorial.md) i [upute za korištenje koncentratora obavijesti iz i](./notification-hubs-php-push-notification-tutorial.md).

1. Desnom tipkom miša kliknite rješenje, odaberite **Dodaj** i **Novi projekt...**, zatim u odjeljku **Visual C#**, kliknite **Windows** i **Aplikacije konzole**i kliknite **u redu**.

    ![Aplikacije konzole za Visual Studio – novi projekt-][6]

    Time se dodaje nove Visual C# konzole aplikacije rješenje. Možete učiniti i to u zasebnom rješenja.

4. Kliknite **Alati**, kliknite **Upravitelj paketa biblioteke**, a zatim **Konzole za Upravitelj paketa**.

    Prikazat će se konzole za Upravitelj paketa.

5.  U prozoru **Upravitelja paketima konzole** postavite **zadane projekta** na novi projekt konzole za aplikaciju, a zatim u prozoru konzole, pokrenite sljedeću naredbu:

        Install-Package Microsoft.Azure.NotificationHubs

    Time se dodaje referenca SDK koncentratora obavijesti Azure pomoću značajke <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification koncentratora NuGet paketa</a>.

6. Otvaranje u `Program.cs` datoteku i dodajte sljedeće `using` izjava:

        using Microsoft.Azure.NotificationHubs;

6. U na `Program` klase, dodajte na sljedeći način:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Provjerite je li da biste zamijenili na `<hub name>` rezervirano mjesto s nazivom središtu obavijesti koji se pojavljuje na portalu. Osim toga, rezervirano mjesto za niz veze zamijenili niz za povezivanje naziva **DefaultFullSharedAccessSignature** koji ste nabavili u odjeljku "Konfiguriranje koncentratora za obavijesti."

    >[AZURE.NOTE]Provjerite je li koristiti niz za povezivanje s **potpunim** pristupom, nije **preslušali** pristup. Niz preslušavanja pristup imati dozvole za slanje automatske obavijesti.

4. Dodajte na sljedeći redak u vašem `Main` metoda:

         SendNotificationAsync();
         Console.ReadLine();

5. Pomoću sustava Windows Phone emulator pokrenut i aplikacije Zatvoreno, postaviti aplikacije project konzole kao zadani pokretanje projekta, a zatim pritisnite u `F5` ključ da biste pokrenuli aplikaciju.

    Primit ćete skočnoj automatske obavijesti. Dodirnite natpis skočnoj učitava aplikaciju.

Moguće payloads možete pronaći u [katalog skočnoj] i [pločica kataloga] teme na MSDN-u.

##<a name="next-steps"></a>Daljnji koraci

U ovom primjeru jednostavne raspoređena automatske obavijesti svih svojih uređaja Windows Phone 8. 

Da biste ciljnu određene korisnike odnose se na vodič za [Korištenje obavijesti koncentratora za slanje obavijesti korisnicima] . 

Ako želite fazi korisnika po grupama kamatu, možete čitati [Korištenje koncentratora obavijesti da biste poslali važnih vijesti]. 

Dodatne informacije o korištenju koncentratora obavijesti u [Uputama koncentratora obavijesti].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express za Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Upute za koncentratora obavijesti]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Automatske obavijesti korisnicima pomoću obavijesti koncentratora]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[Skočno kataloga]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[pločica kataloga]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Obavijest o koncentratora – vodič za Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

