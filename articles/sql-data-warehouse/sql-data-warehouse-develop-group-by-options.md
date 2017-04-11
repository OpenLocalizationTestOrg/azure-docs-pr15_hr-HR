<properties
   pageTitle="Grupiraj po mogućnostima u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za implementaciju Grupiranje po mogućnostima u skladištu podataka za SQL Azure za razvoj rješenja."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="group-by-options-in-sql-data-warehouse"></a>Grupiranje prema mogućnosti u SQL Data Warehouse

Uvjet [GROUP BY][] koristi se za prikupljanje podataka sažetka skup redaka. Ima nekoliko mogućnosti koje se šire je funkcija koje su potrebne da biste radili oko kao što su Azure SQL Data Warehouse ne podržava izravno.

Te su mogućnosti
- GROUP BY s funkcijom ROLLUP
- SKUPOVI GRUPIRANJA
- GROUP BY s KOCKE

## <a name="rollup-and-grouping-sets-options"></a>Određivanje mogućnosti skupne vrijednosti i grupiranja
Mogućnost najjednostavniji ovdje je da biste koristili `UNION ALL` umjesto da biste izvršili u skupne vrijednosti umjesto potrebe za oslanjanjem na eksplicitnih sintakse. Rezultat je točno onaj

U nastavku je primjer grupe pomoću naredbe na `ROLLUP` mogućnost:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Pomoću funkcije ROLLUP ne možemo tražene sljedeće zbrajanja:
- Države i regije
- Države
- Sveukupni zbroj

Da biste zamijenili to morat ćete koristiti `UNION ALL`; Određivanje u zbrajanja obavezan izričito vraćaju iste rezultate:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Za GRUPIRANJE SKUPOVE sve moramo učinite je prihvaćaju isti glavni, ali samo stvaranje UNIJE svih sekcija za zbrajanje razine želimo da biste vidjeli

## <a name="cube-options"></a>Mogućnosti kocke
Nije moguće stvoriti GRUPU tako da s KOCKOM pomoću UNION ALL pristup. Problem je li kod brzo može postati zamorno i unwieldy. Da biste smanjili to možete koristiti sljedeće dodatne mogućnosti pristup.

Poslužimo se značajkom u primjeru iznad.

U prvi je korak da biste definirali 'kocke' koji definira sve razine zbrajanja koju želite stvoriti. Važno je da uzeti u obzir u UNAKRSNI uključivanje od dviju tablica izvedenih. Taj se način stvara sve razine za us. Ostatak kod postoji li zaista za oblikovanje.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Rezultati u CTAS mogu vidjeti ispod:

![][1]

Drugi korak je da biste odredili ciljnu tablicu pohrana privremeni rezultata:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Treći korak je Ponavljaj putem naš kocke stupaca izvođenje skupljanja u. Upit će se izvoditi na jednom za svaki redak u tablici privremene #Cube i spremiti rezultate u tablici temp #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Na kraju ćemo možete se vratiti rezultate tako samo čitanju #Results privremene tablice

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Prekidanje kod prema gore u sekcije i generiranje konstrukta petlje kod postaje moguće upravljati i maintainable.


## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[Pregled razvoj]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GRUPIRAJ PREMA]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
