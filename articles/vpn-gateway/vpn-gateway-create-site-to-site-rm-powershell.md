<properties
   pageTitle="Stvaranje virtualne mreže s web-mjesto VPN vezu pomoću upravitelja resursa Azure i PowerShell | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz stvaranje VNet pomoću modela implementacije Voditelj resursa i povezivanje s mrežom lokalne lokalnog pomoću S2S VPN veza pristupnika."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Stvaranje na VNet s vezom za web-mjesto pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasični – klasični Portal](vpn-gateway-site-to-site-create.md)

U ovom se članku vodit će vas voditi kroz stvaranje virtualne mreže i web-mjesto VPN vezu pristupnika lokalne mreže pomoću modela implementaciju upravljanja resursima Azure. Veza web-mjesto koje se mogu koristiti za više lokalni i Hibridni konfiguracije.

![Dijagram web-mjesto] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "web-mjesto") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Uvođenje modela i postupci za veze na web-mjesto

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Sljedeća tablica prikazuje trenutno dostupno implementacije modela i postupci za konfiguracije web-mjesto. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatna konfiguracija

Ako želite da se povežu VNets, ali stvarate vezu s lokalne lokacije, potražite u članku [Konfiguriranje VNet VNet veze](vpn-gateway-vnet-vnet-rm-ps.md). Ako želite dodati vezu web-mjesto VNet koji već ima veze, potražite u članku [Dodavanje veze S2S VNet s postojeće veze pristupnika VPN-a](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Prije početka

Provjerite imate li sljedeće stavke prije početka konfiguracije.

- Kompatibilni uređaj VPN-a i nekome tko se može konfigurirati. Potražite u članku [o uređajima VPN-a](vpn-gateway-about-vpn-devices.md). Ako ne poznajete konfiguriranje uređaju VPN, ili nisu poznati s IP adresom raspona koji se nalazi u vaše lokalne mreže konfiguracije, potrebno je koordinaciji s nekim tko može zatražiti te detalje za vas.

- Vanjsko nasuprotne javnu IP adresu za VPN uređaj. U ovom IP adresa nije ga moguće pronaći iza na NAT.
    
- Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).
    
- Najnovija verzija cmdleta Azure resursima PowerShell. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .


## <a name="Login"></a>1. povezati pretplate 

