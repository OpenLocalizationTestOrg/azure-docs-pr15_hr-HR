<properties
    pageTitle="Proces znanstvenog timu podataka u akciji: korištenje HDInsight Hadoop klaster na Criteo dataset 1 TB | Microsoft Azure"
    description="Pomoću postupka timu podataka znanosti za kraj do kraja scenarij u employing programa HDInsight Hadoop klaster omogućuje stvaranje i implementaciju modela pomoću velike dataset javno dostupna (1 TB)"
    services="machine-learning,hdinsight"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Postupka znanstvenog timu podataka u akciji – pomoću Azure HDInsight Hadoop klastere na 1 TB skup podataka

U ovom vodiču demonstrirati ćemo pomoću postupka timu podataka znanstvenog u do kraja do kraja scenarij [Azure HDInsight Hadoop klaster](https://azure.microsoft.com/services/hdinsight/) za pohranu, istraživanje, značajka inženjering, i ogledne podatke iz jednog od skupova podataka za javno dostupna [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dolje. Da biste sastavili binarni klasifikacija modela na te podatke koristimo Azure strojnog učenja. Pokazat ćemo i upute za jednu od ovih modelima Objavi kao članak na web-servisa.

Također je moguće koristiti za IPython bilježnicu da biste izvršili zadatke navedene u ovom vodiču. Korisnici koji biste željeli pokušati takvog trebali proučiti temi [Criteo vodič pomoću vrste Hive ODBC veze](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Opis Criteo skup podataka

Criteo podaci kliknite predviđanje skup podataka koje je otprilike 370GB gzip spojene TSV datoteke (~1.3TB dekomprimirati), comprising više 4,3 milijarde zapisa. Koristi se s 24 dana kliknite podaci učinio dostupnima [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Pogodnost fizičari podataka, ne možemo ste raspakirana podataka dostupnih za nam da isprobate.

Svaki zapis u ovaj skup podataka sadrži 40 stupaca:

- prvi stupac sadrži natpis stupca koji označava je li korisnik klikne gumb za **Dodavanje** (vrijednost 1) ili kliknite neku (vrijednost 0)
- sljedeće 13 stupci su numerički, a
- zadnji 26 su categorical stupci

Stupci se anonymized i crtu koristite niz numerirana imena: "Stupcu 1" (za fotokopije) da biste "Col40" (za zadnji stupac categorical).            

Evo u isječak prvih 20 stupaca dva opažanja (redaka) iz ovog skupa podataka.

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Postoje vrijednosti koje nedostaju u stupac numeričkih i categorical mjesec u ovaj skup podataka. Ne možemo opisuju jednostavan način rukovanja vrijednosti koje nedostaju. Dodatne pojedinosti podaci su istražili kada ćemo ih spremiti u vrste Hive tablice.

**Definicija:** *Slijednih stopa (GRUPU):* To je postotak klikova u podacima. U ovom dataset Criteo u GRUPU je otprilike 3,3% ili 0.033.

## <a name="mltasks"></a>Primjeri predviđanje zadataka
U ovom vodiču u članku se dva uzorka predviđanje problema:

1. **Binarni klasifikacija**: predviđa li korisnik klikom na Dodaj:
    - 0 klasa: Nije definiran klik
    - Klase 1: kliknite

2. **Regresije**: predviđa vjerojatnost da je kliknite ad iz korisničke značajke.


## <a name="setup"></a>Postavljanje gore an HDInsight Hadoop klaster za znanstvenog podataka

**Bilješke:** To je obično zadatak za **administratore** .

Postavljanje okruženja Azure podataka znanosti za izrade rješenja predvidljivu analize s HDInsight klastere tri koraka:

1. [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md): račun za pohranu se koristi za spremanje podataka u spremište blobova platforme Azure. Podaci koji se koriste u HDInsight klastere pohranjen u nastavku.

2. [Prilagodba Azure HDInsight Hadoop klastere za znanstvenog podataka](machine-learning-data-science-customize-hadoop-cluster.md): ovaj korak stvara sustava Azure HDInsight Hadoop klaster s 64-bitni Anaconda Python 2.7 instalirano sve čvorove. Postoje dvije važnih koraka (što je opisano u nastavku) da biste dovršili prilikom prilagodbe klaster HDInsight.

    * Morate povezati s računom za pohranu stvorili u koraku 1 s svoj klaster HDInsight prilikom stvaranja. Taj račun za pohranu koristi se za pristup podacima koje možete obraditi unutar klaster.

    * Nakon stvaranja, čvor glavni klaster morate Omogućivanje daljinskog pristupa. Imajte na umu vjerodajnice za daljinski pristup koje navedete (razlikuju od onih za klaster pri njegova stvaranja): su vam potrebne da biste dovršili sljedeće postupke.

3. [Stvaranje radnog prostora programa Azure ML](machine-learning-create-workspace.md): ovo računalo Azure učenje radnog prostora se koristi za stvaranje strojnog učenja modela nakon do početne podatke Istraživanje i dolje uzorkovanje na klasteru HDInsight.

## <a name="getdata"></a>Početak i zauzeti podatke iz javnog izvora

Klikom na vezu, prihvatite uvjete korištenja, a omogućuje naziv može pristupiti [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) skupu podataka. Ovdje je prikazana snimke kako to izgleda:

![Prihvatite uvjete Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Kliknite **Nastavi da biste preuzimanje** da biste potražite dodatne informacije o skupu podataka i njegov dostupnost.

Podaci koji se nalaze na javnom mjestu [Azure bloba prostora za pohranu](../storage/storage-dotnet-how-to-use-blobs.md) : wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "wasb" odnosi se na mjesto spremište blobova platforme Azure. 

1. Podaci u ovom javno blobova sastoji se od tri podmape raspakiranu datoteku paketa podataka.

    1. Podmapu *neobrađenog/count/* sadrži prvi 21 dana podataka – od dana\_00 dan\_20
    2. Podmapu *neobrađenog/vlaku/* sastoji se od jedan dan podataka, dan\_21
    3. Podmapu *neobrađenog i testiranje/* sastoji se od dva dana podataka, dan\_22 i dan\_23

2. Za osobe koje želite započeti s gzip sirovim podacima, te dostupne su i u glavnu mapu *neobrađenog /* kao day_NN.gz, gdje se NN dolazi od 00 do 23.

Zamjenski pristup da biste pristupili, istraživanje, a model ove podatke koje je potrebno sve lokalne preuzimanja uputama u nastavku ovog vodiča kada ćemo stvoriti grozd tablice.

## <a name="login"></a>Prijavite se na headnode klaster

Da biste se prijavili headnode klaster pomoću [portala za Azure](https://ms.portal.azure.com) da biste pronašli klaster. Kliknite ikonu slona HDInsight na lijevoj strani, a zatim dvokliknite naziv svoj klaster. Idite na karticu **Konfiguracija** , dvokliknite ikonu POVEZIVANJE na dno stranice i unesite vjerodajnice za daljinski pristup kada se to od vas zatraži. Tako ćete doći do headnode klaster.

Evo izgleda na uobičajeni prvi prijavite se na headnode klaster:

![Prijavite se u skupine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Na lijevoj strani dobivamo "Hadoop naredbenog retka", što je našeg workhorse za istraživanje podataka. Možemo vidjeti i dva korisne URL - "Hadoop Yarn Status" i "Hadoop naziv čvor". Yarn status URL prikazuju tijek zadatka i naziv URL-a čvor daje pojedinosti o konfiguraciji klaster.

Sada ćemo postavljeni i spremni za početak prvi dio u prikazu: Istraživanje podataka pomoću grozd i Priprema podataka za Azure strojnog učenja.

## <a name="hive-db-tables"></a>Stvaranje baze podataka grozd i tablice

Da biste stvorili grozd tablice za naše Criteo skup podataka, otvorite ***Hadoop naredbenog retka*** na radnoj površini čvor glavni pa unesite direktorija grozd tako da unesete naredbu

    cd %hive_home%\bin

>[AZURE.NOTE] Pokreni sve grozd naredbe u prikazu s smeće grozd / direktorija upit. To brine problema put automatski. Koristimo uvjete "Grozd direktorija pitanje", "grozd smeće / upit direktorija", "Hadoop naredbenog retka" i naizmjence.

>[AZURE.NOTE]  Za izvođenje bilo koji upit grozd, jedan uvijek upotrijebite sljedeće naredbe:

        cd %hive_home%\bin
        hive

Kada se pojavi ZAMIJENI grozd s na "grozd >"potpis, jednostavno izrezivanje i lijepljenje upit da biste ga izvršiti.

Sljedeći kod stvara bazu podataka "criteo", a zatim proizvodi 4 tablice:


* *tablica za generiranje broji* utemeljena na dan dana\_00 dan\_20,
* *tablica za skup podataka vlaku* utemeljena na dan\_21, a
* dvije *tablice za korištenje kao skupova podataka za testiranje* utemeljena na dan\_22 i dan\_23 odnosno.

Ne možemo podijelite naš dataset test dvije različite tablice jer je jedan od dana praznika i želimo da biste odredili ako model može prepoznati razlike između praznika i koje nisu praznika iz slijednih stopa.

Skripta [uzorka & #95; grozd & #95; stvaranje & #95 ";" criteo "i" #95; & #95; baze podataka i & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) je prikazan pogodnost:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Ne možemo Imajte na umu da te tablice vanjskih kao što smo jednostavno pokažite na mjesta spremište blobova platforme Azure (wasb).

**Dva su načina za izvršavanje upita bilo koje vrste Hive koje ćemo sada spomenuti.**

1. **Korištenje naredbenog retka ZAMIJENI grozd**: prvi je izdavanje "grozd" naredbe i kopiranje i lijepljenje upita pri ZAMIJENI grozd naredbenog retka. Da biste to učinili, učinite:

        cd %hive_home%\bin
        hive

    Sada na naredbenog retka ZAMIJENI izrezivanje i lijepljenje upit izvršava ga.

2. **Spremanje upita u datoteku i izvršavanja naredbu**: druga je da biste spremili upiti .hql datoteku ([uzorka & #95; grozd & #95; stvaranje & #95 ";" criteo "i" #95; & #95; baze podataka i & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) i zatim izdati sljedeću naredbu za izvršavanje upita:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Potvrda stvaranja baze podataka i tablice

Nakon toga je potrebna potvrda stvaranja baze podataka pomoću sljedeće naredbe iz smeće grozd / direktorija upit:

        hive -e "show databases;"

Tako ćete dobiti:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

To potvrđuje stvaranje nove baze podataka, "criteo".

Da biste vidjeli koje tablice koju smo stvorili, ne možemo jednostavno problema se naredba iz smeće grozd / direktorija upit:

        hive -e "show tables in criteo;"

Zatim Vidimo sljedeći rezultat:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Istraživanje podataka u grozd

Sada ćemo spremni ste za istraživanje neke osnovne podatke u grozd. Ne možemo započeti prebrojavanje primjerima u u vlaku i testiranje podatkovne tablice.

### <a name="number-of-train-examples"></a>Broj od primjera vlaku

Sadržaj [uzorka & #95; grozd & #95; count i #95; vlaku & #95; tablice & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) prikazana su ovdje:

        SELECT COUNT(*) FROM criteo.criteo_train;

To rezultira:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Osim toga, jedan možda izdavati sljedeću naredbu iz smeće grozd / direktorija upit:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Broj test primjerima u dvaju skupova podataka za testiranje

Ne možemo sada Brojanje primjerima u dvaju skupova podataka za testiranje. Sadržaj [uzorak i #95 grozd & #95; count i #95; criteo & #95; test & #95; dan i #95; 22 & #95 ";" tablice "&" #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) se ovdje:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

To rezultira:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Uobičajeni, ne možemo možda i poziva skripta iz smeće grozd / imeničkog upit po izdavanja naredbu:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Na kraju, ne možemo pregledajte broj test primjerima u test skup podataka koji se temelji na dan\_23.

Naredba za to je slično onom samo (pogledajte [uzorka & #95; grozd & #95; count i #95; criteo & #95; test & #95; dan i #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Tako ćete dobiti:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Natpis distribuciju u skupu podataka vlaku

Distribuciju natpis u skupu podataka vlaku je koji vas zanimaju. Da biste vidjeli, Pokazat ćemo sadržaj [uzorka & #95; grozd & #95; criteo & #95; oznaku & #95; raspodjele & #95; vlaku & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

To rezultira raspodjele oznaka:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Imajte na umu da postotak pozitivan natpisa otprilike 3,3% (usklađene s izvornog skup podataka).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Distribucija histogram neke numeričke varijabli u skupu podataka vlaku

Koristimo lokalnog grozd, "histogram\_numerički" funkcija da biste saznali izgleda raspodjele numerički varijabli. Evo sadržaj [uzorak i #95; grozd & #95; criteo & #95; histogram & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Rezultira sljedeće:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

LATERAL PRIKAZ – razrezivanje kombinaciju služi grozd, čime se dobiva SQL nalik izlaz umjesto uobičajeni popisa. Imajte na umu da u ovom tablice u prvom stupcu odgovara centar za smeće, a drugi za smeće učestalost.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Približna percentili neke numeričke varijabli u skupu podataka vlaku

Osim toga interesa s numerički varijable je izračuni djelomičnog percentili. Grozd je izvorni "percentil\_approx" postupa s us. Sadržaj [uzorka & #95; grozd & #95; criteo & #95; djelomičnog & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) su:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

To rezultira:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Ne možemo napomenu da raspodjele percentili blisko povezana je s distribucija histogram sve numerička varijabla obično.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Pronađite broj jedinstvenih vrijednosti za neke categorical stupce u skupu podataka vlaku

Nastavite Istraživanje podataka, ne možemo naći, za neke categorical stupce, broj jedinstvenih vrijednosti proizvela. Da biste to učinili, Pokazat ćemo sadržaj [uzorka & #95; grozd & #95; criteo & #95; jedinstveni & #95 ";" vrijednosti "&" #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

To rezultira:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Ne možemo Imajte na umu da Col15 ima jedinstvene vrijednosti 19M! Korištenje naïve tehnike kao što su "jedan tipkovne kodiranje" za kodiranje takve visoke dimenzionalni categorical varijable je šifriraju. Točnije, ne možemo objašnjavaju i demonstrirati napredne, robusne tehnika pod naslovom [Upoznavanje s broji](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) za učinkovito tackling taj problem.

U ovom se odjeljku podređenu smo završiti tako da pogledate broj jedinstvenih vrijednosti za neke druge categorical stupce i. Sadržaj [uzorka & #95; grozd & #95; criteo & #95; jedinstveni & #95; vrijednosti i #95 više & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) su:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

To rezultira:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Ponovno Vidimo da osim Col20, sve druge stupce imaju brojne jedinstvene vrijednosti.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Suautorstvo pojavu broji od parova categorical varijable u skupu podataka vlaku

Suautorstvo pojavu brojanja parova categorical varijable je i koja vas zanima. To mogu se odrediti pomoću koda u [uzorka & #95; grozd & #95; criteo & #95; upareni & #95; categorical & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Radimo obrnutim Poredaj na broji prema svojim pojavu i u tom slučaju potražite pri vrhu 15. Tako ćete dobiti nam:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Dolje uzorka skupova podataka za Azure strojnog učenja

Imate istražili u skupove podataka i što je prikazano kako možda moramo ovu vrstu Istraživanje za sve varijable (uključujući kombinacije), ne možemo sada dolje uzorka skupove podataka tako da bismo mogli stvoriti modelima u Azure strojnog učenja. Opoziv koji se problem ne možemo usredotočite se na: uz skup primjer atributa (značajka vrijednosti iz st.2 - Col40), ne možemo predviđanje je li se stupcu 1 0 (nije definiran klik) i 1 (kliknite).

Dolje poslušajte naše vlaku i testiranje skupova podataka na izvornu veličinu % 1, ne možemo funkciju grozd, izvorni RAND(). Sljedeće skripte [uzorak i #95; grozd & #95; criteo & #95; Smanji & #95; vlaku & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) to za skup podataka vlaku:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

To rezultira:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Skripta [uzorka & #95; grozd & #95; criteo & #95; Smanji & #95; test & #95; dan i #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) ne je za testiranje podatke, dan\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

To rezultira:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Na kraju, skripte [uzorka & #95; grozd & #95; criteo & #95; Smanji & #95; test & #95; dan i #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) ne je za testiranje podatke, dan\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

To rezultira:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Uz to, ne možemo spremni za korištenje naše dolje sampled vlaku i testiranje skupova podataka za izgradnju modelima u Azure strojnog učenja.

Postoji konačni važne komponente prije nego što smo prijeći na učenje Azure računala koja je opasnosti tablici count. U sljedećem odjeljku podređenu ćemo razmotriti to u nekoliko detalja.

##<a name="count"></a>Kratak rasprave u tablici count

Kao što smo vidjeli više varijabli categorical imaju vrlo visoka dimensionality. U našem vodiču smo nude Napredna tehnika naziva kodiranje tih varijabli na način učinkovitiji, robusne [Upoznavanje s broji](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) . Dodatne informacije o ove tehnike nalazi se u vezu.

**Bilješke:** U ovom vodiču smo fokus na korištenje tablica count čime se dobiva sažetom reprezentacije visoke dimenzionalni categorical značajke. Ovo nije jedini način kodiranje categorical značajke; Dodatne informacije o drugih tehnika zainteresirani korisnici mogu provjeriti [jedan tipkovne-kodiranja](http://en.wikipedia.org/wiki/One-hot) i [značajka raspršivanje](http://en.wikipedia.org/wiki/Feature_hashing).

Da biste sastavili count tablice na broj podataka, u mapi neobrađenog/Brojanje koristimo podatke. U odjeljku Modeliranje Pokazat ćemo korisnici sastavljanje u ovim su tablicama broj za categorical značajke od nule, ili možete koristiti tablicu gotove broj za svoje explorations. Na koji način kada objedinjuje "gotove count tablice", ne možemo znači pomoću count tablice koje nudimo. Detaljne upute o tome kako pristupiti u ovim su tablicama navedeni su u sljedećem odjeljku.

## <a name="aml"></a>Sastavljanje model s Azure strojnog učenja

Naš model stvara postupak u Azure strojnog učenja slijedi ove korake:

1. [Dohvaćanje podataka iz tablica koje se vrste Hive u Azure strojnog učenja](#step1)
2. [Stvaranje pokusa: očistiti podatke, odaberite učenik i featurize s tablicama za brojanje](#step2)
3. [Obuka modela](#step3)
4. [Rezultat model podataka za testiranje](#step4)
5. [Analiza u model](#step5)
6. [Model Objavi kao članak na web-servisa za potrošnju](#step6)

Sada ćemo spremni ste za izradu modela u studio Azure strojnog učenja. Naš uzorkovanja podaci se spremaju kao grozd tablica u skupine prema dolje. Da biste pročitali te podatke koristimo modul Azure strojnog učenja **Uvoz podataka** . Vjerodajnice za pristup računu za pohranu od ovom klasteru isporučuju se što slijedi.

### <a name="step1"></a>Korak 1: Dohvaćanje podataka iz tablica koje se vrste Hive u Azure strojnog učenja modul uvoz podataka i odaberite za strojnog učenja eksperiment

Počnite odabirom s **+ NOVO** -> **EKSPERIMENT** -> **Eksperiment prazno**. Zatim iz okvira za **pretraživanje** u gornjem lijevom kutu potražite "Uvoz podataka". Povucite i ispustite modul za **Uvoz podataka** u području crtanja eksperiment (srednjem dijelu zaslona) da biste koristili modula za pristup podacima.

Evo kako **Uvoz podataka** izgleda prilikom dohvaćanja podataka iz tablice grozd:

![Uvoz podataka dohvaća podatke](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Za modul za **Uvoz podataka** samo Primjeri sortiranje vrijednosti, morate upisati su vrijednosti parametara koji navode na grafiku. Evo nekih opće smjernice o tome kako ispuniti parametar postavljen za modul za **Uvoz podataka** .

1. Odaberite "Grozd upita" za **Izvor podataka**
2. U **vrste Hive upita baze podataka** okvir jednostavne odaberite * FROM < vaše\_baze podataka\_name.your\_tablice\_naziv >-je dovoljno.
3. **Poslužitelj Hcatalog URI**: Ako svoj klaster je "abc", a zatim je jednostavno: https://abc.azurehdinsight.net
4. **Hadoop korisničko ime**: korisničko ime odabrali u trenutku commissioning klaster. (Nije daljinski pristup korisničko ime!)
5. **Lozinka Hadoop korisničkog računa**: lozinka za korisničko ime odabrali u trenutku commissioning klaster. (Nije daljinski pristup lozinku!)
6. **Mjesto za izlazne podatke**: odaberite "Azure"
7. **Naziv računa spremišta Azure**: račun za pohranu koji je povezan s klaster
8. **Ključ za račun Azure prostora za pohranu**: ključ računa za pohranu povezan s klaster.
9. **Naziv Azure spremnika**: Ako je naziv klaster "abc", a zatim to je jednostavno "abc", obično.


Nakon **Uvoza podataka** dovrši dohvaćanje podataka (zeleno na osi na vidite modul), spremiti podatke kao skup podataka (pod nazivom po izboru). Kako to izgleda:

![Uvoz podataka spremanje podataka](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Desnom tipkom miša kliknite izlazni priključak modula za **Uvoz podataka** . To objavljuje svoje mogućnosti za **Spremanje u obliku skup podataka** i **Vizualiziraj** mogućnost. Mogućnost **Vizualiziraj** ako kliknete, prikazuje 100 redaka podataka, zajedno s desnom oknu koje su korisne za neke statistički sažetak. Da biste spremili podataka, jednostavno odaberite **Spremi kao skup podataka** i slijedite upute.

Da biste odabrali spremljeni skup podataka za korištenje u strojnog učenja eksperiment, pronađite skupove podataka pomoću okvira za **pretraživanje** prikazano na sljedećoj slici. Jednostavno upišite naziv koji ste dali skup podataka djelomično da biste joj pristupiti i povucite skupu podataka na glavnom ploča. Da ga ispustite na glavnom ploča je odabire za korištenje u Modeliranje strojnog učenja.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] To se u vlaku i skupova podataka za testiranje. Osim toga, imajte na umu da biste koristili naziv baze podataka i nazive tablica koji ste dodijelili tu svrhu. Vrijednosti koji se koriste na slici su isključivo za ilustracija purposes.* *

### <a name="step2"></a>Korak 2: Stvaranje jednostavne eksperiment u Azure strojnog učenja za predviđanje klikova / nema klikova

Naš eksperiment Azure ML izgleda ovako:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Sada ćemo pregledajte ključne komponente ovaj eksperimenta. Podsjećamo moramo povucite naš spremljenu vlaku i najprije provjerite skupova podataka na našem eksperiment područje crtanja.

#### <a name="clean-missing-data"></a>Čišćenje podatke koji nedostaju

**Clean podataka koji nedostaju** modul ne što predlaže nazivu: ga briše podatke koji nedostaju na načine koji mogu biti naveden korisnik. Pogled u ovom modulu, možemo vidjeti sljedeće:

![CLEAN podatke koji nedostaju](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Ovdje, ne možemo odabrali da biste zamijenili sve vrijednosti koje nedostaju s 0. Dostupne su druge mogućnosti kao i, koju možete vidjeti tako da pogledate dropdowns u modulu.

#### <a name="feature-engineering-on-the-data"></a>Značajka inženjering na podatke

Može postojati milijune jedinstvene vrijednosti za neke categorical značajke velikih skupova podataka. Pomoću metode naïve kao što su jedan tipkovne kodiranja koji predstavlja značajke kao što su visoke dimenzionalni categorical je potpuno šifriraju. U ovom vodiču smo Demonstracija count značajke pomoću ugrađenih modula Azure strojnog učenja za generiranje sažetom nikakva jamstva od tih visoke dimenzionalni categorical varijabli. Rezultat završetka je manja veličina model, brže puta obuka i performanse metriku koja su prilično usporediti s pomoću drugih tehnika.

##### <a name="building-counting-transforms"></a>Sastavni brojeći pretvaranja

Da biste sastavili count značajke, koristimo modul **Sastavljanje brojeći transformacija** koja je dostupna u Azure strojnog učenja. Modul izgleda ovako:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Napomena o važnosti** : U okviru **broj stupaca** ne možemo unesite one stupce koje želimo izvedete broji. Obično su (kao što je rečeno) visoke dimenzionalni categorical stupaca. Na početku, ne možemo navedenim ima li Criteo dataset 26 categorical stupaca: iz Col15 za Col40. Ovdje, ne možemo Brojanje na svim od njih, a zatim dajte njihove indekse (od 15 do 40 odvojene zarezima, kao što je prikazano).

Da biste koristili modul u načinu MapReduce (prikladna za velikih skupova podataka), moramo pristup programa klaster HDInsight Hadoop (onog koji je korišten za istraživanje značajka se može ponovno koristiti u tu svrhu) i njegov vjerodajnice. Prethodni brojke prikazuju koje ispunjavaju u vrijednosti izgledaju (zamjena vrijednosti ilustracija s onima prikladno za vlastitu koristi slova).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

Na slici iznad, Pokazat ćemo kako unesite mjesto unos blob. To mjesto sadrži podatke rezervirana za sastavljanje tablice broj.


Nakon ovom modulu završi s radom, ne možemo možete spremiti transformacija za kasnije tako da desnom tipkom miša kliknete modul i odabirom mogućnosti za **Spremanje u obliku transformacija** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

U našem arhitekturi eksperiment gore navedenoj sintaksi dataset "ytransform2" odgovara točno spremljenu brojanja transformacije. Za ostatak ovaj eksperiment pretpostavkom da je koristiti modul za **Sastavljanje brojeći transformacija** na neke podatke za generiranje broji čitač, a možete koristiti te broji generiranje count značajke u vlaku i testiranje skupova podataka.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Odabir značajki koje broj da biste dodali kao dio vlaku i testiranje skupova podataka

Kada imamo count transformacija spremna, korisnik može odabrati što odlikuje se uključiti u svoje vlaku i testiranje skupove podataka modul **Izmijenite parametre Count tablice** . Ne možemo pokazati ovom modulu ovdje dovršenosti, ali u interese jednostavnosti zapravo ga ne koriste u našem eksperiment.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

U ovom slučaju, kao što je vidljivo, ne možemo ste odabrali da biste koristili samo zapisnika-vjerojatnost i da biste zanemarili natrag isključivanje stupca. Ne možemo, možete postaviti parametra kao što su smeće praga smeća koliko pseudo-prethodnog primjera da biste dodali za izglađivanje i hoćete li koristiti neki Šum Laplacian ili ne. Sve to su napredne značajke, a zatim je navedeno jesu dobru početnu točku za korisnike koji su nove toj vrsti generacije značajku na zadane vrijednosti.

##### <a name="data-transformation-before-generating-the-count-features"></a>Transformaciju podataka prije stvaranja značajke count

Sada ćemo usredotočite se na važne točke o Pretvorba naše vlaku i testiranje podataka prije no što zapravo generiranje značajke count. Imajte na umu da postoje dva **Izvršavanje skripte R** moduli korištene prije no što smo primijeniti transformaciju broj s oglednim podacima.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Ovo je prvi R skripte:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


U ovu skriptu R smo preimenujte naš stupca u "stupcu 1" da "Col40" Nazivi. To je zato brojanja transformacije očekuje imena ovaj oblik.

U drugom skripti R, ne možemo saldo raspodjele između pozitivne i negativne klase (klasa iz registra 1 i 0 odnosno) tako da smanjivanja negativan predmete. Skripta R ovdje prikazuje kako to učiniti:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

U ovom jednostavnu skriptu R koristimo "položaj\_neg\_proporcija" da biste postavili saldo između na pozitivne i negativne klase. To je važno da biste učinili jer obično poboljšanje predmete neravnoteži sadrži performansama za klasifikaciju probleme gdje se nalazi raspodjele predmete korelacije (opoziv da u našem slova imamo 3,3% pozitivan predmete i negativne klase 96.7%).

##### <a name="applying-the-count-transformation-on-our-data"></a>Primjena brojanja transformacije na oglednim podacima

Na kraju, koristimo modul **Primijeniti transformaciju** da biste primijenili brojanja transformacije naše vlaku i testiranje skupova podataka. Ovaj modul uzima pretvorbe spremljenu count kao jedan unos i skupova podataka vlaku ili test kao za ulazni i vraća podatke sa značajkama za brojanje. Ovdje je prikazana je:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>U isječak koji broj značajke izgleda

To je značajka da biste vidjeli broj značajke izgleda u našem slučaj. Ovdje pokazat u isječak ovog:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

U ovom isječak Pokazat ćemo za stupce koje ćemo broje se na ćemo dobiti na broji te prijavu vjerojatnost uz sve relevantne backoffs.

Spremni smo da biste sastavili modelu Azure strojnog učenja pomoću ove transformiranih skupova podataka. U sljedećem odjeljku, Pokazat ćemo kako to možete učiniti.

#### <a name="azure-machine-learning-model-building"></a>Azure sastavnih strojnog učenja modela

##### <a name="choice-of-learner"></a>Odabir učenik

Najprije ćemo morati odabrati u učenik. Ne možemo namjeravate koristiti stabla odlučivanja dva klase boosted kao naš učenik. Evo zadane mogućnosti za ovaj učenik:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Za naše eksperiment smo prolaze da biste odabrali zadane vrijednosti. Ne možemo Imajte na umu da zadanih obično smislen i dobar način da biste brzo referente vrijednosti na performanse. Možete poboljšati na performanse sweeping parametara Ako odlučite kada imate referentne vrijednosti.

#### <a name="train-the-model"></a>Obuka modela

Za osposobljavanje, jednostavno smo pozivanje modula **Vlaku modela** . Dva unosa na nju su učenik stabla odlučivanja Boosted dva predmete i naše vlaku skup podataka. To je prikazano ovdje:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Rezultat u model

Kada imamo obučeni model, ne možemo spremni rezultatu na test dataset i procjenu njegovih performansi. Ne možemo učiniti i pomoću modula **Rezultat modela** prikazano na sljedećoj slici, zajedno s modul za **Procjenu modela** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Korak 5: Analiza u model

Na kraju, željeli bismo analiza performansi modela. Obično dva probleme (binarne) klasifikacija predmete, mjera dobro je na AUC. Da biste to vizualizacija, smo priključiti modul **Rezultat modela** modul za **Procjenu Model** za to. Klikom na **Vizualiziraj** na modul za **Procjenu modela** rezultira grafiku poput sljedećih:

![Analiza modul BDT modela](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

U binarni (ili dvije klase) klasifikacija probleme, dobro mjeru točnosti predviđanja se u područje u odjeljku krivulje (AUC). U što slijedi, Pokazat ćemo naš rezultata pomoću ovog modela na našem test skup podataka. Da biste to, desnom tipkom miša kliknite priključak izlazne **Procijeniti modela** modul i zatim **Vizualiziraj**.

![Vizualizacija modul vrednovati modela](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Korak 6: Model Objavi kao članak na web-servisa
Mogućnost modelu Azure strojnog učenja Objavi kao članak na web-servisi s najmanje fuss je koristan značajka za podnošenje široko dostupan. Nakon toga, svi mogli upućivati pozive na web-usluge s ulazne podatke koje su im potrebne predviđanja za i web-servisa koristi ogledni model da biste se vratili te predviđanja.

Da biste to učinili, ne možemo spremiti naš obučeni modela kao obučeni modela objekta. To možete učiniti tako da pritisnete modul **Vlaku modela** i pomoću mogućnosti za **Spremanje u obliku obučeni modela** .

Sljedeći je korak da biste stvorili ulazni i izlazni priključke za naše web-servisa:

* unos priključak uzima podatke u obrascu isti kao podatke koje je potrebna predviđanja za
* izlazni priključak vraća natpise testu dobije i povezane vjerojatnosti.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Odabir nekoliko redaka podataka za unos priključak

Dobro je koristiti modul za **Primijeniti transformaciju SQL** da biste odabrali samo 10 redaka da bi služio kao priključak za unos podataka. Odaberite samo ova redaka podataka za naše ulazni priključak što je ovdje prikazano SQL upita:

![Priključak za unos podataka](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Web-servisa
Sada ćemo spremni pokrenuti small eksperiment koje je moguće koristiti za objavljivanje naš web-servisa.

#### <a name="generate-input-data-for-webservice"></a>Generiranje ulaznih podataka za webservice

Kao korak zeroth jer je prevelik broj tablice, ne možemo poduzeti nekoliko redaka podataka test i generiranje izlazne podatke iz njih sa značajkama za brojanje. To vam može poslužiti kao ulaznih podataka oblika za naše webservice. To je prikazano ovdje:

![Stvaranje BDT unos podataka](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Za oblikovanje ulaznih podataka sada koristimo IZLAZ iz modula **Count Featurizer** . Kada ovaj eksperiment završi s radom, spremite Izlaz iz modula **Count Featurizer** kao skup podataka. Ovaj skup podataka koristi se za unos podataka u na webservice.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Bilježenje rezultata eksperiment za objavljivanje webservice

Najprije ćemo pokazati kako to izgleda. Ključna struktura je **Rezultat modela** modul koji prihvaća naš obučeni modela objekta i nekoliko redaka ulaznih podataka koje ćemo generiraju u prethodnim koracima modul **Count Featurizer** . Koristimo "Odaberite stupce u skupu podataka" projekt natpise Scored i rezultat vjerojatnosti.

![Odaberite stupce u skupu podataka](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Obratite pozornost na to kako koristiti modul za **Odabir stupaca u skupu podataka** za 'filtrira' podataka iz skup podataka. Pokazat ćemo sadržaj ovdje:

![Filtriranje s odabir stupaca u modulu za skup podataka](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Da biste dobili plava ulazni i izlazni priključaka, jednostavno kliknite **Priprema webservice** slobodnog prostora. Izvodi ovaj eksperiment omogućuje nam da biste objavili web-usluge: kliknite ikonu **OBJAVLJIVANJE web-SERVISA** pri dnu desno, što je prikazano ovdje:

![Objavljivanje web-servisa](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Kada je objavljen na webservice, ćemo dobiti preusmjereni na stranicu koja izgleda stoga:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Na lijevoj strani Vidimo dvije veze za webservices:

* **ZAHTJEVA i odgovora** servisa (ili RRS) je namijenjena za jednu predviđanja i je što smo koristiti u ovom radionicu.
* Servis za **OBRADU IZVOĐENJA** (BES) koristi se za obradu predviđanja i zahtijeva da ulazne podatke koristi da bi predviđanja nalaze u spremište blobova platforme Azure.

Klikom na vezu **ZAHTJEVA i odgovora** nam vodi na stranicu koja daje nam unaprijed Konzervirano kod u C#, python i R. Kod može se praktično koristiti za upućivanje poziva na webservice. Imajte na umu da ključ za API na ovoj stranici nije potrebno koristiti za provjeru autentičnosti.

Dobro je da biste kopirali kod python na novu ćeliju u bilježnici IPython.

Ovdje Pokazat ćemo segmentu python kod s ispravan ključ za API-JA.

![Kod Python](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Imajte na umu da ćemo zamijeniti ključ za API zadani ključ za API naše webservices. Klikom na tu ćeliju u bilježnici IPython **pokretanje** rezultira odaziva na sljedeće:

![IPython odgovor](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Možemo vidjeti da za dva testa primjera ćemo vas o (u okvir JSON python skripte), ne možemo odgovori natrag u obliku "Scored naljepnice, Scored vjerojatnosti". Imajte na umu da u ovom slučaju ne možemo odabrali zadane vrijednosti koje unaprijed Konzervirano kod nudi (0 za sve stupce s brojčanim vrijednostima i niz "vrijednost" za sve stupce categorical).

To završava naš vodič završetka do kraja pokazuje kako rukovati veliki skup podataka pomoću Azure strojnog učenja. Ne možemo rada s terabajta podataka, konstruirana model predviđanje i implementirati kao web-servisa u oblaku.
