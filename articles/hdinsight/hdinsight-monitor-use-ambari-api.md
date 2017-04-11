<properties
    pageTitle="Praćenje klastere Hadoop u HDInsight pomoću Ambari API | Microsoft Azure"
    description="Koristite Apache Ambari API-ji za stvaranje, upravljanje i nadzor klastere Hadoop. Alati za intuitivno operator i API-sakriti složenost Hadoop."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Praćenje klastere Hadoop u HDInsight pomoću Ambari API-JA

Saznajte kako praćenje klastere HDInsight pomoću Ambari API-ji.

> [AZURE.NOTE] Informacije u ovom članku je prvenstveno za klastere HDInsight utemeljen na sustavu Windows, čime se omogućuje verziju Ambari REST API-JA koja je samo za čitanje. Sustavom Linux klastere potražite u članku [Upravljanje Hadoop klastere pomoću Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Što je Ambari?

[Apache Ambari] [ ambari-home] koristi se za dodjelu resursa, upravljanje i nadzor klastere Apache Hadoop. Obuhvaća intuitivno skup alata za operator i robusni skup API-ji skrivanje složenost Hadoop, pojednostavljivanje operacija klastere. Dodatne informacije o API-ji potražite u članku [Referenca za API Ambari][ambari-api-reference]. 

HDInsight trenutno podržava samo Ambari nadzora značajku. Ambari API 1.0 podržava klastere verzije 3.0 i 2.1 HDInsight. U ovom se članku opisuje pristupanju Ambari API-ji na klastere verzijom 3.1 i 2.1 HDInsight. Ključna razlika između dviju je neke komponente ste promijenili uz Uvod u nove mogućnosti (kao što je poslužitelj povijesti zadatka). 

**Preduvjeti**

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Radne stanice s Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Neobavezno) [cURL] [curl]. Da biste ga instalirali, potražite u članku [cURL izdanja i preuzimanja][curl-download].

    >[AZURE.NOTE] Kada upotrijebite naredbu zakretanja u sustavu Windows, koristite dvostrukim navodnicima umjesto jednog navodnim znakovima za mogućnost vrijednosti.

- **Klaster programa Azure HDInsight**. Upute o klaster dodjele resursa potražite u članku [Prvi koraci pri korištenju HDInsight] [ hdinsight-get-started] ili [HDInsight Dodjela resursa za klastere][hdinsight-provision]. Trebate prođite kroz vodič sljedeće podatke:

    Svojstvo klaster|Azure naziv varijable PowerShell|Vrijednost|Opis
    ---|---|---|---
    Naziv HDInsight klaster|$clusterName||Naziv svoj klaster HDInsight.
    Korisničko ime klaster|$clusterUsername||Klaster korisničko ime navedeno stvaranja klaster.
    Lozinka klaster|$clusterPassword||Klaster korisničke lozinke.

    >[AZURE.NOTE] Popunjavanje vrijednosti u tablici. To će biti korisno za prolaze kroz ovog praktičnog vodiča.

## <a name="jump-start"></a>Prelazak na početak

Praćenje klastere HDInsight pomoću Ambari na nekoliko načina.

**Koristite Azure PowerShell**

Slijedi skripte Azure PowerShell da biste dobili informacije o praćenju zadatku MapReduce *programa klasteru HDInsight 3.1.*  Ključne razlike je ne možemo povući te detalje iz na servis YARN (umjesto MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Ovo je skripte Azure PowerShell za početak na MapReduce posao Evidencija informacije *u klasteru HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Rezultat je:

![Izlaz Jobtracker][img-jobtracker-output]

**Korištenje zakretanja**

Slijedi primjer dohvaćanje podataka klaster pomoću zakretanja:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Rezultat je:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Na 10/8/2014 izdanja**:

Prilikom korištenja krajnju točku Ambari "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", polje *host_name* vraća na potpuno kvalificirani naziv domene (FQDN) čvora umjesto naziv glavnog računala. Prije izdanja 10/8/2014 u ovom primjeru vraća samo "**headnode0**". Nakon izdavanja 10/8/2014., dobivate FQDN "**headnode0. { ClusterDNS} .azurehdinsight .net**", kao što je prikazano u prethodnom primjeru. Ta promjena potreban je da biste olakšali scenariji u kojima se više vrsta klaster (kao što su HBase i Hadoop) može uvesti u jedan virtualne mreže (VNET). To se događa, na primjer, korištenje HBase kao pozadinske platformu za Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari API-ji za nadzor

U sljedećoj su tablici navedene neke od najčešćih Ambari nadzor API poziva. Dodatne informacije o API Sučelja potražite u članku [Referenca za API Ambari][ambari-api-reference].

Monitor API poziva|URI-JA|Opis
---|---|---
Početak klastere|`/api/v1/clusters`|
Zatražite informacije klaster.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|klastere servise, glavno računalo
Usluge|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Usluge obuhvaćaju: hdfs, mapreduce
Zatražite informacije o servisima.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Početak komponente za servis|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode, datanode<br/>MapReduce: jobtracker; tasktracker
Zatražite informacije o komponenti.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo, glavno računalo komponente, mjerenja
Početak domaćini|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
Zatražite informacije glavnog računala.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Početak komponente glavnog računala|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode, resourcemanager
Zatražite informacije o komponenti glavnog računala.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles komponente, glavno računalo, a zatim mjerenja
Početak konfiguracija|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Vrste config: core web-mjesta, web-mjesta hdfs, mapred web-mjesta, grozd web-mjesta
Zatražite informacije o konfiguraciji.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Vrste config: core web-mjesta, web-mjesta hdfs, mapred web-mjesta, grozd web-mjesta


##<a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako koristiti Ambari nadzor API poziva. Da biste saznali više, pogledajte:

- [Upravljanje klastere HDInsight pomoću portala za Azure][hdinsight-admin-portal]
- [Upravljanje klastere HDInsight pomoću komponente PowerShell Azure][hdinsight-admin-powershell]
- [Upravljanje klastere HDInsight pomoću sučelja naredbenog retka][hdinsight-admin-cli]
- [Dokumentacija HDInsight][hdinsight-documentation]
- [Početak rada s HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
