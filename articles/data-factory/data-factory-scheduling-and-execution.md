<properties
    pageTitle="Planiranje i izvođenja s podacima tvorničke | Microsoft Azure"
    description="Saznajte zakazivanja i izvođenja aspekte Azure podataka tvorničke modelu."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Planiranje na tvorničke podataka i izvođenja
U ovom se članku objašnjava zakazivanja i izvođenja aspekte na tvorničke podataka Azure modelu. 

## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da razumijete osnove modela koncepti aplikacije tvorničke podataka, uključujući aktivnosti, kanali, povezani servisi i skupova podataka. Osnovni koncepti od tvorničke Azure podataka potražite u sljedećim člancima:

- [Uvod u tvorničke podataka](data-factory-introduction.md)
- [Kanali](data-factory-create-pipelines.md)
- [Skupove podataka](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Plan aktivnosti

Pomoću odjeljka rasporeda aktivnosti JSON, možete odrediti ponavljajući raspored za neku aktivnost. Na primjer, možete zakazati aktivnosti svaki sat na sljedeći način:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Primjer rasporeda](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Kao što je prikazano u dijagramu, određivanje rasporeda aktivnosti stvara niz tumbling windows. Tumbling windows su nizovi-veličina, koje se ne preklapaju, neprekinute vremenska razdoblja. Ove windows logičke tumbling aktivnosti se nazivaju *windows aktivnosti*.

Za trenutno izvršni prozor aktivnosti možete pristupiti vremenski interval povezan s prozorom aktivnosti s [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) i [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) varijable sustava u aktivnosti JSON. Ove varijable možete koristiti za različite svrhe u aktivnosti JSON. Ako, na primjer, možete ih koristite da biste odabrali podatke s ulazni i izlazni skupove podataka koji predstavlja vrijeme niza podataka.

Svojstvo **rasporeda** ne podržava isti podsvojstva kao svojstvo **dostupnost** u skup podataka. Detalje potražite u članku [dostupnosti skupa podataka](data-factory-create-datasets.md#Availability) . Primjeri: planiranje pri pomak određeno vrijeme ili postavljanje način da biste poravnali obrada na početak ili kraj interval za prozor aktivnost.

Možete navesti svojstva **raspored** za neku aktivnost, ali to svojstvo nije **obavezno**. Ako navedete svojstvo mora podudarati cadence navedete u definiciji izlazni skup podataka. Trenutno dataset izlaz je što pogoni rasporedu, tako da je izlazni skup podataka morate stvoriti čak i ako se aktivnosti neće proizvesti bilo kakav izlaz. Ako želite da aktivnosti ne svi se unosi, možete preskočiti stvaranje unosa skupu podataka.

## <a name="time-series-datasets-and-data-slices"></a>Vrijeme niza skupova podataka i podataka isječaka

Niz vrijeme podaci neprekinuti niz točaka podataka koji se obično sastoji se od uzastopni mjere unijeli putem vremenski interval. Uobičajeni podataka niza vremena Primjeri senzor podataka i aplikacije telemetrijskih podataka.

S tvorničke podataka provodite vrijeme izvođenja podataka odbacivanja način s aktivnosti. Obično je ponavljajućeg cadence na kojoj se ulaznih podataka stigne poruka i slanje podataka mora biti proizvodi. U ovom cadence je katalog modeliran navođenjem **dostupnost** u skupu podataka na sljedeći način:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Svaku jedinicu podataka potrošena i osnovu za pokretanje aktivnosti zove podataka isječak. Na sljedećem su dijagramu prikazani Primjeri aktivnosti s jednog unos skupa podataka i jedan izlazni skup podataka. Ove skupove podataka imaju **dostupnost** postavljena na svaki sat učestalost.

![Dostupnost rasporeda](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Na prethodni dijagramu prikazani svaki sat isječke podataka za ulazni i izlazni skup podataka. Dijagram prikazuje tri unos isječke koji su spremni za obradu. Aktivnosti 10 11 sati je u tijeku, proizvodnje izlaz isječak 10 11 sati.

Možete pristupiti vremenski interval povezan s trenutnom isječkom se proizvodi u skupu podataka JSON s varijable [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) i [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).

Trenutno tvorničke podataka potreban je raspored naveden u aktivnosti točno odgovara je raspored naveden u **dostupnosti** skupa podataka za izlaz. Dakle, **WindowStart**, **WindowEnd**, **SliceStart**i **SliceEnd** uvijek mapirajte u istom vremenskom razdoblju i jedan izlazni odsječak.

Dodatne informacije o različitim svojstva dostupna za odjeljak o dostupnosti, potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Premještanje podataka iz baze podataka SQL u spremište blobova platforme

Pogledajmo staviti neke stvari zajedno i u akciji stvaranjem kanala koja se kopira podatke iz tablice baze podataka SQL Azure spremište blobova platforme Azure svaki sat.

**Unos: Skup podataka baze podataka SQL Azure**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**Učestalost** je postavljeno na **sat** i **interval** postavite na **1** u odjeljku dostupnost.

**Izlaz: Skup podataka za spremište blobova platforme Azure**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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


**Učestalost** je postavljeno na **sat** i **interval** postavite na **1** u odjeljku dostupnost.



**Aktivnosti: Aktivnosti Kopiraj**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


Uzorak prikazuje raspored aktivnosti i sekcije dostupnosti skupa podataka postavite na svaki sat učestalost. Na primjer prikazuje kako možete koristiti **WindowStart** i **WindowEnd** da biste odabrali odgovarajući podaci za neku aktivnost pokrenuti i njezino kopiranje blob s odgovarajućim **folderPath**. **FolderPath** je parametrizirane imati zasebne mape za svaki sat

Kada se tri odsječaka između 8 – 11 sati izvršavanje podataka u bazi podataka SQL Azure je na sljedeći način:

![Ogledni unos](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Nakon uvodi kanal, blobova platforme Azure se popunjava na sljedeći način:

-   Datoteka mypath/2015/1/1/8/podataka. &lt;Guid&gt;.txt s podacima

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;GUID&gt; je zamijenjena funkcijom stvarne guid. Primjer naziv datoteke: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   Datoteka mypath/2015/1/1/9/podataka. &lt;Guid&gt;.txt s podacima:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   Datoteka mypath/2015/1/1/10/podataka. &lt;Guid&gt;.txt bez podataka.


## <a name="active-period-for-pipeline"></a>Aktivni razdoblje, za kanal

[Stvaranje kanali](data-factory-create-pipelines.md) uvedena pojam aktivni razdoblja za kanal naveli postavljanjem svojstva **Početak** i **Kraj** .

Početni datum za razdoblje aktivnog kanala možete postaviti u prošlosti. Tvorničke podataka automatski izračunava sve isječke podataka (stražnju ispuna) u prošlosti i započinje obrađivati ih.

## <a name="parallel-processing-of-data-slices"></a>Paralelni obrada podataka isječaka
Možete konfigurirati da biste pozadinu ispunjava podataka da biste pokrenuli paralelno postavljanjem **istodobnosti** svojstva u odjeljku pravila aktivnosti JSON. Dodatne informacije o tom svojstvu potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Ponovno pokrenite podataka nije uspjelo isječkom 
Možete nadzirati izvođenja isječaka obogaćenog vizualne način. Pročitajte članak [nadzor i upravljanje kanali pomoću Azure portala blades](data-factory-monitor-manage-pipelines.md) ili [aplikacija za nadzor i upravljanje](data-factory-monitor-manage-app.md) detalje.

Razmislite o sljedećem primjeru, koji pokazuje dvije aktivnosti. Activity1 daje dataset niza vremena s isječaka kao rezultat koji se koriste kao ulaz Activity2 čime se dobiva skup konačni izlaz vrijeme niza podataka.

![Nije uspjelo isječkom](./media/data-factory-scheduling-and-execution/failed-slice.png)

Dijagram prikazuje koje iz tri nedavne isječaka, pojavila se pogreška proizvodnje isječak 9 10 AM za Dataset2. Tvorničke podataka automatski prati ovisnosti za skup podataka niza vremena. Zbog toga ga se ne pokreće aktivnosti pokretanje za isječkom do 9-10 AM.

Alati za nadzor i upravljanje podataka tvorničke omogućuju dubinski analizirati dijagnostičke zapisnike za nije uspjelo isječak da biste lakše pronaći uzrok problema i alata za popravak. Nakon što ste otkloniti problem, jednostavno možete pokrenuti aktivnosti pokrenuti čime se dobiva nije uspjelo odsječak. Dodatne informacije o tome kako ponovno pokrenite i razumijevanje prijelaza stanja za isječke podataka potražite u članku [nadzor i upravljanje kanali pomoću Azure portala blades](data-factory-monitor-manage-pipelines.md) ili [aplikaciju za nadzor i upravljanje njima](data-factory-monitor-manage-app.md).

Nakon što ponovno pokrenite isječak 9-10 AM za **Dataset2**podataka tvorničke započinje Pokreni za isječkom zavisne AM 9 10 na konačni skup podataka.

![Ponovno pokrenite nije uspjelo isječkom](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Pokretanje aktivnosti u nizu
Dvije aktivnosti (pokrenuti jednu aktivnost drugim) možete lanac postavljanjem izlazni skup podataka jedan aktivnosti kao unos dataset druge aktivnosti. Aktivnosti može biti u istoj kanalu ili u drugu kanali. Drugi aktivnosti izvršava samo kada prvoga Završi uspješno.

Na primjer, razmotrite sljedeće slučaj:

1.  Kanal P1 ima A1 aktivnosti koje zahtijeva vanjski unos dataset D1 i daje izlaz dataset D2.
2.  Kanal P2 ima A2 aktivnosti koji zahtijeva unos dataset D2 i daje izlaz dataset D3.

U ovom scenariju aktivnosti A1 i A2 su u različitim kanali. Aktivnost A1 pokreće kada Vanjski podaci dostupni su i zbirka dostigne učestalost zakazanog dostupnost. Aktivnost A2 pokreće kad zakazani isječke iz D2 postaju dostupne i zbirka dostigne učestalost zakazanog dostupnost. Ako je došlo do pogreške na jedan od isječaka u skupu podataka D2, A2 neće pokrenuti za suprotnu dok ne postane dostupna.

Prikaz dijagrama izgleda kao na sljedećem su dijagramu:

![Ulančavanje aktivnosti u dva kanali](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Kao što je rečeno ranije, aktivnosti mogu biti u istoj kanal. Prikaz dijagrama s obje aktivnosti u istom kanalu izgledati na sljedećem su dijagramu:

![Ulančavanje aktivnosti u istom kanalu](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Sekvencijalno Kopiraj
Nije moguće pokrenuti više kopija postupke nakon međusobno način uzastopnih/naručili. Ako, na primjer, možda dvije aktivnosti Kopiraj u kanalu (CopyActivity1 i CopyActivity2) s ulaznih podataka izlaz skupove podataka:   

CopyActivity1

Unos: skup podataka. Izlaz: Dataset2.

CopyActivity2

Unos: Dataset2.  Izlaz: Dataset3.

CopyActivity2 će se pokrenuti samo ako je na CopyActivity1 uspješne i dostupna je Dataset2.

Evo oglednih kanal JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Obratite pozornost na to u primjeru dataset izlaz aktivnosti prvi Kopiraj (Dataset2) je li navedena kao ulaz za druge aktivnosti. Zbog toga drugog aktivnosti pokreće samo kada je spreman dataset Izlaz iz prve aktivnosti.  

U ovom primjeru CopyActivity2 može imati različite unos, kao što su Dataset3, ali navedete Dataset2 kao ulaz CopyActivity2, pa aktivnosti neće se pokrenuti dok CopyActivity1 Završi. Ako, na primjer:

CopyActivity1

Unos: Dataset1. Izlaz: Dataset2.

CopyActivity2

Unosi: Dataset3 Dataset2. Izlaz: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Imajte na umu da u primjeru dvaju skupova podataka unos određeni su klauzulom druge aktivnosti Kopiraj. Kada se određeni su klauzulom više unosa, samo prvi unos skupu podataka koristi se za kopiranje podataka, ali druge skupove podataka koje se koriste kao ovisnosti. CopyActivity2 želite pokrenuti tek nakon što su ispunjeni sljedeći uvjeti:

- CopyActivity1 je uspješno dovršena i dostupna je Dataset2. Ovaj skup podataka ne koristi se kada kopiranje podataka na Dataset4. Samo djeluje kao zakazivanja ovisnosti za CopyActivity2.   
- Dostupna je Dataset3. Ovaj skup podataka predstavlja podatke u koji se kopiraju u odredište.  



## <a name="model-datasets-with-different-frequencies"></a>Model skupove podataka s različitim učestalost

U uzorcima, učestalost za ulazni i izlazni skupova podataka i zakazanog aktivnosti su isti. Neke scenarije potrebna mogućnost čime se dobiva izlaz po učestalosti pojavljivanja razlikuje se od učestalosti jedan ili više unosa. Tvorničke podataka podržava Modeliranje tih scenarija.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Primjer 1: Proizvesti dnevno izvješće o izlaz za unos podataka koji je dostupan svaki sat

Razmislite o scenarij u kojem ste mydomain mjera podatke iz senzori dostupna svaki sat u spremište blobova platforme Azure. Želite proizvesti dnevnih aggregate izvješće s Statistika kao što je srednja vrijednost, maksimum i minimum za dan s [podacima tvorničke vrste hive aktivnosti](data-factory-hive-activity.md).

Evo kako modela scenarij s tvorničke podataka:

**Skup podataka za unos**

Svaki sat ulaznih datoteka ispuštaju se u mapu za navedeni dan. Dostupnost za unos postavljena na **sat** (učestalost: H, interval: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Izlazni skup podataka**

Jedan Izlazna datoteka stvara se svakodnevno u mapu na dan. Dostupnost izlaza postavljen **danom** (učestalost: dan i interval: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktivnost: grozd aktivnosti u u kanalu**

Skripta grozd prima odgovarajuće podatke *datuma i vremena* kao parametar koji koriste varijablu **WindowStart** kao što je prikazano u sljedećim isječka. Skripta grozd koristi ovu varijablu da biste učitali podatke iz odgovarajuću mapu za dan i pokrenuli zbrajanja za generiranje izlaz.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

Sljedeći dijagram prikazuje scenarij iz prikaza vrsta podataka ovisnost zareza.

![Ovisnost podataka](./media/data-factory-scheduling-and-execution/data-dependency.png)

Odsječak izlaz za svaki dan ovisi o 24 svaki sat isječke iz ulaznog skup podataka. Tvorničke podataka formula izračunava te ovisnosti automatski tako da pomoću ulaznih podataka isječke koje se nalaze u istom vremenskom razdoblju kao isječak izlaz treba proizvesti. Ako bilo koji od 24 unos isječaka nije dostupna, podataka tvorničke čeka unos isječak biti jeste li spremni prije pokretanja dnevne aktivnosti pokrenuti.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Primjer 2: Navedite ovisnosti s izrazima i funkcijama tvorničke podataka

Ćemo razmotriti drugi scenarij. Recimo da imate grozd aktivnosti koje obrađuje dva unosa skupova podataka. Jedan od njih svakodnevno sadrži nove podatke, no jedan od njih će nove podatke svaki tjedan. Pretpostavimo da ste htjeli učiniti spoj preko dva unosa i daju se izlazni svaki dan.

Jednostavan pristup u koje tvorničke podataka automatski slika odgovor s desne strane unos isječaka za obradu po poravnanje za izlazne podatke isječak vrijeme razdoblje ne funkcionira.

Morate navesti da svaki pokretanje aktivnosti, tvorničke podataka trebali biste koristiti prošli tjedan podataka odsječak tjedni unos skupu podataka. Pomoću funkcije tvorničke Azure podataka kao što je prikazano u sljedećim isječak implementirati takvo ponašanje.

**Input1: Blobova platforme Azure**

Prvi unos je blobova platforme Azure svakodnevno ažurirati.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Blobova platforme Azure**

Input2 je blobova platforme Azure tjednih ažurirati.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Izlaz: Blobova platforme Azure**

Jedan Izlazna datoteka se stvara svakodnevno u mapi za dan. Dostupnost izlaza postavljen na **dan** (učestalost: dan, interval: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktivnost: grozd aktivnosti u u kanalu**

Aktivnosti grozd uzima dva unosa i daje do izlazne isječak svaki dan. Možete navesti svaki dan izlaz isječak ovise o prethodnom tjednu unos isječak s predlošcima tjednih unos na sljedeći način.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Podatkovne funkcije na tvorničke i sistemske varijable   

Potražite u članku [funkcije tvorničke podataka i sistemske varijable](data-factory-functions-variables.md) popis funkcija i sistemske varijable koji podržava tvorničke podataka.

## <a name="data-dependency-deep-dive"></a>Podaci ovisnost precizno dive

Da biste generirali dataset odsječak na pokretanjem aktivnosti, tvorničke podataka koristi sljedeće *ovisnost model* da biste odredili odnose između skupova podataka potrošena i koje je stvorio aktivnost.

Vremenski raspon unos skupove podataka potrebne da biste generirali dataset isječak izlaz zove *ovisnost razdoblje*.

Za pokretanje aktivnosti generira dataset isječak samo kad isječaka podataka u unos skupova podataka unutar razdoblja ovisnost postanu dostupni. Drugim riječima, sve unos isječke comprising razdoblje ovisnost mora biti u stanju **spremno** za aktivnost pokrenuti čime se dobiva u isječak izlazni skup podataka.

Da biste generirali isječak dataset [**start**, **Krajnji**], funkcije morate mapirati isječak dataset njegov ovisnost razdoblje. Ova funkcija je zapravo formulu koja pretvara početka i završetka isječka dataset početka i završetka razdoblja ovisnosti. Više formally:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F** i **g** mapiranje funkcija koje izračunavaju početka i završetka razdoblja ovisnost za svaku aktivnost za unos.

Kako se vidi u uzorcima, isto kao razdoblja za isječkom podataka prvenstveno je razdoblje ovisnosti. U tim slučajevima tvorničke podataka automatski izračunava unos isječke koji ulaze u razdoblju ovisnosti.  

Ako, na primjer, u uzorku zbrajanja gdje izlaz svakodnevno proizvodi i ulaznih podataka dostupna svaki sat, isječak razdoblje podataka je 24 sata. Tvorničke podataka pronađe odgovarajući svaki sat unos presijeca za ovog vremenskog razdoblja, a čini isječak izlaza ovisi o unos isječak.

Možete unijeti i vlastiti mapiranje za razdoblje ovisnosti kao što je prikazano u uzorku, gdje jednom od ulaza je tjedno, a isječak izlaz svakodnevno proizvodi.

## <a name="data-dependency-and-validation"></a>Ovisnost podataka i provjere valjanosti

Skup podataka može imati definirana pravila provjere valjanosti koja navodi kako možete provjeriti generira isječak izvođenja podataka prije nego što je spremna za potrošnju. Detalje potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) .

U tim slučajevima, kada je isječak je završio izvođenja, status odsječak izlaz mijenja se u **čeka** s substatus **provjere valjanosti**. Kada se potvrđuju isječke, odsječak status se mijenja **u**.

Ako podatke isječak proizvedeno, ali nije prošla provjeru, pokreće aktivnosti za do isječke koje ovise o ovom isječak ne obrađuju.

[Nadzor i upravljanje kanali](data-factory-monitor-manage-pipelines.md) pokriva različitih država podataka isječaka tvorničke podataka.

## <a name="external-data"></a>Vanjski podaci

Skup podataka može biti označen kao vanjski (kao što je prikazano u sljedećim isječak JSON), implying ga je generirao tvorničke podataka. U tom slučaju pravila za skup podataka možete imati dodatne skup parametara s opisom provjere valjanosti i ponovno pokušajte pravila za skup podataka. Opis sva svojstva potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) .

Slično skupove podataka koje je stvorio tvorničke podataka, isječke podataka za vanjske podatke moraju biti jeste li spremni prije nego što se može obraditi zavisne isječke.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Onetime kanal
Možete stvoriti i zakazivanje kanal za povremeno pokretanje (na primjer: zaračunava ili dnevno) unutar vremena početka i završetka navedete u definiciji kanal. Detalje potražite u članku [zakazivanje aktivnosti](#scheduling-and-execution) . Možete stvoriti i kanala koji se pokreće samo jedanput. Da biste to učinili, postavite svojstvo **pipelineMode** u definiciji kanal **onetime** kao što prikazuje sljedeći primjer JSON. Zadane vrijednosti za to svojstvo je **zakazano**.

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Imajte na umu sljedeće:

- Vrijeme **početka** i **završetka** za kanal nisu navedeni.
- **Dostupnost** ulazni i izlazni skupova podataka nije naveden (**Učestalost** i **intervala**), čak i ako se podaci tvorničke koristiti vrijednosti.  
- Prikaz dijagrama prikaz jednokratni kanali. Ovo je zadano ponašanje dizajna.
- Jednokratno kanali nije moguće ažurirati. Možete Kloniraj jednokratni kanal, preimenovati, ažuriranje svojstava i implementacija da biste stvorili drugu.
