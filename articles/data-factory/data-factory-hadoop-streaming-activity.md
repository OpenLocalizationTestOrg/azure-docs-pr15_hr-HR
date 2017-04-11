<properties 
    pageTitle="Hadoop strujanje aktivnosti" 
    description="Saznajte kako koristiti Hadoop strujanje aktivnosti u na tvorničke Azure podataka za pokretanje programa Hadoop strujanje na na – zahtjev/vaše vlastite klaster za HDInsight." 
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
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop strujanje aktivnosti
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)

Možete koristiti aktivnosti HDInsightStreamingActivity pozivanje Hadoop strujanje zadatak iz programa Azure podataka tvorničke kanal. Sljedeći isječak JSON prikazuje sintaksu za korištenje na HDInsightStreamingActivity u datoteci JSON kanal. 

HDInsight strujanje aktivnosti u podataka tvorničke [kanal](data-factory-create-pipelines.md) izvršava Hadoop strujanje programe na [vlastitu](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ili [osvježavati](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) klaster HDInsight/Linux utemeljen na sustavu Windows. U ovom se članku sastavlja na članak [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) koja predstavlja Općenito pregled i transformaciju podataka te aktivnosti podržani transformaciju.

## <a name="json-sample"></a>Ogledna JSON
HDInsight klaster automatski popunjen primjer programa (wc.exe i cat.exe) i podataka (davinci.txt). Prema zadanim postavkama, naziv klaster sam je naziv spremnika koji koriste klaster HDInsight. Ako, na primjer, ako je naziv klaster myhdicluster, naziv spremnika blob povezanog bio bi myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Imajte na umu sljedeće točke:

1. Postavite **linkedServiceName** naziv povezane servis koji upućuje na svoj klaster HDInsight na kojem se izvodi strujanje mapreduce posao.
2. Postavljanje vrste aktivnosti da biste **HDInsightStreaming**.
3. **Mapiranje** svojstva Navedite naziv mapiranje izvršne datoteke. U ovom primjeru cat.exe je izvršna mapiranje.
4. Svojstvo **reducer** Navedite naziv reducer izvršne datoteke. U ovom primjeru wc.exe je reducer izvršne datoteke.
5. Za **unos** svojstvo vrsta navedite ulazne datoteke (uključujući lokacije) na mapiranje. U ovom primjeru: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample spremnik blob, podataka/primjer/Gutenberg je mapa, a davinci.txt blob-om.
6. Za svojstvo vrsta **izlaza** odredite Izlazna datoteka (uključujući lokacije) na reducer. Izlaz iz posao Hadoop strujanje zapisuje na mjesto navedeno za to svojstvo.
7. U odjeljku **filePaths** navedite putove izvršne datoteke mapiranje i reducer. U ovom primjeru: "adfsample/example/apps/wc.exe" adfsample spremnik blob, primjer/aplikacija je mapa, a wc.exe izvršnu datoteku.
8. Svojstvo **fileLinkedService** navedite servisa Azure prostora za pohranu povezana koji predstavlja Azure pohrane koja sadrži navedene u odjeljku filePaths datoteke.
9. Svojstvo **argumente** navedite argumente za strujanje posao.
10. Svojstvo **getDebugInfo** je neobavezni element. Ako je postavljen pogreške, zapisnike preuzimaju se samo na pogreške. Ako je postavljen na sve, zapisnika uvijek se preuzimaju bez obzira na stanje izvršavanja.

> [AZURE.NOTE] Kao što je prikazano u primjeru, navedite skup podataka za izlaz Hadoop strujanje aktivnosti za svojstvo **Proizvodi** . U ovom dataset je samo taj skup podataka koji je potreban za Poticanje raspored za kanal. Nije potrebno da biste naveli bilo koji unos dataset aktivnosti za svojstvo **unosa** .  

    
## <a name="example"></a>Primjer
Kanal u prikazu pokreće program karte/smanjivanje strujanje za Brojanje riječi na svoj klaster Azure HDInsight. 

### <a name="linked-services"></a>Povezani servisi

#### <a name="azure-storage-linked-service"></a>Azure servis za pohranu povezana
Prvo, stvorite povezani servisa za povezivanje Azure prostora za pohranu koji koriste klaster Azure HDInsight na tvorničke Azure podataka. Ako ste kopirajte i zalijepite sljedeći kod, ne zaboravite da biste zamijenili naziv računa i ključ za račun s imenom i ključ Azure prostora za pohranu. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight povezana servisa
Nakon toga stvorite povezani servisa da biste se povezali svoj klaster Azure HDInsight tvorničke Azure podataka. Ako ste kopirajte i zalijepite sljedeći kod, zamijeniti HDInsight klaster naziv s nazivom svoj klaster HDInsight pa promijenite korisničko ime i lozinku vrijednosti. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Skupove podataka

#### <a name="output-dataset"></a>Izlazni skup podataka
Kanal u ovom primjeru neće moći koristiti sve unose. Navedite skup podataka za izlaz HDInsight strujanje aktivnosti. U ovom dataset je samo taj skup podataka koji je potreban za Poticanje raspored za kanal. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Kanal

Kanal u ovom primjeru sadrži samo jedan aktivnosti koje je vrste: **HDInsightStreaming**. 

HDInsight klaster automatski popunjen primjer programa (wc.exe i cat.exe) i podataka (davinci.txt). Prema zadanim postavkama, naziv klaster sam je naziv spremnika koji koriste klaster HDInsight. Ako, na primjer, ako je naziv klaster myhdicluster, naziv spremnika blob povezanog bio bi myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Vidi također
- [Grozd aktivnosti](data-factory-hive-activity.md)
- [Svinja aktivnosti](data-factory-pig-activity.md)
- [MapReduce aktivnosti](data-factory-map-reduce.md)
- [Pozivanje Spark programi](data-factory-spark.md)
- [Pozivanje R skripti](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

