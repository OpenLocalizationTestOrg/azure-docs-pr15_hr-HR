<properties
   pageTitle="MapReduce s Hadoop na HDInsight | Microsoft Azure"
   description="Upute za izvođenje zadataka MapReduce Hadoop u klastere HDInsight. Ćete pokrenuti postupak count osnovni word kao Java MapReduce posao."
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

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Korištenje MapReduce u Hadoop na HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

U ovom se članku će Saznajte kako izvršiti MapReduce poslove Hadoop u klastere HDInsight. Ne možemo pokrenuti postupak count osnovni word kao Java MapReduce posao.

##<a id="whatis"></a>Što je MapReduce?

Hadoop MapReduce je framework softver za pisanje poslove koji obrađivati veliku količine podataka. Neovisno blokova koji se zatim obrađuju paralelno preko čvorove u svoj klaster je podijelite ulaznih podataka. Posao MapReduce se sastojati od dvije funkcije:

* **Mapiranje**: troši ulaznih podataka, analizira (obično s filtriranje i sortiranje operacije) i emits redni (parovima ključnih vrijednosti)
* **Reducer**: troši redni čuje tako da na mapiranje i izvodi sažetak postupak koji stvara manje, kombinirane rezultat iz mapiranje podataka

Osnovni word primjer count MapReduce posao je što je prikazano na sljedećem su dijagramu:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Izlaz iz ovaj zadatak je broj koliko je puta došlo je do svake riječi u tekstu koji je analizirati.

* Na mapiranje uzima svakog retka iz unosa teksta kao ulaz i dijeli na riječi. Ga emits ključa/vrijednosti par svaki put kada se pojavljuje riječ riječi slijedi a 1. Prije no što pošaljete reducer su sortirani izlaz.

* Na reducer zbraja te pojedinačne broji za svaku riječ, a emits paru jedna ključ/vrijednost koja sadrži riječ slijedi zbroj njegov pojavljivanja.

MapReduce se mogu primijeniti na različitim jezicima. Java je najčešće implementacija i koristi svrhe pokazni u ovom dokumentu.

### <a name="hadoop-streaming"></a>Hadoop strujanje

Jezike ili okviri koji se temelje na Java i Java virtualni stroj (na primjer, Scalding ili kaskadnog,) možete pokrenuli izravno kao posao MapReduce, slično Java aplikacije. Drugim korisnicima, kao što su C# ili Python ili samostalnu izvršne datoteke, morate koristiti Hadoop strujanje.

Hadoop strujanje komunicira s mapiranje i reducer nad STDIN i STDOUT - mapiranje i reducer pročitajte podataka redak istovremeno STDIN i pisati izlaz STDOUT. Svaki redak čitati ili čuje mapiranje i reducer mora biti u obliku par ključa vrijednosti, razgraničeno tako da kartica charaacter:

    [key]/t[value]

Dodatne informacije potražite u članku [Hadoop strujanje](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Primjeri korištenja Hadoop strujanje HDInsight potražite u sljedećim člancima:

* [Razvoj Python MapReduce poslova](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>O oglednim podacima

U ovom primjeru za uzorak podataka će koristiti bilježnica Leonardo Da Vinci, koji se prikazuje se kao tekst dokumenta u svoj klaster HDInsight.

Ogledni podaci pohranjena u spremište blobova platforme Azure, što HDInsight koristi kao zadani datotečni sustav za klastere Hadoop. HDInsight možete pristupiti datotekama pohranjenima u spremište blobova platforme pomoću prefiks **wasb** . Na primjer, da biste pristupili sample.log datoteku, služite sljedeću sintaksu:

    wasbs:///example/data/gutenberg/davinci.txt

Budući da je spremište blobova platforme Azure zadani prostor za pohranu za HDInsight, datoteku možete pristupiti i pomoću **/example/data/gutenberg/davinci.txt**.

> [AZURE.NOTE] U sintaksi prethodne **wasbs: / / /** koristi se za pristup datotekama koje su pohranjene u spremniku zadani prostor za pohranu za svoj klaster HDInsight. Ako dodatni prostor za pohranu računi koje ste naveli pri dodjeli svoj klaster, a želite pristupiti datotekama pohranjenima u tih računa, možete pristupiti podatke navođenjem spremnik naziv i pohranu računa adresu. Na primjer, **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>O primjer MapReduce

MapReduce zadatak koji se koristi u ovom primjeru nalazi se na **wasbs://example/jars/hadoop-mapreduce-examples.jar**pa je dao svoj klaster HDInsight. Primjer Brojanje riječi koji će se izvoditi na temelju **davinci.txt**sadrži.

> [AZURE.NOTE] Na klastere HDInsight 2.1 **wasbs:///example/jars/hadoop-examples.jar**je mjesto datoteke.

Za referencu, sljedeći kod Java za posao MapReduce Brojanje riječi:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

Upute za pisanje MapReduce posla, potražite u članku [razviti MapReduce Java programa za HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>Pokretanje sustava MapReduce

HDInsight mogu se izvoditi HiveQL zadatke pomoću na različite načine. U sljedećoj su tablici pomoću odlučiti koju metodu vam najviše odgovara, a zatim slijedite vezu vodič.

| **Tom se mogućnošću poslužite**...                                                    | **..) (.da biste to učinili**                                       | .. .with **klaster operacijski sustav** | .. .from taj **klijent operacijski sustav** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Koristite naredbu Hadoop kroz **SSH**                  | Linux                                     | Linux, Unix, Mac OS X ili Windows        |
| [Zakretanja](hdinsight-hadoop-use-mapreduce-curl.md)                     | Slanje zadatka daljinski pomoću **REST**               | Linux ili Windows                          | Linux, Unix, Mac OS X ili Windows        |
| [Komponente Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | Slanje zadatka daljinski pomoću **Komponente Windows PowerShell** | Linux ili Windows                          | Windows                                  |
| [Udaljena radna površina](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Koristite naredbu Hadoop putem **Udaljene radne površine**       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Daljnji koraci

Iako MapReduce nudi naprednih mogućnosti dijagnostičkih, može se malo zahtjevne matrice. Postoji nekoliko utemeljena na okviri koje olakšavaju definiranje MapReduce aplikacija, kao i tehnologije kao što su Svinja i grozd, čime se omogućuje jednostavniji način za rad s podacima u HDInsight. Dodatne informacije potražite u sljedećim člancima:

* [Razvoj Java MapReduce programe za HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Razvoj Python strujanje MapReduce programe za HDInsight](hdinsight-hadoop-streaming-python.md)

* [Razvoj Scalding MapReduce poslove s Apache Hadoop na HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Korištenje grozd s HDInsight][hdinsight-use-hive]

* [Korištenje Svinja s HDInsight][hdinsight-use-pig]

* [Pokretanje servisa HDInsight uzorka][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
