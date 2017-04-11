<properties 
    pageTitle="Pozivanje MapReduce programa tvorničke Azure podataka" 
    description="Upute za obradu podataka tako da pokrenete MapReduce programe na programa Azure HDInsight klaster iz programa tvorničke Azure podataka." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Pozivanje MapReduce programe tvorničke podataka
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)

HDInsight MapReduce aktivnosti u podataka tvorničke [kanal](data-factory-create-pipelines.md) izvršava MapReduce programe na [vlastitu](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ili [osvježavati](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) klaster HDInsight/Linux utemeljen na sustavu Windows. U ovom se članku sastavlja na članak [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) koja predstavlja Općenito pregled i transformaciju podataka te aktivnosti podržani transformaciju.

## <a name="introduction"></a>Uvod 
Kanal u na tvorničke Azure podataka obrađuje podatke u servise za pohranu povezane pomoću povezane računalnim services. Sadrži niz aktivnosti, gdje svaki aktivnosti izvodi operacije na određene obrada. U ovom se članku opisuje korištenje MapReduce aktivnosti HDInsight.
 
Potražite u članku [Svinja](data-factory-pig-activity.md) i [vrste Hive](data-factory-hive-activity.md) detalje o pokretanju Svinja/grozd skripte na/Linux utemeljen na sustavu Windows HDInsight klaster iz na kanal pomoću HDInsight Svinja i grozd aktivnosti. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON HDInsight MapReduce aktivnosti 

U definiciji JSON HDInsight aktivnosti: 
 
1. Postavljanje **vrste** **aktivnosti** za **HDInsight**.
3. Navedite naziv klase **NazivKlase** svojstva.
4. Navedite put do datoteke POSUDU uključujući naziv datoteke za svojstvo **jarFilePath** .
5. Određivanje povezane usluge koje se odnosi na spremište blobova platforme Azure s datotekom POSUDU **jarLinkedService** svojstva.   
6. Da biste odredili sve argumente za MapReduce program u odjeljku **argumente** . Tijekom rada, pročitajte članak je nekoliko dodatnih argumente (na primjer: mapreduce.job.tags) iz MapReduce framework. Razlikovanje svoje argumente s argumentima MapReduce, preporučujemo da koristite mogućnost i vrijednost kao argumenti kao što je prikazano u sljedećem primjeru (- s – unos, – izlaz itd., su mogućnosti odmah slijedi njihove vrijednosti).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

Aktivnosti MapReduce HDInsight možete koristiti da biste pokrenuli bilo koju datoteku posudu MapReduce na programa klaster HDInsight. U sljedeće ogledne JSON definiciji na kanal, aktivnosti HDInsight je konfiguriran za izvođenje Mahout POSUDU datoteke.

## <a name="sample-on-github"></a>Ogledna na GitHub
Možete preuzeti uzorka za korištenje aktivnost MapReduce HDInsight iz: [Uzorka tvorničke podataka na GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Pokretanje programa za Brojanje riječi
Kanal u ovom primjeru pokreće program karte smanjivanje Brojanje riječi na svoj klaster Azure HDInsight.   

### <a name="linked-services"></a>Povezani servisi
Prvo, stvorite povezani servisa za povezivanje Azure prostora za pohranu koji koriste klaster Azure HDInsight na tvorničke Azure podataka. Ako ste kopirajte i zalijepite sljedeći kod, ne zaboravite da biste zamijenili **naziv računa** i **ključ za račun** s imenom i ključ Azure prostora za pohranu. 

#### <a name="azure-storage-linked-service"></a>Azure servis za pohranu povezana

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
Nakon toga stvorite povezani servisa da biste se povezali svoj klaster Azure HDInsight tvorničke Azure podataka. Ako ste kopirajte i zalijepite sljedeći kod, zamijeniti **HDInsight klaster naziv** s nazivom svoj klaster HDInsight pa promijenite korisničko ime i lozinku vrijednosti.   

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
Kanal u ovom primjeru neće moći koristiti sve unose. Navedite skup podataka za izlaz aktivnosti MapReduce HDInsight. U ovom dataset je samo taj skup podataka koji je potreban za Poticanje raspored za kanal.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Kanal
Kanal u ovom primjeru sadrži samo jedan aktivnosti koje je vrste: HDInsightMapReduce. Neke važne svojstva na JSON su: 

Svojstvo | Bilješke
:-------- | :-----
Vrsta | Vrsta mora biti postavljeno na **HDInsightMapReduce**. 
NazivKlase | Naziv klase: **wordcount**
jarFilePath | Put do datoteke posudu koja sadrži klasu. Ako ste kopirajte i zalijepite sljedeći kod, ne zaboravite da biste promijenili naziv klaster. 
jarLinkedService | Azure servis za pohranu povezanu s datotekom posudu. Servis za povezane se odnosi na prostora za pohranu koji je pridružen klaster HDInsight. 
Argumenti | Wordcount program uzima dva argumenta, ulazni i na izlaz. Ulazna datoteka je davinci.txt datoteku.
Učestalost/interval | Vrijednosti tih svojstava odgovaraju izlazni skup podataka. 
linkedServiceName | odnosi se na servis HDInsight povezani ste prethodno stvorili.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Pokretanje programa Spark
Aktivnosti MapReduce možete koristiti za pokretanje programa Spark na svoj klaster HDInsight Spark. Detalje potražite u članku [pozivanje Spark programe tvorničke Azure podataka](data-factory-spark.md) .  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Vidi također
- [Grozd aktivnosti](data-factory-hive-activity.md)
- [Svinja aktivnosti](data-factory-pig-activity.md)
- [Hadoop strujanje aktivnosti](data-factory-hadoop-streaming-activity.md)
- [Pozivanje Spark programi](data-factory-spark.md)
- [Pozivanje R skripti](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
