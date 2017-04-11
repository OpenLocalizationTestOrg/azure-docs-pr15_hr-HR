<properties
    pageTitle="Analiza podataka odgode leta pomoću Hadoop u HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti jedan skripte komponente Windows PowerShell za stvaranje programa HDInsight klaster, Pokreni grozd, Pokreni Sqoop i brisanje klaster."
    services="hdinsight"
    documentationCenter=""
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

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analizirajte podatke o kašnjenju leta pomoću grozd u HDInsight

Grozd nudi srednje vrijednosti primjene Hadoop MapReduce zadatke programa SQL nalik skriptnog jezika pod nazivom * [HiveQL][hadoop-hiveql]*, koji se mogu primijeniti pri sažimanje, slanje upita i analiza velike količine podataka.

> [AZURE.NOTE] Koraci u ovom dokumentu potreban je utemeljen na sustavu Windows HDInsight klaster. Koraci koje funkcioniraju sa sustavom Linux klaster, potražite u članku [Analiza podatke o kašnjenju leta pomoću Hive u HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Jedna od glavnih prednosti Azure HDInsight je odvojenosti pohrana podataka i računalnim. HDInsight koristi spremište blobova platforme Azure za pohranu podataka. Uobičajeni posao obuhvaća tri dijela:

1. **Spremanje podataka u spremište blobova platforme Azure.**  Ako, na primjer, weather podataka, podaci senzora, zapisnika web i u ovom slučaju, podatke o kašnjenju leta spremaju se u spremište blobova platforme Azure.
2. **Pokrenite zadatke.** Kad dođe vrijeme za obradu podataka pokretanja skripte komponente Windows PowerShell (ili klijentska aplikacija) da biste stvorili klaster programa HDInsight pokrenite zadacima i brisanje klaster. Poslove spremiti podatke za spremište blobova platforme Azure. Čak i nakon brisanja klaster zadržava se za izlazne podatke. Na taj način platite samo što koju ste potrošena.
3. **Dohvaćanje Izlaz iz spremišta blobova Azure**, ili pomoću ovog praktičnog vodiča, izvezite podatke u bazu podataka Azure SQL.

Na sljedećem su dijagramu ilustrira scenarij i strukturu ovog praktičnog vodiča:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Napomena**: brojevi u dijagramu odgovaraju naslove. **M** označava glavni postupak. **A** označava sadržaja u dodatak.

Glavni dio vodič prikazuje kako koristiti jedan skripte komponente Windows PowerShell za izvođenje sljedećih zadataka:

- Stvaranje programa klaster HDInsight.
- Pokrenuti posao grozd na klasteru za izračun prosječnog kašnjenja pri zračne luke. Podatke o kašnjenju leta pohranjuju se u račun za spremište blobova platforme Azure.
- Pokreni posao Sqoop da biste izvezli izlaz posao grozd baze podataka Azure SQL.
- Brisanje klaster HDInsight.

U appendixes, možete pronaći upute za prijenos podatke o kašnjenju leta, stvaranje i prijenos niza upita grozd i priprema baze podataka Azure SQL za posao Sqoop.

> [AZURE.NOTE] Koraci u ovom dokumentu su specifične za klastere HDInsight utemeljen na sustavu Windows. Koraci koji funkcioniraju sa sustavom Linux klaster, potražite u članku [Analiza leta podatke o kašnjenju pomoću grozd u HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće stavke:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Datoteke koje se koriste ovog praktičnog vodiča**

Pomoću ovog praktičnog vodiča koristi performanse na vrijeme podacima o zrakoplovnim letovima iz [istraživanja i Inovativne tehnologije Administracija, Statistika ured prijevoza ili RITA] [rita-website]. U spremniku za pohranu blobova platforme Azure s dozvolom pristupa javno Blob prenesena kopiju podataka. Dio skriptu PowerShell kopira podatke iz spremnik javno blob spremnik blob zadani svoj klaster. Skripte HiveQL također se kopiraju u isti spremnik Blob. Ako želite saznati kako get/prijenos podataka na račun za pohranu i upute za stvaranje i prijenos datoteka skripte HiveQL, pročitajte članak [dodatak A](#appendix-a) i [B dodatak](#appendix-b).

U sljedećoj su tablici navedeni datoteke koji se koriste u ovom ćete praktičnom vodiču:

<table border="1">
<tr><th>Datoteka</th><th>Opis</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Datoteka za HiveQL skripte koristi grozd posao. Ova skripta prenesena račun spremište blobova platforme Azure s javni pristup. <a href="#appendix-b">Dodatak B</a> sadrži upute za pripremu i prenijeti datoteku na vaš račun spremište blobova platforme Azure.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Unos podataka za posao grozd. Podaci prenesena račun spremište blobova platforme Azure s javni pristup. <a href="#appendix-a">Dodatak</a> ima upute za dohvaćanje podataka i prijenos podataka na vaš račun spremište blobova platforme Azure.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Izlaz put za posao grozd. Spremnik zadani služi za pohranu za izlazne podatke.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Grozd posao status mapa na zadani kontejner.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Stvaranje klaster i pokretanje grozd/Sqoop poslove

Hadoop MapReduce je obrade. Većina učinkovit način Pokreni grozd je da biste stvorili klaster za posao i izbrišite posao nakon dovršetka posla. Sljedeću skriptu pokriva cijelog procesa. Dodatne informacije o stvaranju programa HDInsight klaster i izvođenje zadataka grozd potražite u članku [Stvaranje Hadoop klastere u HDInsight] [ hdinsight-provision] i [Koristite vrste Hive s HDInsight][hdinsight-use-hive].

**Da biste pokrenuli grozd upita prema Azure PowerShell**

1. Stvaranje baze podataka Azure SQL i tablice u rezultatu Sqoop posla pomoću upute u [C dodatak](#appendix-c).
3. Otvorite Windows Očisti filtar i pokrenite sljedeću skriptu:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
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
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Povezivanje s bazom podataka sustava SQL i potražite stavku kašnjenja letova average po gradu u tablici AvgDelays:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Dodatak A - prijenos podatke o kašnjenju leta za spremište blobova platforme Azure
Prijenos podatkovne datoteke i datoteke skripti HiveQL (pročitajte članak [Dodatak B](#appendix-b)) zahtijeva planiranje. Ideja je spremati podatkovne datoteke i datoteke HiveQL prije stvaranja programa HDInsight klaster i pokretanja grozd posla. Imate dvije mogućnosti:

- **Koristiti isti prostor za pohranu Azure račun koji će klaster HDInsight koristiti kao zadani datotečni sustav.** Jer klaster HDInsight će imati pristup ključ za račun za pohranu, ne morate dodatno promijenite.
- **Koristite neki drugi račun za Azure prostora za pohranu iz servisa HDInsight klaster zadani datotečni sustav.** Ako je to slučaj, morate izmijeniti stvaranja dio skripte komponente Windows PowerShell u [klaster HDInsight stvaranje i pokretanje grozd/Sqoop zadataka](#runjob) da biste se povezali račun za pohranu kao još jedan račun za pohranu. Upute potražite u članku [Stvaranje Hadoop klastere u HDInsight][hdinsight-provision]. HDInsight klaster zatim zna pristupni ključ za račun za pohranu.

>[AZURE.NOTE] Put spremišta blobova platforme podatkovne datoteke je teško u HiveQL skriptna datoteka. Ne morate je ažurirati sukladno tome.

**Da biste preuzeli leta podataka**

1. Idite na [Istraživanje i Inovativne tehnologije Administracija ured za statistiku prijevoza][rita-website].
2. Na stranici odaberite sljedeće vrijednosti:

    <table border="1">
    <tr><th>Ime</th><th>Vrijednost</th></tr>
    <tr><td>Filtar godina</td><td>2013 </td></tr>
    <tr><td>Filtriranje razdoblje.</td><td>Siječanj</td></tr>
    <tr><td>Polja</td><td>*Godine*, *FlightDate*, *UniqueCarrier*, *prijenosni*, *FlightNum*, *OriginAirportID*, *polazište*, *OriginCityName*, *OriginState*, *DestAirportID*, *odredišne*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *kašnjenje*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (Očisti sva su druga polja)</td></tr>
    </table>

3. Kliknite **Preuzmi**.
4. Raspakiraj datoteku u mapu **C:\Tutorials\FlightDelay\2013Data** . Svaka datoteka je u CSV datoteku i približno 60GB po veličini.
5.  Preimenujte datoteku da biste naziv mjeseca koji sadrži podatke za. Ako, na primjer, datoteku koja sadrži podatke siječanj će biti pod nazivom *January.csv*.
6. Ponovite korake 2 i 5 za preuzimanje datoteke za svaki od 12 mjeseci u 2013. Trebat će vam najmanje jednu datoteku da biste pokrenuli vodič.  

**Da biste prenijeli podatke o kašnjenju leta spremište blobova platforme Azure**

1. Priprema parametre:

    <table border="1">
    <tr><th>Naziv varijable</th><th>Bilješke</th></tr>
    <tr><td>$storageAccountName</td><td>Račun za pohranu Azure mjesto na koje želite prenijeti podatke koje želite.</td></tr>
    <tr><td>$blobContainerName</td><td>Spremnik Blob mjesto na koje želite prenijeti podatke koje želite.</td></tr>
    </table>
2. Otvorite Azure Očisti filtar.
3. Zalijepite sljedeću skriptu u oknu skripte:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Pritisnite **F5** da biste pokrenuli skriptu.

Ako odlučite koristiti druge metode za prijenos datoteke, provjerite put datoteke je vodiči za/flightdelay/podataka. Vidjet ćete da sintaksa za pristup datotekama je:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Put vodiči za/flightdelay/podaci virtualna mapa koje ste napravili kad ste prenijeli datoteke. Provjerite ima li 12 datoteke, jedan za svaki mjesec.

>[AZURE.NOTE] Morate ažurirati grozd upita za čitanje na novo mjesto.

> Morate konfigurirati ili dozvole za pristup spremnik biti javno ili povezati s računom za pohranu klaster HDInsight. U suprotnom nizu upita grozd nećete moći pristupiti podatkovne datoteke.

---
##<a id="appendix-b"></a>Dodatak B - stvoriti i prenijeti HiveQL skripte

Pomoću komponente PowerShell Azure, možete pokrenuti višestruke izjave o HiveQL jedan po jedan ili pakiranje izjava HiveQL u skriptna datoteka. U ovom se odjeljku pokazuje kako stvoriti skriptu HiveQL i prenijeti skriptu u spremište blobova platforme Azure pomoću Azure PowerShell. Grozd zahtijeva HiveQL skripte za pohranu u spremište blobova platforme Azure.

Skripta HiveQL će učinite sljedeće:

1. **Odbacivanje delays_raw tablice**, u slučaju tablice već postoji.
2. **Stvaranje vanjske tablicu vrste Hive delays_raw** koja pokazuje na mjesto za pohranu Blob s datotekama odgode leta. Ovaj upit određuje da su polja razgraničeno po "," i retke su prekinuta "\n". To predstavlja problem kada vrijednosti polja sadrže zareze jer grozd ne može razlikovati točku sa zarezom koja je graničnik polja i one koja je dio vrijednosti polja (koji je slučaja u vrijednosti polja za IZVOR\_GRAD\_ime i Odredišne\_GRAD\_naziv). Da biste to riješili, upit stvara TEMP stupaca sadrže podatke koje neispravno podijelite u stupce.  
3. **Odbacivanje kašnjenja tablice**, u slučaju tablice već postoji.
4. **Stvaranje tablice kašnjenja**. Da biste očistili podatke prije daljnje obrade korisno je. Ovaj upit stvara novu tablicu, *kašnjenja*u tablici delays_raw. Imajte na umu da se ne kopiraju TEMP stupaca (kao što je već rečeno) i da biste uklonili navodnim znakovima iz podataka koristi funkciju **podniz** .
5. **Izračun prosjeka vremenske prognoze odgode i grupama rezultate s nazivom grada.** Će Izlaz i rezultate u spremište blobova platforme. Imajte na umu da upit će uklonili apostrofe iz podataka i redaka u kojima je null vrijednost za **weather_delay** će se isključiti. To je nužno jer Sqoop, koristiti kasnije u ovom ćete praktičnom vodiču ne se bez poteškoća rukovati te se vrijednosti po zadanom.

Potpuni popis naredbi HiveQL potražite u članku [Vrste Hive jezika za definiranje podataka][hadoop-hiveql]. Svaka naredba HiveQL morate završiti točkom sa zarezom.

**Da biste stvorili HiveQL skriptna datoteka**

1. Priprema parametre:

    <table border="1">
    <tr><th>Naziv varijable</th><th>Bilješke</th></tr>
    <tr><td>$storageAccountName</td><td>Mjesto na koje želite prenijeti HiveQL skripta za račun Azure prostor za pohranu.</td></tr>
    <tr><td>$blobContainerName</td><td>Spremnik Blob mjesto na koje želite prenijeti HiveQL skripta za.</td></tr>
    </table>
2. Otvorite Azure Očisti filtar.

3. Kopirajte i zalijepite sljedeću skriptu u oknu skripte:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Evo varijable koristi u skripti:

    - **$hqlLocalFileName** - skriptu sprema skriptna datoteka HiveQL lokalno prije prijenosa sa spremištem blobova. To je naziv datoteke. Zadana vrijednost je <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** – to je HiveQL skripte blob naziv datoteke koristiti u spremište blobova platforme Azure. Zadana vrijednost je tutorials/flightdelay/flightdelays.hql. Budući da se ona će biti napisani izravno u spremište blobova platforme Azure, nema u "/" na početku blob naziv. Ako želite pristupiti datoteci iz spremišta blobova, morat ćete dodati na "/" na početku naziv datoteke.
    - **$srcDataFolder** i **$dstDataFolder** - = "flightdelay/vodiči za/data" = "flightdelay/vodiči za/izlaz"


---
##<a id="appendix-c"></a>Dodatak C - priprema baze podataka Azure SQL za izlaz Sqoop posla
**Da biste pripremili SQL baze podataka (spajanje to sa scenarijem Sqoop)**

1. Priprema parametre:

    <table border="1">
    <tr><th>Naziv varijable</th><th>Bilješke</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Naziv poslužitelja baze podataka Azure SQL. Unesite ništa da biste stvorili novi poslužitelj.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Ime za prijavu za poslužitelj baze podataka Azure SQL. Ako je $sqlDatabaseServerName postojećeg poslužitelja, prijavu i lozinka za prijavu koriste se za provjeru autentičnosti s poslužiteljem. U suprotnom se koriste za stvaranje novog poslužitelja.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Lozinka prijava za poslužitelj baze podataka Azure SQL.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Ta se vrijednost koristi samo kada stvarate novi poslužitelj za Azure baze podataka.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>SQL baza podataka koristi za stvaranje tablice AvgDelays za posao Sqoop. Ostavite praznima će stvoriti bazu pod nazivom HDISqoop. Naziv tablice u rezultatu posao Sqoop je AvgDelays. </td></tr>
    </table>
2. Otvorite Azure Očisti filtar.
3. Kopirajte i zalijepite sljedeću skriptu u oknu skripte:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Skripta koristi servis za prijenos (REST) representational stanje, http://bot.whatismyipaddress.com, za dohvaćanje vanjskih IP adresa. IP adresa se koristi za stvaranje pravila vatrozida za SQL server za bazu podataka.  

    Evo nekih varijable koristi u skripti:

    - **$ipAddressRestService** - zadana vrijednost je http://bot.whatismyipaddress.com. Je javna IP adresa servisa REST za dohvaćanje vanjskih IP adrese. Ako želite možete koristiti druge servise. Vanjske IP adrese dohvatiti pomoću usluge će se koristiti za stvaranje pravila vatrozida za poslužitelj baze podataka Azure SQL tako da se pristup bazi podataka iz vaše radne stanice (pomoću skripte komponente Windows PowerShell).
    - **$fireWallRuleName** – to je naziv pravila vatrozida za poslužitelj baze podataka Azure SQL. Zadani naziv je <u>FlightDelay</u>. Možete preimenovati ako želite.
    - **$sqlDatabaseMaxSizeGB** - tu vrijednost koristi se samo tijekom stvaranja novog poslužitelj baze podataka Azure SQL. Zadana je vrijednost 10GB. 10GB je dovoljni za ovog praktičnog vodiča.
    - **$sqlDatabaseName** - tu vrijednost koristi se samo tijekom stvaranja nove baze podataka Azure SQL. Zadana vrijednost je HDISqoop. Ako je preimenovati, skripte komponente Windows PowerShell Sqoop morate ažurirati sukladno tome.

4. Pritisnite **F5** da biste pokrenuli skriptu.
5. Provjerite valjanost skripte izlaz. Provjerite skripte je uspješno pokrenut.

##<a id="nextsteps"></a>Daljnji koraci
Sada znate kako prenijeti datoteke na spremište blobova platforme Azure, kako pomoću podataka iz spremišta blobova Azure popunjavanje tablicu vrste Hive, pokretanje grozd upite i kako koristiti Sqoop za izvoz podataka iz HDFS s bazom podataka Azure SQL. Dodatne informacije potražite u sljedećim člancima:

* [Uvod u HDInsight][hdinsight-get-started]
* [Korištenje grozd s HDInsight][hdinsight-use-hive]
* [Korištenje Oozie s HDInsight][hdinsight-use-oozie]
* [Korištenje Sqoop s HDInsight][hdinsight-use-sqoop]
* [Korištenje Svinja s HDInsight][hdinsight-use-pig]
* [Razvoj Java MapReduce programe za HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
