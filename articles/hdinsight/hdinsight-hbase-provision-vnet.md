<properties
    pageTitle="Stvaranje HBase klastere u virtualne mreže | Microsoft Azure"
    description="Početak rada s HBase u Azure HDInsight. Saznajte kako stvoriti HDInsight HBase klastere na Azure virtualne mreže."
    keywords=""
    services="hdinsight,virtual-network"
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
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Stvaranje HBase klastere u Azure virtualne mreže 

Saznajte kako stvoriti Azure HDInsight HBase klastere [Azure virtualne mreže][1].

Uz integraciju virtualne mreže klastere HBase može uvesti u istom virtualne mreže kao aplikacija bi aplikacija možete komunicirati s HBase izravno. Prednosti obuhvaćaju sljedeće:

- Izravni povezivanje web-aplikaciju za čvorove klaster HBase koja omogućuje komunikaciju putem udaljene procedure HBase Java poziv API-ji (RPC).
- Bolje performanse tako da ne morate prometa prijeđite preko više pristupnika i učitavanje balancers.
- Mogućnost za obradu povjerljive podatke više siguran način bez će javno krajnjoj točki.

###<a name="prerequisites"></a>Preduvjeti
Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Radne stanice s Azure PowerShell**. Potražite u članku [Instalacija i korištenje Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>Stvaranje HBase klaster u virtualne mreže

U ovom ćete odjeljku stvarate sustavom Linux HBase klaster s računom zavisne Azure prostora za pohranu u Azure virtualne mreže pomoću [predloška Azure Voditelj resursa](../resource-group-template-deploy.md). Drugi načini stvaranja klaster i razumijevanje postavke, potražite u članku [Stvaranje HDInsight klastere](hdinsight-hadoop-provision-linux-clusters.md). Dodatne informacije o korištenju predloška za stvaranje klastere Hadoop u HDInsight potražite u članku [Stvaranje Hadoop klastere u HDInsight pomoću predložaka Voditelj resursa za Azure](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Neka svojstva su programiranih u predložak. Ako, na primjer:
>
> * __Lokacija__: Istočni sad 2
> * Verzija __Cluster: 3.4
> * __Klaster tempiranja čvor count__: 4
> * __Zadani račun za pohranu__: &lt;klaster naziv > Spremanje
> * __Naziv mreže virtualne__: &lt;klaster naziv >-vnet
> * __Virtualna mreže adresnog prostora__: 10.0.0.0/16
> * __Naziv podmreže__: zadani
> * __Raspon adresu podmreže__: 10.0.0.0/24
>
> &lt;Skupine naziv > zamijenit će se naziv klaster unesete prilikom korištenja predloška.

1. Kliknite na sljedećoj slici da biste otvorili predložak na portalu za Azure. Predložak nalazi se u spremniku javno blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Iz plohu **implementacije Prilagođeno** unesite sljedeće:

    - **Pretplate**: Odaberite Azure pretplatu koji se koriste za stvaranje HDInsight klaster, zavisne račun za pohranu i Azure virtualne mreže.
    - **Grupa resursa**: odaberite **Stvaranje nove**i navedite novi naziv grupe resursa.
    - **Lokacija**: Odaberite mjesto za grupu resursa.
    - **ClusterName**: Unesite naziv klaster Hadoop koje ćete stvoriti.
    - **Klaster korisničko ime i lozinku**: ime za prijavu zadani je **administrator**.
    - **SSH korisničko ime i lozinku**: korisničko ime zadani je **sshuser**.  Možete je preimenovati. 
    - **Li pristajete da na uvjete i odredbe koji je naveden iznad**: (odaberite)
    

3. Kliknite **kupite**. Potrebno je oko oko 20 minuta da biste stvorili klaster. Nakon stvaranja klaster, možete kliknuti plohu klaster na portalu da biste ga otvorili.

Kada dovršite vodič, možda ćete morati izbrisati klaster. S HDInsight, vaši podaci se pohranjuju u Azure prostor za pohranu, da biste mogli sigurno izbrisati klaster kada se ne koristi. Također se naplatiti klaster programa HDInsight čak i ako se ne koristi. Budući da su naknade za klaster više puta veći od naknade za pohranu, je li bolje Ekonomske da biste izbrisali klastere kada se ne nalaze u upotrebi. Upute o brisanju klaster, potražite u članku [Upravljanje Hadoop klastere u HDInsight pomoću portala za Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

Da biste započeli rad s svoj novi HBase klaster, možete koristiti postupaka koji se nalaze u [Prvi koraci pri korištenju HBase s Hadoop u HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Povezivanje s HBase klaster pomoću HBase Java RPC API-ji

1.  U istoj Azure virtualne mreže i istoj podmreži stvorite do infrastrukture kao usluge (IaaS) virtualnog računala. Upute o stvaranju nove IaaS virtualnog računala potražite u članku [Stvaranje virtualnog računala izvodi Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Kada slijedite korake iz ovog dokumenta, morate koristiti sljedeće za konfiguraciju mreže:

    - __Virtualna mreže__: &lt;klaster naziv >-vnet
    - __Podmreže__: zadani

    > [AZURE.IMPORTANT] Zamjena &lt;klaster naziv > pod nazivom koristiti pri stvaranju klaster HDInsight u prethodnim koracima.

    Pomoću ove vrijednosti će konfigurirati virtualnog računala da biste koristili isti virtualne mreže i podmreže kao klaster HDInsight. Tako ćete omogućiti njihovo međusobno izravno komunicirati.

2.  Prilikom korištenja aplikacije za Java daljinsko povezivanje HBase, morate koristiti na potpuno kvalificirani naziv domene (FQDN). Da biste utvrdili, morate dobiti sufiks DNS specifične za klaster HBase. Da biste to učinili, možete koristiti jedan od sljedećih načina:

    * Da biste poziv Ambari pomoću web-preglednika:
    
        Pronađite https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hosts?minimal_response = true. To uključuje JSON datoteku s nastavke DNS.

    * Korištenje Ambari web-mjesta:

        1. Pronađite https://&lt;ClusterName >. azurehdinsight.net.
        2. Na gornjoj izborniku kliknite **domaćini** .

    * Korištenje zakretanja za OSTALE pozive:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        U vraćenih podataka JavaScript objekt notaciju (JSON) pronađite stavku "host_name". To će sadržavati FQDN za čvorove u klasteru. Ako, na primjer:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Dio početak naziva domene s nazivom klaster je DNS nastavak. Na primjer, mycluster.b1.cloudapp.net.

    * Koristite Azure PowerShell
    
        Koristite sljedeću skriptu Azure PowerShell da biste registrirali funkciju **Get-ClusterDetail** koji se može koristiti da biste se vratili DNS nastavak:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Nakon izvođenja skriptu Azure PowerShell, koristite sljedeću naredbu da biste se vratili DNS sufiks pomoću funkcije **Get-ClusterDetail** . Kada koristite tu naredbu, navedite HDInsight HBase klaster ime, naziv administrator i administratorsku lozinku.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Vratit će DNS nastavak. Na primjer, **yourclustername.b4.internal.cloudapp.net**.

    * Korištenje RDP
    
        Udaljena radna površina možete koristiti i za povezivanje s klaster HBase (koji će se povezati s glavni čvora) i pokretanje **ipconfig** iz naredbenog retka da biste dobili DNS nastavak. Upute za omogućivanje protokola udaljene radne površine (RDP) i povezivanje s klaster pomoću RDP, potražite u članku [Upravljanje Hadoop klastere u HDInsight pomoću portala za Azure][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Da biste potvrdili da virtualnog računala možete komunicirati s HBase klaster, upotrijebite naredbu `ping headnode0.<dns suffix>` iz virtualnog računala. Na primjer, pomoću naredbe ping headnode0.mycluster.b1.cloudapp.net.

Da biste koristili te podatke u Java računala, možete slijedite korake u [Maven koristite da biste sastavili Java aplikacije koje koriste HBase s HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) da biste stvorili aplikacije. Da bi aplikacija povezati udaljeni poslužitelj HBase, izmijenite **hbase site.xml** datoteke u ovom se primjeru koristi FQDN za Zookeeper. Ako, na primjer:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Dodatne informacije o Razrješavanje imena u Azure virtualne mreže, uključujući način upotrebe vlastite DNS poslužitelj potražite u članku [Razrješavanje naziva (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako stvoriti programa HBase klaster. Da biste saznali više, pogledajte:

- [Početak rada s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Konfiguriranje replikacije HBase u HDInsight](hdinsight-hbase-geo-replication.md)
- [Stvaranje klastere Hadoop u HDInsight](hdinsight-provision-clusters.md)
- [Početak rada s HBase s Hadoop u HDInsight](hdinsight-hbase-tutorial-get-started.md)
- [Analiza Twitter šalju s HBase u HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Pregled virtualne mreže][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Pojedinosti dodjele resursa za novog HBase klaster"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Akcijom skriptu da biste prilagodili programa HBase klaster"

[azure-preview-portal]: https://portal.azure.com
