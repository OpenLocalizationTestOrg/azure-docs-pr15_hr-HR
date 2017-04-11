<properties 
    pageTitle="Premještanje podataka iz Amazon Redshift pomoću podataka tvorničke | Microsoft Azure" 
    description="Saznajte kako premjestiti podatke iz Amazon Redshift pomoću tvorničke Azure podataka." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Premještanje podataka iz Amazon Redshift pomoću tvorničke Azure podataka

U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka da biste premjestili podatke iz Amazon Redshift na druge podatke trgovine. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) , što predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i popis izvora/primatelj podataka trgovine.  

Tvorničke podataka trenutno podržava samo premještanje podataka iz Amazon Redshift u drugim spremišta podataka, ali ne i za premještanje podataka s drugim podacima trgovine Amazon Redshift.

## <a name="prerequisites"></a>Preduvjeti

- Ako premještate podataka u spremište lokalnih podataka, dodijeliti pristupnik za upravljanje podacima (korištenje IP adresa na računalu) pristup klaster Amazon Redshift. Upute potražite u članku [autorizacija pristup klaster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) . 
- Ako podataka premještate u spremištu Azure podataka, pročitajte članak [Azure podataka centra IP raspone](https://www.microsoft.com/download/details.aspx?id=41653) na izračunati rasponi IP adresa (uključujući SQL raspona) koristi središta podataka Microsoft Azure. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Najlakši način stvaranja kanal koja se kopira podatke iz Amazon Redshift je za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Pokazuje kako kopirati podatke iz Amazon Redshift spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Primjer: Kopiranje podataka iz Amazon Redshift blobova platforme Azure
Ovaj primjer prikazuje način da biste kopirali podatke iz baze podataka programa Amazon Redshift blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

- Povezane servis vrste [AmazonRedshift](#linked-service-properties).
- Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Za unos [dataset](data-factory-create-datasets.md) vrste [RelationalTable](#dataset-type-properties).
- Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [RelationalSource](#copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka iz rezultata upita u Amazon Redshift blob svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

**Amazon Redshift povezana servisa**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Unos dataset Amazon Redshift**

Postavljanje **"vanjski": true** obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka. Postavljanje tog svojstva na true unosa skup podataka koji je osnovu aktivnosti u kanalu.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
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
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyAmazonRedshiftToBlob",
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
                            "name": "AmazonRedshiftInputDataset"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Svojstva povezane servisa

Sljedeća tablica sadrži opis JSON elementi specifične za servisa Amazon Redshift povezani.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **AmazonRedshift**. | Da |
| poslužitelj | IP adresa ili naziv glavnog računala poslužitelja Amazon Redshift. | Da |
| priključak | Broj priključka TCP koji poslužitelj Amazon Redshift koristi da biste preslušali za povezivanje. | Ne, zadana vrijednost: 5439 |
| baze podataka | Naziv baze podataka Amazon Redshift. | Da |
| korisničko ime | Ime korisnika koji imaju pristup bazi podataka.| Da |
| lozinke | Lozinka za korisnički račun.| Da |


## <a name="dataset-type-properties"></a>Svojstva vrste skup podataka

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **RelationalTable** (koji sadrži skup podataka Amazon Redshift) ima sljedeća svojstva

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| tableName | Naziv tablice u bazi podataka za Amazon Redshift koji se odnosi povezane servisa. | Ne (Ako je naveden je **upit** **RelationalSource** ) | 

## <a name="copy-activity-type-properties"></a>Kopiranje svojstva vrste aktivnosti

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku **typeProperties** aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kada je izvor aktivnosti Kopiraj vrste **RelationalSource** (koji sadrži Amazon Redshift) sljedeća svojstva dostupne su u odjeljku typeProperties:

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- |
| upit | Čitanje podataka pomoću prilagođenog upita. | SQL niz upita. Na primjer: Odaberite * iz MojaTablica. | Ne (Ako je naveden **tableName** **skup podataka** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Mapiranje vrsta za Amazon Redshift

Kao što je rečeno u članku [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) Kopiraj aktivnosti izvodi Automatska vrsta pretvorbi iz sita vrste s sljedeća dva koraka pristup vrste izvora:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Prilikom pomicanja podataka za Amazon Redshift, sljedeće mapiranja će se vrste Amazon Redshift vrste .NET.

Vrsta Redshift Amazon | .NET temelji vrsta
-------------------- | ---------------
SMALLINT | Int16
CIJELI BROJ | Int32
BIGINT | Int64
BROJA DECIMALNIH MJESTA | Broja decimalnih mjesta
REALNI | Jedan
DVOSTRUKE PRECIZNOSTI | Dvaput
BOOLEOVE VRIJEDNOSTI | Niz
CHAR | Niz
VARCHAR | Niz
DATUM | Datum i vrijeme
VREMENSKA OZNAKA | Datum i vrijeme
TEKST | Niz



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.

## <a name="next-steps"></a>Daljnji koraci
Potražite u sljedećim člancima: 
- [Kopiraj aktivnosti vodič](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) za detaljne upute za stvaranje na kanal s Kopiraj aktivnosti. 