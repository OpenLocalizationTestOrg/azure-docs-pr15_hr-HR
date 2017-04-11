<properties
    pageTitle="Korištenje Hadoop Sqoop u sustavom Linux HDInsight | Microsoft Azure"
    description="Saznajte kako da biste pokrenuli Sqoop uvoz i izvoz između Hadoop Linux utemeljen na HDInsight klaster i baze podataka Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Korištenje Sqoop s Hadoop u HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saznajte kako koristiti Sqoop za uvoz i izvoz između sustavom Linux HDInsight klaster i baze podataka SQL Azure ili SQL Server bazu podataka.

> [AZURE.NOTE] Koraci u ovom članku pomoću SSH za povezivanje s operacijskim sustavom Linux HDInsight klaster. Klijenti za Windows možete koristiti Azure PowerShell i HDInsight .NET SDK za rad sa Sqoop na klastere sustavom Linux. Da biste otvorili te članke pomoću birač tabulatora.

##<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **A Hadoop klaster u HDInsight** i bazom __Baze podataka SQL Azure__: koraci u ovom dokumentu temelje se na klaster i baza podataka stvorena je pomoću dokumenta [klaster Stvori i SQL baze podataka](hdinsight-use-sqoop.md#create-cluster-and-sql-database) . Ako već imate HDInsight klaster i SQL baze podataka, možete zamijeniti one vrijednosti koji se koristi u ovom dokumentu.
- **Radne stanice**: računalo s klijent za SSH.

##<a name="install-freetds"></a>Instalacija FreeTDS

