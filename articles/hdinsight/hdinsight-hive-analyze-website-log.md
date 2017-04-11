<properties 
    pageTitle="Korištenje grozd uz Hadoop za analizu zapisnika web-mjesta | Microsoft Azure" 
    description="Saznajte kako koristiti grozd s HDInsight da biste analizirali zapisnika web-mjesta. Ćete koristiti datoteke zapisnika kao unos u tablicu programa HDInsight, a pomoću HiveQL poslati upit podatke." 
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
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Korištenje grozd s HDInsight da biste analizirali zapisnika s web-mjesta

Saznajte kako pomoću servisa HDInsight HiveQL da biste analizirali zapisnika s web-mjesta. Analiza zapisnik web-mjesta mogu se fazi publici na temelju slične aktivnosti, kategoriziranje posjetitelji web-mjesta tako da demografskih podataka, a da biste saznali sadržaj su prikaz, mogu potjecati iz web-mjesta i tako dalje.

U ovom primjeru programa HDInsight klaster će koristiti da biste analizirali web-mjesta datoteke zapisnika da biste dobili uvid u učestalost posjeta web-mjestu s vanjskih web-mjesta u dana. Ćete stvoriti i sažetak pogreške web-mjesto koje korisnici će se pojaviti. Saznat ćete kako:

- Povezivanje sa spremištem blobova platforme Azure, koja sadrži web-mjesta datoteke zapisnika.
- Stvaranje tablica GROZD upita te zapisnika.
- Stvaranje upita GROZD za analizu podataka.
- Koristiti Microsoft Excel da biste se povezali sa servisom HDInsight (pomoću Povezivost otvorene baze podataka (ODBC) za dohvaćanje analiziranom podataka.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Preduvjeti

- Potrebno je dodjeli Hadoop klaster na Azure HDInsight. Upute potražite u članku [Dodjeljivanje HDInsight klastere][hdinsight-provision]. 
- Morate imati Microsoft Excel 2013 ili instalirani Excel 2010.
- Morate imati [Microsoft vrste Hive ODBC upravljački program](http://www.microsoft.com/download/details.aspx?id=40886) za uvoz podataka iz Hive u Excel.


##<a name="to-run-the-sample"></a>Da biste pokrenuli uzorka

1. S [Portala za Azure](https://portal.azure.com/), iz Startboard (Ako je prikvačena na klaster), kliknite pločicu klaster na kojem želite pokrenuti uzorka.

2. Iz plohu klaster, u odjeljku **Brze veze**kliknite **Nadzorna ploča klaster**, pa iz plohu **Klaster nadzorne ploče** **Nadzorne ploče klaster HDInsight**. Umjesto toga možete otvoriti izravno na nadzornoj ploči pomoću sljedeći URL:

        https://<clustername>.azurehdinsight.net
    
    Kada se to od vas zatraži, autentičnost pomoću administrator korisničko ime i lozinku koje ste koristili prilikom dodjele resursa klaster.
  
2. S web-stranice kliknite karticu **Galerija početak rada** , pa kategoriji **rješenja s oglednim podacima** uzorka **Analizu zapisnika web-mjesta** .

3. Slijedite upute na web-stranicu da biste dovršili uzorka.

##<a name="next-steps"></a>Daljnji koraci
Isprobajte sljedeći primjer: [analiziranje senzor podataka pomoću grozd s HDInsight](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
