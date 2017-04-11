<properties
   pageTitle="Stvaranje tablice označavanja (CTAS) u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za kodiranje s tablicom stvaranje kao iskaza select (CTAS) u skladištu podataka za SQL Azure za razvoj rješenja."
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

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Stvaranje tablice kao potvrdite (CTAS) u SQL Data Warehouse
Stvaranje tablice u obliku odaberite ili `CTAS` jedan je od najvažnijih T SQL značajke dostupne. Nije potpuno parallelized postupak koji stvara novu tablicu koja se temelji na rezultatu naredbe SELECT. `CTAS`je najjednostavnije i najbrži način stvoriti kopiju tablice. Smatrate da bi bio supercharged verziju `SELECT..INTO` ako želite. Ovaj dokument sadrži primjere i najbolje prakse za `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Upotreba CTAS za kopiranje tablice

Možda jedan od najčešće koristi `CTAS` stvara kopiju tablice tako da možete promijeniti na DDL. Ako, primjerice izvorno stvorio tablice u obliku `ROUND_ROBIN` , a sada ga želite promijeniti distribuirati u stupcu tablice `CTAS` je kako bi promjena stupca za raspodjelu. `CTAS`Također se poslužite da biste promijenili vrste particija, indeksiranje ili stupac.

Recimo da ste stvorili u ovoj su tablici pomoću zadane raspodjele vrste `ROUND_ROBIN` distributed jer nema stupca raspodjele navedenog u na `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Sada želite stvoriti novu kopiju u ovoj su tablici s Columnstore indeksom grupirani tako da možete iskoristiti prednost performanse grupirani Columnstore tablice. Također želite distribuirati u ovoj su tablici na ključ proizvoda jer su predviđanje spojevi u tom stupcu i želite da biste izbjegli premještanje podataka tijekom spojevi u ključ proizvoda. Na kraju i želite dodati particija OrderDateKey tako da možete brzo izbrisati stari podaci ispuštanje stare particije. Evo izjava CTAS koji želite kopirati stare tablice u novu tablicu.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Na kraju možete preimenovati tablice da biste zamijenili u novu tablicu i ispustite stare tablice.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL Data Warehouse ne još nisu podršku automatsko stvaranje i automatsko ažuriranje Statistika.  Da biste ostvarili najbolje performanse zatražite od upite, važno je da Statistika se mogu stvarati na sve stupce sve tablice nakon prve učitavanja ili znatno promjene se pojavljuju u podacima.  Detaljno objašnjenje Statistika u sintaksi [Statistika][] u grupi razvoju tema.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Korištenje CTAS da biste zaobišli nepodržane značajke

`CTAS`Također se poslužite da biste zaobišli brojne značajke koje nisu podržane navedena u nastavku. To možete često dokazali da je win/win situacija ne samo će kod biti usklađen, no on će često izvoditi brže na SQL Data Warehouse. Ovo je uslijed dizajn potpuno parallelized. Scenariji koji mogu biti radili oko CTAS obuhvaćaju sljedeće:

- ODABERITE... U
- ANSI SPOJEVA ažuriranja
- ANSI spojevi u briše
- Naredba za SPAJANJE

> [AZURE.NOTE] Pokušajte razmišljati "CTAS prvi". Ako mislite da možete riješiti problem pomoću `CTAS` pa to je obično najbolje je - postići čak i ako se kao rezultat pišete više podataka.
>

## <a name="selectinto"></a>ODABERITE... U
Ne mogu pronaći `SELECT..INTO` pojavit će se u broj mjesta u rješenje.

Primjer u nastavku je na `SELECT..INTO` izjava:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Da biste pretvorili iznad da biste `CTAS` je prilično klikom Proslijedi:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS trenutno potrebno navesti stupca raspodjele.  Ako ne namjerno pokušavate promjena stupca za raspodjelu sustava `CTAS` će izvesti na fastest Ako odaberete raspodjele stupac koji je isti kao tablica u podlozi kao što je ovaj strategije izbjegava premještanje podataka.  Ako stvarate malu tablicu gdje performansama nije faktor, a zatim možete odrediti `ROUND_ROBIN` da biste izbjegli odluči stupac distribuciju.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI spoja zamjena za ažuriranje izvješća

Može se dogoditi imaju složene ažuriranje koji spaja više od dviju tablica Suradnja pomoću ANSI uključivanja sintaksa za izvođenje ažuriranje i brisanje.

Zamislite morali ste ažurirali u ovoj su tablici:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Izvorni upit možda ste pokušavali otprilike ovako:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Budući da SQL Data Warehouse ne podržava ANSI spaja na `FROM` uvjet programa `UPDATE` naredbe, ne možete kopirati kod putem bez promjene je malo.

