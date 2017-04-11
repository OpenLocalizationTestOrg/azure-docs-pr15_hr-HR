<properties 
    pageTitle="Premještanje podataka iz MySQL | Tvorničke Azure podataka" 
    description="Saznajte više o premještanje podataka iz baze podataka MySQL pomoću tvorničke Azure podataka." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Premještanje podataka iz MySQL pomoću tvorničke Azure podataka

U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz MySQL s drugom podacima pohraniti. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

Podatkovnog servisa tvorničke podržava povezivanje s lokalnim MySQL izvorima pomoću pristupnika za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) informacije o pristupnik za upravljanje podacima i detaljne upute za postavljanje pristupnika. 

> [AZURE.NOTE] Pristupnik je potrebna čak i ako je baza podataka MySQL smješten u Azure IaaS virtualnog računala (VM). Možete instalirati pristupnika na istom VM kao spremište podataka ili na različitim VM dok god pristupnika možete povezati s bazom podataka.  

Tvorničke podataka trenutno podržava samo premještanje podataka iz MySQL u drugim spremišta podataka, ali ne za premještanje podataka iz drugih trgovina podataka MySQL.

## <a name="installation"></a>Instalacija 
Za pristupnik za upravljanje podacima da biste se povezali s bazom podataka MySQL, morate instalirati [Poveznik MySQL/neto 6.6.5 za Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) na istom sustavu kao pristupnik za upravljanje podacima.

> [AZURE.NOTE] [Problema](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) potražite u članku otklanjanje poteškoća pristupnika za savjeta za otklanjanje poteškoća veze/pristupnik probleme vezane uz. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Najlakši način stvaranja kanal koja se kopira podatke iz baze podataka MySQL u bilo koji služi za pohranu podataka podržani primatelj je za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal. Vodič s detaljnim uputama u članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) . Podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Primjer: Kopiranje podataka iz MySQL blobova platforme Azure
Ovaj primjer prikazuje način da biste kopirali podatke iz lokalne baze podataka MySQL blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  

> [AZURE.IMPORTANT] Ovaj primjer nudi JSON isječci. Ne sadrži detaljne upute za stvaranje tvorničke podataka. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) detaljne upute. 
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka iz rezultata upita u bazi podataka MySQL blob zaračunava. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

Kao prvi korak, instalacija pristupnik za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) su upute. 

**Servis za MySQL povezana**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**Unos u skupu podataka MySQL**

Uzorak pretpostavlja stvorite tablicu "MojaTablica" u MySQL i sadrži stupac pod nazivom "timestampcolumn" za vrijeme niza podataka.

Postavljanje "vanjski": "true" obavještava servis tvorničke podataka u tablici ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }



**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **RelationalSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **upita** odabrat ćete podatke u zadnjem satu da biste kopirali.
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Svojstva MySQL povezana servisa

Sljedeća tablica sadrži opis JSON elementi specifične za servis MySQL povezani.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **OnPremisesMySql** | Da |
| poslužitelj | Naziv poslužitelja MySQL. | Da |
| baze podataka | Naziv baze podataka MySQL. | Da | 
| shema  | Naziv sheme u bazi podataka. | ne | 
| authenticationType | Vrsta provjere autentičnosti koji se koristi za povezivanje s bazom podataka MySQL. Moguće vrijednosti su: anonimna, Basic i Windows. | Da | 
| korisničko ime | Ako koristite Basic i Windows provjeru autentičnosti, navedite korisničko ime. | ne | 
| lozinke | Unesite lozinku za korisnički račun koji ste naveli za korisničko ime. | ne | 
| gatewayName | Naziv pristupnika za servis tvorničke podatke trebali biste koristiti da biste se povezali s bazom podataka MySQL lokalnog. | Da |

Detalje o postavljanju vjerodajnica za lokalni izvor podataka MySQL u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="mysql-dataset-type-properties"></a>Svojstva vrste skup podataka MySQL

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **RelationalTable** (koji sadrži skup podataka MySQL) ima sljedeća svojstva

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| tableName | Naziv tablice u instanci baze podataka MySQL koji se odnosi povezane servisa. | Ne (Ako je naveden je **upit** **RelationalSource** ) | 

## <a name="mysql-copy-activity-type-properties"></a>Svojstva za vrste aktivnosti Kopiraj MySQL

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, a zatim ulazni i izlazni tablice su pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku **typeProperties** aktivnosti s druge strane razlikuju uz svaku vrstu aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kada je izvor u kopiju aktivnosti vrste **RelationalSource** (koji sadrži MySQL) sljedeća svojstva dostupne su u odjeljku typeProperties:

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- |
| upit | Čitanje podataka pomoću prilagođenog upita. | SQL niz upita. Na primjer: Odaberite * iz MojaTablica. | Ne (Ako je naveden **tableName** **skup podataka** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Mapiranje vrsta za MySQL

Kao što je rečeno u članku [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) Kopiraj aktivnosti izvodi Automatska vrsta pretvorbi iz sita vrste s sljedeća dva koraka pristup vrste izvora:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Kada premještanje podataka MySQL sljedeće mapiranja koriste vrste MySQL u .NET vrste.

| Vrsta baze podataka MySQL | Vrsta .NET framework |
| ------------------- | ------------------- | 
| bigint nepotpisane | Broja decimalnih mjesta |
| bigint | Int64 |
| bitne | Broja decimalnih mjesta |
| blob | Bajt] |
| booleovom |  Booleove vrijednosti | 
| CHAR | Niz |
| Datum | Datum i vrijeme |
| Datum i vrijeme | Datum i vrijeme |
| broja decimalnih mjesta | Broja decimalnih mjesta |
| dvostruke preciznosti | Dvaput |
| Dvostruki | Dvaput |
| Redni broj | Niz |
| vrijednost s pomičnim zarezom | Jedan |
| INT nepotpisane | Int64 |
| INT | Int32 |
| cijeli broj nepotpisane | Int64 |
| cijeli broj | Int32 | 
| Dugi varbinary | Bajt] |
| Dugi varchar | Niz |
| longblob | Bajt] |
| LONGTEXT | Niz | 
| mediumblob |  Bajt] | 
| mediumint nepotpisane | Int64 |
| mediumint | Int32 | 
| mediumtext | Niz |
| brojčani | Broja decimalnih mjesta |
| realni |  Dvaput |
| Postavljanje | Niz |
| SMALLINT nepotpisane | Int32 |
| SMALLINT | Int16 |
| tekst | Niz |
| vrijeme | Vremenski raspon |
| Vremenska oznaka | Datum i vrijeme |
| tinyblob | Bajt] |
| tinyint nepotpisane |  Int16 | 
| tinyint | Int16 |
| tinytext | Niz | 
| VARCHAR | Niz | 
| godine | INT | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.

