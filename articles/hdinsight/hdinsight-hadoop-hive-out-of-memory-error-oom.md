<properties
    pageTitle="Iz memorije pogreške (nema dovoljno slobodne Memorije) – postavke grozd | Microsoft Azure"
    description="Ispravite pogrešku memorije (nema dovoljno slobodne Memorije) iz grozd upita o odsutnosti u Hadoop u HDInsight. Scenarij kupca je upit preko mnogo velikih tablica."
    keywords="iz memorije pogreške, nema dovoljno slobodne Memorije, grozd postavke"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Rješavanje pogreške o memorije (nema dovoljno slobodne Memorije) s postavkama memorije grozd u Hadoop u Azure HDInsight

Neke uobičajene probleme naših korisnika nominalne će sve dobiti pogrešku o memorije (nema dovoljno slobodne Memorije) prilikom korištenja vrste Hive. U ovom se članku opisuje scenarij klijenta i postavke grozd preporučujemo da biste riješili problem.

## <a name="scenario-hive-query-across-large-tables"></a>Scenarij: Grozd upita preko velike tablice

Klijent pokrenuli upit ispod pomoću grozd.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Neke nuances ovaj upit:

* T1 je pseudonim u širu tablicu Tablica1, koja ima mnogo vrste NIZ stupaca.
* Druge tablice koje nisu veliki ali velik broj stupaca.
* Sve tablice se pridružuju drugoga, u nekim slučajevima s više stupaca u parametru TABLE1 i drugim korisnicima.

Kada je korisnik odabrao pokrenuli upit koji se koristi grozd na MapReduce na 24 čvor klaster A3, upita pokrenuli 26 minuta. Klijent primijetili sljedeće poruke upozorenja prilikom pokretanja upita pomoću grozd na MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Budući da se upit završi izvođenje 26 minuta, klijenta zanemaruje te upozorenja i umjesto pokrenuli usredotočili na poboljšanju ovog dodatno performanse upita.

Klijent konzultirali [optimiziranje vrste Hive upite za Hadoop u HDInsight](hdinsight-hadoop-optimize-hive-query.md)i odlučili koristiti modul Tez izvršavanja. Kada istog upita pokretala je omogućena postavka Tez upita pokrenuli 15 minuta, a izbacio sljedeća pogreška:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Klijent zatim odlučili koristiti s većim VM (odnosno D12) mislim veći VM imati više skupova prostora. Čak i nakon toga kupca Nastavi da biste vidjeli pogrešku. Klijent Pristigla timu za HDInsight zatražite pomoć za ispravljanje pogrešaka taj problem.

## <a name="debug-the-out-of-memory-oom-error"></a>Ispravljanje pogrešaka pogreške o memorije (nema dovoljno slobodne Memorije)

Naš podršku i inženjerskim timovima zajedno pronašla je jedan od problema koji uzrokuje pogrešku o memorije (nema dovoljno slobodne Memorije) je na [Poznati problem što je opisano u Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306). Iz opis u JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Potvrdili smo **hive.auto.convert.join.noconditionaltask** uistinu postavljen na **true** tako da potražite u odjeljku grozd site.xml datoteka:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Ovisno o upozorenje i na JIRA, naš pretpostavke je uključiti karte je uzrok pogreške Java skupova prostor nema dovoljno slobodne Memorije. Tako ćemo dug dublju u taj problem.

Prema uputama iz članka za blog [Hadoop Yarn postavki memorije u HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), kada se koristi Tez izvođenja modul je prostor koji zauzimaju skupova pripada spremnik Tez. Pogledajte sliku ispod s opisom spremnik memorije Tez.

![Tez spremnik memorije dijagram: vrste Hive iz memorije pogreške nema dovoljno slobodne Memorije](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Definiranje kao Predloži bloga sljedeće postavke dva memorije memorije spremnik za na skupova: **hive.tez.container.size** i **hive.tez.java.opts**. Iz naših sučelje iznimku nema dovoljno slobodne Memorije znači veličinu kontejnera je premalen. To znači da je presitan skupova veličinu Java (hive.tez.java.opts). Da bi se svaki put kada se prikaže nema dovoljno slobodne Memorije, pokušajte da biste povećali **hive.tez.java.opts**. Ako je potrebno možda ćete morati povećati **hive.tez.container.size**. Postavka **java.opts** mora biti oko 80% **container.size**.

> [AZURE.NOTE]  Postavka **hive.tez.java.opts** uvijek mora biti manja od **hive.tez.container.size**.

Budući da stroj D12 ima 28GB memorije, ne možemo odlučili veličinu kontejnera od 10GB (10240MB), a zatim dodijelite 80% java.opts. To je učinjeno na konzoli grozd koristeći postavku u nastavku:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Ovisno o tim postavkama, upit uspješno je u odjeljku deset minuta.

## <a name="conclusion-oom-errors-and-container-size"></a>Zaključak: Nema dovoljno slobodne Memorije pogrešaka i veličinu kontejnera

Prikazivanja pogreške nema dovoljno slobodne Memorije ne moraju nužno biti znači da je veličina spremnik presitan. Umjesto toga, trebali biste konfigurirati postavke memorije da bi veličinu skupova povećava se i je barem 80% veličina memorije kontejner.
