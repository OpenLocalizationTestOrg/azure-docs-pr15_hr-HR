<properties 
    pageTitle="Premještanje podataka u/iz tablice Azure | Microsoft Azure" 
    description="Upute za premještanje podataka iz spremišta tablica Azure pomoću tvorničke Azure podataka." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Premještanje podataka i iz tablice Azure pomoću tvorničke Azure podataka

U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz tablice Azure s drugom spremišta podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka i kombinacijama podržanih trgovine s Kopiraj aktivnosti.

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanal koja se kopira podatke iz spremišta tablica Azure najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

Sljedeći primjeri sadrže definicije JSON uzorka koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke i iz spremišta tablica Azure i baza podataka blobova platforme Azure. Međutim, podaci mogu biti kopirana **izravno** iz bilo kojeg od izvora na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Primjer: Kopiranje podataka iz tablice Azure blobova platforme Azure

Sljedeći primjer prikazuje:

1.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (koristi se za tablice i blob).
2.  Za unos [dataset](data-factory-create-datasets.md) vrste [Azure](#azure-table-dataset-type-properties).
3.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  [Kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [AzureTableSource](#azure-table-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Uzorak kopira podataka pripadaju zadana particija u tablici programa Azure da biste blob svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere.

**Servis za Azure prostora za pohranu povezana:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure tvorničke podataka podržava dvije vrste servise za pohranu Azure povezana: **AzureStorage** i **AzureStorageSas**. Prve faze koju navedete niz za povezivanje koja sadrži ključ za račun i za kasnije onaj koji navedete Uri zajednički pristup potpis (SAS). Odjeljak [Povezani servisi](#linked-services) potražite u članku dodatne informacije.  

**Azure dataset unos tablice:**

Uzorak pretpostavlja da ste stvorili tablicu "MojaTablica" u tablici Azure.
 
Postavljanje "vanjski": "true" obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
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

**Blobova platforme Azure izlazni skup podataka:**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kanali s Kopiraj aktivnosti:**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **AzureTableSource** , a **primatelj** je vrsta **BlobSink**. SQL upit navedene za svojstvo **AzureTableSourceQuery** odabrat ćete podatke iz zadana particija svaki sat da biste kopirali.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Primjer: Kopiranje podataka iz blobova platforme Azure Azure tablice

Sljedeći primjer prikazuje:

1.  Servis za povezane vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (koristi se za tablice i blob)
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [Azure](#azure-table-dataset-type-properties). 
4.  [Kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) i [AzureTableSink](#azure-table-copy-activity-type-properties). 


Ogledni Primjerci vremenski niz podataka iz programa Azure bloba za programa Azure tablice hourly. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere.

**Servis za Azure (prostor za pohranu i Azure & tablice Blob) povezane:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure tvorničke podataka podržava dvije vrste servise za pohranu Azure povezana: **AzureStorage** i **AzureStorageSas**. Prvoga, navedite niz za povezivanje koja sadrži ključ za račun i za kasnije onaj koji navedete Uri zajednički pristup potpis (SAS). Odjeljak [Povezani servisi](#linked-services) potražite u članku dodatne informacije. 

**Azure Blob unos skup podataka:**

Podaci se izdvojiti iz novi blob svaki sat (učestalost: h, interval: 1). Put i naziv mape za blob-om dinamički vrednuju se temelji na vrijeme početka isječak koji obrađuje. Put do mape koristi godinu, mjesec i dan dio vremena početka i naziv datoteke koristi dio sat vremena početka. "vanjski": "true" postavka obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Tablica platforme Azure izlazni skup podataka:**

Uzorak kopira podatke u tablicu pod nazivom "MojaTablica" u tablici Azure. Stvaranje tablice programa Azure imati isti broj stupaca kao što ste očekivali Blob CSV datoteku koja će sadržavati. Novi reci dodaju se u tablici svaki sat. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Kanal s Kopiraj aktivnosti:**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **BlobSource** , a **primatelj** je vrsta **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
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

## <a name="linked-services"></a>Povezani servisi
Postoje dvije vrste povezane services možete koristiti da biste se povezali Azure blobova na tvorničke Azure podataka. Su: **AzureStorage** povezana i servis **AzureStorageSas** povezani. Servis za pohranu Azure povezana nudi tvorničke podataka s programom access globalni Azure za pohranu. Dok ste povezani u Azure prostora za pohranu SAS (zajednički pristup potpisa) usluga omogućuje tvorničke podataka s programom access ograničena/vrijeme-granica Azure za pohranu. Postoje nema razlike između tih dvaju povezanih servisa. Odaberite povezani servis koji odgovara vašim potrebama. Sljedeći odjeljci sadrže dodatne detalje na tih dvaju povezanih servisa.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Svojstva vrste Azure Dataset tablice

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku typeProperties razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak **typeProperties** za skup podataka vrste **Azure** ima sljedeća svojstva.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| tableName | Naziv tablice u instanci baze podataka u tablici Azure koji se odnosi povezane servisa. | Da. Kada je tableName je navedeno bez programa azureTableSourceQuery, sve zapise iz tablice kopiraju se na odredište. Ako je azureTableSourceQuery i nije naveden, zapisi iz tablice koja zadovoljava upit se kopiraju odredište. |

### <a name="schema-by-data-factory"></a>Shema po tvorničke podataka
Servis podataka tvorničke izvodi u shemi trgovine sheme slobodno podataka kao što su tablice Azure, na jedan od sljedećih načina:

1.  Ako navedete strukturu podataka korištenjem svojstva **strukture** u definiciji skup podataka, servis podataka tvorničke poštuje ovu strukturu kao u shemi. U ovom slučaju ako redak sadrži vrijednost stupca, vrijednost null navedeni su za nju.
2. Ako ne navedete strukturu podataka korištenjem svojstva **strukture** u definiciji skup podataka, tvorničke podataka određuje sheme pomoću prvi redak podataka. U ovom slučaju Ako prvi redak sadrži cijeli shemu, neki stupci su Propušteni u rezultatu kopiranja.

Stoga za izvore podataka sheme slobodno, najbolje je da biste odredili strukture podataka pomoću svojstvo **strukturu** .

## <a name="azure-table-copy-activity-type-properties"></a>Azure svojstva vrste aktivnosti Kopiraj tablicu

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni skupova podataka i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

**AzureTableSource** podržava sljedeća svojstva u odjeljku typeProperties:

Svojstvo | Opis | Dopuštena vrijednost | Obavezno
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Čitanje podataka pomoću prilagođenog upita. | Tablica platforme Azure niza upita. Pogledajte primjere u sljedećem odjeljku. | ne. Kada je tableName je navedeno bez programa azureTableSourceQuery, sve zapise iz tablice kopiraju se na odredište. Ako je azureTableSourceQuery i nije naveden, zapisi iz tablice koja zadovoljava upit se kopiraju odredište.
azureTableSourceIgnoreTableNotFound | Označava hoće li swallow iznimku tablice ne postoji. | TRUE<br/>FALSE | ne |

### <a name="azuretablesourcequery-examples"></a>Primjeri azureTableSourceQuery

Ako je stupac tablice Azure vrsta niza: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Ako je stupac tablice Azure vrsta datuma/vremena: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** podržava sljedeća svojstva u odjeljku typeProperties:


Svojstvo | Opis | Dopuštena vrijednost | Obavezno  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Zadani particija vrijednost ključa koji se mogu koristiti primatelj. | Vrijednost niza. | ne 
azureTablePartitionKeyName | Navedite naziv stupca čije vrijednosti koje se koriste kao tipke particije. Ako nije naveden, AzureTableDefaultPartitionKeyValue se koristi kao tipku particije. | Naziv stupca. | ne |
azureTableRowKeyName | Navedite naziv stupca čije se vrijednosti stupca se koriste kao redak ključ. Ako nije naveden, koristite GUID za svaki redak. | Naziv stupca. | ne  
azureTableInsertType | Način rada da biste umetnuli podatke u tablicu Azure.<br/><br/>Ovo svojstvo određuje hoće li postojećih redaka u izlaznoj tablici s usklađenim particija i redak tipke su vrijednosti zamijenjene ili spojiti. <br/><br/>Da biste saznali kako funkcioniraju te postavke (pisma i zamijeni), potražite u članku [Umetanje ili spajanja entitet](https://msdn.microsoft.com/library/azure/hh452241.aspx) , a [Umetanje ili zamjena entitet](https://msdn.microsoft.com/library/azure/hh452242.aspx) teme. <br/><br> Ova postavka primjenjuje se na razini retka, ne tablice razinu i nijedna mogućnost briše retke u tablici Izlazni koje ne postoje u unos. | Spajanje (zadano)<br/>Zamjena | ne 
writeBatchSize | Umeće podatke u tablici Azure kada kliknete writeBatchSize ili writeBatchTimeout. | Cijeli broj (broj redaka)| Ne (zadani: 10000) 
writeBatchTimeout | Umeće podatke u tablici Azure kada kliknete writeBatchSize ili writeBatchTimeout | Vremenski raspon<br/><br/>Primjer: "00: 20:00" (20 minuta) | Ne (Zadana vremenskog ograničenja za pohranu klijenta zadane vrijednost 90 sec)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Mapirajte izvorni stupac odredište stupca pomoću prevoditelj JSON svojstvo mogli koristiti u odredišnom stupcu kao u azureTablePartitionKeyName.

U sljedećem primjeru izvorni stupac DivisionID mapirano na odredišnom stupcu: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

U DivisionID je naveden kao ključ particije. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Mapiranje vrsta za tablica platforme Azure

Kao što je rečeno u članku [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) Kopiraj aktivnosti izvodi Automatska vrsta pretvorbe vrste izvora da biste sita vrste s sljedeća dva koraka pristup.

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Prilikom pomicanja podataka da biste & iz tablice Azure, sljedeće [mapiranja definira servisa Azure tablice](https://msdn.microsoft.com/library/azure/dd179338.aspx) koriste OData tablice Azure vrste .NET vrstu i obrnuto. 

| Vrsta podataka OData | Vrsta .NET | Pojedinosti |
| --------------- | --------- | ------- |
| Edm.Binary | bajt] | Polje bajtova do 64 KB. |
| Edm.Boolean | booleovom | Booleova vrijednost. |
| Edm.DateTime | Datum i vrijeme | 64-bitni vrijednost izraženu kao koordiniranog vremena (UTC). Podržani raspon datuma i vremena počinje od ponoći 12:00, siječanj 1, 1601 N.E. (C.E.), UTC-A. Raspon završava na prosinac 31 9999. |
| Edm.Double | Dvostruki | 64-bitni plutajućih zareza vrijednosti. |
| Edm.Guid | GUID | Globalno jedinstveni identifikator 128-bitno. |
| Edm.Int32 | Int32 ili int | 32-bitnu cjelobrojnu. |
| Edm.Int64 | Int64 ili dugu | 64-bitnu cjelobrojnu. |
| Edm.String | Niz | Vrijednost UTF-16-om. Niz vrijednosti može biti do 64 KB. |

### <a name="type-conversion-sample"></a>Ogledna pretvorbe vrsta

Sljedeća Ogledna je za kopiranje podataka iz programa Azure blobova platforme Azure tablicu s pretvorbe vrsta. 

Pretpostavimo da Blob skupu podataka u obliku CSV i sadrži tri stupca. Jedan od njih je stupac datuma i vremena u obliku prilagođenog datetime korištenje skraćeni francuski naziva dana u tjednu. 

Definiranje dataset Blob izvora na sljedeći način zajedno sa stilovima za stupce.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

Danih mapiranje vrsta Azure tablice OData vrsta .NET vrsta, bi određujete tablicu u tablici Azure sa shemom sljedeće. 

**Azure shema tablice:**

Naziv stupca | Vrsta
----------- | --------
ID korisnika | Edm.Int64
ime | Edm.String 
lastlogindate | Edm.DateTime

Nakon toga definirati skup podataka tablice Azure na sljedeći način. Nije potrebno da biste odredili "struktura" odjeljak s informacijama vrste jer vrsta podaci navedeni su već u podlozi spremišta podataka.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

U ovom slučaju tvorničke podataka automatski pretvorbe vrsta uključujući polja datuma i vremena u obliku prilagođenog datuma i vremena pomoću kulture "fr fr" pri premještanju podataka iz blobova platforme Azure tablicu.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Da biste saznali ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati, potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md).