Provjerite je li se prebacite na način PowerShell da bi koristio Cmdlete Voditelj resursa. Dodatne informacije potražite u članku [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

Otvorite konzole za PowerShell i povezati s računom. Pomoću sljedeće ogledne možete povezati:

    Login-AzureRmAccount

Provjerite pretplate za račun.

    Get-AzureRmSubscription 

Navedite pretplatu u koju želite koristiti.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. stvaranje virtualne mreže i podmreži pristupnika

Primjeri koriste podmreži pristupnika od /28. Dok je moguće da biste stvorili pristupnik podmreže što manje /29, preporučujemo da stvorite veće podmreže koja sadrži više adresa tako da odaberete barem /28 ili /27. Tako ćete omogućiti za dovoljno adrese kako bi odgovarao moguće dodatne konfiguracije koji se preporučuje se u budućnosti.

Ako već imate virtualne mreže s podmreži pristupnika koji je/29 ili veće, možete prebaciti da biste [dodali pristupnik za lokalnu mrežu](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Da biste stvorili virtualne mreže i podmreži pristupnika

Sljedeći primjer možete koristiti za stvaranje virtualne mreže i podmreži pristupnika. Zamjenske vrijednosti za vlastitu. 

Prvo, stvorite grupu resursa:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Nakon toga stvorite virtualne mreže. Provjerite je li da za to predviđen adresu navedete ne preklapa neku adresu razmake koje imate lokalne mreže.

Sljedeća Ogledna formula stvara virtualne mreže s nazivom *testvnet* i dva podmreže, jedan pod nazivom *GatewaySubnet* i drugog naziva *Subnet1*. Važno da biste stvorili jedan podmreže pod nazivom posebno *GatewaySubnet*je. Ako ste nazvali je nešto drugo, konfiguraciju veze neće uspjeti. 

Postavite varijabli.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Stvorite na VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Da biste dodali podmreži pristupnika virtualne mreže koje ste već stvorili

Ovaj korak potreban je samo ako je potrebno dodati podmreži pristupnika VNet koju ste prethodno stvorili.

Vašoj podmreži pristupnika možete stvoriti pomoću sljedećih uzorka. Provjerite naziv podmreže pristupnik 'GatewaySubnet'. Ako ste nazvali je nešto drugo, stvorite podmreži, ali Azure ne smatra podmreži pristupnika.

Postavite varijabli.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Stvaranje podmreže pristupnika.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Postavljanje konfiguracije. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>dodavanje pristupnik za lokalnu mrežu

U odjeljku virtualne mreža pristupnika lokalne mreže obično se odnosi lokalne lokacije. Naziv koji Azure možete se referirati na nju, a i Navedite adresu prefiks prostora za pristupnik za lokalnu mrežu dati web-mjesta. 

Azure koristi prefiks IP adresa navedete da biste odredili koji promet da biste poslali lokalne lokacije. To znači da morate navesti svaku adresu prefiks koji želite da se povežu s pristupnikom lokalne mreže. Jednostavno možete ažurirati te prefiksi ako promjene za lokalnu mrežu. 

Kada koristite PowerShell Primjeri, imajte na umu sljedeće:
    
- *GatewayIPAddress* je IP adresa uređaja lokalnog VPN-a. Uređaju VPN-a ne može se nalaziti iza na NAT. 
- *AddressPrefix* je prostor za adresu vaše lokalne.

Da biste dodali pristupnik lokalne mreže s prefiksom jedne adrese:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Da biste dodali pristupnik lokalne mreže s više adresa prefiksi:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Da biste izmijenili IP adresa prefiksi za pristupnik za lokalnu mrežu

Katkad se vaše lokalne mreže pristupnika prefiksi promijeniti. Korake koje poduzmete da biste izmijenili vaše IP adresa prefiksi ovise o tome li stvorite VPN veza pristupnika. U ovom se članku u odjeljku [Izmjena IP adresa prefiksi za pristupnik za lokalnu mrežu](#modify) .


## <a name="PublicIP"></a>4. zahtjev javnu IP adresa za pristupnik za VPN-a

Nakon toga zahtjev javnu IP adresu će se dodijeliti pristupnik za Azure VNet VPN. Nije isti IP adresa koji je dodijeljen uređaju VPN-a Umjesto toga što vam je dodijeljen pristupnika Azure VPN sam. Ne možete navesti IP adresa koji želite koristiti. Dinamički dodijeliti u vašem pristupnik. Koristite ovu IP adresu prilikom konfiguriranja uređaju lokalnog VPN-a da biste se povezali s pristupnikom.

Pristupnik za Azure VPN-a za model upravljanja resursima implementacije trenutno podržava samo javnu IP adresa pomoću metode amortizacije dinamički dodijeljeni. Međutim, to znači promijenit će se s IP adresom. Samo vrijeme promjenama Azure VPN pristupnik IP adrese je kada pristupnika se briše i ponovno stvoriti. Pristupnik javnu IP adresu neće se promijeniti preko promjena veličine, ponovno postavljanje ili druge interne održavanja/nadogradnje pristupnika za Azure VPN-a.

Koristite sljedeće ogledne PowerShell:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. stvaranje pristupnika IP adresa konfiguracija

Konfiguraciju pristupnika definira podmreži i javnu IP adresu da biste koristili. Sljedeća Ogledna koristite da biste stvorili konfiguraciju pristupnika.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. stvaranje pristupnika za virtualne mreže

U ovom ćete koraku stvoriti pristupnika virtualne mreže. Stvaranje pristupnika može potrajati da biste dovršili. Često 45 minuta ili više njih. 

Pomoću sljedeće vrijednosti:

- *-GatewayType* za konfiguraciju za web-mjesto je *VPN-a*. Vrsta pristupnika uvijek je specifične za konfiguraciju koja su implementacije. Ako, na primjer, drugi konfiguracijama pristupnika može zahtijevati - GatewayType ExpressRoute. 

- *-VpnType* može biti *RouteBased* (naziva se dinamički pristupnika u nekim dokumentaciju) ili *PolicyBased* (naziva se statične pristupnika u nekim dokumentaciju). Dodatne informacije o vrstama pristupnika VPN-a potražite u članku [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md#vpntype).
- *-GatewaySku* može biti *Osnovni*, *standardnih*ili *HighPerformance*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. konfiguriranje uređaju VPN-a

Sada morate javnu IP adresu pristupnika virtualne mreže za konfiguriranje uređaju lokalnog VPN-a. Rad s informacije o konfiguraciji određenog proizvođača uređaja. Možete se referirati na [Uređajima VPN-a](vpn-gateway-about-vpn-devices.md) dodatne informacije.

Da biste pronašli javnu IP adresu pristupnika za virtualne mreže, koristite sljedeći primjer:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. stvaranje VPN veza

Zatim stvorite web-mjesto VPN veza između pristupnikom virtualne mreže i uređaju VPN-a. Provjerite vrijednosti zamijeniti vlastitim. Zajednički ključ mora odgovarati vrijednost koja se koristi za konfiguraciju uređaju VPN-a. Primijetit ćete da se `-ConnectionType` za web-mjesto je *IPsec*. 

Postavite varijabli.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Stvaranje veze.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Nakon kratko vrijeme veze će uspostaviti. 

## <a name="toverify"></a>Da biste provjerili VPN veza

Postoje na nekoliko različitih načina da biste provjerili VPN veza.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Da biste izmijenili IP adresa prefiksi za pristupnik za lokalnu mrežu

Ako morate promijeniti prefiksi za pristupnik za lokalnu mrežu, koristite sljedeće upute. Navedeni su dva skupa upute. Upute odaberete ovise o tome koje ste već stvorili vezu s pristupnika. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Da biste izmijenili pristupnik IP adresa za pristupnik za lokalnu mrežu

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Daljnji koraci

- Možete dodati virtualnim strojevima virtualne mreže. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

- Informacije o BGP potražite u članku [Pregled BGP](vpn-gateway-bgp-overview.md) i [konfiguriranju BGP](vpn-gateway-bgp-resource-manager-ps.md).

