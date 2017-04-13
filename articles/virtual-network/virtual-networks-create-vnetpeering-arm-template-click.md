<properties
   pageTitle="Stvaranje VNet Peering pomoću predložaka resursima | Microsoft Azure"
   description="Saznajte kako stvoriti na virtualne mreže peering pomoću predložaka u upravitelju resursa."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Stvaranje VNet Peering pomoću predložaka Voditelj resursa

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Da biste stvorili VNet peering pomoću predložaka Voditelj resursa, slijedite ove korake:

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.

    > [AZURE.NOTE] Cmdlet ljuske PowerShell za upravljanje VNet peering se isporučuju uz [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Tekst u nastavku prikazuje definiciju VNet peering veze za VNet1 do VNet2, ovisno o scenariju iznad. Kopiranje sadržaja u nastavku i spremite ga u datoteku pod nazivom VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Odjeljak u nastavku prikazuje definiciju VNet peering veze za VNet2 do VNet1, ovisno o scenariju iznad.  Kopiranje sadržaja u nastavku i spremite ga u datoteku pod nazivom VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Kao što se vidi u predlošku iznad, postoji nekoliko konfigurirati svojstva VNet peering:

  	|Mogućnost|Opis|Zadani|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Hoće li je prostor adresu ravnopravnih članova VNet dio oznake virtual_network.|Da|
  	|AllowForwardedTraffic|Je li prihvaćen ili ispušteno promet ne potječu peered VNet.|ne|
  	|AllowGatewayTransit|Omogućuje ravnopravnih članova VNet da biste koristili pristupnikom VNet.|ne|
  	|UseRemoteGateways|Koristi vaš ravnopravnog VNet pristupnik. Ravnopravni VNet mora imati konfiguriran pristupnik i AllowGatewayTransit odabran. Ne možete koristiti tu mogućnost ako imate konfiguriran pristupnik.|ne|

    Svaka veza VNet peering sadrži skup svojstava iznad. Ako, na primjer, postavite AllowVirtualNetworkAccess na True za VNet peering vezu VNet1 VNet2 i postavite na False VNet peering veze u drugome smjeru.


4. Da biste implementirali datoteku predloška, možete pokrenuti cmdleta New AzureRmResourceGroupDeployment za stvaranje ili ažuriranje uvođenje. Dodatne informacije o korištenju predložaka resursima potražite u ovom [članku](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Zamijenite grupu naziva i datoteke resursa po potrebi.

    Ispod se primjer temelji na iznad scenarija:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Izlaz prikazuje:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Izlaz prikazuje:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Nakon dovršetka implementacijskih možete pokrenuti da biste vidjeli peering stanje cmdlet:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Izlaz prikazuje:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Kada je pokrenut peering u ovom scenariju, moći da biste započeli veze iz bilo kojeg virtualnog računala da biste sve virtualnog računala u oba VNets. Prema zadanim postavkama AllowVirtualNetworkAccess je True, a VNet peering Dodjela proper ACL-a dopustiti komunikaciju između VNets. I dalje možete primijeniti mreže sigurnosnih grupa (NSG) pravila da biste blokirali veze između određenih podmreže ili virtualnim strojevima precizno Žitne kontrole programa access između dvije virtualne mreže.  Dodatne informacije o stvaranju pravila NSG potražite u ovom [članku](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Da biste stvorili VNet peering preko pretplate, slijedite ove korake:

1. Prijavite se u Azure s ovlastima korisnika-A je račun u pretplatu-A i pokrenite sljedeći cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Ovo nije potrebna, peering možete uspostaviti čak i ako korisnici pojedinačno Potenciranje peering zahtjeve za svoje odgovarajući Vnets pod uvjetom da zadovoljavaju zahtjeve. Dodavanje povlaštene korisnike na VNet kao korisnike u lokalnom VNet olakšava za postavljanje.

2. Prijavite se u Azure s računom za povlaštene korisnika-B za pretplatu B i pokrenite sljedeći cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. U korisnika-A je sesiji prijave, pokrenite ovaj cmdlet:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Evo kako je definirano JSON datoteku.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. U sesiji prijave korisnika-B, pokrenite sljedeći cmdlet:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Evo kako je definirano JSON datoteke:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Kada je pokrenut peering u ovom scenariju, moći da biste započeli veze iz bilo kojeg virtualnog računala sve virtualnog računala od oba VNets preko različitih pretplate.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. U ovom scenariju možete implementirati primjer predloška u nastavku da biste uspostavili VNet peering.  Morat ćete postaviti svojstvo AllowForwardedTraffic na True, koji omogućuje potražite virtualne mreže u peered VNet za slanje i primanje promet.

    Evo predloška za stvaranje VNet peering iz HubVNet za VNet1. Imajte na umu da je AllowForwardedTraffic postavljen na false.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Evo predloška za stvaranje VNet peering iz VNet1 za HubVnet. Imajte na umu da je AllowForwardedTraffic postavljen na true.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Kada je pokrenut peering, možete se referirati na ovaj [članak](virtual-network-create-udr-arm-ps.md) da biste definirali korisnički definirane routes(UDR) za preusmjeravanje prometa VNet1 putem virtualne uređaj da biste koristili njegove mogućnosti. Kada u usmjeravanje navedete adresu sljedeći put, možete je postaviti IP adresi virtualne potražite u ravnopravnih članova VNet HubVNet.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Da biste stvorili peering između virtualne mreže iz modela različite implementaciju, slijedite ove korake:
1. Tekst u nastavku prikazuje definiciju VNet peering veze za VNET1 VNET2 u ovom scenariju. Za ravnopravni klasični virtualne mreži Azure resursa virtualne mreže manager potreban je samo jednu vezu.

    Provjerite jeste li postavili identifikacijskog Broja za pretplatu za gdje se nalazi klasični virtualne mreže ili VNET2 i promijenite MyResouceGroup naziv grupe odgovarajuće resursa.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parametara": {}, "varijable": {}, "resursi": [{"apiVersion": "2016-06-01", "Vrsta": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "naziv": "VNET1 na LinkToVNET2", "mjesto": "[resourceGroup () .location]", "Svojstva": {"allowVirtualNetworkAccess": true, "allowForwardedTraffic": false, "allowGatewayTransit": false, "useRemoteGateways": false, "remoteVirtualNetwork": {"id": "[resourceId (' Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]"}}}]}

2. Da biste implementirali datoteku predloška, pokrenite sljedeći cmdlet za stvaranje ili ažuriranje uvođenje.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Nakon uspješnog implementacijskih možete pokrenuti sljedeći cmdlet da biste pogledali peering stanja:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Kada je pokrenut peering između klasični VNet i Voditelj resursa VNet, moći da biste započeli veze iz bilo kojeg virtualnog računala u VNET1 s bilo kojeg virtualnog računala u VNET2 i obrnuto.
