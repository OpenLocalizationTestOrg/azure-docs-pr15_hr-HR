<properties 
    pageTitle="Premještanje podataka iz Amazon jednostavne servis za pohranu pomoću podataka tvorničke | Microsoft Azure" 
    description="Saznajte više o premještanje podataka iz Amazon jednostavne servis za pohranu (S3) pomoću tvorničke Azure podataka." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Premještanje podataka iz Amazon jednostavne servis za pohranu pomoću tvorničke Azure podataka

U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka da biste premjestili podatke iz Amazon jednostavne servis za pohranu (S3) na druge podatke trgovine. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koji predstavlja Općenito pregled premještanje podataka i popis podržanih izvora i primatelj podataka trgovine s Kopiraj aktivnosti.  

Tvorničke podataka trenutno podržava samo premještanje podataka iz Amazon S3 u drugim spremišta podataka, ali ne i za premještanje podataka s drugim podacima trgovine Amazon S3.

## <a name="required-permissions"></a>Potrebne dozvole

Da biste kopirali podatke iz Amazon S3, provjerite je li vam dodijeljena ispod dozvole:

- **s3:GetObject** i **s3:GetObjectVersion** za Amazon S3 objekt operacije
- **s3:ListBucket** i **s3:ListAllMyBuckets** (koja se koristi u čarobnjaku za kopiranje samo) za Amazon S3 grupe operacije 

Cijeli popis Amazon S3 permisions s detaljima možete pronaći iz [Određivanje dozvola u pravilu](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanal koja se kopira podatke iz Amazon S3 najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Pokazuje kako kopirati podatke iz Amazon S3 spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Primjer: Kopiranje podataka iz Amazon S3 blobova platforme Azure
Ovaj primjer pokazuje kako kopirati podatke iz programa Amazon S3 blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

- Povezane servis vrste [AwsAccessKey](#linked-service-properties).
- Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Za unos [dataset](data-factory-create-datasets.md) vrste [AmazonS3](#dataset-type-properties).
- Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [FileSystemSource](#copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka iz Amazon S3 blobova platforme Azure svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

**Amazon S3 povezana servisa**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
            }
        }
    }

**Azure servis za pohranu povezana**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Unos dataset Amazon S3**

Postavljanje **"vanjski": true** obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka. Postavljanje tog svojstva na true unosa skup podataka koji je osnovu aktivnosti u kanalu.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }



**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Kanal s Kopiraj aktivnosti**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **FileSystemSource** , a **primatelj** je vrsta **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Svojstva povezane servisa

Sljedeća tablica sadrži opis elemenata JSON određene za Amazon S3 (**AwsAccessKey**) povezana servisa.

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | ID tajnu tipkovni prečac. | niz | Da |
| secretAccessKey | Tipka za pristup tajnu sam. | Šifrirani tajnu niz | Da | 


## <a name="dataset-type-properties"></a>Svojstva vrste skup podataka

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **AmazonS3** (koji sadrži skup podataka Amazon S3) ima sljedeća svojstva

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------- | ------ | 
| bucketName | Naziv grupe S3. | Niz | Da |
| ključ | Ključ S3 objekta. | Niz | ne | 
| Prefiks | Prefiks tipke S3 objekta. Odabrani su objekte čiji tipke počinju ovaj prefiks. Odnosi se samo kad je prazan ključ. | Niz | ne | 
| verzija | Verzija objekta S3 ako je omogućeno određivanje verzije S3. | Niz | ne |  
| Oblikovanje | Podržane su sljedeće vrste oblika: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Postavite svojstvo **Vrsta** u odjeljku oblik na jednu od ovih vrijednosti. [Određivanje TextFormat](#specifying-textformat), [Pri određivanju AvroFormat](#specifying-avroformat), [Određivanje JsonFormat](#specifying-jsonformat), [Određivanje OrcFormat](#specifying-orcformat)i [Određivanje ParquetFormat](#specifying-parquetformat) sekcije detalje potražite u članku. Ako želite kopirati datoteke kao-je između utemeljenih na datotekama trgovine (binarni kopiranje), možete preskočiti u odjeljku oblik i definicije ulazni i izlazni skup podataka.| ne
| Sažimanje | Navedite vrstu i razina kompresije za podatke. Podržane vrste su: **GZip**, **Deflate**i **BZip2** i podržanim razine: **Optimal** i **najbrže**. Trenutno postavki sažimanja nisu podržani za podatke u **AvroFormat** ili **OrcFormat**. Dodatne informacije potražite u članku [podrška za sažimanje](#compression-support) sekciji.  | ne |

> [AZURE.NOTE] bucketName + tipka određuje mjesto objekta S3 pri čemu je grupe spremnik za S3 objekte, a ključ cijeli put do objekta S3.

### <a name="sample-dataset-with-prefix"></a>Ogledna dataset s prefiksom

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Ogledna skup podataka (s verzija)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dinamični putove za S3

U uzorku, koristimo fiksnih vrijednosti za ključ i bucketName svojstava u skupu podataka Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Možete imati podataka tvorničke izračun ključ i bucketName dinamički prilikom izvođenja korištenjem varijable sustava, kao što su SliceStart.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

To možete učiniti na isti način za svojstvo prefiks programa Amazon S3 skup podataka. Potražite u članku [funkcije tvorničke podataka i sistemske varijable](data-factory-functions-variables.md) popis podržanih funkcija i varijabli. 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Kopiranje svojstva vrste aktivnosti

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku **typeProperties** aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kada je izvor u kopiju aktivnosti vrste **FileSystemSource** (koji sadrži Amazon S3), sljedeća svojstva dostupne su u odjeljku typeProperties:

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- | 
| rekurzivne | Određuje je li za rekurzivno popis S3 objekata u direktoriju. | TRUE i false | ne | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.

## <a name="next-steps"></a>Daljnji koraci
Potražite u sljedećim člancima: 
- [Kopiraj aktivnosti vodič](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) za detaljne upute za stvaranje na kanal s Kopiraj aktivnosti. 
