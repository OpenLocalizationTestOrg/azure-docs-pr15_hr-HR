<properties
    pageTitle="Svih tema vezanih uz servisa SQL Data Warehouse | Microsoft Azure"
    description="Popis svih tema vezanih uz servisa za Azure pod nazivom SQL Data Warehouse koji postoje na http://azure.microsoft.com/documentation/articles/, naslova i opisa."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Svih tema vezanih uz servisa Azure SQL Data Warehouse

Ova tema sadrži popis svaki temi koja se odnosi izravno na servis za **Skladištu podataka SQL** Azure. Ova web-stranica za ključne riječi možete pretraživati pomoću **Ctrl + F**, da biste pronašli teme trenutni kamata.




## <a name="new"></a>Novi

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 1 | [SQL Data Warehouse sigurnosne kopije](sql-data-warehouse-backups.md) | Saznajte više o sigurnosne kopije ugrađene baze podataka SQL Data Warehouse koji omogućuju vam da biste vratili programa Azure SQL Data Warehouse točku vraćanja ili drugu zemljopisnom području. |


## <a name="updated-articles-sql-data-warehouse"></a>Ažurirani članci, SQL Data Warehouse

U ovom se odjeljku navedeni članci koje su ažurirane nedavno, gdje je ažuriranje veliku ili značajan. Za svaki ažurirani članak gruba isječak teksta dodane markdown prikazat će se. Članci su ažurirani unutar datumskog raspona **2016, 08 i 22** **2016, 10 i 05**.

