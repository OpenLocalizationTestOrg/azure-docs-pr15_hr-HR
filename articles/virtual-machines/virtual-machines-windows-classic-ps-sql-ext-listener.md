<properties
    pageTitle="Konfiguriranje vanjskog ga Slušatelj uvijek na dostupnost grupa | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake za stvaranje programa uvijek na dostupnost grupe ga Slušatelj u Azure kojemu se vanjsko može pristupiti pomoću javnog virtualne IP adresu povezane oblaku."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Konfiguriranje vanjskog ga slušatelj za uvijek na dostupnost grupe u Azure

> [AZURE.SELECTOR]
- [Interna ga Slušatelj](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Vanjski ga Slušatelj](virtual-machines-windows-classic-ps-sql-ext-listener.md)

U ovoj se temi objašnjava konfiguriranje ga slušatelj za uvijek na dostupnost grupi koja je vanjsko dostupno na Internetu. To postala je moguće Pridruživanjem adresa **Javnog virtualne IP (VIP)** usluge oblaka ga slušatelj.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dostupnost grupi mogu sadržavati replike koji su lokalni samo Azure samo ili obuhvaćaju lokalnog i Azure hibridne konfiguracije. Azure replike se mogu nalaziti unutar iste područja ili preko više područja pomoću više virtualne mreža (VNets). Koraci u nastavku pretpostavlja koji ste već [konfigurirali granu dostupnost](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , ali ste konfigurirali u ga slušatelj.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Smjernice i ograničenja za vanjski slušače

Kada implementirate pomoću oblaka servisa penis VIP adresu Imajte na umu sljedeće smjernice o dostupnosti ga slušatelj grupe u Azure:

- Ga slušatelj dostupnost grupa je podržana u sustavu Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.

- Klijentska aplikacija mora se nalaziti na neki servis u oblaku za različite od one koja sadrži grupe dostupnosti VMs. Azure ne podržava povratna Izravni poslužitelja s postupak klijentske i poslužiteljske u istom servis u oblaku.

- Po zadanom korake u ovom se članku prikazuje kako konfigurirati jedan ga slušatelj da biste upotrijebili adresu oblaka servis virtualne IP (VIP). Međutim, moguće je rezervirati i stvoriti više adresa VIP za servis u oblaku. Omogućuje da biste stvorili više slušače svakog povezane s različitim VIP slijedite korake u ovom članku. Informacije o tome kako stvoriti više adresa VIP potražite u članku [Više VIPs po servis u oblaku](../load-balancer/load-balancer-multivip.md).

- Ako stvarate ga slušatelj za hibridnog okruženja, lokalne mreže morate povezati s Internetom javno osim VPN-a web-mjesto s Azure virtualne mreže. Kada se nalazite u Azure podmreže ga slušatelj dostupnost grupa je dostupna isključivo javnu IP adresu odgovarajući oblaku.

- Nije podržana za stvaranje vanjskog ga slušatelj na servisu oblaka kojima imate Interna ga slušatelj pomoću internog učitavanja opterećenja (ILB).

## <a name="determine-the-accessibility-of-the-listener"></a>Određivanje pristupačnosti ga slušatelj

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

U ovom se članku usredotočuje se na stvaranje ga slušatelj koji koristi **vanjski za ujednačavanje opterećenja**. Ako želite da ga slušatelj koja je s mrežom virtualna privatna, potražite u članku verziju u ovom se članku opisuje korake za postavljanje [ga slušatelj s ILB](virtual-machines-windows-classic-ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Stvaranje uravnoteženja krajnje točke VM s poslužiteljem za Izravni povrata

Vanjski opterećenja koristi u virtualne javno virtualne IP adrese servisa u oblaku koji hostira vaša VMs. Pa nije potrebno da biste stvarali ili konfigurirali raspoređivača opterećenja u tom slučaju.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Kopirajte skriptu PowerShell ispod u uređivaču teksta i postavljanje varijable vrijednosti da bi odgovarao okolini (zadano je dodijeljen za neke parametre). Imajte na umu da ako vašoj grupi dostupnost obuhvaća Azure područja, morate pokrenuti skriptu jednom u svakom podatkovnog centra za servis u oblaku i čvorove koje se nalaze u toj podatkovnog centra.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Nakon postavljanja varijabli skriptu kopirati iz uređivač teksta u sesiju Azure PowerShell pokrenite je. Ako i dalje prikazuje upit >>, upišite ENTER da biste provjerite skripte pokreće pokrenut.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Provjerite je li KB2854082 instaliran ako je potrebno

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Otvaranje priključaka u vatrozidu u čvorove grupe dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Stvaranje grupe ga slušatelj dostupnosti

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Za vanjske opterećenja, morate dobiti javno virtualne IP adrese servisa u oblaku koja sadrži vaše replike. Prijavite se na portal sustava Azure klasični. Idite na servis u oblaku koja sadrži grupe dostupnosti VM. Otvorite prikaz **nadzorne ploče** .

3. Obratite pažnju na adresi koja je prikazana u odjeljku **Adresa javnog virtualne IP (VIP)**. Ako rješenje obuhvaća VNets, ponovite taj korak za svaki servis u oblaku koja sadrži VM koji hostira kopiju.

4. Na nešto u VMs kopirajte skriptu PowerShell ispod u uređivaču teksta, a zatim postavite varijablama vrijednosti ste ranije zabilježili.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Jednom koji ste postavili varijable, otvorite prozor programa za dodatnim komponente Windows PowerShell, a zatim skriptu kopirati iz uređivač teksta i zalijepiti u sesiju Azure PowerShell pokrenite je. Ako i dalje prikazuje upit >>, upišite ENTER da biste provjerite skripte pokreće pokrenut.

1. Ponovite to na svakom VM. Ova skripta konfigurira resursa IP adrese s IP adresom od servisa u oblaku i postavlja drugi parametri kao što su probni priključak. Kada resursa IP adrese na mreži, to možete odgovoriti na ankete na priključak probni iz uravnoteženja krajnju točku stvorili ranije u ovom ćete praktičnom vodiču.

## <a name="bring-the-listener-online"></a>Premjesti ga slušatelj putem Interneta

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Praćenje stavki

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Provjera dostupnosti ga slušatelj grupe (unutar iste VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testiranje ga slušatelj grupe dostupnosti (putem Interneta)

Da biste pristupili ga slušatelj iz izvan virtualne mreže, morate koristiti vanjski/javno opterećenja (što je opisano u ovoj temi) umjesto ILB, koja je samo accesible unutar iste VNet. U nizu za povezivanje Navedite naziv usluge oblaka. Ako, na primjer, ako imate neki servis u oblaku s nazivom *mycloudservice*izjava sqlcmd bio na sljedeći način:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Za razliku od u prethodnom primjeru SQL provjere autentičnosti mora se koristiti, jer pozivatelj ne može koristiti provjeru autentičnosti sustava windows putem Interneta. Dodatne informacije potražite u članku [uvijek na dostupnost grupe u Azure VM: scenariji za povezivanje klijent](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Kada koristite SQL provjeru autentičnosti, provjerite je li stvoriti istu Prijava na glavne. Dodatne informacije o otklanjanju poteškoća s prijave s grupama dostupnost potražite u članku [kako mapiranje prijave ili koristite nalazi korisnik SQL baze podataka za povezivanje s drugim replike i mapiranje dostupnosti baze podataka](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Ako replike uvijek na su u različitim podmreže, klijenti morate navesti **MultisubnetFailover = True** u nizu za povezivanje. Ovo rezultira pokušaja paralelnog povezivanja na replike u različitim podmreže. Imajte na umu da scenarij sadrži implementacije uvijek na grupe dostupnosti više područja.

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
