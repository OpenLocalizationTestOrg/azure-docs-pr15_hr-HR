<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Vrste aplikacija možete sastaviti u Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Vrste aplikacija

Azure Active Directory (Azure AD) B2C podržava provjeru autentičnosti raznih arhitekturi modernim aplikacije. Svi oni se temelje na na standardnih industrijskih protokola [OAuth 2.0](active-directory-b2c-reference-protocols.md) ili [OpenID povezati](active-directory-b2c-reference-protocols.md). Ovaj dokument ukratko opisuje vrste aplikacija koje možete izraditi, neovisno o jeziku ili platforma radije. Također olakšava razumijevanje više razine scenariji prije [start izradu aplikacija](active-directory-b2c-overview.md#getting-started).

## <a name="the-basics"></a>Osnove
Svaku aplikaciju koja koristi Azure AD B2C mora biti registriran u [imeniku B2C](active-directory-b2c-get-started.md) putem [Portala za Azure](https://portal.azure.com/). Postupak registracije aplikacije prikuplja i nekoliko vrijednosti dodjeljuje aplikacije:

- **ID aplikacije** koje služi kao jedinstvena identifikacija aplikacije.
- **Preusmjeravanje URI** koje je moguće koristiti za usmjeravanje odgovore ponovno pokrenite aplikaciju.
- Sve ostale vrijednosti specifične za scenarij. Dodatne informacije, Saznajte kako [registrirati aplikaciju](active-directory-b2c-app-registration.md).

Kada je registrirana aplikaciju, komunikaciju s Azure AD slanjem zahtjeva za Azure AD krajnjoj 2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Svaki zahtjev koji se šalju Azure AD B2C određuje **pravila**. Pravilo nadzire ponašanje Azure AD. Da biste stvorili iznimno prilagodljive skup bogatih možete koristiti i te krajnje točke. Uobičajeni pravila obuhvaćaju prijave, prijavu i uređivanje profila pravila. Ako niste upoznati s pravilima, pročitajte o Azure AD B2C [okvir extensible pravila](active-directory-b2c-reference-policies.md) prije nastavka.

Interakcija svaki aplikacije s 2.0 krajnje slijedi uzorak slične više razine:

1. Aplikaciju usmjerava korisnika krajnju točku 2.0 izvršiti [pravila](active-directory-b2c-reference-policies.md).
2. Korisnik dovrši pravila prema definiciju pravila.
4. Aplikaciju prima neke vrste sigurnosnog tokena iz krajnju točku 2.0.
5. Aplikaciju koristi sigurnosni token za pristup zaštićeni informacije ili zaštićeni resursa.
6. Poslužitelj resursa provjerava sigurnosni token da biste potvrdili da se mogu dodijeliti pristup.
7. Aplikaciju povremeno osvježava sigurnosni token.

<!-- TODO: Need a page for libraries to link to -->
Ove korake mogu se razlikovati malo ovise o vrsti aplikacije se stvara. Otvori izvor biblioteke možete pozabaviti detalje za vas.

## <a name="web-apps"></a>Web-aplikacije
Web-aplikacijama (uključujući .NET, PHP, Java, fonetski zapis, Python i Node.js) koji se nalazi na poslužitelju i pristupiti putem preglednika Azure AD B2C podržava [Povezivanje OpenID](active-directory-b2c-reference-protocols.md) za sve korisničkog sučelja. To obuhvaća prijave, prijave, i upravljanje profila. Web-aplikaciju programa u Azure AD B2C implementaciji OpenID povezivanje, pokreće te bogatih slanjem zahtjeva za provjeru autentičnosti za Azure AD. Rezultat zahtjev je za `id_token`. U ovom sigurnosni token predstavlja identitet korisnika. Sadrži i informacije o korisniku u obliku zahtjevima:

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

Dodatne informacije o vrstama tokeni i zahtjevima dostupne u aplikaciji [B2C token referencu](active-directory-b2c-reference-tokens.md).

U web-aplikacijama, svaki izvođenja [pravila](active-directory-b2c-reference-policies.md) uzima korake više razine:

![Web-aplikacije Funkcijske trake slike](./media/active-directory-b2c-apps/webapp.png)

Provjera valjanosti na `id_token` putem je javni ključ potpisa koji je poslao Azure AD potrebne da biste provjerili identitet korisnika. Time se postavlja i sesiju kolačića koje je moguće koristiti za identifikaciju korisnika na zahtjeve za sljedeće stranice.

Da biste vidjeli scenarij u praksi, učinite nešto od primjera kod za prijavu u aplikaciju web u našem [Prvi koraci u sekciji](active-directory-b2c-overview.md#getting-started).

Osim olakšavanja jednostavne prijavu, web-aplikacijama poslužitelju možda ćete morati pristup web-pozadinskoj servisa. U ovom slučaju web-aplikaciji možete izvršiti malo drugačije [OpenID povezivanje tijek](active-directory-b2c-reference-oidc.md) i Nabava tokeni pomoću kodova autorizacije i osvježite tokena. Ovaj scenarij je prikazan u sljedećoj [sekciji API -ji za Web](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>API-ji za web
Azure AD B2C možete koristiti radi zaštite web-servisi kao što su pokrenite aplikaciju RESTful API na webu. Web API-ji pomoću OAuth 2.0 provjere autentičnosti HTTP zahtjevi za korištenje tokena sigurne svoje podatke. Pozivatelja na API na webu dodaje token u zaglavlju autorizacije HTTP zahtjev:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

API na webu možete koristiti token da biste provjerili identitet pozivatelja API-JA i pronaći informacije o pozivatelju zahtjevima koji su kodirana token. Dodatne informacije o vrstama tokeni i zahtjevima dostupne u aplikaciji [Azure AD B2C token referencu](active-directory-b2c-reference-tokens.md).

> [AZURE.NOTE]
    Azure AD B2C trenutno podržava samo web API-ji koji je moguće pristupiti putem vlastitog poznati klijente. Ako, primjerice, dovršavanje aplikacije može obuhvaćati aplikaciju iOS, aplikacija za Android i pozadinske web API-JA. U ovom arhitektura potpuno podržane. Dopuštanje klijent partnera, kao što su nekoj drugoj aplikaciji iOS da biste pristupili isti web API trenutno nisu podržani. Svih komponenti aplikacije dovršeno morate zajedničko korištenje u jednoj aplikaciji ID-a.

Web-mjesto API-JA možete primati tokena iz razne vrste klijenata, uključujući web-aplikacije, radne površine i mobilne aplikacije, aplikacije za jednu stranicu, poslužiteljsko daemons i drugim web-API-ji. Evo primjera dovršavanje tijeka za web-aplikacije koje se poziva na API na webu:

![Web-aplikacija Web API Funkcijske trake slike](./media/active-directory-b2c-apps/webapi.png)

Da biste saznali više o autorizacije šifre, osvježavanje tokeni i koraci za pribavljanje tokena, Saznajte više o [OAuth 2.0 protokol](active-directory-b2c-reference-oauth-code.md).

Da biste saznali kako sigurne API web-mjesto pomoću Azure AD B2C, pogledajte webu API vodiče u našem [Prvi koraci u sekciji](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Mobile i izvorni aplikacije
Aplikacijama koje su instalirani na uređajima, kao što su aplikacije za mobilne uređaje i stolna računala, često moraju imati pristup pozadinske services ili web-API-ji ime korisnika. Prilagođene identiteta upravljanje sučelja možete dodati pomoću nativnog aplikacija i sigurno poziv pozadinske servisa pomoću Azure AD B2C i [OAuth 2.0 autorizacije kod tijek](active-directory-b2c-reference-oauth-code.md).  

U ovom tijek aplikaciju izvršava [pravila](active-directory-b2c-reference-policies.md) i prima programa `authorization_code` iz Azure AD kada korisnik dovrši pravila. Na `authorization_code` predstavlja aplikacije dozvolu za usluge pozadinske ime korisnika koji je trenutno prijavljeni. Aplikacije pa možete razmjenjivati na `authorization_code` u pozadini za programa `id_token` i `refresh_token`.  Možete koristiti aplikaciju na `id_token` za provjeru autentičnosti web-pozadinskoj API-JA u zahtjevima za HTTP. Također možete koristiti u `refresh_token` da biste dobili novi `id_token` kada istekne starije jedan.

> [AZURE.NOTE]
    Azure AD B2C trenutno podržava samo tokena koje se koriste za pristup na servis aplikacije vlastitu pozadinsku web. Ako, primjerice, dovršavanje aplikacije može obuhvaćati aplikaciju iOS, aplikacija za Android i pozadinske web API-JA. U ovom arhitektura potpuno podržane. Dopuštanje iOS aplikacije da biste pristupili pomoću tokeni OAuth 2.0 pristup partnera web API trenutno nisu podržani. Svih komponenti aplikacije dovršeno morate zajedničko korištenje u jednoj aplikaciji ID-a.

![Izvorni aplikacije Funkcijske trake slika](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Trenutni ograničenja
Azure AD B2C trenutno ne podržava sljedeće vrste aplikacije, ali su na na roadmapy. Dodatna ograničenja i ograničenja vezane uz Azure AD B2C opisani su [ograničenja i ograničenja](active-directory-b2c-limitations.md).

### <a name="single-page-apps-javascript"></a>Jedna stranica aplikacije (JavaScript)
Mnoge Moderna aplikacije imati jednu stranicu aplikacije sučelje napisan prvenstveno JavaScript. Često koriste framework kao što su AngularJS, Ember.js ili Durandal. Općenito dostupan servisa Azure AD podržava te aplikacije pomoću tijek implicitnog OAuth 2.0. Međutim, ovaj tijek još nije dostupna u Azure AD B2C.

### <a name="daemonsserver-side-apps"></a>Daemons/poslužiteljsko aplikacije
Aplikacije koje sadrže dugoročnih procesa ili koji rade bez prisutnosti korisnika treba i način pristupa zaštićenim resurse kao što je web API-ji. Ove aplikacije možete provjeru autentičnosti i dobiti tokeni pomoću aplikacije identiteta (umjesto ovlaštenog identitet korisnika) i tako da klijenti za OAuth 2.0 tijek vjerodajnice.

Ovaj tijek ne trenutno podržava Azure AD B2C. Aplikacije možete dobiti tokeni tek kada je došlo je do tijek interaktivnog korisnika.

### <a name="web-api-chains-on-behalf-of-flow"></a>Nabavom API na webu (vode na-ime-od)
Mnoge arhitekturi obuhvaćaju API koju je potrebno da biste pozvali druge do API na webu, gdje su oba zaštićene po Azure AD B2C web-mjesto. Taj se scenarij uobičajenih u izvorni klijentskim programima koji imaju na webu API pozadinske. To se zatim poziva Microsoftov mrežni servis kao što su API Azure AD grafikonu.

Scenarij ulančana API na webu mogu podržavati pomoću na OAuth 2.0 JWT nošenja vjerodajnica grant, poznata i kao na-ime-od tijeka.  Međutim, tijek na-ime-od nije trenutno implementirana u Azure AD B2C.
