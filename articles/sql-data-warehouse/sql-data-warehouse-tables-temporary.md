<properties
   pageTitle="Privremena tablica u SQL Data Warehouse | Microsoft Azure"
   description="Početak rada s privremenih tablica u skladištu podataka za SQL Azure."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Privremena tablica u SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled][]
- [Vrste podataka][]
- [Distribucija][]
- [Indeks][]
- [Particija][]
- [Statistika][]
- [Privremeni][]

Privremenih tablica vrlo su korisne prilikom obrade podataka – osobito tijekom transformacije gdje su tranzitne Srednja rezultate. U SQL Data Warehouse privremenih tablica postoji na razini sesiju.  Samo su vidljive sesije u kojoj su stvorene i automatski ispuštaju se prilikom te sesije Odjavi.  Privremenih tablica nude performanse pogodnost jer njihove rezultate zapisuju lokalnim umjesto udaljenog spremišta.  Privremene tablice su malo razlikovati u skladištu podataka za SQL Azure od baze podataka SQL Azure kao što su možete pristupiti iz bilo gdje unutar sesije, uključujući unutar i izvan nje omogućiti pohranjena procedura.

U ovom se članku sadrži ključne smjernica za korištenje privremenih tablica i ističe načela sesiju privremene tablice na razini. Pomoću informacija u ovom se članku omogućuju modularize kodu i poboljšanje reusability i olakšani održavanja koda.

## <a name="create-a-temporary-table"></a>Stvaranje privremene tablice

Privremenih tablica stvaraju se jednostavno dodavanje prefiksa naziv tablice s na `#`.  Ako, na primjer:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Privremenih tablica mogu se kreirati i pomoću programa `CTAS` pomoću na isti način:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`vrlo naprednih naredbi i imaju dodane prednost vrlo učinkovitog u njezino korištenje prostora zapisnik transakcija. 


## <a name="dropping-temporary-tables"></a>Odbacivanje privremenih tablica

Prilikom stvaranja nove sesije nema privremenih tablica trebala bi postojati.  Međutim, ako zovete isti pohranjena procedura, čime se privremenom s istim nazivom kako bi vaše `CREATE TABLE` izjave su uspješno jednostavne potvrdite stara postojanje s na `DROP` mogu se koristiti kao u u sljedećem primjeru:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Za kodiranje dosljednost je dobro koristiti ovaj uzorak za tablice i privremene tablice.  Preporučuje se i dobro je koristiti `DROP TABLE` da biste uklonili privremene tablice nakon što završite s njima u kodu.  U pohranjena procedura razvoja je vrlo uobičajeni da biste vidjeli naredbe padajuće u vezanoj instalaciji zajedno na kraju postupka da biste bili sigurni tih objekata očistiti.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kod

Budući da privremenih tablica mogu vidjeti na bilo koje mjesto u sesiji korisnika, to možete exploited da biste lakše modularize kodu aplikacije.  Ako, na primjer, pohranjena procedura ispod objedinjuje preporučeni Načini rada odozgo da biste generirali DDL koji će se ažurirati sve statističkih podataka u bazi podataka prema nazivu statistike.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

U ovoj fazi samo akciju koja se pojavila se stvaranje pohranjena procedura koju ćete jednostavno generira privremena, #stats_ddl, s DDL naredbe.  U ovom pohranjena procedura će odbaciti #stats_ddl ako već postoji da biste bili sigurni da neće uspjeti ako pokrenuti više puta unutar sesije.  No Budući da je bez `DROP TABLE` na kraju pohranjena procedura kada pohranjena procedura dovrši, ga ostavlja stvorili tablicu tako da ga možete pročitati izvan pohranjena procedura.  U SQL Data Warehouse za razliku od drugih baza podataka sustava SQL Server nije moguće koristiti privremena izvan postupak u kojem je stvorena.  SQL Data Warehouse privremenih tablica može biti ispunjene **bilo gdje** unutar sesiju. To može dovesti do više modularan i moguće upravljati kod u kao u sljedećem primjeru:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Privremena ograničenja

SQL Data Warehouse nametnuti nekoliko ograničenja prilikom implementacije privremenih tablica.  Trenutno samo sesiju opsegu privremene tablice su podržane.  Globalni privremene tablice nisu podržane.  Osim toga, prikaza nije moguće stvoriti na privremenih tablica.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u člancima na [Pregled tablica][Pregled], [Vrste podataka u tablici][Vrste podataka], [distribucija tablice][Raspodijeli], [Indeksiranje tablice][indeks], [particija tablice][particija] i [Održavanje Statistika tablice][Statistika].  Dodatne informacije o najbolje prakse potražite u članku [Najbolje prakse skladištu podataka SQL][].

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

<!--MSDN references-->

<!--Other Web references-->
