<properties 
    pageTitle="Simulirani hibridnog okruženja za testiranje oblaka | Microsoft Azure" 
    description="Stvaranje Simulirani hibridnog okruženja oblaka za IT pro ili razvoj testirati pomoću dvije Azure virtualne mreže i VNet VNet veze." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Postavljanje oblaka Simulirani hibridnog okruženja za testiranje

U ovom se članku vodi vas kroz stvaranje Simulirani hibridnog okruženja oblak pomoću Microsoft Azure pomoću dvije Azure virtualne mreže. Evo dobivene konfiguracije.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Simulira hibridnog okruženja za proizvodnju oblaka i sastoji se od:

- Simulirani i pojednostavljeni lokalne mreže smješten u Azure virtualne mreže (virtualne mreže TestLab).
- Lokacija Simulirani-virtualne mreže smješten u Azure (TestVNET).
- VNet VNet veza između dvije virtualne mreže.
- Na sekundarnom kontroler TestVNET virtualne mreže.

Pruža temelj i uobičajenih početni pokažite na kojem možete:

- Razvoj i testiranje aplikacije u oblak Simulirani hibridnog okruženja.
- Stvaranje test konfiguracije računala, neke unutar TestLab virtualne mreže i neke unutar TestVNET virtualne mreže, u programu Publisher hibridnog oblaku IT radnih opterećenja.

Postoje četiri glavne faze postavljanje hibridno okruženje oblaka test:

1.  Konfiguriranje TestLab virtualne mreže.
2.  Stvaranje više lokacija virtualne mreže.
3.  Stvaranje VNet VNet VPN veza.
4.  Konfiguriranje DC2. 

