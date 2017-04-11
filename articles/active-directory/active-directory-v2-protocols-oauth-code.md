

<properties
    pageTitle="Azure AD 2.0 OAuth autorizacije kod tijek | Microsoft Azure"
    description="Sastavni web-aplikacije pomoću Azure AD implementaciju OAuth 2.0 protokola za provjeru autentičnosti."
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>Tijek 2.0 protokoli - kod autorizacije OAuth 2.0

Dodjela kod za autorizaciju OAuth 2.0 može se koristiti u aplikacijama koje se instaliraju na uređaju da biste pristupili zaštićeni resurse, kao što je web API-ji.  Pomoću aplikacije 2.0 model implementacije OAuth 2.0, možete dodati prijava i API pristup prijenosnih i stolnih aplikacija.  Ovaj vodič je jezik neovisno i opisuje način slanja i primanja poruka HTTP bez korištenja bilo koji od naše biblioteke Otvori izvor.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

Tijek kod za autorizaciju OAuth 2.0 je opisano u sekciju [4.1 specifikacija OAuth 2.0](http://tools.ietf.org/html/rfc6749).  Koristi se za izvođenje provjere autentičnosti i autorizacije u većini vrste aplikacije, uključujući [web-aplikacije](active-directory-v2-flows.md#web-apps) i [nativno instalirane aplikacije](active-directory-v2-flows.md#mobile-and-native-apps).  Omogućuje aplikacije sigurno dobiti access_tokens koji se može koristiti za pristup resurse koji su zaštićeni pomoću krajnju točku 2.0.  

## <a name="protocol-diagram"></a>Protokol dijagrama
Visoke razine tijek cijelu provjere autentičnosti za aplikaciju lokalnog/mobile izgleda malo ovako:

![Tijek OAuth kod provjere autentičnosti](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Zahtjev za autorizaciju kod
Kod tijek autorizacije počinje koja usmjeruje korisniku klijenta s `/authorize` krajnjoj točki.  U ovaj zahtjev klijent označava dozvole potrebne za dohvaćanje od korisnika:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Kliknite vezu da biste taj zahtjev za izvršavanje! Nakon prijave, vaš preglednik mora biti preusmjereni `https://localhost/myapp/` s na `code` u adresnu traku.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| klijent | obavezno | Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su `common`, `organizations`, `consumers`, i smjernice za identifikatore.  Više pojedinosti potražite u članku [Osnove protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| response_type | obavezno | Mora sadržavati `code` za tijek kod za autorizaciju. |
| redirect_uri | preporučeno | Redirect_uri aplikacije, gdje mogu poslati i primio aplikacije odgovora za provjeru autentičnosti.  Ona mora u potpunosti odgovarati nešto redirect_uris registrirana na portalu osim mora biti kodirati url-om.  Za izvorni i mobilne aplikacije koristite zadanu vrijednost od `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| opseg | obavezno | Prostor odvojene popis [dosega](active-directory-v2-scopes.md) koji želite korisnik pristajete.  |
| response_mode | preporučeno | Određuje način na koji želite koristiti za slanje rezultata tokena natrag u aplikaciju.  Može biti `query` ili `form_post`.  |
| Stanje | preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena.  Možda ćete niz sav sadržaj koji želite.  Slučajno generiranim jedinstvenu vrijednost obično koristi za [Sprječavanje napada krivotvorina zahtjev za web-mjesta](http://tools.ietf.org/html/rfc6749#section-10.12).  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |
| upit | Neobavezno | Navodi vrstu interakcije s korisnikom koji je potreban.  Samo valjane trenutno su vrijednosti 'prijava', 'ništa,' i 'pristanak.  `prompt=login`korisniku unesite svoje vjerodajnice na taj zahtjev, negating jedinstvene prijave na će obavezno.  `prompt=none`suprotan - će osigurati da korisnik ne prikazuju s bilo kojeg interaktivne neće retkom.  Ako je zahtjev ne može dovršiti tihu putem jedinstvene prijave na krajnjoj točki 2.0 će vratiti pogrešku.  `prompt=consent`dijaloški okvir OAuth pristanak će se pokrenuti nakon korisnikove prijave u odjeljku pitanje korisniku dodijeliti dozvole za aplikaciju. |
| login_hint | Neobavezno | Može se koristiti unaprijed ispunjavanja polja adresa korisničko ime/e-pošte za prijavu na stranici za korisnika ako znate svoje korisničko ime na vrijeme.  Ovaj parametar često aplikacija će se koristiti tijekom ponovnu provjeru autentičnosti, već imate dobivenih korisničko ime iz prethodne prijavu pomoću na `preferred_username` zahtjeva. |
| domain_hint | Neobavezno | Može biti jedna od `consumers` ili `organizations`.  Ako je uključena, će preskočiti e-pošte istrage taj korisnik prolazi kroz na stranici 2.0 prijava, oni mogu dovesti do malo više optimiziranog korisničkog sučelja.  Često aplikacija će koristiti taj parametar tijekom ponovnu provjeru autentičnosti tako što izvlači na `tid` iz na prethodni prijavu.  Ako u `tid` zahtjeva je vrijednost `9188040d-6c67-4c5b-b112-36a304b66dad`, trebali biste koristiti `domain_hint=consumers`.  U suprotnom koristite `domain_hint=organizations`. |

U ovom trenutku korisnika će se tražiti da unositi vjerodajnice i dovršite provjeru autentičnosti.  Krajnja točka 2.0 i provjerite da korisnik ima pristanak dozvola označen u `scope` parametarski upit.  Ako korisnik ima ne pristanak na bilo koju od tih dozvola, ona će uputite korisnika da pristanak potrebne dozvole.  Detalji o [dozvolama, pristanak, i više klijentske aplikacije su navedene](active-directory-v2-scopes.md).

Kada korisnik potvrđuje i daje pristanak, vratit će krajnju točku 2.0 odgovor na aplikaciju na na navedeni `redirect_uri`, na način naveden u na `response_mode` parametar.

#### <a name="successful-response"></a>Uspješan odgovor
Uspješan odgovor pomoću `response_mode=query` izgleda:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| kod | Authorization_code koji ste tražili aplikaciju. Aplikaciju možete koristiti autorizacije kod da biste zatražili pristupni token resursa cilj.  Authorization_codes su vrlo kratkog lived, obično isteknu nakon 10 minuta. |
| Stanje | Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |

#### <a name="error-response"></a>Odgovor pogreška
Pogreška odgovora može biti poslan i da biste u `redirect_uri` tako da ih aplikaciju možete pravilno rukovati:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Kodovi pogrešaka za autorizaciju krajnjoj točki pogreške

U sljedećoj tablici opisane različite šifre pogrešaka koje se mogu vratiti u na `error` parametar odgovor pogreška.

| Kod pogreške | Opis | Akcija klijenta |
|------------|-------------|---------------|
| invalid_request | Protokol pogreška, kao što su Nedostaje obavezan parametar. | Popravite i pošaljite zahtjev. Ovo je u razvoju pogreške obično otkrivena tijekom početne testiranja.|
| unauthorized_client | Klijentska aplikacija nije dopušteno zahtjev za autorizaciju kod. | To se obično događa kada klijentska aplikacija nije registriran u Azure AD ili se dodaje na korisnikovu Azure AD klijent. Aplikaciju možete Pitaj korisnika s uputama za instalaciju aplikacije i dodavanje Azure AD. |
| access_denied | Vlasnik resursa zabranjen pristanak | Klijentska aplikacija možete obavijestiti korisnika koji se ne može nastaviti osim ako korisnik identifikacijskom. |
| unsupported_response_type | Autorizacija poslužitelj ne podržava vrstu odgovora u zahtjev. | Popravite i pošaljite zahtjev. Ovo je u razvoju pogreške obično otkrivena tijekom početne testiranja.|
|server_error | Poslužitelj je naišao na pogrešku. | Ponovno zahtjev. Te pogreške može biti privremeni uvjeta. Klijentska aplikacija možda se objašnjava što korisniku njegov odgovor kasni dospijeća privremenu pogrešku. |
| temporarily_unavailable | Poslužitelj privremeno je zauzet obraditi zahtjev. | Ponovno zahtjev. Klijentska aplikacija može se objašnjava što korisniku njegov odgovor kasni dospijeća privremeno. |
| invalid_resource |Cilj resursa nije valjan jer ne postoji, ne možete pronaći Azure AD ili nije ispravno konfigurirano.| To označava resursa, ako postoji, nije konfiguriran u klijentu. Aplikaciju možete Pitaj korisnika s uputama za instalaciju aplikacije i dodavanje Azure AD. |

## <a name="request-an-access-token"></a>Zahtjev za token za pristup
Sad kad ste nabavili programa authorization_code i dodijeljena dozvola korisnika, mogu iskoristiti u `code` za programa `access_token` željeni resursa, slanjem u `POST` zahtjev za na `/token` krajnja točka:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Pokušajte izvršava taj zahtjev u Postman! (Ne zaboravite da biste zamijenili na `code`)     [ ![Pokreću se u Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| klijent | obavezno | Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su `common`, `organizations`, `consumers`, i smjernice za identifikatore.  Više pojedinosti potražite u članku [Osnove protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| grant_type | obavezno | Mora biti `authorization_code` za tijek kod za autorizaciju. |
| opseg | obavezno | Popis prostora odvojene opsega.  Opsega zatražili u ovom leg, morate biti jednaku vrijednost u ili podskup opsega zatražili u prvom leg.  Ako opsezi navedeni u ovom zahtjevu protežu na više resursa poslužitelja, krajnju točku 2.0 će vratiti tokena za navedeni u prvom opsega resurs.  Detaljnije objašnjenje opsega priručniku [dozvola, dozvole, i opsega](active-directory-v2-scopes.md).  |
| kod | obavezno | Authorization_code koju ste nabavili u prvom leg tijek.   |
| redirect_uri | obavezno | Isti redirect_uri vrijednost koji je korišten za dohvaćanje u authorization_code. |
| client_secret | potrebne za web-aplikacije | Tajna aplikaciju koju ste stvorili na portalu za registraciju aplikacije za aplikacije.  To se ne smiju koristiti u aplikaciji za izvorni jer client_secrets nije pouzdano pohraniti na uređajima.  Za web-aplikacije i na webu API koji imaju mogućnost sigurna pohrana u client_secret na strani poslužitelja potreban je. |

#### <a name="successful-response"></a>Uspješan odgovor
Uspješan tokena odgovor će izgledati:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametar | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token traženi programa access. Aplikaciju možete koristiti ovaj token za provjeru autentičnosti zaštićenim resursa, kao što je web-mjesto API-JA. |
| token_type | Označava vrijednost tokena vrste. Samo vrsta s podrškom za Azure AD je nošenja  |
| expires_in | Koliko dugo token za pristup vrijedi (u sekundama). |
| opseg | Opsezi na access_token vrijedi za. |
| refresh_token |  OAuth 2.0 token osvježavanja. Aplikaciju možete koristiti ovaj token potrebne dodatne pristupna tokena nakon isteka trenutne token za pristup.  Refresh_tokens su long-lived, a možete koristiti da biste zadržali pristup resursima za prošireni vremenskog razdoblja.  Više detalja potražite [2.0 tokena referencu](active-directory-v2-tokens.md).  |
| id_token | Za nepotpisane JSON Web tokena (JWT). Base64Url mogu aplikacije dekodiranje segmenata ovaj token zahtijevanje informacija o korisniku prijavljenom. Aplikaciju možete predmemoriju vrijednosti te ih prikazati, ali ga ne oslanjate na njih u autorizacije i sigurnosna ograničenja.  Dodatne informacije o id_tokens potražite u članku [2.0 krajnjoj točki tokena referencu](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Odgovor pogreška
Pogreška odgovori će izgledati:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti.  |
| error_codes | Popis kodova STS konkretne pogreške koji mogu olakšati Dijagnostika.  |
| Vremenska oznaka | Vrijeme u kojem se pogreške. |
| trace_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika.  |
| correlation_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika preko komponente. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Kodovi pogrešaka pogrešaka tokena krajnje točke

| Kod pogreške | Opis | Akcija klijenta |
|------------|-------------|---------------|
| invalid_request | Protokol pogreška, kao što su Nedostaje obavezan parametar. | Rješavanje i pošaljite zahtjev |
| invalid_grant | Kod za provjeru autentičnosti nije valjan ili je istekla. | Isprobajte novi zahtjev za na `/authorize` krajnje točke |
| unauthorized_client | Čija je autentičnost provjerena klijent nemate ovlasti za korištenje ove vrste Dodjela autorizacije. | To se obično događa kada klijentska aplikacija nije registriran u Azure AD ili se dodaje na korisnikovu Azure AD klijent. Aplikaciju možete Pitaj korisnika s uputama za instalaciju aplikacije i dodavanje Azure AD. |
| invalid_client | Nije uspjela provjera autentičnosti klijenta. | Klijent vjerodajnice nisu valjane. Da biste riješili problem, administratora aplikacije ažurira vjerodajnice. |
| unsupported_grant_type | Autorizacija poslužitelj ne podržava vrstu grant autorizacije. | Promjena vrste Dodjela u zahtjev. Tu vrstu pogreške bi se trebale provoditi samo tijekom razvoja i se provjeravati tijekom početne testiranja. |
| invalid_resource | Cilj resursa nije valjan jer ne postoji, ne možete pronaći Azure AD ili nije ispravno konfigurirano. | To označava resursa, ako postoji, nije konfiguriran u klijentu. Aplikaciju možete Pitaj korisnika s uputama za instalaciju aplikacije i dodavanje Azure AD. |
| interaction_required | Zahtjev zahtijeva interakcije s korisnikom. Ako, na primjer, u koraku dodatno unositi potreban je. | Ponovno zahtjev uz isti resurs. |
| temporarily_unavailable | Poslužitelj privremeno je zauzet obraditi zahtjev. | Ponovno zahtjev. Klijentska aplikacija može se objašnjava što korisniku njegov odgovor kasni dospijeća privremeno.|

## <a name="use-the-access-token"></a>Korištenje token za pristup
Sad kad ste nabavili uspješno je `access_token`, token možete koristiti u zahtjevima za API na webu tako da uvrstite u na `Authorization` zaglavlja:

> [AZURE.TIP] Izvršavanje zahtjev u Postman! (Zamjena na `Authorization` zaglavlje prve)     [ ![Pokreću se u Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Osvježavanje token za pristup
Access_tokens su kratki mogli živjeti i morate ih osvježavaju nakon isteknu pristupu resursi.  To možete učiniti tako da drugi slanje `POST` zatražiti na `/token` krajnje točke, ovaj put pod uvjetom u `refresh_token` umjesto na `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Pokušajte izvršava taj zahtjev u Postman! (Ne zaboravite da biste zamijenili na `refresh_token`)     [ ![Pokreću se u Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametar | | Opis |
| ----------------------- | ------------------------------- | -------- |
| klijent | obavezno | Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su `common`, `organizations`, `consumers`, i smjernice za identifikatore.  Više pojedinosti potražite u članku [Osnove protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| grant_type | obavezno | Mora biti `refresh_token` za ovaj leg tijek autorizacije kod. |
| opseg | obavezno | Popis prostora odvojene opsega.  Opsega zatražili u ovom leg, morate biti jednaku vrijednost u ili podskup opsega zatražili u izvornom leg zahtjev authorization_code.  Ako opsezi navedeni u ovom zahtjevu protežu na više resursa poslužitelja, krajnju točku 2.0 će vratiti tokena za navedeni u prvom opsega resurs.  Detaljnije objašnjenje opsega priručniku [dozvola, dozvole, i opsega](active-directory-v2-scopes.md).  |
| refresh_token | obavezno | Refresh_token koju ste nabavili u drugom leg tijek.   |
| redirect_uri | obavezno | Isti redirect_uri vrijednost koji je korišten za dohvaćanje u authorization_code. |
| client_secret | potrebne za web-aplikacije | Tajna aplikaciju koju ste stvorili na portalu za registraciju aplikacije za aplikacije.  To se ne smiju koristiti u aplikaciji za izvorni jer client_secrets nije pouzdano pohraniti na uređajima.  Za web-aplikacije i na webu API koji imaju mogućnost sigurna pohrana u client_secret na strani poslužitelja potreban je. |

#### <a name="successful-response"></a>Uspješan odgovor
Uspješan tokena odgovor će izgledati:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametar | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token traženi programa access. Aplikaciju možete koristiti ovaj token za provjeru autentičnosti zaštićenim resursa, kao što je web-mjesto API-JA. |
| token_type | Označava vrijednost tokena vrste. Samo vrsta s podrškom za Azure AD je nošenja  |
| expires_in | Koliko dugo token za pristup vrijedi (u sekundama). |
| opseg | Opsezi na access_token vrijedi za. |
| refresh_token |  Novi token OAuth 2.0 osvježavanja. Stari token osvježavanja treba zamijeniti ovaj token upravo nabave Osvježi da biste bili sigurni token za osvježavanje ostaju vrijedi za dugo moguće.  |
| id_token | Za nepotpisane JSON Web tokena (JWT). Base64Url mogu aplikacije dekodiranje segmenata ovaj token zahtijevanje informacija o korisniku prijavljenom. Aplikaciju možete predmemoriju vrijednosti te ih prikazati, ali ga ne oslanjate na njih u autorizacije i sigurnosna ograničenja.  Dodatne informacije o id_tokens potražite u članku [2.0 krajnjoj točki tokena referencu](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Odgovor pogreška
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti.  |
| error_codes | Popis kodova STS konkretne pogreške koji mogu olakšati Dijagnostika.  |
| Vremenska oznaka | Vrijeme u kojem se pogreške. |
| trace_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika.  |
| correlation_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika preko komponente. |

Opis šifre pogrešaka i akciju preporučeni klijent potražite [pogreške šifre pogrešaka tokena krajnjoj točki](#error-codes-for-token-endpoint-errors).
