<properties
    pageTitle="Stvaranje Spark klaster na HDInsight Linux i pomoću SQL Spark iz Jupyter interaktivne analize | Microsoft Azure"
    description="Detaljne upute o tome kako brzo stvoriti programa Spark Apache skupine u HDInsight, a zatim pomoću Spark SQL iz Jupyter bilježnice da biste pokrenuli interaktivne upita."
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
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Početak rada: Stvaranje Apache Spark klaster na HDInsight Linux i pokretanje interaktivne upita pomoću Spark SQL

Upute za stvaranje programa klaster Apache Spark u HDInsight, a zatim pomoću [Jupyter](https://jupyter.org) bilježnice da biste pokrenuli Spark SQL interaktivne upita na klasteru Spark.

   ![Prvi koraci pri korištenju Apache Spark u HDInsight] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Početi koristiti Apache Spark HDInsight praktičnog vodiča. Koraci prikazanom: stvaranje računa za pohranu; Stvaranje klaster; Pokrenite Spark SQL naredbe")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Preduvjeti

- **Mogući Azure pretplate**. Prije početka ovog praktičnog vodiča, morate imati pretplatu na Azure. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Klijent ljuske sigurne odgovora (SSH)**: Linux, Unix i OS X sustavi provied programa SSH klijenta do na `ssh` naredbe. Za sustave Windows, preporučujemo da [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Tipke sigurne ljuske (SSH) (neobavezno)**: možete zaštititi SSH računa koji se koristi za povezivanje s klaster pomoću lozinke ili javni ključ. Pomoću lozinke rada brzo dobiti i koristite ovu mogućnost ako želite brzo stvaranje klaster i pokretanje neke zadatke test. Korištenje ključa je sigurnije, no bit će potrebno dodatne postavke. Možda ćete morati koristiti takvog prilikom stvaranja radnog klaster. U ovom se članku koristimo pristup lozinku. Upute za stvaranje i korištenje SSH tipke sa servisa HDInsight potražite u sljedećim člancima:

    -  Linux s računala sa sustavom – [Korištenje SSH s operacijskim sustavom Linux HDInsight (Hadoop) iz Linux, Unix, ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Na računalu sa sustavom Windows – [Korištenje SSH sa sustavom Linux HDInsight (Hadoop) iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] U ovom se članku koristi predložak Upravitelj Azure resursa za stvaranje Spark klaster koji koristi [Blob polja za pohranu Azure kao klaster prostora za pohranu](hdinsight-hadoop-use-blob-storage.md). Možete stvoriti i Spark klaster koji koristi [Pohranu Lake podataka za Azure](../data-lake-store/data-lake-store-overview.md) kao programa dodatni prostor za pohranu, osim blob polja za pohranu Azure kao zadani prostor za pohranu. Upute potražite u članku [Stvaranje programa HDInsight klaster s trgovinom Lake podataka](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Stvaranje Spark klaster

U ovom ćete odjeljku stvorite HDInsight verziju 3.4 klaster (Spark verzija 1.6.1) pomoću upravitelja predloška Azure resursa. Informacije o verzijama HDInsight i njihovih SLA potražite u članku [HDInsight komponente verzijama](hdinsight-component-versioning.md). Drugi načini stvaranja klaster potražite u članku [Stvaranje HDInsight klastere](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknite na sljedećoj slici da biste otvorili predložak na portalu za Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Predložak nalazi se u spremniku javno blob, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Iz plohu parametara unesite sljedeće:

    - **ClusterName**: Unesite naziv klaster Hadoop koje ćete stvoriti.
    - **Klaster korisničko ime i lozinku**: ime za prijavu zadani je administrator.
    - **SSH korisničko ime i lozinku**.
    
    Zapišite ove vrijednosti.  Morate ih kasnije u ovom praktičnom vodiču.

    > [AZURE.NOTE] SSH koristi se za daljinski pristup klaster HDInsight pomoću programa naredbenog retka. Korisničko ime i lozinku koje koristite se koristi pri povezivanju s klaster kroz SSH. Osim toga, SSH korisničko ime mora biti jedinstvena, kao što je stvara korisnički račun na sve čvorove klaster HDInsight. Sljedeće su neke od naziva računa rezervirana za usluge na klaster, a ne može koristiti kao SSH korisničko ime:
    >
    > Korijenska, hdiuser, oluja, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, grozd, oozie, falcon, sqoop, administrator, tez, hcat, hdinsight zookeeper.

    > Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u nekom od sljedećih članaka:

    > * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3. kliknite **u redu** da biste spremili parametre.

4. iz plohu **implementacije Prilagođeno** kliknite **grupu resursa** padajući okvir, a zatim **Novo** da biste stvorili novu grupu resursa. Grupa resursa je spremnik koje grupira klaster, račun zavisne prostora za pohranu i ostale povezane resursa.

5. kliknite **pravne uvjete**, a zatim kliknite **Stvori**.

6. kliknite **Stvori**. Prikazat će se novi pločicu pod naslovom Submitting uvođenje predloška implementacije. Potrebno je oko oko 20 minuta da biste stvorili klaster i SQL baze podataka.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Pokretanje Spark SQL upita pomoću Jupyter bilježnice

U ovom odjeljku koristite Jupyter bilježnicu da biste pokrenuli Spark SQL upita na temelju klaster Spark. HDInsight Spark klastere sadrže dva jezgre koje možete koristiti s bilježnicom Jupyter. To su:

* **PySpark** (za aplikacije pisane Python)
* **Spark** (za aplikacije pisane Scala)

U ovom se članku će koristiti otklanjanje PySpark. U članku [dostupne na Jupyter bilježnica s Spark HDInsight klastere jezgre](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) može čitati u pojedinosti o prednosti korištenja otklanjanje PySpark. Međutim, nekoliko ključne prednosti korištenja otklanjanje PySpark su:

* Ne morate postaviti na konteksta za Spark i grozd. Te se automatski postavljaju umjesto vas.
* Koristite magics ćelije, kao što su `%%sql`, izravno pokretanje upite SQL ili grozd, bez sve prethodne koda.
* Izlaz upita SQL ili grozd automatski vizualizacije.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Stvaranje bilježnice Jupyter s otklanjanje PySpark 

1. S [Portala za Azure](https://portal.azure.com/), iz startboard, kliknite pločicu za svoj klaster Spark (Ako ste ga prikvačiti na startboard). Možete se kretati svoj klaster u odjeljku **Pregledaj sve** > **Klastere HDInsight**.   

2. Iz klaster plohu Spark kliknite **Nadzorna ploča klaster**, a zatim **Jupyter bilježnice**. Ako se to od vas zatraži, unesite administratorskih vjerodajnica za klaster.

    > [AZURE.NOTE] Možda dosegne i Jupyter bilježnicu za svoj klaster tako da otvorite sljedeći URL u pregledniku. Zamijenite __CLUSTERNAME__ pod nazivom svoj klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Stvaranje nove bilježnice. Kliknite **Novo**, a zatim kliknite **PySpark**.

    ![Stvaranje nove bilježnice Jupyter] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Stvaranje nove bilježnice Jupyter")

3. Nova bilježnica se stvara i otvara pod nazivom Untitled.pynb. Kliknite naziv bilježnice na vrhu, a zatim upišite neslužbeni naziv.

    ![Osiguraj naziv bilježnice] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Osiguraj naziv bilježnice")

4. Budući da ste stvorili pomoću otklanjanje PySpark bilježnice, ne morate izričito stvaranje sve konteksta. Konteksta Spark i grozd će se automatski stvara za vas kada pokrenete prvu ćeliju kod. Možete započeti tako da uvezete vrste potrebne za taj scenarij. Da biste to učinili, zalijepite sljedeći isječak koda u ćeliji, a zatim pritisnite **SHIFT + ENTER**.

        from pyspark.sql.types import *
        
    Svaki put kada pokrenete posao u Jupyter, naslov prozora preglednika web prikazivat će se status **(zauzet)** uz naziv bilježnice. Prikazat će se i Ispunjeni krug pokraj teksta **PySpark** u gornjem desnom kutu. Po dovršetku posao to promijeniti će prazni kruga.

     ![Status posla Jupyter bilježnice] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status posla Jupyter bilježnice")

4. Učitavanje ogledne podatke u privremenu tablicu. Kada stvorite Spark klaster u HDInsight, oglednu datoteku podataka **hvac.csv**se kopira račun povezan prostora za pohranu u odjeljku **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    U praznu ćeliju, zalijepite u sljedećem primjeru kod, a zatim pritisnite **SHIFT + ENTER**. U ovom se primjeru kod registrira podatke u privremenu tablicu pod nazivom **hvac**.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Budući da koristite PySpark otklanjanje, možete odmah izravno pokrenuti SQL upita na privremena **hvac** koju ste upravo stvorili pomoću na `%%sql` poseban. Dodatne informacije o na `%%sql` poseban, kao i druge magics dostupno u sklopu otklanjanje PySpark potražite u članku [jezgre dostupne na Jupyter bilježnica s klastere Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Kada posao je uspješno dovršena, po zadanom prikazuju sljedeće tablični izlaz.

    ![Izlaz tablice rezultata upita] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Izlaz tablice rezultata upita")

    Možete pogledati i rezultatima u drugim vizualizacijama. Na primjer, u područje grafikon u rezultatu isti izgledati sljedeće.

    ![Površinski grafikon rezultata upita] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Površinski grafikon rezultata upita")


6. Nakon što dovršite pokrenuti aplikaciju, trebali biste zatvaranja bilježnice da biste predali resurse. Da biste to učinili, na izborniku **datoteka** bilježnicu, kliknite **Zatvori i zaustavili**. Ovaj će zatvaranja i Zatvori bilježnicu.

##<a name="delete-the-cluster"></a>Brisanje klaster

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Vidi također


* [Pregled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariji

* [Spark bi: izvođenje analiza interaktivnih podataka pomoću Spark u HDInsight s alatima za Poslovno obavještavanje](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s strojnog učenja: korištenje Spark u HDInsight za analizu sastavnih temperatura pomoću HVAC podataka](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s strojnog učenja: korištenje Spark u HDInsight za predviđanje rezultata provjere za hranu](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark strujeće: Korištenje Spark u HDInsight za izgradnju u stvarnom vremenu strujanje aplikacije](hdinsight-apache-spark-eventhub-streaming.md)

* [Web-mjesto zapisnika analize pomoću Spark u HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Aplikacija uvid telemetrijskih analiza podataka pomoću Spark u HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

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

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
