<properties
    pageTitle="Pregled protokol za Azure AD .NET | Microsoft Azure"
    description="U ovom se članku opisuje način korištenja HTTP poruke da biste autorizirali pristup web-aplikacije i web-API-ji na klijentu pomoću servisa Azure Active Directory i OpenID povezivanje."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Autorizirajte pristup web-aplikacije pomoću OpenID povezivanje i Azure Active Directory

[Povezivanje OpenID](http://openid.net/specs/openid-connect-core-1_0.html) je sloj jednostavne identiteta on nudi protokol OAuth 2.0. OAuth 2.0 definira mehanizme da biste dobili i koristite **tokeni pristup** da biste pristupili zaštićeni resursa, ali ne definiraju standardne metode možete unijeti informacije o identitetu. Povezivanje OpenID implementira provjere autentičnosti kao datotečni nastavak za autorizaciju postupak OAuth 2.0 dajući informacije o krajnjeg korisnika u obliku programa `id_token` koji provjerava identitet korisnika, kao i navode Osnovni profil informacija o korisniku.

Povezivanje OpenID je naš preporuke ako izrađujete web-aplikacije koja se nalazi na poslužitelju i pristupiti putem preglednika.

## <a name="authentication-flow-using-openid-connect"></a>Provjera autentičnosti tijek pomoću OpenID povezivanja

Osnovni tijek adresa za prijavu sadrži sljedeće korake: svaku od njih je opisan u nastavku.

![OpenId povezivanje tijek provjere autentičnosti](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Pošaljite zahtjev za prijavu

Kada web-aplikacije morate provjere autentičnosti korisnika, morate Izravni korisniku na `/authorize` krajnjoj točki. Zahtjev je slična prvi leg od na [OAuth 2.0 autorizacije kod tijeka](active-directory-protocols-oauth-code.md), s nekoliko važnih distinctions:

- Zahtjev mora sadržavati opsega `openid` u na `scope` parametar.
- Na `response_type` parametar mora sadržavati `id_token`.
- Zahtjev mora sadržavati na `nonce` parametar.

Da bi se zahtjev za uzorak izgleda ovako:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| klijent | obavezno | Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su identifikatori klijenta, npr. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` ili `contoso.onmicrosoft.com` ili `common` za klijent neovisno tokena |
| client_id | obavezno | Id aplikacije dodijeljene aplikacije kada ste se registrirali s Azure AD. To možete pronaći na portalu klasični Azure. Kliknite na **Servisu Active Directory**, kliknite imenik, kliknite na aplikaciju pa na **Konfiguracija** |
| response_type | obavezno | Mora sadržavati `id_token` za povezivanje OpenID prijavu.  Uključivati i druge response_types, kao što su `code`. |
| opseg | obavezno | Popis prostora odvojene opsega.  Za povezivanje OpenID, morate uključiti opsega `openid`, koja se prevodi dozvolu "Možemo prijaviti" u pristanak korisničkog Sučelja.  Uključivati i drugih dosega u zahtjev za traženje odobrenja. |
| Jednokratna šifra | obavezno | Vrijednost uključeni u zahtjevu za generira aplikacije koje će biti obuhvaćene na rezultat `id_token` kao zahtjev.  Aplikaciju možete provjeriti pa tu vrijednost u smanjenju tokena Ponovi napadi.  Vrijednost je obično u kombinaciji, jedinstveni niz ili GUID koje je moguće koristiti za prepoznavanje polazište zahtjev.  |
| redirect_uri | preporučeno | Redirect_uri aplikacije, gdje mogu poslati i primio aplikacije odgovora za provjeru autentičnosti.  Ona mora u potpunosti odgovarati nešto redirect_uris registrirana na portalu osim mora biti kodirati url-om. |
| response_mode | preporučeno | Određuje način na koji želite koristiti za slanje rezultata authorization_code vratite se u aplikaciju.  Podržani vrijednosti su `form_post` za *HTTP obrasca objavite* ili `fragment` za *URL djelić*.  Za web-aplikacije preporučujemo korištenje `response_mode=form_post` da biste bili sigurni najsigurnija prijenos tokeni u aplikaciji.  
| Stanje | preporučeno | Vrijednost obuhvaćene zahtjev koji će se i vratiti u odgovoru tokena.  Možda ćete niz sav sadržaj koji želite.  Slučajno generiranim jedinstvenu vrijednost obično koristi za [Sprječavanje napada krivotvorina zahtjev za web-mjesta](http://tools.ietf.org/html/rfc6749#section-10.12).  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |
| upit | Neobavezno | Navodi vrstu interakcije s korisnikom koji je potreban.  Samo valjane trenutno su vrijednosti 'prijava', 'ništa,' i 'pristanak.  `prompt=login`korisniku unesite svoje vjerodajnice na taj zahtjev, negating jedinstvene prijave na će obavezno.  `prompt=none`suprotan - će osigurati da korisnik ne prikazuju s bilo kojeg interaktivne neće retkom.  Ako se zahtjev ne može dovršiti tihu putem jedinstvene prijave na krajnjoj će vratiti pogrešku.  `prompt=consent`dijaloški okvir OAuth pristanak će se pokrenuti nakon korisnikove prijave u odjeljku pitanje korisniku dodijeliti dozvole za aplikaciju. |
| login_hint | Neobavezno | Može se koristiti unaprijed ispunjavanja polja adresa korisničko ime/e-pošte za prijavu na stranici za korisnika ako znate svoje korisničko ime na vrijeme.  Ovaj parametar često aplikacija će se koristiti tijekom ponovnu provjeru autentičnosti, već imate dobivenih korisničko ime iz prethodne prijavu pomoću na `preferred_username` zahtjeva. |


U ovom trenutku korisnika će se tražiti da unositi vjerodajnice i dovršite provjeru autentičnosti.

### <a name="sample-response"></a>Ogledna odgovor

Ogledna odgovor, nakon provjere autentičnosti korisnika može izgledati ovako:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| id_token | Na `id_token` koji ste tražili aplikaciju. Možete koristiti u `id_token` da biste provjerili identitet korisnika i počnite sesije s korisnikom.  |
| Stanje | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena. Slučajno generiranim jedinstvenu vrijednost obično koristi za [Sprječavanje napada krivotvorina zahtjev za web-mjesta](http://tools.ietf.org/html/rfc6749#section-10.12).  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |

### <a name="error-response"></a>Odgovor pogreška
Pogreška odgovora može biti poslan i da biste u `redirect_uri` tako da ih aplikaciju možete pravilno rukovati:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
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

## <a name="validate-the-idtoken"></a>Provjerite valjanost u id_token

Samo primanje programa `id_token` nije dovoljno provjere autentičnosti korisnika; morate provjeriti valjanost potpisa i provjerite je li zahtjevima u na `id_token` po preduvjeti za aplikaciju programa. Krajnja točka za Azure AD koristi tokeni JSON Web (JWTs) i javni ključ šifriranja za potpisivanje tokena i provjerite jesu li valjan.

Možete odabrati da biste provjerili valjanost u `id_token` u klijentu kodu, ali uobičajeno je slanje na `id_token` pozadinski poslužitelj i izvršiti provjeru postoji. Nakon što ste provjeriti potpis na `id_token`, postoji nekoliko zahtjevima vas će se tražiti da biste potvrdili.

Možda želite provjeriti dodatne zahtjevima ovisno o scenariju. Neke uobičajene provjere obuhvaćaju sljedeće:

- Osiguravanje korisnika/tvrtka ili ustanova ima prijavili za aplikaciju.
- Jamči da korisnik ima odgovarajuće autorizacija/ovlasti
- Osiguravanje određene jačinu provjere autentičnosti došlo je do, kao što su višestruke provjere autentičnosti.

Kada ste potpuno nastavlja na `id_token`, možete početi sesije s korisnikom i koristiti zahtjevima u na `id_token` da biste dobili informacije o korisniku u svojoj aplikaciji. Ove informacije mogu koristiti za prikaz, zapise, autorizacijama itd. Dodatne informacije o tokena vrste i zahtjevima pročitajte [podržane tokena i vrste zahtjeva](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Slanje Odjava zahtjev

Kada želite da biste se odjavili iz aplikacije korisnika, nije dovoljno da biste očistili kolačiće ili neki drugi način end pokrenite aplikaciju sesije s korisnikom.  Obavezno preusmjeravanje i korisniku na `end_session_endpoint` za odjavu.  Ako da biste to učinili, korisnik će moći ponovno provjeru autentičnosti aplikacije bez unosa svoja uvjerenja ponovno, budući da imaju valjane jednu prijave sesiju s krajnju točku Azure AD.

Jednostavno možete preusmjeriti korisniku na `end_session_endpoint` popisu metapodataka dokumenta OpenID povezivanje:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametar | | Opis |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | preporučeno | URL koji korisnik mora biti preusmjereni nakon uspješne odjavite.  Ako nije uvršten, korisnik će biti prikazani generički poruke.  |

## <a name="token-acquisition"></a>Nabava tokena

Mnoge web-aplikacije moraju ne samo se korisnik nalazi se prijaviti, ali pristupiti web-servisa za tog korisnika pomoću OAuth. Ovaj scenarij kombinira OpenID povezivanje za provjeru autentičnosti korisnika prilikom istodobno dobivanje programa `authorization_code` koja se može koristiti da biste dobili `access_tokens` pomoću tijeka kod OAuth autorizacije.

## <a name="get-access-tokens"></a>Dobiti pristup tokena

Dobiti pristup tokena, morat ćete izmijeniti zahtjevu za prijavu odozgo:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Uključujući opsega dozvola u zahtjevu za i korištenjem `response_type=code+id_token`, `authorize` krajnjoj točki će osigurati da korisnik ima pristanak dozvola označen u `scope` parametarski upit, a zatim se vratite aplikacije programa autorizacije koda za exchange za token za pristup.

### <a name="successful-response"></a>Uspješan odgovor

Uspješan odgovor pomoću `response_mode=form_post` izgleda:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| id_token | Na `id_token` koji ste tražili aplikaciju. Možete koristiti u `id_token` da biste provjerili identitet korisnika i počnite sesije s korisnikom. |
| kod | Authorization_code koji ste tražili aplikaciju. Aplikaciju možete koristiti autorizacije kod da biste zatražili pristupnog tokena resursa cilj. Authorization_codes su vrlo kratkog lived, obično isteknu nakon 10 minuta. |
| Stanje | Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |

### <a name="error-response"></a>Odgovor pogreška

Pogreška odgovora može biti poslan i da biste u `redirect_uri` tako da ih aplikaciju možete pravilno rukovati:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer identificiranje uzrok pogreške provjere autentičnosti.  |

Opis šifre mogućih pogrešaka i njihovih akcija preporučeni klijent potražite [šifre pogrešaka za autorizaciju krajnjoj točki pogreške](#error-codes-for-authorization-endpoint-errors).

Nakon što ste navikli u autorizacije `code` i `id_token`, možete Prijava korisnika i dobiti pristup tokeni u njihovo ime.  Da biste se prijavili se korisnik nalazi, morate provjeriti na `id_token` točno onako kao što je opisano iznad. Da biste dobili pristup tokena, slijedite korake opisane u našem [OAuth protokol dokumentacije](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token).
