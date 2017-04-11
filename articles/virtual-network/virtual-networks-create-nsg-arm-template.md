<properties
   pageTitle="Kako stvoriti NSGs u načinu ARM pomoću predloška | Microsoft Azure"
   description="Saznajte kako stvoriti i implementirati NSGs u OBLAK pomoću predloška"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-using-a-template"></a>Kako stvoriti NSGs pomoću predloška

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [stvoriti NSGs u modelu klasični implementacije](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>NSG resursa u datoteku predloška

Možete pogledati i preuzimanje [oglednog predloška](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).

Odjeljak u nastavku prikazuje definicije sučelje NSG, ovisno o scenariju iznad.

      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('frontEndNSGName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "NSG - Front End"
      },
      "properties": {
        "securityRules": [
          {
            "name": "rdp-rule",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 100,
              "direction": "Inbound"
            }
          },
          {
            "name": "web-rule",
            "properties": {
              "description": "Allow WEB",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "80",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 101,
              "direction": "Inbound"
            }
          }
        ]
      }

Da biste povezali NSG u sučelje podmreži, morate promijeniti definiciju podmreži u predlošku i koristiti reference id za na NSG.

        "subnets": [
          {
            "name": "[parameters('frontEndSubnetName')]",
            "properties": {
              "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
              "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
              }
            }
          }, ...

Obratite pozornost na to isto se gotovo pozadinskih NSG i podmreži pozadinskih u predlošku.

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Uvođenje predloška ARM pomoću kliknite radi implementacije

Predložak uzorak u spremištu javno koristi parametar datoteku koja sadrži zadane vrijednosti za generiranje scenarij prethodno opisan. Da biste implementirali ovaj predložak pomoću kliknite implementacija, slijedite [ove veze](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), zatim **Implementiraj za Azure**, zamijenite zadane vrijednosti parametra ako je potrebno i slijedite upute na portalu.

## <a name="deploy-the-arm-template-by-using-powershell"></a>Uvođenje predloška ARM pomoću komponente PowerShell

Da biste implementirali ARM predložak koji ste preuzeli pomoću komponente PowerShell, slijedite korake u nastavku.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.

3. Pokretanje u **`New-AzureRmResourceGroup`** cmdlet da biste stvorili grupu resursa pomoću ovog predloška.

        New-AzureRmResourceGroup -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Očekivani izlaz:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Uvođenje predloška ARM pomoću EŽA Azure

Uvođenje predloška ARM pomoću EŽA Azure, slijedite korake u nastavku.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.
2. Pokretanje u **`azure config mode`** naredbu da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Evo očekivanog izlaza iznad naredbe:

        info:    New mode is arm

4. Pokretanje u **`azure group deployment create`** cmdleta za implementaciju novi VNet pomoću predloška i parametar datoteke možete preuzeti i izmijeniti iznad. Popis nakon izlaz objašnjava parametri korišteni.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Očekivani izlaz:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

    - **-n (ili – naziv)**. Naziv grupe resursa koji će biti stvoren.
    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti grupu resursa.
    - **-f (ili – datoteku predloška)**. Put do OKVIRA datoteku predloška.
    - **-e (ili – parametara datoteka)**. Put do datoteke parametara OKVIRA.
