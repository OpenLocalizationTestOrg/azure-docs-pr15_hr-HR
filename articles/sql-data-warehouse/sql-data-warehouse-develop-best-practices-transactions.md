<properties
   pageTitle="Optimiziranje transakcije za SQL Data Warehouse | Microsoft Azure"
   description="Najbolja praksa smjernice o pisanje učinkovitog transakcije ažuriranja u Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimiziranje transakcije za SQL Data Warehouse

U ovom se članku objašnjava kako optimiziranja performansi transakcijskih koda tijekom minimiziranje rizika za dugačke vraćanja na staro stanje.

## <a name="transactions-and-logging"></a>Transakcije i zapisivanje

Transakcije su važne komponenta modul za relacijske baze podataka. SQL Data Warehouse koristi transakcije tijekom izmjene. Te transakcije može biti eksplicitnih ili implicitnih. Jedan `INSERT`, `UPDATE` i `DELETE` izjave su sve Primjeri implicitno transakcije. Eksplicitnih transakcije su izričito sastavljene razvojni inženjer pomoću `BEGIN TRAN`, `COMMIT TRAN` ili `ROLLBACK TRAN` i obično koriste se višestruke izjave o izmjenama moraju biti povezane zajedno u jednom jedinica atomske. 

Azure SQL Data Warehouse pamti promjene u bazi podataka pomoću zapisnika transakcije. Svaki raspodjele je vlastitu zapisnik transakcija. Zapisivanje zapisnik transakcija se automatski. Postoji bez konfiguracije obavezno. Međutim, whilst postupak jamčiti pisanja ga predstavljanje programa indirektni u sustavu. U ovom utjecaj rješenje smanjite pisanjem putem transakcije učinkovitog kod. Putem transakcije učinkovitog kod njih svim korisnicima Dopusti pada u dvije kategorije.

- Korištenje minimalnog zapisivanje konstrukta gdje je to moguće
- Postupak podataka pomoću opsegu serije da bi se izbjeglo jedinstven dugo izvodi transakcije
- Prihvaćaju particija promjene uzorka za velike izmjene navedeni partition

## <a name="minimal-vs-full-logging"></a>Minimalna nasuprot cijelog zapisivanje

Za razliku od potpuno prijavljenih operacije koje koriste zapisnik transakcija da biste pratili svake promjene retka, dodali prijavljenih operacije pratio opsegu alokacija i samo promjene meta-podataka. Stoga minimalnog zapisivanje uključuje zapisivanje informacije koje su potrebne za vraćanje transakcije u slučaju pogreške ili zahtjeva za razmjenu eksplicitnih (`ROLLBACK TRAN`). Kao što je mnogo manje prati informacije u zapisnik transakcija, dodali prijavljenih operacija izvodi bolje slične veličine potpuno prijavljenih operacija. Osim toga, jer manje zapisivanja se zapisnik transakcija, generirat ćete koliko manju količinu podaci iz zapisnika te se više/i učinkovito.

Ograničenja za sigurnost transakcije primjenjuju se samo na potpuno prijavljenih operacije.

>[AZURE.NOTE] Dodali prijavljenih operacije može sudjelovati u eksplicitnih transakcije. Kao što je sve promjene u dodijeljeni strukture prate, moguće je vratiti dodali prijavljenih operacije. Da biste shvatili da se promjene "dodali" prijavljen je važno je nije nepotvrđen prijavljenih.

## <a name="minimally-logged-operations"></a>Dodali prijavljenih operacije

Instaliranog dodali zapisuju su sljedeće postupke:

- Stvaranje TABLICE kao odaberite ([CTAS][])
- UMETANJE... ODABERITE
- STVARANJE INDEKSA
- IZVOĐENJE ALTER INDEKSA
- ODBACIVANJE INDEKSA
- IZREŽITE TABLICE
- ODBACIVANJE TABLICE
- ISKAZ ALTER TABLICE PROMJENU PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Operacije premještanja internih podataka (kao što su `BROADCAST` i `SHUFFLE`) sigurnost ograničenje transakcije ne utječe.

## <a name="minimal-logging-with-bulk-load"></a>Minimalna zapisivanje s skupno učitavanja

`CTAS`i `INSERT...SELECT` skupno operacije Učitaj. Međutim, oba utjecati definiciju tablice cilj i ovise o scenariju Učitaj. U nastavku je tablica koja objašnjava ako masovne operacije zapisat će se u potpunosti ili dodali:  

| Primarni indeksa               | Scenarij učitavanja                                            | Način prijave |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Skupova                        | Sve                                                      | **Minimalna**  |
| Indeks             | Prazan ciljne tablice                                       | **Minimalna**  |
| Indeks             | Učitavanje redaka preklopite s postojećih stranica u ciljnoj | **Minimalna**  |
| Indeks             | Preklapanje učitati retke s postojećih stranica u ciljnoj        | Potpuna         |
| Indeks Columnstore | Obrada veličina > = 102,400 po particija poravnat raspodjele | **Minimalna**  |
| Indeks Columnstore | Obrada veličina < 102,400 po particija poravnat raspodjele  | Potpuna         |

To vrijedi sudjelovanje sve zapisivanja da biste ažurirali sekundarnom ili nisu grupirani indeksi uvijek biti potpuno prijavljenih operacije.

