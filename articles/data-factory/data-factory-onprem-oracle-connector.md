<properties 
    pageTitle="Premještanje podataka do/od Oracle pomoću podataka tvorničke | Microsoft Azure" 
    description="Saznajte kako možete premjestiti podataka iz baze podataka tvrtke Oracle koja je lokalnog pomoću tvorničke Azure podataka." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Premještanje podataka iz lokalnog Oracle pomoću tvorničke Azure podataka 

U ovom se članku opisuje kako pomoću podataka tvorničke Kopiraj aktivnosti za premještanje podataka iz Oracle s drugom podataka pohraniti. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

## <a name="installation"></a>Instalacija 
Za servis tvorničke Azure podataka da biste se mogli povezati s lokalne baze podataka tvrtke Oracle, potrebno je instalirati sljedeće komponente: 

- Pristupnik za upravljanje podacima na istom računalu koje hostira baze podataka ili na zasebnom računalo da biste izbjegli natječu za resurse s bazom podataka. Pristupnik za upravljanje podacima je klijent agent koja povezuje lokalnih izvora podataka za servise u oblaku sigurne i upravljani način. Potražite u članku [Premještanje podataka između lokalnog i oblaka](data-factory-move-data-between-onprem-and-cloud.md) članak detalje o pristupnik za upravljanje podacima. 
- Oracle Data Provider za .NET. Komponenta je sve obuhvaćeno [Oracle podataka programa Access komponente za Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Instalirajte odgovarajuću verziju (32-64 bitnu) na glavno računalo na kojem je instaliran pristupnik. [Oracle podataka davatelja .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) možete pristupiti s bazom podataka tvrtke Oracle 10 g izdanje 2 ili novije.

    Ako odaberete "XCopy instalacija", slijedite korake u na readme.htm. Preporučujemo da odaberete instalacijski program s korisničkog Sučelja (koji nisu-XCopy nešto). 
 
    Nakon instalacije davatelja usluge, ponovno pokrenite servis glavnog računala za pristupnik za upravljanje podataka na vašem računalu pomoću servisa apleta (ili) upravitelja konfiguracije pristupnika za upravljanje podacima.  

> [AZURE.NOTE] [Problema](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) potražite u članku otklanjanje poteškoća pristupnika za savjeta za otklanjanje poteškoća veze/pristupnik probleme vezane uz. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanala koja se kopira podatke s bazom podataka tvrtke Oracle svih podržanih primatelj služi za pohranu podataka najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke s bazom podataka tvrtke Oracle u spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Primjer: Kopiranje podataka iz Oracle blobova platforme Azure
Ovaj primjer pokazuje kako kopirati podatke iz baze podataka tvrtke Oracle lokalnog blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  Na [kanal](data-factory-create-pipelines.md) s aktivnosti Kopiraj koji koristi [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) kao izvor i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) kao primatelj.

Uzorak kopiranje podataka iz tablice u bazi podataka tvrtke Oracle lokalnog blob zaračunava. Dodatne informacije o različitim svojstvima koji se koriste u uzorku potražite u dokumentaciji u odjeljcima pratiti primjere.

**Servis za Oracle povezana:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure blobova povezana servisa:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Unos dataset tvrtke Oracle:**

Uzorak pretpostavlja stvorite tablicu "MojaTablica" u Oracle i sadrži stupac pod nazivom "timestampcolumn" za vrijeme niza podataka. 

Postavljanje "vanjski": "true" obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put i naziv mape za blob-om dinamički vrednuju se temelji na vrijeme početka isječak koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.
    
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


**Kanal s Kopiraj aktivnosti:**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje zaračunava. U kanalu JSON definicija vrsta **izvora** postavljen na **OracleSource** , a **primatelj** je vrsta **BlobSink**.  SQL upit navedene za svojstvo **oracleReaderQuery** odabrat ćete podatke u zadnjem satu da biste kopirali.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Morate prilagoditi niza upita na temelju konfiguraciji datuma u bazi podataka tvrtke Oracle. Ako se prikaže sljedeća poruka o pogrešci: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

