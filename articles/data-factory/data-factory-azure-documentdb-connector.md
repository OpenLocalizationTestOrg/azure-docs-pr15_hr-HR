<properties 
    pageTitle="Premještanje podataka do/od DocumentDB | Microsoft Azure" 
    description="Saznajte kako premjestiti podatke/iz zbirke Azure DocumentDB pomoću tvorničke Azure podataka" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Premještanje podataka i s DocumentDB pomoću tvorničke Azure podataka

U ovom se članku opisuje način možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka da biste premjestili podatke Azure DocumentDB iz drugog podataka pohranu i premještanje podataka iz DocumentDB u drugom spremišta podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

Sljedeća primjera pokazuju kako kopirati podatke i iz Azure DocumentDB i spremište blobova platforme Azure. Međutim, podaci mogu biti kopirana **izravno** iz bilo kojeg od izvora na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  

> [AZURE.NOTE] Kopiranje podataka iz podataka na – lokalno/Azure IaaS pohranjuje Azure DocumentDB obrnuto podržani su i s pristupnik za upravljanje podacima verzija 2.1 i iznad.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Primjer: Kopiranje podataka iz DocumentDB blobova platforme Azure

Prikazuje se uzorak u nastavku:

1. Povezane servis vrste [DocumentDb](#azure-documentdb-linked-service-properties).
2. Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Za unos [dataset](data-factory-create-datasets.md) vrste [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka u Azure DocumentDB blobova platforme Azure. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere.

**Azure DocumentDB povezana servisa:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure blobova povezana servisa:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure dataset unos DB dokumenta:**

Uzorak pretpostavlja da imate zbirku pod nazivom **osobe** u bazi podataka programa Azure DocumentDB.
 
Postavljanje "vanjski": "true" i određivanje externalData informacije o pravilima servisa Azure podataka tvorničke tablici ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Blobova platforme Azure izlazni skup podataka:**

Podaci kopiraju se u novi blob svaki sat putom za blob o određeni datetime s preciznosti sat.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Uzorak JSON dokumenta u zbirci osoba u bazi podataka DocumentDB: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB podržava upite dokumenata putem SQL kao što je sintaksa hijerarhijsku JSON dokumenata. 

Primjer: Odaberite Person.PersonId, ime kao Person.Name.First Person.Name.Middle kao MiddleName, prezime kao Person.Name.Last NEKE osobe

Kanal sljedeće kopiranje podataka iz zbirke osoba u bazi podataka DocumentDB blobova platforme Azure. Kao dio aktivnosti Kopiraj unos i izlaz nisu navedeni skupova podataka.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Primjer: Kopiranje podataka iz blobova platforme Azure Azure DocumentDB

Prikazuje se uzorak u nastavku:

1. Povezane servis vrste [DocumentDb](#azure-documentdb-linked-service-properties).
2. Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Za unos [dataset](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) i [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


Uzorak kopira podatke iz Azure blobova platforme za Azure DocumentDB. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere.

**Azure blobova povezana servisa:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DocumentDB povezana servisa:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob unos skup podataka:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB izlazni skup podataka:**

Uzorak kopira podatke u zbirku pod nazivom "Osobe".

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Sljedeće kanal kopira podatke iz blobova platforme Azure zbirke osoba u na DocumentDB. Kao dio aktivnosti Kopiraj unos i izlaz nisu navedeni skupova podataka. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Ako je blob unos uzorka kao 

    1,John,,Doe

Zatim izlaz JSON u DocumentDB bit će kao:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB je spremište NoSQL za JSON dokumente koje su dopuštene ugniježđene strukture. Azure tvorničke podataka omogućuje korisnika za označavanje hijerarhije putem **nestingSeparator**, što je "." u ovom primjeru. S razdjelnik, aktivnosti Kopiraj generirat će objekt "Naziv" s tri podređenih elemenata najprije srednje ime i prezime, prema "Name.First", "Name.Middle" i "Name.Last" u definiciju tablice.

## <a name="azure-documentdb-linked-service-properties"></a>Azure svojstva DocumentDB povezane servisa

Sljedeća tablica sadrži opis JSON elementi specifične za servisa Azure DocumentDB povezani. 

| **Svojstvo** | **Opis** | **Obavezno** |
| -------- | ----------- | --------- |
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **DocumentDb** | Da |
| connectionString | Odredite podatke potrebne za povezivanje s bazom podataka Azure DocumentDB. | Da |

## <a name="azure-documentdb-dataset-type-properties"></a>Svojstva vrste Azure DocumentDB skup podataka

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Dijelove, npr strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).
 
U odjeljku typeProperties razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **DocumentDbCollection** ima sljedeća svojstva.

| **Svojstvo** | **Opis** | **Obavezno** |
| -------- | ----------- | -------- |
| collectionName | Naziv zbirke dokumenata DocumentDB. | Da |


Primjer:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Shema po tvorničke podataka
Servis podataka tvorničke izvodi u shemi trgovine sheme slobodno podataka kao što su DocumentDB, na jedan od sljedećih načina:  

1.  Ako navedete strukturu podataka korištenjem svojstva **strukture** u definiciji skup podataka, servis podataka tvorničke poštuje ovu strukturu kao u shemi. U ovom slučaju ako redak sadrži vrijednost stupca, vrijednost null objavit ćemo zajedno za njega.
2.  Ako ne navedete strukturu podataka korištenjem svojstva **strukture** u definiciji skup podataka, servis tvorničke podataka određuje sheme pomoću prvi redak podataka. U ovom slučaju Ako prvi redak sadrži cijeli shemu, neki stupci bit će nedostaju u rezultatu kopiranja.

Stoga za izvore podataka sheme slobodno, najbolje je da biste odredili strukture podataka pomoću svojstvo **strukturu** .

## <a name="azure-documentdb-copy-activity-type-properties"></a>Svojstva vrste Azure DocumentDB Kopiraj aktivnosti

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti.
 
**Bilješke:** Kopiraj aktivnosti uzima samo jedan unos i daje samo jedan izlaz.

Svojstva dostupna u odjeljku typeProperties aktivnosti s druge strane razlikovati za svaku vrstu aktivnosti i u slučaju Kopiraj aktivnosti mogu se razlikovati ovisno o tome vrste izvora i primatelji.

U slučaju aktivnosti kopija kada je izvor vrste **DocumentDbCollectionSource** sljedeća svojstva dostupne su u odjeljku **typeProperties** :

| **Svojstvo** | **Opis** | **Dopuštena vrijednost** | **Obavezno** |
| ------------ | --------------- | ------------------ | ------------ |
| upit | Odredite upit za čitanje podataka. | Niz podržava DocumentDB u upitu. <br/><br/>Primjer:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | ne <br/><br/>Ako nije naveden, SQL naredbi koja se izvršava:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Posebni znak da biste označili dokument je ugniježđeno | Bilo koji znak. <br/><br/>DocumentDB je spremište NoSQL za JSON dokumente koje su dopuštene ugniježđene strukture. Azure tvorničke podataka omogućuje korisnika za označavanje hijerarhije putem nestingSeparator, što je "." u gornjem primjerima. S razdjelnik, aktivnosti Kopiraj generirat će objekt "Naziv" s tri podređenih elemenata najprije srednje ime i prezime, prema "Name.First", "Name.Middle" i "Name.Last" u definiciju tablice. | ne

**DocumentDbCollectionSink** podržava sljedeća svojstva:

| **Svojstvo** | **Opis** | **Dopuštena vrijednost** | **Obavezno** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Potreban je posebnog znaka u naziv izvorišnog stupca da biste naznačili ugniježđene dokumenta. <br/><br/>Na primjer iznad: `Name.First` u izlaz tablice stvara sljedeću strukturu JSON u dokumentu DocumentDB:<br/><br/>"Naziv": {<br/>  "Prvi": "Luka"<br/>}, | Znak koji se koristi za razdvajanje razina gniježđenja.<br/><br/>Zadana vrijednost je `.` (točka). | Znak koji se koristi za razdvajanje razina gniježđenja. <br/><br/>Zadana vrijednost je `.` (točka). | ne | 
| writeBatchSize | Broj paralelnih zahtjeva za uslugu DocumentDB za stvaranje dokumenata.<br/><br/>Kada kopirate podatke iz DocumentDB pomoću tog svojstva možete dodatno prilagoditi performanse. Možete očekivati od boljih performansi povećavate writeBatchSize jer više paralelnih zahtjevi za DocumentDB šalju. No morat ćete izbjeći Reguliranje koji mogu vratiti se poruka o pogrešci: "Zahtjev je velika brzina".<br/><br/>Ograničavanje navedena je nekoliko čimbenika, uključujući veličina dokumenata, broj uvjeta u dokumentima, indeksiranje pravila odredišne zbirke, itd. Za operacije kopiranja bolje zbirke (npr. S3) možete koristiti da bi se najčešće propusnost dostupna (2,500 zatražite jedinice/sekundi). | Cijeli broj | Ne (zadani: 10000) |
| writeBatchTimeout | Pričekajte vrijeme operacije da biste dovršili prije isteka. | Vremenski raspon<br/><br/> Primjer: "00: 30:00" (30 minuta). | ne |
 
## <a name="appendix"></a>Dodatak
1. **Pitanje:**  
    ne Kopiraj aktivnosti podršku ažuriranje postojećih zapisa?

    **Odgovor:**  
    ne.

2. **Pitanje:**  
    kako već na pokušaj kopiju DocumentDB dijeli s kopirali zapisa?

    **Odgovor:**  
    ako zapisa imaju polje "ID" i kopiranje pokušava zapis s istim ID-om, kopiranje throws pogrešku.  
 
3. **Pitanje:** 
    tvorničke podataka podržava [raspon ili raspršivanje na temelju podataka particija](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Odgovor:** 
    ne. 
4. **Pitanje:** 
    li navesti više od jedne DocumentDB zbirke za tablicu?
    
    **Odgovor:** 
    ne. Sada možete navesti samo jednu zbirku.
     
## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.
