<properties
    pageTitle="Automatske obavijesti poslati Chrome aplikacije s koncentratorima obavijesti Azure | Microsoft Azure"
    description="Saznajte kako koristiti koncentratora Azure obavijesti da biste poslali automatske obavijesti aplikacija za Chrome."
    services="notification-hubs"
    keywords="Mobilni automatske obavijesti, slanje obavijesti automatske obavijesti, chrome, slanje obavijesti"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Automatske obavijesti poslati Chrome aplikacije s koncentratorima obavijesti Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

U ovoj se temi objašnjava korištenje koncentratora Azure obavijesti da biste poslali automatske obavijesti na aplikaciju za Chrome, koji će se prikazati u kontekstu preglednika Google Chrome. U ovom ćete praktičnom vodiču smo će stvoriti aplikaciju za Chrome koja prima slanje obavijesti putem [Razmjene poruka Google Cloud (GCM)](https://developers.google.com/cloud-messaging/). 

>[AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Vodič vodit će vas kroz ove osnovne korake da biste omogućili slanje obavijesti:

* [Omogućivanje Google Cloud razmjene poruka](#register)
* [Konfiguriranje koncentratora za obavijesti](#configure-hub)
* [Povezivanje aplikacije Chrome koncentrator obavijesti](#connect-app)
* [Automatske obavijesti poslati aplikacije vizualnog okvira](#send)
* [Dodatne funkcije i mogućnosti](#next-steps)

>[AZURE.NOTE] Automatske obavijesti aplikacija za chrome nisu generički obavijesti u pregledniku – su specifične za proširivanje preglednika model (pogledajte [Pregled aplikacija za Chrome] detalje). Uz preglednik za stolna računala vizualnog aplikacije izvoditi na mobile (Android i iOS) do Apache Cordova. Pročitajte članak [Chrome aplikacija na mobilne] da biste saznali više.

Konfiguriranje GCM i Azure obavijesti koncentratora jednak konfiguriranja za Android, jer zastarjela [Razmjene poruka Google Cloud za Chrome] i iste GCM sada podržava uređaje sa sustavom Android i Chrome instance.

##<a id="register"></a>Omogućivanje Google Cloud razmjene poruka

1. Dođite do web-mjesta [Google Cloud konzole] , prijavite se pomoću vjerodajnica Google račun, a zatim gumb **Stvori projekt** . Upišite odgovarajući **Naziv projekta**, a zatim kliknite gumb **Stvori** .

    ![Google Cloud konzole – Stvaranje projekta][1]

2. Zabilježite **Broj projekta** na stranici **projekata** projekta koji ste upravo stvorili. Koristit ćete to kao **GCM ID pošiljatelja** u aplikaciji Chrome za registriranje GCM.

    ![Google Cloud konzole - broj projekta][2]

3. U lijevom oknu kliknite **API-ji & auth**, i pomaknite se prema dolje, a zatim Preklopni da biste omogućili **Google Cloud poruka za Android**. Ne morate omogućiti **Razmjene poruka Google Cloud za Chrome**.

    ![Google Cloud konzole - ključ poslužitelja][3]

4. U lijevom oknu kliknite **vjerodajnice** > **Stvaranje novog ključa** > **Poslužitelja ključ** > **Stvori**.

    ![Google Cloud konzole - vjerodajnice][4]

5. Zabilježite poslužitelja **Ključ za API -JA**. Ćete konfigurirati to u koncentratora za obavijesti nakon toga da biste omogućili slanje obavijesti poslati GCM.

    ![Google Cloud konzole - ključ za API-JA][5]

##<a id="configure-hub"></a>Konfiguriranje koncentratora za obavijesti

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. u plohu **Postavke** odaberite **Servise obavijesti** , a zatim **Google (GCM)**. Unesite ključ za API-JA i spremiti.

&emsp;&emsp;![Azure obavijesti koncentratora - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Povezivanje aplikacije Chrome koncentrator obavijesti

Koncentratora za obavijesti sada je konfiguriran za rad s GCM, a imate nizove veze da biste registrirali aplikacije i primati i slati automatske obavijesti. LK

###<a name="create-a-new-chrome-app"></a>Stvorite novu aplikaciju vizualnog okvira

Ispod uzorka temelji se na [Ogledne GCM aplikacija za Chrome] i koristi preporučeni način stvaranja aplikacija za Chrome. Ne možemo će istaknuti korake specifičnih za Azure obavijesti koncentratora. 

>[AZURE.NOTE] Preporučujemo da preuzimanje izvorišnog web-mjesta za Chrome aplikacije iz [Chrome aplikacije obavijesti koncentrator uzorka].

Aplikacija za Chrome stvorena je pomoću JavaScript, a možete koristiti bilo koji vaše uređivači preferiranog programa word za stvaranje. U nastavku je ta aplikacija za Chrome izgleda.

![Google Chrome aplikacije][15]

1. Stvaranje mape i nazovite ih `ChromePushApp`. Naravno, naziv je proizvoljne – ako ste nazvali je nešto drugo, provjerite je li zamijenite put unutar potreban kod segmenata.

2. Preuzmite [šifriranu js biblioteke] u mapi koju ste stvorili u drugom koraku. Ova mapa biblioteke će sadržavati dva podmape: `components` i `rollups`.

3. Stvaranje na `manifest.json` datoteku. Sve aplikacije Chrome se sigurnosno manifesta datoteka koja sadrži metapodatke aplikacije i, većina važno je napomenuti da, sve dozvole koje su dodijeljene aplikaciju kada korisnik instalira ga.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Obavijest na `permissions` element koji određuje Chrome aplikacija će se moći primajte automatske obavijesti iz GCM. Morate navesti i URI koncentratora obavijesti Azure gdje aplikaciju Chrome će uputiti poziv OSTALE da biste registrirali.
    Naša aplikacija za uzorak i koristi datotečni ikona `gcm_128.png`, koji ćete pronaći na izvor koji se koristi s izvornog uzorka GCM. Možete je zamijeniti za bilo koju sliku koja odgovara [ikona kriterija](https://developer.chrome.com/apps/manifest/icons).

4. Stvoriti datoteku pod nazivom `background.js` s sljedeći kod:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Ovo je datoteka koje se skočni prozor aplikacije Chrome HTML (**register.html**) i određuje rukovatelj **messageReceived** učiniti dolazne automatske obavijesti.

5. Stvoriti datoteku pod nazivom `register.html` – to definira korisničkog Sučelja aplikacije za Chrome. 

   >[AZURE.NOTE] Ovaj primjer koristi **CryptoJS v3.1.2**. Ako ste preuzeli neku drugu verziju sustava u biblioteku, provjerite je li pravilno zamijeniti u verzija na `src` put.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Stvoriti datoteku pod nazivom `register.js` koda u nastavku. Tu datoteku određuje skripte iza `register.html`. Aplikacija za chrome ne dopuštaju izvršavanje u istoj razini, tako da morate stvoriti zasebnu sigurnosnom skriptu za vaše korisničko Sučelje.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Iznad skripte sastoji se od sljedećih parametara ključa:
    - **Window.onLoad** definira događaje klikom na gumb dva gumba korisničkog Sučelja. Jedan dnevnike s GCM, a drugi koristi ID Registracija koja je vraćena nakon registracije s GCM da biste registrirali s koncentratorima Azure obavijesti.
    - **updateLog** je funkcija koja omogućuje nam učiniti jednostavne zapisivanje mogućnosti.
    - **registerWithGCM** je prvi rukovatelj klikom na gumb, što čini u `chrome.gcm.register` poziva GCM da biste registrirali trenutne instance aplikacija za Chrome.
    - **registerCallback** je funkcija povratnog koji se naziva kada pronađe registraciju poziva GCM.
    - **registerWithNH** je drugi rukovatelj klikom na gumb koji registrira s koncentratorima obavijesti. To može vidjeti `hubName` i `connectionString` (koji korisnik naveo) i crafts poziv obavijesti koncentratora Registracija REST API-JA.
    - **splitConnectionString** i **generateSaSToken** su helpers koji predstavljaju implementacije JavaScript postupak tokena stvaranja SaS koje valja koristiti u svim pozive REST API-JA. Dodatne informacije potražite u članku [Uobičajenih koncepata](http://msdn.microsoft.com/library/dn495627.aspx).
    - **sendNHRegistrationRequest** je funkcija koja omogućuje HTTP OSTALE poziva s koncentratorima Azure obavijesti.
    - **registrationPayload** definira XML opseg registracije. Dodatne informacije potražite u članku [Stvaranje Registracija NH REST API -JA]. Ažuriramo Registracija ID-a u njemu s što smo dobili od GCM.
    - **klijent** je instance komponente **XMLHttpRequest** koji koristimo za upućivanje zahtjeva za HTTP POST. Imajte na umu da ažuriramo na `Authorization` zaglavlja s `sasToken`. Uspješan dovršetak ovog poziva će registrirati ovu instancu Chrome aplikacije s koncentratorima Azure obavijesti.


Ukupna struktura mapa za taj projekt oblik treba sličan to:     ![Google Chrome aplikaciju – struktura mapa][21]

###<a name="set-up-and-test-your-chrome-app"></a>Postavljanje i testiranje aplikacije vizualnog okvira

1. Otvorite preglednik Chrome. Otvorite **Chrome proširenja** i omogućivanje **načina rada za razvojne inženjere**.

    ![Google Chrome - Omogući način rada za razvojne inženjere][16]

2. Kliknite **Učitaj raspakirane kućni broj** , a zatim dođite do mape u koju ste stvorili datoteke. Također možete koristiti u **Chrome aplikacije i alat za razvojne inženjere proširenja**. Ovaj alat je aplikacija za Chrome u samu (instalirali iz trgovine Web Chrome) i njihovi naprednih mogućnosti za ispravljanje pogrešaka za vaše razvoj aplikacija za Chrome.

    ![Google Chrome - učitavanje raspakirane proširenja][17]

3. Ako bez pogreške je stvorena aplikacija za Chrome, će biti navedeno Chrome aplikacije prikazuju.

    ![Google Chrome – prikaz aplikacija za Chrome][18]

4. Unesite **Broj projekta** koju ste ranije dobili **Google Cloud konzole** kao ID pošiljatelja, a kliknite **morate registrirati GCM**. Mora se prikazati poruka **Registracija s GCM uspjela.**

    ![Google Chrome - prilagodbe aplikacija za Chrome][19]

5. Unesite svoje **Ime koncentrator obavijesti** i **DefaultListenSharedAccessSignature** koje ste dobili na portalu ranije pa kliknite **morate registrirati koncentrator Azure obavijesti**. Mora se prikazati poruka **obavijesti koncentrator Registracija uspješno!** i detalje o odgovor Registracija koja sadrži Registracija Azure obavijesti koncentratora ID-a.

    ![Google Chrome - navedite detalje koncentrator obavijesti][20]  

##<a name="send"></a>Pošaljite obavijest u aplikaciju za Chrome

Za testiranje, ne možemo poslat će Chrome automatske obavijesti pomoću programa .NET konzole aplikacije. 

>[AZURE.NOTE] Automatske obavijesti s obavijesti koncentratora možete poslati iz bilo kojeg pozadine putem naš javno <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">OSTALE sučelja</a>. Pogledajte naš [portal dokumentaciju](https://azure.microsoft.com/documentation/services/notification-hubs/) za više primjera različite platforme.

1. U Visual Studio, na izborniku **datoteka** , odaberite **Novo** , a zatim **projekta**. U odjeljku **Visual C#**, kliknite **Windows** i **Aplikacije konzole**, a zatim kliknite **u redu**.  Time se stvara novi projekt aplikacije konzole.

2. Na izborniku **Alati** kliknite **Upravitelj paketa biblioteke** , a zatim **Konzole za Upravitelj paketa**. Prikazat će se konzole za Upravitelj paketa.

3. U prozoru konzole, pokrenite sljedeću naredbu:

        Install-Package Microsoft.Azure.NotificationHubs

    Time se dodaje referenca SDK Bus servisa Azure s <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet paketa</a>.

4. Otvaranje `Program.cs` i dodajte sljedeće `using` izjava:

        using Microsoft.Azure.NotificationHubs;

5. U na `Program` klase, dodajte na sljedeći način:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Provjerite je li da biste zamijenili na `<hub name>` rezervirano mjesto s nazivom središtu obavijesti koji se pojavljuje na [portal](https://portal.azure.com) u svoje plohu koncentrator obavijesti. Osim toga, rezervirano mjesto za niz veze zamijenili niz za povezivanje naziva `DefaultFullSharedAccessSignature` koji ste nabavili u odjeljku Konfiguriranje koncentrator za obavijesti.

    >[AZURE.NOTE] Provjerite je li koristiti niz za povezivanje s **potpunim** pristupom, nije **preslušali** pristup. Niz veze **preslušali** pristup Dodjela dozvola za slanje automatskih obavijesti.

5. Dodajte sljedeće pozive u na `Main` metoda:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Provjerite je li pokrenut Chrome i pokrenite konzolu aplikaciju.

7. Vidjet ćete sljedeću obavijest skočnim na radnoj površini.

    ![Google Chrome – obavijesti][13]

8. Pomoću prozora preglednika Chrome obavijesti na programskoj traci (u sustavu Windows) možete vidjeti sve obavijesti kada radi vizualnog okvira.

    ![Google Chrome - popis obavijesti][14]

>[AZURE.NOTE] Ne morate imati pokretanje aplikacija za Chrome ili otvorite u web-pregledniku (iako preglednika Chrome sam mora biti pokrenut). Dobit ćete konsolidirani prikaz svih obavijesti u prozoru preglednika Chrome obavijesti.

## <a name="next-steps"> </a>Sljedeće korake

Saznajte više o koncentratora obavijesti u [Pregled koncentratora obavijesti].

Da biste ciljnu određene korisnike, pogledajte vodič za [Azure obavijesti koncentratora obavijestite korisnike] . 

Ako želite fazi korisnika po grupama kamatu, možete pratiti vodič [Azure obavijesti koncentratora najnovije vijesti] .

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome aplikacije obavijesti koncentrator uzorka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud konzole]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Pregled koncentratora obavijesti]: notification-hubs-push-notification-overview.md
[Pregled aplikacija za chrome]: https://developer.chrome.com/apps/about_apps
[Ogledna aplikacije GCM vizualnog okvira]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome aplikacija na mobilne]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Stvaranje NH Registracija REST API-JA]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[šifriranu js biblioteke]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud razmjene poruka za Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure obavijesti koncentratora obavještavanje korisnika]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure važnih obavijesti koncentratora vijesti]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