Možda ćete morati promijeniti upit, kao što prikazuje sljedeći primjer (pomoću funkcija to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Primjer: Kopiranje podataka iz blobova platforme Azure Oracle
Ovaj primjer pokazuje kako kopirati podatke iz blobova Azure bazom podataka tvrtke Oracle lokalnog. Međutim, podaci mogu biti kopirana **izravno** s bilo kojeg na izvora navedeni [ovdje](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) kao izvora [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) kao primatelj.

Uzorak kopira podatke s blob tablice u bazu podataka tvrtke Oracle lokalnog svaki sat. Dodatne informacije o različitim svojstvima koji se koriste u uzorku potražite u dokumentaciji u odjeljcima pratiti primjere.

**Servis za Oracle povezana:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure blobova povezana servisa:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure Blob unos dataset**

Podaci se izdvojiti iz novi blob svaki sat (učestalost: h, interval: 1). Put i naziv mape za blob-om dinamički vrednuju se temelji na vrijeme početka isječak koji obrađuje. Put do mape koristi godinu, mjesec i dan dio vremena početka i naziv datoteke koristi dio sat vremena početka. "vanjski": "true" postavka obavještava servis tvorničke podataka u ovoj su tablici ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

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

**Oracle izlazni skup podataka:**

Uzorak pretpostavlja da ste stvorili tablicu "MojaTablica" u Oracle. Stvaranje tablice u Oracle imati isti broj stupaca kao što ste očekivali Blob CSV datoteku koja će sadržavati. Novi reci dodaju se u tablici svaki sat.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Kanal s Kopiraj aktivnosti:**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **BlobSource** , a **primatelj** vrsta postavljena na **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Oracle povezana svojstva servisa

Sljedeća tablica sadrži opis elemenata JSON specifične za Oracle povezana servisa. 

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
Vrsta | Svojstvo vrsta mora biti postavljeno na: **OnPremisesOracle** | Da
connectionString | Odredite podatke koji su potrebni za povezivanje s bazom podataka tvrtke Oracle instancom za svojstvo connectionString. | Da 
gatewayName | Naziv pristupnika koji se koristi za povezivanje s lokalnog poslužitelja za Oracle | Da

Detalje o postavljanju vjerodajnica za lokalni izvor podataka tvrtke Oracle u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .
## <a name="oracle-dataset-type-properties"></a>Svojstva vrste skup podataka tvrtke Oracle

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Oracle, blobova platforme Azure, tablica platforme Azure itd.).
 
U odjeljku typeProperties razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste OracleTable sadrži sljedeća svojstva:

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
tableName | Naziv tablice u bazi podataka tvrtke Oracle koji se odnosi povezane servisa. | Ne (Ako je naveden **oracleReaderQuery** **OracleSource** )

## <a name="oracle-copy-activity-type-properties"></a>Svojstva vrste aktivnosti Kopiraj za Oracle

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

> [AZURE.NOTE]
Kopiraj aktivnosti uzima samo jedan unos i daje samo jedan izlaz.

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

### <a name="oraclesource"></a>OracleSource
U kopiji aktivnosti, kad je izvor vrste **OracleSource** sljedeća svojstva dostupne su u odjeljku **typeProperties** :

Svojstvo | Opis |Dopuštena vrijednost | Obavezno
-------- | ----------- | ------------- | --------
oracleReaderQuery | Čitanje podataka pomoću prilagođenog upita. | SQL niz upita. Na primjer: odaberite *iz MojaTablica <br/> <br/>ako nisu navedeni SQL naredbi koja se izvršava: Odaberite* iz MojaTablica | Ne (Ako je naveden **tableName** **skup podataka** )

### <a name="oraclesink"></a>OracleSink
**OracleSink** podržava sljedeća svojstva:

Svojstvo | Opis | Dopuštena vrijednost | Obavezno
-------- | ----------- | -------------- | --------
writeBatchTimeout | Pričekajte vremena za umetanje skupne operacije da biste dovršili prije isteka. | Vremenski raspon<br/><br/> Primjer: 00:30:00 (30 minuta). | ne
writeBatchSize | Umeće podatke u tablicu SQL kada Veličina međuspremnika dosegne writeBatchSize.   | Cijeli broj (broj redaka)| Ne (zadani: 10000)  
sqlWriterCleanupScript | Odredite upit za kopiranje aktivnost izvršiti tako da se očistiti podatke određene isječka. | Naredbe upita. | ne
sliceIdentifierColumnName | Navedite naziv stupca za kopiranje aktivnosti da biste ispunili identifikator automatski generira isječak koji se koristi za čišćenje podataka određeni isječak kada ponovno pokrenite. | Naziv stupca stupac koji sadrži vrstu podataka binary(32). | ne


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Mapiranje vrsta za Oracle

Kao što je rečeno u aktivnost Kopiraj članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) izvodi Automatska vrsta pretvorbi iz sita vrste pomoću sljedećih koraka 2 pristup vrste izvora:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Prilikom pomicanja podataka iz Oracle, sljedeće mapiranja koriste se iz vrsta podataka tvrtke Oracle .NET vrstu i obrnuto.

Vrsta podataka tvrtke Oracle | Vrste podataka .NET framework
---------------- | ------------------------
BFILE | Bajt]
BLOB | Bajt]
CHAR | Niz
CLOB | Niz
DATUM | Datum i vrijeme
VRIJEDNOST S POMIČNIM ZAREZOM | Broja decimalnih mjesta
CIJELI BROJ | Broja decimalnih mjesta
INTERVAL GODINE MJESEC | Int32
INTERVAL DAN SEKUNDU | Vremenski raspon
DUGI | Niz
DUGO NEOBRAĐENOG | Bajt]
NCHAR | Niz
NCLOB | Niz
BROJ | Broja decimalnih mjesta
NVARCHAR2 | Niz
NEOBRAĐENOG | Bajt]
ROWID | Niz
VREMENSKA OZNAKA | Datum i vrijeme
VREMENSKA OZNAKA S LOKALNU VREMENSKU ZONU | Datum i vrijeme
VREMENSKA OZNAKA S VREMENSKE ZONE | Datum i vrijeme
CIJELI BROJ BEZ PREDZNAKA | Broj
VARCHAR2 | Niz
XML | Niz

## <a name="troubleshooting-tips"></a>Savjeti za otklanjanje poteškoća

**Problem:** Prikazat će se sljedeća **poruka o pogrešci**: Kopiraj aktivnosti ispunjen parametri koji nisu valjani: 'UnknownParameterName', detaljnu poruku: nije moguće pronaći tražene .net Framework davatelj podataka. Možda nije instalirana".  

**Mogući uzroci:**

1. .NET Framework davatelj podataka za Oracle nije instaliran.
2. .NET Framework davatelj podataka za Oracle je bio instaliran za .NET Framework 2.0 i nije pronađen u mapama .NET Framework 4.0. 

**Razlučivost/zaobilazno rješenje:**

1. Ako još niste instalirali .NET davatelj za Oracle, [instalirajte ga](http://www.oracle.com/technetwork/topics/dotnet/downloads/) i ponovite scenarija. 
2. Ako primite poruku o pogrešci čak i nakon instalacije davatelja usluge, učinite sljedeće: 
    1. Otvorite konfiguracije računala .NET 2.0 iz mape: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Pretraživanje za **Oracle Data Provider za .NET**, a trebali biste moći pronaći stavku kao što je prikazano u sljedeće ogledne unwn u nastavku poslušajte poništavanjeder **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Kopirajte ovu stavku u datoteku machine.config u sljedećoj mapi v4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config te promijenite verziju koju ćete 4.xxx.x.x.
3.  Instalacija "< ODP.NET instalirani put > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" u predmemoriji sklopova (GAC) tako da pokrenete `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.
