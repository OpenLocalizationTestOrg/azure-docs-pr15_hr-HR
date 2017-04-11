<properties
    pageTitle="Što je HBase u HDInsight? | Microsoft Azure"
    description="Uvod u Apache HBase u HDInsight, baze podataka NoSQL nadograđuju Hadoop. Dodatne informacije o slučajevima koristi i usporedite HBase s drugim klastere Hadoop."
    keywords="bigtable, nosql, što je hbase"
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
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Što je HBase u HDInsight: A NoSQL baze podataka koje pruža mogućnosti nalik BigTable za Hadoop

Apache HBase je Otvori izvor NoSQL baza podataka koja se temelji na Hadoop i katalog modeliran nakon Google BigTable. HBase nudi izravnim pristupom i istaknuti dosljednost za velike količine podataka u bazu schemaless razvrstan po stupcu linije nestrukturirane i semistructured.

Podaci se pohranjuju u recima tablice i grupiranja podataka u retku po stupcu obitelj. HBase je schemaless baze podataka u smislu da stupce ni vrstu podataka pohranjenih u njima moraju biti definiran prije njihova korištenja. Otvorite izvorni kod Linearno mijenja veličinu za rukovanje petabytes podataka na tisuće čvorove. Možete se oslanjate redundanciju podataka jer, obrade i ostale značajke koje nudi raspodijeljeno aplikacije u zajednici Hadoop.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Kako se HBase implementira u Azure HDInsight?

HDInsight HBase je ponuđen kao upravljanih klaster koji je integriran u okruženje za Azure. Skupina konfigurirani tako da biste pohranili podatke izravno u spremište blobova platforme Azure, što omogućuje niske latencije i povećanu elasticity u performanse i trošak mogućnosti. Time se omogućuje klijentima za izgradnju interaktivnih web-mjesta na kojima se koriste velikih skupova podataka da biste sastavili servise koje spremanje senzor i telemetrijskih podataka iz milijune krajnje točke i radi analize podataka u sa zadacima Hadoop. HBase i Hadoop je praktično početne točke za projekt velikih skupova podataka u Azure; Točnije, možete omogućiti u stvarnom vremenu pomoću aplikacije za rad s velikih skupova podataka.

Implementacija HDInsight upravlja arhitektura skaliranje iz HBase omogućuje automatsko sharding tablica, istaknuti dosljednost za čitanja i pisanja i automatsko prebacivanje. Performanse je poboljšana tako da u memoriji predmemoriju za čitanje i visokom propusnošću strujanje za zapisivanje. Dodjeljivanje virtualne mreže i dostupna je za HDInsight HBase. Detalje potražite u članku [Dodjela resursa za HDInsight klastere na Azure virtualne mreže] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Kako se upravlja podataka iz servisa HDInsight HBase?

Podaci se upravlja iz HBase pomoću na `create`, `get`, `put`, i `scan` naredbe iz ljuske HBase. Podaci se upisuju u bazu podataka pomoću `put` i čitati pomoću `get`. Na `scan` naredba koristi se za dohvaćanje podataka iz više redaka u tablici. Podatke možete se upravlja i pomoću na HBase C# API-JA, koji pruža klijentska biblioteka pri vrhu HBase REST API-JA. Baze podataka programa HBase može biti mu i pomoću grozd. Uvod u te programiranje modele, u odjeljku [Prvi koraci pri korištenju HBase s Hadoop u HDInsight][hbase-get-started]. Suautorstvo procesora dostupne su i, koji omogućuju obradu podataka u čvorove koja hostiraju bazu podataka.


## <a name="scenarios-use-cases-for-hbase"></a>Scenariji: Korištenje slučajeva za HBase
Slučaj Kanonski koristi za koje BigTable (i postaju HBase) stvorena je pretraživanje weba. Tražilice sastavljanje indeksa koji mapiranje uvjeta u web-stranice koje ih sadrže. No postoje mnogim drugim korištenje slučajevima koja je prikladna za HBase – nekoliko od kojih su detaljni popis u ovom odjeljku.

- Spremište ključa vrijednosti

    HBase mogu se koristiti kao u trgovini ključa vrijednosti, a nije prikladna za upravljanje sustavi za poruku. Facebook koristi HBase za sustav svoje poruke, a je idealna je za pohranu i upravljanje komunikacijama Internet. WebTable koristi HBase da biste potražili i upravljanje tablice koje se izdvajaju iz web-stranice.

- Podaci senzora

    HBase je korisno za dohvaćanje podataka koji se prikupljaju se postupno iz različitih izvora. To obuhvaća društvenih analize, vremenski niz čuvanja interaktivne nadzorne ploče najnovije trendova i mjerača i upravljanje sustavi zapisnika nadzora. Primjeri Bloomberg trader terminal i otvaranje vrijeme niz baze podataka (OpenTSDB), koji pohranjuje i omogućuje pristup metrike koji se prikupljaju o stanju sustavi server.

- U stvarnom vremenu upita

    [Phoenixu](http://phoenix.apache.org/) je SQL engine upita Apache HBase. Pristupa kao JDBC upravljački program, a omogućuje slanje upita i upravljanje HBase tablice pomoću SQL.

- HBase kao platforma

    Aplikacije mogu se izvoditi pri vrhu HBase korištenjem kao na datastore. Primjeri Phoenixu OpenTSDB, Kiji te značajke Titan iz. Aplikacije i možete integrirati s HBase. Primjeri grozd, Svinja, Solr, oluja, Flume, Impala, Spark, Ganglia i analize.


##<a name="next-steps"></a>Daljnji koraci

- [Početak rada s HBase s Hadoop u HDInsight][hbase-get-started]
- [Dodjela resursa za klastere HDInsight na Azure virtualne mreže] [hbase-provision-vnet]
- [Konfiguriranje replikacije HBase u HDInsight](hdinsight-hbase-geo-replication.md)
- [Analiza Twitter šalju s HBase u HDInsight][hbase-twitter-sentiment]
- [Kako koristiti Maven za izradu Java aplikacije koje koriste HBase s HDInsight (Hadoop)][hbase-build-java-maven]

##<a name="see-also"></a>Vidi također

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: Raspodijeljeno prostora za pohranu sustav za strukturiranih podataka](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
