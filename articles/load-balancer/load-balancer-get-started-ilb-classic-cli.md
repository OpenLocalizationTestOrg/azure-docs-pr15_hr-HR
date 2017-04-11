<properties
   pageTitle="Stvaranje programa Interna opterećenja pomoću EŽA Azure u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako stvoriti na interni opterećenja pomoću EŽA Azure u modelu klasični implementacije"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Početak rada prilikom stvaranja za interne opterećenja (klasični) pomoću EŽA Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Da biste stvorili Interna opterećenja postavite za virtualnim strojevima

Da biste stvorili skupa opterećenja Interna opterećenja i poslužitelje koji će se slati svojim promet na njega, učinite sljedeće:

1. Stvaranje instance komponente Interna učitati opterećenja koji će biti krajnje promet se rasporediti na poslužiteljima skupa uravnoteženja opterećenje.

1. Dodavanje krajnje točke odgovara virtualnim strojevima koje će primati dolazne promet.

1. Konfiguriranje s poslužiteljima koji će slanja promet se rasporediti da biste poslali njihove promet na adresu virtualne IP (VIP) instance Interna učitati opterećenja opterećenje.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Korak po korak stvaranje programa Interna raspoređivača opterećenja pomoću EŽA

Ovaj vodič prikazuje kako stvoriti na interni opterećenja ovisno o scenariju iznad.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.

2. Pokrenite naredbu **azure config način** da biste prešli na klasičan način rada, kao što je prikazano u nastavku.

        azure config mode asm

    Očekivani izlaz:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Stvaranje krajnjoj točki i postavljanje raspoređivača učitavanja

Scenarij pretpostavlja virtualnim strojevima "DG1" i "DB2" u oblaku pod nazivom "mytestcloud". Oba virtualnim strojevima koriste virtualne mreže pod nazivom Moje "testvnet" s podmreže "podmreže-1".

Ovaj vodič će stvoriti skupa raspoređivača učitavanja Interna koristi priključak 1433 kao privatne priključak i 1433 kao lokalnog priključka.

Ovo je uobičajeni scenarij u kojem postoje SQL virtualnim strojevima na pozadinskih jamči poslužitelja za bazu podataka neće se izložiti izravno pomoću javnu IP adresu pomoću internog opterećenja.


### <a name="step-1"></a>Korak 1

Stvaranje programa Interna opterećenja skupa pomoću `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parametri koji se koriste:

**-r** - naziv servisa oblaka.<BR>
**-n** - naziv raspoređivača učitavanja interni<BR>
**-t** – podmreže naziv (istoj podmreži po virtualnim strojevima ćete dodati interne opterećenja)<BR>
**-na** - (neobavezno) dodajte statične privatne IP adresa<BR>

Pogledajte `azure service internal-load-balancer --help` dodatne informacije.

Možete provjeriti svojstva raspoređivača Interna učitavanje pomoću naredbe `azure service internal-load-balancer list` *Naziv usluge oblaka*.

U nastavku slijedi primjer izlaz:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Korak 2

Konfiguriranje skup opterećenja Interna učitavanja prilikom dodavanja prvog krajnje točke. Ćete povezati krajnje točke, virtualnog računala i probni priključak skup opterećenja Interna Učitaj u ovom ćete koraku.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parametri koji se koriste:

**– k** - priključak lokalne virtualnog računala<BR>
**-t** – probni priključak<BR>
**-r** - protokol probni<BR>
**-e** - probni interval u sekundama<BR>
**-f** - interval vremenskog ograničenja u sekundama <BR>
**-i** - naziv internog učitavanja raspoređivača <BR>


## <a name="step-3"></a>Korak 3

Provjerite pomoću konfiguracije za učitavanje raspoređivača `azure vm show` *naziv virtualnog računala*

    azure vm show DB1

Rezultat će biti:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Stvaranje udaljene radne površine krajnja točka za virtualnog računala

Možete stvoriti udaljene radne površine krajnja točka za prosljeđivanje mrežni promet iz javne priključka Lokalni priključak za korištenje određenih virtualnog računala `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Uklanjanje virtualnog računala s opterećenja

Virtualnog računala možete ukloniti iz programa Interna opterećenja postaviti tako da izbrišete povezana krajnje točke. Nakon uklanjanja krajnju točku raspoređivača opterećenja postavite više neće pripadati virtualnog računala.

 Korištenje gornji primjer, možete ukloniti krajnju točku stvorene za virtualnog računala "DG1" iz internog opterećenja "ilbset" pomoću naredbe `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Pogledajte `azure vm endpoint --help` dodatne informacije.


## <a name="next-steps"></a>Daljnji koraci

[Konfiguriranje načina distribucije raspoređivača učitavanja pomoću afiniteta izvorni IP](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)