<properties
    pageTitle="Pregled protokol za Azure AD .NET | Microsoft Azure"
    description="U ovom se članku opisuje način korištenja HTTP poruke da biste autorizirali pristup web-aplikacije i web-API-ji na klijentu pomoću servisa Azure Active Directory i OAuth 2.0."
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


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Autorizirajte pristup web-aplikacije pomoću OAuth 2.0 i Azure Active Directory

Azure Active Directory (Azure AD) koristi OAuth 2.0 da biste omogućili da biste autorizirali pristup web-aplikacije i web-API-ji u klijentu za Azure AD. Ovaj vodič jezik neovisno i opisuje način slanja i primanja poruka HTTP bez korištenja bilo koji od naše biblioteke Otvori izvor.

Tijek kod za autorizaciju OAuth 2.0 opisan u [odjeljku 4.1 specifikacija OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.1) . Koristi se za izvođenje provjere autentičnosti i autorizacije u većini vrsta aplikacije, uključujući web-aplikacije i nativno instalirane aplikacije.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>Tijek autorizacije OAuth 2.0

Visoke razine tijek cijelu provjeru autentičnosti za aplikaciju izgleda malo ovako:

![Tijek OAuth kod provjere autentičnosti](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Zahtjev za autorizaciju kod

Kod tijek autorizacije počinje koja usmjeruje korisniku klijenta s `/authorize` krajnjoj točki. U ovaj zahtjev klijent označava dozvole potrebne za dohvaćanje od korisnika. Krajnje točke OAuth 2.0 možete pristupiti iz vaše aplikacije stranice Azure klasični portalu, gumb za **Prikaz krajnje točke** u donjem ladica.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| klijent | obavezno | Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su identifikatori klijenta, npr. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` ili `contoso.onmicrosoft.com` ili `common` za klijent neovisno tokena |
| client_id | obavezno | Id aplikacije dodijeljene aplikacije kada ste se registrirali s Azure AD. Ne možete pronaći na portalu za upravljanje Azure. Kliknite na **Servisu Active Directory**, kliknite imenik, kliknite na aplikaciju pa na **Konfiguracija** |
| response_type | obavezno | Mora sadržavati `code` za tijek kod za autorizaciju. |
| redirect_uri | preporučeno | Redirect_uri aplikacije, gdje mogu poslati i primio aplikacije odgovora za provjeru autentičnosti.  Ona mora u potpunosti odgovarati nešto redirect_uris registrirana na portalu osim mora biti kodirati url-om.  Za izvorni i mobilne aplikacije koristite zadanu vrijednost od `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | preporučeno | Određuje način na koji želite koristiti za slanje rezultata tokena natrag u aplikaciju.  Može biti `query` ili `form_post`.  |
| Stanje | preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena. Slučajno generiranim jedinstvenu vrijednost obično koristi za [Sprječavanje napadi krivotvorina zahtjev za web-mjesta](http://tools.ietf.org/html/rfc6749#section-10.12).  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |
| resurs | Neobavezno | ID URI aplikacija API-JA (zaštićenim resurs) web-mjesta. Da biste pronašli ID URI aplikacija API-JA, web-mjesta na portalu za upravljanje Azure, kliknite **Servisa Active Directory**, kliknite imenik, kliknite aplikacije, a zatim kliknite **Konfiguriraj**. |
| upit | Neobavezno |  Određivanje vrste interakcije s korisnikom koji je potreban.<p> Valjane vrijednosti su: <p> *Prijava*: korisnik mora se zatražiti da ponovno provjeru autentičnosti. <p> *pristanak*: dozvole korisnika odobren, ali potrebno ažurirati. Korisnik mora se zatražiti da pristanak. <p> *admin_consent*: administrator tražiti pristanak ime svi korisnici u tvrtki ili ustanovi |
| login_hint | Neobavezno | Može se koristiti unaprijed ispunjavanja polja adresa korisničko ime/e-pošte za prijavu na stranici za korisnika ako znate svoje korisničko ime na vrijeme.  Ovaj parametar često aplikacija će se koristiti tijekom ponovnu provjeru autentičnosti, već imate dobivenih korisničko ime iz prethodne prijavu pomoću na `preferred_username` zahtjeva. |
| domain_hint | Neobavezno | Nudi podsjetnik o klijentu ili domenu koju korisnik mora koristiti za prijavu. Vrijednost na domain_hint je registriranih domena za klijenta. Ako je klijent pridružene da biste lokalnog imenika, AAD se preusmjerava na vanjskom poslužitelju navedeni klijenta. |

> [AZURE.NOTE] Ako je korisnik dio tvrtki ili ustanovi, administrator tvrtke ili ustanove mogu pristanak odbijanje u ime korisnika ili dopušta korisniku pristanak. Korisniku se prikazuje mogućnost pristanak samo kada je administrator ustanova to dopušta.

Sada korisnika će se zatražiti unos vjerodajnica i pristanak dozvola označen u `scope` parametarski upit. Kada korisnik potvrđuje i daje pristanak, Azure AD šalje odgovor na aplikaciju na na `redirect_uri` adresu na vaš zahtjev.

### <a name="successful-response"></a>Uspješan odgovor

Uspješan odgovor može izgledati ovako:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametar | Opis |
| -----------------------| --------------- |
| admin_consent | Vrijednost je True ako je administrator pristanak na upit pristanak zahtjev.|
| kod | Autorizacija kod koji ste tražili aplikacije. Aplikaciju možete koristiti kod autorizacije da biste zatražili pristupni token resursa cilj. |
| session_state | Jedinstvena vrijednost koja služi za identifikaciju trenutna sesija korisnika. Ta vrijednost je GUID, ali smatrati neprozirne vrijednost koja se prosljeđuje bez analize. |
| Stanje | Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Provjerite jesu li vrijednosti stanja u zahtjeva i odgovora identične prije korištenja odgovor aplikacije dobro je. Pomaže da biste otkrili [napada krivotvorina zahtjev na više web-mjesta (CSRF)](https://tools.ietf.org/html/rfc6749#section-10.12) na temelju klijent.  

### <a name="error-response"></a>Odgovor pogreška

Pogreška odgovora može biti poslan i da biste u `redirect_uri` tako da ih aplikacija može pravilno rukovati.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametar | Opis |
|-----------|-------------|
| Pogreška | Vrijednosti pogreške kod definirano u odjeljku 5,2 od [OAuth 2.0 autorizacije Framework](http://tools.ietf.org/html/rfc6749). Na sljedeću tablicu opisuju šifre pogrešaka koje vraća Azure AD. |
| error_description | Više detaljan opis pogreške. Ova poruka je namijenjen biti prikladnu za krajnjeg korisnika. |
| Stanje | Stanje vrijednost je slučajno generiranim koje nisu ponovno iskoristiti vrijednost koja se šalje u zahtjevu za i vratiti u odgovoru da biste spriječili napadi krivotvorina (CSRF) zahtjev za web-mjesta. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Pomoću koda za autorizaciju da biste zatražili token za pristup

Sad kad ste nabavili je kod autorizacije i dodijeljena dozvola korisnika, mogu iskoristiti šifru token za pristup resursu željeni slanjem zahtjeva za objavu da biste na `/token` krajnja točka:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| klijent | obavezno |  Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su identifikatori klijenta, npr. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` ili `contoso.onmicrosoft.com` ili `common` za klijent neovisno tokena |
| client_id | obavezno | Id aplikacije dodijeljene aplikacije kada ste se registrirali s Azure AD. To možete pronaći na portalu klasični Azure. Kliknite na **Servisu Active Directory**, kliknite imenik, kliknite na aplikaciju pa na **Konfiguracija** |
| grant_type | obavezno | Mora biti `authorization_code` za tijek kod za autorizaciju. |
| kod | obavezno | Na `authorization_code` koji koju ste nabavili u prethodnom odjeljku   |
| redirect_uri | obavezno | Isti `redirect_uri` koji je korišten za dohvaćanje vrijednosti u `authorization_code`. |
| client_secret | potrebne za web-aplikacije | Tajna aplikaciju koju ste stvorili na portalu za registraciju aplikacije za aplikacije.  To se ne smiju koristiti u aplikaciji za izvorni jer client_secrets nije pouzdano pohraniti na uređajima.  Za web-aplikacije i na webu API-ji, koji mogu sadržavati potreban je na `client_secret` sigurno na strani poslužitelja. |
| resurs | obavezno ako je naveden u autorizacije kod zahtjeva, još nije obavezno | ID URI aplikacija API-JA (zaštićenim resurs) web-mjesta.
Da biste pronašli ID URI aplikacija na portalu za upravljanje Azure, kliknite **Servisa Active Directory**, kliknite imenika, pritisnite aplikaciju i kliknite **Konfiguriraj**.

### <a name="successful-response"></a>Uspješan odgovor

Azure AD vraća pristupni token nakon uspješne odgovor. Da biste minimizirali mreže pozive iz klijentske aplikacije i njihove pridružene Latencija klijentska aplikacija treba pristupna tokena predmemoriju za tokena vijek koji je naveden u odgovoru OAuth 2.0. Da biste utvrdili tokena vijek, koristite na `expires_in` ili `expires_on` vrijednosti parametra.

Vraća API resursa web-mjesto programa `invalid_token` kod pogreške, to može upućivati da je resurs utvrdio token je istekao. Ako sat vremena klijenta i resursa su različite (adresna "jedan skew"), razmislite o resursa token istekla prije token isključen iz predmemorije klijenta. U tom slučaju poništite tokena iz predmemorije, čak i ako je i dalje unutar njegov izračunati života.

Uspješan odgovor može izgledati ovako:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token tražene programa access. Aplikaciju možete koristiti ovaj token za provjeru autentičnosti zaštićenim resursa, kao što je web-mjesto API-JA. |
| token_type | Označava vrijednost tokena vrste. Samo vrsta s podrškom za Azure AD je nošenja. Dodatne informacije o nošenja tokeni potražite u članku [OAuth2.0 autorizacije Framework: korištenje tokena nošenja (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Koliko dugo token za pristup vrijedi (u sekundama). |
| expires_on | Vrijeme isteka token za pristup. Datum predstavljen kao broj sekundi iz 1970-01-01T0:0:0Z UTC-a dok vrijeme isteka. Ta vrijednost koristi se za određivanje vijek predmemorirani tokena. |
| resurs | ID URI aplikacija API-JA (zaštićenim resurs) web-mjesta.|
| opseg | Oponašanje pravo na klijentskoj aplikaciji. Zadane dozvole `user_impersonation`. Vlasnik zaštićenim resursa možete registrirati dodatne vrijednosti u Azure AD.|
| refresh_token |  OAuth 2.0 token osvježavanja. Aplikaciju možete koristiti ovaj token potrebne dodatne pristupna tokena nakon isteka trenutne token za pristup.  Osvježavanje tokeni su long-lived pa se poslužite da biste zadržali pristup resursima za prošireni vremenskog razdoblja. |
| id_token | Za nepotpisane JSON Web tokena (JWT). Base64Url mogu aplikacije dekodiranje segmenata ovaj token zahtijevanje informacija o korisniku prijavljenom. Aplikaciju možete predmemoriju vrijednosti te ih prikazati, ali ga ne oslanjate na njih u autorizacije i sigurnosna ograničenja. |

### <a name="jwt-token-claims"></a>JWT tokena zahtjevima
Token JWT vrijednost u `id_token` parametar je moguće dekodirati u zahtjevima za sljedeće:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Na `id_token` parametar obuhvaća sljedeće vrste zahtjeva. Dodatne informacije o JSON web tokeni potražite [JWT IETF skice specifikacija](http://go.microsoft.com/fwlink/?LinkId=392344). Dodatne informacije o tokena vrste i zahtjevima pročitajte [podržane tokena i vrste zahtjeva](active-directory-token-and-claims.md)

| Vrsta zahtjeva | Opis |
|------------|-------------|
| aud | Ciljne skupine tokena. Kada se token izda s klijentskom aplikacijom, gledatelji je na `client_id` klijenta.
| Exp | Vrijeme isteka. Vrijeme isteka token. Simbol moraju biti valjane, trenutnog datuma/vremena mora biti manja od ili jednako na `exp` vrijednost. Vrijeme predstavljen kao broj sekundi iz siječanj 1, 1970 (1970-01-01T0:0:0Z) UTC do vremena izdavanja token. |
| family_name | Korisničke posljednje ime ili prezime. Aplikaciju možete prikazati tu vrijednost. |
| given_name | Korisnikovo ime. Aplikaciju možete prikazati tu vrijednost. |
| iat | Izdana trenutku. Vrijeme kad je na JWT izdan. Vrijeme predstavljen kao broj sekundi iz siječanj 1, 1970 (1970-01-01T0:0:0Z) UTC do vremena izdavanja token. |
| ISS | Služi za identifikaciju tokena izdavač |
| nbf | Ne prije vrijeme. Vrijeme kad postane stvarni token. Simbol moraju biti valjane, trenutnog datuma/vremena mora biti veći od ili jednako vrijednosti Nbf. Vrijeme predstavljen kao broj sekundi iz siječanj 1, 1970 (1970-01-01T0:0:0Z) UTC do vremena izdavanja token. |
| OID | Identifikator objekta (ID) korisničkom objektu u Azure AD. |
| Sub | Identifikator tokena predmet. Ovo je stalni i immutable identifikator za korisnika koji opisuje token. Koristite tu vrijednost u predmemoriju logike. |
| tid | Smjernice za identifikator (ID) koji je izdao token klijent Azure AD. |
| unique_name | Jedinstveni identifikator za koje se mogu prikazati korisniku. To je obično na korisnikovo Glavno ime (UPN). |
| UPN-a | Korisnikovo Glavno ime korisnika. |
| ver | Verzija. Verzija JWT token, obično 1.0. |

### <a name="error-response"></a>Odgovor pogreška

Krajnja točka pogreške tokena izdavanja su HTTP kodovi pogreške, jer klijent poziva krajnju točku tokena izdavanja izravno. Osim HTTP kod stanja tokena izdavanja krajnje Azure AD vraća JSON dokumenta s objektima koji opisuje pogrešku.

Pogreška odgovor uzorka može izgledati ovako:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer identificiranje uzrok pogreške provjere autentičnosti.  |
| error_codes | Popis kodova STS konkretne pogreške koji mogu olakšati Dijagnostika. |
| Vremenska oznaka | Vrijeme u kojem se pogreške. |
| trace_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika.  |
| correlation_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika preko komponente.|

#### <a name="http-status-codes"></a>HTTP kodovima stanja

U sljedećoj su tablici navedeni HTTP kodovima stanja vraća krajnje tokena izdavanja. U nekim slučajevima, kod pogreške je dovoljno opisuju odgovor, ali u slučaju pogreške, morat ćete raščlaniti prateću JSON dokument i pregledajte kôd pogreške.

| HTTP kod | Opis |
|-----------|-------------|
| 400       | Zadani HTTP kod. U većini slučajeva koristi te je obično zbog oblikovan zahtjev. Popravite i pošaljite zahtjev. |
| 401       | Provjera autentičnosti nije uspjela. Na primjer, zahtjev nedostaje parametar client_secret.|
| 403       | Autorizacija nije uspjelo. Na primjer, korisnik imati dozvolu za pristup resursu. |
| 500       | Na servis pojavila se interna pogreška. Ponovno zahtjev. |

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

## <a name="use-the-access-token-to-access-the-resource"></a>Koristite token za pristup za pristup resursu

Sad kad ste nabavili uspješno je `access_token`, token u zahtjevima za Web API-ji možete koristiti tako da uvrstite u na `Authorization` zaglavlje. Specifikacija [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) objašnjava kako pomoću nošenja tokeni u HTTP zahtjeva pristupa zaštićenim resursi.

### <a name="sample-request"></a>Zahtjev za uzorak

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Odgovor pogreška

Osigurano resursa koji implementira RFC 6750 problem HTTP kodovima stanja. Ako zahtjev obuhvaćaju vjerodajnice za provjeru autentičnosti ili nedostaje token, sadrži odgovor na `WWW-Authenticate` zaglavlje. Ako zahtjev ne uspije, resursa poslužitelja odgovara na HTTP kod stanja i pripadnog koda pogreške.

Slijedi primjer neuspješnih odgovor kada zahtjev klijenta uključuje nošenja tokena:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Parametri pogreške

| Parametar | Opis |
|-----------|-------------|
| authorization_uri | URI (fizičke krajnje točke) provjeru autentičnosti poslužitelja. Vrijednost se koristi kao ključ pretraživanja da biste dobili dodatne informacije o poslužitelju od krajnje točke za predočavanje elektroničkih dokumenata. <p><p> Klijent morate provjeriti je li poslužitelj za autorizaciju pouzdan. Kada resurs zaštićen Azure AD, dovoljno je da biste potvrdili da se URL počinje s https://login.windows.net ili neki drugi naziv glavnog računala s podrškom za Azure AD. Resurs specifične za klijenta mora uvijek vratiti specifične za klijent autorizacije URI. |
| Pogreška | Vrijednosti pogreške kod definirano u odjeljku 5,2 od [OAuth 2.0 autorizacije Framework](http://tools.ietf.org/html/rfc6749).|
| error_description | Više detaljan opis pogreške. Ova poruka je namijenjen biti prikladnu za krajnjeg korisnika.|
| resource_id | Vraća Jedinstveni identifikator resursa. Klijentska aplikacija možete koristiti ovaj identifikator kao vrijednost na `resource` parametar kada se zatraži token za resurs. <p><p> Vrlo je važno za klijentsku aplikaciju da biste provjerili tu vrijednost, u suprotnom zlonamjerni servis moći induce u slučaju napada **povećanje ovlasti** <p><p> Preporučena strategija sprječavaju u slučaju napada je da biste potvrdili da u `resource_id` odgovara osnovni API URL web-mjesta pristupa. Na primjer, ako je pristupa https://service.contoso.com/data, u `resource_id` može biti htttps://service.contoso.com/. Morate odbaciti klijentsku aplikaciju s `resource_id` koje ne počinju osnovni URL osim ako postoji pouzdan drugi način da biste provjerili id. |

#### <a name="bearer-scheme-error-codes"></a>Kodovi pogrešaka shemu nošenja

Specifikacija RFC 6750 definira sljedeće pogreške za resurse koji koriste pomoću provjere autentičnosti "www" Zaglavlje i nošenja shemu u odgovoru.

| Šifra stanja HTTP | Kod pogreške | Opis | Akcija klijenta |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | Zahtjev je ispravnog. Ako, na primjer, je možda nedostaje parametar ili dvaput koristeći istu parametar. | Ispravljanje pogreške i ponovno pokušajte zahtjev. Tu vrstu pogreške bi se trebale provoditi samo tijekom razvoja i se provjeravati u početne testiranja. |
| 401 | invalid_token   | Pristupni token je nedostaju, koji nisu valjani ili je povučen. Vrijednost parametra error_description pruža dodatne pojedinosti. |  Zatražiti novi token provjeru autentičnosti poslužitelja. Ako ne uspije novi token došlo je do neočekivane pogreške. Poslati poruku o pogrešci korisnika i pokušajte ponovno nakon slučajni kašnjenja. |
| 403 | insufficient_scope | Pristupni token sadrže oponašanja dozvole potrebne za pristup resursu. | Novi zahtjev za autorizaciju poslati krajnja točka za autorizaciju. Ako odgovor sadrži parametar opseg, upotrijebite vrijednost opseg zahtjeva za resurs. |
| 403 | insufficient_access | Predmet token imati dozvole koje su potrebne za pristup resursu. | Pitaj korisnika pomoću nekog drugog računa ili zatražite dozvole za određeni resurs. |

## <a name="refreshing-the-access-tokens"></a>Osvježavanje pristupna tokena

Pristupna tokena su short-lived i moguće osvježiti nakon isteknu pristupu resursi. Možete osvježiti u `access_token` slanjem drugi `POST` zahtjev za na `/token` krajnje točke, ali pruža vrijeme na `refresh_token` umjesto na `code`.

Osvježavanje tokeni su navedeni vijekom trajanja. Vijekom trajanja od osvježavanja tokeni su obično relativno dugačkih. No u nekim slučajevima osvježavanja tokeni istječe, opoziva ili nemaju ovlasti za željenu akciju. Aplikacija treba očekivati i obradu pogrešaka vratio krajnju točku tokena izdavanja pravilno.

Kada dobijete odgovor uz osvježavanja tokena poruku o pogrešci, odbaciti trenutnu token osvježavanja i zahtjev za novu autorizacije kod ili pristup token. Posebno kada pomoću osvježavanja token u tijeku autorizacije kod Grant Ako dobivate odgovor s na `interaction_required` ili `invalid_grant` šifre pogrešaka Odbaci token osvježavanja i zahtjev za novu autorizacije kod.

Uzorak zahtjev za krajnje točke **specifične za klijenta** (vam može poslužiti **uobičajenih** krajnje točke) da biste dobili novi pristup tokena pomoću osvježavanja token izgleda ovako:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parametar | Opis |
|-----------|-------------|
| access_token | Novi token pristupa koji ste tražili.|
| expires_in   | Preostali vijek tokena u sekundama. Uobičajeni vrijednost je 3600 (jedan sat). |
| expires_on   | Datum i vrijeme token istekne. Datum predstavljen kao broj sekundi iz 1970-01-01T0:0:0Z UTC-a dok vrijeme isteka. |
| refresh_token | Novi OAuth 2.0 refresh_token koje je moguće koristiti da biste zatražili novi pristupna tokena kada istekne onom u odgovor. |
| resurs     | Označava zaštićenim resurs koji se mogu token za pristup korištenih u programu access. |
| opseg        | Oponašanje pravo na aplikaciju za nativni klijent. Dozvola za zadane je **user_impersonation**. Vlasnik resursa ciljnu Azure AD možete registrirati zamjenske vrijednosti. |
| token_type   | Vrsta tokena. Samo podržanih vrijednost je **nošenja**. |

### <a name="successful-response"></a>Uspješan odgovor

Uspješan tokena odgovor će izgledati:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Odgovor pogreška

Pogreška odgovor uzorka može izgledati ovako:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer identificiranje uzrok pogreške provjere autentičnosti.  |
| error_codes | Popis kodova STS konkretne pogreške koji mogu olakšati Dijagnostika. |
| Vremenska oznaka | Vrijeme u kojem se pogreške. |
| trace_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika.  |
| correlation_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika preko komponente.|

Opis šifre pogrešaka i akciju preporučeni klijent potražite [pogreške šifre pogrešaka tokena krajnjoj točki](#error-codes-for-token-endpoint-errors).
