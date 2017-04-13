<properties 
   pageTitle="Upravljanje NSGs pomoću komponente PowerShell u upravitelju resursa | Microsoft Azure"
   description="Informirajte se o upravljanju exising NSGs pomoću komponente PowerShell u upravitelju resursa"
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
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>Upravljanje NSGs pomoću komponente PowerShell

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Dohvaćanje informacija

Možete vidjeti vaše postojeće NSGs, dohvaćanje pravila za postojeće NSG i saznati resursima sustava NSG je povezan.

### <a name="view-existing-nsgs"></a>Prikaz postojeće NSGs
Da biste vidjeli sve postojeće NSGs u pretplatu, pokrenite na `Get-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku.

    Get-AzureRmNetworkSecurityGroup

Očekivani rezultat:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Da biste pogledali popis NSGs određene grupe resursa, pokrenite na `Get-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Očekivani izlaz:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Popis svih pravila za programa NSG

Da biste pogledali pravila programa NSG pod nazivom **NSG sučelju**, pokrenite na `Get-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Očekivani izlaz:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Možete koristiti `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` na popisu zadana pravila iz **NSG sučelju** NSG.

### <a name="view-nsgs-associations"></a>Prikaz NSGs pridruživanja

Da biste vidjeli što resursi NSG **NSG sučelju** se pridružiti, pokrenite na `Get-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Potražite svojstva **NetworkInterfaces** i **podmreže** kao što je prikazano u nastavku:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

U gornjem primjeru u NSG nije povezan nijedno sučelje mreže (NIC-ovi), a zatim je povezan sa podmreže pod nazivom **sučelju**.

## <a name="manage-rules"></a>Upravljanje pravilima

Možete dodati pravila za postojeće NSG uredite postojeće pravila, a uklonite pravila.

### <a name="add-a-rule"></a>Dodavanje pravila

Da biste dodali pravilo dopuštanja **dolazni** promet za priključak **443** iz bilo kojeg uređaja za **NSG FrontEnd** NSG, slijedite korake u nastavku.

1. Pokretanje u `Get-AzureRmNetworkSecurityGroup` cmdlet za dohvaćanje postojeće NSG i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Pokretanje u `Add-AzureRmNetworkSecurityRuleConfig` cmdlet, kao što je prikazano u nastavku.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Da biste spremili promjene u NSG, pokrenite na `Set-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Očekivani izlaz prikazuje samo sigurnosna pravila:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Promjena pravila

Da biste promijenili pravilo stvorio iznad da dopušta unutarnje promet s **Interneta** samo, slijedite korake u nastavku.

1. Pokretanje u `Get-AzureRmNetworkSecurityGroup` cmdlet za dohvaćanje postojeće NSG i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Pokretanje u `Set-AzureRmNetworkSecurityRuleConfig` cmdlet, kao što je prikazano u nastavku.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Da biste spremili promjene u NSG, pokrenite na `Set-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Očekivani izlaz prikazuje samo sigurnosna pravila:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Brisanje pravila

