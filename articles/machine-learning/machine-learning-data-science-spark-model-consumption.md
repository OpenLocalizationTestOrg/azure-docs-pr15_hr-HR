<properties
    pageTitle="Rezultat Spark u komponenti strojno učenje modela | Microsoft Azure"
    description="Kako se rezultat učenje modela koji su pohranjeni u Azure Blob prostora za pohranu (WASB)."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="score-spark-built-machine-learning-models"></a>Rezultat Spark u komponenti strojno učenje modela 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

U ovoj se temi opisuje kako učitavanje strojno modela u učenje (ML) u koji ste je izgrađene pomoću Spark MLlib i pohranjene u spremište blobova na Azure (WASB) te kako ih rezultatu s skupove podataka koje se pohranjuju u WASB. Prikazuje se kako unaprijed obraditi ulaznih podataka, pretvaranje značajke pomoću funkcije indeksiranja i kodiranje u komplet alata za MLlib i kako stvoriti objekt označen kao točke podataka koje je moguće koristiti kao ulaz za bilježenje rezultata s modelima ML. Modeli koji se koriste za bilježenje rezultata obuhvaćaju linearne regresije, logistika regresije, Nasumični modela skupa stabala i prijelaza povećanje stabla modela.


## <a name="prerequisites"></a>Preduvjeti

