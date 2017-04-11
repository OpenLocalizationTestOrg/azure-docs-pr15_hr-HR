<properties
   pageTitle="Stvaranje klastere sustavom Linux Hadoop u HDInsight pomoću predložaka Voditelj resursa Azure | Microsoft Azure"
    description="Saznajte kako stvoriti klastere za Azure HDInsight pomoću upravitelja resursa Azure Azure predložaka."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-linux-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Stvaranje klastere sustavom Linux Hadoop u HDInsight pomoću predložaka Voditelj resursa za Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Saznajte kako stvoriti klastere HDInsight pomoću predložaka Manager(ARM) Azure resursa. Dodatne informacije potražite u članku [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](../resource-group-template-deploy.md). Radi stvaranja druge klaster alata i značajki kliknite Odaberi kartica pri vrhu stranice ovu stranicu ili pogledajte [načina stvaranja klaster](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Preduvjeti:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Prije nego počnete upute u ovom članku, morate imati sljedeće:

- [Azure pretplate](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell i/ili Azure EŽA

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Voditelj resursa predložaka

Voditelj resursa predložak olakšava stvaranje HDInsight klastere, njihove zavisne resursi (kao što je zadani račun za pohranu) i ostali resursi (kao što su Azure SQL baze podataka da biste koristili Apache Sqoop) za svoju aplikaciju u jednom, usklađenih postupak. U predlošku, definirajte resurse koji su potrebni za aplikaciju i navedite implementacije parametara za unos vrijednosti za različite okruženja. Predložak se sastoji od JSON i izraza koji možete koristiti za sastavljanje vrijednosti za implementaciju sustava.

Voditelj resursa predloška za stvaranje programa HDInsight klaster i o njima ovisne račun za Azure prostora za pohranu pronaći ćete u [Dodatak-A](#appx-a-arm-template). Koristiti različite platforme [VSCode](https://code.visualstudio.com/#alt-downloads) s [resursima datotečnih nastavaka](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) ili uređivaču teksta da biste spremili predložak u datoteku na vaše radne stanice. Ćete naučiti da biste nazvali predloška na različite načine.

Dodatne informacije o resursima predloška potražite u članku

- [Autor Voditelj resursa Azure predložaka](../resource-group-authoring-templates.md)
- [Implementacija aplikacije pomoću predloška Azure Voditelj resursa](../resource-group-template-deploy.md)

Da biste saznali JSON sheme za određene elemente, slijedite postupak u nastavku:

1. Otvorite [portal za Azure](https://porta.azure.com) da biste stvorili programa klaster HDInsight.  Potražite u članku [Stvaranje Linux sustavom klastere u HDInsight pomoću portala za Azure](hdinsight-hadoop-create-linux-clusters-portal.md).
2. Konfiguriranje obaveznih elemenata i elemente morate shemi JSON.
3. Prije nego što kliknete **Stvori**, kliknite **Mogućnosti Automatizacija** kao što je prikazano u sljedećim snimku zaslona:

    ![HDInsight Hadoop klaster resursima predložak sheme Automatizacija mogućnosti stvaranja](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-automation-option.png)

    Na portalu stvara predložak Voditelj resursa koji se temelji na vašem konfiguracije.
## <a name="deploy-with-powershell"></a>Implementacija sa servisom PowerShell

Sljedeći postupak stvara klaster sustavom Linux HDInsight.

**Za implementaciju klaster pomoću predloška Voditelj resursa**

1. Spremite datoteku json u [dodatak A](#appx-a-arm-template) vaše radne stanice. U skriptu PowerShell *C:\HDITutorials-ARM\hdinsight-arm-template.json*je naziv datoteke.
2. Postavljanje parametara i varijable prema potrebi.
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
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    Skriptu PowerShell samo konfigurira klaster naziv. Naziv računa spremišta je koji nisu u predlošku. Koje će se zatražiti da unesete lozinku klaster korisnika (korisničko ime zadani je *administrator*); i lozinke korisničkog SSH (korisničko ime za SSH zadani je *sshuser*).  
    
Dodatne informacije potražite u članku [uvođenja sa servisom PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Implementacija s Azure EŽA

Sljedeći primjer stvara klaster i zavisne prostora za pohranu računa i spremnik tako da nazovete predloška Voditelj resursa:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"
    
Zatražit će se da biste unijeli naziv klaster, klaster korisničke lozinke (korisničko ime zadani je *administrator*) i lozinke korisničkog SSH (korisničko ime za SSH zadani je *sshuser*). Možete unijeti parametara u redak:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

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

Sljedeći predložak upravitelj resursa Azure stvara Linux temelje Hadoop klaster s računom za zavisne Azure prostora za pohranu. 

> [AZURE.NOTE] Uzorak sadrži informacije o konfiguraciji grozd metastore i Oozie metastore.  Uklanjanje sekcije ili konfigurirajte odjeljku prije korištenja predloška.

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
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
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
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
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
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
