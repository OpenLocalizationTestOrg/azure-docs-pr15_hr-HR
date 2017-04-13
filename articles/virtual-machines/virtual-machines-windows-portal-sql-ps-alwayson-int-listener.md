<properties
   pageTitle="Konfiguriranje uvijek na grupe dostupnosti slušače – Microsoft Azure"
   description="Konfiguriranje dostupnosti grupe slušače na model Azure Voditelj resursa, internog opterećenja pomoću jednu ili više IP adresa."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Konfiguriranje jedan ili više uvijek na dostupnost grupe slušače - Voditelj resursa 

U ovoj se temi objašnjava napravite dvije stvari:

- Stvaranje internog opterećenja za SQL Server dostupnost grupe pomoću cmdleta ljuske PowerShell.

- Dodavanje dodatnih IP adresa u opterećenja podržava više od jedne grupe dostupnosti SQL Server. 

U sustavu SQL Server na ga slušatelj grupe dostupnosti je virtualne mreže naziv koji se klijenti povezuju s da biste pristupili bazi podataka u primarni ili sekundarnog replike. Na Azure virtualnim strojevima raspoređivača opterećenja sadrži IP adresu da ga slušatelj. Raspoređivača opterećenja usmjerava promet na instancu sustava SQL Server koji se priključuje na probni priključak. U većini slučajeva, granu dostupnost koristi interni opterećenja. Programa Azure Interna opterećenja možete hostirati jednu ili više IP adresa. Svaka IP adresa koristi priključak određene probni. Ovaj dokument pokazuje kako pomoću ljuske PowerShell za stvaranje novog opterećenja ili dodavanje IP adrese postojeće raspoređivača opterećenja za SQL Server dostupnost grupe. 