> [AZURE.IMPORTANT] SQL Data Warehouse ima 60 distribucije. Dakle, uz pretpostavku da su svi reci ravnomjerno distributed i odredišne u jedna particija, seriju morat ćete sadrže 6,144,000 retke ili veća dodali biti prijavljeni prilikom pisanja Columnstore indeks s grupirani. Ako je particije u tablici, a redaka koji se umeće obuhvaćaju particija ograničenja, pa ćete 6,144,000 redaka po particija granicu pretpostavkom čak i distribucija podataka. Svaki particije u svakom raspodjele samostalno smije praga 102,400 redak za umetanje dodali biti prijavljeni u distribuciju.

Učitavanja podataka u tablicu koje nisu prazne pomoću grupni indeks često mogu sadržavati različite potpuno prijavljenih i dodali prijavljenih redaka. Grupni indeks je Balansirani stabla (b stabla) stranica. Ako na stranice koje se zapisuju već sadrži retke iz druge transakcije, zatim te zapisivanja zapisat će se u potpunosti. Međutim, ako je prazna stranica zatim pisanje na tu stranicu zapisat će se dodali.

## <a name="optimizing-deletes"></a>Optimiziranje briše

`DELETE`nije potpuno prijavljenih operacija.  Ako trebate izbrisati veliku količinu podataka u tablici ili particije, često smisla više `SELECT` podatke koje želite zadržati, koji mogu se izvoditi kao dodali prijavljenih operacija.  Da biste to postigli, stvorite novu tablicu s [CTAS][].  Nakon stvaranja, pomoću [Preimenovanje][] zamijeniti stari tablice novostvorenu tablicu.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimiziranje ažuriranja

`UPDATE`nije potpuno prijavljenih operacija.  Ako trebate ažurirati velikog broja redaka u tablici ili particija često može biti puno više učinkovitiji dodali prijavljenih operacija kao što su [CTAS][] da biste to učinili.

U primjeru niže cijelog tablice ažuriranje je pretvorena u `CTAS` tako da bude moguće minimalnog zapisivanje.

U ovom slučaju ne možemo retrospectively dodajete iznos popusta prodajom u tablici:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Ponovno stvaranje velike tablice su prednosti korištenja značajke upravljanja radno opterećenje SQL Data Warehouse. Dodatne informacije potražite u odjeljku Upravljanje radno opterećenje [istodobnosti][] članka.

## <a name="optimizing-with-partition-switching"></a>Optimiziranje s particije prijelaz

Kad se nađu velike skaliranje izmjene unutar [tablice particije][], particija promjene uzorka čini mnogo odgovara. Ako je izmjene podataka, a obuhvaća više particija, zatim jednostavno iterating putem particije postiže isti rezultat.

Slijede upute za izvođenje skretnice particije:
1. Stvaranje praznog više particija
2. Izvođenje "ažuriranje" kao na CTAS
3. Prijelaz izgleda s postojećim podacima izvan tablice
4. Prelazak na nove podatke
5. Čišćenje podataka

Međutim, da biste lakše prepoznali particije da biste se prebacili će najprije moramo sastavljanje Pomoćnik za postupak kao ono koje su ispod. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Ovaj postupak Maksimizira kod iskoristite i zadržava particija prijelaz primjer više sažetom.

Kod u nastavku prikazuje pet koraka spomenutih da biste postigli cijelog particija prijelaz Rutina.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Minimiziranje zapisivanje s manjim grupama

Za velike podatkovne operacije izmjene, je možda smisla podjele postupka u blokova ili serije se opseg skupne jedinice rad.

Dobrog primjera navedene u nastavku. Da biste istaknuli predviđa je postavljen veličinu serije trivial broj. U stvari veličinu serije bi znatno veću. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Zaustavljanje i skaliranja upute

Azure SQL Data Warehouse omogućuje zadržite pokazivač, životopis i promjena veličine skladištu podataka na zahtjev. Kada zaustavite ili promjena veličine SQL Data Warehouse važno je znati da su sve in-flight transakcije odmah; prekinut Uzrok sve otvorene transakcije za vraćen. Ako svoje radno opterećenje imali izda dugo pokrenut i nepotpuna podataka izmjene prije postupka zaustavljanje ili promjena veličine, zatim to funkcioniralo morat ćete moguće poništiti. To može utjecati na vrijeme potrebno za pauziranje ili promjena veličine Azure SQL Data Warehouse baze podataka. 

> [AZURE.IMPORTANT] Oba `UPDATE` i `DELETE` potpuno prijavljenih postupci i da ih poništiti/ponoviti poništeno operacije može potrajati znatno dulje nego je ekvivalent dodali prijavljeni operacije. 

Scenarij najboljeg je da biste omogućili u leta podataka izmjene transakcije dovršeno prije no što pauze ili mijenjanje veličine SQL Data Warehouse. No to uvijek možda nije praktično. U smanjenju rizika od dugo vraćanje uzmite u obzir jednu od sljedećih mogućnosti:

- Ponovno pisanje dugo izvodi postupke pomoću [CTAS][]
- Podijelite postupak prema dolje u blokova; na podskup redaka

## <a name="next-steps"></a>Daljnji koraci

U odjeljku [transakcija u SQL Data Warehouse][] da biste saznali više o razine odvajanja i transakcijskih ograničenja.  Pregled drugih najbolje prakse, potražite u članku [Najbolje prakse skladištu podataka SQL][].

<!--Image references-->

<!--Article references-->
[Transakcije u skladištu podataka za SQL]: ./sql-data-warehouse-develop-transactions.md
[particija tablice]: ./sql-data-warehouse-tables-partition.md
[Istodobnosti]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Najbolje prakse skladištu podataka SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[PREIMENOVANJE]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

