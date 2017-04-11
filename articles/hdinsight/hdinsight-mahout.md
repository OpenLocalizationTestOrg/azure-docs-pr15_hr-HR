<properties
    pageTitle="Generiranje preporuke pomoću Mahout i HDInsight utemeljen na sustavu WIndows | Microsoft Azure"
    description="Saznajte kako koristiti Apache Mahout strojnog učenja biblioteke da biste generirali film preporuke za HDInsight utemeljen na sustavu Windows (Hadoop)."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Generiranje film preporuke pomoću Apache Mahout Hadoop u HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Saznajte kako koristiti [Apache Mahout](http://mahout.apache.org) strojnog učenja biblioteka sa servisom Azure HDInsight da biste generirali film preporuke.

> [AZURE.NOTE] Koraci u ovom dokumentu potreban je Windows klijenta i utemeljen na sustavu Windows HDInsight klaster. Informacije o korištenju Mahout iz Linux, OS X ili Unix klijent, s operacijskim sustavom Linux HDInsight klaster, potražite u članku [Generiraj film preporuke pomoću Mahout Apache s operacijskim sustavom Linux Hadoop u HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Što ćete saznati

Mahout je na [računalu učenje] [ ml] biblioteka za Apache Hadoop. Mahout sadrži algoritama za obradu podataka, kao što su filtriranja, klasifikacije, i Klasteriranje. U ovom se članku će koristiti modul za preporuke za generiranje preporuke film koji se temelje na filmova pojavljuje prijateljima. Će Saznajte i kako izvesti klasifikacije s skupa stabala odluka. To će naučiti sljedeće:

* Pokretanje Mahout zadatke pomoću komponente Windows PowerShell

* Iz naredbenog retka Hadoop pokretanje Mahout poslove

* Kako instalirati Mahout na HDInsight 3.0 i HDInsight 2.0 klaster

    > [AZURE.NOTE] Mahout navedeni su verzijom HDInsight 3.1 skupina. Ako koristite stariju verziju servisa HDInsight potražite u članku [Instalacija Mahout](#install) prije nego što nastavite.

##<a name="prerequisites"></a>preduvjeti

- **Klaster utemeljen na sustavu programa Windows Hadoop u HDInsight**. Informacije o stvaranju nešto potražite u članku [Prvi koraci pri korištenju Hadoop u HDInsight][getstarted]
- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Generiranje preporuke pomoću komponente Windows PowerShell

> [AZURE.NOTE] Iako posao koristi u ovaj odjeljak radi pomoću komponente Windows PowerShell, mnoge klase dao Mahout trenutno funkcionirati s komponentom Windows PowerShell i mora se izvoditi pomoću naredbenog retka Hadoop. Popis klase koji rade s komponentom Windows PowerShell potražite u odjeljku [Otklanjanje poteškoća](#troubleshooting) .
>
> Primjer korištenja naredbenog retka Hadoop pokretanje Mahout zadacima, potražite u članku [Classify podataka pomoću Hadoop naredbenog retka](#classify).

Jedna od funkcija koju je dodijelio Mahout je preporuka modul. Ovaj modul prihvaća podataka u obliku `userID`, `itemId`, i `prefValue` (preferencama korisnika za stavku). Mahout zatim izvedite analize zajednički occurance da biste odredili: _korisnicima koji imaju preferenca stavke su i preference za te stavke_. Mahout zatim određuje korisnika završavaju na preference poput stavke, koji se može koristiti za preporuke.

Slijedi primjer iznimno jednostavne koji koristi filmova:

* __Suautorstvo occurance__: Joe, Alice i Teo sviđa _Wars zvjezdica_, _U Empire Strikes natrag_i _Povratna od na Jedi_. Mahout određuje da korisnika i kao što su neki od ovih filmova kao što su dvije.

* __Suautorstvo occurance__: Teo i Alice i sviđa _U Phantom Menace_, _napada na Clones_i _Revenge u Sith_. Mahout određuje da korisnici kojima se sviđa prethodna tri filmovi i vam se sviđa među njima.

* __Sličnosti preporuka__: jer Joe sviđa prva tri filmova, Mahout razmatra filmova taj drugi korisnici s slične preference sviđa, ali Joe sadrži nadzirane (sviđa/ocjena). U ovom slučaju Mahout preporučuje _U Phantom Menace_, _napada na Clones_i _Revenge u Sith_.

###<a name="understanding-the-data"></a>Razumijevanje podataka

Jednostavnijeg [Istraživanje GroupLens] [ movielens] pruža podatke ocjena filmova u obliku koji je kompatibilan s Mahout. Tih podataka dostupna je na svoj klaster zadani pohranu pri `/HdiSamples/MahoutMovieData`.

Postoje dvije datoteke `moviedb.txt` (informacije o filmovi,) i `user-ratings.txt`. Korisnik ratings.txt datoteka koristi tijekom analize, dok moviedb.txt koristi radi infromation jednostavnih tekst prilikom prikaza rezultata analizu.

Podatke sadržani u korisnika ratings.txt sadrži strukture `userID`, `movieID`, `userRating`, i `timestamp`, koje nam govore kako se svaki korisnik s ocjenom film. Evo primjera podataka:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Pokrenuti posla

Da biste pokrenuli zadatak koji koristi preporuke modul Mahout s podacima film, koristite sljedeću skriptu komponente Windows PowerShell:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
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
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout poslove uklanjanje privremene podataka koji je stvoren tijekom obrade posao. Na `--tempDir` parametar je naveden u posao primjer izdvojiti privremene datoteke u određenim imenik.

Zadatak Mahout vratite izlaz STDOUT. Umjesto toga, sprema se u direktoriju navedeni izlazni kao __dio-r-00000__. Skripta preuzeti datoteku __output.txt__ u trenutnom direktoriju na vaše radne stanice.

Slijedi primjer sadržaja ove datoteke:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Prvi stupac na `userID`. Vrijednosti koji se nalazi u ' [' i '] "su `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Prikaz Izlaz

Iako generirani Izlaz možda u redu za korištenje u aplikaciji, nije vrlo čitljiv. Na `moviedb.txt` s poslužitelja možete koristiti da biste riješili na `movieId` u film ime, ali morate preuzeti i ocjene datoteku s poslužitelja pomoću sljedeće skripte:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
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
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Nakon što ste preuzeli datoteke, koristite sljedeću skriptu komponente PowerShell da biste prikazali preporuke s nazivima film:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Slijedi primjer pokretanjem skripte:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Izlaz trebao izgledati otprilike ovako:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Klasifikaciju podataka pomoću Hadoop naredbenog retka

Jedan od načina klasifikacije dostupno u sklopu Mahout je da biste sastavili [slučajni skupa stabala][forest]. To je u više koraka koji uključuje pomoću obuka podataka da biste generirali odlukama koja se zatim koriste za klasifikaciju podataka. Koristi se klase __org.apache.mahout.classifier.df.tools.Describe__ nudi Mahout. Trenutno mora biti funkcionirat pomoću Hadoop naredbenog retka.

###<a name="load-the-data"></a>Učitavanje podataka

1. Preuzmite sljedeće datoteke iz [skupa podataka u NSL KDD](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): obuka datoteka

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): testiranje podataka

2. Otvorite svaku datoteku i uklonite retke na vrhu koje započinju s '@', , a zatim spremite datoteke. Ako to nije uklonjen, primit ćete poruku o pogrešci prilikom korištenja ove podatke s Mahout.

2. Prenesite datoteke __oglednim podacima__. To možete učiniti pomoću sljedeće skripte. Zamijenite __CLUSTERNAME__ naziv klaster HDInsight. Naziv datoteke zamijenite nazivom fonta datoteku da biste je moguće prenijeti.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>Pokrenuti posla

1. Ovaj zadatak zahtijeva Hadoop naredbenog retka. Omogućivanje udaljene radne površine za HDInsight klaster, a zatim s njim povezati slijedeći upute u [klastere HDInsight pomoću RDP za povezivanje](hdinsight-administer-use-management-portal.md#rdp).

3. Otvorite naredbeni redak Hadoop pomoću ikone __Hadoop naredbeni redak__ nakon povezivanja:

    ![hadoop eža][hadoopcli]

3. Koristite sljedeću naredbu da biste generirali opisnika datoteke (__KDDTrain + .info__), koji koristi Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    Na `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` opisuje atribute podatke u datoteci. Na primjer, L označava oznaku.

4. Stvaranje skupa stabala od odlukama pomoću sljedeće naredbe:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Izlaz iz ovog postupka pohranjenu u direktoriju __nsl-skupa stabala__ , koja se nalazi u prostor za pohranu za svoj klaster HDInsight pri __ wasbs://user/&lt;korisničko ime > / nsl-skupa stabala/nsl-forest.seq. Na &lt;korisničko ime > je korisničko ime koje ste koristili za sesije udaljene radne površine. Datoteka je pročitati ljudi.

5. Testirajte u skup stabala klasifikacije __KDDTest + .arff__ skupu podataka. Koristite sljedeću naredbu:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Ta naredba Vrati sažetak informacija o postupku klasifikacija sličnu ovoj:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Ovaj zadatak proizvodi i datoteke koja se nalazi pri __wasbs:///example/data/predictions/KDDTest+.arff.out__. No tu datoteku nije pročitati ljudi.

> [AZURE.NOTE] Mahout poslove prebrisati datoteke. Ako želite ponovno pokrenuti ove zadatke, morate izbrisati datoteke koje su stvorene pomoću prethodnih poslova.

##<a name="troubleshooting"></a>Otklanjanje poteškoća

###<a name="install"></a>Instalacija Mahout

Mahout je instaliran na klastere HDInsight 3.1 pa ga možete ručno instalirati na HDInsight 3.0 ili HDInsight 2.1 klastere pomoću sljedećih koraka:

1. Verzija Mahout da biste koristili ovisi o verziji HDInsight svoj klaster. Verzija klaster možete pronaći tako da Prikaz svojstava za klaster na portalu za Azure.

  * __HDInsight 2.1__, možete preuzeti Java arhiva (POSUDU) datoteka koja sadrži [Mahout 0.9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar).

  * __HDInsight 3.0__, morate [Sastavljanje Mahout iz izvora] [ build] i određivanje verzija Hadoop nudi HDInsight. Instalirali preduvjete navedene na stranici Stvaranje, preuzmite izvorišnog web-mjesta, a zatim koristite sljedeću naredbu da biste stvorili datoteke posudu Mahout:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Kada je Mahout 1.0, moći će se koristiti ugrađene paketa za HDInsight 3.0.

2. Prijenos datoteke posudu __primjer/staklenke__ u zadani prostor za pohranu za svoj klaster. Zamijenite CLUSTERNAME u sljedeću skriptu naziv svoj klaster HDInsight i naziv datoteke zamijenite put do datoteke __mahout-coure-0.9-job.jar__ ...

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Ne može se prebrisati datoteke

Nemoj poslove mahout ne očistiti privremene datoteke koje su stvorene tijekom obrade. Osim toga, poslove će prebrisati postojeće Izlazna datoteka.

Da biste izbjegli pogreške prilikom izvođenja Mahout zadacima, brisanje privremenih i izlazni datoteka između pokretanja ili koristite nazive jedinstveni privremene i izlazni direktorija.

###<a name="cannot-find-the-jar-file"></a>Nije moguće pronaći datoteku POSUDU

HDInsight 3.1 klastere obuhvaćaju Mahout. Put i naziv uvrstiti broj verzije Mahout koji je instaliran na klaster. Primjer skripte komponente Windows PowerShell ovog praktičnog vodiča koristi put valjane na 2015 studenom, ali broj verzije promijenit će se u budućnosti ažuriranja HDInsight. Da biste utvrdili trenutni put do datoteke Mahout POSUDU za svoj klaster, koristite sljedeću naredbu komponente Windows PowerShell i izmjena skripte referentni put datoteke koje se vraćaju:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Tečajevi koji rade s komponentom Windows PowerShell

Mahout poslove koje koriste sljedeće klase vraćaju raznih poruka o pogrešci ako se koriste iz komponente Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Da biste pokrenuli poslove koje koriste ove klase, povezivanje s HDInsight klaster, a pokrenuti zadatke pomoću Hadoop naredbenog retka. Primjer potražite u članku [Classify podataka pomoću Hadoop naredbenog retka](#classify) .

##<a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako koristiti Mahout, Uvod u rad s podacima na HDInsight drugi načini:

* [Vrste Hive s HDInsight](hdinsight-use-hive.md)
* [Svinja s HDInsight](hdinsight-use-pig.md)
* [MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
