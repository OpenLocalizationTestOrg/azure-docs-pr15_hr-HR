<properties
   pageTitle="Stvaranje opterećenja na mjesto na internetu u upravitelju resursa pomoću komponente PowerShell | Microsoft Azure"
   description="Saznajte kako stvoriti na mjesto na Internetu opterećenja u upravitelju resursa pomoću komponente PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="get-started-article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
   ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="get-started"></a>Stvaranje opterećenja na mjesto na internetu u upravitelju resursa pomoću komponente PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model implementacije Voditelj resursa. Možete i [Saznajte kako stvoriti mjesto na Internetu opterećenja pomoću klasične implementacije modela](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Implementacija rješenja pomoću komponente PowerShell Azure

Sljedeći postupci objašnjavaju kako stvoriti raspoređivača opterećenja na mjesto na Internetu pomoću upravitelja resursa Azure PowerShell. Pomoću upravitelja Azure resursa, svaki resurs je stvorio i pojedinačno konfigurirani, a zatim stavite zajedno da biste stvorili raspoređivača opterećenja.

Morate stvoriti i konfigurirati sljedeće objekte za implementaciju raspoređivača opterećenja:

- Pristupne IP konfiguracija: sadrži javnu IP (TOČAKA) adrese za dolazne mrežni promet.
- Skupna pozadinsku adresa: sadrži sučelje mreže (NIC-ovi) za virtualnim strojevima mrežni promet primanje raspoređivača opterećenja.
- Pravila za ujednačavanje opterećenja: sadrži pravila koja mapiranje javno priključak na raspoređivača opterećenja priključak u pozadinskoj adresu.
- Ulazna pravila NAT: sadrži pravila koja mapiranje javno priključak na raspoređivača opterećenja priključak za određene virtualnog računala u pozadinskoj adresu.
- Probes: sadrži stanje probes koristi da biste provjerili dostupnost instanci virtualnog računala u pozadinskoj adresu.

Dodatne informacije potražite u članku [Upravljanje resursima Azure podrške za raspoređivača opterećenja](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Postavljanje ljuske PowerShell za korištenje Voditelj resursa

Provjerite je li najnoviju verziju radnog modul Azure Voditelj resursa za PowerShell:

1. Prijavite se u Azure.

        Login-AzureRmAccount

    Unesite vjerodajnice kada se to od vas zatraži.

2. Provjerite pretplate za račun.

        Get-AzureRmSubscription

3. Odabir pretplate Azure da biste koristili.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Stvorite grupu resursa. (Preskočite ovaj korak ako koristite postojeću grupu resursa.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Stvaranje virtualne mreže i javnu IP adresa za sučelja Skupna IP

1. Stvorite podmreži i virtualne mreže.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Stvaranje programa Azure javnu IP adresa resursa, pod nazivom **PublicIP**, koriste sučelja Skupna IP s nazivom DNS **loadbalancernrp.westus.cloudapp.azure.com**. Sljedeća naredba koristi vrstu statične dodijeljeni.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Raspoređivača opterećenja koristi oznaku domene javnu IP kao prefiks za njegov FQDN. Time se razlikuje od model klasični implementacije koju koristi servis u oblaku kao raspoređivača opterećenja FQDN.
    >U ovom se primjeru FQDN je **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Stvaranje pristupne Skupna IP i na skupna pozadinsku adresa

1. Stvaranje pristupne Skupna IP pod nazivom **LB sučelju** koja koristi **PublicIp** resursa.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Stvorite na skupna pozadinsku adresa pod nazivom **LB pozadinskog**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Stvaranje pravila NAT, pravila raspoređivača opterećenja, na probni i raspoređivača opterećenja

U ovom se primjeru stvara sljedećih stavki:

- NAT pravilo da biste preveli sve dolazne promet na priključak 3441 priključak 3389
- NAT pravilo da biste preveli sve dolazne promet na priključak 3442 priključak 3389
- Probni pravilo da biste provjerili status stanja na stranicu pod nazivom **HealthProbe.aspx**
- Saldo sve dolazne promet na priključak 80 priključak 80 na adrese u pozadinskoj pravilo raspoređivača učitavanja
- Opterećenja koji koristi tih objekata

Slijedite ove korake:

1. Stvaranje pravila za NAT.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Stvaranje stanja probni. Konfiguriranje programa probni na dva načina:

    HTTP probni

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    TCP probni

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Stvorite pravilo raspoređivača opterećenja.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Stvaranje raspoređivača opterećenja pomoću prethodno stvorena objekata.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Stvaranje NIC-ovi

Stvaranje sučelje mreže (ili izmijeniti postojeće), a zatim pridružiti NAT pravila, pravila raspoređivača opterećenja i probes:

1. Zatražite virtualne mreže i podmreži virtualne mreže tamo gdje se na NIC-ovi potreban će biti stvoren.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Stvaranje NIC pod nazivom **lb-nic1-biti**, i povezati je s prvo pravilo NAT i na prvi (i samo) adresu pozadinske resurse.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Stvaranje NIC pod nazivom **lb-nic2-biti**, i povezati je s drugo pravilo NAT i na prvi (i samo) adresu pozadinske resurse.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Provjerite na NIC-ovi.

        $backendnic1

    Očekivani izlaz:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Korištenje na `Add-AzureRmVMNetworkInterface` cmdlet na NIC-ovi dodijeliti različite VMs.

## <a name="create-a-virtual-machine"></a>Stvaranje virtualnog računala

Upute o stvaranju virtualnog računala i dodjela na NIC potražite u članku [Stvaranje programa VM Azure pomoću komponente PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Dodavanje mrežnog sučelja raspoređivača opterećenja

1. Dohvaćanje raspoređivača opterećenja iz Azure.

    Učitavanje resursa raspoređivača opterećenja u varijablu (Ako dosad niste koji još). Varijabla zove **$lb**. Koristite iste nazive resursa raspoređivača opterećenja koji ste prethodno stvorili.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Učitavanje konfiguracije pozadinske tjednog prikaza kalendara.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Učitavanje već stvoreni mrežnog sučelja u varijablu. Naziv varijable je **$nic**. Naziv mreže sučelja je istu onu iz starije primjera.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Promjena pozadinske konfiguracija mrežnog sučelja.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Spremanje objekta sučelja mreže.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Kada dodate mrežno sučelje resurse za pozadinsku raspoređivača opterećenja se pokreće primanje mrežni promet na temelju pravila za ujednačavanje opterećenja za resursa raspoređivača opterećenja.

## <a name="update-an-existing-load-balancer"></a>Ažuriranje postojećih raspoređivača opterećenja

1. Pomoću opterećenja iz starije primjera na objekt raspoređivača opterećenja varijable **$slb** pomoću dodjeljuje `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. U sljedećem primjeru Dodavanje ulaznog NAT pravilo – pomoću priključak 81 u sučelja i priključak 8181 za skup pozadinske – da biste postojeću opterećenja.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Spremite novu konfiguraciju pomoću `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Uklanjanje raspoređivača opterećenja

Pomoću naredbe `Remove-AzureLoadBalancer` da biste izbrisali prethodno stvorena raspoređivača opterećenja koji se naziva **NRP LB** u grupu resursa pod nazivom **NRP ru**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Možete koristiti dodatni parametar **– prisilno** da biste izbjegli upita za brisanje.

## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-ps.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)
