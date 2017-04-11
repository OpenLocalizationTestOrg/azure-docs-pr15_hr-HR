<properties
pageTitle="Priključci koristi HDInsight | Azure"
description="Popis priključaka koristi Hadoop servise HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Priključci i ji koristi HDInsight

Ovaj dokument sadrži popis priključaka koristi Hadoop servise utemeljene na Linux HDInsight klastere. Također nudi informacije o priključke koji se koristi za povezivanje s klaster pomoću SSH.

## <a name="public-ports-vs-non-public-ports"></a>Javni priključke nasuprot-javne priključci

Sustavom Linux HDInsight klastere izlaže samo tri priključke javno na Internetu; 22, 23 i 443. Koriste se za sigurno pristupali klaster pomoću SSH i servise izložen putem sigurne HTTPS protokola.

Interno, HDInsight je implementirana po nekoliko virtualnim računalima sustava Azure (čvorove unutar klaster,) sustavom Azure virtualne mreže. Iz unutar virtualne mreže možete pristupiti priključke izložen putem Interneta. Ako, na primjer, ako se povezujete na neki od glavni čvorove pomoću SSH, glavni čvorovi možete pa izravno pristupiti servise čvorove klaster.

> [AZURE.IMPORTANT] Kada stvorite klaster programa HDInsight ako ne navedete Azure virtualne mreže kao mogućnost za konfiguraciju, bilježnica će se stvoriti; Međutim, ne možete uključiti druga računala (kao što su druge virtualnim računalima sustava Azure ili razvoj računalu klijentskog) da biste to automatski stvara virtualne mreže. 

