<properties
    pageTitle="Integracija Azure Mobile radnje Web SDK | Microsoft Azure"
    description="Najnovija ažuriranja i postupke za Azure Mobile radnje Web SDK-a"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integriranje Azure Mobile radnje u web-aplikaciji

> [AZURE.SELECTOR]
- [Univerzalni za Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Postupci u ovom se članku opisuju najjednostavniji način da biste aktivirali analitikom i nadzor funkcije Azure Mobile radnje u web-aplikaciji.

Slijedite korake da biste aktivirali izvješća zapisnika koji su potrebni za izračun svih statistike o korisnika, sesije, aktivnosti, ruši i technicals. Za statistiku ovisne aplikacija, kao što su događaji, pogrešaka i zadacima, morate aktivirati izvješća zapisnika ručno pomoću radnje API-JA Mobile Azure. Dodatne informacije, Saznajte [kako koristiti dodatne radnje Mobile označavanje API-JA u web-aplikaciji](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Uvod

[Preuzmite Web Azure mobilne radnje SDK](http://aka.ms/P7b453).
SDK Web radnje za mobilne otpremljene je kao u jednu datoteku JavaScript azure-engagement.js koje ćete morati uključiti u svakoj stranici web-mjesta ili web-aplikacije.

> [AZURE.IMPORTANT] Prije nego što pokrenete ovu skriptu, morate pokrenuti skripte ili kod isječak upisanog konfiguriranje radnje mobilne aplikacije.

## <a name="browser-compatibility"></a>Kompatibilnost s preglednikom

SDK Web Mobile radnje koristi nativni JSON kodiranje i dekodiranje, osim domenama AJAX zahtjeva (potrebe za oslanjanjem na specifikacija W3C CORS). Ovo je kompatibilan sa sljedećim preglednicima:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome, 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Konfiguriranje mobilnog radnje

Napisati skriptu koja stvara globalni `azureEngagement` objekt JavaScript, kao u sljedećem primjeru. Jer web-mjesto možda imaju višestrukih stranica, pretpostavlja se da je Ova skripta uključen u svake stranice. U ovom primjeru objekt JavaScript pod nazivom `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Na `connectionString` vrijednost za svoju aplikaciju prikazuje se na portalu za Azure.

> [AZURE.NOTE] `appVersionName`i `appVersionCode` nisu obavezni. No preporučujemo da konfigurirate ih tako da se analize možete obraditi informacije o verziji.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Uključiti mobilni radnje skripte na stranicama
Dodavanje Mobile radnje skripte na stranice na jedan od sljedećih načina:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Ili ovako:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Pseudonim

Kada skripte Mobile radnje Web SDK učita, stvorit će se **radnje** pseudonim za pristup API-ji SDK. Da biste definirali konfiguracije SDK nećete moći koristiti taj pseudonim. Pseudonima koristi se kao referencu u ove dokumentacije.

Imajte na umu da ako zadane pseudonim u sukobu s drugom globalna varijabla sa stranice, možete ponovno definirati ga u konfiguraciji na sljedeći način prije učitati Web SDK za mobilne radnje:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Osnovni izvješćivanje o pogreškama

Osnovni izvješćivanje o pogreškama u Mobile radnje pokriva statistike sesiju razine, kao što su statističkih podataka o korisnicima, sesije, aktivnosti i ruši.

### <a name="session-tracking"></a>Sesije za praćenje

Sesije Mobile radnje podijeljen u nizu aktivnosti, svaki otkrije naziv.

Klasični web-mjestu, preporučujemo da deklarirati različite aktivnosti na svakoj stranici web-mjesta. Za web-mjesta ili web-aplikaciju u kojem na trenutnoj stranici nikad ne mijenja, možda ćete morati pratiti aktivnosti manje mjerilo, kao što su unutar stranice.

U svakom slučaju, da biste započeli ili promijenite trenutne aktivnosti korisnika, nazovite na `engagement.agent.startActivity` (opis funkcije). Ako, na primjer:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Poslužitelj Mobile radnje automatski završava otvorenu sesiju u tri minute nakon zatvaranja aplikacije stranice.

Osim toga, možete završiti sesije ručno tako da nazovete `engagement.agent.endActivity`. Postavlja trenutne aktivnosti korisnika za stanje 'neaktivnosti."  Sesije će završiti kasnije 10 sekundi osim ako novi poziv `engagement.agent.startActivity` primijenila sesiju.

Možete konfigurirati odgode 10 sekundi u objektu globalni radnje, na sljedeći način:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Ne možete koristiti `engagement.agent.endActivity` u na `onunload` povratnog jer ne možete upućivati pozive za AJAX-a u ovoj fazi.

## <a name="advanced-reporting"></a>Dodatna izvješća

Po želji, ako želite da biste prijavili specifičnim aplikacijama događaje, pogrešaka i zadacima, trebate koristiti API radnje Mobile. Pristup API radnje Mobile putem na `engagement.agent` objekt.

Možete pristupiti svim naprednih mogućnosti u Mobile radnje u Mobile radnje API-JA. U API je detaljan u članku [kako koristiti dodatne radnje Mobile označavanje API-JA u web-aplikaciji](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>Prilagodba URL-ovi koriste za pozive AJAX-a

Možete prilagoditi URL-ovi koji koristi Web SDK za mobilne radnje. Na primjer, da biste ponovno definirali URL zapisnika (SDK krajnja točka za zapisivanje), možete nadjačati konfiguraciju ovako:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Ako vaš URL vraćaju niza koji započinje `/`, `//`, `http://`, ili `https://`, Zadana shema ne koristi. Po zadanom se `https://` sheme služi za te URL-ove. Ako želite da biste prilagodili zadanu shemu, nadjačati konfiguraciju, ovako:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
