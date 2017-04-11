<properties
    pageTitle="Azure AD 2.0 protokoli | Microsoft Azure"
    description="Vodič kroz protokole koje podržava krajnja točka za Azure AD 2.0."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20--openid-connect"></a>Povezivanje protokoli - OAuth 2.0 & OpenID 2.0

Krajnja točka 2.0 možete koristiti Azure AD za identitet-kao-na-servis pomoću standardnih industrijskih protokola, OpenID povezivanje i OAuth 2.0.  Dok je servis usklađene sa standardima, može biti Neupadljiva razlike između bilo koje dvije implementacije te protokola.  Informacije Ovdje će biti korisno ako se odlučite za pisanje koda slanjem obrade zahtjeva za HTTP ili proizvođača za korištenje s 3 otvorite zamjena umjesto pomoću jednog od naše biblioteke Otvori izvor.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Osnove
U gotovo sve OAuth i povezivanje OpenID tokova, postoje četiri strane uvrštene u sustavu exchange:

![Uloge OAuth 2.0](../media/active-directory-v2-flows/protocols_roles.png)

- **Poslužitelj za autorizaciju** je krajnju točku 2.0.  Nije odgovoran za osiguravanje identitet korisnika, dodjelu i Opoziv pristupa resursima i izdavanja tokena.  Se nazivaju i davatelja identiteta – sigurno obrađuje veze s korisničke podatke, njihov pristup i pouzdanost odnose između stranama u u tijeku.
- **Vlasnik resursa** obično je krajnji korisnik.  To je zabava vlasništvu podaci, a sadrži power dopustili trećih strana za pristup te podatke ili resurs.
- **Klijent OAuth** je aplikacije, otkrije njegov aplikacija ID-a.  To je obično strana koju krajnji korisnik stupi u interakciju s i zahtjeve tokeni provjeru autentičnosti poslužitelja.  Klijent mora biti odobren dozvolu za pristup resursu vlasnik resursa.
- **Resursa poslužitelja** je gdje se nalazi resursa ili podatke.  Smatra pouzdanima provjeru autentičnosti poslužitelja sigurno provjeru autentičnosti i OAuth klijenta i koristi nošenja access_tokens da biste bili sigurni da se mogu dodijeliti pristup resursu.


## <a name="app-registration"></a>Registracija aplikacije
Svaku aplikaciju koja koristi krajnju točku 2.0 ćete morati registrirana na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) prije nego što možete raditi koristeći OAuth ili povezivanje OpenID.  Postupak registracije aplikacija će prikupljanje i dodjeljivanje nekoliko vrijednosti u aplikaciju za:

- **Id aplikacije** koje služi kao jedinstvena identifikacija aplikacije
- **Preusmjeravanje URI** ili **Identifikator paket** koji se mogu koristiti za usmjeravanje odgovore ponovno pokrenite aplikaciju
- Nekoliko drugih vrijednosti specifične za scenarij.

Dodatne informacije, Saznajte kako [registrirati aplikaciju](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Krajnje točke
Kada se registrirali, aplikaciju obavještava korisnike s Azure AD slanjem zahtjeva za krajnju točku 2.0:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Gdje se `{tenant}` možete poduzeti jednu od četiri različite vrijednosti:

| Vrijednost | Opis |
| ----------------------- | ------------------------------- |
| `common` | Omogućuje korisnicima s osobni računi za Microsoft i poslovne/školske računi iz Azure Active Directory da biste se prijavili u aplikaciju. |
| `organizations` | Omogućuje samo korisnici s računima za poslovne/školske od Azure Active Directory da biste se prijavili u aplikaciju. |
| `consumers` | Omogućuje samo korisnici s osobni računi za Microsoft (MSA) da biste se prijavili u aplikaciju. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`ili`contoso.onmicrosoft.com` | Omogućuje samo korisnici s računima za poslovne/školske od određenog klijenta Azure Active Directory da biste se prijavili u aplikaciju.  Može se koristiti ili naziv domene neslužbeni Azure AD klijenta ili identifikator guid na klijentu.  |

Dodatne informacije o tome kako raditi s te krajnje točke, odaberite vrstu određenu aplikaciju.

## <a name="tokens"></a>Tokeni
Implementacija 2.0 OAuth 2.0 i povezivanje OpenID provjerite proširenom korištenje nošenja tokena, uključujući nošenja tokeni predstavljen kao JWTs. Nošenja token je laganih sigurnosni token koja omogućuje pristup "nošenja" na zaštićenom resurs. U ovom smislu "nošenja" je bilo koji proizvođača koje je moguće izlagati token. Iako zabavu morate najprije uspješnoj Azure AD prima token nošenja ako potrebne korake ne uzimaju se sigurnost token u prijenosa i prostora za pohranu, možete se intercepted i koristi neželjeni proizvođača. Dok neke sigurnosnih tokena ima ugrađene mehanizam za sprječava neovlaštenog stranama njihova korištenja, nošenja tokeni nemaju ovaj mehanizam i mora se prenijeti u sigurnog kanala kao što je sigurnost sloja prijenosa (HTTPS). Ako nošenja token prenose se u isključite, na Muškarac u srednjem napadi mogu se zlonamjerni strana Nabava token koji ćete koristiti za neovlašten pristup resursu zaštićeni. Na isti način sigurnost primijeniti kada pohranjivanje ili predmemoriranje nošenja tokeni za kasnije korištenje. Uvijek provjerite je li aplikacije prenosi i pohranjuje nošenja tokeni siguran način. Dodatne sigurnosna pitanja vezana uz na nošenja tokena, potražite u članku [RFC 6750 odjeljak 5](http://tools.ietf.org/html/rfc6750).

Dodatne pojedinosti o različite vrste tokeni koje se koriste u krajnju točku 2.0 dostupan je u [tokena reference za krajnju točku 2.0](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protokola

Ako ste spremni za prikaz nekih primjera zahtjeva za početak rada s jednim od u nastavku vodiče.  Svaki od njih odgovara scenarij određeni provjere autentičnosti.  Ako vam je potrebna pomoć pri određivanju koji je desno tijek umjesto vas, pogledajte [vrste aplikacija možete izraditi s na 2.0](active-directory-v2-flows.md).

- [Sastavljanje Mobile i matičnoj aplikaciji s OAuth 2.0](active-directory-v2-protocols-oauth-code.md)
- [Sastavljanje Web aplikacija Otvori ID povezivanje](active-directory-v2-protocols-oidc.md)
- [Stvaranje aplikacije za jednu stranicu tokove implicitno OAuth 2.0](active-directory-v2-protocols-implicit.md)
- [Sastavljanje Daemons ili poslužitelja strani procesa pomoću vjerodajnica za klijent OAuth 2.0 tijeka](active-directory-v2-protocols-oauth-client-creds.md)
- Se tokeni API Web s na OAuth 2.0 uključeno ime slobode tijek (uskoro dostupno)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
