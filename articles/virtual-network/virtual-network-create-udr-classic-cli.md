<properties 
   pageTitle="Kontrola usmjeravanje i korištenje virtualne aparata pomoću EŽA Azure u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako odrediti usmjeravanju u VNets pomoću EŽA Azure u modelu klasični implementacije"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Kontrola usmjeravanje i korištenje virtualnih aparata (klasični) pomoću EŽA Azure

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [kontrola usmjeravanje i korištenje virtualne aparata u modelu implementacije Voditelj resursa](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Ispod primjere Azure EŽA naredbi očekivati okruženju jednostavne već stvorili ovisno o scenariju iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, stvorite okruženje prikazano [Stvaranje VNet koji će biti (klasični) pomoću EŽA Azure](virtual-networks-create-vnet-classic-cli.md).

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Stvaranje UDR za podmreže sučelje
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreže sučelje ovisno o scenariju iznad, slijedite korake u nastavku.

1. Pokretanje u **`azure config mode`** da biste prešli na klasičan način rada.

        azure config mode asm

    Izlaz:

        info:    New mode is asm

3. Pokretanje u **`azure network route-table create`** naredbu da biste stvorili tablicu usmjeravanje za podmreže sučelje.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Izlaz:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parametri:
    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti novi NSG. Za naše scenarij *westus*.
    - **-n (ili – naziv)**. Naziv za novu NSG. Za naše scenarij *NSG sučelju*.

4. Pokretanje u **`azure network route-table route set`** naredbu da biste stvorili rutu usmjeravanje tablici koja je stvorena iznad da biste poslali sve promet namijenjene prikazivanju u pozadini podmreži (192.168.2.0/24) da biste **FW1** VM (192.168.0.4).

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Izlaz:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parametri:
    - **-r (ili – usmjeravanje, tablice i ime)**. Naziv tablice usmjeravanje gdje će se dodati smjer. Za naše scenarij *UDR sučelju*.
    - **-(ili – adresa prefiks)**. Adresa prefiks podmreže kojima su namijenjene pakete prikazivanju da biste. Za naše scenarij *192.168.2.0/24*.
    - **-t (ili – dalje skoka vrsta)**. Vrsta objekta promet poslat će se. Moguće vrijednosti su *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*ili *ništa*.
    - **-p (ili – dalje-skoka-ip-adresu**). IP adresa za sljedeći put. Za naše scenarij *192.168.0.4*.

5. Pokretanje u **`azure network vnet subnet route-table add`** naredba povezivati usmjeravanje tablice iznad stvorenih podmreže **sučelju** .

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Izlaz:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parametri:
    - **-t (ili – vnet naziv)**. Naziv VNet gdje se nalazi podmreži. Za naše scenarij *TestVNet*.
    - **- n (ili – podmreže naziv**. Naziv podmreže usmjeravanje tablice dodat će se. Za naše scenarij *sučelju*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Stvaranje UDR za podmreže pozadinskih
Da biste stvorili tablicu usmjeravanje i usmjeravanje koja su potrebna za podmreže pozadinskih ovisno o scenariju iznad, slijedite korake u nastavku.

3. Pokretanje u **`azure network route-table create`** naredbu da biste stvorili tablicu usmjeravanje za podmreže pozadinskih.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Pokretanje u **`azure network route-table route set`** naredbu da biste stvorili rutu usmjeravanje tablici koja je stvorena iznad da biste poslali sve promet namijenjene prikazivanju u sučelje podmreži (192.168.1.0/24) da biste **FW1** VM (192.168.0.4).

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Pokretanje u **`azure network vnet subnet route-table add`** naredbu za pridruživanje tablice usmjeravanje stvorena iznad s **pozadinskom** podmreže.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

