<properties
    pageTitle="Stvaranje Hadoop, HBase, oluja ili Spark klastere na Linux u HDInsight pomoću komponente PowerShell Azure | Microsoft Azure"
    description="Saznajte kako stvoriti Hadoop, HBase, oluja ili Spark klastere na Linux za HDInsight pomoću Azure PowerShell."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Stvaranje sustavom Linux klastere u HDInsight pomoću komponente PowerShell Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell je Napredna okruženje za skriptiranje koje možete koristiti za upravljanje i automatizirati implementacije i upravljanja vaše radnih opterećenja u Microsoft Azure. Ovaj dokument pruža informacije o stvaranju sustavom Linux HDInsight klaster pomoću Azure PowerShell. Obuhvaća i skripte primjer.

> [AZURE.NOTE] Azure PowerShell dostupna samo u sustavu Windows klijentima. Ako koristite Linux, Unix ili Mac OS X klijent, potražite u članku [Stvaranje sustavom Linux HDInsight klaster pomoću Azure EŽA](hdinsight-hadoop-create-linux-clusters-azure-cli.md) informacije o korištenju EŽA Azure da biste stvorili klaster.

## <a name="prerequisites"></a>Preduvjeti
Morate imati sljedeće prije početka ovog postupka:

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Dodatne informacije o korištenju Azure PowerShell sa servisa HDInsight potražite u članku [administriranje HDInsight pomoću komponente PowerShell](hdinsight-administer-use-powershell.md). Na popisu cmdleta ljuske Windows PowerShell servisa HDInsight potražite u članku [Referenca cmdlet HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Stvaranje klastere

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Da biste stvorili programa klaster HDInsight pomoću komponente PowerShell Azure, morate poduzeti sljedeće postupke:

- Stvaranje grupe sustava Azure resursa
- Stvorite račun za pohranu za Azure
- Stvaranje kontejnera za blobova platforme Azure
- Stvaranje programa HDInsight klaster

Dva najvažnija parametre koje morate postaviti da biste stvorili Linux klastere se oni koji odredite vrstu OS i Detalji o korisniku SSH:

- Provjerite je li navedete parametar **- OSType** kao **Linux**.
- Da biste koristili SSH udaljene sesije na skupina, možete odrediti korisničku lozinku SSH ili SSH javni ključ. Ako navedete SSH korisničku lozinku i javni ključ SSH tipku će zanemariti. Ako želite koristiti ključ SSH za udaljene sesije morate navesti praznu lozinku SSH upit za jednu. Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u nekom od sljedećih članaka:

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Sljedeću skriptu pokazuje kako stvoriti novi klaster:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Vrijednosti koje navedete za **$clusterCredentials** se koriste za stvaranje korisnički račun Hadoop za klaster. Pomoću ovog računa za povezivanje s klaster.

Vrijednosti koje navedete za **$sshCredentials** se koriste za stvaranje SSH korisnika za klaster. Pomoću ovog računa pokrenuti udaljenu sesiju SSH na klaster i koristiti zadatke.

> [AZURE.IMPORTANT] U ovom skripti morate navesti broj radnih čvorove koji će biti u klasteru. Ako namjeravate koristiti više od 32 čvorove tempiranja (ili pri klaster stvaranje ili promjena veličine skupine nakon stvaranja), morate navesti i glavni čvor veličina s najmanje 8 jezgri i 14 GB RAM-a.
>
> Dodatne informacije o čvor veličine i pridruženi troškova potražite u članku [HDInsight cijene](https://azure.microsoft.com/pricing/details/hdinsight/).

Može potrajati do 20 minuta da biste stvorili klaster.

Sljedeći primjer pokazuje kako dodati račun za dodatni prostor za pohranu:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Prilagodba klastere

- U odjeljku [Prilagodba HDInsight klastere pomoću samopokretanja programa](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- U odjeljku [klastere utemeljen na sustavu Windows za prilagođavanje HDInsight pomoću skripte akcije](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Brisanje klaster

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili uspješno je HDInsight klaster, pomoću sljedećih resursa da biste saznali kako raditi s svoj klaster.

### <a name="hadoop-clusters"></a>Hadoop klastere

* [Korištenje grozd s HDInsight](hdinsight-use-hive.md)
* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)
* [Korištenje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase klastere

* [Početak rada s HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Razvoj aplikacija Java HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Klastere oluja

* [Razvoj Java topologija za oluja na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Korištenje komponente Python oluja na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementacija i nadzirati topologija s oluja na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark klaster

* [Stvaranje samostalne aplikacije pomoću Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Pokretanje zadataka na Spark klaster pomoću Livije](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark bi: izvođenje analiza interaktivnih podataka pomoću Spark u HDInsight s alatima za Poslovno obavještavanje](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s strojnog učenja: korištenje Spark u HDInsight za predviđanje rezultata provjere za hranu](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark strujeće: Korištenje Spark u HDInsight za izgradnju u stvarnom vremenu strujanje aplikacije](hdinsight-apache-spark-eventhub-streaming.md)