Da biste pristupili dodatnim računalima virtualne mrežu, morate najprije stvorite virtualne mreže i ga navedete prilikom stvaranja svoj klaster HDInsight. Dodatne informacije potražite u odjeljku [Mogućnosti proširivanje servisa HDInsight pomoću Azure virtualne mreže](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Javni priključci

Sve čvorove programa HDInsight klaster nalaze se u Azure virtualne mreže, a ne možete izravno pristupiti putem Interneta. Pristupnik za javno omogućuje pristup Internetu sljedeće priključke koji su uobičajeni kroz sve vrste klaster HDInsight.

| Servis | Priključak | Protokol | Opis |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Povezuje klijenti sshd na primarni headnode. U odjeljku [Korištenje SSH s operacijskim sustavom Linux HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Povezuje klijenti sshd na rub čvor (samo za HDInsight Premium). U odjeljku [Prvi koraci pri korištenju R Normalni prikaz na HDInsight](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Povezuje klijenti sshd na sekundarnom headnode. U odjeljku [Korištenje SSH s operacijskim sustavom Linux HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari web korisničkog Sučelja. Potražite u članku [Upravljanje HDInsight pomoću Ambari Web korisničkog Sučelja](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari REST API-JA. Potražite u članku [Upravljanje HDInsight pomoću Ambari REST API -JA](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog REST API-JA. Potražite u članku [Korištenje grozd s zakretanja](hdinsight-hadoop-use-pig-curl.md), [Svinja s zakretanja želite koristiti](hdinsight-hadoop-use-pig-curl.md), [MapReduce s zakretanja](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Povezuje se grozd pomoću ODBC. Potražite u članku [Povezivanje programa Excel sa servisom HDInsight pomoću Microsoft ODBC upravljački program](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Povezuje se grozd pomoću JDBC. Potražite u članku [Povezivanje grozd na HDInsight pomoću vrste Hive JDBC upravljački program](hdinsight-connect-hive-jdbc-driver.md) |

Ovo su dostupne za klaster određene vrste:

| Servis | Priključak | Protokol |Vrsta klaster | Opis |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase REST API-JA. U odjeljku [Prvi koraci pri korištenju HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livije | 443 | HTTPS |  Spark | Spark REST API-JA. U odjeljku [Slanje Spark poslove daljinski pomoću Livije](hdinsight-apache-spark-livy-rest-interface.md) |
| Oluja | 443 | HTTPS | Oluja | Oluja web korisničkog Sučelja. U odjeljku [uvođenje i upravljanje oluja topologija na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Provjera autentičnosti

Sve servise javno izložen putem Interneta mora proći:

| Priključak | Vjerodajnice |
| ---- | ----------- |
| 22 ili 23 | Navedeni tijekom stvaranja klaster korisničke vjerodajnice za SSH |
| 443 | Ime za prijavu (zadani: administrator) i lozinku koje su postavljeni tijekom stvaranja klaster |

## <a name="non-public-ports"></a>Osobe koje nisu javno priključci

> [AZURE.NOTE] Neke usluge dostupne su samo na klaster određene vrste. Ako, na primjer, HBase dostupna je samo za vrste HBase klaster.

### <a name="hdfs-ports"></a>HDFS priključci

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode web korisničkog Sučelja | Čvorovi glave | 30070 | HTTPS | Web korisničkog Sučelja za prikaz trenutnog statusa |
| Servis NameNode metapodataka | Glavni čvorove | 8020 | IPC | Metapodaci sustava datoteka 
| DataNode | Sve čvorove tempiranja | 30075 | HTTPS | Web-Sučelja za prikaz statusa, zapisnika itd. |
| DataNode | Sve čvorove tempiranja | 30010 | &nbsp; | Prijenos podataka |
| DataNode | Sve čvorove tempiranja | 30020 | IPC | Operacije metapodataka |
| Sekundarni NameNode | Čvorovi glave | 50090 | HTTP | Kontrolna točka za NameNode metapodatke |

### <a name="yarn-ports"></a>Priključci YARN

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Voditelj resursa web korisničkog Sučelja | Čvorovi glave | 8088 | HTTP | Korisničko Sučelje za Voditelj resursa za web |
| Voditelj resursa web korisničkog Sučelja | Čvorovi glave | 8090 | HTTPS | Korisničko Sučelje za Voditelj resursa za web |
| Voditelj resursa administrator sučelja | Glavni čvorove | 8141 | IPC | Za aplikacije poslanih stavki (grozd, grozd poslužitelj, Svinja, itd.) |
| Voditelj resursa rasporeda | Glavni čvorove | 8030 | HTTP | Administrativni sučelja |
| Voditelj resursa sučelje aplikacije | Glavni čvorove | 8050 | HTTP |Adresa Upravitelj sučelja aplikacije |
| NodeManager | Sve čvorove tempiranja | 30050 | &nbsp; | Adresa upravitelju spremnik |
| NodeManager web korisničkog Sučelja | Sve čvorove tempiranja | 30060 | HTTP | Sučelje za upravitelj resursa |
| Adresa vremenske trake | Čvorovi glave | 10200 | RPC | Servis RPC servis vremenske crte. |
| Vremenska crta web korisničkog Sučelja | Čvorovi glave | 8181 | HTTP | Na webu vremenske trake servisa korisničkog Sučelja |

### <a name="hive-ports"></a>Grozd priključci

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Čvorovi glave | 10001 | Thrift | Servis za povezivanje programski grozd (Thrift/JDBC) |
| HiveServer | Čvorovi glave | 10000 | Thrift | Servis za povezivanje programski grozd (Thrift/JDBC) |
| Metastore grozd | Čvorovi glave | 9083 | Thrift | Servis za povezivanje programski grozd metapodataka (Thrift/JDBC) |

### <a name="webhcat-ports"></a>Priključci WebHCat

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat poslužitelja | Čvorovi glave | 30111 | HTTP | API na webu pri vrhu HCatalog i drugim servisima za Hadoop |

### <a name="mapreduce-ports"></a>Priključci MapReduce

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Čvorovi glave | 19888 | HTTP | MapReduce JobHistory web korisničkog Sučelja |
| JobHistory | Čvorovi glave | 10020 | &nbsp; | MapReduce JobHistory poslužitelja |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Srednja karte prijenosi Proizvodi za traženje Reducers |

### <a name="oozie"></a>Oozie

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie poslužitelja | Čvorovi glave | 11000 | HTTP | URL servisa Oozie |
| Oozie poslužitelja | Čvorovi glave | 11001 | HTTP | Priključak za administratore Oozie |

### <a name="ambari-metrics"></a>Ambari mjerenja

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Vremenska crta (aplikaciju povijest) | Čvorovi glave | 6188 | HTTP | Na webu vremenske trake servisa korisničkog Sučelja |
| Vremenska crta (aplikaciju povijest) | Čvorovi glave | 30200 | RPC | Na webu vremenske trake servisa korisničkog Sučelja |

### <a name="hbase-ports"></a>Priključci HBase

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Čvorovi glave | 16000 | &nbsp; | &nbsp; |
| Informacije o HMaster korisničkog Sučelja Web | Čvorovi glave | 16010 | HTTP | Priključak za matricu HBase web-mjesto korisničkog Sučelja |
| Područje poslužitelja | Sve čvorove tempiranja | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Priključak koji klijenti koristiti za povezivanje s ZooKeeper |

### <a name="kafka-ports"></a>Priključci Kafka

| Servis | Node(s) | Priključak | Protokol | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Broker  | Čvorovi tempiranja | 9092 | [Protokol žičani Kafka](http://kafka.apache.org/protocol.html) | Koristi se za komunikaciju klijenta |
| &nbsp; | Čvorovi zookeeper | 2181 | &nbsp; | Priključak koji klijenti koristiti za povezivanje s Zookeeper |
