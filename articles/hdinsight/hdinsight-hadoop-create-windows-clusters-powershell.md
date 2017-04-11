<properties
   pageTitle="Stvaranje klastere utemeljen na sustavu Windows Hadoop u HDInsight pomoću komponente PowerShell Azure | Microsoft Azure"
    description="Saznajte kako stvoriti klastere za Azure HDInsight pomoću Azure PowerShell."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Stvaranje klastere utemeljen na sustavu Windows Hadoop u HDInsight pomoću komponente PowerShell Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Saznajte kako stvoriti klastere HDInsight pomoću komponente PowerShell Azure. Azure PowerShell je modul koji sadrži cmdleti za upravljanje Azure s komponentom Windows PowerShell. Radi stvaranja druge klaster alata i značajki kliknite Odaberi kartica pri vrhu stranice ovu stranicu ili pogledajte [načina stvaranja klaster](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Preduvjeti:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Prije nego počnete upute u ovom članku, morate imati sljedeće:

- Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Stvaranje klastere
Azure PowerShell je Napredna okruženje za skriptiranje koje možete koristiti za upravljanje i automatizirati implementacije i upravljanja vaše radnih opterećenja servisu Azure. U ovom se odjeljku daju upute o stvaranju programa klaster HDInsight pomoću Azure PowerShell. Informacije o konfiguriranju radne stanice da biste pokrenuli cmdleta ljuske Windows PowerShell servisa HDInsight potražite u članku [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Dodatne informacije o korištenju Azure PowerShell sa servisa HDInsight potražite u članku [administriranje HDInsight pomoću komponente PowerShell](hdinsight-administer-use-powershell.md). Na popisu cmdleta HDInsight Windows PowerShell potražite u članku [Referenca cmdlet HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).


Da biste stvorili programa klaster HDInsight pomoću Azure PowerShell su vam potrebne sljedeće postupke:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Stvaranje klastere pomoću predloška za ARM

Azure PowerShell možete koristiti za implementaciju predložak OKVIRA koji stvara sustava klaster HDInsight.  U odjeljku [pozivanje predložaka pomoću komponente PowerShell Azure](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Prilagodba klastere

- U odjeljku [Prilagodba HDInsight klastere pomoću samopokretanja programa](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- U odjeljku [klastere utemeljen na sustavu Windows za prilagođavanje HDInsight pomoću skripte akcije](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Daljnji koraci
U ovom se članku ste naučili nekoliko načina stvaranja programa klaster HDInsight. Dodatne informacije potražite u sljedećim člancima:

* [Početak rada sa servisom Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – upute za početak rada s svoj klaster HDInsight
* [Slanje Hadoop poslovi programski](hdinsight-submit-hadoop-jobs-programmatically.md) – Saznajte kako programski slanje zadataka HDInsight
* [Upravljanje Hadoop klastere u HDInsight pomoću komponente PowerShell](hdinsight-administer-use-powershell.md) – upute za rad s HDInsight pomoću komponente PowerShell Azure
* [Azure HDInsight SDK dokumentaciji]  [ hdinsight-sdk-documentation] -otkrivanje HDInsight SDK




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
