<properties
   pageTitle="Otklanjanje poteškoća s Azure SQL Data Warehouse | Microsoft Azure"
   description="Otklanjanje poteškoća Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Otklanjanje poteškoća Data Warehouse Azure SQL

Ova tema sadrži popis neka od najčešćih pitanja otklanjanje poteškoća smo čuli naših korisnika.

## <a name="connecting"></a>Povezivanje

| Problem                              | Razlučivost                                      |
| :----------------------------------| :---------------------------------------------- |
| Prijava nije uspjela za korisnika "NT AUTHORITY\ANONYMOUS prijavu". (Microsoft SQL Server, pogreška: 18456) | Ta se pogreška pojavljuje kada korisnik sustava AAD pokušava se povezati s glavnom bazom podataka, ali ste korisnik matrici slajda.  Da biste riješili taj problem ili odredite SQL Data Warehouse koju želite povezati s u trenutku povezivanja ili dodavanje korisnika u glavnom bazom podataka.  Pogledajte članak [Pregled sigurnosti][] više pojedinosti.|
|Poslužitelj glavni "MyUserName" nije uspio pristup bazi podataka "matrica" u trenutnom kontekstu sigurnost. Nije moguće otvoriti korisnika zadanu bazu podataka. Prijava nije uspjela. Prijava nije uspjela za korisnika 'MyUserName'. (Microsoft SQL Server, pogreška: 916) | Ta se pogreška pojavljuje kada korisnik sustava AAD pokušava se povezati s glavnom bazom podataka, ali ste korisnik matrici slajda.  Da biste riješili taj problem ili odredite SQL Data Warehouse koju želite povezati s u trenutku povezivanja ili dodavanje korisnika u glavnom bazom podataka.  Pogledajte članak [Pregled sigurnosti][] više pojedinosti.|
| CTAIP pogreške                        | Ta se pogreška može dogoditi prilikom prijave na SQL server glavne baze podataka, ali ne u bazi podataka za SQL Data Warehouse stvorena.  Ako se pojavi Ova pogreška, pogledajte u članku [Pregled sigurnosti][] .  U ovom se članku objašnjava kako stvoriti stvaranje prijavu i korisničko na matricu, a zatim Stvaranje korisnika u bazi podataka za SQL Data Warehouse.|
| Vatrozid blokira                |Baze podataka SQL Azure nisu zaštićeni razine vatrozidima poslužitelj i bazu podataka da biste bili sigurni samo poznati IP adrese imaju pristup bazi podataka. U vatrozidima su sigurne po zadanim postavkama, što znači da morate izričito omogućiti i IP adresa ili raspon adrese prije povezivanja.  Da biste konfigurirali vatrozida za pristup, slijedite korake u [Konfiguriranje poslužitelja Vatrozid za IP klijenta][] u [Provisioning upute][].|
| Ne možete povezati s alat ili upravljački program | SQL Data Warehouse preporučuje korištenje [SSMS][], [SSDT za Visual Studio 2015][]ili [sqlcmd][] za dohvaćanje podataka. Dodatne informacije o upravljačke programe i povezivanje s SQL Data Warehouse u člancima [upravljačke programe za Azure SQL Data Warehouse][] i [Povezivanje s Azure SQL Data Warehouse][] .|


## <a name="tools"></a>Alati

| Problem                              | Razlučivost                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio objekt explorer nedostaje AAD korisnika | Ovo je poznat problem.  Kao zaobilazno rješenje, pogledati korisnike [sys.database_principals][].  U odjeljku [Provjera autentičnosti za Azure SQL Data Warehouse][] da biste saznali više o korištenju Azure Active Directory sa SQL Data Warehouse.|

## <a name="performance"></a>Performanse

|  Problem                             | Razlučivost                                      |
| :----------------------------------| :---------------------------------------------- |
| Otklanjanje poteškoća s performansama za upite  | Ako pokušavate otklanjanje poteškoća s određenom upita, započnite s [učenjem kako praćenje upite][].|
| Performanse loše upita i tarife često je rezultat nedostaju Statistika   | Najčešći uzrok slabe performanse je nedostatak statistike o tablice.  Pogledali [statistiku tablice Maintaining] [ Statistics] pojedinosti o stvaranju Statistika i zašto su važnosti da performansi.|
| Niska istodobnosti / u redu čekanja upita   | Objašnjenje [upravljanja radno opterećenje][] je važno da biste razumjeli kako uskladiti dodjelu memorije s istodobnosti.|
| Kako implementirati najbolje prakse    | Najbolje postavite da biste pokrenuli da biste naučili poboljšanje performansi upita [SQL Data Warehouse najbolje prakse][] članka.|
| Kako poboljšati performanse s skaliranja  | Ponekad je rješenje za poboljšanje performansi jednostavno dodati više izračunati power upitima po [Skaliranje SQL Data Warehouse][].|
| Performanse loše upita kao rezultat indeksa loše kvalitete | Nekoliko puta upite možete usporenje zbog [kvalitete nisku columnstore indeksa][].  U ovom se članku dodatne informacije potražite u članku te kako [ponovno stvaranje indeksa radi poboljšanja kvalitete segmenta][].|

## <a name="system-management"></a>Upravljanje sustavom

