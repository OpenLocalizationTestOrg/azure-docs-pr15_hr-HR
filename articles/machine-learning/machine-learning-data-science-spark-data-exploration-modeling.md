<properties
    pageTitle="Istraživanje podataka i Modeliranje s Spark | Microsoft Azure"
    description="Showcases podataka Istraživanje i Modeliranje mogućnosti komplet alata za Spark MLlib."
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

# <a name="data-exploration-and-modeling-with-spark"></a>Istraživanje podataka i Modeliranje s Spark

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ovaj vodič koristi HDInsight Spark za istraživanje podataka i binarni klasifikacija regresije Modeliranje zadataka na primjer u NEW taksi putovanja i prijevoz 2013 skup podataka.  Vodi vas kroz korake [Postupka znanstvenog podataka](http://aka.ms/datascienceprocess), krajnji do kraja, pomoću programa HDInsight Spark skupine za obradu i Azure blob-ova za pohranu podataka i u modelima. Postupak istražuje i vizualiziraju podataka nastalih iz programa Azure spremišta blobova i Priprema podataka za izgradnju predvidljivu modela. Ove modeli su međuverzije korištenje kompleta alata za Spark MLlib da biste učinili binarni klasifikaciju i regresije Modeliranje zadatke.

- **Binarni klasifikacija** zadatak je za predviđanje hoće li je savjet plaćenu na putovanja. 
- Zadatak **regresije** je za predviđanje iznos koji se temelji na druge značajke savjet savjet. 

Modeli koristimo obuhvaćaju logistika i linearne regresije, slučajni šuma i prijelaza boosted stabala:

- [Linearna regresija s SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) je linearna regresija modelu koji koristi metodu Stochastic prijelaza Descent (SGD) i skaliranje za predviđanje iznosi savjet platiti za optimizaciju i značajke. 
- [Logistika regresije s LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) ili "logit" regresije je regresijskog modela koje je moguće koristiti kada je zavisne varijable categorical učiniti klasifikacije podataka. LBFGS je postupak Novak optimizaciju algoritam koji približan algoritam Broyden – Fletcher – Goldfarb – Shanno (BFGS) pomoću ograničeno memorijom računala i široko koja se koristi u strojnog učenja.
- [Slučajno šuma](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) su ensembles odlukama.  Mogu kombinirati više odlukama da biste smanjili rizik od overfitting. Slučajni šuma se koriste za regresije i klasifikaciju i može rukovati categorical značajke i može se produljiti multiclass klasifikacija postavku. Ne zahtijeva značajka skaliranje i mogu da biste zabilježili koje nisu linearities i značajka interakcije. Slučajni šuma su jedna od najčešće uspješno strojnog učenja modela za klasifikaciju i regresije.
- [Prijelaz boosted stabla](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) su ensembles odlukama. GBTs uvježbati odlukama ponovno da biste minimizirali gubitka funkcija. GBTs se koriste za regresije i klasifikaciju možete rukovati categorical značajke, ne zahtijeva značajka skaliranje i mogu da biste zabilježili koje nisu linearities i značajka interakcije. I korištenja multiclass klasifikacija postavku.

Koraci za Modeliranje sadrže i kod koji se pokazuje kako uvježbati, analiza i spremite svake vrste modela. Python korišten za rješenja s kodom i da bi se prikazala odgovarajući crtanje.   


>[AZURE.NOTE] Iako komplet alata za Spark MLlib je kompatibilan na velikih skupova podataka, relativno malog oglednog (pomoću 170K retke, otprilike 0,1% izvorne dataset NEW ~ 30 Mb) koristi se ovdje pogodnost. Vježbu navedene ovdje pokreće učinkovito (u oko 10 minuta) programa HDInsight klaster s 2 čvorove tempiranja. Istu šifru s manji izmjene se može koristiti za obradu veće-skupova podataka, s odgovarajućim izmjene za predmemoriranje podataka u memoriji i promjenu klaster.

## <a name="prerequisites"></a>Preduvjeti

Potreban je račun za Azure i programa HDInsight Spark koje treba li vam je 1.6 za HDInsight 3.4 Spark klaster da biste dovršili ovaj vodič. Pogledajte [Pregled sustava podataka znanstvenog pomoću Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md) upute o tome kako zadovoljava te preduvjete. Tu temu sadrži opis NEW 2013 taksi podaci koji se koriste u nastavku i upute za izvršavanje koda iz Jupyter bilježnice na klasteru Spark. **PySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb** bilježnice koja sadrži primjere koda u ovoj temi dostupna je u [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark). 


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Postavljanje: mjesta za pohranu, biblioteke i unaprijed Spark kontekstu

Spark je za čitanje i pisanje Azure spremišta blobova (poznat i kao WASB). Da bi vaše postojeće podatke pohranjene postoji može obraditi pomoću Spark i ponovno pohranjene u WASB rezultate.

Da biste spremili modela ili datoteka u WASB, put mora biti naveden pravilno. Spremnik zadani priložiti klaster Spark možete se pozivati pomoću put počevši: "wasb: / / /". Se pozivaju na druga mjesta "wasb: / /".


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Postavljanje putova direktorija za mjesta za pohranu u WASB

Sljedećim primjerom koda određuje mjesto podataka za čitanje i put do direktorija za pohranu modela sprema izlaz modela:


    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Uvoz biblioteke

Postavljanje zahtijeva i uvoz potrebne biblioteke. Postavljanje konteksta spark i uvoz potrebne biblioteke s sljedeći kod:


    # IMPORT LIBRARIES
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

Jezgre PySpark koji se isporučuju sa Jupyter bilježnica imali unaprijed kontekst. Tako da ne morate postaviti na Spark ili razvijate grozd konteksta izričito prije početka rada s aplikacijom. Ove konteksta dostupnih za prema zadanim postavkama. Ove konteksta su:

- SC - za Spark 
- sqlContext - za grozd

Otklanjanje PySpark sadrži neke unaprijed definirane "magics", koji su posebne naredbe da biste uputili poziv s %%. Postoje dvije takve naredbe koje se koriste u te primjere koda.

- **%% Lokalni** Određuje kod kontrola sadržaja sastavnog bloka želite izvršiti lokalno. Šifra mora biti valjane Python kod.
- **%%sql -o <variable name>** Izvršava upit grozd poslan na sqlContext. Ako je parametar -o prenosi, rezultat upita je ista i na na %% lokalne kontekst Python kao Pandas DataFrame.
 

Za dodatne informacije o jezgre Jupyter bilježnice i na unaprijed definirane "magics" koji omogućuju, potražite u članku [jezgre dostupne za Jupyter bilježnice sa servisa HDInsight Spark Linux klastere na HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).
 

## <a name="data-ingestion-from-public-blob"></a>Ingestion podatke iz javne blobova platforme

Prvi korak u postupku znanstvenog podataka je ingest podatke koje želite analizirati i iz izvora gdje se nalazi u okruženju Istraživanje i Modeliranje podataka. Okruženje je Spark u prikazu. Ova sekcija sadrži kod da biste dovršili niz zadataka:

- ingest uzorak podataka da biste se katalog modeliran
- Pročitajte u unos dataset (spremljen kao datoteku .tsv)
- Oblikovanje i čišćenje podataka
- Stvaranje i predmemoriju objekata (RDDs ili podataka okvira) u memoriji
- Registrirajte u tablici temp u SQL kontekstu.

Evo šifru za ingestion podataka.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
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
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
    
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 51.72 sekundi


## <a name="data-exploration--visualization"></a>Istraživanje podataka i vizualizaciju

Kada podatke sadrži su uvezeni u Spark, na sljedeći korak u postupku znanstvenog podataka je da bi se dobio dublju razumijevanje podataka putem Istraživanje i vizualizaciju. U ovom ćete odjeljku smo pregledajte podataka taksi pomoću SQL upita te iscrtajte cilj varijabli i mogućim značajke za vizualni provjere. Konkretno, ne možemo iscrtati učestalost putnika broji trips taksi, učestalost savjet iznosi i kako savjeti razlikuju se po iznosa i vrsti.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Iscrtajte histogram putnika Brojanje učestalosti u uzorku taksi trips

Ovaj kod i kasnije isječci pomoću poseban SQL upita za uzorak i lokalne poseban za iscrtavanje podataka.

- **Poseban SQL (`%%sql`)** Otklanjanje HDInsight PySpark podržava jednostavno retku HiveQL upite odabiranja na sqlContext. Na (-o VARIABLE_NAME) argument nastavi izlaz SQL upita kao Pandas DataFrame na poslužitelju Jupyter. To znači da je dostupan u načinu rada za lokalni.
- U ** `%%local` poseban** koristi se za pokretanje koda lokalno na poslužitelju Jupyter, headnode klaster HDInsight. Obično ćete koristiti `%%local` uz zajedno s na `%%sql` uz -o parametra. Parametar -o bi zadržava izlaz SQL upita lokalno i zatim %% lokalne poseban želite pokrenuti sljedeći skup isječak koda da biste pokrenuli lokalno izlaz SQL upita koji je ista lokalno i

Izlaz automatski vizualizacije kada pokrenete kod.

Ovaj upit dohvaća trips po putnika count. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Kod stvara lokalne okvira podataka iz izlaza upita i iscrtava podatke. Na `%%local` poseban stvara lokalne podatke – okvira, `sqlResults`, koja se koristi za iscrtavanje s matplotlib. 

>[AZURE.NOTE] U ovom poseban PySpark koristi više puta u prikazu. Ako je velike količine podataka, trebali biste ogledne da biste stvorili podataka više okvira koji stane u lokalnom memorije.

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Evo kod da biste iscrtali trips po putnika broji

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**IZLAZ:**

![Učestalost putovanja po putnika count](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Možete odabrati jednu od nekoliko različitih vrsta vizualizacija (tablice, tortni, linijski, područje ili trake) pomoću gumba izbornika **Vrsta** u bilježnici. Traka crtanja je prikazano ovdje.
    
### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Iscrtajte histogram savjet iznosi i kako savjet iznos razlikuje putnika count i prijevoz iznosi.

Pomoću SQL upita na ogledne podatke.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE
    
    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Tu ćeliju kod koristi SQL upita da biste stvorili tri iscrtavaju podatke.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**IZLAZ:** 

![Savjet Distribucija iznosa](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Savjet iznos po putnika count](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Savjet iznos iznosom prijevoz](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Značajka Priprema inženjering, transformaciju i podataka za Modeliranje
U ovom se odjeljku opisuje i sadrži kod postupak koji se koristi za Priprema podataka za korištenje u Modeliranje ML. Prikazuje kako obaviti sljedeće zadatke:

- Stvoriti novu značajku binning sata u memorijski blokovi promet vremena
- Indeksiranje i kodiranje categorical značajke
- Stvaranje objekata označene točke za unos u ML funkcije
- Stvaranje slučajno uzorkovanje dodatni podaci, a zatim podijelite u obuka i testiranje skupova
- Značajka skaliranja
- Predmemorija objekata u memoriji


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Stvoriti novu značajku binning sata u memorijski blokovi promet vremena

Kod prikazuje kako stvoriti novu značajku binning sata u memorijski blokovi promet vrijeme, a zatim kako predmemoriju okvira podataka stvorenom u memoriji. Gdje je prebacuju Distributed skupove podataka (RDDs) i podataka okvira koriste više puta, predmemoriranja vodi poboljšane vremena izvođenja. Sukladno tome, ne možemo predmemoriju RDDs i podataka okvira na nekoliko faza u prikazu. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**IZLAZ:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Indeksiranje i kodiranje categorical značajke za unos u Modeliranje funkcije

U ovom se odjeljku objašnjava indeksa ili kodiranje categorical značajke za unos u Modeliranje funkcije. Za Modeliranje i predviđanje funkcije MLlib potrebne značajke s categorical ulaznih podataka indeksirana ili kodirani prije korištenja. Ovisno o modelu, morate kazalo ili ih kodiranje na različite načine:  

- **Modeliranje utemeljen na stabla** zahtijeva kategorije da biste kodirati kao numeričke vrijednosti (na primjer, značajka s tri kategorije mogu kodirati s 0; 1; 2). To je nudi funkcija [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) MLlib korisnika. Ova funkcija kodira niz stupac natpisa na stupac indeksi oznake koje su poredane po učestalosti natpis. Iako indeksirana s numeričke vrijednosti za unos i obradu podataka, možete navesti algoritama utemeljen na stabla ih pravilno tretira kao kategorije. 

- **Logistic i linearna regresija modeli** zahtijevaju jedan tipkovne kodiranja, mjesto, na primjer, možete proširiti značajka s tri kategorije u tri stupca značajka svaki koji sadrže 0 ili 1, ovisno o kategorija je opažanje. MLlib nudi [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funkcija učiniti jednu tipkovne kodiranja. Ovaj encoder mapira stupac natpis indekse stupac binarni vektori, s najviše jedne jedan-vrijednosti. Ovaj način kodiranja omogućuje algoritmima pomoću kojih očekivati numeričke vrijednosti značajke, kao što su logistika regresije, zatvoriti categorical značajke.

Evo koda za indeksiranje i kodiranje categorical značajke:


    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 1.28 sekundi

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Stvaranje objekata označene točke za unos u ML funkcije

Ova sekcija sadrži kod koji pokazuje kako indeksirati categorical tekstne podatke kao vrstu podataka s natpisima točke i kodiranje tako da se koristi za obuka i testirati MLlib logistika regresije i druge klasifikacije modela. Označena točka objekti su prebacuju Distributed skupove podataka (RDD) oblikovano tako da se većina ML algoritama u MLlib potrebno kao ulaznih podataka. Na [koje su označene točke](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) je lokalni vektorski dense ili kratke, pridružene oznake/odgovor.  

Ova sekcija sadrži kod koji pokazuje kako indeksirati categorical tekstne podatke u obliku [s natpisom točke](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) podataka i kodiranje tako da se koristi za obuka i testirati MLlib logistika regresije i druge klasifikacija modela. Označena točka objekti su prebacuju Distributed skupove podataka (RDD) koji se sastoji od natpis (cilj/odgovor varijabla) i značajka vektorski. Ovaj oblik potreban je kao ulaz mnogo algoritama ML u MLlib.

Evo koda za indeksiranje i kodiranje teksta značajke za klasifikaciju binarni.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Evo koda za kodiranje i indeksirati značajki categorical teksta za analizu linearne regresije.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Stvaranje slučajno uzorkovanje dodatni podaci, a zatim podijelite u obuka i testiranje skupova

Kod stvara slučajno uzorkovanje podataka (ovdje služi 25%). Iako nije obavezno u ovom primjeru zbog veličine skupu podataka, ne možemo prikazuju kako vam može poslušajte ovdje da biste znali kako koristiti vlastiti problema po potrebi. Kada su velike uzorke, to možete spremiti značajan vremena tijekom obuka modela. Sljedeće smo podijelite uzorka obuka dio (ovdje 75%) i testiranja dijela (ovdje 25%) u klasifikacije i Modeliranje regresije.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 0.24 sekundi


### <a name="feature-scaling"></a>Značajka skaliranja

Značajka skaliranja poznata i kao normalizacija podataka insures da značajke široko disbursed vrijednosti za koje su nisu navedeni viškom računa u funkciji ciljna. Kod za značajku skaliranje koristi [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) za promjenu veličine značajke za varijancu jedinicu. To je nudi MLlib za korištenje u linearne regresije s Stochastic prijelaza Descent (SGD), popularne algoritam za obuku te širokog raspona druge strojnog učenja modela kao što su regularized nedostatke ili podršku vektorski strojeva (SVM).

>[AZURE.NOTE] Naišli algoritam LinearRegressionWithSGD biti osjetljive će biti istaknuti skaliranja.

Evo koda za skaliranje varijable za korištenje s algoritam regularized linearne SGD.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Vrijeme za izvršavanje iznad ćelije: 13.17 sekundi


### <a name="cache-objects-in-memory"></a>Predmemorija objekata u memoriji

Vrijeme potrebno za obuku i testiranje ML algoritama mogu se smanjiti tako da predmemoriranje okvira ulaznih podataka objekata koristi za klasifikaciju, regresije, i skalirana značajke.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:** 

Vrijeme za izvršavanje iznad ćelije: 0,15 sekundi


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Predviđanje hoće li se savjet plaćenu s modelima binarni klasifikacija

U ovom se odjeljku pokazuje kako pomoću tri modelima za zadatak binarni klasifikacija procjenjivanje hoće li plaćenu savjeta za putovanje taksi. Modeli prikazane su:

- Regularized logistika regresije 
- Slučajni skupa stabala modela
- Povećanje stabala za prijelaza

Svaki model stvara pozivni broj dijeli korake: 

1. **Obuka model** podataka s jednog skup parametara
2. **Procjena modela** na skupu podataka test s mjerenja
3. **Spremite model** u blob za buduće potrošnje

### <a name="classification-using-logistic-regression"></a>Klasifikacija pomoću logistika regresije

Kod u ovom odjeljku pokazuje kako uvježbati, analiza i spremiti logistika regresije modela [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) predviđa hoće li se savjet platiti za putovanje u NEW taksi putovanja i prijevoz skupu podataka.

**Obuka logistika regresijskog modela pomoću Životopis i hyperparameter sweeping**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    
    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**IZLAZ:** 

Koeficijenti: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

INTERCEPT:-0.0111216486893

Vrijeme za izvršavanje iznad ćelije: 14.43 sekundi

**Analiza binarni klasifikacije modela pomoću standardnih mjerenja**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**IZLAZ:** 

Područje u odjeljku Cijena = 0.985297691373

Područje u odjeljku ROC = 0.983714670256

Sažetak stat

Preciznost = 0.984304060189

Opoziv = 0.984304060189

F1 Rezultat = 0.984304060189

Vrijeme za izvršavanje iznad ćelije: 57.61 sekundi

**Crtanje krivulje ROC.**

*PredictionAndLabelsDF* je registrirana kao tablicu, *tmp_results*, na prethodnu ćeliju. *tmp_results* mogu se upiti učinite i izlazni rezultati u sqlResults podataka više okvira za iscrtavanje. Evo kod.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Evo kod predviđanja i crtanje krivulje ROC.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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
    

**IZLAZ:**

![Curve.png ROC logistika regresije](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)


### <a name="random-forest-classification"></a>Slučajni skupa stabala klasifikacija

Kod u ovom odjeljku pokazuje kako uvježbati, analiza i Spremi slučajne skupa stabala modelu koji predviđa hoće li se savjet platiti za putovanje u NEW taksi putovanja i prijevoz skupu podataka.
    
    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Područje u odjeljku ROC = 0.985297691373

Vrijeme za izvršavanje iznad ćelije: 31.09 sekundi


### <a name="gradient-boosting-trees-classification"></a>Klasifikacija prijelaza povećanje stabla

Kod u ovom odjeljku pokazuje kako uvježbati, Analiza, i spremanje prijelaza boosting model stabla koji predviđa hoće li se savjet plaćenu za putovanje u taksi putovanja SEBI i prijevoz skup podataka.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**IZLAZ:**

Područje u odjeljku ROC = 0.985297691373

Vrijeme za izvršavanje iznad ćelije: 19.76 sekundi


## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Predviđanje iznose savjeta za taksi trips s regresije modelima

U ovom se odjeljku pokazuje kako pomoću tri modela za zadatak regresije procjenjivanje količinu savjet plaćenu taksi putovanja koji se temelji na druge značajke savjet. Modeli prikazane su:

- Regularized linearna regresija
- Slučajni skupa stabala
- Povećanje stabala za prijelaza

Ove modelima su opisane u uvodu. Svaki model stvara pozivni broj dijeli korake: 

1. **Obuka model** podataka s jednog skup parametara
2. **Procjena modela** na skupu podataka test s mjerenja
3. **Spremite model** u blob za buduće potrošnje

### <a name="linear-regression-with-sgd"></a>Linearna regresija s SGD 

Kod u ovom odjeljku pokazuje kako pomoću značajke skaliranu linearna regresija koji koristi stochastic prijelaza descent (SGD) za optimizaciju uvježbati te kako rezultatu, analiza i spremite model u Azure Blob prostora za pohranu (WASB).

>[AZURE.TIP] U našem sučelje može biti probleme s Konvergencija LinearRegressionWithSGD modela i parametara moraju biti promijenili/optimizirana pažljivo stjecanje valjani modela. Promjena veličine varijable znatno pomaže vam Konvergencija. 


    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

Koeficijenti: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

INTERCEPT: 0.853872718283

RMSE = 1.24190115863

R sqr = 0.608017146081

Vrijeme za izvršavanje iznad ćelije: 58.42 sekundi


### <a name="random-forest-regression"></a>Skup stabala slučajna regresije

Kod u ovom odjeljku pokazuje kako uvježbati, vrednovati i spremanje slučajni skupa stabala regresije koji predviđa savjet iznos za NEW taksi putovanja podatke.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

RMSE = 0.891209218139

R sqr = 0.759661334921

Vrijeme za izvršavanje iznad ćelije: 49.21 sekundi


### <a name="gradient-boosting-trees-regression"></a>Regresije prijelaza povećanje stabla

Kod u ovom odjeljku pokazuje kako uvježbati, analiza i spremanje prijelaza boosting model stabla koji predviđa savjet iznos za NEW taksi putovanja podatke.

**Obuka i analiza**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**IZLAZ:**

RMSE = 0.908473148639

R sqr = 0.753835096681

Vrijeme za izvršavanje iznad ćelije: 34.52 sekundi

**Crtanje**

*tmp_results* je registrirana kao tablicu vrste Hive na prethodnu ćeliju. Rezultati iz tablice su Izlaz u *sqlResults* podataka – okvir za iscrtavanje. Evo kod

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Evo koda za iscrtavanje podataka pomoću poslužitelja Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)
    

**IZLAZ:**

![Stvarni – Dodavanje veze za vanjskih-predviđene-savjet-iznosi](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

    
## <a name="clean-up-objects-from-memory"></a>Čišćenje objekte iz memorije

Korištenje `unpersist()` da biste izbrisali objekte koji su predmemorirani u memoriji.
        
    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Zapis mjesta za pohranu modela za potrošnje i bilježenje rezultata

Zauzeti i neovisno skup podataka opisane u rezultatu na [rezultat i procjenu modela Spark u komponenti strojnog učenja](machine-learning-data-science-spark-model-consumption.md) tema, morate kopirati i zalijepiti te nazive datoteka koja sadrži spremljenu modelima ovdje stvoriti u bilježnicu Jupyter potrošnje. Ovdje je kod za ispis i puteve trebate li datoteka modela.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**IZLAZ**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"


## <a name="whats-next"></a>Što je sljedeće?

Sad kad ste stvorili regresije i klasifikacija modeli s Spark MlLib, spremni ste da biste saznali kako rezultatu i procjenu te modela. Istraživanje napredne podataka i Modeliranje bilježnice dives dublju u više provjere valjanosti, hyper-parametar sweeping, uključujući i modela procjenu. 

**Modela potrošnje:** Da biste saznali kako rezultatu i procjenu klasifikaciju i regresije modele stvorene u ovoj temi, potražite u članku [rezultat i procjenu Spark u komponenti strojno učenje modelima](machine-learning-data-science-spark-model-consumption.md).

**Unakrsno provjere valjanosti i hyperparameter sweeping**: na kako može biti modela u odjeljku [Napredne Istraživanje podataka i Modeliranje s Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) put uvježbavan pomoću unakrsno provjere valjanosti i hyper parametar sweeping