1. Pokretanje u `Get-AzureRmNetworkSecurityGroup` cmdlet za dohvaćanje postojeće NSG i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Pokretanje u `Remove-AzureRmNetworkSecurityRuleConfig` cmdlet, kao što je prikazano u nastavku.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Da biste spremili promjene u NSG, pokrenite na `Set-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Očekivani izlaz prikazuje samo pravila sigurnost, obavijest **https pravila** uklonjena s popisa:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Upravljanje pridruživanja

Možete povezati programa NSG da biste podmreže i NIC-ovi. Možete dissociate i programa NSG iz bilo kojeg resursa koji je pridružen.

### <a name="associate-an-nsg-to-a-nic"></a>Povezivanje programa NSG da biste na NIC

Da biste povezali **NSG sučelju** NSG za **TestNICWeb1** NIC, slijedite korake u nastavku.

1. Pokretanje u `Get-AzureRmNetworkSecurityGroup` cmdlet za dohvaćanje postojeće NSG i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Pokretanje u `Get-AzureRmNetworkInterface` cmdlet za dohvaćanje postojeće NIC i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Svojstvo **NetworkSecurityGroup** varijable **NIC** vrijednost varijablu **NSG** , kao što je prikazano u nastavku.

        $nic.NetworkSecurityGroup = $nsg

4. Da biste spremili promjene NIC-a, pokrenite na `Set-AzureRmNetworkInterface` cmdlet kao što je prikazano u nastavku.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Očekivani izlaz prikazuje samo **NetworkSecurityGroup** svojstvo:

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Dissociate programa NSG iz programa NIC

Da biste dissociate **NSG sučelju** NSG iz **TestNICWeb1** NIC, slijedite korake u nastavku.

1. Pokretanje u `Get-AzureRmNetworkSecurityGroup` cmdlet za dohvaćanje postojeće NSG i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Pokretanje u `Get-AzureRmNetworkInterface` cmdlet za dohvaćanje postojeće NIC i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Svojstvo **NetworkSecurityGroup** varijable **NIC** na **$null**, kao što je prikazano u nastavku.

        $nic.NetworkSecurityGroup = $null

4. Da biste spremili promjene NIC-a, pokrenite na `Set-AzureRmNetworkInterface` cmdlet kao što je prikazano u nastavku.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Očekivani izlaz prikazuje samo **NetworkSecurityGroup** svojstvo:

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Dissociate programa NSG iz podmreži

Da biste dissociate **NSG sučelju** NSG iz podmreže **sučelju** , slijedite korake u nastavku.

1. Pokretanje u `Get-AzureRmVirtualNetwork` cmdlet za dohvaćanje postojeće VNet i spremite ga u varijablu, kao što je prikazano u nastavku.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Pokretanje u `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet za dohvaćanje podmreže **sučelju** i spremite ga u varijablu, kao što je prikazano u nastavku.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Svojstvo **NetworkSecurityGroup** varijable **podmreže** na **$null**, kao što je prikazano u nastavku.

        $subnet.NetworkSecurityGroup = $null

4. Da biste spremili promjene podmreži, pokrenite na `Set-AzureRmVirtualNetwork` cmdlet kao što je prikazano u nastavku.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Prikazuje samo svojstva podmreže **sučelju** očekivanog izlaza. Obratite pozornost na to nije moguće svojstvo **NetworkSecurityGroup**:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Povezivanje programa NSG da biste podmreži

Da biste ponovno povezali **NSG sučelju** NSG podmreže **FronEnd** , slijedite korake u nastavku.

1. Pokretanje u `Get-AzureRmVirtualNetwork` cmdlet za dohvaćanje postojeće VNet i spremite ga u varijablu, kao što je prikazano u nastavku.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Pokretanje u `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet za dohvaćanje podmreži **sučelju** i spremite ga u varijablu, kao što je prikazano u nastavku.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Pokretanje u `Get-AzureRmNetworkSecurityGroup` cmdlet za dohvaćanje postojeće NSG i spremite ga u varijablu, kao što je prikazano u nastavku.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Svojstvo **NetworkSecurityGroup** varijable **podmreže** na **$null**, kao što je prikazano u nastavku.

        $subnet.NetworkSecurityGroup = $nsg

4. Da biste spremili promjene podmreži, pokrenite na `Set-AzureRmVirtualNetwork` cmdlet kao što je prikazano u nastavku.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Očekivani izlaz prikazuje samo **NetworkSecurityGroup** svojstvo podmreže **FrontEnd** :

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Brisanje programa NSG

Na NSG možete izbrisati samo ako je nije povezana za neki resurs. Da biste izbrisali programa NSG, slijedite korake u nastavku.

1. Da biste provjerili resursi povezani programa NSG, pokrenite na `azure network nsg show` kao što je prikazano u [prikazu NSGs pridruživanja](#View-NSGs-associations).
2. Ako je povezan sa sve NIC-ovi u NSG, pokrenite na `azure network nic set` kao što je prikazano u [Dissociate programa NSG iz programa NIC](#Dissociate-an-NSG-from-a-NIC) za svaku NIC. 
3. Ako je na NSG povezana u bilo kojem podmreži, pokrenite na `azure network vnet subnet set` kao što je prikazano u [Dissociate programa NSG iz podmreži](#Dissociate-an-NSG-from-a-subnet) za svaki podmreže.
4. Da biste izbrisali na NSG, pokrenite na `Remove-AzureRmNetworkSecurityGroup` cmdlet kao što je prikazano u nastavku.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] Na **– prisilno** parametar osigurava nije potrebno da biste potvrdili brisanje.

## <a name="next-steps"></a>Daljnji koraci

- [Omogućite zapisivanje](virtual-network-nsg-manage-log.md) NSGs.