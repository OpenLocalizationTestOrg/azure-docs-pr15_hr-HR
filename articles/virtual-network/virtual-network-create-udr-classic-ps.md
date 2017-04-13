<properties 
   pageTitle="Kontrola usmjeravanje i korištenje virtualne aparata pomoću komponente PowerShell u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako odrediti usmjeravanju u VNets pomoću komponente PowerShell u modelu klasični implementacije"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Kontrola usmjeravanje i korištenje virtualnih aparata (klasični) pomoću komponente PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Uzorak Azure PowerShell naredbe ispod očekivati okruženju jednostavne već stvorene na temelju scenarij iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, stvorite u okruženju prikazano [Stvaranje VNet koji će biti (klasični) pomoću komponente PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Stvaranje UDR za podmreže sučelje
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreže sučelje ovisno o scenariju iznad, slijedite korake u nastavku.

3. Pokretanje u **`New-AzureRouteTable`** cmdlet da biste stvorili tablicu usmjeravanje za podmreže sučelje.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Izlaz:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Pokretanje u **`Set-AzureRoute`** cmdlet da biste stvorili rutu u tablici usmjeravanje stvorena iznad da biste poslali sve promet namijenjene prikazivanju u pozadini podmreži (192.168.2.0/24) da biste **FW1** VM (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Izlaz:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Pokretanje u **`Set-AzureSubnetRouteTable`** cmdlet za pridruživanje tablice usmjeravanje stvorenih iznad podmreže **sučelju** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Stvaranje UDR za podmreže pozadinskih
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreže pozadinskih ovisno o scenariju iznad, slijedite korake u nastavku.

3. Pokretanje u **`New-AzureRouteTable`** cmdlet da biste stvorili tablicu usmjeravanje za podmreže pozadinskih.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Pokretanje u **`Set-AzureRoute`** cmdlet da biste stvorili rutu u tablici usmjeravanje stvorena iznad da biste poslali sve promet namijenjene prikazivanju u sučelje podmreži (192.168.1.0/24) da biste **FW1** VM (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Pokretanje u **`Set-AzureSubnetRouteTable`** cmdlet za pridruživanje tablice usmjeravanje stvorena iznad s **pozadinskom** podmreže.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Omogućivanje IP prosljeđivanja na FW1 VM
Da biste omogućili IP prosljeđivanja u FW1 VM, slijedite korake u nastavku.

1. Pokretanje u **`Get-AzureIPForwarding`** cmdlet da provjeri status prosljeđivanje IP

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Izlaz:

        Disabled

2. Pokretanje u **`Set-AzureIPForwarding`** naredbu da biste omogućili IP prosljeđivanja za *FW1* VM.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
