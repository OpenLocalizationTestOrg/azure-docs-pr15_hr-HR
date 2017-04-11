<properties
    pageTitle="Saznajte što je grozd te kako koristiti HiveQL | Microsoft Azure"
    description="Informirajte se o vrste Hive Apache i kako ga koristiti uz Hadoop u HDInsight. Odaberite Pokreni grozd i korištenju HiveQL da biste analizirali oglednu datoteku za log4j Apache."
    keywords="hiveql, što je grozd"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Pomoću grozd i HiveQL Hadoop u HDInsight da biste analizirali oglednu datoteku za log4j Apache

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


U ovom ćete praktičnom vodiču ćete Naučite koristiti Apache vrste Hive u Hadoop HDInsight i odaberite kako Pokreni grozd. Također ćete naučiti o HiveQL i analizi oglednu datoteku za log4j Apache.

##<a id="why"></a>Što je grozd i zašto je koristiti?
[Vrste Hive Apache](http://hive.apache.org/) podataka skladištu sustav je za Hadoop, koji omogućuje summarization podataka, slanje upita i analiza podataka pomoću HiveQL (jezika za upite slično SQL). Grozd može se koristiti interaktivno pregledavanje podataka ili stvaranje grupe za ponovno korištenje obrada zadataka.

Grozd omogućuje strukturu projekta na uglavnom nestrukturirane podatke. Nakon definiranja strukturu, grozd možete koristiti za dohvaćanje podataka bez znanja Java ili MapReduce. **HiveQL** (grozd jezik upita) omogućuje vam pisanje upita pomoću naredbe koje su slične T SQL.

Grozd možete koristiti način rada s strukturirane i djelomično strukturirane podatke, poput tekstnih datoteka na kojoj su razgraničena polja tako da određene znakove. Grozd podržava i prilagođene **serijalizatora/deserializers (SerDe)** za složene ili nepravilnog strukturirane podatke. Dodatne informacije potražite u članku [kako koristiti prilagođene SerDe JSON s HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Korisnički definirane funkcije (UDF)

Grozd mogu se proširiti i putem **korisnički definirane funkcije (UDF)**. Na UDF omogućuje implementirati funkcionalnost ili logiku koja nije katalog jednostavno modeliran u HiveQL. Primjer pomoću korisnički definiranih funkcija grozd, potražite u sljedećim člancima:

* [Korištenje Java korisnički definirana funkcija s grozd](hdinsight-hadoop-hive-java-udf.md)

* [Pomoću Python grozd i Svinja u HDInsight](hdinsight-python.md)

* [Korištenje C# s grozd i Svinja u HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Kako dodati prilagođene vrste Hive UDF HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Prilagođene vrste Hive UDF primjer da biste pretvorili oblike datuma/vremena u vremensku oznaku grozd](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Vrste Hive Interna tablice Dodavanje veze za vanjskih vanjske tablice

Što trebate znati o Interna tablicu vrste Hive i vanjske tablice na nekoliko stvari:

- Naredba **CREATE TABLE** stvara Interna tablice. Podatkovne datoteke moraju nalaziti u spremniku zadani.
- Naredba **CREATE TABLE** premješta podatkovnu datoteku /hive/warehouse/<TableName> mapu.
- Naredba **CREATE TABLE za VANJSKE** stvara vanjsku tablicu. Podatkovne datoteke može nalaziti izvan spremnik zadani.
- Naredba **CREATE TABLE za VANJSKE** premještanje podatkovne datoteke.
- Naredba **CREATE TABLE za VANJSKE** ne dopušta sve mape na mjestu. Ovo je razloga zašto vodič čini kopiju datoteke sample.log.

Dodatne informacije potražite u članku [HDInsight: vrste Hive interne i vanjske tablice Uvod][cindygross-hive-tables].


##<a id="data"></a>O oglednim podacima, datoteku log4j Apache

U ovom se primjeru koristi Ogledna datoteka *log4j* , koji je pohranjen na **/example/data/sample.log** u spremniku za pohranu na blob. Svaki zapisnika unutar datoteke sastoji se od redak polja koji sadrži na `[LOG LEVEL]` polja da bi se prikazala vrstu i težinu, na primjer:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

U prethodnom primjeru razinu zapisnika je pogreška.

> [AZURE.NOTE] Možete i generiranje log4j datoteka pomoću alata za zapisivanje [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) , a zatim Prenesite datoteku kontejner s blob. Upute potražite u članku [Prijenos podataka za HDInsight](hdinsight-upload-data.md) . Dodatne informacije o korištenju spremište blobova platforme Azure sa servisa HDInsight potražite u članku [Korištenje blobova platforme Azure s HDInsight](hdinsight-hadoop-use-blob-storage.md).

Ogledni podaci pohranjuju u spremište blobova platforme Azure koju HDInsight koristi kao zadani datotečni sustav. HDInsight možete pristupiti datotekama pohranjenima u BLOB-ova pomoću prefiks **wasb** . Na primjer, da biste pristupili sample.log datoteku, služite sljedeću sintaksu:

    wasbs:///example/data/sample.log

Budući da je spremište blobova platforme Azure zadani prostor za pohranu za HDInsight, datoteku možete pristupiti i pomoću **/example/data/sample.log** iz HiveQL.

> [AZURE.NOTE] Vidjet ćete da sintaksa, **wasbs: / / /**, koristi se za pristup datoteka pohranjenih u spremniku zadani prostor za pohranu za svoj klaster HDInsight. Ako dodatni prostor za pohranu računi koje ste naveli pri dodjeli svoj klaster, a želite pristupiti datotekama pohranjenima u tih računa, podatke možete pristupiti navođenjem spremnik naziv i pohranu računa adresu, na primjer, **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Ogledna posao: Project stupaca na razdvojeni podataka

Sljedeće naredbe HiveQL će project stupaca na razdvojeni podataka pohranjenih u na **wasbs: / / / primjer/podataka** direktorija:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

U prethodnom primjeru, naredbe HiveQL izvoditi sljedeće radnje:

* __Postavljanje hive.execution.engine=tez;__: postavlja modul izvođenja da biste koristili Tez. Korištenje Tez umjesto MapReduce unijeti Povećava performanse upita. Dodatne informacije o Tez potražite u odjeljku [Korištenje Apache Tez za bolje performanse](#usetez) .

    > [AZURE.NOTE] Izjavu o zaštiti je samo potrebne za korištenje utemeljen na sustavu Windows HDInsight klaster; Tez je modul izvođenja zadano za HDInsight koji se temelji na Linux.

* **DROP TABLE**: brisanje tablice i podatkovne datoteke ako već postoji u tablici.
* **Stvaranje VANJSKE TABLICE**: stvara novu tablicu **vanjskih** u grozd. Vanjski tablice samo spremiti definiciju tablice u grozd; Podaci ostaje na izvornom mjestu i u izvornom obliku.
* **OBLIKOVANJE REDAKA**: govori vrste Hive kako će se podaci oblikovani. U ovom slučaju odvojenih zarezom polja u svakom zapisnika.
* **SPREMLJENI mjesto kao TEXTFILE**: govori vrste Hive gdje se nalazi podatke pohranjene (oglednim podacima imenik) i koji je pohranjen kao tekst. Podatke možete biti u jednoj datoteci ili širenje preko više datoteka u direktoriju.
* **Odaberite**: odabire ukupan broj sve retke u kojima je stupac **t4** sadrži vrijednost **[pogreške]**. Budući da postoje tri retke koji sadrže vrijednost to mora vratiti vrijednost **3** .
* **INPUT__FILE__NAME kao što su "%.log"** - govori vrste Hive koje ćemo samo mora vratiti podatke iz datoteka u. zapisnika. Ovo pretraživanje ograničava sample.log datoteku koja sadrži podatke, a zadržava iz podatkovne datoteke koje odgovaraju shemi definirali vraća podatke iz druge primjera.

> [AZURE.NOTE] Vanjski tablice moraju se koristiti kada očekujete temeljni podaci iz vanjskog izvora, kao što su prijenos proces automatiziranog podataka ili drugim postupkom MapReduce ažurirati, a uvijek želite grozd upita da biste koristili najnovije podatke.
>
> Ispuštanje vanjska tablica ne **ne** izbrišete podatke, brišu se samo definiciju tablice.

Nakon stvaranja vanjska tablica sljedeće naredbe se koriste za stvaranje **internog** tablice.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Te naredbe izvoditi sljedeće radnje:

* **Stvaranje TABLICA ako ne postoji**: Stvaranje tablice, ako se još ne postoji. Budući da ne koristi **VANJSKI** ključnu riječ, to je interni tablice, koja je pohranjena u skladištu grozd podataka i potpuno upravlja grozd.
* **SPREMLJENI kao ORC**: sprema podatke u obliku Optimizirano retka stupčastu (ORC). Ovo je vrlo optimizirana i učinkovito oblik za spremanje grozd podataka.
* PREBRIŠI **Umetanje... Odaberite**: odabire redaka iz tablice **log4jLogs** koja sadrži **[pogreške]**, a zatim umeće podatke u tablici **errorLogs** .

> [AZURE.NOTE] Za razliku od vanjskih tablice ispuštanje Interna tablice briše i podatke u podlozi.

##<a id="usetez"></a>Korištenje Apache Tez za bolje performanse

[Apache Tez](http://tez.apache.org) je framework koji omogućuje intenzivno aplikacije za podatke, kao što su grozd, mnogo učinkovitije raditi na razini. U najnovijem izdanju HDInsight grozd podržava sustavom Tez. Tez omogućena je prema zadanim postavkama za klastere sustavom Linux HDInsight.

> [AZURE.NOTE] Tez je trenutno po zadanom isključena za klastere HDInsight utemeljen na sustavu Windows, a mora biti omogućen. Da biste iskoristili Tez, morate postaviti sljedeću vrijednost grozd upita:
>
> ```set hive.execution.engine=tez;```
>
>To se može poslati na temelju po upit tako da na početku upit. Možete postaviti tako da se na po zadanom na klaster postavljanjem vrijednost konfiguracije prilikom stvaranja klaster. Dodatne informacije možete pronaći u [Dodjeljivanje klastere HDInsight](hdinsight-provision-clusters.md).

[Vrste Hive na dokumentima dizajn Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) sadrže broj detalje o implementaciji mogućnosti i ugađanje konfiguracije.

Da biste olakšali ispravljanje pogrešaka poslove pokrenuli pomoću Tez, HDInsight nudi sljedeće web UIs koji omogućuju prikaz detalja o Tez zadataka:

* [Korištenje Tez korisničko Sučelje na HDInsight utemeljen na sustavu Windows](hdinsight-debug-tez-ui.md)

* [Korištenje prikaza Ambari Tez na sustavom Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Odaberite način na koji Pokreni HiveQL

HDInsight mogu se izvoditi HiveQL zadatke pomoću na različite načine. U sljedećoj su tablici pomoću odlučiti koju metodu vam najviše odgovara, a zatim slijedite vezu vodič.

| **Tu mogućnost koristite** ako želite...                                                     | .. .an **interaktivne** shell | ... **Skupna** obrada | .. .with **klaster operacijski sustav** | .. .from taj **klijent operacijski sustav** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Prikaz grozd](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Bilo kojeg (preglednika koji se temelje) |
| [Naredba beeline (iz sesiju SSH)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X ili Windows        |
| [Naredba grozd (iz sesiju SSH)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X ili Windows        |
| [Zakretanja](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux ili Windows                          | Linux, Unix, Mac OS X ili Windows        |
| [Konzola za upit](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Bilo kojeg (preglednika koji se temelje)                            |
| [Alati za HDInsight za Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux ili Windows                          | Windows                                  |
| [Komponente Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux ili Windows                          | Windows                                  |
| [Udaljena radna površina](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Izvođenje zadataka grozd na Azure HDInsight pomoću lokalnog SQL Integracijskim uslugama poslužitelja

Da biste pokrenuli grozd posao možete koristiti i SQL Server integraciju servisa (SSIS). Azure paket značajki za SSIS nudi sljedeće komponente koje funkcioniraju sa zadacima grozd na HDInsight.


- [Azure HDInsight grozd zadatka][hivetask]
- [Azure Upravitelja veze za pretplatu][connectionmanager]


Dodatne informacije o paket značajki za Azure za SSIS [ovdje][ssispack].


##<a id="nextsteps"></a>Daljnji koraci

Sad kad ste naučili što grozd te kako ga koristiti uz Hadoop u HDInsight, koristite sljedeće veze da biste istražili druge načine za rad sa servisom Azure HDInsight.


- [Prijenos podataka HDInsight][hdinsight-upload-data]
- [Korištenje Svinja s HDInsight][hdinsight-use-pig]
- [Korištenje Sqoop s HDInsight](hdinsight-use-sqoop.md)
- [Korištenje Oozie s HDInsight](hdinsight-use-oozie.md)
- [Korištenje MapReduce poslove s HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
