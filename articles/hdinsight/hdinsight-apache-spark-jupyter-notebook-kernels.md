<properties 
    pageTitle="Dostupno u sklopu Jupyter bilježnica na HDInsight Spark jezgre klaster na Linux | Microsoft Azure" 
    description="Saznajte više o na dodatne Jupyter bilježnice jezgre dostupno u sklopu klaster Spark na HDInsight Linux." 
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


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Dostupno za Jupyter bilježnica s Apache Spark klastere na HDInsight Linux jezgre

Klaster Apache Spark na HDInsight (Linux) sadrži Jupyter bilježnice koje možete koristiti za testiranje aplikacija. Na otklanjanje je program koji će se pokrenuti i tumači kod. HDInsight Spark klastere sadrže dva jezgre koje možete koristiti s bilježnicom Jupyter. To su:

1. **PySpark** (za aplikacije pisane Python)
2. **Spark** (za aplikacije pisane Scala)

U ovom se članku će informacije o korištenju tih jezgre i koje su prednosti koje ste dobili od njihova korištenja.

**Preduvjeti:**

Morate imati sljedeće:

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Upute potražite u članku [Stvaranje Spark Apache klastere u Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Kako se koristi u jezgre? 

1. S [Portala za Azure](https://portal.azure.com/), iz startboard, kliknite pločicu za svoj klaster Spark (Ako ste ga prikvačiti na startboard). Možete se kretati svoj klaster u odjeljku **Pregledaj sve** > **Klastere HDInsight**.   

2. Iz klaster plohu Spark kliknite **Nadzorna ploča klaster**, a zatim **Jupyter bilježnice**. Ako se to od vas zatraži, unesite administratorskih vjerodajnica za klaster.

    > [AZURE.NOTE] Možda dosegne i Jupyter bilježnicu za svoj klaster tako da otvorite sljedeći URL u pregledniku. Zamijenite __CLUSTERNAME__ pod nazivom svoj klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Stvaranje nove bilježnice s novi jezgre. Kliknite **Novo**, a zatim kliknite **Pyspark** ili **Spark**. Trebali biste koristiti Spark Jezgra za Scala aplikacije i otklanjanje PySpark Python aplikacije.

    ![Stvaranje nove bilježnice Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Stvaranje nove bilježnice Jupyter") 

3. To otvarajte novu bilježnicu s otklanjanje koji ste odabrali.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Zašto koristiti jezgre PySpark ili Spark?

Evo nekoliko prednosti korištenja nove jezgre.

1. **Postavljanje konteksta**. S **PySpark** ili **Spark** jezgre koji se isporučuju sa Jupyter bilježnice, ne morate izričito postavljanje konteksta Spark ili grozd prije no što počnete raditi s aplikacijom razvijate; To su dostupni za po zadanom. Ove konteksta su:

    * **sc** - za Spark kontekst
    * **sqlContext** - za grozd kontekst


    Tako, ne morate pokrenuti naredbe kao što su sljedeće da biste postavili na konteksta:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Umjesto toga možete izravno koristite unaprijed konteksta u aplikaciji.
    
2. **Magics ćelije**. Otklanjanje PySpark sadrži neke unaprijed definirane "magics", koji su posebne naredbe da biste uputili poziv s `%%` (npr. `%%MAGIC` <args>). Uz naredbe mora biti prve riječi u ćeliji kod i omogućuju više redaka sadržaja. Uz word mora biti prve riječi u ćeliji. Dodavanje ništa prije poseban, čak i komentari, uzrokovat će pogrešku.   Dodatne informacije o magics potražite [ovdje](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    U tablici u nastavku navedene različite magics dostupne putem na jezgre.

  	| Poseban     | Primjer                         | Opis  |
  	|-----------|---------------------------------|--------------|
  	| Pomoć      | `%%help`                            | Stvara tablice dostupne magics s primjer i opis |
  	| Info      | `%%info`                          | Izlaze sesiju informacije za trenutni Livije krajnja točka |
  	| Konfiguriranje | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Konfigurira parametara za stvaranje sesije. Zastavica prisilno (-f) je obavezna ako sesije već stvoren i sesiju bit će ispušteni i ponovno stvoriti. Pogledajte [Livije na objavu /sessions tijelu zahtjeva](https://github.com/cloudera/livy#request-body) za popis valjanih parametara. Parametri mora proslijediti u kao niz JSON i mora biti u sljedećem retku nakon poseban, kao što je prikazano u primjeru stupac. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Izvršava upit grozd poslan na sqlContext. Ako u `-o` proslijeđen parametar, rezultat upita je ista i na na %% lokalne kontekst Python kao [Pandas](http://pandas.pydata.org/) dataframe.   |
  	| Lokalni     |     `%%local`<br>`a=1`              | Kod kontrola sadržaja sastavnog bloka će se izvršavati lokalno. Šifra mora biti valjane Python kod. |
  	| Zapisnici      | `%%logs`                        | U zapisnicima trenutna sesija Livije izlaza.  |
  	| Brisanje    | `%%delete -f -s <session number>` | Brisanje određenih sesiju trenutni Livije krajnje točke. Imajte na umu da nije moguće izbrisati sesiju koje se aktivira za otklanjanje sam. |
  	| Čišćenje diska   | `%%cleanup -f`                    | Briše sve sesije za trenutni Livije krajnju točku, uključujući sesiju ovu bilježnicu. Prisilno zastavice -f je obavezna.  |

    >[AZURE.NOTE] Osim magics dodao otklanjanje PySpark, možete koristiti i [ugrađene IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), uključujući `%%sh`. Možete koristiti u `%%sh` poseban pokrenuti skripte i blokova Šifra headnode klaster. 

3. **Automatsko vizualizaciju**. Otklanjanje **Pyspark** automatski vizualiziraju izlaz upita grozd i SQL. Imate mogućnost da biste odabrali jednoliko nekoliko različitih vrsta vizualizacija, uključujući tablice, tortni, linijski, područje, traka.

## <a name="parameters-supported-with-the-sql-magic"></a>Parametri podržan u %% poseban sql

U %% poseban sql podržava druge parametre koje možete koristiti da biste odredili vrstu izlaza koje primate prilikom pokretanja upita. U sljedećoj su tablici navedeni izlaz.

| Parametar     | Primjer                         | Opis  |
|-----------|---------------------------------|--------------|
| -o      | `-o <VARIABLE NAME>`                          | Koristite ovaj parametar održati rezultat upita u na %% lokalne kontekst Python, kao [Pandas](http://pandas.pydata.org/) dataframe. Naziv varijable dataframe je naziv varijable koju navedete. |
| -pitanja      | `-q`                          | Koristi se za isključivanje vizualizacije za ćeliju. Ako ne želite da automatski vizualizacija sadržaj ćelije, a samo želite snimiti kao u dataframe, a zatim pomoću `-q -o <VARIABLE>`. Ako želite isključiti vizualizacije bez dohvaćanje rezultata (npr. za izvođenje SQL upita sa strane efekata, poput na `CREATE TABLE` izjava), jednostavno iskoristiti `-q` bez navođenja na `-o` argument. |
| -m       |  `-m <METHOD>`    | Gdje je **način** **ponijeti** ili **uzorka** (Zadana vrijednost je **poduzeti**). Ako je način **poduzeti**, Jezgra sustava izdvajanja elemenata od vrha skup podataka rezultat određen MAXROWS (što je opisano u nastavku ove tablice). Ako je način **uzorka**, Jezgra sustava slučajno poslušajte elementi skupa podataka na sljedeći način `-r` parametar, opisano u sljedećoj tablici.   |
| -r     |     `-r <FRACTION>`            | Ovdje je **RAZLOMAK** broj s pomičnim zarezom između 0.0 i 1.0. Ako je način uzorka za SQL upita `sample`, a zatim Jezgra sustava slučajno uzoraka navedeni razlomak elemenata rezultat postavljen za vas; Npr. Ako pokrenete SQL upita s argumentima `-m sample -r 0.01`, zatim 1% redaka rezultat će biti slučajno uzorkovanja. |
| -n      | `-n <MAXROWS>`                        | **MAXROWS** je cijeli broj. Jezgra sustava će ograničiti broj redaka za izlaz za **MAXROWS**. Ako je **MAXROWS** negativan broj kao što su **-1**, zatim broj redaka u skupu rezultata neće biti ograničeni. |

**Primjer:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

Iskaz iznad čini sljedeće:

* Odabire sve zapise iz **hivesampletable**.
* Budući da koristimo - pitanja, isključuju se automatski vizualizaciju.
* Budući da koristimo `-m sample -r 0.1 -n 500` slučajno uzoraka 10% više redaka u na hivesampletable i ograničenjima veličine 500 redova skupa rezultata.
* Na kraju, jer smo `-o query2` i sprema Izlaz u dataframe pod nazivom **upit2**.
    

## <a name="considerations-while-using-the-new-kernels"></a>Okolnosti pri korištenju novog jezgre

Bez obzira otklanjanje koristite (PySpark ili Spark), ostavite zauzeti će bilježnica izvodi klaster resurse.  Pomoću ove jezgre jer su unaprijed u kontekstu, jednostavno izađite iz bilježnice ne ukloni kontekstu i zato na resurse klaster će se i dalje se koristi. Bilo bi dobro s jezgre PySpark i Spark da biste koristili mogućnost **Zatvori i zastoj** **zajedničko** korištenje bilježnice. Time se kills kontekstu i Zatvori bilježnicu.    


## <a name="show-me-some-examples"></a>Pokaži mi primjere

Kada otvorite bilježnicu Jupyter, vidjet ćete dvije mape dostupne na korijenskoj razini.

* Mapa **PySpark** sadrži ogledne bilježnice koje pomoću novog **Python** otklanjanje.
* Mapa **Scala** sadrži ogledne bilježnice koje pomoću novog otklanjanje **Spark** .

**00 - [ČITATI ME prvi] Spark značajke otklanjanje poseban** bilježnice možete otvoriti iz mape **PySpark** ili **Spark** dodatne informacije o različitim magics dostupna. Možete koristiti i druge uzorka bilježnice dostupne u odjeljku dvije mape da biste saznali kako postizanju scenarija Jupyter bilježnica pomoću klastere HDInsight Spark.

## <a name="where-are-the-notebooks-stored"></a>Gdje su pohranjene bilježnice?

Bilježnica Jupyter spremaju se na račun za pohranu koji je povezan s klaster u mapi **/HdiNotebooks** .  Bilježnica, tekstne datoteke i mape koje stvarate iz unutar Jupyter bit će dostupni iz WASB.  Na primjer, ako koristite Jupyter da biste stvorili mapu **myfolder** i bilježnice **myfolder/mynotebook.ipynb**, možete pristupiti bilježnice na `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Na vrijedi i obrnuto, odnosno ako prenesete bilježnice izravno na račun za pohranu na `/HdiNotebooks/mynotebook1.ipynb`, bilježnica će biti vidljivi Jupyter kao i.  Bilježnica će ostati u račun za pohranu čak i nakon brisanja klaster.

Način bilježnice su spremljeni na račun za pohranu kompatibilan sa sustavom HDFS. Pa, ako SSH u klaster možete koristiti arhivirate upravljanje naredbama kao što su sljedeće:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


U slučaju da postoje problemi pristupate računu za pohranu za klaster, bilježnice također će se spremiti na na headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Podržani preglednik
Bilježnica Jupyter izvodi protiv klastere HDInsight Spark podržani su samo na Google Chrome.

## <a name="feedback"></a>Povratne informacije

Novi jezgre u razvoj fazu, a će sadržaj za odrasle tijekom vremena. To nije moguće i znači da API-ji može promijeniti kako se te jezgre sadržaj za odrasle. Želite cijenimo povratne informacije koje imate tijekom korištenja ove novi jezgre. To će biti vrlo koristan u oblikovanju konačno izdanje ove jezgre. Ostavite komentare i povratne informacije u odjeljku **Komentari** pri dnu ovog članka.


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

* [Korištenje vanjskih paketa s bilježnicama Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Na računalo instalirati Jupyter i povezati se HDInsight Spark klaster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Upravljanje resursima

* [Upravljanje resursima za klaster Apache Spark u Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Praćenje i ispravljanje pogrešaka zadataka izvodi na programa klaster Apache Spark u HDInsight](hdinsight-apache-spark-job-debugging.md)
