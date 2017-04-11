<properties 
    pageTitle="Premještanje podataka iz lokalnog HDFS | Tvorničke Azure podataka" 
    description="Saznajte kako premjestiti podatke iz lokalnog HDFS pomoću tvorničke Azure podataka." 
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

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Premještanje podataka iz lokalnog HDFS pomoću tvorničke Azure podataka
U ovom se članku opisuje način možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz programa HDFS lokalnog u drugom spremišta podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koji predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih spremište.

Tvorničke podataka trenutno podržava samo premještanje podataka iz programa HDFS lokalnog u drugim spremišta podataka, ali ne za premještanje podataka iz trgovine drugih podataka na lokalni HDFS.


## <a name="enabling-connectivity"></a>Omogućivanje povezivanje
Podatkovnog servisa tvorničke podržava povezivanje s lokalnim HDFS pomoću pristupnika za upravljanje podacima. U članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) informacije o pristupnik za upravljanje podacima i detaljne upute za postavljanje pristupnika. Pomoću pristupnika možete povezati HDFS čak i ako se hostira na VM IaaS Azure. 

Dok pristupnika možete instalirati na tom računalu lokalnog ili VM Azure kao u HDFS, preporučujemo da instalirate pristupnika na na zasebnom strojno/Azure IaaS VM. Imate pristupnika na zasebnom računalo smanjuje Nadmetanje resursa i poboljšavaju performanse. Kada instalirate pristupnika na zasebnom računalo, računalo mora biti možete pristupiti na računalu s na HDFS. 


## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Najlakši način stvaranja kanal koja se kopira podatke iz lokalnih HDFS je za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

Sljedeći primjeri sadrže definicije JSON uzorka koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke iz programa HDFS lokalnog blobova Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Primjer: Kopirajte podatke iz lokalnih HDFS blobova platforme Azure

