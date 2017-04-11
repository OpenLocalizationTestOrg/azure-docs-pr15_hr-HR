<properties 
   pageTitle="Korištenje Apache Phoenixu i SQuirreL u HDInsight | Microsoft Azure" 
   description="Saznajte kako koristiti Apache Phoenixu u HDInsight i kako instalirati i konfigurirati SQuirreL na vaše radne stanice za povezivanje programa klaster HBase u HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Korištenje Apache Phoenixu sa sustavom Linux HBase klastere u HDInsight  

Saznajte kako koristiti [Apache Phoenixu](http://phoenix.apache.org/) u HDInsight te kako koristiti SQLLine. Dodatne informacije o Phoenixu potražite u članku [Phoenixu 15 minuta ili manje](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Gramatiku Phoenixu potražite u članku [Phoenixu gramatiku](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Phoenixu informacije o verziji u HDInsight, potražite u članku [što je novo u verzijama klaster Hadoop nudi HDInsight?] [hdinsight-versions].

##<a name="use-sqlline"></a>Korištenje SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je uslužni program naredbenog retka za izvođenje SQL naredbe. 

###<a name="prerequisites"></a>Preduvjeti
Prije korištenja SQLLine mora imati sljedeće:

- **A HBase klaster u HDInsight**. Dodatne informacije o dodjele resursa HBase skupine, pročitajte članak [Početak rada s Apache HBase u HDInsight][hdinsight-hbase-get-started].
- **Povezivanje s klaster HBase putem protokola udaljene radne površine**. Upute potražite u članku [Upravljanje Hadoop klastere u HDInsight pomoću portala za klasični Azure][hdinsight-manage-portal].


Kada se povežete s programa HBase klaster, morat ćete povezati s nekom od na Zookeepers. Svaki klaster HDInsight ima 3 Zookeepers. 

**Da biste saznali naziv glavnog računala Zookeeper**

1. Da biste otvorili Ambari, Pregledaj da biste **https://<ClusterName>. azurehdinsight.net**.
2. Unesite HTTP (skup) korisničko ime i lozinku za prijavu.
3. Kliknite **ZooKeeper** na lijevom izborniku. 3 **ZooKeeper poslužitelj** naveden moraju vidjeti.
4. Kliknite jednu od **Poslužitelja ZooKeeper** na popisu. U oknu sažetka pronađite **naziv glavnog računala**. Slično *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*je.

**Da biste koristili SQLLine**

1. Povezivanje s klaster pomoću SSH. Upute potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, Unix, ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md) ili [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md) ovisno o klijentskom računalu s operacijskim Sustavom.

2. Iz SSH, pokrenite sljedeće naredbe da biste pokrenuli SQLLine:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Pokrenite sljedeće naredbe za stvaranje tablice HBase i umetnuti neki podaci:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Dodatne informacije potražite u članku [ručno SQLLine](http://sqlline.sourceforge.net/#manual) i [Phoenixu gramatiku](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Daljnji koraci
U ovom se članku ste naučili kako koristiti Apache Phoenixu u HDInsight.  Da biste saznali više, pročitajte članak

- [Pregled HDInsight HBase][hdinsight-hbase-overview]: HBase je Apache, Otvori izvor NoSQL baza podataka utemeljena na Hadoop koja omogućuje izravnim pristupom i istaknuti dosljednost velikih količina podataka nestrukturirane i semistructured.
- [Dodjela resursa za klastere HBase na Azure virtualne mreže][hdinsight-hbase-provision-vnet]: uz integraciju virtualne mreže klastere HBase može uvesti u istom virtualne mreže kao aplikacija bi aplikacija možete komunicirati s HBase izravno.
- [Konfiguriranje HBase replikacije u HDInsight](hdinsight-hbase-geo-replication.md): Saznajte kako konfigurirati replikacije HBase preko dva Azure podatkovnim centrima. 
- [Analiza Twitter šalju s HBase u HDInsight][hbase-twitter-sentiment]: Saznajte kako u stvarnom vremenu [šalju analiza](http://en.wikipedia.org/wiki/Sentiment_analysis) velikih skupova podataka pomoću HBase klasteru Hadoop u HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
