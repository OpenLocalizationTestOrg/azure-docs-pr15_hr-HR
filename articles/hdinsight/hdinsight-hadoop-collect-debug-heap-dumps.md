<properties
    pageTitle="Ispravljanje pogrešaka i analiziranje Hadoop usluge s ugovorima o skupova ispisi | Microsoft Azure"
    description="Automatski prikupljanje skupova ispisi za Hadoop servise i smjestiti u računa spremišta blobova platforme Azure za ispravljanje pogrešaka i analiza."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Ako ste u spremište blobova platforme za ispravljanje pogrešaka i analizu Hadoop services prikupite skupova

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Ispisi skupova sadrže snimku memorije aplikacije, uključujući vrijednosti varijabli trenutku stvorena na ispis. Da biste ih vrlo koristan i za dijagnosticiranje problema koji se pojavljuju pri izvođenju. Ispisi skupova mogu automatski koji se prikupljaju za Hadoop servise i smješten unutar računa spremišta blobova platforme Azure korisnika u odjeljku HDInsightHeapDumps /. 

Skup skupova ispisi za različite servise mora biti omogućen za usluge na pojedinačne klastere. Kao zadanu za tu značajku je isključiti za klaster. Ispisi te skupova mogu biti velike, pa je da praćenje računa spremišta blobova platforme gdje se spremaju kada je omogućena za zbirku.

> [AZURE.NOTE] Informacije u ovom članku odnosi se samo na HDInsight utemeljen na sustavu Windows. Informacije o Linux temelje servisa HDInsight potražite u članku [Omogućivanje skupova ispisi za Hadoop usluge na Linux temelje HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Servise za ispisi skupova

Možete omogućiti skupova ispisi za sljedeći servisi:

*  **hcatalog** - tempelton
*  **grozd** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **yarn** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfiguracija elemenata koji omogućuju ispisi skupova

Da biste uključili skupova ispisi za uslugu, morate postaviti odgovarajuće konfiguracije elemenata u odjeljku za tu uslugu koja je navedena po **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Vrijednost **service_name** može biti bilo koji od gore navedenih servisa: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, ili namenode.

## <a name="enable-using-azure-powershell"></a>Omogućivanje pomoću komponente PowerShell Azure

Na primjer, da biste uključili skupova ispisi pomoću Azure PowerShell za jobhistoryserver, bi učinite sljedeće:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Omogućivanje korištenja .NET SDK

Na primjer, da biste uključili skupova ispisi pomoću SDK za .NET Azure HDInsight za jobhistoryserver, bi učinite sljedeće:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
