<properties
   pageTitle="Particija tablica u SQL Data Warehouse | Microsoft Azure"
   description="Uvod u tablice particija u skladištu podataka za SQL Azure."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Stvaranje particija tablica u SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled][]
- [Vrste podataka][]
- [Distribucija][]
- [Indeks][]
- [Particija][]
- [Statistika][]
- [Privremeni][]

Particija podržana za sve vrste tablice SQL Data Warehouse; obuhvaća grupirani columnstore, indeks i skupova.  Particija podržano je i na svim vrstama raspodjele, uključujući raspršivanje ili kružnog distribuirati.  Particija omogućuje vam podjele podataka u manjim grupama podataka i u većini slučajeva, koji su particija obavlja na stupac datuma.

## <a name="benefits-of-partitioning"></a>Prednosti particija

Particija može koristiti podatke performanse održavanja i upit.  Je li koristi oba ili samo jedan ovisno o tome kako učitavanja podataka i je li isti stupac mogu se koristiti oba svrhe jer particija mogu izvršiti samo jedan stupac.

### <a name="benefits-to-loads"></a>Prednosti opterećenje

Glavna prednost particija u SQL Data Warehouse je unaprijediti učinkovitost i performansi učitavanja podataka uz korištenje particija brisanja, prijelaz i spajanje.  U većini slučajeva podataka ima particije na stupca s datumima blisko povezana niz koji učitavanja podataka u bazu podataka.  Jedno od najveće prednosti korištenja particije održavati podatke je svrhu zapisivanje transakcije.  Dok je jednostavno umetanje, ažuriranje i brisanje podataka može biti najviše jednostavne pristup, s malo misli i trud, pomoću particija tijekom učitavanja značajno možete poboljšati performanse.

Prebacivanje particija se poslužite da biste brzo uklonili ili zamijenite dio tablice.  Tablice činjenica prodaje, na primjer, možda sadrže samo podatke za zadnjih 36 mjeseci.  Na kraju svakog mjeseca od najstarijeg mjesec podataka o prodaji izbrisati iz tablice.  Ti podaci se može izbrisati pomoću naredba delete da biste izbrisali podatke za najstarije mjesec.  Međutim, brisanje veliku količinu podataka redak po redak s naredba delete možete vrlo dugo trajati, kao i stvoriti rizik velike transakcije koji može potrajati za vraćanje ako nešto pošlo po redu.  Više optimalnih pristup je jednostavno ispustite najstarije particija podataka.  Gdje brisanje redaka za pojedinačne može potrajati sati, brisanjem cijele particije može potrajati sekundi.

### <a name="benefits-to-queries"></a>Prednosti upita

Particija također se poslužite da biste poboljšali performanse upita.  Ako upit primjenjuje filtar particioniranom stupac, to možete ograničiti pregled radi samo odgovarajući particije koje se mogu mnogo manje podskup podataka, izbjegavanje skeniranja cijelog tablice.  Uz Uvod u grupirani columnstore indeksi, performanse pogodnosti predikata uklanjanja su manje korisni, ali u nekim slučajevima može biti pogodnost u upite.  Na primjer, ako tablice činjenica prodaje se particije u 36 mjeseci pomoću polja datum prodaje, a zatim upiti filtar na datum prodaje možete preskočiti pretraživanje u particije koje ne podudaraju s filtrom.

## <a name="partition-sizing-guidance"></a>Upute za promjenu veličine partition

Dok particija može koristiti da biste poboljšali performanse u nekim slučajevima, stvorite tablicu s **previše** particije može usporavati performanse u određenim okolnostima.  Ove opasnosti istiniti osobito za grupirani columnstore tablice.  Za particija biti koristan, je važno da biste razumjeli kada koristiti particija i broj particije da biste stvorili.  Postoji nijedno teško brzo pravilo kao koliko su previše, što ovisi o podataka i koliko partitions koje su opterećenje istodobno.  No kao u Općenito pravilo palca, razmislite dodavanja 10s da biste 100s razdjeljivanja, ne 1000s.

