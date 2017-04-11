<properties
    pageTitle="Kako koristiti Apache Cordova dodatak za Azure mobilne aplikacije"
    description="Kako koristiti Apache Cordova dodatak za Azure mobilne aplikacije"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Kako koristiti Apache Cordova klijentska biblioteka za Azure mobilne aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ovaj vodič ručica za izvođenje uobičajeni scenariji pomoću najnovije [Apache Cordova dodatak za Azure mobilne aplikacije]. Ako ste novi korisnik aplikacije Mobile Azure, prvi cjelovit [Azure mobilne aplikacije brzi početak rada] da biste stvorili pozadinskog, stvorite tablicu i preuzimanje ugrađene Apache Cordova projekta. U ovom vodiču smo fokus na klijentskoj strani Apache Cordova dodatak.

## <a name="supported-platforms"></a>Podržane platforme

U ovom SDK podržava Apache Cordova v6.0.0 i kasnije iOS, Android i Windows uređaja.  Podrška za platformu je na sljedeći način:

* Android API 19 24 (KitKat do Nougat)
* iOS 8.0 i novije verzije.
* Windows Phone 8.0
* Windows Phone 8.1
* Univerzalni Windows platforma

##<a name="Setup"></a>Postavljanje i preduvjeti

Ovaj vodič pretpostavlja da ste stvorili s pozadinskom s tablicom. Ovaj vodič pretpostavlja da tablica ima istu shemu kao tablice u tim vodičima. Ovaj vodič i pretpostavlja da ste dodali dodatak za Cordova Apache kod.  Ako niste učinili, možete dodati dodatak Apache Cordova projekta u naredbenom retku:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Dodatne informacije o stvaranju [prvi Apache Cordova aplikacije]potražite u članku njihovu dokumentaciju.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Kako: provjeru autentičnosti korisnika

Aplikacije servisa za Azure podržava provjere autentičnosti i dopuštanja korisnici aplikacije pomoću davatelja usluge za različite vanjskih identiteta: Facebook, Google, Microsoftov Account i Twitter. Postavljanje dozvola za tablice ograničavanja pristupa za određene operacije samo korisnicima čija je autentičnost provjerena. Identitet korisnika čija je autentičnost provjerena možete koristiti i za primjenu pravila za provjeru autentičnosti u skriptama poslužitelja. Dodatne informacije potražite u članku vodič za [Početak rada s provjerom autentičnosti] .

Kada pomoću provjere autentičnosti u aplikaciju Apache Cordova, dodaci za sljedeće Cordova moraju biti dostupne:

* [cordova, dodatak i uređaji]
* [cordova dodatak inappbrowser]

Podržane su dvije tokova provjere autentičnosti: na poslužitelju i klijent tijek.  Tijek server nudi najjednostavniji sučelje za provjeru autentičnosti kao ovisi vašeg davatelja usluge web-sučelja za provjeru autentičnosti. Tijek klijent omogućuje dublju Integracija s mogućnosti specifične za uređaj kao što su-jedinstvene prijave kao ovisi SDK-ovi za specifične za uređaj karakteristične za davatelja.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Kako: Konfiguriranje mobilne aplikacije servisa za vanjske preusmjeravanja URL-ove.

Nekoliko vrsta Apache Cordova aplikacije pomoću povratna mogućnost za rukovanje tokova OAuth korisničkog Sučelja.  Korisničko Sučelje OAuth tokova na localhost uzrokovati probleme jer servis za provjeru autentičnosti samo zna upute za korištenje na servisu prema zadanim postavkama.  Problematična korisničkog Sučelja OAuth tokova Primjeri:

- Emulator val.
- Live ponovno Učitaj s Ionic.
- Mobilni pozadinskog izvodi lokalno
- Mobilni pozadinskog izvodi u različite servisne aplikacije Azure od jednog providing provjeru autentičnosti.

Slijedite ove upute za dodavanje lokalne postavke konfiguracije:

1. Prijavite se na [portal za Azure]
2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv mobilnu aplikaciju.
3. Kliknite **Alati**
4. Na izborniku OBSERVE kliknite **explorer resursa** , a zatim kliknite **Idi**.  Otvorit će se novi prozor ili tabulator.
5. Proširite **config**, **authsettings** čvorove web-mjesta u lijevom navigacijskom oknu.
6. Kliknite **Uređivanje**
7. Obratite pažnju na element "allowedExternalRedirectUrls".  Može biti postavljena na null ili polje s vrijednostima.  Promijenite vrijednost u sljedeću vrijednost:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Zamijenite URL-ove URL-ove usluge.  Primjeri "http://localhost:3000" (na primjer servis Node.js) ili "http://localhost:4400" (za servis Val).  Međutim, URL-ove su primjeri - situaciji, uključujući za servise koji se spominju u primjerima mogu se razlikovati.
8. Kliknite gumb za **Čitanje/pisanje** u gornjem desnom kutu zaslona.
9. Kliknite na zeleni gumb **STAVITI** .

Postavke spremaju se sada.  Zatvorite prozor preglednika dok ne dovršite postavke spremanja.
Dodati URL-ove povratna CORS postavki aplikacije servisa:

1. Prijavite se na [portal za Azure]
2. Odaberite **sve resurse** ili **Aplikacije servisa** , a zatim kliknite naziv mobilnu aplikaciju.
3. Postavke plohu otvara se automatski.  Ako ne, kliknite **Sve postavke**.
4. Kliknite **CORS** na izborniku API-JA.
5. Unesite URL koji želite dodati u okviru naveden i pritisnite Enter.
6. Ako je potrebno, unesite dodatne URL.
7. Kliknite **Spremi** da biste spremili postavke.

Potrebno više od 10 do 15 sekundi da se nove postavke stupile na snagu.

##<a name="register-for-push"></a>Kako: registracija za slanje obavijesti

Instalirajte [phonegap, dodatak i automatske] učiniti automatske obavijesti.  Ovaj dodatak mogu se jednostavno dodati pomoću na `cordova plugin add` naredbu u naredbenom retku ili putem instalacijskog programa brojka dodatak programa Visual Studio.  Sljedeći kod u svojoj aplikaciji Apache Cordova registrira uređaj za slanje obavijesti:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Putem koncentratora SDK obavijesti o automatske obavijesti s poslužitelja.  Nikad ne šalji automatske obavijesti izravno s klijentima. Time se mogu se upotrijebiti za pokretanje uskraćivanje servisa napada protiv koncentratora obavijesti ili na PNS.  U PNS nije ban prometa uslijed takve napadi.

<!-- URLs. -->
[Portal za Azure]: https://portal.azure.com
[Azure mobilne aplikacije Quick Start]: app-service-mobile-cordova-get-started.md
[Početak rada s provjerom autentičnosti]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Dodatak za Cordova Apache za Azure mobilne aplikacije]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[prvi Apache Cordova aplikacije]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap dodatak pribadače]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova, dodatak i uređaji]: https://www.npmjs.com/package/cordova-plugin-device
[cordova dodatak inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
