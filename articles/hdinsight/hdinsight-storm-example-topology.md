<properties
 pageTitle="Primjer Apache oluja topologija na HDInsight | Microsoft Azure"
 description="Popis topologija oluja primjer stvara i s Apache oluja na HDInsight uključujući basic i C# Java Topologija i radu s koncentratorima događaj."
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

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Primjer oluja toplogies i komponente za Apache oluja na HDInsight

Slijedi popis Primjeri stvara i održava Microsoft za korištenje s Apache oluja na HDInsight. U ovim se primjerima pokrivaju različite teme u stvaranju osnovne C# i Java topologija za rad s Azure servise kao što je koncentratora događaj, DocumentDB, Power BI, SQL baze podataka, HBase HDInsight i Azure prostora za pohranu. Primjeri demonstrirati i kako raditi s tehnologije koje nisu Azure ili čak i drugih proizvođača, kao što su SignalR i Socket.IO

| Opis                                                                                             | Pokazuje                                         | Jezik/Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Pisanje Azure podataka Lake spremište Apache oluja](hdinsight-storm-write-data-lake-store.md) | Pisanje Lake spremišta podataka za Azure | Java |
| [Spout koncentrator za događaj i munje izvor](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Izvor za događaj koncentrator Spout i munje | Java |
| [Razvoj utemeljena na topologija za Apache oluja na HDInsight][5797064f]                                 | Maven                                                | Java                       |
| [Razvoj C# topologija za Apache oluja na HDInsight pomoću Visual Studio][16fce2d1]                     | Alati za HDInsight za Visual Studio                    | C#, Java                   |
| [Stvaranje većeg broja strujanja podataka u C# oluja topologije][ec5a4064]                                         | Većeg broja strujanja                                     | C#                         |
| [Određivanje Twitter trendova teme s oluja na HDInsight][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Postupak događaje iz koncentratora događaj Azure s oluja na HDInsight (C#)][844d1d81]                                | Događaj koncentratora                                           | C# i Java                |
| [Postupak događaje iz koncentratora događaj Azure s oluja na HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Događaj koncentratora | Java |
| [Vizualizirajte podatke iz oluja topologije pomoću dodatka Power Bi][94d15238]                              | Power BI                                             | C#                         |
| [Analiza podataka senzor s oluja i HBase u HDInsight][ab894747]                                     | Događaj koncentratora HBase, Socket.IO, nadzorne ploče za Web          | C#, Java, JavaScript, HTML |
| [Obrada podataka senzor vozila iz koncentratora događaja pomoću oluja na HDInsight][246ee964]                        | Događaj koncentratora DocumentDb, Azure spremišta blobova (WASB)    | C#, Java                   |
| [Izdvajanje, transformacija i učitavanje (ETL) iz koncentratora Azure događaja da biste HBase, korištenje oluja na HDInsight][b4b68194] | HBase događaj koncentratora                                    | C#                         |
| [Topologija projekta C# oluja predloška za rad s Azure servisi oluja na HDInsight][ce0c02a2]  | Događaj koncentratora DocumentDb, SQL baze podataka, HBase, SignalR | C#, Java                   |
| [Skalabilnost jednonitnih za čitanje iz koncentratora za događaj Azure HDInsight pomoću oluja][d6c540e3]           | Poruka propusnost, koncentratora događaj SQL baze podataka         | C#, Java                   |
| [Povezivanje događaja pomoću oluja i HBase na HDInsight](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Korištenje Python s oluja na HDInsight](hdinsight-storm-develop-python-topology.md) | Komponente Python s Java i Clojure temelji topologija oluja | Python |

## <a name="next-steps"></a>Daljnji koraci

* [Početak rada s Apache oluja na HDInsight][2b8c3488]

* [Saznajte kako uvesti i upravljanje oluja topologija s oluja na HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Saznajte kako stvoriti na oluja na HDInsight klaster i korištenje oluja nadzorne ploče za implementaciju topologija primjer."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Saznajte kako uvesti i upravljanje topologija pomoću na nadzornoj ploči oluja utemeljen na webu i oluja korisničkog Sučelja ili Alati za HDInsight za Visual Studio."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Saznajte kako stvoriti C# oluja topologija pomoću alata za HDInsight za Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Saznajte kako stvoriti oluja topologija u Java, pomoću Maven, kao što su stvaranje osnovne wordcount topologije."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Pokazuje kako zapisivanje podataka dodatka Power bi iz C# topologije, a zatim stvaranje grafikona i nadzorne ploče s podacima."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Pokazuje osnovna oluja topologije koja izvršava wordcount, implementirana u C#. Ovo prikazuje i kako stvoriti više tokova podataka unutar C# topologije."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Saznajte kako za čitanje i pisanje podataka iz koncentratora događaj Azure s oluja na HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Saznajte kako pomoću Apache oluja na HDInsight obrada podataka senzor iz koncentratora Azure događaj, vizualizacija pomoću D3.js i (neobavezno), spremite ga HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Saznajte kako koristiti Trident da biste stvorili oluja topologije koji određuje trendova teme (na temelju hashtags,) na Twitteru."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Saznajte kako koristiti oluja topologije čitati poruke iz koncentratora događaj Azure čitati dokumente iz Azure DocumentDB za pozivanju podataka i spremanje podataka za pohranu Azure."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Nekoliko topologija da bismo pokazali propusnost kada čitanju iz koncentratora događaj Azure i pohranu s bazom podataka SQL pomoću Apache oluja na HDInsight."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Saznajte kako čitati podatke iz Azure događaj koncentratora zbrajanja i pretvaranje podataka, a zatim ga spremiti HBase na HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Taj projekt sadrži predloške za spouts "," Vijci "i" Topologija interakciju s različitim Azure servisa, kao što je koncentratora događaj, DocumentDB i SQL baze podataka."
 
