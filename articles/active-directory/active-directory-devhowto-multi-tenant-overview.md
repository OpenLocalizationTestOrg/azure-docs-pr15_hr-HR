<properties
   pageTitle="Sastavljanje aplikacije koje mogu se prijaviti u bilo koji korisnik Azure Active Directory | Microsoft Azure"
   description="Korak po korak upute za stvaranje aplikacije koje mogu se prijaviti u korisnika iz bilo kojeg Azure Active Directory klijent, poznata i kao više klijentske aplikacije."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Kako se prijaviti u bilo koji korisnik Azure Active Directory (AD) pomoću uzorak više klijentske aplikacije
Ako nude softverski kao servisne aplikacije za mnoge tvrtke ili ustanove, možete konfigurirati aplikacije da biste prihvatili znak dodataka iz bilo kojeg Azure AD klijenta.  U Azure AD Ovo se zove upućivanje vaš klijent više aplikacija.  Korisnici u bilo kojem klijentu Azure AD moći će se prijaviti u aplikaciju nakon pristali na to da biste koristili svoj račun s aplikacijom.  

Ako imate postojeću aplikaciju koja ima vlastiti sustav računa ili podržava druge vrste prijava s drugih davatelja oblaka, dodavanje Azure AD za prijavu iz bilo kojeg klijenta jednostavan je Registracija aplikacije, dodavanje znak u kodu putem OAuth2, povezivanje OpenID ili SAML i stavlja je prijava s gumbom Microsoft aplikacije. Kliknite gumb da biste saznali više o vizualnom identitetu vaše aplikacije.

[! [Prijavite se u gumb] [AAD-prijavu]][AAD-App-Branding]


U ovom se članku pretpostavlja da ste već upoznati s stvara jednu klijentske aplikacije za Azure AD.  Ako niste, lakši vratili na [početnu stranicu vodič za razvojne inženjere] [ AAD-Dev-Guide] i isprobajte jedan od naše brzog pokretanja!

Postoje četiri jednostavnih koraka da biste pretvorili vaše aplikacije u Azure AD više klijentske aplikacije:

1.  Ažuriranje aplikacije registracije biti više klijenta
2.  Ažuriranje kod da biste Pošalji zahtjeve za/Common krajnje točke 
3.  Ažuriranje kod za rukovanje višestruke vrijednosti izdavač
4.  Razumijevanje pristanak korisnika i administratora i unesite odgovarajuću šifru promjene

