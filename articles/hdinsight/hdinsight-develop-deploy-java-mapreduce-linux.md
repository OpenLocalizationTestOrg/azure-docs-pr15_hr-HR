<properties
    pageTitle="Razvoj Java MapReduce programe za HDInsight koji se temelji na Linux | Microsoft Azure"
    description="Saznajte kako razviti Java MapReduce programa i uvesti ih sustavom Linux HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Razvoj Java MapReduce programe za Hadoop na HDInsight Linux

U ovom dokumenata vodit će vas kroz pomoću Apache Maven stvaranje MapReduce aplikacije, a zatim implementacije i pokrenuti na Hadoop Linux utemeljen na klasteru HDInsight.

##<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 ili noviji (ili ekvivalent, kao što je OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Azure pretplate**

- **Azure EŽA**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Konfiguriranje varijable okruženja

Kada instalirate Java i na JDK možda postavljena sljedeće varijable okruženja. Međutim, trebali biste provjerite da postoje i sadrže odgovarajuće vrijednosti za sustav.

* **JAVA_HOME** – moraju pokazivati direktoriju u kojem je instaliran okruženje za izvođenje Java (JRE). Ako, na primjer, u sustavu OS X, Unix ili Linux, treba imati sljedeći vrijednost koja je slična `/usr/lib/jvm/java-7-oracle`. U sustavu Windows, želite imati slično kao vrijednost`c:\Program Files (x86)\Java\jre1.7`

* **Put** – mora sadržavati sljedeće putovi:

    * **JAVA_HOME** (ili ekvivalentne puta)

    * **JAVA_HOME\bin** (ili ekvivalentne puta)

    * Direktoriju u kojem je instaliran Maven

##<a name="create-a-new-maven-project"></a>Stvaranje novog Maven projekta

1. Iz terminal sesiju ili naredbenog retka u razvojno okruženje, promijenite direktorija na mjesto na koje želite pohraniti taj projekt.

3. Koristite naredbu __mvn__ , koji se instalira uz Maven, da biste generirali scaffolding za projekt.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Time ćete stvoriti novi imenik u trenutnog direktorija pod nazivom navedeno parametrom __artifactID__ (**wordcountjava** u ovom primjeru.) Taj imenik će sadržavati sljedeće stavke:

    * __pom.xml__ - [Projekta Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) koji sadrži podatke i konfiguracije detalje koristi za stvaranje projekta.

    * __src__ - direktorij koji sadrži __glavne/java/organizacijski/apache/hadoop/Primjeri__ direktorija, gdje će autor aplikacije.

3. Brisanje datoteka __src/test/java/org/apache/hadoop/examples/apptest.java__ dok ga se ne koriste u ovom primjeru.

##<a name="add-dependencies"></a>Dodavanje ovisnosti

1. Uređivanje datoteke __pom.xml__ i dodajte sljedeće unutar na `<dependencies>` odjeljak:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Ovo govori Maven projekta potrebna je biblioteke (naveden u &lt;artifactId\>) s određenom verzijom (naveden u &lt;verziju\>). Kompiliranje vrijeme, to će se preuzeti u spremištu Maven zadani. [Maven spremište pretraživanja](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) možete koristiti da biste pregledali više.

    Na `<scope>provided</scope>` govori Maven da te ovisnosti neće biti isporučen aplikacije, oni će biti dala klaster HDInsight pri izvođenju.

2. Dodajte sljedeće __pom.xml__ datoteku. Time se mora nalaziti unutar na `<project>...</project>` oznake u datoteci. na primjer, između `</dependencies>` i `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Dodatak za prvu konfigurira [Maven Nijansa dodatak](http://maven.apache.org/plugins/maven-shade-plugin/), koji se koristi za izradu programa uberjar (ponekad se naziva na fatjar), koji sadrži ovisnosti potrebnih aplikacije. Sprječava i dupliciranje licenci unutar paketa posudu koji mogu uzrokovati probleme u nekim sustavima.

    Dodatak za drugi konfigurira compiler Maven koji se koristi za postavljanje verzije programa Java potrebnih ovu aplikaciju verzija koristi na klasteru HDInsight.

3. Spremite datoteku __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>Stvaranje aplikacije MapReduce

1. Idite na imenik __wordcountjava/src/glavne/java/organizacijski/apache/hadoop/primjere__ i preimenujte datoteku __App.java__ __WordCount.java__.

2. Otvorite __WordCount.java__ datoteku u uređivaču teksta i zamijeniti sadržaj sljedeće:

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

    Obratite pozornost na naziv paketa je **org.apache.hadoop.examples** , a naziv klase **WordCount**. Nazivi će koristiti kada pošaljete MapReduce posao.

3. Spremite datoteku.

##<a name="build-the-application"></a>Stvaranje aplikacije

1. Ako se ne nalazite li, promijenite u direktoriju __wordcountjava__ .

2. Da biste sastavili POSUDU datoteku koja sadrži aplikaciju, koristite sljedeću naredbu:

        mvn clean package

    To će se očistiti sve prethodne artefakte Sastavi, preuzmite sve ovisnosti već je instaliran, a zatim Sastavljanje i paket aplikacije.

3. Kada se dovrši naredbu, direktorija __wordcountjava/ciljne__ će sadržavati datoteku pod nazivom __wordcountjava 1.0 SNAPSHOT.jar__.

    > [AZURE.NOTE] Datoteka __wordcountjava 1.0 SNAPSHOT.jar__ je uberjar koja sadrži ne samo na WordCount posla, ali i ovisnosti koje je potrebno posao prilikom izvođenja.


##<a id="upload"></a>Prijenos posudu

Da biste prenijeli datoteke posudu na HDInsight headnode, koristite sljedeću naredbu:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

To čvor glavni kopira datoteka s lokalnog sustava.

> [AZURE.NOTE] Ako ste koristili lozinku radi zaštite računa SSH, zatražit će se za lozinku. Ako ste koristili ključa SSH, možda ćete morati koristiti u `-i` parametar i put do privatni ključ. Na primjer, `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Pokreni MapReduce

1. Povezivanje sa servisom HDInsight pomoću SSH kao što je opisano u sljedećim člancima:

    - [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Iz sesije SSH za pokretanje aplikacije MapReduce koristite sljedeću naredbu:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    To će koristiti aplikaciju WordCount MapReduce za Brojanje riječi u datoteci davinci.txt i spremiti rezultate u __wasbs: / / / primjer/podataka/wordcountout__. Ulazna datoteka i izlaz spremaju se zadana za pohranu za klaster.

3. Nakon dovršetka posla za prikaz rezultata koristite sljedeće:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Trebali biste dobiti popis riječi i broji vrijednostima sličnu ovoj:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Daljnji koraci

U ovom dokumentu ste naučili kako razviti Java MapReduce posao. Pogledajte sljedeće dokumente za drugi načini rada s HDInsight.

- [Korištenje grozd s HDInsight][hdinsight-use-hive]
- [Korištenje Svinja s HDInsight][hdinsight-use-pig]
- [Korištenje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

Dodatne informacije potražite u odjeljku [Razvojni centar za Java](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