| &nbsp; | Članak | Ažurirani tekst, isječka | Kada ažurirati |
| --: | :-- | :-- | :-- |
| 2 | [Podatke iz spremišta blobova platforme Azure učitali u SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | / – Da biste pratili bajtova i datoteke odaberite r.command, s.request_id, r.status, zbroj (distinct input_name) kao nbr_files, sum (s.bytes_processed) / 1024/1024 kao gb_processed s sys.dm_pdw_exec_requests r unutarnji spoj sys.dm_pdw_dms_external_work s na r.request_id = s.request_id r gdje. natpis = ' CTAS: cso Učitaj. Dimenzije proizvoda "r ili. natpis = ' CTAS: cso Učitaj. FactOnlineSales' GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files desc, gb_processed desc;  | 2016-09-07 |
| 3 | [Vraćanje SQL Data Warehouse](sql-data-warehouse-restore-database-overview.md) | **Mogu li vratiti pauzirano podataka skladištu?** Da biste vratili skladištu podataka koji je pauziran, morate najprije Premjesti nazad putem Interneta. Nakon što vratite u mrežni skladištu podataka, imate sedam dana točke vraćanja na raspolaganju. **Vraćanje na područje suvišnih zemlj.** Ako koristite zemlj suvišnih prostora za pohranu, skladištu podataka možete vratiti u Centar za upareni podataka u nekoj drugoj regiji geografske. Vratit će se skladištu podataka iz zadnjeg dnevna sigurnosna kopija. **Vraćanje vremenske trake** Vraćanje baze podataka vrati postavite u zadnjih sedam dana. Brze snimke pokrenite svakih četiri do osam sati i dostupne su za sedam dana. Kada je snimka starije od sedam dana, što istekne, a njezine točke vraćanja više nije dostupan. **Vraćanje troškovi** Iznos za pohranu za skladištu vraćene podatke je naplaćeno brzinom Azure Premium prostora za pohranu. Ako zaustavite vraćene podatke skladištu vam se naplatiti za pohranu brzinom Azure Premium prostora za pohranu. Prednost pauze je niste trošak | 2016 09 29 |





## <a name="get-started"></a>Početak rada

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 4 | [Provjera autentičnosti za Azure SQL Data Warehouse](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) i SQL Server provjere autentičnosti za Azure SQL Data Warehouse. |
| 5 | [Najbolje prakse za Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) | Preporuke i najbolje prakse kao što je potrebno znati razvijati rješenja za Azure SQL Data Warehouse. Te će vam biti uspješno. |
| 6 | [Upravljačke programe za Azure SQL Data Warehouse](sql-data-warehouse-connection-strings.md) | Nizove veze i upravljačke programe za SQL Data Warehouse |
| 7 | [Povezivanje s Data Warehouse Azure SQL](sql-data-warehouse-connect-overview.md) | Kako pronaći poslužitelja naziva i veze niz za vaše za Azure SQL Data Warehouse |
| 8 | [Analiza podataka pomoću Azure strojnog učenja](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Da biste sastavili predvidljivu strojnog učenja modela na temelju podataka pohranjenih u skladištu podataka za SQL Azure pomoću Azure strojnog učenja. |
| 9 | [Upit Azure SQL Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Slanje upita Azure SQL Data Warehouse s sqlcmd Utility naredbenog retka. |
| 10 | [Stvaranje baze podataka SQL Data Warehouse pomoću Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Saznajte kako stvoriti programa Data Warehouse SQL Azure s TSQL |
| 11 | [Upute za stvaranje zahtjev za podršku možete za SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) | Kako stvoriti zahtjev za podršku možete u Azure SQL Data Warehouse. |
| 12 | [Učitavanje podataka s tvorničke Azure podataka](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Saznajte kako učitavanje podataka s tvorničke Azure podataka |
| 13 | [Učitavanje podataka s PolyBase u skladištu podataka za SQL](sql-data-warehouse-get-started-load-with-polybase.md) | Saznajte što je PolyBase i kako je koristiti za podatke skladištenje scenarijima. |
| 14 | [Stvaranje programa Data Warehouse Azure SQL](sql-data-warehouse-get-started-provision.md) | Saznajte kako stvoriti programa Azure SQL Data Warehouse na portalu za Azure |
| 15 | [Stvaranje SQL Data Warehouse pomoću komponente PowerShell](sql-data-warehouse-get-started-provision-powershell.md) | Stvaranje SQL Data Warehouse pomoću komponente PowerShell |
| 16 | [Vizualni prikaz podataka pomoću dodatka Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Vizualni prikaz podataka za SQL Data Warehouse pomoću dodatka Power BI |
| 17 | [Upit Azure SQL Data Warehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Upit SQL Data Warehouse s Visual Studio. |



## <a name="develop"></a>Razvoj

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 18 | [Optimiziranje transakcije za SQL Data Warehouse](sql-data-warehouse-develop-best-practices-transactions.md) | Najbolja praksa smjernice o pisanje učinkovitog transakcije ažuriranja u Azure SQL Data Warehouse |
| 19 | [Upravljanje istodobnosti i radno opterećenje u SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) | Objašnjenje istodobnosti i radno opterećenje upravljanje u Azure SQL Data Warehouse za razvoj rješenja. |
| 20 | [Stvaranje tablice kao potvrdite (CTAS) u SQL Data Warehouse](sql-data-warehouse-develop-ctas.md) | Savjeti za kodiranje s tablicom stvaranje kao iskaza select (CTAS) u skladištu podataka za SQL Azure za razvoj rješenja. |
| 21 | [Dinamični SQL u skladištu podataka za SQL](sql-data-warehouse-develop-dynamic-sql.md) | Savjeti za korištenje dinamički SQL programa Azure SQL Data Warehouse za razvoj rješenja. |
| 22 | [Grupiranje prema mogućnosti u SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) | Savjeti za implementaciju Grupiranje po mogućnostima u skladištu podataka za SQL Azure za razvoj rješenja. |
| 23 | [Koristi natpise u instrument upite u skladištu podataka za SQL](sql-data-warehouse-develop-label.md) | Savjeti za korištenje oznake instrument upitima u skladištu podataka za SQL Azure za razvoj rješenja. |
| 24 | [Petlje u SQL Data Warehouse](sql-data-warehouse-develop-loops.md) | Savjeti za Transact-SQL petlje i zamjene pokazivači u skladištu podataka za SQL Azure za razvoj rješenja. |
| 25 | [Pohranjene procedure u skladištu podataka za SQL](sql-data-warehouse-develop-stored-procedures.md) | Savjeti za implementaciju pohranjene procedure u skladištu podataka za SQL Azure za razvoj rješenja. |
| 26 | [Transakcije u skladištu podataka za SQL](sql-data-warehouse-develop-transactions.md) | Savjeti za implementaciju transakcije u skladištu podataka za SQL Azure za razvoj rješenja. |
| 27 | [Korisnički definirane sheme u SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) | Savjeti za korištenje Transact-SQL sheme programa Azure SQL Data Warehouse za razvoj rješenja. |
| 28 | [Dodjeljivanje varijable u SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) | Savjeti za dodjeljivanje Transact-SQL Varijable u Azure SQL Data Warehouse za razvoj rješenja. |
| 29 | [Prikazi u skladištu podataka za SQL](sql-data-warehouse-develop-views.md) | Savjeti za korištenje Transact-SQL prikaza programa Azure SQL Data Warehouse za razvoj rješenja. |
| 30 | [Dizajniranje odluke i odbijanje tehnike za SQL Data Warehouse](sql-data-warehouse-overview-develop.md) | Razvoj koncepti, odluke dizajna, preporuke i odbijanje postupaka za SQL Data Warehouse. |



## <a name="manage"></a>Upravljanje

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 31 | [Upravljanje računalnim power u skladištu podataka SQL Azure (pregled)](sql-data-warehouse-manage-compute-overview.md) | Performanse skaliranje više mogućnosti u skladištu podataka za SQL Azure. Skaliranje prilagodbom DWUs ili zaustaviti i nastaviti računalnim resursi troškova. |
| 32 | [Upravljanje računalnim power u skladištu podataka SQL Azure (portal za Azure)](sql-data-warehouse-manage-compute-portal.md) | Azure zadaci portala za upravljanje izračunati power. Promjena veličine izračunati resursi prilagodbom DWUs. Ili, zadržite pokazivač i nastaviti računalnim resursi troškova. |
| 33 | [Upravljanje računalnim power u skladištu podataka SQL Azure (PowerShell)](sql-data-warehouse-manage-compute-powershell.md) | Zadaci ljuske PowerShell za upravljanje izračunati power. Promjena veličine izračunati resursi prilagodbom DWUs. Ili, zadržite pokazivač i nastaviti računalnim resursi troškova. |
| 34 | [Upravljanje računalnim power u Azure SQL podataka skladištu (REST)](sql-data-warehouse-manage-compute-rest-api.md) | Zadaci ljuske PowerShell za upravljanje izračunati power. Promjena veličine izračunati resursi prilagodbom DWUs. Ili, zadržite pokazivač i nastaviti računalnim resursi troškova. |
| 35 | [Upravljanje računalnim power u skladištu podataka SQL Azure (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Zadatke SQL transakcija (T-SQL) skaliranje iz performanse prilagodbom DWUs. Spremite troškove skaliranje natrag tijekom vremena koje nisu Vršna. |
| 36 | [Praćenje svoje radno opterećenje pomoću DMVs](sql-data-warehouse-manage-monitor.md) | Saznajte kako praćenje svoje radno opterećenje pomoću DMVs. |
| 37 | [Upravljanje bazama podataka u skladištu podataka za SQL Azure](sql-data-warehouse-overview-manage.md) | Pregled upravljanje bazama podataka za SQL Data Warehouse. Obuhvaća alate za upravljanje, DWUs i skaliranje Izlaz, otklanjanje poteškoća s performansama performanse upita, uspostavljanje dobar sigurnosne pravilnike i vraćanje baze podataka iz oštećenja podataka ili regionalne prekida. |
| 38 | [Praćenje korisnika upita u skladištu podataka za SQL Azure](sql-data-warehouse-overview-manage-user-queries.md) | Pregled pitanja vezana uz, najbolje prakse i zadatke za nadzor korisnika upita u skladištu podataka za SQL Azure |
| 39 | [Vraćanje SQL Data Warehouse](sql-data-warehouse-restore-database-overview.md) | Pregled mogućnosti Vraćanje baze podataka za oporavak baze podataka u skladištu podataka za SQL Azure. |
| 40 | [Vraćanje Azure SQL Data Warehouse (Portal)](sql-data-warehouse-restore-database-portal.md) | Azure portala zadataka za vraćanje programa Data Warehouse SQL Azure. |
| 41 | [Vraćanje Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-restore-database-powershell.md) | Zadaci ljuske PowerShell za vraćanje programa Data Warehouse SQL Azure. |
| 42 | [Vraćanje Azure SQL Data Warehouse (REST API-JA)](sql-data-warehouse-restore-database-rest-api.md) | Zadaci REST API-JA za vraćanje programa Data Warehouse SQL Azure. |



## <a name="tables-and-indexes"></a>Tablica i indeksa

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 43 | [Vrste podataka u tablicama u SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) | Uvod u vrste podataka za Azure SQL Data Warehouse tablice. |
| 44 | [Distribucija tablica u SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) | Početak rada s distribucija tablice u skladištu podataka za SQL Azure. |
| 45 | [Indeksiranje tablica u SQL Data Warehouse](sql-data-warehouse-tables-index.md) | Početak rada s tablicom indeksiranje u skladištu podataka za SQL Azure. |
| 46 | [Pregled tablica u SQL Data Warehouse](sql-data-warehouse-tables-overview.md) | Uvod u skladištu tablice s podacima sustava SQL Azure. |
| 47 | [Stvaranje particija tablica u SQL Data Warehouse](sql-data-warehouse-tables-partition.md) | Uvod u tablice particija u skladištu podataka za SQL Azure. |
| 48 | [Upravljanje statistike o tablica u SQL Data Warehouse](sql-data-warehouse-tables-statistics.md) | Početak rada s Statistika tablicama u skladištu podataka za SQL Azure. |
| 49 | [Privremena tablica u SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) | Početak rada s privremenih tablica u skladištu podataka za SQL Azure. |



## <a name="integrate"></a>Integracija

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 50 | [Korištenje tvorničke Azure podataka sa SQL Data Warehouse](sql-data-warehouse-integrate-azure-data-factory.md) | Savjeti za korištenje tvorničke Azure podataka (ADF) pomoću Azure SQL Data Warehouse za razvoj rješenja. |
| 51 | [Azure strojnog učenja pomoću SQL Data Warehouse](sql-data-warehouse-integrate-azure-machine-learning.md) | Praktični vodič za korištenje Azure strojnog učenja za Azure SQL Data Warehouse za razvoj rješenja. |
| 52 | [Azure strujanje analize pomoću SQL Data Warehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) | Savjeti za korištenje Azure strujanje analize pomoću Azure SQL Data Warehouse za razvoj rješenja. |
| 53 | [Korištenje servisa Power BI s SQL Data Warehouse](sql-data-warehouse-integrate-power-bi.md) | Savjeti za korištenje servisa Power BI pomoću Azure SQL Data Warehouse za razvoj rješenja. |
| 54 | [Korištenje drugim servisima SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) | Alati i partnerima s rješenjima integriranih s SQL Data Warehouse.  |



## <a name="load"></a>Učitavanje

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 55 | [Podatke iz spremišta blobova platforme Azure učitali u Azure SQL Data Warehouse (Azure podataka tvorničke)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Saznajte kako učitavanje podataka s tvorničke Azure podataka |
| 56 | [Podatke iz spremišta blobova platforme Azure učitali u SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Saznajte kako koristiti PolyBase da biste učitali podatke iz spremišta blobova platforme Azure u SQL Data Warehouse. Učitavanje nekoliko tablica iz javnih podataka u shemi Contoso maloprodaja Data Warehouse. |
| 57 | [Učitavanje podataka iz sustava SQL Server u Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Koristi bcp za izvoz podataka iz sustava SQL Server u plošnu datoteka, AZCopy da biste uvezli podatke u spremište blobova platforme Azure i PolyBase za ingest podatke u Azure SQL Data Warehouse. |
| 58 | [Učitavanje podataka iz sustava SQL Server u Azure SQL Data Warehouse (paušalni datoteke)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Za veličinom small podataka koristi bcp za izvoz podataka iz sustava SQL Server u plošnu datoteke i uvezli podatke izravno u Azure SQL Data Warehouse. |
| 59 | [Učitavanje podataka iz sustava SQL Server u Azure SQL podataka skladištu (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Prikazuje kako stvoriti paket sustava SQL Server integraciju servisa (SSIS) za premještanje podataka iz raznih izvora podataka u SQL Data Warehouse. |
| 60 | [Učitavanje podataka s PolyBase u skladištu podataka za SQL](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Koristi bcp za izvoz podataka iz sustava SQL Server u plošnu datoteka, AZCopy da biste uvezli podatke u spremište blobova platforme Azure i PolyBase za ingest podatke u Azure SQL Data Warehouse. |
| 61 | [Vodič za korištenje PolyBase u SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) | Upute i preporuke za upotrebu PolyBase u SQL Data Warehouse scenarijima. |
| 62 | [Učitavanje oglednim podacima u SQL Data Warehouse](sql-data-warehouse-load-sample-databases.md) | Učitavanje oglednim podacima u SQL Data Warehouse |
| 63 | [Učitavanje podataka s bcp](sql-data-warehouse-load-with-bcp.md) | Naučite koje bcp te kako ga koristiti za podatke skladištenje scenarijima. |
| 64 | [Podatke učitali u skladištu podataka za SQL Azure](sql-data-warehouse-overview-load.md) | Saznajte uobičajeni scenariji za podatke učitavanje u SQL Data Warehouse. To obuhvaća pomoću PolyBase, blobova platforme Azure, paušalni datoteke i dostave disk. Možete koristiti i alati drugih proizvođača. |



## <a name="migrate"></a>Migriranje

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 65 | [Migriranje SQL koda u SQL Data Warehouse](sql-data-warehouse-migrate-code.md) | Savjeti za migriranje SQL koda za Azure SQL Data Warehouse za razvoj rješenja. |
| 66 | [Migracija podataka](sql-data-warehouse-migrate-data.md) | Savjeti za migriranju podataka Azure SQL Data Warehouse za razvoj rješenja. |
| 67 | [Za migraciju podataka skladištu (pretpregled)](sql-data-warehouse-migrate-migration-utility.md) | Migrirati u SQL Data Warehouse. |
| 68 | [Migriranje sheme u SQL Data Warehouse](sql-data-warehouse-migrate-schema.md) | Savjeti za migriranje sheme za Azure SQL Data Warehouse za razvoj rješenja. |
| 69 | [Prenesite rješenje SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) | Migracija smjernice za prenošenje rješenje platformu Microsoft Azure SQL Data Warehouse. |



## <a name="partners"></a>Partneri

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 70 | [SQL Data Warehouse poslovne inteligencije partnere](sql-data-warehouse-partner-business-intelligence.md) | Popis proizvođača poslovne inteligencije partnere s rješenja koja podržavaju SQL Data Warehouse. |
| 71 | [Partneri za integraciju SQL Data Warehouse podataka](sql-data-warehouse-partner-data-integration.md) | Popis proizvođača partnere s rješenjima za integraciju s podacima koji podržavaju Azure SQL Data Warehouse. |
| 72 | [Partneri za upravljanje SQL Data Warehouse podataka](sql-data-warehouse-partner-data-management.md) | Popis partnera za upravljanje podataka drugih proizvođača s rješenja koja podržavaju SQL Data Warehouse. |



## <a name="reference"></a>Referenca

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 73 | [Teme referenca za SQL Data Warehouse](sql-data-warehouse-overview-reference.md) | Pregled sadržaja veze za SQL Data Warehouse. |
| 74 | [PowerShell cmdleti i REST API-ji za SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) | Pronađite gornji PowerShell cmdleti za Azure SQL Data Warehouse uključujući kako zaustaviti i nastaviti baze podataka. |
| 75 | [Elementi jezika](sql-data-warehouse-reference-tsql-language-elements.md) | Popis veza do referentne sadržaje za elemente Transact-SQL jezika koji se koriste za SQL Data Warehouse. |
| 76 | [SQL transakcija teme](sql-data-warehouse-reference-tsql-statements.md) | Veze na referentne sadržaje za teme u Transact-SQL koristi SQL Data Warehouse. |
| 77 | [Sistemski prikazi](sql-data-warehouse-reference-tsql-system-views.md) | Veza na sustav prikaze sadržaja za SQL Data Warehouse. |



## <a name="security"></a>Sigurnost

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 78 | [SQL Data Warehouse - sa starijim verzijama podrške za nadzor i dinamične podatke Masking](sql-data-warehouse-auditing-downlevel-clients.md) | Dodatne informacije o podršci za klijente hijerarhijski niži SQL Data Warehouse za nadzor podataka |
| 79 | [Nadzor u skladištu podataka Azure SQL](sql-data-warehouse-auditing-overview.md) | Početak rada s nadzora u skladištu podataka za SQL Azure |
| 80 | [Početak rada s prozirnim podataka šifriranja (TDE) u skladištu podataka za SQL](sql-data-warehouse-encryption-tde.md) | Šifriranje prozirne podataka (TDE) u skladištu podataka za SQL |
| 81 | [Početak rada s prozirnim podataka šifriranja (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Šifriranje prozirne podataka (TDE) u SQL Data Warehouse (T-SQL) |
| 82 | [Zaštita baze podataka u skladištu podataka za SQL](sql-data-warehouse-overview-manage-security.md) | Savjeti za osiguravanje baze podataka u skladištu podataka za SQL Azure za razvoj rješenja. |



## <a name="miscellaneous"></a>Različiti

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 83 | [Instalirajte Visual Studio 2015 i SSDT za SQL Data Warehouse](sql-data-warehouse-install-visual-studio.md) | Instalirajte Visual Studio i SQL Server razvoj Tools (SSDT) za Azure SQL Data Warehouse |
| 84 | [Migracija Premium prostora za pohranu detalja](sql-data-warehouse-migrate-to-premium-storage.md) | Upute za premještanje postojeće SQL Data Warehouse premium za pohranu |
| 85 | [Početak rada s prijetnju otkrivanje](sql-data-warehouse-security-threat-detection.md) | Upute za početak rada s prijetnju otkrivanje |
| 86 | [Ograničenja SQL Data Warehouse kapaciteta](sql-data-warehouse-service-capacity-limits.md) | Maksimalna vrijednost za veze, baze podataka, tablice i upite za SQL Data Warehouse. |
| 87 | [Otklanjanje poteškoća Data Warehouse Azure SQL](sql-data-warehouse-troubleshoot.md) | Otklanjanje poteškoća Azure SQL Data Warehouse. |

