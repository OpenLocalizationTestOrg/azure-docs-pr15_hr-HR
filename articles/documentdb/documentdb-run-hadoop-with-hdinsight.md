<properties
    pageTitle="Pokrenuti posao Hadoop pomoću DocumentDB i HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti jednostavne grozd, Svinja i MapReduce posao s DocumentDB Azure HDInsight."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Pokrenuti posao Hadoop pomoću DocumentDB i HDInsight

Pomoću ovog praktičnog vodiča prikazuje kako pokrenuti [Vrste Hive Apache][apache-hive], [Apache Svinja][apache-pig], i [Apache Hadoop] [ apache-hadoop] MapReduce poslove na Azure HDInsight s DocumentDB, Hadoop poveznik. DocumentDB, Hadoop connector omogućuje DocumentDB će poslužiti kao izvor i primatelj za grozd, Svinja i MapReduce zadatke. Pomoću ovog praktičnog vodiča koristit će DocumentDB kao izvor podataka i odredišni Hadoop zadatke.

Po dovršetku ovog praktičnog vodiča ćete je moći odgovaraju na sljedeća pitanja:

- Kako učitati podatke iz DocumentDB pomoću grozd, Svinja ili MapReduce posla?
- Kako spremiti podatke u DocumentDB pomoću grozd, Svinja ili MapReduce posla?

Preporučujemo da prvi koraci sljedeće videozapisu gdje možemo pokrenuti putem grozd posla pomoću DocumentDB i HDInsight.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Zatim, vratite se u ovom se članku gdje ćete primiti sve detalje na pokretanja analize zadatke na DocumentDB podataka.

> [AZURE.TIP] Pomoću ovog praktičnog vodiča podrazumijeva prethodnog sučelje pomoću Apache Hadoop, grozd i/ili Svinja. Ako ste novi korisnik Apache Hadoop, grozd i Svinja, preporučujemo da potražite u [dokumentaciji Apache Hadoop][apache-hadoop-doc]. Pomoću ovog praktičnog vodiča i pretpostavlja da prethodnog doživljaj DocumentDB i imate li račun DocumentDB. Ako ste novi korisnik DocumentDB ili nemate račun za DocumentDB, provjerite pogledajte naše [Uvod] [ getting-started] stranice.

Ne vrijeme da biste dovršili vodič, a samo želite potpunu ogledne skripte komponente PowerShell pribaviti grozd, Svinja i MapReduce? Nije problem, zatražite ih [ovdje][documentdb-hdinsight-samples]. Preuzimanje i sadrži hql, svinja i java datoteke za te primjere.

## <a name="NewestVersion"></a>Najnovija verzija

<table border='1'>
    <tr><th>Poveznik za Hadoop verzija</th>
        <td>1.2.0</td></tr>
    <tr><th>Uri skripte</th>
        <td>https://portalcontent.blob.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Datum izmjene</th>
        <td>04/26/2016</td></tr>
    <tr><th>Podržani HDInsight verzije</th>
        <td>3.1, 3,2</td></tr>
    <tr><th>Evidencija promjena</th>
        <td>Ažurirani DocumentDB Java SDK za 1.6.0</br>
            Podrška za particioniranom zbirke kao izvor i primatelj</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Preduvjeti
Prije nego što slijedeći upute u ovom ćete praktičnom vodiču, provjerite možete li se sljedeće:

- DocumentDB računa, baze podataka, a zbirke s dokumentima u. Dodatne informacije potražite u članku [Uvod u DocumentDB][getting-started]. Uvoz ogledne podatke u svoj račun DocumentDB [DocumentDB uvoz alat][documentdb-import-data].
- Propusnost. Čitanja i pisanja iz servisa HDInsight će se brojati pri vaš zahtjev za dodijeljenog jedinica za zbirke. Dodatne informacije potražite u članku [Provisioned propusnost, jedinice zahtjev i postupaka baze podataka][documentdb-manage-throughput].
- Kapacitet za dodatne pohranjena procedura svako izlaz zbirke. Pohranjene procedure koriste se za prijenos dobivene dokumenata. Dodatne informacije potražite u članku [zbirke i dodijeljenu propusnost][documentdb-manage-document-storage].
- Kapacitet dobivene dokumenata sa zadacima grozd, Svinja ili MapReduce. Dodatne informacije potražite u članku [Upravljanje DocumentDB kapaciteta i performanse][documentdb-manage-collections].
- [*Neobavezni*] Kapacitet za dodatne zbirke. Dodatne informacije potražite u članku [Provisioned spremišta i indeksa indirektni][documentdb-manage-document-storage].