Prilikom stvaranja particija **grupirani columnstore** tablicama, važno je uzeti u obzir redaka koliko će otvarati svaki particije.  Radi optimalnog sažimanje i performansi grupirani columnstore tablica, potrebno je najmanje 1 milijun redaka po raspodjele i particije.  Prije stvaranja particije SQL Data Warehouse već dijeli svaku tablicu u 60 raspodijeljeno baze podataka.  Bilo koji particija dodati u tablicu je uz distribucija stvorene u pozadini.  U ovom se primjeru koristi ako tablice činjenica prodaje nalazi 36 mjesečni particije i given da SQL Data Warehouse ima 60 distribucija, zatim tablice činjenica prodaje mora sadržavati 60 milijuna redaka mjesečno ili redaka 2.1 milijarde kad bili popunjeni sve mjesece.  Ako tablica sadrži znatno manje redaka od preporučene najmanji broj redaka po particije, preporučujemo da koristite manje particije da biste omogućili povećati broj redaka po particije.  Također u članku [Indeksiranje][indeks] koji obuhvaća upite koji se može pokrenuti na SQL Data Warehouse ocijeniti kvalitete klaster columnstore indeksi.

## <a name="syntax-difference-from-sql-server"></a>Sintaksa razlika iz sustava SQL Server

SQL Data Warehouse predstavlja pojednostavljeni definiciju razdjeljivanja koji je malo drugačiju iz sustava SQL Server.  Stvaranje particija funkcije i sheme se ne koriste u SQL Data Warehouse kao i u sustavu SQL Server.  Umjesto toga, sve što trebate napraviti je prepoznavanje particioniranom stupca, a točke granicu.  Dok je sintaksa particija može se malo razlikovati iz sustava SQL Server, osnovni koncepti su uvijek jednake.  Podrška za SQL Server i SQL Data Warehouse jedan stupac particija po tablici, što može biti ranged particije.  Dodatne informacije o stvaranju particija potražite u članku [particije tablica i indeksa][].

U sljedećem primjeru naredbe SQL Data Warehouse particije [Stvaranje TABLICE][] partitions tablici FactInternetSales OrderDateKey stupac:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migracija particija iz sustava SQL Server

Da biste jednostavno migrirati definicije particija SQL Server SQL Data Warehouse:

- Uklanjanje sustava SQL Server [particija shemu][].
- Dodajte definiciju [funkcija partition][] CREATE TABLE.

Ako premještate particioniranom tablice iz instancu komponente SQL Server na ispod SQL može vam pomoći da interrogate broj redaka koji su u svakom particije.  Imajte na umu ako na SQL Data Warehouse koristi iste stvaranje particija preciznosti, broj redaka po particija će smanjiti faktorom od 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Upravljanje radno opterećenje

Jedan konačni informaciju razmotriti za faktor da biste particija odluka tablice je [radno opterećenje upravljanje][].  Upravljanje radno opterećenje u SQL Data Warehouse je uglavnom upravljanje memorije i istodobnosti.  U SQL Data Warehouse maksimalnog zauzeća memorije dodijeliti svakom raspodjele tijekom izvršavanja upita je klasa mjerodavni resursa.  Najbolje vaše particije će veličine in consideration of Ostali čimbenici kao što su potrebe memorije izgradnje grupirani columnstore indeksi.  Grupirani columnstore indeksi pogodnost znatno kada ih se dodijeliti dodatnu memoriju.  Stoga želite biti sigurni da se na izvođenje particija indeksa nije starved memorije. Povećavanjem memorije u upit možete postići prebacivanjem iz zadane uloge, smallrc, jednu od druge uloge kao što su largerc.

Informacije o dodjelu memorije po raspodjele dostupna postavljanjem upita prikaza za dinamičku upravljanja governor resursa. U stvari vaše Dodjela memorije bit će manji od slika u nastavku. Međutim, omogućuje razine upute koje možete koristiti kada vaš particije za postupke za upravljanje podacima za promjenu veličine.  Pokušajte izbjeći vaše particije izvan Dodjela memorije nudi predmete vrlo velike resursa za promjenu veličine. Ako vaš particije Povećaj izvan slika pokrenete rizika tlaka memorije koji shodno potencijalnih klijenata za manje optimalne spajanja.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Prebacivanje partition

SQL Data Warehouse podržava particija podjele, spajanje i promjene. Svaka od tih funkcija je excuted pomoću iskaz [ALTER TABLE][] .

