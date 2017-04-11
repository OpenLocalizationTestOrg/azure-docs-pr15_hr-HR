<properties
    pageTitle="Pregled podataka znanstvenog pomoću Spark Azure HDInsight | Microsoft Azure"
    description="Komplet alata za Spark MLlib poziva dosta strojno učenje Modeliranje mogućnosti raspodijeljeno okruženje HDInsight."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Pregled podataka znanstvenog pomoću Spark Azure HDInsight

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ovaj paket teme pokazuje kako koristiti HDInsight Spark da biste dovršili uobičajene zadatke znanstvenog podataka kao što je ingestion podataka, značajka inženjering, Modeliranje i procjenu modela. Podaci koji se koriste se uzorak 2013 NEW taksi putovanja i prijevoz skupu podataka. Modeli u komponenti obuhvaćaju logistika i linearne regresije, slučajni šuma i prijelaza boosted stabla. Tema prikazati i kako spremiti te modela u spremište blobova platforme Azure (WASB) i rezultatu i procjenu njihove predvidljivu performanse. Dodatne teme pokrivaju kako može biti modelima put uvježbavan pomoću unakrsno provjere valjanosti i hyper parametar sweeping. U ovoj se temi pregled i opisuje kako postaviti klaster Spark koje morate proći kroz korake u tri vodiči navedene. 

[Spark](http://spark.apache.org/) je programa paralelno Otvori izvor obrade framework koji podržava obrada u memoriji da biste uvećali performanse velikih skupova podataka analitički aplikacije. Modul za obradu Spark ugrađena je za brzinu, lakšeg korištenja i složene analize. Mogućnosti u memoriji raspodijeljeno izračunavanje Spark na ga činite dobar izbor za izračun s iteracijama algoritama computations strojnog učenja i grafikonu. [MLlib](http://spark.apache.org/mllib/) je biblioteka učenje skalabilni strojno Spark, koji objedinjuje mogućnosti modeliranja ovaj raspodijeljeno okruženje. 

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) je Azure glavnom računalu funkcija ovog Spark Otvori izvor. Sadrži i podrška za **Jupyter PySpark bilježnice** na klaster Spark koji se može izvoditi Spark SQL interaktivne upite radi pretvorba, filtriranja i vizualizacija podataka pohranjenih u Azure blob polja (WASB). PySpark je API Python za Spark. Na koda koje omogućuju rješenja te se navode odgovarajući iscrtavaju na vizualizaciju podataka ovdje pokrenuti u bilježnicama Jupyter instalirano klastere Spark. Modeliranje korake navedene u ovim temama sadržavati kod koji pokazuje kako uvježbati, Analiza, spremanje i zauzeti svake vrste modela. 

Korake za postavljanje i koda u prikazu odnosi na HDInsight 3.4 Spark 1.6. Međutim, kod ovdje u bilježnicama je generički i raditi na bilo kojem Spark klaster. Ako se ne koriste HDInsight Spark, klaster korake postavljanja i upravljanje mogu se razlikovati od što je prikazano ovdje.

## <a name="prerequisites"></a>Preduvjeti

