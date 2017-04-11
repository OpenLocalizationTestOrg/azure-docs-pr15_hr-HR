<properties
    pageTitle="Dodavanje automatske obavijesti u aplikaciju za univerzalni platforme Windows (UWP) | Azure mobilne aplikacije"
    description="Saznajte kako pomoću mobilne aplikacije za Azure aplikacija servisa Azure obavijesti koncentratora šaljite automatske obavijesti u aplikaciju za univerzalni platforme Windows (UWP)."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Dodavanje automatske obavijesti aplikacija za Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Pregled

U ovom ćete praktičnom vodiču dodate automatske obavijesti [start sustava Windows brzi](app-service-mobile-windows-store-dotnet-get-started.md) projekt tako da se automatske obavijesti poslati na uređaju svaki put kada se umetne zapis.

Ako ne koristite project server preuzete brzi početak rada, morate paket za nastavak automatske obavijesti. Dodatne informacije potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="configure-hub"></a>Konfiguriranje obavijesti koncentratora

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Registracija aplikacije za slanje obavijesti

Morate poslati aplikacije u trgovini Windows, a zatim konfigurirati projekta poslužitelja za integraciju sa servisa Windows obavijesti (WNS) da biste poslali automatske.

1. U programu Explorer Visual Studio u rješenje, desnom tipkom miša kliknite projekt UWP aplikaciju, kliknite **trgovina** > **Aplikacije za povezivanje s trgovinom...**. 

    ![Pridruživanje aplikacija iz Windows trgovine](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. U čarobnjaku kliknite **Dalje**, prijavite se pomoću Microsoftova računa, upišite naziv aplikacije u **rezerviranje novi naziv aplikacije**, a zatim kliknite **rezerviranje**.

3. Nakon registracije za aplikaciju uspješno stvorili, odaberite novi naziv aplikacije, kliknite **Dalje**, a zatim **povezati**. To su potrebne informacije o registraciji iz Windows trgovine dodaje programski manifest.  

7. Otvorite [Windows Razvojni centar](https://dev.windows.com/en-us/overview)za prijavu pomoću Microsoftova računa kliknite novi Registracija aplikacije u **Moje aplikacije**, a zatim proširite **Services** > **automatske obavijesti**. 

8. Na stranici **automatske obavijesti** , kliknite **Live bilježaka i brošura** u odjeljku **Microsoft Azure mobilne usluge**.

9. Na stranici Registracija zabilježite vrijednost u odjeljku **aplikacije tajne** i **SID paketa**, koji ćete koristiti sljedeće da biste konfigurirali pozadinskog sustava mobilne aplikacije. 

    ![Pridruživanje aplikacija iz Windows trgovine](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Tajna klijenta i paket SID su važne sigurnosne vjerodajnice. Zajedničko korištenje ove vrijednosti sa svima i distribucija ih s aplikacijom. **Id aplikacije** se koristi s u tajna za konfiguriranje provjere autentičnosti Microsoftov Account.

##<a name="configure-the-backend-to-send-push-notifications"></a>Konfiguriranje pozadinskog da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Ažuriranje poslužitelja za slanje automatskih obavijesti

Korištenje postupak koji odgovara vašem pozadinskog vrsta projekta&mdash; [pozadinskog .NET](#dotnet) ili [Node.js pozadinskog](#nodejs).

### <a name="dotnet"></a>.NET pozadinskog projekta

1. U Visual Studio, desnom tipkom miša kliknite server project i kliknite **Upravljanje NuGet paketa**, potražite Microsoft.Azure.NotificationHubs, a zatim kliknite **Instaliraj**. Time se instalira klijentska biblioteka koncentratora obavijesti.

2. Proširite **kontrolera**, otvorite TodoItemController.cs i dodajte sljedeće pomoću naredbe:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. U načinu **PostTodoItem** nakon poziv **InsertAsync**dodati sljedeći kod:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Kod govori središte obavijesti da biste poslali obavijest o automatske novu stavku nakon unosa.

4. Ponovno objaviti server project.

### <a name="nodejs"></a>Node.js pozadinskog projekta

1. Ako ste još niste učinili, [Preuzmite brzi početak rada projekta](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) jer inače koristite [online editor na portalu za Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Zamijenite postojeće kod u datoteci todoitem.js sljedeće:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Šalje WNS skočnoj obavijesti koja sadrži na item.text umetanju novu stavku obveze.

2. Prilikom uređivanja datoteka na lokalnom računalu, ponovno objaviti server project.

##<a id="update-app"></a>Automatske obavijesti dodati aplikacije

Nakon toga aplikacije morate se registrirati za slanje obavijesti na pokretanja. Ako ste već omogućili provjeru autentičnosti, provjerite je li da korisnik znak u prije no što pokušate Registrirajte se za slanje obavijesti.

1. Otvorite datoteku **App.xaml.cs** projekta i dodajte sljedeće `using` izvješća:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. U istoj datoteci dodali sljedeću definiciju način **InitNotificationsAsync** predmete **aplikacije** :

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Kod dohvaća ChannelURI aplikacije iz WNS, a zatim registrira te ChannelURI s aplikacijom za mobilne aplikacije servisa.

3. Pri vrhu **OnLaunched** događajima u **App.xaml.cs**, dodajte mijenjanje **asinkrone** definiciju način i dodajte sljedeće poziv na nov način **InitNotificationsAsync** kao u sljedećem primjeru:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Time će se jamčiti short-lived ChannelURI je registrirana prilikom svakog pokretanja aplikacije.

4. Ponovno stvaranje projekta UWP aplikacije. Aplikacija je sada prima skočnoj obavijesti.

##<a id="test"></a>Testiranje automatske obavijesti u aplikaciji

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Daljnji koraci

Dodatne informacije o automatske obavijesti:

* [Kako koristiti upravljane klijent za Azure mobilne aplikacije](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Predlošci omogućuju fleksibilnost slanje ih gura različite platforme i lokalizirane ih gura. Saznajte kako registrirati predložaka.

* [Dijagnosticiranje problema automatske obavijesti](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Postoje razni razloga zašto obavijesti možda se prekine ili prekid prema gore na uređajima. U ovoj se temi objašnjava za analizu i utvrđivanje uzrok neuspjeha automatske obavijesti. 

Imajte na umu nastavka na jedan od sljedećih vodiči za:

+ [Dodavanje provjere autentičnosti za aplikaciju](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Upute za provjeru autentičnosti korisnika aplikacije s davateljem identiteta.

+ [Omogućivanje izvanmrežne sinkronizacije za aplikaciju](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Saznajte kako dodati aplikacije pomoću programa pozadinskog mobilnu aplikaciju Izvanmrežna podrška. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

