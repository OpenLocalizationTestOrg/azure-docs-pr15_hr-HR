<properties
    pageTitle="Znanstvena podataka pomoću Scala i Spark na Azure | Microsoft Azure"
    description="Kako koristiti Scala za supervised strojnog učenja zadatke s Spark skalabilni MLlib i Spark ML pakete na programa Azure HDInsight Spark klaster."  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Znanstvena podataka pomoću Scala i Spark na Azure

U ovom se članku objašnjava korištenje Scala za supervised strojnog učenja zadataka s Spark skalabilni MLlib i Spark ML pakete na programa Azure HDInsight Spark klaster. Vodi vas kroz zadatke koje čine [postupak znanstvenog podataka](http://aka.ms/datascienceprocess): ingestion podataka i istraživanje, vizualizacija, značajka inženjering, Modeliranje i potrošnje modela. Modeli u članku obuhvaćaju logistika i linearne regresije, slučajni šuma i prijelaza boosted stabala (GBTs), uz dvije uobičajene zadatke supervised strojnog učenja:

- Regresije problem: predviđanje iznosa savjet ($) za taksi putovanja
- Binarni klasifikacija: predviđanje savjet ili za putovanje taksi bez savjet (1/0)

Postupak Modeliranje zahtijeva obuka i procjenu na test zadani skup podataka i odgovarajući točnost metriku. U ovom se članku možete saznati kako spremiti te modela u spremište blobova platforme Azure i rezultatu i procjenu njihove predvidljivu performanse. U ovom se članku opisuje i dodatne teme kako optimizirati modela pomoću unakrsno provjere valjanosti i hyper parametar sweeping. Podaci koji se koriste se uzorak 2013 NEW taksi putovanja i prijevoz skup podataka dostupni na GitHub.

[Scala](http://www.scala-lang.org/), jezika Java virtualni stroj na temelju integrira vezanima uz objekt i funkcionalne jezik koncepata. Je skalabilni jezik koji je dobro prikladniji raspodijeljeno obrade u oblak i pokreće na klastere Azure Spark.

[Spark](http://spark.apache.org/) je framework paralelno obrada za otvaranje izvora koji podržava obrada u memoriji da biste uvećali performanse velikih skupova podataka analize aplikacije. Obrada modul Spark ugrađena je za brzinu, lakšeg korištenja i složene analize. Mogućnosti u memoriji raspodijeljeno izračunavanje Spark na ga činite dobar izbor za izračun s iteracijama algoritama computations strojnog učenja i grafikonu. Paket [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) sastoji se od više razine API-ji ugrađeni preko podataka okvira koje omogućuju stvaranje i ugađanje praktično strojnog učenja kanali uniform skupa. [MLlib](http://spark.apache.org/mllib/) je biblioteka skalabilni strojnog učenja Spark korisnika, koji unosi mogućnosti modeliranja ovaj raspodijeljeno okruženje.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) je funkcija Azure hostira ovog Spark Otvori izvor. Također sadrži podršku za Jupyter Scala bilježnica na Spark klaster, a možete pokrenuti Spark SQL interaktivne upita pretvorba, filtriranje i vizualizaciju podataka pohranjenih u spremište blobova platforme Azure. Na Scala koda u ovom članku koje omogućuju rješenja te se navode odgovarajući iscrtavaju na vizualizaciju podataka pokrenuti u bilježnicama Jupyter instalirano klastere Spark. Modeliranje korake navedene u ovim temama imati kod koji pokazuje kako uvježbati, Analiza, spremanje i zauzeti svake vrste modela.

Korake za postavljanje i kod u ovom članku su za Azure HDInsight 3.4 Spark 1.6. Međutim, kod u ovom članku i u [Bilježnici Jupyter Scala](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) su generički i raditi na bilo kojem Spark klaster. Klaster korake postavljanja i upravljanje može se malo razlikovati od što je prikazano u ovom se članku ako se ne koriste HDInsight Spark.

> [AZURE.NOTE] Temu koja pokazuje kako koristiti Python umjesto Scala za izvršavanje zadataka za proces završetka do kraja znanstvenog podataka potražite u članku [Znanstvenog podataka pomoću Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Preduvjeti

-   Morate imati pretplatu na Azure. Ako već nemate onaj koji [se programa Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   Potreban vam je klaster Azure HDInsight 3.4 Spark 1.6 da biste dovršili sljedeće postupke. Da biste stvorili klaster, slijedite upute u odjeljku [Početak rada: Stvaranje Spark Apache na Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Postavljanje vrste klaster i verzija na izborniku **Odaberite vrstu klaster** .

![Konfiguriranje vrsta za HDInsight klaster](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Opis NEW taksi putovanja podatke i upute za izvršavanje koda iz bilježnice Jupyter na Spark klaster, potražite u članku odjeljke u [Pregled od podataka znanstvenog pomoću Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Izvršavanje kod Scala iz Jupyter bilježnice na Spark klaster

Možete pokrenuti Jupyter bilježnice na portalu Azure. Pronalaženje klaster Spark na nadzornu ploču, a zatim ga da biste unijeli na stranicu Upravljanje za svoj klaster. Nakon toga kliknite **Klaster nadzornih ploča**, a zatim **Jupyter bilježnice** da biste otvorili bilježnicu pridružene klaster Spark.

![Nadzorna ploča klaster i Jupyter bilježnica](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Također možete pristupiti Jupyter bilježnice na https://&lt;clustername&gt;.azurehdinsight.net/jupyter. Zamijenite *clustername* naziv svoj klaster. Potrebna je lozinka za administratorski račun za pristup bilježnicama Jupyter.

![Otvorite Jupyter bilježnice tako da koristi taj naziv klaster](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Odaberite **Scala** da biste vidjeli direktoriju nekoliko primjera unaprijed zadane pakete bilježnice koje pomoću PySpark API-JA. Modeliranje Istraživanje i Bodovanje pomoću Scala.ipynb bilježnice koja sadrži primjere koda ovaj paket Spark tema dostupna je na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Bilježnice izravno iz GitHub s poslužiteljem Jupyter bilježnice možete prenijeti na svoj klaster Spark. Na početnoj stranici Jupyter, kliknite gumb **Prenesi** . U eksploreru za datoteke zalijepite URL (neobrađenog sadržaja) GitHub Scala bilježnice, a zatim kliknite **Otvori**. Bilježnice Scala dostupna je na sljedeći URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Postavljanje: Zadana postava Spark i grozd konteksta Spark magics i Spark biblioteke

### <a name="preset-spark-and-hive-contexts"></a>Unaprijed Spark i grozd konteksta

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Jezgre Spark koji se isporučuju sa Jupyter bilježnice su unaprijed konteksta. Ne morate izričito postavljena na Spark ili razvijate grozd konteksta prije početka rada s aplikacijom. Unaprijed postavljeni konteksta su:

- `sc`za SparkContext
- `sqlContext`za HiveContext


### <a name="spark-magics"></a>Spark magics

Otklanjanje Spark sadrži neke unaprijed definirane "magics", koji su posebne naredbe da biste uputili poziv s `%%`. Dva te naredbe koristi u sljedeće primjere koda.

- `%%local`Određuje da će kod kontrola sadržaja sastavnog bloka lokalno se izvršava. Šifra mora biti valjane Scala kod.
- `%%sql -o <variable name>`izvršava grozd upita na temelju `sqlContext`. Ako u `-o` proslijeđen parametar, rezultat upita je ista i na na `%%local` Scala kontekst kao okvira Spark podataka.

Za dodatne informacije o jezgre za Jupyter bilježnica i njihovih unaprijed definirane "magics" koji poziva s `%%` (Ako, na primjer, `%%local`), potražite u članku [jezgre dostupne za Jupyter bilježnice sa servisa HDInsight Spark Linux klastere na HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Uvoz biblioteke

Uvoz u Spark i MLlib i u ostalim bibliotekama morat ćete putem sljedeći kod.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Ingestion podataka

U prvom koraku postupka znanstvenog podataka je ingest podatke koje želite analizirati. Prenijeti podatke iz vanjskih izvora ili sustavi kojoj se nalazi u okruženju Istraživanje i Modeliranje podataka. U ovom se članku ingest vam podaci spojene uzorka 0,1% taksi putovanja i prijevoz datoteke (spremljen kao datoteku .tsv). Okruženje za pretraživanje i Modeliranje podataka je Spark. Ova sekcija sadrži kod da biste dovršili sljedeće niz zadataka:

1. Postavljanje putova direktorija za pohranu podaci i model.
2. Pročitajte u skupu podataka za unos (spremljen kao datoteku .tsv).
3. Definiranje shema za podatke i očistiti podatke.
4. Stvaranje okvira očišćene podataka i predmemorije u memoriji.
5. Registrirajte se podatke kao privremena u SQLContext.
6. Upita u tablici, a zatim uvoz rezultate u okvir podataka.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Postavljanje putova direktorija za mjesta za pohranu u spremište blobova platforme Azure

Spark lakše čitali i pisali za spremište blobova platforme Azure. Možete upotrijebiti Spark za obradu postojeće podatke i ponovno spremiti rezultate u spremište blobova platforme.

Da biste spremili modela ili datoteka u spremište blobova platforme, morate je ispravno Navedite put. Referenca spremnik zadani priložene klaster Spark pomoću put koji počinju sa `wasb:///`. Upućuju na druga mjesta pomoću `wasb://`.

Sljedećim primjerom koda određuje mjesto ulaznih podataka za čitanje i put do spremište blobova platforme koja je priložena klaster Spark Spremanje modela.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Uvoz podataka, stvorite je RDD i definiranje okvira podataka prema shemi

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Izlaz:**

Vrijeme pokretanje ćeliju: 8 sekundi.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Upita u tablici, a zatim uvoz rezultata u okviru podataka

Nakon toga upita u tablici prijevoz, putnika i savjet podatke. Filtriranje podataka oštećena i vanjski; i ispis nekoliko redaka.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Izlaz:**

fare_amount|passenger_count|tip_amount|tipped
-----------|---------------|----------|------
       13.5|            1.0|       2.9|   1.0
       16.0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Istraživanje podataka i vizualizaciju

Nakon što u Spark unijeti podatke, na sljedeći korak u postupku znanstvenog podataka je da bi se dobio dublju razumijevanje podataka putem Istraživanje i vizualizaciju. U ovom ćete odjeljku pregledajte taksi podataka pomoću SQL upita. Nakon toga uvesti rezultate u okvira podataka da biste iscrtali cilj varijabli i mogućim značajke za vizualni provjere pomoću značajke automatskog vizualizacije Jupyter.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Iscrtavanje podataka pomoću lokalne i poseban SQL

Prema zadanim postavkama Izlaz iz bilo kojeg isječak koda koji se izvode iz bilježnice Jupyter je dostupna u kontekstu sesije koja je ista i na čvorove tempiranja. Ako želite spremiti putovanja čvorove tempiranja za svaku izračuni, a ako sve podatke potrebne za vaše izračuni nije dostupan lokalno na čvor poslužitelja Jupyter (što je glavni čvora), možete koristiti u `%%local` poseban pokretanje koda na poslužitelju Jupyter.

- **SQL poseban** (`%%sql`). Otklanjanje HDInsight Spark podržava jednostavno retku HiveQL upite odabiranja SQLContext. U (`-o VARIABLE_NAME`) argument nastavi izlaz SQL upita kao okvira Pandas podataka na poslužitelju Jupyter. To znači da će biti dostupna u načinu rada za lokalni.
- `%%local`**poseban**. Na `%%local` poseban lokalno pokreće kod na poslužitelju Jupyter, čvor glavni klaster HDInsight. Obično ćete koristiti `%%local` uz zajedno s na `%%sql` uz s na `-o` parametar. Na `-o` parametar bi zadržava izlaz SQL upita lokalno, a zatim `%%local` poseban želite pokrenuti sljedeći skup isječak koda da biste pokrenuli lokalno izlaz SQL upita koji je ista i lokalno.

### <a name="query-the-data-by-using-sql"></a>Podatke pomoću SQL upita
Ovaj upit dohvaća trips taksi prijevoz iznos, putnika count i iznos savjet.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

U sljedeći kod u `%%local` poseban stvara okvira lokalnih podataka i sqlResults. Pomoću sqlResults iscrtati pomoću matplotlib.

> [AZURE.TIP] Lokalni poseban koristi više puta u ovom članku. Ako je prevelik skupu podataka provjerite ogledne da biste stvorili okvir podataka koji stane u lokalnom memorije.

### <a name="plot-the-data"></a>Iscrtavanje podataka

Možete prikazati pomoću koda Python nakon okvira podataka u lokalnom kontekstu kao okvira Pandas podataka.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Kada pokrenete kod otklanjanje Spark automatski vizualiziraju izlaz upita SQL (HiveQL). Na raspolaganju vam je nekoliko vrsta vizualizacija:
 
- Tablica
- Tortnog grafikona
- Redak
- Područje
- Traka

Evo kod za iscrtavanje podataka:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Izlaz:**

![Savjet iznos histograma](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Savjet iznos po putnika count](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Savjet iznos iznosom prijevoz](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Stvaranje značajke pretvorbe značajke i Priprema podataka za unos u Modeliranje funkcije

Za funkcije utemeljen na stabla Modeliranje Spark ML i MLlib, imate Priprema ciljnog i značajke na različite tehnike, kao što su binning, indeksiranje, jedan tipkovne kodiranja i vectorization. Slijedi nekoliko postupaka za praćenje u ovom odjeljku:

1. Stvoriti novu značajku **binning** sata u memorijski blokovi promet vremena.
2. Primjena **indeksiranja i jedan tipkovne kodiranje** categorical značajke.
3. **Uzorak i Podijeli skup podataka** u obuka i testiranje razlomci.
4. **Navedite obuka varijabla i značajke**, zatim stvorite indeksiranih ili jedan tipkovne kodirani obuka i testiranje unos označen kao točku prebacuju distributed skupove podataka (RDDs) ili okvira podataka.
5. Automatski **Kategoriziraj i vectorize značajke i ciljnih web-mjesta** da biste koristili kao unosa modela strojnog učenja.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Stvoriti novu značajku binning sata u memorijski blokovi promet vremena

Kod prikazuje kako ga stvoriti novu značajku binning sata u memorijski blokovi vrijeme promet i predmemoriju okvira podataka stvorenom u memoriji. Gdje su RDDs i podataka iz okvira koristiti više puta, predmemoriranja potencijalne klijente poboljšane vremena izvođenja. Sukladno tome, ćete predmemoriju RDDs i okvira podataka u nekoliko fazama sljedećih postupaka.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indeksiranje i jedan tipkovne kodiranje categorical značajki

Za Modeliranje i predviđanje funkcije MLlib potrebne značajke s categorical ulaznih podataka indeksirana ili kodirani prije korištenja. U ovom se odjeljku objašnjava kazalo ili kodiranje categorical značajke za unos u Modeliranje funkcije.

Morate kazalo ili kodiranje vaše modela na različite načine, ovisno o modelu. Ako, na primjer, modelima logistika i linearne regresije potreban je jedan tipkovne kodiranja. Ako, na primjer, značajka s tri kategorije možete proširiti u tri stupca značajke. Svaki stupac će sadržavati 0 ili 1, ovisno o kategorija je opažanje. MLlib nudi funkciju [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) za jedan tipkovne kodiranja. Ovaj encoder mapira stupac natpis indeksi stupac binarni vektori s najviše jedne jedan-vrijednosti. S ovom kodiranje algoritmima pomoću kojih očekivati numeričke vrijednosti značajke, kao što su logistika regresije se mogu primijeniti categorical značajki.

Ovdje transformirati samo četiri varijable da bi se prikazala Primjeri niza znakova. Također možete indeksirati druge varijable, kao što su dan u tjednu, predstavlja numeričke vrijednosti, kao categorical varijabli.

Za indeksiranje, koristite `StringIndexer()`, i za jedan tipkovne kodiranje koristite `OneHotEncoder()` funkcija MLlib. Evo koda za indeksiranje i kodiranje categorical značajke:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Izlaz:**

Vrijeme pokretanje ćeliju: 4 sekunde.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Uzorak i podjelu skup podataka u obuka i testiranje razlomke

Kod stvara slučajno uzorkovanje podataka (25%, u ovom primjeru). Iako uzorkovanje nije potreban u ovom primjeru zbog Veličina skupa podataka, u članku prikazuje kako možete poslušajte tako da znate kako je koristiti za probleme sa svojom po potrebi. Kada velike uzorke, to možete uštedjeti vrijeme značajan dok uvježbati modela. Nakon toga podijelite uzorka obuka dijela (75%, u ovom primjeru) i testiranja dijela (25%, u ovom primjeru) u klasifikaciju i Modeliranje regresije.

Dodajte slučajni broj (između 0 i 1) za svaki redak (u stupcu "rand") koje je moguće koristiti da biste odabrali više valjanosti savijanje tijekom obuku.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Izlaz:**

Vrijeme pokretanje ćeliju: dvije sekunde.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Navedite varijablu obuka i značajke, a zatim stvorite indeksiranih ili jedan tipkovne kodirani obuka i testiranje unos označena okvira RDDs točke ili podataka

Ova sekcija sadrži kod koji pokazuje kako indeksirati categorical tekstne podatke kao vrstu podataka s natpisima točke i kodiranje da biste mogli koristiti obuka i testiranje MLlib logistika regresije i druge klasifikacije modela. Označena točka objekti su RDDs koje su oblikovane tako da se većina strojnog učenja algoritama u MLlib potrebno kao ulaznih podataka. Na [koje su označene točke](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je lokalni vektorski dense ili kratke, pridružene oznake/odgovor.

U kodu, navedite varijabla (zavisne) cilj i značajke za uvježbati modela. Pa stvorite indeksiranih ili jedan tipkovne kodirani obuka i testiranje unos označena RDDs točke ili podataka okvira.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Izlaz:**

Vrijeme pokretanje ćeliju: 4 sekunde.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automatski kategorizirati i vectorize značajke i ciljnih web-mjesta da biste koristili kao unosa strojnog učenja modela

Pomoću Spark ML cilj i značajke za korištenje u funkcijama utemeljen na stabla Modeliranje Kategoriziraj. Kod dovršava dvije stvari:

-   Stvara binarni cilj za klasifikaciju tako da svaka točka podataka između 0 i 1 dodijelite vrijednost 0 ili 1 pomoću vrijednosti praga od 0,5.
- Automatski kategorizira značajke. Ako je broj različitih numeričke vrijednosti za sve značajke manje od 32, ta značajka je kategorizirana.

Evo šifru za ove dvije stvari.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binarni klasifikacija modela: predviđanje hoće li se treba platiti savjet

U ovom ćete odjeljku stvoriti tri vrste modela binarni klasifikacija za predviđanje hoće li se treba platiti savjeta:

- **Model logistika regresije** pomoću Spark ML `LogisticRegression()` (funkcija)
- **Slučajni skupa stabala klasifikacije modela** pomoću Spark ML `RandomForestClassifier()` (funkcija)
- **Prijelaza povećanje stabla klasifikacija modela** pomoću u MLlib `GradientBoostedTrees()` (funkcija)

### <a name="create-a-logistic-regression-model"></a>Stvaranje modela logistika regresije

Nakon toga stvoriti model logistika regresije pomoću Spark ML `LogisticRegression()` (opis funkcije). Možete stvoriti model stvara kod u niz koraka:

1. Postavite **vlaku model** podataka s jednog parametra.
2. **Procijeni modela** na skupu podataka test s metriku.
3. **Spremite model** u spremište blobova platforme za buduće potrošnje.
4. **Rezultat modela** na temelju podataka za testiranje.
5. **Prikazati rezultate** s tekstnog okvira radi krivulje karakteristikama (ROC).

Evo kod te procedure:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Učitavanje, rezultat i spremiti rezultate.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Izlaz:**

ROC na podacima test = 0.9827381497557599


Crtanje krivulje ROC pomoću Python na lokalni okvira Pandas podataka.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Izlaz:**

![Savjet ili bez savjet ROC krivulje](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Stvaranje modela slučajni skupa stabala klasifikacija

Nakon toga stvoriti model klasifikacija slučajni skupa stabala pomoću Spark ML `RandomForestClassifier()` funkcija i procjenu model podataka za testiranje.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Izlaz:**

ROC na podacima test = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>Stvaranje modela GBT klasifikacija

Nakon toga stvoriti model klasifikacije GBT pomoću MLlib na `GradientBoostedTrees()` funkcija i procjenu model podataka za testiranje.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Izlaz:**

Područje u odjeljku ROC krivulje: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Model regresije: predviđanje savjet iznos

U ovom odjeljku možete stvoriti dvije vrste modela regresije za predviđanje iznos savjeta:

- Na **regularized linearna regresija modela** pomoću Spark ML `LinearRegression()` (opis funkcije). Uštedjet ćete modela i procjenu model podataka za testiranje.
- **Prijelaz povećanje model regresije stabla** pomoću Spark ML `GBTRegressor()` (opis funkcije).


### <a name="create-a-regularized-linear-regression-model"></a>Stvaranje modela regularized linearna regresija

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Izlaz:**

Vrijeme pokretanje ćeliju: 13 sekundi.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Izlaz:**

R-sqr na podacima test = 0.5960320470835743


Nakon toga upita rezultate testiranja kao okvira podataka, a zatim ga vizualizacija pomoću AutoVizWidget i matplotlib.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Kod stvara okvira lokalnih podataka iz izlaza upita i iscrtava podatke. Na `%%local` poseban stvara sličicu lokalnih podataka `sqlResults`, koji možete koristiti da biste iscrtali s matplotlib.

>[AZURE.NOTE] U ovom poseban Spark koristi više puta u ovom članku. Ako je velike količine podataka, trebali biste ogledne da biste stvorili okvir podataka koji stane u lokalnom memorije.

Stvaranje iscrtavaju pomoću Python matplotlib.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Izlaz:**

![Savjet iznos: stvarni i predviđene](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Stvaranje modela regresije GBT

Stvaranje modela regresije GBT pomoću Spark ML `GBTRegressor()` funkcija i procjenu model podataka za testiranje.

[Prijelaz boosted stabla](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) su ensembles odlukama. GBTs uvježbati odlukama ponovno da biste minimizirali gubitka funkcija. GBTs možete koristiti za regresije i klasifikaciju. Ih možete rukovati categorical značajke ne zahtijeva značajka skaliranje, a možete snimiti nonlinearities i interakcije značajke. I koristite ih u multiclass klasifikacija postavku.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Izlaz:**

Testiranje R-sqr je: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Napredno oblikovanje uslužni programi za optimizaciju

U ovom ćete odjeljku pomoću uslužni programi učenje strojno razvojnim inženjerima često koristite za optimizaciju modela. Konkretno, možete optimizirati strojnog učenja modelima tri različite načine pomoću parametar sweeping i unakrsno provjere valjanosti:

-   Podijelite podatke u vlaku i provjere valjanosti skupovima optimiziranje modela pomoću hyper parametar sweeping na skupu obuka i procjenu na skupu provjere valjanosti (linearna regresija)
-   Optimiziranje modela pomoću unakrsno provjere valjanosti i hyper-parametar sweeping pomoću funkcije CrossValidator Spark ML (binarni klasifikacije)
-   Optimiziranje modela pomoću prilagođenog koda unakrsno provjere valjanosti, a parametar sweeping da biste koristili neki strojnog učenja – opis funkcije i parametar skup (linearna regresija)


Tehnika koje assesses i kako će generalize model obučavanju članova na poznati skupa podataka za predviđanje značajke skupa podataka na kojem je nije uvježban je **Unakrsno provjere valjanosti** . Općenito ideja iza ove tehnike je da model je put uvježbavan na skupa podataka od poznatih podataka, a zatim se testira točnosti njegov predviđanja protiv neovisno skupa podataka. Uobičajeni implementaciju je podjele skupa podataka u *k*-savijanje pa uvježbati modela u način kružnog na sve osim jednog na savijanje.

**Optimizacija Hyper-parametar** je problem odabiru skup hyper-parametara za algoritam za učenje obično s ciljem optimiziranje mjera na algoritam performanse neovisno skupa podataka. Hyper-parametar je vrijednost koja morate navesti izvan obuka postupak modela. Pretpostavke o vrijednostima hyper parametar može utjecati na fleksibilnost i točnost modela. Odlukama hyper-parametre, koristite, na primjer, kao što su željeni dubine i broj ostavlja u stablu. Morate postaviti misclassification kazna termina za podršku vektorski računalo (SVM).

Uobičajeni način da biste izvršili optimizaciju hyper parametar je pomoću pretraživanja rešetke, naziva se i **parametar Počisti**. U rešetki pretraživanja, iscrpan pretraživanje provodi kroz vrijednosti podskup navedena hyper parametar prostora za učenje algoritam. Unakrsno valjanosti možete navesti metrike performanse da biste sortirali out optimalne rezultate koje je stvorio algoritam pretraživanje rešetke. Ako koristite više valjanosti hyper parametar sweeping, može pomoći ograničenje probleme kao što su overfitting model za obuku podataka. Na taj način model zadržava kapaciteta da biste primijenili Općenito skupa podataka iz kojeg je izvedena obuka podataka.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Optimiziranje modela linearne regresije s sweeping hyper parametra

Nakon toga podijelite podataka u vlaku i provjere valjanosti skupovima, korištenje hyper-parametar sweeping na skupu obuka za optimiziranje modela i procjenu na skupu provjere valjanosti (linearna regresija).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Izlaz:**

Testiranje R-sqr je: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Optimiziranje modela binarni klasifikacija pomoću unakrsno provjere valjanosti i hyper parametar sweeping

U ovom se odjeljku objašnjava optimizirati model binarni klasifikacije pomoću unakrsno provjere valjanosti i hyper parametar sweeping. Pomoću Spark ML `CrossValidator` (opis funkcije).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Izlaz:**

Vrijeme pokretanje ćeliju: 33 sekundi.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Optimiziranje modela linearne regresije pomoću prilagođenog koda unakrsno provjere valjanosti, a parametar sweeping

Nakon toga optimiziranje modela pomoću prilagođenog koda, a pomoću kriterija najveće točnost prepoznavanja najbolje modela parametre. Pa stvorite konačni model, procijeniti modela test podataka i spremite model u spremište blobova platforme. Na kraju, Učitavanje modela, rezultatu test podataka i procjenu točnost.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Izlaz:**

Vrijeme pokretanje ćeliju: 61 sekundi.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Korištenje Spark u komponenti strojnog učenja modela automatski pomoću Scala

Pregled tema koje će vas voditi kroz zadatke koje čine postupka znanstvenog podataka u Azure, potražite u članku [Timu podataka znanstvenog postupak](http://aka.ms/datascienceprocess).

[Timu podataka znanstvenog procese](data-science-process-walkthroughs.md) opisuju druge završetka do kraja vodiči kojima se ilustrira koraka u postupku znanstvenog timu podataka za nekim konkretnim scenarijima. Prikazi prikazuju i upute za kombiniranje oblaka i lokalnog alate i usluge u tijeku rada ili kanal za stvaranje Inteligentna aplikacije.

[Spark u komponenti rezultat strojnog učenja modelima](machine-learning-data-science-spark-model-consumption.md) pokazuje kako pomoću koda Scala automatski učitati i rezultatu novi skupovi podataka s strojnog učenja modelima ugrađena Spark i spremljene u spremište blobova platforme Azure. Slijedite upute postoji i samo zamijeniti kod Python Scala koda u ovom članku automatiziranog potrošnje.
