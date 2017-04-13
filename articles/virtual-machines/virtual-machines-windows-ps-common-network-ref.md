<properties
    pageTitle="Zajednički mrežni PowerShell naredbe za VMs | Microsoft Azure"
    description="Uobičajene naredbe ljuske PowerShell za pokretanje stvaranja virtualne mreže i njegove pridružene resurse za VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Uobičajene naredbe ljuske PowerShell Azure mreže za VMs

Ako želite stvoriti virtualnog računala, morate stvoriti [virtualne mreže](../virtual-network/virtual-networks-overview.md) ili zna u kojem je moguće dodati u VM postojeće virtualne mreže. Obično prilikom stvaranja na VM trebate razmislite o stvaranju resursi opisane u ovom članku.

Informacije o instalacija najnovije verzije sustava Azure PowerShell, Odabir pretplate i prijave na račun potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-network-resources"></a>Stvaranje mrežni resursi

Zadatak | Naredba 
-------------- | -------------------------
Stvaranje konfiguracije podmreže | $subnet1 = [Novo AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -naziv "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$subnet2 = novo AzureRmVirtualNetworkSubnetConfig-naziv "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Uobičajeni mrežom možda podmreže za programa [internet nasuprotne raspoređivača opterećenja](../load-balancer/load-balancer-internet-overview.md) i zasebnom podmreže za [interne raspoređivača opterećenja](../load-balancer/load-balancer-internal-overview.md). |
Stvaranje virtualne mreže | $vnet = [Novo AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -naziv "virtual_network_name" - ResourceGroupName "resource_group_name"-mjesto "location_name" - AddressPrefix XX. X.X.X/XX-podmreže $subnet1, $subnet2
Test za naziv jedinstveni domene | [Test AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "Naziv_domene"-mjesto "location_name"<BR><BR>Možete navesti naziv DNS domene za [javno IP resursa](../virtual-network/virtual-network-ip-addresses-overview-arm.md), čime se mapiranje za domainname.location.cloudapp.azure.com na javnu IP adresu u Azure upravlja DNS poslužitelji. Naziv može sadržavati samo slova, brojeve i spojnice. Znak imena i prezimena mora biti slovo ili broj i naziv domene mora biti jedinstvena u položaju Azure. Ako je **True** , vraća se, globalno jedinstven je predloženog naziva.
Stvaranje javne IP adrese | $pip = [Novo AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -naziv "ip_address_name" - ResourceGroupName "resource_group_name" - DomainNameLabel "Naziv_domene"-mjesto "location_name" - AllocationMethod dinamičkim<BR><BR>Na javnu IP adresu koristi naziv domene koju ste prethodno testirati i koristi sučelju konfiguracije raspoređivača opterećenja.
Stvaranje sučelju IP konfiguracija | $frontendIP = [Novo AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -naziv "frontend_ip_name" - PublicIpAddress $pip<BR><BR>Konfiguriranje sučelju obuhvaća javnu IP adresu koju ste prethodno stvorili za dolazne mrežni promet.
Stvaranje skupna adresa za pozadinskog | $beAddressPool = [Novo AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -naziv "backend_pool_name"<BR><BR>Nudi interne adrese za pozadinskog raspoređivača opterećenja koji je moguće pristupiti putem mreže sučelja.
Stvaranje na probni | $healthProbe = [Novo AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -naziv "probe_name" - RequestPath 'HealthProbe.aspx'-protokol http-priključak 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Sadrži probes stanja koristiti da biste provjerili dostupnost virtualnim strojevima instanci u skupna adresa pozadinskog.
Stvaranje na pravila za ujednačavanje opterećenja | $lbRule = [Novo AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -naziv HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-isprobati $healthProbe-protokolom Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Sadrži pravila koja će se dodijeliti javno priključak raspoređivača opterećenja priključak u skupna adresa pozadinskog.
Stvaranje ulazna pravila za NAT | $inboundNATRule = [Novo AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -naziv "rule_name" - FrontendIpConfiguration $frontendIP-protokolom TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Sadrži pravila mapiranja javno priključak na raspoređivača opterećenja priključak za određene virtualnog računala u skupna adresa pozadinskog.
Stvaranje raspoređivača opterećenja | $loadBalancer = "resource_group_name" [Novo AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName-naziv "load_balancer_name"-mjesto "location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-isprobati $healthProbe
Stvaranje mrežnog sučelja | $nic1 = "resource_group_name" [Novo AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName-naziv "network_interface_name"-mjesto "location_name" - PrivateIpAddress XX. X.X.X-podmreže subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Stvorite mrežno sučelje pomoću javnu IP adresa i podmreže virtualne mreže koju ste prethodno stvorili.
    
## <a name="get-information-about-network-resources"></a>Informacije o mrežni resursi

Zadatak | Naredba 
-------------- | -------------------------
Popis virtualne mreže | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Navodi sve virtualne mreže u grupu resursa.
Informacije o virtualne mreže | Get-AzureRmVirtualNetwork-naziv "virtual_network_name" - ResourceGroupName "resource_group_name"
Popis podmreže u virtualne mreže | Get-AzureRmVirtualNetwork-naziv - ResourceGroupName "resource_group_name"virtual_network_name"" & #124; Odaberite podmreže
Informacije o podmreži | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -naziv "subnet_name" - VirtualNetwork $vnet<BR><BR>Dohvaća podatke o podmreži u navedenom virtualne mreže. Vrijednost $vnet predstavlja objekt koji je vratio Get-AzureRmVirtualNetwork.
Popis IP adresa | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Popis javnu IP adresa u grupu resursa.
Popis opterećenja balancers | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Popis svih učitavanja balancers grupu resursa.
Sučelje za popis mreže | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Popis svih sučelje mreže u grupu resursa.
Informacije o mrežnom sučelju | Get-AzureRmNetworkInterface-naziv "network_interface_name" - ResourceGroupName "resource_group_name"<BR><BR>Dohvaća podatke o određenim mrežno sučelje.
Pribavljanje konfiguracije IP mrežno sučelje | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -naziv "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Dohvaća podatke o konfiguraciji IP navedeni mrežnog sučelja. Vrijednost $nic predstavlja objekt koji je vratio Get-AzureRmNetworkInterface.

## <a name="manage-network-resources"></a>Upravljanje mrežni resursi

Zadatak | Naredba 
-------------- | -------------------------
Dodavanje podmreži virtualne mreže | [Dodavanje AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX-naziv "subnet_name" - VirtualNetwork $vnet<BR><BR>Dodaje podmreži postojeće virtualne mreže. Vrijednost $vnet predstavlja objekt koji je vratio Get-AzureRmVirtualNetwork.
Brisanje virtualne mreže | [Uklanjanje AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -naziv "virtual_network_name" - ResourceGroupName "resource_group_name"<BR><BR>Uklanja navedeni virtualne mreže iz grupe resursa.
Brisanje mrežno sučelje | [Uklanjanje AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -naziv "network_interface_name" - ResourceGroupName "resource_group_name"<BR><BR>Uklanja navedeni mrežnog sučelja iz grupe resursa.
Brisanje raspoređivača opterećenja | [Uklanjanje AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -naziv "load_balancer_name" - ResourceGroupName "resource_group_name"<BR><BR>Uklanja navedeni opterećenja iz grupe resursa.
Brisanje javnu IP adresa | [Uklanjanje AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-naziv "ip_address_name" - ResourceGroupName "resource_group_name"<BR><BR>Uklanja navedeni javnu IP adresu iz grupe resursa.

## <a name="next-steps"></a>Daljnji koraci

- Korištenje mrežnog sučelja koji ste upravo stvorili kada [stvorite na VM](virtual-machines-windows-ps-create.md).
- Informirajte se o tome kako možete [stvoriti VM s više mreža sučelja](../virtual-network/virtual-networks-multiple-nics.md).
