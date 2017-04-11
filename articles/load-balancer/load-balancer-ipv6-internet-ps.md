<properties
    pageTitle="Stvaranje internetsku nasuprotne opterećenja s IPv6 pomoću komponente PowerShell za Voditelj resursa | Microsoft Azure"
    description="Saznajte kako stvoriti internetsku nasuprotne opterećenja s IPv6 pomoću komponente PowerShell za Voditelj resursa"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure opterećenja, dvostruki snop, javnu ip, izvorni ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Početak rada prilikom stvaranja internetsku nasuprotne opterećenja s IPv6 pomoću komponente PowerShell za Voditelj resursa

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure EŽA](./load-balancer-ipv6-internet-cli.md)
- [Predložak](./load-balancer-ipv6-internet-template.md)

Azure opterećenja je opterećenja za sloj-4 (TCP, UDP). Raspoređivača opterećenja nudi visoke dostupnosti raspodijeliti dolazne promet među instanci dobar servisa u oblaku servise ili virtualnim strojevima u skupu raspoređivača opterećenja. Azure opterećenja također možete prikazati tih servisa na više priključaka, više IP adresa ili i jedno i drugo.

## <a name="example-deployment-scenario"></a>Primjer implementacije scenarija

Sljedeći dijagram prikazuje rješenje uvodi u ovom članku za ujednačavanje opterećenja.

![Scenarij raspoređivača učitavanja](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

U ovom scenariju ćete stvoriti u sljedećim resursima Azure:

- mjesto na Internetu raspoređivača opterećenja s na IPv4 i IPv6 javnu IP adresa
- dva učitavanje ujednačavanje pravila za mapiranje javno VIPs privatne krajnje točke
- Dostupnost postavljena na koji sadrži dvije VMs
- dva virtualnim strojevima (VMs)
- virtualne mreže sučelje za svaki VM IPv4 i IPv6 adrese dodijeljeno

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Implementacija rješenja pomoću komponente PowerShell Azure

Na sljedeći način prikažite kako stvoriti internetsku nasuprotne opterećenja Voditelj resursa Azure pomoću komponente PowerShell. Pomoću upravitelja Azure resursa, svaki resurs stvara i konfigurirali pojedinačno, zatim stavi zajedno da biste stvorili resurs.

Da biste implementirali raspoređivača opterećenja, stvaranje i konfiguriranje sljedeće objekte:

- Pristupne Konfiguracija IP - sadrži javnu IP adresa za dolazne mrežni promet.
- Skupna pozadinsku adresa – sadrži sučelje mreže (NIC-ovi) za virtualnim strojevima mrežni promet primanje raspoređivača opterećenja.
- Opterećenja pravila - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak u pozadinskoj adresu.
- Ulazna pravila NAT - sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak za određene virtualnog računala u pozadinskoj adresu.
- Probes – sadrži stanje probes koristi da biste provjerili dostupnost virtualnim strojevima instanci u pozadinskoj adresu.

Dodatne informacije potražite u članku [Upravljanje resursima Azure podrške za raspoređivača opterećenja](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Postavljanje ljuske PowerShell za korištenje Voditelj resursa

Provjerite je li najnoviju verziju radnog modul Azure Voditelj resursa za PowerShell.

1. Prijavite se u Azure

        Login-AzureRmAccount

    Unesite vjerodajnice kada se to od vas zatraži.

2. Provjera pretplate za račun

        Get-AzureRmSubscription

3. Odabir pretplate Azure da biste koristili.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Stvaranje grupe resursa (preskoči ovaj korak ako koristi postojeću grupu resursa)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Stvaranje virtualne mreže i javnu IP adresa za sučelja Skupna IP

1. Stvaranje virtualne mreže s podmreži.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Stvaranje Azure javnu IP adrese (TOČAKA) resursi za sučelja Skupna IP adresa.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Raspoređivača opterećenja koristi oznaku domene javnu IP kao prefiks za njegov FQDN. U ovom primjeru u certifikatom su *lbnrpipv4.westus.cloudapp.azure.com* i *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Stvaranje Front-End IP konfiguracija i na skupna pozadinsku adresa

1. Stvaranje konfiguracije sučelja adresu koja koristi javnu IP adresa koju ste stvorili.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Stvaranje grupe pozadinske adresu.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Stvaranje LB pravila, NAT pravila, u probni i raspoređivača opterećenja

U ovom se primjeru stvara sljedećih stavki:

- NAT pravilo da biste preveli sve dolazne promet na priključak 443 priključak 4443
- pravilo raspoređivača opterećenja za saldo sve dolazne promet na priključak 80 priključak 80 na adrese u pozadinskoj.
- pravilo raspoređivača opterećenja omogućuje povezivanje RDP VMs na priključak 3389.
- Probni pravilo da biste provjerili status stanja na stranicu pod nazivom *HealthProbe.aspx* ili servisa na priključak 8080
- opterećenja koji koristi tih objekata

1. Stvaranje pravila za NAT.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Stvaranje stanja probni. Konfiguriranje programa probni na dva načina:

    HTTP probni

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    ili TCP probni

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    U ovom primjeru smo namjeravate koristiti TCP probes.

3. Stvorite pravilo raspoređivača opterećenja.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Stvaranje opterećenja pomoću prethodno stvorena objekata.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Stvaranje NIC-ovi za pozadinsku VMs

1. Zatražite virtualne mreže i virtualne mreže podmreže kojima se NIC-ovi morati stvoriti.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Stvaranje IP konfiguracije i NIC-ovi za na VMs.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Stvaranje virtualnim strojevima i dodjela novostvorenu mrežne kartice

Dodatne informacije o stvaranju na VM potražite u članku [Stvori i prethodno konfigurirati virtualnog računala za Windows s resursima i Azure PowerShell](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Stvorite račun za postavljanje dostupnosti i prostora za pohranu

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Stvaranje svaki VM i dodjela prethodnih stvorili NIC-ovi

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-ps.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
