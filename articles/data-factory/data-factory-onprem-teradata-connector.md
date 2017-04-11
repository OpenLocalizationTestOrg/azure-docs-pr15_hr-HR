<properties 
    pageTitle="Premještanje podataka iz tvrtke Teradata | Tvorničke Azure podataka" 
    description="Saznajte više o Teradata poveznik za servis tvorničke podataka koji omogućuje premještanje podataka iz baze podataka tvrtke Teradata" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Premještanje podataka iz tvrtke Teradata pomoću tvorničke Azure podataka

U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz tvrtke Teradata s drugom podacima pohraniti. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

Tvorničke podataka podržava povezivanje s lokalnim izvorima Teradata putem pristupnik za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) informacije o pristupnik za upravljanje podacima i detaljne upute za postavljanje pristupnika. 

> [AZURE.NOTE]
Čak i ako na Teradata nalazi se u programa Azure IaaS VM potreban je pristupnika. Možete instalirati pristupnika na isti VM IaaS kao spremište podataka ili na različitim VM dok god pristupnika možete povezati s bazom podataka. 

Tvorničke podataka podržava samo premještanje podataka iz tvrtke Teradata u drugim spremišta podataka, a ne s drugim spremišta podataka da biste Teradata.

## <a name="installation"></a>Instalacija 

Za pristupnik za upravljanje podacima da biste se povezali s bazom podataka tvrtke Teradata, morate instalirati [.NET davatelj podataka za proizvode tvrtke Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) na istom sustavu kao pristupnik za upravljanje podacima.

> [AZURE.NOTE] [Problema](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) potražite u članku otklanjanje poteškoća pristupnika za savjeta za otklanjanje poteškoća veze/pristupnik probleme vezane uz. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Najlakši način stvaranja kanal koja se kopira podatke iz tvrtke Teradata u bilo koji služi za pohranu podataka podržani primatelj je za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirajte podatke iz tvrtke Teradata u spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Primjer: Kopiranje podataka iz tvrtke Teradata blobova platforme Azure

Ovaj primjer prikazuje način da biste kopirali podatke iz baze podataka tvrtke Teradata blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  [Kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka iz rezultata upita u bazi podataka tvrtke Teradata blob svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

Kao prvi korak, instalacija pristupnik za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) su upute.

**Servis za Teradata povezana:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
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


**Unos dataset tvrtke Teradata:**

Uzorak pretpostavlja stvorite tablicu "MojaTablica" u Teradata i sadrži stupac koji se zove "vremenske oznake" za vrijeme niza podataka.

Postavljanje "vanjski": true obavještava servis tvorničke podataka u tablici ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
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
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Kanal s Kopiraj aktivnosti:**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje zaračunava. U kanalu JSON definicija vrsta **izvora** postavljen na **RelationalSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **upita** odabrat ćete podatke u zadnjem satu da biste kopirali.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Svojstva Teradata povezana servisa

Sljedeća tablica sadrži opis JSON elementi specifične za usluge tvrtke Teradata povezani. 

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
Vrsta | Svojstvo vrsta mora biti postavljeno na: **OnPremisesTeradata** | Da
poslužitelj | Naziv poslužitelja Teradata. | Da
authenticationType | Vrsta provjere autentičnosti koji se koristi za povezivanje s bazom podataka tvrtke Teradata. Moguće vrijednosti su: anonimna, Basic i Windows. | Da
korisničko ime | Ako koristite Basic i Windows provjeru autentičnosti, navedite korisničko ime. | ne 
lozinke | Unesite lozinku za korisnički račun koji ste naveli za korisničko ime. | ne 
gatewayName | Naziv pristupnika za servis tvorničke podatke trebali biste koristiti da biste se povezali s bazom podataka tvrtke Teradata lokalni. | Da

Detalje o postavljanju vjerodajnica za lokalni izvor podataka tvrtke Teradata u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="teradata-dataset-type-properties"></a>Svojstva vrste skup podataka tvrtke Teradata

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Trenutno nema svojstava vrsta podržana za skup podataka tvrtke Teradata. 


## <a name="teradata-copy-activity-type-properties"></a>Svojstva za vrste aktivnosti Kopiraj tvrtke Teradata

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kada je izvor vrste **RelationalSource** (koji sadrži Teradata) sljedeća svojstva dostupne su u odjeljku **typeProperties** :

Svojstvo | Opis | Dopuštena vrijednost | Obavezno
-------- | ----------- | -------------- | --------
upit | Čitanje podataka pomoću prilagođenog upita. | SQL niz upita. Na primjer: Odaberite * iz MojaTablica. | Da

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Mapiranje vrsta za proizvode tvrtke Teradata

Kao što je rečeno u članku [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) aktivnosti Kopiraj izvodi Automatska vrsta pretvorbi iz vrste izvora da biste sita vrste s sljedeće pristup 2 koraka:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Kada premještanje podataka tvrtke Teradata sljedeće mapiranja koriste iz tvrtke Teradata vrste .NET vrstu.

Vrsta podataka tvrtke Teradata | Vrsta .NET framework
----------------- | ---------------------------
CHAR | Niz
Clob | Niz
Grafika | Niz
VarChar | Niz
VarGraphic | Niz
Blob | Bajt]
Bajt | Bajt]
VarByte | Bajt]
BigInt | Int64
ByteInt | Int16
Broja decimalnih mjesta | Broja decimalnih mjesta
Dvaput | Dvaput
Cijeli broj | Int32
Broj | Dvaput
SmallInt | Int16
Datum | Datum i vrijeme
Vrijeme | Vremenski raspon
Vrijeme s vremenske Zone | Niz
Vremenska oznaka | Datum i vrijeme
Vremenska oznaka s vremenske Zone | DateTimeOffset
Interval dan | Vremenski raspon
Interval dan na sat | Vremenski raspon
Interval dan minutu | Vremenski raspon
Interval dan sekundu | Vremenski raspon
Interval sat | Vremenski raspon
Interval sat minutu | Vremenski raspon
Interval sat sekundu | Vremenski raspon
Interval minutu | Vremenski raspon
Interval minutu sekundi | Vremenski raspon
Interval drugi | Vremenski raspon
Interval godine | Niz
Interval godine mjesec | Niz
Interval mjesec | Niz
Period(date) | Niz
Period(Time) | Niz
Razdoblje (vrijeme s vremenske Zone) | Niz
Period(TIMESTAMP) | Niz
Razdoblje (Vremenska oznaka s vremenske Zone) | Niz
XML | Niz

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.