<properties 
   pageTitle="Upute za postavljanje statičke IP privatni klasičnog načina ausing na EŽA | Microsoft Azure"
   description="Razumijevanje statične privatne IP-ovi (DIPs) i kako upravljati njima u klasičan način rada pomoću na EŽA"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Kako postaviti statičke privatne IP adrese (klasični) u EŽA Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [Upravljanje statičke privatne IP adrese u modelu implementacije Voditelj resursa](virtual-networks-static-private-ip-arm-cli.md).

Ispod primjere Azure EŽA naredbi očekivati okruženju jednostavne već stvorili. Ako želite da biste pokrenuli naredbe, kako se prikazuju u ovom dokumentu, najprije sastavljanje opisane u [Stvaranje na vnet](virtual-networks-create-vnet-classic-cli.md)okruženje za testiranje.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kako odrediti statičke privatne IP adrese prilikom stvaranja na VM
Da biste stvorili novi VM pod nazivom *DNS01* u nove servise u oblaku pod nazivom *TestService* ovisno o scenariju iznad, slijedite ove korake:

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.
1. Pokrenite naredbu **azure servis stvoriti** da biste stvorili servisa u oblaku.

        azure service create TestService --location uscentral

    Očekivani izlaz:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Pokrenite naredbu **azure stvaranje vm** da biste stvorili na VM. Obratite pozornost na vrijednost za statične privatne IP adrese. Popis nakon izlaz objašnjava parametri korišteni.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Očekivani izlaz:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (ili – mjesto)**. Azure regija kojem će se stvoriti u VM. Za naše scenarij *centralus*.
    - **-n (ili – vm naziv)**. Naziv VM će biti stvoren.
    - **-w (ili – virtualne--naziv mreže)**. Naziv VNet u kojem će se stvoriti u VM. 
    - **Do nedjelje (ili – statičnu ip)**. Statički privatne IP adresa u VM.
    - **TestService**. Naziv servisa u oblaku na kojem će se stvoriti u VM.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Slika koja se koristi za stvaranje na VM.
    - **adminuser**. Lokalni administrator za VM sustava Windows.
    - **AdminP@ssw0rd**. Lokalni administratorsku lozinku za VM sustava Windows.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Upute za dohvaćanje statične privatne IP adresa informacija o na VM
Da biste pogledali statički privatne podataka IP adrese za VM stvorene pomoću skripte iznad, pokrenite sljedeću naredbu EŽA Azure i pridržavajte vrijednost za *StaticIP mreže*:

    azure vm static-ip show DNS01

Očekivani izlaz:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kako ukloniti statičke privatne IP adrese iz programa VM
Da biste uklonili privatne statičke IP adrese dodati VM u skripti iznad, pokrenite sljedeću naredbu Azure EŽA:
    
    azure vm static-ip remove DNS01

Očekivani izlaz:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Kako dodati statičke IP privatne postojeće VM
Da biste dodali u statični privatne IP adresu da biste VM stvoren pomoću skripte iznad runt on naredbe:

    azure vm static-ip set DNS01 192.168.1.101

Očekivani izlaz:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [rezervirane javnu IP](virtual-networks-reserved-public-ip.md) adrese.
- Informacije o adresama u [instancu razinom javnu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Pogledajte [rezervirane IP REST API-ji](https://msdn.microsoft.com/library/azure/dn722420.aspx).
