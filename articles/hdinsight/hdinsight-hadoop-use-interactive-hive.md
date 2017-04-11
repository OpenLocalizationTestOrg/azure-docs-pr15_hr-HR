<properties
    pageTitle="Korištenje interaktivnog grozd u HDInsight | Microsoft Azure"
    description="Saznajte kako pomoću interaktivnih vrste Hive (grozd na LLAP) u HDInsight."
    keywords=""
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Korištenje interaktivnog grozd u HDInsight (pretpregled)

Interaktivni vrste Hive (poznatog [Postupak i uživo dugo]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) je li novi HDInsight [klaster vrsta]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktivni grozd dopušta u memorije predmemoriranje koji olakšava grozd upita znatno interaktivni i brže. Nova značajka čini HDInsight od na svijetu Većina performant fleksibilna, i otvaranje rješenje velikih skupova podataka na oblaku s u memoriji predmemorira (pomoću grozd i Spark) i napredne analize do niže integraciju s komponentom servisa R. 

Interaktivni vrste Hive klaster razlikuje se od Hadoop klaster. Sadrži samo grozd servisa. 

> [AZURE.NOTE] MapReduce, Svinja, Sqoop, Oozie i ostale servise uklanjaju se iz ove vrste klaster uskoro.
Servis grozd klasteru interaktivne vrste Hive je dostupan samo putem prikaza Ambari grozd, Beeline i vrste Hive ODBC. Nije moguće pristupiti putem konzole grozd, Templeton, EŽA Azure i Azure PowerShell. 


 


## <a name="create-an-interactive-hive-cluster"></a>Stvaranje klaster za interaktivno vrste Hive

Interaktivni grozd klaster podržana samo za klastere sustavom Linux. Informacije o stvaranju klastere HDInsight potražite u članku [Stvaranje Linux sustavom Hadoop klastere u HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Izvršavanje grozd iz interaktivne grozd

Postoje različite mogućnosti kako izvršiti grozd upita:

- Pokretanje grozd prikazu Ambari grozd

    Informacije o korištenju prikaza vrste Hive, potražite u članku [korištenje prikaza vrste Hive s Hadoop u HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Pokretanje grozd pomoću Beeline

    Informacije o korištenju Beeline na HDInsight potražite u članku [Korištenje vrste Hive s Hadoop u HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).

    Koristite Beeline na headnode ili je prazan rub čvor.  Preporučuje se korištenje Beeline iz programa čvor prazan rub.  Informacije o stvaranju programa HDInsight klaster s prazan edgenode, potražite u članku [Korištenje prazan rub čvorove u HDInsight](hdinsight-apps-use-edge-node.md).

- Pokretanje grozd pomoću vrste Hive ODBC

    Informacije o korištenju ODBC vrste Hive, potražite u članku [Povezivanje programa Excel sa Hadoop s Microsoft vrste Hive ODBC upravljački program](hdinsight-connect-excel-hive-odbc-driver.md).

**Da biste pronašli JDBC niz za povezivanje:**

1.  Prijavite se Ambari pomoću sljedećih URL: https://<ClusterName>. AzureHDInsight.net.
2.  Kliknite **vrste Hive** na lijevom izborniku.
3.  Kliknite ikonu istaknuti da biste kopirali URL:

    ![HDInsight Hadoop interaktivne grozd LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Vidi također
-   [Stvaranje Linux sustavom Hadoop klastere u HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Saznajte kako stvoriti klastere interaktivne vrste Hive u HDInsight.
-   [Korištenje vrste Hive s Hadoop u HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md): Saznajte kako koristiti Beeline slanje grozd upita.
-   [Povezivanje programa Excel sa Hadoop s Microsoft vrste Hive ODBC upravljački program](hdinsight-connect-excel-hive-odbc-driver.md): Saznajte kako se povezati Excel grozd.
