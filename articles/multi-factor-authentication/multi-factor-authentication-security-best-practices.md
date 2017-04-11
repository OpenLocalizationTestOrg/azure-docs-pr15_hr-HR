<properties 
    pageTitle="Sigurnost Praktični savjeti za korištenje Azure MFA"
    description="Ovaj dokument sadrži najbolje prakse oko Azure MFA pomoću računa za Azure"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Sigurnost Praktični savjeti za korištenje Azure višestruka provjera autentičnosti s računima za Azure AD

Kada je riječ o poboljšanju proces provjere autentičnosti, višestruka provjera autentičnosti je željeni odabir za većinu tvrtki ili ustanova. Azure višestruke provjere autentičnosti (MFA) omogućuje tvrtki da bi odgovarao njihove preduvjeti za sigurnost i usklađenost tijekom pružanja na jednostavne sučelje za prijavu za svoje korisnike. U ovom se članku obrađuje najbolje postupke koje ste morate imati na umu prilikom planiranja prihvaćanja Azure MFA.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Najbolje prakse za Azure višestruke provjere autentičnosti u oblaku
Da biste mogli bi svi vaši korisnici višestruka provjera autentičnosti i iskoristite prednosti prošireni značajke koje nudi Azure višestruke provjere autentičnosti, morat ćete omogućiti Azure višestruke provjere autentičnosti na svim svojim korisnicima.  To možete napraviti pomoću nešto od sljedećeg:

- Azure AD Premium ili mobilnost paket Enterprise
- Davatelja usluge provjere autentičnosti višestruke provjere

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Paket mobilnost Azure AD Premium i Enterprise

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Prvi korak preporučenih za usvojio Azure MFA u oblak pomoću Azure AD Premium ili mobilnost paket Enterprise je da biste korisnicima dodijelili licence.  Azure višestruka provjera autentičnosti je dio te pakete i kao tvrtke ili ustanove ne morate ništa dodatne da biste proširili mogućnost višestruke provjere autentičnosti za sve korisnike.

Kada postavljanje višestruke provjere autentičnosti uzeti u obzir sljedeće:

- Nije potrebno da biste stvorili davatelja za provjeru autentičnosti višestruke provjere.  Azure AD Premium i mobilnost paket Enterprise u sklopu Azure višestruke provjere autentičnosti.  Ako stvorite davatelja provjere autentičnosti ne može se dvostruki naplatiti.
- Azure AD Connect preduvjet je snimanje samo ako sinkronizirate lokalnog okruženja za Active Directory sa u imeniku Azure AD. Ako koristite samo u imeniku Azure AD koja će se sinkronizirati s lokalnim instance komponente Active Directory, ne morate Azure AD Connect.


### <a name="multi-factor-auth-provider"></a>Davatelja usluge provjere autentičnosti višestruke provjere

