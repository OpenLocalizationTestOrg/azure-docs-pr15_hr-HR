<properties
    pageTitle="Azure Active Directory B2C: Ograničenja i ograničenja | Microsoft Azure"
    description="Popis ograničenja i ograničenja s Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: Ograničenja i ograničenja

Postoji nekoliko značajki i radovi B2C Azure Active Directory (Azure AD) koji još nisu podržani. Mnoge od tih poznatim problemima i ograničenjima će se spomenuti odsad nadalje, ali imajte na umu da ih ako su gradnje potrošača dostupnog aplikacije koje se koriste Azure AD B2C.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Problemi prilikom stvaranja Azure AD B2C klijenata

Ako naiđete na probleme tijekom [stvaranja klijent za Azure AD B2C](active-directory-b2c-get-started.md), pročitajte članak [Stvaranje klijent za Azure AD ili klijent za Azure AD B2C – problemi i rješenja](active-directory-b2c-support-create-directory.md) za upute.

Imajte na umu da su tamo Poznati problemi kada izbrišete postojećem klijentu za B2C i ponovno stvoriti s istim nazivom domene. Morate stvoriti B2C klijenta s drugi naziv domene.

## <a name="note-about-b2c-tenant-quotas"></a>Napomena o kvotama B2C klijenta

Po zadanom će broj korisnika na klijentu B2C ograničeno je na 50.000 korisnika. Ako vam je potrebna Potenciranje kvota za vaš klijent B2C, obratite se podršci.

## <a name="branding-issues-on-verification-email"></a>Mogući problemi na provjere e-pošte

Zadani provjere e-pošte sadrži Microsoft brendiranje. Uklonit ćemo je u budućnosti. Zasad, možete je ukloniti pomoću [brendiranje značajka tvrtke](../active-directory/active-directory-add-company-branding.md).

## <a name="restrictions-on-applications"></a>Ograničenja aplikacije

