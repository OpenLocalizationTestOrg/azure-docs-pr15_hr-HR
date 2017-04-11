<properties
    pageTitle="Početak rada sa zadacima elastic baze podataka"
    description="kako koristiti poslove elastic baze podataka"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Početak rada sa zadacima Elastic baze podataka

Elastic zadatke baze podataka (pretpregled) za baze podataka SQL Azure omogućuje vam pouzdanosti izvršavanje T-SQL skripte koje se protežu na više baza podataka tijekom automatski ponovni i omogućivanje jamstva usmjerenog dovršenog. Dodatne informacije o značajci posao Elastic baze podataka, pročitajte članak [značajke implicitno](sql-database-elastic-jobs-overview.md).

U ovoj se temi proširuje uzorka pronađeni u [Početak rada s alatima Elastic baze podataka](sql-database-elastic-scale-get-started.md). Kada završi, će: Saznajte kako stvoriti i upravljanje zadacima koji upravljaju grupe povezanih baze podataka. Nije potrebno da biste koristili alate Elastic mjerilo da bi se mogla iskoristiti prednosti Elastic zadatke.

## <a name="prerequisites"></a>Preduvjeti

Preuzmite i pokrenite [Uvod u rad s uzorak Alati Elastic baze podataka](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Stvaranje karte Upravitelj pomoću aplikacije za uzorak u shard

Ovdje ćete stvoriti kartu shard Upravitelj zajedno s nekoliko shards, nakon čega slijedi unosa podataka u na shards. Ako već imate shards postaviti s sharded podataka, preskočite na sljedeći način i prijelaz na sljedeću sekciju.

1. Stvaranje i pokretanje aplikacije za **Početak rada s alatima Elastic baze podataka** uzorka. Slijedite korake do koraka 7 u odjeljku [preuzeti i pokrenuti aplikaciju uzorka](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Na kraju 7 korak, vidjet ćete sljedeći naredbeni redak:

    ![naredbeni redak][1]

2.  U prozoru za naredbe upišite "1", a zatim pritisnite tipku **Enter**. Stvara se shard Upravitelj karte i dodaje dva shards na poslužitelj. Upišite "3" pa pritisnite **Enter**; Ponovite ovu akciju četiri puta. To umeće ogledne podatkovne retke u vašem shards.

3.  [Portal za Azure](https://portal.azure.com) trebao pokazati tri nove baze podataka u poslužitelja v12:

    ![Potvrda Visual Studio][2]

    Sada ćemo stvoriti zbirka prilagođene baze podataka koja odražava sve baze podataka na karti shard. To će nam omogućuju stvaranje i izvršavanje posla koji preko shards dodali novu tablicu.

U nastavku ćemo bi obično stvoriti cilj kartu shard, pomoću cmdleta **New-AzureSqlJobTarget** . Upravitelj baze podataka shard karte mora biti postavljeno kao cilj baze podataka, a zatim će se određene shard karte navedena kao cilj. Umjesto toga, ne možemo prelaska Nabrajanje sve baze podataka na poslužitelju te dodavanje baze podataka u novu prilagođenu zbirku osim glavnom bazom podataka.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Stvoriti prilagođene zbirke i dodaje sve baze podataka na poslužitelju ciljni prilagođene zbirke osim matrice.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Stvaranje T SQL skripte za izvršavanje preko baze podataka

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Stvaranje zadatka za izvršavanje skriptu preko prilagođenu grupu baze podataka

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Izvršavanje posla 

Sljedeću skriptu komponente PowerShell mogu se izvoditi postojećeg posla:

Ažurirajte sljedeće varijabla u skladu s vizualnim naziv željene posla koji će se izvršiti:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Dohvaćanje stanja jedan zadatak izvođenja

Koristite isti cmdlet **Get-AzureSqlJobExecution** s parametrom **IncludeChildren** da biste prikazali stanje podređeni izvršavanja posla, odnosno određene stanja za svaki zadatak izvođenja svaki bazi podataka ciljani posla.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Prikaz stanja preko više izvršavanja posla

Cmdlet **Get-AzureSqlJobExecution** ima više neobavezni parametar koji se mogu koristiti za prikaz više izvršavanja posla, filtrirano kroz navedeni parametre. Sljedeće prikazuje dio mogući načini korištenja Get-AzureSqlJobExecution:

Dohvati sva izvršavanja aktivan posao gornje razine:

    Get-AzureSqlJobExecution

Dohvati sva izvršavanja gornje razine posla, uključujući izvršavanja Neaktivni posao:

    Get-AzureSqlJobExecution -IncludeInactive

Dohvati sva izvršavanja posla podređene ID navedeni posla izvođenja, uključujući izvršavanja Neaktivni posla:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Dohvaćanje sva posla izvršavanja stvoren pomoću raspored / posla kombinacije, uključujući Neaktivni zadaci:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Dohvati sve zadatke ciljanja kartu navedeni shard, uključujući neaktivne zadacima:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Dohvati sve zadatke ciljanja navedeni prilagođene zbirka, uključujući neaktivne zadacima:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Dohvaćanje popisa izvršavanja zadataka posla unutar određeni posao izvođenja:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Dohvaćanje detalja za izvršavanje zadataka posla:

Da biste pogledali detalje o izvršavanje zadataka posla koji je osobito praktična za ispravljanje pogrešaka Neuspjelo izvršavanje može se koristiti sljedeću skriptu komponente PowerShell.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Dohvaćanje pogreške unutar izvršavanja zadataka posla

Objekt JobTaskExecution obuhvaća svojstva za životni ciklus zadatka uz svojstvu poruke. Ako je zadatak zadatka nije uspjelo izvršavanje na, svojstvo životni ciklus će biti postavljeno na *nije uspjelo* i svojstvo poruke postavit će se rezultat poruka iznimke i snopa. Ako posao nije uspjela, važno je da biste vidjeli detalje zadataka posla koji nije uspjela zadani posla.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Čekanje izvođenja zadatka da biste dovršili

Sljedeću skriptu komponente PowerShell mogu koristiti ćete morati pričekati da biste dovršili zadatak:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Stvaranje pravila prilagođenih izvođenja

Elastic baze podataka zadataka podržava stvaranje pravila prilagođenih izvođenja koje se mogu primijeniti prilikom pokretanja zadataka.
  
Za definiranje trenutno dopušta izvršavanje pravila:

* Naziv: Identifikator za izvršavanje pravila.
* Vremensko ograničenje posla: Ukupno vrijeme prije posao će prekinuti Elastic poslovi baze podataka.
* Početni Interval pokušaj: Interval čekanja prije prve pokušajte ponovno.
* Maksimalan Interval pokušaj: Kapica intervala Ponovi da biste koristili.
* Ponovite Interval Backoff koeficijent: Koeficijent koriste za izračun sljedeći interval između ponovne pokušaje.  Koristi sljedeću formulu: (početni Interval ponovnog pokušaja) * Math.pow ((Interval Backoff koeficijent) (broj od ponovnih pokušaja) - 2). 
* Maksimalna pokušaja: Maksimalni broj pokušaj pokušava izvršiti unutar posao.

Zadani pravilnik izvođenja koristi sljedeće vrijednosti:

* Naziv: Zadana izvođenja pravila
* Vremensko ograničenje posla: jedan tjedan
* Početni Interval pokušaj: 100 milisekundama
* Interval maksimalni pokušaj: 30 minuta
* Ponovite koeficijent Interval: 2
* Maksimalna pokušaja: 2.147.483.647

Stvaranje pravila za željenu izvođenja:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Ažuriranje pravila za prilagođene izvođenja

Ažurirajte željeni izvođenja pravila da biste ažurirali:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Otkazivanje posla

Elastic baze podataka zadataka podržava zahtjeva za otkazivanje zadatke.  Ako Elastic baze podataka zadataka otkrije otkazivanja zahtjeva za posao trenutno izvršava, će pokušati zaustaviti posao.

Postoje dva načina Elastic zadatke baze podataka možete izvršiti otkazati:

1. Otkazivanje trenutno izvršavanja zadataka: Ako otkazati je otkriven pokrenutom zadatak trenutno, otkazati će se izvršiti unutar trenutno izvršni aspekt zadatak.  Na primjer: Ako dugo izvodi upit trenutno se izvodi kad je pokušaj otkazivanja, će biti pokušaj otkazivanje upita.
2. Odustajanje od ponovne pokušaje zadatka: Ako niti kontrola otkazati otkrio prije pokretanja zadatak za izvršavanje, kontrola niti će izbjegli pokretanje zadatka i deklarirati zahtjev kao otkazana.

Ako je zadatak otkazivanja zatražili za posao nadređenog, zahtjev za otkazivanje će biti na snazi za posao nadređenog i svih njezinih podređenih zadataka.
 
Da biste poslali zahtjev za otkazivanje, koristite cmdlet **Zaustavi AzureSqlJobExecution** i postavite parametar **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Brisanje posla po nazivu i povijest posla

Elastic baze podataka zadataka podržava asinkronog brisanja zadataka. Zadatak možete označiti za brisanje i sustav će izbrisati posla i njegov dosadašnje iskustvo nakon sva izvršavanja zadatka dovršite za posao. Sustav će otkazati automatski izvršavanja aktivan posao.  

Umjesto toga Zaustavi AzureSqlJobExecution morate pozvati da biste poništili izvršavanja aktivan posao.

Da biste pokrenuli brisanja zadatka, koristite cmdlet **Ukloni AzureSqlJob** i postavite parametar **JobName** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Stvorite ciljnu prilagođene baze podataka
Ciljevi prilagođene baze podataka može se definirati u zadacima Elastic baze podataka koji se može koristiti za izvršavanje izravno ili za uključivanje u skupini prilagođene baze podataka. Budući da **grupe Elastic baze podataka** nisu još izravno podržane putem komponente PowerShell API-ji, jednostavno je stvoriti cilj prilagođene baze podataka i ciljne zbirke prilagođene baze podataka što obuhvaća sve baze podataka u.

Postavite sljedeće varijable u skladu s vizualnim na željene informacije o bazi podataka:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Stvaranje zbirke cilj prilagođene baze podataka
Da biste omogućili izvođenja preko više definiranih baze podataka ciljnih web-mjesta može se definirati cilj zbirke prilagođene baze podataka. Nakon stvaranja grupe baze podataka baze podataka može biti povezan s ciljem prilagođene zbirke.

Postavite sljedeće varijable u skladu s vizualnim konfiguriranje ciljne željene prilagođene zbirke:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Dodavanje baza podataka u ciljni zbirke prilagođene baze podataka

Ciljevi baze podataka mogu se pridružiti prilagođene baze podataka zbirke ciljnih web-mjesta da biste stvorili grupu baza podataka. Prilikom svakog zadatka stvaranja namijenjen ciljni zbirke prilagođene baze podataka, će se proširiti ciljne baze podataka u vrijeme izvođenja pridružene grupi.

Dodajte željene baze podataka na određenu zbirku prilagođene:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Pregledajte baze podataka unutar zbirke cilj prilagođene baze podataka

Pomoću cmdleta **Get-AzureSqlJobTarget** za dohvaćanje podređeni baze podataka unutar zbirke cilj prilagođene baze podataka. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Stvaranje zadatka za izvršavanje skriptu preko cilj zbirke prilagođene baze podataka

Pomoću cmdleta **New-AzureSqlJob** da biste stvorili zadatak prema grupi baze podataka koje je definirao cilj zbirke prilagođene baze podataka. Elastic baze podataka zadataka će proširite posao u više podređenih zadataka svaki odgovara s bazom podataka povezan s ciljnom zbirke prilagođene baze podataka i provjerite je li skriptu izvršeno na svakom bazi podataka. Ponovno je važno skripte su idempotent se prebacuju na nekoliko pokušaja.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Prikupljanje podataka preko baze podataka

**Elastic baze podataka zadataka** podržava izvršavanja upita u grupi baze podataka i šalje rezultate tablicu navedenu baze podataka. Tablice se možete mu nakon toga da biste vidjeli rezultate upita iz svakog baze podataka. To omogućuje asinkronog mehanizam za izvršavanje upita preko mnoge baze podataka. Pogreška slučajevima kao što je jedan od baze podataka koja se privremeno nedostupan upravlja automatski putem ponovne pokušaje.

Navedeni odredišne tablice će se automatski stvarati ako se još ne postoji, koji se podudaraju u shemi skup vraćenih rezultata. Skripta izvođenja vraća više skupova rezultata, zadacima Elastic baze podataka samo pošaljite prvoga u navedeni odredišnu tablicu.

Sljedeću skriptu komponente PowerShell mogu se izvoditi skriptu prikupljanje rezultatima u navedenoj tablici. Ova skripta pretpostavlja skriptu T SQL stvoreno je koji proizvodi skupu jedan rezultat, a stvoreno je cilj zbirke prilagođene baze podataka.

Postavite sljedeće željeni skripte, vjerodajnice i izvođenja cilj:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Stvaranje i pokretanje zadatka scenarije zbirke podataka
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Stvaranje rasporeda za izvršavanje posla korištenja okidača za posao

Sljedeću skriptu komponente PowerShell se može koristiti za stvaranje reoccurring rasporeda. Ova skripta koristi interval jedne minute, ali novo AzureSqlJobSchedule podržava i - DayInterval, - HourInterval, - MonthInterval, i - WeekInterval parametara. Mogu se kreirati rasporede koji izvesti samo jedanput po prosljeđivanje - OneTime.

Stvaranje novog rasporeda:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Stvaranje zadatka okidača da bi se zadatak izvršeno zakazano vrijeme

Okidač posao može se definirati da bi se zadatak izvršiti prema zakazano vrijeme. Da biste stvorili zadatak okidača se poslužite sljedeću skriptu komponente PowerShell.

Postavite sljedeće varijable odgovaraju željeni posla i rasporedom:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Uklanjanje zakazanog pridruživanje da biste zaustavili posao izvršavaju na raspored

Da biste prekinuli ponovno pojavljivanje izvođenja posla pomoću okidača posla, moguće je ukloniti okidača posao. Uklonite okidač zadatak da biste zaustavili posao iz koja se izvršava u skladu s rasporedom pomoću cmdleta **Ukloni AzureSqlJobTrigger** .

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Uvoz rezultata upita elastic baze podataka u programu Excel

 Rezultati iz upita u datoteke programa Excel možete uvesti.

1. Pokrenite Excel 2013.
2.  Otiđite na vrpci **Podaci** .
3.  Kliknite **Iz drugih izvora** , a zatim kliknite **Iz sustava SQL Server**.

    ![Excel uvoz iz drugih izvora][5]
4.  U **Čarobnjaku za povezivanje podataka** upišite poslužitelja naziva i prijava vjerodajnice. Zatim kliknite **Dalje**.
5.  U dijaloškom okviru **Odaberite bazu podataka koja sadrži podatke koje želite**, odaberite **ElasticDBQuery** bazu podataka.
6.  Odaberite tablicu **Kupci** u prikazu popisa, a zatim kliknite **Dalje**. Zatim kliknite **Završi**.
7.  U obrascu **Uvoz podataka** u odjeljku **Odaberite način na koji želite prikazati podatke u radnu knjigu**, odaberite **tablicu** , a zatim kliknite **u redu**.

Sve retke iz tablice **Kupci** , pohranjene u različitim shards popuniti na list programa Excel.

## <a name="next-steps"></a>Daljnji koraci
Sada možete koristiti funkcije podataka programa Excel. Pomoću niza za povezivanje s naziv poslužitelja, naziv baze podataka i vjerodajnice za povezivanje vaše BI i podataka Alati za integraciju s bazom podataka elastic upita. Provjerite je li SQL Server podržana kao izvor podataka za vaše alat. Pogledajte elastic upita baze podataka i vanjske tablice baš kao i sve druge baze podataka SQL Server i SQL Server tablice koje se želite povezati pomoću alata za vaše.

### <a name="cost"></a>Trošak
Postoji bez dodatnih troškova za korištenje značajke upita Elastic baze podataka. Međutim, trenutno ta je značajka dostupna samo na baze podataka premium kao krajnju točku, ali u shards mogu biti bilo koji servis sloju.

Informacije o cijenama potražite u članku [Cijene Detalji baze podataka za SQL](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