Ovaj primjer pokazuje kako kopirati podatke iz programa HDFS lokalnog spremište blobova platforme Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [FileShare](#hdfs-dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [FileSystemSource](#hdfs-copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka iz programa HDFS lokalnog blobova platforme Azure svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

Postavljanje pristupnika za upravljanje podacima u prvom koraku. Upute u članku [Premještanje podataka između lokalne lokacije i oblaka](data-factory-move-data-between-onprem-and-cloud.md) . 

**HDFS povezana servisa** U ovom se primjeru koristi provjeru autentičnosti sustava Windows. Sekciji [HDFS povezani servisa](#hdfs-linked-service-properties) potražite u članku za različite vrste provjere autentičnosti možete koristiti. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
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

**HDFS unos skup podataka** Ovaj skup podataka koji se odnosi na mapu HDFS DataTransfer/UnitTest /. Kanal kopira sve datoteke u ovu mapu na odredište. 

Postavljanje "vanjski": "true" obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje ove ulazni i izlazni skupova podataka, a zatim je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **FileSystemSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **upita** odabrat ćete podatke u zadnjem satu da biste kopirali.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Svojstva HDFS povezane servisa

Sljedeća tablica sadrži opis elemenata JSON specifične za HDFS povezana servisa.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **Hdfs** | Da | 
| URL-a | URL-a na HDFS | Da |
| encryptedCredential | [Novo AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) Izlaz iz programa access vjerodajnica. | ne |
| korisničko ime | Provjera autentičnosti korisničko ime za Windows. | Da (za provjeru autentičnosti sustava Windows)
| lozinke | Lozinka za provjeru autentičnosti sustava Windows. | Da (za provjeru autentičnosti sustava Windows)
| authenticationType | Windows, ili anonimnih. | Da |
| gatewayName | Naziv pristupnika za servis tvorničke podatke trebali biste koristiti da biste se povezali s HDFS. | Da |   

Detalje o postavljanju vjerodajnica za lokalni HDFS u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="using-anonymous-authentication"></a>Korištenje anonimna provjera autentičnosti

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Pomoću provjere autentičnosti sustava Windows
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>Svojstva vrste HDFS skup podataka

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **FileShare** (koji sadrži skup podataka HDFS) ima sljedeća svojstva

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
folderPath | Put do mape. Primjer:`myfolder`<br/><br/>Koristiti prespojni znak ' \ ' za posebne znakove u nizu. Na primjer: folder\subfolder, navedite mapu\\\\podmape i d:\samplefolder, navedite d:\\\\samplefolder.<br/><br/>To svojstvo, možete kombinirati s **partitionBy** da bi putevima mapa temelju isječak početak i kraj datuma-vremena. | Da
Naziv datoteke | Ako želite da se u tablici da biste se pozvali na određenom datotekom u mapi, navedite naziv datoteke u **folderPath** . Ako ne navedete bilo koja vrijednost za ovo svojstvo tablice upućuje na sve datoteke u mapi.<br/><br/>Kada se naziv datoteke za skup podataka za izlaz nije naveden, naziv datoteke generirani bio u nastavku ovaj oblik: <br/><br/>Podaci. <Guid>.txt (na primjer:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | ne
partitionedBy | partitionedBy se može koristiti da biste odredili dinamičke folderPath naziv datoteke za vrijeme niza podataka. Primjer: folderPath s parametrima za svaki sat podataka. | ne
fileFilter | Navedite filtar koji će se koristiti da biste odabrali podskup datoteka na folderPath umjesto sve datoteke. <br/><br/>Dopuštene su vrijednosti: `*` (više znakova) i `?` (pojedinačni znak).<br/><br/>Primjeri 1:`"fileFilter": "*.log"`<br/>Primjer 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Napomena**: fileFilter je primjenjivo za unos FileShare skup podataka | ne
| Oblikovanje | Podržane su sljedeće vrste oblika: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**i **ParquetFormat**. Postavite svojstvo **Vrsta** u odjeljku oblik na jednu od ovih vrijednosti. [Određivanje TextFormat](#specifying-textformat), [Pri određivanju AvroFormat](#specifying-avroformat), [Određivanje JsonFormat](#specifying-jsonformat), [Određivanje OrcFormat](#specifying-orcformat)i [Određivanje ParquetFormat](#specifying-parquetformat) sekcije detalje potražite u članku. Ako želite kopirati datoteke kao-je između utemeljenih na datotekama trgovine (binarni kopiranje), možete preskočiti u odjeljku oblik i definicije ulazni i izlazni skup podataka. | ne 
| Sažimanje | Navedite vrstu i razina kompresije za podatke. Podržane vrste su: **GZip**, **Deflate**i **BZip2** i podržanim razine: **Optimal** i **najbrže**. Trenutno postavki sažimanja nisu podržani za podatke u **AvroFormat** ili **OrcFormat**. Dodatne informacije potražite u članku [podrška za sažimanje](#compression-support) sekciji.  | ne |



> [AZURE.NOTE] Naziv datoteke i fileFilter nije moguće koristiti istodobno.


### <a name="using-partionedby-property"></a>Pomoću svojstva partionedBy

Kao što je rečeno u prethodnom odjeljku, možete odrediti dinamičke folderPath naziv datoteke za vrijeme niza podataka pomoću partitionedBy. To možete učiniti pomoću makronaredbe tvorničke podataka i sustava varijablu SliceStart, SliceEnd koje označavaju logičke vremensko razdoblje podataka isječak. 

Da biste saznali više o skupova vrijeme niz podataka, Planiranje i isječke, potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md), [Raspored i izvršavanje](data-factory-scheduling-and-execution.md)i [Stvaranje kanali](data-factory-create-pipelines.md) članke. 

#### <a name="sample-1"></a>Primjer 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

U ovom primjeru {isječak} je zamijenjena funkcijom vrijednost varijable tvorničke podataka sustava SliceStart u obliku (YYYYMMDDHH) naveden. U SliceStart se odnosi na početak isječak. Na folderPath razlikuje za svaki isječak. Na primjer: wikisampledataout/wikidatagateway/2014100103 ili wikisampledataout/wikidatagateway/2014100104.

#### <a name="sample-2"></a>Primjer 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

U ovom primjeru u zasebnom varijabli koje koriste folderPath i naziv svojstva se izdvajaju godinu, mjesec, dan i vrijeme SliceStart.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Svojstva vrste aktivnosti HDFS kopiju

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kopiraj aktivnosti, kada je izvor vrste **FileSystemSource** sljedeća svojstva dostupne su u odjeljku typeProperties:

**FileSystemSource** podržava sljedeća svojstva:

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- |
| rekurzivne | Upućuje na to je li podatke čitanje rekurzivno iz mape sub ili samo u određenu mapu. | TRUE, False (zadano)| ne |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.

