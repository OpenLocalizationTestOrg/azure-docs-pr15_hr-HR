<properties
    pageTitle="Vodič za sigurnost Azure prostora za pohranu | Microsoft Azure"
    description="Detalji o mnogo načina zaštita Azure prostor za pohranu, uključujući ostalog RBAC, šifriranje servis za pohranu, klijentsko šifriranja, SMB 3.0 i šifriranje Azure."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Vodič za sigurnost Azure prostora za pohranu

##<a name="overview"></a>Pregled

Azure prostora za pohranu nudi opsežan skup sigurnosne mogućnosti koji zajedno omogućiti inženjerima omogućuje stvaranje sigurnog aplikacije. Račun za pohranu sam mogu zaštiti pomoću na temelju uloga kontrola pristupa i Azure Active Directory. Podatke možete zaštićenim na putu između aplikacije i Azure pomoću [Klijentsko šifriranja](storage-client-side-encryption.md), HTTPS ili SMB 3.0. Podatke možete postaviti automatski šifriranje kada zapisan Azure prostora za pohranu pomoću [Prostora za pohranu servisa šifriranja (SSE)](storage-service-encryption.md). OS i podataka koristi virtualnim strojevima diskova moguće je postaviti šifriranje pomoću [Šifriranja Azure Disk](../security/azure-security-disk-encryption.md). Delegirana pristup objektima podataka u spremište Azure se mogu dodijeliti pomoću [Zajednički pristup potpisi](storage-dotnet-shared-access-signature-part-1.md).

U ovom članku pronaći ćete pregled svaka od tih značajki sigurnosti koje se mogu koristiti u sklopu Azure prostora za pohranu. Veze su navedene članaka koji steći pojedinosti svake značajke tako da možete lako koristiti dodatno istrage na svaku temu.

Evo tema da biste se prekriveno u ovom članku:

