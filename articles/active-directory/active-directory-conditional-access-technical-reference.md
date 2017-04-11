
<properties
    pageTitle="Azure Active Directory uvjetno pristup Tehničke reference | Microsoft Azure"
    description="S kontrolom uvjetno pristup Azure Active Directory provjerava određenim uvjetima koje ste odabrali prilikom provjere autentičnosti korisnika i prije omogućivanja pristupa aplikaciji. Kada su ti uvjeti zadovoljeni, korisnik je autentičnost provjerena i dopuštenje za pristup aplikaciji."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory uvjetno pristup Tehničke reference

## <a name="services-enabled-with-conditional-access"></a>Services omogućena za uvjetno programa access
Pravila uvjetnog pristup podržani su preko različitih vrsta Azure AD aplikacija. Ovaj popis sadrži:

- Pridruženi aplikacije iz galerije aplikacije Azure AD
- Lozinka aplikacijama iz galerije aplikacije Azure AD
- Aplikacija registriranim Proxy aplikacije Azure
- Razvijen retka tvrtke i više klijentske aplikacije registriranim Azure AD
- Visual Studio putem Interneta
- Aplikacija za Azure Remote
-   Dynamics CRM
- Yammer za Microsoft Office 365
- Microsoft Office 365 Exchange Online
- Microsoft Office 365 SharePoint Online (obuhvaća OneDrive za tvrtke)


## <a name="enable-access-rules"></a>Omogućivanje pristupa pravila

Svako pravilo možete ga omogućiti ili onemogućiti na na po baza aplikacije. Kada su pravila **o** oni će i omogućiti provesti za korisnike pristupa aplikacije. Kada su **ISKLJUČENO** oni će se koristiti i neće utjecati Prijava korisnika iskustvo.

## <a name="applying-rules-to-specific-users"></a>Primjena pravila na određene korisnike
Pravila se mogu primijeniti na određene skupove korisnika na temelju sigurnosne grupe postavljanjem **Primijeni na**. **Primijeni na** moguće je postaviti za **Sve korisnike** ili **grupe**. Kada postavite **Svim** korisnicima se pravila primjenjuju svi korisnici s pristupom aplikacije. Mogućnost **grupe** omogućuje određeni sigurnost i grupama za raspodjelu da biste odabrali, pravila samo provest će se za te grupe.

Kada implementirate pravilo, uobičajeno je da biste je najprije primijenili na ograničeno skup korisnika koji su članovi piloting grupa. Kada je dovršena pravilo možete primijeniti na **Sve korisnike**. Time će se pravilo provest će se za sve korisnike u tvrtki ili ustanovi.

Odabir grupe mogu biti izuzeti i iz pravila pomoću mogućnosti **osim** . Svi članovi ove grupe bit će izuzeti čak i ako se pojavljuju u uključene grupi.

## <a name="at-work-networks"></a>"Na poslu" mreža


Pravila za uvjetno pristup koji se koristi u mreži "na poslu" ovise o pouzdano rasponi IP adresa podešena u Azure AD ili njihovog korištenja zahtjeva "unutar corpnet" iz AD FS. Ta pravila obuhvaćaju sljedeće:

- Obavezan višestruka provjera autentičnosti kada nisu na poslu
- Blokiranje pristupa kada nisu na poslu

Mogućnosti za vrijednost određujući kotangens "na poslu" mreža

1. Konfiguriranje pouzdanih rasponi IP adresa na [stranici za konfiguraciju višestruke provjere autentičnosti](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Uvjetno pravilnik o pristupu koristit će konfiguriran raspone na svaki zahtjev za provjeru autentičnosti i tokena izdavanja za procjenu pravila. 
2. Konfiguriranje korištenje unutrašnje corpnet zahtjeva, ona može se koristiti s pridruženim direktorija, pomoću servisa AD FS. [Dodatne informacije o unutarnjim zahtjevima coronet](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Konfiguriranje javno rasponi IP adresa. Na kartici Konfiguriraj za direktorija, možete postaviti javnu IP adrese. Uvjetno Access će ih koristiti kao "na poslu" IP adrese, time je dodatne raspone može konfigurirati, iznad 50 IP adresa ograničenja za koje je postavio stranicu postavke MFA.



## <a name="rules-based-on-application-sensitivity"></a>Pravila koja se temelje na osjetljivost aplikacije

Pravila konfigurirane svaku aplikaciju dopuštanja visoke vrijednosti servisa za sigurnu vezu bez utjecaja pristup drugim servisima. Na kartici **Konfiguriranje** aplikacije moguće je konfigurirati pravila za uvjetno pristup. 

Trenutno je ponuđen pravila:

- **Obavezan višestruka provjera autentičnosti**
 - Svi korisnici koji se primjenjuje ovaj pravilnik bit će potrebno za provjeru autentičnosti putem barem jednom višestruke provjere autentičnosti.
 
- **Obavezan višestruka provjera autentičnosti kada nisu na poslu**
 - Ako se to pravilo primjenjuje, svi korisnici će se zatražiti da obavite višestruke provjere autentičnosti barem jednom pristup servisu s udaljenog mjesta koje nisu rad. Ako ih premjestite na poslu na udaljenom mjestu, oni će se morati izvršiti multifactor provjere autentičnosti prilikom pristupanja servis.
 
- **Blokiranje pristupa kada nisu na poslu** 
 - Kad korisnici premjestite s udaljenog mjesta, oni će se blokirati ako pravila "Blokiranje pristupa kada nisu na poslu" primjenjuje se na njih.  Oni će se ponovno dopuštenje za pristup pri radnu lokaciju.


## <a name="related-topics"></a>Povezane teme

- [Zaštita pristup sustavu Office 365 i drugih aplikacija s Azure Active Directory](active-directory-conditional-access.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