Tu konfiguraciju zahtijeva Azure pretplate. Ako imate pretplatu na MSDN i Visual Studio, potražite u članku [Mjesečna Azure odobrenja za pretplatnike na Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Virtualnim strojevima i virtualne mreže pristupnici Azure plaćati tijeku novčanog trošak kada su pokrenuti. Taj trošak je naplaćeno u odnosu na MSDN ili plaćenu pretplatu. Pristupnik za Azure VPN je implementirana kao skup dva Azure virtualnih računala. Da biste minimizirali troškova, stvorite okruženje za testiranje i najbrže što može izvršiti vaše potrebne testiranje i pokazni.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Faza 1: Konfiguriranje TestLab virtualne mreže

Poslužite se uputama u temi [Osnovni konfiguraciju testirajte okruženje](https://technet.microsoft.com/library/mt771177.aspx) za konfiguriranje računala DC1, APP1 i CLIENT1 u Azure virtualne mreže pod nazivom TestLab. 

Nakon toga pokrenite odzivnik Azure PowerShell.

> [AZURE.NOTE] Sljedeća naredba postavlja korištenje Azure PowerShell 1.0 i noviji.

Prijavite se na račun.

    Login-AzureRMAccount

Pronađite naziv svoje pretplate pomoću sljedeće naredbe.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Postavite Azure pretplatu. Koristite iste pretplate na koji ste koristili za stvaranje osnovne konfiguraciju u fazi 1. Zamijenite sve unutar navodnika, uključujući na < i > znakova s točan naziv.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Zatim dodajte podmreži pristupnika TestLab virtualne mreže osnovni konfiguracije, koji će se koristiti za hostiranje Azure pristupnika.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Nakon toga zahtjev javnu IP adresu da biste dodijelili pristupnik za TestLab virtualne mreže.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Nakon toga stvorite pristupnikom.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Imajte na umu da novi pristupnika možete poduzeti 20 minuta ili više da biste stvorili.

Povezivanje s DC1 s vjerodajnicama CORP\User1 na portalu Azure na lokalnom računalu. Da biste konfigurirali CORP domene da bi računala i korisnici koristiti svoje kontrolera lokalne domene za provjeru autentičnosti, pokrenite sljedeće naredbe administratorske komponente Windows PowerShell naredbeni DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Ovo je trenutna konfiguracija.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Faza 2: Stvaranje TestVNET virtualne mreže

Prvo, stvorite TestVNET virtualne mreže i zaštita s mreže sigurnosne grupe.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Nakon toga zahtjev javnu IP adresu možete dodijeliti pristupnik za TestVNET virtualne mreže i stvoriti pristupnikom.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Ovo je trenutna konfiguracija.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Faza 3: Stvaranje VNet VNet veze

Najprije nabavite slučajni jake, 32 znaka zajednički ključ od administratora mreže ili sigurnost. Umjesto toga koristite podatke na [Stvaranje slučajni niz ključ koji IPsec](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) da biste dobili zajednički ključ.

Da biste stvorili vezu VNet VNet VPN-a koji se može potrajati neko vrijeme da biste dovršili sljedeće, pomoću te naredbe.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Nakon nekoliko minuta, trebali biste uspostaviti vezu.

Ovo je trenutna konfiguracija.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Faza 4: Konfiguriranje DC2

Prvo, stvorite virtualnog računala za DC2. Pokrenite sljedeće naredbe u naredbenom retku Azure PowerShell na lokalnom računalu.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Nakon toga povezati nove DC2 virtualnog računala s portala za Azure.

Nakon toga konfigurirati pravila vatrozida za Windows da dopušta promet za testiranje osnovne. Iz programa administratorske komponente Windows PowerShell naredbenog retka na DC2, pokreće te naredbe.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Naredbe ping moraju rezultirati četiri uspješno Odgovori s IP adresom 10.0.0.4. To je test prometa preko VNet VNet veze.

Nakon toga dodajte dodatni podaci disk na DC2 kao novu jedinicu s slovo F:.

1.  U lijevom oknu Upravitelj poslužitelja, kliknite **datoteka, a servise za pohranu**, a zatim **diskova**.
2.  U oknu sadržaj u grupi **diskova** kliknite **disk 2** (s **particija** postavljena na **Nepoznato**).
3.  Kliknite **Zadaci**, a zatim kliknite **Nova jedinica**.
4.  Na prije početka stranica čarobnjaka novi glasnoće, kliknite **Dalje**.
5.  Na stranici Odabir poslužitelja i na disku, kliknite **2 na disku**, a zatim kliknite **Dalje**. Kada se to od vas zatraži, kliknite **u redu**.
6.  Na stranici navođenje veličina stranice glasnoće, kliknite **Dalje**.
7.  Na stranici Dodjela na stranicu pogon slovo ili mapu, kliknite **Dalje**.
8.  Na stranici Postavke sustava odaberite datoteka, kliknite **Dalje**.
9.  Na stranici Potvrda odabira, kliknite **Stvori**.
10. Po dovršetku kliknite **Zatvori**.

Nakon toga konfigurirati DC2 kao kontroler domene replike corp.contoso.com domene. Te naredbe iz naredbenog retka komponente Windows PowerShell se izvoditi na DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Imajte na umu da se od vas zatraži da CORP\User1 lozinku i direktorij servisa vraćanje način (DSRM) lozinku, a da biste ponovno pokrenuli DC2.

Sad kad TestVNET virtualne mreže ima vlastitu DNS poslužitelja (DC2), morate konfigurirati TestVNET virtualne mreže da biste koristili DNS poslužitelj.

1.  U lijevom oknu portala za Azure, kliknite ikonu virtualne mreže, a zatim **TestVNET**.
2.  Na kartici **Postavke** kliknite **DNS poslužitelji**.
3.  U **primarni DNS poslužitelj**upišite **192.168.0.4** da biste zamijenili 10.0.0.4.
4.  Kliknite **Spremi**.

Ovo je trenutna konfiguracija. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Sada je spremna za testiranje Simulirani hibridno okruženje oblaka.

## <a name="next-step"></a>Sljedeći korak

- Postavite [redak poslovnoj aplikaciji utemeljen na webu,](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) u ovom okruženju.
