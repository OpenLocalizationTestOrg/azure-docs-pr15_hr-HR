<properties 
    pageTitle="Svinja aktivnosti" 
    description="Saznajte kako koristiti aktivnosti Svinja u na tvorničke Azure podataka da pokreću skripte Svinja na na – zahtjev/vaše vlastite klaster za HDInsight." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Svinja aktivnosti
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)

Svinja HDInsight aktivnosti u podataka tvorničke [kanala](data-factory-create-pipelines.md) izvršava Svinja upita na [vlastitu](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ili [osvježavati](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) klaster HDInsight/Linux utemeljen na sustavu Windows. U ovom se članku sastavlja na članak [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) koja predstavlja Općenito pregled i transformaciju podataka te aktivnosti podržani transformaciju.

## <a name="syntax"></a>Sintaksa

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Detalji o sintaksi

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
ime | Naziv aktivnosti | Da
Opis | Tekst koji opisuje što aktivnost koristi | ne
Vrsta | HDinsightPig | Da
unosa | Jedan ili više unosa troše Svinja aktivnosti | ne
izlaze | Jedan ili više izlaze osnovu Svinja aktivnosti | Da
linkedServiceName | Referenca na klaster HDInsight koja je registrirana kao povezane servisa u tvorničke podataka | Da
skripta | Odredite skripte umetnute Svinja | ne
Put skripte | Spremanje Svinja skripte u Azure blobova i navedite put do datoteke. Svojstvo "skriptu" ili "scriptPath". Oba nije moguće koristiti zajedno. Naziv datoteke je velika i mala slova. | ne
definira | Odredite parametre kao parove ključa vrijednosti za pozivanju unutar Svinja skripte | ne

## <a name="example"></a>Primjer

Ćemo razmotriti primjera igraće zapisnika analize koju želite prepoznavanje vremena utrošenog na uređaje koji se reproducira igre pokrene u vašoj tvrtki.
 
Sljedeće ogledne igraće zapisnika je datoteka odvojenih zarezom (;). Sadrži sljedeća polja – ProfileID, SessionStart, trajanje, SrcIPAddress i GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Svinja skripta** za obradu tih podataka:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Izvršavanje Ova skripta Svinja u podataka tvorničke kanal, učinite sljedeće:

1. Stvaranje povezanih servisa registrirati [vlastite HDInsight izračunati klaster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ili konfiguriranje [HDInsight na zahtjev za izračun klaster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Pogledajmo nazovite povezane servis **HDInsightLinkedService**.
2.  Stvaranje [povezanih servisa](data-factory-azure-blob-connector.md) konfigurirati vezu sa spremištem blobova platforme Azure pohrane podataka. Pogledajmo nazovite povezane servis **StorageLinkedService**.
3.  Stvaranje [skupova podataka](data-factory-create-datasets.md) koja pokazuje na unos i za izlazne podatke. Pogledajmo poziva unos skup podataka **PigSampleIn** i izlazni skup podataka **PigSampleOut**.
4.  Kopiranje Svinja upita u datoteci spremište blobova platforme Azure konfiguriran u koraku #2. Ako Azure prostora za pohranu koji hostira podatke razlikuje se od one koju hostira datoteke upita, stvorite zaseban Azure prostora za pohranu povezani. Odnose se na servis za povezane u konfiguraciji aktivnosti. Pomoću **scriptPath **odredite put do datoteka skripte svinja i **scriptLinkedService**. 
    
    > [AZURE.NOTE] Skripta umetnute Svinja u definiciji aktivnosti možete unijeti i pomoću **skripte** svojstva. Međutim, ne možemo takvog preporučene zamjene sve posebni znakovi u skripti treba unijeti prespojni znak, a može uzrokovati probleme s za ispravljanje pogrešaka. Preporučenim načinom rada tako da slijedite korake #4.
5. Stvorite kanal s HDInsightPig aktivnosti. Aktivnost obrađuje ulaznih podataka tako da pokrenete Svinja skripte na klasteru HDInsight.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Implementacija kanal. Pogledajte članak [Stvaranje kanali](data-factory-create-pipelines.md) detalje. 
7. Praćenje kanala pomoću tvorničke nadzor i upravljanje prikaza podataka. Potražite u članku [nadzor i upravljanje podacima tvorničke kanali](data-factory-monitor-manage-pipelines.md) članak detalje.

## <a name="specifying-parameters-for-a-pig-script"></a>Određivanje parametara za skripte Svinja 

Razmislite o sljedećem primjeru: igraće zapisnicima su ingested svakodnevno u spremište blobova platforme Azure i pohranjene u mapi particioniranom na temelju datuma i vremena. Želite parameterize Svinja skripte i dinamički prolaz ulazne mape tijekom izvođenja i proizvesti rezultat particije s datumom i vremenom.
 
Da biste koristili s parametrima Svinja skripte, učinite sljedeće:

- Definiranje parametara u **definira**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- U skripti Svinja pogledajte parametara pomoću '**$parameterName**' kao što je prikazano u sljedećem primjeru:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Vidi također
- [Grozd aktivnosti](data-factory-hive-activity.md)
- [MapReduce aktivnosti](data-factory-map-reduce.md)
- [Hadoop strujanje aktivnosti](data-factory-hadoop-streaming-activity.md)
- [Pozivanje Spark programi](data-factory-spark.md)
- [Pozivanje R skripti](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


