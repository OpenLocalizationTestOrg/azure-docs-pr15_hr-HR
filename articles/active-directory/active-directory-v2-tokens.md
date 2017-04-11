<properties
    pageTitle="Azure AD 2.0 tokena referenca | Microsoft Azure"
    description="Vrste tokeni i zahtjevima čuje po krajnju točku 2.0"
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

# <a name="v20-token-reference"></a>Referenca za tokena 2.0

Krajnja točka 2.0 emits nekoliko vrsta sigurnosnih tokena u obradi svaki [tijek provjere autentičnosti](active-directory-v2-flows.md). Ovaj dokument opisuju oblik, značajke sigurnosti i sadržaj svaku vrstu tokena.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Vrste tokena

Krajnja točka 2.0 podržava [protokol za autorizaciju OAuth 2.0](active-directory-v2-protocols.md), koje čini korištenje access_tokens i refresh_tokens.  Također podržava provjeru autentičnosti i prijavu putem [OpenID povezivanje](active-directory-v2-protocols.md#openid-connect-sign-in-flow), koji predstavlja treći vrstu tokena, u id_token.  Svaki od tih tokeni smanjivati "nošenja token".

Nošenja token je laganih sigurnosni token koja omogućuje pristup "nošenja" na zaštićenom resurs. U ovom smislu "nošenja" je bilo koji proizvođača koje je moguće izlagati token. Iako zabavu morate najprije uspješnoj Azure AD prima token nošenja ako potrebne korake ne uzimaju se sigurnost token u prijenosa i prostora za pohranu, možete se intercepted i koristi neželjeni proizvođača. Dok neke sigurnosnih tokena ima ugrađene mehanizam za sprječava neovlaštenog stranama njihova korištenja, nošenja tokeni nemaju ovaj mehanizam i mora se prenijeti u sigurnog kanala kao što je sigurnost sloja prijenosa (HTTPS). Ako nošenja token prenose se u isključite, na Muškarac u srednjem napadi mogu se zlonamjerni strana Nabava token koji ćete koristiti za neovlašten pristup resursu zaštićeni. Na isti način sigurnost primijeniti kada pohranjivanje ili predmemoriranje nošenja tokeni za kasnije korištenje. Uvijek provjerite je li aplikacije prenosi i pohranjuje nošenja tokeni siguran način. Dodatne sigurnosna pitanja vezana uz na nošenja tokena, potražite u članku [RFC 6750 odjeljak 5](http://tools.ietf.org/html/rfc6750).

Mnoge tokeni izdala krajnju točku 2.0 implementirana kao Json Web tokeni ili JWTs.  U JWT je sažetom, URL sigurnih srednje vrijednosti prijenos informacija između dvije strane.  Informacije koje se nalaze u JWTs nazivaju se "zahtjevima" ili assertions informacija o nošenja i predmet tokena.  Zahtjevima u JWTs su JSON objekti šifriraju i serijalizirani za prijenos.  Budući da JWTs izdala krajnju točku 2.0 prijavljeni, ali se ne šifrirane, možete jednostavno provjeri sadržaj na JWT za ispravljanje pogrešaka svrhe. Dodatne informacije o JWTs može se odnositi na [JWT specifikacija](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens su obrasca tokena sigurnosti za prijavu koja prima aplikacije prilikom izvršavanja provjera autentičnosti pomoću [OpenID povezivanja](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Oni su prikazane kao [JWTs](#types-of-tokens), a sadrže zahtjevima koje možete koristiti za potpisivanje korisnika u aplikaciju.  Možete koristiti na zahtjevima u programa id_token kao svojim potrebama – obično na koji se koriste za prikaz podataka o računu ili zbog odluke kontrole programa access u aplikaciju.  Krajnja točka 2.0 problemi samo jedne vrste id_token koja ima dosljedan skup zahtjevima bez obzira na vrstu korisnika koji je potpisao u.  To je izgovorite da oblik i sadržaj u id_tokens će biti iste za oba osobni Microsoftov Account korisnika i računa tvrtke ili obrazovne ustanove.

Id_tokens prijavljeni, ali se ne šifrirane trenutno.  Kada aplikacije primi programa id_token, ga morate [provjeriti potpis](#validating-tokens) da biste dokazali autentičnost na token i provjeriti nekoliko zahtjevima token da biste dokazali njezinu valjanost.  Zahtjevima valjanost aplikacije razlikuju se ovisno o scenariju preduvjeti, no postoje neke [uobičajene provjere valjanosti zahtjeva](#validating-tokens) aplikacije morate izvršiti u svakoj scenariju.

Sve detalje na zahtjevima u id_tokens navedene u nastavku, kao i ogledne id_token.  Imajte na umu da zahtjevima u id_tokens se vraćaju u nekom određenom redoslijedu.  Uz to, novi zahtjevima biti uvedene u id_tokens u bilo kojem trenutku u vremenu – aplikacije treba prelomiti kao uvode se novi zahtjevima.  Na popisu u nastavku sadrži zahtjevima koje aplikacije mogu kvalitetno tumače trenutku pisanja ovog.  Ako je potrebno, čak i detaljnije pronaći ćete u [OpenID povezivanje specifikacija](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Ogledni id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Za vježbe, pokušajte pregled zahtjevima u id_token uzorka lijepljenjem u [calebb.net](https://calebb.net).

#### <a name="claims-in-idtokens"></a>Zahtjevima u id_tokens
| Ime | Zahtjeva | Primjer vrijednost | Opis |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Ciljne skupine | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Označava je namijenjen token.  U id_tokens, gledatelji je Id aplikacije za aplikaciju programa, kao što je dodijeljen aplikacije na portalu za registraciju aplikacije.  Aplikaciju treba provjeriti tu vrijednost i odbacivanje token ako ne odgovara. |
| Izdavač | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Služi za identifikaciju servis sigurnosnih tokena (STS) koji konstrukcije i vraća token, kao i Azure AD klijenta u kojem se autentičnost korisnika.  Pokrenite aplikaciju bi trebali provjeriti izdavača zatraži da biste bili sigurni da se token dolazi od krajnje 2.0.  Ga, trebali biste koristiti guid dio zahtjeva za ograničavanje skup klijenata dopušteni da biste se prijavili u aplikaciju.  Guid koji označava korisnik je korisnik korisničke od Microsofta je račun `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Izdana u | `iat` | `1452285331` | Vrijeme u kojem se token izdavanja predstavljeni u vremenu epoch. |
| Vrijeme isteka | `exp` | `1452289231` | Vrijeme u kojem postaje koji nisu valjani, token predstavljeni u vremenu epoch.  Pokrenite aplikaciju trebali biste koristiti ovaj zahtjeva da biste provjerili valjanost tokena života.  |
| Ne prije | `nbf` | `1452285331` |  Vrijeme u kojem postaje valjana, token predstavljeni u vremenu epoch. To je obično jednaki vremena izdavanja.  Pokrenite aplikaciju trebali biste koristiti ovaj zahtjeva da biste provjerili valjanost tokena života.  |
| Verzija | `ver` | `2.0` | Verzija id_token, kako je definirano Azure AD.  Za krajnju točku 2.0 će se vrijednost `2.0`. |
| Id klijenta | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Guid koji predstavlja Azure AD klijenta koji je korisnik iz.  Tvrtke i obrazovne ustanove za račune za guid bit će immutable klijentu ID tvrtke ili ustanove kojoj pripada korisnika.  Za osobni računi će se vrijednost `9188040d-6c67-4c5b-b112-36a304b66dad`.  Na `profile` opseg je potrebna lozinka primanje ovaj zahtjev. |
| Raspršivanje kod | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Raspršivanje kod je sve obuhvaćeno id_tokens samo kada u id_token izdaje duž je OAuth 2.0 autorizacije kod.  Može se koristiti za provjeru autentičnosti i pripadnog koda autorizacije.  Potražite u članku [Povezivanje OpenID specifikacija](http://openid.net/specs/openid-connect-core-1_0.html) detalje na izvođenje ovog provjere valjanosti. |
| Pristup tokena raspršivanje | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Raspršivanje tokena pristup je sve obuhvaćeno id_tokens samo kada u id_token izdaje duž OAuth 2.0 pristupni token.  Može se koristiti za provjeru autentičnosti token za pristup.  Potražite u članku [Povezivanje OpenID specifikacija](http://openid.net/specs/openid-connect-core-1_0.html) detalje na izvođenje ovog provjere valjanosti. |
| Jednokratna šifra | `nonce` | `12345` | Na Jednokratna šifra je Strategije za mitigating napadi tokena Ponovi.  Pokrenite aplikaciju možete odrediti na Jednokratna šifra u zahtjev za autorizaciju pomoću na `nonce` parametarski upit.  Vrijednost unesete u zahtjevu za će biti čuje u u id_token `nonce` nepromijenjenu zahtjeva.  Time se omogućuje aplikacije da biste provjerili vrijednost u odnosu navedena na zahtjev aplikacije sesiju pridružuje navedeni id_token vrijednost.  Pokrenite aplikaciju provoditi ovaj provjere valjanosti tijekom postupka id_token provjere valjanosti. |
| Ime | `name` | `Babe Ruth` | Naziv zahtjeva nudi Ljudski čitljiv vrijednost koja određuje predmet token. Ta vrijednost se zajamčeno biti jedinstvena, je mutable, a namijenjen samo za potrebe prikaza.  Na `profile` opseg je potrebna lozinka primanje ovaj zahtjev. |
| E-pošte | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Adresu e-pošte primarni je pridružen korisnički račun ako postoji.  Vrijednost je mutable i mijenjaju za određene korisnike tijekom vremena.  Na `email` opseg je potrebna lozinka primanje ovaj zahtjev. |
| Preferirani korisničko ime | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Primarni korisničko ime koje predstavljaju korisnika u krajnju točku 2.0.  Može biti adresu e-pošte, telefonski broj ili generički korisničko ime bez navedenom obliku.  Vrijednost je mutable i mijenjaju za određene korisnike tijekom vremena.  Na `profile` opseg je potrebna lozinka primanje ovaj zahtjev. |
| Predmet | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Glavnica o token nametanja podatke, kao što su korisnički aplikacije. Ta vrijednost je immutable i ne može se ponovno ili ponovno koristiti, pa ga mogu se izvršiti provjeru autorizacije sigurno, primjerice kada se token koristi da biste pristupili resurs. Budući da je predmet uvijek su prisutne u tokeni Azure AD problema, preporučujemo da se pomoću tu vrijednost u sustavu za autorizaciju opće namjene. |
| ID objekta | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Objekt Id računa tvrtke ili obrazovne ustanove u sustavu za Azure AD.  U ovom zahtjeva će izdati osobnog Microsoftova računa.  Na `profile` opseg je potrebna lozinka primanje ovaj zahtjev. |


## <a name="access-tokens"></a>Pristupna tokena

Pristupna tokena izdala krajnju točku 2.0 su samo potrošni Microsoft Services tom trenutku u trenutku.  Aplikacija ne treba potrebne za izvođenje provjere valjanosti ni provjere pristupna tokena za bilo koji od trenutno podržanih scenarija.  Pristupna tokena možete tretira kao potpuno neprozirne – su samo nizovi koji aplikacije u zahtjevima za HTTP prenositi Microsoftu.

U skorijoj budućnosti krajnju točku 2.0 će predstavljanje mogućnost aplikacije prima pristupna tokena iz drugih klijenata.  Trenutku, ti podaci će se ažurirati informacijama aplikacije mora poduzeti tokena provjere valjanosti u programu access i druge slične zadatke.

Kada token za pristup zatražite od krajnje 2.0, krajnju točku 2.0 vraća neke metapodatke o token za pristup za potrošnju pokrenite aplikaciju.  Ti podaci obuhvaćaju vrijeme isteka token za pristup i opsega za koje je valjan.  Time se omogućuje aplikacije da biste izvršili Inteligentna predmemoriranje pristupna tokena bez potrebe za raščlaniti Otvori token za pristup sam.

## <a name="refresh-tokens"></a>Osvježavanje tokena

Osvježavanje tokeni su sigurnosnih tokena kojih aplikacije mogu dobiti novi pristup tokeni u tijeku je OAuth 2.0.  Omogućuje aplikacije da biste postigli Dugoročne pristup resursima ime korisnika bez interakcija korisnika.

Osvježavanje tokeni su više resursa.  Što znači da osvježavanje token primljene tokena zahtjev za jedan resurs može biti aktivacije za tokeni pristup resursu potpuno drugačije.

Da biste dobili osvježavanja tokena odgovor, pokrenite aplikaciju morate zatražiti & biti odobren na `offline_acesss` opseg.   Da biste saznali više o na `offline_access` opseg, pogledajte [u nastavku članka pristanak & opsega](active-directory-v2-scopes.md).

Osvježavanje tokeni su pa će se odnositi, potpuno neprozirne aplikacije.  Oni su izdala 2.0 krajnje Azure AD i možete samo se provjerava ima li & protumačiti krajnju točku 2.0.  Su long-lived, ali moraju biti napisani aplikacije možete očekivati da će osvježavanja token zadnji sve određenog vremenskog razdoblja.  Tokeni osvježavanja može biti invalidated u bilo kojem trenutku vrijeme iz nekog razloga.  Jedini način za aplikaciju znati osvježavanja token vrijedi jest da biste iskoristili tako da tokena zahtjev za krajnju točku 2.0.

Kada iskoristili osvježavanja token novi token pristup (i ako je dodijeljen aplikacije na `offline_access` opseg), dobit ćete novi token osvježavanja tokena odgovor.  Spremite token upravo Izdana osvježavanja zamjene onaj koji ste koristili u zahtjevu za.  To će jamči da ostanu token za osvježavanje vrijedi za dugo moguće.

## <a name="validating-tokens"></a>Provjera valjanosti tokena

Tom trenutku u trenutku, samo tokena provjere valjanosti aplikacija treba potrebne za izvođenje je provjera valjanosti id_tokens.  Da biste provjerili valjanost programa id_token, aplikacije bi trebali provjeriti potpis na id_token i zahtjevima u na id_token.

<!-- TODO: Link -->
Nudimo biblioteke i primjere koda koji pokazuju kako jednostavno rukovati tokena provjere valjanosti - u ispod informacije samo navedeni su za osobe koje želite da biste shvatili postupak podlozi.  Postoje i nekoliko 3 strana Otvori izvor biblioteke dostupan za provjeru valjanosti JWT – postoji najmanje jednu mogućnost gotovo svaki platforme i jezika nudi.

#### <a name="validating-the-signature"></a>Provjera valjanosti potpisa
Na JWT sadrži tri segmenti koje su odvojene na `.` znakova.  Prvi segment zove **Zaglavlje**drugi kao **tijela**i treću kao **potpis**.  Segment potpis može se koristiti za provjeru autentičnosti u id_token tako da ga možete moraju vjerovati aplikacije.

Id_Tokens prijavljeni pomoću algoritama standardne asimetričnim šifriranja industrijskih, kao što su RSA 256. U zaglavlju na id_token sadrži informacije o način ključ i šifriranje koji se koristi za potpisivanje tokena:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Na `alg` zahtjeva označava algoritam koji je korišten za potpisivanje tokena, dok se `kid` zahtjeva označava određeni javni ključ koji je korišten za potpisivanje tokena.

U bilo kojem trenutku zadanog vremena, krajnju točku 2.0 potpisati programa id_token u nekom od određenog skupa javno privatni ključ parove.  Krajnja točka 2.0 zakreće mogući skup tipke na periodičnim temelj, tako da se aplikacijom moraju biti napisani automatski rukovati promjenama ključa.  Je pametnije učestalost da biste provjerili postoje li ažuriranja za javni ključ koristi krajnju točku 2.0 24 sata.

Možete nabaviti potpisivanja ključne podatke potrebne za provjeru valjanosti potpis pomoću dokumenta metapodataka OpenID povezivanje koji se nalazi na:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Pokušajte ovaj URL u pregledniku!

Ovaj dokument metapodataka je JSON objekt koji sadrži nekoliko dijelova korisne informacije, kao što su mjesto različite krajnje točke potrebne za izvođenje OpenID povezivanje provjere autentičnosti.  

Uključuje i na `jwks_uri`, koji pruža mjesto skup javni ključ koji se koristi za potpisivanje tokena.  Dokument JSON nalazi se na u `jwks_uri` sadrži sve javno važne informacije u koristi trenutku tu određenu.  Možete koristiti aplikaciju na `kid` zahtjeva u zaglavlju JWT da biste odabrali koje javni ključ u ovom dokumentu korišten da biste se prijavili određeni token.  Zatim možete izvršiti pomoću odgovarajuće javnim ključem i navedeni algoritam provjere valjanosti potpisa.

Izvođenje provjere valjanosti za potpis je izvan opsega ovog dokumenta – nema dostupnih za učiniti ako je potrebno bibliotekama s mnogo Otvori izvor.

#### <a name="validating-the-claims"></a>Provjera valjanosti na zahtjevima
Kada aplikacije primi programa id_token nakon korisničku prijavu, ga i provoditi nekoliko provjere u odnosu na zahtjevima u na id_token.  Ona obuhvaćaju, ali nije ograničena su:

- Zahtjev **publike** – da biste potvrdili da se id_token je namijenjen da biste dobili aplikacije.
- U **Ne prije** i **Vrijeme isteka** zahtjevima – da biste provjerili je li na id_token nije istekao.
- Zahtjev **izdavač** – da biste potvrdili da token uistinu izdala u aplikaciju za krajnju točku 2.0.
- Na **Jednokratna šifra** - kao ublažiti za napada tokena Ponovi.
- i mnoge druge...

Cijeli popis provjere valjanosti zahtjeva provoditi aplikacije, potražite [OpenID povezivanje specifikacija](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Detalje o očekivanim vrijednostima te zahtjevima uključene su iznad u [id_token sekciju](#id_tokens).


## <a name="token-lifetimes"></a>Tokena vijekom trajanja

Sljedeće tokena vijekom trajanja se daju isključivo za razumijevanje, kao što su mogu olakšati razvoja i ispravljanje pogrešaka aplikacije.  Aplikacija moraju biti napisani očekivati neki od ovih vijekom trajanja da biste ostaju iste – mogu i će promijeniti u bilo kojem trenutku navedene u vremenu.

| Tokena | Života. | Opis |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (računi tvrtke ili obrazovne ustanove) | 1 sat | Id_Tokens vrijede obično za jedan sat.  Web-aplikaciju programa možete koristiti ovaj isti vrijeme upotrebljivosti u održavanje vlastitu sesije s korisnikom (preporučeno) ili odaberite potpuno drugačije sesiju života.  Ako je potrebno aplikacije da biste dobili novi id_token, jednostavno potreban da bi novi prijavu zahtjev za na 2.0 autorizirali krajnjoj točki.  Ako korisnik ima sesije valjani preglednika s krajnju točku 2.0, oni možda neće morati ponovno unositi vjerodajnice. |
| Id_Tokens (osobni računi) | 24 sata | Id_Tokens za osobne račune vrijede obično 24 sata.  Web-aplikaciju programa možete koristiti ovaj isti vrijeme upotrebljivosti u održavanje vlastitu sesije s korisnikom (preporučeno) ili odaberite potpuno drugačije sesiju života.  Ako je potrebno aplikacije da biste dobili novi id_token, jednostavno potreban da bi novi prijavu zahtjev za na 2.0 autorizirali krajnjoj točki.  Ako korisnik ima sesije valjani preglednika s krajnju točku 2.0, oni možda neće morati ponovno unositi vjerodajnice. |
| Pristupna tokena (računi tvrtke ili obrazovne ustanove) | 1 sat | Naznačen u tokena odgovore u sklopu tokena metapodataka. |
| Pristupna tokena (osobni računi) | 1 sat | Naznačen u tokena odgovore u sklopu tokena metapodataka.  Access_tokens izdavanja ime osobni računi možda konfigurirana za različite života, ali jedan sat obično je to slučaj. |
| Osvježavanje tokeni (račun tvrtke ili obrazovne ustanove) | Do 14 dana | Jedan osvježavanja token vrijedi za najviše 14 dana.  Međutim, token osvježavanja može postati koji nisu valjani u bilo kojem trenutku za bilo koji broj razloga, tako da se aplikacijom nastaviti da biste pokušali pomoću tokena Osvježi sve dok se ne uspijeva ili dok aplikacije zamjenjuje s novi token osvježavanja.  Osvježavanje token također može prouzročiti nevaljanost ako je prošlo 90 dana korisnik unese vjerodajnica. |
| Osvježavanje tokeni (osobni računi) | Do 1 godine | Jedan osvježavanja token vrijedi za najviše godinu.  Međutim, token osvježavanja može postati koji nisu valjani u bilo kojem trenutku za bilo koji broj razloga, tako da se aplikacijom nastaviti da biste pokušali pomoću tokena Osvježi sve dok se ne uspijeva. |
| Kodovi za autorizaciju (računi tvrtke ili obrazovne ustanove) | 10 minuta | Kodovi za autorizaciju su namjerno prekinuto short-lived i mora biti odmah aktivacije za access_tokens i refresh_tokens po primitku. |
| Kodovi za autorizaciju (osobni računi) | pet minuta | Kodovi za autorizaciju su namjerno prekinuto short-lived i mora biti odmah aktivacije za access_tokens i refresh_tokens po primitku.  Kodovi autorizacije izdavanja ime osobni računi su i jednokratno korištenje. |
