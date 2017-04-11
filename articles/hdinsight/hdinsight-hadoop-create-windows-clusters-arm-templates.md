<properties
   pageTitle="Stvaranje klastere utemeljen na sustavu Windows Hadoop u HDInsight pomoću predložaka Voditelj resursa Azure | Microsoft Azure"
    description="Saznajte kako stvoriti klastere za Azure HDInsight pomoću predložaka Azure Voditelj resursa."
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
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Stvaranje klastere utemeljen na sustavu Windows Hadoop u HDInsight pomoću predložaka Voditelj resursa za Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Saznajte kako stvoriti klastere HDInsight pomoću predložaka Azure Voditelj resursa. Dodatne informacije potražite u članku [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](../resource-group-template-deploy.md). Radi stvaranja druge klaster alata i značajki kliknite Odaberi kartica pri vrhu stranice ovu stranicu ili pogledajte [načina stvaranja klaster](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Preduvjeti:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Prije nego počnete upute u ovom članku, morate imati sljedeće:

- [Azure pretplate](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell ili Azure EŽA

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Voditelj resursa predložaka

Voditelj resursa predložak olakšava stvaranje HDInsight klastere, njihove zavisne resursi (kao što je zadani račun za pohranu) i ostali resursi (kao što su Azure SQL baze podataka da biste koristili Apache Sqoop) za svoju aplikaciju u jednom, usklađenih postupak. U predlošku, definirajte resurse koji su potrebni za aplikaciju i navedite implementacije parametara za unos vrijednosti za različite okruženja. Predložak se sastoji od JSON i izraza koji možete koristiti za sastavljanje vrijednosti za implementaciju sustava.

Voditelj resursa predloška za stvaranje programa HDInsight klaster i o njima ovisne račun za Azure prostora za pohranu pronaći ćete u [Dodatak-A](#appx-a-arm-template). Da biste spremili predložak u datoteku na vaše radne stanice, koristite uređivač teksta. Ćete naučiti da biste nazvali predložak pomoću raznih alata.

Dodatne informacije o resursima predloška potražite u članku

- [Autor Voditelj resursa Azure predložaka](../resource-group-authoring-templates.md)
- [Implementacija aplikacije pomoću predloška Azure Voditelj resursa](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Implementacija sa servisom PowerShell

Sljedeći postupak stvara sustava klaster HDInsight.

**Za implementaciju klaster pomoću predloška Voditelj resursa**

1. Spremite datoteku json u [dodatak A](#appx-a-arm-template) vaše radne stanice.
2. Ako je potrebno, postavite parametre.
3. Pokrenite predložak pomoću sljedeće skripte komponente PowerShell:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    Skriptu PowerShell samo konfigurira klaster ime i naziv računa za pohranu.  U predlošku Voditelj resursa možete postaviti druge vrijednosti. 
    
Dodatne informacije potražite u članku [uvođenja sa servisom PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Implementacija s Azure EŽA

Sljedeći primjer stvara klaster i zavisne prostora za pohranu računa i spremnik tako da nazovete predloška Voditelj resursa:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>Implementacija s REST API-JA

Potražite u članku [Implementacija s REST API-JA](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Implementacija s Visual Studio

Visual Studio, možete stvoriti grupni projekt resursa i implementacija Azure putem korisničkog sučelja. Odaberite vrstu resursa koje želite uvrstiti u projektu, a tih resursa automatski se dodaju resursima predložak. Projekt također nudi skriptu PowerShell uvođenje predloška.

Uvod u korištenje Visual Studio s grupama resursa, potražite u članku [Stvaranje i implementacija grupe Azure resursa kroz Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Daljnji koraci
U ovom se članku ste naučili nekoliko načina stvaranja programa klaster HDInsight. Dodatne informacije potražite u sljedećim člancima:


- Primjer implementacija resursi putem klijenta biblioteke .NET, potražite u članku [uvođenja resursima pomoću .NET biblioteke i predložak](../virtual-machines/virtual-machines-windows-csharp-template.md).
- Detaljnije primjera implementacija aplikacije, potražite u članku [dodjele resursa i implementacija microservices predvidljivije u Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Upute za implementaciju rješenje u različitim okruženjima, potražite u članku [razvoj i testiranje okruženja u Microsoft Azure](../solution-dev-test-environments.md).
- Da biste saznali više o sekcije predloška Azure Voditelj resursa, u odjeljku [Predlošci na dokumentima](../resource-group-authoring-templates.md).
- Popis funkcija možete koristiti u predlošku Azure Voditelj resursa, potražite u članku [funkcije predložak](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Voditelj resursa Appx o: predložak

Sljedeći predložak upravitelj resursa Azure stvara utemeljen na sustavu Windows Hadoop klaster s računom za zavisne Azure prostora za pohranu.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

