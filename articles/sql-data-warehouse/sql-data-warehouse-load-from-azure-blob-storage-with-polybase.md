<properties
   pageTitle="Podatke iz spremišta blobova platforme Azure učitali u SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Saznajte kako koristiti PolyBase da biste učitali podatke iz spremišta blobova platforme Azure u SQL Data Warehouse. Učitavanje nekoliko tablica iz javnih podataka u shemi Contoso maloprodaja Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Podatke iz spremišta blobova platforme Azure učitali u SQL Data Warehouse (PolyBase)

> [AZURE.SELECTOR]
- [Tvorničke podataka](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Da biste učitali podatke iz spremišta blobova platforme Azure u skladištu podataka za SQL Azure pomoću PolyBase i T SQL naredbe. 

Da biste zadržali jednostavne, ovog praktičnog vodiča učitava dvije tablice iz javnog Azure spremišta blobova platforme u shemi Contoso maloprodaja Data Warehouse. Da biste učitali cijeli skup podataka, pokrenite primjer [Učitavanje cijelog Data Warehouse maloprodaja Contoso][] u spremištu Microsoft SQL Server uzorka.

U ovom ćete praktičnom vodiču:

1. Konfiguriranje PolyBase da biste učitali iz spremišta blobova platforme Azure
2. Učitavanje javnih podataka u bazu podataka
3. Izvođenje optimizacije nakon završetka rada opterećenje.


## <a name="before-you-begin"></a>Prije početka
Da biste pokrenuli ovaj Praktični vodič, potreban vam je račun za Azure kojem je već instalirana bazom podataka SQL Data Warehouse. Ako nemate to, potražite u članku [Stvaranje SQL Data Warehouse][].

## <a name="1-configure-the-data-source"></a>1. konfiguriranje izvora podataka

PolyBase koristi vanjski objekata T-SQL da biste odredili mjesto i atribute vanjskih podataka. Definicija vanjskog objekt spremaju se u SQL Data Warehouse. Podaci sam spremljeni kao vanjski.

### <a name="11-create-a-credential"></a>1.1. Stvaranje vjerodajnice

**Preskoči ovaj korak** ako učitavate javnih podataka tvrtke Contoso. Budući da je dostupan svakome nije potrebno sigurnog pristupa javnih podataka

Ako koristite ovog praktičnog vodiča kao predložak za učitavanje vlastitim podacima, **ne preskočite ovaj korak** . Da biste pristupili podataka putem vjerodajnice, koristite sljedeću skriptu da biste stvorili vjerodajnice za bazu podataka u dosegu pa ga koristiti prilikom definiranja mjesto izvora podataka.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Prijeđite na drugi korak 2.

### <a name="12-create-the-external-data-source"></a>1.2. Stvaranje vanjskog izvora podataka

Koristite tu naredbu za [Stvaranje VANJSKOG IZVORA podataka][] za pohranu mjesto podataka i vrste podataka. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Ako odlučite objaviti blobova platforme azure potpisu, imajte na umu da vlasnik podataka koje naplatiti za podatke naknade za izlazne kada podataka napusti podatkovnog centra. 

## <a name="2-configure-data-format"></a>2. konfiguriranje oblik podataka

Podaci spremljeni u tekstne datoteke u spremište blobova platforme Azure, a svako polje je odvojite razdjelnika. Pokretanja naredbe [Stvaranje VANJSKIH OBLIK datoteke][] da biste odredili oblikovanje podataka u tekstne datoteke. Podacima tvrtke Contoso nije komprimiran i zarezom kanala.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. Stvorite vanjski tablice

Sad kad ste naveli oblik izvora i datoteku podataka, spremni ste za stvaranje vanjskog tablica. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Stvori shemu za podatke. 

Da biste stvorili mjesto za spremanje Contoso podataka u bazu podataka, stvoriti shemu.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3,2. Stvaranje vanjske tablica. 

Pokrenite ovaj skriptu da biste stvorili vanjski tablice dimenzije proizvoda i FactOnlineSales. Sve možemo rade ovdje je definiranje naziva stupaca i vrste podataka i njihovo povezivanje mjesto i format datoteke za pohranu blobova platforme Azure. Definicija pohranjena u SQL Data Warehouse, a podaci se još nalazi u spremište blobova Azure.

Parametar **mjesto** je mapa u odjeljku korijenske mape u spremište blobova Azure. Svaku tablicu je u neku drugu mapu.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. učitali podatke
Postoji drugi načini pristupa vanjskim podacima.  Možete tražiti podatke izravno iz vanjske tablice, učitali podatke u nove tablice u bazi podataka ili dodavanje vanjskih podataka u postojeće tablice baze podataka.  


### <a name="41-create-a-new-schema"></a>4.1. Stvaranje nove sheme

CTAS stvara novu tablicu koja sadrži podatke.  Prvo, stvorite shema za podacima tvrtke contoso.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Učitavanje podatke u nove tablice

Da biste učitali podatke iz spremišta blobova platforme Azure i spremite ga u tablicu unutar bazu podataka, koristite izjavu [STVORITE tablicu kao odaberite (Transact-SQL)][] . Učitavanje s CTAS upravlja svakako upisani vanjske tablice koje ste upravo stvorili. Da biste učitali podatke u nove tablice, koristite jedan [CTAS][] izjava po tablici. 

CTAS stvara novu tablicu, a popunjava s rezultatima naredbe select. CTAS definira novu tablicu da bi iste stupce i vrste podataka kao rezultati naredbe SELECT. Ako ste odabrali sve stupce iz vanjske tablice, novu tablicu bit će replike stupaca i vrste podataka u tablici vanjski.

U ovom primjeru ćemo stvoriti dimenziju i tablice činjenica kao raspršivanja raspodijeljeno tablice. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4,3 pratiti napredak učitavanja

Možete pratiti napredak vaše učitavanje pomoću prikaza dinamički management (DMVs). 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. optimiziranje columnstore spajanja

SQL Data Warehouse po zadanom sprema u tablici kao grupirani columnstore indeksa. Po završetku opterećenjem neke retke podataka možda neće se spojiti u na columnstore.  Postoji mnoštvo razloga zašto to se može dogoditi. Dodatne informacije potražite u članku [Upravljanje columnstore indeksi][].

Da biste optimizirali performanse upita i sažimanje columnstore nakon opterećenjem, ponovno stvaranje tablice da biste nametnuli indeks columnstore da biste spojili sve retke. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Dodatne informacije o zaštiti columnstore indeksi, potražite u članku [Upravljanje columnstore indeksi][] .

## <a name="6-optimize-statistics"></a>6. optimiziranje Statistika

Preporučuje se da biste stvorili jedan stupac Statistika odmah nakon opterećenjem. Postoji nekoliko mogućnosti za statistiku. Ako, na primjer, ako stvarate Statistika jedan stupac na svaki stupac ga može potrajati na ponovno stvaranje svih statističkih podataka. Ako znate da određene stupce se premještaju se u upitu predikati, možete preskočiti stvaranjem statističkih podataka u tim stupcima.

Ako odlučite da biste stvorili Statistika jedan stupac na svaki stupac svaku tablicu, možete koristiti uzorak koda pohranjena procedura `prc_sqldw_create_stats` u članku [Statistika][] .

U sljedećem primjeru je dobru početnu točku za stvaranje Statistika. Statistika jedan stupac stvara svakog stupca u tablici dimenzija, a zatim na svaki spojno stupac tablice činjenica. Uvijek možete dodati jedan ili više stupaca Statistika na ostale stupce tablice činjenica kasnije.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Postignuća otključana!

Uspješno je imati učitana javnih podataka u Azure SQL Data Warehouse. Odličan posao!

Sada možete započeti slanje upita tablica pomoću upita kao što je sljedeća:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Daljnji koraci
Da biste učitali svih Contoso maloprodaja Data Warehouse podataka, koristiti skripte u dodatne savjete za razvoj, potražite u članku [Pregled SQL Data Warehouse razvoj][].

<!--Image references-->

<!--Article references-->
[Stvaranje SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Pregled SQL Data Warehouse razvoj]: sql-data-warehouse-overview-develop.md
[Upravljanje columnstore indeksa]: sql-data-warehouse-tables-index.md
[Statistika]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[STVARANJE VANJSKOG IZVORA PODATAKA]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[STVARANJE VANJSKE DATOTEKE OBLIKA]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Stvaranje TABLICE kao odaberite (-SQL transakcija)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Učitavanje cijelog Data Warehouse maloprodaja Contoso]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
