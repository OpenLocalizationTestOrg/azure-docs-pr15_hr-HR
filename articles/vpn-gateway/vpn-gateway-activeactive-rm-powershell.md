<properties
   pageTitle="Kako konfigurirati aktivno aktivno S2S VPN povezivanja s Azure VPN pristupnika pomoću upravitelja resursa Azure i PowerShell | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz konfiguriranje aktivno aktivna veza s Azure VPN pristupnika pomoću upravitelja resursa Azure i PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Konfiguriranje aktivno aktivno S2S VPN veza s Azure VPN pristupnika pomoću upravitelja resursa Azure i komponente PowerShell

U ovom se članku vodit će vas kroz korake da biste stvorili aktivno aktivno-lokacija i VNet VNet veze pomoću model implementacije Voditelj resursa i PowerShell.


**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>O iznimno dostupno više lokacija veze

Da biste postigli visoke dostupnosti mogućnosti povezivanja radi više lokacija i VNet VNet implementaciju pristupnici više VPN-a i uspostaviti više paralelnih veza između mreže i Azure. Pročitajte članak [iznimno dostupno više lokacija i povezivanje VNet VNet](./vpn-gateway-highlyavailable.md) pregled mogućnosti povezivanja i topologije.

Ovaj članak sadrži upute za postavljanje VPN veza aktivno aktivno-lokacija i aktivno aktivna veza između dvije virtualne mreže:

