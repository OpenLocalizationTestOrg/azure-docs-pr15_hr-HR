
<properties
   pageTitle="Provjera autentičnosti scenariji za Azure AD | Microsoft Azure"
   description="Pregled pet Najčešći provjere autentičnosti scenariji za Azure Active Directory (AAD)"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Provjera autentičnosti scenariji za Azure AD

Azure Active Directory (Azure AD) pojednostavljuje provjere autentičnosti za razvojne inženjere unosom identitet kao izvor servisa s podrškom za standardni protokole kao što su OAuth 2.0 i povezivanje OpenID, kao i otvaranje biblioteka za različite platforme za početak kodiranje brzo. Ovaj dokument će vam olakšati razumijevanje različitim scenarijima Azure AD podržava i vidjet ćete kako započeti rad. Dijeli na sljedeće odjeljke:

- [Osnove provjere autentičnosti u Azure AD](#basics-of-authentication-in-azure-ad)

- [Sporove u Azure AD sigurnosnih tokena](#claims-in-azure-ad-security-tokens)

- [Osnove Registracija aplikacije u Azure AD](#basics-of-registering-an-application-in-azure-ad)

- [Vrsta aplikacija i scenarijima](#application-types-and-scenarios)

  - [Web-preglednika u web-aplikaciju](#web-browser-to-web-application)

  - [Jedna stranica aplikacije (SPA)](#single-page-application-spa)

  - [Izvorna aplikacija za API na webu](#native-application-to-web-api)

  - [Web-aplikaciju za API na webu](#web-application-to-web-api)

  - [Daemon ili poslužiteljsku aplikaciju za API na webu](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Osnove provjere autentičnosti u Azure AD

Ako niste upoznati s osnovni koncepti provjere autentičnosti u Azure AD, pročitajte ovaj odjeljak. U suprotnom, preporučujemo vam da biste preskočili prema dolje do [Vrsta aplikacija i scenarijima](#application-types-and-scenarios).

Pogledajmo zamislite osnovni kojima je potrebno identiteta: korisnik u web-pregledniku treba za provjeru autentičnosti web-aplikacije. Ovaj scenarij je opisan u nešto podrobnije u odjeljku [Web-preglednika u web-aplikaciju](#web-browser-to-web-application) , ali je korisno početnu točku da biste ilustrirali mogućnosti Azure AD i conceptualize funkcioniranje scenarija. Na sljedećem su dijagramu za taj scenarij Imajte na umu:

![Pregled jedinstvene prijave na web-aplikacije](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

S dijagramom iznad je imati na umu, Evo što trebate znati o njegove razne komponente:

- Azure AD je davatelja identiteta, dužni potvrđivanja identiteta korisnicima i aplikacijama koje postoje u tvrtki ili ustanovi direktorija i konačni izdavanja sigurnosnih tokena nakon uspješne provjera autentičnosti tim korisnicima i aplikacijama.


- Aplikacije sustava želi spoljni provjere autentičnosti za Azure AD mora biti registriran u Azure AD koji registrira i služi kao jedinstvena identifikacija aplikacije u direktoriju.


- Razvojni inženjeri pomoću provjere autentičnosti biblioteke Otvori izvor Azure AD olakšavaju provjeru autentičnosti po rukovanje detalje protokola. Dodatne informacije potražite u članku [Azure Active Directory provjera autentičnosti biblioteke](active-directory-authentication-libraries.md) .


• Kada korisnik je provjerena autentičnost, aplikacija morate provjeriti korisnika sigurnosni token da biste bili sigurni da je uspjelo za svrhu stranama autentičnosti. Razvojni inženjeri pomoću ponuđena provjere autentičnosti biblioteke možete rukovati provjere valjanosti sve tokena iz Azure AD, uključujući tokeni JSON Web (JWT) ili SAML 2.0. Ako želite ručno izvođenje provjere valjanosti potražite u dokumentaciji [JWT tokena događajima](https://msdn.microsoft.com/library/dn205065.aspx) .


> [AZURE.IMPORTANT] Azure AD koristi javnim ključem šifriranja za potpisivanje tokena i provjerite jesu li valjan. Pogledajte [Važne informacije o potpisivanja ključa prilikom prelaska u Azure AD](active-directory-signing-key-rollover.md) dodatne informacije o potrebne logike u aplikacije da biste bili sigurni da morate imati uvijek ažurira najnovije tipke.


• Tijek zahtjeva i odgovora za postupak provjere ovisi o protokol provjere autentičnosti koja se koristila, kao što su 2.0 OAuth OpenID povezati, WS Federacija ili SAML 2.0. Tim protokolima se spominju u više detalja u temi [Azure Active Directory provjere autentičnosti protokoli](active-directory-authentication-protocols.md) i u sljedećim odjeljcima.

> [AZURE.NOTE] Azure AD podržava OAuth 2.0 i povezivanje OpenID standarde koji čine proširenom korištenje nošenja tokena, uključujući nošenja tokeni predstavljen kao JWTs. Nošenja token je laganih sigurnosni token koja omogućuje pristup "nošenja" na zaštićenom resurs. U ovom smislu "nošenja" je bilo koji proizvođača koje je moguće izlagati token. Iako zabavu morate najprije uspješnoj Azure AD prima token nošenja ako potrebne korake ne uzimaju se sigurnost token u prijenosa i prostora za pohranu, možete se intercepted i koristi neželjeni proizvođača. Dok neke sigurnosnih tokena ima ugrađene mehanizam za sprječava neovlaštenog stranama njihova korištenja, nošenja tokeni nemaju ovaj mehanizam i mora se prenijeti u sigurnog kanala kao što je sigurnost sloja prijenosa (HTTPS). Ako nošenja token prenose se u isključite, na Muškarac u srednjem napadi mogu se zlonamjerni strana Nabava token koji ćete koristiti za neovlašten pristup resursu zaštićeni. Na isti način sigurnost primijeniti kada pohranjivanje ili predmemoriranje nošenja tokeni za kasnije korištenje. Uvijek provjerite je li vaša aplikacija prenosi i pohranjuje nošenja tokeni siguran način. Dodatne sigurnosna pitanja vezana uz na nošenja tokena, potražite u članku [RFC 6750 odjeljak 5](http://tools.ietf.org/html/rfc6750).


Sad kad ste pregled osnove, pročitajte sljedeći odjeljak da biste razumjeli kako radi dodjele resursa u Azure AD te podržava uobičajeni scenariji Azure AD.


## <a name="claims-in-azure-ad-security-tokens"></a>Sporove u Azure AD sigurnosnih tokena

Sigurnosnih tokena koje izdaje Azure AD sadrže zahtjevima ili assertions podatke o temi koja je provjerena autentičnost. Ove zahtjevima može se koristiti u aplikaciji za različite zadatke. Na primjer, možete se koriste za provjeru valjanosti token prepoznavanje klijentu direktorija u predmetu, prikazuju podaci o korisniku, određivanje autorizacije u predmet i tako dalje. Zahtjevima za sve navedene sigurnosni token nisu ovise o vrsti tokena, a zatim vrstu vjerodajnica koje se koriste za provjeru autentičnosti korisnika i konfiguracija aplikacije. Kratak opis svake vrste zahtjeva čuje po Azure AD navedeni su u tablici u nastavku. Dodatne informacije pogledajte [podržane tokena i vrste zahtjeva](active-directory-token-and-claims.md).


| Zahtjeva | Opis |
|-------|-------------|
| ID aplikacije | Služi za identifikaciju aplikaciju koja koristi token.
| Ciljne skupine | Označava primatelja resurs token namijenjen. |
| Aplikacija provjere autentičnosti kontekst predmete Reference | Upućuje na to način na koji klijent je čija je autentičnost provjerena (javno klijent nasuprot povjerljive klijent). |
| Provjera autentičnosti trenutnih | Snima datum i vrijeme kada došlo je do provjeru autentičnosti. |
| Način provjere autentičnosti | Upućuje na to koliko je autentičnost provjerena predmet token (lozinka, certifikat, itd.). |
| Ime | Nudi navedenog ime korisnika kao što je postavljeno u Azure AD. |
| Grupa | Sadrži grupa ID-a Azure AD objekata koje je korisnik član. |
| Davatelja identiteta | Snima davatelja identiteta čija je autentičnost provjerena predmet token. |
| Izdana u | Snima vrijeme na kojoj se token izdavanja, često se koristi za tokena svježina. |
| Izdavač | Služi za identifikaciju STS koji čuje token, kao i u klijent za Azure AD. |
| Prezime | Nudi Prezime korisnika kao što je postavljeno u Azure AD. |
| Ime | Nudi Ljudski čitljiv vrijednost koja određuje predmet token. |
| Id objekta | Sadrži immutable, Jedinstveni identifikator predmeta u Azure AD. |
| Uloge | Sadrži neslužbeni nazive Azure AD uloga aplikacije koje vam je korisnik. |
| Opseg | Označava pravo na klijentskoj aplikaciji. |
| Predmet | Označava glavnicu o token nametanja informacije. |
| Id klijenta | Sadrži immutable, Jedinstveni identifikator klijenta direktorija koja je izdala token. |
| Tokena života. | Definira razdoblje u kojem je valjan token. |
| Korisnikovo Glavno ime | Sadrži korisničko ime glavni predmet. |
| Verzija | Prikazuje broj verzije tokena. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Osnove Registracija aplikacije u Azure AD

Bilo koji program koji outsources provjera autentičnosti za Azure AD mora biti registriran u direktoriju. Ovaj korak uključuje Azure AD obavještava o aplikacije, uključujući URL-a na kojem je smještena, URL-a za slanje odgovora nakon provjere autentičnosti, URI za prepoznavanje aplikacije i drugo. Ove informacije potreban je iz nekoliko razloga ključa:

- Azure AD mora koordinate za komunikaciju s aplikacijom zadužen za prijavu ili razmjene tokena. To obuhvaća sljedeće:

  - Aplikacija ID URI: Identifikator za aplikaciju. Ta vrijednost se šalje Azure AD tijekom provjere autentičnosti da biste naznačili koje aplikacije pozivatelj želi tokena za. Uz to, tu vrijednost je sve obuhvaćeno token tako da se aplikacija zna bile na predviđeno odredište.


  - Odgovorite URL i preusmjeravanje URI: slučaju web API ili web-aplikacije je URL odgovor na mjesto na kojem Azure AD ćete poslati odgovor provjeru autentičnosti, uključujući token ako provjeru autentičnosti nije uspjelo. U slučaju izvornoj aplikaciji URI preusmjeravanje je jedinstveni identifikator kojoj Azure AD bit ćete preusmjereni na korisnički agent u zahtjev za OAuth 2.0.


  - ID klijenta: ID za aplikaciju, koji je generirao Azure AD kada se aplikacija registrira. Kada se zatraži provjeru autentičnosti kod ili token, ID klijenta i ključ šalju Azure AD tijekom provjere autentičnosti.


  - Ključ: Ključ koji je poslan i ID klijenta prilikom provjere autentičnosti za Azure AD da biste uputili poziv API web-mjesto.


- Azure AD mora biti sigurni aplikacija ima potrebne dozvole za pristup podacima direktorija, druge aplikacije u tvrtki ili ustanovi i tako dalje

Dodjeljivanje postaje jasniji prikaz kada znate da postoje dvije kategorije aplikacije koje se mogu razviti i integriran s Azure AD:

- Jedan klijentske aplikacije: jedan klijentske aplikacije namijenjen za korištenje u jedan tvrtke ili ustanove. To su obično redak specifični za poslovanje (LoB) aplikacije napisao programa developer enterprise. Jedan klijentske aplikacije samo potrebno moguće pristupiti u jedan direktorija, a kao rezultat je samo mora biti dodijeljena jedan direktorija. Ove obično registriranih razvojni inženjer u tvrtki ili ustanovi.


- Više klijentske aplikacije: više klijentske aplikacije je namijenjena za mnoge organizacije, ne samo jednu tvrtke ili ustanove. To su obično softver-kao-na-(SaaS) aplikacije servisa napisao proizvođači softvera dobavljača (Neovisni). Više klijentske aplikacije moraju biti dodijeljena svaki direktorija gdje će se koristiti, koji su potrebni pristanak korisnik ili administrator da biste registrirali ih. Ovaj postupak pristanak počinje aplikacije registrirani u direktoriju i je dodijeljen pristup API grafikonu ili možda drugi API na webu. Kad korisnik ili administrator u tvrtki ili ustanovi za različite registrira se da biste koristili aplikaciju, su navedene pomoću dijaloškog okvira koji prikazuje dozvole zahtijeva aplikacije. Korisnik ili administrator može zatim pristanak aplikacije, koji omogućuje pristup aplikacije navedeni podataka i na kraju registrira aplikacija njihov direktorija. Dodatne informacije potražite u članku [Pregled Framework pristanak](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Neka dodatna razmatranja pojaviti prilikom razvoja više klijentske aplikacije umjesto jedan klijentske aplikacije. Ako, na primjer, ako stvarate vaše aplikacije dostupne korisnicima u više direktorija, morat ćete mehanizam da biste utvrdili koji klijent oni pripadaju. Jedan klijentske aplikacije samo treba izgledati u vlastitom direktorija za korisnika u više klijentske aplikacije potrebno pronalaženje određenih korisnika iz svih direktorija Azure AD. Da biste izvršili taj zadatak, Azure AD sadrži uobičajene provjere autentičnosti krajnje gdje bilo koju aplikaciju više klijenta možete uputiti prijavu zahtjeve, umjesto krajnje točke specifične za klijenta. U ovom krajnje točke je https://login.microsoftonline.com/common za sve mape u Azure AD dok krajnje točke specifične za klijent možda https://login.microsoftonline.com/contoso.onmicrosoft.com. Uobičajeni krajnje je osobito važno treba uzeti u obzir prilikom razvoja aplikacije jer je potreban vam je potrebno logike za rukovanje više klijenata prilikom prijave, sign-out, i tokena provjere valjanosti.

Ako trenutno razvoj aplikacija za jednog klijenta, ali želite učiniti dostupnima za mnoge tvrtke ili ustanove, možete jednostavno mijenjate aplikacije i svoju konfiguraciju u Azure AD da bi ga više klijentu koje je moguće povezati. Osim toga, Azure AD koristi isti potpisivanja ključa svih tokena u svim direktorijima, hoće li se omogućuje provjeru autentičnosti u klijentu za jednu ili više klijentske aplikacije.

Svaki scenarij navedena u ovom dokumentu sadrži podređenu odjeljak koji opisuje njegov preduvjeti za dodjelu resursa. Detaljnije informacije o aplikacije u Azure AD i razlike između jednostrukih i više klijentske aplikacije za dodjelu resursa, dodatne informacije potražite [Integraciji aplikacije pomoću servisa Azure Active Directory](active-directory-integrating-applications.md) . Nastavite čitati da biste razumjeli uobičajeni scenariji aplikacije u Azure AD.

## <a name="application-types-and-scenarios"></a>Vrsta aplikacija i scenarijima

Svaki slučajevi opisani u ovom dokumentu mogu se razviti pomoću različite jezike i platformama. Oni su sve sigurnosno po primjere dovršeno koda koji su dostupni u našem [vodiču primjere koda](active-directory-code-samples.md)ili izravno iz odgovarajuće [Github uzorka spremišta](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Osim toga, ako aplikacija treba određenog dijela ili segment do kraja do kraja scenarija, u većini slučajeva te funkcije mogu se dodati neovisno. Ako, na primjer, ako imate matičnoj aplikaciji koja se poziva na API na webu, jednostavno možete dodati web-aplikacije koja se poziva na API na webu. Na sljedećem su dijagramu ilustrira tih scenarija i vrsta aplikacija i kako se može dodati razne komponente:

![Vrsta aplikacija i scenarijima](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Ovo su pet scenariji primarni aplikacija podržava Azure AD:

- [Web-preglednika u web-aplikaciju](#web-browser-to-web-application): korisnik mora se prijaviti na web-aplikacije koja je osigurani po Azure AD.

- [Jednu stranicu aplikacija (SPA)](#single-page-application-spa): korisnik mora se prijaviti u aplikaciju za jednu stranicu koja je osigurani po Azure AD.

- [Matičnoj aplikaciji za API na webu](#native-application-to-web-api): matičnoj aplikaciji koja se izvršava na telefona, tableta ili PC-JU treba provjere autentičnosti korisnika da biste dobili resurse u web-osigurava Azure AD API-JA.

- [Web-aplikaciju za API na webu](#web-application-to-web-api): web-aplikacije mora se resursi s weba API osigurava Azure AD.

- [Daemon ili poslužiteljsku aplikaciju za API na webu](#daemon-or-server-application-to-web-api): daemon aplikacije ili poslužitelju aplikacijom web korisničko sučelje treba uzeti resursi s web-mjesta osigurava Azure AD API-JA.

### <a name="web-browser-to-web-application"></a>Web-preglednika u web-aplikaciju

U ovom se odjeljku opisuju aplikacije koja potvrđuje korisnika u web-pregledniku u web-aplikaciju. U ovom scenariju web-aplikaciji usmjerava korisnikov preglednik da biste se prijavili ih Azure AD. Azure AD vraća odgovor prijave putem preglednika korisnika koji sadrži zahtjevima o korisniku u sigurnosni token. Ovaj scenarij podržava prijavu putem protokola WS Federacija, SAML 2.0 i povezivanje OpenID.


#### <a name="diagram"></a>Dijagram
![Provjera autentičnosti tijek za preglednik web-aplikaciju](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Opis toka protokola


1. Kad korisnik posjeti aplikacije i mora prijaviti, oni se preusmjerava putem zahtjeva za prijavu na krajnjoj točki provjere autentičnosti u Azure AD.


2. Korisnik se prijavi na stranici za prijavu.


3. Ako provjera autentičnosti ne uspije, Azure AD stvara token za provjeru autentičnosti i vraća odgovor prijavu aplikacije odgovor URL-a koji je konfiguriran na portalu za upravljanje Azure. Za aplikaciju proizvodnje, ovaj odgovor URL mora biti HTTPS. Vraćeni tokena sadrži zahtjevima o korisnika i Azure AD koji su potrebni za računala da biste provjerili valjanost token.


4. Aplikacija Provjeri valjanost token pomoću javni ključ potpisivanja i izdavatelj informacije dostupne na dokument metapodataka za vanjski pristup za Azure AD. Kada aplikacija Provjeri valjanost token, Azure AD pokreće novu sesiju s korisnikom. Ovu sesiju omogućuje korisniku za pristup aplikaciji dok što istekne.


#### <a name="code-samples"></a>Primjere koda


Potražite primjere koda web-pregledniku na scenarije web-aplikacije. Možete i rijetko natrag – dodat ćemo nove uzorke cijelo vrijeme. [Web-preglednika u web-aplikaciju](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Registracija


- Jedan klijent: Ako izrađujete aplikaciju samo za tvrtku ili ustanovu, ga mora biti registriran u imeniku tvrtke pomoću portala za upravljanje Azure.


- Više klijent: Ako izrađujete aplikacije koje možete koristiti korisnicima izvan tvrtke ili ustanove, ona mora biti registriran u imeniku tvrtke, ali i mora biti registriran u direktoriju svaki tvrtke ili ustanove koji će koristiti aplikaciju. Da bi aplikacija dostupna u svoje direktorija, možete uključiti postupak za prijave za klijente koji omogućuje da pristanak u aplikaciji. Kada se registrirate za aplikaciju, će se prikazivati pomoću dijaloškog okvira koji prikazuje dozvole zahtijeva aplikacija, a zatim mogućnost pristanak. Ovisno o potrebne dozvole administratora iz druge tvrtke ili ustanove možda morati dodijeliti dozvole. Kada korisnik ili administrator identifikacijskom, aplikacija je registrirana u svoje direktoriju. Dodatne informacije potražite u članku [Integraciji aplikacije pomoću servisa Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Istek tokena

Sesija ističe se kada istekne vijek tokena koje izdaje Azure AD. Aplikacija Skratite vremensko razdoblje prema želji, kao što su Odjava korisnika na temelju određenog razdoblja neaktivnosti. Kada istekne sesiju korisnika će se zatražiti da ponovno se prijavite.





### <a name="single-page-application-spa"></a>Jedna stranica aplikacije (SPA)

U ovom se odjeljku opisuju provjere autentičnosti za jednu stranicu aplikaciju koji koristi Azure AD i autorizacije implicitno OAuth 2.0 dodijeliti sigurne njegove web završili API natrag. Aplikacije za jednu stranicu strukturirane su obično kao JavaScript prezentacije sloj (sučelje) koji se izvodi u pregledniku, a Web API pozadini koji se izvodi na poslužitelju i implementira poslovne logike aplikacije. Dodatne informacije o implicitnoj autorizacije Dodjela te pomoć za određivanje je li za vašu situaciju aplikacije potražite u članku [objašnjenje tijek implicitno grant OAuth2 servisa Azure Active Directory](active-directory-dev-understanding-oauth2-implicit-grant.md).

U ovom scenariju, kada se korisnik prijavi, JavaScript prednje završetka koristi [Provjera autentičnosti biblioteke imenika Active Directory za JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) i dodjela implicitno autorizacije da biste dobili token ID-a (id_token) iz Azure AD. Token nije lokalno i klijent pridružuje je zahtjev kao token nošenja kada upućivanje poziva na njegov API Web sigurnosno završi, zaštićen je proizvod OWIN. 
#### <a name="diagram"></a>Dijagram

![Dijagram jednu stranicu aplikacije](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Opis toka protokola

1. Korisnik vodi na web-aplikaciju.


2. Aplikacija vraća JavaScript sučelja (prezentacije layer) u preglednik.


3. Korisnik pokreće prijavite se, na primjer tako da kliknete prijava vezu. Web-pregledniku šalje VIDJELI krajnjoj autorizacije Azure AD da biste zatražili token za ID-a. Taj zahtjev obuhvaća klijent URL ID i odgovori u parametre upita.


4. Azure AD Provjeri valjanost URL odgovor protiv registrirani URL odgovor koji je konfiguriran na portalu za upravljanje Azure.


5. Korisnik se prijavi na stranici za prijavu.


6. Ako provjera autentičnosti ne uspije, Azure AD stvara sustava token ID-a i vraća je kao dio URL-a (#) URL adresa za odgovor u aplikacije. Za aplikaciju proizvodnje, ovaj odgovor URL mora biti HTTPS. Vraćeni tokena sadrži zahtjevima o korisnika i Azure AD koji su potrebni za računala da biste provjerili valjanost token.


7. JavaScript kod klijenta radi u web-pregledniku izdvaja tokena iz odgovora u zaštita pozive web-aplikacije na kraj API ponovno koristiti.


8. Web-pregledniku poziva web aplikacije API natrag završavati token za pristup u zaglavlju autorizacije.

#### <a name="code-samples"></a>Primjere koda


Pogledajte primjere koda za scenariji za jednu stranicu aplikacija (SPA). Obavezno provjerite natrag često – dodat ćemo nove uzorke cijelo vrijeme. [Jedna stranica aplikacije (SPA)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Registracija


- Jedan klijent: Ako izrađujete aplikaciju samo za tvrtku ili ustanovu, ga mora biti registriran u imeniku tvrtke pomoću portala za upravljanje Azure.


- Više klijent: Ako izrađujete aplikacije koje možete koristiti korisnicima izvan tvrtke ili ustanove, ona mora biti registriran u imeniku tvrtke, ali i mora biti registriran u direktoriju svaki tvrtke ili ustanove koji će koristiti aplikaciju. Da bi aplikacija dostupna u svoje direktorija, možete uključiti postupak za prijave za klijente koji omogućuje da pristanak u aplikaciji. Kada se registrirate za aplikaciju, će se prikazivati pomoću dijaloškog okvira koji prikazuje dozvole potreban je aplikacija, a zatim mogućnost pristanak. Ovisno o potrebne dozvole administratora iz druge tvrtke ili ustanove možda morati dodijeliti dozvole. Kada korisnik ili administrator identifikacijskom, aplikacija je registrirana u svoje direktoriju. Dodatne informacije potražite u članku [Integraciji aplikacije pomoću servisa Azure Active Directory](active-directory-integrating-applications.md).

Nakon registracije aplikaciju, morate ga konfigurirati za korištenje protokola OAuth 2.0 implicitno Dodjela. Prema zadanim postavkama protokol koji je potrebno je onemogućen za aplikacije. Da biste omogućili OAuth2 implicitno Grant protokol za svoju aplikaciju, preuzmite manifestu aplikacije iz trgovine Azure Portal za upravljanje, postavite vrijednost "oauth2AllowImplicitFlow" na true i zatim prenesite manifesta natrag na portal. Detaljne upute potražite u članku [Omogućivanje OAuth 2.0 implicitno dodjela za jednu stranicu aplikacije](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Istek tokena

Kada koristite ADAL.js da biste upravljali provjera autentičnosti s Azure AD, pogodnost nekoliko značajki koje olakšavaju osvježavanje istekle token kao i početak tokeni dodatne web API resursa koje mogu pozivati aplikacija. Kada korisnik uspješno potvrđuje s Azure AD, sesije osigurava kolačić je uspostavljena za korisnika iz preglednika i Azure AD. Nije važno Imajte na umu da postoji sesiju između korisnika i Azure AD i nije korisnik i web-aplikacije koji se izvodi na poslužitelju. Kada istekne token ADAL.js koristi ovu sesiju tihu dobiti novi token. To čini pomoću skrivene iFrame za slanje i primanje zahtjeva pomoću protokola OAuth implicitno Dodjela. ADAL.js možete koristiti ovaj isti mehanizam za tihu dobivanje pristupna tokena Azure AD za druge web-mjesto API resursi koji se poziva aplikacije dok god te resurse podržava više polazište resursa zajedničko korištenje (CORS), registrirane u korisnikove mape i sve potrebne dozvole dobiven korisnik prilikom prijave.


### <a name="native-application-to-web-api"></a>Izvorna aplikacija za API na webu


U ovom se odjeljku opisuju matičnoj aplikaciji koja se poziva na API na webu ime korisnika. Ovaj scenarij je utemeljena na grant vrste OAuth 2.0 kod autorizacije s javnim klijent, kao što je opisano u odjeljku 4.1 [OAuth 2.0 specifikacija](http://tools.ietf.org/html/rfc6749). Matičnoj aplikaciji dohvaća token za pristup za korisnika pomoću protokola OAuth 2.0. Token za pristup zatim šalje u zahtjevu za API koja neadministratorskog korisnika i vraća željeni resursa web-mjestu.

#### <a name="diagram"></a>Dijagram

![Izvorna aplikacija na web-API dijagrama](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Provjera autentičnosti tijek nativni aplikacija API-JA

#### <a name="description-of-protocol-flow"></a>Opis toka protokola


Ako koristite AD provjera autentičnosti biblioteke, većina detalje protokola opisanim rukuje umjesto vas, primjerice skočni prozor preglednika, tokena predmemoriranje i rukovanje tokeni osvježavanja.

1. Pomoću preglednika skočni matičnoj aplikaciji postaje zahtjeva za autorizaciju krajnjoj u Azure AD. Zahtjev sadrži ID klijenta i preusmjeravanje URI matičnoj aplikaciji, kao što je prikazano u Portal za upravljanje i aplikacije ID URI za web-mjesto API-JA. Ako korisnik nije već prijavljeni, oni zatraži da biste se prijavili


2. Azure AD potvrđuje korisnika. Ako je više klijentske aplikacije i dozvole potrebne da biste koristili aplikaciju, korisnik će morati pristanak ako se još niste učinili. Nakon što date pristanak i nakon uspješne provjere autentičnosti Azure AD problemi je odgovor kod autorizacije natrag na klijentskoj aplikaciji preusmjeravanje URI.


3. Kada Azure AD problemi je odgovor kod autorizacije natrag za preusmjeravanje URI, klijentska aplikacija zaustavlja interakcije preglednika i izdvaja kod autorizacije iz odgovor. Pomoću ovog koda za provjeru autentičnosti, klijentska aplikacija šalje zahtjev Azure AD tokena krajnju točku koja sadrži kôd autorizacije, Detalji o klijentskoj aplikaciji (ID klijenta i preusmjeravanje URI), a zatim željeni resursa (aplikacija ID URI za web-mjesto API-JA).


4. Kod autorizacije i informacije o klijentskog računala i web-API valjanost Azure AD. Nakon uspješne provjere Azure AD vraća dva tokena: token za pristup JWT i JWT token osvježavanja. Osim toga, Azure AD vraća osnovne informacije o korisniku, kao što su njihove zaslonsko ime i klijentu ID-a.


5. Putem HTTPS, klijentska aplikacija koristi vraćeni JWT token za pristup da biste dodali JWT niz s "Nošenja" oznaka u zaglavlju autorizacije zahtjeva za API na webu. API na webu pa Provjeri valjanost JWT token i ako Provjera valjanosti ne uspije, vraća željeni resursa.


6. Po isteku token za pristup klijentska aplikacija će primiti pogrešku koja pokazuje korisnik mora ponovno autentičnost. Ako aplikacija ima valjan osvježavanja tokena, može koristiti dobiti novi token pristup bez postavljanja upita korisniku se ponovno prijaviti. Ako osvježavanje token istekne, aplikacija morat ćete interaktivno opet provjere autentičnosti korisnika.


> [AZURE.NOTE] Osvježavanje token izdala Azure AD se može koristiti da biste pristupili višestrukih resursa. Ako, na primjer, ako imate klijentska aplikacija s dozvolom za pozivanje dva web API-ji, token osvježavanja može koristiti da biste pristupili programa tokena na drugim web API kao i.


#### <a name="code-samples"></a>Primjere koda


Pogledajte primjere koda za matičnoj aplikaciji na scenarije Web API. Možete i rijetko natrag – dodat ćemo nove uzorke cijelo vrijeme. [Izvorna aplikacija za API na webu](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Registracija


- Jedan klijent: Oba s lokalnog računala i na webu API mora biti registriran u istom direktoriju u Azure AD. Da biste otkrili skup dozvola koje se koriste za ograničavanje pristupa matičnoj aplikaciji njegov resursima moguće je konfigurirati za API na webu. Klijentska aplikacija pa odabir željene dozvole iz padajućeg izbornika "Dozvole za ostale aplikacije" na portalu za upravljanje Azure.


- Više klijent: Najprije matičnoj aplikaciji samo ikad registrirani u programer ili imenika programa publisher. Drugo, matičnoj aplikaciji je konfiguriran za određivanje dozvola bit će potrebno više funkcionirati. Ovaj popis potrebne dozvole se prikazuje u dijaloškom okviru kada korisnik ili administrator u imeniku odredište omogućuje pristanak aplikacije koji ga čini dostupnim njihovoj tvrtki ili ustanovi. Neke aplikacije potreban je samo korisnik razinu dozvole koje možete pristajete bilo koji korisnik u tvrtki ili ustanovi. Druge aplikacije potreban je administratorske dozvole koje se ne može pristajete korisnika u tvrtki ili ustanovi. Samo administrator direktorija možete dati pristanak aplikacije koji zahtijevaju ta razina dozvola. Kada korisnik ili administrator identifikacijskom, samo na webu API registriran u svoje direktorija. Dodatne informacije potražite u članku [Integraciji aplikacije pomoću servisa Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Istek tokena


Kada matičnoj aplikaciji koristi njegov autorizacije kod da biste pristupili na JWT tokena, također prima token JWT osvježavanja. Kada istekne token za pristup token osvježavanja može se koristiti da biste ponovno provjere autentičnosti korisnika koji ne zahtijeva ih ponovno prijaviti. Ovaj token osvježavanja tada se koristi za provjeru autentičnosti korisnika koji rezultira novi token access i osvježavanje token.





### <a name="web-application-to-web-api"></a>Web-aplikaciju za API na webu


U ovom se odjeljku opisuju web-aplikacije koje treba uzeti resurse u web-API-JA. U ovom scenariju postoje dvije vrste identiteta za provjeru autentičnosti i poziva na API na webu možete koristiti web-aplikaciji: identitet aplikacije ili identitet ovlaštenog korisnika.

*Identitet aplikacije:* Ovaj scenarij koristi OAuth 2.0 klijent vjerodajnice grant Provjeri kao aplikacije i pristup webu API-JA. Prilikom korištenja aplikacije identitet, na webu API samo možete otkriti da web-aplikacija je pozivanje, kao na web-mjesto API primio nikakve informacije o korisniku. Ako aplikacija prima podatke o korisniku, ona će biti poslana putem protokola aplikacije pa ga je potpisao Azure AD. Na webu API smatra pouzdanima web-aplikaciji provjere autentičnosti korisnika. Zbog toga ovaj uzorak zove pouzdani podsustav.

*Delegirani identitet korisnika:* Ovaj scenarij je moguće napraviti na dva načina: povezivanje OpenID i OAuth 2.0 autorizacije kod grant povjerljive klijenta. Web-aplikacija dohvaća token za pristup za korisnika dokazuje API koje uspješno autentičnost korisnika na web-aplikacije i koje je web-aplikacija možete nabaviti identitet korisnika ovlaštenog da biste nazvali web API na webu. U zahtjevu za API koja neadministratorskog korisnika i vraća željeni resursa web-mjestu šalje se token za pristup.

#### <a name="diagram"></a>Dijagram

![Web-aplikaciju za Web API dijagrama](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Opis toka protokola

Identitet aplikacije i vrste identitet korisnika ovlaštenog se spominju u nastavku tijek. Ključne razlike između njih je da identitet korisnika ovlaštenog morate najprije pribaviti je kod autorizacije prije korisnika možete prijaviti i ostvariti pristup webu API-JA.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Identitet aplikacije s klijentom OAuth 2.0 vjerodajnice Grant

1. Korisnik prijavljen je na Azure AD u web-aplikaciji (pogledajte [Web-preglednika u web-aplikaciju](#web-browser-to-web-application) gore).


2. Web-aplikaciju mora Nabava pristupnog tokena tako da mogu autentičnost API na webu i dohvaćanje željeni resursa. Čini zahtjeva za krajnje tokena Azure AD, koja omogućuje vjerodajnicu, ID klijenta i web-mjesta API aplikacija ID URI.


3. Azure AD potvrđuje aplikacije i vraća token za pristup JWT koji se koristi za poziv u API na webu.


4. Web-aplikaciji putem HTTP, koristi vraćeni JWT token za pristup da biste dodali JWT niz s "Nošenja" oznaka u zaglavlju autorizacije zahtjeva za API na webu. API na webu pa Provjeri valjanost JWT token i ako Provjera valjanosti ne uspije, vraća željeni resursa.

##### <a name="delegated-user-identity-with-openid-connect"></a>Povezivanje identitet ovlaštenog korisnika s OpenID

1. Korisnik prijavljen je na web-aplikacije pomoću Azure AD (pogledajte odjeljak [Web-preglednika u web-aplikaciju](#web-browser-to-web-application) gore). Ako je korisnik web-aplikacije sadrži nije još pristanak na dopuštanja web-aplikacije da biste nazvali web API-JA na njegovo ime, korisnika morat ćete pristanak. Aplikacija će se prikazivati potrebna je dozvola, a ako su neki od ovih administratorske dozvole, normalni korisnika u imeniku neće moći pristanak. Ovaj postupak pristanak odnosi samo na više klijentske aplikacije, aplikacija ne jednog klijenta, kao što je aplikacija već imate potrebne dozvole. Kada korisnik prijavljen, web-aplikacija primio token za ID-a s informacijama o korisniku, kao što je kod za autorizaciju.


2. Pomoću koda za autorizaciju izdala Azure AD, web-aplikaciji šalje zahtjev Azure AD tokena krajnju točku koja sadrži kôd autorizacije, Detalji o klijentskoj aplikaciji (ID klijenta i preusmjeravanje URI), a zatim željeni resursa (aplikacija ID URI za web-mjesto API-JA).


3. Šifra autorizacije i informacije o web-aplikacije i web API valjanost Azure AD. Nakon uspješne provjere Azure AD vraća dva tokena: token za pristup JWT i JWT token osvježavanja.


4. Web-aplikaciji putem HTTP, koristi vraćeni JWT token za pristup da biste dodali JWT niz s "Nošenja" oznaka u zaglavlju autorizacije zahtjeva za API na webu. API na webu pa Provjeri valjanost JWT token i ako Provjera valjanosti ne uspije, vraća željeni resursa.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Identitet ovlaštenog korisnika s OAuth 2.0 autorizacije kod Grant

1. Korisnik već prijavili na web-aplikacija ne ovisi o Azure AD čije mehanizam provjere autentičnosti.


2. Web-aplikacije potreban je kod autorizacije Nabava pristupnog tokena, pa je izdao zahtjeva putem web-pregledniku Azure AD krajnjoj točki autorizacije, koja omogućuje ID klijenta i preusmjeravanje URI za web-aplikaciju nakon uspješne provjere autentičnosti. Korisnikove prijave Azure AD.


3. Ako je korisnik web-aplikacije sadrži nije još pristanak na dopuštanja web-aplikacije da biste nazvali web API-JA na njegovo ime, korisnika morat ćete pristanak. Aplikacija će se prikazivati potrebna je dozvola, a ako su neki od ovih administratorske dozvole, normalni korisnika u imeniku neće moći pristanak. Ovaj postupak pristanak odnosi samo na više klijentske aplikacije, aplikacija ne jednog klijenta, kao što je aplikacija već imate potrebne dozvole.


4. Kada korisnik ima pristanak, web-aplikaciji prima autorizacije kod koji je potrebno Nabava token za pristup.


5. Pomoću koda za autorizaciju izdala Azure AD, web-aplikaciji šalje zahtjev Azure AD tokena krajnju točku koja sadrži kôd autorizacije, Detalji o klijentskoj aplikaciji (ID klijenta i preusmjeravanje URI), a zatim željeni resursa (aplikacija ID URI za web-mjesto API-JA).


6. Šifra autorizacije i informacije o web-aplikacije i web API valjanost Azure AD. Nakon uspješne provjere Azure AD vraća dva tokena: token za pristup JWT i JWT token osvježavanja.


7. Web-aplikaciji putem HTTP, koristi vraćeni JWT token za pristup da biste dodali JWT niz s "Nošenja" oznaka u zaglavlju autorizacije zahtjeva za API na webu. API na webu pa Provjeri valjanost JWT token i ako Provjera valjanosti ne uspije, vraća željeni resursa.

#### <a name="code-samples"></a>Primjere koda

Pogledajte primjere koda za web-aplikaciju na scenarije Web API. Možete i rijetko natrag – dodat ćemo nove uzorke cijelo vrijeme. Web- [aplikaciju za API na webu](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Registracija

- Jedan klijent: Za identitet aplikacije i slučajeva identiteta ovlaštenog korisnika, web-aplikacije i web API mora biti registriran u istom direktoriju u Azure AD. Da biste otkrili skup dozvola koji se koriste ograničiti pristup web-aplikacije njegovih resursa moguće je konfigurirati web API-JA. Ako se koristi vrstu identiteta ovlaštenog korisnika, web-aplikacije mora da biste odabrali željene dozvole s padajućeg izbornika "Dozvole za ostale aplikacije" na portalu za upravljanje Azure. Ovaj korak nije potrebna ako se koristi vrsta identiteta aplikacije.


- Više klijent: Najprije web-aplikacija je konfiguriran za određivanje dozvola bit će potrebno više funkcionirati. Ovaj popis potrebne dozvole se prikazuje u dijaloškom okviru kada korisnik ili administrator u imeniku odredište omogućuje pristanak aplikacije koji ga čini dostupnim njihovoj tvrtki ili ustanovi. Neke aplikacije potreban je samo korisnik razinu dozvole koje možete pristajete bilo koji korisnik u tvrtki ili ustanovi. Druge aplikacije potreban je administratorske dozvole koje se ne može pristajete korisnika u tvrtki ili ustanovi. Samo administrator direktorija možete dati pristanak aplikacije koji zahtijevaju ta razina dozvola. Kada korisnik ili administrator identifikacijskom, web-aplikacije i web API oba bilježe se u njihove direktorija.

#### <a name="token-expiration"></a>Istek tokena

Kada web-aplikaciji koristi njegov autorizacije kod da biste pristupili na JWT tokena, također prima token JWT osvježavanja. Kada istekne token za pristup token osvježavanja može se koristiti da biste ponovno provjere autentičnosti korisnika koji ne zahtijeva ih ponovno prijaviti. Ovaj token osvježavanja tada se koristi za provjeru autentičnosti korisnika koji rezultira novi token access i osvježavanje token.


### <a name="daemon-or-server-application-to-web-api"></a>Daemon ili poslužiteljsku aplikaciju za API na webu


U ovom se odjeljku opisuju daemon ili poslužitelj za aplikaciju koja treba uzeti resurse u web-API-JA. Postoje dva podređenu scenarija koji se odnose na ovom odjeljku: daemon koju je potrebno da biste uputili poziv API-JA, ugrađeni tipu OAuth 2.0 klijent vjerodajnice Dodjela; web-mjesto i poslužitelj aplikacije (primjerice web API-JA) koji je potrebno da biste uputili poziv API-JA, utemeljena na OAuth 2.0 On-Behalf-Of skice specifikacija web-mjesto.

Scenarija kada treba daemon aplikacije da biste nazvali web API-JA, važno je znati nekoliko stvari. Najprije interakcije s korisnikom nije moguće aplikacijom daemon koji su potrebni aplikacije vlastitu identitet. Primjer daemon aplikacije je obrade ili servis za operacijski sustav koji se izvodi u pozadini. Ovu vrstu aplikacije zahtijeva token za pristup pomoću svoj identitet aplikacije i izlaganje klijentu ID, vjerodajnica (lozinka ili certifikat) i aplikacije ID URI za Azure AD. Nakon uspješne provjere autentičnosti daemona prima pristupnog tokena iz Azure AD kojom se zatim poziva na API na webu.

Scenarija kada poslužiteljska aplikacija treba da biste nazvali web API-JA, korisno je za korištenje primjera. Zamislite da je korisnik provjerene na matičnoj aplikaciji, a ovaj matičnoj aplikaciji treba poziva na API na webu. Azure AD problemi token za pristup JWT da biste uputili poziv na API na webu. Ako je potrebno web API da biste pozvali druge do API na webu, ga pomoću tijek na-ime-od delegatu identitet korisnika i provjere autentičnosti na webu drugog sloja API-JA.

#### <a name="diagram"></a>Dijagram

![Daemon ili poslužiteljsku aplikaciju Web API dijagram](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Opis toka protokola

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Identitet aplikacije s klijentom OAuth 2.0 vjerodajnice Grant

1. Najprije poslužiteljske aplikacije mora uspješnoj Azure AD kao sam bez Ljudski interakcije kao što je interaktivne prijave dijaloški okvir. Čini zahtjeva za krajnje tokena Azure AD, koja omogućuje vjerodajnica, ID klijenta i aplikacije ID URI.


2. Azure AD potvrđuje aplikacije i vraća token za pristup JWT koji se koristi za poziv u API na webu.


3. Web-aplikaciji putem HTTP, koristi vraćeni JWT token za pristup da biste dodali JWT niz s "Nošenja" oznaka u zaglavlju autorizacije zahtjeva za API na webu. API na webu pa Provjeri valjanost JWT token i ako Provjera valjanosti ne uspije, vraća željeni resursa.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Identitet ovlaštenog korisnika s specifikacija skica na-ime-od OAuth 2.0

Tijek spominju ispod pretpostavlja da korisnik ima provjerena autentičnost na neku drugu aplikaciju (kao što su matičnoj aplikaciji) i njihov identitet korisnika korišten dobiti token za pristup web prvog sloja API-JA.

1. Matičnoj aplikaciji šalje token za pristup web prvog sloja API-JA.


2. API prvog sloja web šalje zahtjev Azure AD tokena krajnje, koja omogućuje klijentu ID i vjerodajnice, kao i token za pristup korisnika. Osim toga, zahtjev šalje s programa on_behalf_of parametar koji pokazuje na webu API zahtijeva novi tokeni da biste nazvali do web-mjesto API ime izvornog korisnika.


3. Azure AD potvrđuje web prvog sloja API ima dozvole za pristup webu drugog sloja API-JA, a Provjeri valjanost zahtjev za vraćanje token za pristup JWT a na JWT osvježavanje token web prvog sloja API-JA.


4. Putem HTTPS, API prvog sloja web zatim poziva web drugog sloja API dodavanjem tokena niza u zaglavlju autorizacije u zahtjevu za. Prvi sloja web API-JA možete nastaviti poziva web drugog sloja API dok god tokena access i osvježavanje tokeni vrijede.

#### <a name="code-samples"></a>Primjere koda

Pogledajte primjere koda za Daemon ili poslužiteljsku aplikaciju na scenarije Web API. Možete i rijetko natrag – dodat ćemo nove uzorke cijelo vrijeme. [Poslužitelj ili Daemon aplikaciju za API na webu](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registracija

- Jedan klijent: Za identitet aplikacije i slučajeva identiteta ovlaštenog korisnika, aplikacije daemon ili poslužitelja mora biti registriran u istoj mapi u Azure AD. Da biste otkrili skup dozvola koje se koriste za ograničavanje daemona ili pristup poslužitelja njegovih resursa moguće je konfigurirati web API-JA. Ako se koristi vrstu identiteta ovlaštenog korisnika, poslužiteljske aplikacije mora da biste odabrali željene dozvole s padajućeg izbornika "Dozvole za ostale aplikacije" na portalu za upravljanje Azure. Ovaj korak nije potrebna ako se koristi vrsta identiteta aplikacije.


- Više klijent: Najprije daemon ili poslužitelj za aplikaciju je konfiguriran za određivanje dozvola bit će potrebno više funkcionirati. Ovaj popis potrebne dozvole se prikazuje u dijaloškom okviru kada korisnik ili administrator u imeniku odredište omogućuje pristanak aplikacije koji ga čini dostupnim njihovoj tvrtki ili ustanovi. Neke aplikacije potreban je samo korisnik razinu dozvole koje možete pristajete bilo koji korisnik u tvrtki ili ustanovi. Druge aplikacije potreban je administratorske dozvole koje se ne može pristajete korisnika u tvrtki ili ustanovi. Samo administrator direktorija možete dati pristanak aplikacije koji zahtijevaju ta razina dozvola. Kada korisnik ili administrator identifikacijskom, oba web-mjesta API-ji registriran u svoje direktorija.

#### <a name="token-expiration"></a>Istek tokena

Kada prvog aplikacija koristi njegov kod autorizacije da biste pristupili na JWT tokena, i prima token JWT osvježavanja. Kada istekne token za pristup token osvježavanja može se koristiti da biste ponovno provjere autentičnosti korisnika bez postavljanja upita za vjerodajnice. Ovaj token osvježavanja tada se koristi za provjeru autentičnosti korisnika koji rezultira novi token access i osvježavanje token.

## <a name="see-also"></a>Vidi također

[Vodič za programiranje Azure Active Directory](active-directory-developers-guide.md)

[Primjere koda Azure Active Directory](active-directory-code-samples.md)

[Važne informacije o prijavi ključa prilikom prelaska u Azure AD](active-directory-signing-key-rollover.md)

[OAuth 2.0 u Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)
