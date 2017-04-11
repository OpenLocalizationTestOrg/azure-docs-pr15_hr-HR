<properties
    pageTitle="Promijeniti u krajnje 2.0 Azure AD | Microsoft Azure"
    description="Opis o promjenama unesenima u tijeku protokoli javno pretpregled 2.0 modela aplikacija."
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

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Važna ažuriranja za provjeru autentičnosti protokoli 2.0
Razvojni inženjeri pozornost! Putem sljedeće dva tjedna, ne možemo će skenira sliku u nekoliko ažuriranja za naše protokola za provjeru autentičnosti 2.0 koje može značiti najnovije promjene ste napisali tijekom razdoblja naš pretpregleda aplikacija.  

## <a name="who-does-this-affect"></a>Tko to utječe na?
Aplikacija koje napisan da biste koristili u 2.0 konvergira krajnje provjere autentičnosti

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Moguće je pronaći dodatne informacije o krajnjoj točki 2.0 [ovdje](active-directory-appmodel-v2-overview.md).

Ako ste ugrađena aplikacije pomoću kodiranje izravno u protokol 2.0 krajnju točku 2.0, na bilo koji od naše OpenID povezati ili OAuth middlewares web ili pomoću drugih 3 biblioteka sudionika da biste izvršili provjeru autentičnosti, koje treba se pripremite i testiranje projekte mijenjati po potrebi.

## <a name="who-doesnt-this-affect"></a>Tko ne to utječe na?
Aplikacija koje napisan protiv provjere autentičnosti krajnje radnog Azure AD

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Protokol koji je potrebno je u kamen i će biti postoje li sve promjene.

Osim toga, ako vaša aplikacija **samo** koristi naše ADAL biblioteke da biste izvršili provjeru autentičnosti, ne morate ništa mijenjati.  ADAL sadrži shielded aplikacije s promjenama.  

## <a name="what-are-the-changes"></a>Što su promjene?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Uklanjanje x5t vrijednost iz zaglavlja JWT
Krajnja točka 2.0 koristi JWT tokeni opsežno, koje sadrže parametara sekcije zaglavlja s odgovarajući metapodatke o token.  Ako je dekodiranje zaglavlja jednog od naše trenutni JWTs, pronaći otprilike ovako:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Gdje svojstva "x5t" i "dijete" odredite javni ključ koji želite koristiti za provjeru valjanosti u token potpis, kao što je dohvaćeni iz metapodataka krajnju točku OpenID povezivanje.

Promjena smo ovdje upućivanje je da biste uklonili svojstvo "x5t".  Možete nastaviti koristiti isti mehanizme za provjeru valjanosti tokena, ali potrebno je samo za svojstva "dijete" za dohvaćanje ispravne javni ključ, kao što je navedeno u protokol za povezivanje OpenID. 

> [AZURE.IMPORTANT] **Vaš posao: Provjerite je li aplikacije ovise o postojanje parametra x5t vrijednost.**

### <a name="removing-profileinfo"></a>Uklanjanje profile_info
Prethodno, krajnju točku 2.0 sadrži je vraćanje na objekt JSON base64 kodirani u tokena odgovora pod nazivom `profile_info`.  Prilikom traženja token za pristup od krajnje 2.0 slanjem zahtjeva za:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Sljedeći objekt JSON bi izgledao odgovor:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Na `profile_info` vrijednosti koje se nalaze informacije o korisniku prijavljenom u aplikaciju - svoje ime, ime, prezime, adresu e-pošte, identifikator i tako dalje.  Prije svega, u `profile_info` korišten za tokena predmemoriranje i prikaz svrhe.

Sada ćemo uklanjate na `profile_info` vrijednost –, ali ne brinite, ne možemo i dalje su vam informacije programerima malo drugačije mjestu.  Umjesto `profile_info`, krajnju točku 2.0 sada će se vratiti na `id_token` u svaki tokena odgovor:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Možda dekodiranje i analizirati id_token za dohvaćanje iste podatke koji vam je poslao profile_info.  U id_token je na JSON Web tokena (JWT), sa sadržajem koji određuje OpenID povezivanje.  Kod za to pa mora biti vrlo slično – jednostavno želite izdvojiti srednjem segment (tijelo) u id_token i base64 dekodiranje ga da biste pristupili JSON objekta unutar.

