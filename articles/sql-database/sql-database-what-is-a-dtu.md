<properties
    pageTitle="SQL baza podataka: Što je na DTU? | Microsoft Azure"
    description="Objašnjenje koje Azure SQL baze podataka je transakcija jedinica."
    keywords="mogućnosti za baze podataka, performanse baze podataka"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Jedinice transakcije baze podataka (DTUs) te elastic baze podataka transakcije jedinice (eDTUs)

U ovom se članku objašnjava jedinice transakcije baze podataka (DTUs) i elastic baze podataka transakcije jedinice (eDTUs) i što se događa kada kliknete maksimalne DTUs ili eDTUs.  

## <a name="what-are-database-transaction-units-dtus"></a>Što su jedinice transakcije baze podataka (DTUs)

U DTU je mjernu jedinicu resursa koji su zajamčiti da bi bio dostupan s bazom podataka Azure SQL samostalne performanse određene razine unutar [samostalne baze podataka usluge sloju](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels). U DTU je stopljena mjera procesora i memorije podataka/i i/i zapisnik transakcija u omjer određen programa OLTP usporednih radno opterećenje dizajnirani tako da su uobičajeni od radnih opterećenja OLTP stvarnog života. Udvostručavanja u DTUs povećavanjem razinom performansi baze podataka daje rezultat u obliku udvostručavanja skup resursa za tu bazu podataka. Ako, na primjer, Premium P11 za rad sa baze podataka pomoću 1750 DTUs nudi 350 x više DTU izračunati power od osnovni baze podataka pomoću 5 DTUs. Da biste shvatili methodology iza OLTP radno opterećenje usporednih upotrijebljena za određivanje DTU blend, potražite u članku [Pregled usporednih SQL baze podataka](sql-database-benchmark-overview.md).

![Uvod u SQL baze podataka: jedna baza podataka DTUs sloju i razina](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Možete [promijeniti razine servisa](sql-database-scale-up.md) s minimalnim isključiti u bilo kojem trenutku u aplikaciji (obično izračunavanje prosjeka u odjeljku četiri sekunde). Za mnoge tvrtke i aplikacije mogućnost stvaranja baze podataka i birati performanse jedne baze podataka prema gore ili dolje na zahtjev je dovoljno, osobito ako uzoraka korištenja relativno predvidljivi. Ali ako imate nepredvidljive upotrebe, je bio teško upravljanje troškove i modelu tvrtke. U ovom scenariju pomoću elastic skup s određenim brojem eDTUs.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Što su elastic baze podataka transakcije jedinice (eDTUs)

U eDTU je mjernu jedinicu skup resursa (DTUs) koji se mogu zajednički koristiti skup baze podataka na poslužitelju komponente Azure SQL - pod nazivom [elastic skup](sql-database-elastic-pool.png). Elastic grupe omogućuje jednostavno troškova učinkovitih rješenje za upravljanje ciljeve za više bazu podataka koja je široko promjenjivih i nepredvidljive upotrebe. U odjeljku [elastic grupe i razine servisa](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) da biste saznali više.

![Uvod u SQL baze podataka: eDTUs sloju i razina](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Zajedničko područje izražen je broj eDTUs za postavljanje cijena. Unutar grupe, pojedinačne baze podataka imaju fleksibilnost za automatsko skaliranje unutar postavljati parametre. U odjeljku velikim opterećenjem baze podataka može raditi više eDTUs da bi odgovarao zahtjev. Baza podataka u odjeljku osnovna opterećenje zauzeti manji, a baza podataka ne opterećenju zauzeti bez eDTUs. Dodjeljivanje resursa za cijelu grupu, a ne za jednu baze podataka pojednostavljuje zadataka upravljanja. Te su predvidljivi proračuna na resurse.

Dodatni eDTUs mogu dodati postojeće grupe aplikacija ne nedostupnost baze podataka ili bez utjecaja na baze podataka u elastic. Isto tako, ako je više nisu potrebni dodatni eDTUs ih je moguće ukloniti iz postojeće grupe aplikacija bilo kojem trenutku u vremenu. Možete dodati ili oduzimanje baze podataka na resurse. Ako bazu podataka je predvidljivije u odjeljku-korištenja resursa, premjestite je.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Kako odrediti broj DTUs potrebne Moje radno opterećenje?

Ako vas zanimaju će se migrirati postojećeg lokalnog ili SQL Server virtualnog računala radno opterećenje s bazom podataka SQL Azure, [DTU Kalkulator](http://dtucalculator.azurewebsites.net/) možete koristiti za određivanje broja DTUs potrebne. Za postojeće radno opterećenje baze podataka SQL Azure, možete koristiti [SQL baza podataka upita performanse uvid](sql-database-query-performance.md) da biste shvatili potrošnje resursa baze podataka (DTUs) da biste dobili detaljnije uvid u kako optimizirati svoje radno opterećenje. [Sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV možete koristiti i da biste dobili informacije o resursu potrošnje na zadnji jedan sat. Osim toga, prikaz kataloga [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) možete također može poslati upit da biste dobili iste podatke za u zadnjih 14 dana iako i na donjem vjernost od pet minuta prosjeka.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Kako znati je li nije prednosti elastic skup resursa?

Za veliki broj baza podataka uzorcima određene Upotreba prilagođene grupe. Za dani baze podataka se ovaj uzorak određene argumentima niskog average Upotreba s relativno rijetko Upotreba krivina. Baze podataka SQL automatski procjenjuje povijesne resursa baze podataka u postojeće poslužitelj baze podataka SQL i preporučuje odgovarajući skup konfiguracije na portalu za Azure. Dodatne informacije potražite u članku [kada je skup elastic baze podataka želite koristiti?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Što se događa kada se pogoditi iz prvog Moje Maksimalna DTUs

Performanse razine kalibrirani i mjerodavni omogućuju potrebne resursa za izvođenje baze podataka radno opterećenje do ograničenja maksimalni dopušteni sloju/performanse razinu zaštite odabrani servis. Ako svoje radno opterećenje je odlazak ograničenja na jedan od ograničenja IO IO/zapisnika CPU-podataka, nastavite primati resurse na najveći dopušteni razini, ali ćete vjerojatno da biste vidjeli povećanu latencies za upite. Ta ograničenja rezultat na sve pogreške, ali umjesto usporenje u radno opterećenje, osim ako se na usporenje postaje pa loši da upiti započeti tempiranje. Ako su odlazak ograničenja maksimalni dopušteni Istodobni korisnički sesije/zahtjeva za (radnih niti), vidjet ćete eksplicitnih pogreške. Potražite u članku [ograničenja resursa baze podataka SQL Azure](sql-database-resource-limits.md) informacije o ograničenja resursa koji nije procesora, memorije, podataka/i i/i zapisnik transakcija.

## <a name="next-steps"></a>Daljnji koraci

- Informacije o DTUs i eDTUs koje su dostupne za samostalne baze podataka i elastic grupe potražite u članku [servis sloju](sql-database-service-tiers.md) .
- Potražite u članku [ograničenja resursa baze podataka SQL Azure](sql-database-resource-limits.md) informacije o ograničenja resursa koji nije procesora, memorije, podataka/i i/i zapisnik transakcija.
- U odjeljku [SQL baza podataka upita performanse uvid](sql-database-query-performance.md) da biste shvatili vaše potrošnje (DTUs).
- Potražite u članku [Pregled usporednih SQL baze podataka](sql-database-benchmark-overview.md) da biste shvatili methodology iza OLTP radno opterećenje usporednih upotrijebljena za određivanje DTU blend.