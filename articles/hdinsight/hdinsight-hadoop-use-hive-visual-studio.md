<properties
   pageTitle="Vrste Hive upita pomoću alata za Hadoop za Visual Studio | Microsoft Azure"
   description="Saznajte kako koristiti grozd s Hadoop u HDInsight pomoću alata za Visual Studio Hadoop."
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Pokretanje grozd upita pomoću alata za HDInsight za Visual Studio

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

U ovom se članku ćete naučiti slanje upita grozd programa klaster HDInsight pomoću alata za HDInsight za Visual Studio.

> [AZURE.NOTE] Ovaj dokument Navedite što učiniti HiveQL izraza koji se koriste u primjerima detaljan opis. Informacije o HiveQL koja se koristi u ovom primjeru, potražite u članku [Korištenje vrste Hive s Hadoop na HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće.

* Klaster programa Azure HDInsight (Hadoop na HDInsight) (Linux ili utemeljen na sustavu Windows)

* Visual Studio (jedan od sljedećih verzija):

    Visual Studio 2013 zajednice/Professional/Premium/Ultimate s [obnove 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (zajednice na Enterprise)

- HDInsight Alati za Visual studio. Potražite u članku [Prvi koraci pri korištenju alata Visual Studio Hadoop za HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) informacije o instaliranju i konfiguriranju alate.

##<a id="run"></a>Pokretanje grozd upita pomoću alata za HDInsight za Visual Studio

1. Otvorite **Visual Studio** , a zatim odaberite **Novo** > **projekta** > **HDInsight** > **Vrste Hive aplikacije**. Navedite naziv za taj projekt.

2. Otvorite datoteku **Script.hql** koji je stvoren taj projekt i zalijepite sljedeće naredbe HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Te naredbe izvoditi sljedeće radnje:

    * **DROP TABLE**: brisanje tablice i podatkovne datoteke ako već postoji u tablici.
    * **Stvaranje VANJSKE TABLICE**: stvara novu tablicu 'vanjskih u grozd. Vanjski tablice spremati samo definiciju tablice u grozd (podatke ostaje na izvorno mjesto).

        > [AZURE.NOTE] Vanjski tablice treba koristiti kada očekujete podatke u podlozi ažurirati iz vanjskog izvora (kao što su prijenos proces automatiziranog podataka) ili drugim postupkom MapReduce, ali želite uvijek grozd upita da biste koristili najnovije podatke.
        >
        > Odbacivanje vanjska tablica ne **ne** Izbriši podatke, samo definiciju tablice.

    * **OBLIKOVANJE REDAKA**: govori vrste Hive kako će se podaci oblikovani. U ovom slučaju odvojenih zarezom polja u svakom zapisnika.
    * **SPREMLJENI mjesto kao TEXTFILE**: govori vrste Hive gdje se nalazi podatke pohranjene (oglednim podacima imenik) i koji je pohranjen kao tekst.
    * **Odaberite**: Odaberite broj sve retke u kojima je stupac **t4** sadrže vrijednost **[pogreške]**. Budući da postoje tri retke koji sadrže vrijednost to mora vratiti vrijednost **3** .
    * **INPUT__FILE__NAME kao što su "%.log"** - govori vrste Hive koje ćemo samo mora vratiti podatke iz datoteka u. zapisnika. Ovo pretraživanje ograničava sample.log datoteku koja sadrži podatke, a zadržava iz podatkovne datoteke koje odgovaraju shemi definirali vraća podatke iz druge primjera.

3. Na alatnoj traci odaberite **Klaster HDInsight** koji želite koristiti za ovaj upit, a zatim odaberite **Pošalji da biste WebHCat** za izvođenje naredbe kao grozd posao koristeći WebHCat. Možete poslati i posla pomoću gumba za __izvršavanje putem HiveServer2__ ako je dostupan na vašoj verziji klaster HiveServer2. **Vrste Hive sažetak posla** pojavit će se i prikazuje informacije o izvodi posao. Pomoću veze za **Osvježavanje** da biste osvježili informacije o zadatku dok se ne promijeni **Status zadatka** **dovršiti**.

4. Koristite **Posao izlazne** veze za prikaz izlaz ovaj zadatak. Što treba prikazati `[ERROR] 3`, koji je vrijednost koju vraća iskaza SELECT.

5. Možete i pokrenuti grozd upita bez stvaranja projekta. Pomoću **Programa Explorer poslužitelja**proširite **Azure** > **HDInsight**, desnom tipkom miša kliknite poslužitelj za HDInsight, a zatim odaberite **Pisanje vrste Hive upita**.

6. U dokumentu **temp.hql** koji će se prikazati, dodajte sljedeće naredbe HiveQL:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Te naredbe izvoditi sljedeće radnje:

    * **Stvaranje TABLICA ako ne postoji**: Stvaranje tablice ako ona još ne postoji. Budući da ne koristi **VANJSKI** ključnu riječ, to je interni tablice, koja je pohranjena u skladištu grozd podataka i potpuno upravlja grozd.

        > [AZURE.NOTE] Za razliku od **VANJSKIH** tablice ispuštanje Interna tablice briše i podatke u podlozi.

    * **SPREMLJENI kao ORC**: sprema podatke u retku optimizirana stupčastu (ORC) obliku. Ovo je vrlo optimizirana i učinkovito oblik za spremanje grozd podataka.
    * PREBRIŠI **Umetanje... Odaberite**: služi za odabir redaka iz tablice **log4jLogs** koji sadrže **[pogreške]**, zatim umeće podatke u tablici **errorLogs** .

7. Na alatnoj traci odaberite **Pošalji** da biste pokrenuli posao. Određivanje je uspješno dovršena posla pomoću **Status zadatka** .

8. Da biste provjerili zadatak dovršen, a stvorili novu tablicu, pomoću **Programa Explorer poslužitelja** i proširivanje **Azure** > **HDInsight** > svoj klaster HDInsight > **Vrste Hive baze podataka** > i **zadane**. Trebali biste vidjeti tablici **errorLogs** i tablici **log4jLogs** .

##<a id="summary"></a>Sažetak

Kao što možete vidjeti na HDInsight alate za Visual Studio pružaju jednostavan način pokrenuti grozd upita programa HDInsight klaster, praćenje stanja zadatka i dohvaćanje izlaz.

##<a id="nextsteps"></a>Daljnji koraci

Opće informacije o grozd u HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Dodatne informacije o alatima za HDInsight za Visual Studio:

* [Početak rada s alatima za HDInsight za Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