Putem sljedeće dva tjedna, trebali biste kod aplikacije dohvatiti podatke o korisniku s bilo kojeg na `id_token` ili `profile_info`; Ovisno o tome što je prikaz.  Na taj način prilikom promjene aplikacije možete jednostavno rukovati prijelaza s `profile_info` da biste `id_token` bez prekida.

> [AZURE.IMPORTANT] **Vaš posao: Provjerite je li aplikacija ne ovisi o postojanje parametra u `profile_info` vrijednost.**

### <a name="removing-idtokenexpiresin"></a>Uklanjanje id_token_expires_in
Slično `profile_info`, ne možemo i uklanjanje na `id_token_expires_in` parametar iz odgovore.  Prethodno, krajnju točku 2.0 će vratiti vrijednost za `id_token_expires_in` uz svaki odgovor id_token, na primjer u je odgovor ovlasti:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Ili kao tokena odgovor:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Na `id_token_expires_in` vrijednost biste označili željeni broj sekundi u id_token će ostati vrijedi za.  Sada ćemo uklanjate s `id_token_expires_in` potpuno vrijednost.  Umjesto toga možete koristiti standardne povezivanje OpenID `nbf` i `exp` zahtjeve za ispitivanje valjanost programa id_token.  Potražite u članku [2.0 tokena referenca](active-directory-v2-tokens.md) dodatne informacije na tih zahtjeva.

> [AZURE.IMPORTANT] **Vaš posao: Provjerite je li aplikacija ne ovisi o postojanje parametra u `id_token_expires_in` vrijednost.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Promjena zahtjevima vratio scope = openid
Ta promjena će se na najviše značajan – zapravo, jer će utjecati na gotovo svaki aplikacije koja koristi krajnju točku 2.0.  Mnoge aplikacije Pošalji zahtjeve za korištenje 2.0 krajnje točke na `openid` opseg, npr.:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Danas, kada korisnik daje pristanak za na `openid` opseg, pokrenite aplikaciju dobivene id_token dobiva mnoštvo informacija o korisniku.  Ove zahtjevima možete uključiti njegovo ime, preferirani korisničko ime, adresu e-pošte, ID objekta i više.

U ovom ažuriranju smo mijenjate podatke koji na `openid` opseg affords aplikaciju access, da biste bolje comform s specifikacija OpenID povezivanje.  Na `openid` opseg će samo Omogućivanje aplikacije za prijavu korisnika u te primati identifikatoru aplikacije specifične za korisnika u `sub` zahtjeva od na id_token.  Sporove u programa id_token samo na `openid` opseg odobren bit će devoid of osobne podatke.  Primjer id_token zahtjevima su:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Ako želite da biste dobili osobnu identifikaciju osobe (PII) o korisniku u aplikaciji, pokrenite aplikaciju morat ćete od korisnika tražiti dodatne dozvole.  Ne možemo su Uvod podrška za dva nova opsege iz specifikacija OpenID povezivanje – u `email` i `profile` opsege – koje omogućuju vam da biste to učinili.

- Na `email` opseg je vrlo jednostavne – omogućuje pristup korisničke adrese e-pošte primarnog putem aplikacije na `email` zahtjeva u na id_token.  Imajte na umu da se `email` zahtjeva uvijek neće postojati u id_tokens – samo bit će uključene ako je dostupna u korisničkom profilu.
- Na `profile` opseg affords pristup aplikacije za sve druge osnovne informacije o korisnika – svoje ime, preferirani korisničko ime, ID objekta i tako dalje.

