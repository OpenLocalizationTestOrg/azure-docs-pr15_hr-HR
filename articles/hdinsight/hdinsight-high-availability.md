<properties
    pageTitle="Raspoloživost Hadoop klaster u HDInsight | Microsoft Azure"
    description="HDInsight uvodi iznimno dostupan je i pouzdan klastere s dodatnim glavni čvor."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Dostupnost i pouzdanost klastere utemeljen na sustavu Windows Hadoop u HDInsight


>[AZURE.NOTE] Koraci u ovom dokumentu su specifične za klastere HDInsight utemeljen na sustavu Windows. Ako koristite klaster sa sustavom Linux, potražite u članku [dostupnosti i pouzdanost klastere sustavom Linux Hadoop u HDInsight](hdinsight-high-availability-linux.md) informacije specifične za Linux.

HDInsight omogućuje klijentima za implementaciju razne vrste klaster, za radnih opterećenja analize podataka. Vrste klaster nudi Danas su Hadoop klastere za upit i analizu radnih opterećenja, klastere HBase za NoSQL radnih opterećenja i oluja klastere za radnih opterećenja obrada događaj stvarnom vremenu. Unutar vrstu navedeni klaster postoje različite uloge za različite čvorove. Ako, na primjer:



- Hadoop klastere za HDInsight uvode se sa dva uloge:
    - Lakši čvor (2 čvorove)
    - Čvor podataka (najmanje 1 čvora)

- HBase klastere za HDInsight uvode se sa tri uloge:
    - Lakši poslužitelje (2 čvorove)
    - Područje poslužitelje (najmanje 1 čvora)
    - Matrica/Zookeeper čvorove (3 čvorove)

- Oluja klastere za HDInsight uvode se sa tri uloge:
    - Čvorovi nimbus (2 čvorove)
    - Nadzornik poslužitelje (najmanje 1 čvora)
    - Čvorovi zookeeper (3 čvorove)

Standardni implementacije Hadoop klastere obično imaju jedan glavni čvor. HDInsight uklanja jednu od ovoga kvara s dodavanja sekundarne glavni čvor /head poslužitelja/Nimbus čvor da biste povećali dostupnosti i pouzdanosti potrebne za upravljanje radnih opterećenja servisa. Ove čvorove poslužitelja/Nimbus glavni čvorove/glave osmišljene su da provođenju upravljanje neuspjeh čvorove radnih, ali sve kvarove osnovne servise čvor glavni prouzročiti klaster prestat rad.


Čvorovi [zooKeeper](http://zookeeper.apache.org/ ) (ZKs) dodane i koriste se za izbore Vodeća crta od glavni čvorove, a da biste osigurali tu radnih čvorove i pristupnika (GWs) znati kada se neće putem sekundarne glavni čvor (glave Node1) kada aktivni glavni čvor (glave Node0) postane neaktivna.

![Dijagram iznimno pouzdanog glavni čvorove u implementaciji HDInsight Hadoop.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Provjerite status servisa active glavni čvor
Da biste odredili koji glavni čvor je aktivna i Provjera statusa services pokrenut na taj glavni čvor, morate se povezati s klaster Hadoop pomoću Remote Desktop Protocol (RDP). Upute za RDP potražite u članku [Upravljanje Hadoop klastere u HDInsight pomoću portala za Azure](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Kada ste povezani s klaster, dvokliknite ikonu **Hadoop usluge dostupne** koja se nalazi na radnoj površini da biste dobili stanja koji glavni čvor u Namenode, Jobtracker, Templeton, Oozieservice, Metastore i Hiveserver2 services pokrenut ili za services HDI 3.0, Namenode, Voditelj resursa, povijest poslužitelj, Templeton, Oozieservice, Metastore i Hiveserver2.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Na snimku zaslona, čvor aktivni glavni je *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Datoteke zapisnika programa Access na sekundarnom glavni čvor

Da biste pristupili posao zapisnika na sekundarnom glavni čvor za slučaj da postala je aktivan glavni čvor, pregledavanja JobTracker korisničkog Sučelja i dalje funkcionira kao i za primarni aktivni čvor. Da biste pristupili JobTracker, morate povezati s klaster Hadoop pomoću RDP opisan u prethodnom odjeljku. Nakon što dodate udaljen u klaster, dvokliknite ikonu **Hadoop naziv čvor Status** koja se nalazi na radnoj površini, a zatim kliknite na **NameNode zapisnika** da biste pristupili direktorij zapisnika na sekundarnom glavni čvor.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Konfiguriranje glavni čvor veličina
Glavni čvorove dodijeliti su kao velike virtualnim strojevima (VMs) prema zadanim postavkama. Ovo je prikladno za upravljanje Većina Hadoop zadatke koji su pokrenuti na klaster. No postoje scenarija koji se može zahtijevati vrlo veliki VMs za glavni čvorove. Primjer je kada klaster ima da biste upravljali velik broj small Oozie zadataka.

Vrlo veliki VMs moguće je konfigurirati pomoću cmdleta ljuske PowerShell Azure ili HDInsight SDK.

Stvaranje i dodjeljivanje klastere pomoću Azure PowerShell navedenih u [HDInsight upravljati pomoću komponente PowerShell](hdinsight-administer-use-powershell.md). Konfiguriranje vrlo veliki glavni čvor zahtijeva dodavanja u `-HeadNodeVMSize ExtraLarge` parametar u `New-AzureRmHDInsightcluster` cmdlet koji se koriste u kod.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Za SDK, slično je priče. Stvaranje i dodjeljivanje klastere pomoću SDK-navedenih u [Pomoću SDK .NET HDInsight](hdinsight-provision-clusters.md#sdk). Konfiguriranje vrlo veliki glavni čvor zahtijeva dodavanja u `HeadNodeSize = NodeVMSize.ExtraLarge` parametar u `ClusterCreateParameters()` način koji se koristi u kod.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Daljnji koraci

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Povezivanje s klastere HDInsight pomoću RDP](hdinsight-administer-use-management-portal.md#rdp)
- [Korištenje servisa HDInsight .NET SDK](hdinsight-provision-clusters.md#sdk)