Pogledajmo svakog koraka detaljno. Također možete se odmah baciti na [ovom popisu više klijentu uzorci][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Ažuriranje registracije biti više klijenta
Prema zadanim postavkama web app/API registracije u Azure AD su jednog klijenta.  Možete unijeti svoju registraciju više klijentu pronalaženjem parametar "Aplikacija je više klijent" na stranici Konfiguracija svoju registraciju aplikacija [Azure klasični portal] [ AZURE-classic-portal] i postavljanje na "Da".

Napomena: Prije aplikacije više klijent, Azure AD potreban je ID URI aplikacije aplikacije globalno jedinstven. ID URI aplikacija je jedan od načina aplikacija je označena poruka protokol.  Za aplikacije u jednom klijentu je dovoljni za ID URI aplikacija biti jedinstvena u klijentu.  Za više klijentske aplikacije, mora biti globalno jedinstveni da bi Azure AD možete pronaći aplikacije preko svih drugih korisnika.  Globalni jedinstvenosti postavio je potrebno ID URI aplikacije da bi se naziv glavnog računala koja odgovara provjerene domene Azure AD klijenta.  Ako, na primjer, ako ime klijenta sustava je contoso.onmicrosoft.com, a zatim valjani aplikacije ID URI bio `https://contoso.onmicrosoft.com/myapp`.  Ako vaš klijent provjereno domenu `contoso.com`, zatim valjan URI ID aplikacije bi `https://contoso.com/myapp`.  Postavljanje aplikacije kao više klijent neće uspjeti ako ID URI aplikacija ne poštuje ovaj uzorak.

Registracija za nativni klijent su više klijentu prema zadanim postavkama.  Nije potrebno ništa poduzimati da biste s lokalnog računala Registracija više klijenta.

## <a name="update-your-code-to-send-requests-to-common"></a>Ažuriranje kod zahtjeva za slanje/Common
U aplikaciji za jednog klijenta zahtjeve za prijavu šalju se u klijent za prijavu krajnje točke.   Na primjer, za contoso.onmicrosoft.com krajnju točku bio sljedeći:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Zahtjevi za šalju se u klijentu krajnje mogu se prijaviti u korisnici (ili Gosti) u klijentu za aplikacije u klijentu.  Aplikacijom više klijentske aplikacije ne zna unaprijed koji klijent korisnik potječe, tako da se ne može poslati zahtjeva za krajnje točke na klijentu.  Umjesto toga zahtjeva za koju se šalju krajnje multiplexes preko svih Azure AD klijenata:

    https://login.microsoftonline.com/common

Kada Azure AD primi zahtjev na na/Common krajnje točke, on se korisnik prijavi i kao consequence otkrije koji klijent korisnik potječe iz.  Na/uobičajenih krajnjoj točki radi sa svim protokole provjere autentičnosti koje podržava Azure AD: povezivanje OpenID, OAuth 2.0, SAML 2.0 i WS Federacija.

Znak kao odgovor na aplikaciju pa sadrži token koji predstavlja korisnika.  Vrijednost izdavača u token govori aplikacije koje klijentu korisnik potječe iz.  Kada vraća odgovor na/Common krajnje točke, vrijednost izdavača u token će odgovarati korisnika klijentu.  Važno je da se obratite pažnju na/Common krajnje točke nije klijenta i nije je izdavač, je samo u multiplexer.  Prilikom korištenja/Common logike u aplikaciji za provjeru valjanosti tokena mora se ažurirati da bi taj račun. 

Kao što je rečeno ranije, više klijentske aplikacije i mora sadržavati prijavu dosljednost za korisnike, pratiti aplikacije Azure AD brendiranje smjernice. Kliknite gumb da biste saznali više o vizualnom identitetu vaše aplikacije.

[! [Prijavite se u gumb] [AAD-prijavu]][AAD-App-Branding]

Pogledajmo na korištenje na/Common krajnjoj točki i implementaciju kod detaljnije.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Ažuriranje kod za rukovanje višestruke vrijednosti izdavač
Web-aplikacije i na webu API-ji primati i tokena iz Azure AD za provjeru valjanosti.  

> [AZURE.NOTE] Dok nativni klijentske aplikacije zahtjev i primanje tokeni Azure AD, mogu učiniti da biste poslali API-ji, gdje se provjeriti.  Izvorni aplikacije provjeru tokeni i morate ih tretira kao neprozirne.

Pogledajmo pri kako će se aplikacija Provjeri valjanost tokeni primi iz Azure AD.  Jedan klijentske aplikacije obično se vrijednost krajnju točku kao što su:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

i koristiti za sastavljanje URL metapodataka (u ovom slučaju OpenID povezivanje) kao što su:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

Da biste preuzeli dva ključnih dijelovi informacija koje se koriste za provjeru valjanosti tokena: klijentu je prijava ključeva i izdavatelj vrijednost.  Svaki klijent Azure AD ima vrijednost jedinstveni izdavač obrasca:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

pri čemu je vrijednost GUID Preimenuj sigurnih verzija klijenta ID klijenta.  Ako kliknete vezu metapodataka iznad za `contoso.onmicrosoft.com`, vidjet ćete tu vrijednost izdavača u dokumentu.

Kad jednom klijentske aplikacije Provjeri valjanost token, provjerava potpis token protiv tipki potpisa iz dokumenta metapodataka i provjerava je li vrijednost izdavača u token odgovara onome koji pronađen u dokumentu metapodataka.

Nakon na/Common krajnje točke ne odgovaraju na klijent, a ne na izdavača kada pregledate izdavač vrijednost u metapodataka za/uobičajeni je s predloškom URL umjesto stvarnih vrijednosti:

    https://sts.windows.net/{tenantid}/

Zbog toga više klijentske aplikacije ne može provjeriti valjanost tokeni pomoću odgovarajuće vrijednosti izdavača u metapodacima s na `issuer` vrijednost u token.  Više klijentske aplikacije mora logike ne odlučite vrijednosti koje izdavač vrijede i koji su, ovisno o klijentu ID dio vrijednost izdavač.  

Na primjer, ako više klijentske aplikacije samo dopušta prijava s određenim drugih korisnika koji su se prijavili za svoje servis, pa ga morate potvrdite izdavač vrijednost ili `tid` zahtjeva vrijednost u token da biste bili sigurni da je taj klijent u svoj popis pretplatnika.  Ako više klijentske aplikacije samo bavi osobe, a ne odluke sve pristupa na temelju klijenata, pa je možete zanemariti vrijednost izdavač potpuno.

U više klijentu uzorci pronaći ćete u odjeljku [Povezani sadržaj](#related-content) na kraju ovog članka, provjere valjanosti izdavač je onemogućen da biste omogućili sve Azure AD klijent za prijavu.

Sada Pogledajmo korisničko sučelje za korisnike koji su prijave u više klijentske aplikacije.

## <a name="understanding-user-and-admin-consent"></a>Razumijevanje korisnika i dozvole za administratore
Za korisnika da biste se prijavili aplikaciju u Azure AD aplikacije morate predstavljeni u klijentu za korisnika.  Time se omogućuje tvrtki ili ustanovi radnje kao jedinstveni pravilnike primijenite korisnike iz svoje klijenta za prijavu u aplikaciju.  Jedan klijentske aplikacije u ovom Registracija je jednostavno; je onaj koji se događa kada se registrirate aplikacije [Azure klasični portal][AZURE-classic-portal].

Za više klijentske aplikacije, Početna registracije za aplikaciju nalazi se u klijentu Azure AD koji se koriste za razvojne inženjere.  Kada korisnik iz različitih klijenta prijavi aplikaciju prvi put, Azure AD vas pita da pristanak dozvola zatražio aplikacije.  Ako oni pristanak, pa će se prikaz aplikacije pod nazivom *servis glavnog* stvorene u klijentu korisnika, a možete nastaviti prijava. Delegiranje stvara se i u direktoriju zapisa korisnika pristanak za aplikaciju. U odjeljku [aplikacije i usluge objekte] [ AAD-App-SP-Objects] pojedinosti o aplikacije aplikacije i ServicePrincipal objekata i kakav je odnos između međusobno povezani.

![Pristajete jednoslojnih aplikacije][Consent-Single-Tier] 

Sučelje za pristanak utječe dozvole zatražio aplikacije.  Azure AD podržava dvije vrste dozvole samo za aplikacije i ovlaštenog:

- Delegirana dozvola daje aplikaciju možete učiniti mogućnost poslužiti kao traje Prijava korisnika za podskup stvari korisnika.  Ako, na primjer, možete dodijeliti aplikacije ovlaštenog dozvolu za čitanje kalendara traje Prijava korisnika.
- Programa dozvole samo za aplikaciju je dodijelio izravno identitet aplikacije.  Ako, na primjer, aplikaciju možete dodijeliti dozvole samo za aplikacije za čitanje na popis korisnika na klijentu, a će moći to bez obzira na to koji se prijavili u aplikaciju.

Neke dozvole može biti consented da biste prema redoviti korisnik dok neke su potrebne dozvole administratora klijenta. 

### <a name="admin-consent"></a>Dozvole za administratore
Dozvole samo za aplikacije potreban je uvijek pristanak administrator klijenta.  Ako aplikacija zahtjeve za dozvole samo za aplikacije i običan korisnik pokuša da biste se prijavili aplikaciju, aplikacija će dobiti poruku o pogrešci u kojoj piše da korisnik ne uspije pristanak.

Određene ovlaštenog dozvole zahtijevaju i pristanak administrator klijenta.  Na primjer, mogućnost da biste odgovorili Azure AD kao prijavljeni korisnik zahtijeva pristanak administrator klijenta.  Kao što su dozvole samo za aplikaciju, ako svakodnevne korisnik pokuša da biste se prijavili aplikaciju koji zahtijeva ovlaštenog dozvola koje je potrebno odobrenje administratora aplikacija će primiti pogrešku.  Je li potrebna je dozvola administratorske dozvole ovisi o programiranje objavljenog resursa, a može pronaći u dokumentaciji za resurs.  Veza na temu koja opisuje dozvole dostupne za Azure AD grafikonu API-JA i Microsoft Graph API su u odjeljku [Povezani sadržaj](#related-content) ovog članka.

Ako aplikacija koristi dozvole koje zahtijevaju administratorske dozvole, morate koristiti geste u aplikaciji kao što je gumb ili vezu gdje administrator može započeti akciju.  Zahtjev za aplikaciju šalje za ovu akciju je uobičajeni OAuth2/OpenID povezivanje zahtjeva provjeru autentičnosti, ali koji sadrži i na `prompt=admin_consent` parametra niza upita.  Nakon što administrator ima pristanak i glavnicu servisa stvara se korisnik klijenta, naknadna prijava zahtjeva nije potrebna na `prompt=admin_consent` parametar.   Od administratora odlučio su potrebne dozvole prihvatljiva, drugim korisnicima u klijentu će se tražiti pristanak od tog trenutka prema naprijed.

Na `prompt=admin_consent` parametar može se koristiti u aplikacijama u kojima se traže dozvole koje zahtijevaju administratorske dozvole, ali želite dati iskustvo gdje administrator klijenta "registrira" za aplikaciju jedanput pa nema korisnika će se zatražiti pristanak od tog trenutka na.

Ako aplikacija zahtijeva administratorske dozvole, a administrator se prijavi na aplikaciju, ali u `prompt=admin_consent` parametar ne šalju, administrator moći uspješno pristanak na aplikaciju, ali će se samo pristanak na svoj korisnički račun.  Obični korisnici neće moći prijava i pristanak na aplikaciju.  To je korisno ako želite omogućiti administrator klijenta Istražite aplikacije prije nego što drugim korisnicima programa access.

Administrator klijenta možete onemogućiti običnog korisnicima pristanak aplikacijama.  Ako je onemogućen tu mogućnost, administrator pristanak uvijek potreban je za aplikaciju za postavljanje na klijentu.  Ako želite da biste testirali aplikacija uz pristanak redoviti korisnik onemogućena, možete pronaći parametar konfiguraciju u klijentu za Azure AD sekciji konfiguracija [Azure klasični portal][AZURE-classic-portal].

> [AZURE.NOTE] Neke aplikacije želite pruža gdje Obični korisnici će moći pristanak na početku i kasnije aplikaciju može obuhvaćati administrator i zatražite dozvole za koje je potrebno administratorske dozvole.  Ne postoji način da biste to učinili s jedne aplikacije registracija u Azure AD danas.  Na nadolazeće Azure AD v2 krajnje omogućuje aplikacije dozvole prilikom izvođenja, umjesto u trenutku Registracija koji će se omogućiti scenarij.  Dodatne informacije potražite u članku [Vodič za razvojne inženjere v2 modela aplikacija za Azure AD][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Dozvole i više razina aplikacijama
Aplikacija može imati više razine, svaka predstavlja svoju registraciju u Azure AD.  Na primjer, matičnoj aplikaciji koja poziva web API ili web-aplikacije poziva API web-mjesto.  U oba slučaja klijenta (nativni aplikacije ili web-aplikacije) zahtjeva za dozvole da biste nazvali resursa (API na webu).  Za klijent za biti uspješno pristanak u klijentu za klijenta, svih resursa na koji je zahtjeva za dozvole mora postojati u korisnik klijenta.  Ako ovaj uvjet nije zadovoljen, Azure AD će vratiti pogrešku da resurs mora dodati prvi put.

To može biti problem ako logičke aplikacija sastoji se od dva ili više računala registracije, primjerice zasebnom klijenta i resursa.  Kako prenijeti resursa u klijentu kupca prvi?  Azure AD pokriva taj slučaj omogućivanjem klijenta i resursa biti pristanak u jednom koraku, gdje će se korisnik ne vidi ukupni zbroj dozvole zatražio klijenta i resursa na stranici pristanak.  Da biste omogućili tu funkciju, Registracija aplikacija resursa mora sadržavati ID aplikacije klijenta kao na `knownClientApplications` u manifestu aplikacije.  Ako, na primjer:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Ovo svojstvo se može ažurirati putem resursa [aplikacije manifest][AAD-App-Manifest], a planirati više razina nativni klijent pozivanje uzorka API na webu u sekciji [Povezani sadržaj](#related-content) na kraju ovog članka. Dijagramu u nastavku sadrži pregled pristanak za aplikaciju s više razina:

![Pristanak aplikaciju poznati klijenta s više razina][Consent-Multi-Tier-Known-Client] 

Slične slučaja se događa ako različite razine aplikacije bilježe se u različitim korisnicima.  Na primjer, razmotrite kutije izgradnje nativni klijentska aplikacija koja se poziva na Office 365 Exchange Online API-JA.  Za razvoj lokalnog aplikaciju i noviji za matičnoj aplikaciji da biste pokrenuli u klijentu za klijenta, Exchange Online glavnicu usluge mora postojati.  U ovom slučaju klijenta mora kupiti Exchange Online za servis glavni će biti stvoren u svoje klijentu.  Slučaju API ugrađeni tvrtka ili ustanova koja nije Microsoft, programiranje na API treba omogućuje za svoje kupce u pristanak svoje aplikacije u klijentu za klijenta, primjerice web-stranica pogoni pristanak pomoću mehanizme opisane u ovom članku.  Nakon stvaranja glavnicu servisa u klijentu matičnoj aplikaciji se tokeni za na API-JA.

Dijagramu u nastavku sadrži pregled pristanak za aplikaciju s više razina u različitim korisnicima:

![Pristanak više sudionika aplikaciju s više razina][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Opozivati dozvole
Pristanak za aplikacije u bilo kojem trenutku možete opozvati korisnicima i administratorima:

- Korisnici pristup pojedinačne aplikacije opozvati tako da ih uklonite iz [Aplikacije programa Access ploča] [ AAD-Access-Panel] popis.
- Administratori opozvati pristup aplikacijama tako da ih uklonite iz Azure AD pomoću sekciji [Azure klasični portal]za upravljanje Azure AD[AZURE-classic-portal].

Ako administrator identifikacijskom u aplikaciju za sve korisnike u klijent, korisnici ne možete pojedinačno opozvati pristup.  Samo administrator možete opozvati pristup i samo za cijelu aplikaciju.

### <a name="consent-and-protocol-support"></a>Dozvole i podrška protokola
Pristanak podržava Azure AD putem OAuth, OpenID povezati, WS Federacija i protokoli za SAML.  Protokoli SAML i WS Federacija ne podržavaju na `prompt=admin_consent` parametar, pa pristanak administrator je samo putem OAuth i povezivanje OpenID.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Više klijentske aplikacije i predmemoriranja pristupna tokena
Pristupna tokena da biste uputili poziv API-ji koji nisu zaštićeni Azure AD možete pristupati i više klijentske aplikacije.  Česta pogreška kada u Active Directory provjera autentičnosti biblioteke (ADAL) pomoću više klijentske aplikacije je prethodno zatražiti token za korisnika pomoću/Common, dobivate odgovor, a zahtjev kasnije tokena za taj isti korisnik također koristi/common.  Budući da odgovor Azure AD ne dolaze klijent, / uobičajene, ADAL predmemorira token kao iz klijenta. Sljedeći poziv/Common da biste dobili pristupni token za korisnika pronalaženja u predmemoriji stavka predmemorije, a od korisnika će se zatražiti da ponovno se prijavite.  Da biste izbjegli nedostaje predmemorije, provjerite je li naknadni pozivi već prijavljeni korisnik nastaju na krajnjoj točki na klijentu.

## <a name="related-content"></a>Srodni sadržaji

- [Aplikacija više klijentu uzorci][AAD-Samples-MT]
- [Brendiranje smjernice za aplikacije][AAD-App-Branding]
- [Vodič za Azure AD programiranje][AAD-Dev-Guide]
- [Aplikacija i servisa glavnicu objekata][AAD-App-SP-Objects]
- [Integriranje aplikacija sa Azure Active Directory][AAD-Integrating-Apps]
- [Pregled Framework pristanak][AAD-Consent-Overview]
- [Microsoft Graph API dozvola dosega][MSFT-Graph-AAD]
- [Opsega dozvola Azure AD grafikonu API-JA][AAD-Graph-Perm-Scopes]

Slanje povratnih informacija i Pomozite nam da budemo sužavanje i oblika sadržaj, koristite odjeljak Komentari Disqus ispod.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














