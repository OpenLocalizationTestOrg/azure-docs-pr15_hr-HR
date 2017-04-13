<properties
   pageTitle="Implementacija VM pomoću statičke IP javno pomoću predloška u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako implementirati VMs pomoću statičke IP javno pomoću predloška u upravitelju resursa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Implementacija VM pomoću statičke IP javno pomoću predloška

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Javnu IP resursa u datoteku predloška

Možete pogledati i preuzimanje [oglednog predloška](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Odjeljak u nastavku prikazuje definicije javnu IP resursa, ovisno o scenariju iznad.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Obratite pozornost na to svojstvo **publicIPAllocationMethod** koji je u *statični*. Ovo svojstvo može biti *dinamički* (Zadana vrijednost) ili *statične*. Postavka za statične jamstva koja javnu IP adresu dodijeljeni promijenit će se nikad ne.

Odjeljak u nastavku prikazuje vezu na javnu IP adresu s mrežnog sučelja.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Obratite pozornost na to svojstvo **publicIPAddress** koja pokazuje na **Id** resursa pod nazivom **variables('webVMSetting').pipName**. To je naziv javnu IP resursa gore navedenoj sintaksi.

Na kraju, iznad mrežno sučelje nalazi u svojstvu **networkProfile** VM stvoren.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Uvođenje predloška pritiskom za implementaciju

Predložak uzorak u spremištu javno koristi parametar datoteku koja sadrži zadane vrijednosti za generiranje scenarij prethodno opisan. Da biste implementirali ovaj predložak pomoću kliknite za implementaciju, kliknite **uvođenja za Azure** u datoteci Readme.md predloška [VM s statične TOČAKA](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) . Zamjena zadane vrijednosti za parametar po želji, a zatim unesite vrijednosti za parametre prazno.  Slijedite upute na portalu za stvaranje virtualnog računala s statičke javnu IP adrese.

## <a name="deploy-the-template-by-using-powershell"></a>Uvođenje predloška pomoću komponente PowerShell

Da biste implementirali predložak koji ste preuzeli pomoću komponente PowerShell, slijedite korake u nastavku.

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../powershell-install-configure.md) i slijedite upute u koracima od 1 do 3.

2. U konzole za PowerShell pokrenite cmdlet **Novo AzureRmResourceGroup** da biste stvorili novu grupu resursa, ako je potrebno. Ako već imate grupu resursa stvorili, idite na 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Očekivani izlaz:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. U konzole za PowerShell pokrenite cmdlet **Novo AzureRmResourceGroupDeployment** uvođenje predloška.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Očekivani izlaz:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Uvođenje predloška pomoću EŽA Azure

Da biste pomoću EŽA Azure uvođenje predloška, slijedite korake u nastavku.

1. Ako još niste koristili Azure EŽA, slijedite korake u članku [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md) , a zatim korake za povezivanje s EŽA vašoj pretplati u odjeljku "Koristi azure prijava za provjeru autentičnosti interaktivno" u članku [Povezivanje s Azure pretplate s Azure sučelja naredbenog retka (Azure EŽA)](../xplat-cli-connect.md) .
2. Pokrenite naredbu **azure config način** da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Evo očekivanog izlaza iznad naredbe:

        info:    New mode is arm

3. Otvorite [datoteku parametar](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json)odaberite njezin sadržaj, a spremite datoteku na računalu. U ovom primjeru parametre spremaju se u datoteku pod nazivom *parameters.json*. Promijeniti vrijednosti parametara unutar datoteke prema želji, ali najmanje, preporučuje se promijenite vrijednost za parametar adminPassword jedinstveni, složene lozinku.

4. Pokrenite cmdlet **Stvaranje implementacije azure grupe** za implementaciju novi VNet pomoću datoteke predloška i parametar preuzeti i izmijeniti iznad. U nastavku naredba zamijenite <path> putom koje ste spremili datoteku. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Očekivani izlaz (popisa vrijednosti parametara koristi):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
