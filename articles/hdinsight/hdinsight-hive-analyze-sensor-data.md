<properties
    pageTitle="Analiziranje podataka senzor pomoću grozd i Hadoop | Microsoft Azure"
    description="Saznajte kako analize senzor podataka pomoću upita konzole vrste Hive HDInsight (Hadoop), a zatim vizualni prikaz podataka u programu Microsoft Excel s PowerView."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Analiza podataka senzor korištenju konzole za vrste Hive upita na Hadoop u HDInsight

Saznajte kako analiza senzor podataka pomoću upita konzole vrste Hive HDInsight (Hadoop), a zatim vizualizacija podataka u programu Microsoft Excel pomoću značajke Power View.

> [AZURE.NOTE] Koraci u ovom dokumentu samo funkcioniraju za klastere HDInsight utemeljen na sustavu Windows.

U ovom primjeru, koristit ćete grozd postupak povijesne podatke koje je stvorio zagrijavanje, ventilacije i sustavi uređaja (HVAC) da biste odredili sustavima koji ne mogu kvalitetno održavanje skup temperatura. Saznat ćete kako:

- Stvorite tablice GROZD upita podataka pohranjenih u datoteke razdvojene zarezom (CSV).
- Stvaranje upita GROZD za analizu podataka.
- Microsoft Excel koristi za povezivanje s HDInsight (pomoću Povezivost otvorene baze podataka (ODBC) za dohvaćanje analiziranom podataka.
- Vizualni prikaz podataka pomoću značajke Power View.

![Dijagram arhitekturu rješenja](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Preduvjeti

* Klaster programa HDInsight (Hadoop): informacije o stvaranju klaster potražite u članku [Dodjeljivanje Hadoop klastere u HDInsight](hdinsight-provision-clusters.md) .

* Microsoft Excel 2013

    > [AZURE.NOTE] Microsoft Excel koristi se za vizualizaciju podataka sa [Značajkom Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft grozd ODBC upravljački program](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Da biste pokrenuli uzorka

1. Web-pregledniku otvorite sljedeći URL. Zamjena `<clustername>` pod nazivom svoj klaster HDInsight.

        https://<clustername>.azurehdinsight.net

    Kada se to od vas zatraži, autentičnost pomoću administrator korisničko ime i lozinka koje koristite prilikom dodjele resursa u ovoj grupi.

2. S web-stranice kliknite karticu **Galerija početak rada** i kategoriji **rješenja s oglednim podacima** , kliknite **Analiza podataka senzor** uzorka.

    ![Početak rada galerije](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Slijedite upute na web-stranicu da biste dovršili uzorka.
