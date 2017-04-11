<properties
    pageTitle="Generiranje preporuke pomoću Mahout i sustavom Linux HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti Apache Mahout strojnog učenja biblioteke da biste generirali preporuke film s operacijskim sustavom Linux HDInsight (Hadoop)."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Generiranje film preporuke pomoću Apache Mahout s operacijskim sustavom Linux Hadoop u HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Saznajte kako koristiti [Apache Mahout](http://mahout.apache.org) strojnog učenja biblioteka sa servisom Azure HDInsight da biste generirali film preporuke.

Mahout je na [računalu učenje] [ ml] biblioteka za Apache Hadoop. Mahout sadrži algoritama za obradu podataka, kao što su filtriranja, klasifikacija, i Klasteriranje. U ovom se članku će koristiti modul za preporuke za generiranje preporuke film koji se temelje na filmova pojavljuje prijateljima.

> [AZURE.NOTE] Koraci u ovom dokumentu zahtijevaju Hadoop Linux utemeljen na klasteru HDInsight. Informacije o korištenju Mahout s klaster utemeljen na sustavu Windows potražite u članku [Generiraj film preporuke pomoću Mahout Apache s utemeljen na sustavu Windows Hadoop u HDInsight](hdinsight-mahout.md)

##<a name="prerequisites"></a>Preduvjeti

* U operacijskim sustavom Linux Hadoop na klasteru HDInsight. Informacije o stvaranju nešto potražite u članku [Početak rada s operacijskim sustavom Linux Hadoop u HDInsight][getstarted]

##<a name="mahout-versioning"></a>Rad s verzijama mahout

Dodatne informacije o verziji Mahout uključene svoj klaster servisa HDInsight potražite u članku [HDInsight verzije i Hadoop komponente](hdinsight-component-versioning.md).

> [AZURE.WARNING] Dok je moguće prenijeti neku drugu verziju programa Mahout HDInsight klaster, samo komponente dao klaster HDInsight su u potpunosti podržane, a Microsoft Support pomoći će vam da biste izdvojili i rješavanje problema vezanih uz te komponente.
>
> Prilagođene komponente dobili komercijalno pametnije podršku radi daljnje rješavanje problema. To može rezultirati rješavanju problema ili s pitanjem želite li sudjelovati dostupnih kanala tehnologija Otvori izvor gdje se nalazi niže stručna znanja za taj tehnologiju. Na primjer, postoje mnogo web-mjesta zajednice koje je moguće koristiti, npr.: [MSDN forum za HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekti Apache imaju web-mjesta projekta na [http://apache.org](http://apache.org), na primjer: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

##<a name="recommendations"></a>Razumijevanje preporuke

Jedna od funkcija koju je dodijelio Mahout je preporuka modul. Ovaj modul prihvaća podataka u obliku `userID`, `itemId`, i `prefValue` (preferencama korisnika za stavku). Mahout možete izvedite analize zajednički occurance da biste odredili: _korisnicima koji imaju preferenca stavke su i preference za te stavke_. Mahout zatim određuje korisnika završavaju na preference poput stavke, koji se može koristiti za preporuke.

Slijedi primjer iznimno jednostavne koji koristi filmova:

* __Suautorstvo occurance__: Joe, Alice i Teo sviđa _Wars zvjezdica_, _U Empire Strikes natrag_i _Povratna od na Jedi_. Mahout određuje da korisnika i kao što su neki od ovih filmova kao što su dvije.

* __Suautorstvo occurance__: Teo i Alice i sviđa _U Phantom Menace_, _napada na Clones_i _Revenge u Sith_. Mahout određuje da korisnici kojima se sviđa prethodna tri filmova i vam se sviđa među njima.

* __Sličnosti preporuka__: jer Joe sviđa prva tri filmova, Mahout razmatra filmova taj drugi korisnici s slične preference sviđa, ali Joe sadrži nadzirane (sviđa/ocjena). U ovom slučaju Mahout preporučuje _U Phantom Menace_, _napada na Clones_i _Revenge u Sith_.

###<a name="understanding-the-data"></a>Razumijevanje podataka

Jednostavnijeg [Istraživanje GroupLens] [ movielens] pruža podatke ocjena filmova u obliku koji je kompatibilan s Mahout. Tih podataka dostupna je na svoj klaster zadani pohranu pri `/HdiSamples/HdiSamples/MahoutMovieData`.

Postoje dvije datoteke `moviedb.txt` (informacije o filmovi,) i `user-ratings.txt`. Korisnik ratings.txt datoteka koristi tijekom analize, dok moviedb.txt koristi radi infromation jednostavnih tekst prilikom prikaza rezultata analizu.

Podatke sadržani u korisnika ratings.txt sadrži strukture `userID`, `movieID`, `userRating`, i `timestamp`, koje nam govore kako se svaki korisnik s ocjenom film. Evo primjera podataka:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Pokrenuti analizu

Da biste pokrenuli preporuke zadatak, koristite sljedeću naredbu:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] Posao može potrajati nekoliko minuta da biste dovršili, i pokrenuti više MapReduce zadataka.

##<a name="view-the-output"></a>Prikaz Izlaz

1. Nakon dovršetka posla, koristite sljedeću naredbu da biste pogledali generirani izlaz:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Izlaz pojavit će se na sljedeći način:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Prvi stupac na `userID`. Vrijednosti koji se nalazi u ' [' i '] "su `movieId`:`recommendationScore`.

2. Izlaz uz moviedb.txt, možete koristiti da biste prikazali dodatne informacije o korisniku neslužbeni. Najprije ćemo treba da biste kopirali datoteke lokalno pomoću sljedeće naredbe:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Time ćete kopirati podatke u datoteku pod nazivom **recommendations.txt** u trenutnog direktorija uz filmske datoteke podataka.

3. Da biste stvorili novu Python skriptu koja će potražiti film naziva za izlaz preporuke, koristite sljedeću naredbu:

        nano show_recommendations.py

    Kad se otvori uređivaču, koristite sljedeće kao sadržaj datoteke:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Pritisnite **CTRL + X**, **Y**i na kraju **Enter** da biste spremili podatke.

3. Da biste izvršnu datoteku, koristite sljedeću naredbu:

        chmod +x show_recommendations.py

4. Pokrenuti skriptu Python. Sljedeće podrazumijeva da vam je u direktoriju u kojem su sve datoteke preuzete:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    To će pogledajte preporuke za korisničkog ID-a 4.

    * Datoteka **korisnika ratings.txt** koristi za dohvaćanje filmovima koji je korisnik ocjenjivanje
    * Datoteka **moviedb.txt** koristi za dohvaćanje imena filmova
    * **Recommendations.txt** se koristi za dohvaćanje film preporuke za tog korisnika

    Izlaz iz ta naredba bit će otprilike ovako:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Brisanje privremenih podataka

Mahout poslove uklanjanje privremene podataka koji je stvoren tijekom obrade posao. Na `--tempDir` parametar je naveden u posao primjer izdvojiti privremene datoteke u određenom putu za jednostavno brisanja. Da biste uklonili privremene datoteke, koristite sljedeću naredbu:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Ako želite ponovno pokrenite naredbu, morate izbrisati i izlaznog direktorija. Da biste izbrisali taj imenik, koristite sljedeće:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako koristiti Mahout, Uvod u rad s podacima na HDInsight drugi načini:

* [Vrste Hive s HDInsight](hdinsight-use-hive.md)
* [Svinja s HDInsight](hdinsight-use-pig.md)
* [MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
