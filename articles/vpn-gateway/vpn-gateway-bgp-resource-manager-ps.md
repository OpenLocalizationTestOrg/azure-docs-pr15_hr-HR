<properties
   pageTitle="Kako konfigurirati BGP na Azure VPN pristupnika pomoću upravitelja resursa Azure i PowerShell | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz konfiguriranje BGP s Azure VPN pristupnika pomoću upravitelja resursa Azure i PowerShell."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Kako konfigurirati BGP na Azure VPN pristupnika pomoću upravitelja resursa Azure i komponente PowerShell

U ovom se članku vodit će vas kroz korake da biste omogućili BGP na web-lokacija VPN-a web-mjesta web-na-mjesta (S2S) i VNet VNet veze pomoću model implementacije Voditelj resursa i PowerShell.


**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>O BGP

BGP je standardni protokol usmjeravanja obično se koristi u Interneta razmjenu usmjeravanje i reachability informacija između dvije ili više mreža. BGP omogućuje VPN pristupnika Azure i VPN uređaje lokalnog, pod nazivom BGP kolegama ili susjeda, razmjenu "usmjerava" koji će obavijestiti oba pristupnika na dostupnosti i reachability za te prefiksi prođite kroz pristupnika ili usmjerivača koji je uključen. BGP možete omogućiti prijenosa usmjeravanje među više mreža tako da prijenos usmjerava pristupnik BGP Pratit će iz jednog BGP ravnopravnih članova na druge BGP kolegama.

Za dodatne rasprave na pogodnosti BGP i da biste shvatili Tehnički preduvjeti i napomene korištenja BGP pogledajte [Pregled BGP s Azure VPN pristupnika](./vpn-gateway-bgp-overview.md) .

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Početak rada s BGP na Azure VPN pristupnika

U ovom se članku će vas voditi kroz korake da biste učinili sljedeće zadatke:

- [Dio 1 - Omogući BGP na pristupnik za Azure VPN-a](#enablebgp)

- [Dio 2 – uspostavljanje veze više lokacija s BGP](#crossprembgp)

- [Dio 3 – uspostaviti vezu VNet VNet s BGP](#v2vbgp)

Svaki dio upute čini osnovni sastavni blok za omogućivanje BGP u mrežne veze. Ako dovršite sve tri dijela, kao što je prikazano na sljedećem su dijagramu će Sastavi topologije:

![Topologija BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Možete kombinirati te zajedno da biste sastavili složenije, više hope prijenosa mrežom koja ne odgovara vašim potrebama.

## <a name ="enablebgp"></a>Dio 1 - konfiguriranje BGP na pristupnika Azure VPN-a

Sljedeći koraci za konfiguraciju će postavljanje parametara BGP pristupnika Azure VPN kao što je prikazano na sljedećem su dijagramu:

![BGP pristupnika](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Prije početka

- Provjerite je li Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).
    
- Morat ćete instalirati cmdleta Azure resursima PowerShell. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

### <a name="step-1---create-and-configure-vnet1"></a>Korak 1 – stvaranje i konfiguriranje VNet1 

#### <a name="1-declare-your-variables"></a>1. deklarirati varijabli

Za ovu vježbu ćete smo najprije deklariranje naš varijabli. U primjeru u nastavku deklarira varijable pomoću vrijednosti za ovu vježbu. Pripazite da zamijenite vrijednosti vlastite prilikom konfiguriranja za proizvodnju. Ove varijable možete koristiti ako koristite kroz korake da biste se upoznali s tom vrstom konfiguracije. Izmjena varijabli, a zatim kopirajte i zalijepite konzole za PowerShell.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. povezati svoju pretplatu i stvorite novu grupu resursa

Provjerite je li se prebacite na način PowerShell da bi koristio Cmdlete Voditelj resursa. Dodatne informacije potražite u članku [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

Otvorite konzole za PowerShell i povezati s računom. Pomoću sljedeće ogledne možete povezati:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. stvaranje TestVNet1

Ogledna ispod stvara virtualne mreže pod nazivom TestVNet1 i tri podmreže, jedan pod nazivom GatewaySubnet, jedan pod nazivom sučelju i jednom pod nazivom pozadinskog. Kada zamjena vrijednosti, važno je naziv vašoj podmreži pristupnika Konkretno GatewaySubnet. Ako ste nazvali je nešto drugo, vaše stvaranje pristupnika neće uspjeti. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Korak 2 – da biste stvorili pristupnik za VPN-a za TestVNet1 s BGP parametra

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. stvaranje konfiguracija, a IP podmreže

Zahtjev javnu IP adresu će se dodijeliti pristupnika će stvoriti za vaše VNet. Ćete definirati i podmreže i konfiguracija IP obavezno. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. stvaranje pristupnika za VPN-a s brojem kao

Stvaranje pristupnika virtualne mreže za TestVNet1. Imajte na umu da BGP zahtijeva utemeljen na usmjeravanje VPN pristupnika i zbrajanje parametar - Asn, da biste postavili ASN (kao broj) za TestVNet1. Stvaranje pristupnika može potrajati neko vrijeme (30 minuta ili više da biste dovršili).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. dohvaćanje Azure BGP ravnopravnih članova IP adrese

Nakon stvaranja pristupnika, morat ćete dobiti BGP ravnopravnih članova IP adresa pristupnika Azure VPN-a. Za konfiguriranje VPN pristupnika Azure kao ravnopravnim BGP za uređaje lokalnog VPN-a potreban je tu adresu.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

Posljednje naredbe prikazat će odgovarajuće BGP konfiguracije na VPN pristupnika Azure; Ako, na primjer:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Nakon stvaranja pristupnika ovaj pristupnika možete koristiti da biste uspostavili više lokacija veze ili VNet VNet s BGP. U sljedećim se odjeljcima će voditi kroz korake da biste dovršili na vježbu.

## <a name ="crossprembbgp"></a>Dio 2 – uspostavljanje veze više lokacija s BGP

Da biste uspostavili vezu više lokacija potrebnih za stvaranje lokalne mreže pristupnik za predstavljanje uređaju VPN-a na lokalni i vezu za povezivanje pristupnika Azure VPN s pristupnika lokalne mreže. Razlika između upute u ovom članku je dodatna svojstva potrebne da biste odredili BGP konfiguracijske parametre.

![BGP za više lokacija](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Prije nego što nastavite, provjerite jeste li izvršili [dio 1](#enablebgp) ovu vježbu.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Korak 1 – stvaranje i konfiguriranje pristupnika lokalne mreže

#### <a name="1-declare-your-variables"></a>1. deklarirati varijabli

Ovu vježbu će i dalje da biste sastavili konfiguracije prikazano u dijagramu. Pripazite da zamijenite vrijednosti one koje želite koristiti za konfiguraciju.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Nekoliko Napomena vezane uz pristupnika parametara lokalne mreže:

- Pristupnik za lokalnu mrežu može se na istom ili drugom mjestu i grupu resursa kao pristupnika VPN-a. U ovom se primjeru prikazuju ih u grupe različite resursa na različitim mjestima.

- Minimalna prefiksa morate deklarirati za pristupnik za lokalnu mrežu je adresa glavnog računala za BGP ravnopravnih članova IP adrese na uređaju VPN-a. U ovom slučaju je na /32 prefiks "10.52.255.254/32".

- Kao podsjetnik, morate koristiti različite ASNs BGP između lokalne mreže i Azure VNet. Ako su isti, morate promijeniti svoje ASN VNet ako uređaju VPN lokalnog već pomoću na ASN ravnopravni s drugim BGP susjeda.
    
Prije nego što nastavite, provjerite jeste li povezani s pretplatom 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. stvaranje pristupnika za lokalne mreže za Site5

Ne zaboravite da biste stvorili grupu resursa ako ga se ne stvara, prije stvaranja pristupnik za lokalnu mrežu. Obratite pozornost na dvije dodatne parametre za pristupnik za lokalnu mrežu: Asn i BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Korak 2 – povezivanje pristupnika VNet i lokalne mreže pristupnik

#### <a name="1-get-the-two-gateways"></a>1. se dva pristupnika

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. stvaranje TestVNet1 Site5 vezu

U ovom ćete koraku će stvorite vezu s TestVNet1 Site5. Morate navesti "-EnableBGP $True" da biste omogućili BGP za tu vezu. Kao što je spomenuli, moguće je da bi BGP i koje nisu BGP veze za pristupnik za VPN isti Azure. Ako BGP nije omogućeno u svojstvu veze, Azure neće moći BGP za ovu vezu čak i ako se parametri BGP konfigurirane već na oba pristupnika.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


U primjeru u nastavku navedeni parametri ćete unijeti u odjeljku Konfiguriranje BGP na uređaju lokalnog VPN-a za ovu vježbu:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Prefiksi objaviti: (na primjer) 10.51.0.0/16 i 10.52.0.0/16
    - Azure VNet ASN: 65010
    - IP BGP Azure VNet: 10.12.255.30
    - Statički smjer: dodavanje smjer 10.12.255.30/32, s nexthop se VPN sučelja tunelom na uređaju
    - eBGP Multihop: Provjerite mogućnost "multihop" za eBGP omogućena na uređaju ako je potrebno

Trebali biste uspostaviti vezu nakon nekoliko minuta, a BGP peering sesiju počet će uspostavljanja IPsec vezu.
 
## <a name ="v2vbgp"></a>Dio 3 – uspostaviti vezu VNet VNet s BGP

U ovom se odjeljku dodaje VNet VNet veze sa BGP, kao što je prikazano u dijagramu u nastavku. 

![BGP za VNet VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Upute u nastavku nastaviti s prethodne korake navedene. Dovršite [prvi dio](#enablebgp) za stvaranje i konfiguriranje web-mjesto TestVNet1 i u okvir za pristupnik za VPN-a s BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Korak 1 – stvaranje TestVNet2 i pristupnika VPN-a

Nije važno da biste bili sigurni da prostor IP adrese novi virtualne mreže, TestVNet2, ne preklapa s bilo kojom VNet raspona.

U ovom primjeru virtualne mreže pripada iste pretplate. Možete postaviti VNet VNet veza između različitih pretplate; Pogledajte [Konfiguriraj VNet VNet vezu](./vpn-gateway-vnet-vnet-rm-ps.md) da biste saznali više pojedinosti. Provjerite je li dodate na "-EnableBgp $True" prilikom stvaranja veze da biste omogućili BGP.

#### <a name="1-declare-your-variables"></a>1. deklarirati varijabli

Pripazite da zamijenite vrijednosti one koje želite koristiti za konfiguraciju.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. stvaranje TestVNet2 u novu grupu resursa

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. stvaranje pristupnika za VPN-a za TestVNet2 s BGP parametra

Zahtjev javnu IP adresu će se dodijeliti pristupnika će stvoriti za vaše VNet. Ćete definirati i podmreže i konfiguracija IP obavezno. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Stvaranje pristupnika VPN-a kao broj. Imajte na umu morate nadjačali zadane ASN na vaše Azure VPN pristupnika. ASNs za povezanog VNets moraju biti različiti da biste omogućili BGP i prijenosa usmjeravanja.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Korak 2 – povezivanje pristupnika TestVNet1 i TestVNet2

U ovom primjeru oba pristupnika su u okviru iste pretplate. Dovršite ovaj korak u istom sesiju ljuske PowerShell.

#### <a name="1-get-both-gateways"></a>1. se oba pristupnika

Provjerite je li prijava i povezivanje s pretplatom 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. stvaranje oba veza

U ovom ćete koraku ćete stvoriti vezu TestVNet1 TestVNet2 i veze s TestVNet2 TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Ne zaboravite da biste omogućili BGP za oba veze.

Nakon što izvršite ove upute, veza se stvorit će nekoliko minuta, a BGP peering sesiju bit će se puta VNet VNet veze ne završi.

Ako ste dovršili sve tri dijela ovu vježbu, će ste uspostavili mrežna topologija kao što je prikazano u nastavku:

![BGP za VNet VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Daljnji koraci

Nakon dovršetka vezu, možete dodati virtualnim strojevima virtualne mreže. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

