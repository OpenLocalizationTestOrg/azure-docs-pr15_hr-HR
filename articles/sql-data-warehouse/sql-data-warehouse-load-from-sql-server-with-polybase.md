<properties
   pageTitle="Učitavanje podataka iz sustava SQL Server u Azure SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Koristi bcp za izvoz podataka iz sustava SQL Server u plošnu datoteka, AZCopy da biste uvezli podatke u spremište blobova platforme Azure i PolyBase za ingest podatke u Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Učitavanje podataka s PolyBase u skladištu podataka za SQL

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Pomoću ovog praktičnog vodiča pokazuje kako podatke učitali u SQL Data Warehouse pomoću AzCopy i PolyBase. Kada završite, znat ćete kako:

- Korištenje AzCopy da biste kopirali podatke blobova platforme Azure
- Stvaranje objekata baze podataka za definiranje podataka
- Pokretanje T SQL upita da biste učitali podatke

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Preduvjeti

Da biste se pomicali kroz ovaj Praktični vodič, morate

- Baze podataka SQL Data Warehouse.
- Račun Azure prostora za pohranu vrstu standardne lokalno suvišnih prostora za pohranu (standardna-LRS) standardni zemlj Redundant prostora za pohranu (standardna GRS) ili standardni pristup za čitanje zemlj Redundant prostora za pohranu (standardna RAGRS).
- AzCopy pomoćnog programa naredbenog retka. Preuzmite i instalirajte [najnoviju verziju AzCopy][] koji se instalira uz Alati za pohranu sustava Microsoft Azure.

    ![Alati za Azure prostora za pohranu](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Korak 1: Dodavanje oglednih podataka za spremište blobova platforme Azure

Da biste učitali podatke, moramo postavljanje oglednih podataka u Azure blobova. U ovom ćete koraku ne možemo popuniti Azure spremište blobova platforme s oglednim podacima. Kasnije, koristit ćemo PolyBase da biste učitali ove ogledne podatke u bazu podataka sustava SQL Data Warehouse.

### <a name="a-prepare-a-sample-text-file"></a>ODGOVORI. Priprema oglednih tekstne datoteke

Priprema oglednih tekstne datoteke:

1. Otvorite Blok za pisanje, a zatim kopirajte sljedeće retke podataka u novu datoteku. Spremite to lokalni temp direktorij kao % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Pronađite svoje krajnja točka servisa blobova platforme

Da biste pronašli svoje krajnja točka servisa blob:

1. Na portalu Azure odaberite **Pregledaj** > **Račune za pohranu**.
2. Kliknite račun za pohranu koji želite koristiti.
3. U računu plohu prostora za pohranu kliknite blob-ova

    ![Kliknite blob-ova](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Spremanje URL krajnje točke servisa blobova platforme za kasnije.

    ![Krajnja točka servisa blobova platforme](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Pronalaženje ključa Azure prostora za pohranu

Da biste pronašli ključ Azure prostora za pohranu:

1. Portal za Azure odaberite **Pregledaj** > **Račune za pohranu**.
2. Kliknite na računu za pohranu koji želite koristiti.
3. Odaberite **sve postavke** > **pristupnih tipki**.
4. Kliknite okvir Kopiraj na jedan od pristupnih tipki kopiranje u međuspremnik.

    ![Kopiranje ključa za pohranu za Azure](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Kopirajte ogledne datoteke blobova platforme Azure

Da biste kopirali podatke blobova platforme Azure:

1. Otvorite naredbeni redak, a zatim promijenite direktorija u direktoriju AzCopy instalaciju. Ta naredba mijenja se u zadanom imeniku instalacije na 64-bitni Windows klijent.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Pokrenite sljedeću naredbu da biste prenijeli datoteku. Navedite blob servisa krajnjoj točki URL za <blob service endpoint URL> i ključ za račun Azure prostora za pohranu za < azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Pogledajte i [Početak rada s AzCopy naredbenog retka uslužni][najnoviju verziju AzCopy].

### <a name="e-explore-your-blob-storage-container"></a>E. Istražite vaše spremnik blob prostora za pohranu

Da biste vidjeli datoteku prenijeli sa spremištem blobova:

1. Vratite se na vaše plohu Blob servisa.
2. U odjeljku spremnika, dvokliknite **datacontainer**.
3. Da biste istražili put vašim podacima, kliknite mapu **datedimension** i vidjet ćete prenesenih datoteka **DimDate2.txt**.
4. Da biste pogledali svojstva kliknite **DimDate2.txt**.
5. Imajte na umu da u svojstva plohu Blob možete preuzeti ili izbrišite datoteku.

    ![Prikaz Azure spremišta blobova](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Korak 2: Stvaranje vanjska tablica za uzorak podataka

U ovom odjeljku ćemo stvoriti vanjski tablice koji definira ogledne podatke.

PolyBase koristi vanjski tablice za pristup podacima u spremište blobova platforme Azure. Budući da se podaci ne spremaju unutar SQL Data Warehouse, PolyBase rukuje provjere autentičnosti s vanjskim podacima pomoću vjerodajnica za servis baze podataka.

Primjer u ovom ćete koraku koristi te Transact-SQL naredbe za stvaranje vanjska tablica.

- [Stvorite glavni ključ (Transact-SQL)][] da biste šifrirali tajna bazu podataka iz djelokruga vjerodajnica.
- [Stvaranje baze podataka iz djelokruga vjerodajnica (Transact-SQL)][] da biste odredili podatke za provjeru autentičnosti za vaš račun za Azure prostora za pohranu.
- [Stvaranje vanjskog izvora podataka (Transact-SQL)][] da biste odredili mjesto spremišta blobova platforme Azure.
- [Stvaranje vanjske oblik datoteke (Transact-SQL)][] da biste odredili oblik podataka.
- [Stvaranje vanjske tablice (Transact-SQL)][] da biste odredili definicija tablice i mjesto podataka.

Pokrenite ovaj upit bazi podataka sustava SQL Data Warehouse. Stvorit će vanjsku tablicu pod nazivom DimDate2External u shemi vlasnika baze podataka koja upućuje na DimDate2.txt ogledne podatke iz spremišta blobova platforme Azure.


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


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


U programu Explorer objekta SQL Server u Visual Studio, vidjet ćete u vanjskom formatu datoteke, vanjski izvor podataka i tablicu DimDate2External.

![Prikaz vanjskim tablice](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Korak 3: Podatke učitali u SQL Data Warehouse

Nakon stvaranja vanjske tablice možete učitali podatke u novu tablicu ili umetnite u postojećoj tablici.

- Da biste učitali podatke u novu tablicu, pokrenite naredbu za [Stvaranje TABLICE kao odaberite (Transact-SQL)][] . Nova tablica će imati stupaca imenovan u upitu. Vrste podataka stupaca će odgovarati vrste podataka u definiciji vanjska tablica.
- Da biste učitali podatke u postojećoj tablici, koristite [Umetanje... Odabir (Transact-SQL)][] izjava.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Korak 4: Stvaranje Statistika na upravo učitavanja podataka

SQL Data Warehouse ne automatsko stvaranje ili ažuriranje automatske statistike. Dakle, da biste postigli performanse visoke upita, važno je da biste stvorili Statistika na svaki se stupac argumenta svaku tablicu nakon prve Učitaj. Također važno je da biste ažurirali Statistika nakon znatno promjene u podacima.

U ovom se primjeru stvara Statistika jednog stupca u novu tablicu DimDate2.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Dodatne informacije potražite u članku [Statistika][].  


## <a name="next-steps"></a>Daljnji koraci
Potražite u [vodiču PolyBase][] dodatne informacije koje se moraju znati kako razvijati rješenja koja ih koriste PolyBase.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Vodič za PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[najnoviju verziju AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[Stvaranje VANJSKOG IZVORA podataka (-SQL transakcija)]:https://msdn.microsoft.com/library/dn935022.aspx
[Stvaranje VANJSKE datoteke OBLIKA (-SQL transakcija)]:https://msdn.microsoft.com/library/dn935026.aspx
[Stvaranje VANJSKE TABLICE (-SQL transakcija)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Stvaranje TABLICE kao odaberite (-SQL transakcija)]:https://msdn.microsoft.com/library/mt204041.aspx
[UMETANJE... Odabir (-SQL transakcija)]:https://msdn.microsoft.com/library/ms174335.aspx
[STVORITE GLAVNI KLJUČ (-SQL transakcija)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[Stvaranje baze podataka iz DJELOKRUGA VJERODAJNICA (-SQL transakcija)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