Sljedeće vrste aplikacija trenutno nisu podržane u Azure AD B2C. Opis podržane vrste aplikacija u paketu [Azure AD B2C: vrste aplikacija](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Jedna stranica aplikacije (JavaScript)

Mnoge modernim aplikacije ste na jednu stranicu aplikacija (SPA) sučelja napisan prvenstveno JavaScript i često koristi se framework SPA kao što su AngularJS, Ember.js, Durandal itd. Ovaj tijek još nije dostupna u Azure AD B2C.

### <a name="daemons--server-side-applications"></a>Daemons / poslužiteljsko aplikacije

Programi koji sadrže dugoročnih procesa ili koji rade bez prisutnosti korisnika treba i način pristupa zaštićenim resurse, kao što su API-ji za Web. Te aplikacije možete provjeru autentičnosti i dobiti tokeni pomoću aplikacije identiteta (umjesto u korisničke ovlaštenog identiteta) u [OAuth 2.0 klijent vjerodajnice tijek](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). Ovaj tijek još nije dostupna u Azure AD B2C, da Zasad aplikacijama mogli primati tokeni tek kada je došlo je do interaktivnih potrošača prijavu tijek.

### <a name="standalone-web-apis"></a>API-ji Web samostalni

U Azure AD B2C imate mogućnost da biste [sastavili Web API koji je osigurani pomoću tokeni OAuth 2.0](active-directory-b2c-apps.md#web-apis). Međutim, taj API Web samo moći primati tokena iz klijenta koji se zajednički koristi na isti ID aplikacije. Stvaranje Web API-JA na kojoj se pristupa iz nekoliko različitih klijenata nije podržana.

### <a name="web-api-chains-on-behalf-of"></a>Web-nabavom API-JA (na-ime-od)

Mnoge arhitekturi obuhvaćaju API Web koji je potrebno da biste pozvali druge do API Web, oba osigurava Azure AD B2C. Taj se scenarij uobičajenih u izvorni klijentskim programima koji imaju Web API pozadini, koja naizmjence poziva Microsoftov mrežni servis kao što su API Azure AD grafikonu.

Scenarij ulančana Web API mogu podržavati pomoću dodijelite OAuth 2.0 Jwt nošenja vjerodajnice, u suprotnom naziva tijek uključeno-ime-od. Međutim, tijek uključeno-ime-od nije trenutno implementirana u Azure AD B2C.

## <a name="restriction-on-libraries-and-sdks"></a>Ograničenja biblioteke i SDK-ovi

Postavljanje Microsoft podržane biblioteka koje funkcioniraju Azure AD B2C trenutno vrlo ograničeni. Imamo podrške za .NET temelji web-aplikacije i servise, kao i NodeJS web-aplikacijama i servisima.  Imamo i biblioteci klijent .NET za pretpregled koja se naziva MSAL koji se mogu koristiti u sklopu Azure AD B2C u Windows i drugim aplikacijama za .NET.

Ne možemo trenutno imate biblioteke podržava sve jezike ili platforme, uključujući iOS i Android.  Ako želite da se nadograđuju različite platforme od onih spomenutih, preporučujemo korištenje SDK Otvori izvor, koje upućuju na našem [OpenID referenca protokol za povezivanje i OAuth 2.0](active-directory-b2c-reference-protocols.md) prema potrebi.  Azure AD B2C implementira OAuth & OpenID povezati, koji omogućuje korištenje generički OAuth ili povezivanje OpenID biblioteka za integraciju.

Naš iOS i vodiči za brzi početak rada sa sustavom Android pomoću Otvori izvor biblioteke koje ćemo testirate kompatibilnom sa Azure AD B2C.  Sve naše vodiče za Brzi start dostupne su u našem [Prvi koraci u](active-directory-b2c-overview.md#getting-started) sekciji.

## <a name="restriction-on-protocols"></a>Ograničenja protokola

Azure AD B2C podržava povezivanje OpenID i OAuth 2.0. Međutim, ne svim značajkama i mogućnostima svakog protokola ste implementirana. Da biste bolje razumjeli opseg podržanim protokolom funkcionalnosti u Azure AD B2C, pročitajte našu stranicu za [Povezivanje OpenID i OAuth 2.0 protokol referenca](active-directory-b2c-reference-protocols.md). SAML WS Fed protokol službe za podršku i nije dostupna.

## <a name="restriction-on-tokens"></a>Ograničenja tokena

Mnoge tokena koje izdaje Azure AD B2C implementirana kao JSON Web tokeni ili JWTs. Međutim, neke informacije koje se nalaze u JWTs (poznatom kao "zahtjevima") je prilično kao je trebao bi biti ili u cijelosti nedostaje. Primjeri obuhvaćaju "sub" i "preferred_username" zahtjevima.  Kao vrijednosti, oblik ili značenje zahtjevima promjene tijekom vremena, tokena postojeća pravila će ostati ostaju isti – možete oslanjate njihove vrijednosti u aplikacijama za proizvodnju.  Kao vrijednosti promijene, ne možemo steći ćete prilike da biste konfigurirali te promjene za svaku police.  Da biste bolje razumjeli tokeni čuje trenutno Azure AD B2C servis, pročitajte našu [tokena referenca](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Ograničenja ugniježđene grupe

Članstva ugniježđenih grupa nisu podržane u klijenata za Azure AD B2C. Ne možemo namjeravate dodajte tu mogućnost.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Ograničenja značajka razlikovno upita na grafikonu Azure AD API-JA

[Značajka razlikovno upita na grafikonu Azure AD API](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) nije podržana u klijenata za Azure AD B2C. Ovo je na našem Dugoročne vodič.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Problemi s Upravljanje korisnicima na portalu Azure klasični

B2C značajke nisu dostupne na portalu Azure. Međutim, koristite Azure klasični portal za pristup drugih značajki klijentu, uključujući i upravljanje korisnicima. Trenutno postoji nekoliko Poznati problemi vezani uz upravljanje korisnicima (kartica **korisnika** ) na portalu Azure klasični:

- Za lokalni račun korisnika (odnosno potrošača tko registrira s adrese e-pošte i lozinku, ili korisničko ime i lozinku), polje **Korisničko ime** ne odgovara prijavu identifikator (adresu e-pošte ili korisničko ime) koji je korišten prilikom registracije. To je zato što je polje prikazuje na portalu za Azure klasični zapravo korisnika glavni ime (UPN-a), koji se koristi u B2C scenarijima. Da biste pogledali identifikator za prijavu u lokalni račun, pronađite korisničkom objektu u [Programu Explorer grafikonu](https://graphexplorer.cloudapp.net/). Vidjet ćete isti problem s korisnikom društvenih račun (odnosno potrošača tko registrira sa servisa Facebook, Google +, itd.), ali u tom slučaju postoji bez prijave identifikator govori o.

    ![Lokalni račun - UPN-a](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Za lokalni račun korisnika, koji će se ne mogu uređivati nijedno polje i spremiti promjene na kartici **profil** .

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Problemi s administrator pokreće za vraćanje izvorne lozinke na portalu Azure klasični

Ako ponovno postavljanje lozinke za lokalni račun utemeljen potrošača Azure klasični portala ( **Ponovno postavljanje lozinke** naredbe na kartici **korisnici** ) te potrošača nećete moći da biste promijenili svoj lozinke prilikom sljedeće prijave, ako koristite znak prema gore ili prijavite se u pravilo, a će se zaključati iz aplikacije. Kao zaobilazno rješenje, koristite [Azure AD grafikonu API](active-directory-b2c-devquickstarts-graph-dotnet.md) za ponovno postavljanje lozinke u potrošača (bez Istek lozinke) ili koristite znak pravila umjesto znak prema gore ili prijavite se u pravila.

## <a name="issues-with-creating-a-custom-attribute"></a>Problemi sa stvaranjem prilagođeni atribut

U [Prilagođeni atribut dodali na portalu za Azure](active-directory-b2c-reference-custom-attr.md) nije odmah stvoren u klijentu B2C. Morat ćete pomoću prilagođeni atribut u najmanje jednoj pravilnika za nju stvaraju u klijentu B2C i postaju dostupne putem API grafikonu.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Problemi s potvrdom domene na portalu Azure klasični

Trenutno ne može potvrditi domenu uspješno na [Azure klasični portal](https://manage.windowsazure.com/).

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Problemi s prijavom u pravilima MFA u preglednicima Safari

Zahtjevi za prijavu pravilnike (MFA uključena) nije uspjelo povremeno u preglednicima Safari s pogreškama 400 HTTP (neispravan zahtjev). Ovo je rok ograničenja veličine niskog kolačića, Safari. Postoji nekoliko načina da zaobiđete ovaj problem:

- Koristite "registracije ili prijave pravilnik" umjesto "prijavu pravila".
- Smanjite broj **zahtjevima aplikacije** Tražena u pravilo.