![Davatelja usluge provjere autentičnosti višestruke provjere](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Ako nemate Azure AD Premium ili mobilnost paket Enterprise pa u prvom koraku preporučenih za usvojio Azure MFA u oblaku je da biste stvorili davatelja usluge provjere autentičnosti MFA. Iako MFA dostupna je prema zadanim postavkama za globalne administratore koji imaju Azure Active Directory, kada implementirate MFA za tvrtku ili ustanovu ćete morati proširiti mogućnost višestruka provjera autentičnosti svim korisnicima i koje su vam potrebne davatelja provjere autentičnosti višestruke provjere.
Kada odaberete davatelja usluge provjere autentičnosti, morate odaberite imenik i Imajte na umu sljedeće:

- Ne morate u imeniku Azure AD da biste stvorili davatelja za provjeru autentičnosti višestruke provjere.
- Morat ćete pridružiti davatelja provjere autentičnosti višestruke provjere u imeniku Azure AD ako želite produljiti višestruke provjere autentičnosti za sve korisnike i/ili želite da vaše globalnog administratora da biste mogli iskoristiti sve značajke kao što su portal za upravljanje, prilagođene čestitke i izvješća.
- DirSync ili AAD Sync su samo zahtjeva ako sinkronizirate lokalnog okruženja za Active Directory sa u imeniku Azure AD. Ako koristite samo u imeniku Azure AD koja će se sinkronizirati s lokalnim instance komponente Active Directory, ne morate DirSync ili AAD Sync.
- Ako imate Azure AD Premium ili mobilnost paket Enterprise, nije potrebno da biste stvorili davatelja za provjeru višestruke provjere autentičnosti. Što je potrebno da biste korisniku dodijelili licencu, a zatim možete početi s uključivanjem MFA za korisnike.
- Provjerite jeste li odaberite model desnom korištenje za tvrtke (po provjere autentičnosti ili po korisniku omogućeni), kad odaberete korištenje modela ga nije moguće promijeniti.

### <a name="user-account"></a>Korisnički račun
Prilikom omogućivanja MFA za korisnike, korisničke račune može biti u jednom od tri stanja core: onemogućena, omogućena ili provesti.
Slijedite smjernice u nastavku da biste bili sigurni da koristite mogućnost najprikladniji za implementaciju sustava:

- Kada je stanje korisnika postavljeno onemogućeno, korisnik ne koristi višestruke provjere autentičnosti. To je zadano stanje.
- Kada korisnika stanje postavljen na omogućena, to znači da korisnik je omogućen, ali nije dovršena postupak registracije. Će se zatražiti da biste dovršili postupak na sljedeći prijava. Ta postavka ne utječe na aplikacije koje nisu preglednika. Sve aplikacije nastavit će funkcionirati dok ne završi postupak registracije.
- Kada korisnika stanje postavljen na nametnuti, to znači da korisnik može ili bila registracije. Ako su dovršite postupak registracije zatim koriste višestruke provjere autentičnosti. U suprotnom korisnika će se zatražiti da biste dovršili postupak registracije na sljedeći prijava. Ta postavka utječe na aplikacije koje nisu preglednika. Ove aplikacije neće funkcionirati dok se ne stvara i koristi aplikacije lozinke.
- Pomoću predloška obavijesti za korisnika u članku [Uvod u rad s Azure višestruke provjere autentičnosti u oblaku](multi-factor-authentication-get-started-cloud.md) da biste poslali poruku e-pošte korisnika vezana uz prihvaćanja MFA.

### <a name="supportability"></a>Pružanje podrške

Budući da se većina korisnika navikli u lozinkama samo za provjeru autentičnosti, važno je vaša tvrtka Premjesti Informiranost svim korisnicima vezane uz taj proces. U ovom Informiranost možete smanjiti vjerojatnost koje korisnici će poziva službi za podršku za manje probleme vezane uz MFA.
No postoje neke scenarije u kojima je potrebno privremeno onemogućiti MFA. Slijedite smjernice u nastavku da biste razumjeli kako rukovati tih scenarija:

- Provjerite je li obučavanju osoblje za tehničku podršku članova na scenarije ručicu gdje mobilne aplikacije ili telefon ne prima obavijest ili telefonskom pozivu i zbog toga korisnik ne može se prijaviti. Omogućuju jednokratni zaobilazak mogućnost da biste korisniku za provjeru autentičnosti jednom vremenskom po "zaobilaženje" višestruke provjere autentičnosti. Na zaobilazak privremeni i istječe nakon navedenog broja sekundi.
- Ako je potrebno, na raspolaganju mogućnost pouzdana IP-ovi u Azure MFA. Ova značajka omogućuje administratorima upravljanih ili vanjski klijent mogućnost da biste zaobišli višestruke provjere autentičnosti za korisnike koji su prijave u Lokalni intranet tvrtke. Značajke dostupne su za Azure AD korisnicima koji imaju Azure AD Premium, paket Enterprise mobilnost ili Azure višestruka provjera autentičnosti licenci.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Najbolje prakse za Azure višestruka provjera autentičnosti lokalnog
Ako vaša tvrtka ipak odražava vlastitu infrastrukture da biste omogućili MFA, bit će potrebno za implementaciju sustava Azure višestruke provjere autentičnosti informacije o lokalnom poslužitelju. Poslužitelj za MFA komponente prikazuju se u dijagramu u nastavku:

![Davatelja usluge provjere autentičnosti multi-factor](./media/multi-factor-authentication-security-best-practices/server.png)
*prema zadanim postavkama nije instaliran ** instaliran ali nije omogućeno prema zadanim postavkama


Azure višestruke provjere autentičnosti poslužitelja može se koristiti za zaštitu oblaka resurse i lokalnog resursa koje je moguće pristupiti putem računa za Azure AD.  No to može samo napraviti pomoću vanjski pristup.  To jest, morate AD FS i imate li ga vanjskog s klijentom Azure AD.
Kada postavljanje višestruke provjere autentičnosti poslužitelja uzeti u obzir sljedeće:

- Ako ste zaštita resurse Azure AD pomoću Active Directory Federation Services, a zatim se izvodi 1st faktor provjere autentičnosti lokalnog pomoću servisa AD FS i 2 faktor je izvesti lokalnog honoring zahtjev.
- Nije preduvjet da Azure višestruku provjeru autentičnosti poslužitelja biti instalirana na poslužitelj za vanjski pristup za AD FS Međutim višestruke provjere autentičnosti prilagodnik za AD FS mora biti instalirana na programa Windows Server 2012 R2 pokretanje AD FS. Možete instalirati na poslužitelj na nekom drugom računalu dok god je podržanu verziju i instalirati prilagodnik za AD FS zasebno na poslužitelj za vanjski pristup za AD FS. Potražite u članku postupak upute o instaliranju prilagodnik zasebno.
- Čarobnjak za instalaciju višestruka provjera autentičnosti AD FS prilagodnik stvara sigurnosnu grupu naziva PhoneFactor administratori u servisu Active Directory i dodaje račun servisa AD FS servisa za vanjski pristup ovu grupu. Preporučuje se da vam provjerite je li na upravljaču domene grupi administratori PhoneFactor uistinu stvorili i da AD FS račun za servis je član grupe. Ako je potrebno, dodajte račun servisa AD FS u grupu administratori PhoneFactor na upravljaču domene ručno.

### <a name="user-portal"></a>Portal za korisnika
Ovaj portal pokreće se u na Internet Information Server (IIS) web-mjesto, koja omogućuje samouslužne mogućnosti i daje cijeli skup mogućnosti za administriranje korisnika. Konfiguriranje komponente, slijedite smjernice u nastavku:

- Potreban je IIS 6 ili noviji
- ASP.NET v2.0.507207 moraju biti instaliran i registriran
- Ovaj poslužitelj može uvesti u mreži opseg.



### <a name="app-passwords"></a>Aplikacija lozinki
Ako vaša tvrtka ili ustanova pridružene jedinstvene Prijave pomoću Azure AD i namjeravate koristiti Azure MFA, zatim Imajte na umu sljedeće prilikom korištenja lozinki aplikacije (Imajte na umu da to se odnosi samo na vanjsko (SSO) koristi):

- Lozinka aplikacije potvrde Azure AD i stoga zaobilazi vanjski pristup. Vanjski pristup koristi se samo prilikom postavljanja aplikacije lozinku.
- Za vanjsko (SSO) lozinke korisnika će se spremiti u polju id tvrtke ili ustanove. Ako korisnik napusti tvrtku, te informacije mora tijek organizacijske ID-a pomoću DirSync u stvarnom vremenu. Onemogućivanje brisanja račun može proći do 3 sata da biste sinkronizirali, što Onemogući/brisanje lozinku aplikacije u Azure AD.
- Postavke za kontrolu pristupa klijenta lokalnog su snazi aplikacije lozinkom
- Provjere autentičnosti lokalnog prijava / nadzora mogućnost dostupna je za aplikaciju lozinke
- Za klijentskog programa Microsoft Lync 2013 potreban je više Obrazovanje krajnjeg korisnika.
- Za određene napredne arhitektonski dizajna možda ćete morati pomoću kombinacije tvrtke ili ustanove korisničkog imena i lozinke i aplikacije lozinke prilikom korištenja višestruka provjera autentičnosti s klijentima, ovisno o tome gdje su provjeru autentičnosti. Za klijente provjeru autentičnosti infrastruktura za lokalni, koristit ćete tvrtke ili ustanove korisničko ime i lozinku. Za klijente provjeru autentičnosti Azure AD, koristit ćete lozinku aplikacije.
- Prema zadanim postavkama, korisnici ne mogu stvoriti aplikaciju lozinke, ako vaša tvrtka mora ili ako je potrebno dopustiti korisnicima da biste stvorili lozinku aplikacije u nekim slučajevima, provjerite je li odabrana mogućnost Dopusti korisnicima da biste stvorili lozinke aplikacije da biste se prijavili u aplikacije koje nisu preglednika.

## <a name="additional-considerations"></a>Dodatne napomene
Koristite na popisu u nastavku da biste razumjeli neke Dodatne napomene i najbolje prakse za svaku komponentu koja će biti implementiran lokalnog:

Način|Opis
:------------- | :------------- |
[Servisa Active Directory Federation](multi-factor-authentication-get-started-adfs.md)|Informacije o postavljanju višestruka provjera autentičnosti Azure AD fs.
[POLUMJER Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  Informacije o instalaciju i konfiguriranje Azure MFA Server s POLUMJER.
[Provjera autentičnosti IIS](multi-factor-authentication-get-started-server-iis.md)|Informacije o instalaciju i konfiguriranje Azure MFA Server s IIS-om.
[Windows Authenticaton](multi-factor-authentication-get-started-server-windows.md)|  Informacije o instalaciju i konfiguriranje Azure MFA Server pomoću provjere autentičnosti sustava Windows.
[Provjera autentičnosti LDAP](multi-factor-authentication-get-started-server-ldap.md)|Informacije o instalaciju i konfiguriranje Azure MFA Server s provjerom autentičnosti LDAP.
[Udaljene radne površine pristupnik i pomoću POLUMJER na poslužitelj za Azure višestruke provjere autentičnosti](multi-factor-authentication-get-started-server-rdg.md)|  Informacije o instalaciju i konfiguriranje Azure MFA Server s udaljene radne površine pristupnika pomoću POLUMJER.
[Sinkronizacija s Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)|Informacije o instalaciju i konfiguriranje sinkronizacije između servisa Active Directory i poslužitelj za Azure MFA.
[Implementacija Azure višestruka provjera autentičnosti poslužitelja mobilne aplikacije web-usluge](multi-factor-authentication-get-started-server-webservice.md)|Informacije o instalaciju i konfiguriranje web-servisa server Azure MFA.
[Konfiguracija naprednih VPN-a pomoću Azure višestruka provjera autentičnosti](multi-factor-authentication-advanced-vpn-configurations.md)|Informacije o konfiguriranju Cisco ASA, Citrix Netscaler i sigurne VPN tvrtke Juniper/servisom Pulse aparata pomoću LDAP ili POLUMJER.


## <a name="additional-resources"></a>Dodatni resursi
Dok je u ovom se članku ističe najbolje postupke za Azure MFA, postoje ostale resurse koji vam može poslužiti prilikom planiranja implementaciju sustava MFA. Na popisu u nastavku sadrži neke ključne članaka koji vam mogu pomoći tijekom postupka:

- [Izvješća u Azure višestruka provjera autentičnosti](multi-factor-authentication-manage-reports.md)
- [Postavljanje okruženje za Azure višestruka provjera autentičnosti](multi-factor-authentication-end-user-first-time.md)
- [Najčešća pitanja vezana uz Azure višestruka provjera autentičnosti](multi-factor-authentication-faq.md)
