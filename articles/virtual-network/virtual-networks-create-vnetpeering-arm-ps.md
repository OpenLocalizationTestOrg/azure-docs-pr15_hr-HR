<properties
   pageTitle="Stvaranje VNet Peering pomoću cmdleta ljuske Powershell | Microsoft Azure"
   description="Saznajte kako stvoriti virtualne mreže pomoću portala za Azure u upravitelju resursa."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
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
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Stvaranje VNet Peering pomoću cmdleta ljuske Powershell

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Da biste stvorili VNet peering pomoću komponente PowerShell, slijedite ove korake:

1. Ako još niste koristili Azure PowerShell, pročitajte članak [Instaliranje i konfiguriranje Azure PowerShell](../powershell-install-configure.md) i slijedite upute do kraja do kraja se prijaviti u Azure i odaberite svoju pretplatu.

> [AZURE.NOTE] Cmdlet ljuske PowerShell za upravljanje VNet peering se isporučuju uz [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Čitanje objekata virtualne mreže:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Da biste uspostavili VNet peering, morate stvoriti dvije veze, jedan za svaki smjer. Sljedeći korak će stvoriti VNet peering vezu za VNet1 za VNet2 najprije:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Izlaz prikazuje:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Ovaj korak će stvoriti VNet peering vezu za VNet2 za VNet1:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Izlaz prikazuje:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Nakon stvaranja VNet peering vezu, možete vidjeti stanje veze:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Izlaz prikazuje:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Postoji nekoliko konfigurirati svojstva VNet peering:

  	|Mogućnost|Opis|Zadani|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Je li adresa prostora ravnopravnih članova VNet biti dio oznake Virtual_network|Da|
  	|AllowForwardedTraffic|Hoće li promet ne potječu peered VNet prihvatili ili ispušteno|ne|
  	|AllowGatewayTransit|Omogućuje VNet da biste koristili pristupnikom VNet ravnopravnih članova|ne|
  	|UseRemoteGateways|Koristi vaš ravnopravnog VNet pristupnik. Ravnopravni VNet mora imati konfiguriran pristupnik i AllowGatewayTransit odabran. Ne možete koristiti tu mogućnost ako imate konfiguriran pristupnik|ne|

    Svaka veza VNet peering sadrži skup svojstava iznad. Ako, na primjer, postavite AllowVirtualNetworkAccess na True za VNet peering vezu VNet1 VNet2 i postavite na False VNet peering veze u drugome smjeru.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Možete pokrenuti Get-AzureRmVirtualNetworkPeering za provjeru dvostruke vrijednosti svojstva nakon promjene. Iz izlaza, vidjet ćete AllowForwardedTraffic mijenja skup TRUE nakon pokretanja iznad cmdleta.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Kada je pokrenut peering u ovom scenariju, moći da biste započeli veze iz bilo kojeg virtualnog računala sve virtualnog računala od oba VNets. Prema zadanim postavkama AllowVirtualNetworkAccess je True, a VNet peering Dodjela proper ACL-a dopustiti komunikaciju između VNets. I dalje možete primijeniti mreže sigurnosnih grupa (NSG) pravila da biste blokirali veze između određenih podmreže ili virtualnim strojevima precizno Žitne kontrole programa access između dvije virtualne mreže.  Dodatne informacije o stvaranju pravila NSG potražite u ovom [članku](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Da biste stvorili VNet peering preko pretplate pomoću komponente PowerShell, slijedite ove korake:

1. Prijavite se u Azure s ovlastima korisnika-A je račun za pretplatu-A i pokretanje sljedeći cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Ovo nije potrebna, peering možete uspostaviti čak i ako korisnici pojedinačno Potenciranje peering zahtjeve za svoje odgovarajući VNets pod uvjetom da zadovoljavaju zahtjeve. Dodavanje povlaštene korisnike na VNet kao korisnik u lokalnom VNet olakšava za postavljanje.

2. Prijavite se u Azure s računom za povlaštene korisnika-B za pretplatu B i pokrenite sljedeći cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. U korisnika-A je sesiji prijave, pokrenite cmdlet u nastavku:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. U sesiji prijave korisnika-B, pokrenite cmdlet u nastavku:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Kada je pokrenut peering, sve virtualnog računala u VNet3 trebali biste moći komunicirati s bilo kojeg virtualnog računala u VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. U ovom scenariju možete pokrenuti dolje da biste uspostavili VNet peering cmdleta ljuske PowerShell.  Morate postavite svojstvo AllowForwardedTraffic na True i povezati VNET1 HubVNet, koji omogućuje dolazni promet s izvan peering VNet adresni prostor.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Kada je pokrenut peering, možete potražite u [članku](virtual-network-create-udr-arm-ps.md) i definirati korisnički definirane usmjeravanje (UDR) za preusmjeravanje prometa VNet1 putem virtualne uređaj da biste koristili njegove mogućnosti. Kada u smjer navedete adresu sljedeći put, možete je postaviti IP adresi virtualne potražite u ravnopravnih članova VNet HubVNet. U nastavku je uzorka:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Da biste stvorili VNet peering između klasični virtualne mreže i upravljanja resursima Azure virtualne mreže u ljusci PowerShell, slijedite ove korake:

1. Pročitajte objekt virtualne mreže za **VNET1**, Voditelj resursa Azure virtualne mreže na sljedeći način:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Da biste uspostavili VNet peering u ovom scenariju, samo jednu vezu nije potrebna, posebice vezu iz **VNET1** **VNET2**. Ovaj korak zahtijeva zna ID klasični VNet resursa. Oblikovanje ID za grupu resursa izgleda ovako:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Obavezno SubscriptionID, ResourceGroupName i VirtualNetworkName zamijenite odgovarajući nazivi.

    To je moguće napraviti tako da sljedeće:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Jednom VNet peering veza se stvara, možete vidjeti stanje veza kao što je prikazano u nastavku:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Uklanjanje VNet Peering

1.  Da biste uklonili VNet peering, morate pokrenuti sljedeći cmdlet:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Kada uklonite jedne veze u VNET peering, stanja veze ravnopravnih članova upućivat će prekinuta veza. U ovom stanju, ne možete ponovno stvoriti vezu dok se ne promijeni stanje veza ravnopravnih članova pokrenuo. Preporučujemo da uklonite oba veze da biste mogli ponovno stvoriti VNet peering.
