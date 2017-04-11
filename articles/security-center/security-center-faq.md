<properties
   pageTitle="Centar za sigurnost Azure najčešća pitanja | Microsoft Azure"
   description="U ovim najčešćim Pitanjima navedeni odgovori na pitanja o centru za sigurnost Azure."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Najčešća pitanja o centru za sigurnost Azure

U ovim najčešćim Pitanjima navedeni odgovori na pitanja o Azure centar za sigurnost, servis koji olakšava sprječavanje, otkrivanje i odgovaranje na prijetnji s bolje vidio u i kontrolu nad sigurnost resursi za Microsoft Azure.

## <a name="general-questions"></a>Općenita pitanja

### <a name="what-is-azure-security-center"></a>Što je centar za sigurnost Azure?
Centar za sigurnost Azure pomaže spriječiti, otkrivanje i odgovoriti prijetnji s bolje vidio u i kontrolu nad sigurnost Azure resurse. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

### <a name="how-do-i-get-azure-security-center"></a>Kako nabaviti centar za sigurnost Azure?
Centar za sigurnost Azure je omogućeno za vašu pretplatu na Microsoft Azure i pristupiti s [portala za Azure](https://azure.microsoft.com/features/azure-portal/). ([Prijavite se na portal sustava](https://portal.azure.com), odaberite **Pregledaj**i pomaknite se do **Centar za sigurnost**).  

## <a name="billing"></a>Naplata

### <a name="how-does-billing-work-for-azure-security-center"></a>Kako naplate posla za centar za sigurnost Azure?
Centar za sigurnost je ponuđen u dvije razine: besplatni i standardne.

Besplatni sloju omogućuje vam da biste postavili sigurnosne pravilnike i primanje sigurnosnih upozorenja, kupljenih i preporuke za koje će vas voditi kroz postupak konfiguriranja potrebnih kontrola. Pomoću besplatnog sloju možete pratiti i stanje sigurnosti Azure resurse i partnerskih rješenja integriran s pretplatom Azure.

Standardni sloju nudi u dlp-a besplatne sloju značajke plus napredne: prijetnju obavještavanje, behavioral analizu, rušenje analizu i značajkom prepoznavanja. Dostupna je besplatnu probnu verziju 90 dana od standardne sloju. Da biste nadogradili, odaberite cijene sloju [Sigurnosna pravila](security-center-policies.md#setting-security-policies-for-subscriptions). U odjeljku [Centar za sigurnost cijene](security-center-pricing.md) da biste saznali više.

## <a name="data-collection"></a>Prikupljanje podataka

Centar za sigurnost prikuplja podatke iz virtualnim strojevima da bi se procijenite njihovo stanje sigurnost, navedite preporuke o sigurnosti i upozorava vas na prijetnji. Kada pristupite centru za sigurnost, prikupljanje podataka je omogućena na svim računalima virtualne u svoju pretplatu. Prikupljanje podataka preporučuje, ali koje možete onemogućivanju, [Onemogućivanje prikupljanje podataka](#how-do-i-disable-data-collection) u centru za sigurnost pravila.

### <a name="how-do-i-disable-data-collection"></a>Kako onemogućiti prikupljanje podataka?

**Prikupljanje podataka** možete onemogućiti pretplate u sigurnosna pravila u bilo kojem trenutku. ([Prijava na portal za Azure](https://portal.azure.com), odaberite **Pregledaj**, odaberite **Centar za sigurnost**i odaberite **pravilnik**.)  Kad odaberete pretplatu, otvara se novi plohu i pruža mogućnost da biste isključili **Prikupljanje podataka** . Odaberite mogućnost **Izbriši agenata** na gornjoj vrpci da biste uklonili agenata iz postojeće virtualnim računalima.

> [AZURE.NOTE] Sigurnosna pravila možete postaviti na razini Azure pretplate i razina grupe resursa, ali morate odabrati pretplate da biste isključili prikupljanje podataka.

### <a name="how-do-i-enable-data-collection"></a>Kako omogućiti prikupljanje podataka?
Prikupljanje podataka možete omogućiti za vaše Azure subscription(s) u sigurnosna pravila. Da biste omogućili prikupljanje podataka, [prijavite se na portal za Azure](https://portal.azure.com)odaberite **Pregledaj**, odaberite **Centar za sigurnost**i odaberite **pravila**. Postavite **Prikupljanje podataka** **na** i konfiguriranje računa za pohranu gdje želite da se podaci prikupljeni u (u odjeljku pitanje "[Moji podaci pohrane?](#where-is-my-data-stored)"). Kada je omogućen **Prikupljanje podataka** , ga automatski prikuplja informacije o konfiguraciji i događaja sigurnosti iz svih podržanih virtualnim strojevima u pretplatu.

> [AZURE.NOTE] Sigurnosna pravila možete postaviti na razini Azure pretplate i razina grupe resursa, ali konfiguracije prikupljanje podataka koji se pojavljuje na razini pretplate samo.

### <a name="what-happens-when-data-collection-is-enabled"></a>Što se događa kada je omogućen prikupljanje podataka?
Prikupljanje podataka omogućeno je putem agenta za nadzor Azure i nastavak Azure sigurnost nadzor. Proširenje Azure sigurnost nadzor skenira za različite konfiguracija odgovarajuće sigurnosti i šalje u kašnjenja [Događaj praćenje za Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Nadalje, operacijski sustav stvara unose u zapisnik događaja.  Agent za nadzor Azure čita unose u zapisnik događaja i ETW prati i kopira ih na račun za pohranu za analizu.  To je prostor za pohranu račun konfiguriran u sigurnosna pravila. Dodatne informacije o računu za pohranu potražite u članku pitanje "[Moji podaci pohrane?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Ne proširenje Agent za praćenje i nadzor sigurnosti utječe na performanse Moje poslužitelji?
Agent i nastavak troši nominal količinu sistemske resurse i mora imati učinka na performanse.

### <a name="where-is-my-data-stored"></a>Gdje se pohranjuju moje podatke?
Za svako područje koja vam omogućuje virtualnim strojevima pokrenut, odaberite račun za pohranu koji se pohranjuju podaci prikupljeni s tim virtualnim strojevima. To olakšava zadržati podatke u istom zemljopisnom području izjave o zaštiti privatnosti i svrhe samostalnosti podataka. Odaberite račun za pohranu za pretplatu u sigurnosna pravila. ([Prijava na portal za Azure](https://portal.azure.com), odaberite **Pregledaj**, odaberite **Centar za sigurnost**i odaberite **pravilnik**.) Kada kliknete na pretplatu, otvorit će se novi plohu. Odaberite **računi prostora za pohranu za odabir** da biste odabrali područje.

> [AZURE.NOTE] Sigurnosna pravila možete postaviti na razini Azure pretplate i razina grupe resursa, ali odabir područja za vaš račun za pohranu pojavljuje na razini pretplate samo.

Da biste saznali više o Azure prostora za pohranu i račune za pohranu, potražite u [Dokumentaciji za pohranu](https://azure.microsoft.com/documentation/services/storage/) i [račune za pohranu o Azure](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Centar za sigurnost Azure

### <a name="what-is-a-security-policy"></a>Što je sigurnosna pravila?
Sigurnosna pravila definira skup kontrola koje su preporučena resursi unutar navedena pretplata ili grupu resursa. U centru za sigurnost Azure definirate pravilnike za Azure pretplate i grupa resursa prema sigurnosne preduvjete koje je vaša tvrtka i vrstu aplikacije ili osjetljivosti podatke iz pretplate.

Ako, na primjer, resursa koji se koriste za razvoj ili test možda različite sigurnosne preduvjete koje je od onih koje koristi za proizvodnju aplikacije. Isto tako, aplikacija pomoću regulated podataka kao što su PII (Personally Identifiable Information) može zahtijevati više razine sigurnosti. Sigurnosna pravila omogućena u centru za sigurnost Azure će pogon preporuke o sigurnosti i nadzor. Da biste saznali više o sigurnosnim pravilnicima, potražite u članku [Sigurnost stanja u centru za sigurnost Azure za nadzor](security-center-monitoring.md).

> [AZURE.NOTE] U slučaju sukoba između razina pravila pretplate i razine pravila grupe za resursa, razine pravila grupe resursa prednost.

### <a name="who-can-modify-a-security-policy"></a>Tko možete izmijeniti sigurnosna pravila?
Sigurnosna pravila konfigurirana je za svaku pretplatu ili grupu resursa. Da biste izmijenili pravila sigurnosti na razini pretplate ili razina grupe resursa, morate biti vlasnik ili suradničke tu pretplatu.

Da biste saznali kako konfigurirati pravila zaštite, potražite u članku [pravila sigurnosne postavke u centru za sigurnost Azure](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Što je preporuka za sigurnost?
Centar za sigurnost Azure analizira stanja sigurnosti Azure resurse. Kada potencijalni slabe prepoznaju, stvaraju se preporuke. Preporuke će vas voditi kroz postupak konfiguriranja potrebne kontrole. Primjeri su:

- Dodjeljivanje od antimalware da biste lakše identificirati i ukloniti zlonamjernog softvera
- Konfiguriranje [Mreže sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) i pravila za kontrolu promet na virtualnim računalima
- Dodjeljivanje vatrozida za aplikaciju web da biste lakše obrane protiv napada ciljnog web-aplikacije
- Implementacija nedostaju ažuriranja sustava
- Adresiranje OS konfiguracije koje odgovaraju preporučene referente vrijednosti

Samo preporuke omogućene sigurnosne pravilnike prikazane su u nastavku.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Kako vidjeti trenutno stanje sigurnosti Moje Azure resursa?
Pločicu **Resursi stanja** na **Centar za sigurnost** plohu prikazuje cjelokupni posture sigurnost vašeg okruženja raščlanjuju virtualnih računala, web-aplikacije i ostale resurse. Svaki resurs ima programa pokazatelja prikazuje ako identificirali sve potencijalne slabe. Klikom na stanje pločicu resursi prikazuje resurse i određuje koje je potrebno obratiti pozornost ili možda postoje problemi.

### <a name="what-triggers-a-security-alert"></a>Što se pokreće sigurnosno upozorenje?
Centar za sigurnost Azure automatski prikuplja, analizira i fuses podaci iz zapisnika iz Azure resursa, mreže i partnerskih rješenja kao što su antimalware i vatrozida. Kada se otkriju prijetnji, stvara se sigurnosno upozorenje. Primjeri otkrivanje:

- Ugrožena virtualnim strojevima komunikaciju s poznatim zlonamjerni IP adrese
- Napredni zlonamjernog softvera otkriven pomoću izvješćivanje o pogreškama u sustavu Windows
- Brute prisilno napada protiv virtualnim strojevima
- Sigurnosnih upozorenja iz integrirani partnerskih rješenja sigurnosti kao što su zlonamjernog ili vatrozidima aplikaciju za Web

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Koja je razlika između prijetnji provjeravati i upozorenja na putem Microsoft Security Response Center nasuprot centar za sigurnost Azure?
Microsoft sigurnost odgovor centra (MSRC) izvodi odaberite sigurnost nadzor Azure mreže i Infrastruktura i prima prijetnju obavještavanje i zloupotreba pritužbe iz trećim stranama. Kad postane MSRC umu da podatke o klijentu pristupanja strana nezakonitom ili neovlaštenih ili da kupca korištenje Azure nije u skladu s uvjete za korištenje prihvatljiva, sigurnost incidenta upravitelja obavještava je korisnik odabrao. Obavijest o obično događa slanjem poruke e-pošte da biste kontakte sigurnost naveden u centru za sigurnost Azure ili vlasnik Azure pretplate ako kontakta sigurnost nije naveden.

Centar za sigurnost je Azure usluga koje kontinuirano nadzire na korisnikova okruženja za Azure i primjenjuje analytics za automatsko otkrivanje širokog raspona potencijalno štetne aktivnosti. Ove dlp-a kada su povučete kao sigurnosnih upozorenja na nadzornoj ploči centar za sigurnost.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Kako se upravlja dozvole u centru za sigurnost Azure?
Centar za sigurnost Azure podržava pristupa na temelju uloga. Da biste saznali više o kontrola pristupa na temelju uloga (RBAC) u Azure, potražite u članku [Utemeljen na Azure Active Directory uloga kontrola pristupa](../active-directory/role-based-access-control-configure.md).

Kada korisnik Otvori centar za sigurnost, samo preporuke i upozorenja koja se odnose na resurse korisnik ima pristup će se prikazivati. To znači da korisnici vidjet će samo stavke koje se odnose na resurse gdje korisnika je dodijeljena uloga vlasnika, suradnik ili čitač pretplatu ili grupu resursa koji pripada resurs.

Ako je potrebno:

- **Uređivanje pravila zaštite**, morate biti vlasnik ili suradničke pretplate.
- **Primijeni i preporuke**, morate biti vlasnik ili suradničke pretplate.
- **Imate uvid u stanja sigurnosti na svim svojim pretplatama**, mora biti programa vlasnik, suradnik ili čitač (administrator, tim sigurnost) svake pretplate.
- **Imate uvid u stanja sigurnosti resursa**, mora biti grupu resursa vlasnika, suradnik ili čitač (DevOps).

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Odabir resursa koji Azure nadzire Centar za sigurnost Azure?
Centar za sigurnost Azure nadzire u sljedećim resursima Azure:

- Virtualnim strojevima (VMs) (uključujući [Servise u Oblaku](../cloud-services/cloud-services-choose-me.md))
- Azure virtualne mreže
- Servisa Azure SQL
- Partnerskih rješenja integriran s pretplatom Azure kao što je aplikacija vatrozid web na VMs i [Okruženja aplikacije servisa](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtualnim strojevima

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Koje će se vrste virtualnim strojevima podržane?
Nadzor stanja sigurnosti i preporuke dostupni su za virtualnim strojevima (VMs) stvorene pomoću oba [Klasični i resursima implementacije modela](../azure-classic-rm.md).

Podržani Windows VMs:

- Windows Server 2008 R2
- Windows Server 2012.
- Windows Server 2012 R2

Podržani Linux VMs:

- Ubuntu verzije 12.04, 14.04, 16.04
- Debian verzije 7, 8
- CentOS verzije 6. \*, 7.*
- Crvena je vaša Enterprise Linux (RHEL) verzije 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) verzije 11. \*, 12.*
- Oracle Linux verzije 6. \*, 7.*

Podržani su i VMs koji se izvodi u oblaku. Samo oblak services uloge web i tempiranja u radnog slobodnih provjeravaju. Da biste saznali više o servisu u oblaku, potražite u članku [Pregled servise u Oblaku](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Zašto se Azure centar za sigurnost ne prepoznaje rješenje antimalware radi na moj VM Azure?

Centar za sigurnost Azure sadrži samo uvid u antimalware instalira putem Azure proširenja. Na primjer, centar za sigurnost nije uspio pronaći antimalware koji je bio unaprijed instaliran na sliku koje ste naveli ili ako ste instalirali antimalware na virtualnim računalima sustava pomoću vlastite procesa (kao što su konfiguracija sustava za upravljanje).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Zašto se poruka "Nema skeniranje Data" za moj VM?

Može potrajati neko vrijeme (obično manje od jednog sata) za pregled podataka za popunjavanje nakon prikupljanje podataka omogućen u centru za sigurnost Azure. Preglede se će popuniti za VMs u prestao stanju.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Zašto se javlja poruka "VM Agent nedostaje?"

Agent za VM na VMs mora biti instaliran da biste omogućili prikupljanje podataka. Agent za VM se instalira prema zadanim postavkama za VMs koji su raspoređeni iz trgovine Azure Marketplace. Informacije o instalaciji VM Agent na druge VMs potražite u članku na blogu [VM Agent i proširenja](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
