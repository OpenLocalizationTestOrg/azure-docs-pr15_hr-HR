<properties
   pageTitle="Korištenje Hadoop Svinja s udaljene radne površine u HDInsight | Microsoft Azure"
   description="Saznajte kako pomoću naredbe Svinja da biste pokrenuli latinica Svinja naredbe iz veze udaljene radne površine klaster utemeljen na sustavu Windows Hadoop u HDInsight."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Pokretanje poslove Svinja s veze udaljene radne površine

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ovaj dokument sadrži vodič za korištenje naredbe Svinja da biste pokrenuli latinica Svinja izvatke iz veze udaljene radne površine utemeljen na sustavu Windows HDInsight klaster. Svinja latinica omogućuje stvaranje MapReduce aplikacija tako da s opisom transformacije podataka umjesto mapiranje i smanjivanje funkcije.

U ovom dokumentu, Saznajte kako

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće.

* Klaster utemeljen na sustavu Windows HDInsight (Hadoop na HDInsight)

* Klijentskog računala sa sustavom Windows 10, Windows 8 ili Windows 7

##<a id="connect"></a>Povezivanje s udaljene radne površine

Omogućivanje udaljene radne površine za HDInsight klaster, a zatim s njim povezati slijedeći upute u [klastere HDInsight pomoću RDP za povezivanje](hdinsight-administer-use-management-portal.md#rdp).

##<a id="pig"></a>Koristite naredbu Svinja

2. Nakon što ste vezu udaljene radne površine, najprije pomoću ikone na radnoj površini **Hadoop naredbenog retka** .

2. Da biste pokrenuli naredbu Svinja, koristite sljedeće:

        %pig_home%\bin\pig

    Primit ćete s na `grunt>` upit.

3. Unesite sljedeću naredbu:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Ta se naredba učitava sadržaj datoteke sample.log u datoteke ZAPISNIKA. Sadržaj datoteke možete prikazati pomoću sljedeće naredbe:

        DUMP LOGS;

4. Pretvaranje podataka primjenom Uobičajeni izraz za izdvajanje samo željenu razinu zapisivanja iz svaki zapis:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **Ispis** možete koristiti za prikaz podataka nakon transformaciju. U ovom slučaju `DUMP LEVELS;`.

5. Primjena transformacije pomoću sljedeće naredbe i dalje. Korištenje `DUMP` da biste vidjeli rezultat pretvorbe nakon svakog koraka.

    <table>
    <tr>
    <th>Naredba</th><th>Funkcija</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = razine FILTRIRANJE po LOGLEVEL nije null;</td><td>Uklanja retke koji sadrže null vrijednost za razinu zapisnika i sprema rezultate u FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS GRUPE tako da LOGLEVEL;</td><td>Grupe redaka prema razini zapisnika i sprema rezultate u GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>UČESTALOST = foreach GROUPEDLEVELS generiranje grupe kao LOGLEVEL, broj (FILTEREDLEVELS. LOGLEVEL) kao što je BROJANJE;</td><td>Stvara novi skup podataka koji sadrži svaki jedinstveni zapisnika vrijednost razine i koliko se puta je pojavljuje se. To je pohranjena u UČESTALOST</td>
    </tr>
    <tr>
    <td>REZULTAT = redoslijed UČESTALOSTI po COUNT desc;</td><td>Narudžbe razine zapisnika po count (silazni) i pohranjuje u REZULTAT</td>
    </tr>
    </table>

6. Rezultati transformaciju možete spremiti i pomoću na `STORE` izjava. Na primjer, sljedeću naredbu sprema na `RESULT` direktorij **/example/data/pigout** u spremniku zadani prostor za pohranu za svoj klaster:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Navedeni direktorij u datotekama **dio nnnnn**pohrane podataka. Ako već postoji imenik, primit ćete poruku o pogrešci.

7. Da biste napustili grunt upit, unesite sljedeću naredbu.

        QUIT;

###<a name="pig-latin-batch-files"></a>Svinja latinica grupe za datoteke

Da biste pokrenuli latinica Svinja koji je sadržan u datoteci možete koristiti i naredbu Svinja.

3. Nakon izlaska iz njega grunt upit, otvorite **Blok za pisanje** i stvorite novu datoteku pod nazivom **pigbatch.pig** u direktoriju **PIG_HOME %** .

4. Upišite ili zalijepite sljedeće retke u **pigbatch.pig** datoteku i spremite je:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Koristite sljedeće da biste pokrenuli **pigbatch.pig** datoteku pomoću naredbe za svinja.

        pig %PIG_HOME%\pigbatch.pig

    Kada se završi obrada, trebali biste vidjeti sljedeće Izlaz, što bi trebalo biti jednaki kada koristi `DUMP RESULT;` u prethodnim koracima:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Sažetak

Kao što možete vidjeti naredbu Svinja omogućuje interaktivno pokrenuti MapReduce operacije ili pokrenuti latinica Svinja zadatke koji su pohranjenu u naredbena datoteka.

##<a id="nextsteps"></a>Daljnji koraci

Općenite informacije o Svinja u HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