- [Dio 1 – stvaranje i konfiguriranje pristupnik za Azure VPN-a u načinu aktivno aktivno](#aagateway)

- [Dio 2 – uspostavljanje veza aktivno aktivno-lokacija](#aacrossprem)

- [Dio 3 – uspostavljanje aktivno aktivno VNet VNet veza](#aav2v)

- [Dio 4 – ažuriranje postojećih pristupnika između aktivno aktivno i aktivno čekanja](#aaupdate)

Možete kombinirati te zajedno da biste sastavili složenije, Visoko dostupna mrežna topologija koji odgovara vašim potrebama.

>[AZURE.IMPORTANT] Primijetite da način aktivno aktivno funkcionira samo u HighPerformance SKU


## <a name ="aagateway"></a>Dio 1 – stvaranje i konfiguriranje aktivno aktivno VPN pristupnika

Na sljedeći način će konfigurirati pristupnik za Azure VPN u načinima aktivno aktivno. Ključne razlike između pristupnika aktivno aktivno i aktivno čekanja:

- Morate stvoriti dvije konfiguracijama pristupnika IP s dva javna IP adresa
- Morate postaviti zastavicu EnableActiveActiveFeature
- Pristupnik SKU mora biti HighPerformance

Druga svojstva isti su kao aktivna-neaktivna pristupnika. 

### <a name="before-you-begin"></a>Prije početka

- Provjerite je li Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).
    
- Morat ćete instalirati cmdleta Azure resursima PowerShell. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

### <a name="step-1---create-and-configure-vnet1"></a>Korak 1 – stvaranje i konfiguriranje VNet1

#### <a name="1-declare-your-variables"></a>1. deklarirati varijabli

Za ovu vježbu ćete smo najprije deklariranje naš varijabli. U primjeru u nastavku deklarira varijable pomoću vrijednosti za ovu vježbu. Pripazite da zamijenite vrijednosti vlastite prilikom konfiguriranja za proizvodnju. Ove varijable možete koristiti ako koristite kroz korake da biste se upoznali s tom vrstom konfiguracije. Izmjena varijabli, a zatim kopirajte i zalijepite konzole za PowerShell.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. povezati svoju pretplatu i stvorite novu grupu resursa

Provjerite je li se prebacite na način PowerShell da bi koristio Cmdlete Voditelj resursa. Dodatne informacije potražite u članku [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

Otvorite konzole za PowerShell i povezati s računom. Pomoću sljedeće ogledne možete povezati:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. stvaranje TestVNet1

Ogledna ispod stvara virtualne mreže pod nazivom TestVNet1 i tri podmreže, jedan pod nazivom GatewaySubnet, jedan pod nazivom sučelju i jednom pod nazivom pozadinskog. Kada zamjena vrijednosti, važno je naziv vašoj podmreži pristupnika posebno GatewaySubnet. Ako ste nazvali je nešto drugo, vaše stvaranje pristupnika neće uspjeti. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Korak 2 – da biste stvorili pristupnik za VPN-a za TestVNet1 s načinom rada za aktivno aktivno

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. stvorili javnu IP adresa i pristupnik IP konfiguracija

Zahtjev za dva javnu IP adrese će se dodijeliti pristupnika će stvoriti za vaše VNet. Ćete definirati i podmreže i konfiguracija IP obavezno. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. stvaranje pristupnika za VPN-a s konfiguracijom aktivno aktivno

Stvaranje pristupnika virtualne mreže za TestVNet1. Imajte na umu da postoje dvije GatewayIpConfig stavke, a postavlja se zastavica EnableActiveActiveFeature. Način rada s aktivno aktivno zahtijeva VPN utemeljen na usmjeravanje pristupnika od HighPerformance SKU. Stvaranje pristupnika može potrajati neko vrijeme (30 minuta ili više da biste dovršili).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. nabaviti pristupnika javnu IP adrese i BGP ravnopravnih članova IP adresa

Nakon stvaranja pristupnika, morat ćete dobiti BGP ravnopravnih članova IP adresa pristupnika Azure VPN-a. Za konfiguriranje VPN pristupnika Azure kao ravnopravnim BGP za uređaje lokalnog VPN-a potreban je tu adresu.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Koristite sljedeće Cmdlete da biste prikazali dva javnu IP adrese za pristupnik za VPN-a i njihove odgovarajuće BGP ravnopravnih članova IP adresa za svaku instancu pristupnika dodijeljeno:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Redoslijed javnu IP adrese za instance pristupnika i odgovarajuće Peering adrese BGP jednake. U ovom primjeru 10.12.255.4 pristupnika VM s javnu IP 40.112.190.5 će se koristiti kao BGP Peering adresa, a pristupnika s 138.91.156.129 će koristiti 10.12.255.5. Kada postavite na lokalni VPN uređajima povezivanje pristupnika aktivno aktivno nije potrebno taj podatak. Pristupnik je prikazano u dijagramu ispod s sve adrese:

![aktivni aktivno pristupnika](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Nakon stvaranja pristupnika ovaj pristupnika možete koristiti da biste uspostavili aktivno aktivno-lokacija ili VNet VNet veze. U sljedećim se odjeljcima će voditi kroz korake da biste dovršili na vježbu.


## <a name ="aacrossprem"></a>Dio 2 – uspostaviti vezu aktivno aktivno-lokacija

Da biste uspostavili vezu više lokacija potrebnih za stvaranje lokalne mreže pristupnik za predstavljanje uređaju VPN-a na lokalni i vezu za povezivanje pristupnika Azure VPN s pristupnika lokalne mreže. U ovom primjeru pristupnika Azure VPN je u načinu za aktivno aktivno. Kao rezultat, čak i ako postoji samo jedan lokalnog VPN uređaj (lokalne mreže pristupnik) i resursa za jednu vezu, oba instance pristupnika Azure VPN-a stvorit će se S2S VPN tunnels s lokalnim uređajem.

Prije nego što nastavite, provjerite jeste li izvršili [dio 1](#aagateway) ovu vježbu.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Korak 1 – stvaranje i konfiguriranje pristupnika lokalne mreže

#### <a name="1-declare-your-variables"></a>1. deklarirati varijabli

Ovu vježbu će i dalje da biste sastavili konfiguracije prikazano u dijagramu. Pripazite da zamijenite vrijednosti one koje želite koristiti za konfiguraciju.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Nekoliko Napomena vezane uz pristupnika parametara lokalne mreže:

- Pristupnik za lokalnu mrežu može se na istom ili drugom mjestu i grupu resursa kao pristupnika VPN-a. U ovom se primjeru pokazuje ih u grupama različite resursa, ali na istom mjestu Azure.

- Ako postoji samo jedan uređaj VPN lokalnog kao što je prikazano gore, aktivno aktivna veza možete raditi sa ili bez BGP protokol. U ovom se primjeru koristi BGP više lokacija veze.

- Ako je omogućeno BGP, prefiksa morate deklarirati za pristupnik za lokalnu mrežu je adresa glavnog računala za BGP ravnopravnih članova IP adrese na uređaju VPN-a. U ovom slučaju je na /32 prefiks "10.52.255.253/32".

- Kao podsjetnik, morate koristiti različite ASNs BGP između lokalne mreže i Azure VNet. Ako su isti, morate promijeniti svoje ASN VNet ako uređaju VPN lokalnog već koristi u ASN prema s drugim BGP susjeda.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. stvaranje pristupnika za lokalne mreže za Site5
    
Prije nego što nastavite, provjerite jeste li povezani s pretplatom 1. Ako još niste stvorili, stvorite grupu resursa.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Korak 2 – povezivanje pristupnika VNet i lokalne mreže pristupnik

#### <a name="1-get-the-two-gateways"></a>1. se dva pristupnika

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. stvaranje TestVNet1 Site5 vezu

U ovom ćete koraku će stvorite vezu s TestVNet1 Site5_1 s "EnableBGP" Postavljanje $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN-a i BGP parametara za svoj uređaj lokalnog VPN-a

U primjeru u nastavku navedeni parametri ćete unijeti u odjeljku Konfiguriranje BGP na uređaju lokalnog VPN-a za ovu vježbu:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Prefiksi objaviti: (na primjer) 10.51.0.0/16 i 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 za tunelom za 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 za tunelom za 138.91.156.129
    - Statički smjerovi: 10.12.255.4/32 odredište, nexthop tunelom VPN-a za 40.112.190.5 sučelja 10.12.255.5/32 odredište, nexthop tunelom VPN sučelja za 138.91.156.129
    - eBGP Multihop: Provjerite mogućnost "multihop" za eBGP omogućena na uređaju ako je potrebno

Trebali biste uspostaviti vezu nakon nekoliko minuta, a BGP peering sesiju počet će uspostavljanja IPsec vezu. U ovom se primjeru dosad konfigurirao samo jedan uređaj VPN-a na lokalni, rezultirajuća u dijagramu u nastavku:

![aktivni aktivno crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Korak 3 – povezivanje dva uređaja VPN-a na lokalni pristupnika aktivno aktivno VPN-a

Ako imate dva uređaja VPN-a na istu lokalnu mrežu, možete postići dvostruki zalihosti povezivanjem Azure VPN pristupnika na drugi uređaj VPN-a.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. stvaranje pristupnika za drugi lokalne mreže za Site5

Imajte na umu da pristupnik IP adresa, adresa prefiks i BGP peering adresa za drugi pristupnik za lokalnu mrežu morate preklapa s prethodne pristupnika lokalne mreže za istu lokalne mreže. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. VNet pristupnika i povezivanje drugi pristupnika lokalne mreže

Stvaranje veze s TestVNet1 Site5_2 s "EnableBGP" Postavljanje $True

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN-a i BGP parametara za drugi uređaj lokalnog VPN-a

Isto tako, ispod popisa parametre ćete unijeti u drugi uređaj VPN:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Prefiksi objaviti: (na primjer) 10.51.0.0/16 i 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 za tunelom za 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 za tunelom za 138.91.156.129
    - Statički smjerovi: 10.12.255.4/32 odredište, nexthop tunelom VPN-a za 40.112.190.5 sučelja 10.12.255.5/32 odredište, nexthop tunelom VPN sučelja za 138.91.156.129
    - eBGP Multihop: Provjerite mogućnost "multihop" za eBGP omogućena na uređaju ako je potrebno

Kada se uspostaviti vezu (tunnels), imat ćete dva suvišnih VPN uređaja i tunnels povezivanje lokalne mreže i Azure:

![lišće zalihosti crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Dio 3 – uspostaviti vezu VNet VNet aktivno aktivno

U ovom se odjeljku stvara aktivno aktivno VNet VNet vezu s BGP. 

Upute u nastavku nastaviti s prethodne korake navedene. Morate dovršiti [dio 1](#aagateway) za stvaranje i konfiguriranje web-mjesto TestVNet1 i u okvir za pristupnik za VPN-a pomoću BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Korak 1 – stvaranje TestVNet2 i pristupnika VPN-a

Nije važno da biste bili sigurni da prostor IP adrese novi virtualne mreže, TestVNet2, ne preklapa s bilo kojom VNet raspona.

U ovom primjeru virtualne mreže pripada iste pretplate. Možete postaviti VNet VNet veza između različitih pretplate; Pogledajte [Konfiguriraj VNet VNet vezu](./vpn-gateway-vnet-vnet-rm-ps.md) da biste saznali više pojedinosti. Provjerite je li dodate na "-EnableBgp $True" prilikom stvaranja veze da biste omogućili BGP.

#### <a name="1-declare-your-variables"></a>1. deklarirati varijabli

Pripazite da zamijenite vrijednosti one koje želite koristiti za konfiguraciju.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. stvaranje TestVNet2 u novu grupu resursa

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. stvaranje pristupnika za VPN aktivno aktivno za TestVNet2

Zahtjev za dva javnu IP adrese će se dodijeliti pristupnika će stvoriti za vaše VNet. Ćete definirati i podmreže i konfiguracija IP obavezno. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Stvaranje pristupnika VPN-a s brojem kao i zastavice "EnableActiveActiveFeature". Imajte na umu morate nadjačali zadane ASN na vaše Azure VPN pristupnika. ASNs za povezanog VNets moraju biti različiti da biste omogućili BGP i prijenosa usmjeravanja.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Korak 2 – povezivanje pristupnika TestVNet1 i TestVNet2

U ovom primjeru oba pristupnika su u okviru iste pretplate. Dovršite ovaj korak u istom sesiju ljuske PowerShell.

#### <a name="1-get-both-gateways"></a>1. se oba pristupnika

Provjerite je li prijaviti i povezivanje s pretplatom 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. stvaranje oba veza

U ovom ćete koraku ćete stvoriti vezu TestVNet1 TestVNet2 i veze s TestVNet2 TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Ne zaboravite da biste omogućili BGP za oba veze.

Nakon tih koraka, veza stvorit će se u nekoliko minuta, a na BGP peering sesije bit će se kada se veza VNet VNet dovrši s dva zalihosti:

![aktivni aktivno v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Dio 4 – ažuriranje postojećih pristupnika između aktivno aktivno i aktivno čekanja

U odjeljku zadnju će opisuju kako konfigurirati postojeće Azure VPN pristupnika iz Aktivno čekanja aktivno aktivno način i obratno.

>[AZURE.IMPORTANT] Primijetite da način aktivno aktivno funkcionira samo u HighPerformance SKU

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Konfigurirati pristupnik za aktivno čekanja za aktivno aktivno pristupnika

#### <a name="1-gateway-parameters"></a>1. parametri pristupnika

U sljedećem primjeru pretvara pristupnik za aktivno čekanja u pristupnik za aktivno aktivno. Morate stvoriti drugi javnu IP adresu, a zatim dodajte drugi konfiguracije pristupnika IP. Ispod prikazuje parametre koristi:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. stvorili javnu IP adresu, a zatim dodajte drugi IP konfiguraciju pristupnika

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. Omogućivanje načina rada za aktivno aktivno i ažuriranje pristupnika

Postavite objekt pristupnika ljuske PowerShell za pokretanje stvarni ažuriranja. SKU objekt pristupnika mora se promijeniti i u HighPerformance jer je stvoren prethodno kao standardno.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

To ažuriranje može potrajati od 30-45 minuta.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Konfigurirati pristupnik za aktivno aktivan u pristupnik za aktivno čekanja

#### <a name="1-gateway-parameters"></a>1. parametri pristupnika

Koristite iste parametre kao gore, dobiti naziv IP konfiguracije koji želite ukloniti.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. uklonite IP konfiguracije pristupnika i onemogućiti način rada aktivno aktivno

Isto tako, morate postaviti pristupnika objekta u ljuske PowerShell za pokretanje stvarni ažuriranja.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

To ažuriranje može potrajati do 30 45 minuta.


## <a name="next-steps"></a>Daljnji koraci

Nakon dovršetka vezu, možete dodati virtualnim strojevima virtualne mreže. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

