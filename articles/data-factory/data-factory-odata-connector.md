<properties 
    pageTitle="Premještanje podataka iz izvora OData | Tvorničke Azure podataka" 
    description="Saznajte više o premještanje podataka iz izvora OData pomoću tvorničke Azure podataka." 
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
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Premještanje podataka iz za OData izvora pomoću tvorničke Azure podataka
U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz izvora OData drugi spremišta podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

> [AZURE.NOTE] U ovom OData poveznik podrška za kopiranje podataka iz obje oblaka OData i lokalnih izvora OData. Za drugu mogućnost, morate instalirati pristupnik za upravljanje podacima. Potražite u članku [Premještanje podataka između lokalnog i oblaka](data-factory-move-data-between-onprem-and-cloud.md) članak detalje o pristupnik za upravljanje podacima.

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanal koja se kopira podatke iz izvora OData najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

Sljedeći primjeri sadrže definicije JSON uzorka koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke iz izvora OData blobova Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.


## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Primjer: Kopiranje podataka iz izvora OData blobova platforme Azure

Ovaj primjer prikazuje način da biste kopirali podatke iz izvora OData spremište blobova platforme Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OData](#odata-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [ODataResource](#odata-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [RelationalSource](#odata-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopira podataka iz upita prema izvora OData da biste blobova platforme Azure svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

**OData povezana servisa** U ovom se primjeru koristi osnovnu provjeru autentičnosti. Sekciji [OData povezani servisa](#odata-linked-service-properties) potražite u članku za različite vrste provjere autentičnosti možete koristiti. 

    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
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

**Unos dataset OData**

Postavljanje "vanjski": "true" obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.
    
    {
        "name": "ODataDataset",
        "properties": 
        {
            "type": "ODataResource",
            "typeProperties": 
            {
                "path": "Products" 
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3               
            }
        }
    }

Određivanje **put** u skupu podataka definicija nije obavezno. 


**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **RelationalSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **upita** odabire najnovije (najnoviji) podatke iz izvora OData.
    
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
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
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


Određivanje **upita** u definiciji kanal nije obavezno. Je **URL** servis tvorničke podataka koji se koristi za dohvaćanje podataka: URL naveden u servis povezane (obavezno) + put naveden u skupu podataka (neobavezno) + upita u kanalu (neobavezno). 

## <a name="odata-linked-service-properties"></a>OData povezana svojstva servisa

Sljedeća tablica sadrži opis elemenata JSON specifične za usluge OData povezani.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **OData** | Da |
| URL-a| URL OData servisa. | Da |
| authenticationType | Vrsta provjere autentičnosti koji se koristi za povezivanje s OData izvor. <br/><br/> Za oblak OData mogućih vrijednosti su anonimni i Basic. Za lokalni OData, mogući su vrijednosti anonimno, Basic i Windows. | Da | 
| korisničko ime | Ako koristite osnovnu provjeru autentičnosti, navedite korisničko ime. | Da (samo ako koristite osnovnu provjeru autentičnosti) | 
| lozinke | Unesite lozinku za korisnički račun koji ste naveli za korisničko ime. | Da (samo ako koristite osnovnu provjeru autentičnosti) | 
| gatewayName | Naziv pristupnika za servis tvorničke podatke trebali biste koristiti za povezivanje sa servisom OData lokalnog. Navedite samo ako kopirate podatke iz izvora OData lokalno. | ne |

### <a name="using-basic-authentication"></a>Korištenje Osnovna provjera autentičnosti

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
                "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Korištenje anonimna provjera autentičnosti
    
    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Pomoću provjere autentičnosti sustava Windows pristupanja lokalnim izvorom OData

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
                "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }



## <a name="odata-dataset-type-properties"></a>Svojstva vrste dataset OData

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **ODataResource** (koji sadrži skup podataka OData) ima sljedeća svojstva

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| put | Put do OData resursa | ne | 

## <a name="odata-copy-activity-type-properties"></a>Svojstva vrste OData Kopiraj aktivnosti

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kada je izvor vrste **RelationalSource** (koji sadrži OData) sljedeća svojstva dostupne su u odjeljku typeProperties:

| Svojstvo | Opis | Primjer | Obavezno |
| -------- | ----------- | -------------- | -------- |
| upit | Čitanje podataka pomoću prilagođenog upita. | "? $select = naziv, opis i $top = 5" | ne | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>Mapiranje vrsta za OData

Kao što je rečeno u članku [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) Kopiraj aktivnosti izvodi Automatska vrsta pretvorbi iz vrste izvora da biste sita vrste s sljedeće pristup 2 koraka:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Kada se pohranjuje premještanje podataka iz OData podataka, vrste podataka OData mapiraju se vrste .NET.


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.

