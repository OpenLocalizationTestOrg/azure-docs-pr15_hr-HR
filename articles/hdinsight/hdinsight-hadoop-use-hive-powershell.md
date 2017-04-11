<properties
   pageTitle="Hadoop grozd pomoću komponente PowerShell u HDInsight | Microsoft Azure"
   description="Korištenje ljuske PowerShell za pokretanje grozd upita u Hadoop na HDInsight."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Pokretanje grozd upita pomoću komponente PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ovaj dokument sadrži primjer korištenja Azure PowerShell u načinu rada za grupu resursa Azure pokrenuti grozd upita u na Hadoop klaster HDInsight.

> [AZURE.NOTE] Ovaj dokument Navedite što učiniti HiveQL izraza koji se koriste u primjerima detaljan opis. Informacije o HiveQL koja se koristi u ovom primjeru, potražite u članku [Korištenje vrste Hive s Hadoop na HDInsight](hdinsight-use-hive.md).


**Preduvjeti**

Da biste dovršili korake u ovom članku, morate sljedeće.

- **Klaster Azure HDInsight (Hadoop na HDInsight) (utemeljen na sustavu Windows ili Linux sustavom)**
- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Pokretanje grozd upita pomoću komponente PowerShell Azure

Azure PowerShell sadrži *cmdleta* koji omogućuju daljinsko pokretanje grozd upita na HDInsight. Interno, to je napraviti pomoću OSTALE pozive [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (prethodno pod nazivom Templeton) sustavom klaster HDInsight.

Kada se pokrene grozd upita na udaljenom HDInsight koriste se sljedeće Cmdlete:

* **Dodavanje AzureRmAccount**: potvrđuje Azure PowerShell u pretplatu za Azure

* **Novo AzureRmHDInsightHiveJobDefinition**: stvara novu *definiciju posla* pomoću navedenog izraza HiveQL

* **Početak AzureRmHDInsightJob**: šalje definiciju posla HDInsight, pokreće posla i vraća objekt *posla* koji se mogu koristiti da biste provjerili status posla

* **Čekanje AzureRmHDInsightJob**: koristi objekt posla da biste provjerili status zadatka. Će Pričekajte dok se ne dovrši posao ili je premašena vrijeme čekanja.

* **Get-AzureRmHDInsightJobOutput**: korišten za dohvaćanje izlaz posla

* **Pozovite AzureRmHDInsightHiveJob**: koristiti za izvođenje naredbe HiveQL. To će onemogućiti upit dovrši, a zatim vraća rezultat

* **Korištenje AzureRmHDInsightCluster**: postavlja trenutni klaster da biste koristili naredbu **Pozovite AzureRmHDInsightHiveJob**

Sljedeći koraci pokazuju kako pomoću tih cmdleta Pokreni u svoj klaster HDInsight:

1. Korištenje uređivača, spremite sljedeći kod kao **hivejob.ps1**. Morate zamijeniti **CLUSTERNAME** s nazivom svoj klaster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Otvorite novi **Azure PowerShell** naredbeni redak. Promjena direktorija mjesto datoteke **hivejob.ps1** , a zatim pokrenuti skriptu pomoću sljedeće naredbe:

        .\hivejob.ps1

    Prilikom pokretanja skripte, zatražit će se da biste unijeli HTTP/administratorskih vjerodajnica računa za svoj klaster. Možda i zatražiti prijava u pretplatu za Azure.
    
7. Nakon dovršetka posla, ona mora vratiti podatke sličnu ovoj:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Kao što je rečeno ranije, **Pozovite grozd** može koristiti da biste pokrenuli upit i pričekajte da odgovor. Koristite sljedeće naredbe i **CLUSTERNAME** zamijenite nazivom svoj klaster:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Izlaz izgledat će ovako:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Za više HiveQL upite, možete koristiti cmdlet Azure PowerShell **Ovdje nizove** ili HiveQL skripte datoteke. Sljedeći isječak pokazuje kako pomoću cmdleta **Pozovite grozd** da biste pokrenuli HiveQL skriptna datoteka. Datoteka skripte HiveQL morate prenijeti wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Dodatne informacije o **Ovdje nizovi**potražite u članku <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Korištenje Windows PowerShell ovdje-nizova</a>.

##<a name="troubleshooting"></a>Otklanjanje poteškoća

Ako nema podataka o se nakon dovršetka posla, možda ste pojavila se pogreška tijekom obrade. Da biste vidjeli informacije o pogrešci za ovaj zadatak, dodajte sljedeće na kraj **hivejob.ps1** datoteku, spremite je i ponovno ga pokrenite.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

To vraća podatke koji zapisuje na STDERR na poslužitelju kada ste pokrenuli posao, a mogu pomoći odrediti Zašto se ne uspijeva posao.

##<a name="summary"></a>Sažetak

Kao što vidite, Azure PowerShell omogućuje jednostavnu pokretanje grozd upita u programa HDInsight klaster, praćenje stanja zadatka i dohvaćanje izlaz.

##<a name="next-steps"></a>Daljnji koraci

Opće informacije o grozd u HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
