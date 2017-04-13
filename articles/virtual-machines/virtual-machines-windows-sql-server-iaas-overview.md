<properties
    pageTitle="Pregled sustava SQL Server na virtualnim računalima za Azure | Microsoft Azure"
    description="Saznajte kako pokrenuti cijelog izdanja sustava SQL Server Azure virtualnog računala. Stvorite Izravni veze na sve slike SQL Server VM i povezani sadržaj."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Pregled sustava SQL Server na virtualnim računalima za Azure

U ovoj se temi opisuju mogućnosti za pokretanje sustava SQL Server na virtualnim Azure računalima (VMs), [veze portala slike](#option-1-create-a-sql-vm-with-per-minute-licensing) i pregled [uobičajenih zadataka](#manage-your-sql-vm).

>[AZURE.NOTE] Ako ste već upoznati s SQL Server, a samo želite li vidjeti kako implementirati na SQL Server VM, potražite u članku [Dodjeljivanje virtualnog računala za SQL Server Azure portalu](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Pregled
Ako ste administrator baze podataka ili razvojni inženjer Azure VMs omogućuje da biste premjestili radnih opterećenja lokalnog sustava SQL Server i aplikacije u oblak. U sljedećem videozapisu naći Tehnički pregled sustava SQL Server Azure VMs.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Videozapis pokriva sljedećih područja:

|Vrijeme|Područje|
|---|---|
| 00:21 | Što su Azure VMs? |
| 01:45 | Sigurnost |
| 02:50 | Povezivanje |
| 03:30 | Prostor za pohranu pouzdanost i performanse |
| 05:20 | Veličina VM |
| 05:54 | Visoke dostupnosti i SLA |
| 07:30 | Podrška za konfiguraciju |
| 08:00 | Nadzor |
| 08:32 | Pokazni videozapis: Stvaranje na VM SQL Server 2016 |

>[AZURE.NOTE] Videozapis usredotočuje se na SQL Server 2016, ali Azure nudi VM slike za više verzija sustava SQL Server, uključujući 2008, 2012, 2014 i 2016. 

## <a name="scenarios"></a>Scenariji
Postoji mnogo je razloga zbog koje se mogu odlučiti za hostiranje podataka u Azure. Ako aplikacija da je premjestite na Azure, poboljšava performanse i premještanje podataka. No postoje drugi prednosti. Automatski imati pristup više podataka centara za globalnu podaci o prisutnosti i Izrada oporavak. Podaci i vrlo sigurnih i durable.

SQL Server na Azure VMs je jednu mogućnost za pohranu relacijskih podataka u Azure. Je dobar izbor za nekoliko scenarija. Na primjer, možda ćete morati konfigurirati Azure VM slično kao moguće je računalo lokalnog sustava SQL Server. Ili, možda ćete morati pokrenuti dodatne aplikacija i servisa na istom poslužitelju baze podataka. Postoje dva glavna resursa kojih možete shvatiti kroz još više scenariji i napomene:

 - [SQL Server na virtualnim računalima za Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) sadrži pregled najbolje scenariji za korištenje sustava SQL Server Azure VMs. 
 - [Odaberite oblak mogućnost SQL Server: baze podataka SQL Azure (PaaS) ili SQL Server na Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) sadrži detaljne usporedbe između SQL baze podataka i SQL Server sustavom na VM.

## <a name="create-a-new-sql-vm"></a>Stvaranje nove VM SQL
Sljedeći odjeljci sadrže izravne veze na portalu za Azure Galerija slika za SQL Server virtualnog računala. Ovisno o slike koje odaberete, možete ili plaćanje za SQL Server licenciranje troškove na temelju-minutni ili možete prenijeti vlastite licence (BYOL).

Pronađite detaljne upute za taj postupak u ovom praktičnom vodiču, [Nakon dodjele resursa virtualnog računala za SQL Server Azure portalu](virtual-machines-windows-portal-sql-server-provision.md). Osim toga, pregledajte na [performanse najbolje prakse za SQL Server VMs](virtual-machines-windows-sql-performance.md), koji se objašnjava da biste odabrali odgovarajući strojno veličina i druge značajke dostupne prilikom dodjele resursa.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Mogućnost 1: Stvaranje SQL VM s-minutni licenciranje
Sljedeća tablica sadrži matrica dostupnih slika sustava SQL Server u galeriji virtualnog računala. Kliknite bilo koju vezu da biste započeli stvaranje nove SQL VM s navedenom verziju, edition i operacijski sustav.

|Verzija|Operacijski sustav|Izdanje|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[ [Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [standardni](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), web-mjesto](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Razvojni](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL SP1 2014.**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [standardni](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014.**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [standardni](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [standardni](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [standardni](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012.|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [standardni](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [standardni](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012.|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Mogućnost 2: Stvaranje SQL VM s postojeća licenca
Možete premjestiti i vlastitu licence (BYOL). U ovom scenariju samo platite VM bez bilo kakvih dodatnih troškova za licenciranje sustava SQL Server. Da biste koristili vlastiti licence, koristite matricu verzija sustava SQL Server, izdanja i operacijski sustavi ispod. Na portalu mjestu su nazivi slika s **{BYOL}**.

|Verzija|Operacijski sustav|Izdanje|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [standardni BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SP1 SQL Server 2014.**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [standardni BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [standardni BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Da biste koristili BYOL VM slike, morate imati i Enterprise Agreement s [Mobilnost licenci putem Tehnološko jamstvo na Azure](https://azure.microsoft.com/pricing/license-mobility/). Morate valjana licenca za verziju/izdanja sustava SQL Server koji želite koristiti. Morate [unijeti informacije potrebne BYOL za Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) **10** dana aktiviranja sustava VM.

## <a name="manage-your-sql-vm"></a>Upravljanje vaše VM SQL
Nakon dodjele resursa sustava SQL Server VM, postoji nekoliko zadataka upravljanja nije obavezno. U mnogo aspekte, konfiguriranje i upravljanje SQL Server točno onako kao da bi upravljanje lokalnim instance sustava SQL Server. Međutim, neki zadaci su specifične za Azure. U sljedećim se odjeljcima istaknute su neke od tih područja veze na dodatne informacije.

### <a name="connect-to-the-vm"></a>Povezivanje s VM
Jedna od najčešće Osnovni koraci za upravljanje je povezati s sustava SQL Server VM kroz alate, kao što je SQL Server Management Studio (SSMS). Upute o povezivanju svoje nove VM SQL Server potražite u članku [povezivanje za SQL Server virtualni stroj na Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migracija podataka
Ako imate postojeću bazu podataka, ćete htjeti premjestiti koji upravo dodijeljenu VM SQL. Popis mogućnosti migracije i upute, potražite u članku [Migracija baze podataka SQL Server na programa Azure VM](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Konfiguriranje visoke dostupnosti
Ako tražite visoke dostupnosti, razmislite o konfiguriranju grupe dostupnosti za SQL Server. To obuhvaća više Azure VMs u virtualne mreže. Portal za Azure sadrži predložak koji postavlja tu konfiguraciju umjesto vas. Dodatne informacije potražite u članku [Konfiguriranje grupe dostupnosti AlwaysOn na Voditelj resursa Azure virtualnim računalima sustava](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Ako želite ručno konfiguriranje grupe dostupnosti i pridruženi ga slušatelj potražite u članku [Konfiguriranje grupe dostupnosti AlwaysOn u Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Ostala razmatranja visoke dostupnosti potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Stvaranje sigurnosne kopije podataka
Azure VMs iskoristite prednosti [Automatskog sigurnosnog kopiranja](virtual-machines-windows-sql-automated-backup.md), čime se redovito sigurnosno kopiranje baze podataka sa spremištem blobova. Ovu tehniku možete i ručno koristiti. Dodatne informacije potražite u članku [Korištenje Azure prostor za pohranu i vraćanja sigurnosne kopije sustava SQL Server](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Da biste saznali sve mogućnosti sigurnosnog kopiranja i vraćanja potražite u članku [sigurnosnog kopiranja i vraćanja za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatiziranje ažuriranja
Azure VMs možete [Automatski zakrpa](virtual-machines-windows-sql-automated-patching.md) zakažite pomoću prozora Održavanje za instaliranje važne sustava windows i SQL Server automatski ažurira.

### <a name="customer-experience-improvement-program-ceip"></a>Program unaprjeđivanja zadovoljstva korisnika (CEIP)
Po zadanom je omogućena na unaprjeđivanja Program (CEIP). To povremeno šalje izvješća Microsoftu radi poboljšanja sustava SQL Server. Postoji bez zadatka upravljanja obavezna CEIP osim ako ne želite da biste onemogućili nakon dodjele resursa. Možete prilagoditi ili onemogućite CEIP povezivanjem VM putem udaljene radne površine. Izvedite uslužni **pogreška SQL poslužitelja i izvješćivanje o korištenju** . Slijedite upute da biste onemogućili izvješćivanje. 

Dodatne informacije potražite u odjeljku CEIP [Prihvatite licencne odredbe](https://msdn.microsoft.com/library/ms143343.aspx) teme. 

## <a name="next-steps"></a>Daljnji koraci
[Istraživanje tečaj](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) za SQL Server na virtualnim računalima za Azure.

Dodatne pitanje? Najprije potražite u članku [SQL Server na najčešća pitanja vezana uz Azure virtualnih računala](virtual-machines-windows-sql-server-iaas-faq.md). No pri dnu bilo koje teme SQL VM interakciju s web-mjesto Microsoft i u okvir za zajednicu dodati pitanja i komentare.
