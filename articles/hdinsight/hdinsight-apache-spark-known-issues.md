<properties 
    pageTitle="Poznati problemi Apache Spark u HDInsight | Microsoft Azure" 
    description="Poznati problemi Apache Spark u HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Poznati problemi prilikom Apache Spark klaster na HDInsight Linux

Ovaj dokument evidentira svim poznatim problemima za pretpregled javno HDInsight Spark.  

##<a name="livy-leaks-interactive-session"></a>Livije curenje interaktivne sesiju
 
Kada Livije ponovnog pokretanja s interaktivne sesiju (iz Ambari ili zbog ponovnog pokretanja virtualnog računala headnode 0) i dalje aktivnosti sesiju interaktivne posao će leaked. Zbog toga novih zadataka možete zamrzne u stanju prihvaćena, a ne može se pokrenuti.

**Ublažiti:**

Koristite sljedeći postupak da biste zaobilazno rješenje problem:

1. Ssh u headnode. 
2. Pokrenite sljedeću naredbu da biste pronašli ID-ove aplikacije interaktivne poslove rada kroz Livije. 

        yarn application –list

    Zadani posla imena bit će navedeni Livije ako su poslove pokrenuti sa sesijom interaktivne Livije bez izričitog naziva, sesije za Livije započeo Jupyter bilježnice, naziv zadatka započinje s remotesparkmagics_ *. 

3. Pokrenite sljedeću naredbu uklanjanje ti Poslovi. 

        yarn application –kill <Application ID>

Započet će novih zadataka izvodi. 

##<a name="spark-history-server-not-started"></a>Spark povijest poslužitelja nije pokrenuto 

Spark povijest Server ne pokreće automatski nakon stvaranja klaster.  

**Ublažiti:** 

Ručno pokretanje poslužitelja povijest iz Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Problem dozvola u direktoriju Spark zapisnika 

Kad hdiuser pošalje zadatak s spark-submit, postoji programa java.io.FileNotFoundException pogreške: /var/log/spark/sparkdriver_hdiuser.log (dozvola zabranjen) i zapisnika upravljački program je napisan. 

**Ublažiti:**
 
1. Dodavanje hdiuser Hadoop grupu. 
2. Navedite 777 dozvole na /var/log/spark nakon stvaranja klaster. 
3. Ažurirajte mjesto zapisnika spark pomoću Ambari biti direktorij s dozvolama za 777.  
4. Pokreni spark – Pošalji kao sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Problema vezanih uz Jupyter bilježnica

Slijede neki poznatih problema vezanih uz Jupyter bilježnice.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Bilježnica u nazivi datoteka sadrži znakove koji nisu ASCII

Jupyter bilježnice koje se mogu koristiti u Spark HDInsight klastere treba ne sadrži znakove koji nisu ASCII nazivi datoteka. Ako pokušavate prenijeti datoteke putem korisničkog Sučelja Jupyter koja ima naziv koji ASCII, on neće uspjeti tihu (to jest, Jupyter neće vam prijenos datoteke, ali ga neće vratiti vidljivi pogreške ili). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Pogreška prilikom učitavanja bilježnica veće

Možda će se prikazati pogreška **`Error loading notebook`** prilikom učitavanja bilježnice koje su veće veličine.  

**Ublažiti:**

Ako vam se ova pogreška, ona ne znači podataka oštećena je ili izgubiti.  Bilježnice se i dalje na disku u `/var/lib/jupyter`, pa ćete moći SSH u klaster im pristupiti. Bilježnice možete kopirati iz svoj klaster na lokalno računalo (pomoću Pronađenim ili WinSCP) kao sigurnosnu kopiju da biste spriječili gubitak važne podatke u bilježnici. Možete zatim SSH tunelom u vašem headnode pri priključak 8001 pristup Jupyter bez prolaze kroz pristupnika.  Iz nje, poništite izlaz bilježnice pa ponovno spremite ga da biste minimizirali veličinu bilježnice.

Da biste spriječili da ta se pogreška događa u budućnosti, morate slijediti najbolje postupke:

* Važno da biste zadržali veličinu bilježnice small je. Bilo kakav izlaz iz Spark poslova koji se šalju natrag Jupyter je ista i u bilježnici.  Je najbolji način s Jupyter Općenito da biste izbjegli pokretanje `.collect()` na velike RDD korisnika ili dataframes; Umjesto toga, ako želite pogled na sadržaj programa RDD, razmislite o pokretanje `.take()` ili `.sample()` da bi izlaz ne pristupiti prevelik.
* Osim toga, kada spremate bilježnice, očistite sve izlazne ćelija da biste smanjili veličinu.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Početna pokretanje bilježnice traje dulje nego je očekivano 

Prva naredba kod u bilježnici Jupyter pomoću Spark poseban može potrajati više od jedne minute.  

**Objašnjenje:**
 
To se događa jer kada se izvršava na prvu ćeliju kod. U pozadini to pokreće konfiguracije sesije i Spark, a zatim SQL pa se postavljaju grozd konteksta. Nakon tih konteksta postavljene, prva naredba se pokreće, a to daje dojam koji iskaz traje predugo da biste dovršili.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Prekoračenje vremena Jupyter bilježnicu u stvaranju sesiju

Kada je Spark klaster nema dovoljno resursa, Spark i Pyspark jezgre u bilježnici Jupyter će Prekoračenje vremena stvaranja sesiju. 

**Mitigations:** 

1. Oslobodite nekih resursa u svoj klaster Spark po:

    - Zaustavljanje drugih Spark bilježnice tako da odete na izborniku Zatvori i zastoj ili kliknete zatvaranja u programu explorer bilježnice.
    - Zaustavljanje drugim aplikacijama Spark iz YARN.

2. Ponovno pokrenite bilježnicu u kojoj ste pokušali pokretanja. Dovoljno resursa mora biti dostupan za stvaranje sesije sada.

##<a name="see-also"></a>Vidi također

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

* [Na računalo instalirati Jupyter i povezati se HDInsight Spark klaster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Upravljanje resursima

* [Upravljanje resursima za klaster Apache Spark u Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Praćenje i ispravljanje pogrešaka zadataka izvodi na programa klaster Apache Spark u HDInsight](hdinsight-apache-spark-job-debugging.md)
