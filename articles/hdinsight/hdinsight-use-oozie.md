<properties
    pageTitle="Korištenje Hadoop Oozie u HDInsight | Microsoft Azure"
    description="Korištenje Hadoop Oozie u HDInsight, servis za velikih skupova podataka. Saznajte kako definiranje tijeka rada za Oozie i slanje na Oozie posla."
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
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Definiranje i pokrenuti tijek rada u HDInsight Oozie pomoću Hadoop

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Saznajte kako koristiti Apache Oozie definiranje tijeka rada i pokrenuti tijek rada na HDInsight. Da biste saznali više o koordinator Oozie, potražite u članku [Korištenje temeljene Hadoop Oozie koordinator s HDInsight][hdinsight-oozie-coordinator-time]. Tvorničke Azure podataka potražite u članku [Korištenje Svinja i grozd s podacima tvorničke][azure-data-factory-pig-hive].

Apache Oozie je tijek rada/koordinaciji sustava koja upravlja Hadoop zadatke. Je integriran s Hadoop stog i podržava Hadoop poslove Apache MapReduce, Apache Svinja, vrste Hive Apache i Apache Sqoop. Može se koristiti i zakazivanje zadataka koji su specifične za sustav, kao što su Java programe ili skripti ljuske.

Tijek rada će implementirati slijedeći upute u ovom ćete praktičnom vodiču sadrži dvije akcije:

![Dijagram tijeka rada][img-workflow-diagram]

1. Akcija za grozd pokreće HiveQL skripte za Brojanje učestalosti svaku vrstu zapisnika razina u datoteci log4j. Svaku datoteku log4j sastoji se od retka polja koja sadrži polje [ZAPISNIKA RAZINA] koja prikazuje vrstu i težinu, na primjer:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Izlaz skripte grozd slično je:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Dodatne informacije o grozd potražite u članku [Korištenje vrste Hive s HDInsight][hdinsight-use-hive].

