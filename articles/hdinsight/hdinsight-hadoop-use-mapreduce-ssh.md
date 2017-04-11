<properties
   pageTitle="MapReduce i SSH veze sa Hadoop u HDInsight | Microsoft Azure"
   description="Saznajte kako koristiti SSH da biste pokrenuli MapReduce zadatke pomoću Hadoop na HDInsight."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Korištenje MapReduce s Hadoop na HDInsight s SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

U ovom se članku će Saznajte kako koristiti sigurne ljuske (SSH) za povezivanje s Hadoop na HDInsight klaster i zatim poslati MapReduce zadatke pomoću naredbe Hadoop.

> [AZURE.NOTE] Ako ste već upoznati s komponentom sustavom Linux Hadoop poslužiteljima, ali ste novi korisnik servisa HDInsight, pročitajte članak [Savjeti sustavom Linux HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Klaster sustavom Linux HDInsight (Hadoop na HDInsight)

* Klijent za SSH. Operacijski sustavi Linux, Unix i Mac mora obuhvaćati klijent za SSH. Korisnici sustava Windows morate preuzeti klijent, kao što su [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Povezivanje s SSH

Povezivanje s na potpuno kvalificirani naziv domene (FQDN) od svoj klaster HDInsight pomoću naredbe "SSH". FQDN bit će dao klaster, nakon čega slijedi naziv **. azurehdinsight.net**. Na primjer, sljedeće želite povezati klaster pod nazivom **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Ako ste unijeli ključ certifikat za provjeru autentičnosti SSH** stvaranja HDInsight klaster, možda ćete morati navedite mjesto privatni ključ na klijentskim sustavima, na primjer:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Ako upišete lozinku za provjeru autentičnosti SSH** stvaranja HDInsight klaster, morat ćete unijeti lozinku kad se to od vas zatraži.

Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>PuTTY (Windows klijenti)

Windows ne nudi ugrađene SSH klijenta. Preporučujemo korištenje **PuTTY**, koji možete preuzeti s [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Dodatne informacije o korištenju PuTTY potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Korištenje naredbi za Hadoop

1. Kada ste povezani s HDInsight klaster, koristite sljedeću naredbu **Hadoop** pokretanje MapReduce posao:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Time ćete klase **wordcount** koji je sadržan u datoteci **hadoop mapreduce examples.jar** . Kao ulaz, koristi **wasbs://example/data/gutenberg/davinci.txt** dokument i izlazni pohranjuju se na **wasbs: / / / primjer/podataka/WordCountOutput**.

    > [AZURE.NOTE] Dodatne informacije o posao MapReduce i oglednim podacima potražite u članku [Korištenje MapReduce u Hadoop na HDInsight](hdinsight-use-mapreduce.md).

2. Posao emits pojedinosti obrađuje i vraća informacije o sličnu ovoj nakon dovršetka posla:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Nakon dovršetka posla, koristite sljedeću naredbu da biste dobili popis Izlazna datoteka pohranjenih na **wasbs://example/data/WordCountOutput**:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Time se treba prikazati dvije datoteke, **_SUCCESS** i **dio-r-00000**. **Dio r 00000** datoteka sadrži izlaz za taj zadatak.

    > [AZURE.NOTE] Neke zadatke MapReduce možda Podijeli rezultate preko više **dijelova r ###** datoteka. Ako je tako, koristite na ### sufiks da biste odredili redoslijed datoteka.

4. Da biste pogledali Izlaz, koristite sljedeću naredbu:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Prikazat će se popis riječi koje se nalaze u datoteci **wasbs://example/data/gutenberg/davinci.txt** i koliko je puta došlo je do svake riječi. Slijedi primjer podataka koji će se nalaziti u datoteku:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Sažetak

Kao što možete vidjeti naredbe Hadoop pružaju jednostavan način da biste pokrenuli MapReduce zadatke programa klasteru HDInsight i prikaz izlaz posao.

##<a id="nextsteps"></a>Daljnji koraci

Opće informacije o zadacima MapReduce u HDInsight:

* [Korištenje MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)
