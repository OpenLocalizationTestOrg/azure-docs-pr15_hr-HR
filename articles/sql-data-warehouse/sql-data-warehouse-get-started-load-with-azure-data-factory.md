<properties
   pageTitle="Učitavanje podataka s Azure podataka tvorničke | Microsoft Azure"
   description="Saznajte kako učitavanje podataka s tvorničke Azure podataka"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Učitavanje podataka s tvorničke Azure podataka 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Tvorničke podataka](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

Pomoću ovog praktičnog vodiča prikazuje kako stvoriti na kanal u Azure tvorničke podataka da biste premjestili sadržaj iz Azure spremišta blobova Azure SQL Data Warehouse. Sa sljedećim koracima ćete:

+ Postavljanje ogledne podatke iz programa Azure spremišta blobova.
+ Povezivanje resursi tvorničke Azure podataka.
+ Stvorite kanal za premještanje podataka s blob-ova za pohranu na SQL Data Warehouse.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Prije početka

Upoznajte se s tvorničke Azure podataka, potražite u članku [Uvod u tvorničke Azure podataka][].

### <a name="create-or-identify-resources"></a>Stvaranje i prepoznavanje resursa

Prije pokretanja ovog praktičnog vodiča, morate koristiti u sljedećim resursima:

   + **Azure spremišta blobova**: ovog praktičnog vodiča koristi Azure spremišta blobova kao izvor podataka za kanal tvorničke Azure podataka i tako morate znači da je dostupan za pohranu ogledne podatke. Ako ga nemate već, Saznajte kako [Stvaranje računa za pohranu][].

   + **SQL Data Warehouse**: ovog praktičnog vodiča premješta podatke iz blobova platforme Azure prostora za pohranu u SQL Data Warehouse i tako moraju imati s podacima skladištu online koja je učitana s oglednim podacima AdventureWorksDW. Ako već nemate podataka skladištu, Saznajte kako [Dodjela nešto][Create a SQL Data Warehouse]. Ako imate podatke skladištu, ali niste Dodjela s oglednim podacima, možete [ga ručno učitati][Load sample data into SQL Data Warehouse].

   + **Azure podataka tvorničke**: Azure podataka tvorničke dovršava stvarni opterećenja i tako morate imati jedan koje možete koristiti da biste sastavili kanal za premještanje podataka. Ako ne postoji već, Saznajte kako stvoriti u koraku 1 [Početak rada s tvorničke Azure podataka (Uređivač tvorničke podataka)][].

   + **AZCopy**: morate AZCopy da biste kopirali ogledne podatke iz lokalno klijentsko vaše Azure spremišta blobova. Upute za instalaciju potražite u [dokumentaciji AZCopy][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Korak 1: Kopirajte ogledne podatke blobova platforme Azure prostora za pohranu

Nakon što dodate sve dijelove spreman, spremni ste za vaše Azure spremišta blobova kopirajte ogledne podatke.

1. [Preuzimanje oglednih podataka][]. Ove podatke dodaje drugi tri godine s podacima o prodaji AdventureWorksDW oglednim podacima.

2. Upotrijebite tu naredbu AZCopy da biste kopirali tri godine podataka na Azure spremišta blobova.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Korak 2: Povezati resursi tvorničke Azure podataka

Sad kad se podaci nalaze u mjesto možemo stvoriti kanal tvorničke Azure podataka na članak premještanje podataka iz spremišta blobova platforme Azure SQL Data Warehouse.

Početak rada, otvorite [portal za Azure][] i na lijevom izborniku odaberite tvorničke vaše podatke.

### <a name="step-21-create-linked-service"></a>2.1 korak: Stvaranje povezanih servisa

Račun za Azure prostora za pohranu i SQL Data Warehouse veza na tvorničke vaše podatke.  

1. Najprije započnite postupak registracije tako da kliknete u odjeljku 'Povezani servisi' na tvorničke podataka, a zatim kliknite "pohranite novih podataka." Odaberite ime da biste registrirali azure prostora za pohranu u odjeljku Odabir Azure prostora za pohranu kao vrstu, a zatim unesite naziv računa i ključ za račun.

2. Da biste registrirali SQL Data Warehouse pronađite odjeljak "Autor i uvođenja", odaberite "Novo spremišta podataka", a zatim "Azure SQL Data Warehouse". Kopiranje i lijepljenje u ovaj predložak, a zatim unesite u određenih informacija.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>2.2 korak: Definiranje skup podataka

Kada stvorite povezani servisi, imamo definiranje skupa podataka.  Ovdje to znači da koji definira strukturu podataka koji je unosa premještena s prostora za pohranu na skladištu podataka.  Dodatne informacije o stvaranju

1. Da biste pokrenuli taj proces tako da odete do odjeljka "Autor i uvođenja" na tvorničke podataka.

2. Kliknite "Novi skup podataka", a zatim "Azure blobova" da biste se povezali prostora za pohranu na tvorničke podataka.  Možete koristiti u nastavku skripta za definiranje podataka u spremište blobova platforme Azure:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```

3. Sada ćemo definirati naš skup podataka za SQL Data Warehouse. Ne možemo pokrenuti na isti način tako da kliknete "Nova dataset", a zatim "Azure SQL Data Warehouse".

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Korak 3: Stvaranje i pokretanje na kanal

Na kraju, postavljanje i pokretanje kanal u tvorničke Azure podataka.  Ovo je operacija koja izvršava premještanja stvarnih podataka.  Možete pronaći cijeli prikaz postupaka koje možete izvršiti pomoću SQL Data Warehouse i Azure podataka tvorničke [ovdje][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

U odjeljku "Autor i uvođenja" kliknite "Više naredbi", a zatim "Novi kanal".  Kada stvorite kanal, možete koristiti u ispod kod da biste prenijeli podatke u skladištu podataka:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više, najprije prikaz:

- [Azure podataka tvorničke tečaj][].
- [Podataka azure SQL skladištu poveznik][]. Ovo je tema referenca core za Azure podataka tvorničke pomoću Azure SQL Data Warehouse.


Ove teme pružaju podrobne informacije o tvorničke Azure podataka. Oni raspravljati baze podataka SQL Azure ili HDInsight, ali se informacije odnose i na Azure SQL Data Warehouse.

- [Praktični vodič: početak rada s Azure podataka tvorničke][] Ovo je vodič core za obradu podataka pomoću tvorničke Azure podataka. U ovom ćete praktičnom vodiču će izgraditi vaš prvi kanal koji koristi HDInsight transformacija i analizirajte web zapisnika mjesec. Imajte na umu postoji bez aktivnosti kopiju ovog praktičnog vodiča.
- [Praktični vodič: kopiranje podataka iz Azure spremišta blobova s bazom podataka SQL Azure][]. U ovom ćete praktičnom vodiču stvorite kanal u Azure tvorničke podataka da biste kopirali podatke iz Azure spremišta blobova s bazom podataka SQL Azure.

<!--Image references-->

<!--Article references-->
[Dokumentacija AZCopy]: ../storage/storage-use-azcopy.md
[Poveznik za skladištu podataka Azure SQL]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Stvaranje računa za pohranu]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Početak rada s tvorničke Azure podataka (Uređivač tvorničke podataka)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Uvod u tvorničke Azure podataka]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Praktični vodič: Kopiranje podataka iz Azure spremišta blobova s bazom podataka SQL Azure]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Praktični vodič: Početak rada s tvorničke Azure podataka]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure podataka tvorničke tečaj]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Portal za Azure]: https://portal.azure.com
[Preuzimanje oglednih podataka]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
