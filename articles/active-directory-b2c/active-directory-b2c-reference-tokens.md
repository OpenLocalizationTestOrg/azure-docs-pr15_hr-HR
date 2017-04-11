<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Vrste tokeni izdavanja u Azure Active Directory B2C."
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


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Referenca Token

Azure Active Directory (Azure AD) B2C emits nekoliko vrsta sigurnosnih tokena kao obrađuje svaki [tijek provjere autentičnosti](active-directory-b2c-apps.md). Ovaj dokument opisuje oblik, značajke sigurnosti i sadržaj svaku vrstu tokena.

## <a name="types-of-tokens"></a>Vrste tokena

Azure AD B2C podržava [protokol za autorizaciju OAuth 2.0](active-directory-b2c-reference-protocols.md), koje čini korištenje tokena access i osvježavanje tokena. I podržava provjeru autentičnosti i prijavu putem [OpenID povezivanje](active-directory-b2c-reference-protocols.md), koji predstavlja treći vrstu tokena: token ID-a. Svaki od tih tokeni smanjivati nošenja token.

Nošenja token je laganih sigurnosni token koja omogućuje pristup "nošenja" na zaštićenom resurs. Na nošenja je bilo koji proizvođača koje je moguće izlagati token. Azure AD morate najprije provjere autentičnosti na tulum možete primati nošenja token. No ako potrebne korake ne uzimaju se sigurnost tokena prijenosa i prostora za pohranu, može biti intercepted i koristi neželjeni proizvođača. Neke sigurnosnih tokena su ugrađene mehanizam za sprječava neovlaštenog stranama njihova korištenja, ali nošenja tokeni nemaju ovaj mehanizam. Morate se prenijeti u sigurnog kanala, kao što je sigurnost sloja prijenosa (HTTPS).

Ako nošenja token prenosi izvan sigurnog kanala, zlonamjerni strana pomoću Muškarac-u-u – srednje napada Nabava token te ga koristiti da bi se dobio neovlaštenog pristupa zaštićenim resursa. Na isti način sigurnost primijeniti kada nošenja tokeni i spremanja predmemorirane za kasnije korištenje. Uvijek provjerite je li aplikacije prenosi i pohranjuje nošenja tokeni siguran način.