2.  Sqoop akcija izvozi izlaz HiveQL u tablicu u bazi podataka Azure SQL. Dodatne informacije o Sqoop potražite u članku [Korištenje Hadoop Sqoop s HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Podržani verzije Oozie na HDInsight klastere, potražite u članku [što je novo u verzijama klaster Hadoop nudi HDInsight?] [hdinsight-versions].

###<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Radne stanice s Azure PowerShell**. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
    Za izvođenje skripte komponente Windows PowerShell, morate Pokreni kao administrator i postavite pravila izvođenja *RemoteSigned*. Dodatne informacije potražite u članku [skripte komponente Windows PowerShell pokrenite][powershell-script].

##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definiranje Oozie tijek rada i povezane HiveQL skripte

Definicija Oozie tijekovi rada zapisuju u hPDL (u XML postupak Definition Language). Zadani naziv datoteke tijek rada je *workflow.xml*. Slijedi tijeka rada datoteka će se koristiti u ovom ćete praktičnom vodiču.

    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>

Postoje dvije akcije definirane u tijeku rada. Akcija za početak je *RunHiveScript*. Ako uspješno izvodi akciju, *RunSqoopExport*je sljedeća akcija.

Na RunHiveScript ima više varijabli. Prilikom slanja posao Oozie iz vaše radne stanice Azure PowerShell proći vrijednosti.

<table border = "1">
<tr><th>Varijable tijeka rada</th><th>Opis</th></tr>
<tr><td>${jobTracker}</td><td>Određuje URL Evidencija posao Hadoop. Korištenje <strong>jobtrackerhost:9010</strong> u HDInsight verzije 3.0 i 2.1.</td></tr>
<tr><td>${nameNode}</td><td>Određuje URL čvor naziva Hadoop. Ako, na primjer, pomoću zadane datoteke adrese sustava, <i>wasbs: / /&lt;naziv spremnika&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${queueName}</td><td>Određuje reda čekanja naziv koji će se poslati posao. Koristite <strong>zadani</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Varijabla grozd akcija</th><th>Opis</th></tr>
<tr><td>${hiveDataFolder}</td><td>Određuje izvorišni direktorij za stvaranje uvesti tablicu vrste Hive naredbu.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Određuje Izlazna datoteka za naredbu za umetanje PREBRISATI.</td></tr>
<tr><td>${hiveTableName}</td><td>Određuje naziv tablicu vrste Hive koji referencira log4j podatkovne datoteke.</td></tr>
</table>

<table border = "1">
<tr><th>Varijabla Sqoop akcija</th><th>Opis</th></tr>
<tr><td>${sqlDatabaseConnectionString}</td><td>Određuje niz za povezivanje baze podataka Azure SQL.</td></tr>
<tr><td>${sqlDatabaseTableName}</td><td>Navodi tablice baze podataka Azure SQL gdje podataka će se izvesti.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Određuje Izlazna datoteka za naredbu vrste Hive Umetanje PREBRISATI. To je u istu mapu za izvoz Sqoop (Izvoz dir).</td></tr>
</table>

Dodatne informacije o Oozie tijek rada i korištenje akcija tijeka rada potražite u [dokumentaciji Apache Oozie 4.0] [ apache-oozie-400] (za HDInsight verzije 3.0) ili [dokumentaciju Apache Oozie 3.3.2] [ apache-oozie-332] (za HDInsight verzija 2.1).


Grozd akcije u tijeku rada poziva HiveQL skriptna datoteka. Datoteka skripte sadrži tri izvješća HiveQL:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **U TABLICI ISPUSTITE izjava** briše tablicu vrste Hive log4j ako postoji.
2. **U Naredba CREATE TABLE** stvara log4j vanjskih tablicu vrste Hive tih točaka mjesto datoteke zapisnika log4j. Graničnik polja nije ",". Graničnik redak zadani je "\n". Vanjska tablica grozd se koristi da bi se izbjeglo podatkovne datoteke uklanja s izvornog mjesta ako želite pokrenuti tijek rada Oozie više puta.
3. **Izjavu PREBRISIVANJE je okvir Umetanje** broji pojavljivanja svaku vrstu zapisnika razina u tablici log4j grozd, a sprema izlaz blobova platforme Azure pohrane u.


Postoje tri varijable koje se koriste u skripti:

- ${hiveTableName}
- ${hiveDataFolder}
- ${hiveOutputFolder}

Datoteka za definiciju tijeka rada (workflow.xml ovog praktičnog vodiča) prosljeđuje te vrijednosti Ova skripta HiveQL vrijeme izvođenja.

Datoteka za tijek rada i HiveQL datoteke spremaju se u spremniku blob.  Skriptu PowerShell će koristiti kasnije u ovom ćete praktičnom vodiču će kopirajte obje datoteke na zadani račun za pohranu. 

##<a name="submit-oozie-jobs-using-powershell"></a>Slanje Oozie zadatke pomoću komponente PowerShell

Azure PowerShell trenutno ne nudi sve cmdleta za definiranje Oozie zadatke. Cmdlet **Pozovite RestMethod** možete koristiti za pozivanje Oozie web-servisa. Oozie web services API je na HTTP REST JSON API-JA. Dodatne informacije o web-servisi Oozie API potražite u [dokumentaciji Apache Oozie 4.0] [ apache-oozie-400] (za HDInsight verzije 3.0) ili [dokumentaciju Apache Oozie 3.3.2] [ apache-oozie-332] (za HDInsight verzija 2.1).

Skriptu PowerShell u ovom odjeljku izvršava sljedeće korake:

1. Povezivanje s Azure.
2. Stvaranje grupe sustava Azure resursa. Dodatne informacije potražite u članku [Azure korištenje ljuske PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).
3. Stvaranje s poslužiteljem baze podataka SQL Azure, baze podataka Azure SQL i dvije tablice. Te se koristi Sqoop akcije u tijeku rada.

    Naziv tablice je *log4jLogCount*.

4. Stvaranje programa HDInsight Klaster se pokreće Oozie zadatke.

    Da biste pregledali klaster, možete koristiti portal za Azure ili Azure PowerShell.

5. Kopirajte datoteku oozie tijeka rada i skriptna datoteka HiveQL zadani datotečni sustav.

    Obje datoteke spremaju se u spremniku javno Blob.
    
    - Kopirajte skripte HiveQL (useoozie.hql) Azure prostora za pohranu (wasbs:///tutorials/useoozie/useoozie.hql).
    - Kopirajte workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
    - Kopiranje podatkovnu datoteku (/ example/data/sample.log) da biste wasbs:///tutorials/useoozie/data/sample.log.
     
6. Slanje u Oozie posla.

    Da biste pregledali rezultate OOzie posla, koristite Visual Studio ili druge alate za povezivanje s bazom podataka SQL Azure.

Evo skriptu.  Možete pokrenuti skriptu iz Windows Očisti filtar. Samo morate konfigurirati prvih 7 varijabli.

    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure subscription ID>"
    
    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"
    
    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"
    
    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion
    
    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green
    
    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    #endregion
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - copy Oozie workflow and HiveQL files
    
    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green
    
    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    
    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml
    
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql
    
    #endregion
    
    #region - copy the sample.log file
    
    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
    
    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log
    
    #endregion
    
    #region - submit Oozie job
    
    $storageUri="wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"
    
    $oozieJobName = $namePrefix + "OozieJob"
    
    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10
    
    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"
    
    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
    
    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>
    
    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>
    
    <property>
        <name>queueName</name>
        <value>default</value>
    </property>
    
    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>
    
    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>
    
    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>
    
    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>
    
    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>
    
    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>
    
    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>
    
    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>
    
    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>
    
    </configuration>
    "@
    
    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."
    
    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."
    
    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug
    
    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
    
    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")
    
    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }
    
    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green
    
    #endregion


**Da biste ponovno pokrenuli vodič**

Da biste ponovno pokrenuli tijek rada, morate izbrisati sljedeće:

- Izlazna datoteka skripte grozd
- Podaci u tablici log4jLogsCount

Slijedi primjer PowerShell skripte koje možete koristiti:

    $resourceGroupName = "<AzureResourceGroupName>"
    
    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

##<a name="next-steps"></a>Daljnji koraci
U ovom ćete praktičnom vodiču naučili kako definirati tijeka rada za Oozie i pokretanje programa Oozie posla pomoću komponente PowerShell. Dodatne informacije potražite u sljedećim člancima:

- [Korištenje temeljene koordinator Oozie s HDInsight][hdinsight-oozie-coordinator-time]
- [Početak rada s Hadoop s grozd u HDInsight radi analize korištenja mobilne slušalica][hdinsight-get-started]
- [Pomoću spremište blobova platforme Azure HDInsight][hdinsight-storage]
- [Administriranje HDInsight pomoću komponente PowerShell][hdinsight-admin-powershell]
- [Prijenos podataka za Hadoop poslove u HDInsight][hdinsight-upload-data]
- [Korištenje Sqoop s Hadoop u HDInsight][hdinsight-use-sqoop]
- [Korištenje grozd s Hadoop na HDInsight][hdinsight-use-hive]
- [Korištenje Svinja s Hadoop na HDInsight][hdinsight-use-pig]
- [Razvoj Java MapReduce programe za HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
