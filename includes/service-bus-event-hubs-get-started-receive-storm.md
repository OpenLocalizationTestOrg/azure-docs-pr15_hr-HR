## <a name="receive-messages-with-apache-storm"></a>Primanje poruka uz Apache oluja

[**Apache oluja**](https://storm.incubator.apache.org) je sustav raspodijeljeno u stvarnom vremenu izračuni koji pojednostavljuje pouzdanog obrada neograničeno strujanja podataka. U ovom se odjeljku opisano kako pomoću programa spout događaj koncentratora oluja prima događaje iz koncentratora za događaj. Korištenje Apache oluja, možete razdvojiti događaje preko više procesa smješten u različitim čvorove. Događaj koncentratora Integracija s oluja pojednostavljuje potrošnje događaja po proziran checkpointing tijeku putem oluja na Zookeeper instalacije, upravljanje stalni checkpoints i paralelno prima iz koncentratora za događaj.

Dodatne informacije o događajima koncentratorima primanje uzoraka, potražite u članku [Pregled koncentratorima događaj][].

Pomoću ovog praktičnog vodiča koristi instalacije [Oluja HDInsight][] koji se isporučuje se s već dostupne spout koncentratora za događaj.

1. Slijedite postupak za [HDInsight oluja – prvi koraci](../articles/hdinsight/hdinsight-storm-overview.md) za stvaranje novog HDInsight klaster i s njim povezati putem udaljene radne površine.

2. Kopiraj u `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` datoteke za lokalni platforme. Sadrži događaje-oluja-spout.

3. Koristite sljedeću naredbu da biste instalirali paket u lokalnom spremištu Maven. Omogućuje dodavanje kao referencu u programu project oluja u noviji koraku.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. U Eclipse, stvorite novi projekt Maven (kliknite **datoteka**, a zatim **Novo**, a zatim **projekt**).

    ![][12]

5. Odaberite **Koristi zadano mjesto za radni prostor**, a zatim kliknite **Dalje**

6. Odaberite archetype **maven-archetype – brzi početak rada** , a zatim kliknite **Dalje**

7. Umetanje **GroupId** i **ArtifactId**, a zatim kliknite **Završi**

8. U **pom.xml**, dodajte sljedeće ovisnosti u na `<dependency>` čvor.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. U mapi **src** stvoriti datoteku pod nazivom **Config.properties** i kopirajte sljedeći sadržaj, zamjenjujući sljedeće vrijednosti:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    Vrijednost za **eventhub.receiver.credits** određuje koliko se disciplina su odbacivanja prije izdavanja oluja kanal. U ovom se primjeru radi jednostavnosti, postavlja tu vrijednost 10. U radni, je potrebno obično postaviti na višu vrijednosti; na primjer, 1024.

10. Stvaranje nove predmete naziva **LoggerBolt** sa sljedeći kod:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Ovaj munje oluja zapisuje sadržaj primljene događaja. To možete jednostavno se proširiti za pohranu redni u servis za pohranu. [Vodič za analizu senzor HDInsight] koristi takvog isti radi pohrane podataka u HBase.

11. Stvaranje klasu naziva **LogTopology** sa sljedeći kod:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Klase stvara novi događaj koncentratorima spout, pomoću svojstva u konfiguraciji datoteke instatiate ga. Važno je da Imajte na umu da u ovom primjeru stvara proizvoljan broj spouts zadatke kao broj particija u središtu događaja da biste mogli koristiti maksimalni parallelism dopušteno mjerodavnim koncentratora događaj.

<!-- Links -->
[Pregled događaja koncentratora]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight oluja]: ../articles/hdinsight/hdinsight-storm-overview.md
[Vodič za analizu senzor HDInsight]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png