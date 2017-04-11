<properties
    pageTitle="SQL u memoriji, koraci | Microsoft Azure"
    description="SQL memorije tehnologije znatno poboljšati performanse transakcijskih i radnih opterećenja analize. Saznajte kako da iskoristite prednost tih tehnologija."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Uvod u memoriji (pretpregled) u SQL baze podataka

Značajke u memoriji znatno poboljšati performanse transakcijskih i radnih opterećenja analize u desnom situacijama.

U ovoj se temi ističe dva demonstracije, jedan za OLTP u memoriji i jedan za analizu u memoriji. Svaki pokazni videozapis dolazi s korake i kod, morat ćete pokrenuti videozapis. Možete:

- Da biste testirali varijacije da biste vidjeli razlike u rezultatima performanse; pomoću koda ili
- Kod da biste shvatili scenarij, a da biste vidjeli kako stvoriti i koristiti objekata u memoriji za čitanje.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [1 za brzi početak: memorije tehnologije OLTP brže performanse T SQL](http://msdn.microsoft.com/library/mt694156.aspx) -je drugi članak olakšati početak rada.

#### <a name="in-memory-oltp"></a>OLTP u memoriji

Značajke u memoriji [OLTP](#install_oltp_manuallink) (obrada transakcije online) obuhvaćaju sljedeće:

- Memorija optimizirana tablice.
- Nativno prevesti pohranjene procedure.


Memorija optimizirana tablica ima jedan prikaz sam u aktivni memoriji, osim standardni prikaz na tvrdom disku. Poslovne transakcije na tablici izvoditi brže jer izravno interakcije s prikaz koji se nalazi u aktivni memorije.

S OLTP u memoriji, možete se postići da bi se dobio 30 vremena u propusnost transakcije, ovisno o detaljima povećavaju.


Nativno kompilirane pohranjene procedure potrebno manje strojno upute tijekom vremena izvođenja od tradicionalni interpretiraju pohranjene procedure. Ne možemo vidjeti rezultat nativni sastavljanja u trajanja koja ima vrijednost 1/100th interpreted trajanja.


#### <a name="in-memory-analytics"></a>Analitički u memoriji 

Značajka u memoriji [analize](#install_analytics_manuallink) je:

Indeksi Columnstore poboljšati performanse analitičkih podataka i izvješćivanje o upita. 


#### <a name="real-time-analytics"></a>U stvarnom vremenu analize

[U stvarnom vremenu](http://msdn.microsoft.com/library/dn817827.aspx) analitičkih kombinirati OLTP u memoriji i analitiku da biste dobili:

- Uvid u stvarnom vremenu tvrtke ovisno o radu sa servisom podataka.


#### <a name="availability"></a>Dostupnost


GA, Općenito dostupan:

- [Indeksi Columnstore](http://msdn.microsoft.com/library/dn817827.aspx) *na disku*.


Pregled:

- OLTP u memoriji
- U stvarnom vremenu radu Analytics


Razmatranja dok se pretpregled značajke u memoriji su što je opisano [u nastavku ove teme](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Te pretpregled su značajke dostupne samo za [*Premium*](sql-database-service-tiers.md) baza podataka nije u elastic grupe, a nije dostupan za sve Basic ili Standard baze podataka.  Podrška za Premium baze podataka u elastic grupe uskoro dostupno. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>ODGOVORI. Instalacija uzorka OLTP u memoriji

AdventureWorksLT [V12] oglednu bazu podataka možete stvoriti tako da samo nekoliko klikova [Azure portal](https://portal.azure.com/). Zatim koraci u ovom odjeljku objašnjavaju kako obogatiti AdventureWorksLT bazom podataka pomoću:

- Tablica u memoriji.
- Nativno kompilirane pohranjena procedura.


#### <a name="installation-steps"></a>Korake za instalaciju

1. [Portal za Azure](https://portal.azure.com/)stvorite bazu podataka Premium na poslužitelju V12. Postavljanje **izvora** za AdventureWorksLT [V12] oglednu bazu podataka.
 - Detaljne upute, vidjet ćete [Stvaranje prve baze podataka Azure SQL](sql-database-get-started.md).

2. Povezivanje s bazom podataka s SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Kopiranje [u memoriji OLTP Transact-SQL skripte](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) u međuspremnik.
 - T-SQL skripta stvara potrebne u memoriji objekata u AdventureWorksLT oglednu bazu podataka koju ste stvorili u koraku 1.

4. Zalijepite T SQL skripta SSMS i izvršavanje skriptu.
 - Presudne je na `MEMORY_OPTIMIZED = ON` naredbe CREATE TABLE uvjet, kao u:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Pogreška 40536


Ako se pojavi pogreška 40536 kada pokrenete T SQL skripta, pokrenite sljedeću skriptu T SQL da biste provjerili podržava li baza podataka u memoriji:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Rezultat **0** znači da nije podržan u memoriji, a 1 znači da je podržana. Dijagnosticiranje problema:

- Provjerite je li baza podataka stvorena je nakon značajke u memoriji OLTP postala aktivna za pretpregled.
- Provjerite je li baza podataka na servis sloju Premium.


#### <a name="about-the-created-memory-optimized-items"></a>O stvoreni memorija optimizirana stavkama

**Tablica**: uzorka sadrži memorija optimizirana u tablicama u nastavku:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Memorija optimizirana tablice pomoću **Programa Explorer objekta** u SSMS tako da možete provjeriti:

- Desnom tipkom miša kliknite **tablica** > **Filtar** > **Postavki filtra** > **Memorije optimizacije** jednako je 1.


Ili upit kataloga prikaze možete poslati kao što su:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Nativno kompilirane pohranjena procedura**: SalesLT.usp_InsertSalesOrder_inmem možete potražiti putem upita za prikaz kataloga:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Pokretanje povećavaju OLTP uzorka

Jedina razlika između u sljedeća dva *pohranjene procedure* je da prvi postupak koristi verzije optimizirane za memorije tablice, dok drugi postupak koristi običan tablice na disku:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


U ovom odjeljku objašnjavaju kako koristiti pri ruci **ostress.exe** utility za izvršavanje dva pohranjene procedure pri stressful razine. Koliko dugo traje da se pokreće dva opterećenjem da biste dovršili možete usporediti.


Kada pokrenete ostress.exe, preporučujemo da proslijedite vrijednosti parametara oboje namijenjenu:

- Pokretanje velikog broja Istodobni veze korištenjem - n100.
- Po ste svakoj petlji veze stotine puta, pomoću - r500.


Međutim, preporučujemo da biste započeli s mnogo manje vrijednosti kao što su - n10 i - radi r50 da bi se sve.


### <a name="script-for-ostressexe"></a>Skripta za ostress.exe


U ovom se odjeljku prikazuje T SQL skriptu koja je uložen u našem ostress.exe naredbenog retka. Skripta koristi stavke koje su stvorene pomoću skripte T SQL ste prethodno instalirali.


Sljedeću skriptu umeće uzorka narudžbenici s pet stavke u sljedećim memorija optimizirana *tablice*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Da biste _ondisk verziju prethodnom SQL T za ostress.exe, samo želite zamijeniti oba pojavljivanja podniz *_inmem* s *_ondisk*. Ove se zamjenjuje utjecati na nazive tablica i pohranjene procedure.


### <a name="install-rml-utilities-and-ostress"></a>Instalirajte uslužni programi RML i ostress


Najbolje bi namjeravate pokrenuti ostress.exe programa Azure VM. Želite stvoriti [Azure virtualnog računala](https://azure.microsoft.com/documentation/services/virtual-machines/) na istom Azure zemljopisnom području gdje se nalazi AdventureWorksLT bazu podataka. No možete umjesto toga pokrenuti ostress.exe na prijenosnom računalu.


Na VM ili na neku drugu hostira odaberite, instalirajte uslužni programi Ponovi oznake jezika (RML), što obuhvaća ostress.exe.

- Potražite u članku ostress.exe rasprave u [Oglednu bazu podataka za OLTP u memoriji](http://msdn.microsoft.com/library/mt465764.aspx).
 - Ili se obratite [Oglednu bazu podataka za OLTP u memoriji](http://msdn.microsoft.com/library/mt465764.aspx).
 - Ili pročitajte članak [na blogu za instalaciju ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Prvo pokretanje povećavaju _inmem opterećenjem


Prozor programa *RML Cmd upita* možete koristiti da biste pokrenuli naše ostress.exe naredbenog retka. Parametri naredbenog retka s izravnim ostress da biste:

- Istovremeno pokretanje 100 veze (-n100).
- Svaki vezu pokrenuti skriptu T SQL 50 vremena (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Da biste pokrenuli prethodni ostress.exe naredbenog retka:


1. Vraćanje izvornih postavki sadržaja podataka baze podataka pokretanjem sljedeće naredbe u SSMS da biste izbrisali sve podatke koji je umetnut po bilo kojem prethodnih izvođenja:
```
EXECUTE Demo.usp_DemoReset;
```

2. Kopiranje teksta prethodni ostress.exe naredbenog retka u međuspremnik.

3. Zamjena na `<placeholders>` za na parametara -S - U -P -d ispravnim realni vrijednostima.

4. U prozoru za razmjenu RML Cmd pokrenite uređivati naredbenog retka.


#### <a name="result-is-a-duration"></a>Rezultat je trajanje


Kada se dovrši ostress.exe, piše izvođenja trajanje kao njegov posljednjeg retka izlaza u prozoru RML Cmd. Na primjer, kraću cilja Testiraj lasted 1,5 minuta:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Ponovno postavljanje, urediti _ondisk, a zatim ponovno pokrenite


Nakon što ste rezultat izvođenja _inmem, za _ondisk pokretanje poduzeti sljedeće korake:


1. Vraćanje izvornih postavki baze podataka pokretanjem sljedeće naredbe u SSMS da biste izbrisali sve podatke koji je umetnut prethodne pokretanjem:
```
EXECUTE Demo.usp_DemoReset;
```

2. Uređivanje ostress.exe naredbenog retka da biste zamijenili sve *_inmem* *_ondisk*.

3. Ponovno pokrenite ostress.exe za drugi put, a zatim snimite rezultat trajanje.

4. Ponovno Vrati bazu podataka za odgovoran brisanje koje se mogu veliku količinu podataka za testiranje.


#### <a name="expected-comparison-results"></a>Usporedba očekivane rezultate

Naš testira u memoriji prikazati na poboljšanje performansi **9 vremena** za ovog simplistic posla s ostress sustavom programa Azure VM na istom Azure području kao baza podataka.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Instalacija uzorka analize u memoriji


U ovom se odjeljku uspoređuju rezultate IO i statističkih podataka prilikom korištenja columnstore indeksa i tradicionalni b stabla indeksa.


U stvarnom vremenu analitičkih na programa OLTP radno opterećenje često je najbolje koristiti NONclustered columnstore indeksa. Detalje potražite u članku [Opisane indeksi Columnstore](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Priprema columnstore analytics test


1. Stvaranje Osvježi AdventureWorksLT baze podataka iz oglednog pomoću portala za Azure.
 - Koristite taj točan naziv.
 - Odaberite bilo koju sloju Premium servisa.

2. Kopiranje [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) u međuspremnik.
 - T-SQL skripta stvara potrebne u memoriji objekata u AdventureWorksLT oglednu bazu podataka koju ste stvorili u koraku 1.
 - Skripta stvara tablicu dimenzija i dvije tablice činjenica. 3.5 milijun redaka svaki se popunjava tablice činjenica.
 - Skripta može potrajati 15 minuta.

3. Zalijepite T SQL skripta SSMS i izvršavanje skriptu.
 - Presudne se **COLUMNSTORE** ključnu riječ na iskaz **CREATE INDEX** , kao:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Postavite AdventureWorksLT razinu kompatibilnosti 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Razina 130 nisu izravno povezani s značajke u memoriji. No razinu 130 obično omogućuje brže performanse upita od 120.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Presudne tablica i columnstore indeksa


- vlasnika baze podataka. FactResellerSalesXL_CCI je tablicu koja sadrži grupirani **columnstore** indeks koji naprednim sažimanje na razini *podataka* .

- vlasnika baze podataka. FactResellerSalesXL_PageCompressed je tablicu koja sadrži ekvivalentan uobičajeni grupirani indeksa, koji je komprimiran samo na razini *stranice* .


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Presudne upita da biste usporedili columnstore indeksa


[Ovdje](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) nekoliko vrsta upita T SQL možete pokrenuti da biste vidjeli poboljšanja performansi. Iz T SQL skripta korak 2, postoji par upiti koji su izravno interesa. Dva upita razlikuju se samo na jedan redak:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Grupirani columnstore indeks je u tablici**_CCI** FactResellerSalesXL.

Sljedeći isječak skripte T SQL ispisuje Statistika za IO i VREMENA za upit za svaku tablicu.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Pretpregled zahtjevi za OLTP u memoriji


Značajke u memoriji OLTP u bazi podataka SQL Azure postala [aktivna za pretpregled na listopad 28, 2015](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/).


Na trenutni pretpregled u memoriji OLTP podržani su samo za:

- Baze podataka koje se nalaze na servis sloj *Premium* .

- Baze podataka stvorene nakon značajke u memoriji OLTP postala aktivna.
 - Novu bazu podataka ne podržava OLTP u memoriji ako ga je vraćen iz baze podataka koja je stvorena prije značajke u memoriji OLTP postala aktivna.


Kada se nalazite u niste sigurni, možete izvesti u sljedeće T-SQL odaberite da biste provjerili podržava li bazu podataka u memoriji OLTP. Rezultat operacije **1** znači bazu podataka ne podržava OLTP u memoriji:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Ako je upit vraća **1**, u memoriji OLTP podržana u toj bazi podataka, a sve kopiju baze podataka i vraćanje baze podataka stvorene na temelju Ova baza podataka.


#### <a name="objects-allowed-only-at-premium"></a>Objekti dopušten samo Premium


Ako bazu podataka sadrži neki od sljedećih vrste objekata u memoriji OLTP ili vrste, nije podržano prijelaz na stariju sloju servisa baze podataka od Premium osnovne i standardne. Vraćanje na stariju verziju baze podataka, najprije odbaciti tih objekata:

- Memorija optimizirana tablice
- Vrste memorija optimizirana tablica
- Nativno kompilirane moduli


#### <a name="other-relationships"></a>Druge odnose


- Korištenje OLTP u memoriji značajke s bazama podataka u elastic grupe nije podržana tijekom pretpregleda.
 - Da biste premjestili bazu podataka koja sadrži ili je prošao objekata u memoriji OLTP elastic resurse, slijedite ove korake:
  - 1. Odbacivanje sve memorija optimizirana tablica, vrste tablica i nativno kompilirane T SQL module u bazi podataka
  - 2. Promijenili sloju servis baze podataka u standardni prikaz
  - 3. Premještanje baze podataka u elastic skup

- Korištenje OLTP u memoriji s SQL Data Warehouse ne podržava.
 - Značajka columnstore indeksa u memoriji analize podržano je u SQL Data Warehouse.

- Spremište upita snimite upita unutar nativno prevedena module.

- Neke značajke Transact-SQL nisu podržani s OLTP u memoriji. To se odnosi na Microsoft SQL Server i baze podataka SQL Azure. Detalje potražite u članku:
 - [SQL transakcija podrška za OLTP u memoriji](http://msdn.microsoft.com/library/dn133180.aspx)
 - [SQL transakcija konstrukta ne podržava OLTP u memoriji](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Daljnji koraci


- Pokušajte [OLTP u memoriji za korištenje u postojeću aplikaciju SQL Azure.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Dodatni resursi

#### <a name="deeper-information"></a>Detaljnije informacije

- [Dodatne informacije o OLTP u memoriji, koji se odnosi na Microsoft SQL Server i baze podataka SQL Azure](http://msdn.microsoft.com/library/dn133186.aspx)

- [Dodatne informacije o u stvarnom vremenu radu analize na MSDN-u](http://msdn.microsoft.com/library/dn817827.aspx)

- Studija na [uobičajene uzorke radno opterećenje i razmatranja preseljenja](http://msdn.microsoft.com/library/dn673538.aspx), koji opisuje radno opterećenje uzoraka gdje u memoriji OLTP obično pruža značajnu performansi.

#### <a name="application-design"></a>Dizajniranje aplikacije

- [OLTP (u memoriji optimizaciju) u memoriji](http://msdn.microsoft.com/library/dn133186.aspx)

- [Korištenje memorije OLTP u postojeću aplikaciju SQL Azure.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Alati

- [SQL Server podataka Alati za pregled (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), za najnoviju verziju mjesečno.

- [Opis jezika (RML) uslužni programi Ponovi oznake za SQL Server](http://support.microsoft.com/en-us/kb/944837)

- [Monitor u memorijski prostor za pohranu](sql-database-in-memory-oltp-monitoring.md) za OLTP u memoriji.

