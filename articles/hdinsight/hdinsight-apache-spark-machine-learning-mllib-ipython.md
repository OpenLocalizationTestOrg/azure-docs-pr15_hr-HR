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


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Učenje na računalu: predvidljivu analizi Hrana provjere podataka pomoću MLlib Apache Spark skupine na HDInsight Linux

> [AZURE.TIP] Pomoću ovog praktičnog vodiča i nije dostupan kao Jupyter bilježnice na klaster Spark (Linux) koje ste stvorili u HDInsight. Sučelje za bilježnicu omogućuje pokretanje isječci Python iz bilježnice sam. Da biste izvršili vodič iz bilježnice, stvorili Spark klaster, pokretanje bilježnice za Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a zatim pokrenite bilježnice **Spark strojno učenje - predvidljivu analizi podataka provjere Hrana pomoću MLLib.ipynb** u mapi **Python** .


U ovom se članku opisuje način pomoću **MLLib**, Spark, ugrađene strojnog učenja biblioteke za jednostavne predvidljivu analizu na otvoren skup podataka. MLLib je biblioteka Spark core koji omogućuje brojne uslužni programi koji se koriste za strojnog učenja zadataka, uključujući uslužni programi koji su prikladna za:

* Klasifikacija

* Regresije

* Klasteriranje

* Modeliranje teme

* Razlaganja jedinstven vrijednost (SVD) i glavni komponente analysis (PCA)

* Pretpostavke testiranje i izračun statistike uzorka

U ovom se članku predstavlja jednostavan pristup *klasifikacije* kroz logistika regresije.

## <a name="what-are-classification-and-logistic-regression"></a>Što su klasifikacije i logistika regresije?

*Klasifikacija*, vrlo uobičajeni strojnog učenja zadatka, je postupak sortiranja ulaznih podataka u kategorije. Je posao algoritam klasifikaciju o tome kako dodijeliti "oznaka" za unos podataka koje unesete. Ako, na primjer, nije moguće mislite da algoritma učenje računalu koje priznaje podataka o dionicama kao ulaz i dijeli akcijama u dvije kategorije: dionice koje treba prodajete i dionice koje treba zadržati.