1. Povezivanje sa sustavom Linux HDInsight klaster pomoću SSH. Je li adresa za korištenje prilikom povezivanja `CLUSTERNAME-ssh.azurehdinsight.net` i priključak je `22`.

    Dodatne informacije o korištenju SSH da biste se povezali sa servisom HDInsight potražite u članku sljedeće dokumente:

    * **Linux, Unix ili OS X klijenata**: potražite u članku [Povezivanje sa sustavom Linux HDInsight klaster Linux, OS X ili Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Klijenti za Windows**: potražite u članku [Povezivanje sa sustavom Linux HDInsight klaster iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Da biste instalirali FreeTDS, koristite sljedeću naredbu:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS će se koristiti u nekoliko koraka da biste se povezali s bazom podataka SQL.

##<a name="create-the-table-in-sql-database"></a>Stvaranje tablice u bazi podataka SQL

> [AZURE.IMPORTANT] Ako koristite HDInsight klaster i SQL baze podataka stvorene pomoću koraka u [Stvaranje klaster i SQL baze podataka](hdinsight-use-sqoop.md), zanemarite koraci u ovom odjeljku kao bazu podataka i tablicu koji su stvoreni u sklopu koraka u tom dokumentu.

1. Iz SSH veze sa servisom HDInsight, koristite sljedeću naredbu za povezivanje s bazom podataka sustava SQL server i stvor tablici koja će se koristiti u ostatak ovih koraka:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Primit ćete izlaz sličnu ovoj:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Pri na `1>` upit, unesite sljedeće:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Kada se `GO` izjava unese, vrednovao prethodne naredbe. Najprije se stvara tablica **mobiledata** , a zatim ga (obavezno baze podataka SQL.) se dodaje u indeks

    Da biste potvrdili da je stvorena u tablici, koristite sljedeće:

        SELECT * FROM information_schema.tables
        GO

    Trebali biste vidjeti izlaz sličnu ovoj:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Unesite `exit` pri na `1>` upita da biste izašli iz uslužni tsql.

##<a name="sqoop-export"></a>Izvoz Sqoop

3. Iz SSH veze sa servisom HDInsight, Oda sljedeću naredbu da biste provjerili Sqoop možete vidjeti SQL baze podataka:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    To će se vratiti popis baza podataka, uključujući **sqooptest** bazu podataka koju ste prethodno stvorili.

4. Izvoz podataka iz **hivesampletable** **mobiledata** tablici, koristite sljedeću naredbu:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    To upućuje Sqoop za povezivanje s bazom podataka SQL baza podataka **sqooptest** i izvoz podataka iz na **wasbs: / / / grozd/skladištu/hivesampletable** (fizičke datoteke za *hivesampletable*) u tablici **mobiledata** .

5. Po završetku naredbu da biste se povezali s bazom podataka pomoću TSQL koristite sljedeće:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Nakon uspostave da biste potvrdili da podaci izvezeni **mobiledata** tablicu pomoću sljedeće naredbe:

        SELECT * FROM mobiledata
        GO

    Trebali biste vidjeti unos podataka u tablici. Vrsta `exit` da biste izašli iz uslužni tsql.

##<a name="sqoop-import"></a>Uvoz Sqoop

1. Koristite sljedeće da biste uvezli podatke iz tablice **mobiledata** u bazi podataka SQL do na **wasbs: / / / vodiči za/usesqoop/importeddata** imeničkog na HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Uvezene podatke sadržavat će polja koja su odvojene znak tabulatora, a reci će prekinuti znakom novi redak.

2. Kada se uvoz dovrši, koristite sljedeću naredbu popis podatke u novu imeniku:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Korištenje sustava SQL Server

Sqoop možete koristiti i za uvoz i izvoz podataka iz sustava SQL Server u podatkovnom centru ili na virtualnog računala smješten u Azure. Razlike između korištenja baze podataka SQL i SQL Server su:

* HDInsight i SQL Server mora se nalaziti na istom Azure virtualne mreže

    > [AZURE.NOTE] HDInsight podržava samo temeljena virtualne mreže, a ne trenutno radi s afinitet grupne virtualne mreže.

    Kada koristite SQL Server u vašem podatkovnog centra, morate konfigurirati virtualne mreže kao *web-mjesto* ili *točke na web*.

    > [AZURE.NOTE] Za **točku web** virtualne mreže, SQL Server mora biti pokrenut VPN klijent program za konfiguriranje koji je dostupan **nadzorne ploče** Azure virtualne mrežna konfiguracija.

    Dodatne informacije o Azure virtualne mreže potražite u članku [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md).

* Da biste omogućili SQL provjeru autentičnosti, morate ga konfigurirati SQL Server. Dodatne informacije potražite u članku [Odabir načina provjere autentičnosti](https://msdn.microsoft.com/ms144284.aspx)

* Možda ćete morati konfigurirati SQL Server da biste prihvatili daljinske veze. Dodatne informacije potražite u članku [upute za otklanjanje poteškoća s povezivanjem modul baze podataka sustava SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* Morate stvoriti **sqooptest** bazu podataka sustava SQL Server pomoću programa kao što je **SQL Server Management Studio** ili **tsql** – upute za korištenje EŽA Azure funkcioniraju samo za baze podataka SQL Azure

    Naredbe TSQL da biste stvorili tablicu **mobiledata** slični su onima za SQL baze podataka, osim stvaranja na clusterd indeksa – to nije obavezno za SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Prilikom povezivanja s poslužiteljem SQL iz servisa HDInsight, možda ćete morati koristiti IP adrese sustava SQL Server, osim ako ste konfigurirali u DNS Domain Name System () za razrješavanje imena u mreži virtualne Azure. Ako, na primjer:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Ograničenja

* Skupno izvoz - Linux s operacijskim sustavom HDInsight, Sqoop poveznik za izvoz podataka u Microsoft SQL Server ili baze podataka SQL Azure trenutno ne podržava umeće skupno.

* Grupno slanje promjena - sa sustavom Linux HDInsight kada se koristi u `-batch` prebacivanje prilikom izvršavanja umeće, Sqoop će obavljanje više umeće umjesto grupnog slanja promjena operacije Umetanje.

##<a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako koristiti Sqoop. Da biste saznali više, pogledajte:

- [Korištenje Oozie s HDInsight][hdinsight-use-oozie]: korištenje Sqoop akcije u tijeku rada Oozie.
- [Analizirati podatke o kašnjenju leta pomoću HDInsight][hdinsight-analyze-flight-data]: korištenje vrste Hive da biste analizirali leta odgađanje podataka, a zatim pomoću Sqoop za izvoz podataka u bazu podataka Azure SQL.
- [Prijenos podataka HDInsight][hdinsight-upload-data]: pronalaženje drugih načina za prijenos podataka u spremište blobova platforme HDInsight/Azure.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