1. Potreban je račun za Azure i programa HDInsight Spark koje treba li vam je 1.6 za HDInsight 3.4 Spark klaster da biste dovršili ovaj vodič. Pogledajte [Pregled sustava podataka znanstvenog pomoću Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) upute o tome kako zadovoljava te preduvjete. Tu temu sadrži opis NEW 2013 taksi podaci koji se koriste u nastavku i upute za izvršavanje koda iz Jupyter bilježnice na klasteru Spark. **PySpark-machine-learning-data-science-spark-model-consumption.ipynb** bilježnice koja sadrži primjere koda u ovoj temi dostupna je u [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Morate stvoriti i strojnog učenja modela da biste se testu dobije ovdje krenete kroz [Istraživanje podataka i Modeliranje s Spark](machine-learning-data-science-spark-data-exploration-modeling.md) temu.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Postavljanje: mjesta za pohranu, biblioteke i unaprijed Spark kontekstu

Spark je za čitanje i pisanje na programa Azure spremišta blobova (WASB). Da bi vaše postojeće podatke pohranjene postoji može obraditi pomoću Spark i ponovno pohranjene u WASB rezultate.

Da biste spremili modela ili datoteka u WASB, put mora biti naveden pravilno. Spremnik zadani priložiti klaster Spark možete se pozivati pomoću put počevši: *"wasb / /"*. Sljedećim primjerom koda određuje mjesto podataka za čitanje i put do direktorija za pohranu modela sprema izlaz modela. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Postavljanje putova direktorija za mjesta za pohranu u WASB

Modeli se spremaju u: "wasb: / / / korisnik/remoteuser/NYCTaxi/modelima". Ako ovaj put nije ispravno postavljena, modelima neće se učitati za bilježenje rezultata.

Scored rezultati su spremljeni u: "wasb: / / / korisnik/remoteuser/NYCTaxi/ScoredResults". Ako put do mape nije valjana, rezultat se ne spremaju u tu mapu.   


>[AZURE.NOTE] Put do mjesta datoteke možete kopirati i zalijepiti u rezerviranim mjestima u kod iz izlaza do posljednje ćelije **machine-learning-data-science-spark-data-exploration-modeling.ipynb** bilježnice.   


Evo kod da biste postavili putova direktorija: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**IZLAZ:**

DateTime.datetime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Uvoz biblioteke

Postavljanje konteksta spark i uvoz potrebne biblioteke s sljedeći kod

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Postavljanje konteksta Spark i PySpark magics

Jezgre PySpark koji se isporučuju sa Jupyter bilježnica imali unaprijed kontekst. Tako da ne morate postaviti na Spark ili razvijate grozd konteksta izričito prije početka rada s aplikacijom. To su dostupni za po zadanom. Ove konteksta su:

- SC - za Spark 
- sqlContext - za grozd

Otklanjanje PySpark sadrži neke unaprijed definirane "magics", koji su posebne naredbe da biste uputili poziv s %%. Postoje dvije takve naredbe koje se koriste u te primjere koda.

- **%% Lokalni** Navedeni se lokalno izvršava kod kontrola sadržaja sastavnog bloka. Šifra mora biti valjane Python kod.
- **%% sql -o<variable name>** 
- Izvršava upit grozd poslan na sqlContext. Ako je parametar -o prenosi, rezultat upita je ista i na na %% lokalne kontekst Python kao Pandas dataframe.
 

Za dodatne informacije o jezgre Jupyter bilježnice i na unaprijed definirane "magics" koji omogućuju, potražite u članku [jezgre dostupne za Jupyter bilježnice sa servisa HDInsight Spark Linux klastere na HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Ingest podataka i stvaranje sličicu očišćene podataka

Ova sekcija sadrži kod za niz zadatke koji su potrebni za ingest podatke koje želite da se testu dobije. Pročitajte u spojene uzorka 0,1% taksi putovanja i prijevoz datoteke (spremljen kao datoteku .tsv), oblikovanje podataka, a stvara okvira clean podataka.

Taksi putovanja i prijevoz datoteke su spajaju prema postupak naveden u u: [u tim znanstvenog proces podataka u akciji: korištenje klastere HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md) temu.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 46.37 sekundi


## <a name="prepare-data-for-scoring-in-spark"></a>Priprema podataka za bilježenje rezultata u Spark 

U ovom se odjeljku objašnjava indeks, kodiranje i promjena veličine categorical značajke pripremite ih za korištenje u MLlib supervised učenje algoritama za klasifikaciju i regresije.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Značajka transformacije: indeksa i kodiranje categorical značajke za unos u modelima za bilježenje rezultata 

U ovom se odjeljku objašnjava indeksirati categorical podataka pomoću programa `StringIndexer` i kodiranje značajke s `OneHotEncoder` unos u u modelima.

[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kodira niz stupac natpisa na stupac natpis indeksi. Indeksi su poredane po učestalosti natpis. 

[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mapira stupac indeksi natpisa stupca binarni vektori, s najviše jedne jedan-vrijednosti. Ovaj način kodiranja omogućuje algoritmima pomoću kojih očekivati neprekinuti vrijednosti značajke, kao što su logistika regresije, zatvoriti categorical značajke.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 5.37 sekundi


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Stvaranje objekata RDD uz značajku polja za unos u modelima

Ova sekcija sadrži kod koji pokazuje kako indeksirati categorical tekstne podatke kao objekt RDD i jedan tipkovne kodiranja tako da koristi za obuka i testirati MLlib logistika regresije i sustavom stabla modela. Indeksirani podaci se pohranjuju u objektima [Prebacuju raspodijeliti skup podataka (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) . Slijede osnovni apstrakcije u Spark. Objekt RDD predstavlja programa immutable, particioniranom zbirka elemenata koji mogu biti kojim upravlja na paralelno s Spark.

Također sadrži kod koji pokazuje kako mijenjati veličinu podataka s na `StandardScalar` nudi MLlib za korištenje u linearne regresije s Stochastic prijelaza Descent (SGD), popularne algoritam za obuku te širokog raspona strojnog učenja modela. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) koristi se za promjenu veličine značajke za varijancu jedinicu. Značajka skaliranja poznata i kao normalizacija podataka insures da značajke široko disbursed vrijednosti za koje su nisu navedeni viškom računa u funkciji ciljna. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 11.72 sekundi


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Rezultat s logistika Model regresije i spremanje Izlaz na bloba

Kod u ovom se odjeljku objašnjava Učitavanje modela logistika regresije koji je spremljen u spremište blobova platforme Azure i koristiti ga za predviđanje hoće li se savjet plaćenu na taksi putovanja, rezultat s metriku standardne klasifikacija pa Spremi i prikazati rezultate u spremište blobova platforme. Scored rezultate spremaju se u RDD objekte. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 19.22 sekundi


## <a name="score-a-linear-regression-model"></a>Rezultat Model linearna regresija

Da biste linearne regresije modela predviđanje količinu savjet plaćenu pomoću Stochastic prijelaza Descent (SGD) za optimizaciju uvježbati koristi [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) . 

Kod u ovom se odjeljku objašnjava Učitavanje modela linearne regresije iz spremišta blobova platforme Azure rezultatu pomoću skaliranu varijable, a zatim spremiti rezultate natrag blob-om.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 16.63 sekundi


## <a name="score-classification-and-regression-random-forest-models"></a>Rezultat klasifikaciju i regresije slučajni skupa stabala modeli

Kod u ovom se odjeljku objašnjava učitavanje spremljenu klasifikacija i regresije slučajni skupa stabala modelima spremljene u spremište blobova platforme Azure, rezultatu njihove performanse sa standardnom klasifikatora i regresije mjere, a zatim spremite rezultate natrag u spremište blobova platforme.

[Slučajno šuma](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) su ensembles odlukama.  Mogu kombinirati više odlukama da biste smanjili rizik od overfitting. Slučajni šuma možete rukovati categorical značajke proširivanje postavku multiclass klasifikacija ne zahtijeva značajka skaliranje te mogu da biste zabilježili koje nisu linearities i značajka interakcije. Slučajni šuma su jedna od najčešće uspješno strojnog učenja modela za klasifikaciju i regresije.

[spark.mllib](http://spark.apache.org/mllib/) podržava slučajni šuma binarnim i multiclass klasifikacije i regresije, pomoću značajke neprekinuti i categorical. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 31.07 sekundi


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Rezultat klasifikaciju i regresije prijelaza povećanje stabla modela

Kod u ovom se odjeljku objašnjava učitavanje klasifikaciju i regresije prijelaza povećanje stabla modela iz spremišta blobova platforme Azure, rezultatu njihove performanse sa standardnom klasifikatora i regresije mjere, a zatim spremite rezultate natrag u spremište blobova platforme. 

**spark.mllib** podržava GBTs za binarni klasifikaciju i regresije, pomoću značajke neprekinuti i categorical. 

[Povećanje stabala za prijelaza](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) su ensembles odlukama. GBTs uvježbati odlukama ponovno da biste minimizirali gubitka funkcija. GBTs možete rukovati categorical značajke, ne zahtijeva značajka skaliranje i mogu da biste zabilježili koje nisu linearities i značajka interakcije. I korištenja multiclass klasifikacija postavku.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 14.6 sekundi


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Čišćenje objekte iz memorije i ispis mjesta testu dobije datoteka

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**IZLAZ:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016 05 0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016 05 0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016 05 0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016 05 0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Korištenje modela Spark putem web-sučelja

Spark nudi mehanizam daljinski slanje obrade ili interaktivne upita putem sučelja za OSTALE s komponentom naziva Livije. Livije omogućena je prema zadanim postavkama na svoj klaster HDInsight Spark. Dodatne informacije o Livije potražite u članku: [Slanje Spark poslove daljinski pomoću Livije](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Koristite Livije daljinski slanje zadatak koji skupna brojčane pokazatelje datoteku koja je pohranjena u blobova platforme Azure i zatim piše rezultate u drugom blob. Da biste to učinili, prenesite skriptu Python iz  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) u blob klaster Spark. Da biste kopirali skriptu blob klaster možete koristiti alat kao što je **Microsoft Azure prostora za pohranu Explorer** ili **AzCopy** . U slučaju da naš ćemo prenijeti skripta ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] Tipke za pristup koja vam je potrebna možete pronaći na portalu za pohranu račun povezan s Spark klaster. 


Nakon prijenosa na to mjesto, Ova skripta se pokreće unutar klaster Spark raspodijeljeno kontekst. Učitava modela i pokrenuti predviđanja unos datoteke ovisno o modelu.  

Ova skripta možete pozvati daljinski uspostavljanjem jednostavne zahtjev HTTP/OSTALE na Livije.  Evo zakretanja naredbu za sastavljanje HTTP zahtjev za daljinsko pozivanje skripte Python. Zamijenite CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME odgovarajuće vrijednosti za svoj klaster Spark.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Možete koristiti bilo koji jezik na udaljeni sustav za pozivanje posao Spark kroz Livije uspostavljanjem jednostavne HTTPS poziva s osnovnu provjeru autentičnosti.   


>[AZURE.NOTE] Bio bi praktično koristiti biblioteku Python zahtjeve pri stvaranju ovog HTTP poziva, ali je trenutno nije instaliran po zadanom u funkcijama Azure. Da bi starije HTTP biblioteke koriste se umjesto toga.   


Evo Python kod za poziv HTTP:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Možete dodati i kod Python [Azure funkcije](https://azure.microsoft.com/documentation/services/functions/) da biste pokrenuli podneska posla na Spark da rezultati blob koji se temelji na različite događaje kao što su mjerača vremena, stvaranje ili ažuriranje blob. 

Ako biste radije sučelje za besplatni klijent kod, korištenje [Azure logike aplikacije](https://azure.microsoft.com/documentation/services/app-service/logic/) za pozivanje obradu Spark bilježenje rezultata koji definira HTTP akcije na **Logike aplikacije Designer** i postavljanjem parametara. 

- Azure, na portalu stvorite novu aplikaciju logike tako da odaberete **+ Novo** -> **Web + Mobile** -> **Logike aplikacije**. 
- Da biste otvorili **Dizajner logike aplikacije**, unesite naziv logike aplikacije i planiranje usluga aplikacije.
- Odaberite akciju HTTP, a zatim unesite parametre prikazano na sljedećoj slici:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Što je sljedeće? 

**Unakrsno provjere valjanosti i hyperparameter sweeping**: na kako može biti modela u odjeljku [Napredne Istraživanje podataka i Modeliranje s Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) put uvježbavan pomoću unakrsno provjere valjanosti i hyper parametar sweeping.