> [AZURE.WARNING] Da biste izbjegli stvaranja nove zbirke tijekom neke zadatke, možete ispis rezultata da biste stdout, spremite izlaz vaše WASB spremniku, ili odredite već postojeću zbirku. U slučaju navodeći postojeću zbirku nove dokumente stvorit će se unutar zbirke i već postojeće dokumente samo utjecat će ako nema sukoba *ID-ove dokumenata*. **Poveznik automatski Prebriši postojeće dokumente s ID-a sukobe**. Tu značajku možete isključiti tako da postavite mogućnost upsert FALSE. Ako je vrijednost false upsert i do sukoba dolazi, Hadoop posao neće uspjeti; izvješćivanje o pogreškama pogrešku sukoba ID-a.


## <a name="ProvisionHDInsight"></a>Korak 1: Stvaranje novog HDInsight klaster
Pomoću ovog praktičnog vodiča koristi skripte akciju s portala za Azure da biste prilagodili svoj klaster HDInsight. U ovom ćete praktičnom vodiču koristit ćemo Portal za Azure da biste stvorili svoj klaster HDInsight. Upute o tome kako pomoću cmdleta ljuske PowerShell i HDInsight .NET SDK potražite u članku [Prilagodba HDInsight klastere pomoću skripte akcije] [ hdinsight-custom-provision] članka.

1. Prijava na [Portal za Azure][azure-portal].

2. Kliknite **+ Novo** pri vrhu stranice u lijevom navigacijskom oknu traženje **HDInsight** traku pretraživanja na novu plohu.

3. **HDInsight** objavljuje **Microsoft** će se pojaviti pri vrhu rezultata. Kliknite je, a zatim kliknite **Stvori**.

4. Na novu HDInsight klaster stvaranje plohu, unesite svoje **Ime klaster** i odaberite **pretplatu** koju želite Dodjela resursa za ovaj resurs u odjeljku.

    <table border='1'>
        <tr><td>Naziv klaster</td><td>Naziv klaster.<br/>
   DNS naziv mora pokrenuti završavati znak alfa numeričke i mogu sadržavati crtice.<br/>
   Polje mora biti niz između 3 i 63 znakova.</td></tr>
        <tr><td>Naziv pretplate</td>
            <td>Ako imate više pretplata Azure, odaberite pretplatu u koju će hostira svoj klaster HDInsight. </td></tr>
    </table>

5. Kliknite **Odaberite vrstu klaster** i postaviti sljedeća svojstva navedene vrijednosti.

    <table border='1'>
        <tr><td>Vrsta klaster</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Razina klaster</td><td><strong>Standardna</strong></td></tr>
        <tr><td>Operacijski sustav</td><td><strong>Windows</strong></td></tr>
        <tr><td>Verzija</td><td>najnovija verzija</td></tr>
    </table>

    Sada kliknite **ODABERI**.

    ![Detalje Hadoop HDInsight početne klaster][image-customprovision-page1]

6. Kliknite **vjerodajnice** da biste postavili prijava i vjerodajnice za daljinski pristup. Odaberite **korisničko ime za prijavu klaster** i **Lozinka za prijavu klaster**.

    Ako želite da udaljena u svoj klaster, odaberite *da* pri dnu zaslona u plohu i navedite korisničko ime i lozinku.

7. Kliknite na **Izvor podataka** da biste postavili svom primarnom mjestu za pristup podacima. Odaberite **Način odabira** i navedite već postojeći račun za pohranu ili stvorite novi.

8. Na istom plohu odredite **Zadani spremnik** i **mjesto**. Možete i kliknite **ODABERI**.

    > [AZURE.NOTE] Odaberite mjesto blizu vašoj regiji DocumentDB računa za bolje performanse

8. Kliknite **određivanje cijena** da biste odabrali broj i upišite čvorove. Možete zadržati zadanu konfiguraciju i kasnije skalirali broj čvorove tempiranja.

