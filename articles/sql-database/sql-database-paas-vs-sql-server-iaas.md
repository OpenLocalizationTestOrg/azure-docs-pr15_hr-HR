<properties
    pageTitle="Baze podataka SQL (PaaS) i SQL Server u oblaku na VMs (IaaS) | Microsoft Azure"
    description="Saznajte koju mogućnost SQL Server oblaka odgovara aplikacije: baze podataka SQL Azure (PaaS) ili SQL Server u oblaku na virtualnim strojevima sa sustavom Azure."
    services="sql-database, virtual-machines"
    keywords="Oblak sustava SQL Server, SQL Server u oblaku, PaaS baze podataka, SQL Server, DBaaS u oblaku"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Odaberite oblak sustava SQL Server mogućnost: baze podataka SQL Azure (PaaS) ili SQL Server na Azure VMs (IaaS)

Azure sastoji se od dvije mogućnosti za smještanje radnih opterećenja sustava SQL Server u Microsoft Azure:

* [Baze podataka SQL Azure](https://azure.microsoft.com/services/sql-database/): baze podataka A SQL izvorni u oblak, poznata i kao platforme kao baze podataka usluge (PaaS) ili baze podataka usluge (DBaaS) koja je optimizirana za razvoj aplikacija za softver-kao-na-service (SaaS). Nudi kompatibilnosti s većina značajki sustava SQL Server. Dodatne informacije o PaaS potražite u članku [što je PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server na virtualnim strojevima sa sustavom Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server instaliran i smješten u oblak na Windows Server virtualnim strojevima (VMs) sustavom Azure, poznata i kao programa infrastrukture kao service (IaaS).
SQL Server na virtualnim računalima za Azure optimiziran je za prijenos postojećih aplikacija sustava SQL Server. Dostupne su sve verzije i izdanja sustava SQL Server. Nudi 100% kompatibilnost sa sustavom SQL Server, što omogućuje hostira proizvoljan broj baza podataka kao potrebne i izvođenje transakcija izdvojiti bazu podataka. Nudi Potpuna kontrola na SQL Server i Windows.

Saznajte kako svake mogućnosti pristaju platformu Microsoft podataka i pomoć podudaranja pravu mogućnost da biste s poslovnim potrebama. Hoće li vam određivanje prioriteta ušteda troškova ili minimalnog administraciju na sve ostalo, pomoći da odlučite pristupa nudi prema potrebama tvrtke koje najviše zanimaju ovog članka.


## <a name="microsofts-data-platform"></a>Platforme podataka tvrtke Microsoft

Nešto od prvog da biste shvatili u sve rasprave Azure usporedbi baza podataka sustava SQL Server lokalnog je da biste je mogli koristiti sve. Platforme podataka tvrtke Microsoft upravlja tehnologije SQL Server, a čini dostupnim preko fizičke lokalnog računala, privatni oblaka okruženja, drugih proizvođača hostiranu privatne oblaka okruženja te javno oblaka. SQL Server na Azure virtualne mchines omogućuje potrebama tvrtke jedinstven i raznih kombinacijom lokalne i implementacije oblak hostira pri korištenju isti skup poslužiteljskim proizvodima, Alati za razvoj i stručna znanja preko tih okruženja.

   ![Mogućnosti sustava SQL Server u oblaku: SQL server na IaaS ili SaaS SQL baze podataka u oblaku.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Kao što se vidi u dijagramu, svakog izdanja možete se određene razine Administracija imate putem infrastrukture (na osi X) i stupanj trošak učinkovitosti postići razinu konsolidacije baze podataka i automatizaciju (na osi Y).

Prilikom dizajniranja aplikacije za hostiranje sustava SQL Server dio aplikacije dostupne su četiri osnovne mogućnosti:

- SQL Server na koje nisu virtualiziranom fizičke
- SQL Server na lokalni virtualiziranom strojeva (privatne oblaka)
- SQL Server u Azure virtualnog računala (Microsoft cloud javno)
- Azure SQL baze podataka (Microsoft cloud javno)

U sljedećim se odjeljcima saznat ćete o SQL Server na Microsoft cloud javno: baze podataka SQL Azure i SQL Server na Azure VMs. Osim toga, Istražite uobičajene poslovne motivators za određivanje koju mogućnost najbolje za svoju aplikaciju.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Detaljnije Upoznavanje baze podataka SQL Azure i SQL Server na Azure VMs

**Baze podataka SQL Azure** je relacijske baze podataka – kao-na-servis (DBaaS) smješten u Azure oblaka koja se nalazi u kategorije industrijskih softver-kao-na-(SaaS) i *Servis* *platformu-kao-na-(PaaS)*. [SQL baze podataka](sql-database-technical-overview.md) se temelji na standardizirani hardvera i softvera koja je vlasnik, hostira i održava Microsoft. SQL baze podataka, možete razviti izravno na servis pomoću ugrađene značajke i funkcije. Kada koristite baze podataka SQL, koje pay-as-you-go s mogućnostima za promjenu veličine prema gore ili prema van za veće power s prekida.

**SQL Server na virtualnim računalima sustava Azure (VMs)** spada u kategoriju industrijskih *Infrastrukture-kao-na-Service (IaaS)* , a omogućuje vam pokretanje sustava SQL Server unutar virtualnog računala u oblaku. Slično SQL baze podataka, ona je utemeljena na standardizirani hardver koji je vlasnik, hostira i održava Microsoft. Pri korištenju sustava SQL Server na na VM, možete ga ili plaćanje-kao vam odasvud licence sustava SQL Server već uključeni u sliku sustava SQL Server ili jednostavno koristiti postojeće licence. Možete i jednostavno skaliranje prema gore/dolje i Zaustavi/nastavi VM prema potrebi.

Općenito govoreći, ove dvije mogućnosti SQL su optimizirani za različite potrebe:

- Da biste smanjili ukupni trošak najmanje za dodjelu resursa i upravljanje mnoge baze podataka optimizirana je **SQL baze podataka** . Smanjuje troškove tijeku Administracija jer nemate da biste upravljali virtualnim strojevima, operacijski sustav ili softver za bazu podataka. Ne morate upravljati nadogradnje, visoke dostupnosti ili [sigurnosne kopije](sql-database-automated-backups.md). Općenito govoreći, baze podataka SQL Azure možete znatno povećati broj baza podataka upravlja jedan IT ili razvoj resursa.
- **SQL Server sustavom Azure VMs** optimiziran je za migriranje postojeće aplikacije za Azure ili proširivanje postojeće lokalnog aplikacije u oblak u hibridnim implementacijama. Osim toga, možete koristiti SQL Server u virtualnog računala za razvoj i testiranje tradicionalni aplikacija sustava SQL Server. Sa sustavom SQL Server Azure VMs, imate potpuni administratorska prava nad namjenski instancu sustava SQL Server i VM u oblaku. Kada je tvrtka ili ustanova već ima IT resurse koji su dostupni za održavanje virtualnim strojevima je savršen odabir. Ove mogućnosti omogućuju stvaranje visoko prilagođenih sustava da biste riješili određene performanse i preduvjeti dostupnost vaša aplikacija.

U sljedećoj su tablici navedene osobina glavnom bazom podataka SQL i SQL Server na Azure VMs:

|       | SQL baze podataka | SQL Server na Azure virtualnog računala|
| -------------- | ------------ | ---------------------- |
| **Najbolje za:** | Novi oblaka namijenjene aplikacije s ograničenjima vrijeme u razvoju i marketing. |Postojeće aplikacije koje je potrebno brzo migracije u oblak minimalnog promjenama. Brz test i razvoj scenarija kada ne želite kupnji lokalnog koje nisu radnog SQL Server hardvera. |
|| Ekipa koje je potrebno ugrađene visoke dostupnosti, oporavak Izrada i nadogradnje za bazu podataka. |Ekipa koje se može konfigurirati i upravljati visoke dostupnosti, oporavak Izrada i zakrpa za SQL Server. Neke navedeni su značajke automatskog izrazito Pojednostavnite to. |
||Ekipa koje želite upravljati temeljnog operacijskog sustava i konfiguracijske postavke.| Ako vam je potrebna prilagođene okruženju s puno administratorska prava.|
||Baza podataka do 1 TB ili veće baze podataka koje može biti [okomito ili vodoravno particije](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) pomoću uzorak skaliranje izlaz.|SQL Server instancama s do 64 TB prostora za pohranu. Instancu podržavaju proizvoljan broj baza podataka po potrebi. |
||[Sastavni softver-kao-na-(SaaS) aplikacije servisa](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migracija i stvaranje enterprise i hibridnog aplikacija.|
|||||
|**Resursi:**|Ne želite uključivanja IT resurse za konfiguriranje i upravljanje podlozi infrastrukture, ali želite usredotočite se na aplikacijskom sloju.|Imate nekoliko IT resursa za konfiguraciju i upravljanje njima. Neke navedeni su značajke automatskog izrazito Pojednostavnite to.|
|**Ukupni trošak vlasništva:**|Uklanja troškove hardver i smanjuje administrativne troškove.|Uklanja troškove hardvera.|
|**Neprekidno poslovanje:**|Osim ugrađene kvara odstupanje infrastrukture mogućnostima, baze podataka SQL Azure nudi značajke, kao što su [automatskog sigurnosnog kopiranja](sql-database-automated-backups.md), [Vratite točke u vrijeme](sql-database-recovery-using-backups.md#point-in-time-restore), [Zemlj vraćanja](sql-database-recovery-using-backups.md#geo-restore)i [Aktivni replikacije zemlj](sql-database-geo-replication-overview.md) da biste povećali poslovanje. Dodatne informacije potražite u članku [Pregled continuity za SQL baze podataka tvrtke](sql-database-business-continuity.md).|SQL Server na Azure VMs omogućuje postavljanje visoke dostupnosti i Izrada oporavak rješenje specifičnim potrebama svoje baze podataka. Dakle, imate sustav iznimno optimiziranog za svoju aplikaciju. Možete testirati i pokrenuti failovers sami po potrebi. Dodatne informacije potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Oblak hibridnog:**|Lokalni aplikacija može pristupiti podacima u bazi podataka SQL Azure.|Sa sustavom SQL Server Azure VMs, imate aplikacije koje pokreću se djelomično u oblak i djelomično lokalnog. Ako, na primjer, možete proširiti lokalne mreže i Active Directory domene s oblakom putem [Virtualne mreže Azure](../virtual-network/virtual-networks-overview.md). Osim toga, možete spremiti lokalne podatkovne datoteke u spremište Azure pomoću [SQL Server podatkovne datoteke u Azure](http://msdn.microsoft.com/library/dn385720.aspx). Dodatne informacije potražite u članku [Uvod u SQL Server 2014 hibridnog oblaka](http://msdn.microsoft.com/library/dn606154.aspx).|
||Podržava [transakcijskih replikacije SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) kao pretplatnika za replikaciju podataka.|Potpuno podržava [transakcijskih replikacije sustava SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [Grupe dostupnosti AlwaysOn](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), servisima za integraciju i zapisnika dostavu za replikaciju podataka. Osim toga, tradicionalni sigurnosne kopije sustava SQL Server u potpunosti su podržani|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Motivations tvrtke za odabir baze podataka SQL Azure ili SQL Server na Azure VMs

### <a name="cost"></a>Trošak

Hoće li se prilikom pokretanja koji je strapped novca, ili tim u utvrđene tvrtki koja radi u odjeljku ograničenja čvrsto proračun, ograničeni funding često je upravljački program za primarni pri odabiru kako hostira baze podataka. U ovom se odjeljku informacije o naplata i licenciranje osnove programa Azure koja se odnosi na sljedeće dvije mogućnosti relacijske baze podataka: SQL baze podataka i SQL Server na Azure VMs. Koje i dodatne informacije o izračunavanju aplikacije ukupni trošak.

#### <a name="billing-and-licensing-basics"></a>Naplata i licenciranje osnove

**Baze podataka SQL** prodaje klijentima kao servis, a ne uz licencu.  [SQL Server na Azure VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) prodaje s uključene licencu za plaćate-minute. Ako imate licencu za postojeće, vam može poslužiti ga.  

Trenutno **Baze podataka SQL** dostupan je u nekoliko razine servisa koji su zaračunava naplatiti na fiksnom stopom na temelju sloju servisa i odaberete razina performansi. Osim toga, se naplatiti za odlazni promet Internet na obični [tečajeve za prijenos podataka](https://azure.microsoft.com/pricing/details/data-transfers/). Servis razine Basic, standardna i Premium dizajnirani izlaganje predvidljivi performanse s više razina performanse u skladu potrebama Vršna vaše aplikacije. Možete promijeniti između razine usluge i razine performanse u skladu s potrebama vaše aplikacije brojeva propusnost. Ako baza podataka sadrži velik transakcijskih i potrebe za podršku mnogo Istodobni korisnika, preporučujemo Premium sloja u sustavu. Najnovije informacije na trenutne razine podržanom servisu potražite u članku [Azure SQL baze podataka usluge razine](sql-database-service-tiers.md). Možete i stvoriti [grupe elastic baze podataka](sql-database-elastic-pool.md) za omogućivanje zajedničkog korištenja resursa performanse među instanci baze podataka.

S **Bazom podataka SQL**softver za bazu podataka automatski konfigurirana te patched i nadograditi Microsoft koji smanjuje troškove Administracija. Uz to, njegovim mogućnostima [ugrađene sigurnosne kopije](sql-database-automated-backups.md) pomoći postigli značajan trošak štednju, osobito kada imate velik broj baza podataka.

**SQL Server na Azure VMs**, možete koristiti bilo koju sliku SQL Server pod uvjetom platformu (koji sadrži licence) ili Premjesti licence sustava SQL Server. Sve podržane verzije sustava SQL Server (2008R2, 2012, 2014, 2016) i dostupne su izdanja (za razvojne inženjere, Express, Web, Standardno, Enterprise). Osim toga, Premjesti-vaše – vlasnik – licence verzije (BYOL) slike dostupne su. Kada pomoću na Azure dani slike, radu trošak ovisi o veličina VM i izdanja sustava SQL Server koje odaberete. Bez obzira na to veličina VM ili SQL Server edition, plaćanje-minutni licenciranja trošak SQL Server i Windows Server uz trošak Azure prostora za pohranu za VM diskova. Mogućnost naplate-minutni ne omogućuje korištenje SQL Server za sve dok se morate bez kupnje Zbrajanje SQL Server licence. Ako stavi licence sustava SQL Server Azure, vam se naplatiti za Windows Server i troškovi spremanja. Dodatne informacije o Premjesti-vaše-vlasnik licenciranje, potražite u članku [Mobilnost licenci putem Tehnološko jamstvo na Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Izračunavanje aplikacije ukupni trošak

Kada pokrenete pomoću platforma oblaka, troškovi pokretanja aplikacije sadrži troškove razvoj i administracije uz troškove servisa platforme javno oblaka.

Evo izračun detaljne troškova za svoju aplikaciju radi u SQL baze podataka i SQL Server na Azure VMs:

**Kada koristite baze podataka SQL Azure:**

*Ukupni trošak aplikacije = iznimno minimiziranom Administracija troškove + software development troškove + troškove servisa SQL baze podataka*

**Pri korištenju sustava SQL Server na Azure VMs:**

*Ukupni trošak aplikacije = iznimno minimiziranom softver razvoj trošak + Administracija troškove + SQL Server i Windows Server licencnim troškove + troškovi Azure spremanja*

Dodatne informacije o cijenama, potražite u sljedećim resursima:

- [Cijene za SQL baze podataka](https://azure.microsoft.com/pricing/details/sql-database/)
- [Cijene virtualnog računala](https://azure.microsoft.com/pricing/details/virtual-machines/) [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) i za [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure cijene kalkulator](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Postoji mali podskup značajki na SQL Server koje nisu primjenjivih ili nije dostupno sa SQL baze podataka. Dodatne informacije potražite u [Općenito baze podataka SQL ograničenja i smjernice](sql-database-general-limitations.md) i [Transact-SQL baze podataka SQL informacije](sql-database-transact-sql-information.md) . Ako premještate postojeće rješenje sustava SQL Server u oblak, potražite u članku [Migracija baze podataka SQL Server s bazom podataka SQL Azure](sql-database-cloud-migrate.md). Kada migrirati postojeću aplikaciju lokalnog sustava SQL Server s bazom podataka SQL, razmislite o ažuriranju računala da biste iskoristili mogućnosti koje ponude za servise u oblaku. Na primjer, razmotrite pomoću [Azure web-aplikacije servisa](https://azure.microsoft.com/services/app-service/web/) ili [Azure servise u Oblaku](https://azure.microsoft.com/services/cloud-services/) za hostiranje aplikacijskom sloju da biste povećali pogodnosti trošak.

### <a name="administration"></a>Administracija

Za mnoge se tvrtke odluka prijelaz na servis u oblaku je mnogo o rasteretite složenost Administracija jer je trošak. Microsoft se upravlja njima podlozi hardver s **Bazom podataka SQL**. Microsoft automatski replicira sve podatke možete unijeti visoke dostupnosti, konfigurira nadograđuje softver za bazu podataka, upravlja opterećenja i ne prozirne prebacivanje ako postoji pogreška poslužitelja. Možete nastaviti upravljati bazu podataka, ali vam više nije potrebna za upravljanje modul baze podataka, poslužitelj operacijski sustav i hardvera.  Stavke možete nastaviti upravljati Primjeri baze podataka i prijave, indeks i ugađanje upita, i nadzor i sigurnost.

**SQL Server na Azure VMs**imaju potpunu kontrolu nad operacijski sustav i konfiguracija instancu sustava SQL Server. S VM, je prema gore da biste odlučili kada Ažuriraj/nadogradnje operacijskog sustava i softver za bazu podataka, a kada instalirati neki dodatni softver kao što je antivirusni. Neke značajke automatskog dostupni su izrazito Pojednostavnite kojemu, sigurnosno kopiranje i visoka dostupnost. Osim toga, možete kontrolirati veličinu na VM, broj diskova i njihova konfiguracija prostora za pohranu. Azure omogućuje promjenu veličine na VM prema potrebi. Informacije potražite u članku [virtualnog računala i veličine servisa oblaka za Azure](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Ugovor o razini usluge (SLA)

Više odjela IT sastanak obaveze gore vrijeme od na servis razinu ugovor (SLA) je gornja prioritet. U ovom ćete odjeljku smo pogledajte što SLA primjenjuje na svaku bazu podataka u kojem se nalazi mogućnost.

Za **Baze podataka SQL** Basic, standardna i Premium razine servisa Microsoft pruža dostupnost SLA 99,99%. Najnovije informacije potražite u članku [O usluzi razinu](https://azure.microsoft.com/support/legal/sla/sql-database/). Najnovije informacije na razine servisa SQL baze podataka i podržanim poslovne planove continuity potražite u članku [Servis razine](sql-database-service-tiers.md).

Za **SQL Server sustavom Azure VMs**, Microsoft pruža dostupnost SLA % 99.95 koji prekriva samo virtualnog računala. Ovaj SLA pokrivaju procesa (primjerice, SQL Server) sustavom u VM i zahtijeva glavno računalo barem dvije instance VM u skupu dostupnost. Najnovije informacije potražite u članku [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Za bazu podataka visoke dostupnosti (HA) unutar VMs, trebali biste konfigurirati jednu od opcija podržani visoke dostupnosti u sustavu SQL Server, kao što su [Grupe dostupnosti AlwaysOn](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Pomoću podržanih visoke dostupnosti mogućnosti ne nude dodatne SLA, ali omogućuje vam da biste postigli > dostupnost 99,99% baze podataka.

### <a name="market"></a>Vrijeme na tržištu

**Baze podataka SQL** je pravo rješenje za aplikacije oblak namijenjene kad su ključnih produktivnost za razvojne inženjere i brzo vrijeme-na-tržištu. Funkcionalnošću programski nalik DBA, nije savršen za web-mjesto oblaka projektantima i u okvir za razvojne inženjere kao što je Spušta potrebe za upravljanje Temeljni operacijski sustav i baze podataka. Ako, na primjer, možete koristiti [REST API -JA](http://msdn.microsoft.com/library/azure/dn505719.aspx) i [Cmdleta ljuske PowerShell](http://msdn.microsoft.com/library/azure/dn546726.aspx) za automatiziranje i upravljanje administratora operacija za tisuće baze podataka. Značajke kao što su [Elastic baze podataka grupe](sql-database-elastic-pool.md) omogućuju fokusiranje na aplikacijskom sloju i brže isporučiti rješenje na tržištu.

**SQL Server sustavom Azure VMs** je savršen ako aplikacija postojeći ili novi zahtijeva velikih baza podataka, povezanih baze podataka ili pristup svim značajkama web-mjesto sustava SQL Server ili u okvir za Windows. Preporučuje se i dobro rješenje kada želite migrirati postojećeg lokalnog aplikacija i baza podataka za Azure kao-je. Budući da ne morate promijeniti prezentacije, aplikacije i slojeve podataka, spremite vrijeme i Proračun na rearchitecting postojeće rješenje. Umjesto toga, možete se fokusirati na Migracija svih rješenja Azure i u način optimizacije neke performanse koje možda će biti potrebno platforma Azure. Dodatne informacije potražite u članku [Performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Sažetak

U ovom se članku istražili SQL baze podataka i SQL Server na virtualnim računalima sustava Azure (VMs) i spominju uobičajene poslovne motivators koji mogu utjecati na vaša odluka. Ovo je sažetak prijedloge za koje treba uzeti u obzir:

Odaberite **baze podataka Azure SQL** ako:

- Su stvaranje nove aplikacije utemeljene na oblaku da biste iskoristili ušteda troškova i navedite optimizaciju performansi koje servise u oblaku. Taj se način nudi prednosti potpuno upravljanih oblaku, pomaže u donjem početne vrijeme-na-tržištu i pružaju Dugoročne optimizaciju trošak.

- Želite imati Microsoft provode uobičajeni postupci upravljanja na baze podataka i zahtijevaju jači dostupnost SLA za baze podataka.



Odaberite **SQL Server na Azure VMs** :

- Imate postojeće lokalnog aplikacije koju želite migrirati ili proširivanje u oblaku, ili ako želite izraditi enterprise aplikacije veće od 1 TB. Taj se način omogućuje prednost 100% SQL kompatibilnosti, kapacitet velike baze podataka, a zatim potpunu kontrolu nad SQL Server i Windows i sigurne tuneliranje na lokalni. Ovaj pristup minimizira troškove za razvoj i promjene postojećih aplikacija.

- Imate postojećih IT resursa i konačni mogu biti vlasnici zakrpa, kopija i visoke dostupnosti baze podataka. Imajte na umu da neke značajke automatskog znatno Pojednostavnite te operacije. 


## <a name="next-steps"></a>Daljnji koraci
- Potražite u članku [baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md) za početak rada s bazom podataka SQL.
- U odjeljku [baze podataka SQL cijena] (https://azure.microsoft.com/pricing/details/sql-database/).
- Potražite u članku [Dodjeljivanje virtualnog računala sustava SQL Server u Azure](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) za početak rada sa sustavom SQL Server na Azure VMs.
- U odjeljku [SQL Server na Azure virtualnog računala: tečaj](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
