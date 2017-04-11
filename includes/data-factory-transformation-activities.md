Azure tvorničke podataka podržava sljedeće transformaciju aktivnosti koje možete dodati kanali ili pojedinačno ili povezane s druge aktivnosti.

Aktivnosti transformacije podataka |  Okruženje za izračun 
:----------------------- | :--------------------
[Grozd](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Svinja](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop strujanje](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Strojne grupe učenje aktivnosti: Execution seriju i resursa za ažuriranje](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Pohranjena procedura](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL, Azure SQL Data Warehouse ili SQL Server |
[U-SQL Lake analize podataka](../articles/data-factory/data-factory-usql-activity.md) | Analitički Lake Azure podataka 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] ili grupe za Azure
   
> [AZURE.NOTE] 
> Aktivnosti MapReduce možete koristiti za pokretanje programa Spark na svoj klaster HDInsight Spark. Detalje potražite u članku [pozivanje Spark programe tvorničke Azure podataka](../articles/data-factory/data-factory-spark.md) .
> Možete stvoriti prilagođene aktivnosti da pokreću skripte R na svoj HDInsight klaster s R instaliran. Potražite u članku [pokrenuti skriptu R pomoću tvorničke Azure podataka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).