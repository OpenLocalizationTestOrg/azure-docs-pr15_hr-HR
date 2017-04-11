<properties 
    pageTitle="Vanjski pakete koristiti s bilježnicama Jupyter u klastere Apache Spark na HDInsight | Azure"
    description="Detaljne upute za konfiguriranje Jupyter bilježnica dostupna uz klastere HDInsight Spark da biste koristili vanjske Spark paketa." 
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
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Korištenje vanjskih paketa s Jupyter bilježnica u klastere Apache Spark na HDInsight Linux

>[AZURE.NOTE] Ovaj se članak odnosi primjenjivo Spark 1.5.2 na 3,3 HDInsight i Spark 1.6.2 na HDInsight 3.4. 

Saznajte kako konfigurirati Jupyter bilježnicu u skupini Apache Spark na HDInsight (Linux) da biste koristili vanjske, zajednice pridonio paketa koji nisu obuhvaćeni Izlaz u-tvorničke klaster. 

Možete pretraživati [spremište Maven](http://search.maven.org/) za popis svih paketa koji su dostupni. Popis dostupnih paketa možete dobiti i iz drugih izvora. Na primjer, popis svih paketa zajednice pridonio dostupna je na [Spark paketa](http://spark-packages.org/).

U ovom se članku će Saznajte kako pomoću značajke pakiranja [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) s bilježnicom Jupyter.

##<a name="prerequisites"></a>Preduvjeti

Morate imati sljedeće:

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Upute potražite u članku [Stvaranje Spark Apache klastere u Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Korištenje vanjskih paketa s bilježnicama Jupyter 

1. S [Portala za Azure](https://portal.azure.com/), iz startboard, kliknite pločicu za svoj klaster Spark (Ako ste ga prikvačiti na startboard). Možete se kretati svoj klaster u odjeljku **Pregledaj sve** > **Klastere HDInsight**.   

2. Iz klaster plohu Spark kliknite **Brze veze**, pa iz plohu **Nadzorne ploče klaster** **Jupyter bilježnice**. Ako se to od vas zatraži, unesite administratorskih vjerodajnica za klaster.

    > [AZURE.NOTE] Možda dosegne i Jupyter bilježnicu za svoj klaster tako da otvorite sljedeći URL u pregledniku. Zamijenite __CLUSTERNAME__ pod nazivom svoj klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Stvaranje nove bilježnice. Kliknite **Novo**, a zatim kliknite **Spark**.

    ![Stvaranje nove bilježnice Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Stvaranje nove bilježnice Jupyter")

3. Nova bilježnica se stvara i otvara pod nazivom Untitled.pynb. Kliknite naziv bilježnice na vrhu, a zatim upišite neslužbeni naziv.

    ![Osiguraj naziv bilježnice] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Osiguraj naziv bilježnice")

4. Koju ćete koristiti u `%%configure` poseban da biste konfigurirali bilježnicu koju ćete koristiti vanjski paketa. U bilježnicama koje koriste vanjske paketa, provjerite je li pozovete na `%%configure` uz u prvu ćeliju kod. Na taj način Jezgra sustava konfiguriran za korištenje paketa prije pokretanja sesiju.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Ako zaboravite da biste konfigurirali Jezgra sustava u prvu ćeliju, možete koristiti u `%%configure` s na `-f` parametar, ali će se pokrenuti sesiju i izgubit će se sve napredak.

5. U isječak gornjeg `packages` očekuje popis maven koordinate u središnjem spremniku Maven. U ovom isječak `com.databricks:spark-csv_2.10:1.4.0` je koordinatnog maven paketa **spark csv** . Evo kako stvoriti koordinate za paket.

    na. Pronađite paketa u spremištu Maven. Za ovaj vodič koristimo [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. U spremištu prikupite vrijednosti za **GroupId**, **ArtifactId**i **verziju**.

    ![Korištenje vanjskih paketa s Jupyter bilježnice] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Korištenje vanjskih paketa s Jupyter bilježnice")

    c. CONCATENATE tri vrijednosti odvojene zarezom (****:).

        com.databricks:spark-csv_2.10:1.4.0

6. Pokrenite kod ćeliju koja sadrži na `%%configure` poseban. To će konfigurirati podlozi Livije sesiju da bi pomoću značajke pakiranja koje ste naveli. U sljedećim ćelijama u bilježnici, sada možete koristiti paket, kao što je prikazano u nastavku.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Zatim pokretanja isječci, poput što je prikazano ispod, da biste prikazali podatke iz dataframe koji ste stvorili u prethodnom koraku.

        df.show()

        df.select("Time").count()


## <a name="seealso"></a>Vidi također


* [Pregled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

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

* [Na računalo instalirati Jupyter i povezati se HDInsight Spark klaster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Upravljanje resursima

* [Upravljanje resursima za klaster Apache Spark u Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Praćenje i ispravljanje pogrešaka zadataka izvodi na programa klaster Apache Spark u HDInsight](hdinsight-apache-spark-job-debugging.md)
