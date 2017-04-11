<properties
   pageTitle="Razvoj utemeljena na topologija za Apache oluja | Microsoft Azure"
   description="Saznajte kako stvoriti oluja topologija u Java tako da stvorite topologiju count jednostavne word."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Razvoj utemeljena na Topologija aplikacije osnovni Brojanje riječi s oluja Apache i Maven na HDInsight

Saznajte kako stvoriti topologije utemeljenih za Apache oluja na HDInsight pomoću Maven. Će voditi kroz postupak stvaranja osnovne Brojanje riječi aplikacije pomoću Maven i Java, gdje je definiran topologije u Java. Nakon toga će saznat ćete kako da biste definirali topologije pomoću Flux framework.

> [AZURE.NOTE] Flux framework dostupna je u oluja 0.10.0 ili noviji. Oluja 0.10.0 dostupan je sa servisa HDInsight 3,3 i 3.4.

Nakon dovršetka postupka u ovom dokumentu, imat ćete osnovni topologije koju možete implementirati Apache oluja na HDInsight.

> [AZURE.NOTE] Dovršenu verziju topologija stvorena u ovom dokumentu dostupan je na [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

##<a name="prerequisites"></a>Preduvjeti

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java za razvojne inženjere Kit (JDK) verzije 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven</a>: Maven je sustav Sastavi projekta za Java projekte.

* Uređivaču teksta kao što je blok za pisanje, <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Sublime teksta</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Ili pomoću programa integrirano razvojno okruženje (IDE) kao što je <a href="https://eclipse.org/" target="_blank">Eclipse</a> (verzija Luna ili noviji).

    > [AZURE.NOTE] Uređivanje ili IDE možda određene funkcije za rad s Maven koju je adresirana u ovom dokumentu. Informacije o mogućnostima okruženje za uređivanje, potražite u dokumentaciji za proizvod koji koristite.

##<a name="configure-environment-variables"></a>Konfiguriranje varijable okruženja

Kada instalirate Java i na JDK možda postavljena sljedeće varijable okruženja. Međutim, trebali biste provjerite da postoje i sadrže odgovarajuće vrijednosti za sustav.

* **JAVA_HOME** – moraju pokazivati direktoriju u kojem je instaliran okruženje za izvođenje Java (JRE). Ako, na primjer, u raspodjelom Unix ili Linux je potrebno vrijednost koja je slična `/usr/lib/jvm/java-7-oracle`. U sustavu Windows, želite imati slično kao vrijednost`c:\Program Files (x86)\Java\jre1.7`

* **Put** – mora sadržavati sljedeće putovi:

    * **JAVA_HOME** (ili ekvivalentne puta)

    * **JAVA_HOME\bin** (ili ekvivalentne puta)

    * Direktoriju u kojem je instaliran Maven

##<a name="create-a-new-maven-project"></a>Stvaranje novog Maven projekta

Iz naredbenog retka, koristite sljedeći kod da biste stvorili novi projekt Maven pod nazivom **WordCount**:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

To će stvoriti novi direktorij pod nazivom **WordCount** na trenutno mjesto koje sadrži osnovnog projekta Maven.

Direktorija **WordCount** će sadržavati sljedeće stavke:

* **pom.XML**: sadrži postavke za Maven projekt.

* **src\main\java\com\microsoft\example**: sadrži kodu aplikacije.

* **src\test\java\com\microsoft\example**: sadrži testovi za svoju aplikaciju. U ovom primjeru smo hoće neće stvarati testira.

###<a name="remove-the-example-code"></a>Uklanjanje primjer koda

Jer će smo stvaranje naš aplikacije, izbrišite generirani test i datoteka aplikacije:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Dodaj svojstva

Maven omogućuje definiranje projekta razinom vrijednosti pod nazivom svojstva. Dodajte sljedeće nakon na `<url>http://maven.apache.org</url>` redak:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Sada možete koristiti te vrijednosti u ostale sekcije. Ako, na primjer, pri određivanju verzije oluja komponente, možete koristiti `${storm.version}` umjesto teško kodiranje vrijednost.

##<a name="add-dependencies"></a>Dodavanje ovisnosti

Budući da je oluja topologije, morate dodati ovisnosti za oluja komponente. Otvorite datoteku **pom.xml** i dodati sljedeći kod u u ** &lt;ovisnosti >** odjeljak:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

Kompiliranje vrijeme Maven koristi taj podatak da biste potražili **oluja core** u spremištu Maven. Najprije se traži u spremište na lokalnom računalu. Ako datoteka ne postoji, preuzmite ih iz javne spremište Maven će se i sprema ih u lokalnom spremištu.

> [AZURE.NOTE] Obavijest na `<scope>provided</scope>` retka u odjeljku dodali smo. Ovo govori Maven za isključivanje **oluja core** iz svih datoteka POSUDU ćemo stvoriti jer će se pružati sustav. Time se omogućuje pakete stvarate treba nešto manje i osigurava da će koristiti bitova **oluja core** uvrštene u oluja na klasteru HDInsight.

##<a name="build-configuration"></a>Sastavljanje konfiguracija

Dodaci za maven omogućuju vam prilagodbu Sastavi faze projekta, kao što su kako prikupljaju projekt ili kako pakiranje POSUDU u datoteku. Otvorite datoteku **pom.xml** i dodati sljedeći kod izravno iznad u `</project>` redak.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

U ovom se odjeljku će se koristiti da biste dodali dodatke, resursa i ostale mogućnosti konfiguracije Sastavi. Punu referencu __pom.xml__ datoteka potražite u članku [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###<a name="add-plug-ins"></a>Dodavanje dodataka

Topologija oluja <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Dodatak za Maven izvršavanja prve</a> je korisno jer omogućuje jednostavno lokalno pokretanje topologije u razvojno okruženje. Dodajte sljedeće da biste na `<plugins>` odjeljku **pom.xml** datoteke da biste dodali dodatak za Maven izvršavanja prve:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Imajte na umu da se `<mainClass>` unosa koristi `${storm.topology}`. Ne možemo niste definirati ranije u odjeljku Svojstva (ali nije moguće imamo.) Umjesto toga, ne možemo će tu vrijednost postavite iz naredbenog retka kada se pokrene topologije s razvojno okruženje u noviji koraku.

Drugo korisno je dodatak <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache Maven Compiler dodatak</a>, koji se koristi da biste promijenili mogućnosti sastavljanja. Primarni razloga moramo to je da biste promijenili verziju jezika Java koji koristi Maven za izvorišno i odredišno aplikacije. Želimo verzija 1.7.

Dodajte sljedeće u na `<plugins>` dio datoteke **pom.xml** uključiti dodatak alata za Kompiliranje Maven Apache i postavite verzije izvorišno i odredišno 1.7.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Konfiguriranje resursi

U odjeljku resursi omogućuje uključuju resurse koji nisu kod kao što su konfiguracija datoteke koje su potrebne komponente u topologiji. U ovom primjeru dodajte sljedeće u na `<resources>` odjeljku **pom.xml** datoteke.

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Time se dodaje direktorija resursa u korijenskoj mapi projekta (`${basedir}`) kao mjesto koje sadrži resurse i sadrži datoteku pod nazivom __log4j2.xml__. Datoteka koristi se za konfiguraciju informacije koje se prijave topologije.

##<a name="create-the-topology"></a>Stvaranje topologije

Utemeljena na oluja topologije sastoji se od tri komponente koje morate autor (ili referenca) kao u opsegu.

* **Spouts**: čita podataka iz vanjskih izvora i emits strujanja podataka u topologiji.

* **Vijci s maticama**: provodi obrada strujanja čuje spouts ili druge Vijci i emits jedan ili više strujanja.

* **Topologija**: definira kako spouts i Vijci raspoređivati i navedeni točka unosa za topologije.

###<a name="create-the-spout"></a>Stvaranje na spout

Da biste smanjili preduvjeti za postavljanje vanjskim izvorima podataka, sljedeće spout jednostavno emits slučajni rečenica. To je izmijenjena verzija spout koje ste dobili s [primjerima oluja Starter](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [AZURE.NOTE] Primjer spout koja čita iz vanjskog izvora podataka potražite u nekom od sljedećih primjera:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): na primjer spout koja čita iz Twitter
>
> * [Oluja Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): spout koja čita iz Kafka

Za spout, stvorite novu datoteku pod nazivom **RandomSentenceSpout.java** u direktoriju **src\main\java\com\microsoft\example** i koristiti sljedeće kao sadržaja:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Potrajati koji trenutak da biste pročitali kroz komentare kod da biste razumjeli kako funkcionira ova spout.

> [AZURE.NOTE] Iako ovaj topologije koristi samo jedan spout, drugima možda nekoliko sažetka sadržaja podataka iz različitih izvora u topologiji.

###<a name="create-the-bolts"></a>Stvaranje na Vijci

Vijci ručicu za obradu podataka. Za ovaj topologije imamo još dvije Vijci:

* **SplitSentence**: dijeli rečenica čuje po **RandomSentenceSpout** na pojedinačne riječi.

* **WordCount**: Broji koliko je puta došlo je do svake riječi.

> [AZURE.NOTE] Vijci možete učiniti doslovno ništa, na primjer, izračuni, postojanost ili razgovor sa vanjske komponente.

Stvorite dva nove datoteke, **SplitSentence.java** i **WordCount.Java** u direktoriju **src\main\java\com\microsoft\example** . Koristite sljedeće kao sadržaja za datoteke:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Potrajati koji trenutak da biste pročitali kroz komentare kod da biste razumjeli kako funkcionira svaki munje.

###<a name="define-the-topology"></a>Definiranje topologije

Topologije ties na spouts i Vijci zajedno s maticama u grafikonu koji definira tok podataka između komponente. Također nudi parallelism savjete koji oluja koristi pri stvaranju instance komponente unutar klaster.

Slijedi osnovni dijagram grafikonu komponente za ovaj topologije.

![dijagram s prikazom rasporeda spouts i Vijci](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Da biste implementirate topologije, stvorite novu datoteku pod nazivom **WordCountTopology.java** u direktoriju **src\main\java\com\microsoft\example** . Koristite sljedeće kao sadržaj datoteke:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Potrajati koji trenutak da biste pročitali kroz komentare kod da biste razumjeli kako definirani i poslati klaster topologije.

###<a name="configure-logging"></a>Konfiguriranje zapisivanja

Oluja koristi Apache Log4j za zapisivanje podataka. Ako ne konfigurirate zapisivanje, topologije će šalji mnogo dijagnostičke informacije, što može biti teško čitati. Da biste odredili što zapisuje, stvorite datoteku pod nazivom __log4j2.xml__ u direktoriju __Resursi__ . Koristite sljedeće kao sadržaj datoteke.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

To konfigurira novi zapisivaču klase __com.microsoft.example__ , što obuhvaća komponente u ovom primjeru topologiji. Razina postavljen je na praćenja za ovaj zapisivaču koji će snimite sve informacije zapisivanje čuje komponente u ovom topologiji. Ako ponovno vidjeli putem koda za taj projekt, primijetit ćete da samo datoteku WordCount.java implementira zapisivanje; To će prijavite Brojanje riječi.

Na `<Root level="error">` odjeljak konfigurira korijenskoj razini prijave (što nije u __com.microsoft.example__) Zapisujte samo podatke o pogreškama.

> [AZURE.IMPORTANT] Dok je to uvelike smanjuje informacije zapisuje testiranju topologije u razvojno okruženje, uklonite sve podatke ispravljanje pogrešaka proizvodi prometom klaster u radni. Da biste smanjili te podatke, morate postaviti ispravljanje pogrešaka FALSE u konfiguraciji šalje na klaster. Kako se prikazuje kod WordCountTopology.java u ovom dokumentu, na primjer 

Dodatne informacije o konfiguriranju za Log4j zapisivanje potražite u članku [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Verzija oluja 0.10.0 koristi Log4j 2.x. Starije verzije oluja koristi Log4j 1.x koji koristi neki drugi oblik za konfiguraciju zapisnika. Informacije o starije konfiguraciji potražite u članku [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##<a name="test-the-topology-locally"></a>Testiranje topologije lokalno

Nakon spremanja datoteke, koristite sljedeću naredbu da biste testirali topologije lokalno.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Dok se izvodi topologije prikazat će se informacije o pokretanju. A zatim ga započinje prikazani reci slično sljedećem su rečenica čuje iz na spout a obrađuje na Vijci.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Tako da pogledate zapisivanje čuje po munje WordCount, ne možemo koji mogu vidjeti "i" sadrži čuje 113 vremena. Broj će i dalje da biste se pomaknuli prema gore dok god topologije pokreće jer u spout neprestano emits isti rečenica.

Dva će biti 5 drugi interval između ispuštanje riječi i brojanja. To se događa jer komponentu __WordCount__ je konfiguriran za samo šalji podatke kada stigne n-torke crtičnih pa ga zatraži da takve redni samo isporučuju svakih 5 sekundi prema zadanim postavkama.

## <a name="convert-the-topology-to-flux"></a>Pretvaranje topologije Flux

Flux je novi framework s oluja 0.10.0, koji omogućuje razdvajanje konfiguraciju iz implementacije. Komponente (Vijci i spouts,) i dalje su definirani u Java, ali topologije definiran pomoću YAML datoteke.

Datoteka YAML definira komponente za topologiji, tok podataka između njih, a što vrijednosti za pokretanje komponente. Možete uključiti YAML datoteke kao dio posudu datoteku koja sadrži projekta kada njegove implementacije kod ili koristite vanjsku datoteku YAML prilikom pokretanja topologije.

1. Premještanje datoteke __WordCountTopology.java__ iz projekta. Prethodno, to definirano topologije, ali ne možemo neće biti ga koristiti za Flux.

2. U direktoriju __resursa__ , stvorite novu datoteku pod nazivom __topology.yaml__. Koristite sljedeće kao sadržaj ove datoteke.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Potrajati pročitajte i razumijevanje funkcija svake sekcije i njegovom definiciju utemeljenih u datoteci __WordCountTopology.java__ .

3. Provjerite sljedeće promjene u datoteku __pom.xml__ .

    * Dodajte sljedeće nove ovisnost u na `<dependencies>` odjeljak:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Sljedeći je dodatak da biste dodali u `<plugins>` sekciju. Ovaj dodatak rukuje stvaranje paketa (posudu datoteka) za projekt, a primjenjuje neke određene transformacije Flux pri stvaranju paketa.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
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

    * U __izvršavanja prve maven dodatak__ `<configuration>` sekcije, promijenite vrijednost za `<mainClass>` za `org.apache.storm.flux.Flux`. Time se omogućuje Flux učiniti radi topologije dok ne možemo ga pokrenuti lokalno u razvoju.

    * U na `<resources>` sekcije, dodajte sljedeće da biste na `<includes>`. To obuhvaća YAML datoteku koja definira topologije u sklopu projekta.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Testiranje flux topologiji lokalno

1. Koristite sljedeće Kompiliranje i izvršenje topologije Flux pomoću Maven.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Ako koristite PowerShell, koristite sljedeće:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Ako se u sustavu Unix/Linux/OS X, a imate [instaliran oluja u razvojno okruženje](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), možete koristiti sljedeće naredbe:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    Na `--local` parametar pokreće topologije u načinu rada za lokalni na razvojno okruženje. Na `-R /topology.yaml` parametar koristi u `topology.yaml` datoteka resursa iz datoteke posudu da biste definirali topologije.

    Dok se izvodi topologije prikazat će se informacije o pokretanju. A zatim ga započinje prikazani reci slično sljedećem su rečenica čuje iz na spout a obrađuje na Vijci.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Prikazat će se 10 drugi odgode između serijama od zabilježeni podaci, kao na `topology.yaml` datoteke prosljeđuje vrijednost `10` pri stvaranju komponentu WordCount. To postavlja interval odgode za n-torke crtičnih 10 sekundi.

2.  Napravite njezinu kopiju u `topology.yaml` datoteke iz projekta. Nazovite ga nešto poput `newtopology.yaml`. U datoteci, pronađite sljedeće sekcije i promijenite vrijednost `10` da biste `5`. Time se mijenja interval između emitira serijama od Brojanje riječi iz 10 sekundi 5.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Da biste pokrenuli topologije, koristite sljedeću naredbu:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Ili, ako imate oluja okruženje za razvoj Unix/Linux/OS X:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Promjena u `/path/to/newtopology.yaml` da biste put do datoteke newtopology.yaml koji ste stvorili u prethodnom koraku. Ta naredba koristit će se newtopology.yaml kao definiciju topologije. Budući da ne možemo niste uključili u `compile` parametar Maven će ponovno korištenje verzija programa project ugrađen u prethodnim koracima.

    Kada se pokrene topologije, trebali biste Primijetit ćete da je vrijeme između čuje serije promijenila u skladu s vizualnim vrijednost u newtopology.yaml. Da biste vidjeli koji konfiguraciju možete promijeniti pomoću YAML datoteke bez potrebe za prevoditi topologije.

Postoji nekoliko drugih značajki da Flux nudi koji se su ovdje nije objašnjena kao što su varijable zamjena u YAML datoteku na temelju parametara proslijeđena pri izvođenju ili iz varijable okruženja. Dodatne informacije o ovim i druge značajke Flux framework potražite u članku [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident je više razine apstrakcije koju je dodijelio oluja. Podržava s praćenjem stanja obrada. Primarni prednost Trident jest da možete ga jamči da svaku poruku koja unosi topologije obrađuje samo jedanput. Ovo je teško da biste postigli u neobrađenog Java topologiju, koje jamstvo na poruke obradit će se barem jednom. Postoje razlike, kao što su ugrađene komponente koje je moguće koristiti umjesto stvaranja Vijci. Zapravo Vijci potpuno zamjenjuju manje generic komponente, kao što su filtri, projekcija i funkcije.

Trident aplikacije moguće stvoriti pomoću Maven projektima. Koristite iste osnovne korake kao navedene u ovom članku – samo kod razlikuje. Trident i ne (trenutno) može koristiti s Flux framework.

Dodatne informacije o Trident potražite u članku <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Pregled Trident API -JA</a>.

Primjer Trident aplikacije, potražite u članku [Twitteru trendova teme s Apache oluja na HDInsight](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Daljnji koraci

Saznali ste kako stvoriti oluja topologije pomoću Java. Sada naučiti kako:

* [Uvođenje i upravljanje Apache oluja topologija na HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Razvoj C# topologija za Apache oluja na HDInsight pomoću Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

[Primjer topologija za oluja na HDInsight](hdinsight-storm-example-topology.md)web-mjestu možete pronaći dodatne primjer topologija oluja.
