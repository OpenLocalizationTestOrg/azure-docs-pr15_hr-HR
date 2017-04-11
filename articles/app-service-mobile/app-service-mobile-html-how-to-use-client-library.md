<properties
    pageTitle="Kako koristiti JavaScript SDK za Azure mobilne aplikacije"
    description="Upute za korištenje v za Azure mobilne aplikacije"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Kako koristiti biblioteku JavaScript klijenta za Azure mobilne aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ovaj vodič ručica za izvođenje uobičajeni scenariji pomoću najnovije [JavaScript SDK za Azure mobilne aplikacije]. Ako ste novi korisnik aplikacije Mobile Azure, prvo morate dovršiti [Azure mobilne aplikacije brzi početak rada] da biste stvorili s pozadinskom, a zatim stvorite tablicu. U ovom vodiču smo fokus na korištenje mobilne pozadine u HTML-JavaScript web-aplikacijama.

## <a name="supported-platforms"></a>Podržane platforme

Ne možemo ograničiti podrška preglednika na trenutni i zadnje verzije važnije preglednike: Google Chrome, Microsoft Edge, Microsoft Internet Explorer i Mozilla Firefox.  Očekivanog SDK funkciji s bilo kojeg relativno Moderna preglednika.

Paket distribuira kao JavaScript modula univerzalni tako da podržava globals, AMD, i CommonJS oblicima.

##<a name="Setup"></a>Postavljanje i preduvjeti

Ovaj vodič pretpostavlja da ste stvorili s pozadinskom s tablicom. Ovaj vodič pretpostavlja da tablica ima istu shemu kao tablice u tim vodičima.

Instaliranje Azure SDK za mobilne aplikacije JavaScript možete napraviti putem na `npm` naredba:

```
npm install azure-mobile-apps-client --save
```

Kad instalirate, u biblioteku nalazi se u `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Kopirajte ovu datoteku na područje web.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Biblioteku može se koristiti kao ES2015 modula, unutar CommonJS okruženja kao što su Browserify i Webpack i kao biblioteku AMD.  Ako, na primjer:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Kako: provjeru autentičnosti korisnika

Aplikacije servisa za Azure podržava provjere autentičnosti i dopuštanja korisnici aplikacije pomoću davatelja usluge za različite vanjskih identiteta: Facebook, Google, Microsoftov Account i Twitter. Postavljanje dozvola za tablice ograničavanja pristupa za određene operacije samo korisnicima čija je autentičnost provjerena. Identitet korisnika čija je autentičnost provjerena možete koristiti i za primjenu pravila za provjeru autentičnosti u skriptama poslužitelja. Dodatne informacije potražite u članku vodič za [Početak rada s provjerom autentičnosti] .

Podržane su dvije tokova provjere autentičnosti: na poslužitelju i klijent tijek.  Tijek server nudi najjednostavniji sučelje za provjeru autentičnosti kao ovisi vašeg davatelja usluge web-sučelja za provjeru autentičnosti. Tijek klijent omogućuje dublju Integracija s mogućnosti specifične za uređaj kao što su-jedinstvene prijave kao ovisi SDK-ovi karakteristične za davatelja.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Kako: Konfiguriranje mobilne aplikacije servisa za vanjske preusmjeravanja URL-ove.

Nekoliko vrsta JavaScript aplikacije pomoću povratna mogućnost za rukovanje tokova OAuth korisničkog Sučelja.  Ove mogućnosti obuhvaćaju sljedeće:

* Na servisu izvodi lokalno
* Pomoću uživo ponovno Učitaj Ionic Framework
* Preusmjeravanje aplikacije servisa za provjeru autentičnosti. 

Izvodi lokalno može uzrokovati probleme jer je prema zadanim postavkama aplikacije servisa za provjeru autentičnosti samo konfigurira tako da pristupa iz svoje pozadine za mobilnu aplikaciju. Da biste promijenili postavke aplikacije servisa za omogućivanje provjere autentičnosti dok je pokrenut server lokalno, poduzmite sljedeće korake:

1. Prijavite se na [portal za Azure]
2. Dođite do svoje pozadinskog mobilnu aplikaciju.
3. Odaberite **resurs explorer** na izborniku **Alati za RAZVOJ** .
4. Kliknite **Idi** da biste otvorili explorer resursa za vaše pozadinskog mobilnu aplikaciju na novoj kartici ili u prozoru.
5. Proširite **config** > **authsettings** čvor aplikacije.
6. Kliknite gumb **Uređivanje** da biste omogućili uređivanje resursa.
7. Traženje element **allowedExternalRedirectUrls** što bi trebalo biti null. Dodajte svoje URL-ove u polju:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    URL-ovi u polju zamijenite URL-ove usluge, koji se u ovom primjeru je `http://localhost:3000` za lokalni servis Node.js uzorka. Možete koristiti i `http://localhost:4400` za servis Val ili neke druge URL-a, ovisno o tome kako je konfigurirano vaše aplikacije.

8. Pri vrhu stranice kliknite **Čitanje/pisanje**, a zatim kliknite **Postavljanje** da biste spremili ažuriranja.

Morate dodati iste povratna URL-ove postavke whitelist CORS:

1. Otvorite [portal za Azure].
2. Dođite do svoje pozadinskog mobilnu aplikaciju.
3. Kliknite **CORS** na izborniku **API-JA** .
4. Unesite sve URL-ove u tekstni okvir prazna **Dopušteni drugačijeg izvora** .  Stvara se novi tekstni okvir.
5. Kliknite **SPREMI**
    
Kada pozadinski ažurira, moći koristiti nove povratne URL-ove u svojoj aplikaciji.

<!-- URLs. -->
[Azure mobilne aplikacije Quick Start]: app-service-mobile-cordova-get-started.md
[Početak rada s provjerom autentičnosti]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Portal za Azure]: https://portal.azure.com/
[JavaScript SDK za Azure mobilne aplikacije]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

