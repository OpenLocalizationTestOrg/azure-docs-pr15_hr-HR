<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Stvaranje web-aplikacije pomoću servisa Azure Active Directory implementacije protokol za povezivanje OpenID provjere autentičnosti."
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web-prijava s OpenID povezivanje

OpenID Connect nije protokol provjere autentičnosti, on nudi OAuth 2.0 koje je moguće koristiti za sigurno potpisivanje korisnicima u web-aplikacije.  Pomoću OpenID povezivanje implementacije B2C Azure Active Directory (Azure AD) možete spoljni prijave, prijavu i drugih upravljanje identitetom iskustvo u web-aplikacijama za Azure AD. Ovaj vodič vidjet ćete upute za taj jezik-neovisno o postavkama.. To će opisuju slanje i primanje poruka HTTP bez korištenja bilo koji od naše biblioteke Otvori izvor.

[Povezivanje OpenID](http://openid.net/specs/openid-connect-core-1_0.html) proširuje protokol *autorizacije* OAuth 2.0 za korištenje kao protokol za *provjeru autentičnosti* . Omogućuje obavljaju jedinstvenu prijavu pomoću OAuth. On predstavlja pojam programa `id_token`, što je sigurnosni token koji omogućuje klijentskog programa da biste provjerili identitet korisnika i dobiti Osnovni profil informacija o korisniku.

Budući da se proteže OAuth 2.0, također omogućuje aplikacije sigurno dobiti **access_tokens**. Možete koristiti access_tokens pristupa resursima koji su zaštićeni [provjeru autentičnosti poslužitelja](active-directory-b2c-reference-protocols.md#the-basics). Preporučujemo povezivanje OpenID ako ste stvaranje web-aplikacije koja se nalazi na poslužitelju i pristupiti putem preglednika. Ako želite dodati upravljanje identitetom prijenosno ili stolno aplikacija pomoću Azure AD B2C, trebali biste koristiti [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) umjesto OpenID povezivanje.

Azure AD B2C proširuje standardni OpenID povezivanje protokol potrebno više od jednostavne provjere autentičnosti i autorizacije. On predstavlja [**parametar pravila**](active-directory-b2c-reference-policies.md), koji omogućuje korištenje OpenID povezivanje da biste dodali korisničkog sučelja aplikacije – kao što su prijave, prijava i upravljanje profila. Ovdje Pokazat ćemo vam kako koristiti OpenID povezivanje i pravila za implementaciju svaki od tih mogućnosti u web-aplikacijama. Također Pokazat ćemo vam kako access_tokens za pristup web API-ji.

Donji primjer HTTP zahtjeva koristit će naš direktorija B2C uzorka, **fabrikamb2c.onmicrosoft.com**, kao i naše uzorka aplikacije **https://aadb2cplayground.azurewebsites.net** i pravila. Slobodno isprobajte zahtjeve sami pomoću ove vrijednosti ili ih možete zamijeniti s vlastitim.
Saznajte kako [dobiti vlastiti B2C klijent, aplikaciju, i pravila](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Slanje zahtjeva za provjeru autentičnosti
Kada web-aplikaciju programa morate provjere autentičnosti korisnika i izvršavanje pravilo, možete uputiti korisniku na `/authorize` krajnjoj točki. Ovo je interaktivne dio tijek, gdje korisnik će zapravo akcija, ovisno o pravilniku.

U ovaj zahtjev klijent označava dozvole koje je potrebno Nabava korisnika u na `scope` parametar i pravilo koje će se izvršiti u na `p` parametar. Tri primjera navedene u nastavku (s prijelomima zbog čitljivosti), svaki putem različitih pravila. Da biste dobili na dojam za svaki zahtjev za funkcioniranje, pokušajte zahtjev zalijepite u pregledniku, a zatim ga pokrenete.

#### <a name="use-a-sign-in-policy"></a>Pomoću pravila za prijavu

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Koristi pravilo za prijavu

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Korištenje pravilo za uređivanje profila

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametar | Obavezan? | Opis |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Obavezno | ID aplikacije koji [Azure portal](https://portal.azure.com/) dodijeljene aplikacije. |
| response_type | Obavezno | Upišite odgovor koji moraju sadržavati `id_token` za povezivanje OpenID. Ako web-aplikaciju programa i potrebna tokeni za pozivanje API web-mjesto, možete koristiti `code+id_token`, kao što je napravili smo ovdje. |
| redirect_uri | Preporučeno | Redirect_uri aplikacije, gdje mogu poslati i primio aplikacije odgovora za provjeru autentičnosti. Ga mora u potpunosti odgovarati nešto redirect_uris koju ste registrirali portalu osim što se mora biti kodirati URL-om. |
| opseg | Obavezno | Popis prostora odvojene opsega. Opseg jednu vrijednost označava za Azure AD i dozvola koje su tražena. Na `openid` opseg označava dozvola Prijava korisnika i dohvaćanje podataka o korisniku u obliku **id_tokens** (više da biste došli na ovo u nastavku članka). Na `offline_access` opseg nije obavezna za web-aplikacije. Označava da aplikacije ćete **refresh_token** long-lived pristupa resursima. |
| response_mode | Preporučeno | Način na koji želite koristiti za slanje rezultata authorization_code natrag u aplikaciju. To može biti nešto od "query", 'form_post' ili 'fragment'.  Za najbolje sigurnost, preporučuje se 'form_post'. |
| Stanje | Preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena. Možda ćete niz sav sadržaj koji želite. Slučajno generiranim jedinstvenu vrijednost obično koristi za sprječavanje napada krivotvorina zahtjev za web-mjesta. Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su na stranici. |
| Jednokratna šifra | Obavezno | Vrijednost koja se uključiti u zahtjevu za (generira aplikaciju) koje će biti obuhvaćene dobivene id_token kao zahtjev. Aplikaciju možete provjeriti pa tu vrijednost u smanjenju tokena Ponovi napada. Vrijednost je obično kombinaciji, jedinstveni niz koji se mogu koristiti za prepoznavanje polazište zahtjev. |
| p | Obavezno | Pravila koja će se izvršavati. To je naziv pravila koja je stvorena u klijentu B2C. Vrijednost za naziv pravilnika započeti s "b2c\_1\_". Dodatne informacije o pravilima u [okvir Extensible pravila](active-directory-b2c-reference-policies.md). |
| upit | Neobavezno | Vrste interakcije s korisnikom koji je potreban. Samo valjana vrijednost trenutno je 'prijava', koje navodi korisnika da biste unijeli vjerodajnice na taj zahtjev. Jedinstvenu prijavu ne primjenjuje se. |

Sada korisnika će se zatražiti da biste dovršili pravila tijeka rada. To može obuhvaćati korisnika unesete korisničko ime i lozinku, prijaviti pomoću društvenih identitet registracije za imenika ili bilo kojeg drugog broja koraka, ovisno o tome kako je definirano pravila.

Kada korisnik dovrši pravilnik, vratit će Azure AD odgovor na aplikaciju na na navedeni `redirect_uri`, pomoću metode amortizacije koja je navedena u na `response_mode` parametar. Odgovor će biti potpuno isto za svaku iznad slučajeva, neovisno o pravilniku izvršen.

Uspješan odgovor pomoću `response_mode=fragment` izgledati:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| id_token | Id_token koji ste tražili aplikaciju. Da biste provjerili identitet korisnika i počnite sesije s korisnikom možete koristiti u id_token. Dodatne informacije o id_tokens i njihov sadržaj nalaze se u [Azure AD B2C tokena referencu](active-directory-b2c-reference-tokens.md). |
| kod | Authorization_code koji aplikaciju tražili, ako ste koristili `response_type=code+id_token`. Aplikaciju možete koristiti autorizacije kod da biste zatražili programa access_token resursa cilj. Authorization_codes su vrlo kratkog lived. Obično isteknu nakon 10 minuta. |
| Stanje | Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |

Pogreška odgovora i može poslati na `redirect_uri` tako da ih aplikaciju možete pravilno rukovati:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti. |
| Stanje | Pogledajte cijeli opis u prethodnoj tablici. Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |


## <a name="validate-the-idtoken"></a>Provjerite valjanost u id_token
Samo primanje programa id_token nije dovoljno za provjeru autentičnosti korisnika – morate provjeriti valjanost potpisa u id_token i provjerite je li zahtjevima u token po preduvjeti za aplikaciju programa. Azure AD B2C koristi [Tokeni JSON Web (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) i javni ključ šifriranja za potpisivanje tokena i provjerite jesu li valjan.

Nema više Otvori izvor biblioteke koje su dostupne za provjeru valjanosti JWTs, ovisno o jezik Preference. Preporučujemo da Istraživanje te mogućnosti umjesto implementacijom vlastite logiku provjere valjanosti. Informacije Ovdje će biti korisno u može ispravno pomoću te biblioteke.

Azure AD B2C ima OpenID povezivanje metapodataka krajnje, koji omogućuje uređivanje aplikacije za dohvaćanje informacija o Azure AD B2C tijekom rada. Te informacije obuhvaćaju krajnje točke, tokena sadržaj i token potpisivanja tipke. Postoji dokumenta JSON metapodataka za svaku u klijentu B2C. Na primjer dokument metapodataka za na `b2c_1_sign_in` pravila u `fabrikamb2c.onmicrosoft.com` nalazi se na:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Jedna je od svojstava ovog dokumenta za konfiguraciju na `jwks_uri`, čija vrijednost za bio ista pravila:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

U narudžbe da biste odredili pravilo korišten u potpis je id_token (i mjesto za dohvaćanje metapodataka iz), imate dvije mogućnosti. Najprije obuhvaćeno naziv pravila u `acr` zahtjeva u na id_token. Informacije o raščlaniti zahtjevima iz programa id_token, potražite u članku [Azure AD B2C tokena referencu](active-directory-b2c-reference-tokens.md). Druga mogućnost je kodiranje pravila u vrijednosti u `state` parametar kada je poslao zahtjev i dekodiranje da biste odredili koristila pravilo. Savršeno vrijedi bilo koji način.

Nakon što ste nabavili dokument metapodataka iz metapodataka krajnju točku OpenID povezivanje, možete koristiti RSA 256 javni ključ (koji se nalaze na ovom krajnje točke) da biste provjerili valjanost potpisa u id_token. Mogu postojati više tipki navedeni na ovom krajnjoj točki bilo kojem trenutku navedene u vremenu, svaki otkrije na `kid`. U zaglavlju na id_token sadrži i na `kid` zahtjeva, koji upućuje na to koju od tih ključeva koristila da biste se prijavili u id_token. Potražite u članku [Azure AD B2C tokena referenca](active-directory-b2c-reference-tokens.md) dodatne informacije, uključujući sekcije na [Provjera valjanosti tokeni](active-directory-b2c-reference-tokens.md#validating-tokens) i [važne informacije o prijavi ključa prilikom prelaska](active-directory-b2c-reference-tokens.md#validating-tokens).
<!--TODO: Improve the information on this-->

Nakon što ste potvrđuju potpis na id_token, postoji nekoliko zahtjevima koje je potrebno da biste provjerili, na primjer:

- Trebali biste provjeriti na `nonce` zahtjeva da biste spriječili tokena Ponovi napadi. Vrijednost mora biti naveden u zahtjevu za prijavu.
- Trebali biste provjeriti na `aud` zahtjeva da bi se id_token izdavanja aplikacije. Vrijednost mora biti ID aplikacije aplikacije.
- Trebali biste provjeriti na `iat` i `exp` zahtjeve da biste bili sigurni da se id_token nije istekao.

Postoji nekoliko dodatne provjere valjanosti moraju izvršiti, opisani u [OpenID specifikacija Core za povezivanje](http://openid.net/specs/openid-connect-core-1_0.html).  Možda želite provjeriti dodatne zahtjevima, ovisno o scenariju. Neke uobičajene provjere obuhvaćaju sljedeće:

- Osiguravanje da se korisnik/tvrtka ili ustanova registrirala za aplikaciju.
- Provjera ima li korisnik proper autorizacija/ovlasti.
- Osiguravanje da određene jačinu provjere autentičnosti došlo je do, kao što su Azure višestruke provjere autentičnosti.

Dodatne informacije o zahtjevima u programa id_token potražite u članku [Azure AD B2C tokena referencu](active-directory-b2c-reference-tokens.md).

Nakon što ste potpuno nastavlja na id_token, možete započeti sesije s korisničkim i koristiti na zahtjevima u u id_token da biste dobili informacije o korisniku u aplikaciju. Ove informacije može se koristiti za prikaz, zapisa, autorizacije i tako dalje.

## <a name="get-a-token"></a>Početak token
Ako je sve vaše potrebe web app da biste učinili izvršavanje pravila, možete preskočiti na sljedeće odjeljke nekoliko. Odnosi se na web-aplikacije koje morate donijeti autentičnost pozive na web-mjesto API-JA i i zaštićen Azure AD B2C su samo ove sekcije.

Ne možete iskoristiti authorization_code koji koju ste nabavili (pomoću `response_type=code+id_token`) token željeni resursa slanjem u `POST` zahtjev za na `/token` krajnje točke. Trenutno samo resurs koji možete zatražiti token za je vaše aplikacije vlastite pozadinskog API na webu. Konvencije za traženje token sebi je da biste koristili aplikaciju programa ID klijenta kao opseg:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parametar | Obavezan? | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| p | Obavezno | Pravila koja se koristila dobiti kod za autorizaciju. Ne možete koristiti različite pravila u zahtjev. Imajte na umu dodati taj parametar u **nizu upita**, ne i u tijelu objavu. |
| client_id | Obavezno | ID aplikacije koji [Azure portal](https://portal.azure.com/) dodijeljene aplikacije. |
| grant_type | Obavezno | Vrsta dodijelite, mora biti `authorization_code` za tijek kod za autorizaciju. |
| opseg | Preporučeno | Popis prostora odvojene opsega. Opseg jednu vrijednost označava za Azure AD i dozvola koje su tražena. Na `openid` opseg označava dozvola Prijava korisnika i dohvaćanje podataka o korisniku u obliku **id_tokens**. To se može koristiti da biste dobili tokeni vaše aplikacije vlastite pozadinskog web-mjesto API, se iskazuje isti ID aplikacije kao klijentskog programa. Na `offline_access` opseg označava da aplikacije ćete **refresh_token** long-lived pristupa resursima. |
| kod | Obavezno | Authorization_code koju ste nabavili u prvom leg tijek. |
| redirect_uri | Obavezno | Redirect_uri aplikaciju koju ste primili na authorization_code. |
| client_secret | Obavezno | Tajna aplikacije koji je generirao [Azure portal](https://portal.azure.com/). U ovom aplikaciji tajna je artefakt zaštitu. Pohranite je sigurno na poslužitelj. Treba vam i pobrinuti da biste zakrenuli ovaj tajna klijenta na temelju periodički. |

Uspješan tokena odgovor će izgledati:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametar | Opis |
| ----------------------- | ------------------------------- |
| not_before | Vrijeme na kojoj se smatra valjani u vremenu epoch token. |
| token_type | Vrijednost tokena vrste. Samo vrsta s podrškom za Azure AD je nošenja. |
| access_token | Traje JWT token koji ste tražili. |
| opseg | Dosezi koji token vrijedi za, koji se može koristiti za predmemoriranje tokeni za kasnije korištenje. |
| expires_in | Duljina vremenskih vrijednosti duljih na access_token valjani (u sekundama). |
| refresh_token | Na refresh_token OAuth 2.0. Aplikaciju možete koristiti ovaj token potrebne dodatne tokeni nakon isteka trenutne token.  Refresh_tokens su mogli dugo živjeti pa se poslužite da biste zadržali pristup resursima za prošireni vremenskog razdoblja. Više detalja potražite [B2C tokena referencu](active-directory-b2c-reference-tokens.md). Napomena morate koristite opsega `offline_access` u autorizacije i tokena zahtjeva da bi primili na refresh_token. |

Pogreška odgovori će izgledati:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti. |

## <a name="use-the-token"></a>Korištenje tokena
Sad kad ste nabavili uspješno programa `access_token`, token možete koristiti u zahtjevima za vaše pozadinski web-API-ji stvorenim u na `Authorization` zaglavlja:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Osvježavanje tokena
Na id_tokens su kratkog lived. Morate ih osvježavaju nakon isteknu i dalje moći pristupati resursi. To možete učiniti tako da drugi slanje `POST` zatražiti na `/token` krajnjoj točki. Navedite ovaj put u `refresh_token` umjesto na `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametar | Obavezno | Opis |
| ----------------------- | ------------------------------- | -------- |
| p | Obavezno | Pravila koja se koristila za izvorni refresh_token. Ne možete koristiti različite pravila u zahtjev. Imajte na umu dodati taj parametar u **nizu upita**, ne i u tijelu objavu. |
| client_id | Obavezno | ID aplikacije koji [Azure portal](https://portal.azure.com/) dodijeljene aplikacije. |
| grant_type | Obavezno | Vrsta dodijelite, mora biti `refresh_token` za ovaj leg tijek autorizacije kod. |
| opseg | Preporučeno | Popis prostora odvojene opsega. Opseg jednu vrijednost označava za Azure AD i dozvola koje su tražena. Na `openid` opseg označava dozvola Prijava korisnika i dohvaćanje podataka o korisniku u obliku **id_tokens**. To se može koristiti da biste dobili tokeni vaše aplikacije vlastite pozadinskog web-mjesto API, se iskazuje isti ID aplikacije kao klijentskog programa. Na `offline_access` opseg označava da aplikacije ćete **refresh_token** long-lived pristupa resursima. |
| redirect_uri | Preporučeno | Redirect_uri aplikaciju koju ste primili na authorization_code. |
| refresh_token | Obavezno | Na izvornom refresh_token koju ste nabavili u drugom leg tijek. Napomena morate koristite opsega `offline_access` u autorizacije i tokena zahtjeva da bi primili na refresh_token. |
| client_secret | Obavezno | Tajna aplikacije koji je generirao [Azure portal](https://portal.azure.com/). U ovom aplikaciji tajna je artefakt zaštitu. Pohranite je sigurno na poslužitelj. Treba vam i pobrinuti da biste zakrenuli ovaj tajna klijenta na temelju periodički. |

Uspješan tokena odgovor će izgledati:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametar | Opis |
| ----------------------- | ------------------------------- |
| not_before | Vrijeme na kojoj se smatra valjani u vremenu epoch token. |
| token_type | Vrijednost tokena vrste. Samo vrsta s podrškom za Azure AD je nošenja. |
| access_token | Traje JWT token koji ste tražili. |
| opseg | Dosezi koji token vrijedi za, koji se može koristiti za predmemoriranje tokeni za kasnije korištenje. |
| expires_in | Duljina vremenskih vrijednosti duljih na access_token valjani (u sekundama). |
| refresh_token | Na refresh_token OAuth 2.0. Aplikaciju možete koristiti ovaj token potrebne dodatne tokeni nakon isteka trenutne token.  Refresh_tokens su mogli dugo živjeti pa se poslužite da biste zadržali pristup resursima za prošireni vremenskog razdoblja. Više detalja potražite [B2C tokena referencu](active-directory-b2c-reference-tokens.md). |

Pogreška odgovori će izgledati:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti. |


## <a name="send-a-sign-out-request"></a>Slanje zahtjeva za sign-out

Kada želite da biste se odjavili iz aplikacije korisnika nije dovoljno da biste očistili kolačiće ili neki drugi način end pokrenite aplikaciju sesije s korisnikom. Korisnik mora preusmjeriti i za Azure AD za odjavu. Ako vam se neće da biste to učinili, korisnik možda moći reauthenticate na aplikaciju bez ponovnog unosa vjerodajnica. To znači da imaju valjane jednu prijave sesiju s Azure AD.

Jednostavno možete preusmjeriti korisniku na `end_session_endpoint` koji je naveden u istom OpenID povezivanje metapodataka dokumentu opisane u prethodnom odjeljku "Provjeri valjanost na id_token":

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametar | Obavezan? | Opis |
| ----------------------- | ------------------------------- | ------------ |
| p | Obavezno | Pravilo koje želite koristiti za prijavu korisnika iz aplikacije. |
| post_logout_redirect_uri | Preporučeno | URL koji korisnik mora biti preusmjereni nakon uspješne sign-out. Ako nije uvrštena, korisnik prikazat će se generički poruke tako da Azure AD B2C.  |

> [AZURE.NOTE]
    Tijekom koja usmjeruje korisniku na `end_session_endpoint` će izbrisati neke od korisnika jedan prijave stanje s Azure AD B2C, će Prijava korisnika iz društvenih identiteta sesija davatelja (IDP). Ako korisnik odabere iste IDP tijekom na sljedeće prijave, oni će biti reauthenticated, bez unosa vjerodajnica. Ako korisnik želi da biste se odjavili iz aplikacije B2C, ona ne znači nužno žele da biste se potpuno odjavili svoj račun za Facebook. Međutim, ako lokalni računi korisnika sesija će se završiti pravilno.

## <a name="use-your-own-b2c-tenant"></a>Korištenje B2C klijentu

Ako želite isprobati te zahtjeve za sebe, morate najprije izvođenje ta tri koraka, a zatim zamijenite primjere vrijednosti iznad s vlastitim:

- [Stvaranje B2C klijenta](active-directory-b2c-get-started.md)i koristite naziv svoje klijenta u zahtjeve.
- [Stvaranje aplikacije](active-directory-b2c-app-registration.md) da biste dobili ID aplikacije i u redirect_uri. Koje želite uvrstiti u **web-aplikacije/webom api** u aplikacije i po želji stvaranje **aplikaciji tajna**.
- [Stvaranje police](active-directory-b2c-reference-policies.md) da biste dobili naziva pravila.
