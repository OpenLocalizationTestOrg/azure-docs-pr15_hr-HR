<properties 
    pageTitle="Pregled Apache Spark u HDInsight | Microsoft Azure" 
    description="Uvod u Apache Spark u HDInsight i scenariji za korištenje Spark na HDInsight u aplikacije." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Pregled: Apache Spark na HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> je programa paralelno Otvori izvor obrade framework koji podržava obrada u memoriji da biste uvećali performanse velikih skupova podataka analitički aplikacije. Modul za obradu Spark ugrađena je za brzinu, lakšeg korištenja i složene analize. Mogućnosti izračuna u memoriji Spark na ga činite dobar izbor za izračun s iteracijama algoritama computations strojnog učenja i grafikonu. Spark je kompatibilan s Azure blobova (WASB) i tako postojeće podatke pohranjene u Azure možete jednostavno obrađuju putem Spark.

Kada stvorite Spark klaster u HDInsight, stvorite Azure računalnim resursi s Spark instalacije i konfiguracije. Potrebno je samo deset minuta da biste stvorili Spark klaster u HDInsight. Podaci za obradu se pohranjuju u spremište blobova platforme Azure. Potražite u članku [Korištenje blobova platforme Azure s HDInsight][hdinsight-storage].

![Apache Spark na Azure HDInsight] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark na Azure HDInsight")


