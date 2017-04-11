<properties
    pageTitle="Dodavanje automatske obavijesti Apache Cordova aplikaciju s Azure mobilne aplikacije za | Aplikacije servisa za Azure"
    description="Saznajte kako koristiti Azure mobilne aplikacije za slanje obavijesti poslati Apache Cordova aplikacije."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Aplikaciju za Apache Cordova dodati automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Pregled

U ovom ćete praktičnom vodiču dodate automatske obavijesti projekt [Apache Cordova Brzi start] tako da se automatske obavijesti poslati na uređaju svaki put kada se umetne zapis.

Ako ne koristite project server preuzete brzi početak rada, morate paket za nastavak automatske obavijesti. Dodatne informacije potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča pokriva aplikaciju Apache Cordova razvio s Visual Studio 2015 koja se izvršava na Google Emulator za Android, uređaju sa sustavom Android, uređaju sa sustavom Windows i uređaju sa sustavom iOS.

Da biste dovršili ovaj Praktični vodič, potrebno je:

* Na Računalu sa [Visual Studio zajednice 2015] ili novije verzije.
* [Visual Studio Tools for Apache Cordova].
* [Račun za Azure active](https://azure.microsoft.com/pricing/free-trial/).
* Dovršeni [Apache Cordova koji se brzi početak rada] projekta.
* (Android) Na [Google račun] pomoću adrese e-pošte potvrđeni.
* (iOS) Članstvo u Program za razvojne inženjere za Apple i uređaju sa sustavom iOS (iOS Simulator ne podržava automatske).
* (Windows) Račun za razvojne inženjere za iz trgovine sustava Windows i uređaj sa sustavom Windows 10.

##<a name="configure-hub"></a>Konfiguriranje obavijesti koncentratora

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Pogledajte videozapis koji prikazuje korake u ovom odjeljku](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Ažuriranje project server da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Izmjena aplikacije Cordova da biste primajte automatske obavijesti

Provjerite nalazi li se aplikacija projekta Apache Cordova jeste li spremni za rukovanje automatske obavijesti tako da instalirate dodatak za automatske Cordova plus servisi ovisne automatske.

#### <a name="update-the-cordova-version-in-your-project"></a>Ažuriranje verzije Cordova u projektu.

Preporučujemo da ih ažurirate projekta klijent Cordova 6.1.1 ako projekt je konfiguriran pomoću starije verzije. Da biste ažurirali projekt, desnom tipkom miša kliknite config.xml da biste otvorili dizajner konfiguracije. Odaberite karticu platforme i odaberite 6.1.1 **Cordova EŽA** tekstni okvir.

Odaberite **Stvaranje**, a zatim **Stvaranje rješenja** da biste ažurirali projekt.

#### <a name="install-the-push-plugin"></a>Instalirajte dodatak za slanje

Aplikacija Apache Cordova rukovati nativno uređajem ili mrežom mogućnosti.  Dodaci koje su objavljene na [npm](https://www.npmjs.com/) ili GitHub nudi sljedeće mogućnosti.  Na `phonegap-plugin-push` dodatak koristi se za rukovanje mreže automatske obavijesti.

Možete instalirati dodatak za slanje u neki od sljedećih načina:

**Iz u naredbenom retku:**

Pokrenite sljedeću naredbu:

    cordova plugin add phonegap-plugin-push

**Iz programa Visual Studio:**

1.  U pregledniku rješenja, otvorite na `config.xml` datoteke kliknite **Dodaci** > **Prilagođeno**odaberite **brojka** kao izvor instalacije, a zatim unesite `https://github.com/phonegap/phonegap-plugin-push` kao izvor.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Kliknite strelicu pokraj izvor instalacije.

3. U **SENDER_ID**, ako već imate ID numerički projekta za projekt konzole za razvojne inženjere Google možete dodati ga ovdje. U suprotnom, unesite vrijednost rezervirano mjesto, kao što su 777777, i ako birate Android možete ažurirati tu vrijednost u config.xml kasnije.

4. Kliknite **Dodaj**.

Sada je instaliran dodatak za slanje.

####<a name="install-the-device-plugin"></a>Instalirajte dodatak za uređaj

Slijedite isti postupak koristite da biste instalirali dodatak za slanje, ali ćete pronaći dodatak za uređaj na popisu dodataka Core (kliknite **Dodaci** > **Core** da biste ga pronašli). Morate ovaj dodatak da biste dobili naziv platformu (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Registracija uređaja radi automatske na pokretanja

Na početku, ne možemo neće sadržavati stavku neke minimalnog kod za Android. Kasnije, ne možemo će izmijenite neke small da biste pokrenuli za iOS ili Windows 10.

1. Dodavanje poziv **registerForPushNotifications** tijekom povratnog postupka prijave ili pri dnu **onDeviceReady** metoda:

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    U ovom se primjeru prikazuje pozivanje **registerForPushNotifications** Nakon uspješnog provjeru autentičnosti, koji se preporučuje prilikom korištenja automatske obavijesti i provjeru autentičnosti u aplikaciji.

2. Nova metoda **registerForPushNotifications** dodajte na sljedeći način:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) U gornjem kod zamijenite `Your_Project_ID` s numeričkim project ID-a za aplikacije s [Konzole za razvojne inženjere za Google].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Neobavezno) Konfiguriranje i pokretanje aplikacija na sustavu Android

Dovršite ovaj odjeljak da biste omogućili slanje obavijesti za Android.

####<a name="enable-gcm"></a>Omogućivanje Firebase Cloud razmjene poruka

Jer smo birate platforme Google Android prethodno, morate omogućiti Firebase oblaka poruka. Isto tako, ako su ciljanja uređaja Microsoft Windows, omogućiti WNS podršku.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>Konfiguriranje pozadinskog mobilnu aplikaciju slanje zahtjeva za automatske pomoću FCM

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Konfiguriranje Cordova aplikacija za Android

Aplikaciji Cordova otvoriti config.xml i zamijeniti `Your_Project_ID` s numeričkim project ID-a za aplikacije s [Konzole za razvojne inženjere za Google].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Otvaranje index.js i ažuriranje kod da biste koristili svoj ID numerički projekta.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Konfiguriranje uređaju sa sustavom Android za ispravljanje pogrešaka USB

Prije nego što možete implementirati aplikacije na uređaju sa sustavom Android, morate omogućiti USB ispravljanje pogrešaka.  Na telefonu sa sustavom poduzeti sljedeće korake:

1. Idite na **Postavke** > **o telefon**, a zatim dodirnite na **broj izdanja** do programerskom načinu omogućena (oko 7 puta).

2. Vratite se u **postavkama** > **Mogućnosti za razvojne inženjere** omogućiti **USB ispravljanje pogrešaka**, a zatim povezati telefon s Androidom razvoj računalo pomoću USB kabela.

Ne možemo testira to pomoću s uređaja X Google Nexus 5 sa sustavom Android 6.0 (uskršnje).  Međutim, tehnika su uobičajeni preko bilo koje izdanje Moderna sa sustavom Android.

#### <a name="install-google-play-services"></a>Instaliranje servisa Google Play

Dodatak za automatske ovisi o Android Google Play s servisi za slanje obavijesti.  

1.  U **Visual Studio**, kliknite **Alati** > **Android** > **Android SDK Upravitelj**, proširite mapu **Dodaci** i potvrdite okvir da biste provjerili je li svaki od sljedećih SDK-ovi instaliran.
    * Android 2.3 ili noviji
    * Spremište Google revizije 27 ili noviji
    * Servisi Google Play 9.0.2 ili noviji

2.  Kliknite **Instaliranje paketa** i pričekajte da biste dovršili instalaciju.

Trenutni potrebna biblioteka navedene su u [dokumentaciji phonegap dodatak automatske instalacije].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Testiranje automatske obavijesti u aplikacija u sustavu Android

Sada probno automatske obavijesti to ćete učiniti tako pokrenuti aplikaciju i umetanje stavki u tablici TodoItem. To možete s istom uređaju ili neki drugi uređaj, pod uvjetom da koristite isti pozadinskog. Testiranje Cordova aplikacije na platformi Android na jedan od sljedećih načina:

- **Na uređaju fizičke:**  
Priloži uređaj sa sustavom Android s računalom razvoj putem USB kabela.  Umjesto **Google Android Emulator**odaberite **uređaj**. Visual Studio će implementaciju aplikacija na uređaju te ga pokrenuti.  Zatim možete raditi s računala na uređaju.  
Poboljšanju razvoj.  Zaslon zajedničko korištenje aplikacija kao što je [Mobizen] može vam pomoći pri razvoju aplikacija sa sustavom Android tako da ste aktivirali projektor Android zaslona u web-pregledniku na PC-JU.

- **Na Android emulator:**  
Postoje dodatni koraci konfiguriranja potrebna kada se pokrene s programa emulator.

    Provjerite je li se za implementaciju u ili ispravljanje pogrešaka na virtualnog uređaja koji je Google API-ji Postavi kao cilj, kao što je prikazano u nastavku u upravitelju Android virtualne uređaja (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Ako želite koristiti brže x86 emulator, [Instaliranje upravljačkog programa za HAXM](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) i konfiguriranje na emulator ga koristiti.

    Dodajte Google račun Android uređaj tako da kliknete **aplikacije** > **Postavke** > **Dodavanje računa**, a zatim slijedite upute da biste dodali postojeće Google račun na uređaju (preporučujemo korištenje postojeći račun umjesto stvaranja novog).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Pokrenite aplikaciju todolist kao prije i umetnite novu stavku obveze. Ovaj put u području obavijesti prikazuje se ikona obavijesti. Možete otvoriti ladica obavijesti da biste prikazali cijeli tekst obavijesti.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Neobavezno) Konfiguriranje i pokrenuti iOS

U ovom se odjeljku je radi Cordova projekt na uređajima sa sustavom iOS. Ako ne radite s uređajima sa sustavom iOS, preskočite ovaj odjeljak.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Instalirajte i pokrenite agent za iOS remotebuild servis za Mac ili oblaka

Prije pokretanja Cordova aplikacije na iOS pomoću Visual Studio, poduzmite korake u [iOS vodič za postavljanje](http://taco.visualstudio.com/en-us/docs/ios-guide/) za instalaciju i pokretanje remotebuild agent.

Provjerite je li moguće stvoriti aplikaciju za iOS. Koraci u vodič za postavljanje su potrebne za sastavljanje za iOS iz Visual Studio. Ako nemate Mac, možete izraditi za iOS pomoću remotebuild agent na neki servis kao što je MacInCloud. Dodatne informacije potražite u članku [pokretanje aplikacije iOS u oblaku](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] Da biste koristili dodatak za slanje na iOS potreban je XCode 7 ili noviji.

####<a name="find-the-id-to-use-as-your-app-id"></a>Pronalaženje ID-a da biste upotrijebili identifikacijskog Broja za aplikaciju

Prije registrirali aplikacije za slanje obavijesti, otvaranje config.xml aplikaciji Cordova pronađite u `id` atribut element miniaplikacije i njegovo kopiranje za kasnije korištenje. U sljedećim XML ID je `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

U ovom identifikator koristiti kasnije, prilikom stvaranja aplikacije ID tvrtke Apple portala za razvojne inženjere. (Ako stvorite drugačiji ID aplikacije portala za razvojne inženjere, a želite koristiti taj, morat ćete poduzeti nekoliko dodatnih koraka u nastavku ovog praktičnog vodiča da biste promijenili taj ID u config.xml. ID-a element miniaplikacije moraju podudarati ID aplikacije portala za razvojne inženjere.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registracija aplikacije za slanje obavijesti na Appleova portala za razvojne inženjere

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Pogledajte videozapis koji prikazuje slične korake](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Konfiguriranje Azure da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Provjerite da odgovara identifikacijskog Broja za aplikaciju Cordova aplikacije

Ako već stvorene na vašem računu za razvojne inženjere Apple ID aplikacije udovoljava ID miniaplikacije element u config.xml, možete preskočiti ovaj korak. Međutim, ako se ne podudaraju ID-ove, učinite sljedeće:

1. Izbrišite mapu platforme iz projekta.

2. Izbrišite mapu dodataka iz projekta.

3. Izbrišite mapu node_modules iz projekta.

4. Ažurirajte atribut id miniaplikacije elementa u config.xml da biste koristili aplikaciju ID-a koji ste stvorili na vašem računu za razvojne inženjere Apple.

5. Ponovno stvaranje projekta.

#####<a name="test-push-notifications-in-your-ios-app"></a>Testiranje automatske obavijesti u aplikaciju za iOS

1. U Visual Studio, obavezno **iOS** kao cilj implementaciju, a zatim **uređaj** da biste pokrenuli na uređaju povezani sa sustavom iOS.

    Možete pokrenuti na uređaju sa sustavom iOS povezani s Računalom putem iTunes. IOS Simulator ne podržava slanje obavijesti.

2. Pritisnite gumb za **pokretanje** ili **F5** u Visual Studio omogućuje stvaranje projekta i pokretanje aplikacija na uređaju sa sustavom iOS, a zatim kliknite **u redu** da biste prihvatili automatske obavijesti.

    >[AZURE.NOTE] Morate izričito prihvatili automatske obavijesti iz aplikacije. Ovaj zahtjev pojavljuje se samo prvi put da se pokreće aplikaciju.

3. U aplikaciji, upišite zadatak, a zatim kliknite znak plus (+) ikona.

4. Provjerite je li da obavijest primitka, a zatim kliknite u redu da biste odbacili obavijest.

##<a name="optional-configure-and-run-on-windows"></a>(Neobavezno) Konfiguriranje i izvode u sustavu Windows

U ovom se odjeljku je za pokretanje aplikacije project Apache Cordova na uređajima Windows 10 (automatske dodatak PhoneGap podržana za Windows 10). Ako ne radite s uređaje sa sustavom Windows, preskočite ovaj odjeljak.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrirajte Windows aplikacije za slanje obavijesti s WNS

Da biste koristili mogućnosti iz trgovine u Visual Studio, odaberite ciljni Windows na popisu rješenje platforme kao što su **Windows x64** ili **Windows x86** (izbjegli **Windows AnyCPU** za slanje obavijesti).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Pogledajte videozapis koji prikazuje slične korake](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Konfiguriranje obavijesti koncentrator za WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Konfiguriranje Cordova aplikacije za podršku slanje obavijesti za Windows

Otvorite designer konfiguracije (desnom tipkom miša kliknite config.xml i odaberite **Dizajner prikaza**), odaberite karticu **sustava Windows** pa odaberite **Windows 10** u odjeljku **Windows ciljne verzije**.

>[AZURE.NOTE] Ako koristite Cordova verziju prije no što Cordova 5.1.1 (6.1.1 preporučuje se), morate postaviti i skočnoj instaliranih zastavice na true u config.xml.

Za podršku automatske obavijesti u zadanoj (Provjera pogrešaka) sastavlja, build.json otvorene datoteke. Kopirajte konfiguracije "izdanje" konfiguraciju ispravljanje pogrešaka.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Nakon ažuriranja, prethodni kod trebao bi izgledati ovako.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Stvaranje aplikacije i provjerite jeste li bez pogrešaka. Klijent aplikacije sada trebala bi se registrirati za obavijesti iz pozadine za mobilnu aplikaciju. U ovom se odjeljku ponovite za svaki projekt programa Windows u rješenje.

####<a name="test-push-notifications-in-your-windows-app"></a>Testiranje automatske obavijesti u aplikacija za Windows

U Visual Studio, provjerite je li platformu sustava Windows kao cilj implementaciju, kao što su **Windows x64** ili **Windows x86**. Da biste pokrenuli aplikaciju na Windows 10 PC-JU hosting Visual Studio, odaberite **Lokalnog računala**.

Pritisnite gumb Pokreni omogućuje stvaranje projekta i pokrenite aplikaciju.

U aplikaciji, upišite naziv nove todoitem, a zatim kliknite znak plus (+) ikonu da biste je dodali.

Provjerite je li je primili obavijest prilikom dodavanja stavke.

##<a name="next-steps"></a>Daljnji koraci

* Saznajte više o [Obavijesti koncentratora] dodatne informacije o automatske obavijesti.
* Ako već niste učinili, i dalje Praktični vodič tako da [Dodate provjere autentičnosti] za aplikaciju Apache Cordova.

Saznajte kako koristiti u SDK-ovi.

* [Apache Cordova SDK]
* [Poslužitelj ASP.NET SDK]
* [Poslužitelj Node.js SDK]

<!-- URLs -->
[Dodavanje provjere autentičnosti]: app-service-mobile-cordova-get-started-users.md
[Brzi početak Apache Cordova]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google račun]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Konzola za razvojne inženjere za Google]: https://console.developers.google.com/home/dashboard
[Dokumentacija phonegap dodatak automatske instalacije]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio zajednice 2015.]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Obavijest o koncentratora]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Poslužitelj ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Poslužitelj Node.js SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
