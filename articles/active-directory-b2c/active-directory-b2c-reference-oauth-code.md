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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0 autorizacije kod tijek

Dodjela kod za autorizaciju OAuth 2.0 može se koristiti u aplikacijama koje se instaliraju na uređaju da biste pristupili zaštićeni resurse, kao što je web API-ji. Pomoću implementacije B2C Azure Active Directory (Azure AD) OAuth 2.0 možete dodati identitet za prijavu, prijavu i druge zadatke upravljanja mobilnim i stolnih aplikacija. Ovaj je vodič namijenjen neovisno jezik. To opisuje slanje i primanje poruka HTTP bez korištenja bilo koji od naše biblioteke Otvori izvor.

<!-- TODO: Need link to libraries -->

Tijek kod za autorizaciju OAuth 2.0 opisan u [odjeljku 4.1 specifikacija OAuth 2.0](http://tools.ietf.org/html/rfc6749). Možete je koristiti za izvođenje provjere autentičnosti i autorizacije u većini vrste aplikacije, uključujući [web-aplikacije](active-directory-b2c-apps.md#web-apps) i [nativno instalirane aplikacije](active-directory-b2c-apps.md#mobile-and-native-apps). Omogućuje aplikacija sigurno dobiti **access_tokens**, koji se može koristiti za pristup resurse koji su zaštićeni [provjeru autentičnosti poslužitelja](active-directory-b2c-reference-protocols.md#the-basics).

Ovaj vodič sadrži određene flavor od OAuth 2.0 autorizacije kod tijek –**javno klijente**. Javno klijent je klijentska aplikacija koje se radi o nepouzdanoj sigurno održavanje cjelovitosti tajnu lozinke. To obuhvaća mobilne aplikacije, aplikacija za stolna računala i vrlo koliko bilo koju aplikaciju koja se izvodi na uređaju, a treba uzeti access_tokens. Ako želite dodati upravljanje identitetom web App pomoću Azure AD B2C, trebali biste koristiti [OpenID povezivanje](active-directory-b2c-reference-oidc.md) umjesto OAuth 2.0.

Azure AD B2C proširuje standardna OAuth 2.0 teče potrebno više od jednostavne provjere autentičnosti i autorizacije. On predstavlja [**parametar pravila**](active-directory-b2c-reference-policies.md), koji omogućuje koristiti OAuth 2.0 bogatih da biste dodali aplikaciju programa, kao što su prijave, prijava i upravljanje profila. U nastavku ćemo vam pokazati kako koristiti OAuth 2.0 i pravila za svaki od tih sučelja implementirati u izvornim aplikacijama. Ćemo vam pokazati kako access_tokens za pristup web API-ji.

Donji primjer HTTP zahtjeva koristit će naš direktorija B2C uzorka, **fabrikamb2c.onmicrosoft.com**, kao i naše primjer aplikacije i pravila. Slobodno isprobajte zahtjeva za odgovor sami pomoću ove vrijednosti ili ih možete zamijeniti s vlastitim.
Saznajte kako [dobiti vlastiti direktorija B2C, aplikaciju, i pravila](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. preuzimate kod za provjeru autentičnosti
Kod tijek autorizacije počinje koja usmjeruje korisniku klijenta s `/authorize` krajnjoj točki. To je interaktivne dio tijeka, gdje je korisnik će zapravo akcija. U ovaj zahtjev klijent označava dozvole koje je potrebno Nabava korisnika u na `scope` parametar i pravilo koje će se izvršiti u na `p` parametar. Tri primjera navedene u nastavku (s prijelomima zbog čitljivosti), svaki putem različitih pravila.

#### <a name="use-a-sign-in-policy"></a>Pomoću pravila za prijavu

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Koristi pravilo za prijavu

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Korištenje Pravilnik za uređivanje profila

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parametar | Obavezan? | Opis |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Obavezno | ID aplikacije koji [Azure portal](https://portal.azure.com) dodijeljene aplikacije. |
| response_type | Obavezno | Upišite odgovor koji moraju sadržavati `code` za tijek kod za autorizaciju. |
| redirect_uri | Obavezno | Redirect_uri aplikacije, gdje mogu poslati i primio aplikacije odgovora za provjeru autentičnosti. Ga mora u potpunosti odgovarati nešto redirect_uris koju ste registrirali portalu osim što se mora biti kodirati URL-om. |
| opseg | Obavezno | Popis prostora odvojene opsega. Opseg jednu vrijednost označava za Azure AD i dozvola koje su tražena. Upotreba ID klijenta kao opseg označava aplikacije potreban je **pristup tokena** koje je moguće koristiti protiv vlastite servisa ili API na webu, predstavlja isti ID klijenta  Na `offline_access` opseg označava da će aplikacije moraju **refresh_token** long-lived pristupa resursima.  Možete koristiti u `openid` opseg da biste zatražili **id_token** iz Azure AD B2C. |
| response_mode | Preporučeno | Način na koji želite koristiti za slanje rezultata authorization_code natrag u aplikaciju. To može biti nešto od "query", 'form_post' ili 'fragment'.
| Stanje | Preporučeno | Vrijednost koja se uključiti u zahtjevu za također će se vratiti u odgovoru tokena. Možda ćete niz sav sadržaj koji želite. Slučajno generiranim jedinstvenu vrijednost obično koristi za sprječavanje napada krivotvorina zahtjev za web-mjesta. Stanje u kojem se koristi za kodiranje informacije o stanju korisnika u aplikaciji prije nego što je došlo do zahtjev za provjeru autentičnosti, kao što su na stranici ili pravila koja se izvršava. |
| p | Obavezno | Pravila koja će se izvršavati. To je naziv pravila koja je stvorena u direktoriju B2C. Vrijednost za naziv pravilnika započeti s "b2c\_1\_". Dodatne informacije o pravilima u [okvir Extensible pravila](active-directory-b2c-reference-policies.md). |
| upit | Neobavezno | Vrste interakcije s korisnikom koji je potreban. Samo valjana vrijednost trenutno je 'prijava', koje navodi korisnika da biste unijeli vjerodajnice na taj zahtjev. Jedinstvenu prijavu ne primjenjuje se. |

Sada korisnika će se zatražiti da biste dovršili pravila tijeka rada. To može obuhvaćati korisnika unesete korisničko ime i lozinku, prijaviti pomoću društvenih identitet registracije za imenika ili bilo kojeg drugog broja koraka, ovisno o tome kako je definirano pravila.

Kada korisnik dovrši pravilnik, vratit će Azure AD odgovor na aplikaciju na na navedeni `redirect_uri`, pomoću način naveden u na `response_mode` parametar. Odgovor će biti potpuno isto za svaku iznad slučajeva, neovisno o pravilniku izvršen.

Uspješan odgovor koji koristi `response_mode=query` izgleda:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| kod | Authorization_code koji ste tražili aplikaciju. Aplikaciju možete koristiti autorizacije kod da biste zatražili programa access_token resursa cilj. Authorization_codes su vrlo kratkog lived. Obično isteknu nakon 10 minuta. |
| Stanje | Pogledajte cijeli opis u prethodnoj tablici. Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |

Pogreška odgovora može biti poslan i da biste u `redirect_uri` tako da ih aplikaciju možete pravilno rukovati:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametar | Opis |
| ----------------------- | ------------------------------- |
| Pogreška | Do pogreške kod niz koji mogu se klasificirati vrste pogrešaka koje će se pojaviti, a mogu se Upoznajte pogreške. |
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer da biste odredili uzrok pogreške provjere autentičnosti. |
| Stanje | Pogledajte cijeli opis u prvoj tablici u ovom odjeljku. Ako je parametar stanje uključena u zahtjevu za jednaku vrijednost prikazivati u odgovoru. Aplikaciju trebali biste provjerite jesu li vrijednosti stanja u zahtjeva i odgovora jednake. |


## <a name="2-get-a-token"></a>2. zatražite token
Sad kad ste nabavili programa authorization_code, ne možete iskoristiti u `code` token željeni resursa slanjem u `POST` zahtjev za na `/token` krajnjoj točki. U Azure AD B2C, samo resurs koji možete zatražiti token za je vaše aplikacije vlastite pozadinski API na webu. Konvencije koja se koristi za traženje token sebi je da biste koristili aplikaciju programa ID klijenta kao opseg:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametar | Obavezan? | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| p | Obavezno | Pravila koja se koristila dobiti kod za autorizaciju. Ne možete koristiti različite pravila u zahtjev. Imajte na umu dodati taj parametar u *nizu upita*, ne i u tijelu objavu. |
| client_id | Obavezno | ID aplikacije koji [Azure portal](https://portal.azure.com) dodijeljene aplikacije. |
| grant_type | Obavezno | Vrsta dodijelite, mora biti `authorization_code` za tijek kod za autorizaciju. |
| opseg | Reccommended | Popis prostora odvojene opsega. Opseg jednu vrijednost označava za Azure AD i dozvola koje su tražena. Upotreba ID klijenta kao opseg označava aplikacije potreban je **pristup tokena** koje je moguće koristiti protiv vlastite servisa ili API na webu, predstavlja isti ID klijenta  Na `offline_access` opseg označava da će aplikacije moraju **refresh_token** long-lived pristupa resursima.  Možete koristiti u `openid` opseg da biste zatražili **id_token** iz Azure AD B2C. |
| kod | Obavezno | Authorization_code koju ste nabavili u prvom leg tijek. |
| redirect_uri | Obavezno | Redirect_uri aplikaciju koju ste primili na authorization_code. |

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
| access_token | Traje JSON Web tokena (JWT) token koji ste tražili. |
| opseg | Dosezi koji token vrijedi za, koji se može koristiti za predmemoriranje tokeni za kasnije korištenje. |
| expires_in | Trajanja koji token vrijedi (u sekundama). |
| refresh_token | Na refresh_token OAuth 2.0. Aplikaciju možete koristiti ovaj token potrebne dodatne tokeni nakon isteka trenutne token. Refresh_tokens su mogli dugo živjeti pa se poslužite da biste zadržali pristup resursima za prošireni vremenskog razdoblja. Više detalja potražite [B2C tokena referencu](active-directory-b2c-reference-tokens.md). |

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
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer da biste odredili uzrok pogreške provjere autentičnosti. |

## <a name="3-use-the-token"></a>3. koristite tokena
Sad kad ste nabavili uspješno programa `access_token`, token možete koristiti u zahtjevima za vaše pozadinski web-API-ji stvorenim u na `Authorization` zaglavlja:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. osvježavanje tokena
Pristup i Id tokena su kratkog lived. Morate ih osvježavaju nakon isteknu i dalje moći pristupati resursi. To možete učiniti tako da drugi slanje `POST` zatražiti na `/token` krajnjoj točki. Navedite ovaj put u `refresh_token` umjesto na `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametar | Obavezan? | Opis |
| ----------------------- | ------------------------------- | -------- |
| p | Obavezno | Pravila koja se koristila za izvorni refresh_token. Ne možete koristiti različite pravila u zahtjev. Imajte na umu dodati taj parametar u *nizu upita*, ne i u tijelu objavu. |
| client_id | Preporučeno | ID aplikacije koji [Azure portal](https://portal.azure.com) dodijeljene aplikacije. |
| grant_type | Obavezno | Vrsta dodijelite, mora biti `refresh_token` za ovaj leg tijek autorizacije kod. |
| opseg | Preporučeno | Popis prostora odvojene opsega. Opseg jednu vrijednost označava za Azure AD i dozvola koje su tražena. Upotreba ID klijenta kao opseg označava aplikacije potreban je **pristup tokena** koje je moguće koristiti protiv vlastite servisa ili API na webu, predstavlja isti ID klijenta  Na `offline_access` opseg označava da će aplikacije moraju **refresh_token** long-lived pristupa resursima.  Možete koristiti u `openid` opseg da biste zatražili **id_token** iz Azure AD B2C. |
| redirect_uri | Neobavezno | Redirect_uri aplikaciju koju ste primili na authorization_code. |
| refresh_token | Obavezno | Na izvornom refresh_token koju ste nabavili u drugom leg tijek. |

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
| access_token | Traje JSON Web tokena (JWT) token koji ste tražili. |
| opseg | Dosezi koji token vrijedi za, koji se može koristiti za predmemoriranje tokeni za kasnije korištenje. |
| expires_in | Trajanja koji token vrijedi (u sekundama). |
| refresh_token | Na refresh_token OAuth 2.0. Aplikaciju možete koristiti ovaj token potrebne dodatne tokeni nakon isteka trenutne token. Refresh_tokens su mogli dugo živjeti pa se poslužite da biste zadržali pristupa resursima za prošireni vremenskog razdoblja. Više detalja potražite [B2C tokena referencu](active-directory-b2c-reference-tokens.md). |

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
| error_description | Na određenu poruku o pogrešci koje omogućuju razvojni inženjer identificiranje uzrok pogreške provjere autentičnosti. |

## <a name="use-your-own-b2c-directory"></a>Korištenje B2C direktorija

Ako želite isprobati te zahtjeve za sebe, morate najprije izvođenje ta tri koraka, a zatim zamijenite primjere vrijednosti iznad s vlastitim:

- [Stvaranje direktorij B2C](active-directory-b2c-get-started.md) i korištenje naziva direktorija u zahtjeve.
- [Stvaranje aplikacije](active-directory-b2c-app-registration.md) da biste dobili ID aplikacije i u redirect_uri. Želite obuhvatiti **nativni klijent** za aplikacije.
- [Stvaranje police](active-directory-b2c-reference-policies.md) da biste dobili naziva pravila.