Logistika regresije je algoritam koju koristite za klasifikaciju. Spark-logistika regresije API je korisno za *Binarni klasifikacija*ili klasifikaciju ulazne podatke u neku od dvije grupe. Dodatne informacije o logistika nedostatke potražite u članku [Wikipedije](https://en.wikipedia.org/wiki/Logistic_regression).

Ukratko, postupak logistika regresije daje *logistika funkcija* koja se može se koristiti za predviđanje vjerojatnost da se unos vektorski pripada u jednu grupu ili na drugi.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Što su Pokušavamo izvršili u ovom članku?

Spark će se koristiti za neke predvidljivu analizu Hrana provjere podataka (**Food_Inspections1.csv**) koji je dobiven putem [portala Grad Chicago podataka](https://data.cityofchicago.org/). Ovaj skup podataka sadrži informacije o provjerama hrane koja su obavljaju u Chicagu, uključujući informacije o svakom uspostavljanje hrane koja je provjerava ima li, kršenja pronađene (ako ih ima) i rezultate provjere. CSV datoteku podataka već dostupan je u račun za pohranu koji je povezan s klaster pri **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

U koracima u nastavku, razviti model da biste vidjeli što je potrebno proći ili neće uspjeti Hrana provjere. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Počeli raditi aplikacije za učenje računala pomoću Spark MLlib

1. S [Portala za Azure](https://portal.azure.com/), iz startboard, kliknite pločicu za svoj klaster Spark (Ako ste ga prikvačiti na startboard). Možete se kretati svoj klaster u odjeljku **Pregledaj sve** > **Klastere HDInsight**.   

2. Iz klaster plohu Spark kliknite **Nadzorna ploča klaster**, a zatim **Jupyter bilježnice**. Ako se to od vas zatraži, unesite administratorske vjerodajnice za klaster.

    > [AZURE.NOTE] Možda dosegne i Jupyter bilježnicu za svoj klaster tako da otvorite sljedeći URL u pregledniku. Zamijenite __CLUSTERNAME__ pod nazivom svoj klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Stvaranje nove bilježnice. Kliknite **Novo**, a zatim kliknite **PySpark**.

    ![Stvaranje nove bilježnice Jupyter] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Stvaranje nove bilježnice Jupyter")

3. Nova bilježnica se stvara i otvara pod nazivom Untitled.pynb. Kliknite naziv bilježnice na vrhu, a zatim upišite neslužbeni naziv.

    ![Osiguraj naziv bilježnice] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Osiguraj naziv bilježnice")

3. Budući da ste stvorili pomoću otklanjanje PySpark bilježnice, ne morate izričito stvaranje sve konteksta. Konteksta Spark i grozd će se automatski stvara za vas kada pokrenete prvu ćeliju kod. Počeli raditi na strojnog učenja aplikacije uvozom vrste potrebne za taj scenarij. Da biste to učinili, postavite pokazivač u ćeliju, a zatim pritisnite **SHIFT + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Sastavljanje unos dataframe

Možete koristiti `sqlContext` za izvođenje transformacije na strukturiranih podataka. Prvi zadatak je da biste učitali oglednih podataka ((**Food_Inspections1.csv**)) u Spark SQL *dataframe*. 

1. Budući da je sirovim podacima u obliku CSV, moramo koriste Spark kontekst za izvlačenje svaki redak datoteke u memoriju kao nestrukturirane tekst; Nakon toga pomoću biblioteke CSV Python, pojedinačno raščlaniti svaki redak. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. Sada imamo CSV datoteku kao programa RDD. Javite nam dohvatiti jedan redak s RDD da biste shvatili shema podataka.


        inspections.take(1)


    Trebali biste vidjeti na Izlaz kao što je sljedeća:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Iznad izlaz daje nam uvid u shemi Ulazna datoteka; datoteka sadrži naziv svake uspostavljanje, vrstu uspostavljanje, adresa, podatke o preglede i mjesto, između ostalog. Odaberimo nekoliko stupaca koji će biti korisno za naše predvidljivu analizu i grupirati rezultate kao dataframe koja ne možemo koristiti da biste stvorili privremenu tablicu.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Sada imamo *dataframe* `df` na kojem smo izvršava naš analize. Imamo i privremena poziva **CountResults**. Ne možemo stvaraju 4 stupca interes na dataframe: **id**, **naziv**, **rezultate**i **kršenja**. 
    
    Recimo da se mali uzorak podataka:

        df.show(5)

    Trebali biste vidjeti na Izlaz kao što je sljedeća:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Razumijevanje podataka

1. Započnimo saznati što sadrži naš skup podataka. Na primjer, koje su različite vrijednosti u stupcu **rezultata** ?


        df.select('results').distinct().show()

    
    Trebali biste vidjeti na Izlaz kao što je sljedeća:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Brzi vizualizacije možete Pridonesite razloga o raspodjele te ishoda. Ne možemo već imate podatke u privremena **CountResults**. Možete pokrenuti sljedeće SQL upita na temelju tablice da biste bolje razumjeli kako su distributed rezultate.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    Na `%%sql` poseban slijedi `-o countResultsdf` osigurava izlaz upita lokalno dosljedan na poslužitelju Jupyter (obično headnode klaster). Rezultat je ista i kao [Pandas](http://pandas.pydata.org/) dataframe s navedenim nazivom **countResultsdf**.
    
    Trebali biste vidjeti na Izlaz kao što je sljedeća:
    
    ![Izlaz upita za SQL] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "Izlaz upita za SQL")

    Dodatne informacije o na `%%sql` poseban, kao i druge magics dostupno u sklopu otklanjanje PySpark potražite u članku [jezgre dostupne na Jupyter bilježnica s klastere Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Matplotlib, u biblioteku korišten za sastavljanje vizualizaciju podataka, možete koristiti i da biste stvorili grafički prikaz. Budući da u crtanja moraju se stvoriti iz dataframe lokalno postojanog **countResultsdf** , moraju započinjati koda u `%%local` poseban. Na taj način kod lokalno pokrenut na poslužitelju Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Trebali biste vidjeti na Izlaz kao što je sljedeća:

    ![Izlazni rezultat](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Vidjet ćete da postoje 5 distinct rezultata koji mogu sadržavati provjeru:
    
    * Tvrtke koje se ne nalazi 
    * Nije uspjelo
    * Lozinka
    * PSS w/ uvjete i
    * Izvan tvrtke 

    Javite nam razviti modelu koji mogu pogoditi rezultat provjere u Hrana navedene u kršenja. Budući da logistika regresije je način binarni klasifikacije, je li bolje da biste grupirali podatke u dvije kategorije: **neće uspjeti** i **prenesite**. Na "prolaz w/ uvjeta" i dalje je lozinka, tako da kada smo uvježbati model, ne možemo ćemo dodavanje razmotriti dva rezultata ekvivalentan. Podatke s drugih rezultata ("Tvrtke ne nalazi"; "izvan tvrtke") su korisne tako uklonit ćemo ih iz skupa naš tečaj. To mora biti redu od tih dviju kategorija ipak čine vrlo malen postotak rezultate.

5. Javite nam nastaviti i pretvorite naš postojeće dataframe (`df`) u novi dataframe gdje se svaki provjere predstavlja u paru natpis kršenja. U našem slučaju oznaku od `0.0` predstavlja pogreške, natpisa od `1.0` predstavlja vjerojatnost_u i oznaku od `-1.0` predstavlja neke rezultate osim one dva. Ne možemo će filtriraju te rezultate kada računalstvo novi okvir podataka.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Pogledajmo dohvatiti jedan redak s natpisima podataka da biste vidjeli kako izgleda.


        labeledData.take(1)


    Trebali biste vidjeti na Izlaz kao što je sljedeća:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Stvaranje modela logistika regresije iz ulaznog dataframe

Naš konačni zadatak je da biste pretvorili s natpisima podataka u obliku koji možete analizirati prema logistika regresije. Unos logistika regresije algoritam mora biti skup *parove vektorski značajka natpisa*, pri čemu je "vektorski značajka" vektor brojeva koji predstavlja točke unosa na mobitel. Tako, moramo njihovo pretvaranje stupca "kršenja" koje se djelomično strukturirani i sadrži mnogo komentara u slobodno teksta, u polju realnih brojeva stroj nije lako razumjeti. 

Jednom standardnom strojnog učenja pristup za obradu prirodnim jezikom je dodijeliti svakoj distinct riječi "indeks" i prenesite vektor strojnog učenja algoritam tako da svaki indeks vrijednost sadrži relativni učestalost te riječi u tekstnom nizu. 

MLLib omogućuje jednostavnu za izvođenje ovog postupka. Najprije ćemo ćete "tokenize" svaki niz kršenja da biste dobili pojedinačne riječi u svakom nizu, a zatim ćemo pomoću programa `HashingTF` svaki skup tokeni pretvoriti vektora značajke koje se zatim proslijediti logistika regresije algoritam za sastavljanje modela. Ćete provest svi ti koraci u nizu pomoću "kanal".


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Analiza modela na zasebnom test skup podataka

Koristimo modela koju smo stvorili ranije za *predviđanje* koje rezultate novi provjerama bit će, ovisno o kršenja koji su opaženih. Ne možemo put uvježbavan modela u sustavu dataset **Food_Inspections1.csv**. Javite nam koristite drugi skup podataka, **Food_Inspections2.csv**treba *procijeniti* jačinu ovaj model na nove podatke. U ovom drugi skup podataka (**Food_Inspections2.csv**) već mora biti u spremniku za pohranu zadani pridružene klaster.

1. Ispod isječak stvara novi dataframe **predictionsDf** koja sadrži predviđanje generira modela. U isječak stvara i privremena **predviđanja** na temelju na dataframe.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Trebali biste vidjeti na Izlaz kao što je sljedeća:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Pogledajte neke na predviđanja. Pokrenite ovaj isječak:

        predictionsDf.take(1)

    Vidjet ćete predviđanje za prvu stavku u skupu podataka za testiranje.

3. Na `model.transform()` način će primijeniti iste transformaciju novim podacima s istom shemom i dolaze predviđanje kako klasifikaciju podatke. Možemo nekoliko jednostavnih Statistika da biste saznati koliko su naš predviđanja:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Rezultat izgleda ovako:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Korištenje logistika regresije s Spark daje nam točne model odnos između kršenja opise svojstava u engleskom jeziku i li danih poslovnih želite proslijediti ili neće uspjeti Hrana provjere. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Stvaranje vizualni prikaz za predviđanje

Ne možemo možete sada izgraditi konačni vizualizacije da biste nam pomogli razloga o rezultate ovaj test. 

1. Ne možemo najprije izdvajanje različitih predviđanja i rezultate iz **predviđanja** privremena tako što ste ranije stvorili. Sljedeće upita odvojite izlaz kao *true_positive*, *false_positive*, *true_negative*i *false_negative*. U nastavku upitima smo isključite vizualizacija pomoću `-q` i spremiti i izlaz (pomoću `-o`) kao dataframes koji se zatim koristiti s na `%%local` poseban. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Na kraju, koristite sljedeće isječak da biste generirali crtanja pomoću **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Trebali biste vidjeti sljedeće izlaz.
    
    ![Predviđanje Izlaz](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    Na tom grafikonu, rezultat "pozitivan" se odnosi na provjere nije uspjelo Hrana dok negativni rezultat se odnosi na proslijeđeni provjere.

## <a name="shut-down-the-notebook"></a>Isključivanje bilježnice

Nakon što dovršite pokrenuti aplikaciju, trebali biste zatvaranja bilježnice da biste predali resurse. Da biste to učinili, na izborniku **datoteka** bilježnicu, kliknite **Zatvori i zaustavili**. Ovaj će zatvaranja i Zatvori bilježnicu.


## <a name="seealso"></a>Vidi također


* [Pregled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariji

* [Spark bi: izvođenje analiza interaktivnih podataka pomoću Spark u HDInsight s alatima za Poslovno obavještavanje](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s strojnog učenja: korištenje Spark u HDInsight za analizu sastavnih temperatura pomoću HVAC podataka](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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
