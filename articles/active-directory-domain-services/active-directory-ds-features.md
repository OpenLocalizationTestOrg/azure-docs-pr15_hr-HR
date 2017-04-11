<properties
    pageTitle="Azure Active Directory Domain Services: Značajke | Microsoft Azure"
    description="Značajke programa Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure servisa Active Directory Domain Services

## <a name="features"></a>Značajke
Sljedeće značajke dostupne su u Azure AD domenske servise koji se upravlja domene.

- **Iskustvo jednostavne uvođenja:** Možete omogućiti Azure AD domenske servise za vaš klijent Azure AD pomoću samo nekoliko klikova. Bez obzira na to jesu li vaš klijent Azure AD je klijentu oblaka ili sinkroniziraju s lokalnog imenika, upravljanih domene možete dodjeli brzo.

- **Podrška za domenu spoja:** Možete jednostavno domene spoj računala Azure virtualne mreže upravljanih domene dostupan je u. Iskustva domene spoj na klijenta za Windows i poslužitelja operacijski sustavi radi Integracija s domenama kvalificiranom Azure servisa Active Directory Domain Services. Možete koristiti i automatiziranog domene spoj tooling protiv takve domene.

- **Jednu domenu instanci po Azure AD direktorija:** Možete stvoriti jednu domene servisa Active Directory za svaki Azure AD direktorij.

- **Stvaranje domene pomoću prilagođenog naziva:** Možete stvoriti domene s prilagođenog naziva (na primjer, "contoso100.com") pomoću Azure servisa Active Directory Domain Services. Možete koristiti neki nazivi provjereni ili Neprovjereni domena. Po želji možete i stvoriti domene s nastavak ugrađene domene (to jest, "*. onmicrosoft.com') nudi Azure AD direktorija.

- **Integriran s Azure AD:** Ne morate konfigurirati i upravljanje replikacije za Azure AD domenske servise. Korisnički računi, članstva u grupi i korisničke vjerodajnice (lozinke) iz imenika Azure AD su automatski dostupan u Azure AD domenske servise. Nove korisnike, grupe ili promjene atributa s klijentom Azure AD ili iz lokalnog imenika automatski sinkronizirati sa Azure servisa Active Directory Domain Services.

- **NTLM i Kerberos provjere autentičnosti:** S podrškom za provjeru autentičnosti NTLM i Kerberos možete implementirati aplikacije koje se temelje na integriranu provjeru autentičnosti sustava Windows.

- **Koristite svoje službene vjerodajnice/lozinke:** Lozinke za korisnike u klijentu za Azure AD raditi s Azure servisa Active Directory Domain Services. Korisnici mogu pomoću vjerodajnica za tvrtke na računalima domene spoj, prijavite se u interaktivno ili putem udaljene radne površine i provjeru autentičnosti upravljanih domene.

- **LDAP vezanja & LDAP čitati podrška:** Možete koristiti aplikacije koje se temelje na LDAP povezuje za provjeru autentičnosti korisnika u domenama kvalificiranom Azure servisa Active Directory Domain Services. Uz to, aplikacije koje koriste operacije čitanja LDAP atribute korisnika/računalo upit iz direktorija možete raditi i protiv Azure servisa Active Directory Domain Services.

- **Sigurne LDAP (LDAPS):** Možete omogućiti pristup direktoriju putem sigurne LDAP (LDAPS). Sigurna LDAP pristup dostupna unutar virtualne mreže prema zadanim postavkama. Možete, međutim, možete omogućiti sigurnog pristupa LDAP putem Interneta.

- **Pravilima grupe:** Za korisnike i računala možete koristiti jedan ugrađene GPO svaki spremnike da bi osigurale poštivanje potrebne sigurnosne pravilnike za korisničke račune i domene pridruženo računala.

- **Upravlja DNS-om:** Članovi grupe 'AAD Kontroler administratori' možete upravljati DNS za domenu upravljanih pomoću poznatih alata za administraciju DNS kao što je dodatak za BLOG Administracija DNS.

- **Stvoriti prilagođene Organizacijska jedinica (organizacijske jedinice):** Članovi grupe "AAD Kontroler administratori" možete stvoriti prilagođeni organizacijske jedinice u upravljanih domene. Ti korisnici odobravaju punim administratorskim ovlastima putem prilagođene organizacijske jedinice, pa ih možete Dodavanje i uklanjanje računa servisa, računala, a zatim grupe itd unutar te prilagođene organizacijske jedinice.

- **Dostupno u više Azure regijama:** Posjetite stranicu [servisa Azure po regijama](https://azure.microsoft.com/regions/#services/) znati Azure regije u kojoj je dostupan Azure servisa Active Directory Domain Services.

- **Visoke dostupnosti:** Azure servisa Active Directory Domain Services nudi visoke dostupnosti za svoju domenu. Ova značajka omogućuje garancije veći vrijeme aktivnosti servisa i resilience na pogreške. Ugrađeni stanja praćenje ponuda automatski olakšava od pogrešaka putem adrese e gore nove instance da biste zamijenili nije uspjelo slučajeve i nastavak usluga za svoju domenu.

- **Koristiti alate za upravljanje poznatih:** Pomoću poznatih alata za upravljanje Windows Server Active Directory kao što je Active Directory Administrativni centar sustava ili Active Directory PowerShell administriranje upravljanog domene.
