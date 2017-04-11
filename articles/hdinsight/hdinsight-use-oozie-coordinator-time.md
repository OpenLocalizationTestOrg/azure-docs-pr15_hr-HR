<properties
    pageTitle="Korištenje temeljene koordinator Hadoop Oozie u HDInsight | Microsoft Azure"
    description="Korištenje temeljene koordinator Hadoop Oozie u HDInsight, servis za velikih skupova podataka. Saznajte kako se definirati Oozie tijekovi rada i coordinators, a zatim objavite zadatke."
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


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Pomoću temeljene koordinator Oozie Hadoop u HDInsight definiranje tijekova rada i koordiniranje poslova

U ovom se članku ćete saznati kako definirati tijekovi rada i coordinators te kako pokrenuti poslove koordinator koji se temelji na vremenu. Dobro je svladati prođite kroz [Korištenje Oozie s HDInsight] [ hdinsight-use-oozie] prije nego što je ovaj članak. Osim Oozie, također mogu zakazivati zadatke koji se pomoću tvorničke Azure podataka. Da biste saznali tvorničke Azure podataka, potražite u članku [Korištenje Svinja i grozd s tvorničke podataka](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] U ovom se članku potreban je utemeljen na sustavu Windows HDInsight klaster. Informacije o korištenju Oozie, uključujući temeljene zadatke, na klasteru sa sustavom Linux potražite u članku [Korištenje Oozie s Hadoop definiranje i pokrenuti tijek rada na sustavom Linux HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Što je Oozie

Apache Oozie je tijek rada/koordinaciji sustava koja upravlja Hadoop zadatke. Je integriran s Hadoop stog i podržava Hadoop poslove Apache MapReduce, Apache Svinja, vrste Hive Apache i Apache Sqoop. Može se koristiti i zakazivanje zadataka koji su specifične za sustav, kao što su Java programe ili skripti ljuske.

Sljedeća slika prikazuje tijek rada će implementirati:

![Dijagram tijeka rada][img-workflow-diagram]

Tijek rada sadrži dvije akcije:

1. Akcija za grozd pokreće HiveQL skripte za Brojanje učestalosti svaku vrstu zapisnika razina u datoteci zapisnika log4j. Svaki log4j zapisnika sastoji se od retka polja koja sadrži polje [ZAPISNIKA RAZINA] da bi se prikazala vrstu i težinu, na primjer:

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

2.  Sqoop akcija izvozi izlaz HiveQL akcija u tablicu u bazi podataka Azure SQL. Dodatne informacije o Sqoop potražite u članku [Korištenje Sqoop s HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Podržani verzije Oozie na HDInsight klastere, potražite u članku [što je novo u verzijama klaster nudi HDInsight?] [hdinsight-versions].


##<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Klaster programa HDInsight**. Informacije o stvaranju programa klaster servisa HDInsight potražite u članku [Stvaranje HDInsight klastere][hdinsight-provision], ili [Početak rada s HDInsight][hdinsight-get-started]. Trebate prođite kroz vodič sljedeće podatke:

    <table border = "1">
    <tr><th>Svojstvo klaster</th><th>Naziv varijable komponente Windows PowerShell</th><th>Vrijednost</th><th>Opis</th></tr>
    <tr><td>Naziv HDInsight klaster</td><td>$clusterName</td><td></td><td>HDInsight klaster na kojem će pokrenuti ovog praktičnog vodiča.</td></tr>
    <tr><td>Korisničko ime HDInsight klaster</td><td>$clusterUsername</td><td></td><td>HDInsight klaster korisničko ime. </td></tr>
    <tr><td>HDInsight klaster korisničke lozinke </td><td>$clusterPassword</td><td></td><td>HDInsight klaster korisničku lozinku.</td></tr>
    <tr><td>Naziv računa za Azure</td><td>$storageAccountName</td><td></td><td>Prostor za pohranu Azure račun dostupan klaster HDInsight. Za ovaj vodič pomoću zadanog računa za pohranu koji ste naveli tijekom postupka dodjele klaster.</td></tr>
    <tr><td>Naziv spremnika Azure blobova platforme</td><td>$containerName</td><td></td><td>U ovom primjeru pomoću spremnik spremište blobova platforme Azure koji se koristi za zadani HDInsight klaster datotečni sustav. Prema zadanim postavkama ima isti naziv kao klaster HDInsight.</td></tr>
    </table>

- **Baza podataka programa SQL Azure**. Morate konfigurirati pravila vatrozida za baze podataka SQL server da biste omogućili pristup iz vaše radne stanice. Za upute o stvaranju Azure SQL baze podataka i konfiguriranje vatrozida, pročitajte članak [Početak rada s bazom podataka Azure SQL][sqldatabase-get-started]. Ovaj članak sadrži skripte komponente Windows PowerShell za stvaranje tablica baze podataka Azure SQL koje su vam potrebne za ovog praktičnog vodiča.

    <table border = "1">
    <tr><th>Svojstva baze podataka za SQL</th><th>Naziv varijable komponente Windows PowerShell</th><th>Vrijednost</th><th>Opis</th></tr>
    <tr><td>Naziv poslužitelja baze podataka za SQL</td><td>$sqlDatabaseServer</td><td></td><td>Baze podataka SQL server u koju će Sqoop izvoz podataka. </td></tr>
    <tr><td>Ime za prijavu na baze podataka SQL</td><td>$sqlDatabaseLogin</td><td></td><td>Ime za prijavu SQL baze podataka.</td></tr>
    <tr><td>Lozinka za prijavu SQL baze podataka</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Lozinka za prijavu SQL baze podataka.</td></tr>
    <tr><td>Naziv baze podataka SQL</td><td>$sqlDatabaseName</td><td></td><td>Azure SQL baza podataka u koju će Sqoop izvoz podataka. </td></tr>
    </table>

    > [AZURE.NOTE] Prema zadanim postavkama baze podataka Azure SQL omogućuje veze servisa Azure, kao što su Azure HDInsight. Ako je ta postavka vatrozid onemogućen, morate je omogućiti na portalu Azure. Upute o stvaranju SQL baze podataka i konfigurirati pravila vatrozida potražite u članku [Stvaranje i konfiguriranje baze podataka SQL][sqldatabase-get-started].


> [AZURE.NOTE] Popunjavanje vrijednosti u tablicama. Će biti koristan prolaze kroz ovog praktičnog vodiča.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definiranje Oozie tijek rada i povezane HiveQL skripte

Definicija Oozie tijekovi rada zapisuju u hPDL (u XML postupak definition language). Zadani naziv datoteke tijek rada je *workflow.xml*.  Će tijeka rada datoteku spremiti lokalno i implementirajte ga klaster HDInsight pomoću Azure PowerShell u nastavku ovog praktičnog vodiča.

Grozd akcije u tijeku rada poziva HiveQL skriptna datoteka. Datoteka skripte sadrži tri izvješća HiveQL:

1. **U TABLICI ISPUSTITE izjava** briše tablicu vrste Hive log4j ako postoji.
2. **U Naredba CREATE TABLE** stvara log4j grozd vanjske tablice, koja pokazuje na mjesto datoteke zapisnika log4j;
3.  **Mjesta datoteke zapisnika log4j**. Graničnik polja nije ",". Graničnik redak zadani je "\n". Vanjska tablica grozd se koristi da bi se izbjeglo uklanja s izvornog mjesta podatkovne datoteke u slučaju da želite pokrenuti tijek rada Oozie više puta.
3. **Izjavu PREBRISIVANJE je okvir Umetanje** broji pojavljivanja svaku vrstu zapisnika razina u tablici log4j grozd, a sprema Izlaz na neko mjesto spremišta blobova platforme Azure.

**Napomena**: postoji poznat problem grozd put. Će naiđete na problem prilikom slanja programa Oozie posao. Upute za rješavanje problema možete pronaći na TechNet Wiki: [HDInsight grozd pogreška: nije moguće preimenovati][technetwiki-hive-error].

**Da biste definirali datoteka skripte HiveQL obraćanje tijeka rada**

1. Stvaranje tekstne datoteke s sljedećeg sadržaja:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Postoje tri varijable koje se koriste u skripti:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Datoteka za definiciju tijeka rada (workflow.xml ovog praktičnog vodiča) proći kroz ove vrijednosti za ovu skriptu HiveQL vrijeme izvođenja.

2. Spremite datoteku u obliku **C:\Tutorials\UseOozie\useooziewf.hql** pomoću kodiranje ANSI (ASCII). (Blok za pisanje koristite ako uređivač teksta ne nudi tu mogućnost) Datoteka skripte će biti implementirano u klaster HDInsight kasnije u ovom praktičnom vodiču.



**Da biste definirali tijeka rada**

1. Stvaranje tekstne datoteke s sljedećeg sadržaja:

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

    Postoje dvije akcije definirane u tijeku rada. Akcija za početak je *RunHiveScript*. Ako akciju izvodi *u redu*, *RunSqoopExport*je sljedeća akcija.

    Na RunHiveScript ima više varijabli. Prilikom slanja posao Oozie iz vaše radne stanice Azure PowerShell proći vrijednosti.

    <table border = "1">
    <tr><th>Varijable tijeka rada</th><th>Opis</th></tr>
    <tr><td>${jobTracker}</td><td>Navedite URL Evidencija posao Hadoop. Korištenje <strong>jobtrackerhost:9010</strong> na HDInsight klaster verzije 3.0 i 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Navedite URL čvor naziva Hadoop. Koristite na zadani datoteka sustava wasbs: / / adresu, na primjer, <i>wasbs: / /&lt;naziv spremnika&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${queueName}</td><td>Određuje reda čekanja naziv koji će se poslati posao. Koristite <strong>zadani</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Varijabla grozd akcija</th><th>Opis</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Izvorišni direktorij za stvaranje uvesti tablicu vrste Hive naredbu.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Izlazna datoteka za naredbu za umetanje PREBRISATI.</td></tr>
    <tr><td>${hiveTableName}</td><td>Naziv tablicu vrste Hive koji referencira log4j podatkovne datoteke.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Varijabla Sqoop akcija</th><th>Opis</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Niz za povezivanje s bazom podataka SQL.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Tablica baze podataka Azure SQL u kojem će biti izvezene podatke.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Izlazna datoteka izjava vrste Hive Umetanje PREBRISATI. To je u istu mapu za izvoz Sqoop (Izvoz dir).</td></tr>
    </table>

    Dodatne informacije o Oozie tijek rada i korištenje akcija tijeka rada potražite u [dokumentaciji Apache Oozie 4.0] [ apache-oozie-400] (za HDInsight klaster verzije 3.0) ili [dokumentaciju Apache Oozie 3.3.2] [ apache-oozie-332] (za HDInsight klaster verzija 2.1).

2. Spremite datoteku u obliku **C:\Tutorials\UseOozie\workflow.xml** pomoću kodiranje ANSI (ASCII). (Blok za pisanje koristite ako uređivač teksta ne nudi tu mogućnost)

**Da biste definirali koordinator**

1. Stvaranje tekstne datoteke s sljedećeg sadržaja:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Postoji pet varijabli koje se koriste u datoteku definicije:

  	| Varijabla          | Opis |
  	| ------------------|------------ |
  	| ${coordFrequency} | Vrijeme Pauziraj posao. Učestalost uvijek izražena u minutama. |
  	| ${coordStart}     | Vrijeme početka posla. |
  	| ${coordEnd}       | Vrijeme završetka posla. |
  	| ${coordTimezone}  | Oozie procesa koordinator zadataka fixed vremenske zone s bez zimskog računanja vremena (obično predstavljene pomoću UTC-a). Ovo je vremenska zona nazivaju se "obrada Oozie vremenske." |
  	| ${wfPath}         | Put na workflow.xml.  Ako naziv datoteke tijeka rada nije zadani naziv datoteke (workflow.xml), potrebno je navesti. |

2. Spremite datoteku u obliku **C:\Tutorials\UseOozie\coordinator.xml** pomoću kodiranje ANSI (ASCII). (Blok za pisanje koristite ako uređivač teksta ne nudi tu mogućnost)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Implementacija Oozie projekta i pripremite vodič

Pokretati skripte Azure PowerShell da biste učinili sljedeće:

- Kopiranje skripte HiveQL (useoozie.hql) u spremište blobova platforme Azure wasbs:///tutorials/useoozie/useoozie.hql.
- Kopirajte workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
- Kopirajte coordinator.xml wasbs:///tutorials/useoozie/coordinator.xml.
- Kopiranje podatkovnu datoteku (/ example/data/sample.log) da biste wasbs:///tutorials/useoozie/data/sample.log.
- Stvaranje tablice programa za baze podataka Azure SQL za spremanje Sqoop izvoz podataka. Naziv tablice je *log4jLogCount*.

**Razumijevanje HDInsight prostora za pohranu**

HDInsight koristi spremište blobova platforme Azure za pohranu podataka. wasbs: / / je implementacija tvrtke Microsoft Hadoop distributed datotečnog sustava (HDFS) u spremište blobova platforme Azure. Dodatne informacije potražite u članku [Korištenje Azure blobova s HDInsight][hdinsight-storage].

Prilikom dodjele resursa za HDInsight klaster računa spremišta blobova platforme Azure i određene kontejner s tog računa je označen kao zadanog datotečnog sustava, kao što su u HDFS. Uz ovaj račun za pohranu, možete dodati dodatni prostor za pohranu računa iz iste pretplate na Azure ili različite Azure pretplate tijekom postupka dodjele resursa. Upute o dodavanju računa dodatni prostor za pohranu potražite u člancima [klastere dodjele resursa HDInsight][hdinsight-provision]. Da biste pojednostavnili Azure PowerShell skripta koja se koristi u ovom ćete praktičnom vodiču, sve datoteke spremaju se u kontejner zadanog datotečnog sustava koja se nalazi na */tutorials/useoozie*. Prema zadanim postavkama, ovaj spremnik ima isti naziv kao naziv klaster HDInsight.
Vidjet ćete da sintaksa je:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Samo na *wasb: / /* sintaksa podržava HDInsight klaster verzije 3.0. U starijim *asv: / /* sintaksa je podržano u HDInsight 2.1 i 1,6 klastere, no nije podržan u klastere HDInsight 3.0.

> [AZURE.NOTE] Na wasb: / / put je virtualne. Dodatne informacije potražite u članku [Korištenje Azure blobova s HDInsight][hdinsight-storage].

Datoteku koja je spremljena u kontejner zadanog datotečnog sustava možete pristupiti iz servisa HDInsight pomoću nekog od sljedećih ji (koristim workflow.xml kao primjer):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Ako želite pristupiti datoteci izravno s računa za spremište blobova platforme naziv datoteke je:

    tutorials/useoozie/workflow.xml

**Razumijevanje grozd interne i vanjske tablice**

Što trebate znati o grozd interne i vanjske tablice na nekoliko stvari:

- Naredba CREATE TABLE stvara Interna tablice, poznata i kao upravljanih tablice. Podatkovne datoteke moraju nalaziti u spremniku zadani.
- Naredba CREATE TABLE premješta podatkovnu datoteku /hive/warehouse/<TableName> mapa u spremniku zadani.
- Naredba CREATE TABLE za VANJSKE stvara vanjsku tablicu. Podatkovne datoteke može nalaziti izvan spremnik zadani.
- Naredba CREATE TABLE za VANJSKE premještanje podatkovne datoteke.
- Naredba CREATE TABLE za VANJSKE ne dopušta sve podmape mape koja je navedena u uvjetu mjesto. Ovo je razloga zašto vodič čini kopiju datoteke sample.log.

Dodatne informacije potražite u članku [HDInsight: vrste Hive interne i vanjske tablice Uvod][cindygross-hive-tables].

**Da biste pripremili vodič**

1. Otvorite Windows Očisti filtar (u sustavu Windows 8 početnom zaslonu upišite **PowerShell_ISE**, a zatim **Očisti filtar za Windows**. Dodatne informacije potražite u članku [Pokretanje Windows PowerShell u sustavu Windows 8 i Windows][powershell-start]).
2. U oknu s dna pokrenite sljedeću naredbu za povezivanje s Azure pretplatu:

        Add-AzureAccount

    Će se zatražiti da unesete vjerodajnice račun za Azure. Taj način dodavanja veze pretplata istekne vrijeme, a nakon 12 sati, morat ćete ponovno pokrenite cmdlet.

    > [AZURE.NOTE] Ako imate više pretplata Azure i zadane pretplate nije jednak onome koji želite koristiti, koristite cmdlet <strong>Odaberite AzureSubscription</strong> da biste odabrali pretplatu.

3. Kopirajte sljedeću skriptu u oknu skripte, a zatim postavite prvih šest varijable:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Dodatne opise varijabli potražite u odjeljku [preduvjeti](#prerequisites) ovog praktičnog vodiča.

3. Dodati skripte u oknu skripte na sljedeći način:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Kliknite **Izvedi skriptu** ili pritisnite **F5** da biste pokrenuli skriptu. Rezultat će biti sličan:

    ![Praktični vodič Priprema Izlaz][img-preparation-output]

##<a name="run-the-oozie-project"></a>Pokrenite projekt Oozie

Azure PowerShell trenutno ne nudi sve cmdleta za definiranje Oozie zadatke. Cmdlet **Pozovite RestMethod** možete koristiti za pozivanje Oozie web-servisa. Oozie web services API je na HTTP REST JSON API-JA. Dodatne informacije o web-servisi Oozie API potražite u [dokumentaciji Apache Oozie 4.0] [ apache-oozie-400] (za HDInsight klaster verzije 3.0) ili [dokumentaciju Apache Oozie 3.3.2] [ apache-oozie-332] (za HDInsight klaster verzija 2.1).

**Da biste poslali programa Oozie posla**

1. Otvorite Windows Očisti filtar (u sustavu Windows 8 početnom zaslonu upišite **PowerShell_ISE**, a zatim **Očisti filtar za Windows**. Dodatne informacije potražite u članku [Pokretanje Windows PowerShell u sustavu Windows 8 i Windows][powershell-start]).

3. Kopirajte sljedeću skriptu u oknu skripte, a zatim postavite najprije ČETRNAEST varijable (no preskočiti i **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Dodatne opise varijabli potražite u odjeljku [preduvjeti](#prerequisites) ovog praktičnog vodiča.

    $coordstart i $coordend se tijek rada početno i završno vrijeme. Da biste saznali UTC-GMT vrijeme, potražite "utc vrijeme" na servisu bing.com. U $coordFrequency je koliko često u minutama želite pokrenuti tijek rada.

3. Dodavanje sljedeće skripte. Taj dio definira opseg Oozie:

        #OoziePayload used for Oozie web service submission
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
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
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
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Glavna razlika datoteku tereta slanje tijeka rada u usporedbi je varijable **oozie.coord.application.path**. Kada pošaljete zadatak tijeka rada, možete koristiti **oozie.wf.application.path** umjesto toga.

4. Dodavanje sljedeće skripte. Taj dio provjerava stanje servisa web Oozie:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Dodavanje sljedeće skripte. Taj dio stvara zadatak programa Oozie:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Kada pošaljete zadatak tijeka rada, morate poslati poziva da biste pokrenuli posao nakon stvaranja zadatka drugom web-servisa. U ovom slučaju posao koordinator aktivira vremena. Automatski će se pokrenuti posao.

6. Dodavanje sljedeće skripte. Taj dio provjerava Oozie statusu zadatka:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Neobavezno) Dodavanje sljedeće skripte.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Skripta dodati na sljedeći način:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Ako želite pokrenuti dodatne funkcije, uklonite znak #.

7. Ako je svoj klaster HDinsight verzija 2.1, zamijenite "https://$clusterName.azurehdinsight.net:443/oozie/v2/" s "https://$clusterName.azurehdinsight.net:443/oozie/v1/". HDInsight klaster verzija 2.1 ne ne podržava verzija 2 web-servisa.

7. Kliknite **Izvedi skriptu** ili pritisnite **F5** da biste pokrenuli skriptu. Rezultat će biti sličan:

    ![Praktični vodič pokrenuti tijek rada Izlaz][img-runworkflow-output]

8. Povezivanje s bazom podataka SQL da biste vidjeli izvezene podatke.

**Da biste provjerili evidencije pogrešaka za posao**

Da biste otklonili tijeka rada, datoteka zapisnika Oozie možete pronaći na C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log headnode klaster. Informacije o RDP potražite u članku [administriranje HDInsight klastere pomoću portala za Azure][hdinsight-admin-portal].

**Da biste ponovno pokrenite vodič**

Ponovno pokretanje tijeka rada, morate izvršiti sljedeće zadatke:

- Brisanje grozd skripte Izlazna datoteka.
- Brisanje podataka u tablici log4jLogsCount.

Evo ogledne skripte komponente Windows PowerShell koje možete koristiti:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Daljnji koraci
U ovom ćete praktičnom vodiču naučili kako da biste definirali tijeka rada za Oozie i programa Oozie koordinator te kako Pokreni programa Oozie koordinator pomoću Azure PowerShell. Dodatne informacije potražite u sljedećim člancima:

- [Početak rada s HDInsight][hdinsight-get-started]
- [Pomoću spremište blobova platforme Azure HDInsight][hdinsight-storage]
- [Administriranje HDInsight pomoću komponente PowerShell Azure][hdinsight-admin-powershell]
- [Prijenos podataka HDInsight][hdinsight-upload-data]
- [Korištenje Sqoop s HDInsight][hdinsight-use-sqoop]
- [Korištenje grozd s HDInsight][hdinsight-use-hive]
- [Korištenje Svinja s HDInsight][hdinsight-use-pig]
- [Razvoj Java MapReduce programe za HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