|  Problem                             | Razlučivost                                      |
| :----------------------------------| :---------------------------------------------- |
| Poruka 40847: Nije moguće izvesti operaciju jer je poslužitelj prekoračuje dopuštene kvota jedinica transakcije baze podataka za 45000. | Smanjite [DWU][] bazu podataka koju pokušavate stvoriti ili [zahtjev za kvote povećava][].|
| Istražuje Upotreba prostora    | U odjeljku [Veličina tablice][] da biste shvatili Upotreba prostora sustava.|
| Pomoć pri postavljanju tablice          | Potražite u članku [Pregled tablica] [ Overview] članak pomoć za upravljanje tablice.  Članak sadrži i veze na detaljnije teme kao što su [vrste podataka u tablici][Data types], [distribucija tablice][Distribute], [Indeksiranje tablice][Index], [particija tablice][Partition], [Maintaining tablice Statistika] [ Statistics] i [privremene tablice][Temporary].|

## <a name="polybase"></a>Polybase

|  Problem                             | Razlučivost                                      |
| :----------------------------------| :---------------------------------------------- |
| Pogreška UTF-8                        |  Trenutno PolyBase podržava samo učitavanja podatkovne datoteke koje su UTF-8 kodiran.  Potražite u članku [rad oko obavezne PolyBase UTF-8][] upute o tome kako zaobići to ograničenje.|
| Učitavanje neće uspjeti zbog velike redaka   | Trenutno velike retka podrška nije dostupna za Polybase.  To znači da ako tablica sadrži VARCHAR(MAX), NVARCHAR(MAX) ili VARBINARY(MAX), vanjski tablice ne može koristiti da biste učitali podatke.  Opterećenje za retke velike trenutno podržava samo kroz Azure tvorničke podataka (s BCP), Azure strujanje analize, SSIS, BCP ili .NET SQLBulkCopy predmete. PolyBase podrške za velike retke dodat će se u buduće izdanje.|
| ne daje BCP učitavanja tablice s vrstom podataka MAX | Postoji poznat problem koji zahtijeva VARCHAR(MAX), NVARCHAR(MAX) ili VARBINARY(MAX) smjestiti na kraj tablice u nekim scenarijima.  Pokušajte premjestiti MAX stupaca na kraj tablice.|

## <a name="differences-from-sql-database"></a>Razlike iz baze podataka SQL

|  Problem                             | Razlučivost                                      |
| :----------------------------------| :---------------------------------------------- |
| Nepodržane značajke SQL baze podataka  | [Nepodržane značajke tablica][]potražite u članku.|
| Nepodržane vrste podataka za SQL baze podataka  | [Nepodržane vrste podataka][]potražite u članku.|
| Brisanje i ažuriranje ograničenja      | Potražite [zaobilazna rješenja za ažuriranje][], [Brisanje zaobilazna rješenja][] i [Pomoću CTAS da biste zaobišli nepodržane ažuriranje i brisanje sintakse][].  |
| Naredba SPOJI nije podržana   | Potražite u članku [SPAJANJE zaobilazna rješenja][].|
| Pohranjena procedura ograničenja       | Potražite u članku [ograničenja spremljene postupak][] da biste razumjeli neke od ograničenja pohranjene procedure.|
| Korisnički definiranih funkcija ne podržavaju izjava ODABIRA | Ovo je trenutno ograničenje naš UDF-ove.  Vidjet ćete da sintaksa podržavamo potražite u članku [Stvaranje (opis funkcije)][] .   |

## <a name="next-steps"></a>Daljnji koraci

Ako su nije moguće pronaći rješenje problem iznad, Evo nekih drugih resursa možete pokušati.

- [Blogovi]
- [Zahtjeve za značajke]
- [Videozapisi]
- [MAČKA blogova]
- [Stvaranje zahtjev za podršku možete]
- [MSDN forum]
- [Forum prelijevanja stogu]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Pregled sigurnosti]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT za Visual Studio 2015.]: ./sql-data-warehouse-install-visual-studio.md
[Upravljačke programe za Azure SQL Data Warehouse]: ./sql-data-warehouse-connection-strings.md
[Povezivanje s Data Warehouse Azure SQL]: ./sql-data-warehouse-connect-overview.md
[Stvaranje zahtjev za podršku možete]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Promjena veličine SQL Data Warehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[zahtjev za kvote povećanje]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Naučite praćenje upitima]: ./sql-data-warehouse-manage-monitor.md
[Upute za dodjelu resursa]: ./sql-data-warehouse-get-started-provision.md
[Konfiguriranje poslužitelja Vatrozid za IP klijenta]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Najbolje prakse za SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Veličina tablice]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Nepodržane značajke tablica]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Nepodržane vrste podataka]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Kvaliteta nisku columnstore indeksa]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Ponovno stvaranje indeksa radi poboljšanja kvalitete segmenta]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Upravljanje radno opterećenje]: ./sql-data-warehouse-develop-concurrency.md
[Da biste zaobišli nepodržane ažuriranje i brisanje sintaksa pomoću CTAS]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Ažuriranje zaobilazna rješenja]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Brisanje zaobilazna rješenja]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[SPAJANJE zaobilazna rješenja]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Pohranjena procedura ograničenja]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Provjera autentičnosti za Azure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md
[Rad oko obavezne PolyBase UTF-8]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[STVARANJE (FUNKCIJA)]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogovi]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[MAČKA blogova]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Zahtjeve za značajke]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Forum prelijevanja stogu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videozapisi]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

