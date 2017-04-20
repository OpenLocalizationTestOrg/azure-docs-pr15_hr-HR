<properties
   pageTitle="Korištenje Hadoop Pig s SSH na HDInsight klastera | Microsoft Azure"
   description="Naučite kako povezati klastera sustavom Linux Hadoop s SSH, a zatim koristite naredbu Pig interaktivno pokretanje latinica Pig izjave ili kao obrade."
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

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Pokrenite Pig poslove na Linux temelji klastera naredbom Pig (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

U ovom dokumentu će voditi kroz postupak povezivanja klastera sustavom Linux Azure HDInsight pomoću sigurne ljuske (SSH), koristeći naredbu Pig interaktivno, pokretanje latinica Pig izjave ili kao obrade.

Latinica Pig programski jezik omogućuje opisuju transformacije koja se primjenjuju na unos podataka da bi se proizveo željeni izlazni rezultat.

> [AZURE.NOTE] Ako već poznajete pomoću sustavom Linux Hadoop poslužiteljima, ali su nove HDInsight, pogledajte [Savjete sustavom Linux HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, trebat će vam sljedeće.

* Klastera sustavom Linux HDInsight (Hadoop na HDInsight).

* Klijent SSH. Linux, Unix i Mac OS treba dolaze s SSH klijenta. Korisnici sustava Windows morate preuzeti klijent, primjerice [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Povezivanje s SSH

Povezati na potpuno kvalificirani naziv domene (FQDN) od HDInsight klastera pomoću naredbe SSH. U FQDN bit će naziv koji ste dali klastera, zatim **. azurehdinsight.net**. Ako, na primjer, sljedeće želite povezati klastera pod nazivom **myhdinsight**.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Ako navedeni ključ potvrde za provjeru autentičnosti SSH** stvaranja klastera HDInsight, možda ćete morati navesti mjesto privatnog ključa na klijent sustava.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Ako navedena lozinka za provjeru autentičnosti SSH** stvaranja klastera HDInsight, morat ćete lozinku kada se to od vas zatraži.

Dodatne informacije o korištenju SSH s HDInsight potražite [Koristi SSH sa sustavom Linux Hadoop na HDInsight iz Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (Windows temelji klijenti)

Windows pružiti ugrađene SSH klijenta. Preporučujemo korištenje **PuTTY**, koji se preuzimaju iz [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Dodatne informacije o korištenju PuTTY potražite [Koristi SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Koristite naredbu Pig

2. Nakon povezani, pokrenite Pig sučelje naredbenog retka (CLI za Defragmentaciju) pomoću sljedeću naredbu.

        pig

    Nakon nekoliko trenutaka, trebali biste vidjeti na `grunt>` upit.

3. Unesite sljedeći iskaz.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Ova naredba učitava sadržaj sample.log datoteke u ZAPISNICIMA. Sadržaj datoteke možete pogledati pomoću sljedeće.

        DUMP LOGS;

4. Dalje, pretvorba podataka primjenom Regularni izraz izdvojiti samo razina zapisivanja iz svakog zapisa korištenjem sljedeće.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **DUMP** možete koristiti za prikaz podataka nakon transformacije. U tom slučaju koristite `DUMP LEVELS;`.

5. Nastavite pomoću sljedećih tvrdnji primjene transformacije. Korištenje `DUMP` da biste prikazali rezultat pretvorbe nakon svakog koraka.

    <table>
    <tr>
    <th>Iskaz</th><th>Što radi</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = RAZINA filtra po LOGLEVEL nije null;</td><td>Uklanja retke koji sadrže null vrijednost za razinu zapisnika i sprema rezultate u FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS GRUPE po LOGLEVEL;</td><td>Grupe redaka po razini zapisnika i sprema rezultate u GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREKVENCIJE = foreach GROUPEDLEVELS generiranje grupu kao LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL) kao BROJANJE;</td><td>Stvara novi skup podataka koji sadrži svaki jedinstveni zapisnika vrijednost razine i koliko se puta ga pojavljuje. Spremljena je u FREKVENCIJE.</td>
    </tr>
    <tr>
    <td>REZULTAT = naloga FREKVENCIJE po COUNT desc;</td><td>Nalozi razine zapisnika po count (silazno) i pohranjuje u REZULTAT.</td>
    </tr>
    </table>

6. Možete spremiti i rezultate transformaciju korištenjem u `STORE` izjave. Na primjer, sljedeći sprema u `RESULT` direktorij **/example/data/pigout** na zadani pohranu spremnik za vaše klastera.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Podaci spremljeni u navedeni direktorij datoteke naziva **dijela nnnnn**. Ako imenik već postoji, primite poruku o pogrešci.

7. Da biste zatvorili naredbeni redak grunt, unesite sljedeći iskaz.

        QUIT;

###<a name="pig-latin-batch-files"></a>Latinica Pig naredbene datoteke

Naredbu Pig možete koristiti i za pokretanje Pig latinica sadržane u datoteci.

3. Nakon izlaska iz upita grunt, koristite sljedeću naredbu da biste kanal STDIN u datoteku pod nazivom **pigbatch.pig**. Kreirat će se ova datoteka u kućnoj imenik za koju su prijavljeni za sesiju SSH računa.

        cat > ~/pigbatch.pig

4. Upišite ili zalijepite sljedeće retke, a zatim koristite Ctrl + D kada završite.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Koristite sljedeće da biste pokrenuli datoteku **pigbatch.pig** pomoću naredbe Pig.

        pig ~/pigbatch.pig

    Kada se obrada završi, trebali biste vidjeti sljedećih izlaza koji treba biti isti kao kada koristi `DUMP RESULT;` u prethodnim koracima.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Sažetak

Kao što možete vidjeti naredbu Pig omogućuje interaktivno pokrenuti MapReduce postupke korištenjem latinica Pig, kao i pokrenite izvatke pohranjene u skupnu datoteku.

##<a id="nextsteps"></a>Sljedeći koraci

Opće informacije o Pig u HDInsight.

* [Korištenje Pig s Hadoop na HDInsight](hdinsight-use-pig.md)

Informacije na druge načine možete raditi s Hadoop na HDInsight.

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
