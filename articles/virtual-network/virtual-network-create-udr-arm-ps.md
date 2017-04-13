<properties 
   pageTitle="Upravljanje usmjeravanje i koristiti virtualne aparata u upravitelju resursa pomoću komponente PowerShell | Microsoft Azure"
   description="Saznajte kako odrediti usmjeravanje i koristiti virtualne aparata u upravitelju resursa pomoću komponente PowerShell"
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
   ms.date="02/23/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-resource-manager-by-using-powershell"></a>Stvaranje korisnički definirane usmjerava (UDR) u upravitelju resursa pomoću komponente PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [stvoriti UDRs u modelu klasični implementacije](virtual-network-create-udr-classic-ps.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Na temelju uzorka PowerShell naredbe ispod očekivati okruženju jednostavne već stvorili scenarij iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje okruženje za testiranje uvođenjem [Ovaj predložak](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), zatim **Implementiraj za Azure**, zamjena zadane vrijednosti parametra ako je potrebno i slijedite upute na portalu.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Stvaranje UDR za podmreži sučelje
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreže sučelje ovisno o scenariju iznad, slijedite korake u nastavku.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Stvaranje usmjeravanje koristiti da biste poslali sve promet namijenjene prikazivanju u pozadini podmreži (192.168.2.0/24) da biste usmjeriti potražite virtualne **FW1** (192.168.0.4).

        $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
            -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

4. Stvaranje tablice usmjeravanje pod nazivom **UDR sučelju** u regiji **westus** koja sadrži usmjeravanje stvorena iznad.

        $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
            -Name UDR-FrontEnd -Route $route

5. Stvaranje tjednog prikaza kalendara koji sadrži VNet gdje se nalazi podmreži. U našem scenariju na VNet pod nazivom **TestVNet**.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet

6. Povezivanje tablice usmjeravanje stvorena iznad u **sučelju** podmreži.
        
        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable

>[AZURE.WARNING] Izlaz za naredbu iznad prikazuje sadržaj za konfiguraciju objekt virtualne mreže koji postoji samo na računalu na kojem se pokreće PowerShell. Morate pokrenite cmdlet **Skup AzureVirtualNetwork** da biste spremili postavke Azure.

7. Spremite novu konfiguraciju podmreže Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Očekivani izlaz:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
                            
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                                ...,
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              },
                                ...
                            ]   

## <a name="create-the-udr-for-the-back-end-subnet"></a>Stvaranje UDR za podmreže pozadinskih
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreži pozadinskih ovisno o scenariju iznad, slijedite korake u nastavku.

1. Stvaranje usmjeravanje koristiti da biste poslali sve promet namijenjene prikazivanju u sučelje podmreži (192.168.1.0/24) da biste usmjeriti potražite virtualne **FW1** (192.168.0.4).

        $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
            -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

4. Stvaranje tablice usmjeravanje pod nazivom **UDR pozadinskog** u regiji **uswest** koja sadrži usmjeravanje stvorena iznad.

        $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
            -Name UDR-BackEnd -Route $route

5. Povezivanje tablice usmjeravanje stvorili iznad podmreži **pozadinskog** .

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
            -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable

7. Spremite novu konfiguraciju podmreži Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Očekivani izlaz:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
                            
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              ...,
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BacEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="enable-ip-forwarding-on-fw1"></a>Omogućivanje IP prosljeđivanja na FW1
Da biste omogućili IP prosljeđivanja u NIC koristi **FW1**, slijedite korake u nastavku.

1. Stvaranje varijabla koja sadrži postavke za NIC koristi FW1. U našem scenariju NIC-a pod nazivom **NICFW1**.

        $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1

2. Omogućivanje IP prosljeđivanje i spremanje postavki NIC.

        $nicfw1.EnableIPForwarding = 1
        Set-AzureRmNetworkInterface -NetworkInterface $nicfw1

    Očekivani izlaz:

        Name                 : NICFW1
        ResourceGroupName    : TestRG
        Location             : westus
        Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState    : Succeeded
        Tags                 : 
                               Name         Value                  
                               ===========  =======================
                               displayName  NetworkInterfaces - DMZ
                               
        VirtualMachine       : {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1"
                               }
        IpConfigurations     : [
                                 {
                                   "Name": "ipconfig1",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1/ipConfigurations/ipconfig1",
                                   "PrivateIpAddress": "192.168.0.4",
                                   "PrivateIpAllocationMethod": "Static",
                                   "Subnet": {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/DMZ"
                                   },
                                   "PublicIpAddress": {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1"
                                   },
                                   "LoadBalancerBackendAddressPools": [],
                                   "LoadBalancerInboundNatRules": [],
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
        DnsSettings          : {
                                 "DnsServers": [],
                                 "AppliedDnsServers": [],
                                 "InternalDnsNameLabel": null,
                                 "InternalFqdn": null
                               }
        EnableIPForwarding   : True
        NetworkSecurityGroup : null
        Primary              : True

