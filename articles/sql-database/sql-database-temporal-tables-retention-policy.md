<properties
   pageTitle="Upravljanje povijesnim podataka u tablicama vremenski pomoću pravilnika o zadržavanju | Microsoft Azure"
   description="Saznajte kako koristiti pravila zadržavanja vremenski da biste zadržali proteklih podataka u odjeljku kontrole."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Upravljanje povijesnim podataka u tablicama vremenski pomoću pravilnika o zadržavanju

Vremenski tablica može se povećati veličina baze podataka na više obične tablice, pogotovo ako zadržati ranijih podataka za dulje vremensko razdoblje. Dakle, pravilnika o zadržavanju za povijesne podatke je važno aspekt planiranje i upravljanje životnim ciklusom svaki vremenski tablice. Vremenski tablica u bazi podataka SQL Azure isporučuje se s zadržavanja jednostavno koristi mehanizam koji olakšava obaviti taj zadatak.

Vremenski povijest zadržavanja ne smije biti postavljen na razini pojedinačnih tablica, koje korisnicima omogućuje stvaranje fleksibilna dospijeće pravila. Primjena vremenski zadržavanja je jednostavno: bit će potrebno samo se jedan parametar postaviti tijekom stvaranja ili sheme promjena tablice.

Kada definirate pravilnika o zadržavanju, počet će baze podataka SQL Azure redovito provjerava postoje li povijesne retke koji ispunjavaju uvjete za čišćenje podataka. Identifikacijski podudarnih redaka i njihovih uklanjanja iz tablice povijest doći do proziran, u pozadini zadatak koji je zakazano i pokrenite sustav. Uvjet dob za retke tablice povijest je označeno utemeljene na stupcu koji predstavlja kraj SYSTEM_TIME razdoblja. Ako je razdoblje zadržavanja, na primjer, postavljeno na šest mjeseci, reci tablice koji ispunjava uvjete za čišćenje zadovoljava sljedeće uvjete:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

U primjeru iznad smo pretpostavlja da odgovara li na kraju razdoblja SYSTEM_TIME **ValidTo** stupca.

##<a name="how-to-configure-retention-policy"></a>Upute za konfiguriranje pravilnika o zadržavanju?

Prije no što konfigurirate pravilnika o zadržavanju za vremenski tablicu, najprije provjerite je li vremenski povijesne zadržavanja omogućena *na razini baze podataka*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Baza podataka zastavice **is_temporal_history_retention_enabled** postavite na Uključeno po zadanom, no korisnici mogu promijeniti s izjava ALTER baze podataka. Automatski postavlja se na ISKLJUČENO nakon [pokažite u vrijeme vraćanja](sql-database-point-in-time-restore-portal.md) operacije. Da biste omogućili vremenski povijest čišćenje zadržavanja za bazu podataka, izvršavanje sljedeću naredbu:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Možete konfigurirati zadržavanja za vremenski tablice, čak i ako **is_temporal_history_retention_enabled** je ISKLJUČENA, ali automatsko čišćenje za staraca retke u tom slučaju neće se pokrenuti.


Pravila zadržavanja je konfiguriran tijekom stvaranja tablice tako da navedete vrijednost za parametar HISTORY_RETENTION_PERIOD:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Baze podataka SQL Azure omogućuje vam da navedete razdoblje zadržavanja pomoću različitih vremenskih jedinica: dana, TJEDANA, MJESECI i godina. Ako HISTORY_RETENTION_PERIOD izostavi, pretpostavlja se da je BESKONAČNO zadržavanja. BESKONAČNO ključnih riječi možete koristiti i izričito.

U nekim slučajevima, trebali biste konfigurirati zadržavanja nakon stvaranja tablice ili da biste promijenili prethodno konfigurirali vrijednost. U tom slučaju koristite iskaz ALTER TABLE:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  Postavljanje SYSTEM_VERSIONING na ISKLJUČENO *sačuvati* zadržavanja razdoblja vrijednost. Postavljanje SYSTEM_VERSIONING na Uključeno bez HISTORY_RETENTION_PERIOD naveden izričito rezultate u razdoblje zadržavanja BESKONAČNO.