**Želite započeti s Apache Spark na Azure HDInsight?** U odjeljku [brzi početak rada: Stvaranje Spark klaster na HDInsight Linux i pokretanje uzorka aplikacije koje se koriste Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Popis poznatih problema i ograničenja s trenutnom izdanju, potražite u članku [Poznati problemi Apache Spark u Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Zašto koristiti Spark na Azure HDInsight? 

Azure HDInsight nudi potpuno upravljanih Spark servisa. Prednosti korištenja Spark na HDInsight su:

| Značajka                             | Opis       |
|-------------------------------------|-------------------|
| Jednostavno stvaranje klastere            | Možete stvoriti novu klaster Spark na HDInsight u minutama pomoću portala za upravljanje Azure, Azure PowerShell i HDInsight .NET SDK. Potražite u članku [Početak rada s Spark klaster u HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Lakoća korištenja                     | Spark u HDInsight klastere obuhvaća Jupyter bilježnica unaprijed konfiguriran. Pomoću ove interaktivne obradu podataka i vizualizaciju. URL-ova za na je https://CLUSTERNAME.azurehdinsight.net/jupyter. Zamijenite __CLUSTERNAME__ naziv svoj klaster Spark HDInsight.|
| REST API-ji                       | Spark u HDInsight obuhvaća [Livije](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), REST API temelji poslužitelja zadatka Spark daljinski slanje i praćenje zadataka izvodi. |
| Podrška za spremište Lake podataka za Azure | Spark na HDInsight moguće je konfigurirati da biste upotrijebili Lake spremišta podataka za Azure dodatan prostor za pohranu. Dodatne informacije o spremištu Lake podataka potražite u članku [Pregled programa Azure Lake spremišta podataka](../data-lake-store/data-lake-store-overview.md).
| Integraciju s komponentom servisa Azure | Spark na HDInsight u sklopu poveznik s koncentratorima Azure događaj. Korisnici možete izraditi strujanje aplikacije koje se koriste koncentratora događaj, osim [Kafka](http://kafka.apache.org/), koji je već dostupan kao dio Spark. |
| Podrška za poslužitelj R  | R Normalni prikaz na HDInsight Spark klaster možete postaviti da se pokreće raspodijeljeno computations R s brzine Obećani s Spark klaster. Dodatne informacije potražite u članku [Prvi koraci pri korištenju R Normalni prikaz na HDInsight](hdinsight-hadoop-r-server-get-started.md).   |
| Integracija s IntelliJ IDEJA  | Dodatak za HDInsight za IntelliJ možete koristiti za stvaranje i slanje aplikacije programa klaster HDInsight Spark. Dodatne informacije potražite u članku [Korištenje HDInsight Alati dodatak za IntelliJ IDEJA da biste stvorili Spark aplikacije za klaster HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Istodobni upita              | Spark u HDInsight podržava Istodobni upita. Time se omogućuje većeg broja upita iz jednog korisnika ili više upita s različitim korisnicima i aplikacijama da biste zajednički koristili iste klaster resurse. |
| Predmemoriranje SSDs                 | Možete odabrati podatke u predmemoriju memorije ili u SSDs priložiti čvorove klaster. Predmemoriranje u memoriji nudi najbolje performanse upita, ali može biti skupi; predmemoriranje u SSDs nudi izvrstan za poboljšanje performansi upita bez potrebe za stvaranje klaster veličine potrebnog za Prilagodi cijeli skup podataka u memoriji.|
| Integracija s alatima za Poslovno obavještavanje       | Spark za HDInsight nudi poveznici za alatima za Poslovno obavještavanje kao što je [Power BI](http://www.powerbi.com/) i [Tableau](http://www.tableau.com/products/desktop) za analizu podataka.|
| Unaprijed učitani Anaconda biblioteke        | Spark klastere na HDInsight koji se isporučuju s bibliotekama Anaconda unaprijed instalirano. [Anaconda](http://docs.continuum.io/anaconda/) nudi blizu 200 biblioteke strojnog učenja analiza podataka, vizualizaciju, itd.|
| Skalabilnost                     | Iako možete navesti broj čvorove u svoj klaster tijekom stvaranja, trebali biste Povećaj ili Smanji klaster tako da odgovara radno opterećenje. Sve HDInsight klastere omogućuju da biste promijenili broj čvorovi u klasteru. Osim toga, Spark klastere može biti ispušteni bez gubitka podataka jer se svi podaci se pohranjuju u spremište blobova platforme Azure. |
| Podrška za 24-7                    | Spark na HDInsight isporučuje se s podršku 24-7 na razini tvrtke i sustava SLA 99.9% gore-vrijeme.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Što su slučajevi koristi za Spark na HDInsight?

Apache Spark u HDInsight omogućuje sljedeće ključni scenariji.

### <a name="interactive-data-analysis-and-bi"></a>Interaktivni analize i BI

[Pogledajte na vodič](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark u HDInsight sprema podatke u Azure blob-ova. Poslovni stručnjaci i donose odluke tipki možete analizirati i stvaranje izvješća preko tih podataka i stvaranje interaktivnih izvješća iz obrađeno podataka pomoću dodatka Microsoft Power BI. Analitičar analitičar podataka možete pokretanje nestrukturirane/očekivan strukturirane podatke u Azure prostora za pohranu, definiranje shema za podatke pomoću bilježnice i izraditi podatkovnih modela pomoću dodatka Microsoft Power BI. Spark u HDInsight podržava i broj Alati drugih proizvođača BI kao što su Tableau, Qlikview i Lumira SAP čime idealna platformu za analitičar analitičar podataka podataka, poslovni stručnjaci i donose odluke ključa.

### <a name="iterative-machine-learning"></a>Upoznavanje s iteracijama računala

[Pogledajte na Praktični vodič: predviđanje sastavljanjem temperature uisng HVAC podataka](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Pogledajte na Praktični vodič: predviđanje rezultata provjere za hranu](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

U sklopu Apache Spark [MLlib](http://spark.apache.org/mllib/), strojnog učenja biblioteke on nudi Spark. Osim toga, Spark na HDInsight obuhvaća Anaconda, Python raspodjele s raznim paketa za strojnog učenja. To couple s ugrađenu podršku za Jupyter bilježnice, a imate vrha-na-reda okruženja za stvaranje aplikacija strojnog učenja.  

### <a name="streaming-and-real-time-data-analysis"></a>Analiza podataka strujanje i u stvarnom vremenu

[Pogledajte na vodič](hdinsight-apache-spark-eventhub-streaming.md)

Analiza podataka u stvarnom vremenu koristi se za scenariji u rasponu od smanjiti vrijeme da biste uvid u podatke prema obrada podataka kao što je stane, za stvaranje true strujanje rješenja. Spark u HDInsight nudi obogaćenog podrška za izrade rješenja u stvarnom vremenu analize. Dok Spark već ima poveznici za ingest podataka iz više izvora kao što su Kafka, Flume, Twitter, ZeroMQ ili TCP sockets, Spark u HDInsight dodaje jednostavno prva liga podršku za ingesting podatke iz koncentratora Azure događaj. Događaj koncentratora su najčešće široku stavljanja servis Azure. Podrška za izlaz u-tvorničke potrebe za događaj koncentratora čini povećati u HDInsight idealna platformu za kanal analize stvarnom vremenu.

##<a name="next-steps"></a>Komponentama koje su dio paketa Spark klaster?

Spark u HDInsight obuhvaća sljedeće komponente koje su dostupne na klastere prema zadanim postavkama.

- [Temeljni Spark](https://spark.apache.org/docs/1.5.1/). Obuhvaća Spark Core, Spark SQL, Spark strujanje API-ji, GraphX i MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livije](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter bilježnice](https://jupyter.org)

Spark u HDInsight omogućuje [ODBC upravljački program](http://go.microsoft.com/fwlink/?LinkId=616229) za povezivanje za klastere Spark u HDInsight s alatima za Poslovno obavještavanje kao što je Microsoft Power BI i Tableau.

## <a name="where-do-i-start"></a>Gdje pokrenuti?

Započnite sa stvaranjem Spark klaster na HDInsight Linux. U odjeljku [brzi početak rada: Stvaranje Spark klaster na HDInsight Linux i pokretanje uzorka aplikacije koje se koriste Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Daljnji koraci

### <a name="scenarios"></a>Scenariji

* [Spark bi: izvođenje analiza interaktivnih podataka pomoću Spark u HDInsight s alatima za Poslovno obavještavanje](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s strojnog učenja: korištenje Spark u HDInsight za analizu sastavnih temperatura pomoću HVAC podataka](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s strojnog učenja: korištenje Spark u HDInsight za predviđanje rezultata provjere za hranu](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark strujeće: Korištenje Spark u HDInsight za izgradnju u stvarnom vremenu strujanje aplikacije](hdinsight-apache-spark-eventhub-streaming.md)

* [Web-mjesto zapisnika analize pomoću Spark u HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Stvaranje i pokretanje aplikacija

* [Stvaranje samostalne aplikacije pomoću Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Pokretanje zadataka na Spark klaster pomoću Livije](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Alati i proširenja

* [Korištenje servisa HDInsight dodatak Alati za IntelliJ IDEJA za stvaranje i slanje Spark Scala aplikacije](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Korištenje servisa HDInsight dodatak Alati za IntelliJ IDEJA za ispravljanje pogrešaka aplikacije Spark daljinski](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Koristite Zeppelin bilježnice s Spark klaster na HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Dostupno za Jupyter bilježnicu u skupini Spark za HDInsight jezgre](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Korištenje vanjskih paketa s bilježnicama Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Na računalo instalirati Jupyter i povezati se HDInsight Spark klaster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Upravljanje resursima

* [Upravljanje resursima za klaster Apache Spark u Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Praćenje i ispravljanje pogrešaka zadataka izvodi na programa klaster Apache Spark u HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