To vam omogućuje da kod aplikacije na način minimalnog otkrivanja – od korisnika traži samo skup informacija koje se da se aplikacijom zahtijeva učiniti njegov posao.  Ako želite da biste nastavili početak potpunog skupa korisničke informacije koje se trenutno prima aplikacije, trebali biste uključiti sve tri opsege u zahtjevima za autorizaciju:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Pokrenite aplikaciju možete započeti slanje na `email` i `profile` opsega odmah, a krajnje 2.0 će prihvatiti ove dvije opsega i počnite zatraži dozvole korisnika po potrebi.  Međutim, promjenu tumačenja od na `openid` opseg ne primjenjuje se za nekoliko tjedana.

> [AZURE.IMPORTANT] **Vaš posao: dodavanje na `profile` i `email` opsega ako su podaci o korisniku potrebni za aplikacije.**  Imajte na umu da ADAL neće sadržavati stavku od tih dozvola u zahtjevima za prema zadanim postavkama. 

### <a name="removing-the-issuer-trailing-slash"></a>Uklanjanje izdavač završne kosa crta.
Prethodno, izdavača vrijednost koja se pojavljuje u tokena iz krajnju točku 2.0 snimljene obrasca

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Gdje je guid je tenantId Azure AD klijenta koji je izdao token.  S tim promjenama postaje vrijednost izdavač

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

u oba tokena, a u dokument za povezivanje OpenID otkrivanja.

> [AZURE.IMPORTANT] **Vaš posao: Provjerite je li aplikacije prihvaća vrijednosti izdavača s i bez kosu crtu na kraju prilikom provjere valjanosti izdavač.**

## <a name="why-change"></a>Zašto promjena?
Primarni motivation za predstavljanje te promjene je biti usklađen sa specifikacijama standardni OpenID povezivanje.  Ako je OpenID povezivanje usklađen, Nadamo da biste minimizirali razlike između Integracija s Microsoftovim servisima identiteta i s drugim servisima identiteta u na industrijskih.  Želimo da biste omogućili razvojnim inženjerima njihovim bibliotekama provjere autentičnosti omiljene Otvori izvor bez mijenjanja biblioteke kako bi odgovarao Microsoft razlike.

## <a name="what-can-you-do"></a>Što možete učiniti?
Na danas, možete početi upućivanje sve promjene na prethodno opisan.  Trebali biste odmah:

1.  **Uklonite sve ovisnosti na na `x5t` parametar zaglavlja.**
2.  **Obavljanje rukovati prijelaza s `profile_info` za `id_token` u tokena odgovore.**
3.  **Uklonite sve ovisnosti na na `id_token_expires_in` parametar odgovor.**
3.  **Dodavanje u `profile` i `email` dosezi aplikacije ako aplikacije potreban osnovni korisničke informacije.**
4.  **Prihvatite izdavač vrijednosti u tokeni s i bez kosu crtu na kraju.**

Naš [2.0 protokol dokumentaciju](active-directory-v2-protocols.md) već ažurirana u skladu s promjenama, može koristiti kao reference olakšavaju ažuriranje kod.

Ako imate dodatnih pitanja na opseg promjene, provjerite slobodno stupili nam na Twitteru na @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Kako će promjene protokol pojavljivanja?
Ne možemo foresee dodatno prekidanje mijenja protokola za provjeru autentičnosti.  Ne možemo su namjerno usnopljavanje te promjene u jedan izdanje tako da nećete morati proći kroz ove vrste postupak ažuriranja ponovno bilo kojem trenutku uskoro.  Naravno, nastavit ćemo Dodavanje značajki na servis za provjeru autentičnosti konvergira 2.0 da iskoristite prednost, ali te promjene mora biti mogu dodavati i ne prekida postojeći kod.

Na kraju željeli bismo izgovorite Zahvaljujemo na isprobavate stvari tijekom razdoblja za pretpregled.  Uvid i iskustva naš Prijevremeni adopters su neprocjenjive do sad, a Nadamo se i dalje ćete omogućiti zajedničko korištenje svoje mišljenje i ideje.

Kodiranje Sretno!

Dijeljenje Microsoft identiteta
