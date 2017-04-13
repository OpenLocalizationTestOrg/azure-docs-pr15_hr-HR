<properties 
   pageTitle="Kako postaviti statičke IP privatne u načinu ARM pomoću na EŽA | Microsoft Azure"
   description="Razumijevanje IP-statične ovi (DIPs) i kako upravljati njima u načinu ARM pomoću na EŽA"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Upute za postavljanje statičke privatne IP adrese EŽA Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [Upravljanje statičke privatne IP adrese u modelu klasični implementacije](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ispod primjere Azure EŽA naredbi očekivati okruženju jednostavne već stvorili. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje opisane u [Stvaranje na vnet](virtual-networks-create-vnet-arm-cli.md)okruženje za testiranje.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kako odrediti statičke privatne IP adrese prilikom stvaranja na VM
Da biste stvorili VM pod nazivom *DNS01* u *sučelju* podmreži VNet pod nazivom *TestVNet* pomoću statičke IP privatne od *192.168.1.101*, slijedite ove korake:

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.

2. Pokrenite naredbu **azure config način** da biste prešli u način Voditelj resursa, kao što je prikazano u nastavku.

        azure config mode arm

    Očekivani izlaz:

        info:    New mode is arm

3. Pokrenite u **Stvaranje javnog ip azure mreže** da biste stvorili javnu IP za na VM. Popis nakon izlaz objašnjava parametri korišteni.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Očekivani izlaz:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (ili – grupa resursa)**. Naziv javnu IP stvorit će se u grupu resursa.
    - **-n (ili – naziv)**. Naziv javnu IP.
    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti javnu IP. Za naše scenarij *centralus*.

3. Pokrenite naredbu **Stvaranje nic azure mreže** da biste stvorili na NIC statičke IP privatne. Popis nakon izlaz objašnjava parametri korišteni.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Očekivani izlaz:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-(ili – privatna ip adresa)**. Statički privatne IP adresa u NIC.
    - **-m (ili – podmreže vnet naziv)**. Naziv VNet u kojem će se stvoriti NIC-a.
    - **-k (ili – podmreže naziv)**. Naziv podmreže u kojem će se stvoriti NIC-a.

4. Pokrenite naredbu **azure vm stvaranje** da biste stvorili VM pomoću javnu IP i NIC stvorena iznad. Popis nakon izlaz objašnjava parametri korišteni.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Očekivani izlaz:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (ili os – vrsta)**. Operacijski sustav za VM, *Windows* i *Linux*.
    - **-f (ili – nic naziv)**. Naziv NIC koristit će se VM.
    - **-i (ili – javno ip ime)**. Naziv javnu IP koristit će se VM.
    - **-F (ili – vnet naziv)**. Naziv VNet u kojem će se stvoriti u VM.
    - **-j (ili – vnet podmreže ime)**. Naziv podmreže u kojem će se stvoriti u VM.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Upute za dohvaćanje statične privatne IP adresa informacija o na VM

Da biste pogledali statički privatne podataka IP adrese za VM stvorene pomoću skripte iznad, pokrenite sljedeću naredbu EŽA Azure i pridržavajte vrijednosti za *dopuštenih način privatno IP* i *privatno IP adresa*:

    azure vm show -g TestRG -n DNS01

Očekivani izlaz:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kako ukloniti statičke privatne IP adrese iz programa VM
Ne možete ukloniti statičke privatne IP adrese iz NIC u Azure EŽA za Voditelj resursa. Morate stvoriti novi NIC koji koristi dinamičke IP, uklonite prethodnu NIC iz na VM i pa dodajte nove NIC na VM. Da biste promijenili NIC-a za int VM koristi eh naredbe iznad, slijedite korake u nastavku.
    
1. Pokrenite naredbu **Stvaranje nic azure mreže** da biste stvorili novi NIC pomoću dinamičke IP dodijeljeni. Obratite pozornost na to kako nije potrebno da biste odredili IP adresu ovaj put.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Očekivani izlaz:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Pokrenite naredbu **azure vm postavljanje** da biste promijenili NIC koji se koriste u VM.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Očekivani izlaz:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Ako je željeli, pokrenite naredbu **Brisanje nic azure mreže** da biste izbrisali stare NIC.

        azure network nic delete -g TestRG -n TestNIC --quiet

    Očekivani izlaz:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kako dodati statične privatne IP adrese postojeće VM
Da biste dodali statičke privatne IP adrese NIC koristi VM stvoren pomoću skripte iznad, pokrenite sljedeću naredbu:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Očekivani izlaz:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [rezervirane javnu IP](virtual-networks-reserved-public-ip.md) adrese.
- Informacije o adresama u [instancu razinom javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Pogledajte [rezervirane IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx).