Koristite kombinaciju na `CTAS` i implicitno spoj da biste zamijenili kod:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI spoja zamjena za brisanje izvješća
Ponekad je najbolje koristiti za brisanje podataka da biste koristili `CTAS`. Umjesto jednostavnim brisanjem podataka odaberite podatke koje želite zadržati. U ovom posebno točno za `DELETE` naredbe koje koriste ansi spojno sintaksa jer SQL Data Warehouse ne podržava ANSI spaja u na `FROM` uvjet u `DELETE` izjava.

Primjer pretvorene naredba DELETE je dostupan ispod:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Zamjena naredbe za spajanje
Spajanje izvješća mogu zamijeniti, najmanje u dijelu pomoću `CTAS`. Možete ih konsolidirati u `INSERT` i `UPDATE` u jednom izjava. Sve izbrisane zapise morate zatvoriti isključivanje u druga naredba.

Primjer programa `UPSERT` dostupna ispod:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS preporuka: izričito stanja vrste podataka i nullability izlaza

Prilikom migracije kod možda će biti pokrenete preko ovu vrstu kodiranje uzorak:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinctively što mislite kod treba migrirati u na CTAS i bio ispravan. No postoji skrivene problem.

Sljedeći kod yield isti rezultat:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Obratite pozornost na to sadrži li stupac "rezultat" Naprijed vrsta i nullability vrijednosti podataka izraza. To se može dovesti do Neupadljiva varijanci vrijednosti ako niste oprezni.

Pokušajte sljedeće kao primjer:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Razlikuje vrijednošću koja je spremljena rezultata. Kao što je u druge izraze koji se koristi postojanog vrijednost u stupcu rezultat pogreška postaje još značajan.

![][1]

To je osobito važno za migracije podataka. Čak i ako je drugi upit arguably točnije došlo je do problema. Podaci će biti različiti u usporedbi s izvorišnog sustava, a koji vodi na pitanja integritet migraciju. To je jedna od tih rijetko slučajevima gdje se nalazi "pogrešan" odgovora zapravo pravom!

Razlog Vidimo ovaj disparity između dva rezultata je prema dolje do casting implicitno vrsta. U prvom primjeru tablici definira definicija stupca. Kada se umetne u retku programa implicitno vrsta pretvorbe. U drugi se primjer postoji bez pretvaranje implicitno vrste kao parametar expression definira vrstu podataka u stupcu. Imajte na umu i da stupca u drugi se primjer definirana je kao stupac koji dok je u prvom primjeru ima ne. U tablici stvaranja u prvi stupac nullability za primjer je izričito definirana. U drugi primjer ga je samo lijevo da biste izraz, a po zadanom to rezultat definiciju NULL.  

Za rješavanje tih problema morate izričito postaviti pretvorbi vrsta i nullability u na `SELECT` dio na `CTAS` izjava. U dijelu za stvaranje tablice ne možete postaviti tih svojstava.

U primjeru pokazuje kako ispraviti kod:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Imajte na umu sljedeće:
- PREVLAST ili Pretvori nije već iskorištene
- Da biste nametnuli NULLability ne SPOJENOG koristi se ISNULL
- ISNULL je krajnje vanjsko funkcija
- Drugi dio na ISNULL je konstanta odnosno 0

> [AZURE.NOTE] Za nullability pravilno postaviti je ključan pomoću `ISNULL` i ne `COALESCE`. `COALESCE`nije deterministic funkcija i pa rezultat izraza uvijek bit će NULLable. `ISNULL`razlikuje. To je deterministic. Stoga kada drugi dio na `ISNULL` funkcija je konstanta ili na literal tada će biti nastalu vrijednost nije NULL.

Ovo je savjet nije samo korisni za osiguravanje integriteta izračuni. Također važno je za prebacivanje particija tablice. Zamislite imate u ovoj su tablici definiran kao vaše fact:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Međutim, polje vrijednost je izraz izračunatog nije dio izvorišnih podataka.

Da biste stvorili particioniranom sa skupovima podataka možda ćete to učiniti:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Savršeno precizno želite izvoditi upit. Problem dolazi kada pokušate izvesti parametar particije. Definicija tablice se ne podudaraju. Da biste definicije tablici podudaraju s CTAS potrebno izmijeniti.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Vidjet ćete zato da vrsta dosljednost i održavanje nullability svojstva na CTAS inženjerskim najbolje dobro je. Olakšava održavanje cjelovitosti u svojim izračunima i osigurava da promjene particije moguće je.

Pogledajte MSDN dodatne informacije o korištenju [CTAS][]. To je jedan od najvažnijih izjava u skladištu podataka za SQL Azure. Provjerite je li temeljito razumjeti.

## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[Pregled razvoj]: sql-data-warehouse-overview-develop.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
