## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Kako stvoriti VNet pomoću EŽA Azure

Azure EŽA možete koristiti da biste upravljali Azure resurse iz naredbenog retka bilo kojem računalu s operacijskim sustavom Windows, Linux ili OSX. Da biste stvorili na VNet pomoću EŽA Azure, slijedite korake u nastavku.

1. Ako još niste koristili EŽA Azure, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.
2. Pokrenite naredbu **azure config način** da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Evo očekivanog izlaza iznad naredbe:

        info:    New mode is arm

3. Ako je potrebno, pokrenite na **Stvaranje azure grupe** da biste stvorili novu grupu resursa, kao što je prikazano u nastavku. Obratite pozornost na rezultatu naredbe. Popis nakon izlaz objašnjava koristi parametre. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Evo očekivanog izlaza iznad naredbe:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (ili – naziv)**. Naziv za novu grupu resursa. Za naše scenarij *TestRG*.
    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti novu grupu resursa. Za naše scenarij *centralus*.

4. Pokrenite naredbu **Stvaranje vnet azure mreže** da biste stvorili na VNet i podmreži, kao što je prikazano u nastavku. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Evo očekivanog izlaza iznad naredbe:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (ili – grupa resursa)**. Naziv grupe resursa u kojem će se stvoriti u VNet. Za naše scenarij *TestRG*.
    - **-n (ili – naziv)**. Naziv VNet će biti stvoren. Za naše scenarij *TestVNet*
    - **-(ili – adresa prefiksi)**. Popis blokova CIDR koristi za adresni prostor VNet. Za naše scenarij *192.168.0.0/16*
    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti u VNet. Za naše scenarij *centralus*.

5. Pokrenite naredbu **Stvaranje podmreži vnet azure mreže** da biste stvorili podmreži kao što je prikazano u nastavku. Obratite pozornost na rezultatu naredbe. Popis nakon izlaz objašnjava koristi parametre.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Evo očekivanog izlaza iznad naredbe:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (--vnet naziv ili**. Naziv VNet u kojem će se stvoriti podmreži. Za naše scenarij *TestVNet*.
    - **-n (ili – naziv)**. Naziv nove podmreži. Za naše scenarij *sučelju*.
    - **-(ili – adresa prefiks)**. Blokiranje CIDR podmreže. Četiri naš scenarij, *192.168.1.0/24*.

6. Ako je potrebno, ponovite korak 5 iznad da biste stvorili druge podmreže. Za naše scenarij pokrenite naredbu dolje da biste stvorili podmreže *pozadinskog* .

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Pokrenite naredbe **Prikaži vnet azure mreže** da biste pogledali svojstva novi vnet kao što je prikazano u nastavku.

        azure network vnet show -g TestRG -n TestVNet

    Evo očekivanog izlaza iznad naredbe:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
