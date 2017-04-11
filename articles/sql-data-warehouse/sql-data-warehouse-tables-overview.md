<properties
   pageTitle="Pregled tablica u SQL Data Warehouse | Microsoft Azure"
   description="Uvod u skladištu tablice s podacima sustava SQL Azure."
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
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Pregled tablica u SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled][]
- [Vrste podataka][]
- [Distribucija][]
- [Indeks][]
- [Particija][]
- [Statistika][]
- [Privremeni][]

Početak rada sa stvaranjem tablice u SQL Data Warehouse je jednostavno.  Sintaksa osnovni [CREATE TABLE][] slijedi sintakse za uobičajene najčešće već poznajete iz rad s drugim bazama podataka.  Da biste stvorili tablicu, jednostavno morate naziv tablice, naziva stupaca i definirati vrste podataka za svaki stupac.  Ako ste stvorili tablice u drugim bazama podataka, to trebala bi izgledati vrlo poznat.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Gornji primjer stvara tablicu pod nazivom Kupci s dva stupca, ime i prezime.  Svaki se stupac definiran vrstom podataka VARCHAR(25), ograničava podatke koje želite od 25 znakova.  Ta osnovna atributa tablice, kao i drugim korisnicima, uglavnom isti su kao druge baze podataka.  Vrste podataka za svaki stupac definiran i cilju integriteta podataka.  Da biste poboljšali performanse smanjivanjem/i moguće je dodavati indeksi.  Da biste poboljšali performanse kada je potrebno za izmjenu podataka mogu se dodati particija.

[Preimenovanje] [ RENAME] SQL Data Warehouse tablica izgleda ovako:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Raspodijeljeno tablice

Novi temeljne atribut uvedene raspodijeljeno sustavi kao što je SQL Data Warehouse je **stupac za raspodjelu**.  Stupac raspodjele je vrlo slično što je Zvuči kao.  Je stupac koji određuje kako distribuirati ili dijeljenje podataka u pozadini.  Prilikom stvaranja tablice bez navođenja raspodjele stupca u tablici automatski distribuira pomoću **kružnog**.  Iako tablice kružnog mogu biti potrebne u nekim slučajevima, definiranju raspodjele stupaca možete znatno smanjiti premještanje podataka tijekom upita, stoga optimiziranje performanse.  [Distribucija tablice][Raspodijeli] da biste saznali više o tome kako odabrati raspodjele stupca potražite u članku.

## <a name="indexing-and-partitioning-tables"></a>Indeksiranje i particija tablice

Kako postati dodatne mogućnosti korištenja SQL Data Warehouse i želite optimiziranja performansi, ćete htjeti dodatne informacije o dizajna tablice.  Dodatne informacije potražite u člancima na [Vrste podataka u tablici][Vrste podataka], [distribucija tablice][Raspodijeli], [Indeksiranje tablice][indeksa] i [particija tablice][particije].

## <a name="table-statistics"></a>Statistički podaci o tablice

Statistika važan je iznimno da biste postigli najbolje performanse iz SQL Data Warehouse.  Budući da SQL Data Warehouse ne još automatski stvara i ažuriranje Statistika umjesto vas, kao što je možda imaju dođete do očekivati u Azure SQL baze podataka, naš članak [Statistika][] za čitanje možda jedan od najvažnijih članaka čitati da biste bili sigurni da ćete dobiti najbolje performanse iz upita.

## <a name="temporary-tables"></a>Privremenih tablica

Privremene tablice su tablice koje samo postoji trajanje vaša prijava, a ne mogu vidjeti drugi korisnici.  Privremenih tablica može biti dobar način da biste spriječili druge da vide privremene rezultata i smanjiti potrebe za čišćenje diska.  Budući da privremenih tablica i koristiti lokalno spremište, nude brži rad za neke operacije.  Potražite u člancima [Privremena][privremene] dodatne pojedinosti o privremenih tablica.

## <a name="external-tables"></a>Vanjski tablice

