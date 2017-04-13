<properties 
   pageTitle="Upravljanje usmjeravanje i korištenje virtualne aparata u Voditelj resursa pomoću EŽA Azure | Microsoft Azure"
   description="Saznajte kako odrediti usmjeravanje i korištenje virtualnih aparata pomoću EŽA Azure"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Stvorite korisnički definirane usmjerava (UDR) u Azure EŽA

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [stvoriti UDRs u modelu klasični implementacije](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Ispod primjere Azure EŽA naredbi očekivati okruženju jednostavne već stvorili ovisno o scenariju iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje okruženje za testiranje uvođenjem [Ovaj predložak](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), zatim **Implementiraj za Azure**, zamjena zadane vrijednosti parametra ako je potrebno i slijedite upute na portalu.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Stvaranje UDR za podmreže sučelje
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreže sučelje ovisno o scenariju iznad, slijedite korake u nastavku.

3. Pokretanje u **`azure network route-table create`** naredbu da biste stvorili tablicu usmjeravanje za podmreže sučelje.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Izlaz:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parametri:
    - **-g (ili – grupa resursa)**. Naziv grupe resursa u kojem će se stvoriti u UDR. Za naše scenarij *TestRG*.
    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti novi UDR. Za naše scenarij *westus*.
    - **-n (ili – naziv)**. Naziv za novu UDR. Za naše scenarij *UDR sučelju*.

4. Pokretanje u **`azure network route-table route create`** naredbu da biste stvorili rutu usmjeravanje tablici koja je stvorena iznad da biste poslali sve promet namijenjene prikazivanju u pozadini podmreži (192.168.2.0/24) da biste **FW1** VM (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Izlaz:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parametri:
    - **-r (ili – usmjeravanje, tablice i ime)**. Naziv tablice usmjeravanje gdje će se dodati smjer. Za naše scenarij *UDR sučelju*.
    - **-(ili – adresa prefiks)**. Adresa prefiks podmreže kojima su namijenjene pakete prikazivanju da biste. Za naše scenarij *192.168.2.0/24*.
    - **-y (ili – dalje skoka vrsta)**. Vrsta objekta promet poslat će se. Moguće vrijednosti su *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*ili *ništa*.
    - **-p (ili – dalje-skoka-ip-adresu**). IP adresa za sljedeći put. Za naše scenarij *192.168.0.4*.

5. Pokretanje u **`azure network vnet subnet set`** naredba povezivati usmjeravanje tablice iznad stvorenih podmreže **sučelju** .

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Izlaz:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parametri:
    - **-e (ili – vnet naziv)**. Naziv VNet gdje se nalazi podmreži. Za naše scenarij *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Stvaranje UDR za podmreže pozadinskih
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreže pozadinskih ovisno o scenariju iznad, slijedite korake u nastavku.

1. Pokretanje u **`azure network route-table create`** naredbu da biste stvorili tablicu usmjeravanje za podmreže pozadinskih.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Pokretanje u **`azure network route-table route create`** naredbu da biste stvorili rutu usmjeravanje tablici koja je stvorena iznad da biste poslali sve promet namijenjene prikazivanju u sučelje podmreži (192.168.1.0/24) da biste **FW1** VM (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Pokretanje u **`azure network vnet subnet set`** naredbu za pridruživanje tablice usmjeravanje stvorena iznad s **pozadinskom** podmreže.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>Omogućivanje IP prosljeđivanja na FW1
Da biste omogućili IP prosljeđivanja u NIC koristi **FW1**, slijedite korake u nastavku.

1. Pokretanje u **`azure network nic show`** naredbu i obratite pozornost na to vrijednost za **Omogućivanje IP prosljeđivanje**. Mora biti postavljena na *false*.

        azure network nic show -g TestRG -n NICFW1

    Izlaz:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Pokretanje u **`azure network nic set`** naredbu da biste omogućili IP prosljeđivanje.

        azure network nic set -g TestRG -n NICFW1 -f true

    Izlaz:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parametri:

    - **-f (ili – Omogući ip prosljeđivanje)**. *True* ili *false*.
