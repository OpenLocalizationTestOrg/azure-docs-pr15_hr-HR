<properties
   pageTitle="Razvoj Python MapReduce poslove s HDInsight | Microsoft Azure"
   description="Saznajte kako stvoriti i pokrenuti Python MapReduce poslove klastere sustavom Linux HDInsight."
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

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Razvoj Python strujanje programe za HDInsight

Hadoop nudi strujanje API-JA za MapReduce koji omogućuje pisanje karte i smanjivanje funkcije na jezicima koji nisu Java. U ovom se članku će Saznajte kako koristiti Python za izvođenje operacija MapReduce.

> [AZURE.NOTE] Dok Python kod u ovom dokumentu mogu se koristiti s utemeljen na sustavu Windows HDInsight klaster, specifične za klastere sustavom Linux su koraci u ovom dokumentu.

U ovom se članku temelji se na informacije i primjeri objavljuje Goran Noll pri [programa MapReduce Hadoop u Python za pisanje](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/).

##<a name="prerequisites"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Hadoop Linux utemeljen na HDInsight klaster

* Uređivač teksta

    > [AZURE.IMPORTANT] Uređivač teksta morate koristiti LF završetak crte. Ako je riječ o CRLF koristi, to će uzrokovati pogreške prilikom pokretanja posla MapReduce na klastere sustavom Linux HDInsight. Ako niste sigurni, koristite neobavezan korak u odjeljku [Pokretanje MapReduce](#run-mapreduce) da biste pretvorili bilo koju riječ o CRLF LF.

* Za klijente sustava Windows, PuTTY i PSCP. Ove uslužni programi su dostupne sa <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Stranice za preuzimanje PuTTY</a>.

##<a name="word-count"></a>Brojanje riječi

U ovom primjeru će implementirati osnovni riječi, koristeći mapiranje i reducer. Na mapiranje dijeli rečenica na pojedinačne riječi, a na reducer objedinjuje riječi i broji čime se dobiva rezultat.

Sljedeća slika prikazuje što se događa tijekom karte i smanjiti faze.

![Smanjivanje slika karte](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Zašto Python?

Python je općenite namjene, više razine programski jezik koji vam omogućuje eksplicitnih koncepte manje redaka koda od mnoge druge jezike. Ga je nedavno postao popularne s fizičari podataka kao jezik za prototyping jer njegova interpretiraju prirode, dinamični pisati i elegantna sintaksa biti prikladna za brz razvoj aplikacija.

Python je instaliran na sve klastere HDInsight.

##<a name="streaming-mapreduce"></a>Strujanje MapReduce

Hadoop omogućuje vam da biste odredili datoteku koja sadrži kartu i smanjili logiku koja se koristi za posao. Zahtjeve specifične za kartu i smanjiti logike su:

* **Unos**: karte i smanjiti količinu komponente moraju pročitati ulazne podatke iz STDIN.

* **Izlaz**: karte i smanjiti količinu komponente morate napisati izlazne podatke STDOUT.

* **Oblikovanje podataka**: podaci potrošena te proizvesti moraju biti paru ključa vrijednosti odvojene znak tabulatora.

Python možete jednostavno ručicu za tim preduvjetima pomoću modula **sistemskim** čitanje STDIN i korištenjem **Ispis** da biste ispisali STDOUT. Preostali zadatka samo oblikovanje podataka pomoću kartice (`\t`) znakova između ključ i vrijednost.

##<a name="create-the-mapper-and-reducer"></a>Stvaranje mapiranje i reducer

Mapiranje i reducer su tekstne datoteke, u ovom slučaju **mapper.py** i **reducer.py** (da biste ga poništite koji ne što). Možete stvoriti ih pomoću uređivača po izboru.

###<a name="mapperpy"></a>Mapper.PY

Stvorite novu datoteku pod nazivom **mapper.py** i koristiti sljedeći kod kao sadržaja:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Potrajati koji trenutak da biste pročitali putem koda tako da mogu razumjeti funkcija.

###<a name="reducerpy"></a>Reducer.PY

Stvorite novu datoteku pod nazivom **reducer.py** i koristiti sljedeći kod kao sadržaja:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Prijenos datoteka

**Mapper.py** i **reducer.py** moraju biti na glavni čvor klaster smo mogli pokrenuti. Da biste ih prenijeli najjednostavnije da biste koristili **pronađenim** (**pscp** ako koristite Windows klijent).

Putem klijentskog programa u imeniku isti kao **mapper.py** i **reducer.py**, koristite sljedeću naredbu. Zamijenite **korisničko ime** korisnika SSH i **clustername** pod nazivom svoj klaster.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

To čvor glavni kopira datoteka s lokalnog sustava.

> [AZURE.NOTE] Ako ste koristili lozinku radi zaštite računa SSH, zatražit će se za lozinku. Ako ste koristili ključa SSH, možda ćete morati koristiti u `-i` parametar i put do privatni ključ, na primjer, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>Pokretanje MapReduce

1. Povezivanje s klaster pomoću SSH:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Ako ste koristili lozinku radi zaštite računa SSH, zatražit će se za lozinku. Ako ste koristili ključa SSH, možda ćete morati koristiti u `-i` parametar i put do privatni ključ, na primjer, `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Neobavezno) Ako ste koristili uređivaču teksta koji se koristi riječ o CRLF kao u retku koji završava pri stvaranju datoteke mapper.py i reducer.py ili ne znate što završetak crte uređivač koristi, koristite sljedeće naredbe za pretvaranje pojavljivanja riječ o CRLF u mapper.py i reducer.py LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Da biste pokrenuli MapReduce zadatak, koristite sljedeću naredbu.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Ta se naredba sastoji se od sljedećih dijelova:

    * **hadoop streaming.jar**: koristi prilikom izvršavanja strujanje MapReduce operacije. Hadoop je sučelja kod vanjski MapReduce pružate.

    * **-datoteke**: govori Hadoop koje su navedene datoteke potrebne za taj zadatak MapReduce i trebaju biti kopirane na sve čvorove tempiranja.

    * **– Mapiranje**: govori Hadoop datoteku koju želite koristiti kao na mapiranje.

    * **-reducer**: govori Hadoop datoteku koju želite koristiti kao u reducer.

    * **-unos**: Ulazna datoteka koje ćemo treba Brojanje riječi iz.

    * **-Izlaz**: direktorij koji će biti zapisane izlaz.

        > [AZURE.NOTE] Taj imenik stvorit će se posla.

Trebali biste vidjeti skup izjave **informacije** kao pokretanja posla, a na kraju postupka **kartu** i **smanjivanje** prikazuju kao postotaka potražite u članku.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Primit ćete informacije o statusu o zadatka kada se dovrši.

##<a name="view-the-output"></a>Prikaz Izlaz

Nakon dovršetka posla, koristite sljedeću naredbu da biste vidjeli rezultat:

    hdfs dfs -text /example/wordcountout/part-00000

To će prikazati popis došlo je do za riječi i koliko vremena u programu word. Ovo je programa ogledne podatke:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako koristiti strujanje MapRedcue poslove s HDInsight, koristite sljedeće veze da biste istražili druge načine za rad sa servisom Azure HDInsight.

* [Korištenje grozd s HDInsight](hdinsight-use-hive.md)
* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)
* [Korištenje MapReduce poslove s HDInsight](hdinsight-use-mapreduce.md)
