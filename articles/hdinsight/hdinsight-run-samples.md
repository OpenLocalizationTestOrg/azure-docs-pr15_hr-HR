<properties
    pageTitle="Uzorci Hadoop pokretanje u HDInsight | Microsoft Azure"
    description="Početak rada pomoću servisa Azure HDInsight uzorcima navedene. Pomoću komponente PowerShell skripte koje programe MapReduce na klastere podataka."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Pokretanje Hadoop MapReduce uzoraka u HDInsight utemeljen na sustavu Windows

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Skup uzoraka dostupni su vam pomoći da započnete rada izvodi MapReduce zadatke na klastere Hadoop pomoću servisa Azure HDInsight. Ta uzorka postaju dostupne na svim HDInsight upravlja klastere koje ste stvorili. Pokretanje ta uzorka, koji će se Upoznajte s pomoću cmdleta ljuske PowerShell Azure da biste pokrenuli zadatke za klastere Hadoop.

- [**Brojanje riječi**][hdinsight-sample-wordcount]: broji pojavljivanja riječi u tekstnoj datoteci.
- [**C# strujanje riječi**][hdinsight-sample-csharp-streaming]: broji pojavljivanja riječi u tekstnoj datoteci putem sučelja strujanje Hadoop.
- [**Pi estimator**][hdinsight-sample-pi-estimator]: koristi u statističke (postupak SMS Karlo) metoda za procjenu vrijednost broja pi.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: pokrenite opće namjene GraySort na datoteku 10 GB pomoću HDInsight. Postoje tri zadataka da biste pokrenuli: Teragen da biste generirali podatke, Terasort da biste sortirali podatke, a zatim Teravalidate da biste potvrdili da se podaci su se pravilno sortirali.

>[AZURE.NOTE] Izvorni kod pronaći ćete u dodatak. 

Postoji mnogo dodatna dokumentacija na webu za Hadoop povezane tehnologije, kao što su utemeljena na MapReduce programiranje i strujanje i dokumentaciju o cmdletima koji se koriste u ljusci Windows PowerShell skriptiranje. Dodatne informacije o sljedećim resursima potražite u članku:

- [Razvoj Java MapReduce programe za Hadoop u HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Slanje Hadoop zadataka u HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Uvod u Azure HDInsight][hdinsight-introduction]

Danas, mnogo osobe odaberite grozd i Svinja iznad MapReduce.  Dodatne informacije potražite u članku:

- [Korištenje grozd u HDInsight](hdinsight-use-hive.md)
- [Korištenje Svinja u HDInsight](hdinsight-use-pig.md)
 
**Preduvjeti**:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **programa klaster HDInsight**. Upute na različite načine u kojem je moguće stvoriti takve klastere potražite u članku [Stvaranje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md).
- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Word count - Java 

Da biste poslali MapReduce projekta, najprije stvorite definiciju posla MapReduce. U definiciji posla navedite MapReduce programske posudu datoteke i mjesto datoteke posudu koji je **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, naziv klase i argumenata.  Wordcount MapReduce program traži dva argumenta: izvornu datoteku koja će se koristiti za Brojanje riječi i mjesto za izlaz.

Izvorni kod pronaći ćete u [dodatak A](#apendix-a---the-word-count-MapReduce-program-in-java).

Za postupak za razvoj programa Java MapReduce, potražite u članku - [razviti MapReduce Java programa za Hadoop u HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**Da biste poslali posao MapReduce Brojanje riječi**

1. Otvorite **Windows Očisti filtar**. Upute potražite u članku [instalirati i konfigurirati Azure PowerShell][powershell-install-configure].
2. Zalijepite sljedeću skriptu komponente PowerShell:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    Zadatak MapReduce stvara datoteku pod nazivom *dio-r-00000*, koji sadrži riječi i na broji. Skripta koristi naredbu **findstr** da biste dobili popis svih riječi koja sadrži *"nema"*.

3. Postaviti prvo 3 varijable i pokrenuti skriptu.

## <a name="hdinsight-sample-csharp-streaming"></a>Word count - C# strujanje

Hadoop daje strujanje API MapReduce, koji omogućuje pisanje karte i smanjivanje funkcije na jezicima koji nisu Java.

> [AZURE.NOTE] Koraci u ovom ćete praktičnom vodiču primjenjuju se samo na klastere HDInsight utemeljen na sustavu Windows. Primjer strujanja za klastere sustavom Linux HDInsight potražite u članku [razviti Python strujanje programa za HDInsight](hdinsight-hadoop-streaming-python.md).

U primjeru u mapiranje i na reducer su izvršne datoteke koje čitaju unos iz [stdin] [ stdin-stdout-stderr] (redak po redak) i šalji Izlaz u [stdout][stdin-stdout-stderr]. Program broji sve riječi u tekstu.

Kada je za **mappers**navedena izvršne datoteke, svaki zadatak mapiranje pokreće izvršnu datoteku kao drugi proces kada je pokrenut na mapiranje. Kao što je zadatak mapiranje pokreće, ga pretvara njegov unos u retke te sažeci sadržaja u recima [stdin] [ stdin-stdout-stderr] postupka.

U aplikacijom na mapiranje prikuplja vezanima uz redak Izlaz iz stdout postupka. Ga pretvara svaki redak u paru ključa vrijednosti koji se prikupljaju se kao rezultat u mapiranje. Prema zadanim postavkama prefiks crte najviše prvi znak tabulatora ključ, a ostatak retka (bez znak tabulatora) vrijednost. Ako vam se ne znak tabulatora u retku, cijeli redak smatra se kao ključ, a vrijednost nije null.

Kada je za **reducers**navedena izvršne datoteke, svaki zadatak reducer pokreće izvršnu datoteku kao drugi proces kada je pokrenut na reducer. Kao što je zadatak reducer pokreće pretvara njegov parove unos ključa/vrijednosti u retke te ga sažetaka sadržaja u recima [stdin] [ stdin-stdout-stderr] postupka.

U aplikacijom u reducer prikuplja vezanima uz redak Izlaz iz [stdout] [ stdin-stdout-stderr] postupka. Ga pretvara svaki redak u paru ključa vrijednosti koji se prikupljaju se kao rezultat u reducer. Prema zadanim postavkama prefiks crte najviše prvi znak tabulatora ključ, a ostatak retka (bez znak tabulatora) vrijednost.

Dodatne informacije o Hadoop strujanje sučelja potražite u članku [Hadoop strujanje] [hadoop-strujanje].

**Da biste poslali na C# tok posla Brojanje riječi**

- Slijedite postupak u [riječi – Java](#word-count-java)i zamijenite definiciju posla sljedeće:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Izlazna datoteka moraju biti:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>PI estimator

Koristi u statističke pi estimator (postupak SMS Karlo) metoda za procjenu vrijednost broja pi. Točke nasumično smješten unutar jedinica kvadratni i ulaze unutar kruga inscribed unutar kvadrat vjerojatnost jednako područje kruga, pi/4. Od vrijednosti 4R, pri čemu je R omjer broj točaka koje su unutar kruga ukupan broj točaka koje se nalaze u kvadrat možete se Procijenjena vrijednost broja pi. Veća od točaka koje se koriste, ćete bolju procijenjenu vrijednost je uzorak.

Skripta za ovaj uzorak šalje posudu posao Hadoop i postavljena do pokrenuti s vrijednošću 16 karte, od kojih svaka je potreban za izračunavanje 10 milijuna uzorak točaka vrijednosti parametara. Ove vrijednosti parametara mogu se promijeniti da biste poboljšali Procijenjena vrijednost broja pi. Za referencu, prvih 10 decimalnih mjesta pi su 3.1415926535.

**Da biste poslali posao estimator pi**

- Slijedite postupak u [riječi – Java](#word-count-java)i zamijenite definiciju posla sljedeće:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort

Ovaj primjer koristi modest 10GB podataka tako da ga možete pokrenuti relativno brzo. Koristi MapReduce aplikacije razvio Owen O'Malley i Arun Murthy koje su osvojile godišnje usporednih sortiranje terabajta općenite namjene ("daytona") u 2009 s rata 0.578 TB/min (100 TB 173 minuta). Dodatne informacije u ovom i drugim sortiranja jednonitnih potražite u članku [Sortbenchmark](http://sortbenchmark.org/) web-mjesta.

Ovaj primjer koristi tri skupa MapReduce programe:

1. **TeraGen** je program MapReduce koje možete koristiti da biste generirali redaka podataka za sortiranje.
2. **TeraSort** uzoraka ulaznih podataka i koristi MapReduce za sortiranje podataka u ukupni redoslijed. TeraSort je standardni sortiranje funkcija MapReduce, osim prilagođenih partitioner koji koristi sortirani popis N-1 uzorkovanja prečaci koji se definirati ključne raspon za svaki smanjivanje. Sve prečaci takve koji uzorak [i-1] posebice u < = ključ < uzorka [i] šalju da biste smanjili i. To su jamstva izlaze od smanjite je i sve manji od izlaz smanjivanje i + 1.
3. **TeraValidate** je MapReduce program koji provjerava se globalno sortira izlaz. Stvara jedne mape po datoteci u direktoriju izlazne i svako mapiranje osigurava svaki ključ manja od ili jednaka na prethodni slajd. Funkcija karte generira i zapisa tipki imena i prezimena svake datoteke, a funkcija smanjivanje osigurava prvog ključa datoteke i veće od zadnjeg ključ i-1 datoteke. Probleme su permitted na Izlaz u smanjivanje tipke koje su izvan redoslijeda.

Ulazni i izlazni oblik, koristi sve tri aplikacije čita i zapisuje tekstne datoteke u odgovarajući oblik. Izlaz na smanjivanje ima replikacije postavite na 1, umjesto zadanog 3, jer natječaj usporednih potreban je li za izlazne podatke replicirati na više čvorove.

Uzorak svakog odgovara programa MapReduce opisane u uvodu su potrebnih triju zadataka:

1. Generiranje podataka za sortiranje tako da pokrenete MapReduce posao **TeraGen** .
2. Sortiranje podataka tako da pokrenete MapReduce posao **TeraSort** .
3. Provjerite da na povezani pravilno stupac tako da pokrenete **TeraValidate** MapReduce posao.

**Da biste poslali poslove**

- Slijedite postupak u [riječi – Java](#word-count-java)i koristiti sljedeće definicija zadatka:

    $teragen = novo AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - NazivKlase "teragen" "-argumente"-Dmapred.map.tasks=50 ","100000000","/ primjer / / 10 GB-sortiranje-unosa podataka"
    
    $terasort = novo AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - NazivKlase "terasort" "-argumente"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ primjer / / 10 GB-sortiranje-unosa podataka","/ primjer/podataka/10 GB-sortiranje-Izlaz"
    
    $teravalidate = novo AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - NazivKlase "teravalidate" "-argumente"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ primjer/podataka/10 GB-sortiranje-Izlaz","/ primjer/podataka/10 GB-sortiranje – Provjera valjanosti"


##<a name="next-steps"></a>Daljnji koraci 

U ovom se članku i u člancima u svakoj od primjera, naučili kako pokrenuti uzoraka uključene klastere HDInsight pomoću Azure PowerShell. Vodiči za o Svinja, grozd i MapReduce pomoću servisa HDInsight potražite u sljedećim temama:

* [Početak rada s Hadoop s grozd u HDInsight radi analize korištenja mobilne slušalica][hdinsight-get-started]
* [Korištenje Svinja s Hadoop na HDInsight][hdinsight-use-pig]
* [Korištenje grozd s Hadoop na HDInsight][hdinsight-use-hive]
* [Slanje Hadoop zadataka u HDInsight] [hdinsight-submit-jobs]
* [Azure HDInsight SDK dokumentaciji][hdinsight-sdk-documentation]
* [Ispravljanje pogrešaka Hadoop u HDInsight: poruke o pogreškama] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Dodatak A - izvornog koda za Brojanje riječi

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


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Dodatak B – riječi strujanje izvornog koda

MapReduce program koristi aplikacije cat.exe kao mapiranje sučelje za strujanje tekst u konzolu i aplikacije wc.exe kao smanjivanje sučelje za Brojanje riječi koje su strujanjem iz dokumenta. Mapiranje i reducer čitanje znakova, redak-po-redak iz standardnog ulaznog toka (stdin) i pisanje na standardni izlazni strujanje (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



Mapiranje kod u datoteci cat.cs koristi [StreamReader] [ streamreader] objekt da biste pročitali znakova dolazne toka konzoli zapisuje toka strujanje standardni izlazni statične [Console.Writeline] [ console-writeline] način.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Kod reducer u datoteci wc.cs koristi [StreamReader] [ streamreader] objekt da biste pročitali znakove iz standardnog unos toka koja su izlazne po cat.exe mapiranje. Kao što je čita znakova s [Console.Writeline] [ console-writeline] metoda broji riječi, brojeći razmaka i znakova za kraj retka na kraju svake riječi. Zatim piše ukupni zbroj na standardni izlazni strujanje s [Console.Writeline] broj[ console-writeline] način.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Dodatak C - Pi estimator izvornog koda

Pi estimator Java kod koji sadrži funkcije mapiranje i reducer dostupna je za ispitivanje u nastavku. Mapiranje program stvara određeni broj točaka nasumično smješten unutar kvadrat jedinica i broji broj točaka koje su unutar kruga. Reducer program akumulira točke broji prema na mappers i procjenjuje vrijednost broja pi iz formule 4R, pri čemu je R omjer broj točaka broje se unutar kruga ukupan broj točaka koje se nalaze u kvadrat.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Dodatak D - 10gb graysort izvornog koda

Kod za program TeraSort MapReduce raspoređene su za ispitivanje u ovom odjeljku.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
