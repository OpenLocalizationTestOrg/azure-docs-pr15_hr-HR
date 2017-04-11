<properties 
    pageTitle="Premještanje podataka iz DB2 | Tvorničke Azure podataka" 
    description="Dodatne informacije o korištenju premještanje podataka iz baze podataka DB2 pomoću tvorničke Azure podataka" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Premještanje podataka iz DB2 pomoću tvorničke Azure podataka
U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz DB2 s drugom podacima pohraniti. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

Tvorničke podataka podržava povezivanje s lokalnim DB2 izvorima pomoću pristupnika za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) informacije o pristupnik za upravljanje podacima i detaljne upute za postavljanje pristupnika. 

> [AZURE.NOTE]
Pomoću pristupnika možete povezati DB2 čak i ako se ona nalazi u Azure IaaS VMs. Ako pokušavate povezati instance komponente DB2 smješten u oblak, možete instalirati i instance pristupnika u IaaS VM.

Tvorničke podataka trenutno podržava samo premještanje podataka iz DB2 u drugim spremišta podataka, ali ne i iz drugih podataka trgovine za DB2. 

## <a name="installation"></a>Instalacija 

Pristupnik za upravljanje podacima nudi ugrađene DB2 upravljački program koji podržava sljedeće: 

- SQLAM 9 / 10 / 11
- DB2 za LUW (Linux, Unix, Windows)
- DB2 z-OS i DB2 za li (ili kao / 400)

Zbog toga više ne morate ručno instalirati upravljačke programe kada kopirate podatke iz DB2.

> [AZURE.NOTE] [Problema](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) potražite u članku otklanjanje poteškoća pristupnika za savjeta za otklanjanje poteškoća veze/pristupnik probleme vezane uz. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanala koja se kopira podatke iz baze podataka DB2 najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

Sljedeći primjeri sadrže definicije JSON uzorka koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke iz baze podataka DB2 i spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Primjer: Kopiranje podataka iz DB2 blobova platforme Azure

Ovaj primjer prikazuje način da biste kopirali podatke iz lokalne baze podataka DB2 blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Uzorak kopiranje podataka iz rezultata upita u bazi podataka DB2 blobova platforme Azure zaračunava. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

U prvom koraku instalirati i konfigurirati pristupnik za upravljanje podacima. Upute su u članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) .

**Servis za DB2 povezana:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }


**Azure blobova povezana servisa:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }

**Unos dataset DB2:**

Uzorak pretpostavlja stvorite tablicu "MojaTablica" u DB2 i sadrži stupac koji se zove "vremenske oznake" za vrijeme niza podataka.

Postavljanje "vanjski": true obavještava servis podataka tvorničke ovaj skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka. Obratite pozornost na to da je **Vrsta** postavljena na **RelationalTable**. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
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


**Blobova platforme Azure izlazni skup podataka:**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.

    {
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kanal s Kopiraj aktivnosti:**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **RelationalSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **upita** odabrat ćete podatke iz tablice Narudžbe.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Svojstva DB2 povezana servisa

Sljedeća tablica sadrži opis JSON elementi specifične za servis DB2 povezani. 

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **OnPremisesDB2** | Da |
| poslužitelj | Naziv poslužitelja DB2. | Da |
| baze podataka | Naziv baze podataka DB2. | Da |
| shema | Naziv sheme u bazi podataka. Naziv sheme je velika i mala slova. | ne |
| authenticationType | Vrsta provjere autentičnosti koji se koristi za povezivanje s bazom podataka DB2. Moguće vrijednosti su: anonimna, Basic i Windows. | Da |
| korisničko ime | Ako koristite Basic i Windows provjeru autentičnosti, navedite korisničko ime. | ne |
| lozinke | Unesite lozinku za korisnički račun koji ste naveli za korisničko ime. | ne |
| gatewayName | Naziv pristupnika za servis tvorničke podatke trebali biste koristiti da biste se povezali s bazom podataka za lokalni DB2. | Da |

Detalje o postavljanju vjerodajnica za lokalni izvor podataka DB2 u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) . 


## <a name="db2-dataset-type-properties"></a>Svojstva vrste dataset DB2

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku typeProperties razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste RelationalTable (koji sadrži skup podataka DB2) ima sljedeća svojstva.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| tableName | Naziv tablice u slučaju DB2 baze podataka koji se odnosi povezane servisa. U tableName je velika i mala slova. | Ne (Ako je naveden je **upit** **RelationalSource** ) |

## <a name="db2-copy-activity-type-properties"></a>Svojstva za vrste aktivnosti Kopiraj DB2

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kopiraj aktivnosti, kada je izvor vrste **RelationalSource** (koji sadrži DB2) sljedeća svojstva dostupne su u odjeljku typeProperties:


| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------- | -------------- |
| upit | Čitanje podataka pomoću prilagođenog upita. | SQL niz upita. Na primjer: `"query": "select * from "MySchema"."MyTable""`. | Ne (Ako je naveden **tableName** **skup podataka** )|

> [AZURE.NOTE] Nazive tablice i sheme su velika i mala slova. Obuhvatite nazive u "" (dvostruki navodnik) u upitu.  

**Primjer:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Mapiranje vrsta za DB2
Kao što je rečeno u članku [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) aktivnosti Kopiraj izvodi Automatska vrsta pretvorbi iz vrste izvora da biste sita vrste s sljedeće pristup 2 koraka:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Kada premještanje podacima DB2 sljedeće mapiranja koriste iz vrste DB2 za .NET vrsta.

Vrsta DB2 baze podataka | Vrsta .NET framework 
----------------- | ------------------- 
SmallInt | Int16
Cijeli broj | Int32
BigInt | Int64
Realni | Jedan
Dvaput | Dvaput
Vrijednost s pomičnim zarezom | Dvaput
Broja decimalnih mjesta | Broja decimalnih mjesta
DecimalFloat | Broja decimalnih mjesta
Numeričke | Broja decimalnih mjesta
Datum | Datum i vrijeme
Vrijeme | Vremenski raspon
Vremenska oznaka | Datum i vrijeme
XML | Bajt]
CHAR | Niz
VarChar | Niz
LongVarChar | Niz
DB2DynArray | Niz
Binarni | Bajt]
VarBinary | Bajt]
LongVarBinary | Bajtni]
Grafika | Niz
VarGraphic | Niz
LongVarGraphic | Niz
Clob | Niz
Blob | Bajt]
DbClob | Niz
SmallInt | Int16
Cijeli broj | Int32
BigInt | Int64
Realni | Jedan
Dvaput | Dvaput
Vrijednost s pomičnim zarezom | Dvaput
Broja decimalnih mjesta | Broja decimalnih mjesta
DecimalFloat | Broja decimalnih mjesta
Numeričke | Broja decimalnih mjesta
Datum | Datum i vrijeme
Vrijeme | Vremenski raspon
Vremenska oznaka | Datum i vrijeme
XML | Bajt]
CHAR | Niz


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.