Dodatni sigurnosna pitanja vezana uz na nošenja tokena, potražite u članku [RFC 6750 odjeljak 5](http://tools.ietf.org/html/rfc6750).

Mnoge tokena koje izdaje Azure AD B2C implementirana kao tokeni JSON web (JWTs). U JWT je sažetom, URL sigurnih srednje vrijednosti prijenos informacija između dvije strane. JWTs sadrže podatke koji se nazivaju zahtjevima. To su assertions informacije o na nošenja i predmet token. Zahtjevima u JWTs su JSON objekte koji su šifriraju i serijalizirani za prijenos. Budući da JWTs izdala Azure AD B2C su prijavljeni, ali nije šifrirana, možete jednostavno provjeri sadržaj JWT za ispravljanje pogrešaka je. Alati za nekoliko dostupni su koje možete učiniti, uključujući [calebb.net](http://calebb.net). Dodatne informacije o JWTs potražite [JWT specifikacije](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID tokena

Token za ID-a je obrazac za sigurnosni token prima aplikacije iz Azure AD B2C `authorize` i `token` krajnje točke. ID tokena prikazane su kao [JWTs](#types-of-tokens), a mogu sadržavati zahtjevima koje možete koristiti da biste odredili korisnicima u svojoj aplikaciji. Kada se tokeni ID dobivene iz na `authorize` krajnje točke, često koristi za prijavu u korisnicima web-aplikacije. Kada se tokeni ID dobivene iz na `token` krajnje točke, mogu se slati u zahtjevima za HTTP tijekom komunikacije između dviju komponenata aplikacije ili servisa. Možete koristiti na zahtjevima u token za ID-a kao svojim potrebama. Najčešće koriste se za prikaz podataka o računu ili donošenje odluka kontrole programa access u aplikaciji programa.  

ID tokena prijavljeni, ali nije šifrirana trenutno. Kada aplikacije ili API primi token za ID-a, ga morate [provjeriti potpis](#token-validation) da biste dokazali da je tekst autentičnim token. Aplikacije ili API-JA i morate provjeriti nekoliko zahtjevima token tako dokazali da je valjan. Ovisno o preduvjetima scenarij zahtjevima valjanost aplikacije mogu se razlikovati, ali aplikacije moraju izvršiti neke [uobičajene zahtjeva provjere valjanosti](#token-validation) u svakoj scenariju.

#### <a name="sample-id-token"></a>Ogledna token ID-a
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Pristupna tokena

Pristupni token je i oblik sigurnosni token prima aplikacije iz Azure AD B2C `authorize` i `token` krajnje točke. Pristupna tokena su predstavljene i kao [JWTs](#types-of-tokens), a mogu sadržavati zahtjevima koje možete koristiti da biste odredili korisnicima u web-servisi & API-ji.

Pristupna tokena prijavljeni, ali nisu šifrirane trenutku - i slično je tokeni ID-a.  Pristupna tokena treba koristiti za pristup web-servisi & API-ji i za prepoznavanje i provjere autentičnosti korisnika u tim servisima.  Međutim, ne pružaju bilo kojeg dijela autorizacije pri tih servisa.  Što znači koji se `scp` zahtjeva u access tokeni ograničiti ili u suprotnom predstavljaju pristup odobren predmet token.

Kada vaš API primi token za pristup, ga morate [provjeriti potpis](#token-validation) da biste dokazali da je tekst autentičnim token. Vaš API-JA i morate provjeriti nekoliko zahtjevima token tako dokazali da je valjan. Ovisno o preduvjetima scenarij zahtjevima valjanost aplikacije mogu se razlikovati, ali aplikacije moraju izvršiti neke [uobičajene zahtjeva provjere valjanosti](#token-validation) u svakoj scenariju.

### <a name="claims-in-id--access-tokens"></a>Sporove u ID-a i pristup tokena

Kada koristite Azure AD B2C, imate preciznije kontrolirati sadržaj vaše tokena. Možete konfigurirati [pravila](active-directory-b2c-reference-policies.md) da biste poslali određene skupove podataka korisnika u zahtjevima za koje je potrebno aplikacije za operacije. Ove zahtjevima možete uključiti standardna svojstva kao što su korisnika `displayName` i `emailAddress`. Također mogu sadržavati [prilagođene korisničke atribute](active-directory-b2c-reference-custom-attr.md) koje možete definirati u direktoriju B2C. Svaki ID-a i pristup tokena koje primate će sadržavati određeni skup zahtjevima vezane uz sigurnost. Aplikacija možete koristiti te zahtjevima za sigurno provjere autentičnosti korisnika i zahtjeve.

Imajte na umu da zahtjevima u ID tokena se vraćaju u nekom određenom redoslijedu. Uz to, novi zahtjevima možete biti uvedena u ID tokena u bilo kojem trenutku. Aplikacije ne treba prelomiti kao uvode se novi zahtjevima. Evo zahtjevima koji očekujete da postoji u ID-a i pristup tokeni izdala Azure AD B2C. Sve dodatne zahtjevima će određen pravila. Za vježbe, pokušajte pregled zahtjevima u token ID-a uzorak lijepljenjem u [calebb.net](http://calebb.net). Dodatne pojedinosti pronaći ćete u [OpenID povezivanje specifikacija](http://openid.net/specs/openid-connect-core-1_0.html).

| Ime | Zahtjeva | Primjer vrijednost | Opis |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Ciljne skupine | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Zahtjev u ciljne skupine označava je namijenjen token. Za Azure AD B2C, publike je ID aplikacije za aplikaciju programa, kao što je dodijeljen aplikacije na portalu za registraciju aplikacije. Aplikaciju treba provjeriti tu vrijednost i odbacivanje token ako ne odgovara. |
| Izdavač | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | U ovom zahtjeva služi za identifikaciju servis sigurnosnih tokena (STS) koji konstrukcije i vraća token. Također, označava Azure AD direktoriju u kojem je autentičnost korisnika. Pokrenite aplikaciju bi trebali provjeriti izdavača zatraži da biste bili sigurni da se token dolazi od krajnje 2.0. |
| Izdana u | `iat` | `1438535543` | Ovaj zahtjev je vrijeme na kojoj se token izdavanja, prikazan u vremenu epoch. |
| Vrijeme isteka | `exp` | `1438539443` | Vrijeme isteka zahtjev je vrijeme na kojoj se postaje koji nisu valjani, token predstavljeni u epoch vrijeme. Pokrenite aplikaciju trebali biste koristiti ovaj zahtjeva da biste provjerili valjanost tokena života.  |
| Ne prije | `nbf` | `1438535543` | Ovaj zahtjev je vrijeme na kojoj se token postaje valjana, prikazano u vremenu epoch. To je obično jednaki vremena izdavanja token. Pokrenite aplikaciju trebali biste koristiti ovaj zahtjeva da biste provjerili valjanost tokena vijek trajanja.  |
| Verzija | `ver` | `1.0` | Ovo je verzija token ID-a kako je definirano Azure AD. |
| Raspršivanje kod | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Token za ID-a kod raspršivanje obuhvaćeno samo kada token izdaje zajedno s je OAuth 2.0 autorizacije kod. Kod raspršivanje može se koristiti za provjeru autentičnosti i pripadnog koda autorizacije. Potražite u članku [Povezivanje OpenID specifikacija](http://openid.net/specs/openid-connect-core-1_0.html) više pojedinosti o tome da biste izvršili ovaj provjere valjanosti. |
| Pristup tokena raspršivanje | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Raspršivanje tokena programa access je sve obuhvaćeno token za ID-a samo kad token izdaje zajedno s OAuth 2.0 pristupni token. Raspršivanje tokena programa access može se koristiti za provjeru autentičnosti token za pristup. Potražite u članku [Povezivanje OpenID specifikacija](http://openid.net/specs/openid-connect-core-1_0.html) više pojedinosti o tome da biste izvršili ovaj provjere valjanosti. |
| Jednokratna šifra | `nonce` | `12345` | Na Jednokratna šifra je strategija u smanjenju tokena Ponovi napada. Pokrenite aplikaciju možete odrediti na Jednokratna šifra u zahtjev za autorizaciju pomoću na `nonce` parametarski upit. Vrijednost unesete u zahtjevu za će biti čuje nepromijenjenu u na `nonce` zahtjeva ID tokena samo. Time se omogućuje aplikacije da biste provjerili vrijednost u odnosu vrijednost navedena na zahtjev u aplikaciji sesiju pridružuje navedeni token ID-a. Pokrenite aplikaciju provoditi ovaj provjere valjanosti tijekom postupka ID tokena provjere valjanosti. |
| Predmet | `sub` | `Not supported currently. Use oid claim.` | To je glavni o token nametanja podatke, kao što su korisnički aplikacije. Ova vrijednost je immutable i ne mogu ponovno dodijeliti ili ponovno iskoristiti. To se može koristiti za izvršiti provjeru autorizacije sigurno, primjerice kada se token koristi da biste pristupili resurs. Međutim, predmet zahtjeva nije još implementirana u Azure AD B2C. Trebali biste konfigurirati police da biste uključili ID objekta `oid` zahtjeva i koristiti njegovom vrijednošću za identifikaciju korisnika, umjesto korištenja predmet zahtjeva za autorizaciju. |
| Provjera autentičnosti kontekst predmete reference | `acr` | `b2c_1_sign_in` | To je naziv pravila koji je korišten za dohvaćanje token ID-a.  |
| Provjera autentičnosti vremena | `auth_time` | `1438535543` | Ovaj zahtjev je vrijeme koje se u korisničke posljednje unijeti vjerodajnice, predstavljeni u epoch vrijeme. |


### <a name="refresh-tokens"></a>Osvježavanje tokena

Osvježavanje tokeni su sigurnosnih tokena aplikacije pomoću možete dobiti novi ID tokena i pristup tokeni u tijeku je OAuth 2.0. Pružaju aplikacije s Dugoročne pristup resursima ime korisnika bez interakcije s tim korisnicima.

Da biste primali osvježavanja tokena tokena odgovor, aplikacije morate zatražiti na `offline_acesss` opseg. Da biste saznali više o na `offline_access` opseg, pogledajte [Azure AD B2C protokol referencu](active-directory-b2c-reference-protocols.md).

Osvježavanje tokeni su pa će se odnositi, potpuno neprozirne aplikacije. Azure AD izdavanja i mogu se provjerava ima li i protumačiti samo Azure AD. Su long-lived, ali moraju biti napisani aplikacije s očekivanja koji osvježavanja tokena će trajati određenog vremenskog razdoblja. Tokeni osvježavanja može biti invalidated u bilo kojem trenutku raznih razloga. Jedini način za aplikaciju znati osvježavanja token vrijedi jest da biste iskoristili tako da tokena zahtjev za Azure AD.

Kada iskoristili osvježavanja token novi token (i ako vam je aplikacije na `offline_access` opseg), dobit ćete novi token osvježavanja tokena odgovor. Trebali biste spremiti token upravo Izdana osvježavanja. Ona treba zamijeniti tokena osvježavanja ste prethodno koristili u zahtjev. Omogućuje jamči da ostanu token za osvježavanje vrijedi za dugo moguće.

## <a name="token-validation"></a>Tokena provjere valjanosti

Da biste provjerili token aplikacije provjerite potpis i zahtjevima tokena.

Biblioteka s mnogo Otvori izvor dostupni su za provjeru valjanosti JWTs, ovisno o Preferirani jezik. Preporučujemo da vam Istraživanje te mogućnosti umjesto implementirati vlastite logiku provjere valjanosti. Informacije u ovom vodiču omogućuju Naučite ispravno koristiti te biblioteke.

### <a name="validate-the-signature"></a>Provjerite valjanost potpisa
Na JWT sadrži tri segmenata odvojene na `.` znakova. U prvom segmentu je **Zaglavlje**, drugi **tijelo**, a treći **potpis**. Segment potpis može se koristiti za provjeru autentičnosti tokena tako da ga možete moraju vjerovati aplikacije.

Tokeni Azure AD B2C prijavljeni pomoću algoritama za standardni asimetričnim šifriranja, kao što su RSA 256. U zaglavlju tokena sadrži informacije o način ključ i šifriranje koji se koristi za potpisivanje tokena:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Na `alg` zahtjeva označava algoritam koji je korišten za potpisivanje tokena. Na `kid` zahtjeva označava određeni javni ključ koji je korišten za potpisivanje tokena.

U bilo kojem trenutku, Azure AD potpisati token pomoću bilo koju od određenog skupa javno privatni ključ parove. Azure AD povremeno zakreće mogući skup tipke tako da se aplikacijom moraju biti napisani automatski rukovati promjenama ključa. Pametnije učestalost da biste provjerili postoje li ažuriranja za javni ključ koristi Azure AD je svaka 24 sata.

Azure AD B2C sadrži metapodatke krajnje OpenID povezivanje. Time se omogućuje aplikacije za dohvaćanje informacija o Azure AD B2C tijekom rada. Te informacije obuhvaćaju krajnje točke, tokena sadržaj i token potpisivanja tipke. Direktorija B2C sadrži dokument JSON metapodataka za svaku pravila. Na primjer, dokumentu metapodataka za na `b2c_1_sign_in` pravila u `fabrikamb2c.onmicrosoft.com` nalazi se na:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`je B2C direktorija koji se koriste za provjeru autentičnosti korisnika i `b2c_1_sign_in` pravila za dohvaćanje token. Da biste odredili pravilo korišten za prijavu token (i gdje dohvaćati metapodatke), imate dvije mogućnosti. Najprije obuhvaćene naziv pravila u `acr` zahtjeva u token. Možete analizirati zahtjevima iz tijela na JWT po base-64 dekodiranje tijelo i prekidanja serije niz JSON koja je rezultat. Na `acr` zahtjeva bit će naziv pravila koja se koristila za izdavanje token.  Druga mogućnost je kodiranje pravila u vrijednosti u `state` parametar kada je poslao zahtjev i dekodiranje da biste odredili korišten pravilo. Neke od ovih metoda je valjan.

Dokument metapodataka je JSON objekt koji sadrži nekoliko dijelova korisne informacije. To obuhvaća mjesto krajnje točke potrebne za izvršavanje povezivanje OpenID provjeru autentičnosti. Također sadrže `jwks_uri`, koji omogućuje mjesto skup javni ključ koji se koriste za potpisivanje tokena. Da mjesto ovdje dana, ali preporučuje se da dinamički dohvaćati mjesto pomoću metapodataka dokumenta i analize izvan `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

JSON dokumenta koji se nalazi na ovom URL-sadrži sve podatke o javni ključ se koristi u određenom trenutku. Možete koristiti aplikaciju na `kid` zahtjeva u zaglavlju JWT da biste odabrali javni ključ JSON dokumenta koji se koristi da biste se prijavili određeni token. Ga možete izvršiti provjeru valjanosti potpisa pomoću odgovarajuće javni ključ i navedeni algoritam.

Opis kako izvršiti provjeru valjanosti potpisa je izvan opsega ovog dokumenta. Bibliotekama s mnogo Otvori izvor dostupnih za ovaj ako vam je potrebna.

### <a name="validate-the-claims"></a>U zahtjevima za provjeru valjanosti
Kada aplikacije ili API primi token za ID-a, ga i provoditi nekoliko provjere u odnosu na zahtjevima u token ID-a. Te obuhvaćaju, ali nije ograničena su:

- **Ciljne skupine** zahtjeva: To potvrđuje da je token ID-a koji želite dobiti u aplikaciju.
- **Ne prije** i **vrijeme isteka** zahtjevima: te provjerite je li token ID-a nije istekao.
- **Izdavač** zahtjeva: Time se provjerava da token izdala u aplikaciju za Azure AD.
- **Jednokratna šifra**: to je strategija tokena Ponovi napada ublažiti.

Potpuni popis provjere valjanosti provoditi aplikacije, pogledajte [Povezivanje OpenID specifikacija](https://openid.net). Očekivani vrijednosti za ove zahtjevima detalje potražite u prethodnom [tokena sekcije](#types-of-tokens).  

## <a name="token-lifetimes"></a>Tokena vijekom trajanja

Dostupni su sljedeće tokena vijekom trajanja dodatno svoje znanje. One vam mogu pomoći pri razvoj i aplikacije za ispravljanje pogrešaka. Imajte na umu da se aplikacija moraju biti napisani očekivati neki od ovih vijekom trajanja da biste ostaju iste. Možete i će se promijeniti.  Dodatne informacije o prilagodbi tokena vijekom trajanja u Azure AD B2C [ovdje](active-directory-b2c-token-session-sso.md).

| Tokena | Života. | Opis |
| ----------------------- | ------------------------------- | ------------ |
| ID tokena | Jedan sat | ID tokena vrijede obično za jedan sat. Web-aplikaciju programa možete koristiti ovaj vijek da biste zadržali vlastitu sesije s korisnicima (preporučeno). Možete odabrati i različite sesiju života. Ako aplikacija nije potrebno dobiti novi ID tokena, jednostavno potrebne da biste unijeli novi zahtjev za prijavu Azure AD. Ako korisnik ima valjan preglednika sesiju sa Azure AD, taj korisnik možda neće morati ponovno unesite vjerodajnice. |
| Osvježavanje tokena | Do 14 dana | Osvježavanje za jedan znak vrijedi za najviše 14 dana. Međutim, token osvježavanja može postati koji nisu valjani u bilo kojem trenutku za bilo koji broj razloga. Pokrenite aplikaciju nastaviti da biste pokušali pomoću tokena Osvježi sve dok ne uspije zahtjev ili dok aplikacije zamjenjuje token osvježavanja novi.  Osvježavanje token također možete biti valjane ako 90 dana prošlo jer je korisnik unio zadnje vjerodajnice. |
| Kodovi za provjeru autentičnosti | Pet minuta | Šifre autorizacije su namjerno short-lived. Oni moraju biti aktivacije odmah za pristup tokeni, tokeni ID ili osvježavanje tokeni po primitku. |