9. Kliknite **neobavezno konfiguracija**, a zatim **Akcije skripte** u plohu neobavezno konfiguracije.

    U skripti Akcije, unesite sljedeće podatke da biste prilagodili svoj klaster HDInsight.

    <table border='1'>
        <tr><th>Svojstvo</th><th>Vrijednost</th></tr>
        <tr><td>Ime</td>
            <td>Unesite naziv za skripte akciju.</td></tr>
        <tr><td>Skripta URI-JA</td>
            <td>Navedite URI za skriptu koja se poziva da biste prilagodili klaster.</br></br>
   Upišite: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Zaglavlje</td>
            <td>Potvrdite okvir da biste pokrenuli skriptu PowerShell premjestite čvor zaglavlje.</br></br>
            <strong>Potvrdite ovaj okvir</strong>.</td></tr>
        <tr><td>Tempiranja</td>
            <td>Potvrdite okvir da biste pokrenuli skriptu PowerShell premjestite čvor tempiranja.</br></br>
            <strong>Potvrdite ovaj okvir</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Potvrdite okvir da biste pokrenuli skriptu PowerShell premjestite na Zookeeper.</br></br>
            <strong>Nije potreban</strong>.
            </td></tr>
        <tr><td>Parametri</td>
            <td>Navedite parametre, ako je potrebno skripta.</br></br>
            <strong>Potrebne bez parametara</strong>.</td></tr>
    </table>

10. Stvaranje ili novu **Grupu resursa** ili korištenje postojeće grupe resursa u odjeljku pretplate Azure.

11. Provjerite **Prikvači na nadzornoj ploči** za praćenje njegova uvođenja i kliknite **Stvori**!

## <a name="InstallCmdlets"></a>Korak 2: Instalirate i konfigurirate Azure PowerShell

1. Instalirajte Azure PowerShell. Moguće je pronaći upute [u nastavku][powershell-install-configure].

    > [AZURE.NOTE] Osim toga, samo za upite grozd, možete koristiti HDInsight na mreži uređivač vrste Hive. Da biste to učinili, prijavite se na [Portal za Azure][azure-portal], kliknite **HDInsight** u lijevom oknu da biste prikazali popis na klastere HDInsight. Kliknite klaster koju želite pokrenuti grozd upita, a zatim **Konzole za upit**.

2. Otvorite Azure PowerShell integrirani za skriptiranje okruženja:
    - Na računalu sa sustavom Windows 8 ili Windows Server 2012 ili noviji, možete koristiti ugrađeno pretraživanje. Na početnom zaslonu upišite **Očisti filtar** , a zatim pritisnite **Enter**.
    - Na računalu sa sustavom verziju stariju od sustava Windows 8 ili Windows Server 2012, pomoću izbornika Start. S izbornika Start upišite **naredbeni redak** u okvir za pretraživanje, a zatim na popisu rezultata kliknite **naredbeni redak**. U naredbeni redak upišite **powershell_ise** i pritisnite **Enter**.

3. Dodajte račun za Azure.
    1. U oknu konzole upišite **Dodaj AzureAccount** , a zatim pritisnite **Enter**.
    2. Unesite adresu e-pošte povezanog s pretplatom na Azure, a zatim kliknite **Nastavi**.
    3. Upišite u okvir Lozinka za pretplatu Azure.
    4. Kliknite **Prijava**.

