<properties
    pageTitle="Konfiguriranje ILB ga Slušatelj za uvijek na grupe dostupnosti | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča koristi resursa koje su stvorene pomoću model klasični implementacije i stvara se uvijek na dostupnost grupe ga Slušatelj u Azure pomoću programa raspoređivača opterećenja Interna (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfiguriranje programa ILB ga slušatelj za uvijek na dostupnost grupe u Azure

> [AZURE.SELECTOR]
- [Interna ga Slušatelj](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Vanjski ga Slušatelj](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Pregled

U ovoj se temi objašnjava konfiguriranje ga slušatelj za uvijek na dostupnost grupi pomoću **Internog raspoređivača opterećenja (ILB)**.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Konfiguriranje programa ILB ga slušatelj za dostupnost grupi uvijek na u modelu Voditelj resursa, potražite u članku [Konfiguriranje Interna opterećenja za dostupnost grupi uvijek na servisu Azure](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Dostupnost grupi mogu sadržavati replike koji su lokalni samo Azure samo ili obuhvaćaju lokalnog i Azure hibridne konfiguracije. Azure replike se mogu nalaziti unutar iste područja ili preko više područja pomoću više virtualne mreža (VNets). Koraci u nastavku pretpostavlja koji ste već [konfigurirali granu dostupnost](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , ali ste konfigurirali u ga slušatelj.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Smjernice i ograničenja za interne slušače
Imajte na umu sljedeće smjernice o dostupnosti ga slušatelj grupe u Azure pomoću ILB:

- Ga slušatelj dostupnost grupa je podržana u sustavu Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.

- Samo jedan ga slušatelj grupe Interna dostupnost podržano je po servis u oblaku, budući da ga slušatelj konfiguriran tako da na ILB i postoji samo jedan ILB po servis u oblaku. Međutim, nije moguće stvoriti više vanjskih slušače. Dodatne informacije potražite u članku [Konfiguriranje vanjskog ga slušatelj za uvijek na dostupnost grupe u Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Nije podržana za stvaranje internog ga slušatelj na servisu oblaka gdje ćete imati i vanjske ga slušatelj pomoću servisa oblaka javno VIP.

## <a name="determine-the-accessibility-of-the-listener"></a>Određivanje pristupačnosti ga slušatelj

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

U ovom članku fokus je na stvaranje ga slušatelj koji koristi **Interni raspoređivača opterećenja (ILB)**. Ako vam je potrebna je javno/vanjsko ga slušatelj, potražite u članku verziju u ovom se članku opisuje korake za postavljanje [vanjskog ga slušatelj](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Stvaranje uravnoteženja krajnje točke VM s poslužiteljem za Izravni povrata

Za ILB, prvo morate stvoriti Interna opterećenja. To možete učiniti u nastavku skripti.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Za **ILB**, trebali biste dodijeliti statičke IP adrese. Najprije, pregledajte trenutnu VNet konfiguraciju tako da pokrenete sljedeću naredbu:

        (Get-AzureVNetConfig).XMLConfiguration

1. Imajte na umu naziv **podmreže** podmreže koja sadrži VMs koji hostira na replike. To će se koristiti u parametru **$SubnetName** u skripti.

1. Zatim zabilježite naziv **VirtualNetworkSite** i početni **AddressPrefix** za podmreže koja sadrži VMs koji hostira na replike. Potražite IP adresu dostupno prosljeđivanje obje se vrijednosti na naredbu **Test AzureStaticVNetIP** i provjera **AvailableAddresses**. Na primjer, ako je na VNet pod nazivom *MyVNet* i imali podmreže adresu u rasponu koji rada na *172.16.0.128*, sljedeću naredbu bi adrese dostupne:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Odaberite neku od dostupnih adresa te ga koristiti u parametru **$ILBStaticIP** sljedeće skripte.

3. Kopirajte skriptu PowerShell ispod u uređivaču teksta i postavljanje varijable vrijednosti da bi odgovarao okolini (Imajte na umu da je dodijeljen zadanih vrijednosti za neke parametre). Imajte na umu postojeće implementacijama koje koriste afinitet grupe ne možete dodati ILB. Dodatne informacije o preduvjetima za ILB potražite u članku [Interna opterećenja](../load-balancer/load-balancer-internal-overview.md). Osim toga, ako vašoj grupi dostupnost obuhvaća Azure područja, morate pokrenuti skriptu jednom u svakom podatkovnog centra za servis u oblaku i čvorove koje se nalaze u toj podatkovnog centra.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Nakon postavljanja varijabli skriptu kopirati iz uređivač teksta u sesiju Azure PowerShell pokrenite je. Ako i dalje prikazuje upit >>, upišite ENTER da biste provjerite skripte pokreće pokrenut. Napomena

>[AZURE.NOTE] Azure klasični portal ne podržava Interna raspoređivača opterećenja trenutku, pa nećete vidjeti na ILB ili krajnje točke na portalu za Azure klasični. Međutim, **Get-AzureEndpoint** Vraća internu IP adresu ako raspoređivača opterećenja se izvodi na njemu. U suprotnom vraća vrijednost null.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Provjerite je li KB2854082 instaliran ako je potrebno

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Otvaranje priključaka u vatrozidu u čvorove grupe dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Stvaranje grupe ga slušatelj dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Za ILB, morate koristiti IP adrese sustava na interni učitavanja opterećenja (ILB) ste ranije stvorili. Koristite sljedeću skriptu da biste dobili IP adresu u PowerShell.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Na nešto na VMs kopirajte skriptu PowerShell za vaš operacijski sustav u uređivaču teksta, a zatim postavite varijablama vrijednosti ste ranije zabilježili.

    Ili noviji za Windows Server 2012 koristite sljedeću skriptu:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Za Windows Server 2008 R2 koristite sljedeću skriptu:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Jednom koji ste postavili varijable, otvorite prozor programa za dodatnim komponente Windows PowerShell, a zatim skriptu kopirati iz uređivač teksta i zalijepiti u sesiju Azure PowerShell pokrenite je. Ako i dalje prikazuje upit >>, upišite ENTER da biste provjerite skripte pokreće pokrenut.

2. Ponovite to na svakom VM. Ova skripta konfigurira resursa IP adrese s IP adresom od servisa u oblaku i postavlja drugi parametri kao što su probni priključak. Kada resursa IP adrese na mreži, ga možete odgovoriti na ankete na priključak probni iz uravnoteženja krajnju točku stvorili ranije u ovom ćete praktičnom vodiču.

## <a name="bring-the-listener-online"></a>Premjesti ga slušatelj putem Interneta

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Praćenje stavki

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Provjera dostupnosti ga slušatelj grupe (unutar iste VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
