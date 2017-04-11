<properties 
    pageTitle="Premještanje podataka da biste/iz baze podataka SQL Azure | Microsoft Azure" 
    description="Upute za premještanje podataka iz baze podataka SQL Azure pomoću tvorničke Azure podataka." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Premještanje podataka i s bazom podataka SQL Azure pomoću tvorničke Azure podataka
U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz baze podataka SQL Azure s drugom spremišta podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine. 

## <a name="supported-sources-and-sinks"></a>Podržani izvori i primatelji
Pogledajte tablicu [služi za pohranu podataka podržani](data-factory-data-movement-activities.md#supported-data-stores-and-formats) popis spremišta podataka kao izvora ili primatelji podržava aktivnosti Kopiraj. Podatke iz bilo kojeg podržanih izvora podataka iz trgovine s bazom podataka SQL Azure ili iz baze podataka SQL Azure možete pomaknuti na bilo koji podržani primatelj spremišta podataka.

## <a name="create-pipeline"></a>Stvaranje kanala
Možete stvoriti na kanal s Kopiraj aktivnosti koje premješta podatke iz baze podataka Azure SQL pomoću različitih Alati/API-ji.  

- Kopiranje čarobnjaka
- Portal za Azure
- Visual Studio
- Azure PowerShell
- .NET API-JA
- REST API-JA

Detaljne upute za stvaranje na kanal s Kopiraj aktivnosti na različite načine potražite u članku [vodič aktivnosti Kopiraj](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) .   

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanal koja se kopira podatke iz baze podataka SQL Azure najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

Sljedeći primjeri sadrže definicije JSON uzorka koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke i s bazom podataka SQL Azure i spremište blobova platforme Azure. Međutim, podaci mogu biti kopirana **izravno** iz bilo kojeg od izvora na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Primjer: Kopiranje podataka iz baze podataka SQL Azure blobova platforme Azure

Isti se definira sljedeće entiteti tvorničke podataka:

1. Povezane servis vrste [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Za unos [dataset](data-factory-create-datasets.md) vrste [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [SqlSource](#azure-sql-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopira vremenski niz podataka (hourly, dnevno, itd.) iz tablice u bazi podataka Azure SQL blob svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere.  

**Azure SQL povezana usluga**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

U odjeljku [Servis za povezane SQL Azure](#azure-sql-linked-service-properties) za popis svojstava podržava povezane servis. 

**Azure servis za pohranu povezana blobova platforme**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

U članku [Blobova platforme Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) za popis svojstava podržava povezane servis. 

**Azure SQL unos dataset**

Uzorka pretpostavlja da ste stvorili tablicu "MojaTablica" u Azure SQL i sadrži stupac pod nazivom "timestampcolumn" za vrijeme niza podataka. 

Postavljanje "vanjski": "true" obavještava servisa Azure podataka tvorničke skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
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

U odjeljku [Svojstva vrste skup podataka Azure SQL](#azure-sql-dataset-type-properties) za popis svojstava podržava ta vrsta skupa podataka.  

**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

U odjeljku [Svojstva vrste dataset blobova platforme Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) za popis svojstava podržava ta vrsta skupa podataka.  

**Kanal s Kopiraj aktivnosti**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **SqlSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **SqlReaderQuery** odabrat ćete podatke u zadnjem satu da biste kopirali.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

U ovom primjeru **sqlReaderQuery** navedena je za na SqlSource. Aktivnosti Kopiraj ovaj upit se pokreće taj izvor baze podataka SQL Azure dohvaćanje podataka. Osim toga, možete odrediti pohranjena procedura navođenjem **sqlReaderStoredProcedureName** i **storedProcedureParameters** (Ako je pohranjena procedura traje parametara).

Ako ne navedete sqlReaderQuery ili sqlReaderStoredProcedureName, stupce definirane u odjeljku strukturu dataset JSON koriste se za sastavljanje upita da biste pokrenuli bazom podataka SQL Azure. Na primjer: `select column1, column2 from mytable`. Ako definiciju dataset strukturu, odabrane su sve stupce iz tablice. 


Potražite odjeljak [Izvor Sql](#sqlsource) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) popis svojstava podržava SqlSource i BlobSink. 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Primjer: Kopiranje podataka iz blobova platforme Azure s bazom podataka SQL Azure

Uzorak definira sljedeće entiteti tvorničke podataka:  

1.  Povezane servis vrste [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) i [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties).

Ogledni Primjerci vremenski niz podataka (hourly, dnevno, itd.) iz Azure bloba u tablicu u Azure SQL baze podataka svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 


**Azure SQL povezana usluga**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

U odjeljku [Servis za povezane SQL Azure](#azure-sql-linked-service-properties) za popis svojstava podržava povezane servis. 

**Azure servis za pohranu povezana blobova platforme**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

U članku [Blobova platforme Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) za popis svojstava podržava povezane servis.

**Azure Blob unos dataset**

Podaci se izdvojiti iz novi blob svaki sat (učestalost: h, interval: 1). Put i naziv mape za blob-om dinamički vrednuju se temelji na vrijeme početka isječak koji obrađuje. Put do mape koristi godinu, mjesec i dan dio vremena početka i naziv datoteke koristi dio sat vremena početka. "vanjski": "true" postavka obavještava servis tvorničke podataka u ovoj su tablici ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
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

U odjeljku [Svojstva vrste dataset blobova platforme Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) za popis svojstava podržava ta vrsta skupa podataka.

**Azure SQL izlazni skup podataka**

Uzorak kopira podatke u tablicu pod nazivom "MojaTablica" u Azure SQL. Stvaranje tablice u SQL Azure imati isti broj stupaca kao što ste očekivali Blob CSV datoteku koja će sadržavati. Novi reci dodaju se u tablici svaki sat. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

U odjeljku [Svojstva vrste skup podataka Azure SQL](#azure-sql-dataset-type-properties) za popis svojstava podržava ta vrsta skupa podataka.

**Kanal s Kopiraj aktivnosti**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **BlobSource** , a **primatelj** je vrsta **SqlSink**.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

Potražite sekciji [Sql primatelj](#sqlsink) i [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) popis svojstava podržava SqlSink i BlobSource. 


## <a name="azure-sql-linked-service-properties"></a>Svojstva servisa Azure SQL povezana
U uzorcima, koju ste koristili povezane servisa vrste **AzureSqlDatabase** da biste se povezali baze podataka Azure SQL podataka tvorničke. Sljedeća tablica sadrži opis elemenata JSON specifične za servis SQL Azure povezani.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **AzureSqlDatabase** | Da |
| connectionString | Odredite podatke koji su potrebni za povezivanje s bazom podataka SQL Azure instancom za svojstvo connectionString. | Da |

> [AZURE.NOTE] Konfiguriranje [Vatrozida za baze podataka SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) poslužitelj baze podataka da biste [omogućili pristup poslužitelju servisa Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Osim toga, ako iz koje kopirate podatke s bazom podataka SQL Azure izvan Azure uključujući iz lokalnog izvora podataka s podacima tvorničke pristupnika, konfigurirajte odgovarajuće rasponu IP adresa za računalo koje šalje podatke s bazom podataka SQL Azure. 

## <a name="azure-sql-dataset-type-properties"></a>Svojstva vrste dataset Azure SQL
U uzorcima, koju ste koristili skup podataka vrste **AzureSqlTable** za predstavljanje tablice u bazi podataka Azure SQL. 

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.). 

U odjeljku typeProperties razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak **typeProperties** za skup podataka vrste **AzureSqlTable** sadrži sljedeća svojstva:

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| tableName | Naziv tablice u instanci baze podataka SQL Azure koji se odnosi povezane servisa. | Da |

## <a name="azure-sql-copy-activity-type-properties"></a>Azure SQL Kopiraj aktivnosti svojstva vrste
Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti.

> [AZURE.NOTE] Kopiraj aktivnosti uzima samo jedan unos i daje samo jedan izlaz.

Svojstva dostupna u odjeljku **typeProperties** aktivnosti s druge strane razlikuju uz svaku vrstu aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji. 

Ako premještate podataka iz baze podataka Azure SQL, postavite vrsta izvora u aktivnost Kopiraj **SqlSource**. Isto tako, ako su premještanje podataka s bazom podataka Azure SQL, postaviti vrstu primatelj u aktivnost Kopiraj **SqlSink**. U ovom se odjeljku nalaze se popis svojstava podržava SqlSource i SqlSink. 

### <a name="sqlsource"></a>SqlSource

U kopiji aktivnosti, kad je izvor vrste **SqlSource**sljedeća svojstva dostupne su u odjeljku **typeProperties** :

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Čitanje podataka pomoću prilagođenog upita. | SQL niz upita. Primjer: `select * from MyTable`.  | ne |
| sqlReaderStoredProcedureName | Naziv pohranjene procedure koja čita podatke iz izvorišne tablice. | Naziv pohranjene procedure. | ne |
| storedProcedureParameters | Parametara za pohranjene procedure. | Naziv/vrijednost parova. Nazivi i kućište parametara moraju podudarati imena i kućište parametara za pohranjene procedure. | ne |

Ako je **sqlReaderQuery** navedena za na SqlSource, aktivnosti Kopiraj pokreće ovaj upit taj izvor baze podataka SQL Azure dohvaćanje podataka. Osim toga, možete odrediti pohranjena procedura navođenjem **sqlReaderStoredProcedureName** i **storedProcedureParameters** (Ako je pohranjena procedura traje parametara). 

Ako ne navedete sqlReaderQuery ili sqlReaderStoredProcedureName, stupce definirane u odjeljku strukturu dataset JSON koriste se za sastavljanje upita (`select column1, column2 from mytable`) da biste pokrenuli bazom podataka SQL Azure. Ako definiciju dataset strukturu, odabrane su sve stupce iz tablice. 

> [AZURE.NOTE] Kada koristite **sqlReaderStoredProcedureName**, svejedno morate odrediti vrijednost za svojstvo **tableName** u skupu podataka JSON. Postoje bez provjere kroz izvršiti odnosu tablica. 

### <a name="sqlsource-example"></a>Primjer SqlSource

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Definicija pohranjena procedura:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** podržava sljedeća svojstva:

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Pričekajte vremena za umetanje skupne operacije da biste dovršili prije isteka. | Vremenski raspon<br/><br/> Primjer: "00: 30:00" (30 minuta). | ne | 
| writeBatchSize | Umeće podatke u tablicu SQL kada Veličina međuspremnika dosegne writeBatchSize. | Cijeli broj (broj redaka)| Ne (zadani: 10000)
| sqlWriterCleanupScript | Odredite upit za kopiranje aktivnost izvršiti tako da se očistiti podatke određene isječka. Dodatne informacije potražite u odjeljku [repeatability](#repeatability-during-copy). | Naredbe upita.  | ne |
| sliceIdentifierColumnName | Navedite naziv stupca za kopiranje aktivnosti da biste ispunili identifikator automatski generira isječak koji se koristi za čišćenje podataka određeni isječak kada ponovno pokrenite. Dodatne informacije potražite u odjeljku [repeatability](#repeatability-during-copy). | Naziv stupca stupac koji sadrži vrstu podataka binary(32). | ne |
| sqlWriterStoredProcedureName | Naziv pohranjene procedure upserts (ažuriranja na umeće) podatke u ciljnu tablicu. | Naziv pohranjene procedure. | ne |
| storedProcedureParameters | Parametara za pohranjene procedure. | Naziv/vrijednost parova. Nazivi i kućište parametara moraju podudarati imena i kućište parametara za pohranjene procedure. | ne | 
| sqlWriterTableType | Navedite naziv tablice upišite će se koristiti u pohranjena procedura. Aktivnosti kopiju podataka koja se premješta postaje dostupan u temp tablicu s tom vrstom tablice. Pohranjena procedura kod zatim objedinjavanje podataka koja se kopira s postojećim podacima. | Upišite naziv tablice. | ne |

#### <a name="sqlsink-example"></a>Primjer SqlSink

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Identitet stupaca u ciljna baza podataka
U ovom se odjeljku nalaze primjer za kopiranje podataka iz izvorišne tablice bez stupca identiteta odredišnu tablicu s stupcem identiteta. 

**Tablica izvora:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Odredišne tablice:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Imajte na umu da u ciljnu tablicu ima stupac identiteta. 

**Definiciju JSON dataset izvora**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Odredište dataset JSON definicija**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Uočite da se kao izvorišno i odredišno tablice imaju različite shema (cilj ima dodatni stupac s identitetom). U ovom slučaju morate navesti svojstvo **strukture** u definiciji ciljni skup podataka koji ne sadrži stupac identitet. 

Nakon toga mapiranje stupaca iz skupa izvora podataka u stupce u skupu podataka odredište. Odjeljak [uzoraka mapiranja stupca](#column-mapping-samples) potražite u članku primjer. 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Mapiranje vrsta za SQL Server i baze podataka SQL Azure

Kao što je rečeno u aktivnost Kopiraj članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) izvodi Automatska vrsta pretvorbi iz sita vrste pomoću sljedećih koraka 2 pristup vrste izvora:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Pri premještanju podataka da biste & iz SQL Azure, SQL server, Sybase sljedeće mapiranja koriste iz SQL vrste .NET vrstu i obrnuto.

Mapiranje je isti kao vrsta mapiranje SQL podataka poslužitelja za ADO.NET.

| Modul za baze podataka SQL Server vrsta | Vrsta .NET framework |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| Binarni. | Bajt] |
| bitne | Booleove vrijednosti |
| CHAR | Niz, znak] |
| Datum | Datum i vrijeme |
| Datum i vrijeme | Datum i vrijeme |
| datetime2 | Datum i vrijeme |
| Datetimeoffset | DateTimeOffset |
| Broja decimalnih mjesta | Broja decimalnih mjesta |
| Svojstvo FILESTREAM atributa (varbinary(max)) | Bajt] |
| Vrijednost s pomičnim zarezom | Dvaput |
| Slika | Bajt] | 
| INT | Int32 | 
| novac | Broja decimalnih mjesta |
| NCHAR | Niz, znak] |
| ntext | Niz, znak] |
| brojčani | Broja decimalnih mjesta |
| nvarchar | Niz, znak] |
| realni | Jedan |
| ROWVERSION | Bajt] |
| smalldatetime | Datum i vrijeme |
| SMALLINT | Int16 |
| smallmoney | Broja decimalnih mjesta | 
| sql_variant | Objekt * |
| tekst | Niz, znak] |
| vrijeme | Vremenski raspon |
| Vremenska oznaka | Bajt] |
| tinyint | Bajt |
| Jedinstveni identifikator koji | GUID |
| varbinary |  Bajt] |
| VARCHAR | Niz, znak] |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.