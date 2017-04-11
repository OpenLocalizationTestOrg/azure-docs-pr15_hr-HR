<properties
   pageTitle="Pojmovnik za razvojne inženjere Azure Active Directory | Microsoft Azure"
   description="Popis uvjete za najčešće korištenih koncepti za razvojne inženjere Azure Active Directory i značajke."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Pojmovnik za razvojne inženjere za Azure Active Directory
Ovaj članak sadrži definicije za neke od Azure Active Directory (AD) core za razvojne inženjere koncepata koji su korisne prilikom prepoznavanju razvoj aplikacija za Azure AD.

## <a name="access-token"></a>pristup tokena
Vrsta [sigurnosnog tokena](#security-token) izdala za [provjeru autentičnosti poslužitelja](#authorization-server)i koristi [klijentski program](#client-application) da biste pristupili [zaštićeni resursa poslužitelja](#resource-server). Obično u obliku [JSON Web tokena (JWT)][JWT], token embodies autorizacije klijentu dodijelio [vlasnik resursa](#resource-owner), za tražene razinu pristupa. Token sadrži sve odgovarajuće [zahtjevima](#claim) o predmet, omogućivanje klijentske aplikacije da biste koristili kao obrasca vjerodajnica prilikom pristupanja navedene resurse. To je i nema potrebe za vlasnik resursa da biste otkrili vjerodajnice za klijent.

Pristupna tokena ponekad nazivaju "Korisniku + aplikacije" ili "Aplikacija samo", ovisno o vjerodajnice se predstavlja. Na primjer, kada klijentski program koristi na:

- [Dodjela autorizacije "Autorizacije kod"](#authorization-grant), krajnji korisnik najprije vlasnik resursa prenošenjem autorizacije klijentu za pristup resursu potvrđuje. Klijent potvrđuje kasnije prilikom dohvaćanja token za pristup. Token možete biti ponekad preciznije kao "Korisniku + aplikacije" token, kao što je predstavlja oba korisnika koji je ovlasti klijentska aplikacija i računala.
- [Dodjela autorizacije "Klijent vjerodajnica"](#authorization-grant), klijent nudi snosite provjere autentičnosti radi bez resursa – vlasnika provjere autentičnosti/dozvole, tako da token možete ponekad se nazivaju i programa token "Samo za aplikacije".

Potražite u članku [Referenca za Azure AD tokena] [ AAD-Tokens-Claims] više pojedinosti.

## <a name="application-manifest"></a>programski manifest  
Značajka nudi [Azure klasični portal][AZURE-classic-portal], koji stvara prikaz JSON konfiguracija aplikacije identitet koriste kao mehanizam za ažuriranje njezinoj povezane [aplikacije] [ AAD-Graph-App-Entity] i [ServicePrincipal] [ AAD-Graph-Sp-Entity] entiteti. Pročitajte [objašnjenje programski manifest Azure Active Directory] [ AAD-App-Manifest] više pojedinosti.

## <a name="application-object"></a>objekt aplikacije  
Kada ste register/ažuriranje aplikacija za [Azure klasični portal][AZURE-classic-portal], na portalu stvara/ažuriranja objekt aplikacija i odgovarajući [glavni objekt servisa](#service-principal-object) za taj klijent. Aplikacija objekt *definira* aplikacija je identiteta konfiguracije globalno (preko svih drugih korisnika kojem ima pristup), koja omogućuje predloška iz kojeg njegov odgovarajuće objekata glavni servis su *izvedena* za korištenje lokalno vrijeme izvođenja (u određenim klijenta).

U odjeljku [aplikacije i usluge objekata] [ AAD-App-SP-Objects] dodatne informacije.

## <a name="application-registration"></a>Registracija aplikacije  
Da bi dopustila programu Integracija s i delegiranje identiteta i upravljanje pristupom funkcije za Azure AD, on mora biti registriran u Azure AD [klijenta](#tenant). Kada registrirate aplikacija Azure AD, stvaranju u konfiguraciji identiteta za svoju aplikaciju dopuštanja integrirati s Azure AD i koristiti značajke kao što su:

- Robusne upravljanje od jedinstvenu prijavu pomoću upravljanje identitetom Azure AD i [Povezivanje OpenID] [ OpenIDConnect] protokol implementacije
- Brokered pristup [zaštićeni resursi](#resource-server) [klijentske aplikacije](#client-application)putem Azure AD OAuth 2.0 [provjeru autentičnosti poslužitelja](#authorization-server) implementacije
- [Pristanak framework](#consent) za upravljanje pristupom klijent zaštićeni resurse na temelju autorizacije vlasnik resursa.

[Aplikacija Integrating pomoću servisa Azure Active Directory] potražite u članku[ AAD-Integrating-Apps] više pojedinosti.

## <a name="authentication"></a>Provjera autentičnosti
Čin izazov zabavu za ispravne vjerodajnice, koja omogućuje osnovu za stvaranje sigurnosnog upravitelja će se koristiti za kontrolu identiteta i pristup. Tijekom programa [OAuth2 autorizacije dodijelili](#authorization-grant) , na primjer, davatelja provjere autentičnosti je ispunjavanja ulogu [vlasnik resursa](#resource-owner) ili [klijentska aplikacija](#client-application), ovisno o grant koristi.

## <a name="authorization"></a>autorizacija
Čin dodjelu čija je autentičnost provjerena sigurnost glavni dozvole da biste učinili nešto. Postoje dva slučaja prvenstveno se koristi u modelu programiranje Azure AD:

- Tijekom tijek za [OAuth2 autorizacije dodijeliti](#authorization-grant) : kada [vlasnik resursa](#resource-owner) daje autorizacije [klijentska aplikacija](#client-application), što omogućuje klijentski pristup resursima vlasnik resursa.
- Tijekom pristup resursu klijenta: kao što je implementirana [resursa poslužitelja](#resource-server), pomoću [zahtjeva](#claim) vrijednosti prikazivanju [token pristup](#access-token) da biste odluke o kontrola pristupa na temelju njih.

## <a name="authorization-code"></a>autorizacija kod
Kratki mogli živjeti "tokena" nudi [klijentska aplikacija](#client-application) [autorizacije krajnje](#authorization-endpoint)kao dio tijek "autorizacije kod" neke s četiri OAuth2 [daje autorizacije](#authorization-grant). Kod se vraćaju u klijentsku aplikaciju u odgovoru provjere autentičnosti dodijeljenog [vlasnika resursa](#resource-owner), koji označava vlasnik resursa delegirao autorizacije da biste pristupili tražene resursi. Kao dio tijeka, kod je kasnije aktivacije [token za pristup](#access-token).

## <a name="authorization-endpoint"></a>autorizacija krajnje točke
Jedno od krajnje točke implementirana [provjeru autentičnosti poslužitelja](#authorization-server), služi za interakciju s [vlasnik resursa](#resource-owner) da bi im se [dodijeliti autorizacije](#authorization-grant) tijekom OAuth2 odobrenje dodijelili tijek. Ovisno o grant autorizacije tijek koristi, stvarni grant dobili mogu se razlikovati, uključujući [autorizacije kod](#authorization-code) ili [sigurnosni token](#security-token).

Potražite u članku specifikacija OAuth2 [autorizacije dodijeliti vrste] [ OAuth2-AuthZ-Grant-Types] i [autorizacije krajnje] [ OAuth2-AuthZ-Endpoint] sekcije i [OpenIDConnect specifikacija] [ OpenIDConnect-AuthZ-Endpoint] više pojedinosti.

## <a name="authorization-grant"></a>Dodjela autorizacije
Vjerodajnice koji predstavlja [resursa vlasnik](#resource-owner) [autorizacije](#authorization) da biste pristupili zaštićeni resursa, dodijeljene [klijentske aplikacije](#client-application). Klijentska aplikacija možete koristiti jednu od [četiri vrste Dodjela koje definira autorizacije Framework OAuth2] [ OAuth2-AuthZ-Grant-Types] da biste dobili dodijelite, ovisno o vrsti preduvjeti klijenta: "autorizacije kod grant", "klijent vjerodajnice dodijeliti", "implicitno grant" i "Dodjela resursa vlasnik lozinke vjerodajnice". Vjerodajnica vraća klijentu je [token za pristup](#access-token)ili je [autorizacije kod](#authorization-code) (razmijenili kasnije token programa access), ovisno o vrsti autorizacije grant koristi.

## <a name="authorization-server"></a>provjeru autentičnosti poslužitelja
Kako je definirano parametrom [OAuth2 autorizacije Framework][OAuth2-Role-Def], tokeni odgovoran za izdavanje access poslužitelja [klijenta](#client-application) nakon uspješno provjere autentičnosti [vlasnik resursa](#resource-owner) i dobivanje njegov autorizacije. [Klijentska aplikacija](#client-application) stupi u interakciju s poslužiteljem za autorizaciju prilikom izvođenja putem [autorizacije](#authorization-endpoint) i [tokena](#token-endpoint) krajnje točke u skladu s OAuth2 definirani [daje autorizacije](#authorization-grant).

U slučaju Azure AD integraciju aplikacija, Azure AD implementira autorizacije uloga poslužitelja za Azure AD aplikacija i servisa Microsoft API-ji, primjerice [Microsoft Graph API -ji][Microsoft-Graph].

## <a name="claim"></a>rezerviranje
[Sigurnosni token](#security-token) sadrži zahtjevima, čime se omogućuje assertions o jednoj entitet (primjerice [klijentska aplikacija](#client-application) ili [vlasnik resursa](#resource-owner)) u drugi (kao što su [resursa poslužitelja](#resource-server)). Sporove su parova naziv/vrijednost koja se preusmjeravanje činjenice o tokena predmet (na primjer, sigurnost glavnicu koji je autentičnost provjerena [provjeru autentičnosti poslužitelja](#authorization-server)). Koje su prisutne u određenom token zahtjevima su ovisi o tome više varijabli, uključujući vrstu tokena, a zatim vrstu vjerodajnica koje se koriste za provjeru autentičnosti predmet, konfiguracije aplikacije, itd.

Potražite u članku [Referenca za tokena Azure AD] [ AAD-Tokens-Claims] više pojedinosti.

## <a name="client-application"></a>Klijentska aplikacija  
Kako je definirano parametrom [OAuth2 autorizacije Framework][OAuth2-Role-Def], aplikacije koja omogućuje zaštićeni resursa zahtjeve ime [vlasnika resursa](#resource-owner). Izraz "klijent" ukazuju na sve značajke određeni hardver implementacije (Ako, primjerice, hoće li aplikacija izvršava na poslužitelju, prikaza radne površine ili drugih uređaja).  

Klijentska aplikacija zahtjeve [autorizacije](#authorization) od vlasnika resursa da biste sudjelovali u tijeku za [OAuth2 autorizacije dodijeliti](#authorization-grant) , a mogu pristupati API-podataka u ime vlasnika resursa. OAuth2 autorizacije Framework [definira dvije vrste klijenti][OAuth2-Client-Types]"Povjerljivo" i "javno", ovisno o klijenta mogućnost da biste zadržali povjerljivosti njegov vjerodajnice. Aplikacije možete implementirati [web-klijentu (povjerljivim)](#web-client) koji se izvodi na web-poslužitelj, [nativni klijent (javni)](#native-client) instalirana na uređaj ili [korisnički agent - klijentska (javni)](#user-agent-based-client) koji se pokreće se u pregledniku na uređaju.

## <a name="consent"></a>pristanak
Postupak [vlasnik resursa](#resource-owner) dodjelu autorizacije [klijentska aplikacija](#client-application), određene [dozvole](#permissions) za pristup zaštićeni resursa, ime vlasnika resursa. Ovisno o dozvolama zatražio klijent, administrator ili korisnik će se tražiti da pristanak odnosno dopustili pristup svoje tvrtke ili ustanove-određene podatke. Imajte na umu u scenariju s [više klijentske](#multi-tenant-application) aplikacije [servisa glavni](#service-principal-object) je zabilježen u klijentu consenting korisnika.

## <a name="id-token"></a>Token ID-a
Za [Povezivanje OpenID] [ OpenIDConnect-ID-Token] [sigurnosnog tokena](#security-token) koje ste dobili od programa [provjeru autentičnosti poslužitelja](#authorization-server) [krajnja točka za provjeru autentičnosti](#authorization-endpoint), koji sadrži [zahtjevima](#claim) vezani uz za provjeru autentičnosti krajnji korisnik [vlasnik resursa](#resource-owner). Kao što je pristupni token ID tokena i prikazane su kao digitalno potpisanu [JSON Web tokena (JWT)][JWT]. Za razliku od token za pristup kroz, zahtjevima token ID-a ne koriste se za potrebe vezane uz pristup resursa i posebno pristupite kontrolu.

Potražite u članku [Referenca za tokena Azure AD] [ AAD-Tokens-Claims] više pojedinosti.

## <a name="multi-tenant-application"></a>više klijentske aplikacije
Klase [klijentske aplikacije](#client-application) koja omogućuje prijavite se u i [pristanak](#consent) korisnici dodjeli u bilo kojem Azure AD [klijentu](#tenant), uključujući klijenata osim onog gdje je registrirana klijent. Suprotno tome, aplikacija registrirana kao jednog klijenta želite dopustiti samo znak dodataka iz korisničke račune dodjeli u istom klijentu onog gdje je registrirana aplikacije. Više klijentu po zadanom su aplikacije za [nativni klijent](#native-client) dok [web klijentske](#web-client) aplikacije imati mogućnost da biste odabrali između jedne i više klijenta.

Pogledajte [kako se prijaviti u bilo koji korisnik Azure AD pomoću više klijentske aplikacije uzorak] [ AAD-Multi-Tenant-Overview] više pojedinosti.

## <a name="native-client"></a>nativni klijent
Vrsta [klijentska aplikacija](#client-application) koja se instalira nativno na uređaju. Budući da se sav kod se izvršava na uređaju, smatra "javno" klijent zbog njegov nemogućnosti za pohranu vjerodajnica privatno/Povjerljivo. Potražite u članku [vrstama klijenata za OAuth2 i profili] [ OAuth2-Client-Types] više pojedinosti.

## <a name="permissions"></a>dozvole
[Klijentska aplikacija](#client-application) poboljšava pristup [resursa poslužitelja](#resource-server) tako da deklariranje zahtjeva za dozvole. Dostupne su dvije vrste:

- "Ovlaštenog" dozvole koje se zatraži pristup [utemeljen na opseg](#scopes) u odjeljku delegirani autorizacije od prijavljeni u [vlasnik resursa](#resource-owner), predstavljanja resursa pri izvođenju kao ["pronađenim" zahtjevima](#claim) u klijentskog programa [access token](#access-token).
- Dozvole za "Aplikacija" koje traženje pristupa [na temelju uloga](#roles) u odjeljku vjerodajnice identiteta klijentska aplikacija, predstavljanja resursa pri izvođenju kao ["uloge" zahtjeve](#claim) u token klijentskog programa access.

Oni i plošni tijekom [pristanak](#consent) dodjeljivanja administrator ili vlasnik resursa prilike grant/zabranili pristup klijent resursima njihove klijentu.

Zahtjevi za dozvolu konfigurirane "Aplikacije" / "Konfiguriranje" kartica [Azure klasični portal][AZURE-classic-portal], u odjeljku "Dozvole drugim aplikacijama", tako da odabir željene "dodijeliti dozvole" i "Dozvola za aplikaciju" (drugu mogućnost potrebno članstvo u ulozi globalni administrator). Jer [javno klijent](#client-application) ne možete održavati vjerodajnice, ga možete samo zatražiti ovlaštenog dozvole [povjerljive klijent](#client-application) je mogućnost zahtjev za oba delegirani i dozvola za aplikaciju. Klijenta [objekt aplikacije](#application-object) sprema declared dozvole u [svojstvo requiredResourceAccess][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>vlasnik resursa
Kako je definirano parametrom [OAuth2 autorizacije Framework][OAuth2-Role-Def], entitet instaliranog omogućivanju pristupa zaštićenim resurs. Kada vlasnik resursa je osoba, on je se nazivaju krajnjeg korisnika. Na primjer, kada [klijentska aplikacija](#client-application) želi pristup poštanskog sandučića korisnika putem [Programa Microsoft Graph API][Microsoft-Graph], potrebna je dozvola od vlasnika resursa poštanskog sandučića.

## <a name="resource-server"></a>resurse poslužitelja
Kako je definirano parametrom [OAuth2 autorizacije Framework][OAuth2-Role-Def], server da glavno računalo zaštićeno resursa, možete prihvatiti i odgovarati na njih zaštićen resursa zahtjeve [klijentske aplikacije](#client-application) koji dovode do [pristup token](#access-token). Poznata i kao zaštićeni resursa poslužitelju ili aplikacije resursa.

Resursa poslužitelja izlaže API-ji i nameće pristup njegov zaštićeni resursima putem [opsega](#scopes) i [uloge](#roles), pomoću Framework za autorizaciju OAuth 2.0. Primjeri API Azure AD grafikonu koji omogućuje pristup podacima Azure AD klijenta i Office 365 API-ji koji omogućuju pristup podacima kao što su pošta i kalendar. Oboje od sljedećeg također su dostupne putem [Programa Microsoft Graph API][Microsoft-Graph].  

Baš kao i klijentske aplikacije konfiguracija identiteta resursa aplikacije uspostaviti putem [Registracija](#application-registration) Azure AD klijentu, koja omogućuje aplikacija i servisa glavni objekta. Neke API-ovi za Microsoft, kao što su API Azure AD grafikonu unaprijed registriranih servisa upravitelji postao dostupan u svim korisnicima prilikom dodjele resursa.

## <a name="roles"></a>uloge
Kao što su [opsega](#scopes)uloge omogućuju za [resurse poslužitelja](#resource-server) upravljaju pristup svojih zaštićeni resursa. Postoje dvije vrste: "korisnik" uloga implementira kontrole na temelju uloga pristupa za korisnike i grupe koje zahtijevaju pristup resursu, dok je uloga "aplikacija" primjenjuje na isti način za [klijentske aplikacije](#client-application) koji zahtijevaju pristup.

Uloge su resursa definirani nizovi (na primjer "trošak odobravatelj", "Samo za čitanje", "Directory.ReadWrite.All"), upravljanih [Azure klasični portal] [ AZURE-classic-portal] putem resursa [manifestu aplikacije](#application-manifest), i pohranjene u resursa [appRoles svojstvo][AAD-Graph-Sp-Entity]. Azure klasični portal koristi se da bi korisnicima dodijelite uloge "korisnik" i konfiguriranje klijenta [aplikacije dozvole](#permissions) za pristup ulogu "aplikacija".

Detaljne rasprave uloge aplikacije koji prikazuje Azure AD grafikonu API potražite u članku [Opsega dozvola za API grafikonu][AAD-Graph-Perm-Scopes]. Primjer s detaljnim implementaciji potražite u članku [Kontrola pristupa u oblak aplikacije koje se koriste Azure AD na temelju uloga][Duyshant-Role-Blog].

## <a name="scopes"></a>dosega
Kao što su [uloge](#roles)opsega pružaju [resursa poslužitelja](#resource-server) upravljaju pristup svojih zaštićeni resursa. Opsega koriste se za implementaciju [utemeljen na opseg] [ OAuth2-Access-Token-Scopes] pristupa kontrole za [klijentski program](#client-application) koji je dodijelio ovlaštenog pristup resursu njegov vlasnik.

Dosezi su resursa definirani nizovi (na primjer "Mail.Read", "Directory.ReadWrite.All"), upravlja [Azure klasični portal] [ AZURE-classic-portal] putem resursa [manifestu aplikacije](#application-manifest), a pohranjene u resursa [oauth2Permissions svojstvo][AAD-Graph-Sp-Entity]. Azure klasični portal koristi se za konfiguriranje klijentskog računala [dozvole delegiranog](#permissions) da biste pristupili opseg.

Na najbolja praksa konvencija imenovanja, tako da pomoću oblika "resource.operation.constraint". Detaljne rasprave dosega koji prikazuje Azure AD grafikonu API potražite u članku [Opsega dozvola za API grafikonu][AAD-Graph-Perm-Scopes]. Opsega izložen servisima sustava Office 365 potražite u članku [Office 365 API dozvole reference][O365-Perm-Ref].

## <a name="security-token"></a>Sigurnosni token
Potpisani dokument koji sadrži zahtjevima, kao što su OAuth2 token ili SAML 2.0 pridruživanju. Za [autorizaciju dodijeliti](#authorization-grant)OAuth2, programa [access token](#access-token) (OAuth2) i [Token ID -a](OpenID Connect) su vrste sigurnosnih tokena, oba implementirana kao [JSON Web tokena (JWT)][JWT].

## <a name="service-principal-object"></a>Glavni objekt servisa
Kada ste register/ažuriranje aplikacija za [Azure klasični portal][AZURE-classic-portal], na portalu stvara/ažuriranja [objekt aplikacije](#application-object) i odgovarajuće objekt upravitelja servisa za taj klijent. Aplikacija objekt *definira* konfiguracija identitet aplikacije globalno (preko svih drugih korisnika kojem povezane aplikacije je dodijeljen pristup), te je predložak iz kojeg njegov odgovarajuće objekata glavni servis su *izvedena* za korištenje lokalno vrijeme izvođenja (u određenim klijenta).

U odjeljku [aplikacije i usluge objekata] [ AAD-App-SP-Objects] dodatne informacije.

## <a name="sign-in"></a>Prijava
Postupak [klijentske aplikacije](#client-application) pokretanje provjere autentičnosti za krajnjeg korisnika i hvatanje povezane stanje radi pri dohvaćanju [sigurnosni token](#security-token) i određivanje opsega sesiju aplikacije da bi to stanje. Navedite mogu sadržavati artefakte kao što su podatke korisničkog profila te informacije izvedene iz tokena zahtjevima.

Funkcija prijavu u aplikaciju obično koristi za implementaciju-jedinstvene prijave (SSO). Je možda i prethoditi "prijavi" funkcija, kao točku unosa za krajnjeg korisnika da biste pristupili aplikacije (prilikom prve prijave). Funkciju registracije služi za prikupljanje i dodatne stanje specifične za korisnika i dalje pojavljuje, a možda ćete morati [pristanak korisničkog](#consent).

## <a name="sign-out"></a>Sign-Out
Postupak nepotvrđen provjeru autentičnosti krajnjeg korisnika, odvajanje korisnikova stanja pridružene sesiju [klijentska aplikacija](#client-application) prilikom [prijave](#sign-in)

## <a name="tenant"></a>klijent
Instance komponente u imeniku Azure AD se nazivaju se klijent za Azure AD. Pruža mnoštvo značajkama, uključujući:

- servis registra za integrirane aplikacije
- Provjera autentičnosti korisničke račune i registriranih aplikacija
- OSTALE krajnje točke potrebne za podršku različite protokoli uključujući OAuth2 i SAML, uključujući [autorizacije krajnje točke](#authorization-endpoint), [tokena krajnjoj točki](#token-endpoint) i krajnju točku "uobičajenih" koriste [više klijentske aplikacije](#multi-tenant-application).

Klijent je povezan s Azure AD ili pretplatu na Office 365 tijekom dodjeljivanja pretplate, koja omogućuje identiteta i upravljanje pristup značajkama za pretplatu. Saznajte [Kako započeti klijent za Azure Active Directory] [ AAD-How-To-Tenant] detalje na različite načine možete pristupiti na klijent. Pogledajte [kako Azure pretplate pridružuju Azure Active Directory] [ AAD-How-Subscriptions-Assoc] pojedinosti o odnos između pretplate i klijent za Azure AD.

## <a name="token-endpoint"></a>tokena krajnje točke
Jedna od krajnje točke pomoću [provjeru autentičnosti poslužitelja](#authorization-server) za podršku OAuth2 [daje autorizacije](#authorization-grant). Ovisno o dodijelite, korištenja dobiti [pristup token](#access-token) (i povezane token "Osvježi") za [klijenta](#client-application)ili [token ID-a](#ID-token) kada se koristi s [OpenID povezivanje] [ OpenIDConnect] protokol.

## <a name="user-agent-based-client"></a>Korisnički agent utemeljen na klijent
Vrsta [klijentske aplikacije](#client-application) koja se preuzima kod s web-poslužitelj i izvršava unutar na korisnički agent (na primjer, u web-preglednik), kao što su na jednu stranicu aplikacija (SPA). Budući da se sav kod se izvršava na uređaju, smatra "javno" klijent zbog njegov nemogućnosti za pohranu vjerodajnica privatno/Povjerljivo. Potražite u članku [vrstama klijenata za OAuth2 i profili] [ OAuth2-Client-Types] više pojedinosti.

## <a name="user-principal"></a>Glavno ime korisnika
Slično kao objekt za glavni servisa koristi se za predstavljanje instance aplikacije, korisnički glavni je neku drugu vrstu sigurnosnih glavnica, što predstavlja korisnika. Azure AD grafikonu [entitet korisnika] [ AAD-Graph-User-Entity] definira shemu za korisničkom objektu, uključujući odnose na korisnike svojstva kao što su ime i prezime korisnikovo Glavno ime, članstvo u ulozi direktorija, itd. To omogućuje konfiguracija identitet korisnika za Azure AD uspostaviti Glavno ime korisnika pri izvođenju. Korisnikovo glavno koristi se za predstavljanje čija je autentičnost provjerena korisnika za jedinstvenu prijavu, snimanje [pristanak](#consent) delegiranje, čime odluke kontrole programa access, itd.

## <a name="web-client"></a>web-klijentu
Vrsta [klijentske aplikacije](#client-application) koja izvršava sva koda na web-poslužitelju i mogu funkcionirati kao "Povjerljivo" klijent spremanjem sigurno njegov vjerodajnice na poslužitelju. Potražite u članku [vrstama klijenata za OAuth2 i profili] [ OAuth2-Client-Types] više pojedinosti.

## <a name="next-steps"></a>Daljnji koraci
[Vodič za Azure AD programiranje] [ AAD-Dev-Guide] je portal za sve Azure AD razvoj povezane teme, uključujući i pregled [integraciju aplikacija] [ AAD-How-To-Integrate] i osnove [Azure AD provjeru autentičnosti i scenariji podržani provjere autentičnosti][AAD-Auth-Scenarios].

Slanje povratnih informacija i Pomozite nam da budemo sužavanje i oblika sadržaj, koristite sljedeći odjeljak Komentari Disqus.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
