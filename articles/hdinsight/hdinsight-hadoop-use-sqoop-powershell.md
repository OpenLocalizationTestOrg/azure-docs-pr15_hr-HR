<properties
    pageTitle="Korištenje Hadoop Sqoop u HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti Azure PowerShell iz programa radne stanice da biste pokrenuli Sqoop uvoz i izvoz između programa Hadoop klaster i baze podataka Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="run-sqoop-jobs-using-azure-powershell-for-hadoop-in-hdinsight"></a>Pokrenite Sqoop zadatke pomoću Azure PowerShell za Hadoop u HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saznajte kako pomoću ljuske PowerShell za Azure da biste pokrenuli Sqoop zadatke u HDInsight za uvoz i izvoz između HDInsight klaster i baze podataka Azure SQL ili baze podataka SQL Server.

> [AZURE.NOTE] Koraci u ovom članku može se koristiti s ili Windows ili Linux HDInsight klaster; Međutim, ove korake će raditi samo iz klijenta za Windows. Za druge načine za predavanje posla, kliknite birač tabulatora pri vrhu stranice u članku.


###<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **A Hadoop klaster u HDInsight**. Potražite u članku [Stvaranje klaster i SQL baze podataka](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

    
## <a name="run-sqoop-using-powershell"></a>Pokrenite Sqoop pomoću komponente PowerShell

Sljedeću skriptu komponente PowerShell unaprijed obrađuje izvornu datoteku i izvoz s bazom podataka Azure SQL:

    $resourceGroupName = "<AzureResourceGroupName>"
    $hdinsightClusterName = "<HDInsightClusterName>"

    $httpUserName = "admin"
    $httpPassword = "<Password>"

    $defaultStorageAccountName = $hdinsightClusterName + "store"
    $defaultBlobContainerName = $hdinsightClusterName


    $sqlDatabaseServerName = $hdinsightClusterName + "dbserver"
    $sqlDatabaseName = $hdinsightClusterName + "db"
    $sqlDatabaseLogin = "sqluser"
    $sqlDatabasePassword = "<Password>"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
        
    #region - pre-process the source file
        
    Write-Host "`nPreprocessing the source file ..." -ForegroundColor Green
        
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
        
    # Define the connection string
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $defaultStorageAccountName
    $storageContainer = ($storageAccount |Get-AzureStorageContainer -Name $defaultBlobContainerName).CloudBlobContainer
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
        
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
        
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
        
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
        
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
        
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
        
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
        
    #endregion
        
    #region - export the log file from the cluster to the SQL database
        
    Write-Host "Exporting the log file ..." -ForegroundColor Green

    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
        
    $tableName_log4j = "log4jlogs"
    $exportDir_log4j = "/tutorials/usesqoop/data"
    $sqljdbcdriver = "/user/oozie/share/lib/sqoop/sqljdbc41.jar"
        
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1" `
        -Files $sqljdbcdriver

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
        
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    #endregion

##<a name="limitations"></a>Ograničenja

* Skupno izvoz - Linux s operacijskim sustavom HDInsight, Sqoop poveznik za izvoz podataka u Microsoft SQL Server ili baze podataka SQL Azure trenutno ne podržava umeće skupno.

* Grupno slanje promjena - sa sustavom Linux HDInsight kada se koristi u `-batch` prebacivanje prilikom izvršavanja umeće, Sqoop će obavljanje više umeće umjesto grupnog slanja promjena operacije Umetanje.

##<a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako koristiti Sqoop. Da biste saznali više, pogledajte:

- [Korištenje Oozie s HDInsight](hdinsight-use-oozie.md): korištenje Sqoop akcije u tijeku rada Oozie.
- [Analiza leta podatke o kašnjenju pomoću HDInsight](hdinsight-analyze-flight-delay-data.md): korištenje vrste Hive da biste analizirali leta odgađanje podataka, a zatim pomoću Sqoop za izvoz podataka u bazu podataka Azure SQL.
- [Prijenos podataka HDInsight](hdinsight-upload-data.md): pronalaženje drugih načina za prijenos podataka u spremište blobova platforme HDInsight/Azure.


[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
