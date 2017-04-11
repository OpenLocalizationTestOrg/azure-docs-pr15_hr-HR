<properties
    pageTitle="Azure AD 2.0 dosega, dozvolama i pristanak | Microsoft Azure"
    description="Opis autorizacije u Azure AD 2.0 krajnju točku, uključujući dosega, dozvole i odobrenja."
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

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Opsezi, dozvole i pristanak na krajnjoj točki 2.0

Aplikacije koje se integrirati s Azure AD slijedite određeni autorizacije modelu koji omogućuje korisnicima da biste odredili kako aplikaciju možete pristupati svojim podacima.  Implementacija 2.0 ovaj model autorizacije ažurirao, promjenom način na koji morate aplikacije rade s Azure AD.  U ovoj se temi objašnjava osnovni koncepti ovaj model autorizacije, uključujući dosega, dozvole i odobrenja.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Opsega i dozvole

Azure AD implementira protokol za autorizaciju [OAuth 2.0](active-directory-v2-protocols.md) koji je način za dopuštanje 3 aplikacija sudionika da biste pristupili web hostira resursima ime korisnika.  Bilo koji hostira na web resurs koji se integrira s Azure AD će imati identifikator resursa ili **URI ID aplikacije**.  Na primjer resursi web hostira tvrtke Microsoft obuhvaćaju sljedeće:

- Office 365 Sjedinjeno komuniciranje pošte API-JA:`https://outlook.office.com`
- API Azure AD grafikona:`https://graph.windows.net`
- Microsoft Graph:`https://graph.microsoft.com`

Isto vrijedi i za 3 resurse sudionika koji je integriran s Azure AD.  Bilo koji od tih resursa, možete definirati skup dozvola koje je moguće koristiti da biste razdijelili integrira resursa u manjim blokova.  Na primjer, Microsoft Graph je definirao nekoliko dozvole:

