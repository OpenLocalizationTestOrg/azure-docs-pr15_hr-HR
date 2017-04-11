## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Kako stvoriti klasični VNet pomoću EŽA Azure

Azure EŽA možete koristiti da biste upravljali Azure resursa iz naredbenog retka bilo kojem računalu s operacijskim sustavom Windows, Linux ili OSX. Da biste stvorili na VNet pomoću EŽA Azure, slijedite korake u nastavku.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.
2. Pokrenite naredbu **Stvaranje vnet azure mreže** da biste stvorili na VNet i podmreži, kao što je prikazano u nastavku. Popis nakon izlaz objašnjava parametri korišteni.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Očekivani izlaz:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **– vnet**. Naziv VNet će biti stvoren. Za naše scenarij *TestVNet*
    - **-e (ili – prostor adrese)**. VNet adresni prostor. Za naše scenarij *192.168.0.0*
    - **-i (ili - cidr)**. Mrežna maska u obliku CIDR. Za naše scenarij *16*.
    - **- n (ili – podmreži naziv**). Naziv prve podmreže. Za naše scenarij *sučelju*.
    - **-p (ili – podmreže start ip)**. Pokretanje IP adresa za podmreži ili podmreže adresni prostor. Za naše scenarij *192.168.1.0*.
    - **-r (ili – podmreže cidr)**. Mrežna maska u obliku CIDR za podmreže. Za naše scenarij *24*.
    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti u VNet. Za naše scenarij *Središnje SAD -a*.

3. Pokrenite naredbu **Stvaranje podmreži vnet azure mreže** da biste stvorili podmreži kao što je prikazano u nastavku. Popis nakon izlaz objašnjava parametri korišteni.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Evo očekivanog izlaza iznad naredbe:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (--vnet naziv ili**. Naziv VNet u kojem će se stvoriti podmreži. Za naše scenarij *TestVNet*.
    - **-n (ili – naziv)**. Naziv nove podmreže. Za naše scenarij *pozadinskog*.
    - **-(ili – adresa prefiks)**. Blokiranje CIDR podmreže. Četiri naših scenarij, *192.168.2.0/24*.

4. Pokrenite naredbe **Prikaži vnet azure mreže** da biste pogledali svojstva novi vnet kao što je prikazano u nastavku.

            azure network vnet show

    Evo očekivanog izlaza iznad naredbe:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
