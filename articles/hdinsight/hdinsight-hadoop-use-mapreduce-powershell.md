<properties
   pageTitle="Korištenje MapReduce i PowerShell s Hadoop | Microsoft Azure"
   description="Saznajte kako pomoću ljuske PowerShell za daljinsko pokretanje MapReduce poslove s Hadoop na HDInsight."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Mada MapReduce poslove s Hadoop HDInsight pomoću komponente PowerShell

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ovaj dokument sadrži primjer korištenja Azure PowerShell pokrenuti posao MapReduce u na Hadoop klaster HDInsight.

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

- **Klaster Azure HDInsight (Hadoop na HDInsight) (utemeljen na sustavu Windows ili Linux sustavom)**

- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Pokrenuti posao MapReduce pomoću komponente PowerShell Azure

Azure PowerShell sadrži *cmdleti za* koje vam omogućuju da daljinski pokrenuti poslovi MapReduce HDInsight. Interno, to je napraviti pomoću OSTALE pozive [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (prethodno pod nazivom Templeton) sustavom klaster HDInsight.

Sljedeće Cmdlete se koriste prilikom izvođenja MapReduce zadatke na udaljenom HDInsight.

* **Prijava AzureRmAccount**: potvrđuje Azure PowerShell u pretplatu za Azure

* **Novo AzureRmHDInsightMapReduceJobDefinition**: stvara novu *definiciju posla* pomoću navedene informacije MapReduce

* **Početak AzureRmHDInsightJob**: šalje definiciju posla HDInsight, pokreće posla i vraća objekt *posla* koji se mogu koristiti da biste provjerili status posla

* **Čekanje AzureRmHDInsightJob**: koristi objekt posla da biste provjerili status zadatka. To ne čeka dok se ne dovrši posao ili je premašena vrijeme čekanja.

* **Get-AzureRmHDInsightJobOutput**: korišten za dohvaćanje izlaz posla

Sljedeći koraci pokazuju kako pomoću tih cmdleta Pokreni u svoj klaster HDInsight.

1. Korištenje uređivača, spremite sljedeći kod kao **mapreducejob.ps1**. Morate zamijeniti **CLUSTERNAME** s nazivom svoj klaster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Otvorite novi **Azure PowerShell** naredbeni redak. Promjena direktorija mjesto datoteke **mapreducejob.ps1** , a zatim pokrenuti skriptu pomoću sljedeće naredbe:

        .\mapreducejob.ps1
    
    Kada pokrenete skriptu, možete se upit za provjeru autentičnosti u pretplatu za Azure. Će također tražiti da navede HTTP/administrator naziv računa i lozinku za klaster HDInsight.

3. Nakon dovršetka posla, dobivate izlaz sličnu ovoj:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Ovaj izlaz označava uspješno dovršena posao.

    > [AZURE.NOTE] Ako je **ExitCode** vrijednost različita od 0, pročitajte članak [Otklanjanje poteškoća](#troubleshooting).

    U ovom se primjeru će spremiti preuzete datoteke u datoteku **output.txt** u imenika pokrenuti skriptu iz.

###<a name="view-output"></a>Prikaz Izlaz

Otvorite **output.txt** datoteku u uređivaču teksta da biste vidjeli riječi i broji koje je stvorio posao.

> [AZURE.NOTE] Izlazna datoteka posla MapReduce su immutable. Tako ako ponovno pokrenite ovaj uzorak, morate promijeniti naziv Izlazna datoteka.

##<a id="troubleshooting"></a>Otklanjanje poteškoća

Ako nema podataka o se nakon dovršetka posla, možda ste pojavila se pogreška tijekom obrade. Da biste vidjeli informacije o pogrešci za ovaj zadatak, dodavanje sljedeću naredbu na kraj datoteke **mapreducejob.ps1** , spremite ga i ponovno ga pokrenite.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

To vraća podatke napisane na STDERR na poslužitelju kada ste pokrenuli posla, a mogu pomoći odrediti Zašto se ne uspijeva posao.

##<a id="summary"></a>Sažetak

Kao što vidite, Azure PowerShell omogućuje jednostavno pokrenuti MapReduce zadatke programa HDInsight klaster, praćenje stanja zadatka i dohvaćanje izlaz.

##<a id="nextsteps"></a>Daljnji koraci

Opće informacije o zadacima MapReduce u HDInsight:

* [Korištenje MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)
