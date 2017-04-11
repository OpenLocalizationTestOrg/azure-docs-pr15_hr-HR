<properties
    pageTitle="Vrste krajnje točke 2.0 | Microsoft Azure"
    description="Vrste aplikacija i scenariji podržava krajnja točka za Azure AD 2.0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>Vrste aplikacija za krajnju točku 2.0
Krajnja točka 2.0 podržava provjeru autentičnosti raznih arhitekturi Moderna aplikacije koje se temelje na na standardnih industrijskih protokola [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) i/ili [Povezivanje OpenID](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  U ovom dokumentu ukratko opisuje vrste aplikacija možete izraditi, neovisno o jeziku ili platforma radije.  To će vam pomoći razumjeti više razine scenariji prije [odmah u kod](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Osnove
Svaku aplikaciju koja koristi krajnju točku 2.0 ćete morati registrirati pri [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Postupak registracije aplikacija će prikupljanje i dodjeljivanje nekoliko vrijednosti u aplikaciju za:

- **Id aplikacije** koje služi kao jedinstvena identifikacija aplikacije
- **Preusmjeravanje URI** koje je moguće koristiti za usmjeravanje odgovore ponovno pokrenite aplikaciju
- Nekoliko drugih vrijednosti specifične za scenarij.  Više detalja, Saznajte kako [registrirati aplikaciju](active-directory-v2-app-registration.md).

Kada se registrirali, aplikaciju se komunicira s Azure AD slanjem zahtjeva za krajnje 2.0 Azure Active Directory.  Nudimo Otvori izvor okviri i biblioteke koje voditi brigu o pojedinosti te zahtjeve ili možete implementirati logiku provjere autentičnosti sami obratite zahtjevi za te krajnje točke:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web-aplikacije
Web-aplikacijama (.NET, PHP, Java, Ruby, Python, čvor itd) koje je moguće pristupiti putem preglednika možete izvršiti korisnika prijavu pomoću [OpenID povezivanja](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  U OpenID povezivanje prima web-aplikaciji programa `id_token`, sigurnosni token koji provjerava identitet korisnika i daje informacije o korisniku u obliku zahtjevima:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Koje dodatne informacije o svim vrstama tokeni i zahtjevima dostupne za aplikacije u [2.0 tokena referencu](active-directory-v2-tokens.md).

U web-aplikacijama na poslužitelju, prijavu provjere autentičnosti tijek uzima korake više razine:

![Web-aplikacije Funkcijske trake slike](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Provjeru id_token pomoću javni ključ potpisivanja poslao krajnju točku 2.0 je dovoljno provjerite je li identitet korisnika i postavljanje sesiju kolačića koje je moguće koristiti za identifikaciju korisnika na zahtjeve za sljedeće stranice.

Da biste vidjeli scenarij u praksi, isprobajte nešto od primjera kod za prijavu u aplikaciju web u našem odjeljku [Početak rada](active-directory-appmodel-v2-overview.md#getting-started) .

Osim jednostavne prijavu, web-aplikacijama poslužitelju možda ćete morati pristup nekim drugim web-servisa kao što su REST API-JA.  Poslužitelj web-aplikacije u tom slučaju možete sudjelovati u kombinirani OpenID povezivanje i OAuth 2.0 protok pomoću [koda za autorizaciju 2.0 OAuth tijek](active-directory-v2-protocols.md#oauth2-authorization-code-flow). Ovaj scenarij opisana su ispod u našem [WebApp WebAPI Uvod temu](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>API-ji za web
Krajnju točku 2.0 možete koristiti radi zaštite web-servisi kao i, kao što su aplikacije programa RESTful API na webu.  Umjesto id_tokens i sesiju kolačići API-ji Web pomoću OAuth 2.0 access_tokens zaštitu svoje podatke i zahtjevi za provjeru autentičnosti.  Pozivatelja Web API dodaje se access_token u zaglavlju autorizacije HTTP zahtjev:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API možete koristiti u access_token da biste provjerili identitet pozivatelja API-JA i pronaći informacije o pozivatelju zahtjevima koji su kodirana u access_token.  Koje dodatne informacije o svim vrstama tokeni i zahtjevima dostupne za aplikacije u [2.0 tokena referencu](active-directory-v2-tokens.md).

Web API ponudit će korisnici power za uključivanje-u i onemogućivanju određene funkcije ili podataka tako da će se dozvole, u suprotnom naziva [opsega](active-directory-v2-scopes.md).  Za pokrenuti aplikaciju potrebne dozvole za opseg, korisnik mora pristanak doseg tijekom u tijeku.  Krajnja točka 2.0 će voditi brigu o zahtjevom korisnika i snimanje te dozvole u svim access_tokens koja prima Web API.  Sve Web API mora se brinuti o tome je provjera valjanosti access_tokens primi na svaki poziv i izvođenje proper autorizacije provjere.

Web API možete primati access_tokens iz svih vrsta aplikacija, uključujući web-poslužitelj aplikacije, radne površine i mobilne aplikacije, aplikacije za jednu stranicu, daemons strani poslužitelja i i druge API-ji Web.  Više razine tijek za provjeru autentičnosti api web je na sljedeći način:

![Slika Funkcijske trake API na webu](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Da biste saznali više o authorization_codes, refresh_tokens i detaljne korake access_tokens, Saznajte više o [OAuth 2.0 protokol](active-directory-v2-protocols-oauth-code.md).

Da biste saznali kako web-mjesto za sigurnu API-ja s OAuth2 access_tokens pogledajte primjere koda api web u našem [Prvi koraci u sekciji](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile i izvorni aplikacije
Aplikacijama koje su instalirani na uređaju, kao što su aplikacije za mobilne uređaje i stolna računala, često moraju imati pristup pozadinskog services ili Web API-ji koje pohranjujete podatke i izvršavanje raznih funkcija ime korisnika.  Aplikacije možete dodati prijave i autorizacije pozadinskog servise pomoću [koda za autorizaciju 2.0 OAuth tijek](active-directory-v2-protocols-oauth-code.md).  

U ovom tijeku na aplikaciju prima programa authorization_code iz krajnju točku 2.0 nakon korisničku prijavu, koji predstavlja aplikacije dozvole da biste nazvali pozadinski usluge za trenutno prijavljeni korisnik.  Aplikacije pa možete razmjenjivati authoriztion_code u pozadini programa access_token OAuth 2.0 i na refresh_token.  Aplikaciju možete koristiti u access_token za provjeru autentičnosti Web API-ji u zahtjevima za HTTP, a možete koristiti u refresh_token da biste dobili novi access_tokens prilikom starije isteka.

![Izvorni aplikacije Funkcijske trake slika](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Jedna stranica aplikacije (javascript)
Mnoge Moderna aplikacije ste na jednu stranicu aplikacija (SPA) sučelja prvenstveno pisane u javascript i često koristite okviri kao što su AngularJS, Ember.js, Durandal itd.  Krajnja točka za Azure AD 2.0 podržava te aplikacije pomoću [OAuth 2.0 implicitno tijeka](active-directory-v2-protocols-implicit.md).

U ovom tijek aplikaciju prima tokena iz na 2.0 autorizirali krajnjoj točki izravno bez izvođenje bilo kojeg razmjena pozadinskog poslužitelja s poslužiteljem.  Time se omogućuje sve logike za provjeru autentičnosti i rukovanja sesiju da bi smještanje u potpunosti klijent javascript bez izvođenje preusmjeravanja dodatne stranice.

![Implicitno tijek Funkcijske trake slika](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Da biste vidjeli scenarij u praksi, isprobajte nešto od aplikacija primjere koda za jednu stranicu u sekciji naš [Početak rada](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Daemons/server strani aplikacije
Aplikacije koje sadrže dugo izvodi postupke ili koji rade bez prisutnosti korisnika treba i način pristupa zaštićenim resurse, kao što su API-ji za Web.  Aplikacije možete provjeru autentičnosti i se tokeni pomoću aplikacije identiteta (umjesto ovlaštenog identitet korisnika) pomoću klijentskog programa OAuth 2.0 tijek vjerodajnice.

U ovom tijek aplikaciju dohvaća tokeni po izravna interakcija sa na `/token` krajnja točka:

![Daemon aplikacija Funkcijske trake slika](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Da biste sastavili daemon aplikacije, potražite u članku documeenation vjerodajnice za klijenta u našem odjeljku [Početak rada](active-directory-appmodel-v2-overview.md#getting-started) ili se odnositi na [ovu aplikaciju .NET uzorka](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

## <a name="current-limitations"></a>Trenutni ograničenja
Ove vrste aplikacija ne trenutno podržava krajnju točku 2.0, ali uključeni u vodič.  Dodatna ograničenja i ograničenja za krajnju točku 2.0 opisana su u [članku ograničenja 2.0](active-directory-v2-limitations.md).

### <a name="chained-web-apis-on-behalf-of"></a>Ulančana web API-ji (na-ime-od)
Mnoge arhitekturi obuhvaćaju API Web koji je potrebno da biste pozvali druge do API Web, oba osigurava krajnju točku 2.0.  Taj se scenarij uobičajenih u izvorni klijentskim programima koji imaju Web API pozadinskog koja naizmjence poziva servisa Microsoft Online kao što je Office 365 ili API grafikonu.

Scenarij ulančana Web API biti podržane pomoću dodijelite OAuth 2.0 Jwt nošenja vjerodajnice, u suprotnom naziva [On-Behalf-Of tijeka](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow).  Međutim, tijek uključeno-ime-od nije trenutno implementirana u krajnju točku 2.0.  Da biste vidjeli kako će se ovaj tijek funkcionira u načelu dostupan Azure AD servisa, pogledajte [uzorak koda za na-ime-od na GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
