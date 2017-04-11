<properties 
    pageTitle="Instalirajte Jupyter bilježnice na računalo i povežite ga s programa HDInsight Spark klaster | Microsoft Azure" 
    description="Saznajte kako bilježnicu Jupyter lokalno na računalo instalirati i povežite ga s programa Apache Spark klaster na Azure HDInsight." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Instalirajte Jupyter bilježnice na računalo i povezati Apache Spark klaster na HDInsight Linux

U ovom članku će Saznajte kako instalirati bilježnice Jupyter s prilagođene PySpark (za Python) i Spark (za Scala) jezgre s Spark poseban i povezivanje bilježnicu programa klaster HDInsight. Može postojati nekoliko razloga da biste instalirali Jupyter na lokalnom računalu, a može biti neke izazove. Popis razloga i izazove potražite u odjeljku [Zašto instalirati Jupyter na računalu](#why-should-i-install-jupyter-on-my-computer) na kraju ovog članka.

Postoje tri koraka ključa prilikom instalacije Jupyter i poseban Spark na vašem računalu.

* Instalacija Jupyter bilježnice
* Instalacija jezgre PySpark i Spark sa poseban Spark
* Konfiguriranje poseban Spark da biste pristupili Spark klaster na HDInsight

Dodatne informacije o prilagođene jezgre i dostupni za Jupyter bilježnice sa servisa HDInsight klaster poseban Spark potražite u članku [jezgre dostupne za Jupyter bilježnica s Apache Spark Linux klastere na HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Preduvjeti

Preduvjeti nalaziti nisu za instalaciju Jupyter. To su o povezivanju sa servisom Jupyter bilježnicu programa HDInsight klaster nakon instalacije bilježnicu.

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Upute potražite u članku [Stvaranje Spark Apache klastere u Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Instalirajte Jupyter bilježnice na računalo

Morat ćete instalirati Python da biste mogli instalirati Jupyter bilježnice. Python i Jupyter dostupni su u sklopu [Ananconda distribuciju](https://www.continuum.io/downloads). Kada instalirate Anaconda, zapravo instalirajte distribucija Python. Kad instalirate Anaconda, dodajte Jupyter instalaciju tako da pokrenete naredbu. Ovo poglavlje sadrži upute koje morate pratiti.

1. Preuzmite [Anaconda instalacijski program](https://www.continuum.io/downloads) za svoju platformu i pokrenite instalaciju. Prilikom pokretanja čarobnjaka za postavljanje, provjerite jeste li odabrali mogućnost da biste dodali Anaconda put varijablu.

2. Pokrenite sljedeću naredbu da biste instalirali Jupyter.

        conda install jupyter

    Dodatne informacije o installting Jupyter potražite u članku [Instaliranje Jupyter pomoću Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Instalirajte jezgre i poseban Spark

Upute za instaliranje poseban Spark jezgre PySpark i Spark potražite u članku [sparkmagic dokumentaciju](https://github.com/jupyter-incubator/sparkmagic#installation) na GitHub.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Konfiguriranje poseban Spark da biste pristupili HDInsight Spark klaster

U ovom odjeljku Konfiguriranje poseban Spark koji ste ranije instalirali za povezivanje programa Apache Spark klaster koje morate već ste stvorili u Azure HDInsight.

1. Informacije o konfiguraciji Jupyter obično je pohranjena u direktoriju kućne korisnike. Da biste pronašli osnovne mape na bilo kojoj platformi OS, upišite sljedeće naredbe.

    Pokrenite ljuske Python. U prozoru za naredbe upišite sljedeće:

        python

    Na ljuske Python unesite sljedeću naredbu da biste saznali osnovne mape.

        import os
        print(os.path.expanduser('~'))

2. Idite do kućne i stvorite mapu naziva **.sparkmagic** ako se još ne postoji.

3. U mapi, stvorite datoteku s nazivom **config.json** i dodajte sljedeće JSON isječak unutar njega.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. **{USERNAME}**, **{CLUSTERDNSNAME}**i **{BASE64ENCODEDPASSWORD}** zamijeniti odgovarajuće vrijednosti. Da biste generirali base64 kodirani lozinku za lozinku actualy možete koristiti broj uslužni programi omiljene programski jezik ili Internetu. Jednostavan isječak Python za pokretanje iz naredbenog retka bio sljedeći:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Pokrenite Jupyter. Koristite sljedeću naredbu iz naredbenog retka.

        jupyter notebook

6. Provjerite možete li se povezati s klaster koristiti bilježnicu Jupyter i koristiti poseban Spark dostupne s na jezgre. Izvršite sljedeće korake.

    1. Stvaranje nove bilježnice. U desnom kutu kliknite **Novo**. Trebali biste vidjeti zadani otklanjanje **Python2** i na dva nova jezgre koje instalirate, **PySpark** i **Spark**.

        ![Stvaranje nove bilježnice Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Stvaranje nove bilježnice Jupyter")

    
        Kliknite **PySpark**.


    2. Pokrenite sljedeće isječak koda.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Ako je uspješno možete dohvatiti Izlaz, testira vezu klaster HDInsight.

    >[AZURE.TIP] Ako želite ažurirati konfiguraciju bilježnice povezati s različitim klaster, ažurirajte na config.json pomoću novog skupa vrijednosti, kao što je prikazano u koraku 3 iznad. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Zašto instalirati Jupyter na mom računalu?

Može postojati nekoliko razloga zašto možda želite instalirati Jupyter na računalo i povežite ga s klaster Spark na HDInsight.

* Iako Jupyter bilježnica već su dostupni na klasteru Spark u Azure HDInsight, instalacije Jupyter na vašem računalu omogućuje vam mogućnost stvaranja bilježnice lokalno, testirajte aplikacije protiv izvodi klaster, a zatim prijenos bilježnice da biste klaster. Da biste prenijeli bilježnice na klaster, možete prenesite ih pomoću Jupyter bilježnice sa sustavom ili klaster, ili ih spremiti u mapu /HdiNotebooks u račun za pohranu koji je povezan s klaster. Dodatne informacije o kako bilježnice pohranjuju se na klaster, potražite u članku [se bilježnica Jupyter pohranjuju](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* S bilježnicama dostupan lokalno, možete povezati s različitim Spark klastere koji se temelji na svojim potrebama aplikacije.
* Možete koristiti GitHub implementirati izvorišnog sustava kontrole i kontrolirati verzija za bilježnice. Također može imati okruženje za suradnju s istoj bilježnici na kojem možete raditi više korisnika.
* Možete raditi s bilježnicama lokalno pritom ne morate otvarati klaster prema gore. Potrebno je samo klaster da biste testirali bilježnice odnosu, ne želite ručno upravljanje bilježnice ili razvojno okruženje.
* Možda će biti lakše konfiguriranje vlastitu lokalnu razvojno okruženje nego što je da biste konfigurirali instalacije Jupyter klaster.  Možete iskoristiti prednost sve programe koje ste instalirali lokalno niste konfigurirali jedan ili više remote klastere.

>[AZURE.WARNING] S Jupyter na lokalnom računalu instaliran, više korisnika mogu se izvoditi istoj bilježnici na isti klaster Spark u isto vrijeme. U takvim situaciji stvaraju se više Livije sesije. Ako naiđete na problem i želite za ispravljanje pogrešaka koje, bit će složeno za praćenje koji sesiju Livije pripada koji je korisnik.




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

* [Korištenje vanjskih paketa s bilježnicama Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Upravljanje resursima

* [Upravljanje resursima za klaster Apache Spark u Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Praćenje i ispravljanje pogrešaka zadataka izvodi na programa klaster Apache Spark u HDInsight](hdinsight-apache-spark-job-debugging.md)
