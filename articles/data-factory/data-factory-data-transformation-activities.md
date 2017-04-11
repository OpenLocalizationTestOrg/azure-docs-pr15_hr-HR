<properties 
    pageTitle="Transformaciju podataka: Postupak i pretvaranja podataka | Microsoft Azure" 
    description="Saznajte kako Pretvorba podatke ili podatke postupak u Azure podataka tvorničke pomoću Hadoop, strojnog učenja ili Azure podataka Lake analize." 
    keywords="transformaciju podataka, postupak podatke, pretvaranje podataka, a zatim Pretvorba aktivnosti"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Pretvaranje podataka u tvorničke Azure podataka
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Pregled 
U ovom se članku objašnjava aktivnosti transformacije podataka u Azure tvorničke podataka možete koristiti za pretvaranje i obrađuje sirovim podacima u predviđanja i uvide. Aktivnost transformaciju izvršava u okruženju računalno kao što su Azure HDInsight klaster ili grupe za Azure. Pruža veze na članke s detaljne informacije o svakoj transformaciju aktivnosti.
 
Tvorničke podataka podržava sljedeće podatke transformaciju aktivnosti koje možete dodati [kanali](data-factory-create-pipelines.md) ili pojedinačno ili povezane s druge aktivnosti.

> [AZURE.NOTE] Vodič s detaljnim uputama, potražite u članku [Stvaranje kanala s grozd transformaciju](data-factory-build-your-first-pipeline.md) članka.  

## <a name="hdinsight-hive-activity"></a>HDInsight grozd aktivnosti
HDInsight grozd aktivnosti u podataka tvorničke kanalu izvršava grozd upita vlastite ili klaster/Linux utemeljen na sustavu Windows HDInsight na zahtjev. Potražite u članku [Vrste Hive aktivnosti](data-factory-hive-activity.md) članak detalje o aktivnost. 

## <a name="hdinsight-pig-activity"></a>Svinja HDInsight aktivnosti
Svinja HDInsight aktivnosti u podataka tvorničke kanalu izvršava Svinja upiti vlastite ili klaster/Linux utemeljen na sustavu Windows HDInsight na zahtjev. Članak [Svinja aktivnosti](data-factory-pig-activity.md) potražite u članku dodatne informacije o ovom aktivnosti. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce aktivnosti
HDInsight MapReduce aktivnosti u podataka tvorničke kanalu izvršava MapReduce programi vlastite ili klaster/Linux utemeljen na sustavu Windows HDInsight na zahtjev. Članak [MapReduce aktivnosti](data-factory-map-reduce.md) potražite u članku dodatne informacije o ovom aktivnosti.

Aktivnosti MapReduce možete koristiti za pokretanje programa Spark na svoj klaster HDInsight Spark. Detalje potražite u članku [pozivanje Spark programe tvorničke Azure podataka](data-factory-spark.md) .

## <a name="hdinsight-streaming-activity"></a>HDInsight strujanje aktivnosti
HDInsight strujanje aktivnosti u podataka tvorničke kanalu izvršava strujanje Hadoop programi vlastite ili klaster/Linux utemeljen na sustavu Windows HDInsight na zahtjev. Detalje o aktivnost potražite u članku [HDInsight strujanje aktivnosti](data-factory-hadoop-streaming-activity.md) .

## <a name="machine-learning-activities"></a>Učenje aktivnosti na računalu
Azure tvorničke podataka omogućuje jednostavno stvaranje kanali koje koriste objavljenu web-servisa Azure strojnog učenja predvidljivu analitičkih. Pomoću [Grupe za izvršavanje aktivnosti](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) u kanalu na tvorničke Azure podataka, možete pozvati strojnog učenja web-servisa da biste predviđanja podatke iz grupe.