4. Na sljedećem su dijagramu označava važne dijelove Azure PowerShell skriptiranje okruženja.

    ![Dijagram za Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Korak 3: Pokrenuti posao grozd pomoću DocumentDB i HDInsight

> [AZURE.IMPORTANT] Sve varijable označenu < > mora ispuniti pomoću konfiguracijske postavke.

1. Postavite sljedeće varijable u oknu skriptu PowerShell.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>Počnimo izgradnje niz za upit. Ćete pisanju grozd upit koji vodi generira sustav sve dokumente vremenske oznake (_ts) i jedinstveni ID-a (_rid) iz zbirke DocumentDB, bilježi sve dokumente po minuti, a zatim sprema rezultate natrag u novu zbirku DocumentDB.</p>

    <p>Najprije stvaranje tablicu vrste Hive iz naših DocumentDB zbirke. Dodajte sljedeće koda u skriptu PowerShell okno <strong>nakon</strong> koda iz #1. Provjerite je li uključiti u neobavezno DocumentDB.query parametar t trim naš dokumenata samo _ts i _rid.</p>

    > [AZURE.NOTE]**Imenovanja DocumentDB.inputCollections je napisana.** Da, ne možemo omogućuju dodavanje više zbirki kao ulazni: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Nakon toga stvaranje tablicu vrste Hive zbirke izlaz. Svojstva dokumenta Izlaz bit će mjesec, dan, sat, minuta i ukupan broj ponavljanja.

    > [AZURE.NOTE]**Opet, imenovanja DocumentDB.outputCollections nije pogrešku.** Da, ne možemo dopustili dodavanje više zbirki kao se rezultat: </br>
"*DocumentDB.outputCollections*'='*\<naziv zbirke izlazna DocumentDB 1\>*,*\<naziv zbirke izlazna DocumentDB 2\>*" </br> Nazivi zbirke razdjelnik bez razmaka, koristeći samo jedan zarez. </br></br>
Dokumenti će biti kružnog raspodijeljeno-preko više zbirki. Obradu dokumenata bit će spremljeni u jednu zbirku, a zatim druge skupine dokumenata bit će spremljeni u sljedeću zbirku itd.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Na kraju, recimo bilježi dokumente mjesec, dan, sat i minute i umetnite rezultate natrag u izlaz tablicu vrste Hive.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Dodajte sljedeće skripte isječak da biste stvorili definiciju posla grozd iz prethodne upita.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Možete koristiti i promjenu - datoteku da biste odredili datoteku skripte HiveQL na HDFS.

6. Dodajte sljedeće isječak radi uštede vremena početka i slanje grozd posla.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Dodajte sljedeće ćete morati pričekati dovršetak posla grozd.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Dodajte sljedeće da biste ispisali na standardni izlazni i vremena početka i završetka.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Pokretanje** nove skripte! **Kliknite** gumb zelene izvrši.

10. Provjerite rezultate. Prijavite se na [Portal za Azure][azure-portal].
    1. U oknu s lijeve strane kliknite <strong>Pregledaj</strong> . </br>
    2. Kliknite <strong>sve</strong> u gornjem desnom oknu za pregled. </br>
    3. Pronađite i kliknite <strong>DocumentDB računa</strong>. </br>
    4. Nakon toga pronađite <strong>DocumentDB računa</strong>, a zatim <strong>DocumentDB baze podataka</strong> i <strong>DocumentDB zbirke</strong> pridružene zbirke izlaz naveden u upitu grozd.</br>
    5. Kliknite <strong>Dokument Explorer</strong> ispod <strong>Alate za razvojne inženjere</strong>.</br></p>

    Prikazat će se rezultata upita grozd.

    ![Rezultati upita grozd][image-hive-query-results]

## <a name="RunPig"></a>Korak 4: Pokrenuti posao Svinja pomoću DocumentDB i HDInsight

> [AZURE.IMPORTANT] Sve varijable označenu < > mora ispuniti pomoću konfiguracijske postavke.

1. Postavite sljedeće varijable u oknu skriptu PowerShell.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Počnimo izgradnje niz za upit. Ćete pisanju Svinja upit koji vodi generira sustav sve dokumente vremenske oznake (_ts) i jedinstveni ID-a (_rid) iz zbirke DocumentDB, bilježi sve dokumente po minuti, a zatim sprema rezultate natrag u novu zbirku DocumentDB.</p>
    <p>Prvo učitavanje dokumenata iz DocumentDB u HDInsight. Dodajte sljedeće koda u skriptu PowerShell okno <strong>nakon</strong> koda iz #1. Provjerite je li za dodavanje upita DocumentDB neobavezan parametar upita DocumentDB da biste izdvojili naš dokumenata samo _ts i _rid.</p>

    > [AZURE.NOTE]Da, ne možemo omogućuju dodavanje više zbirki kao ulaz: </br>
"*\<Naziv zbirke DocumentDB unos 1\>*,*\<naziv zbirke unos DocumentDB 2\>*"</br> Nazivi zbirke razdjelnik bez razmaka, koristeći samo jedan zarez. </b>

    Dokumenti će biti kružnog raspodijeljeno-preko više zbirki. Obradu dokumenata bit će spremljeni u jednu zbirku, a zatim druge skupine dokumenata bit će spremljeni u sljedeću zbirku itd.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Zatim ćemo bilježi dokumente prema mjesecu, dan, sat, minuta i ukupan broj ponavljanja.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Na kraju, recimo spremiti rezultate u našem novu zbirku izlaz.

    > [AZURE.NOTE]Da, ne možemo dopustili dodavanje više zbirki kao se rezultat: </br>
"\<Naziv zbirke izlazna DocumentDB 1\>,\<naziv zbirke izlazna DocumentDB 2\>"</br> Nazivi zbirke razdjelnik bez razmaka, koristeći samo jedan zarez.</br>
Dokumenti će biti kružnog raspodijeljeno-preko više zbirki. Obradu dokumenata bit će spremljeni u jednu zbirku, a zatim druge skupine dokumenata bit će spremljeni u sljedeću zbirku itd.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Dodajte sljedeće skripte isječak da biste stvorili definiciju posla Svinja iz prethodne upita.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Možete koristiti i promjenu - datoteku da biste odredili datoteku skripte Svinja na HDFS.

6. Dodajte sljedeće isječak radi uštede vremena početka i slanje Svinja posla.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Dodajte sljedeće ćete morati pričekati dovršetak posla Svinja.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Dodajte sljedeće da biste ispisali na standardni izlazni i vremena početka i završetka.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Pokretanje** nove skripte! **Kliknite** gumb zelene izvrši.

10. Provjerite rezultate. Prijavite se na [Portal za Azure][azure-portal].
    1. U oknu s lijeve strane kliknite <strong>Pregledaj</strong> . </br>
    2. Kliknite <strong>sve</strong> u gornjem desnom oknu za pregled. </br>
    3. Pronađite i kliknite <strong>DocumentDB računa</strong>. </br>
    4. Nakon toga pronađite <strong>DocumentDB računa</strong>, a zatim <strong>DocumentDB baze podataka</strong> i <strong>DocumentDB zbirke</strong> pridružene zbirke izlaz naveden u upitu Svinja.</br>
    5. Kliknite <strong>Dokument Explorer</strong> ispod <strong>Alate za razvojne inženjere</strong>.</br></p>

    Prikazat će se rezultata upita Svinja.

    ![Svinja rezultati upita][image-pig-query-results]

## <a name="RunMapReduce"></a>Korak 5: Pokrenuti posao MapReduce pomoću DocumentDB i HDInsight

1. Postavite sljedeće varijable u oknu skriptu PowerShell.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. Ne možemo ćete izvršiti MapReduce zadatak koji broji pojavljivanja za svaki svojstva dokumenta iz zbirke DocumentDB. Dodajte ovu skripte isječak **Kada** iznad isječka.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] U sklopu TallyProperties v01.jar Prilagođena instalacija poveznika Hadoop DocumentDB.

