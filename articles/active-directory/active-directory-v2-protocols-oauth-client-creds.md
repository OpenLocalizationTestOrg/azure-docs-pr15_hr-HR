
<properties
    pageTitle="Azure AD 2.0 OAuth klijent vjerodajnice tijek | Microsoft Azure"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>Tijek protokoli 2.0 – OAuth 2.0 klijent vjerodajnice

Na [dodijeliti OAuth 2.0 klijent vjerodajnice](http://tools.ietf.org/html/rfc6749#section-4.4), ponekad se naziva "dva legged OAuth," se poslužite da biste pristupili web hostira resursima pomoću identiteta aplikacije.  Obično se koristi za poslužitelj-poslužitelj interakcije koje morate pokrenuti u pozadini bez odmah precense od krajnji korisnik.  Ove vrste aplikacija se često se nazivaju **daemons** ili **računa servisa**.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

U na više uobičajeni tri "tri legged OAuth," klijentska aplikacija je dodijeliti dozvolu za pristup resursa ime određenom korisniku.  Dozvole je **Dodijeljeno** korisniku na aplikaciju, obično tijekom postupka [pristanak](active-directory-v2-scopes.md) .  No u vjerodajnice tijek klijent su dozvole dodijeljene **izravno** na same aplikacije.  Kada aplikacija predstavlja token za neki resurs, resurs nameće ima li samoj aplikaciji autorizacije da biste izvršili neku akciju – ne ima li neki korisnik autorizacije.

## <a name="protocol-diagram"></a>Protokol dijagrama
Tijek vjerodajnice za cijeli klijent izgleda otprilike ovako: svakog koraka su detaljno opisane u nastavku.

![Tijek vjerodajnice klijenta](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Početak Izravni autorizacije 
Dva su načina aplikacije obično prima Izravni autorizacije da biste pristupili resursa: putem popis za kontrolu pristupa na mjestu ili putem aplikacije Dodjela dozvola u Azure AD.  Na nekoliko načina drugih resursa odabrati da biste autorizirali svojim klijentima i svakome resursa možda odaberite način koji vam najviše odgovara za njezinoj aplikaciji.  Ta dva načina su najčešće Azure AD i reccommended za klijente i resurse koji želite obaviti vjerodajnice tijek klijenta.

### <a name="access-control-lists"></a>Popisi za upravljanje
Davatelja navedeni resursa može provesti programa potvrdite autorizacije utemeljenog na popisu ID-ove je zna i daje neki određeni razinu pristupa.  Kada resurs prima token iz krajnju točku 2.0, može dešifrirati token i izdvajanje u ID klijenta aplikacije iz sustava `appid` i `iss` zahtjevima.  Zatim možete usporediti da računala s nekim popis kontrole pristupa (ACL) je održava.  Preciznosti i način popis za kontrolu pristupa mogu se razlikovati znatno iz resursa za resurs.

Uobičajena slučaja koristi takve ACL-a je testiranje trkača za web-aplikaciju ili web-API-ja.  Na webu API-ja možda samo dodijelite podskup njegov dozvole za potpunu njegov različite klijente.  No da bi se pokrenuti testira end da biste završetka na API-ja, klijent test se stvara nabaviti tokena iz krajnju točku 2.0 i šalje u API-ja.  Na API-ja možete ACL ID klijenta test aplikacije za potpuni pristup cijelu funkcionalnost na API-ja.  Imajte na umu da ako takav popis o vašoj usluzi, morate biti sigurni da ne samo Provjera valjanosti pozivatelju `appid`, ali i provjeriti koji se `iss` tokena je pouzdan kao i.

Ta vrsta Autorizacija nije zajednička daemons i računi servisa koji moraju imati pristup podacima vlasništvo potrošača korisnika s osobni računi za Microsoft.  Za podatke o vlasništvu tvrtke ili ustanove, preporučuje se nabavljate potrebne autorizacije putem aplikacije perimssions.

### <a name="application-permissions"></a>Dozvole za aplikaciju
Umjesto korištenja ACL-a, API-je možete izložiti skup **dozvola za aplikaciju** koja se mogu dodijeliti u aplikaciju.  Programu dozvolu moguć je aplikaciji administrator tvrtke ili ustanove, a može se koristiti samo za pristup podacima posjeduje svojih zaposlenika i taj tvrtke ili ustanove.  Na primjer, Microsoft Graph izlaže nekoliko dozvola za aplikaciju:

- Čitanje poštu u poštanske sandučiće
- Čitanje i pisanje poštu u poštanske sandučiće
- Slanje e-pošte kao bilo koji korisnik
- Čitanje podataka direktorija
- [+ više](https://graph.microsoft.io)

Da biste dobiti te dozvole u aplikaciji, možete poduzeti sljedeće korake.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Zahtjev za dozvole na portalu za registraciju aplikacije

- Ako to već niste učinili, dođite do aplikacija u [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili [stvorite aplikaciju](active-directory-v2-app-registration.md) .  Morat ćete provjerite je li vaša aplikacija je stvorio barem jedan aplikaciji tajna.
- Pronađite odjeljak **Izravni dozvola za aplikaciju** i dodajte dozvole koje je potrebno aplikacije.
- Provjerite da biste **spremili** Registracija aplikacije

#### <a name="recommended-sign-the-user-into-your-app"></a>Preporučuje: Prijava korisnika u aplikaciju

Obično kada sastavljanje aplikacije koje koristi dozvola za aplikaciju, aplikaciju moraju imati stranice/prikaz koji omogućuje administrator da biste odobrili dozvole za u aplikaciju.  Ova stranica može biti dio aplikacije registracije protok postavkama aplikacije ili namjenski tijek "povezivanje".  U mnogim slučajevima, je li bolje aplikacije da biste prikazali taj "povezivanje" prikaz tek kada korisnik ima prijavljeni na poslu ili u školi Microsoftova računa.

Prijava korisnika u aplikaciju vam omogućuje da utvrdite organziation kojoj korisnik pripada prije da biste zatražili da biste odobrili dozvola za aplikaciju.  Dok je ne isključivo potrebno, može pomoći pri stvaranju više intuitivno sučelje za korisnike tvrtke ili ustanove.  Da biste se prijavili korisnika u Slijedite naše [vodiči za protokol 2.0](active-directory-v2-protocols.md).

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
| klijent | obavezno | Klijent direktorija koji želite zatražite dozvolu.  Možete koristiti u obliku neslužbeni naziv ili guid.  Ako ne poznajete koji klijent korisnik pripada i želite ih prijavite se pomoću bilo kojeg klijentu, koristite `common`. |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| redirect_uri | obavezno | Redirect_uri mjesto na koje želite odgovor slati aplikacije za rukovanje.  Ona mora u potpunosti odgovarati nešto redirect_uris registrirana na portalu osim ga mora biti kodirati url- a mogu sadržavati odsječaka dodatne puta. |
| Stanje | preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena.  Možda ćete niz sav sadržaj koji želite.  Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su stranice ili prikaz su na. |

U ovom trenutku Azure AD nametnuti da samo administrator klijenta mogu se prijaviti u da biste dovršili zahtjev.  Administrator će se zatražiti da biste odobrili sve dozvole Izravni aplikacija tražene aplikacije na portalu za registraciju. 

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

Kada primite uspješno odgovor u aplikaciji krajnja točka za dodjelu resursa, pokrenite aplikaciju kategorizirao dozvola za aplikaciju Izravni to od vas traži.  Sada se može pomaknuti na zahtijeva token za željeni resurs.

## <a name="get-a-token"></a>Početak token
Kada je potrebno autorizacije ste nabavili aplikacije, možete nastaviti s pri dohvaćanju pristupna tokena za API-ji.  Da biste dobili token pomoću klijentskog programa grant vjerodajnice, pošaljite zahtjev za objavu da biste na `/token` krajnjoj točki 2.0:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| client_id | obavezno | Id aplikacije da portala za registraciju ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) dodijeljen aplikacije. |
| opseg | obavezno | Proslijeđena vrijednost u `scope` parametar u zahtjev mora biti identifikator resursa (URI ID aplikacije) željeni resursa, ponuđenima s na `.default` sufiks.  Da bi navedeni primjer Microsoft Graph dali vrijednost mora biti `https://graph.microsoft.com/.default`.  Ta vrijednost obavještava krajnju točku 2.0 da sve Izravni aplikacija dozvola ste konfigurirali za aplikaciju, ga treba poslao tokena za one vezani uz u željenom resursa. |
| client_secret | obavezno | Tajna aplikacije koji je generirao na portalu za registraciju za aplikacije. |
| grant_type | obavezno | Mora biti `client_credentials`. | 

#### <a name="successful-response"></a>Uspješan odgovor
Uspješan odgovor će se obrazac:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token tražene programa access. Aplikaciju možete koristiti ovaj token za provjeru autentičnosti zaštićenim resursa, kao što je web-mjesto API-JA. |
| token_type | Označava vrijednost tokena vrste. Samo vrstu koja podržava Azure AD je `Bearer`.  |
| expires_in | Koliko dugo token za pristup vrijedi (u sekundama). |

#### <a name="error-response"></a>Odgovor pogreška
Obrazac se odgovorom o pogrešci:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer identificiranje uzrok pogreške provjere autentičnosti.  |
| error_codes | Popis kodova STS konkretne pogreške koji mogu olakšati Dijagnostika.  |
| Vremenska oznaka | Vrijeme u kojem se pogreške. |
| trace_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika.  |
| correlation_id | Jedinstveni identifikator za zahtjev koji mogu olakšati Dijagnostika preko komponente. |

## <a name="use-a-token"></a>Pomoću tokena
Sad kad ste nabavili token, možete koristiti taj token da biste zahtjeva za resurs.  Kada istekne token, samo ponovite zahtjev za na `/token` krajnjoj točki dobiti token Osvježi pristup.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Uzorak koda
Da biste vidjeli primjera aplikacija primjenjuje na client_credentials dodijeliti pomoću administrator pristanak je krajnje točke, pogledajte [2.0 daemon kod uzorka](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
