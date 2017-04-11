<properties
   pageTitle="Korištenje ljuske grozd u HDInsight (Hadoop) | Microsoft Azure"
   description="Saznajte kako pomoću ljuske grozd sa sustavom Linux HDInsight klaster. Koje će Saznajte kako se povezati klaster HDInsight pomoću SSh, a zatim pomoću ljuske vrste Hive interaktivno pokretanje upita."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Korištenje grozd s Hadoop u HDInsight s SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

U ovom se članku će Saznajte kako koristiti sigurne ljuske (SSH) za povezivanje s Hadoop na Azure HDInsight klaster i zatim interaktivno slanje upita grozd pomoću sučelja grozd naredbenog retka (EŽA).

> [AZURE.IMPORTANT] Naredba grozd bit će dostupni na klastere sustavom Linux HDInsight, razmislite o korištenju Beeline. Beeline novijeg klijenta za rad s grozd, a nalazi se u sklopu svoj klaster HDInsight. Dodatne informacije o korištenju to potražite u članku [Korištenje vrste Hive s Hadoop u HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* U operacijskim sustavom Linux Hadoop na klasteru HDInsight.

* Klijent za SSH. Linux, Unix i Mac OS mora obuhvaćati klijent za SSH. Korisnici sustava Windows morate preuzeti klijent, kao što su [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Povezivanje s SSH

Povezivanje s na potpuno kvalificirani naziv domene (FQDN) od svoj klaster HDInsight pomoću naredbe "SSH". FQDN bit će naziv koji ste dali klaster, zatim **. azurehdinsight.net**. Na primjer, sljedeće želite povezati klaster pod nazivom **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Ako ste unijeli ključ certifikat za provjeru autentičnosti SSH** stvaranja HDInsight klaster, možda ćete morati navedite mjesto privatni ključ na klijentskim sustavima:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Ako upišete lozinku za provjeru autentičnosti SSH** stvaranja HDInsight klaster, morat ćete unijeti lozinku kad se to od vas zatraži.

Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (klijenti utemeljen na sustavu Windows)

Windows ne nudi ugrađene SSH klijenta. Preporučujemo korištenje **PuTTY**, koji možete preuzeti s [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Dodatne informacije o korištenju PuTTY potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Koristite naredbu grozd

2. Nakon uspostave pokrenuti EŽA vrste Hive u sljedeću naredbu:

        hive

3. Koristi se EŽA, unesite sljedeće naredbe za stvaranje nove tablice pod nazivom **log4jLogs** pomoću ogledne podatke:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Te naredbe izvoditi sljedeće radnje:

    * **DROP TABLE** – brisanje tablice i podatkovne datoteke u slučaju da već postoji u tablici.
    * **Stvaranje VANJSKE TABLICE** – stvara novu tablicu 'vanjskih u grozd. Vanjski tablice spremati samo definiciju tablice u grozd. Podaci ostaje na izvornom mjestu.
    * **OBLIKOVANJE REDAKA** - govori vrste Hive kako će se podaci oblikovani. U ovom slučaju odvojenih zarezom polja u svakom zapisnika.
    * **POHRANJENE mjesto kao TEXTFILE** - govori vrste Hive gdje se nalazi podatke pohranjene (imenik oglednim podacima), a da je pohranjenih kao tekst.
    * **Odaberite** – odabir ukupan broj sve retke u kojima je stupac **t4** sadrži vrijednost **[pogreške]**. To mora vratiti vrijednost **3** kao što su tri retke koji sadrže vrijednost.
    * **INPUT__FILE__NAME kao što su "%.log"** - govori vrste Hive koje ćemo samo mora vratiti podatke iz datoteka u. zapisnika. Ovo pretraživanje ograničava sample.log datoteku koja sadrži podatke, a zadržava iz podatkovne datoteke koje odgovaraju shemi definirali vraća podatke iz druge primjera.

    > [AZURE.NOTE] Vanjski tablice treba koristiti kada očekujete podatke u podlozi ažurirati iz vanjskog izvora, kao što su prijenos proces automatiziranog podataka ili pak tako da drugi MapReduce postupak, ali uvijek želite grozd upita da biste koristili najnovije podatke.
    >
    > Odbacivanje vanjska tablica ne **ne** Izbriši podatke, samo definiciju tablice.

4. Da biste stvorili novu tablicu 'Interna pod nazivom **errorLogs**pomoću sljedeće naredbe:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Te naredbe izvoditi sljedeće radnje:

    * **Stvaranje TABLICA ako ne postoji** - stvara tablicu, ako se još ne postoji. Budući da ne koristi **VANJSKI** ključnu riječ, to je interni tablice, koja je pohranjena u skladištu grozd podataka i potpuno upravlja grozd.
    * **SPREMLJENI kao ORC** - sprema podatke u obliku Optimizirano retka stupčastu (ORC). Ovo je vrlo optimizirana i učinkovito oblik za spremanje grozd podataka.
    * PREBRIŠI **Umetanje... Odaberite** - odabire redaka iz tablice **log4jLogs** koja sadrži **[pogreške]**, a zatim umeće podatke u tablicu **errorLogs** .

    Da biste potvrdili da su samo retke koji sadrže **[pogreške]** u stupcu t4 pohranjene u tablici **errorLogs** , koristite sljedeću naredbu da biste prikazali sve retke iz **errorLogs**:

        SELECT * from errorLogs;

    Tri redaka podataka bi trebala biti vraćena, svi koji sadrže **[pogreške]** u t4 stupca.

    > [AZURE.NOTE] Za razliku od vanjskih tablice ispuštanje Interna tablica će izbrisati podatke u podlozi kao i.

##<a id="summary"></a>Sažetak

Kao što vidite, naredba grozd omogućuje jednostavnu interaktivno pokretanje grozd upita na programa HDInsight klaster, praćenje stanja zadatka i dohvaćanje izlaz.

##<a id="nextsteps"></a>Daljnji koraci

Općenite informacije o grozd u HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

Dodatne informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Ako koristite Tez grozd, pogledajte sljedeće dokumente za ispravljanje pogrešaka informacije:

* [Korištenje Tez korisničko Sučelje na HDInsight utemeljen na sustavu Windows](hdinsight-debug-tez-ui.md)

* [Korištenje prikaza Ambari Tez na sustavom Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

