<properties
   pageTitle="Korištenje Hadoop grozd i udaljene radne površine u HDInsight | Microsoft Azure"
   description="Saznajte kako se povezati sa Hadoop klaster u HDInsight pomoću udaljene radne površine, a zatim pokrenite grozd upita pomoću sučelja vrste Hive naredbenog retka."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Korištenje grozd s Hadoop na HDInsight putem udaljene radne površine

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

U ovom se članku će Saznajte kako se povezati na klaster HDInsight pomoću udaljene radne površine, a zatim pokrenite grozd upita pomoću na vrste Hive sučelje naredbenog retka (EŽA).

> [AZURE.NOTE] Ovaj dokument Navedite što učiniti HiveQL izraza koji se koriste u primjerima detaljan opis. Informacije o HiveQL koja se koristi u ovom primjeru, potražite u članku [Korištenje vrste Hive s Hadoop na HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Klaster utemeljen na sustavu Windows HDInsight (Hadoop na HDInsight)

* Klijentsko računalo sa sustavom Windows 10, prozor 8 ili Windows 7

##<a id="connect"></a>Povezivanje s udaljene radne površine

Omogućivanje udaljene radne površine za HDInsight klaster, a zatim s njim povezati slijedeći upute u [klastere HDInsight pomoću RDP za povezivanje](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hive"></a>Koristite naredbu grozd

Kad ste povezani s računala za HDInsight klaster, poduzmite sljedeće korake da biste radili s grozd:

1. Na radnoj površini HDInsight početnom **Hadoop naredbenog retka**.

2. Unesite sljedeću naredbu da biste pokrenuli EŽA vrste Hive:

        %hive_home%\bin\hive

    Kada je pokrenut na EŽA, prikazat će se upit za vrste Hive EŽA: `hive>`.

3. Koristi se EŽA, unesite sljedeće naredbe za stvaranje nove tablice pod nazivom **log4jLogs** korištenje oglednih podataka:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Te naredbe izvoditi sljedeće radnje:

    * **DROP TABLE**: brisanje tablice i podatkovne datoteke ako već postoji u tablici.

    * **Stvaranje VANJSKE TABLICE**: stvara novu tablicu 'vanjskih u grozd. Vanjski tablice spremiti samo definiciju tablice u grozd (podatke ostaje na izvorno mjesto).

        > [AZURE.NOTE] Vanjski tablice treba koristiti kada očekujete podatke u podlozi ažurirati iz vanjskog izvora (kao što su prijenos proces automatiziranog podataka) ili drugim postupkom MapReduce, ali želite uvijek grozd upita da biste koristili najnovije podatke.
        >
        > Odbacivanje vanjska tablica ne **ne** Izbriši podatke, samo definiciju tablice.

    * **OBLIKOVANJE REDAKA**: govori vrste Hive kako će se podaci oblikovani. U ovom slučaju odvojenih zarezom polja u svakom zapisnika.

    * **SPREMLJENI mjesto kao TEXTFILE**: govori vrste Hive gdje se nalazi podatke pohranjene (oglednim podacima imenik) i koji je pohranjen kao tekst.

    * **Odaberite**: odabire ukupan broj sve retke u kojima je stupac **t4** sadrži vrijednost **[pogreške]**. Budući da postoje tri retke koji sadrže vrijednost to mora vratiti vrijednost **3** .

    * **INPUT__FILE__NAME kao što su "%.log"** - govori vrste Hive koje ćemo samo mora vratiti podatke iz datoteka u. zapisnika. Ovo pretraživanje ograničava sample.log datoteku koja sadrži podatke, a zadržava iz podatkovne datoteke koje odgovaraju shemi definirali vraća podatke iz druge primjera.


4. Da biste stvorili novu tablicu 'Interna pod nazivom **errorLogs**pomoću sljedeće naredbe:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Te naredbe izvoditi sljedeće radnje:

    * **Stvaranje TABLICA ako ne postoji**: Stvaranje tablice ako ona još ne postoji. Budući da ne koristi **VANJSKI** ključnu riječ, to je interni tablice, koja je pohranjena u skladištu grozd podataka i potpuno upravlja grozd.

        > [AZURE.NOTE] Za razliku od **VANJSKIH** tablice ispuštanje Interna tablice briše i podatke u podlozi.

    * **SPREMLJENI kao ORC**: sprema podatke u retku optimizirana stupčastu (ORC) obliku. Ovo je vrlo optimizirana i učinkovito oblik za spremanje grozd podataka.

    * PREBRIŠI **Umetanje... Odaberite**: služi za odabir redaka iz tablice **log4jLogs** koji sadrže **[pogreške]**, zatim umeće podatke u tablici **errorLogs** .

    Da biste potvrdili da samo redaka koji sadrže **[pogreške]** u stupcu t4 su pohranjene u tablici **errorLogs** , koristite sljedeću naredbu da biste se vratili sve retke iz **errorLogs**:

        SELECT * from errorLogs;

    Tri redaka podataka bi trebala biti vraćena, svi koji sadrže **[pogreške]** u t4 stupca.

##<a id="summary"></a>Sažetak

Kao što je vidljivo na naredbu grozd omogućuje jednostavnu interaktivno pokretanje grozd upita na programa HDInsight klaster, praćenje stanja zadatka i dohvaćanje izlaz.

##<a id="nextsteps"></a>Daljnji koraci

Opće informacije o grozd u HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Ako koristite Tez grozd, pogledajte sljedeće dokumente za ispravljanje pogrešaka informacije:

* [Korištenje Tez korisničko Sučelje na HDInsight utemeljen na sustavu Windows](hdinsight-debug-tez-ui.md)

* [Korištenje prikaza Ambari Tez na sustavom Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

