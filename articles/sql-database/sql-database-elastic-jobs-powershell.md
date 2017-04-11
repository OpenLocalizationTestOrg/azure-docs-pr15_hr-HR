<properties 
    pageTitle="Stvaranje i upravljanje zadacima Elastic baze podataka pomoću komponente PowerShell" 
    description="PowerShell koji se koriste za upravljanje bazom podataka SQL Azure grupe" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Stvaranje i upravljanje zadacima elastic baze podataka za SQL baze podataka pomoću komponente PowerShell (pretpregled)

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)



PowerShell API-ji za **poslove Elastic baze podataka** (pretpregled), omogućuju definiranje grupa baza podataka prema kojima će se izvoditi skripte. U ovom se članku prikazuje kako stvoriti i upravljanje **zadacima Elastic baze podataka** pomoću cmdleta ljuske PowerShell. Potražite u članku [Pregled Elastic zadatke](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Preduvjeti
* Azure pretplate. Besplatna probna verzija, potražite u članku [besplatnu probnu verziju jedan mjesec](https://azure.microsoft.com/pricing/free-trial/).
* Postavljanje baze podataka stvorene pomoću alata za Elastic baze podataka. Potražite u članku [Početak rada s alatima Elastic baze podataka](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Detaljne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).
* **Poslovi elastic baze podataka** Paket PowerShell: potražite u članku [Instaliranje Elastic baze podataka zadataka](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Odaberite pretplatu Azure

Da biste odabrali pretplate potrebna vam je pretplata Id (**-SubscriptionId**) ili pretplate naziv (**-SubscriptionName**). Ako imate više pretplata možete pokrenite cmdlet **Get-AzureRmSubscription** i kopirajte željene pretplate podatke iz skupa rezultata. Nakon što dodate informacija o pretplati, pokrenite sljedeći cmdlet da biste postavili ovu pretplatu kao zadani, odnosno cilj za stvaranje i upravljanje zadacima:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[Očisti filtar](https://technet.microsoft.com/library/dd315244.aspx) preporučuje korištenje razviti i izvršenje skripte komponente PowerShell protiv poslove Elastic baze podataka.

## <a name="elastic-database-jobs-objects"></a>Elastic objekata baze podataka zadataka

U sljedećoj su tablici navedeni out sve objekt vrste **Elastic baze podataka** zadataka uz njezin opis i relevantne PowerShell API.

<table style="width:100%">
  <tr>
    <th>Vrsta objekta</th>
    <th>Opis</th>
    <th>Srodni PowerShell API-ji</th>
  </tr>
  <tr>
    <td>Vjerodajnica</td>
    <td>Korisničko ime i lozinku za korištenje prilikom povezivanja s bazama podataka za izvršavanje skripte ili aplikacije DACPACs. <p>Lozinka je šifrirana prije slanja i spremanje u bazi podataka Elastic zadataka za bazu podataka.  Lozinka dešifrira Elastic zadatke baze podataka usluga putem vjerodajnica stvoriti i prenijeti iz skripta za instalaciju.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Novi AzureSqlJobCredential</p><p>Postavljanje AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Skripta</td>
    <td>SQL transakcija skriptu koja će se koristiti za izvršavanje preko baze podataka.  Potrebno je skripta autor biti idempotent jer servis vratit će izvođenja skripte nakon pogreške.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Novi AzureSqlJobContent</p>
    <p>Postavljanje AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Aplikacija podataka sloju </a> paketa primijeniti preko baze podataka.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Novi AzureSqlJobContent</p>
    <p>Postavljanje AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Ciljne baze podataka</td>
    <td>Baze podataka i naziv poslužitelja koja pokazuje na baze podataka SQL Azure.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Novi AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Cilj kartu shard</td>
    <td>Kombinacija cilj baze podataka i vjerodajnica koja će se koristiti za određivanje informacija pohranjenih unutar programa karte shard Elastic baze podataka.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Novi AzureSqlJobTarget</p>
    <p>Postavljanje AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Prilagođene zbirke cilj</td>
    <td>Grupa definirani baza podataka da biste zajednički koristili za izvršavanje.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Novi AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Prilagođene zbirke podređeni cilj</td>
    <td>Cilj baze podataka koja je navedena iz prilagođene zbirke.</td>
    <td>
    <p>Dodavanje AzureSqlJobChildTarget</p>
    <p>Uklanjanje AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Zadatak</td>
    <td>
    <p>Definiranje parametara za posao koje je moguće koristiti za pokretanje izvođenja ili ispunjavanje raspored.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Novi AzureSqlJob</p>
    <p>Postavljanje AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Izvršavanje posla</td>
    <td>
    <p>Spremnik zadataka koje su potrebne za ispunjavanje izvršavanja skriptu ili primjene na DACPAC ciljne pomoću vjerodajnica za veze baze podataka s neuspjeha riješiti općeprihvaćenim u pravilnik izvođenja.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Početak AzureSqlJobExecution</p>
    <p>Zaustavi AzureSqlJobExecution</p>
    <p>Čekanje AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Izvršavanje zadataka posla</td>
    <td>
    <p>Jedna jedinica rada za ispunjavanje posao.</p>
    <p>Ako zadatak uspješno neće moći izvršiti, zapisat će se rezultat poruka iznimke i novi odgovarajući zadatak bit će stvoren i izvršava u općeprihvaćenim navedeni izvođenja pravila.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Početak AzureSqlJobExecution</p>
    <p>Zaustavi AzureSqlJobExecution</p>
    <p>Čekanje AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Pravila za izvršavanje posla</td>
    <td>
    <p>Kontrola zadatka izvođenja vremensko ograničenje, pokušajte ponovno ograničenja i intervala između ponovne pokušaje.</p>
    <p>Elastic zadatke baze podataka sadrži zadani izvođenja pravilnik posla koji uzrokuju zapravo beskonačno ponovne pokušaje o neuspjelim zadatka s eksponencijalnom backoff intervala između svakog pokušajte ponovno.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Novi AzureSqlJobExecutionPolicy</p>
    <p>Postavljanje AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Raspored</td>
    <td>
    <p>Vrijeme temelji specifikacija za izvršavanje odvijati na reoccurring intervala ili po jedan.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Novi AzureSqlJobSchedule</p>
    <p>Postavljanje AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Zadatak okidača</td>
    <td>
    <p>Mapiranje između posla i raspored za izvršavanje posla okidača prema rasporedu.</p>
    </td>
    <td>
    <p>Novi AzureSqlJobTrigger</p>
    <p>Uklanjanje AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Podržani Elastic baze podataka zadataka grupiranje vrste
Posao izvršava skripte Transact-SQL (T-SQL) ili aplikacije DACPACs u grupi baze podataka. Zadatak je poslan na izvršiti u grupi baze podataka, posao "proširuje" u u podređenih zadataka gdje svaki izvodi tražene izvođenja jedan bazi podataka u grupi. 
 
Postoje dvije vrste grupa koje možete stvoriti: 

* Grupa [Shard karte](sql-database-elastic-scale-shard-map-management.md) : kada posao šalje se ciljne kartu shard, posao upiti shard karte da biste odredili trenutni skup shards, a zatim stvoriti podređene zadatke za svaku shard na karti shard.
* Prilagođene grupe zbirke: prilagođeni definirani skup baze podataka. Kad posao pronalaze prilagođene zbirke, on stvara podređeni zadatke za svaku bazu podataka trenutno u prilagođene zbirke.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Da biste postavili Elastic baze podataka poslovi veze

Veza mora biti postavljena na zadacima *bazu podataka kontrole* prije korištenja poslova API-ji. Pokretanje ovaj cmdlet pokrene vjerodajnica prozora iskakati zahtijeva korisničko ime i lozinku stvorenu prilikom instaliranja poslovi Elastic baze podataka. Svim primjerima za navedene u ovoj se temi pretpostavlja da ova prvi korak već izvršena.

Otvaranje veze sa zadacima Elastic baze podataka:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Šifrirana vjerodajnice unutar poslove Elastic baze podataka

Vjerodajnice za bazu podataka možete umetnuti u zadacima *bazu podataka kontrole* s šifrirane lozinku. Nije potrebno vjerodajnice da biste omogućili poslove izvršavanje kasnije, (pomoću rasporedima zadataka) za pohranu.
 
Šifriranje radi kroz certifikat koji je stvoren kao dio skripta za instalaciju. Skripta za instalaciju stvara i prenese certifikat u servisa Azure oblak dešifriranje pohranjene šifriranu lozinku. Servis u Oblaku Azure kasnije pohranjuje javni ključ unutar poslove *bazu podataka kontrole* koja omogućuje sučelja PowerShell API ili Azure klasični Portal za šifriranje navedeni lozinku bez certifikata lokalno instalirati.
 
Vjerodajnica jesu šifrirane i sigurne od korisnika s pristupom samo za čitanje na Elastic objekte baze podataka zadataka. Dok je moguće zlonamjernih korisnici s pristupom čitanja i pisanja objekata Elastic zadatke baze podataka da biste izdvojili lozinku. Vjerodajnice su osmišljeni tako da biste ponovno iskoristiti preko izvršavanja posao. Vjerodajnice prosljeđuju se na ciljnom baze podataka prilikom uspostave veze. Trenutno nema ograničenja na ciljnom baze podataka sustava koristi za svaki vjerodajnica, zlonamjerni korisnik može dodati ciljni baze podataka za bazu podataka u odjeljku kontrolu zlonamjeran korisnik. Korisnik se može pokrenuti naknadno posao ciljanja Ova baza podataka da bi se dobio lozinku na vjerodajnica.

Sigurnost najbolje prakse za zadatke Elastic baze podataka obuhvaćaju sljedeće:

* Ograničite korištenje API-ji pouzdanih pojedincima.
* Vjerodajnice mora imati najmanje ovlasti potrebne za izvođenje zadatka posla.  Dodatne informacije mogu vidjeti u sustavu SQL Server MSDN članka [autorizacija i dozvole](https://msdn.microsoft.com/library/bb669084.aspx) .

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Da biste stvorili šifrirane vjerodajnica za izvršavanje posla preko baze podataka

Da biste stvorili novu vjerodajnice šifrirane, [**cmdlet Get-vjerodajnica**](https://technet.microsoft.com/library/hh849815.aspx) traži korisničko ime i lozinku koju možete proslijediti [**cmdleta New-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Da biste ažurirali vjerodajnice

Prilikom promjene lozinke, koristite [**cmdlet skup AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346062.aspx) i postavite parametar **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Da biste definirali cilj za karte shard programa Elastic baze podataka

Izvršavanje posla sve bazama podataka u skupu shard (stvorena pomoću [Biblioteka klijentski Elastic baze podataka](sql-database-elastic-database-client-library.md)), koristite kartu shard kao cilj baze podataka. U ovom se primjeru potrebna je sharded aplikacija stvoren pomoću klijentska biblioteka Elastic baze podataka. Potražite u članku [Uvod u rad s uzorak Alati Elastic baze podataka](sql-database-elastic-scale-get-started.md).

Baza podataka shard karte upravitelj mora biti postavljeno kao cilj baze podataka, a zatim karti određene shard potrebno je navesti kao odredište.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Stvaranje T SQL skripte za izvršavanje preko baze podataka

Prilikom stvaranja T SQL skripte za izvršavanje je vrlo preporučene izgradnje da budu [idempotent](https://en.wikipedia.org/wiki/Idempotence) i prebacuju protiv pogreške. Elastic baze podataka zadataka vratit će izvršavanje skriptu prilikom izvršavanja naiđe na pogreške, bez obzira na to klasifikacija pogreške.

Pomoću [**cmdleta New-AzureSqlJobContent**](https://msdn.microsoft.com/library/mt346085.aspx) za stvaranje i spremanje skripte za izvršavanje te postavljanje parametara **- ContentName** i **- CommandText** .

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Stvaranje nove skripte iz datoteke
Ako je T SQL skripta definiran u datoteci, ta postavka omogućuje uvoz skripte:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Ažuriranje T-SQL skripta za izvršavanje baze podataka  

U ovom skriptu PowerShell ažurira tekst T SQL naredbe za postojeću.

Postavite sljedeće varijable u skladu s vizualnim definiciju željeno skripte želite postaviti:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Da biste ažurirali definiciju postojećeg skriptu

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Da biste stvorili zadatak za izvršavanje skriptu preko shard karte

Ovu skriptu PowerShell pokreće zadatak za izvršavanje skriptu preko svake shard na karti shard Elastic mjerilo.

Postavite sljedeće varijable željeni skripte i cilj:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Izvršavanje posla 

U ovom skriptu PowerShell izvršava postojeći posao:

Ažurirajte sljedeće varijabla u skladu s vizualnim naziv željene posla koji će se izvršiti:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Dohvaćanje stanja jedan zadatak izvođenja

Pomoću [**cmdleta Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) i postavite parametar **JobExecutionId** da biste pogledali stanje izvođenja posla.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Koristite isti cmdlet **Get-AzureSqlJobExecution** s parametrom **IncludeChildren** da biste prikazali stanje podređeni izvršavanja posla, odnosno određene stanja za svaki zadatak izvođenja svaki bazi podataka ciljani posla.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Da biste pogledali stanje preko više izvršavanja posla

[**Cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) ima više neobavezni parametar koji se mogu koristiti za prikaz više izvršavanja posla, filtrirano kroz navedeni parametre. Sljedeće prikazuje dio mogući načini korištenja Get-AzureSqlJobExecution:

Dohvati sva izvršavanja aktivan posao gornje razine:

    Get-AzureSqlJobExecution

Dohvati sva izvršavanja gornje razine posla, uključujući izvršavanja Neaktivni posao:

    Get-AzureSqlJobExecution -IncludeInactive

Dohvati sva izvršavanja posao podređeni ID navedeni posao izvođenja, uključujući izvršavanja Neaktivni posao:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Dohvaćanje sva posao izvršavanja stvoren pomoću raspored / posla kombinacije, uključujući Neaktivni zadaci:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Dohvati sve zadatke ciljanja kartu navedeni shard, uključujući neaktivne zadacima:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Dohvaćanje svih poslova ciljanja navedeni prilagođene zbirka, uključujući Neaktivni zadaci:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Dohvaćanje popisa izvršavanja zadataka posla unutar određeni posao izvođenja:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Dohvaćanje detalja za izvršavanje zadataka posla:

Da biste vidjeli detalje izvršavanje zadataka zadatak koji je osobito praktična za ispravljanje pogrešaka Neuspjelo izvršavanje može se koristiti sljedeću skriptu komponente PowerShell.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Dohvaćanje pogreške unutar izvršavanja zadataka posla

**Objekt JobTaskExecution** obuhvaća svojstva za životni ciklus zadatka uz svojstvu poruke. Ako je zadatak zadatka nije uspjelo izvršavanje na, svojstvo životni ciklus će biti postavljeno na *nije uspjelo* i svojstvo poruke postavit će se rezultat poruka iznimke i snopa. Ako posao nije uspjela, važno je da biste vidjeli detalje zadataka posla koji nije uspjela zadani posla.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Ćete morati pričekati izvođenja zadatka da biste dovršili

Sljedeću skriptu komponente PowerShell mogu koristiti za Čekanje da biste dovršili zadatak:

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
* Maksimalan Interval pokušaj: 30 minuta
* Ponovite koeficijent Interval: 2
* Maksimalna pokušaja: 2.147.483.647

Stvaranje pravila za željenu izvođenja:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Elastic baze podataka zadataka podržava zahtjeva za otkazivanje zadataka.  Ako Elastic baze podataka zadataka otkrije otkazivanja zahtjev za zadatak trenutno izvršava, će pokušati zaustaviti posao.

Postoje dva načina Elastic zadatke baze podataka možete izvršiti otkazati:

1. Otkazivanje trenutno izvršavanja zadataka: Ako otkazati je otkriven pokrenutom zadatak trenutno, otkazati će se izvršiti unutar trenutno izvršni aspekt zadatak.  Na primjer: Ako dugo izvodi upit trenutno se izvodi kad je pokušaj otkazivanja, će biti pokušaj otkazivanje upita.
2. Otkazivanje ponovne pokušaje zadatak: Ako niti kontrola otkazati otkrio prije pokretanja zadatak za izvršavanje, kontrola niti će izbjeći pokretanje zadatka i deklarirati zahtjev, kao što je otkazan.

Ako je zadatak otkazivanja zatražili za posao nadređene, zahtjev za otkazivanje će biti na snazi za posao nadređene i svih njezinih podređenih zadataka.
 
Da biste poslali zahtjev za otkazivanje, koristite [**cmdlet Zaustavi AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) i postavite parametar **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Da biste izbrisali zadatak i povijest asinkrono zadatka

Elastic poslova baze podataka podržava asinkronog brisanja poslova. Zadatak možete označiti za brisanje i sustav će izbrisati posla i njegov dosadašnje iskustvo nakon sva izvršavanja zadatka dovršite za posao. Sustav će otkazati automatski izvršavanja aktivan posao.  

Pozivanje [**Zaustavljanja AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) da biste poništili izvršavanja aktivan posao.

Da biste pokrenuli brisanja zadatka, koristite [**cmdlet Ukloni AzureSqlJob**](https://msdn.microsoft.com/library/mt346083.aspx) i postavite parametar **JobName** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Da biste stvorili ciljnu prilagođene baze podataka

Možete definirati prilagođene baze podataka ciljnih web-mjesta za Izravni izvođenja ili za uključivanje u skupini prilagođene baze podataka. Ako, na primjer, jer **grupe Elastic baze podataka** nisu još izravno podržane pomoću komponente PowerShell API-ji, možete stvoriti ciljnu prilagođene baze podataka i ciljne zbirke prilagođene baze podataka što obuhvaća sve baze podataka u.

Postavite sljedeće varijable u skladu s vizualnim na željene informacije o bazi podataka:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Da biste stvorili ciljnu zbirke prilagođene baze podataka

Pomoću cmdleta [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) da biste definirali cilj zbirke prilagođene baze podataka da biste omogućili izvođenja preko više definiranih baze podataka ciljnih web-mjesta. Nakon stvaranja grupe baze podataka, baze podataka mogu se pridružiti prilagođene zbirke cilj.

Postavite sljedeće varijable u skladu s vizualnim konfiguriranje ciljne željene prilagođene zbirke:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Da biste dodali baze podataka u ciljni zbirke prilagođene baze podataka

Da biste dodali bazu podataka za određenu zbirku prilagođene pomoću cmdleta [**Dodaj AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) .

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Pregledajte baze podataka unutar zbirke cilj prilagođene baze podataka

Pomoću cmdleta [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) za dohvaćanje podređeni baze podataka unutar zbirke cilj prilagođene baze podataka. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Stvaranje zadatka za izvršavanje skriptu preko cilj zbirke prilagođene baze podataka

Pomoću cmdleta [**New-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) da biste stvorili zadatak prema grupi baza podataka koje je definirao cilj zbirke prilagođene baze podataka. Elastic baze podataka zadataka će proširite posao u više podređenih zadataka svaki odgovara s bazom podataka povezan s zbirke ciljne prilagođene baze podataka i provjerite je li skriptu izvršeno na svakom bazi podataka. Ponovno je važno skripte su idempotent se prebacuju na ponovne pokušaje.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Prikupljanje podataka preko baze podataka

Zadatak možete koristiti za izvođenje upita u grupi baze podataka i poslali rezultate s određenom tablicom. Tablice se možete mu nakon toga da biste vidjeli rezultate upita iz svakog baze podataka. To omogućuje asinkronog način za izvršavanje upita preko mnoge baze podataka. Nije uspjelo pokušaja upravlja automatski putem ponovne pokušaje.

Navedeni odredišne tablice će se automatski stvarati ako se još ne postoji. Nova tablica odgovara sheme skup vraćenih rezultata. Ako skriptu vraća više skupova rezultata, zadacima Elastic baze podataka samo pošaljite prvi odredišnu tablicu.

Sljedeću skriptu komponente PowerShell izvršava skripte i prikuplja rezultatima u navedenoj tablici. Ova skripta pretpostavlja da T SQL skripte stvorena koji proizvodi skupu jedan rezultat i cilj zbirke prilagođene baze podataka je stvorena.

Ova skripta koristi cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) . Postavljanje parametara za skripte, vjerodajnice i izvođenja cilj:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Stvaranje i pokretanje zadatka scenarije zbirke podataka

Ova skripta koristi cmdlet [**Start AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) .
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Da biste zakazali okidača za izvršavanje posla

Da biste stvorili raspored koji se ponavlja se poslužite sljedeću skriptu komponente PowerShell. Ova skripta koristi interval minuta, ali [**Novo AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) podržava i - DayInterval, - HourInterval, - MonthInterval, i - WeekInterval parametara. Mogu se kreirati rasporede koji izvesti samo jedanput po prosljeđivanje - OneTime.

Stvaranje novog rasporeda:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Da biste pokrenuli posao izvršeno zakazano vrijeme

Okidač posao može se definirati da bi se zadatak izvršavati prema zakazano vrijeme. Da biste stvorili zadatak okidača se poslužite sljedeću skriptu komponente PowerShell.

Koristite [Novo AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) i postaviti sljedeće varijable odgovaraju željeni posla i rasporedom:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Da biste uklonili zakazani pridruživanje da biste zaustavili posao izvršavaju na raspored

Da biste prekinuli ponovno pojavljivanje izvođenja posla pomoću okidača posla, moguće je ukloniti okidača posao. Uklonite okidač zadatak da biste zaustavili posao iz koja se izvršava u skladu s rasporedom pomoću [**cmdleta Ukloni AzureSqlJobTrigger**](https://msdn.microsoft.com/library/mt346070.aspx).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Dohvaćanje posao okidača vezana uz zakazano vrijeme

Da biste dobili i prikaz okidača posao registriran u određeno vrijeme raspored mogu se sljedeću skriptu komponente PowerShell.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Dohvaćanje posao okidača vezana uz posao 

Koristite [Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) da biste dobili i prikazati raspored koji sadrži registrirani posao.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Da biste stvorili sloja podataka aplikacije (DACPAC) za izvršavanje preko baze podataka

Da biste stvorili na DACPAC, potražite u članku [sloja podataka aplikacije](https://msdn.microsoft.com/library/ee210546.aspx). Da biste implementirali na DACPAC, pomoću [cmdleta New-AzureSqlJobContent](https://msdn.microsoft.com/library/mt346085.aspx). Na DACPAC mora biti dostupna za servis. Preporučuje se da biste prenijeli stvoren DACPAC Azure za pohranu i stvaranje [Zajednički pristup potpis](../storage/storage-dotnet-shared-access-signature-part-1.md) za na DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Ažuriranje podataka sloja aplikacije (DACPAC) za izvršavanje baze podataka

Postojeće DACPACs registrirana unutar Elastic zadatke baze podataka može se ažurirati na novi ji. Pomoću [**cmdleta skup AzureSqlJobContentDefinition**](https://msdn.microsoft.com/library/mt346074.aspx) da biste ažurirali URI DACPAC na postojeće registrirani DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Da biste stvorili zadatak da biste primijenili sloja podataka aplikacije (DACPAC) preko baze podataka

Kad je DACPAC unutar Elastic zadatke baze podataka, znači da je posao moguće da biste primijenili na DACPAC u grupi baze podataka. Da biste stvorili zadatak DACPAC preko prilagođeni skup baze podataka mogu se sljedeću skriptu komponente PowerShell:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