3. Dodajte sljedeću naredbu za slanje MapReduce posao.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Osim definiciju posla MapReduce i navedite naziv klaster HDInsight mjesto na koje želite pokrenuti posao MapReduce vjerodajnice. Početak AzureHDInsightJob je asynchronized poziv. Da biste provjerili nakon dovršetka posla, koristite cmdlet *Čekanja AzureHDInsightJob* .

4. Dodajte sljedeću naredbu da biste provjerili probleme s pokretanjem MapReduce posao.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Pokretanje** nove skripte! **Kliknite** gumb zelene izvrši.

6. Provjerite rezultate. Prijavite se na [Portal za Azure][azure-portal].
    1. U oknu s lijeve strane kliknite <strong>Pregledaj</strong> .
    2. Kliknite <strong>sve</strong> u gornjem desnom oknu za pregled.
    3. Pronađite i kliknite <strong>DocumentDB računa</strong>.
    4. Nakon toga pronađite <strong>DocumentDB računa</strong>, a zatim <strong>DocumentDB baze podataka</strong> i <strong>DocumentDB zbirke</strong> pridružene zbirke izlaz naveden u MapReduce posla.
    5. Kliknite <strong>Dokument Explorer</strong> ispod <strong>Alate za razvojne inženjere</strong>.

    Prikazat će se rezultati MapReduce posla.

    ![Rezultati upita MapReduce][image-mapreduce-query-results]

## <a name="NextSteps"></a>Daljnji koraci

Čestitamo! Samo je prvi grozd, Svinja i MapReduce poslova pomoću Azure DocumentDB i HDInsight.

Imamo Otvori izvorni termini naše Hadoop poveznik. Ako želite, možete priložiti na [GitHub][documentdb-github].

Dodatne informacije potražite u sljedećim člancima:

- [Razvoj Java aplikacijom Documentdb][documentdb-java-application]
- [Razvoj Java MapReduce programe za Hadoop u HDInsight][hdinsight-develop-deploy-java-mapreduce]
- [Početak rada s Hadoop s grozd u HDInsight radi analize korištenja mobilne slušalica][hdinsight-get-started]
- [Korištenje MapReduce s HDInsight][hdinsight-use-mapreduce]
- [Korištenje grozd s HDInsight][hdinsight-use-hive]
- [Korištenje Svinja s HDInsight][hdinsight-use-pig]
- [Prilagodba klastere HDInsight pomoću skripte akcije][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
