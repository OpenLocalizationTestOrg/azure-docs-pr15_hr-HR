<properties
    pageTitle="Performanse najbolje prakse za SQL Server | Microsoft Azure"
    description="Nudi najbolje prakse za optimiziranje performanse sustava SQL Server u programu Microsoft Azure VMs."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure

## <a name="overview"></a>Pregled

Ova tema sadrži najbolje prakse za optimiziranje performanse sustava SQL Server u programu Microsoft Azure virtualnog računala. Prilikom pokretanja sustava SQL Server na virtualnim računalima sustava u Azure, preporučujemo da nastavite koristiti isti mogućnosti koje se primjenjuju na SQL Server u okruženju lokalnog poslužitelja ugađanju performansi za bazu podataka. Međutim, performanse relacijske baze podataka u javnom oblaka ovisi o mnogo je čimbenika kao što su veličina virtualnog računala i konfiguracija diskova podataka.

Prilikom stvaranja slike sustava SQL Server, [razmislite o dodjeljivanje vaše VMs na portalu za Azure](virtual-machines-windows-portal-sql-server-provision.md). SQL Server VMs dodjeli na portalu s Voditelj resursa implementirate sve najbolje prakse, uključujući konfiguraciju prostora za pohranu.

Ovaj članak namijenjen je postizanje *najbolje* performanse za SQL Server na Azure VMs. Ako je vaš radno opterećenje manje zahtjevne, možda ne zahtijeva svakih optimizaciju navedena u nastavku. Razmislite o potrebama performanse i radno opterećenje uzoraka kao procijeniti te preporuke.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Brzi kontrolni popis

Slijedi popis Brza provjera postigli optimalne performanse sustava SQL Server na virtualnim strojevima sa sustavom Azure:

