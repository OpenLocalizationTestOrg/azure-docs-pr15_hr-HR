<properties
   pageTitle="Kako stvoriti NSGs u Azure Voditelj resursa pomoću komponente PowerShell | Microsoft Azure"
   description="Saznajte kako stvoriti i implementirati NSGs u Azure Voditelj resursa pomoću komponente PowerShell"
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
   ms.date="02/23/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-in-resource-manager-by-using-powershell"></a>Kako stvoriti NSGs u upravitelju resursa pomoću komponente PowerShell

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [stvoriti NSGs u modelu klasični implementacije](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Na temelju uzorka PowerShell naredbe ispod očekivati okruženju jednostavne već stvorili scenarij iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje okruženje za testiranje uvođenjem [Ovaj predložak](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), zatim **Implementiraj za Azure**, zamjena zadane vrijednosti parametra ako je potrebno i slijedite upute na portalu.

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Kako stvoriti NSG za podmreže sučelje
Da biste stvorili programa NSG pod nazivom *NSG sučelju* ovisno o scenariju iznad, slijedite ove korake:

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.

2. Stvorite pravilo sigurnost dopuštanja pristupa s Interneta priključak 3389.

        $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP"
            -Access Allow -Protocol Tcp -Direction Inbound -Priority 100
            -SourceAddressPrefix Internet -SourcePortRange *
            -DestinationAddressPrefix * -DestinationPortRange 3389

3. Stvorite pravilo sigurnost dopuštanja pristupa s Interneta priključak 80.

        $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP"
            -Access Allow -Protocol Tcp -Direction Inbound -Priority 101
            -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix *
            -DestinationPortRange 80

4. Dodavanje pravila stvorena iznad da biste novi NSG pod nazivom **NSG sučelju**.

        $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus
        -Name "NSG-FrontEnd" -SecurityRules $rule1,$rule2

5. Provjera pravila stvorena u na NSG.

        $nsg

    Izlaz koji se prikazuje samo sigurnosna pravila:

        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
                                   "Description": "Allow RDP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "3389",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 100,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "web-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
                                   "Description": "Allow HTTP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "80",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 101,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

6. Povežite NSG stvorena iznad u *sučelju* podmreži.

                    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
                    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
                        -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $nsg

                Output showing only the *FrontEnd* subnet settings, notice the value for the **NetworkSecurityGroup** property:

                    Subnets           : [
                                          {
                                            "Name": "FrontEnd",
                                            "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                            "AddressPrefix": "192.168.1.0/24",
                                            "IpConfigurations": [
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                              },
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                              }
                                            ],
                                            "NetworkSecurityGroup": {
                                              "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                            },
                                            "RouteTable": null,
                                            "ProvisioningState": "Succeeded"
                                          }

    >[AZURE.WARNING] Izlaz za naredbu iznad prikazuje sadržaj za konfiguraciju objekt virtualne mreže koji postoji samo na računalu na kojem se pokreće PowerShell. Morate pokrenuti na `Set-AzureRmVirtualNetwork` cmdlet da biste spremili postavke Azure.

7. Da biste spremili nove postavke VNet za Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Izlaz koji se prikazuje samo dio NSG:

        "NetworkSecurityGroup": {
          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
        }

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Kako stvoriti NSG za podmreži pozadinskih
Da biste stvorili programa NSG pod nazivom *NSG pozadinskog* ovisno o scenariju iznad, slijedite ove korake:

1. Stvorite pravilo sigurnost dopuštanja pristupa iz podmreže sučelje priključak 1433 (zadani je priključak koristi SQL Server).

        $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name frontend-rule -Description "Allow FE subnet"
            -Access Allow -Protocol Tcp -Direction Inbound -Priority 100
            -SourceAddressPrefix 192.168.1.0/24 -SourcePortRange *
            -DestinationAddressPrefix * -DestinationPortRange 1433

2. Stvorite pravilo sigurnost blokira pristup Internetu.

        $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Block Internet"
            -Access Deny -Protocol * -Direction Outbound -Priority 200
            -SourceAddressPrefix * -SourcePortRange *
            -DestinationAddressPrefix Internet -DestinationPortRange *

3. Dodavanje pravila stvorili iznad novi NSG pod nazivom **NSG pozadinskog**.

        $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus -Name "NSG-BackEnd"
            -SecurityRules $rule1,$rule2

4. Povežite NSG stvorili iznad podmreži *pozadinskog* .

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd
            -AddressPrefix 192.168.2.0/24 -NetworkSecurityGroup $nsg

    Izlaz prikazuje samo *pozadinskog* postavke podmreži, obratite pozornost na vrijednost za svojstvo **NetworkSecurityGroup** :

        Subnets           : [
                      {
                        "Name": "BackEnd",
                        "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                        "AddressPrefix": "192.168.2.0/24",
                        "IpConfigurations": [...],
                        "NetworkSecurityGroup": {
                          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "RouteTable": null,
                        "ProvisioningState": "Succeeded"
                      }

5. Da biste spremili nove postavke VNet za Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet


## <a name="how-to-remove-an-nsg"></a>Uklanjanje programa NSG

Da biste izbrisali postojeću NSG pod nazivom *NSG sučelju* u tom slučaju slijedite koraka u nastavku:

Pokrenite **Ukloni AzureRmNetworkSecurityGroup** prikazano u nastavku i obavezno uključite grupu resursa u NSG se.

            Remove-AzureRmNetworkSecurityGroup -Name "NSG-FrontEnd" -ResourceGroupName "TestRG"
