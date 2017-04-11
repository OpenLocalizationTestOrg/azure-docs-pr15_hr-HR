<properties
    pageTitle="Azure AD 2.0 implicitno tijek | Microsoft Azure"
    description="Sastavni web-aplikacije pomoću Azure AD 2.0 implementaciju implicitno tijek za jednu stranicu aplikacije."
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

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>2.0 protokoli - SPAs pomoću implicitno tijek
S krajnju točku 2.0 mogu se prijaviti korisnika u jednu stranicu aplikacije s osobne i poslovne/školske računi od Microsofta.  Jedna stranica i drugih aplikacija za JavaScript koji se pokreću prvenstveno u pregledniku nominalne nekoliko zanimljive izazove kada je riječ o autentičnosti:

- Sigurnost karakteristike ove aplikacije razlikuju znatno tradicionalni utemeljenih na poslužitelju web-aplikacije.
- Mnoge poslužitelji autorizacije i davatelji identiteta ne podržava CORS zahtjeva.
- Cijela stranica preglednika preusmjerava izvan aplikaciju postaju osobito invasive korisničkog sučelja.

Za te aplikacije (razmislite: AngularJS, Ember.js, React.js, itd) Azure AD podržava tijek OAuth 2.0 implicitno Dodjela.  Implicitno tijek je opisano u [OAuth 2.0 specifikacija](http://tools.ietf.org/html/rfc6749#section-4.2).  Njegove primarne prednost je dopušta li aplikaciju da biste tokeni s Azure AD bez provedbe pozadinskog poslužitelja exchange vjerodajnica.  Time se omogućuje aplikaciju Prijava korisnika, održavati sesije i dobiti tokeni drugim web-mjestu API-ji sve unutar klijent JavaScript kod.  Postoji nekoliko važnih sigurnosna pitanja vezana uz uzmete u obzir prilikom korištenja implicitno tijek – Konkretno oko [klijenta](http://tools.ietf.org/html/rfc6749#section-10.3) i [Oponašanje korisnika](http://tools.ietf.org/html/rfc6749#section-10.3).

Ako želite koristiti implicitno tijek i Azure AD da biste dodali provjere autentičnosti za aplikaciju programa JavaScript, preporučujemo da koristite naš Otvori izvor JavaScript biblioteke, [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Dostupno nekoliko AngularJS vodiče [ovdje](active-directory-appmodel-v2-overview.md#getting-started) da biste olakšati početak rada.  

Međutim, ako radije ne biste korištenje biblioteka u jednu stranicu aplikacije i slanje poruka protokol, slijedite općenite korake u nastavku.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Protokol dijagrama
Cijeli implicitno prijava tijek izgleda otprilike ovako: svakog koraka su detaljno opisane u nastavku.

![OpenId povezivanje Funkcijske trake](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Pošaljite zahtjev za prijavu

Prethodno Prijava korisnika u aplikacije, možete poslati zahtjev za [Povezivanje OpenID](active-directory-v2-protocols-oidc.md) autorizacije i dobiti programa `id_token` iz krajnju točku 2.0:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Kliknite vezu da biste taj zahtjev za izvršavanje! Nakon prijave, vaš preglednik mora biti preusmjereni `https://localhost/myapp/` s na `id_token` u adresnu traku.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| klijent | obavezno | Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su `common`, `organizations`, `consumers`, i smjernice za identifikatore.  Više pojedinosti potražite u članku [Osnove protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| response_type | obavezno | Mora sadržavati `id_token` za povezivanje OpenID prijavu.  Uključivati i u response_type `token`. Korištenje `token` ovdje da bi aplikacije primanje pristupnog tokena odmah krajnju točku ovlasti bez potrebe za upućivanje zahtjeva za drugi krajnjoj ovlasti.  Ako koristite na `token` response_type, u `scope` parametar mora sadržavati doseg koji označava koje resursa za izdavanje tokena za. |
| redirect_uri | preporučeno | Redirect_uri aplikacije, gdje mogu poslati i primio aplikacije odgovora za provjeru autentičnosti.  Ona mora u potpunosti odgovarati nešto redirect_uris registrirana na portalu osim mora biti kodirati url-om. |
| opseg | obavezno | Popis prostora odvojene opsega.  Za povezivanje OpenID, morate uključiti opsega `openid`, koja se prevodi dozvolu "Možemo prijaviti" u pristanak korisničkog Sučelja.  Po želji možda želite uvrstiti u `email` ili `profile` [opsega](active-directory-v2-scopes.md) pristupe dodatnih korisničkih podataka.  Uključivati i drugih dosega u zahtjev za traženje pristanak na različite resurse.  |
| response_mode | preporučeno | Određuje način na koji želite koristiti za slanje rezultata tokena natrag u aplikaciju.  Mora biti `fragment` za implicitna tijek.  |
| Stanje | preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena.  Možda ćete niz sav sadržaj koji želite.  Slučajno generiranim jedinstvenu vrijednost obično koristi za [Sprječavanje napadi krivotvorina zahtjev za web-mjesta](http://tools.ietf.org/html/rfc6749#section-10.12).  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |
| Jednokratna šifra | obavezno | Vrijednost koja se uključiti u zahtjevu za generira aplikacije koje će biti obuhvaćene dobivene id_token kao zahtjev.  Aplikaciju možete provjeriti pa tu vrijednost u smanjenju tokena Ponovi napada.  Vrijednost je obično kombinaciji, jedinstveni niz koji se mogu koristiti za prepoznavanje polazište zahtjev.  |
| upit | Neobavezno | Navodi vrstu interakcije s korisnikom koji je potreban.  Samo valjane trenutno su vrijednosti 'prijava', 'ništa,' i 'pristanak.  `prompt=login`korisniku unesite svoje vjerodajnice na taj zahtjev, negating jedinstvene prijave na će obavezno.  `prompt=none`suprotan - će osigurati da korisnik ne prikazuju s bilo kojeg interaktivne neće retkom.  Ako je zahtjev ne može dovršiti tihu putem jedinstvene prijave na krajnjoj točki 2.0 će vratiti pogrešku.  `prompt=consent`dijaloški okvir OAuth pristanak će se pokrenuti nakon korisnikove prijave u odjeljku pitanje korisniku dodijeliti dozvole za aplikaciju. |
| login_hint | Neobavezno | Može se koristiti unaprijed ispunjavanja polja adresa korisničko ime/e-pošte za prijavu na stranici za korisnika ako znate svoje korisničko ime na vrijeme.  Ovaj parametar često aplikacija će se koristiti tijekom ponovnu provjeru autentičnosti, već imate dobivenih korisničko ime iz prethodne prijavu pomoću na `preferred_username` zahtjeva. |
| domain_hint | Neobavezno | Može biti jedna od `consumers` ili `organizations`.  Ako je uključena, će preskočiti e-pošte istrage taj korisnik prolazi kroz na stranici 2.0 prijava, oni mogu dovesti do malo više optimiziranog korisničkog sučelja.  Često aplikacija će koristiti taj parametar tijekom ponovnu provjeru autentičnosti po izdvajanje u `tid` zahtjeva iz na id_token.  Ako u `tid` zahtjeva je vrijednost `9188040d-6c67-4c5b-b112-36a304b66dad`, trebali biste koristiti `domain_hint=consumers`.  U suprotnom koristite `domain_hint=organizations`. |

U ovom trenutku korisnika će se tražiti da unositi vjerodajnice i dovršite provjeru autentičnosti.  Krajnja točka 2.0 i provjerite da korisnik ima pristanak dozvola označen u `scope` parametarski upit.  Ako korisnik ima ne pristanak na bilo koju od tih dozvola, ona će uputite korisnika da pristanak potrebne dozvole.  Detalji o [dozvolama, pristanak, i više klijentske aplikacije su navedene](active-directory-v2-scopes.md).

Kada korisnik potvrđuje i daje pristanak, vratit će krajnju točku 2.0 odgovor na aplikaciju na na navedeni `redirect_uri`, na način naveden u na `response_mode` parametar.

#### <a name="successful-response"></a>Uspješan odgovor

Uspješan odgovor pomoću `response_mode=fragment` i `response_type=id_token+token` sliči sljedeće s prijelomima za čitljivost:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| access_token | Uključeni if `response_type` sadrži `token`. Pristup token koji aplikaciju zatražili, u tom slučaju Microsoft Graph.  Token za pristup treba ne može dekodirati ili inače pregledavaju, mogu se tretirati kao neprozirne niz. |
| token_type | Uključeni if `response_type` sadrži `token`.  Uvijek će biti `Bearer`. |
| expires_in | Uključeni if `response_type` sadrži `token`.  Označava broj sekundi token nije valjan, za predmemoriranje svrhe. |
| opseg | Uključeni if `response_type` sadrži `token`.  Pokazuje dosezi za koju se access_token će moraju biti valjane. |
| id_token | Id_token koji ste tražili aplikaciju. Da biste provjerili identitet korisnika i počnite sesije s korisnikom možete koristiti u id_token.  Dodatne informacije o id_tokens i njihov sadržaj je sve obuhvaćeno [2.0 krajnjoj točki tokena referencu](active-directory-v2-tokens.md).  |
| Stanje | Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |


#### <a name="error-response"></a>Odgovor pogreška
Pogreška odgovora može biti poslan i da biste u `redirect_uri` tako da ih aplikaciju možete pravilno rukovati:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti.  |

## <a name="validate-the-idtoken"></a>Provjerite valjanost u id_token
Samo primanje programa id_token nije dovoljno provjere autentičnosti korisnika; morate provjeriti valjanost potpisa u id_token i provjerite je li zahtjevima u token za preduvjeti za aplikaciju programa.  Krajnja točka 2.0 koristi [Tokeni JSON Web (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) i javni ključ šifriranja za potpisivanje tokena i provjerite jesu li valjan.

Možete odabrati da biste provjerili valjanost u `id_token` u klijentu kodu, ali uobičajeno je slanje na `id_token` pozadinski poslužitelj i izvršiti provjeru postoji.  Kada ste potvrđuju potpis na id_token, dostupne su nekoliko zahtjevima vas će se tražiti da biste potvrdili.  Potražite u članku [2.0 tokena referenca](active-directory-v2-tokens.md) dodatne informacije, uključujući [Tokeni provjere valjanosti](active-directory-v2-tokens.md#validating-tokens) i [Važne informacije o potpisivanja ključ dinamične](active-directory-v2-tokens.md#validating-tokens).  Preporučujemo korištenje biblioteke za analize i provjera valjanosti tokeni – je dostupno barem jedno za većinu jezika i platformama.
<!--TODO: Improve the information on this-->

Možda želite provjeriti dodatne zahtjevima ovisno o scenariju.  Neke uobičajene provjere obuhvaćaju sljedeće:

- Osiguravanje korisnika/tvrtka ili ustanova ima prijavili za aplikaciju.
- Jamči da korisnik ima odgovarajuće autorizacija/ovlasti
- Osiguravanje određene jačinu provjere autentičnosti došlo je do, kao što su višestruke provjere autentičnosti.

Dodatne informacije o zahtjevima u programa id_token potražite u članku [2.0 krajnjoj točki tokena referencu](active-directory-v2-tokens.md).

Kada ste potpuno nastavlja na id_token, možete započeti sesije s korisničkim i koristiti na zahtjevima u u id_token da biste dobili informacije o korisniku u aplikaciju.  Ove informacije mogu koristiti za prikaz zapisa, autorizacijama, itd.

## <a name="get-access-tokens"></a>Dobiti pristup tokena

Sad kad ste prijavljeni korisnik u aplikaciju za jednu stranicu, možete dobiti pristup tokeni za pozivanja web-mjesto API-ji osigurava Azure AD, kao što je [Microsoft Graph](https://graph.microsoft.io).  Čak i ako ste već primili tokena uz pomoć u `token` response_type, koristite ovu metodu dobiti tokena na dodatne resurse bez potrebe za preusmjeravanje korisnika da biste ponovno se prijavite.

U običnom tijek povezivanje OAuth OpenID će to možete učiniti tako da biste na 2.0 upućivanje zahtjeva za `/token` krajnjoj točki.  Međutim, krajnju točku 2.0 ne podržava CORS zahtjeve, pa je upućivanje poziva AJAX-a da biste dobili i osvježili tokena iz pitanje.  Umjesto toga možete koristiti implicitno tijek u skrivene iframe da biste dobili novi tokeni za ostale API-ji web-mjesto: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Pokušajte kopiranja i lijepljenja u nastavku zahtjev u karticu preglednika! (Ne zaboravite da biste zamijenili na `domain_hint` i `login_hint` vrijednosti pomoću odgovarajuće vrijednosti za korisnički)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| klijent | obavezno | Na `{tenant}` vrijednost u parametru path zahtjeva može se koristiti za odabir korisnika koji mogu se prijaviti u aplikaciju.  Dopuštene vrijednosti su `common`, `organizations`, `consumers`, i smjernice za identifikatore.  Više pojedinosti potražite u članku [Osnove protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| response_type | obavezno | Mora sadržavati `id_token` za povezivanje OpenID prijavu.  Uključivati i druge response_types, kao što su `code`. |
| redirect_uri | preporučeno | Redirect_uri aplikacije, gdje mogu poslati i primio aplikacije odgovora za provjeru autentičnosti.  Ona mora u potpunosti odgovarati nešto redirect_uris registrirana na portalu osim mora biti kodirati url-om. |
| opseg | obavezno | Popis prostora odvojene opsega.  Za dohvaćanje tokeni obuhvaćaju svih [opsega](active-directory-v2-scopes.md) obavezan za resursa koji vas zanimaju.  |
| response_mode | preporučeno | Određuje način na koji želite koristiti za slanje rezultata tokena natrag u aplikaciju.  Može biti jedna od `query`, `form_post`, ili `fragment`.  |
| Stanje | preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena.  Možda ćete niz sav sadržaj koji želite.  Slučajno generiranim jedinstvenu vrijednost obično koristi za sprječavanje napada krivotvorina zahtjev za web-mjesta.  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |
| Jednokratna šifra | obavezno | Vrijednost koja se uključiti u zahtjevu za generira aplikacije koje će biti obuhvaćene dobivene id_token kao zahtjev.  Aplikaciju možete provjeriti pa tu vrijednost u smanjenju tokena Ponovi napada.  Vrijednost je obično kombinaciji, jedinstveni niz koji se mogu koristiti za prepoznavanje polazište zahtjev.  |
| upit | obavezno | Za osvježavanje & početak tokeni u skrivene iframe, trebali biste koristiti `prompt=none` da bi se prekinuti vezu na stranici 2.0 prijava iframe i vraća odmah. |
| login_hint | obavezno | Za osvježavanje & početak tokeni u skrivene iframe, morate uključiti korisničko ime korisnika u ovom podsjetnik da biste razlikovali više sesija korisnik može imati u određenom trenutku u vremenu. Korisničko ime možete izdvojiti iz prethodne prijavu pomoću na `preferred_username` zahtjeva. |
| domain_hint | obavezno | Može biti jedna od `consumers` ili `organizations`.  Za osvježavanje & prilikom tokeni skrivene iframe, potrebno je uključiti u domain_hint u zahtjevu za.  Treba izdvojiti u `tid` zahtjeva iz id_token od na prethodni prijava da biste utvrdili koji vrijednosti koju želite koristiti.  Ako u `tid` zahtjeva je vrijednost `9188040d-6c67-4c5b-b112-36a304b66dad`, trebali biste koristiti `domain_hint=consumers`.  U suprotnom koristite `domain_hint=organizations`. |

Zahvale na `prompt=none` parametar zahtjev će ili uspjeti ili neće uspjeti odmah i vratili se na vaše aplikacije.  Uspješan odgovor poslat će se aplikacija pri na navedeni `redirect_uri`, na način naveden u na `response_mode` parametar.

#### <a name="successful-response"></a>Uspješan odgovor
Uspješan odgovor pomoću `response_mode=fragment` izgleda:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token koji ste tražili aplikaciju. |
| token_type | Uvijek će biti `Bearer`. |
| Stanje | Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |
| expires_in | Koliko dugo token za pristup vrijedi (u sekundama). |
| opseg | Opsezi token za pristup vrijedi za. |

#### <a name="error-response"></a>Odgovor pogreška
Pogreška odgovora može biti poslan i da biste u `redirect_uri` tako da ih aplikaciju možete pravilno rukovati.  U the slučaju od `prompt=none`, bit će pogrešku očekivani:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške provjere autentičnosti.  |

Ako se prikaže Ova pogreška u zahtjevu za iframe, korisnik mora interaktivno ponovno prijaviti za dohvaćanje novi token.  Možete učiniti taj slučaj u bilo kakve onako kako vam odgovara aplikacije.

## <a name="refreshing-tokens"></a>Osvježavanje tokena

Oba `id_token`s i `access_token`s istječe nakon tokeni kratko vrijeme, tako da se aplikacijom potrebno pripremiti za osvježavanje te povremeno.  Da biste osvježili bilo koje vrste tokena, na raspolaganju vam isti zahtjev za skrivene iframe from above pomoću na `prompt=none` parametar da biste odredili Azure AD ponašanje.  Ako želite dobiti novi `id_token`, obavezno koristite `response_type=id_token` i `scope=openid`, kao i u `nonce` parametar.


## <a name="send-a-sign-out-request"></a>Slanje Odjava zahtjev

U OpenIdConnect `end_session_endpoint` trenutno nije podržano u krajnju točku 2.0. To znači da aplikacije ne može poslati zahtjev za krajnju točku 2.0 da biste završili sesiju korisnika i očistite kolačiće odredio krajnju točku 2.0.
Da biste se prijavili korisnika izvan aplikacije možete jednostavno prekid vlastitu sesije s korisnikom i sesija ovim ostavite na 2.0 krajnje točke u-tact.  Na sljedećem se korisnik pokuša prijaviti, prikazat će se stranicu "odaberite račun" s njihove aktivno prijavljeni račune na popisu.
Na toj stranici korisnik može odabrati da biste se odjavili iz bilo kojeg računa, Završava sesiju s krajnju točku 2.0.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->