-   [Upravljanje ravnini sigurnost](#management-plane-security) – zaštita računa za pohranu

    Upravljanje ravnini sastoji se od resursa koji se koriste za upravljanje računa za pohranu. U ovom ćete odjeljku ćemo objasniti što model implementacije Voditelj resursa Azure te o tome kako na temelju uloga kontrole pristupa (RBAC) za kontrolu pristupa koristite za pohranu računi. Ne možemo će i razgovarati o upravljanju ključeva za račun za pohranu i kako ih Obnovi.

-   [Podaci ravnini sigurnost](#data-plane-security) – zaštita pristupa podacima

    U ovom ćete odjeljku smo ćete pogledajte dopuštanja pristupa objekata stvarnih podataka na vašem računu za pohranu, kao što su blob-ova, datoteke, redovi i tablice, pomoću zajednički pristup potpisa i spremljene pravilnike za pristup. Ne možemo obrađuje SAS razini usluge i SAS razinom računa. Ne možemo prikazat će se i kako se ograničava pristup određene IP adrese (ili raspon IP adresa), kako ograničiti protokol koji se koristi za HTTPS i kako opozvati pristup potpis zajedničko korištenje bez čekanja da bi isteći.

-   [Šifriranje na putu](#encryption-in-transit)

    U ovom se odjeljku opisuje kako zaštite podataka kada prenosite pojavljivanje ili iščezavanje Azure prostora za pohranu. Ćemo objasniti što preporučuje korištenje HTTPS i šifriranje koristi SMB 3.0 za Azure zajedničke datoteke. Ne možemo, će bacite pogled na klijentskoj strani šifriranja koja omogućuje šifriranje podataka prije nego što se prenosi u prostor za pohranu u klijentskoj aplikaciji i dešifrirati podatke nakon prenose se više mjesta za pohranu.

-   [Šifriranje na ostale](#encryption-at-rest)

    Će ćemo objasniti prostora za pohranu servisa šifriranja (SSE) te kako možete omogućiti je za račun za pohranu, rezultira blob polja blok, stranice blob-ova i dodavanje blob polja koja se automatski šifrirane kada zapisan Azure prostora za pohranu. Ne možemo će i pogledajte kako možete koristiti šifriranje Azure i Istražite osnovne razlike i slučajeva šifriranje nasuprot SSE nasuprot klijentsko šifriranje. Ne možemo će ukratko pogledajte FIPS usklađenost za računala vlada SAD-a.

-   Korištenje [Analytics za pohranu](#storage-analytics) za nadzor pristupa Azure prostora za pohranu

    U ovom se odjeljku opisuje kako pronaći informacije u zapisnicima analytics za pohranu za zahtjev za. Ne možemo će se pogledajte realni prostora za pohranu analize podataka u zapisniku i Saznajte kako stupiti u vezu li zahtjeva upućuje se s ključ računa za pohranu, u potpisom zajedničko korištenje programa Access ili anonimno i hoće li je uspjelo ili nije uspjelo.

-   [Omogućivanje utemeljenima na pregledniku klijente pomoću CORS](#Cross-Origin-Resource-Sharing-CORS)

    U ovom se odjeljku govori o kako omogućiti unakrsno origin resursa zajedničko korištenje (CORS). Ćemo objasniti što domenama access i kako rukovati s mogućnostima CORS ugrađenu Azure prostora za pohranu.

##<a name="management-plane-security"></a>Upravljanje ravnini sigurnosti

Upravljanje ravnini sastoji se od operacije koje utječu na račun za pohranu sam. Na primjer, možete stvoriti ili brisanje računa za pohranu, dobit ćete popis račune za pohranu u pretplatu, dohvatiti tipki račun za pohranu ili obnovi račun tipki za pohranu.

Kada stvorite novi račun za pohranu, odaberite model implementacije klasičnu ili Voditelj resursa. Klasični modela stvaranja resursa u Azure samo omogućuje all-or-nothing pristup pretplate, a zatim uključite, računa za pohranu.

Ovaj vodič usredotočuje se na model Voditelj resursa koji je preporučeni način za stvaranje računa za pohranu. S na računima Voditelj resursa za pohranu, umjesto koji omogućuje pristup cijelu pretplatu, možete upravljati pristupom na razinu više konačne u ravnini upravljanja pomoću na temelju uloga kontrole pristupa (RBAC).

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Upute za sigurnu pohranu račun s uslugom na temelju uloga kontrole pristupa (RBAC)

Ćemo objasniti što je RBAC i kako ga koristiti. Azure pretplate ima Azure Active Directory. Korisnike, grupe i aplikacijama iz taj imenik možete dobiti pristup za upravljanje resursima u Azure pretplate koje koriste model implementacije Voditelj resursa. To se naziva na temelju uloga kontrole pristupa (RBAC). Da biste upravljali taj pristup, možete koristiti [Azure portal](https://portal.azure.com/), [Alati za Azure EŽA](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)ili [Azure prostora za pohranu resursa davatelja REST API -ji](https://msdn.microsoft.com/library/azure/mt163683.aspx).

S modelom resursima stavljate račun za pohranu u resursa grupa te upravljanje pristupom ravnini upravljanje tog računa određene prostora za pohranu pomoću servisa Azure Active Directory. Na primjer, možete dati određene korisnike mogućnost pristupa računu tipki za pohranu dok drugi korisnici možete vidjeti podatke o računu za pohranu, ali ne možete pristupiti tipki račun za pohranu.

####<a name="granting-access"></a>Dopuštanje pristupa

Pristup moguć je dodjelom odgovarajuću ulogu RBAC korisnike, grupe i aplikacije, u desnom dosegu. Da biste dodijelili pristup cijelu pretplatu, dodijelite uloge na razini pretplate. Možete dati pristup svim resursa u grupu resursa dodjelu dozvola grupi resursa sam. Možete dodijeliti i određene uloge na određene resurse, kao što su računi za pohranu.

Ovo su glavni točke koje su vam potrebne informacije o korištenju RBAC da biste pristupili operacije Upravljanje spremištem Azure računa:

-   Kada dodijelite pristup, zapravo dodijelite uloge poslovni kontakt koji želite imati pristup. Možete upravljati pristupom postupaka koji se koriste za upravljanje taj račun za pohranu, ali ne objekata podatke na računu. Na primjer, možete dodijeliti dozvole za dohvaćanje svojstva računa za pohranu (kao što su zalihosti), ali ne i spremnik ili podataka u spremniku unutar spremište blobova platforme.

-   Za nekoga da imaju dozvolu pristupa podacima objekata u račun za pohranu možete mu dodijeliti dozvolu za čitanje računa tipki za pohranu, a taj korisnik možete koristiti te tipke za pristup blob-ova, redove, tablice i datoteke.

-   Uloge se mogu dodijeliti određeni korisnički račun, grupe korisnika ili za određenu aplikaciju.

-   Svaka uloga ima popis akcije i ne akcije. Na primjer, ima ulogu suradnika virtualnog računala akciju s "listKeys" koji omogućuje tipki račun za pohranu za čitanje. U suradničke ima "Ne akcije" kao što su ažuriranje pristupa za korisnike u servisu Active Directory.

-   Uloge za pohranu sadrže (ali nisu ograničeni na) na sljedeći način:

    -   Vlasnik – ih možete upravljati sve, uključujući programa access.

    -   – Ih može obavljati suradnik ništa vlasnik osim Dodjela pristupa. Osoba koja ima ta uloga možete pogledati i obnovi račun tipki za pohranu. S računom ključevima za pohranu mogu pristupiti objekata podataka.

    -   Čitač – mogu vidjeti podatke o računu za pohranu, osim tajne. Na primjer, ako nekome dodijelite uloge s dozvolama za čitanje na računu za pohranu, prikazivanje svojstava računa za pohranu, ali ne možete unesite željene promjene u svojstva ili prikaz računa tipki za pohranu.

    -   Suradnik račun za pohranu – ih možete upravljati račun za pohranu – mogu čitati grupa resursa u pretplatu i resursa, stvaranje i upravljanje implementacijama grupu resursa za pretplatu. Također možete pristupiti tipki za račun za pohranu što shodno znači mogu pristupiti ravnini podataka.

    -   Administrator programa Access za korisnika – one Upravljanje korisničkim pristupom na račun za pohranu. Na primjer, oni može dati pristup čitač za određenog korisnika.

    -   Suradnik virtualnog računala – ih možete upravljati virtualnih računala, ali ne na račun za pohranu na koji su povezani. Ta uloga može prikazati tipke za račun za pohranu, što znači da korisnik kojemu dodjeljujete ta uloga možete ažurirati ravnini podataka.

        Da biste stvorili virtualnog računala korisnika, imaju će moći stvarati odgovarajuću VHD datoteku na računu za pohranu. Da biste to učinili, moraju moći dohvatiti ključ za račun za pohranu i prosljeđivanje API stvaranje na VM. Zbog toga moraju imati tu dozvolu da bi se može prikazati tipki račun za pohranu.

- Mogućnost da biste odredili prilagođene uloge je značajka koja omogućuje vam da biste sastavili skup akcija s popisa dostupnih akcija koje se mogu izvršiti Azure resursa.

- Korisnik može se postaviti u Azure Active Directory prije nego što možete dodijeliti uloge ih.

- Možete stvoriti izvješće o tko odobren/povučen kakvu programa access u kojemu i na koje opseg pomoću programa PowerShell ili EŽA Azure.

####<a name="resources"></a>Resursi

-   [Kontrola pristupa na temelju uloga Azure Active Directory](../active-directory/role-based-access-control-configure.md)

    U ovom se članku objašnjava kontrolu pristupa utemeljen na Azure Active Directory uloga i kako funkcionira.

-   [RBAC: Ugrađeni uloge](../active-directory/role-based-access-built-in-roles.md)

    U ovom se članku detaljno sve dostupne u RBAC ugrađene uloge.

-   [Razumijevanje implementacije Voditelj resursa i klasični implementacije](../resource-manager-deployment-model.md)

    U ovom se članku objašnjava implementacije Voditelj resursa i uvođenje klasičnog modela i objašnjava prednosti korištenja grupe Voditelj resursa i resursa

-   [Azure računalnim, mreže i davatelje usluga u odjeljku Azure upravitelj resursa za pohranu](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    U ovom se članku objašnjava izračunati Azure, mreže i davatelja usluga za pohranu funkcioniranje u odjeljku modela Voditelj resursa.

-   [Upravljanje pristupom utemeljeno na ulogama kontrole s REST API-JA](../active-directory/role-based-access-control-manage-access-rest.md)

    U ovom se članku objašnjava korištenje REST API-JA za upravljanje RBAC.

-   [Azure prostora za pohranu resursa davatelja REST API Reference](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Ovo je vodič za API-je možete koristiti za programski upravljati računom za pohranu.

-   [Vodič za programere za provjeru autentičnosti s resursima API-jem Azure](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    U ovom se članku objašnjava autentičnost korištenja API-ji Voditelj resursa.

-   [Kontrola pristupa na temelju uloga za Microsoft Azure s Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Ovo je veza s videozapisom na kanal 9 iz konferencije Ignite 2015 MS. U ovoj sesiji oni objasniti pristup upravljanja i mogućnosti izvješćivanja u Azure i istraživanje najbolje prakse oko zaštita pristup Azure pretplata pomoću servisa Azure Active Directory.

###<a name="managing-your-storage-account-keys"></a>Upravljanje računom ključeva za pohranu

Prostor za pohranu računa tipke su 512 bita nizove stvorila Azure koji, uz naziv računa spremišta može se koristiti za pristup podacima objekata pohranjenih na račun za pohranu, primjerice blob-ova, entiteti unutar tablice, reda čekanja poruka i datoteka na zajedničko korištenje programa Azure datoteke. Kontrola pristupa na prostora za pohranu tipke kontrole pristupa računu u ravnini podataka za taj račun za pohranu.

Svaki račun za pohranu ima dvije tipke koje se nazivaju "Ključ 1" i "Tipki 2" [Azure portal](http://portal.azure.com/) i cmdleta ljuske PowerShell. To može biti regenerated ručno na jedan od nekoliko načina, uključujući, ali nije ograničena pomoću [portala za Azure](https://portal.azure.com/), PowerShell, EŽA Azure ili programski pomoću klijentska biblioteka za pohranu .NET ili Azure prostora za pohranu servisa REST API-JA.

Postoje bilo koji broj razloga za Obnovi ključeva za račun za pohranu.

-   Možda ih Obnovi redovito sigurnosnih vam razloga.

-   Račun ključeva za pohranu bi Obnovi ako netko upravlja probiti u aplikaciju i dohvatiti ključ koji je koji nisu ili spremljena u konfiguracijskoj datoteci omogućavanja puni pristup računu za pohranu.

-   Drugi slučaj za ključne regeneration je ako vaš tim koristi aplikacija za pohranu Explorer koja zadržava ključ za račun za pohranu, a jedan članovima tima. Aplikacija će nastaviti raditi, što im daje pristup računu za pohranu kada postanu nema više. Ovo je zapravo primarni razloga su stvorili račun razinom zajednički pristup potpisa – možete koristiti na razini račun SAS umjesto pohrani pristupnih tipki u konfiguracijskoj datoteci.

####<a name="key-regeneration-plan"></a>Planiranje ključa regeneration

Ne želite samo Obnovi tipku koristite bez nekim planiranja. Ako to učinite, za taj račun za pohranu, što može izazvati glavne prekidu nije odrezan pristupa. To je zašto postoje dvije tipke. Trebali biste Obnovi ključa jedan po jedan.

Prije Obnovi ključeva, provjerite imate popis svih aplikacija koje ovise o računu za pohranu, kao i druge servise koji se koriste u Azure. Ako, na primjer, ako koristite servisa Azure Media Services koje ovise o računu za pohranu, morate ponovno sinkronizirate pristupnih tipki sa servisom za medijske sadržaje nakon Obnovi tipku. Ako koristite sve programe kao što je prostor za pohranu explorer, morat ćete unijeti nove ključeve u tim aplikacijama. Imajte na umu da imate VMs čije VHD datoteke spremaju se u račun za pohranu, oni će neće utjecati na ponovno stvaranje računa tipki za pohranu.

Možete Obnovi ključeva na portalu za Azure. Kada su tipke regenerated može potrajati do 10 minuta za sinkronizaciju preko servise za pohranu.

Kada budete spremni, Evo postupka Općenito dovršenog kako biste trebali mijenjati ključa. U ovom slučaju, pretpostavlja se da trenutno koristite tipke 1 i prilikom prelaska da biste promijenili sve umjesto korištenja ključa 2.

1.  Obnovi 2 ključ da biste bili sigurni da je sigurna. To možete učiniti na portalu za Azure.

2.  U svim aplikacijama u kojem je pohranjena ključa za pohranu Promjena ključa za pohranu za korištenje ključ 2 novu vrijednost. Testiranje i objavljivanje aplikacija.

3.  Nakon svih aplikacija i servisa sustava su prema gore i uspješno pokrenuti, Obnovi ključ 1. Time se osigurava da bilo koja osoba kojima ste nije izričito dali novi ključ više neće imati pristup računu za pohranu.

Ako trenutno koristite ključ 2, možete koristiti isti postupak, ali obrnuti nazive ključa.

Možete migrirati preko nekoliko dana, promijeniti svaku aplikaciju koju želite koristiti novi ključ, a zatim ga objavite. Nakon što ih sve gotovi, trebali biste vratite i Obnovi stari ključ tako da više ne funkcionira.

Neku drugu mogućnost je vratite ključ za račun za pohranu na [Azure ključ sigurnog](https://azure.microsoft.com/services/key-vault/) kao na tajna i imaju aplikacija dohvatiti ključ iz nje. Zatim kada Obnovi tipku i ažurirati ključ sigurnog Azure, aplikacije ne morat ćete se ponovno implementirati jer oni će obraditi novi ključ iz zbirke ključeva ključ za Azure automatski. Imajte na umu da imate aplikaciju čitati tipku svaki put kada vam je potrebna ili možete predmemorije u memoriji i ako ga ne uspijeva kada ga koristiti dohvatiti tipku ponovno iz zbirke ključeva za Azure ključ.

Pomoću sigurnog Azure ključ dodaje sljedeću razinu sigurnosti za ključeva za pohranu. Ako koristite taj način, nikad imat ćete koji nisu ključa za pohranu u konfiguracijskoj datoteci, čime se uklanja tu avenue od nečijeg početak pristup ključeva bez određene dozvole.

Druga prednost korištenja Azure ključ sigurnog je možete upravljati pristupom za ključeve pomoću servisa Azure Active Directory. To znači da možete dopustiti pristup handful aplikacije koje je potrebno dohvatiti tipki iz zbirke ključeva ključ Azure i znate da druge aplikacije neće moći pristupiti tipke bez dodjelu ih dozvole posebno.

Napomena: preporučuje se da biste koristili samo jednu od tipki u svim aplikacija u isto vrijeme. Ako koristite tipke 1 na nekim mjestima i ključ 2 u drugim korisnicima, nećete moći da biste zakrenuli ključeva bez nekim aplikacije gubitak pristupa.

####<a name="resources"></a>Resursi

-   [O računima Azure prostora za pohranu](storage-create-storage-account.md#regenerate-storage-access-keys)

    U ovom se članku daje pregled računa za pohranu te iznose prikaza, kopiranje i ponovno stvaranje spremišta pristupnih tipki.

-   [Azure prostora za pohranu resursa davatelja REST API Reference](https://msdn.microsoft.com/library/mt163683.aspx)

    Ovaj članak sadrži veze na članke o dohvaćanje tipki račun za pohranu i ponovno stvaranje računa tipki za pohranu za račun Azure pomoću REST API-JA. Napomena: To je za račune Voditelj resursa za pohranu.

-   [Operacije na računima za pohranu](https://msdn.microsoft.com/library/ee460790.aspx)

    U ovom se članku Upravitelj servisa za pohranu REST API Reference sadrži veze na članke na dohvat i ponovno stvaranje tipki račun za pohranu pomoću REST API-JA. Napomena: To je za klasični račune za pohranu.

-   [Nerazumljivim na tipku upravljanje – Upravljanje pristupom Azure prostora za pohranu podataka pomoću Azure AD](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    U ovom se članku objašnjava servisa Active Directory za kontrolu pristupa koristite da biste ključeva Azure prostora za pohranu u sigurnog ključ Azure. Također se prikazuje kako pomoću programa automatizacije Azure posao Obnovi tipke na svaki sat temelj.

##<a name="data-plane-security"></a>Sigurnost ravnini podataka

Sigurnost podataka ravnini odnosi se na metode radi zaštite podataka objekata pohranjenih na Azure prostora za pohranu – blob-ova, redovi, tablice i datoteke. Smo vidjeli načina za šifriranje podataka i sigurnosti tijekom prijenosa podataka, ali kako vam se o dopuštanja pristupa objekti?

Zapravo su dvije metode pristupom sami objekti podataka. Prvi je ograničite pristup računu ključeva za pohranu i druga koristi zajednički pristup potpisa dopustiti pristup određenih podataka objekata određenog vremenskog razdoblja.

Izuzetaka napomenuti da je da možete omogućite javni pristup vašem blob-ova postavljanjem razinu pristupa za spremnik koja sadrži s blob-ova u skladu s tim. Ako ste postavili pristup za kontejner Blob ili spremnik, će omogućiti javno pristup za čitanje za blob-ova u njih. To znači da svi korisnici s URL-a koja pokazuje na blob u njih možete je otvoriti u pregledniku ne s potpisom za zajedničko korištenje programa Access i ako nemate račun tipki za pohranu.

###<a name="storage-account-keys"></a>Tipke za pohranu računa

Prostor za pohranu računa tipke su nizovi 512-bitni stvorio Azure koji, uz naziv računa spremišta možete koristiti za pristup podacima objekata pohranjenih na račun za pohranu.

Na primjer, pročitajte blob-ova, pisanje redovi, stvorite tablice i izmjena datoteka. Mnoge od tih akcija mogu izvršiti putem portala za Azure ili koristite jednu od mnogo prostora za pohranu Explorer aplikacija. Možete upisati i kod tako da koristi REST API-JA ili jedna biblioteka za pohranu klijenta za izvođenje te operacije.

Kako je opisano u odjeljku [Upravljanje ravnini sigurnost](#management-plane-security)pristup ključeva za pohranu za račun za pohranu klasični se mogu dodijeliti dodjeljivanjem puni pristup Azure pretplatu. Pristup ključeva za pohranu za pohranu račun pomoću upravitelja resursa Azure modela moguće je prilagoditi kroz na temelju uloga kontrole pristupa (RBAC).

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Delegiranje pristupa na objekte s računom pomoću zajednički pristup potpisa i spremljene pravilnike za pristup

Zajednički pristup potpis je niz koji sadrži sigurnosnog tokena koje možete priložiti URI koja omogućuje delegiranje pristupa za pohranu objekata i navedite ograničenja kao što su dozvole i raspon datuma/vremena programa access.

Možete dati pristup blob-ova, spremnika, red čekanja poruke, datoteke i tablice. S tablicama, zapravo možete dodijeliti dozvolu za pristup rasponu entiteti u tablici navođenjem particija i redak ključa raspone na koji želite da korisnik ima pristup. Na primjer, ako imate podatke spremljene s ključem particija zemljopisnim stanja, koje nije moguće nekome dati pristup samo podaci za Kaliforniji.

U drugi primjer može dati web-aplikacije SAS token koji omogućuje da bi pisati stavke u redu čekanja i dati u ulogu aplikacije SAS token se poruke iz reda čekanja i obraditi ih. Ili može dati jednog klijenta SAS token ih možete koristiti da slike prenosi spremniku u spremište blobova platforme i dati dozvolu za aplikaciju web da biste pročitali te slike. U oba slučaja, postoji odvojenosti od opasnosti – svaka aplikacija može biti zadano access koje trebaju njihove zadatak. Ovo je moguće pomoću zajednički pristup potpisa.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Zašto na koji želite koristiti zajednički pristup potpisi

Zašto biste koji želite koristiti za SAS umjesto samo dajete ključ za račun za pohranu koji je mnogo lakše? Dajete ključ za račun za pohranu je kao što je zajedničko korištenje ključeva vaše Britanija prostora za pohranu. Dodjeljuje potpuni pristup. Netko nije moguće koristiti ključeva i svoje biblioteke cijelu glazbenu prenijeti na račun servisa za pohranu. Ih nije moguće i datotekama zamijenite virus špijunskim verzije ili ukrasti vaše podatke. Održavanje Odsutan Neograničeni pristup računu za pohranu je nešto što treba ne mogu prenijeti u svijetlim.

Uz zajednički pristup potpisa možete dati klijent dozvole potrebne za ograničenog vremenskog razdoblja. Na primjer, ako netko se prenosi blob vaš račun, možete im dodijelite pristup za pisanje za samo dovoljno vremena za prijenos blob (ovisno o veličini blob-om, naravno). A ako se predomislite, možete opozvati da access.

Osim toga, možete odrediti jesu li zahtjeva putem SAS ograničeni na IP adresa ili rasponu IP adresa izvan Azure. Možete zatražiti da je zahtjeve vrše pomoću određenih protokola (HTTPS ili HTTP/HTTPS). To znači da ako želite samo da dopušta promet HTTPS, možete postaviti potrebne protokol HTTPS samo i HTTP promet bit će blokirane.

####<a name="definition-of-a-shared-access-signature"></a>Definicija zajednički pristup potpisa

Pristup potpis zajednički je skup parametara upita dodan URL pokažete na resurs

koji daje informacije o pristup dopuštene i razdoblje za koje je dopušteno programa access. Slijedi primjer; u ovom URI omogućuje pristup čitanju blob pet minuta. Napomena SAS upit je parametara mora biti kodirati URL-a, kao što je % 3A dvotočku (:) ili 20% za prostor.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Kako pristup potpis zajedničko korištenje provjere autentičnosti putem servisa za pohranu Azure

Kada servis za pohranu primi zahtjev, vodi parametre unos upita i stvara potpisa na isti način kao program za upućivanje poziva. Zatim se uspoređuje dva potpisa. Ako se slažete, servis za pohranu možete provjeriti verziju servisa za pohranu da biste bili sigurni da je valjan, provjerite jesu li trenutni datum i vrijeme unutar prozora navedeni, provjerite je li pristup zatražili odgovara zahtjev, itd.

Ako, na primjer, s URL iznad, ako je URL pokazuje na datoteku, a ne blob, zahtjev zadovoljavaju jer navodi zajednički pristup je za blob. Ako je naredba OSTALE koja se poziva da biste ažurirali blob, bi uspjeti jer Access potpis zajednički određuje je li dopušten pristup samo čitanju.

####<a name="types-of-shared-access-signatures"></a>Vrste zajednički pristup potpisa

-   SAS razini usluge može se koristiti za pristup određene resursa u račun za pohranu. Primjeri to su Dohvaćanje popisa blob-ova u spremniku preuzimanje blob, ažuriranje entitet u tablici, dodavanje poruke u redu čekanja ili prijenos datoteka za zajedničko korištenje datoteka.

-   Na razini račun SAS se može koristiti za sve što razini usluge SAS moguće je koristiti za pristup. Uz to, omogućit mogućnosti resursima koji nisu dopušteni SAS razini usluge, kao što su omogućuje stvaranje spremnika, tablica, redovima i zajedničke datoteke. Pristup većem broju servisa možete odrediti i odjednom. Na primjer, netko može dati pristup blob-ova i datoteke u račun za pohranu.

####<a name="creating-an-sas-uri"></a>Stvaranje programa SAS URI-JA

1.  Ad-hoc URI možete stvoriti na zahtjev, sve parametre upita koji definira svaki put.

    Ovo je zaista fleksibilne, ali ako imate logičke skupa parametara koje su slične svaki put, korištenje pravilnik pristupa pohranjena je bolji uvid.

2.  Možete stvoriti pohranjene pravilnik pristupa za cijelu spremnik, zajedničko korištenje datoteka, tablicu ili red. Zatim možete koristiti taj kao temelj za ji SAS stvarate. Dozvole na temelju pohranjene pravilnike za pristup možete jednostavno povučen. Možete imati do 5 pravila koje je definirao na svaku kontejneru, red, tablicu ili zajedničko korištenje datoteka.

    Na primjer, ako su namjeravate sudjeluje više osoba čitati blob-ova u spremniku za određene, nije moguće stvoriti pravila pristupa pohranjene koja vas obavještava da "dati pristup čitanju" i druge postavke koje će biti ista svaki put. Zatim možete stvoriti URI SAS pomoću postavki u pravilnik o pristupu pohranjene i navođenjem isteka datuma/vremena. Prednost to je ne morate navesti sve parametre upita svaki put.

####<a name="revocation"></a>Opoziv

Pretpostavimo da vaše SAS ugrožena ili ako želite promijeniti zbog sigurnosti za tvrtku ili zahtjeve za usklađenosti s propisima. Kako opozvati pristup resursu pomoću tog SAS Ovisi o načinu stvaranja SAS URI.

Ako koristite ad-hoc URI su vam tri mogućnosti. Možete izdati SAS tokeni s pravilima isteka kratki i jednostavno Pričekajte SAS isteći. Možete preimenovati ili izbrisati resursa (Ako je token ograničena na jedan objekt). Možete promijeniti račun tipki za pohranu. Ta mogućnost posljednje možete imati veliki utjecaj, ovisno o tome koliko services koriste taj račun za pohranu i vjerojatno ne nešto što želite učiniti bez nekim planiranja.

Ako koristite SAS izvedene iz pravilnik pristupa pohranjeni, pristup možete ukloniti tako da opoziv pravilnik pristupa pohranjene – samo ga možete promijeniti tako da ga već istekao ili potpuno ukloniti. Stupa na snagu odmah i poništava svaki SAS stvoren pomoću tog pravilnik o pristupu pohranjena. Ažuriranje ili uklanjanje pravilnik pristupa pohranjene možda utjecaj osobe pristupanju određene njih, zajedničko korištenje datoteka, tablice ili red putem SAS, ali ako klijente zapisuju kako zatražiti novu SAS kad postane staru koji nisu valjani, to će funkcionirati precizno.

Korištenje SAS izvedene iz pravilnik pristupa pohranjene vam omogućuje da biste opozvali te SAS odmah, pa je preporučena preporučeni je način da uvijek koristi pravila pristupa pohranjene kada je to moguće.

####<a name="resources"></a>Resursi

Detaljnije informacije o korištenju zajednički pristup potpisi i spremljene pristup pravila, zajedno s primjerima potražite u sljedećim člancima:

-   To su u članku referenca.

    -   [Servis SAS](https://msdn.microsoft.com/library/dn140256.aspx)

        U ovom se članku nalaze Primjeri razini usluge SAS pomoću blob-ova, reda čekanja poruke, rasponi tablice i datoteke.

    -   [Izgradnje servisa SAS](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Izgradnje račun SAS](https://msdn.microsoft.com/library/mt584140.aspx)

-   Slijede upute za korištenje klijentska biblioteka .NET da biste stvorili zajednički pristup potpisa i spremljene pravilnike za pristup.

    -   [Korištenje potpisa zajednički pristup (SAS)](storage-dotnet-shared-access-signature-part-1.md)

    -   [Zajedničko korištenje programa Access potpise, 2.dio: Stvaranje i korištenje SAS sa servisom blobova platforme](storage-dotnet-shared-access-signature-part-2.md)

        Ovaj članak sadrži objašnjenja SAS modela Primjeri zajednički pristup potpisa i preporuke za preporučeni je način korištenja SAS. Također spominju je opoziva pravo.

-   Ograničavanje pristupa IP adrese (IP ACL)

    -   [Što je krajnje popis za kontrolu pristupa (ACL)?](../virtual-network/virtual-networks-acl.md)

    -   [Izgradnje servisa SAS](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Ovaj je članak referenca na razini usluge SAS; uključuje primjer IP ACLing.

    -   [Izgradnje račun SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Ovo je referentni članak za račun razinom SAS; uključuje primjer IP ACLing.

-   Provjera autentičnosti

    -    [Provjera autentičnosti za servise za pohranu za Azure](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Zajedničko korištenje potpisa pristup Uvod vodiča

    -   [SAS Uvod vodiča](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Šifriranje na putu

###<a name="transport-level-encryption--using-https"></a>Šifriranje prijenosa razini – pomoću HTTPS

Drugi korak koji morate poduzeti da biste bili sigurni sigurnost podataka za pohranu Azure je za šifriranje podataka između klijenta i Azure prostora za pohranu. Preporuke za prvi je uvijek koristi [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protokola koja omogućuje sigurnu komunikaciju putem Interneta javno.

Koristite uvijek HTTPS prilikom pozivanja REST API-ji ili pristupa objektima u prostor za pohranu. **Zajednički pristup potpisa**, koji se može koristiti delegatu pristup objektima Azure prostora za pohranu, obuhvaćaju i mogućnost da biste odredili da se samo HTTPS protokola možete koristiti kada se koristi zajednički pristup potpise, osiguravanje nikome slanja veze s SAS tokeni će koristiti protokol proper.

####<a name="resources"></a>Resursi

-   [Omogućivanje HTTPS za aplikaciju u aplikacije servisa za Azure](../app-service-web/web-sites-configure-ssl-certificate.md)

    U ovom se članku objašnjava da biste omogućili HTTPS za web-aplikaciju programa Azure.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Šifriranjem tijekom prijenosa sa zajedničkim datotekama Azure

Azure pohrani podržava HTTPS prilikom korištenja REST API-JA, ali više obično se koristi kao zajedničko korištenje datoteka programa SMB priključen na VM. SMB 2.1 ne podržava šifriranje, tako da se veza samo dopuštene unutar iste regija u Azure. Međutim, SMB 3.0 podržava šifriranje i može se koristiti sa sustavom Windows Server 2012 R2, Windows 8, Windows 8.1 i Windows 10, što omogućuje regije-pristup i čak i pristup na radnoj površini.

Imajte na umu da tijekom zajedničke datoteke Azure može se koristiti s Unix, Linux SMB klijent još podržava šifriranje, tako da pristupa samo dopuštene unutar Azure područja. Podrška za šifriranje za Linux nalazi se na vodič razvojnim inženjerima Linux odgovorni za funkciju SMB. Prilikom dodavanja šifriranje, imat ćete istu mogućnost za pristup programa Azure zajedničko korištenje datoteka na Linux kao i za Windows.

####<a name="resources"></a>Resursi

-   [Kako koristiti za pohranu datoteka Azure s Linux](storage-how-to-use-files-linux.md)

    U ovom se članku objašnjava postavljanje programa Azure resursu za zajedničko korištenje u sustavu Linux i prijenos i preuzimanje datoteka.

-   [Početak rada s Azure pohrani u sustavu Windows](storage-dotnet-how-to-use-files.md)

    U ovom se članku daje pregled zajedničke datoteke Azure i upute za postavljanje i korištenje ih pomoću komponente PowerShell i .NET.

-   [Unutrašnji Azure pohrani](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    U ovom se članku najavljuje Općenito dostupan Azure prostora za pohranu datoteka i njihovi tehničke pojedinosti o razlozima za odabir 3.0 šifriranje.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Šifriranjem klijentsko radi zaštite podataka koje šaljete prostora za pohranu

Druga mogućnost koji olakšava provjerite je li vaši podaci sigurne dok se prenose između klijentska aplikacija i pohranu je klijentsko šifriranje. Prije nego što se prenose u prostor za pohranu Azure je šifriran podatke. Pri dohvaćanju podataka iz spremišta Azure, podaci dešifrira će se nakon što ste je primili na klijentskoj strani. Čak i ako se podaci šifriran prelaska preko na žičani, preporučujemo da ih koristiti HTTPS, kao što je podataka integritet provjere ugrađena pomoći prevladavanje mrežom pogreške utjecaja integritet podataka.

Šifriranje klijentsko je način za šifriranje podataka na ostale, kao što su podaci spremljeni u obliku šifrirane. Ćemo objasniti što to detaljno u sekciji šifriranje [na ostale](#encryption-at-rest).

##<a name="encryption-at-rest"></a>Šifriranje na ostale

Postoje tri Azure značajke koje omogućuju šifriranje na ostale. Azure šifriranje koristi se za šifriranje diskova OS i podataka u IaaS virtualnih računala. Ostale dvaju – klijentsko šifriranja i SSE – su oba šifrirali podataka u Azure prostora za pohranu. Pogledajmo svaku od njih, pogledajte pa učinite usporedbu i potražite u članku kada svaki od njih mogu koristiti.

Šifriranje klijentsko možete koristiti za šifriranje podataka na putu (koji je i pohranjen u šifrirane obrasca u spremište), bilo bi dobro da jednostavno koriste HTTPS tijekom prijenosa i imaju mobitel podataka automatski šifriranje kada je pohranjena. Dva su načina za to – šifriranje Azure i SSE. Se koristi za izravno šifriranje podataka na OS i podataka diskova koristi VMs, a drugi koristi se za šifriranje podataka zapisivanje spremište blobova platforme Azure.

###<a name="storage-service-encryption-sse"></a>Šifriranje servis za pohranu (SSE)

SSE omogućuje vam da biste zatražili da servis za pohranu automatski šifriranje podataka prilikom pisanja Azure za pohranu. Prilikom čitanja podataka iz spremišta Azure, ona će se dešifrirati servis za pohranu prije nego što se vraćaju. Omogućuje zaštitu podataka bez potrebe za izmjenu kod ili dodavanje koda za sve programe.

Ovo je postavka koja se odnosi na račun za cijelu pohranu. Možete omogućiti i onemogućiti tu značajku promjenom vrijednosti postavke. Da biste to učinili, možete koristiti portal za Azure, PowerShell, EŽA Azure, za pohranu resursa davatelja REST API-JA ili biblioteku .NET prostora za pohranu klijenta. Prema zadanim postavkama SSE je isključen.

Trenutačno ključevi koji se koriste za šifriranje upravlja Microsoft. Ne možemo izvorno generiranje tipki te upravljanje sigurna pohrana tipki, kao i obične zakretanje kako je definirano Interna pravila Microsoft. U budućnosti će se mogućnost upravljanje vlastitim ključeva za šifriranje i navedite put migracije iz Microsoft upravlja tipke kupca upravlja tipke.

Ta je značajka dostupna za račune standardnu i pohranu Premium stvoren pomoću model implementacije Voditelj resursa. SSE odnosi samo na blokirati blob-ova, stranice blob-ova, a zatim Dodaj blob-ova. Druge vrste podataka, uključujući tablice, redovima i datoteke, neće biti šifriran.

Podaci šifriran samo kada je omogućen SSE i podatke napisan sa spremištem blobova. Omogućavanje ili onemogućavanje SSE ne utječu na postojeće podatke. Drugim riječima, kada omogućite šifriranje, bit će ne vratite i šifriranje podataka koji već postoji; niti će dešifrirati podatke koji već postoji kada onemogućite SSE.

Ako želite koristiti tu značajku s računom za pohranu klasični možete stvoriti novi račun Voditelj resursa za pohranu i koristiti AzCopy da biste kopirali podatke na novi račun. 

###<a name="client-side-encryption"></a>Klijentsko šifriranje

Ne možemo navedenim klijentsko šifriranje kada razgovarate o šifriranje podataka na putu. Ova značajka omogućuje programski šifriranje podataka za klijentske aplikacije prije slanja preko na žičani pisana Azure prostora za pohranu i programski dešifrirati podataka nakon preuzimanja s Azure prostora za pohranu.

To nude šifriranje na putu, ali pruža značajka šifriranja na ostale. Imajte na umu da Iako podatke šifriran na putu, i dalje preporučujemo korištenje HTTPS da iskoristite prednost provjere integriteta ugrađene podatke kojima prevladavanje mrežom pogreške utjecaja integritet podataka.

Primjer gdje možete koristiti to je ako imate web-aplikacije koje se sprema blob-ova i dohvaća blob-ova i želite da se aplikacija i podaci se kao sigurne. U tom slučaju služite klijentsko šifriranje. Promet između klijenta i servis blobova platforme Azure sadrži šifrirane resursa, a nitko protumačiti podatke na putu i reconstitute u privatnu blob-ova.

Šifriranje klijentsko ugrađen u na Java i pohranu .NET klijenta biblioteke kojeg shodno Azure ključ sigurnog API-ji, jednostavno ga vrlo za implementaciju. Postupak šifriranje i dešifriranje podataka koristi tehniku omotnice i sprema metapodataka koja koristi značajka šifriranja u svaki objekt prostora za pohranu. Ako, na primjer, za blob-ova, sprema se u metapodacima blobova platforme za redove, ga dodaje u svaku poruku red.

Za šifriranje sam možete generirati i upravljati vlastitim ključeva za šifriranje. Možete koristiti tipke generira klijentska biblioteka za pohranu Azure ili imate sigurnog ključ Azure generiranje tipki. Ključeva za šifriranje možete spremiti u ključa prostora za pohranu na lokalni ili ih možete spremiti u programa sigurnog ključ Azure. Azure ključ sigurnog omogućuje vam da biste dodijelili pristup tajne u sigurnog Azure ključ za određene korisnike pomoću servisa Azure Active Directory. To znači da ne samo nikome mogu čitati ključ sigurnog Azure i dohvaćanje ključeva za šifriranje klijentsko koristite.

####<a name="resources"></a>Resursi

-   [Šifriranje i dešifriranje blob-ova u Azure spremište na platformi Microsoft Azure ključ sigurnog korištenja](storage-encrypt-decrypt-blobs-key-vault.md)

    U ovom se članku objašnjava korištenje klijentsko šifriranja s sigurnog ključ Azure, uključujući kako stvoriti na KEK i spremite ga na sigurnog pomoću komponente PowerShell.

-   [Šifriranje klijentsko i Azure ključa zbirke ključeva za pohranu Microsoft Azure](storage-client-side-encryption.md)

    U ovom se članku daje objašnjenje klijentsko šifriranja i navode Primjeri korištenja klijentska biblioteka za pohranu za šifriranje i dešifriranje resursi s četiri servise za pohranu. Također se govori o sigurnog ključ Azure.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Šifriranjem Azure na disku za šifriranje diskova koriste virtualnim strojevima

Azure šifriranje Disk je nova značajka koja je trenutno u pretpregledu. Ova značajka omogućuje šifriranje OS diskova i diskova podatke koristiti tako da se IaaS virtualnog računala. Za Windows, pogone šifrirane pomoću tehnologije za šifriranje standardni BitLocker. Za Linux, na diskova šifrirane pomoću tehnologije za DM Crypt. To je integriran s Azure sigurnog ključ za nadzor i upravljanje ključeva za šifriranje diska.

Rješenja Azure šifriranje podržava sljedeće scenarije šifriranje tri kupca:

-   Omogućivanje šifriranja na novu IaaS VMs stvorene iz klijenta šifriranih datoteka VHD i ključeva za šifriranje navedeni za klijenta, koje se spremaju u sigurnog ključ Azure.

-   Omogućivanje šifriranja na novu IaaS VMs stvorene iz trgovine Windows Azure.

-   Omogućivanje šifriranja na postojeće IaaS VMs već izvodi u Azure.

>[AZURE.NOTE] Za VMs Linux izvodi Azure ili novi Linux VMs stvorene iz slike u trgovine Windows Azure, šifriranje diska OS trenutno nisu podržani. Šifriranje glasnoću OS Linux VMs je podržano samo za VMs koje su šifrirane lokalnog i prenijeli Azure. Ovo ograničenje primjenjuje se samo na disku OS; šifriranje količine podataka za Linux VM podržana.

Rješenje podržava sljedeće za IaaS VMs za javne pretpregled izdanja kada je omogućen u Microsoft Azure:

-   Integracija s Azure sigurnog ključa

-   Standardni [odgovora "," D "i" G niz IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   Omogućivanje šifriranja IaaS VMs stvoren pomoću [Upravitelja resursa Azure](../azure-resource-manager/resource-group-overview.md) modela

-   Sve Azure javne [regije](https://azure.microsoft.com/regions/)

Ova značajka omogućuje čitanje na ostale u spremište Azure šifriran sve podatke na disk virtualnog računala.

####<a name="resources"></a>Resursi

-   [Azure šifriranje za Windows i Linux IaaS virtualnim strojevima](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    U ovom se članku govori o izdanju pretpregled za šifriranje Azure i njihovi vezu za preuzimanje studiju.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Usporedba Azure šifriranje, SSE i klijentsko šifriranje

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs i njihove VHD datoteke

Za diskova koji se koriste IaaS VMs, preporučujemo korištenje Azure šifriranje. Možete uključiti SSE šifriranje VHD datoteke koje se koriste u pozadinu te diskova Azure prostora za pohranu, ali samo šifrira upravo pisane podataka. To znači da ako stvorite na VM, a zatim omogućiti SSE na računu za pohranu koja sadrži datoteku VHD, samo promjene bit će šifrirana, ne izvornu datoteku VHD.

Ako stvorite na VM korištenja slike iz trgovine Azure Azure izvodi [shallow Kopiraj](https://en.wikipedia.org/wiki/Object_copying) sliku na račun servisa za pohranu u spremište Azure pa je šifriran čak i ako imate SSE omogućena. Kada se stvara u VM i pokreće ažuriranje sliku, počet će SSE podatke. Zbog toga je najbolje koristiti šifriranje Azure na VMs stvorene iz slike u trgovine Windows Azure ako želite potpuno šifrirane.

Ako iz lokalnog unaprijed šifrirane VM unijeti u Azure, moći da biste prenijeli ključeva za šifriranje za Azure ključ sigurnog i nastaviti koristiti šifriranje za tu VM koristili lokalnog. Azure šifriranje je omogućena za rukovanje scenarij.

Ako imate koje nisu šifrirane VHD lokalnim, možete prenijeti u galeriji kao prilagođenu sliku i dodjela VM iz nje. Ako to učinite pomoću predložaka Voditelj resursa, možete zatražiti da biste uključili šifriranje diska Azure kada se pokrene prema gore na VM.

Kada dodate na disku podataka i postavljanje na na VM, možete uključiti šifriranje Azure na tom disku podataka. Najprije ga će šifriranje te podatke na disku lokalno, a zatim upravljanje slojevima servisa će učinite drži pisanje protiv prostora za pohranu tako da je šifriran prostora za pohranu sadržaja.

####<a name="client-side-encryption"></a>Klijentsko šifriranje####

Šifriranje klijentsko je najsigurnija način šifriranja vaše podatke jer je šifrira prije prijenosa te šifrira podatke na ostale. Međutim, je potreban je dodavanje koda za aplikacije pomoću prostor za pohranu možda ne želite učiniti. U tom slučaju možete koristiti HTTPs za podatke u prijenosa i SSE za šifriranje podataka na ostale.

Klijentsko šifriranjem Šifrirajte entiteti tablice, reda čekanja poruke i blob-ova. S SSE, možete šifrirati blob-ova. Ako vam potrebni podaci tablice i reda čekanja šifriranje, trebali biste koristiti klijentsko šifriranje.

Šifriranje klijentsko potpuno upravljaju aplikacije. To je najsigurnija pristup, ali potrebno programski promjene u aplikaciji i postavili procese upravljanja ključem na mjestu. Koristit ćete to kada dodatnu sigurnost tijekom prijenosa, a želite podatke pohranjene šifriranje.

Šifriranje klijentsko je više opterećenja na klijentskom računalu, a imate račun za to u skalabilnost planove, osobito ako su šifriranje i prijenos velike količine podataka.

####<a name="storage-service-encryption-sse"></a>Šifriranje servis za pohranu (SSE)

SSE upravlja Azure prostora za pohranu. Korištenje SSE ne nudi za sigurnost podataka na putu, ali šifriranje podataka kao što je napisan Azure prostora za pohranu. Postoji bez utjecaja na performanse prilikom korištenja ovu značajku.

Možete samo šifriranje bloka blob-ova, dodavanje blob-ova i stranica pomoću SSE blob-ova. Ako vam je potrebna šifriranje tablice ili red podataka, razmislite o šifriranjem klijentsko.

Ako imate arhive ili biblioteci VHD datoteka koju koristite kao osnovu za stvaranje novog virtualnih računala, možete stvoriti novi račun za pohranu, omogućivanje SSE i prijenos datoteka VHD taj račun. Te datoteke VHD bit će šifrirana po Azure prostora za pohranu.

Ako imate Azure šifriranje omogućen za diskova VM i SSE omogućena na račun za pohranu VHD datoteke, funkcionirat će precizno; To će rezultirati dvaput šifrirane podatke upravo napisali.

##<a name="storage-analytics"></a>Prostor za pohranu Analytics

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Korištenje Analytics za pohranu praćenje autorizacije vrsta

Za svaki račun za pohranu, možete omogućiti Azure Analytics za pohranu za izvođenje zapisivanje i pohranu metrika podataka. To je odličan alat za koristite kada želite provjeriti metriku performanse računa za pohranu ili otklanjanje poteškoća s računom za pohranu jer imate poteškoća s performansama.

Drugi dio podataka možete vidjeti u zapisnicima analytics za pohranu je način provjere autentičnosti koristi netko prilikom pristupa za pohranu. Na primjer, s spremište blobova platforme vidjet ćete ako koriste Access potpis zajedničko korištenje ili više njih račun za pohranu ili ako je blob pristupiti javnom.

To može biti zaista korisno ako su čvrsto guarding pristup prostora za pohranu. Na primjer, u spremište blobova platforme možete postavite sve spremnike privatne i implementirati pomoću servisa za SAS cijeloj aplikacija. Zatim možete provjeriti zapisnike redovito da biste vidjeli ako vaš blob-Ova je moguće pristupiti pomoću tipke račun za pohranu koji mogu upućivati nepoštivanja sigurnost, ili ako su s blob-ova javno, ali bi trebalo biti.

####<a name="what-do-the-logs-look-like"></a>Kako zapisnike izgledaju?

Kada omogućite račun metrike pohrane i Prijava putem portala za Azure analize podataka započet će skupiti brzo. Zapisivanje i metrike za svaki servis razlikuje se; Zapisivanje zapisati samo kada je aktivnosti u taj račun za pohranu dok metriku zapisat će se svake minute, svaki sat ili svaki dan, ovisno o tome kako konfigurirati.

U zapisnicima spremaju se u blok blob-ova u kontejner $logs na računu za pohranu. Ovaj spremnik automatski stvara kada je omogućen za pohranu analize. Nakon stvaranja ovaj spremnik nije moguće izbrisati, iako možete izbrisati njegov sadržaj.

U kontejneru $logs postoji mapa za svaki servis, a zatim postoje podmape godina/mjesec/dan/h. U odjeljku sat, zapisnike jednostavno numeriranja. To je strukturu direktorija izgleda:

![Prikaz datoteka zapisnika](./media/storage-security-guide/image1.png)

Prijavljen je svaki zahtjev za Azure prostora za pohranu. Evo snimke datoteku zapisnika, prikazuje prvi nekoliko polja.

![Snimka datoteke zapisnika](./media/storage-security-guide/image2.png)

Vidjet ćete da zapisnike možete koristiti za praćenje bilo kakvu vrstu poziva s računom za pohranu.

####<a name="what-are-all-of-those-fields-for"></a>Što su sva ta polja za?

Postoji članak resursi niže na popisu daje popis polja mnogo u zapisnicima i koje se koriste za. Evo popisa polja u redoslijedu:

![Snimka polja u datoteci zapisnika](./media/storage-security-guide/image3.png)

Ne možemo zanima stavki za GetBlob i kako se provjerava, pa ćemo morate tražiti stavke s vrstom operacija "Get-Blob", a zatim potvrdite zahtjev – statusa (4<sup>tom</sup> stupcu) i autorizaciju-vrstu (8<sup>tom</sup> stupcu).

Ako, na primjer, u prvih nekoliko redaka na popisu iznad status zahtjev je "poruka uspjeh" i vrstu autorizacije "provjere autentičnosti". To znači da zahtjev je provjeriti pomoću ključa za pohranu računa.

####<a name="how-are-my-blobs-being-authenticated"></a>Kako se moj blob-ova koji se provjeriti autentičnost?

Imamo još tri slučajeve koje ćemo vas zanimaju.

1.  Blob-om javnog, a je pristupiti pomoću URL-a bez pristupa potpisa zajedničko korištenje. U ovom slučaju status zahtjev je "AnonymousSuccess" i vrstu autorizacije je "anonimnim".

    1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**200; 124; 37; **anonimni**; mystorage...

2.  Blob-om je privatna i koristila potpisom zajedničko korištenje programa Access. U ovom slučaju status zahtjev je "SASSuccess" i vrstu autorizaciju je "sas".

    1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**200; 416; 64; **sas**; mystorage...

3.  Blob-om je privatna i ključa za pohranu korišten za pristup. U ovom slučaju status zahtjev je "**uspjeh**" i vrstu autorizacije je "**autentičnost**".

    1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Uspjeh**206; 59; 22; **autentičnost**; mystorage...

Microsoftov analizator poruka možete koristiti radi prikaza i analize te zapisnika. Uključuje mogućnosti pretraživanja i filtriranja. Na primjer, možda želite tražiti pojavljivanja GetBlob je li korištenje što ste očekivali, odnosno da biste provjerili nije neovlašteno pristupa računu za pohranu.

####<a name="resources"></a>Resursi

-   [Prostor za pohranu Analytics](storage-analytics.md)

    Ovaj je članak pregled analytics za pohranu i kako ih omogućiti.

-   [Oblik zapisnika Analytics](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    U ovom se članku prikazuje oblik zapisnika analize i detalje o polja dostupna therein, uključujući vrsta provjere autentičnosti-, koji označava vrstu provjere autentičnosti za zahtjev.

-   [Praćenje računa za pohranu na portalu za Azure](storage-monitor-storage-account.md)

    U ovom se članku objašnjava konfiguriranje nadzora od metriku i bilježenja za račun za pohranu.

-   [Otklanjanje poteškoća završetka do kraja pomoću Azure metrike pohrane i zapisivanje, AzCopy i alata za analizu poruke](storage-e2e-troubleshooting.md)

    U ovom se članku govori o otklanjanju poteškoća s korištenjem analitičkih podataka za pohranu i saznati kako koristiti Microsoftov analizator poruke.

-   [Vodič za operacijski Microsoft poruka alata za analizu](https://technet.microsoft.com/library/jj649776.aspx)

    U ovom se članku je vodič za Microsoftov analizator poruke i sadrži veze na Praktični vodič, brzi početak rada i značajka sažetka.

##<a name="cross-origin-resource-sharing-cors"></a>Unakrsno polazište resursa zajedničko korištenje (CORS)

###<a name="cross-domain-access-of-resources"></a>Pristup domenama resursa

Kada web-pregledniku koji se izvodi u jednu domenu izvrši HTTP zahtjev za resurs iz druge domene, to se naziva unakrsno polazište HTTP zahtjev. Ako, na primjer, HTML stranicu poslužena iz contoso.com čini zahtjev za jpeg hostirane na fabrikam.blob.core.windows.net. Sigurnosnih vam razloga preglednici ograničiti unakrsno polazište HTTP zahtjeva pokrenuto iz unutar skripte, kao što su JavaScript. To znači da kada neki JavaScript koda na web-stranicu contoso.com zahtjeve za razgovore te jpeg na fabrikam.blob.core.windows.net, web-pregledniku neće dopustiti zahtjev.

Što ne to morate učiniti s Azure prostora za pohranu? I, ako spremate statične sadržaje, kao što su JSON ili XML podatkovne datoteke u spremište blobova platforme pomoću računa za pohranu naziva Fabrikam, domenu za imovinu bit će fabrikam.blob.core.windows.net, a web-aplikacije contoso.com nećete moći im pristupati pomoću JavaScript jer su različiti domena. To vrijedi i ako pokušavate poziva nešto prostora za pohranu servisa Azure – kao što je spremište tablica – koji vraćaju podatke JSON obraditi JavaScript klijenta.

####<a name="possible-solutions"></a>Moguća rješenja

Da biste riješili taj problem je dodijeliti prilagođenu domenu kao što su "storage.contoso.com" fabrikam.blob.core.windows.net. Problem je da te prilagođene domene možete dodijeliti samo jedan za pohranu račun. Što se događa ako imovinu spremaju se u više računa za pohranu?

Da biste riješili taj problem i tako da bi se web-aplikacije poslužiti kao proxy za pozive za pohranu. To znači da Ako prenosite datoteke sa spremištem blobova, web-aplikacija će zapisivati lokalno i zatim je kopirajte blobova ili želite čitati sve to u memorije i pisati spremište blobova platforme. Umjesto toga možete napisati namjenski web-aplikacije (primjerice Web API-JA) koja se prenosi datoteke lokalno i zapisuje spremište blobova platforme. U svakom slučaju, imate račun za tu funkciju prilikom određivanja na skalabilnost mora.

####<a name="how-can-cors-help"></a>Kako se CORS može pomoći?

Azure prostora za pohranu omogućuje vam da biste omogućili CORS – Unakrsna Origin zajedničko korištenje resursa. Za svaki račun za pohranu, možete odrediti domene koje se može pristupiti resursima u taj račun za pohranu. Na primjer, u slučaju naš strukturirani iznad, ne možemo možete omogućiti CORS na računu za pohranu fabrikam.blob.core.windows.net i konfigurirajte ga tako da dopušta pristup contoso.com. Zatim contoso.com aplikacije web možete izravno pristupiti resursa u fabrikam.blob.core.windows.net.

Jedan napomenuti kako je da CORS omogućuje pristup, ali ne omogućuje provjeru autentičnosti, koje je potrebno za sve koje nisu javno access resursa za pohranu. To znači da samo pristupate blob-ova ako su javno ili uključiti zajednički pristup potpis dodjeljivanja odgovarajuću dozvolu. Tablica, redovima i datoteke nemaju pristup javnim, a zahtijevaju SAS.

Prema zadanim postavkama CORS onemogućen je na svim servisima. CORS možete omogućiti tako da jedan od načina da biste postavili pravila servisa poziva pomoću REST API-JA ili u biblioteku za pohranu klijenta. Kada to učinite, možete uključiti CORS pravila koja je u XML-u. Evo primjera CORS pravila koja je postavljena operacijom postaviti svojstva servisa za servis blobova platforme za račun za pohranu. Možete izvršiti pomoću klijentska biblioteka za pohranu ili REST API-ji za pohranu Azure postupak.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Evo što znači da svaki redak:

-   **AllowedOrigins** Ovo je govori domene koji nije odgovarajuće možete zatražiti i primanje podataka iz servisa za pohranu. Piše da contoso.com i fabrikam.com možete zatražiti podataka iz spremišta blobova računa određene prostora za pohranu. Možete postaviti i to na zamjenski znak (\*) da biste omogućili sve domene zahtjeva za pristup.

-   **AllowedMethods** Ovo je popis metode (HTTP zahtjev glagoli) koji se mogu koristiti pri stvaranju zahtjev. U ovom primjeru STAVI i GET dopušteno. To možete učiniti da biste zamjenski znak (\*) da biste omogućili sve načine koja će se koristiti.

-   **AllowedHeaders** Ovo je zahtjev za zaglavlja koja domenu izvor možete odrediti za upućivanje zahtjeva. U ovom primjeru sva metapodataka zaglavlja počevši od x-ms-meta-podataka, x-ms-meta-cilja, a x-ms-meta-abc dopušteno. Zamjenski znak (\*) označava početak bilo kojeg zaglavlja s prefiksom navedeni dopuštena.

-   **ExposedHeaders** Pokazuje koji zaglavlja odgovor treba vidljiva pomoću preglednika da biste izdavača zahtjev. U ovom primjeru bilo koje zaglavlje počevši od "x-ms - meta-" će biti vidljiva.

-   **MaxAgeInSeconds** Ovo je maksimalnu količinu vremena da u pregledniku predmemoriju prije isporuke zahtjev za mogućnosti. (Dodatne informacije o zahtjevu za prije isporuke provjerite prvog članka dolje.)

####<a name="resources"></a>Resursi

Dodatne informacije o CORS i kako ga omogućiti Provjerite ove resursima.

-   [Zajedničko korištenje (CORS) podrška za servisa Azure prostora za pohranu na Azure.com resursa za više izvora](storage-cors-support.md)

    Ovaj članak sadrži pregled CORS i kako postaviti pravila za servise za različite pohranu.

-   [Zajedničko korištenje (CORS) podrška za Azure prostora za pohranu Services na MSDN-resursa za više izvora](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Ovo je CORS podrška za Azure servise za pohranu potražite u dokumentaciji referencu. Sadrži veze na članke primjene svaki servis za pohranu i prikazuje primjer te objašnjava svaki element u datoteci CORS.

-   [Microsoft Azure prostor za pohranu: Uvod u CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    Ovo je veza na članak prvotna blog objavljivanje CORS i pokazuje kako je koristiti.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Najčešća pitanja o sigurnosti Azure prostora za pohranu

1.  **Kako provjeriti integritet blob-ova mogu se prijenos pojavljivanje ili iščezavanje Azure prostora za pohranu ako ne možete koristiti HTTPS protokola?**

    Ako iz bilo kojeg razloga, morate koristiti HTTP umjesto HTTPS, a vi radite bloka blob-ova, možete koristiti Provjera MD5 provjeriti integritet prenose blob-ova. To će pomoći s Zaštita od mrežni prijenos sloja pogreške, ali ne nužno privremene napada.

    Ako koristite HTTPS, koji omogućuje prijenos razinu sigurnosti, korištenje provjere MD5 je redundantnih i nepotrebne.
    
    Dodatne informacije, provjerite [Azure Blob MD5 pregled](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).

2.  **Što je s FIPS usklađenost za vlade SAD-a?**

    U Sjedinjenim Američkim Državama Savezna informacije Processing Standard (FIPS) definira šifriranja algoritama za korištenje odobri sad Savezna državne računalne sustava za zaštitu osjetljivih podataka. Omogućivanje FIPS načinu rada na Windows server ili radne površine govori OS-a hoće li se koristiti samo FIPS-provjeriti šifriranja algoritama. Ako aplikacija koristi koje nisu usklađene sa algoritmi, aplikacija će prekinuti. With.NET Framework verzije 4.5.2 ili noviji, program automatski će se prebaciti algoritama šifriranja da biste koristili usklađeni s FIPS algoritama kada je računalo u načinu FIPS.

    Microsoft se ostavlja najviše svakoga od njih da biste odlučili želite li omogućiti način FIPS. Ne možemo smatrate postoji nema dovoljno dobar razlog za korisnike koji ne podliježe pravilnicima za državne ustanove da biste omogućili način FIPS prema zadanim postavkama.

    **Resursi**

-   [Zašto mi se ne preporučiti "Način FIPS" više](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    Članak na blogu daje pregled FIPS i objašnjava zašto ne omogućuju Način FIPS prema zadanim postavkama.

-   [FIPS 140 provjere valjanosti](https://technet.microsoft.com/library/cc750357.aspx)

    Ovaj članak sadrži informacije na kako Microsoftovih proizvoda i šifriranja module u skladu s FIPS standard sad Savezna državne.

-   ["Sustava šifriranja: korištenje FIPS usklađen algoritama za šifriranje, raspršivanje i potpisivanje" sigurnosne postavke efekata u sustavu Windows XP i novijim verzijama sustava Windows](https://support.microsoft.com/kb/811833)

    U ovom se članku govori o korištenju Način FIPS u starija računala sa sustavom Windows.