Mogućnost da biste dodijelili više IP adresa za interne opterećenja je novo za Azure i dostupna je samo u modelu Voditelj resursa. Da biste dovršili ovaj zadatak, morate imati grupu dostupnost SQL Server implementiran na Azure virtualnim strojevima u modelu Voditelj resursa. Oba sustava SQL Server na virtualnim strojevima mora pripadati isti skup dostupnost. [Predložak programa Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) možete koristiti da biste automatski stvorili grupu dostupnost u Azure Voditelj resursa. Ovaj predložak se automatski stvara grupe dostupnosti, uključujući interne opterećenja umjesto vas. Ako želite, možete ga [ručno konfiguriranje programa grupe dostupnosti AlwaysOn](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

U ovoj se temi zahtijeva su već konfigurirana dostupnost grupe.  

Povezane teme obuhvaćaju sljedeće:

- [Konfiguriranje grupe dostupnosti AlwaysOn u Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Konfiguriranje veze VNet VNet pomoću upravitelja resursa Azure i komponente PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Konfiguriranje Vatrozida za Windows

Konfiguriranje Vatrozida za Windows da biste omogućili pristup sustava SQL Server. Morat ćete konfigurirati Vatrozid da dopušta veze TCP priključci korištenje instancu sustava SQL Server, kao i priključak koji se koristi tako da ga slušatelj probni. Detaljne upute potražite u članku [Konfiguriranje Vatrozida za Windows za Database Engine Access](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Stvaranje ulazna pravila za priključak za SQL Server i port probni.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Primjer skripte: Stvaranje internog opterećenja sa servisom PowerShell

> [AZURE.NOTE] Ako ste stvorili grupu dostupnost [predložak programa Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) opterećenje nije potrebno da biste dovršili ovaj korak. 

Sljedeću skriptu komponente PowerShell stvara Interna opterećenja, konfigurira pravila za ujednačavanje opterećenja i postavlja IP adresa za raspoređivača opterećenja. Da biste pokrenuli skriptu, otvorite Windows Očisti filtar i zalijepite skriptu u oknu skripte. Korištenje `Login-AzureRMAccount` da biste se prijavili PowerShell. Ako imate više pretplata Azure, pomoću `Select-AzureRmSubscription ` da biste postavili pretplate. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Primjer skripte: dodavanje IP adrese u postojeći raspoređivača opterećenja sa servisom PowerShell

Da biste koristili više od jedne grupe dostupnosti, da biste dodali dodatne IP adresa u postojeće opterećenja pomoću komponente PowerShell. Svaka IP adresa zahtijeva vlastitu pravilo, priključak probni i prednju priključak za ujednačavanje opterećenja.

Priključak sučelje je priključak koji koristite aplikacije za povezivanje s instancu sustava SQL Server. IP adrese za različite dostupnost grupe možete koristiti isti priključak sučelje.

>[AZURE.NOTE] Za SQL Server dostupnost grupe, svaka IP adresa potreban je određeni probni priključak. Na primjer, ako jedan IP adresa servisa raspoređivača opterećenja koristi priključak probni 59999, nema drugih IP adrese na tom opterećenja pomoću probni priključak 59999. 

- Informacije o raspoređivača opterećenja ograničenja potražite u članku **privatne prednju završili IP po raspoređivača opterećenja** u odjeljku [Ograničenja umrežavanje - Azure Voditelj resursa](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

- Informacije o dostupnosti ograničenja grupe potražite u članku [ograničenja (dostupnost grupe)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Sljedeću skriptu dodaje novi IP adresa za postojeće opterećenja. Ažurirajte varijable za vaše okruženje. Na ILB koristi priključak ga slušatelj za priključak sučelje ujednačavanje opterećenja. Ovaj priključak može biti priključka koji se priključuje SQL Server na. Zadane instance sustava SQL Server, to je priključak 1433. Pravilo za grupe sustava dostupnosti za ujednačavanje opterećenja zahtijeva plutajućih IP (Izravni server povrata) tako da priključak pozadinska je isti kao priključak sučelje.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfiguriranje klaster da koristi IP adresu raspoređivača učitavanja 

Sljedeći je korak konfiguriranje ga slušatelj na klaster i otvorili ga slušatelj putem Interneta. Da biste to postigli, učinite sljedeće: 

1. Stvaranje grupe ga slušatelj dostupnost na prebacivanje klaster  

2. Premjesti ga slušatelj putem Interneta

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. Stvaranje grupe ga slušatelj dostupnost na prebacivanje klaster

U ovom ćete koraku dodajte klijent pristupnu točku klaster prebacivanje s pomoćnim klaster Manager, a pomoću komponente PowerShell konfiguriranje resursa klaster slušanje probni priključak. 

1. Korištenje RDP za povezivanje s Azure virtualnog računala koje hostira primarni replike. 

2. Otvorite upravitelj klaster prebacivanje.

3. Odaberite čvor **mreža** i zabilježite naziv mreže klaster. Koristiti taj naziv u na `$ClusterNetworkName` varijable u skriptu PowerShell.

4. Proširite naziv klaster, a zatim kliknite **uloge**.

5. U oknu **uloge** , desnom tipkom miša kliknite naziv grupe dostupnosti, a zatim odaberite **Dodaj resursa** > **Klijent pristupnoj točci**.

6. U okvir **naziv** stvaranje naziva za ovaj novi ga slušatelj, a zatim dvaput kliknite **Dalje** , a zatim **Završi**. Ne premjestiti ga slušatelj ili resursa na Internetu sada.
 
    Naziv da ga slušatelj novi je naziv mreže koju aplikacija će se koristiti za povezivanje s bazama podataka u grupi dostupnost SQL Server.

7. Kliknite karticu **resursa** , a zatim proširite pristupnu točku klijenta koji ste upravo stvorili. Desnom tipkom miša kliknite IP resursa, a zatim kliknite Svojstva. Zapamtite naziv IP adresa. Koristite ovaj naziv u na `$IPResourceName` varijable u skriptu PowerShell.

8. U odjeljku **IP adresa** kliknite **Statičke IP adrese** i postavite statičke IP adrese na istu adresu koju ste koristili kada postavite IP adresa raspoređivača opterećenja portala za Azure. 

9. Onemogući NetBIOS za tu adresu, a zatim kliknite **u redu**. Ponovite ovaj korak za svaki resurs IP Ako rješenje obuhvaća više VNets Azure. 

10. Provjerite resursa grupe dostupnosti za SQL Server ovisi o IP adresa. Dvokliknite resursa u upravitelju klaster, to je na kartici **resursa** u odjeljku **Ostale resurse**. 

11. Na klaster čvor koji trenutno hostira primarni replike otvorite povećane Očisti filtar i zalijepite sljedeće naredbe u novu skriptu. Na kartici **ovisnosti** , kliknite naziv ga slušatelj.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Ažurirajte varijabli i pokrenuti skriptu PowerShell da biste konfigurirali IP adresa i priključaka da ga slušatelj novi.

 >[AZURE.NOTE] Ako SQL Server u zasebnom područja, morate pokrenuti skriptu PowerShell dvaput. Kada prvi put koristite klaster naziv mreže naziv resursa za IP klaster te ga učitajte opterećenja IP adrese iz prve grupe resursa. Drugi put kada koristite naziv mreže klaster, naziv resursa za IP klaster te ga učitajte opterećenja IP adrese iz druge grupe resursa.

Skupine se programa ga slušatelj resursa za dostupnost grupe.

## <a name="2-bring-the-listener-online"></a>2. Premjesti ga slušatelj putem Interneta

S dostupnost grupe ga slušatelj resursa konfigurirana, možete prenijeti ga slušatelj internetu tako da aplikacije mogu se povezati s bazama podataka u grupi dostupnosti s ga slušatelj.

1. Prijeđite natrag u programu prebacivanje klaster Manager. Proširite **uloge** i označite grupu dostupnost. Na kartici **resursa** , desnom tipkom miša kliknite naziv ga slušatelj, a zatim kliknite **Svojstva**.

1. Kliknite karticu **ovisnosti** . Ako postoji više resursa, IP adrese provjerite imaju li ili ne AND, ovisnosti. Kliknite **u redu**.

1. Desnom tipkom miša kliknite naziv ga slušatelj, a zatim kliknite **Premjesti Online**.

1. Nakon što ga slušatelj je na Internetu, na kartici **Resursi** , grupe dostupnosti desnom tipkom miša, a zatim kliknite **Svojstva**.

1. Stvaranje ovisnosti na ga slušatelj naziv resursa (ne IP adresa resursi naziv). Kliknite **u redu**.

1. Pokrenite SQL Server Management Studio i povezati primarni replike.

1. Dođite do **dostupnosti AlwaysOn visoke** | **grupe dostupnosti** | **slušače grupe dostupnosti**. 

1. Prikazat će se sada ga slušatelj naziv koji ste stvorili u upravitelju za prebacivanje klaster. Desnom tipkom miša kliknite naziv ga slušatelj, a zatim kliknite **Svojstva**.

1. U okviru **priključak** navedite broj priključka da ga slušatelj dostupnost grupu pomoću $EndpointPort koji ste koristili ranije (1433 je zadana postavka), zatim kliknite **u redu**.

Sada imate dostupnost grupu sustava SQL Server na Azure virtualnim računalima sustava u načinu rada Voditelj resursa. 

## <a name="test-the-connection-to-the-listener"></a>Testiranje veze da biste ga slušatelj

Da biste testirali veze:

1. RDP sa sustavom SQL Server koja se nalazi u istom virtualne mreže, ali vlasnik replike. To može biti druge SQL Server u klasteru.

2. Koristite **sqlcmd** utility za testiranje veze. Na primjer, sljedeću skriptu uspostavlja vezu **sqlcmd** primarni replike kroz ga slušatelj s provjeru autentičnosti sustava Windows:

    ```
    sqlmd -S <listenerName> -E
    ```

    Ako ga slušatelj koristi priključak koji nisu zadani priključak (1433), navedite priključak u nizu za povezivanje. Ako, na primjer, sljedeću naredbu sqlcmd povezuje ga slušatelj pri priključak izdanje 1435: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Veza SQLCMD automatski povezuje s bez obzira instancu sustava SQL Server hostira primarni replike. 

>[AZURE.NOTE] Provjerite je li priključak navedete otvorena na vatrozid i SQL Server. Oba poslužitelji zahtijevaju ulazna pravila za TCP priključak koji koristite. Potražite u članku [Dodavanje ili uređivanje pravila vatrozida](http://technet.microsoft.com/library/cc753558.aspx) za dodatne informacije. 

## <a name="guidelines-and-limitations"></a>Smjernice i ograničenja

Imajte na umu sljedeće smjernice o dostupnosti ga slušatelj grupe u Azure pomoću internog učitati opterećenja:

- S internim opterećenja samo pristupiti ga slušatelj iz unutar iste virtualne mreže.

## <a name="for-more-information"></a>Dodatne informacije

Dodatne informacije potražite u članku [Konfiguriranje uvijek na dostupnost grupirati Azure VM ručno](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>Cmdleta ljuske PowerShell

Koristite sljedeće Cmdlete ljuske PowerShell za stvaranje internog opterećenja za Azure virtualnih računala.

- `New-AzureRmLoadBalancer`stvara raspoređivača opterećenja. Dodatne informacije potražite u članku [Novo AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`stvara sučelja IP konfiguracija raspoređivača opterećenja. Dodatne informacije potražite u članku [Novo AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`stvara konfiguracije pravila za raspoređivača opterećenja. Dodatne informacije potražite u članku [Novo AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`stvara pozadinskog adresu skup konfiguracije za raspoređivača opterećenja. Dodatne informacije potražite u članku [Novo AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`stvara probni konfiguracija za raspoređivača opterećenja. Dodatne informacije potražite u članku [Novo AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) .

Da biste uklonili raspoređivača opterećenja iz grupe Azure resursa, koristite `Remove-AzureRmLoadBalancer`. Dodatne informacije potražite u članku [Uklanjanje AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).