Vanjski tablice, poznata i kao tablica Polybase su tablice koje možete mu iz SQL Data Warehouse, ali točke podataka vanjskih iz SQL Data Warehouse.  Na primjer, stvorite vanjsku tablicu koja upućuje na datotekama na spremište blobova platforme Azure.  Dodatne informacije o stvaranju i upit vanjska tablica potražite u članku [učitavanja podataka s Polybase][].  

## <a name="unsupported-table-features"></a>Nepodržane značajke tablica

Dok SQL Data Warehouse sadrži mnoge zajedničke značajke tablice koje nudi drugim bazama podataka, postoje neke značajke koje još nije podržana.  Slijedi popis nekih od značajke tablice koje još nije podržana.

| Nepodržane značajke |
| --- |
|[Svojstvo identiteta][] (pogledajte članak [Dodjela zaobilazno zamjenskih ključa][])|
|Primarni ključ, vanjski ključevi jedinstveni i potvrdite [Ograničenja tablice][]|
|[Jedinstvena kazala][]|
|[Izračunati stupci][]|
|[Kratke stupaca][]|
|[Korisnički definirana vrsta][]|
|[Niz][]|
|[Okidača][]|
|[Indeksiranim prikazima][]|
|[Sinonimi][]|

## <a name="table-size-queries"></a>Upiti veličine tablice

Jedan jednostavan način za prepoznavanje prostor i retke koji tablice u svakoj od 60 raspodjele je koristiti [DBCC PDW_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Međutim, pomoću naredbi DBCC može biti prilično ograničavanje.  Dinamični Upravljanje prikazima (DMVs) će vam omogućiti potražite u članku puno više detalja, kao i dati koliko veću kontrolu nad rezultatima upita.  Započnite stvaranjem ovaj prikaz koji će se upućuje mnogim oglednim primjerima u ovom i ostali članci.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Prostor tablicu sažetka

Ovaj upit vraća retke i razmak tablice.  To je odličan upita da biste vidjeli koje je tablice su najveću tablica i jesu li kružnog ili raspršivanje distribuirati.  Za raspršivanje distributed tablice također prikazuje stupcu distribuciju.  U većini slučajeva najveću tablica mora biti raspršivanje raspodijeljen grupirani columnstore indeksa.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Prostor tablice po vrsti distribuciju.

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Prostor tablice po vrsti indeksa

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Prostor raspodjele sažetka

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u člancima na [Vrste podataka u tablici][Vrste podataka], [distribucija tablice][Raspodijeli], [Indeksiranje tablice][indeks], [particija tablice][particije], [Zadržavanje tablice Statistika][Statistika] i [Privremenih tablica][privremene].  Dodatne informacije o najbolje prakse potražite u članku [Najbolje prakse skladištu podataka SQL][].

<!--Image references-->

<!--Article references-->
[Pregled]: ./sql-data-warehouse-tables-overview.md
[Vrste podataka]: ./sql-data-warehouse-tables-data-types.md
[Distribucija]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Particija]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Privremeni]: ./sql-data-warehouse-tables-temporary.md
[Najbolje prakse skladištu podataka SQL]: ./sql-data-warehouse-best-practices.md
[Učitavanje podataka s Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[STVARANJE TABLICE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Svojstvo identiteta]: https://msdn.microsoft.com/library/ms186775.aspx
[Dodjela zamjenskih ključa zaobilazno rješenje]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Ograničenja za tablice]: https://msdn.microsoft.com/library/ms188066.aspx
[Izračunati stupci]: https://msdn.microsoft.com/library/ms186241.aspx
[Kratke stupaca]: https://msdn.microsoft.com/library/cc280604.aspx
[Korisnički definirana vrsta]: https://msdn.microsoft.com/library/ms131694.aspx
[Niz]: https://msdn.microsoft.com/library/ff878091.aspx
[Okidača]: https://msdn.microsoft.com/library/ms189799.aspx
[Indeksiranim prikazima]: https://msdn.microsoft.com/library/ms191432.aspx
[Sinonimi]: https://msdn.microsoft.com/library/ms177544.aspx
[Jedinstvena kazala]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
