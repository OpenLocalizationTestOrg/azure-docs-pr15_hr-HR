<properties 
    pageTitle="Da biste analizirali zapisnika web-mjesto prilagođene biblioteke pomoću programa HDInsight Spark klaster | Microsoft Azure" 
    description="Korištenje prilagođene biblioteke pomoću programa HDInsight Spark klaster da biste analizirali zapisnika web-mjesta" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Analiza zapisnika web-mjesto pomoću prilagođenog biblioteke Apache Spark klaster na HDInsight Linux

Ovu bilježnicu pokazuje analizi podaci iz zapisnika prilagođene biblioteke pomoću Spark na HDInsight. Prilagođene biblioteke koristimo je biblioteka Python pod nazivom **iislogparser.py**.

> [AZURE.TIP] Pomoću ovog praktičnog vodiča i nije dostupan kao Jupyter bilježnice na klaster Spark (Linux) koje ste stvorili u HDInsight. Sučelje za bilježnicu omogućuje pokretanje isječci Python iz bilježnice sam. Da biste izvršili vodič iz bilježnice, stvorili Spark klaster, pokretanje bilježnice za Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a zatim pokrenite bilježnice **Analiza zapisnike s Spark pomoću prilagođenih library.ipynb** u mapi **PySpark** .

**Preduvjeti:**

