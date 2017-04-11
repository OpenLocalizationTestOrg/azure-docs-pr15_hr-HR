<properties
    pageTitle="Krajnja točka za Azure AD 2.0 | Microsoft Azure"
    description="Za usporedbu između izvorne Azure AD i krajnje točke 2.0."
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

# <a name="whats-different-about-the-v20-endpoint"></a>Što je različito o krajnjoj točki 2.0?

Ako poznajete Azure Active Directory ili ste integrirane aplikacije s Azure AD u prošlosti, možda postoje neke razlike u krajnju 2.0 koju ste očekivali.  Ovaj dokument pozive izvan tih razlike za podataka.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft računi i računi servisa Azure AD
Krajnja točka 2.0 razvojnim inženjerima pisati aplikacije koje prihvatiti prijava s Microsoft Accounts i Azure AD računa pomoću krajnjoj točki jedan provjere autentičnosti.  To vam omogućuje da biste napisali aplikacije potpuno računa – agnostic; Možda ćete ignorant vrste računa koji korisnikove prijave u.  Naravno, *mogu* obavijestite aplikacije vrstu računa koji se koristi u određenom sesiji, ali ne morate.

Ako, primjerice, ako aplikacije poziva [Programa Microsoft Graph](https://graph.microsoft.io), neke dodatne funkcije i podataka bit će dostupne korisnicima enterprise, kao što su svoje web-mjesta sustava SharePoint ili imenika podataka.  No za mnoge akcije, kao što su [čitanje pošte korisnika](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), kod možete staviti točno onako za Microsoft Accounts i Azure AD račune.  

Integraciji aplikacije sustava Microsoft Accounts i računi servisa Azure AD sada je jedan jednostavan postupak.  Možete koristiti jedan skup krajnje točke, jednoj biblioteci i jednu aplikaciju Registracija da biste pristupili potrošača i enterprise svijetu.  Da biste saznali više o krajnjoj točki 2.0, pogledajte [odjeljak Pregled](active-directory-appmodel-v2-overview.md).


## <a name="new-app-registration-portal"></a>Novi portal Registracija aplikacije
Krajnja točka 2.0 biti registriran samo na novom mjestu: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Ovo je portal gdje možete dobiti Id aplikacije, prilagodite izgled stranica za prijavu u aplikaciju programa i drugo.  Sve što trebate pristup portalu je račun za Microsoft pokreće - osobne i poslovne/školske računa.  

Nastavit ćemo dodavati više funkcije ovaj Portal Registracija aplikacije tijekom vremena.  Namjera je ovaj portal će se na novo mjesto gdje možete potražiti da biste upravljali ništa i sve što morate učiniti s aplikacija sustava Microsoft.


## <a name="one-app-id-for-all-platforms"></a>Jednu aplikaciju ID-a za sve platforme
U izvornom servisa Azure Active Directory, možda registrirana nekoliko različitih aplikacija za pojedinačni projekt.  Obavezno su korištenje zasebnu aplikaciju registracije za izvorni klijenata i web-aplikacije:

![Registracija staru aplikaciju korisničkog Sučelja](../media/active-directory-v2-flows/old_app_registration.PNG)

Ako, na primjer, ako ste ugrađena web-mjesto i aplikaciju iOS, morali ih registrirati zasebno, pomoću dvije različite ID-ove.  Ako imate web-mjesto i pozadinskog web-API-ja možda registriranih svaki kao zasebnu aplikaciju u Azure AD.  Ako imate aplikaciju za iOS i Android aplikacije i možda registriranih dvije različite aplikacije.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Sve što trebate je sada jedan aplikacija Registracija i jedan Id aplikacije za svaku projekte.  Dodavanje nekoliko "platforme" za svaki projekt i navedite odgovarajuće podatke za svaku platformu dodate.  Naravno, možete stvoriti proizvoljan broj aplikacije koji vam se sviđa po vlastitom nahođenju, no Većina slučajeva samo jedan Id aplikacije mora biti potrebno.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Naš ciljem je koji to će dovesti do više pojednostavljeni upravljanje aplikacije i sučelje za razvoj i stvorite više konsolidirani prikaz jedan projekt koje možete raditi na.


## <a name="scopes-not-resources"></a>Opsezi, ne resursi
Izvorni servisu Azure AD aplikaciju možete ponašaju kao **resursa**ili primatelj tokena.  Resurs možete definirati broj **opsega** ili **oAuth2Permissions** razumije, što klijent aplikacije da biste zatražili tokeni u tom resursu za određeni skup opsega.  Razmislite o API Azure AD grafikon kao primjer resursa:

- Identifikator resursa ili `AppID URI`:`https://graph.windows.net/`
- Opsezi, ili `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, itd.  

Sve to nalaze odnosi na krajnjoj točki 2.0.  Aplikaciju možete i dalje se ponašaju kao resursa, definiranje opsega i prepoznaje URI.  Aplikacije klijenta i dalje možete zatražiti pristup tim opsega.  Međutim, u kojem klijent zahtjeve za razgovore te dozvole promijenjen je način.  U prošlosti, autorizirali programa 2.0 OAuth zahtjev za Azure AD možda ste pokušavali kao što su:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

gdje parametar **resursa** naznačen koji aplikaciju klijent zahtijeva provjeru autentičnosti za.  Azure AD izračunati dozvole potrebnih aplikaciju ovisno o konfiguraciji statične na portalu za Azure i izdavanja tokeni sukladno tome.  Sada isti 2.0 OAuth autorizirali zahtjev čini se da:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

gdje parametar **opseg** označava koje resursa i dozvole za aplikaciju zahtijeva provjeru autentičnosti za. Željeni resurs je i dalje vrlo su prisutne u zahtjevu za – jednostavno encompassed u svakoj vrijednosti parametra opseg.  Parametar opseg na taj način omogućuje krajnju točku 2.0 bude više sukladan s specifikacija OAuth 2.0 i poravnava pobliže s uobičajenih industrijskih prakse.  Omogućuje i aplikacije da biste izvršili [rastuće pristanak](#incremental-and-dynamic-consent), koji je opisan u sljedećem odjeljku.

## <a name="incremental-and-dynamic-consent"></a>Rastuća i dinamički pristanak
Aplikacija registrira u načelu dostupan Azure AD servisa potrebno da biste odredili potrebne dozvole OAuth 2.0 na portalu za Azure vrijeme stvaranja aplikacija:

![Dozvole za registraciju korisničkog Sučelja](../media/active-directory-v2-flows/app_reg_permissions.PNG)

**Statički**konfigurirati aplikaciju potrebne su dozvole.  Dok je dopušteno konfiguracije aplikacije postoje na portalu za Azure i zadržati kod bolje i jednostavno, predstavlja nekoliko problema za razvojne inženjere:

- Aplikaciju morali znate sve dozvole ikada potrebno ga na vrijeme stvaranja aplikacija.  Dodavanjem dozvola s vremenom je teško procesa.
- Aplikaciju morali znati svi resursi koji bi ikad pristupiti na vrijeme.  Ovo je teško stvorite aplikacije koje bi mogle pristupiti proizvoljne broj resursa.
- Aplikaciju morali zahtjev za sve dozvole ikada potrebno ga na korisnikovu prve prijave.  U nekim slučajevima to su doveli prikaže dugačak popis dozvole koje discouraged krajnjim korisnicima iz odobravanje aplikacije programa access na početni prijava.

Pomoću krajnju točku 2.0 možete odrediti dozvole aplikacije **dinamički**, prilikom izvođenja, mora se tijekom običnog korištenje aplikacije.  Da biste to učinili, možete odrediti opsega aplikacije mora bilo kojem trenutku navedene u vremenu tako da ih uvrstite na `scope` parametar zahtjeva za autorizaciju:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Iznad zahtjeva za dozvole za aplikaciju za čitanje podataka direktorija za Azure AD korisnika, kao i zapisivanje podataka njihove direktorija.  Ako korisnik ima pristanak tih dozvola u prošlosti za određenu aplikaciju, jednostavno unositi vjerodajnice i biti prijavljeni u aplikaciju.  Ako korisnik ima ne pristanak na bilo koju od tih dozvola, krajnju točku 2.0 će od korisnika traži pristanak za te dozvole.  Da biste saznali više, može se čitati na [dozvola, dozvole, i opsega](active-directory-v2-scopes.md).

Dopuštanje aplikacije dozvole dinamički putem na `scope` parametar omogućuje potpunu kontrolu nad vaš korisnički doživljaj.  Ako želite, možete odabrati da biste frontload vaš pristanak iskustva i Traži sve dozvole u jedan zahtjev za početne autorizacije.  Ili ako aplikacije potreban je velik broj dozvole, možete odabrati njihovo prikupljanje te dozvole korisnika postupno, kao što je pokušaju koristiti određene značajke aplikacije tijekom vremena.

## <a name="well-known-scopes"></a>Poznati dosega

#### <a name="offline-access"></a>Izvanmrežni pristup
Krajnja točka 2.0 možda potrebno koristiti novu poznati dozvolu za aplikacije - u `offline_access` opseg.  Sve aplikacije ćete morati zatražiti tu dozvolu ako koje su im potrebne za pristup resursa u ime korisnika za razdoblje tijekom duljeg vremena, čak i kada korisnik možda nije aktivno koristite aplikaciju.  Na `offline_access` opseg pojavit će se korisniku u dijaloškim okvirima pristanak kao "Pristup vašim podacima izvanmrežno", koji je korisnik mora pristajete da.  Traži u `offline_access` dozvola omogućit će web aplikacije OAuth 2.0 refresh_tokens primanje krajnju točku 2.0.  Refresh_tokens su long-lived, a mogu razmjenjivati za nove access_tokens OAuth 2.0 za prošireni razdoblja programa access.  

Ako aplikaciju zahtjev za `offline_access` opseg, će primiti refresh_tokens.  To znači da kada iskoristili programa authorization_code u [OAuth 2.0 autorizacije kod tijek](active-directory-v2-protocols.md#oauth2-authorization-code-flow), samo dobit ćete ponovno programa access_token iz na `/token` krajnjoj točki.  Tu access_token će ostati valjani kratko vrijeme (obično jedan sat), ali ti će isteći.  Natrag na koje točke u vremenu, pokrenite aplikaciju morate preusmjeravanje korisnika u na `/authorize` krajnja točka za dohvaćanje novi authorization_code.  Tijekom ovog preusmjeravanje korisnik može ili možda ne morate ponovno unositi vjerodajnice ili ponovno pristanak dozvola, ovisno o na vrstu aplikacije.

Da biste saznali više o OAuth 2.0, refresh_tokens i access_tokens, pogledajte [2.0 protokol referencu](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, profila i e-pošte

Izvorni servisu Azure Active Directory osnovni OpenID povezivanje prijavu tijeka bi i mnoštvo informacije o korisniku u dobivene id_token.  Sporove u programa id_token možete uključiti korisničko ime, preferirani korisničko ime, adresu e-pošte, ID objekta i više.

Mi se sada ograničavanje podatke koji na `openid` opseg affords aplikacije pristup.  Opseg 'openid' samo će Omogućivanje aplikacije za prijavu korisnika u te primati identifikatoru aplikacije specifične za korisnika.  Ako želite da biste dobili osobnu identifikaciju osobe (PII) o korisniku u aplikaciji, pokrenite aplikaciju morat ćete od korisnika tražiti dodatne dozvole.  Ne možemo su Uvod u dva nova opsega – u `email` i `profile` opsege – koje omogućuju vam da biste to učinili.

Na `email` opseg je vrlo jednostavne – omogućuje pristup korisničke adrese e-pošte primarnog putem aplikacije na `email` zahtjeva u na id_token.  Na `profile` opseg affords pristup aplikacije za sve druge osnovne informacije o korisnika – svoje ime, preferirani korisničko ime, ID objekta i tako dalje.

To vam omogućuje da kod aplikacije na način minimalnog otkrivanja – samo od korisnika traži skup informacija da aplikacije potreban je učiniti njegov posao.  Dodatne informacije o ti dosezi potražite [referencu 2.0 opseg](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Tokena zahtjevima

Sporove u tokena koje izdaje krajnju točku 2.0 neće biti jednake tokena koje izdaje načelu dostupan krajnje točke Azure AD - aplikacije Migracija na novom servisu treba pretpostavlja određeni zahtjeva će se prikazivati u id_tokens ili access_tokens.   Tokeni izdala krajnju točku 2.0 su sukladan s specifikacije OAuth 2.0 i povezivanje OpenID, no možda slijede različite semantiku od načelu dostupan servisa Azure AD.

Da biste saznali više o određenim zahtjevima čuje u 2.0 tokena, potražite u članku [Referenca za tokena 2.0](active-directory-v2-tokens.md).

## <a name="limitations"></a>Ograničenja
Postoji nekoliko ograničenja morate imati na umu prilikom korištenja točke 2.0.  Pogledajte [2.0 ograničenja dokument](active-directory-v2-limitations.md) da biste vidjeli Ako bilo koji od ta ograničenja primjenjuju određene scenariju.
