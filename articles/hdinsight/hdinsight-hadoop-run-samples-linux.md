<properties
    pageTitle="Mada Hadoop MapReduce uzoraka sustavom Linux HDInsight | Microsoft Azure"
    description="Početak rada s MapReduce uzorka s operacijskim sustavom Linux HDInsight. Korištenje SSH za povezivanje s klaster, a zatim pomoću naredbe Hadoop pokretanje Ogledna zadataka."
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
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>Uzorci Hadoop pokretanje u HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Sustavom Linux klastere HDInsight unesite skup MapReduce uzorke koje se može koristiti za Upoznajte se s Hadoop MapReduce izvođenje zadataka. U ovom dokumentu, Saznajte više o dostupna uzoraka i voditi kroz pokrenut nekoliko od njih.

##<a name="prerequisites"></a>Preduvjeti

- **Mogući Azure pretplatu**: potražite u članku [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **Klaster sustavom Linux odgovora HDInsight**: potražite u članku [Prvi koraci pri korištenju Hadoop s grozd u HDInsight na Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Mogući SSH klijent**: informacije o korištenju SSH sa servisa HDInsight potražite u sljedećim člancima:

    - [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>Primjere ##

**Lokacija**: uzorcima nalaze se na HDInsight klaster pri **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Sadržaj**: sljedeća primjera nalaze se u tu arhivu:

- **aggregatewordcount**: programa zbrajanja temelji karta/smanjiti program koji broji riječi u ulaznih datoteka
- **aggregatewordhist**: programa zbrajanja temelji karta/smanjiti program koji izračunava histogram riječi u ulaznih datoteka
- **bbp**: Karta/smanjiti program koji koristi Bailey Borwein Plouffe za izračunavanje točno znamenke broja Pi.
- **dbcount**: na primjer posla koji broji zapisnike pageview pohranjene u u bazi podataka
- **distbbp**: Karta/smanjiti program koji koristi BBP vrstu formule za izračunavanje točno bitova Pi
- **grep**: Karta/smanjiti program koji broji podudaranja regex u unos
- **Uključivanje**: zadatak koji efekti spoj preko sortirati, jednako particije skupova podataka
- **multifilewc**: posla koji broji riječi iz više datoteka
- **pentomino**: Karta/smanjiti pločica postavljanje programa za traženje rješenja za probleme pentomino
- **pi**: Karta/smanjiti programa koji se procjenjuje Pi koristeći postupak SMS Karlo način
- **randomtextwriter**: Karta/smanjiti programa koji se piše 10 GB slučajni tekstualnih podataka po čvor
- **randomwriter**: Karta/smanjiti programa koji se piše 10 GB slučajni podataka po čvor
- **secondarysort**: primjer definiranje sekundarne sortiranje da biste na smanjivanje
- **Sortiranje**: program za karte/smanjiti kojom se sortira od podataka koji je napisao slučajni writer
- **sudoku**: sudoku alata za rješavanje
- **teragen**: generiranje podataka za na terasort
- **terasort**: pokretanje u terasort
- **teravalidate**: Provjera rezultate terasort
- **wordcount**: Karta/smanjiti program koji broji riječi u ulaznih datoteka
- **wordmean**: Karta/smanjiti programa koji se izračunava prosječnu duljinu riječi u ulaznih datoteka
- **wordmedian**: Karta/smanjiti program koji broji median duljinu riječi u ulaznih datoteka
- **wordstandarddeviation**: Karta/smanjiti program koji broji standardna devijacija otpornosti duljinu riječi u ulaznih datoteka

**Izvorni kod**: izvornog koda za te primjere nalazi se na HDInsight klaster pri **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] Na `2.2.4.9-1` na put je verzija platforme podataka Hortonworks za HDInsight klaster i može se promijeniti kako se ažurira HDInsight.

## <a name="how-to-run-the-samples"></a>Pokretanje uzorcima ##

1. Povezivanje sa servisom HDInsight pomoću SSH kao što je opisano u sljedećim člancima:

    - [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Iz na `username@#######:~$` pitanje, koristite sljedeću naredbu da biste dobili popis primjera:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Taj se način stvara popis uzorak iz prethodnog odjeljka ovog dokumenta.

3. Koristite sljedeću naredbu da biste dobili pomoć na određene uzorka. U ovom slučaju uzorka **wordcount** :

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    Trebali biste pojavljuje se sljedeća poruka:

        Usage: wordcount <in> [<in>...] <out>

    To znači da možete unijeti nekoliko unos putove za izvorni dokumenti. Konačni put je gdje je pohranjena izlazna (broj riječi u izvorni dokumenti).

4. Da biste prebrojali sve riječi u u bilježnice programa Leonardo Da Vinci, koji su dostupni kao ogledne podatke s svoj klaster, koristite sljedeće:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Unos za ovaj zadatak je za čitanje iz **wasbs:///example/data/gutenberg/davinci.txt**.

    Izlaz za ovaj primjer pohranjuju se u **wasbs: / / / primjer/podataka/davinciwordcount**.

    > [AZURE.NOTE] Kao što je naznačeno u pomoći za uzorak wordcount, nije moguće navesti i više datoteka za unos. Na primjer, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` biste prebrojali riječi u davinci.txt i ulysses.txt.

5. Nakon dovršetka posla, koristite sljedeću naredbu da biste vidjeli rezultat:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Spaja sve izlazne datoteke koje je stvorio posao te ih prikazati. U ovom primjeru osnovne postoji samo jedne datoteke, no ako nema više ta naredba činite ponavljanje ih sve.

    Rezultat je slična sljedećoj:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Svaki redak predstavlja riječi i koliko je puta je došlo je do ulaznih podataka.

## <a name="sudoku"></a>Sudoku

Primjer Sudoku sadrži upute za korištenje Pomalo beskorisno; "Obuhvaćaju slagalica u naredbenom retku".

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) je logike slagalica sastoji od devet 3 x 3 rešetke. Neke ćelije u rešetki sadrži brojeve, dok su prazni drugima, a cilj je riješiti za prazne ćelije. Gornje veze sadrži dodatne informacije o na slagalica, no Svrha ovaj uzorak je riješiti za prazne ćelije. Pa naš unesena vrijednost mora biti datoteku koja je u sljedećem obliku:

- Devet redaka devet stupaca

- Svaki se stupac može sadržavati neki broj ili `?` (koji označava prazna ćelija)

- Ćelije su razdvojena razmakom

Postoji određeni način za sastavljanje i Sudoku; nije moguće ponovite broja u stupcu ili retku. Postoji primjer klaster HDInsight koji pravilno konstruirana. Nalazi se na **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** i sadrži sljedeće:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] Na `2.2.4.9-1` dio puta može promijeniti kako ažuriranja vrše klaster HDInsight.

Da biste pokrenuli to kroz Sudoku primjera, koristite sljedeću naredbu:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Rezultati trebao izgledati otprilike ovako:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>Pi (π)

Ogledna pi koristi u statističke (postupak SMS Karlo) metoda za procjenu vrijednost broja pi. Točke nasumično smješten unutar jedinica kvadratni i ulaze unutar kruga inscribed unutar kvadrat vjerojatnost jednako područje kruga, pi/4. Od vrijednosti 4R, pri čemu je R omjer broj točaka koje su unutar kruga ukupan broj točaka koje se nalaze u kvadrat možete se Procijenjena vrijednost broja pi. Veća od točaka koje se koriste, ćete bolju procijenjenu vrijednost je uzorak.

Mapiranje ovaj primjer generira broj točaka nasumično unutar kvadrat jedinica i broji broj točaka koje su unutar kruga.

Na reducer zatim akumulira točke broji prema na mappers i procjenjuje vrijednost broja pi iz formule 4R, pri čemu je R omjer broj točaka broje se unutar kruga ukupan broj točaka koje se nalaze u kvadrat.

Koristite sljedeću naredbu da biste pokrenuli ovaj uzorak. To koristi 16 karte 10,000,000 uzorka za procjenu vrijednost broja pi:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

To je vratio vrijednost mora biti sličan **3.14159155000000000000**. Reference, prvih 10 decimalnih mjesta pi su 3.1415926535.

##<a name="10gb-greysort"></a>10GB Greysort

GraySort je sortiranje usporednih je čije metrika sortiranje stopa (TB na minuti) koja se postiže prilikom sortiranja vrlo velike količine podataka, obično na 100TB minimalne.

Ovaj primjer koristi modest 10GB podataka tako da ga možete pokrenuti relativno brzo. Koristi MapReduce aplikacije razvio Owen O'Malley i Arun Murthy koje su osvojile godišnje usporednih sortiranje terabajta općenite namjene ("daytona") u 2009 s rata 0.578 TB/min (100 TB 173 minuta). Dodatne informacije u ovom i drugim sortiranja jednonitnih potražite u članku [Sortbenchmark](http://sortbenchmark.org/) web-mjesta.

Ovaj primjer koristi tri skupa MapReduce programe:

- **TeraGen**: program MapReduce koje generira retke s podacima za sortiranje

- **TeraSort**: uzoraka ulaznih podataka i koristi MapReduce da biste sortirali podatke s potpunim

    TeraSort je standardni sortiranje funkcija MapReduce, osim prilagođenih partitioner koji koristi sortirani popis N-1 uzorkovanja prečaci koji se definirati ključne raspon za svaki smanjivanje. Sve prečaci takve koji uzorak [i-1] posebice u < = ključ < uzorka [i] šalju da biste smanjili i. To su jamstva izlaze od smanjite je i sve manji od izlaz smanjivanje i + 1.

- **TeraValidate**: MapReduce programa koji provjerava se globalno sortira Izlaz

    Stvara jedne mape po datoteci u direktoriju izlazne i svako mapiranje osigurava svaki ključ manja od ili jednaka na prethodni slajd. Funkcija karte generira i zapisa tipki imena i prezimena svake datoteke, a funkcija smanjivanje osigurava prvog ključa datoteke i veće od zadnjeg ključ i-1 datoteke. Probleme su permitted na Izlaz u smanjivanje tipke koje su izvan redoslijeda.

Koristite sljedeće korake da biste generirali podatke, sortirati i provjeriti valjanost izlaz:

1. Generiranje 10GB podataka koje će se spremiti HDInsight klaster zadani spremište na **wasbs: / / / primjer / / 10GB-sortiranje-unosa podataka**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    U `-Dmapred.map.tasks` Hadoop obavještava koliko zadaci karte da biste koristili za ovaj zadatak. Konačni dva parametra uputiti zadatak da biste stvorili 10GB vrijednosti podataka i spremite ga na **wasbs: / / / primjer / / 10GB-sortiranje-unosa podataka**.

2. Da biste sortirali podatke, koristite sljedeću naredbu:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    Na `-Dmapred.reduce.tasks` govori Hadoop koliko smanjiti zadataka da biste koristili za posao. Konačni dva parametra su samo ulazni i izlazni mjesta za podatke.

3. Za provjeru valjanosti podataka koji je generirao sortiranje, koristite sljedeće:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Daljnji koraci ##

U ovom se članku naučili kako pokrenuti uzoraka u sklopu klastere sustavom Linux HDInsight. Vodiči za o Svinja, grozd i MapReduce pomoću servisa HDInsight potražite u sljedećim temama:

* [Korištenje Svinja s Hadoop na HDInsight][hdinsight-use-pig]
* [Korištenje grozd s Hadoop na HDInsight][hdinsight-use-hive]
* [Korištenje MapReduce s Hadoop na HDInsight] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