Da biste pregledali trenutno stanje pravilnik zadržavanja, koristite sljedeći upit koji spaja vremenski zadržavanja omogućenja zastavice na razini baze podataka s razdoblja zadržavanja za pojedine tablice:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Kako se briše baze podataka SQL Dospjeli redaka?

Čišćenje postupak ovisi o izgledu indeks povijest tablice. Važno je da obratite pozornost na tom *samo povijest tablica s indeks (B stabla ili columnstore) može imati konfiguriran pravilnika o zadržavanju konačne*. Za izvođenje čišćenje staraca podataka za sve vremenski tablice s razdoblje zadržavanja konačne stvoren je zadatak u pozadini.
Čišćenje logike za indeks rowstore (B stabla) briše staraca retka u manjim blokova (do 10 K) minimiziranje pritisak na zapisnika baze podataka i podsustav/i. Iako se koristi za čišćenje logike potrebna redoslijeda brisanja za retke starije od razdoblje zadržavanja ne može se čvrsto zajamčiti B stabla indeks. Dakle, *neće moći koristiti sve zavisnost o čišćenje redoslijedom aplikacija*.

Čišćenje zadatak za grupirani columnstore uklanja cijele [grupe redaka](https://msdn.microsoft.com/library/gg492088.aspx) odjednom (obično sadrže 1 milijun redaka svaki), koji je vrlo učinkovitog, osobito kada proteklih podataka se generira visoke tempom.

![Grupirani columnstore zadržavanja](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Spajanja izvrstan podataka i učinkovito zadržavanja čišćenje čini grupirani columnstore indeks savršen odabir scenarijima za kada svoje radno opterećenje hitro generira visoke količinu povijesnim podataka. Karakteristično za intenzivno [radnih opterećenja transakcijskih obrada koje koriste vremenski tablice](https://msdn.microsoft.com/library/mt631669.aspx) za evidentiranje promjena i nadzor, analizu trenda ili IoT podataka ingestion je tim uzorkom.

##<a name="index-considerations"></a>Razmatranja indeksa

Čišćenje zadatak za tablice s rowstore indeks započeti s potrebne indeks stupca odgovaraju na kraju razdoblja SYSTEM_TIME. Ako ne postoji takvo indeks, nećete moći konfiguriranje razdoblje zadržavanja konačne:

*Poruka 13765 16 razini 1 stanju <br> </br> postavka razdoblje zadržavanja konačne nije uspjela u tablici sustava određuju vremenski 'temporalstagetestdb.dbo.WebsiteUserInfo' jer tablici povijesti 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' sadrži potrebne indeks. Razmislite o stvaranju grupirani columnstore ili B stabla indeksa počevši od stupca koji odgovara kraj SYSTEM_TIME razdoblja u tablici povijesti.*

Važno je da obratite pozornost na zadanu tablicu povijest stvorio baze podataka SQL Azure već sadrži grupirani indeks koji je usklađena za pravila zadržavanja. Ako pokušate da biste uklonili to kazalo na tablicu s razdoblje zadržavanja konačne, ne uspije s sljedeća pogreška:

*Poruka 13766 16 razine 1 stanje <br> </br> indeks 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' ne može se ispustiti jer se koristi za automatsko čišćenje staraca podataka. Razmislite o postavka HISTORY_RETENTION_PERIOD za BESKONAČNO na odgovarajuće mjesto sustava vremenski tablice ako je potrebno ispustite ovaj indeks.*

Čišćenje na grupirani columnstore indeksa radi optimalnog ako povijesne redaka umeću na uzlaznim redoslijedom (naručili na kraju razdoblja stupaca), koja je uvijek slučaj kada se tablici povijesti popunjava isključivo mehanizam SYSTEM_VERSIONIOING. Ako retke u tablici povijesti su nisu poredane kraja razdoblja stupaca (koji mogu biti slučaj migrirati postojeću ranijih podataka), trebali biste ponovno stvoriti indeks grupirani columnstore pri vrhu B stabla rowstore indeks koji je ispravno naručiti, da biste postigli optimalne performanse.

Izbjegavajte novog grupirani columnstore indeksa u tablici povijesti s razdoblje zadržavanja konačne jer se može promijeniti redoslijed u grupe redaka prirodan nameće operacija sustava verzijama. Ako je potrebno ponovno stvaranje indeksa grupirani columnstore u tablici povijesti učiniti tako da ponovno stvorite pri vrhu usklađen B stabla indeks, uz zadržavanje redoslijedom u rowgroups potrebne za čišćenje podataka u pravilnim. Na isti način treba poduzeti u slučaju stvorite vremenski tablicu s postojećoj tablici povijesti koji sadrži grupirani stupac indeksa bez sigurno podatke redoslijedom:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Kada razdoblje zadržavanja konačne je konfiguriran za tablicu povijest grupirani columnstore indeks, ne možete stvoriti dodatne koje nisu grupirani indeksi B stabla na toj tablici:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Pokušaj izvršiti iznad izjava neće uspjeti zbog sljedeće pogreške:

*Poruka 13772 16 razine 1 stanje <br> </br> nije moguće stvoriti indeks koji nisu grupirani na tablici vremenski povijest 'WebsiteUserInfoHistory' jer je razdoblje zadržavanja konačne i grupirani columnstore indeksa definirani.*

##<a name="querying-tables-with-retention-policy"></a>Slanje upita tablice s pravilnika o zadržavanju

Sve upite u tablici vremenski automatski filtriraju povijesne redaka koji se podudaraju konačne pravilnika o zadržavanju, da biste izbjegli rezultate nepredvidljive i koje nisu usklađene jer staraca retke moguće je izbrisati tako da zadatak čišćenja *bilo kojem trenutku u vremenu i proizvoljne redoslijedom*.

Sljedeća slika prikazuje upit Osmišljavanje jednostavnog upita:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

Planiranje upita sadrži dodatne filtrom primijenjenim na kraju razdoblja stupca (ValidTo) u operator "Grupirani indeks skeniranje" u tablici povijesti (istaknuto). U ovom se primjeru pretpostavlja da to razdoblje zadržavanja 1 MJESEC postavljen je na WebsiteUserInfo tablice.

![Filtar upita zadržavanja](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Međutim, ako izravno upita povijest tablice, možda ćete vidjeti redaka koji su starije od navedenog zadržavanja razdoblje, ali bez sve jamstva za rezultate kontrolirani upita. Sljedeća slika prikazuje upit izvršni plan za upit u tablici povijesti bez dodatnih primijenjenim:

![Slanje upita povijest bez filtra zadržavanja](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Koristi poslovne logike na tablici povijesti izvan razdoblje zadržavanja za čitanje kao mogla bi vam se nedosljedno ili neočekivane rezultate. Preporučuje se da biste koristili vremenski upiti s SYSTEM_TIME za uvjet za analizu podataka u vremenski tablica.

##<a name="point-in-time-restore-considerations"></a>Postavite pokazivač u vrijeme vraćanja pitanja

Kada stvorite novu bazu podataka vraćanjem [postojeću bazu podataka na određeno mjesto u vremenu](sql-database-point-in-time-restore-portal.md), sadrži vremenski zadržavanja onemogućena na razini baze podataka. (zastavica**is_temporal_history_retention_enabled** postavite na ISKLJUČENO). To vam omogućuje da pregledate sve retke povijesne nakon vraćanja, bez brige staraca redaka uklanjaju prije dobijete upit ih. Možete je koristiti za *provjeru proteklih podataka izvan razdoblje zadržavanja konfigurirani*.

Recimo da vremenski tablica ima mjesec dana zadržavanja razdoblja naveden. Ako bazu podataka stvorena je u sloju Premium servis, će moći stvoriti kopiju baze podataka sa statusom baze podataka prema gore da biste 35 dana u prošlosti. Koji učinkovito bi omogućuju da biste analizirali povijesne redaka koji su do 65 dana upitom tablici povijesti izravno.

Ako želite da biste aktivirali čišćenje vremenski zadržavanja, pokrenite sljedeću naredbu Transact-SQL iza zareza u vrijeme vraćanja:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Daljnji koraci

Da biste saznali kako koristiti vremenski tablica u aplikacija pogledajte [Uvod u vremenski tablica u bazi podataka SQL Azure](sql-database-temporal-tables.md).

Posjetite 9 kanala da biste čuli [stvarnih klijenata vremenski implementaciju uspjeha priče](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) i gledanje na [live vremenski pokazni](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Detaljne informacije o tablicama vremenski pregledajte [MSDN dokumentacije](https://msdn.microsoft.com/library/dn935015.aspx).