Da biste se prebacili particije između dviju tablica osigurati da particije poravnati na njihove odgovarajući ograničenja i da odgovara definicije tablice. Kao što je check ograničenja nisu dostupne za nametanje raspon vrijednosti u tablici u izvorišnu tablicu mora sadržavati granice particija isti kao u ciljnu tablicu. Ako to nije tako, parametar particija neće uspjeti kao metapodataka particija se neće sinkronizirati.

### <a name="how-to-split-a-partition-that-contains-data"></a>Kako se dijele particije koja sadrži podatke

Je najučinkovitiji način Podjela particije koji već sadrži podatke za korištenje na `CTAS` izjava. Ako je tablica s particioniranom grupirani columnstore onda particija tablice mora biti prazna prije nego što se može se podijeliti.

Slijedi primjer particioniranom columnstore tablice koja sadrži jedan redak u svakom particija:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Stvaranjem objekta statistike smo provjerite je li tu tablicu metapodataka točnije. Izostavite smo stvaranje Statistika, SQL Data Warehouse će koristiti zadane vrijednosti. Pojedinosti o Statistika pregledajte [Statistika][].

Ne možemo možete zatim upita za korištenje brojanje redaka u `sys.partitions` kataloga prikaza:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Ako pokušate ćemo podijeliti u ovoj su tablici, ne možemo će se pogreška:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Poruka 35346 15 razinu stanje 1, PODJELA 44 u retku uvjet naredba ALTER PARTICIJA nije uspjelo jer particija nije prazan.  Prazan particije može se podijeliti u kada postoji columnstore indeksa u tablici. Razmislite o onemogućivanje indeks columnstore prije izdavanja iskaz ALTER PARTICIJE, a zatim ponovno stvaranje indeksa columnstore nakon dovršetka ALTER PARTICIJE.

Međutim, možete koristiti `CTAS` da biste stvorili novu tablicu za naše podatke.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Kao što su poravnati granice particija skretnice je dopušteno. To će ostavite izvorišnu tablicu s prazan particije koji ne možemo naknadnog možete razdvojiti.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Sve što je preostalo je da biste poravnali naš podatke za novi granice particija pomoću `CTAS` i promjenu oglednim podacima u natrag na glavnu tablicu

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Nakon što ste dovršili premještanje podataka je dobro osvježavanje statističke podatke u ciljnu tablicu da bi se ispravno odražavaju nove distribucije skupa podataka u svoje odgovarajući particije:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tablica particija kontrola izvora

Da biste izbjegli definicije tablice iz **Rđa** u izvorišnom sustavu za kontrolu trebali uzeti u obzir sljedeće pristup:

1. Stvorite tablicu kao particioniranom tablicu, ali ne vrijednostima partition

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`Tablica u sklopu postupka implementacije:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Na taj se način ostaje statične kod u kontroli izvorišnog i stvaranje particija vrijednosti granicu dopušteno se dinamički; razvoj s skladištu tijekom vremena.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u člancima na [Pregled tablica][Pregled], [Vrste podataka u tablici][Vrste podataka], [distribucija tablice][Raspodijeli], [Indeksiranje tablice][indeks], [Zadržavanje tablice Statistika][Statistika] i [Privremenih tablica][privremene].  Dodatne informacije o najbolje prakse potražite u članku [Najbolje prakse skladištu podataka SQL][].

<!--Image references-->

<!--Article references-->
[Pregled]: ./sql-data-warehouse-tables-overview.md
[Vrste podataka]: ./sql-data-warehouse-tables-data-types.md
[Distribucija]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Particija]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Privremeni]: ./sql-data-warehouse-tables-temporary.md
[Upravljanje radno opterećenje]: ./sql-data-warehouse-develop-concurrency.md
[Najbolje prakse skladištu podataka SQL]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Particioniranom tablica i indeksa]: https://msdn.microsoft.com/library/ms190787.aspx
[ISKAZ ALTER TABLICE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[STVARANJE TABLICE]: https://msdn.microsoft.com/library/mt203953.aspx
[Funkcija Partition]: https://msdn.microsoft.com/library/ms187802.aspx
[particija shemu]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
