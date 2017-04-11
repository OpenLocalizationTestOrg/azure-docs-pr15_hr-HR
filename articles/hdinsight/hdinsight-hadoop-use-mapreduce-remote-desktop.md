<properties
   pageTitle="MapReduce i udaljenu radnu površinu s Hadoop u HDInsight | Microsoft Azure"
   description="Saznajte kako koristiti udaljenu radnu površinu za povezivanje s Hadoop na HDInsight i pokretanje MapReduce poslove."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Korištenje MapReduce u Hadoop na HDInsight putem udaljene radne površine

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

U ovom se članku će Saznajte kako se povezati s Hadoop na HDInsight klaster putem udaljene radne površine i pokrenite MapReduce zadatke pomoću naredbe "Hadoop".

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Klaster utemeljen na sustavu Windows HDInsight (Hadoop na HDInsight)

* Klijentskog računala sa sustavom Windows 10, Windows 8 ili Windows 7

##<a id="connect"></a>Povezivanje s udaljene radne površine

Omogućivanje udaljene radne površine za HDInsight klaster, a zatim s njim povezati slijedeći upute u [klastere HDInsight pomoću RDP za povezivanje](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hadoop"></a>Koristite naredbu Hadoop

Kad ste spojeni na radnu površinu za HDInsight klaster, poduzmite sljedeće korake da biste pokrenuli MapReduce posla pomoću naredbe Hadoop:

1. Na radnoj površini HDInsight početnom **Hadoop naredbenog retka**. Otvorit će se novi naredbeni redak u u **c:\apps\dist\hadoop-&lt;broj verzije >** direktorija.

    > [AZURE.NOTE] Broj verzije mijenja kako Hadoop ažurira. Varijabla okruženja **HADOOP_HOME** se može koristiti za traženje put. Na primjer, `cd %HADOOP_HOME%` mijenja direktorija u direktoriju Hadoop bez potrebe da znate broj verzije.

2. Da biste koristili naredbu **Hadoop** pokretanje programa primjer MapReduce posla, koristite sljedeću naredbu:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Time ćete klase **wordcount** koji je sadržan u datoteci **hadoop mapreduce examples.jar** u trenutnom direktoriju. Kao ulaz, koristi **wasbs://example/data/gutenberg/davinci.txt** dokument i izlazni pohranjuju se na: **wasbs: / / / primjer/podataka/WordCountOutput**.

    > [AZURE.NOTE] Dodatne informacije o posao MapReduce i oglednim podacima potražite u članku <a href="hdinsight-use-mapreduce.md">Korištenje MapReduce u HDInsight Hadoop</a>.

2. Posao emits detalje kao ona se obrađuje i vraća informacije o sličnu ovoj nakon dovršetka posla:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Nakon dovršetka posla, koristite sljedeću naredbu da biste dobili popis Izlazna datoteka pohranjenih na **wasbs://example/data/WordCountOutput**:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Time se treba prikazati dvije datoteke, **_SUCCESS** i **dio-r-00000**. **Dio r 00000** datoteka sadrži izlaz za taj zadatak.

    > [AZURE.NOTE] Neke zadatke MapReduce možda Podijeli rezultate preko više **dijelova r ###** datoteka. Ako je tako, koristite na ### sufiks da biste odredili redoslijed datoteka.

4. Da biste pogledali Izlaz, koristite sljedeću naredbu:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Prikazat će se popis riječi koje se nalaze u datoteci **wasbs://example/data/gutenberg/davinci.txt** , zajedno s brojem puta došlo je do svake riječi. Slijedi primjer podataka koji će se nalaziti u datoteku:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Sažetak

Kao što možete vidjeti naredbu Hadoop omogućuje jednostavno pokrenuti MapReduce zadatke programa HDInsight klaster, a zatim pregledavati izlaz posao.

##<a id="nextsteps"></a>Daljnji koraci

Opće informacije o zadacima MapReduce u HDInsight:

* [Korištenje MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)
