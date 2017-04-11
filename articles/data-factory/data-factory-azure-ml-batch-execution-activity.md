<properties 
    pageTitle="Koristite računalo učenje aktivnosti | Microsoft Azure" 
    description="U članku se opisuje kako stvoriti stvaranje predvidljivu kanali pomoću tvorničke Azure podataka i Azure strojnog učenja" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Stvaranje predvidljivu kanali pomoću Azure strojnog učenja aktivnosti   
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Uvod

[Azure strojnog učenja](https://azure.microsoft.com/documentation/services/machine-learning/) omogućuje vam omogućuje stvaranje, testiranje i implementacija rješenja predvidljivu analize. Iz s više razine točku prikaza, ona se mijenja u tri koraka: 

1. **Stvaranje obuka eksperiment**. Ovaj korak postići pomoću Azure ML Studio. ML studio je okruženje za suradnju vizualne razvoj koju koristite za obuka i testiranje predvidljivu analize modela pomoću podataka za obuku.
2. **Ga predvidljivu eksperiment pretvoriti**. Kada se model sadrži je put uvježbavan s postojeće podatke i spremni ste ga koristiti za rezultatu nove podatke, pripremite i pojednostaviti vaše eksperiment za bilježenje rezultata.
3. **Implementirajte ga u obliku web-servisa**. Možete objaviti svoje bilježenja rezultata eksperiment kao servis Azure web. Možete slati podatke modela putem krajnju točku usluge ovom web i primati rezultat predviđanja iz modela.  

Azure tvorničke podataka omogućuje jednostavno stvaranje kanali koje koriste objavljene [Azure strojnog učenja] [ azure-machine-learning] web-servis za predvidljivu analize. Pročitajte članke [Uvod u tvorničke Azure podataka](data-factory-introduction.md) i [Stvaranje vaš prvi kanal](data-factory-build-your-first-pipeline.md) za brzi početak rada sa servisom Azure podataka tvorničke. 

Pomoću **Grupe za izvršavanje aktivnosti** u kanalu na tvorničke Azure podataka, možete pozvati Azure ML web-servisa da biste predviđanja podatke iz grupe. [Pozivanje programa Azure ML web-servisa pomoću izvršavanje aktivnosti obrade](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) sekcije potražite u članku detalje.

S vremenom predvidljivu modelima u ML Azure bilježenje rezultata eksperimenata morati retrained pomoću novi unos skupova podataka. Možete retrain modelu Azure ML iz podataka tvorničke kanal tako da učinite sljedeće: 

1. Stalna obuka (ne predvidljivu eksperiment) Objavi kao web-servisa. U ovom koraku u Azure ML Studio učinite kao da biste otkrili predvidljivu eksperiment kao web-servisa u prethodnom scenariju.
2. Aktivnost Azure ML obradu izvođenja pozvati web-servisa za osposobljavanje pokusa. Zapravo, možete koristiti Azure ML grupe za izvršavanje aktivnosti pozvati obuka web-servisa i bilježenja rezultata web-servisa. 
  
Kada završite s retraining, želite ažurirati bilježenja rezultata web-servisa (predvidljivu eksperiment predstavljeni kao web-servisa) uz upravo obučeni modela. Evo koraka: 

1. Dodajte koji nisu zadani krajnju točku bilježenja rezultata web-servisa. Zadana krajnja točka web-servisa nije moguće ažurirati tako da je potrebno da biste stvorili krajnju točku koji nisu zadani pomoću portala za Azure. Potražite u članku [Stvaranje krajnje točke](../machine-learning/machine-learning-create-endpoint.md) za konceptualnih informacija i proceduralne korake.
2. Ažuriranje postojećih Azure ML povezani servisi za bilježenje rezultata da biste koristili koja nije zadana krajnja točka. Početak korištenja nove krajnja točka za korištenje web-servisa koji će se ažurirati.
3. Koristite **Azure ML ažuriranje resursa aktivnosti** da biste ažurirali web-servisa s upravo obučeni modelom.  

Potražite u članku [Ažuriranje ML Azure modela pomoću resursa aktivnosti ažuriranje](#updating-azure-ml-models-using-the-update-resource-activity) sekcije detalje. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Pozivanje web-servisa pomoću grupe za izvršavanje aktivnosti

Pomoću Azure podataka tvorničke orkestrirali premještanje podataka i obrada i zatim izvedite obradu izvođenja pomoću Azure strojnog učenja. Ovdje su navedeni koraci za najviše razine:

1. Stvaranje programa servisa Azure strojnog učenja povezani. Potrebne su vam sljedeće:
    1. **Zahtjev za URI** za obradu izvođenja API-JA. URI zahtjev možete pronaći tako da kliknete vezu **IZVOĐENJA GRUPE** na web-stranici servisa.
    1. **Ključ za API-JA** za objavljenu web-servisa Azure strojnog učenja. Ključ za API-JA možete pronaći tako da kliknete web-servisa koji ste objavili. 
 2. Pomoću **AzureMLBatchExecution** aktivnosti.

    ![Nadzorna ploča za strojno učenje](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Obradu URI-JA](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenarij: Eksperimenata pomoću Web servisa ulaza/izlaza koji se odnose na podatke u spremište blobova platforme Azure
U ovom scenariju servisa Azure strojnog učenja Web čini predviđanja pomoću podataka iz datoteke u Azure blobova i sprema rezultate za predviđanje u spremište blobova platforme. Sljedeće JSON definira kanal tvorničke podataka s AzureMLBatchExecution aktivnost. Aktivnost sadrži skup podataka **DecisionTreeInputBlob** kao ulaz i **DecisionTreeResultBlob** kao izlaz. **DecisionTreeInputBlob** prosljeđuje kao ulaz web-servisa pomoću JSON svojstva **webServiceInput** . **DecisionTreeResultBlob** je proslijeđena kao na Izlaz web-servisa pomoću JSON svojstva **webServiceOutputs** .  

> [AZURE.IMPORTANT] 
> Ako web-servisa traje više unosa, koristite svojstvo **webServiceInputs** umjesto korištenja **webServiceInput**. U odjeljku [web-servisa zahtijeva više ulaza](#web-service-requires-multiple-inputs) primjer korištenja svojstvo webServiceInputs.
>  
> Skupove podataka koje se pozivaju **webServiceInput**/**webServiceInputs** i **webServiceOutputs** svojstva (u **typeProperties**) i mora biti uključeno u aktivnosti **unose** i **Proizvodi**.
> 
> U svoje eksperiment Azure ML web servis za unos i izlazni priključci i globalni parametara su zadane nazive ("input1", "input2") koje možete prilagoditi. Nazive koji koristite za webServiceInputs, webServiceOutputs i postavke globalParameters mora u potpunosti odgovarati imenima u eksperimenata. Na stranici pomoć za grupe za izvršavanje za Azure ML krajnju točku da biste provjerili očekivani mapiranje možete pogledati opseg zahtjev uzorka. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Samo unose i izlaze aktivnosti AzureMLBatchExecution možete proslijediti kao parametri za web-servisa. Ako, na primjer, u iznad isječak JSON DecisionTreeInputBlob je ulazni aktivnošću AzureMLBatchExecution koji se prenosi kao ulaz web-servisa putem webServiceInput parametar.   

### <a name="example"></a>Primjer

Ovaj primjer koristi pohranu Azure veličini na ulazni i izlazni podataka. 

Preporučujemo da proći [Sastavljanje vaš prvi kanal s podacima tvorničke] [ adf-build-1st-pipeline] vodiča prije prolaze kroz u ovom primjeru. Korištenje uređivača tvorničke podataka da biste stvorili artefakte tvorničke podataka (povezani servisi, skupova podataka, kanala) u ovom primjeru.   
 

1. Stvaranje **povezanih servis** za **Pohranu Azure**. Ako su datoteke ulazni i izlazni u prostor za pohranu za različite račune, morat ćete dva povezana servisa. Evo primjera JSON:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Stvaranje **unosa** Azure podataka tvorničke **skup podataka**. Za razliku od neke druge podatke tvorničke skupova podataka te skupove podataka mora sadržavati vrijednosti **folderPath** i **naziv datoteke** . Možete koristiti particija svake grupe izvršavanja (svaki isječak podataka) za obradu ili jedinstveni unos proizvesti i izlazna datoteka. Možda ćete morati uključiti neke upstream aktivnosti pretvaranje unos u CSV oblik datoteke i njegovo stavljanje u račun za pohranu za svaki isječak. U tom slučaju želite uvrstiti **vanjskih** i **externalData** postavke prikazano u sljedećem primjeru, a vaš DecisionTreeInputBlob bio dataset izlaz različite aktivnosti.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
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
    
    Unos csv datoteke mora imati redak zaglavlja stupca. Ako koristite **Kopiju aktivnosti** da biste stvorili/prebacili u csv spremište blobova platforme, postavite svojstvo primatelj **blobWriterAddHeader** na **true**. Ako, na primjer:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Ako csv datoteka ne sadrži redak zaglavlja, možda ćete vidjeti sljedeće pogreške: **pogreške u aktivnosti: pogreška tijekom čitanja niz. Neočekivani token: StartObject. Put ", redak 1, postavite 1**.
3. Stvaranje **Izlaz** Azure podataka tvorničke **skup podataka**. U ovom se primjeru koristi particija za stvaranje puta jedinstveni izlaz za svaki isječak izvršavanje. Bez particija, aktivnosti će prebrisati datoteku.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Stvaranje **povezanih servisa** vrste: **AzureMLLinkedService**, pruža na API ključ i Modeliranje obradu izvođenja URL.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Na kraju, autor kanal koji sadrži **AzureMLBatchExecution** aktivnost. Tijekom izvođenja kanal izvršava sljedeće korake:
    1. Dohvaća mjesto ulazne datoteke iz vaše unos skupova podataka.
    2. Poziva obradu izvođenja Azure strojnog učenja API-JA
    3. Kopira izlaz izvođenja obrade blob u vašem izlazni skup podataka. 

    > [AZURE.NOTE] Aktivnosti AzureMLBatchExecution može imati nula ili više unosa i jedan ili više izlaza.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Početak** i **Kraj** njih mora biti u [obliku ISO](http://en.wikipedia.org/wiki/ISO_8601). Na primjer: 2014.-10-14T16:32:41Z. Vrijeme **završetka** nije obavezno. Ako ne navedete vrijednost za svojstvo **Završi** , ona se izračunava kao "**Početak + 48 sati.**" Da biste pokrenuli beskonačno kanal, navedite **9999 09 09** vrijednosti za svojstvo **Završi** . Detalje o svojstvima JSON potražite u članku [Referenca za skriptiranje JSON](https://msdn.microsoft.com/library/dn835050.aspx) .

    > [AZURE.NOTE] Određivanje unosa za na AzureMLBatchExecution aktivnosti nije obavezno. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenarij: Eksperimenata pomoću modula za čitanje/pisanje referirati na podatke u različitim storages

Drugi uobičajeni scenarij prilikom stvaranja Azure ML eksperimenata je pomoću modula za čitanje i pisanje. Modul za čitanje koristi da biste učitali podatke u pokusa i modul writer je spremiti podatke iz svoje eksperimenata. Detalje o module za čitanje i pisanje potražite u članku [čitanje](https://msdn.microsoft.com/library/azure/dn905997.aspx) i [Pisanje](https://msdn.microsoft.com/library/azure/dn905984.aspx) na MSDN biblioteke.     

Kada pomoću modula za čitanje i pisanje, dobro je koristiti parametar web-usluge za svako svojstvo te module za čitanje/pisanje. Parametara web omogućuju vam da biste konfigurirali vrijednosti tijekom izvođenja. Na primjer, mogli biste stvoriti pokusa s čitač modul koji koristi baze podataka SQL Azure: XXX.database.windows.net. Kada je postavila web-servisa koji želite omogućiti koje korisnici web-usluge da biste naveli drugi Azure SQL Server pod nazivom YYY.database.windows.net. Da biste omogućili ta vrijednost mora biti konfigurirana tako možete koristiti parametar web-usluge.

> [AZURE.NOTE] Unos servisa web i izlaz razlikuju se od parametara servis za Web. U prvom scenariju ste vidjeti kako ulazni i izlazni možete navesti servisa za Azure ML Web. U ovom scenariju prenesite parametara za web-servisa koji odgovaraju svojstva module za čitanje/pisanje. 

Pogledajmo scenarij za korištenje parametara Web servisa. Imate na Azure strojnog učenja web-usluga uvodi koji koristi čitač modula za čitanje podataka iz jednog izvora podataka podržava Azure strojnog učenja (na primjer: baze podataka SQL Azure). Nakon izvođenja obrade provodi, rezultati zapisuju Writer modul (bazom podataka SQL Azure).  Nema unosa servisa web i izlaza se definiraju na eksperimenata. U ovom slučaju, preporučujemo da konfigurirate odgovarajućeg web servisa parametara za čitanje i pisanje module. Tu konfiguraciju omogućuje čitanje/pisanje moduli mora biti konfigurirana tako prilikom korištenja AzureMLBatchExecution aktivnosti. Navedete Web servisa parametara u odjeljku **globalParameters** u aktivnosti JSON na sljedeći način. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

[Funkcije tvorničke podataka](https://msdn.microsoft.com/library/dn835056.aspx) možete koristiti i u prosljeđivanje vrijednosti za parametre Web servisa kao što je prikazano u sljedećem primjeru:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Servis parametara Web velika i mala slova, stoga nazive koji ste naveli u aktivnost JSON provjerite podudaraju li se one koji prikazuje web-servisa. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Pomoću čitača modula za čitanje podataka iz više datoteka u blobova platforme Azure
Kanali velikih skupova podataka s aktivnosti kao što su Svinja i grozd može proizvesti ili više izlaznih datoteka s nema. Na primjer, kada odredite vanjsku tablicu vrste Hive, podaci za vanjske tablicu vrste Hive moguće pohraniti u spremište blobova platforme Azure s sljedeće 000000_0 naziv. Možete koristiti modul reader u pokusa da biste pročitali više datoteka i namijenjen predviđanja. 

Prilikom korištenja čitača modul u pokusa Azure strojnog učenja, možete odrediti blobova platforme Azure kao ulaz. Datoteke u spremište blobova platforme Azure mogu biti Izlazna datoteka (primjer: 000000_0) koji je stvorio Svinja i grozd skripte na HDInsight. Modul za čitanje omogućuje čitanje datoteke (nema proširenja) tako da konfigurirate na **put do spremnik direktorija/blob**. **Put do spremnik** upućuje na kontejner i **imenik/blob** točke u mapu koja sadrži datoteke kao što je prikazano na sljedećoj slici. Zvjezdica odnosno \*) **koji određuje sve datoteke u spremniku/mapa (to jest, podataka/aggregateddata/godina = mjesec/2014.-6 /\*)** su za čitanje kao dio pokusa.

![Azure Blob svojstva](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Primjer 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Kanal s AzureMLBatchExecution aktivnosti s parametrima servis za Web

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
U primjeru iznad JSON:

- Web-za učenje računala za Azure usluga uvodi koristi čitač i writer modula za čitanje/pisanje podataka s baze podataka SQL Azure. U ovom web-servisa izlaže sljedećih parametara četiri: naziv poslužitelja, naziv baze podataka, poslužitelj korisničko ime i lozinka korisničkog računa poslužitelja za bazu podataka.  
- **Početak** i **Kraj** njih mora biti u [obliku ISO](http://en.wikipedia.org/wiki/ISO_8601). Na primjer: 2014.-10-14T16:32:41Z. Vrijeme **završetka** nije obavezno. Ako ne navedete vrijednost za svojstvo **Završi** , ona se izračunava kao "**Početak + 48 sati.**" Da biste pokrenuli beskonačno kanal, navedite **9999 09 09** vrijednosti za svojstvo **Završi** . Detalje o svojstvima JSON potražite u članku [Referenca za skriptiranje JSON](https://msdn.microsoft.com/library/dn835050.aspx) .

### <a name="other-scenarios"></a>Drugim situacijama

#### <a name="web-service-requires-multiple-inputs"></a>Web-servisa zahtijeva više ulaza
Ako web-servisa traje više unosa, koristite svojstvo **webServiceInputs** umjesto korištenja **webServiceInput**. Skupove podataka koje se pozivaju **webServiceInputs** također moraju biti obuhvaćene aktivnosti **unosa**.
 
U svoje eksperiment Azure ML web servis za unos i izlazni priključci i globalni parametara su zadane nazive ("input1", "input2") koje možete prilagoditi. Nazive koji koristite za webServiceInputs, webServiceOutputs i postavke globalParameters mora u potpunosti odgovarati imenima u eksperimenata. Na stranici pomoć za grupe za izvršavanje za Azure ML krajnju točku da biste provjerili očekivani mapiranje možete pogledati opseg zahtjev uzorka.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Web-servisa ne zahtijeva ulazni

Da biste pokrenuli tijekove za primjer R ili Python skripte, koji može zahtijevati sve unose se poslužite Azure ML obradu izvođenja web-servisi. Ili pokusa može konfigurirati čitač modul koji izlažu sve GlobalParameters. U tom slučaju aktivnosti AzureMLBatchExecution će se konfigurirati na sljedeći način:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web-servisa ne zahtijeva unos/Izlaz
Web-servisa Azure ML obradu izvođenja možda neće imati sve izlazne web-servisa konfiguriran. U ovom primjeru postoji unos web-servisa ni izlazni niti konfigurirane sve GlobalParameters. I dalje postoji u izlaz konfigurirali aktivnosti sam, ali se ne prikazuje kao u webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Čitatelji koristi servis za web i autorima i pokreće aktivnosti samo kada su uspješno druge aktivnosti

Azure ML web servis za čitanje i pisanje module može se konfigurirati za rad sa ili bez sve GlobalParameters. Međutim, možda želite ugraditi pozivi servisa u kanalu koji koristi dataset ovisnosti pozvati servis samo kada se dovrši neke upstream obrada. Neke akcije možete pokrenuti i nakon dovršetka pomoću takvog izvođenja grupe. U tom slučaju možete express ovisnosti pomoću unosa aktivnosti i izlaze, bez imenovanja ih kao web-servisa unosa ili izlaza.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

**Takeaways** su:

-   Ako na krajnjoj točki eksperiment koristi u webServiceInput: predstavlja blob skup podataka i da bi je sve obuhvaćeno unosa aktivnosti i svojstvo webServiceInput. U suprotnom je ispušten svojstvo webServiceInput. 
-   Ako na krajnjoj točki eksperiment koristi webServiceOutput(s): su prikazani su blob skupova podataka i uključeni u izlaze aktivnosti i u svojstvu webServiceOutputs. Aktivnost proizvodi i webServiceOutputs mapiraju se po nazivu svaki poslani u pokusa. U suprotnom je ispušten svojstvo webServiceOutputs.
-   Ako na krajnjoj točki eksperiment izlaže globalParameter(s), dobiju u svojstvu globalParameters aktivnosti kao ključ, parove vrijednosti. U suprotnom je ispušten svojstvo globalParameters. Tipke su velika i mala slova. [Funkcija tvorničke Azure podataka](data-factory-scheduling-and-execution.md#data-factory-functions-reference) može se koristiti u vrijednosti. 
- Dodatne skupove podataka mogu biti obuhvaćene aktivnosti unose i izlaza svojstva, bez referencira typeProperties aktivnosti. Ove skupove podataka upravljaju izvođenja pomoću ovisnosti isječak, ali zanemaruju se inače po AzureMLBatchExecution aktivnosti. 


## <a name="updating-models-using-update-resource-activity"></a>Ažuriranje modela pomoću aktivnosti resursa za ažuriranje
S vremenom predvidljivu modelima u ML Azure bilježenje rezultata eksperimenata morati retrained pomoću novi unos skupova podataka. Kada završite s retraining, želite ažurirati bilježenja rezultata web-servisa s retrained modelom ML. Uobičajeni korake da biste omogućili retraining i ažuriranja Azure ML modeli putem web-servisa su: 

1. Stvaranje pokusa u [Azure ML Studio](https://studio.azureml.net). 
2. Kada ste zadovoljni s modelom, koristite Azure ML Studio za objavljivanje web-servisi za oba na **Poigrajte se obuka** i bilježenje rezultata /**predvidljivu eksperiment**.

U sljedećoj su tablici opisani u ovom primjeru koriste web-usluge.  Potražite u članku [Retrain strojnog učenja modelima programski](../machine-learning/machine-learning-retrain-models-programmatically.md) detalje.

| Vrste web-servisa | Opis 
| :------------------ | :---------- 
| **Obuka web-servisa** | Prima podatke za obuku te stvara obučeni modela. Izlaz iz sustava retraining je .ilearner datoteka u Azure blobova.  **Zadana krajnja točka** automatski se stvara za vas kad objavite pokusa obuka kao web-servisa. Možete stvoriti dodatne krajnje točke, ali u primjeru se koristi samo zadana krajnja točka |
| **Bilježenje rezultata web-servisa** | Prima Primjeri neoznačene podatke i omogućuje predviđanja. Izlaz iz predviđanje može imati različite oblike, kao što je .csv datoteke ili redaka u bazi podataka Azure SQL, ovisno o konfiguraciji pokusa. Zadana krajnja točka automatski se stvara za vas kad objavite predvidljivu pokusa kao web-servisa. Stvorite drugi **koji nisu zadani, a koji se mogu ažurirati krajnjoj točki** pomoću [portala za Azure](https://manage.windowsazure.com). Možete stvoriti dodatne krajnje točke, ali u ovom se primjeru koristi samo jedan koji se mogu ažurirati krajnjoj točki koji nisu zadani. Upute potražite u članku [Stvaranje krajnje točke](../machine-learning/machine-learning-create-endpoint.md) .       
 
Sljedeća slika prikazuje odnos između obuka i bilježenje rezultata krajnje točke u Azure ML. 

![Web-servisi](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


Možete pozvati **obuka web-servisa** pomoću **Azure ML grupe za izvršavanje aktivnosti**. Pozivanje web-servisa za osposobljavanje je isti kao pozivanje na web-servisa Azure ML (bilježenje rezultata web-servisa) za bilježenja rezultata podatke. Prethodne sekcije opisuje pozivanje Azure ML web-servisa iz programa Azure podataka tvorničke kanal detaljno. 
  
Možete pozvati **bilježenje rezultata web-servisa** pomoću **Azure ML ažuriranje resursa aktivnosti** da biste ažurirali web-servisa s upravo obučeni modelom. Kao što je rečeno u gornjoj tablici, morate stvoriti i koristiti koja nije zadana krajnja točka koji se mogu ažurirati. Osim toga, ažurirajte postojeće povezani servisi u na tvorničke podataka da biste koristili koja nije zadana krajnja točka tako da se uvijek koriste najnoviji retrained modela. 

Sljedeći scenarij navedeni su dodatni Detalji. Ima primjer retraining i ažuriranje Azure ML modela iz programa Azure podataka tvorničke kanal. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scenarij: retraining i ažuriranje modelu Azure ML
Ovo poglavlje sadrži ogledne kanal koji koristi **aktivnost Azure ML obradu izvođenja** za retrain modela. Kanal koristi i **resursa za ažuriranje Azure ML aktivnosti** da biste ažurirali modela u bilježenja rezultata web-servisa. U odjeljku također nudi JSON isječci sve povezane usluge, skupova podataka i kanala u primjeru. 

Ovdje je dijagram prikaz kanala uzorka. Kao što vidite, Azure ML grupe za izvršavanje aktivnosti vodi unos obuka i daje obuka izlazna (iLearner datoteka). Aktivnosti resursa za ažuriranje Azure ML vodi ovaj tečaj izlazne i prilagođava modela u bilježenja rezultata web krajnja točka servisa. Aktivnosti resursa za ažuriranje proizvesti bilo kakav izlaz. U placeholderBlob je samo taj izlazni skup podataka koji je potreban servis tvorničke Azure podataka da biste pokrenuli kanal. 

![Dijagram kanal](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure blobova povezana servisa:
Prostor za pohranu Azure sadrži sljedeće podatke:

- Podaci za obuku. Unos podataka za Azure ML obuka web-servisa.  
- datoteka iLearner. Izlaz iz web-servisa za osposobljavanje Azure ML. Datoteka je i unos resursa ažuriranje aktivnošću.  
   
Evo definiciju JSON uzorka povezane usluge: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Unos dataset obuka:
Sljedeće dataset predstavlja podatke u unos obuka za Azure ML obuka web-servisa. Azure ML grupe za izvršavanje aktivnosti uzima ovaj skup podataka kao ulaz. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
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

#### <a name="training-output-dataset"></a>Obuka izlazni skup podataka:
Sljedeće dataset predstavlja Izlazna datoteka iLearner Azure ML obuka web-servisa. Izvršavanje aktivnosti Azure ML obradu daje ovaj skup podataka. Ovaj skup podataka je i unos aktivnošću Azure ML ažuriranje resursa.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Povezane servisa za krajnju točku obuka Azure ML 
Sljedeći isječak JSON definira servis za Azure strojnog učenja povezana koja upućuje na zadane krajnje točke obuka web-servisa. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

U **Azure ML Studio**učinite sljedeće da biste dobili vrijednosti za **mlEndpoint** i **apiKey**:

1. Na lijevom izborniku kliknite **Web-SERVISA** .
2. Kliknite **obuka web-servisa** na popisu web-servisa. 
3. Kliknite Kopiraj pokraj **ključ za API** tekstni okvir. Tipku u međuspremnik zalijepite u uređivaču JSON tvorničke podataka.
4. **Azure ML studio**, kliknite vezu **IZVOĐENJA GRUPE** .
5. Kopirajte **Zahtjev URI** iz odjeljka **zahtjev** i zalijepite ga u uređivaču JSON tvorničke podataka.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Povezane servis za Azure ML koji se mogu ažurirati bilježenja rezultata krajnju točku:
Sljedeći isječak JSON definira servis za Azure strojnog učenja povezana koja upućuje na krajnjoj koji se mogu ažurirati koji nisu zadani bilježenja rezultata web-usluge.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Prije stvaranja i implementacija sustava ML Azure povezane servisa, slijedite korake u članku [Stvaranje krajnje točke](../machine-learning/machine-learning-create-endpoint.md) stvaranje second (koji nisu zadani, a koji se mogu ažurirati) krajnje točke za bilježenja rezultata web-servisa.

Kada stvorite koji se mogu ažurirati krajnju točku koji nisu zadani, učinite sljedeće:

- Kliknite **GRUPE za IZVRŠAVANJE** da biste dobili URI vrijednosti za svojstvo **mlEndpoint** JSON.
- Kliknite vezu **AŽURIRAJ RESURSA** za vrijednost URI za svojstvo **updateResourceEndpoint** JSON. Ključ za API nalazi se na krajnjoj točki samoj stranici (u donjem desnom kutu). 

![koji se mogu ažurirati krajnje točke](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Rezervirano mjesto za izlazni skup podataka:
Aktivnosti Azure ML ažuriranje resursa generirati bilo kakav izlaz. Azure podataka tvorničke zauzima izlazni skup podataka na pogon raspored na kanal. Dakle, u ovom primjeru koristimo sustava/rezervirano mjesto za skup podataka.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Kanal
Kanal sadrži dvije aktivnosti: **AzureMLBatchExecution** i **AzureMLUpdateResource**. Azure ML grupe za izvršavanje aktivnosti uzima podatke obuka kao ulaz te stvara datoteku iLearner kao u izlaz. Aktivnost poziva web-servisa obuka (tečaj eksperiment predstavljeni kao web-servisa) s podacima za unos obuka i prima datoteku ilearner iz na webservice. U placeholderBlob je samo taj izlazni skup podataka koji je potreban servis tvorničke Azure podataka da biste pokrenuli kanal. 

![kanal dijagrama](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Čitanje i pisanje module

Uobičajeni scenarij za korištenje parametara servisa Web je korištenje čitatelji SQL Azure i autorima. Modul za čitanje koristi se za podatke učitali u pokusa iz servisa za upravljanje podacima izvan Azure strojnog učenja Studio. Modul za pisanje je spremiti podatke iz svoje eksperimenata u servisa za upravljanje podacima izvan Azure strojnog učenja Studio.  

Detalje o čitač writer Azure Blob-Azure SQL potražite u članku [čitanje](https://msdn.microsoft.com/library/azure/dn905997.aspx) i [Pisanje](https://msdn.microsoft.com/library/azure/dn905984.aspx) na MSDN biblioteke. U primjeru u prethodnom odjeljku koristi čitač blobova platforme Azure i writer blobova platforme Azure. U ovom se odjeljku opisuje pomoću čitača Azure SQL i writer Azure SQL.


## <a name="frequently-asked-questions"></a>Najčešća pitanja

**Q:** Mogu imati više datoteka koje generira Moje kanali velikih skupova podataka. Mogu li koristiti aktivnosti AzureMLBatchExecution za rad s datotekama?

**A:** Da. U odjeljku **čitač modul za čitanje podataka iz više datoteka u blobova platforme Azure** detalje. 

## <a name="azure-ml-batch-scoring-activity"></a>Bilježenje rezultata aktivnosti obrade Azure ML
Ako koristite aktivnost **AzureMLBatchScoring** za integraciju s Azure strojnog učenja, preporučujemo da koristite najnoviju **AzureMLBatchExecution** aktivnosti. 

Aktivnosti AzureMLBatchExecution uvedena u kolovozu 2015 izdanju Azure SDK i Azure PowerShell.

Ako želite da biste nastavili koristiti AzureMLBatchScoring aktivnosti, nastavite čitanje kroz ovaj odjeljak.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure bilježenje rezultata ML obradu aktivnosti pomoću Azure prostora za pohranu za unos-Izlaz 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Parametri servis za web
Da biste odredili vrijednosti za parametre Web servisa, dodajte sekcije **typeProperties** **AzureMLBatchScoringActivty** sekciju u kanalu JSON kao što je prikazano u sljedećem primjeru: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


[Funkcije tvorničke podataka](https://msdn.microsoft.com/library/dn835056.aspx) možete koristiti i u prosljeđivanje vrijednosti za parametre Web servisa kao što je prikazano u sljedećem primjeru:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Servis parametara Web velika i mala slova, stoga nazive koji ste naveli u aktivnost JSON provjerite podudaraju li se one koji prikazuje web-servisa. 

## <a name="see-also"></a>Vidi također

- [Azure članak na blogu: prvi koraci s tvorničke Azure podataka i Azure strojno učenje](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