Morate imati sljedeće:

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Upute potražite u članku [Stvaranje Spark Apache klastere u Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Spremanje sirovim podacima u obliku programa RDD

U ovom odjeljku da biste pokrenuli poslove koje obrađivati neobrađenog oglednim podacima, a zatim Spremi kao tablicu vrste Hive koristimo bilježnice [Jupyter](https://jupyter.org) pridruženu programa klaster Apache Spark u HDInsight. Ogledni podaci u .csv datoteku (hvac.csv) dostupne na svim klastere po zadanom.

Kada se podaci spremaju kao tablicu vrste Hive, u sljedećem odjeljku smo će povezati s tablicu vrste Hive pomoću alatima za Poslovno obavještavanje kao što je Power BI i Tableau.

1. S [Portala za Azure](https://portal.azure.com/), iz startboard, kliknite pločicu za svoj klaster Spark (Ako ste ga prikvačiti na startboard). Možete se kretati svoj klaster u odjeljku **Pregledaj sve** > **Klastere HDInsight**.   

2. Iz klaster plohu Spark kliknite **Nadzorna ploča klaster**, a zatim **Jupyter bilježnice**. Ako se to od vas zatraži, unesite administratorske vjerodajnice za klaster.

    > [AZURE.NOTE] Možda dosegne i Jupyter bilježnicu za svoj klaster tako da otvorite sljedeći URL u pregledniku. Zamijenite __CLUSTERNAME__ pod nazivom svoj klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Stvaranje nove bilježnice. Kliknite **Novo**, a zatim kliknite **PySpark**.

    ![Stvaranje nove bilježnice Jupyter] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Stvaranje nove bilježnice Jupyter")

3. Nova bilježnica se stvara i otvara pod nazivom Untitled.pynb. Kliknite naziv bilježnice na vrhu, a zatim upišite neslužbeni naziv.

    ![Osiguraj naziv bilježnice] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Osiguraj naziv bilježnice")

4. Budući da ste stvorili pomoću otklanjanje PySpark bilježnice, ne morate izričito stvaranje sve konteksta. Konteksta Spark i grozd će se automatski stvara za vas kada pokrenete prvu ćeliju kod. Možete započeti tako da uvezete vrste koji su potrebni za taj scenarij. Zalijepite sljedeći isječak u praznu ćeliju, a zatim pritisnite **SHIFT + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Stvaranje programa RDD korištenje uzorka zapisnika podaci koji su već dostupni na klaster. Možete pristupiti podacima u zadani prostor za pohranu račun povezan s klaster pri **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Dohvaćanje zapisnik uzorka postavljanje da biste potvrdili da u prethodnom koraku uspješno dovršena.

        logs.take(5)

    Trebali biste vidjeti na Izlaz sličnu ovoj:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analiziranje podataka zapisnika pomoću prilagođenih Python biblioteke

7. U rezultatu iznad, prvi nekoliko redaka obuhvatiti informacije zaglavlja, a svaki preostali redak podudara s shemom opisane u tog zaglavlja. Raščlanjivanje takve zapisnici mogu biti složene. Tako, koristimo prilagođene Python biblioteke (**iislogparser.py**) koji vam se čini Raščlanjivanje takve zapisnika jednostavniji. Prema zadanim postavkama biblioteke nalazi se u sklopu Spark klaster na HDInsight na **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Međutim, nije u tu biblioteku na `PYTHONPATH` tako da ne koristimo korištenjem naredbe Uvezi kao što su `import iislogparser`. Da biste koristili tu biblioteku, ne možemo morate distribuirati sve čvorove tempiranja. Pokrenite sljedeće isječka.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`nudi funkcije `parse_log_line` koji vraća `None` ako redak evidencije redak zaglavlja i vraća instance komponente na `LogLine` klase Ako naiđe crtu zapisnika. Korištenje na `LogLine` predmete izdvojiti samo reci zapisnika iz na RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Dohvaćanje nekoliko redaka izdvojene zapisnika da biste potvrdili da korak uspješno dovršena.

        logLines.take(2)

    Rezultat mora biti sličnu ovoj:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. Na `LogLine` predmete, shodno tome, sadrži neke korisne metode poput `is_error()`, koja vraća ima li stavka evidencije Šifra pogreške. Ta postavka omogućuje izračunati broj pogrešaka u izdvojene zapisnika retke, a zatim se prijavite sve pogreške za neku drugu datoteku.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Trebali biste vidjeti na Izlaz kao što je sljedeća:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. **Matplotlib** možete koristiti i za sastavljanje vizualizacije podataka. Na primjer, ako želite izdvojiti uzrok zahtjeva za koji se izvode na dulje vrijeme, možda ćete morati pronaći datoteke za koje je potrebno najbolje vrijeme za posluživanje u.
Isječak ispod dohvaća podatke u gornjoj 25 resursima snimljene Većina vrijeme za zahtjev.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Trebali biste vidjeti na Izlaz kao što je sljedeća:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Možete prikazati i te informacije u obliku crtanja. Prvi korak da biste stvorili grafički Javite nam najprije stvoriti privremena **AverageTime**. U tablici grupa zapisnike po vremenu da biste vidjeli ako postoje sve krivina neobično kašnjenje u bilo kojem trenutku određeni.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Zatim možete pokrenuti sljedeći SQL upit da biste sve zapise u tablici **AverageTime** .

        %%sql -o averagetime
        SELECT * FROM AverageTime

    Na `%%sql` poseban slijedi `-o averagetime` osigurava izlaz upita lokalno dosljedan na poslužitelju Jupyter (obično headnode klaster). Rezultat je ista i kao [Pandas](http://pandas.pydata.org/) dataframe s navedenim nazivom **averagetime**.

    Trebali biste vidjeti na Izlaz kao što je sljedeća:

    ![Izlaz upita za SQL] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "Izlaz upita za SQL")

    Dodatne informacije o na `%%sql` poseban, kao i druge magics dostupno u sklopu otklanjanje PySpark potražite u članku [jezgre dostupne na Jupyter bilježnica s klastere Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Matplotlib, u biblioteku korišten za sastavljanje vizualizaciju podataka, sada možete koristiti da biste stvorili grafički prikaz. Budući da u crtanja moraju se stvoriti iz dataframe lokalno postojanog **averagetime** , moraju započinjati koda u `%%local` poseban. Na taj način kod lokalno pokrenut na poslužitelju Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Trebali biste vidjeti na Izlaz kao što je sljedeća:

    ![Izlaz Matplotlib] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Izlaz Matplotlib")

16. Nakon što dovršite pokrenuti aplikaciju, trebali biste zatvaranja bilježnice da biste predali resurse. Da biste to učinili, na izborniku **datoteka** bilježnicu, kliknite **Zatvori i zaustavili**. Ovaj će zatvaranja i Zatvori bilježnicu.
    

## <a name="seealso"></a>Vidi također


* [Pregled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariji

* [Spark bi: izvođenje analiza interaktivnih podataka pomoću Spark u HDInsight s alatima za Poslovno obavještavanje](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s strojnog učenja: korištenje Spark u HDInsight za analizu sastavnih temperatura pomoću HVAC podataka](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s strojnog učenja: korištenje Spark u HDInsight za predviđanje rezultata provjere za hranu](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark strujeće: Korištenje Spark u HDInsight za izgradnju u stvarnom vremenu strujanje aplikacije](hdinsight-apache-spark-eventhub-streaming.md)

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
