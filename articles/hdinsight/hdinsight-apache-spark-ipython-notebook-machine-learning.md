<properties 
    pageTitle="Koristite Apache Spark da biste sastavili strojnog učenja aplikacije na HDInsight | Microsoft Azure" 
    description="Upute za korištenje bilježnice s Apache Spark da biste sastavili strojnog učenja aplikacije" 
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
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Izraditi strojnog učenja aplikacije da biste pokrenuli za klastere Apache Spark na HDInsight Linux

Saznajte kako izraditi strojnog učenja aplikacije pomoću programa klaster Apache Spark u HDInsight. U ovom se članku prikazuje kako koristiti bilježnicu Jupyter dostupna uz klaster omogućuje stvaranje i naše aplikacija za testiranje. Aplikacija koristi ogledni podaci HVAC.csv koji je dostupan na sve klastere prema zadanim postavkama.

**Preduvjeti:**

Morate imati sljedeće:

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Upute potražite u članku [Stvaranje Spark Apache klastere u Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Pokaži mi podataka

Prije nego što smo počeli raditi u aplikaciji, Javite nam razumjeti strukturu podataka i vrste analiza moramo na podacima. 

U ovom se članku koristimo oglednu datoteku **HVAC.csv** podataka koja je dostupna u račun za Azure prostora za pohranu koji pridružene klaster HDInsight. Subjekta prostora za pohranu datoteka je na **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Preuzmite i otvorite CSV datoteku da biste snimku podataka.  

![Snimka HVAC podataka] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Brza snimka HVAC podataka")

Podatke prikazuje temperatura cilj i stvarne temperatura zgrade koja ima HVAC sustavi instaliran. Recimo da stupac **sustava** predstavlja ID sustava, a stupac **SystemAge** broj godina koje je sustav HVAC na mjestu na building.

Tih podataka omogućujemo za predviđanje hoće li zgrade biti hotter ili colder na temelju temperature cilj uz zadani ID sustava i dob sustava.

##<a name="app"></a>Pisanje aplikacije za učenje računala pomoću Spark MLlib

U ovoj aplikaciji koristimo Spark ML kanal za izvođenje klasifikacija dokumenta. U kanalu, ne možemo dokument podijelite riječi, riječi pretvoriti numeričke značajka vektor i na kraju izraditi predviđanje modela pomoću značajke vektori i natpise. Izvršite sljedeće korake da biste stvorili aplikacije.

1. S [Portala za Azure](https://portal.azure.com/), iz startboard, kliknite pločicu za svoj klaster Spark (Ako ste ga prikvačiti na startboard). Možete se kretati svoj klaster u odjeljku **Pregledaj sve** > **Klastere HDInsight**.   

2. Iz klaster plohu Spark kliknite **Nadzorna ploča klaster**, a zatim **Jupyter bilježnice**. Ako se to od vas zatraži, unesite administratorske vjerodajnice za klaster.

    > [AZURE.NOTE] Možda dosegne i Jupyter bilježnicu za svoj klaster tako da otvorite sljedeći URL u pregledniku. Zamijenite __CLUSTERNAME__ pod nazivom svoj klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Stvaranje nove bilježnice. Kliknite **Novo**, a zatim kliknite **PySpark**.

    ![Stvaranje nove bilježnice Jupyter] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Stvaranje nove bilježnice Jupyter")

3. Nova bilježnica se stvara i otvara pod nazivom Untitled.pynb. Kliknite naziv bilježnice na vrhu, a zatim upišite neslužbeni naziv.

    ![Osiguraj naziv bilježnice] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Osiguraj naziv bilježnice")

3. Budući da ste stvorili pomoću otklanjanje PySpark bilježnice, ne morate izričito stvaranje sve konteksta. Konteksta Spark i grozd će se automatski stvara za vas kada pokrenete prvu ćeliju kod. Možete započeti tako da uvezete vrste koji su potrebni za taj scenarij. Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Morate sada učitavanje podataka (hvac.csv), raščlaniti je i pomoću njega možete uvježbati modela. To, definirajte funkciju koja provjerava je li stvarni temperatura zgrade veći od temperatura cilj. Ako stvarni temperatura veća, building je tipkovni, označena tako da vrijednost **1.0**. Ako stvarni temperatura je manje, building je Hladna, označena tako da vrijednost **0.0**. 

    Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Konfiguriranje kanal za učenje strojno Spark koji se sastoji od tri faze: izrađivača tokena, hashingTF i lr. Dodatne informacije o novostima u kanal i načina funkcioniranja tijeka rada potražite u članku <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark strojnog učenja kanal</a>.

    Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Prilagodi kanal za dokument za obuku. Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**.

        model = pipeline.fit(training)

7. Provjerite je li dokument za obuku za Kontrolna točka tijek rada s aplikacijom. Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**.

        training.show()

    To mora dati izlaz sličnu ovoj:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Vratite i provjerite je li izlaz protiv neobrađenog CSV datoteke. Ako, na primjer, prvi redak CSV datoteka sadrži ove podatke:

    ![Snimka HVAC podataka] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Brza snimka HVAC podataka")

    Obratite pozornost na to kako stvarni temperatura manja od na ciljnom temperatura predlaganja building je Hladna. Dakle u rezultatu obuka **natpis** u prvom retku je vrijednost **0.0**, što znači da je sastavni tipkovni.

8.  Priprema skupa podataka da biste pokrenuli obučeni modela odnosu. Da biste to učinili, ne možemo bi prosljeđujete ID sustava i sustava starost (označen kao **SystemInfo** za obuku Izlaz), a model bi predviđanje li sastavnih s tom ID sustava i dob sustava bio bi hotter (označena 1.0) ili hladnijom (označena 0,0).

    Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Na kraju omogućite predviđanja podataka za testiranje. Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Trebali biste vidjeti na Izlaz sličnu ovoj:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    U prvom retku u predviđanje, vidjet ćete da za sustav HVAC s ID-a 20 i starost sustava 25 godina, building će biti tipkovni (**predviđanje = 1.0**). Prva vrijednost za DenseVector (0.49999) odgovara predviđanje 0.0 i druge vrijednosti (0.5001) odgovara predviđanje 1.0. U rezultatu, čak i ako je druga vrijednost samo marginally veći modela prikazuje **predviđanje = 1.0**.

11. Nakon što dovršite pokrenuti aplikaciju, trebali biste zatvaranja bilježnice da biste predali resurse. Da biste to učinili, na izborniku **datoteka** bilježnicu, kliknite **Zatvori i zaustavili**. Ovaj će zatvaranja i Zatvori bilježnicu.
           

##<a name="anaconda"></a>Korištenje Anaconda scikit – Saznajte biblioteku za učenje za računala

Klastere Apache Spark na HDInsight obuhvaćaju Anaconda biblioteke. To obuhvaća u **scikit-dodatne** biblioteka za strojnog učenja. Biblioteku obuhvaća i različite skupove podataka koje možete koristiti da biste sastavili probne aplikacije izravno iz Jupyter bilježnice. Primjer korištenjem na scikit – Saznajte biblioteku, pročitajte članak [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Vidi također

* [Pregled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariji

* [Spark bi: izvođenje analiza interaktivnih podataka pomoću Spark u HDInsight s alatima za Poslovno obavještavanje](hdinsight-apache-spark-use-bi-tools.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
