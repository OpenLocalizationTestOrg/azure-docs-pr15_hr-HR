<properties 
    pageTitle="Grozd aktivnosti" 
    description="Saznajte kako koristiti aktivnosti vrste Hive u na tvorničke Azure podataka pokrenuti grozd upita na – zahtjev/vaše vlastite klaster za HDInsight." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Grozd aktivnosti
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)

HDInsight grozd aktivnosti u podataka tvorničke [kanal](data-factory-create-pipelines.md) izvršava grozd upita na [vlastitu](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ili [osvježavati](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) klaster HDInsight/Linux utemeljen na sustavu Windows. U ovom se članku sastavlja na članak [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) koja predstavlja Općenito pregled i transformaciju podataka te aktivnosti podržani transformaciju.

## <a name="syntax"></a>Sintaksa

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
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
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Detalji o sintaksi

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
ime | Naziv aktivnosti | Da
Opis | Tekst koji opisuje što aktivnost koristi | ne
Vrsta | HDinsightHive | Da
unosa | Unosi troše grozd aktivnosti | ne
izlaze | Izlaze osnovu grozd aktivnosti | Da 
linkedServiceName | Referenca na klaster HDInsight koja je registrirana kao povezane servisa u tvorničke podataka | Da 
skripta | Odredite skripte umetnute grozd | ne
Put skripte | Spremanje grozd skripte u Azure blobova i navedite put do datoteke. Svojstvo "skriptu" ili "scriptPath". Oba nije moguće koristiti zajedno. Naziv datoteke je velika i mala slova. | ne 
definira | Odredite parametre kao parove ključa vrijednosti za pozivanju unutar skripte grozd pomoću 'hiveconf'  | ne

## <a name="example"></a>Primjer

Ćemo razmotriti primjera igre zapisnika analize koju želite identificiranje vrijeme utrošeno korisnici koji se reproducira igre pokrene u vašoj tvrtki. 

Sljedeći je zapisnik igre uzorka, što je zarez (`,`) zarezom, a sadrži sljedeća polja – ProfileID, SessionStart, trajanje, SrcIPAddress i GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

U **vrste Hive skripte** za obradu tih podataka:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Izvršavanje ovu skriptu grozd u podataka tvorničke kanala, trebali biste učiniti sljedeće

1. Stvaranje povezanih servisa registrirati [vlastite HDInsight izračunati klaster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ili konfiguriranje [klaster za izračun HDInsight na zahtjev](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Pogledajmo nazovite povezane servis "HDInsightLinkedService".
2. Stvaranje [povezanih servisa](data-factory-azure-blob-connector.md) konfigurirati vezu sa spremištem blobova platforme Azure pohrane podataka. Pogledajmo poziva povezane servis "StorageLinkedService"
3. Stvaranje [skupova podataka](data-factory-create-datasets.md) koja pokazuje na unos i za izlazne podatke. Pogledajmo poziva unos dataset "HiveSampleIn" i izlazni skup podataka "HiveSampleOut"
4. Kopiraj upita grozd u obliku datoteke za spremište blobova platforme Azure konfiguriran u koraku #2. Ako za pohranu za hostiranje podatke razlikuje od onog koji se nalaze datoteku upita, stvorite zaseban Azure prostora za pohranu povezana i pozivate na aktivnosti. Pomoću **scriptPath **odredite put do datoteke grozd upita i **scriptLinkedService** da biste odredili Azure prostora za pohranu koji sadrži skriptna datoteka. 

    > [AZURE.NOTE] Skripta umetnute grozd u definiciji aktivnosti možete unijeti i pomoću **skripte** svojstva. Ne možemo takvog preporučene zamjene sve posebni znakovi u skripti unutar dokumenta JSON mora unijeti prespojni znak, a može uzrokovati probleme s za ispravljanje pogrešaka. Preporučenim načinom rada tako da slijedite korake #4.
5.  Stvorite na kanal s HDInsightHive aktivnosti. Aktivnost procesa/transformacije podataka.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Implementacija kanal. Pogledajte članak [Stvaranje kanali](data-factory-create-pipelines.md) detalje. 
7.  Praćenje kanal pomoću tvorničke nadzor i upravljanje prikaza podataka. Potražite u članku [nadzor i upravljanje podacima tvorničke kanali](data-factory-monitor-manage-pipelines.md) članak detalje. 


## <a name="specifying-parameters-for-a-hive-script"></a>Određivanje parametara za skripte grozd  
U ovom primjeru igraće zapisnicima su ingested svakodnevno u spremište blobova platforme Azure i spremaju se u mapi particije s datumom i vremenom. Želite li parameterize grozd skripte i dinamički prolaz ulazne mape tijekom izvođenja i proizvesti rezultat particije s datumom i vremenom.

Da biste koristili s parametrima skripte grozd, učinite sljedeće

- Definiranje parametara u **definira**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- U skripti vrste Hive odnose se na parametar pomoću **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Vidi također
- [Svinja aktivnosti](data-factory-pig-activity.md)
- [MapReduce aktivnosti](data-factory-map-reduce.md)
- [Hadoop strujanje aktivnosti](data-factory-hadoop-streaming-activity.md)
- [Pozivanje Spark programi](data-factory-spark.md)
- [Pozivanje R skripti](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









