 <properties
   pageTitle="Referenca za Azure AD Token | Microsoft Azure"
   description="Vodič za razumijevanje i procjenu zahtjevima u SAML 2.0 i tokeni JSON Web (JWT) tokeni izdavanja po Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD tokena reference

Azure Active Directory (Azure AD) emits nekoliko vrsta sigurnosnih tokena u obradi svaki protok provjere autentičnosti. Ovaj dokument opisuju oblik, značajke sigurnosti i sadržaj svaku vrstu tokena.

## <a name="types-of-tokens"></a>Vrste tokena

Azure AD podržava [protokol za autorizaciju OAuth 2.0](active-directory-protocols-oauth-code.md), koje čini korištenje access_tokens i refresh_tokens.  Također podržava provjeru autentičnosti i prijavu putem [OpenID povezivanje](active-directory-protocols-openid-connect-code.md), koji predstavlja treći vrstu tokena, u id_token.  Svaki od tih tokeni smanjivati "nošenja token".

Nošenja token je laganih sigurnosni token koja omogućuje pristup "nošenja" na zaštićenom resurs. U ovom smislu "nošenja" je bilo koji proizvođača koje je moguće izlagati token. Iako da biste dobili nošenja token potrebna je provjera autentičnosti s Azure AD, korake, potrebno je poduzeti radi zaštite token da biste spriječili presretanja neželjeni strana. Budući da nošenja tokeni postoje ugrađene mehanizam da biste spriječili neovlašteni stranama njihova korištenja, morate se prenijeti u sigurnog kanala kao što je sigurnost sloja prijenosa (HTTPS). Ako nošenja token prenose se u isključite, na Muškarac u srednjem napadi mogu se Nabava token i dobiti neovlaštenog pristupa zaštićenim resursa. Na isti način sigurnost primijeniti kada pohranjivanje ili predmemoriranje nošenja tokeni za kasnije korištenje. Uvijek provjerite je li aplikacije prenosi i pohranjuje nošenja tokeni siguran način. Dodatne sigurnosna pitanja vezana uz na nošenja tokena, potražite u članku [RFC 6750 odjeljak 5](http://tools.ietf.org/html/rfc6750).

Mnoge tokena koje izdaje Azure AD implementirana kao JSON Web tokeni ili JWTs.  U JWT je sažetom, URL sigurnih srednje vrijednosti prijenos informacija između dvije strane.  Informacije koje se nalaze u JWTs nazivaju se "zahtjevima" ili assertions informacija o nošenja i predmet tokena.  Zahtjevima u JWTs su JSON objekti šifriraju i serijalizirani za prijenos.  Budući da JWTs izdala Azure AD prijavljeni, ali se ne šifrirane, možete jednostavno provjeri sadržaj na JWT za ispravljanje pogrešaka svrhe.  Postoji nekoliko alata dostupni su za to učinite, kao što su [jwt.calebb.net](http://jwt.calebb.net). Dodatne informacije o JWTs može se odnositi na [JWT specifikacija](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens su obrasca tokena sigurnosti za prijavu koja prima aplikacije prilikom izvršavanja provjera autentičnosti pomoću [OpenID povezivanja](active-directory-protocols-openid-connect-code.md).  Oni su prikazane kao [JWTs](#types-of-tokens), a sadrže zahtjevima koje možete koristiti za potpisivanje korisnika u aplikaciju.  Možete koristiti na zahtjevima u programa id_token kao svojim potrebama – obično na koji se koriste za prikaz podataka o računu ili zbog odluke kontrole programa access u aplikaciju.

Id_tokens prijavljeni, ali se ne šifrirane trenutno.  Kada aplikacije primi programa id_token, ga morate [provjeriti potpis](#validating-tokens) da biste dokazali autentičnost na token i provjeriti nekoliko zahtjevima token da biste dokazali njezinu valjanost.  Zahtjevima valjanost aplikacije razlikuju se ovisno o scenariju preduvjeti, no postoje neke [uobičajene provjere valjanosti zahtjeva](#validating-tokens) aplikacije morate izvršiti u svakoj scenariju.

U odjeljku sljedeće informacije na zahtjevima id_tokens, kao i ogledne id_token.  Imajte na umu da zahtjevima u id_tokens se vraćaju u nekom određenom redoslijedu.  Uz to, novi zahtjevima biti uvedene u id_tokens u bilo kojem trenutku u vremenu – aplikacije treba prelomiti kao uvode se novi zahtjevima.  Sljedeći popis sadrži zahtjevima koje aplikacije mogu kvalitetno tumače trenutku pisanja ovog.  Ako je potrebno, čak i detaljnije pronaći ćete u [OpenID povezivanje specifikacija](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Ogledni id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Za vježbe, pokušajte pregled zahtjevima u id_token uzorka lijepljenjem u [calebb.net](http://jwt.calebb.net).

#### <a name="claims-in-idtokens"></a>Zahtjevima u id_tokens

| JWT zahtjeva | Ime | Opis |
|-----------|------|-------------|
| `appid`| ID aplikacije | Služi za identifikaciju aplikacija koja koristi token za pristup resurs. Aplikaciju možete poslužiti kao sam ili ime korisnika. ID aplikacije obično predstavlja objekt aplikacija, ali ga možete predstavljati objekt za glavni servisa u Azure AD. <br><br> **Primjer JWT vrijednosti**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Ciljne skupine | Svrhu primatelj token. U aplikaciji koja prima token morate provjeriti vrijednost ciljne skupine točan i Odbaci sve tokeni namijenjen nekoj drugoj publici. <br><br> **Primjer SAML vrijednosti**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Primjer JWT vrijednosti**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Aplikacija provjere autentičnosti kontekst predmete Reference | Upućuje na to koliko je autentičnost provjerena klijent. Za javne klijent, je vrijednost 0. Ako se koriste ID klijenta i tajna klijent, vrijednost je 1. <br><br> **Primjer JWT vrijednosti**: <br> `"appidacr": "0"`|
| `acr`| Provjera autentičnosti kontekst predmete Reference | Upućuje na to koliko je autentičnost predmet umjesto klijenta u zahtjeva za aplikaciju provjere autentičnosti kontekst predmete referenca. Vrijednost "0" označava provjere autentičnosti za krajnjeg korisnika ne zadovoljava preduvjete ISO IEC 29115. <br><br> **Primjer JWT vrijednosti**: <br> `"acr": "0"`|
| | Provjera autentičnosti trenutnih | Snima datum i vrijeme kada došlo je do provjere autentičnosti. <br><br> **Primjer SAML vrijednosti**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Način provjere autentičnosti | Određuje koliko je autentičnost provjerena predmet token. <br><br> **Primjer SAML vrijednosti**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Primjer JWT vrijednosti**:`“amr”: ["pwd"]` |
| `given_name`| Ime | Omogućuje prve ili "dan" ime korisnika, kao što je postavljeno na korisničkom objektu Azure AD. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `"given_name": "Frank"` |
| `groups`| Grupa | Sadrži ID-ova objekta koji predstavljaju članstva u grupi u predmetu. Ove vrijednosti su jedinstvene (pogledajte ID objekta), a mogu sigurno koristiti za upravljanje pristupom, kao što su provoditi autorizacije da biste pristupili resursa. Grupa obuhvaćeno zahtjeva grupe konfigurirane na temelju po aplikacije, kroz svojstvo "groupMembershipClaims" manifesta aplikacije. Vrijednost null će isključiti sve grupe, vrijednost "SecurityGroup" neće sadržavati samo članstva u grupi sigurnost imenika Active Directory i vrijednost "Sve" će sadržavati i sigurnosne grupe i popisi za Office 365 raspodjelu. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Davatelja identiteta | Snima davatelja identiteta čija je autentičnost provjerena predmet token. Tu vrijednost jednaka vrijednosti zahtjeva izdavač je osim ako je korisnički račun u klijentu za različite od izdavač. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Sprema vremena izdavanja token. Često se koristi za mjerenje tokena svježina. <br><br> **Primjer SAML vrijednosti**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Primjer JWT vrijednosti**: <br> `"iat": 1390234181` |
| `iss` | Izdavač | Služi za identifikaciju servis sigurnosnih tokena (STS) koji konstrukcije i vraća token. Tokeni koje vraća Azure AD izdavač je sts.windows.net. GUID u vrijednost zahtjeva izdavač je ID klijenta Azure AD direktorija. ID klijenta je identifikator immutable i pouzdane direktorija. <br><br> **Primjer SAML vrijednosti**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Primjer JWT vrijednosti**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Prezime | Kako je definirano u korisničkom objektu Azure AD nudi posljednje ime, prezime ili prezime korisnika. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `"family_name": "Miller"` |
| `unique_name`| Ime | Nudi Ljudski čitljiv vrijednost koja određuje predmet token. Ta vrijednost je uvijek biti jedinstveno unutar klijenta i namijenjen samo za potrebe prikaza. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | ID objekta | Jedinstveni identifikator objekta sadrži u Azure AD. Ova vrijednost je immutable i ne mogu ponovno dodijeliti ili ponovno iskoristiti. Prepoznavanje objekta u upitima za Azure AD pomoću ID objekta. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Uloge | Predstavlja sve aplikacije uloge koje predmet odobren i izravno i neizravno putem članstva u grupi i može se koristiti za nametanje kontrola pristupa na temelju uloga. Uloge aplikacije definiraju na temelju web-aplikacijama kroz na `appRoles` svojstvo manifesta aplikacije. Na `value` svojstvo ulogama aplikacija je vrijednost koja se pojavljuje u zahtjeva uloge. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Opseg | Označava oponašanja pravo na klijentskoj aplikaciji. Zadane dozvole `user_impersonation`. Vlasnik zaštićenim resursa možete registrirati dodatne vrijednosti u Azure AD. <br><br> **Primjer JWT vrijednosti**: <br> `"scp": "user_impersonation"`|
| `sub` |Predmet| Služi za identifikaciju glavnicu o token nametanja podatke, kao što su korisnički aplikacije. Ta vrijednost je immutable i ne može se ponovno ili ponovno koristiti, pa ga može se koristiti za sigurno izvršiti provjeru za autorizaciju. Budući da je predmet uvijek su prisutne u tokeni Azure AD problema, preporučujemo da se pomoću tu vrijednost u sustavu za autorizaciju opće namjene. <br> `SubjectConfirmation`nije li zahtjev. Opisuje kako je potvrđena predmet token. `Bearer`upućuje na to da se predmet potvrđuje prema svojim ima tokena. <br><br> **Primjer SAML vrijednosti**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Primjer JWT vrijednosti**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | ID klijenta | Immutable, koji nisu ponovno iskoristiv identifikatora koja služi za identifikaciju klijentu direktorija koja je izdala token. Tu vrijednost možete koristiti da biste pristupili resursa direktorija specifične za klijenta u više klijentske aplikacije. Ako, na primjer, možete koristiti tu vrijednost za prepoznavanje klijenta u poziv na grafikonu API-JA. <br><br> **Primjer SAML vrijednosti**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Primjer JWT vrijednosti**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Tokena života. | Definira razdoblje u kojem je valjan token. Servis koji provjerava token trebali biste provjerite je li trenutni datum unutar na tokena vijek, još ga treba odbacivanje token. Servis možda dopustiti do pet minuta izvan raspona tokena vijek na račun za sve razlike u sat vremena ("skew vrijeme") između Azure AD i usluge. <br><br> **Primjer SAML vrijednosti**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Primjer JWT vrijednosti**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Korisnikovo Glavno ime | Sprema korisničko ime upravitelja korisnika.<br><br> **Primjer JWT vrijednosti**: <br> `"upn": frankm@contoso.com`|
| `ver`| Verzija | Pohranjuje broj verzije tokena. <br><br> **Primjer JWT vrijednosti**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Pristupna tokena

Pristupna tokena su samo potrošni Microsoft Services tom trenutku u trenutku.  Aplikacija ne treba potrebne za izvođenje provjere valjanosti ni provjere pristupna tokena za bilo koji od trenutno podržanih scenarija.  Pristupna tokena možete tretira kao potpuno neprozirne – su samo nizovi koji aplikacije u zahtjevima za HTTP prenositi Microsoftu.

Kada zatražite token za pristup Azure AD vraća neke metapodatke o token za pristup za potrošnju pokrenite aplikaciju.  Ti podaci obuhvaćaju vrijeme isteka token za pristup i opsega za koje je valjan.  Time se omogućuje aplikacije da biste izvršili Inteligentna predmemoriranje pristupna tokena bez potrebe za raščlaniti Otvori token za pristup sam.

## <a name="refresh-tokens"></a>Osvježavanje tokena

Osvježavanje tokeni su sigurnosnih tokena kojih aplikacije mogu dobiti novi pristupna tokena u tijeku je OAuth 2.0.  Omogućuje aplikacije da biste postigli Dugoročne pristupa resursima ime korisnika bez interakcija korisnika.

Osvježavanje tokeni su više resursa, što znači da se možda biti primljene tokena zahtjev za jedan resurs, ali aktivacije za tokeni pristup resursu potpuno drugačije. Da biste naveli više resursa, postavite na `resource` parametar u zahtjev za ciljano resursa.

Osvježavanje tokeni su potpuno neprozirne aplikacije. Su long-lived, ali moraju biti napisani aplikacije možete očekivati da će osvježavanja token zadnji sve određenog vremenskog razdoblja.  Tokeni osvježavanja može biti invalidated u bilo kojem trenutku vrijeme iz nekog razloga.  Jedini način za aplikaciju znati osvježavanja token vrijedi jest da biste iskoristili tako da tokena zahtjev za Azure AD tokena krajnjoj točki.

Kada iskoristili osvježavanja token novi token pristupa, dobit ćete novi token osvježavanja tokena odgovor.  Spremite token upravo Izdana osvježavanja zamjene onaj koji ste koristili u zahtjevu za.  To će jamči da ostanu token za osvježavanje vrijedi za dugo moguće.

## <a name="validating-tokens"></a>Provjera valjanosti tokena

Tom trenutku u trenutku, samo tokena provjere valjanosti aplikacije klijenta mora potrebne za izvođenje je provjera valjanosti id_tokens.  Da biste provjerili valjanost programa id_token, aplikacije bi trebali provjeriti potpis na id_token i zahtjevima u na id_token.

Dajemo biblioteke i primjere koda koji pokazuju kako jednostavno rukovati tokena provjere valjanosti, ako želite da biste shvatili postupak podlozi.  Postoje i nekoliko drugih proizvođača Otvori izvor biblioteke dostupan za provjeru valjanosti JWT, najmanje jednu mogućnost gotovo svaki platforme i jezika. Dodatne informacije o bibliotekama Azure AD provjere autentičnosti i primjere koda, pročitajte članak [Azure AD provjera autentičnosti biblioteke](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Provjera valjanosti potpisa

Na JWT sadrži tri segmenti koje su odvojene na `.` znakova.  Prvi segment zove **Zaglavlje**drugi kao **tijela**i treću kao **potpis**.  Segment potpis može se koristiti za provjeru autentičnosti u id_token tako da ga možete moraju vjerovati aplikacije.

Id_Tokens prijavljeni pomoću algoritama standardne asimetričnim šifriranja industrijskih, kao što su RSA 256. U zaglavlju na id_token sadrži informacije o način ključ i šifriranje koji se koristi za potpisivanje tokena:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Na `alg` zahtjeva označava algoritam koji je korišten za potpisivanje tokena, dok se `x5t` zahtjeva označava određeni javni ključ koji je korišten za potpisivanje tokena.

U bilo kojem trenutku zadanog vremena, Azure AD potpisati programa id_token u nekom od određenog skupa javno privatni ključ parove. Azure AD zakreće mogući skup tipke na periodičnim temelj, tako da se aplikacijom moraju biti napisani automatski rukovati promjenama ključa.  Pametnije učestalost da biste provjerili postoje li ažuriranja za javni ključ koristi Azure AD je svaka 24 sata.

Možete nabaviti potpisivanja ključne podatke potrebne za provjeru valjanosti potpis pomoću dokumenta metapodataka OpenID povezivanje koji se nalazi na:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Pokušajte ovaj URL u pregledniku!

Ovaj dokument metapodataka je JSON objekt koji sadrži nekoliko dijelova korisne informacije, kao što su mjesto različite krajnje točke potrebne za izvođenje OpenID povezivanje provjere autentičnosti.  

Uključuje i na `jwks_uri`, koji pruža mjesto skup javni ključ koji se koristi za potpisivanje tokena.  Dokument JSON nalazi se na u `jwks_uri` sadrži sve javno važne informacije u koristi trenutku taj određeni.  Možete koristiti aplikaciju na `kid` zahtjeva u zaglavlju JWT da biste odabrali koje javni ključ u ovom dokumentu korišten da biste se prijavili određeni token.  Zatim možete izvršiti pomoću odgovarajuće javnim ključem i navedeni algoritam provjere valjanosti potpisa.

Izvođenje provjere valjanosti za potpis je izvan opsega ovog dokumenta – nema dostupnih za učiniti ako je potrebno bibliotekama s mnogo Otvori izvor.

#### <a name="validating-the-claims"></a>Provjera valjanosti na zahtjevima

Kada aplikacije primi programa id_token nakon korisničku prijavu, ga i provoditi nekoliko provjere u odnosu na zahtjevima u na id_token.  Ona obuhvaćaju, ali nije ograničena su:

  - Zahtjev **publike** – da biste potvrdili da se id_token je namijenjen da biste dobili aplikacije.
  - U **Ne prije** i **Vrijeme isteka** zahtjevima – da biste provjerili je li na id_token nije istekao.
  - Zahtjev **izdavač** – da biste potvrdili da token uistinu izdala u aplikaciju za Azure AD.
  - Na **Jednokratna šifra** – da biste smanjili tokena Ponovi napada.
  - i mnoge druge...

Potpuni popis zahtjeva provjere aplikacije provoditi priručniku [Povezivanje OpenID specifikacija](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Očekivani vrijednosti za ove zahtjevima detalje potražite u prethodnom odjeljku [id_token sekciju](#id-tokens) .

## <a name="sample-tokens"></a>Ogledna tokena

U ovom se odjeljku prikazuje primjere SAML i JWT tokena koje vraća Azure AD. Ta uzorka omogućuju potražite u članku zahtjevima u kontekstu.
SAML tokena

Ovo je primjer tipičnog SAML tokena.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT Token - Oponašanje korisnika

Ovo je uzorak standardne JSON web token (JWT) koristi u tijeku za grant kod za autorizaciju.
Osim zahtjevima, token sadrži broj verzije u **ver** i **appidacr**, kontekst provjere autentičnosti predmete reference koji pokazuje kako je autentičnost provjerena klijent. Za javne klijent, je vrijednost 0. Ako je korišten ID klijenta ili tajna klijent, vrijednost je 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Srodni sadržaji
- Potražite u članku Azure AD grafikonu [operacije pravila](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) i [pravila entitet](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), dodatne informacije o upravljanju tokena vijek pravila putem API Azure AD grafikonu.
- Dodatne informacije i na upravljanje pravilima putem cmdleta ljuske PowerShell, uključujući uzorka, potražite u članku [konfigurirati tokena vijekom trajanja u Azure AD](active-directory-configurable-token-lifetimes.md). 