- Čitanje korisničke kalendara
- Pisanje korisničke kalendar
- Slanje pošte kao korisnik
- [+ više](https://graph.microsoft.io)

Definiranjem tih dozvola resursa može imati preciznije kontrolirati njegove podatke i kako ga je izložen izvan svijeta.  3 proizvođača aplikacije možete zatražiti te dozvole iz krajnji korisnik - a krajnji korisnik mora odobriti dozvole prije aplikaciju možete se poslužiti u ime.  Po chunking resursa integrira u manjim skupova dozvola, 3 proizvođača aplikacije možete ugrađeni da biste zatražili samo određene dozvole koje su im potrebne za izvođenje njihove carina.  Omogućuje i krajnjim korisnicima omogućuje da znate točno onako kako aplikaciju će koristiti svoje podatke kako bi bili sigurni da se aplikacije ne ponaša s zloupotrebu.

U Azure AD i OAuth, te dozvole nazivaju **opsega**.  Može se prikazati ih se nazivaju **oAuth2Permissions**.  Opseg predstavlja se Azure AD kao vrijednost niza.  Nastavite s primjer Microsoft Graph, opseg vrijednost za svaku dozvolu je:

- Pročitajte korisničke kalendara:`Calendar.Read`
- Napišite korisničke kalendar:`Mail.ReadWrite`
- Pošalji poštu kao korisnik:`Mail.Send`

Aplikaciju možete zatražiti tih dozvola navođenjem dosezi u zahtjevima za krajnju točku 2.0 prema uputama u nastavku.

## <a name="openid-connect-scopes"></a>Povezivanje OpenId dosega

Povezivanje OpenID implementacije 2.0 sadrži nekoliko dobro definiranom dosega koji se odnose na određeni resurs - `openid`, `email`, `profile`, i `offline_access`.

#### <a name="openid"></a>OpenId

Ako aplikaciju izvodi prijavu pomoću [OpenID povezivanje](active-directory-v2-protocols.md#openid-connect-sign-in-flow), to morate zatražiti na `openid` opseg.  Na `openid` opseg prikazuju se na zaslonu pristanak račun za rad kao dozvole "Možemo prijaviti", a zatim na osobni Microsoftov pristanak zaslonu račun kao dozvole "Prikaz profila i povezivanje s aplikacijama i servisima pomoću Microsoftova računa".  Tu dozvolu omogućuje aplikacije da biste primali Jedinstveni identifikator za korisnika u obliku na `sub` zahtjeva.  Također, affords aplikacije programa access krajnjoj informacije korisnika.  Na `openid` opseg može se koristiti na krajnjoj točki tokena 2.0 dobiti id_tokens koji se može koristiti za zaštitu HTTP pozive između različitih komponenti aplikacije.

#### <a name="email"></a>E-pošte

Na `email` opseg mogu uključiti duž na `openid` opsega i sva ostala polja.  Ga affords pristup aplikacija za korisničke adrese e-pošte primarnog u obliku na `email` zahtjeva.  Na `email` zahtjeva samo obuhvaćene tokeni ako adrese e-pošte povezan s korisnički račun koji nije uvijek slučaj.  Ako koristite na `email` opseg, aplikacije potrebno pripremiti za rukovanje slučaja u kojima se `email` zahtjeva ne postoji u token.

#### <a name="profile"></a>Profil

Na `profile` opseg mogu uključiti duž na `openid` opsega i sva ostala polja.  Ga affords pristup aplikacije mnoštvo informacija o korisniku.  To obuhvaća, ali nije ograničena, korisničko ime, prezime, preferirani korisničko ime, ID objekta i tako dalje.  Popis svih zahtjevima profila dostupne u id_tokens za određene korisnike potražite [2.0 tokena referencu](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

U [ `offline_access` opseg](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) omogućuje aplikaciju programa access resursima ime korisnika za dulje vrijeme.  Na zaslonu pristanak račun rad ovaj doseg prikazat će se kao dozvole "Bilo kojem trenutku pristup vašim podacima".  Osobni Microsoftov račun pristanak na zaslonu u ga prikazat će se kao dozvole "Bilo kojem trenutku pristupiti svoje podatke za".  Kada se korisnik odobri na `offline_access` opseg, aplikacije omogućit će se tokeni osvježavanja primanje tokena krajnju točku 2.0.  Osvježavanje tokeni su long-lived i omogućivanje aplikacije dobiti novi pristupna tokena kao starije istječe.

Ako aplikaciju zahtjev za `offline_access` opseg, će primiti refresh_tokens.  To znači da kada iskoristili programa authorization_code u [OAuth 2.0 autorizacije kod tijek](active-directory-v2-protocols.md#oauth2-authorization-code-flow), samo dobit ćete ponovno programa access_token iz na `/token` krajnjoj točki.  Tu access_token ostat će valjani na kratko vrijeme (obično jedan sat), ali ti će isteći.  Natrag na koje točke u vremenu, pokrenite aplikaciju morate preusmjeravanje korisnika u na `/authorize` krajnja točka za dohvaćanje novi authorization_code.  Tijekom ovog preusmjeravanje korisnik može ili možda ne morate ponovno unositi vjerodajnice ili ponovno pristanak dozvola, ovisno o na vrstu aplikacije.

Dodatne informacije o dobiti i pomoću osvježavanje tokena, pogledajte [2.0 protokol referencu](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Zahtijevanje odobrenja pojedinačnog korisnika

U autorizacije zahtjev [OpenID povezati ili OAuth 2.0](active-directory-v2-protocols.md) aplikaciju možete zatražiti dozvole potrebne pomoću na `scope` parametarski upit.  Ako, na primjer, kada korisnik potpiše u aplikaciju, aplikaciju želite pošaljite zahtjev za ovako (s prijelome čitljivosti):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Na `scope` parametar je popis prostora odvojene dosega koji zahtijeva aplikaciju.  Svaki pojedinih označena je dodavanje opseg vrijednost u identifikator resursa (URI aplikacije ID-a).  Zahtjev za gornje označava aplikaciju potrebna je dozvola za korisnika kalendara za čitanje i slanje poruka kao korisnik.

Kada korisnik unese svoja uvjerenja, krajnju točku 2.0 češće provjerava ima li odgovarajući zapis o **pristanak korisnika**.  Ako korisnik ima ne pristanak na neku razinu dozvole za traženi u prošlosti, krajnju točku 2.0 će uputite korisnika da biste dodijelili potrebne dozvole.  

![Snimka zaslona pristanak za rad računa](../media/active-directory-v2-flows/work_account_consent.png)

Kada korisnik odobri dozvolu, pristanak ostat će zabilježen tako da se korisnik ne mora ponovno pristanak na sljedeći znak dodataka.

## <a name="requesting-consent-for-an-entire-tenant"></a>Traženje odobrenja za cijeli klijent

Često kada tvrtkom ili ustanovom kupi licence ili pretplatu na aplikaciju, što žele potpuno Dodjela resursa za svoje zaposlenike.  Kao dio ovog postupka, administrator tvrtke možete dodijeliti dozvole za tu aplikaciju djelovanje ime sve zaposlenika.  Po date pristanak za cijeli klijent, zaposlenika te organizacije će sučelje zaslonu pristanak za tu aplikaciju.

Da biste zatražili pristanak za sve korisnike na klijentu, pokrenite aplikaciju možete koristiti na **administratore pristanak krajnje točke**, opisanim.

## <a name="admin-restricted-scopes"></a>Administrator ograničene dosega

Za određene visoke ovlasti dozvole u zajednici Microsoft može biti označen kao **administrator ograničena**.  Primjeri takvih opsega obuhvaćaju sljedeće:

- Čitanje podataka na organizaion direktorija:`Directory.Read`
- Zapisivanje podataka u imeniku programa tvrtke ili ustanove:`Directory.ReadWrite`
- Čitanje sigurnosne grupe u imeniku programa tvrtke ili ustanove:`Groups.Read.All`

Dok korisnik korisničke davati aplikacije programa access da biste tih podataka, tvrtke ili ustanove korisnici su ovlašteni dodjeljivanju isti skup podataka osjetljive tvrtke.  Ako program zatraži pristup na neki od ovih dozvola iz tvrtke ili ustanove korisnika, korisnik će primiti poruku o pogrešci koja navodi da su neovlašteno pristanak dozvole za aplikaciju programa.

Ako aplikacije potreban je pristup te opsega ograničeno za administratore u tvrtkama ili ustanovama, trebali biste ih zatražiti izravno iz također koristi za **administratore pristanak krajnje točke**, opisanim administrator tvrtke.

Kada je administrator daje tih dozvola putem krajnju točku administratorske dozvole, pristanak ćete dobiti za sve korisnike u klijent, kao što je opisano iznad.

## <a name="using-the-admin-consent-endpoint"></a>Korištenje krajnjoj točki pristanak za administratore

Slijedeći ove korake aplikacija će moći prikupite dozvole za sve korisnike u zadani klijent, uključujući administrator ograničene opsega.  Da biste vidjeli uzorak koda koji implementira ispod desribed korake, pogledajte [administrator ograničena opsege ogledni](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Zahtjev za dozvole na portalu za registraciju aplikacije

- Ako to već niste učinili, dođite do aplikacija u [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili [stvorite aplikaciju](active-directory-v2-app-registration.md) .
- Pronađite odjeljak **Microsoft Graph dozvole** i dodajte dozvole koje je potrebno aplikacije.
- Provjerite da biste **spremili** Registracija aplikacije

#### <a name="recommended-sign-the-user-into-your-app"></a>Preporučuje: Prijava korisnika u aplikaciju

Obično kada stvaranje aplikacija koja koristi administrator pristanak krajnje točke, aplikaciju moraju imati stranice/prikaz koji omogućuje administrator da biste odobrili dozvole za u aplikaciju.  Ova stranica može biti dio aplikacije registracije protok postavkama aplikacije ili namjenski tijek "povezivanje".  U mnogim slučajevima, je li bolje aplikacije da biste prikazali taj "povezivanje" prikaz tek kada korisnik ima prijavljeni na poslu ili u školi Microsoftova računa.

Prijava korisnika u aplikaciju vam omogućuje da utvrdite organziation kojem administrator pripada prije da biste zatražili da biste odobrili potrebne dozvole.  Dok je ne isključivo potrebno, može pomoći pri stvaranju više intuitivno sučelje za korisnike tvrtke ili ustanove.  Da biste se prijavili korisnika u Slijedite naše [vodiči za protokol 2.0](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Zatražite dozvole iz imenika administrator

Kada budete spremni za dozvole iz tvrtke administrator, možete preusmjeriti korisnika 2.0 **administrator pristanak krajnjoj točki**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| klijent | obavezno | Klijent direktorija koji želite zatražite dozvolu.  Možete koristiti u obliku neslužbeni naziv ili guid. |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| redirect_uri | obavezno | Redirect_uri mjesto na koje želite odgovor slati aplikacije za rukovanje.  Ona mora u potpunosti odgovarati nešto redirect_uris registrirana na portalu. |
| Stanje | preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena.  Možda ćete niz sav sadržaj koji želite.  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |

U ovom trenutku Azure AD nametnuti da samo administrator klijenta mogu se prijaviti u da biste dovršili zahtjev.  Administrator će se zatražiti da biste odobrili sve dozvole koje su zatražili korisnici aplikacije na portalu za registraciju. 

##### <a name="successful-response"></a>Uspješan odgovor
Ako administrator odobrenje dozvole za aplikaciju, bit će uspješno odgovor:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametar | Opis |
| ----------------------- | ------------------------------- | --------------- |
| klijent | U imeniku koji se klijent ovlasti aplikacija se to od vas traži, u obliku guid. |
| Stanje | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena.  Možda ćete niz sav sadržaj koji želite.  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |
| admin_consent | Postavit će se na `True`. |



##### <a name="error-response"></a>Odgovor pogreška
Ako administrator odobriti dozvole za aplikaciju, bit će nije uspjelo odgovor:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametar | Opis |
| ----------------------- | ------------------------------- | --------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer odredite uzrok pogreške.  |

Kada primite uspješno odgovor na krajnjoj točki pristanak za administratore, pokrenite aplikaciju kategorizirao dozvole to od vas traži.  Sada se može pomaknuti na zahtijeva token za željenu resurs prema uputama u nastavku.

## <a name="using-permissions"></a>Pomoću dozvola

Kada korisnik identifikacijskom dozvola za aplikaciju, aplikacije možete dobiti pristup tokena koje predstavljaju pokrenite aplikaciju dozvolu za pristup resursu u nekim kapaciteta.  Token za dani pristup može se koristiti samo za jedan resorce, ali kodirani unutar njega će se svaka dozvola aplikacije odobren za taj resurs.  Dobiti pristupni token aplikacije možete napraviti zahtjeva krajnjoj tokena 2.0:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Dobivene token za pristup se zatim može koristiti u zahtjevima za HTTP resursa – to pouzdano pokazati resursa da aplikacije ima odgovarajuće dozvole za dani zadatak.  

Dodatne detalje na protokol OAuth 2.0 i kako nabaviti access tokeni potražite u članku [2.0 krajnjoj točki protokol referencu](active-directory-v2-protocols.md).