S vremenom predvidljivu modelima u strojnog učenja bilježenje rezultata eksperimenata morati retrained pomoću novi unos skupova podataka. Kada završite s retraining, želite ažurirati bilježenja rezultata web-servisa s retrained modelom strojnog učenja. [Ažuriranje aktivnosti resursa](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) možete koristiti da biste ažurirali web-servisa s upravo obučeni modelom.  

Detalje o te aktivnosti strojnog učenja potražite u članku [Korištenje strojnog učenja aktivnosti](data-factory-azure-ml-batch-execution-activity.md) . 

## <a name="stored-procedure-activity"></a>Pohranjena procedura aktivnosti
SQL Server pohranjena procedura aktivnosti možete koristiti u podataka tvorničke kanalima za pozivanje pohranjena procedura u jednom od sljedećih podataka trgovine: Azure SQL baze podataka, Azure SQL Data Warehouse baze podataka SQL Server u tvrtki ili je VM Azure. Pogledajte članak [Pohranjene Procedure aktivnosti](data-factory-stored-proc-activity.md) detalje.  

## <a name="data-lake-analytics-u-sql-activity"></a>Podatke sustava SQL-Lake analize U aktivnosti
Podataka Lake analize U SQL aktivnosti pokreće U SQL skripta programa klaster Azure podataka Lake analize. Potražite u članku članak [Podataka analize U SQL aktivnosti](data-factory-usql-activity.md) detalje. 

## <a name="net-custom-activity"></a>.NET prilagođene aktivnosti
Ako vam je potrebna pretvaranje podataka na način koji ne podržava tvorničke podatke, možete stvoriti prilagođene aktivnosti s vlastitim obradu podataka i koristite aktivnost u kanalu. Možete konfigurirati prilagođene aktivnosti .NET pokrenuti servis za obradu Azure ili je klaster Azure HDInsight. Potražite u članku članak [korištenje prilagođene aktivnosti](data-factory-use-custom-activities.md) detalje. 

Možete stvoriti prilagođene aktivnosti da pokreću skripte R na svoj klaster HDInsight s R instaliran. Potražite u članku [pokrenuti skriptu R pomoću tvorničke Azure podataka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Izračunavanje okruženja
Stvaranje povezanih servisa računalnim okruženju, a zatim pomoću povezane servisa prilikom definiranja transformaciju aktivnosti. Postoje dvije vrste računalnim okruženja podržava tvorničke podataka. 

1. **Na zahtjev**: U ovom slučaju računalno okruženje potpuno upravlja tvorničke podataka. Automatski stvorena je pomoću servisa tvorničke podataka prije nego što je posao je poslati postupak podataka i ukloniti nakon dovršetka posla. Možete konfigurirati i kontrolu zrnastog postavki okruženja računalnim na zahtjev za izvršavanje posla, upravljanje klaster i pokretački akcije. 
2. **Premjesti vaše vlastite**: U ovom slučaju registrirati vlastite računalno okruženje (na primjer HDInsight klaster) kao povezane servisa u tvorničke podataka. Računalno okruženje upravlja koje i servis tvorničke podataka koristi za izvršavanje aktivnosti. 

Potražite u članku [Izračunati povezani servisi](data-factory-compute-linked-services.md) članka dodatne informacije o izračunati services podržava tvorničke podataka. 


## <a name="summary"></a>Sažetak
Azure tvorničke podataka podržava sljedećih aktivnosti transformacije podataka i računalnim okruženja za aktivnosti. Pretvorba aktivnosti možete dodati kanali ili pojedinačno ili povezane s druge aktivnosti.

Aktivnosti transformacije podataka |  Okruženje za izračun 
:----------------------- | :--------------------
[Grozd](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Svinja](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Strojne grupe učenje aktivnosti: izvođenja grupe i resursa za ažuriranje](data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Pohranjena procedura](data-factory-stored-proc-activity.md) | Azure SQL, Azure SQL Data Warehouse ili SQL Server |
[U-SQL Lake analize podataka](data-factory-usql-activity.md) | Analitički Lake Azure podataka 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] ili grupe za Azure
   

