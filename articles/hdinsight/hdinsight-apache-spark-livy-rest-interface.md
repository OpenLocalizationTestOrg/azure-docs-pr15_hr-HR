<properties
    pageTitle="Slanje Spark poslove daljinski pomoću Livije | Microsoft Azure"
    description="Saznajte kako koristiti Livije s HDInsight klastere daljinski slanje Spark zadatke."
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
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Slanje zadataka Spark daljinski programa Apache Spark klaster na HDInsight Linux pomoću Livije

Klaster Apache Spark na Azure HDInsight obuhvaća Livije, OSTALE sučelje za slanje poslova daljinski Spark klaster. Detaljne dokumentaciju potražite u članku [Livije](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Možete koristiti Livije pokrenuti interaktivne Spark ljuske ili slanje obrade će se pokrenuti na Spark. U ovom se članku govori o korištenju Livije slanje obrade. Vidjet ćete da sintaksa ispod koristi zakretanja za OSTALE pozive krajnjoj Livije.

**Preduvjeti:**

Morate imati sljedeće:

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Klaster Apache Spark na HDInsight Linux. Upute potražite u članku [Stvaranje Spark Apache klastere u Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Slanje na obradu klaster

Prije nego što pošaljete obrade, morate prenijeti posudu aplikacije na prostora za pohranu klaster koji je povezan s klaster. [**AzCopy**](../storage/storage-use-azcopy.md), uslužni program naredbenog retka, možete koristiti da biste to učinili. Postoji mnogo drugim klijentima možete koristiti za prijenos podataka. Možete pronaći dodatne informacije o ih na [prijenos podataka za Hadoop poslove u HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Primjeri**:

* Ako je datoteka posudu na klaster prostora za pohranu (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Ako na koju želite proslijediti filename posudu i na NazivKlase kao dio unosa datoteke (u ovom primjeru input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Dohvaćanje informacija o serije sustavom klaster

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Primjeri**:

* Ako želite dohvatiti sve serije sustavom skupine:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Ako želite dohvatiti određene grupe s određenom batchId

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Brisanje obrade

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Primjer**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livije i visoke dostupnosti

Livije nudi visoku dostupnost Spark zadataka koji se izvode na klaster. Evo nekoliko primjera.

* Ako servis Livije funkcionira nakon što ste poslali zadatak za daljinsko Spark klaster, posao nastavlja se izvoditi u pozadini. Kada je Livije sigurnosnu kopiju, vratit će se status posla i izvješća koji se ponovno.

* Jupyter bilježnice za HDInsight pokreće se Livije u pozadinski. Ako bilježnica se izvodi posao Spark i dobiti ponovno pokrenuti servis Livije, bilježnica će i dalje da biste pokrenuli kod ćelije. 

## <a name="show-me-an-example"></a>Pokaži mi primjer

U ovom ćete odjeljku smo pogledajte primjere o korištenju Livije za slanje Spark aplikacije, praćenje napretka aplikacije, a zatim izbrišite posao. Aplikacija se koristi u ovom primjeru je nešto razvijene u članku [stvaranje samostalne Scala aplikacije i pokrenuti klaster HDInsight Spark](hdinsight-apache-spark-create-standalone-application.md). Koraci u nastavku pretpostavimo da sljedeće:

* Već kopirati posudu računala s računom za pohranu pridružene klaster.
* Imate zakretanja instaliran na računalu na kojem pokušavate ove korake.

Izvršite sljedeće korake.

1. Javite nam najprije morati potvrditi da na skupine nije ostalo Livije. Ne možemo možete učiniti tako da početak popis izvodi serije. Ako je ovo prvi put pokrećete posao koristeći Livije, to mora vratiti nula.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Za izlazni moraju se sličnu ovoj:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Obratite pozornost na to kako posljednjeg retka u izlaz piše **Ukupni zbroj: 0**, čime se predlaže ne izvode serije.

2. Javite nam sada poslati obrade. Isječak u nastavku koristi i Ulazna datoteka (input.txt) za prosljeđivanje posudu ime i naziv klase kao parametri. To je preporučeni način ako koristite ove korake na računalu sa sustavom Windows.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Parametara u datoteci **input.txt** se definira na sljedeći način:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Trebali biste vidjeti na Izlaz sličnu ovoj:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Obratite pozornost na to kako se piše zadnji redak izlaz **Stanje: pokretanje**. Također piše, **id: 0**. To je ID grupe

3. Sada možete dohvatiti na stanje ove određene obrade pomoću ID grupe

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Trebali biste vidjeti na Izlaz sličnu ovoj:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Izlaz sada prikazuje **Stanje: uspjeh**, kojom se predlaže zadatak uspješno je dovršena.

4. Ako želite, možete izbrisati grupu.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Trebali biste vidjeti na Izlaz sličnu ovoj:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Zadnji redak izlaz prikazuje skupine uspješno je izbrisana. Ako izbrišete posao dok je pokrenut, zapravo Ukloni posao. Ako izbrišete zadatak koji je dovršen, uspješno ili u suprotnom briše informacije o zadatku u potpunosti.

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