|Područje|Optimizacije|
|---|---|
|[Veličina VM](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) ili noviji za SQL Enterprise edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) ili noviji za izdanja sustava SQL standardne i na webu.|
|[Prostor za pohranu](#storage-guidance)|Korištenje [Premium prostora za pohranu](../storage/storage-premium-storage.md). Standardni prostora za pohranu preporučuje samo razvojni i testiranje.<br/><br/>[Račun za pohranu](../storage/storage-create-storage-account.md) i SQL Server VM čuvajte na istom području.<br/><br/>Onemogućivanje Azure [zemlj suvišnih prostora za pohranu](../storage/storage-redundancy.md) (zemlj replikacije) na računu za pohranu.|
|[Diskova](#disks-guidance)|Koristite najmanje 2 [P30 diskova](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1 za datoteke zapisnika; 1 za podatkovne datoteke i TempDB).<br/><br/>Izbjegavanje korištenja operacijski sustav ili privremene diskova za pohranu baze podataka ili zapisivanje.<br/><br/>Omogući pročitajte predmemoriranje diskove hosting podatkovne datoteke i TempDB.<br/><br/>Omogućivanje predmemoriranja na diskove koji se nalaze datoteke zapisnika.<br/><br/>Važno: Zaustaviti servis sustava SQL Server prilikom promjene postavki predmemorije za na disk Azure VM.<br/><br/>Stripe više diskova Azure podataka da biste dobili veću propusnost IO.<br/><br/>Oblik s navedenih dodijeljeni veličine.|
|[/ I](#io-guidance)|Omogućite sažimanje stranice baze podataka.<br/><br/>Omogućivanje Inicijalizacija izravne datoteka za podatkovne datoteke.<br/><br/>Ograničiti ili onemogućite autogrow bazu podataka.<br/><br/>Onemogućivanje autoshrink bazu podataka.<br/><br/>Premještanje sve baze podataka diskova podataka, uključujući baze podataka sustava.<br/><br/>Premještanje sustava SQL Server pogreške zapisnika i praćenje datoteke direktorija diskova podataka.<br/><br/>Postavljanje zadane sigurnosne kopije i baza podataka mjesta datoteke.<br/><br/>Omogući zaključanih stranica.<br/><br/>Primjena ispravaka performanse sustava SQL Server.|
|[Posebne značajke](#feature-specific-guidance)|Sigurnosno kopirajte izravno sa spremištem blobova.|

Dodatne informacije o *kako* i *Zašto* da biste te optimizacije pregledajte pojedinosti i upute navedene u sljedećim odjeljcima.

## <a name="vm-size-guidance"></a>Upute za veličina VM

Za performanse osjetljive aplikacije, preporučuje se da koristite sljedeće [virtualnim strojevima veličine](virtual-machines-windows-sizes.md):

- **SQL Server Enterprise Edition**: DS3 ili noviji

- **SQL Server Standard Web izdanja i**: DS2 ili noviji


## <a name="storage-guidance"></a>Upute za pohranu

DS-niza (zajedno DSv2 niza i Oznaka niz) VMs službe za podršku [Za pohranu Premium](../storage/storage-premium-storage.md). Za sve radnih opterećenja radnog preporučuje se Premium prostora za pohranu.

> [AZURE.WARNING] Standardni i prostor za pohranu sadrži različitim latencies i propusnosti preporučuje samo radnih opterećenja razvojni i testiranje. Proizvodne radnih opterećenja trebali biste koristiti Premium prostora za pohranu.

Uz to, preporučujemo da stvorite račun za Azure prostora za pohranu u centru za iste podatke kao SQL Server virtualnih računala da biste smanjili kašnjenja prijenos. Prilikom stvaranja računa za pohranu, onemogućite zemlj replikaciju kao što je redoslijed dosljedan pisanja preko više diskova se zajamčeno. Umjesto toga, razmislite o konfiguriranju tehnologija sustava SQL Server Izrada oporavak između dvije Azure podataka centrima. Dodatne informacije potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Upute za diskova

Postoje tri vrste glavni disk na programa Azure VM:

- **Na disku OS**: prilikom stvaranja programa Azure virtualnog računala platforme će priložiti najmanje jedan disk (koji je označen kao pogon **C** ) VM za vaš operacijski sustav disk. Disk je VHD pohranjenih kao blob stranice u prostor za pohranu.
- **Privremeni diska**: virtualnim računalima sustava Azure sadrže drugi disk pod nazivom privremene disk (koje su označene kao **D**: pogon). Ovo je na disk čvor koje je moguće koristiti za prostor za odlaganje.
- **Diskova podataka**: dodatnih diskova možete priložiti u virtualnog računala kao diskova podataka i oni će se spremiti u prostor za pohranu kao BLOB-Ova stranica.

U sljedećim odjeljcima opisuju preporuke za korištenje tih različitih diskova.

### <a name="operating-system-disk"></a>Operacijski sustav na disku

U operacijskom sustavu disk je VHD koje možete pokrenuti i postavljanje kao tekući verzija operacijskog sustava i označena je kao pogon **C** .

Zadana postavka predmemoriranje pravila na disku operacijski sustav je **Čitanje/pisanje**. Za performanse osjetljive aplikacije, preporučujemo da koristite diskova podataka umjesto disk operacijski sustav. U odjeljku diskova podataka u nastavku.

### <a name="temporary-disk"></a>Privremeni disk

Privremeno spremište pogon, koje su označene kao **D**: pogon, je ista i na spremište blobova platforme Azure. Spremanje datoteke baze podataka za korisnika ili datoteke zapisnika transakcije korisnika na **D**: pogon.

D niz, Dv2 niza i VMs G niz privremene pogon na te VMs je utemeljen na SSD. Ako svoje radno opterećenje čini intenzivnog korištenja TempDB (npr. za privremenih objekata ili spojeva složene), pohranjivanje TempDB na pogonu **D** može rezultirati veću propusnost TempDB i donjem Latencija TempDB.

Za VMs koji podržavaju pohranu Premium (DS niz, DSv2 niza i Oznaka nizova), preporučujemo da pohranite TempDB i/ili međuspremnik skup proširenja na disk koji podržava pohranu Premium s čitanja Predmemoriranje omogućeno. Nema izuzetaka za preporuke; Ako je upotreba TempDB pisanje ćete morati usko, možete postići veći performanse spremanjem TempDB na lokalnom pogonu **D** , koja je i SSD utemeljen na te veličine računala.

### <a name="data-disks"></a>Diskova podataka

- **Korištenje podataka diskova za podatke i datoteka zapisnika**: barem 2 za pohranu Premium koristiti [P30 diskova](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) gdje jedan disk sadrži datoteka zapisnika, a ostale datoteke podataka i TempDB.

- **Podjela na disku**: za dodatne propusnost možete dodati dodatne podatke diskova i koristiti Podjela Disk. Da biste utvrdili broj diskova podataka, morate analizirati broj IOPS koje su dostupne za vaše podatke i zapisnika diskove. Te informacije potražite u članku tablice na IOPS po [Veličina VM](virtual-machines-windows-sizes.md) i diska veličina u sljedećem članku: [Korištenje Premium prostor za pohranu diskova](../storage/storage-premium-storage.md). Koristite sljedeće ove smjernice:

    - Za Windows 8 i Windows Server 2012 ili noviji, koristite [Za pohranu razmake](https://technet.microsoft.com/library/hh831739.aspx). Postavite veličinu pruga 64 KB za OLTP radnih opterećenja i 256 KB za skladištenje radnih opterećenja podataka da biste izbjegli utjecaj na performanse zbog nepravilno poravnanje particije. Osim toga, postavite broj stupaca = broj fizičkih diskova. Da biste konfigurirali prostor za pohranu više od 8 diskova za izričito postavite željeni broj stupaca koji odgovara broju diskova morate koristiti PowerShell (ne Upravitelj poslužitelja UI). Dodatne informacije o konfiguriranju [Prostori za pohranu](https://technet.microsoft.com/library/hh831739.aspx)potražite u članku [Cmdleti za pohranu razmaka u ljusci Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx)

    - Windows 2008 R2 ili neke starije verzije, možete koristiti dinamičkih diskova (OS Prugasta količine), a veličina pruge uvijek je 64 KB. Imajte na umu da ova mogućnost je zastario na Windows 8 i Windows Server 2012. Informacije potražite u izjavi podrške pri [virtualne Disk Service je pri prijelazu na Windows prostora za pohranu upravljanje API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

    - Ako svoje radno opterećenje nije prijavljen intenzivno i namjenski IOPs nije potrebna, možete konfigurirati samo jedan skup prostora za pohranu. U suprotnom, stvorite dvije grupe za pohranu, jedan za datoteke zapisnika i drugi skup za pohranu za podatkovne datoteke i TempDB. Utvrdite broj diskova pridružene svaki skup prostora za pohranu koji se temelji na vaša očekivanja učitavanja. Imajte na umu da različitim veličinama VM dopuštaju različiti broj podataka priložene diskova. Dodatne informacije potražite u članku [veličine za virtualnih računala](virtual-machines-windows-sizes.md).

    - Ako se ne koriste Premium prostora za pohranu (razvojni i testiranje scenariji), za preporuke je da biste dodali maksimalni broj diskova podataka koje podržava [Veličina VM](virtual-machines-windows-sizes.md) i koristiti Podjela Disk.

- **Predmemoriranje pravila**: diskova podataka za pohranu Premium, omogućiti čitanje predmemoriranje diskova podataka samo hosting podatkovne datoteke i TempDB. Ako se ne koriste Premium prostora za pohranu, omogućivanje predmemoriranja na bilo kojem diskova podataka. Upute za konfiguriranje predmemoriranja na disku, potražite u sljedećim temama: [Postavljanje AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) i [Postavljanje AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] Nakon promjene postavke predmemorije Azure VM diskova da biste izbjegli nastanka bilo kakvo oštećenje baze podataka, zaustavite servis sustava SQL Server.

- **Veličina jedinice za dodjelu NTFS**: kada oblikujete disk podataka, preporučuje se korištenje veličina jedinice za dodjelu 64 KB podataka i datoteka zapisnika, kao i TempDB.

- **Najbolje prakse za upravljanje diska**: kada uklanjanjem podatkovni disk ili vrsta predmemorije, mijenja zaustaviti servis sustava SQL Server tijekom promjene. Kada na disku OS mijenjaju se postavkama predmemoriranja, Azure zaustavlja na VM, mijenja vrstu predmemorije i pokreće na VM. Postavke predmemorije diska podaci mijenjaju, na VM je zaustavljena, ali podatkovni disk je odvojene iz na VM tijekom promjene, a zatim reattached.

    >[AZURE.WARNING] Nije uspjelo Zaustavljanje servisa SQL Server tijekom te operacije može uzrokovati oštećenje baze podataka.

## <a name="io-guidance"></a>/ I upute

- Najbolje rezultate s Premium prostora za pohranu postići kada parallelize aplikacije i zahtjeve. Prostor za pohranu Premium namijenjen je scenarijima kojima dubina reda čekanja IO veća od 1, pa će malo ili nimalo performanse pozitivne jednom niti serijski zahtjeva za (čak i ako su za pohranu intenzivno). Na primjer, to utječe na jednom niti test rezultate alata za analizu performanse, kao što su SQLIO.

- Razmislite o korištenju [sažimanja stranice bazu podataka](https://msdn.microsoft.com/library/cc280449.aspx) kao što je može pridonijeti poboljšanju performansi/i intenzivno radnih opterećenja. Međutim, spajanja podataka mogu povećati potrošnju procesora na poslužitelju baze podataka.

- Razmislite o omogućivanju Inicijalizacija izravnu datoteku da biste smanjili vrijeme nužan za dodijeljeni početne datoteke. Da biste iskoristili Inicijalizacija izravne datoteke, dopustiti račun servisa SQL Server (MSSQLSERVER) s SE_MANAGE_VOLUME_NAME i dodavanje sigurnosnog pravilnika **Obavljati zadatke održavanja glasnoću** . Ako koristite sliku platforme SQL Server za Azure zadanog računa servisa (NT Service\MSSQLSERVER) neće dodati sigurnosna pravila **Obavljati zadatke održavanja glasnoću** . Drugim riječima, Inicijalizacija izravne datoteka nije omogućena u sliku platforme SQL Server Azure. Nakon dodavanja računa servisa SQL Server sigurnosna pravila **Obavljati zadatke održavanja glasnoće** , ponovno pokrenite servis sustava SQL Server. Mogući su sigurnosna pitanja vezana uz za korištenje ove značajke. Dodatne informacije potražite u članku [Pokretanje datoteke baze podataka](https://msdn.microsoft.com/library/ms175935.aspx).

- **autogrow** smatra se jednostavno contingency za neočekivane rasta. Upravljanje podacima i zapisnika growth svakodnevno s autogrow. Ako se koristi autogrow unaprijed Povećaj datoteku pomoću promjenu veličine.

- Provjerite je li **autoshrink** onemogućen je da bi se izbjeglo nepotrebno indirektni koji mogu negativno utjecati na performanse.

- Premještanje sve baze podataka diskova podataka, uključujući baze podataka sustava. Dodatne informacije potražite u članku [Premještanje baza podataka sustava](https://msdn.microsoft.com/library/ms345408.aspx).

- Premještanje sustava SQL Server pogreške zapisnika i praćenje datoteke direktorija diskova podataka. To možete učiniti u Upravitelj konfiguracije SQL poslužitelja tako da desnom tipkom miša kliknete instancu sustava SQL Server, a zatim odaberite Svojstva. Pogreške zapisnika i praćenje datoteke postavke možete promijeniti u karticu **Parametri pokretanja** . Ispis direktorija naveden je u karticu **Napredno** . Sljedeće snimka zaslona prikazuje gdje treba tražiti pogreška zapisnika pokretanja parametar.

    ![Snimka zaslona ErrorLog SQL](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Postavljanje zadane sigurnosne kopije i baza podataka mjesta datoteke. Koristite preporuke u ovoj temi i unesite željene promjene u prozoru svojstva poslužitelja. Upute potražite u članku [Pregled ili promjena mjesta zadano za podatke i datoteke zapisnika (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Sljedeća slika prikazuje gdje da te promjene.

    ![Datoteka zapisnika podataka SQL i sigurnosnog kopiranja](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Omogući zaključanih stranica da biste smanjili IO i željene aktivnosti broja stranica. Dodatne informacije potražite u članku [Omogućivanje stranice zaključavanje memorije mogućnost (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Ako koristite sustav SQL Server 2012, instalirajte servis Service Pack 1 kumulativne ažuriranje 10. To ažuriranje sadrži popravak za slabe performanse na/i kada je izvršiti odabir u privremena izjava u sustavu SQL Server 2012. Informacije potražite u [članku iz baze znanja](http://support.microsoft.com/kb/2958012).

- Razmislite o komprimiranju sve podatkovne datoteke pri prijenosu u/izvan programa Azure.

## <a name="feature-specific-guidance"></a>Upute za određene značajke

Neke implementacije možda postigli dodatne performanse prednosti korištenja više napredne postupke za konfiguriranje. Na sljedećem su popisu ističe neke značajke sustava SQL Server koje omogućuju da biste postigli bolje performanse:

- **Sigurnosno kopiranje Azure za pohranu**: prilikom izvršavanja sigurnosne kopije za SQL Server koji se izvodi u Azure virtualnih računala, možete koristiti [Sigurnosnu kopiju SQL Server na URL](https://msdn.microsoft.com/library/dn435916.aspx). Ta je značajka dostupna počevši od SQL Server 2012 SP1 CU2 i preporučuje sigurnosnom najviše diskova priloženom podataka. Kada ste sigurnosno kopiranje i vraćanje iz Azure prostor za pohranu, slijedite preporuke na [SQL Server sigurnosne kopije URL Praktični savjeti i otklanjanje poteškoća i vraćanja iz sigurnosne kopije pohranjene u Azure prostora za pohranu](https://msdn.microsoft.com/library/jj919149.aspx). Možete automatizirati i ove sigurnosne kopije pomoću [Automatskog sigurnosnog kopiranja za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-classic-sql-automated-backup.md).

    Prije no što SQL Server 2012, možete koristiti [Sigurnosne kopije SQL Server Azure alat](https://www.microsoft.com/download/details.aspx?id=40740). Ovaj alat omogućuju da biste povećali sigurnosne kopije propusnost pomoću više sigurnosne kopije pruga ciljnih web-mjesta.

- **SQL Server podatkovne datoteke u Azure**: ta novi, [SQL Server podatkovne datoteke u Azure](https://msdn.microsoft.com/library/dn385720.aspx)je značajka dostupna počevši od 2014 SQL Server. Pokretanje sustava SQL Server s podatkovnim datotekama u Azure pokazuje usporediti performanse značajke kao što su korištenje diskova Azure podataka.

## <a name="next-steps"></a>Daljnji koraci

Ako vas zanima Istraživanje SQL Server i pohranu Premium još detaljniju, potražite u članku [Korištenje Azure Premium prostora za pohranu sa sustavom SQL Server na virtualnim računalima](virtual-machines-windows-classic-sql-server-premium-storage.md).

Najbolje prakse za sigurnost, potražite u članku [Sigurnosna pitanja vezana uz za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-security.md).

Pregledajte druge teme sustava SQL Server virtualnog računala na [SQL Server na pregled virtualnim strojevima Azure](virtual-machines-windows-sql-server-iaas-overview.md).
