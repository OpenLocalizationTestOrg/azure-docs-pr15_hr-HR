<properties
   pageTitle="Kako stvoriti NSGs u klasičan način rada pomoću EŽA Azure | Microsoft Azure"
   description="Saznajte kako stvoriti i implementirati NSGs u klasičan način rada pomoću EŽA Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Kako stvoriti NSGs (klasični) u EŽA Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [stvoriti NSGs u modelu implementacije Voditelj resursa](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Ispod primjere Azure EŽA naredbi očekivan okruženju jednostavne već stvorili ovisno o scenariju iznad. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje okruženje za testiranje tako da [stvorite na VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Kako stvoriti NSG za podmreže sučelje
Da biste stvorili programa NSG pod nazivom imenovani **NSG sučelju** ovisno o scenariju iznad, slijedite korake u nastavku.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.

2. Pokretanje u **`azure config mode`** naredbu da biste prešli na klasičan način rada, kao što je prikazano u nastavku.

        azure config mode asm

    Očekivani izlaz:

        info:    New mode is asm

3. Pokretanje u **`azure network nsg create`** naredbu da biste stvorili programa NSG.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Očekivani izlaz:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametri:

    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti novi NSG. Za naše scenarij *westus*.
    - **-n (ili – naziv)**. Naziv za novu NSG. Za naše scenarij *NSG sučelju*.

4. Pokretanje u **`azure network nsg rule create`** naredbu da biste stvorili pravilo koje omogućuje pristup priključak 3389 (RDP) s Interneta.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Očekivani izlaz:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parametri:

    - **-(ili – nsg naziv)**. Naziv NSG u kojem će se stvoriti pravilo. Za naše scenarij *NSG sučelju*.
    - **-n (ili – naziv)**. Naziv za novo pravilo. Za naše scenarij *rdp pravilo*.
    - **-c (ili – akcije)**. Razina pristupa za pravilo (Nemoj dopustiti ili Dopusti).
    - **-p (ili – protocol)**. Protocol (Tcp, Udp, i *) za pravilo.
    - **-r (ili – vrsta)**. Smjer veze (ulazni i izlazni).
    - **-y (ili – prioritet)**. Prioritet pravila.
    - **-f (ili – izvora, adresa i prefiks)**. Prefiks adresu izvora u CIDR ili pomoću zadane oznake.
    - **-o (ili – izvorišnog priključka raspona)**. Izvorni Priključak ili raspon priključka.
    - **-e (ili – odredište, adresa i prefiks)**. Prefiks odredišnu adresu u CIDR ili pomoću zadane oznake.
    - **-u (ili – odredišni priključak raspon)**. Odredišni priključak ili raspon priključka.

5. Pokretanje u **`azure network nsg rule create`** naredbu da biste stvorili pravilo koje omogućuje pristup priključka 80 (HTTP) s Interneta.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Očekivani putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Pokretanje u **`azure network nsg subnet add`** naredbu da biste se povezali s NSG podmreži sučelje.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Očekivani izlaz:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Kako stvoriti NSG za podmreži pozadinskih
Da biste stvorili programa NSG pod nazivom imenovani *NSG pozadinskog* ovisno o scenariju iznad, slijedite korake u nastavku.

3. Pokretanje u **`azure network nsg create`** naredbu da biste stvorili programa NSG.

        azure network nsg create -l uswest -n NSG-BackEnd

    Očekivani izlaz:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametri:

    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti novi NSG. Za naše scenarij *westus*.
    - **-n (ili – naziv)**. Naziv za novu NSG. Za naše scenarij *NSG sučelju*.

4. Pokretanje u **`azure network nsg rule create`** naredbu da biste stvorili pravilo koje omogućuje pristup priključak 1433 (SQL) iz podmreže sučelje.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Očekivani izlaz:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Pokretanje u **`azure network nsg rule create`** naredbu da biste stvorili pravilo koje onemogućuje se pristup Internetu.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Očekivani putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Pokretanje u **`azure network nsg subnet add`** naredbu da biste se povezali s NSG podmreže pozadinskih.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Očekivani izlaz:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
