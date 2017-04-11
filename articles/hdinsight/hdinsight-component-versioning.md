<properties
    pageTitle="Što su različite komponente dostupno u sklopu programa HDInsight klaster? | Microsoft Azure"
    description="HDInsight podržava više deployable Hadoop klaster komponente i verzije. Potražite u članku Hadoop i HortonWorks podataka platformu (HDP) raspodjele verzije podržane."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="what-are-the-different-hadoop-components-available-with-hdinsight"></a>Što su različite Hadoop komponente dostupno u sklopu HDInsight?

Saznajte više o razinama drugi servis nudi HDInsight, kao i verzije komponente različite hadoop uključene HDInsight.

## <a name="hdinsight-standard-and-hdinsight-premium"></a>Standardna HDInsight i HDInsight Premium

Azure HDInsight nudi ponude oblaka velikih skupova podataka u dvije kategorije: **Standardno** i **Premium**. U tablici niže sekcije navode značajke koje su dostupne **samo u sklopu od Premium**. Značajke koje se ne izričito oblačićima u tablici u nastavku su dostupni kao dio standardno.

>[AZURE.NOTE] HDInsight Premium koja nudi trenutno u pretpregledu i dostupna samo za klastere Linux.

| Značajka HDInsight Premium | Opis |
|--------------|---------------|
| Domene pridruženo HDInsight klaster       | Uključivanje servisa HDInsight klastere domenama Azure Active Directory (AAD) za sigurnost na razini tvrtke. Sada možete konfigurirati popis zaposlenika iz tvrtke koje možete provjeriti autentičnost kroz Azure Active Directory za prijavu na klaster HDInsight. Administrator tvrtke možete konfigurirati i ulogama koji se temelje kontrola pristupa za sigurnost grozd pomoću [Apache Ranger](http://hortonworks.com/apache/ranger/), stoga ograničavanje pristupa podacima samo kao potrebne. Na kraju, administrator možete nadzirati podataka pristupati zaposlenika i promjene gotovo pravilnici kontrole programa access, stoga postizanje veliku upravljanja svoje tvrtke resursa. Dodatne informacije potražite u članku [Konfiguriranje domene pridruženo klastere HDInsight](hdinsight-domain-joined-configure).

### <a name="cluster-types-supported-for-premium"></a>Vrste klaster podržane za Premium

U sljedećoj su tablici navedeni HDInsight klaster vrsta i Premium podršku matrice.

| Vrsta klaster | Standardna  | Premium |
|--------------|---------------|--------------|
| Hadoop       | Da           | Da (HDInsight 3.5 samo)         |
| Spark        | Da           | ne          |
| HBase        | Da           | ne           |
| Oluja        | Da           | ne           |
| Interaktivni grozd (pretpregled) | Da | ne |
| R Server (pretpregled) | Da | ne |

U ovoj su tablici ažurirat će se kao što je više vrsta klaster obuhvaćeni HDInsight Premium.

### <a name="pricing-and-sla"></a>Cijene i SLA

Informacije o cijene i SLA za Premium servisa HDInsight potražite u članku [HDInsight cijene](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Hadoop komponente dostupne s različitim verzijama HDInsight

Azure HDInsight podržava više verzija klaster Hadoop koje mogu biti implementirano u bilo kojem trenutku. Svaki odabir verzija se stvara određenu verziju distribucije Hortonworks podataka platformu (HDP) i skup komponente koje se nalaze u toj raspodjele. U tablici u nastavku su detaljni popis verzije komponente povezane s verzije klaster HDInsight. Imajte na umu da na zadanu verziju klaster koristi Azure HDInsight trenutno 3.4, a, od 09/14/2016, ovisno o HDP 2.4.

> [AZURE.NOTE] Na zadanu verziju servisa može se promijeniti bez prethodne najave. Preporučujemo da odredite verziju prilikom stvaranja klastere pomoću .NET SDK/Azure PowerShell i EŽA Azure ako imate verziju ovisnosti. 

Komponenta|HDInsight verzije 3.5 | HDInsight verzija 3.4 (zadano) | HDInsight verzija 3,3 | HDInsight verzija 3,2 |HDInsight verzijom 3.1 |HDInsight verzije 3.0|
---|---|---|---|---|---|---
Platforme Hortonworks podataka|2,5|2.4|2.3|2.2|2.1.7|2.0|
Apache Hadoop i YARN|2.7.3|2.7.1|2.7.1|2.6.0|2.4.0|2.2.0|
Apache Tez|0.7.0|0.7.0|0.7.0 | 0.5.2|0.4.0||
Svinja Apache|0.16.0|0.15.0|0.15.0|0.14.0|0.12.1|0.12.0|
Grozd Apache i HCatalog|1.2.1.2.5|1.2.1|1.2.1|0.14.0|0.13.1|0.12.0|
Apache HBase |1.1.2|1.1.2|1.1.1|0.98.4|0.98.0||
Apache Sqoop|1.4.6|1.4.6|1.4.6|1.4.5|1.4.4|1.4.4|1.4.3
Apache Oozie|4.2.0|4.2.0|4.2.0|4.1.0|4.0.0|4.0.0|
Apache Zookeeper|3.4.6|3.4.6|3.4.6|3.4.6|3.4.5|3.4.5|
Apache oluja|1.0.1|0.10.0|0.10.0|0.9.3|0.9.1||
Apache Mahout|0.9.0+|0.9.0+|0.9.0+|0.9.0|0.9.0||
Apache Phoenixu|4.7.0|4.4.0|4.4.0|4.2.0|4.0.0.2.1.7.0-2162||
Apache Spark|1.6.2 + 2.0 (samo za Linux)|1.6.0 (samo za Linux)|1.5.2 (Linux samo/Experimental Sastavi)|1.3.1 (samo za Windows)|||

**Početak trenutne informacije o verziji komponente**

Verzije komponente povezane s HDInsight klaster verzije može se promijeniti u budućnosti ažuriranja za HDInsight. Jedan da biste odredili dostupna komponente i provjerite je li se za klaster koriste koje verzije tako da biste koristili Ambari REST API-JA. Naredba **GetComponentInformation** se može koristiti za dohvaćanje informacija o komponenta za servis. Detalje potražite u [dokumentaciji Ambari][ambari-docs]. Da biste dobili informacije i tako da se prijaviti u klaster pomoću udaljene radne površine i pregledajte sadržaj na "C:\apps\dist\" izravno direktorija.


**Napomene**

Dodatne napomene na najnovije verzije servisa HDInsight potražite u članku [HDInsight napomene](hdinsight-release-notes.md) .


## <a name="supported-hdinsight-versions"></a>Podržane verzije HDInsight
U sljedećoj su tablici navedeni verzije HDInsight trenutno dostupno, odgovarajuće verzije platforme Hortonworks podataka koje koriste i njihove datume izdanje. Kada zna, također se navode njihove podršku istek i postupku datume. Uzmite u obzir sljedeće:

* Iznimno dostupna klastere s dva glavni čvorove uvode se prema zadanim postavkama za HDInsight 2.1 i iznad. Nisu dostupne za klastere HDInsight 1.6.
* Kada podršku istekla za određenu verziju, možda neće biti dostupne putem portala za Azure. Sljedeća tablica pokazuje koje verzije dostupne su na portalu klasični Azure. Verzije klaster će se i dalje biti dostupni pomoću na `Version` parametar u naredbu komponente Windows PowerShell [Novo AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) i SDK .NET do datuma postupku.

HDInsight verzija|HDP verzija|VM OS|Visoke dostupnosti|Datum izdanja|Dostupne na portal za Azure|Datum isteka podrška|Datum u postupku
---|---|---|---|---|---|---|---
HDI 3.5 | HDP 2,5| Ubuntu 16 | Da | 30/9/2016 | Da | 
HDI 3.4|HDP 2.4|Ubuntu 14.0.4 LTS|Da|03/29/2016|Da|29/12/2016|9/1/2018|
HDI 3,3|HDP 2.3|Ubuntu 14.0.4 2012R2 LTS ili Windows Server|Da|02/12/2015|Da|27/06/2016|07/31/2017
HDI 3,2|HDP 2.2|Ubuntu 12.04 2012R2 LTS ili Windows Server|Da|18/2/2015|Da|3/1/2016|04/01/2017
HDI 3.1|HDP 2.1|Windows Server 2012R2|Da|6/24/2014.|ne|05/18/2015.|06/30/2016
HDI 3.0|HDP 2.0|Windows Server 2012R2|Da|02/11/2014.|ne|09/17/2014.|06/30/2015.
HDI 2.1|HDP 1.3|Windows Server 2012R2|Da|28/10/2013|ne|05/12/2014.|05/31/2015.
HDI 1.6|HDP 1.1||ne|28/10/2013|ne|04/26/2014.|05/31/2015.

**Uvođenje klastere koji nisu zadani**

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>Ugovor o razini usluge za HDInsight klaster verzije

Na SLA definirana je pomoću "Podršku prozor". Podrška za prozor odnosi se na vremenskog razdoblja koje pomoću verzije klaster HDInsight podržava Microsoftove službe za podršku. Programa HDInsight klaster je izvan prozora podršku njegova verzija sadrži **Datum isteka podršku** prošle trenutni datum. Popis podržanih verzija HDInsight klaster pronaći ćete u gornjoj tablici. Datum isteka podrška za dani HDInsight verziju X (kada je dostupna novija verzija X + 1) se izračunava kao novijem od:  

- Formula 1: Dodavanje 180 dana verziju klaster HDInsight datum izdavanja X.
- Formula 2: Dodavanje 90 dana datumu HDInsight klaster verzije X + 1 (sljedeće verzije nakon X) postane dostupna na portalu.

**Datum postupku** je datum nakon kojeg nije moguće stvoriti verziju klaster na HDInsight.

> [AZURE.NOTE] Utemeljen na sustavu Windows (uključujući verzija 2.1, 3.0, 3.1, 3,2 i 3,3) klaster HDInsight se izvoditi na Azure goste OS obitelji 4, koji koristi 64-bitnu verziju sustava Windows Server 2012 R2 te podržava .NET Framework 4.0, 4,5, 4.5.1 i 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks napomene povezana s verzijama HDInsight##

* HDInsight klaster verziju 3.4 koristi Hadoop raspodjele koji se temelji na [Hortonworks podataka platformu 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html). Ovo je klaster Hadoop **zadani** stvorena prilikom pomoću portala za.



* HDInsight klaster verziju 3,3 koristi Hadoop raspodjele koji se temelji na [Hortonworks podataka platformu 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).
    * Napomene u Apache oluja dostupne su [u nastavku](https://storm.apache.org/2015/11/05/storm0100-released.html).
    * Dostupne vrste Hive Apache napomenama [u nastavku](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).

* HDInsight klaster verziju 3,2 koristi Hadoop raspodjelu koji se temelji na [Hortonworks podataka platformu 2.2][hdp-2-2].  

    * Napomene za određene Apache komponente - [vrste Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Svinja 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenixu 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2,6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2,6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [uobičajenih](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [oluja 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).


* HDInsight klaster verzijom 3.1 koristi Hadoop raspodjelu koji se temelji na [Platformi podataka Hortonworks 2.1.7][hdp-2-1-7]. HDInsight 3.1 klastere stvorenih prije 11/7/2014 temelje se na [Platformi podataka Hortonworks 2.1.1][hdp-2-1-1].

* HDInsight klaster verzije 3.0 koristi Hadoop raspodjelu koji se temelji na [Hortonworks podataka platformu 2.0][hdp-2-0-8].

* HDInsight klaster verzija 2.1 koristi Hadoop raspodjelu koji se temelji na [Hortonworks podataka platformu 1.3][hdp-1-3-0].

* HDInsight klaster verziju 1.6 koristi Hadoop raspodjelu koji se temelji na [Hortonworks podataka platformu 1.1][hdp-1-1-0].


[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
