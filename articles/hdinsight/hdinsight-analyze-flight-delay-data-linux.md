<properties 
    pageTitle="Analiza podataka odgode leta pomoću grozd na sustavom Linux HDInsight | Microsoft Azure" 
    description="Saznajte kako koristiti grozd za analizu podataka leta na sustavom Linux HDInsight, a zatim izvezite podatke u SQL baze podataka pomoću Sqoop." 
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

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analizirajte podatke o kašnjenju leta pomoću grozd u HDInsight

Saznajte kako da biste analizirali podatke o kašnjenju leta pomoću grozd sustavom Linux HDInsight, a zatim izvezite podatke u bazi podataka SQL Azure pomoću pomoću Sqoop.

> [AZURE.NOTE] Tijekom korištenja pojedinačnih dijelova dokumenta s klastere utemeljen na sustavu Windows HDInsight (Python i grozd, primjerice), mnogi koraci specifične za klastere sustavom Linux. Korake koji funkcioniraju s klaster utemeljen na sustavu Windows potražite u članku [Analiza leta podatke o kašnjenju pomoću grozd u HDInsight](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Klaster an HDInsight__. U odjeljku [Prvi koraci pri korištenju Hadoop s grozd u HDInsight na Linux](hdinsight-hadoop-linux-tutorial-get-started.md) upute o stvaranju nove klaster sustavom Linux HDInsight.

- __Baze podataka azure SQL__. Baze podataka Azure SQL će se koristiti kao izvor podataka odredište. Ako nemate bazom podataka SQL već, pročitajte članak [baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL u minutama](../sql-database/sql-database-get-started.md).

- __Azure EŽA__. Ako niste instalirali EŽA Azure, potražite u članku [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md) dodatne korake.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Preuzimanje podataka leta

1. Idite na [Istraživanje i Inovativne tehnologije Administracija ured za statistiku prijevoza][rita-website].
2. Na stranici odaberite sljedeće vrijednosti:

  	| Ime | Vrijednost |
  	| ---- | ---- |
  	| Filtar godina | 2013 |
  	| Filtriranje razdoblje. | Siječanj |
  	| Polja | Godina, FlightDate, UniqueCarrier, prijenosni, FlightNum, OriginAirportID, polazište, OriginCityName, OriginState, DestAirportID, odredišne, DestCityName, DestState, DepDelayMinutes, ArrDelay, kašnjenje, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Očisti sva polja |

3. Kliknite **Preuzmi**. 

##<a name="upload-the-data"></a>Prijenos podataka

1. Da biste prenijeli datoteke zip HDInsight čvor glavni klaster, koristite sljedeću naredbu:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Naziv datoteke zip zamijenite __naziv datoteke__ . Zamijenite SSH prijava za HDInsight klaster __korisničko ime__ . Zamijenite CLUSTERNAME naziv klaster HDInsight.
    
    > [AZURE.NOTE] Ako koristite lozinku za prijavu na SSH za provjeru autentičnosti, zatražit će se za lozinku. Ako ste koristili javni ključ, možda ćete morati koristiti u `-i` parametar i navedite put do podudaranja privatni ključ. Na primjer `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Kada prijenos završi, povezati klaster pomoću SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Dodatne informacije o korištenju SSH s operacijskim sustavom Linux HDInsight potražite u sljedećim člancima:
    
    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Nakon uspostave raspakiraj .zip datoteke pomoću sljedeće:

        unzip FILENAME.zip
    
    To će izdvojite .csv datoteke koje je otprilike 60MB po veličini.
    
4. Koristite sljedeće da biste stvorili novi direktorij na WASB (raspodijeljeno spremišta podataka koristi HDInsight), a zatim kopirajte datoteku:

    hdfs dfs - mkdir -p /tutorials/flightdelays/data hdfs dfs-staviti FILENAME.csv/vodiči za/flightdelays/podataka /
    
##<a name="create-and-run-the-hiveql"></a>Stvaranje i pokretanje u HiveQL

Poduzmite sljedeće korake da biste uvezli podatke iz CSV datoteke u tablicu vrste Hive pod nazivom __kašnjenja__.

1. Stvaranje i uređivanje novu datoteku pod nazivom __flightdelays.hql__, koristite sljedeće:

        nano flightdelays.hql
        
    Sadržaj ove datoteke upotrijebili na sljedeći način:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Da biste spremili datoteku pomoću __Ctrl + X__, a zatim __Y__ .

3. Da biste pokrenuli grozd i pokrenite datoteku __flightdelays.hql__ , koristite sljedeće:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] U ovom primjeru `localhost` koristi Budući da ste povezani s čvor glavni klaster HDInsight koji je na kojem se pokreće HiveServer2.

4. Da biste otvorili sesiju interaktivne Beeline, koristite sljedeću naredbu:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Kada primite u `jdbc:hive2://localhost:10001/>` pitanje, koristite sljedeće za dohvaćanje podataka iz uvezene leta podatke o kašnjenju.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    To će dohvatiti popis gradova koji iskustvom vremenske prognoze kašnjenja, zajedno s Prosječno vrijeme i spremiti ga `/tutorials/flightdelays/output`. Kasnije Sqoop će pročitati podatke s tog mjesta i izvoz s bazom podataka SQL Azure.

6. Da biste napustili Beeline, unesite `!quit` naredbenom.

## <a name="create-a-sql-database"></a>Stvaranje baze podataka SQL

Ako već imate SQL baze podataka, morate dobiti naziv poslužitelja. Možete pronaći to [Azure Portal](https://portal.azure.com) tako da odaberete __Baze podataka SQL__, a zatim filtriranje na naziv baze podataka koju želite koristiti. Naziv poslužitelja nalazi se u stupcu __POSLUŽITELJ__ .

Ako već nemate SQL baze podataka, koristiti podatke u [baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL u minutama](../sql-database/sql-database-get-started.md) da biste je stvorili. Morat ćete spremiti naziv poslužitelja koji se koristi za bazu podataka.

##<a name="create-a-sql-database-table"></a>Stvaranje tablice SQL baze podataka

> [AZURE.NOTE] Da biste se povezali s bazom podataka SQL za stvaranje tablice na više načina. Sljedeći koraci pomoću [FreeTDS](http://www.freetds.org/) iz skupine HDInsight.

1. Povezivanje sa sustavom Linux HDInsight klaster pomoću SSH i pokrenite sljedeće korake iz SSH sesije.

3. Da biste instalirali FreeTDS, koristite sljedeću naredbu:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Kada je instaliran FreeTDS, koristite sljedeću naredbu za povezivanje s bazom podataka sustava SQL server. __Naziv poslužitelja__ zamijenite naziv baze podataka SQL poslužitelja. Zamijenite __adminLogin__ i __adminPassword__ prijave za SQL baze podataka. Zamijenite __ImeBazePodataka__ naziv baze podataka.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Primit ćete izlaz sličnu ovoj:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Pri na `1>` upit, unesite sljedeće:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Kada se `GO` izjava unese, prethodne naredbe će se računati. To će stvoriti novu tablicu pod nazivom __kašnjenja__s indeks (potrebnih baze podataka SQL.)

    Da biste potvrdili da je stvorena u tablici, koristite sljedeće:

        SELECT * FROM information_schema.tables
        GO

    Trebali biste vidjeti izlazne sličnu ovoj:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Unesite `exit` pri na `1>` upita da biste izašli iz uslužni tsql.
    
##<a name="export-data-with-sqoop"></a>Izvoz podataka s Sqoop

2. Da biste provjerili Sqoop možete vidjeti SQL baze podataka, koristite sljedeću naredbu:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    To će se vratiti popis baza podataka, uključujući bazu podataka koju ste prethodno stvorili tablicu kašnjenja u.

3. Izvoz podataka iz hivesampletable mobiledata tablici, koristite sljedeću naredbu:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    To upućuje Sqoop za povezivanje s bazom podataka SQL bazu podataka koja sadrži tablicu kašnjenja i izvoz podataka iz na wasbs: / / / vodiči za/flightdelays/izlaz (smo pohranjuju izlaz upita grozd ranije,) u tablici kašnjenja.

4. Po završetku naredbu da biste se povezali s bazom podataka pomoću TSQL koristite sljedeće:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Nakon uspostave da biste potvrdili da podaci izvezeni mobiledata tablicu pomoću sljedeće naredbe:
    
        SELECT * FROM delays
        GO

    Trebali biste vidjeti unos podataka u tablici. Vrsta `exit` da biste izašli iz uslužni tsql.

##<a id="nextsteps"></a>Daljnji koraci

Sada znate kako prenijeti datoteke na spremište blobova platforme Azure, kako pomoću podataka iz spremišta blobova Azure popunjavanje tablicu vrste Hive, pokretanje grozd upite i kako koristiti Sqoop za izvoz podataka iz HDFS s bazom podataka Azure SQL. Dodatne informacije potražite u sljedećim člancima:

* [Uvod u HDInsight][hdinsight-get-started]
* [Korištenje grozd s HDInsight][hdinsight-use-hive]
* [Korištenje Oozie s HDInsight][hdinsight-use-oozie]
* [Korištenje Sqoop s HDInsight][hdinsight-use-sqoop]
* [Korištenje Svinja s HDInsight][hdinsight-use-pig]
* [Razvoj Java MapReduce programe za HDInsight][hdinsight-develop-mapreduce]
* [Razvoj Python Hadoop strujanje programe za HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
