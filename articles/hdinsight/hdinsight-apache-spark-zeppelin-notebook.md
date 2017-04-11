<properties 
    pageTitle="Koristite Zeppelin bilježnice s Spark klaster na HDInsight Linux | Microsoft Azure" 
    description="Detaljne upute za korištenje Zeppelin bilježnica s klastere Spark na HDInsight Linux." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Koristite Zeppelin bilježnice s Apache Spark klaster na HDInsight Linux

HDInsight Spark klastere obuhvaćaju Zeppelin bilježnice koje možete koristiti da biste pokrenuli Spark zadatke. U ovom članku, Saznajte kako koristiti bilježnicu Zeppelin na programa klaster HDInsight.


**Preduvjeti:**

* Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Programa Apache Spark klaster. Upute potražite u članku [Stvaranje Spark Apache klastere u Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Pokretanje bilježnice za Zeppelin

1. Iz klaster plohu Spark kliknite **Nadzorna ploča klaster**, a zatim **Zeppelin bilježnice**. Ako se to od vas zatraži, unesite administratorskih vjerodajnica za klaster.

    > [AZURE.NOTE] Možda dosegne i Zeppelin bilježnicu za svoj klaster tako da otvorite sljedeći URL u pregledniku. Zamijenite __CLUSTERNAME__ pod nazivom svoj klaster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Stvaranje nove bilježnice. U oknu zaglavlje, kliknite **bilježnicu**, a zatim **Stvoriti novu bilješku**.

    ![Stvaranje nove bilježnice Zeppelin] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Stvaranje nove bilježnice Zeppelin")

    Unesite naziv bilježnice, a zatim kliknite **Stvaranje bilješke**.

3. Osim toga, provjerite je li zaglavlje bilježnice prikazuje povezanog stanje. Označena je tako da zelena točka u gornjem desnom kutu.

    ![Status Zeppelin bilježnice] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Status Zeppelin bilježnice")

4. Učitavanje ogledne podatke u privremenu tablicu. Kada stvorite Spark klaster u HDInsight, oglednu datoteku podataka **hvac.csv**se kopira račun povezan prostora za pohranu u odjeljku **\HdiSamples\SensorSampleData\hvac**.

    U prazan odlomak koji je stvoren po zadanom u novu bilježnicu, zalijepite sljedeći isječak.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Pritisnite **SHIFT + ENTER** ili kliknite gumb **Reproduciraj** za odlomak da biste pokrenuli u isječak. Status u desnom kutu odlomka trebali biste tijek iz SPREMNA na ČEKANJU IZVODI u DOVRŠENO. Rezultat prikazuje pri dnu isti odlomak. Snimku zaslona izgleda ovako:

    ![Stvaranje privremene tablice sa sirovim podacima] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Stvaranje privremene tablice sa sirovim podacima")

    Možete unijeti i naslova svakog odlomka. Iz desnom kutu kliknite ikonu **Postavke** , a zatim kliknite **Prikaži naslov**.

5. Sada možete pokrenuti Spark SQL naredbe u tablici **hvac** . Zalijepite sljedeći upit novog odlomka. Upit dohvaća sastavnih ID-a i razlika između cilj i stvarne temperature pri svakom na navedeni datum. Pritisnite **SHIFT + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    **% Sql** naredbe na početku govori bilježnicu koju ćete koristiti tumačenja Livije Scala.

    Sljedeća slika prikazuje izlaz.

    ![Pokrenite Spark SQL naredbi pomoću bilježnice] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Pokrenite Spark SQL naredbi pomoću bilježnice")

     Kliknite Mogućnosti prikaza (istaknute pravokutnik) da biste se prebacivali između različitih nikakva jamstva za istu izlaz. Kliknite **Postavke** za odabir koje consitutes ključ i vrijednosti u izlaz. Snimke zaslona iznad **buildingID** kao koristi tipku i prosjek **temp_diff** kao vrijednost.

    
6. Možete i pokrenuti Spark SQL naredbe pomoću varijabli u upitu. Sljedeći isječak pokazuje kako da biste definirali varijable **Temp**u upit s mogućim vrijednostima koje želite poslati upit s. Kada prvi put pokrenete upit, padajući automatski je popunjen vrijednosti koju ste naveli za varijablu.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Zalijepite ovu isječak novog odlomka, a zatim pritisnite **SHIFT + ENTER**. Sljedeća slika prikazuje izlaz.

    ![Pokrenite Spark SQL naredbi pomoću bilježnice] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Pokrenite Spark SQL naredbi pomoću bilježnice")

    Za sljedeće upite, možete odabrati novu vrijednost s padajućeg popisa i ponovno pokrenite upit. Kliknite **Postavke** za odabir koje consitutes ključ i vrijednosti u izlaz. Snimke zaslona iznad koristi **buildingID** kao ključ, prosjek **temp_diff** kao vrijednost i **targettemp** kao grupu.

7. Ponovno pokrenite tumačenja Livije da biste izašli iz aplikacije. Da biste to učinili, otvorite postavke tumačenja tako da kliknete na prijavljenih korisničko ime u gornjem desnom kutu, a zatim **tumačenja**.

    ![Pokretanje tumačenja] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Vrste Hive izlaz")

2. Pomaknite se do Livije tumačenja postavke, a zatim **ponovno pokrenite**.

    ![Ponovno pokrenite intepreter Livije] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Ponovno pokrenite Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Kako koristiti vanjski pakete s bilježnicu?

Da biste koristili vanjske, zajednice pridonio paketa koji nisu uvršteni u novom alatu-tvorničke u klasteru možete konfigurirati Zeppelin bilježnice u klaster Apache Spark na HDInsight (Linux). Možete pretraživati [spremište Maven](http://search.maven.org/) za popis svih paketa koji su dostupni. Popis dostupnih paketa možete dobiti i iz drugih izvora. Na primjer, popis svih paketa zajednice pridonio dostupna je na [Spark paketa](http://spark-packages.org/).

U ovom članku, vidjet ćete kako pomoću značajke pakiranja [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) s bilježnicom Jupyter.

1. Otvorite postavke tumačenja. U gornjem desnom kutu kliknite na prijavljenih korisničko ime, a zatim **tumačenja**.

    ![Pokretanje tumačenja] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Vrste Hive izlaz")

2. Pomaknite se do Livije tumačenja postavke, a zatim kliknite **Uređivanje**.

    ![Promjena postavki tumačenja] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Promjena postavki tumačenja")

3. Dodavanje novog ključa koja se naziva **livy.spark.jars.packages** i postavljanje njegovom vrijednošću u obliku `group:id:version`. Tako, ako želite pomoću značajke pakiranja [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) , morate postaviti vrijednost ključa za `com.databricks:spark-csv_2.10:1.4.0`.

    ![Promjena postavki tumačenja] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Promjena postavki tumačenja")

    Kliknite **Spremi** , a zatim ponovno pokrenite tumačenja Livije.

4. **Savjet**: želite li znati kako dolaze unesena vrijednost ključa iznad, Evo kako.

    na. Pronađite paketa u spremištu Maven. Za ovaj vodič koristi [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. U spremištu prikupite vrijednosti za **GroupId**, **ArtifactId**i **verziju**.

    ![Korištenje vanjskih paketa s Jupyter bilježnice] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Korištenje vanjskih paketa s Jupyter bilježnice")

    c. CONCATENATE tri vrijednosti odvojene zarezom (****:).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Gdje se spremaju Zeppelin bilježnice?

Bilježnica Zeppelin spremaju headnodes klaster. Da ako izbrišete klaster, bilježnica će se kao i izbrisati. Ako želite da biste sačuvali bilježnice za kasnije korištenje na druge klastere, morate ih izvesti nakon dovršetka izvodi poslove. Da biste izvezli bilježnicu, kliknite ikonu **izvesti** kao što je prikazano na slici u nastavku.

![Preuzimanje bilježnice] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Preuzimanje bilježnice")

Time se bilježnicu kao datoteku JSON u lokaciju za preuzimanje.

## <a name="livy-session-management"></a>Upravljanje sesijama Livije

Kada pokrenete prvog odlomka kod u bilježnici Zeppelin, novu sesiju Livije stvara se u svoj klaster HDInsight Spark. U svim bilježnicama Zeppelin naknadno stvorite se zajednički koristi ovu sesiju. Ako zbog nekog razloga Livije sesija drugom procesu (klaster ponovno pokrenite računalo, itd.), nećete moći pokrenuti poslovi iz bilježnice Zeppelin.

U tom slučaju morate poduzeti sljedeće korake prije no što počnete izvođenje zadataka iz Zeppelin bilježnice. 

1. Ponovno pokrenite tumačenja Livije iz bilježnice Zeppelin. Da biste to učinili, otvorite postavke tumačenja tako da kliknete na prijavljenih korisničko ime u gornjem desnom kutu, a zatim **tumačenja**.

    ![Pokretanje tumačenja] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Vrste Hive izlaz")

2. Pomaknite se do Livije tumačenja postavke, a zatim **ponovno pokrenite**.

    ![Ponovno pokrenite intepreter Livije] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Ponovno pokrenite Zeppelin intepreter")

3. Pokrenite kod ćeliju iz postojeće bilježnice Zeppelin. Ovako možete stvoriti novu sesiju Livije klasteru HDInsight.

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







