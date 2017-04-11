 <properties
    pageTitle="Uvod u Hadoop – što je Hadoop na HDInsight? | Microsoft Azure"
    description="Uvod u Hadoop, raspodijeljeno veliki obradu podataka i analizu i komponente zajednici Hadoop se u oblaku na HDInsight."
    keywords="Analiza velikih skupova podataka, Uvod u hadoop, što je hadoop, stoga tehnologiju hadoop, hadoop zajednici"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Uvod u zajednici Hadoop na Azure HDInsight

 Ovaj članak sadrži Uvod u Hadoop Azure HDInsight, zajednici i velikih skupova podataka. Saznajte više o komponenti Hadoop, uobičajeni terminologija i scenariji za analizu velikih skupova podataka.

## <a name="what-is-hadoop-on-hdinsight"></a>Što je Hadoop na HDInsight?

Hadoop se odnosi na zajednici Otvori izvor softver koji je okvir za raspodijeljeno obrade, spremanje i analizi velike skupa podataka na klastere računala. Azure HDInsight komponente Hadoop-distribucije **Hortonworks podataka platformu (HDP)** postaje dostupan u oblaku, i uvodi i dodjeljuje upravljanih klastere s visoke pouzdanosti i dostupnost.  

Apache Hadoop je izvorni Otvori izvor project za veliki obradu podataka. Sljedeći je razvoja povezanih softver i uslužnih smatra dijelom tehnologije snopa Hadoop, uključujući Apache vrste Hive, Apache HBase, Apache Spark i mnoge druge. Detalje potražite u članku [Pregled zajednici Hadoop u HDInsight](#overview) .

## <a name="what-is-big-data"></a>Što je velikih skupova podataka?

Velikih skupova podataka opisuju sve velikim tijelo digitalnih informacija iz teksta u Twitter sažetak sadržaja senzor informacija iz industrijske opreme s informacijama o klijentima pregledavanja i kupnjom na web-mjestu. Velikih skupova podataka može biti povijesne (znači pohranjene podatke) ili u stvarnom vremenu (znači strujanjem izravno s izvorišnog web-mjesta). Velikih skupova podataka koji se prikupljaju u radu escalating količine, pri smanjuju velocities i za proširivanje različite oblike.

Za velikih skupova podataka možete unijeti s akcijama obavještavanje ili uvid prikupljanje relevantnih podataka i postavljanje desnom pitanja. Morate provjerite jesu li podaci dostupni, očistiti analizirati, a zatim prezentirati na koristan način. To je gdje možete lakše analiza velikih skupova podataka na Hadoop u HDInsight.

## <a name="overview"></a>Pregled zajednici Hadoop u HDInsight

HDInsight je oblak raspodjele na Microsoft Azure hitro širi Apache Hadoop tehnologije stog analiza velikih skupova podataka. Obuhvaća implementacije Apache Spark, HBase, oluja, Svinja, grozd, Sqoop, Oozie, Ambari i tako dalje. HDInsight i integrira alate poslovne inteligencije (BI) kao što je Power BI, Excel, SQL Server Analysis Services i SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase, Spark, oluja i prilagođene klastere

HDInsight nudi klaster konfiguracije za Apache Hadoop, Spark, HBase ili oluja. Ili možete [prilagoditi klastere s akcijama skripte](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: pohrana podataka pouzdan pruža [HDFS](#hdfs)i jednostavan model programiranja [MapReduce](#mapreduce) za obradu i analiza podataka paralelno.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: paralelno obrada framework koji podržava obrada u memoriji da biste uvećali performanse aplikacija za analizu velikih skupova podataka, povećati works za SQL, strujanja podataka, i učenje na računalu. U odjeljku [Pregled: što je Apache Spark u HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: NoSQL baze podataka koje se temelji na Hadoop koja omogućuje izravnim pristupom i istaknuti dosljednost velike količine nestrukturirane i djelomično strukturirane podatke – potencijalno milijarde redaka puta milijune stupaca. Potražite u članku [Pregled HBase na HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache oluja</a>**: sustav raspodijeljeno, u stvarnom vremenu izračuni za brza obrada velike strujanja podataka. Oluja je ponuđen kao upravljanih klaster u HDInsight. Potražite u članku [Analiza u stvarnom vremenu senzor podataka pomoću oluja i Hadoop](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Primjer skripte za prilagodbu

Akcije skripte su skripte koje pokrenite tijekom dodjeljivanja klaster pa se poslužite da biste instalirali dodatne komponente na klaster. Za klastere sustavom Linux to su tulumu skripti.

Sljedeći primjer skripte nudi timu HDInsight:

* [Nijanse](hdinsight-hadoop-hue-linux.md): A skup web-aplikacije koje se koriste za interakciju s klaster. Samo klaster Linux.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): grafikonu obrade modela odnose između stvari ili osobe.

* [R](hdinsight-hadoop-r-scripts-linux.md): Otvori izvor jezik i okruženje za računalstvo statističke koristi u strojnog učenja.

* [Solr](hdinsight-hadoop-solr-install-linux.md): platforme enterprise skaliranje pretraživanje koji omogućuje pretraživanja cijelog teksta na podacima.

Informacije o razvoju skripte Akcije, potražite u članku [razvoja skripte akciju s HDInsight](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Što su komponente Hadoop i uslužnih?

Sljedeće komponente i uslužnih nalaze se na klastere HDInsight.

* **[Ambari](#ambari)**: skupine dodjele resursa, upravljanje, praćenje i uslužni programi.

* **[Avro](#avro)** (Biblioteka Microsoft .NET za Avro): serijalizacije podataka Microsoft .NET okruženju.

* **[Grozd & HCatalog](#hive)**: Structured Query Language (SQL) – kao što su slanje upita i upravljanje sloj tablice i prostora za pohranu.

* **[Mahout](#mahout)**: skalabilni strojnog učenja aplikacije.

* **[MapReduce](#mapreduce)**: naslijeđen okvir Hadoop distributed obrada i upravljanja resursima. Potražite u članku [YARN](#yarn), framework dalje generacije resursa.

* **[Oozie](#oozie)**: upravljanje tijekom rada.

* **[Phoenixu](#phoenix)**: sloj relacijske baze podataka putem HBase.

* **[Svinja](#pig)**: jednostavniji skriptiranje MapReduce transformacije.

* **[Sqoop](#sqoop)**: podataka uvoz i izvoz.

* **[Tez](#tez)**: omogućuje podataka ćete morati usko procesa da biste pokrenuli učinkovito na razini.

* **[YARN](#yarn)**: dio Hadoop core biblioteke i ama framework softver MapReduce.

* **[ZooKeeper](#zookeeper)**: koordinaciji procesa u raspodijeljeno sustavima.

> [AZURE.NOTE] Podaci na određene komponente i verzija, potražite u članku [Hadoop komponente, rad s verzijama, i ponuda servisa u HDInsight][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari je za dodjelu resursa, upravljanje i nadzor klastere Apache Hadoop. Obuhvaća intuitivno skup alata za operator i robusni skup API-ji skrivanje složenost Hadoop, pojednostavljivanje operacija klastere. Sustavom Linux HDInsight klastere pružaju i na Ambari web-korisničkog Sučelja i Ambari REST API-JA, dok je utemeljen na sustavu Windows klastere predstavljaju podskup REST API-JA. Prikazi Ambari na HDInsight klastere omogućite dodatak mogućnosti korisničkog Sučelja.

Potražite u članku [Upravljanje HDInsight klastere pomoću Ambari](hdinsight-hadoop-manage-ambari.md) (samo za Linux), [Monitor Hadoop klastere u HDInsight pomoću Ambari API -JA](hdinsight-monitor-use-ambari-api.md)i <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API reference</a>.

### <a name="avro"></a>Avro (Microsoft .NET biblioteka za Avro)

U biblioteci Microsoft .NET za Avro primjenjuje oblik za razmjenu Apache Avro sažetom binarne podatke za serijalizacije Microsoft .NET okruženju. Koristi <a target="_blank" href="http://www.json.org/">JavaScript objekt notacije (JSON)</a> da biste definirali jezik agnostic sheme underwrites interoperabilnost jezik, značenje podataka serijalizirani na jednom jeziku možete pročitati u drugu. Detaljne informacije o obliku moguće je pronaći u na < cilj = _ "prazno" href = "http://avro.apache.org/docs/current/spec.html" > Apache Avro specifikacija</a>.
Oblik datoteke Avro podržava raspodijeljeno model programiranja MapReduce. Datoteke su "splittable", što znači da možete traženje bilo koje mjesto u datoteci, a zatim pokrenite čitanje s određenom blok. Da biste saznali kako, potražite u članku [Serialize podataka s bibliotekom Microsoft .NET za Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Raspodijeljeno datoteke Hdps sustava (HADOOP) je raspodijeljeno datotečni sustav koji MapReduce i YARN, je core Hadoop zajednici. HDFS je standardni datotečni sustav za klastere Hadoop na HDInsight.

### <a name="hive"></a>Grozd & HCatalog

<a target="_blank" href="http://hive.apache.org/">Vrste Hive Apache</a> je podataka skladištu softver utemeljena na Hadoop koji vam omogućuje da upita, a zatim upravljanje velikih skupova podataka u raspodijeljeno spremište pomoću jezika SQL poput naziva HiveQL. Grozd, kao što su Svinja, je programa apstrakcije pri vrhu MapReduce. Prilikom pokretanja, grozd prevodi upita u nizu MapReduce zadataka. Grozd je pojmovno bliže sustava za upravljanje relacijske baze podataka od Svinja i stoga se koriste s više strukturiranih podataka. Za nestrukturirane podatke, Svinja je bolji odabir. Potražite u članku [Korištenje grozd s Hadoop u HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> je upravljanje sloj tablice i prostora za pohranu za Hadoop koja predstavlja korisnicima relacijski prikaz podataka. U HCatalog, čitanje i pisanje datoteka u nekom obliku za koje je biti napisani vrste Hive SerDe (serijalizatora deserializer).

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> je biblioteci skalabilni strojnog učenja algoritmima pomoću kojih se izvoditi na Hadoop. Pomoću načela Statistički podaci o računalu učenje aplikacije Podučavanje sustavi dodatne iz podataka i koristite prošle ishoda da biste odredili buduće ponašanje. U odjeljku [Generiraj film preporuke pomoću Mahout na Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce je framework naslijeđene softver za Hadoop za pisanje aplikacije za skupnu postupak velike skupove podataka paralelno. Posao MapReduce dijeli velikih skupova podataka i organizira podatke u parovima ključnih vrijednosti za obradu.

[YARN](#yarn) je okvir Hadoop resursa dalje generacije upravitelj i aplikacije, a naziva se MapReduce 2.0. MapReduce poslove se izvoditi na YARN.

Dodatne informacije o MapReduce potražite u članku <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> u Hadoop Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> je sustav koordinaciji tijeka rada koja upravlja Hadoop zadatke. Integriran u stogu Hadoop i podržava Hadoop zadacima za MapReduce, Svinja, grozd i Sqoop. Može se koristiti i zakazivanje zadataka specifične za sustav, kao što su Java programe ili skripti ljuske. Potražite u članku [Korištenje Oozie s Hadoop](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenixu
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenixu</a> je sloj relacijske baze podataka putem HBase. Phoenixu obuhvaća JDBC upravljački program koji korisnicima omogućuje slanje upita i upravljanje tablice SQL izravno. Phoenixu prevodi upite i ostale naredbe u izvorni pozive NoSQL API - umjesto korištenja MapReduce - stoga Omogućivanje brže aplikacija pri vrhu NoSQL trgovine. U odjeljku [Korištenje Apache Phoenixu i SQuirreL s HBase klastere](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Svinja
<a  target="_blank" href="http://pig.apache.org/">Svinja Apache</a> je više razine platformu koja omogućuje izvođenje složenih transformacije MapReduce na vrlo velike skupove podataka pomoću jednostavne skriptnog jezika naziva latinica Svinja. Svinja prevodi skripte latinica Svinja tako koje ćete pokrenuti unutar Hadoop. Stvorite korisnički definirane funkcije (UDF) da biste proširili latinica Svinja. Potražite u članku [Korištenje Svinja s Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> je alat za da prijenosi skupno podataka između Hadoop i relacijske baze podataka kao što je SQL ili druge trgovine strukturiranih podataka učinkovito moguće. Potražite u članku [Korištenje Sqoop s Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> je programa framework aplikacije utemeljena na YARN Hadoop koji se izvršava složene, acyclic grafova Općenito obradu podataka. To je Fleksibilno i naprednih naslijediti MapReduce okvir koji omogućuje podataka ćete morati usko procesima, kao što su grozd, učinkovitije raditi na razini. U odjeljku ["Koristi Apache Tez za bolje performanse" u vrste Hive koristi i HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN je generacije MapReduce (MapReduce 2.0 ili MRv2), a podržava scenariji za obradu podataka izvan MapReduce skupna obrada s veći skalabilnost i obrada u stvarnom vremenu. YARN omogućuje upravljanje resursima i okvir distribuirane aplikacije. MapReduce poslove se izvoditi na YARN.

Da biste saznali više o YARN, pogledajte <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinate procesa u velikim raspodijeljeno sustavima putem zajedničkog hijerarhijski prostor naziva Registri podataka (znodes). Znodes sadrže manjih količina meta podatke koji su potrebni za koordinaciju procesa: stanje, mjesto, konfiguriranje i tako dalje.

## <a name="programming-languages-on-hdinsight"></a>Programski jezici na HDInsight

HDInsight klastere – Hadoop, HBase, oluja i Spark klastere – podržava broj programskog jezika, ali neke nisu instalirane prema zadanim postavkama. Za biblioteke, moduli ili pakete prema zadanim postavkama nije instaliran pomoću skripte akcije da biste instalirali komponentu. U odjeljku [skripte razvoja akciju s HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Zadani programskom jeziku podrška

Prema zadanim postavkama HDInsight klaster podrške:

* Java

* Python

Dodatne jezike možete instalirati i pomoću akcije skripte: [skripte razvoja akciju s HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Jezika Java virtualnog računala (JVM)

Više jezika koji nije Java mogu se izvoditi pomoću Java virtualnog računala (JVM) Međutim, neke od tih jezika izvodi možda ćete morati dodatne komponente instalirano klaster.

Ovim jezicima koji se temelji na JVM podržani su na klastere HDInsight:

* Clojure

* Jython (Python za Java)

* Scala

### <a name="hadoop-specific-languages"></a>Specifične za Hadoop jezika

HDInsight klastere podržava sljedeće jezike koje su specifične za Hadoop zajednici:

* Svinja latinica za Svinja poslove

* HiveQL grozd zadacima i SparkSQL


## <a name="advantage"></a>Prednosti Hadoop u oblaku

Kao dio zajednici Azure oblaka, Hadoop u HDInsight nudi brojne prednosti, među njima:

* Automatsko dodjeljivanje Hadoop klaster. HDInsight klastere su jednostavniji da biste stvorili od ručno konfiguriranje klastere Hadoop. Detalje potražite u članku [Dodjeljivanje Hadoop klastere u HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* Komponente Hadoop stanje od-na-crteža. Detalje potražite u članku [Hadoop komponente, rad s verzijama, i ponuda servisa u HDInsight][component-versioning].

* Visoke dostupnosti i pouzdanost klastere. Dodatne informacije potražite u [dostupnosti i pouzdanost klastere Hadoop u HDInsight](hdinsight-high-availability-linux.md) .

* Pohrana učinkoviti su i najekonomičniji podataka s Azure blobova, Hadoop kompatibilnog mogućnost. Detalje potražite u članku [Korištenje Azure blobova s Hadoop u HDInsight](hdinsight-hadoop-use-blob-storage.md) .

* Integracija s drugim Azure servisima, uključujući [web-aplikacije](https://azure.microsoft.com/documentation/services/app-service/web/) i [SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/).

* Dodatni VM veličine i vrste radi klastere HDInsight. U odjeljku [Hadoop komponente, rad s verzijama, i ponuda servisa u HDInsight] [ component-versioning] detalje.

* Skupine skaliranja. Skaliranje klaster omogućuje vam da biste promijenili broj čvorove izvodi HDInsight klaster bez potrebe da biste izbrisali ili ponovno stvoriti.

* Virtualna podrška za mrežu. HDInsight klastere možete koristiti u sklopu Azure virtualne mreže za podršku odvajanja oblaka resursa ili scenarija hibridnog koje sadrže vezu oblaka resursi s onima u vašem podatkovnog centra.

* Niska unos troškova. Pokretanje [besplatnu probnu verziju](https://azure.microsoft.com/free/)ili se obratite [informacije o cijenama HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Za dodatne informacije o prednostima Hadoop u HDInsight potražite u članku [stranicu Azure značajke za HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>Standardna HDInsight i HDInsight Premium

HDInsight nudi ponude oblaka velikih skupova podataka u dvije kategorije standardnu i Premium. Standardna HDInsight nudi programa enterprise skaliranje klaster koje tvrtke ili ustanove mogu koristiti da biste pokrenuli njihove radnih opterećenja velikih skupova podataka. HDInsight Premium sastavlja na koji i njihovi napredne analytical i sigurnosne mogućnosti za programa klaster HDInsight. Dodatne informacije potražite u članku [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Resursi za učenje dodatne informacije o analiza velikih skupova podataka, Hadoop i HDInsight

Sastavljanje na ovom Uvod u Hadoop u oblak i analiza velikih skupova podataka s resursima u nastavku.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop dokumentaciji HDInsight

* [Dokumentacija HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/): stranici dokumentacija za Hadoop na Azure HDInsight pomoću veze na članke, videozapise i dodatni resursi.

* [Početak rada s Hadoop u HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): vodič za brzi početak rada za dodjeljivanje klastere HDInsight Hadoop i pokretanje upita grozd uzorka.

* [Početak rada s Spark u HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): vodič za brzi početak rada za stvaranje Spark klaster i pokretanje interaktivne Spark SQL upita.

* [Korištenje R Normalni prikaz na HDInsight](hdinsight-hadoop-r-server-get-started.md): početak korištenja R poslužitelja u HDInsight Premium.

* [Dodjela resursa za HDInsight klastere](hdinsight-hadoop-provision-linux-clusters.md): upute za dodjelu resursa za HDInsight Hadoop klaster kroz Azure portal, Azure EŽA ili Azure PowerShell.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: dodatne informacije o biblioteci Apache Hadoop softver, framework koja omogućuje raspodijeljeno obrada velikih skupova podataka preko klastere računala.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: dodatne informacije o arhitektura i dizajna na Hadoop Distributed File System, sustavu primarni prostora za pohranu koji se koristi Hadoop aplikacije.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">Praktični vodič MapReduce</a>: dodatne informacije o programskom framework za pisanje Hadoop aplikacije koje hitro obrada velike količine podataka paralelno na velike klastere čvorovi računalnim.


### <a name="microsoft-business-intelligence"></a>Microsoft poslovno obavještavanje

Alate poznatih poslovne inteligencije (BI) – kao što je Excel, dodatka PowerPivot, SQL Server Analysis Services i SQL Server Reporting Services - dohvatiti, analizirati i desni prikaz grupa integriran s HDInsight pomoću dodatka Power Query ili Microsoft vrste Hive ODBC upravljački program.

Ti Alati za Poslovno obavještavanje može pomoći u analizi velike podataka:

* [Povezivanje programa Excel sa Hadoop pomoću značajke Power Query](hdinsight-connect-excel-power-query.md): upute za povezivanje programa Excel s računom za pohranu Azure kojoj su pohranjeni podaci koji su povezani s svoj klaster HDInsight pomoću značajke Microsoft Power Query za Excel. Windows radne stanice obavezno. Funkcionira s klaster utemeljen na sustavu Windows ili Linux.

* [Povezivanje programa Excel sa Hadoop s Microsoft vrste Hive ODBC upravljački program](hdinsight-connect-excel-hive-odbc-driver.md): upute za uvoz podataka iz servisa HDInsight s Microsoft vrste Hive ODBC upravljački program. Windows radne stanice obavezno. Funkcionira s klaster utemeljen na sustavu Windows ili Linux.

* [Platforme Microsoft Cloud](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): dodatne informacije o dodatku Power BI za Office 365, preuzmite probnu verziju sustava SQL Server i postavljanje sustava SharePoint Server 2013 i SQL Server BI.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
