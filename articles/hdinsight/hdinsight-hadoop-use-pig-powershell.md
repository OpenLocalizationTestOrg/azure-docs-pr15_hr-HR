<properties
   pageTitle="Korištenje Hadoop Svinja s PowerShell u HDInsight | Microsoft Azure"
   description="Saznajte kako poslati Svinja poslove klaster Hadoop na HDInsight pomoću komponente PowerShell Azure."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Pokrenite Svinja zadatke pomoću komponente PowerShell

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ovaj dokument sadrži primjer korištenja Azure PowerShell slanje zadataka Svinja Hadoop na klasteru HDInsight. Svinja omogućuje napisati MapReduce zadatke pomoću jezika (latinica Svinja), koji se modeli transformacije podataka umjesto mapiranje i smanjivanje funkcije.

> [AZURE.NOTE] Ovaj dokument Navedite što učiniti Svinja latinica izraza koji se koriste u primjerima detaljan opis. Informacije o latinica Svinja u ovom primjeru koriste potražite u članku [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće.

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Pokrenite Svinja zadatke pomoću komponente PowerShell

Azure PowerShell sadrži *cmdleti za* koje vam omogućuju da daljinski pokrenuti poslovi Svinja HDInsight. Interno, to je napraviti pomoću OSTALE pozive [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (prethodno pod nazivom Templeton) sustavom klaster HDInsight.

Kada se pokrene poslove Svinja s udaljenog HDInsight klaster, koriste se sljedeće Cmdlete:

* **Prijava AzureRmAccount**: potvrđuje Azure PowerShell u pretplatu za Azure

* **Novo AzureRmHDInsightPigJobDefinition**: stvara novu *definiciju posla* pomoću navedenog izraza latinica Svinja

* **Početak AzureRmHDInsightJob**: šalje definiciju posla HDInsight, pokreće posla i vraća objekt *posla* koji se mogu koristiti da biste provjerili status posla

* **Čekanje AzureRmHDInsightJob**: koristi objekt posla da biste provjerili status zadatka. Će Pričekajte dok se ne dovrši posao ili vrijeme čekanja prekoračena.

* **Get-AzureRmHDInsightJobOutput**: korišten za dohvaćanje izlaz posla

Sljedeći koraci pokazuju kako pomoću tih cmdleta Pokreni na svoj klaster HDInsight.

1. Korištenje uređivača, spremite sljedeći kod kao **pigjob.ps1**. Morate zamijeniti **CLUSTERNAME** s nazivom svoj klaster HDInsight.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Otvorite novi komponente Windows PowerShell naredbeni redak. Promjena direktorija mjesto datoteke **pigjob.ps1** , a zatim pokrenuti skriptu pomoću sljedeće naredbe:

        .\pigjob.ps1
        
    Najprije zatražit će se prijava u pretplatu za Azure. Nakon toga koji će se tražiti HTTP/administrator računa ime i lozinku za klaster HDInsight.

7. Nakon dovršetka posla, ona mora vratiti podatke sličnu ovoj:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Otklanjanje poteškoća

Ako nema podataka o se nakon dovršetka posla, možda ste pojavila se pogreška tijekom obrade. Da biste vidjeli informacije o pogrešci za ovaj zadatak, dodavanje sljedeću naredbu na kraj datoteke **pigjob.ps1** , spremite ga i ponovno ga pokrenite.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

To će vratiti informacije napisane na STDERR na poslužitelju kada ste pokrenuli posla, a mogu pomoći odrediti Zašto se ne uspijeva posao.

##<a id="summary"></a>Sažetak

Kao što vidite, Azure PowerShell omogućuje jednostavno pokrenuti Svinja zadatke programa HDInsight klaster, praćenje stanja zadatka i dohvaćanje izlaz.

##<a id="nextsteps"></a>Daljnji koraci

Općenite informacije o Svinja u HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
