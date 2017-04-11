<properties
   pageTitle="Početak rada prilikom stvaranja internetsku nasuprotne opterećenja u modelu uvođenje klasičnog pomoću EŽA Azure | Microsoft Azure"
   description="Saznajte kako stvoriti internetsku nasuprotne opterećenja u modelu uvođenje klasičnog pomoću EŽA Azure"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Početak rada prilikom stvaranja internetsku nasuprotne opterećenja (klasični) u EŽA Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [Saznajte kako stvoriti internetsku nasuprotne opterećenja pomoću upravitelja resursa Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Korak po korak stvaranje internetsku nasuprotne opterećenja pomoću EŽA

Ovaj vodič prikazuje kako stvoriti programa Internet raspoređivača opterećenja ovisno o scenariju iznad.

1. Ako još niste koristili Azure EŽA, pročitajte članak [Instaliranje i konfiguriranje EŽA Azure](../../articles/xplat-cli-install.md) i slijedite upute do točke gdje odaberite račun za Azure i pretplate.

2. Pokrenite naredbu **azure config način** da biste prešli na klasičan način rada, kao što je prikazano u nastavku.

        azure config mode asm

    Očekivani izlaz:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Stvaranje krajnjoj točki i postavljanje raspoređivača učitavanja

Scenarij pretpostavlja virtualnim strojevima "webu1" i "webu2" stvorene.
Ovaj vodič će stvoriti skup raspoređivača opterećenja pomoću priključak 80 kao javno priključak i priključak 80 kao lokalnog priključka. Probni priključak je konfiguriran na priključak 80 i imenovani skup raspoređivača opterećenja "lbset".


### <a name="step-1"></a>Korak 1

Stvaranje prve krajnjoj točki i učitavanje opterećenja skupa pomoću `azure network vm endpoint create` za virtualnog računala "webu1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parametri koji se koriste:

**– k** - priključak lokalne virtualnog računala<br>
**-o** - protokol<BR>
**-t** – probni priključak<BR>
**-b** – naziv raspoređivača učitavanja<BR>

## <a name="step-2"></a>Korak 2

Dodajte drugi virtualnog računala "webu2" da biste postavili raspoređivača opterećenja.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Korak 3

Provjerite je li pomoću konfiguracije učitavanja opterećenja `azure vm show` .

    azure vm show web1

Rezultat će biti:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Stvaranje udaljene radne površine krajnja točka za virtualnog računala

Možete stvoriti udaljene radne površine krajnja točka za prosljeđivanje mrežni promet iz javne priključka Lokalni priključak za korištenje određenih virtualnog računala `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Uklanjanje virtualnog računala s opterećenja

Imate da biste izbrisali krajnju točku povezane opterećenja postavite od virtualnog računala. Nakon uklanjanja krajnju točku virtualnog računala ne pripada opterećenja postavite više.

 Korištenje gornji primjer krajnja točka za virtualnog računala "webu1" stvara možete ukloniti iz opterećenja "lbset" pomoću naredbe `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Možete istražili dodatne mogućnosti da biste upravljali krajnje točke pomoću naredbe`azure vm endpoint --help`


## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-ps.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)