1. morate imati pretplatu na Azure. Ako nije već postoji, potražite u članku [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. potreban vam je klaster HDInsight 3.4 Spark 1.6 da biste dovršili ovaj vodič. Da biste je stvorili, pogledajte upute u [Početak rada: Stvaranje Apache Spark na Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Na izborniku **Odaberite vrstu klaster** navedena je vrsta klaster i verziju. 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Tema koji pokazuje kako koristiti Scala umjesto Python da biste dovršili zadatke za proces znanstvenog završetka do kraja podataka potražite u članku [znanstvenog podataka pomoću Scala s Spark na Azure](machine-learning-data-science-process-scala-walkthrough.md).

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>Podatke o SEBI 2013 taksi

NEW taksi putovanja podaci otprilike 20 GB sažete vrijednosti odvojenih zarezom (CSV) datoteke (dekomprimirati ~ 48 GB), comprising više od 173 milijuna pojedinačne trips i na fares platiti za svaki put. Svaki zapis putovanja obuhvaća odaberite prema gore i predaju mjesto i vrijeme, anonymized hack (upravljačkog programa) broj licenci i broj medallion (taksi na jedinstveni ID-ja). Podaci pokriva sve trips u godini 2013 uvjet, a u sljedeća dva skupova podataka za svaki mjesec:

1. CSV datoteke 'trip_data' sadrže detalje putovanja, kao što je broj passengers, Podignite i dropoff točaka, povratnog trajanje i duljine putovanja. Evo nekoliko uzoraka zapisa:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. CSV datoteke 'trip_fare' sadrže detalje o prijevoz platiti za svaki put, kao što je način plaćanja, prijevoz iznos, dodatna naknada i poreza, savjete i tolls, i ukupan iznos plaćen. Evo nekoliko uzoraka zapisa:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5


Ćemo snimanja uzorka 0,1% od ovih datoteka i pridružite se putovanja\_podataka i putovanja\_prijevoz CVS datoteke u jednom skupu podataka da biste koristili kao unos dataset ovaj vodič. Jedinstveni ključ da biste se pridružili putovanja\_podataka i putovanja\_prijevoz sastoji se od polja: medallion, hack\_krajnjeg i prijem\_datuma i vremena. Svaki zapis u skupu podataka sadrži koji predstavlja NEW taksi putovanja sljedećim atributima:


|Polje| Kratak opis
|------|---------------------------------
| medallion |Anonymized taksi medallion (taksi jedinstveni id)
| hack_license |    Anonymized broj znakova za novi red Hackney licence
| vendor_id |   Id dobavljača taksi
| rate_code | NEW taksi rata prijevoz
| store_and_fwd_flag | Pohrana i prosljeđivanje zastavica
| pickup_datetime | Obraditi datum i vrijeme
| dropoff_datetime | Dropoff datum i vrijeme
| pickup_hour | Obraditi sat
| pickup_week | Obraditi tjedan u godini
| dan u tjednu | WEEKDAY (1-7 u rasponu)
| passenger_count | Broj passengers u taksi putovanja
| trip_time_in_secs | Putovanja vrijeme u sekundama
| trip_distance | Udaljenost putovanja putovali u milja
| pickup_longitude | Obraditi dužini
| pickup_latitude | Obraditi zemljopisnu širinu
| dropoff_longitude | Dužina Dropoff
| dropoff_latitude | Zemljopisnu širinu Dropoff
| direct_distance | Izravni udaljenost između odaberite prema gore i dropoff mjesta
| payment_type | Način plaćanja (CA, kreditne kartice itd.)
| fare_amount | Iznos prijevoz u
| Dodatna naknada | Dodatna naknada
| mta_tax | MTA poreza
| tip_amount | Savjet iznos
| tolls_amount | Iznos tolls
| total_amount | Ukupni iznos
| tipped | Zakrenut (0 i 1 za bez ili da)
| tip_class | Savjet klase (0: $0, 1: $0 do 5, 2: $6-10, 3: 11-20 USD, 4: > 20 USD)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Izvršavanje koda iz bilježnice Jupyter na Spark klaster 

Možete pokrenuti Jupyter bilježnice na portalu Azure. Pronađite svoj klaster Spark na nadzornoj ploči i kliknite ga da biste unijeli stranicu za upravljanje za svoj klaster. Da biste otvorili bilježnicu pridružene Spark klaster, kliknite **Nadzornih ploča klaster** -> **Jupyter bilježnice** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Možete pregledavati i da biste ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** za pristup bilježnicama Jupyter. Zamjena dijela CLUSTERNAME ovaj URL naziv vlastite klaster. Potrebna je lozinka za administratorski račun za pristup bilježnicama.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Odaberite PySpark da biste vidjeli direktorij koji sadrži nekoliko primjera unaprijed zapakirani bilježnice koje pomoću PySpark API-JA. Bilježnice koje sadrže primjere koda ovaj paket Spark teme dostupne su na [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


Bilježnica izravno iz Github s poslužiteljem Jupyter bilježnice možete prenijeti na svoj klaster Spark. Na početnoj stranici vaš Jupyter kliknite gumb **Prenesi** na desnom dijelu zaslona. Otvara se Eksplorer za datoteke. Ovdje zalijepite URL Github (neobrađenog sadržaja) bilježnice i kliknite **Otvori**. Bilježnica PySpark dostupne su na sljedeći URL-ovi:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Vidite naziv datoteke na popisu datoteka Jupyter s gumbom za **Prijenos** ponovno. Kliknite ovaj gumb **Prenesi** . Sada ste uvezli bilježnicu. Ponovite te korake da biste prenijeli bilježnica s ovaj vodič.

> [AZURE.TIP] Možete desnom tipkom miša kliknite veze u pregledniku pa odaberite **Kopiraj vezu** na dohvaćanje github neobrađenog sadržaja URL-a. Ovaj URL možete zalijepiti u Jupyter prijenos datoteka explorer dijaloški okvir.

Sada možete učiniti sljedeće:

- Da biste vidjeli šifru tako da kliknete bilježnicu.
- Izvršavanje svaku ćeliju pritiskom na kombinaciju tipki **SHIFT ENTER**.
- Klikom na cijele bilježnice na **ćeliju** -> **Pokreni**.
- Korištenje automatskog vizualizacije upita.

> [AZURE.TIP] Otklanjanje PySpark automatski vizualiziraju izlaz upita SQL (HiveQL). Ponudit će vam mogućnost da biste odabrali nekoliko različitih vrsta vizualizacija (tablice, tortni, linijski, područje ili trake) pomoću gumba izbornika **Vrsta** u bilježnicu:

![Logistika regresije ROC krivulje za generički pristup](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Što je sljedeće?

Sad kad su postavljen na HDInsight Spark klaster, a ste prenijeli Jupyter bilježnice, spremni ste za rješavati temama koje odgovaraju na tri PySpark bilježnice. Prikazuju kako pregledavanje podataka, a zatim kako stvoriti i zauzeti modela. Dodatne podatke Istraživanje i Modeliranje bilježnice pokazuje kako uključiti unakrsno provjere valjanosti, hyper-parametar sweeping, i model procjenu. 

**Istraživanje podataka i Modeliranje s Spark:** Istraživanje skupu podataka i stvaranje rezultatom, i analiza u strojnog učenja modelima krenete kroz temi [Stvaranje binarni klasifikaciju i regresije modela za podataka pomoću alata za Spark MLlib](machine-learning-data-science-spark-data-exploration-modeling.md) .

**Modela potrošnje:** Da biste saznali kako rezultatu klasifikaciju i regresije modele stvorene u ovoj temi, potražite u članku [rezultat i procjenu Spark u komponenti strojno učenje modelima](machine-learning-data-science-spark-model-consumption.md).

**Unakrsno provjere valjanosti i hyperparameter sweeping**: na kako može biti modela u odjeljku [Napredne Istraživanje podataka i Modeliranje s Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) put uvježbavan pomoću unakrsno provjere valjanosti i hyper parametar sweeping

