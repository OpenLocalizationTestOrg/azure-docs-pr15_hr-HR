<properties 
    pageTitle="Premještanje podataka iz Sybase | Tvorničke Azure podataka" 
    description="Saznajte više o premještanje podataka iz baze podataka Sybase pomoću tvorničke Azure podataka." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Premještanje podataka iz Sybase pomoću tvorničke Azure podataka 

U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz Sybase u drugom spremišta podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

Podatkovnog servisa tvorničke podržava povezivanje s lokalnim Sybase izvorima pomoću pristupnika za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) informacije o pristupnik za upravljanje podacima i detaljne upute za postavljanje pristupnika. 

> [AZURE.NOTE]
> Čak i ako je baza podataka Sybase smješten u programa Azure IaaS VM potreban je pristupnika. Možete instalirati pristupnika na isti VM IaaS kao spremište podataka ili na različitim VM dok god pristupnika možete povezati s bazom podataka. 

Tvorničke podataka trenutno podržava samo premještanje podataka iz Sybase u drugim spremišta podataka, ali ne i iz drugih podataka trgovine sa sustavom Sybase.

## <a name="installation"></a>Instalacija

Za pristupnik za upravljanje podacima da biste se povezali s bazom podataka sustava Sybase, morate instalirati [davatelj podataka za Sybase](http://go.microsoft.com/fwlink/?linkid=324846) na istom sustavu kao pristupnik za upravljanje podacima.

> [AZURE.NOTE] [Problema](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) potražite u članku otklanjanje poteškoća pristupnika za savjeta za otklanjanje poteškoća veze/pristupnik probleme vezane uz. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanala koja se kopira podatke iz baze podataka Sybase svih podržanih primatelj služi za pohranu podataka najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke iz baze podataka Sybase spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Primjer: Kopiranje podataka iz Sybase blobova platforme Azure
Ovaj primjer prikazuje način da biste kopirali podatke iz baze podataka Sybase blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  Liked servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka iz rezultata upita u bazi podataka Sybase blob svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

Kao prvi korak, instalacija pristupnik za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) su upute.

**Servis za Sybase povezana:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Unos dataset Sybase:**

Uzorak pretpostavlja stvorite tablicu "MojaTablica" u Sybase i sadrži stupac koji se zove "vremenske oznake" za vrijeme niza podataka.

Postavljanje "vanjski": true obavještava servis podataka tvorničke ovaj skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka. Obavijest o **vrsti** povezane servisa postavljeno na: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**Kanali s Kopiraj aktivnosti:**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje zaračunava. U kanalu JSON definicija vrsta **izvora** postavljen na **RelationalSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **upita** odabrat ćete podatke iz na DBA. Tablicu Narudžbe u bazi podataka.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Svojstva Sybase povezana servisa

Sljedeća tablica sadrži opis elemenata JSON specifične za servis Sybase povezani.

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
Vrsta | Svojstvo vrsta mora biti postavljeno na: **OnPremisesSybase** | Da
poslužitelj | Naziv poslužitelja Sybase. | Da
baze podataka | Naziv baze podataka sustava Sybase. | Da 
shema  | Naziv sheme u bazi podataka. | ne
authenticationType | Vrsta provjere autentičnosti koji se koristi za povezivanje s bazom podataka sustava Sybase. Moguće vrijednosti su: anonimna, Basic i Windows. | Da
korisničko ime | Ako koristite Basic i Windows provjeru autentičnosti, navedite korisničko ime. | ne
lozinke | Unesite lozinku za korisnički račun koji ste naveli za korisničko ime. |  ne
gatewayName | Naziv pristupnika za servis tvorničke podatke trebali biste koristiti da biste se povezali s bazom podataka Sybase lokalni. | Da 

Detalje o postavljanju vjerodajnica za lokalni izvor podataka Sybase u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="sybase-dataset-type-properties"></a>Svojstva vrste skup podataka Sybase

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku typeProperties razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak **typeProperties** za skup podataka vrste **RelationalTable** (koji sadrži skup podataka Sybase) sadrži sljedeća svojstva:

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
tableName | Naziv tablice u bazu podataka Sybase instanci koji se odnosi povezane servisa. | Ne (Ako je naveden je **upit** **RelationalSource** )

## <a name="sybase-copy-activity-type-properties"></a>Svojstva za vrste aktivnosti Kopiraj Sybase 

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) članka. Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kada je izvor vrste **RelationalSource** (koji sadrži Sybase) sljedeća svojstva dostupne su u odjeljku **typeProperties** :

Svojstvo | Opis | Dopuštena vrijednost | Obavezno
-------- | ----------- | -------------- | --------
upit | Čitanje podataka pomoću prilagođenog upita. | SQL niz upita. Na primjer: Odaberite * iz MojaTablica. | Ne (Ako je naveden **tableName** **skup podataka** )

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Mapiranje vrsta za Sybase

Kao što je rečeno u članku [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) aktivnosti Kopiraj izvodi Automatska vrsta pretvorbi iz vrste izvora da biste sita vrste s sljedeće pristup 2 koraka:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Sybase podržava T SQL i T SQL vrste. Mapiranje tablicu iz sql vrste .NET vrsta, potražite u članku [Poveznik SQL Azure](data-factory-azure-sql-connector.md) članka.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.