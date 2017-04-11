## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Uvođenje predloška ARM pomoću EŽA Azure

Da biste implementirali ARM predložak koji ste preuzeli pomoću EŽA Azure, slijedite korake u nastavku.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.
2. Pokretanje u **`azure config mode`** naredbu da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Evo očekivanog izlaza iznad naredbe:

        info:    New mode is arm

3. Ako je potrebno, pokrenite na **`azure group create`** da biste stvorili novu grupu resursa, kao što je prikazano u nastavku. Obratite pozornost na rezultatu naredbe. Popis nakon izlaz objašnjava parametri korišteni. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](../articles/resource-group-overview.md).

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

4. Pokretanje u **`azure group deployment create`** cmdleta za implementaciju novi VNet pomoću predloška i parametar datoteke možete preuzeti i izmijeniti iznad. Popis nakon izlaz objašnjava parametri korišteni.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Evo očekivanog izlaza iznad naredbe:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (ili – grupa resursa)**. Naziv grupe resursa novi VNet stvorit će se u.
    - **-f (ili – datoteku predloška)**. Put do OKVIRA datoteku predloška.
    - **-e (ili – parametara datoteka)**. Put do datoteke parametara OKVIRA.

5. Pokretanje u **`azure network vnet show`** naredbu da biste pogledali svojstva novi vnet, kao što je prikazano u nastavku.

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
