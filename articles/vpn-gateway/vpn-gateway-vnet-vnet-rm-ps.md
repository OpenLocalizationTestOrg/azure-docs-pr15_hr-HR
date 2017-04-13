<properties
   pageTitle="Povezivanje Azure VNets s VPN pristupnika i PowerShell | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz zajedno povezivanje virtualne mreže pomoću upravitelja resursa Azure i PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Konfiguriranje VNet VNet veze za Voditelj resursa pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasični – klasični Portal](virtual-networks-configure-vnet-to-vnet-connection.md)

U ovom se članku vodit će vas kroz korake za stvaranje veze između VNets u modelu implementaciju upravljanja resursima pomoću pristupnika VPN-a. Virtualne mreže može biti u istom ili drugom regijama i s istim ili različitim pretplata.


![V2V dijagrama](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Uvođenje modela i postupci za VNet VNet veze

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Sljedeća tablica prikazuje trenutno dostupno implementacije modela i postupci za VNet VNet konfiguracije. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>O vezama VNet VNet

Povezivanje virtualne mreže s drugom virtualne mreže (VNet-VNet) slično je povezivanja s VNet na mjesto lokalnog web-mjesta. Pristupnik za Azure VPN obje vrste povezivanja koristite sigurne tunelom pomoću IPsec-IKE. VNets povežete može biti u različitim područjima. Ili u različite pretplate. Možete čak i kombinirati VNet VNet komunikacije s konfiguracijama više web-mjesta. Omogućuje uspostavljanje topologija mreže kojima se kombiniraju-lokacija povezivanje s inter-virtualne mrežne veze, kao što je prikazano na sljedećem su dijagramu:


![O vezama](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Zašto se povezati virtualne mreže?

Preporučujemo vam da biste se povezali virtualne mreže iz sljedećih razloga:

- **Unakrsni zemlj zalihosti regija i prisutnosti za zemlj.**
    - Možete postaviti vlastitu zemlj replikacije ili sinkronizaciju s sigurno povezivanje bez prelaska na mjesto na Internetu krajnje točke.
    - S upraviteljskim promet Azure i opterećenja, možete postaviti iznimno dostupna radno opterećenje s zemlj zalihosti preko više Azure regija. Važno primjer je da biste postavili SQL uvijek na s grupama dostupnost šire preko više Azure regija.

- **Regionalne aplikacije s više razina odvajanja ili administratora granice**
    - Unutar iste područja, možete postaviti više razina aplikacije s više mreža virtualne povezanih zbog odvajanja ili preduvjeti za administrativne.


### <a name="vnet-to-vnet-faq"></a>Najčešća pitanja vezana uz VNet VNet

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Koji se skup koraka koristiti?

U ovom članku potražite dvije različite skupove korake. Jedan skup koraka za [VNets koje se nalaze u okviru iste pretplate](#samesub), a drugi za [VNets koje se nalaze u različite pretplate](#difsub). Ključne razlike između skupova je li možete stvarati i konfigurirati svih virtualne mreže i resursa za pristupnik unutar iste sesije PowerShell.

Koraci u ovom članku pomoću varijabli koje su deklarirane na početku svake sekcije. Ako već radite s postojećeg VNets, izmijenite varijable u skladu s vizualnim postavke u vlastitu okruženju. 

![Obje veze](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Kako se povezati VNets koje su dio iste pretplate

![V2V dijagrama](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Prije početka
    
Prije početka, morate instalirati cmdleta Azure resursima PowerShell. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

### <a name="Step1"></a>Korak 1 - planiranje vaše rasponi IP adresa


U sljedećim koracima ćemo stvoriti dvije virtualne mreže zajedno sa svojim podmreže odgovarajući pristupnika i konfiguracije. Zatim ćemo stvoriti VPN veza između dva VNets. Važno je da planiranje rasponi IP adresa za mrežna konfiguracija. Imajte na umu da morate provjeriti da se ništa VNet rasponi ili rasponi lokalne mreže ne preklapa na bilo koji način.

U primjerima koristimo sljedeće vrijednosti:

**Vrijednosti za TestVNet1:**

- Naziv VNet: TestVNet1
- Grupa resursa: TestRG1
- Lokacija: Istočni SAD-a
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- Sučelju: 10.11.0.0/24
- Pozadinskog: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS poslužitelj: 8.8.8.8
- GatewayName: VNet1GW
- Javnu IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- Vrsta veze: VNet2VNet


**Vrijednosti za TestVNet4:**

- Naziv VNet: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- Sučelju: 10.41.0.0/24
- Pozadinskog: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Grupa resursa: TestRG4
- Lokacija: Zapad SAD-a
- DNS poslužitelj: 8.8.8.8
- GatewayName: VNet4GW
- Javnu IP: VNet4GWIP
- VPNType: RouteBased
- Veze: VNet4toVNet1
- Vrsta veze: VNet2VNet



### <a name="Step2"></a>Korak 2 – Stvaranje i konfiguriranje TestVNet1

1. Deklariranje varijabli

    Najprije deklariranje varijabli. U ovom se primjeru deklarira varijable pomoću vrijednosti za ovu vježbu. U većini slučajeva, trebali biste zamijenite vrijednosti vlastitu. Međutim, koristite ove varijable ako koristite kroz korake da biste se upoznali s tom vrstom konfiguracije. Izmjena varijabli ako je potrebno, a zatim kopirajte i zalijepite ih u konzole za PowerShell.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Povezivanje s pretplate

    Prebacivanje u način rada PowerShell da bi koristio Cmdlete Voditelj resursa. Otvorite konzole za PowerShell i povezati s računom. U sljedećem primjeru koristite za povezivanje:

        Login-AzureRmAccount

    Provjerite pretplate za račun.

        Get-AzureRmSubscription 

    Navedite pretplatu u koju želite koristiti.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Stvorite novu grupu resursa

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Stvaranje konfiguracije podmreže za TestVNet1

    U ovom se primjeru stvara virtualne mreže pod nazivom TestVNet1 i tri podmreže, jedan pod nazivom GatewaySubnet, jedan pod nazivom sučelju i jednom pod nazivom pozadinskog. Kada zamjena vrijednosti, važno je naziv vašoj podmreži pristupnika posebno GatewaySubnet. Ako ste nazvali je nešto drugo, vaše stvaranje pristupnika neće uspjeti. 

    U sljedećem primjeru pomoću varijabli koje ste prethodno postavili. U ovom primjeru podmreže pristupnik koristi u /27. Dok je moguće da biste stvorili pristupnik podmreže što manje /29, preporučujemo da stvorite veće podmreže koja sadrži više adresa tako da odaberete barem /28 ili /27. Tako ćete omogućiti za dovoljno adrese kako bi odgovarao moguće dodatne konfiguracije koji se preporučuje se u budućnosti. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Stvaranje TestVNet1

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Zahtjev za javna IP adresa

    Zahtjev javnu IP adresu će se dodijeliti pristupnika će stvoriti za vaše VNet. Primijetite da se AllocationMethod dinamičkih. Ne možete navesti IP adresa koji želite koristiti. Dinamički dodijeliti u vašem pristupnik. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Stvaranje konfiguracije pristupnika

    Konfiguraciju pristupnika definira podmreži i javnu IP adresu da biste koristili. U primjeru koristite da biste stvorili konfiguraciju pristupnika. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Stvaranje pristupnika za TestVNet1

    U ovom ćete koraku stvoriti pristupnika virtualne mreže za vaše TestVNet1. Konfiguracija VNet VNet potreban je RouteBased VpnType. Stvaranje pristupnika može potrajati neko vrijeme (45 minuta ili više da biste dovršili).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Korak 3 – stvaranje i konfiguriranje TestVNet4

Nakon što ste konfigurirali TestVNet1, stvorite TestVNet4. Slijedite korake u nastavku zamjenu vrijednosti s vlastitim po potrebi. Taj korak mogu se izvršiti unutar iste sesije PowerShell jer je u okviru iste pretplate.

1. Deklariranje varijabli

    Pripazite da zamijenite vrijednosti one koje želite koristiti za konfiguraciju.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Prije nego što nastavite, provjerite jeste li povezani s pretplatom 1.

2. Stvorite novu grupu resursa

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Stvaranje konfiguracije podmreže za TestVNet4

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Stvaranje TestVNet4

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Zahtjev za javna IP adresa

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Stvaranje konfiguracije pristupnika

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. Stvaranje pristupnika za TestVNet4

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Korak 4 - povezivanje pristupnika

1. Početak oba pristupnika virtualne mreže

    U ovom primjeru jer su oba pristupnika u okviru iste pretplate ovaj korak možete obaviti u istu sesiju ljuske PowerShell.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. Stvaranje TestVNet1 TestVNet4 vezu

    U ovom ćete koraku koje stvorite vezu s TestVNet1 TestVNet4. Vidjet ćete zajednički ključ referencirani u primjerima. Možete koristiti vlastite vrijednosti za zajednički ključ. Važno je da zajednički ključ mora odgovarati obje veze. Stvaranje veze može potrajati kratki da biste dovršili.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. Stvaranje TestVNet4 TestVNet1 vezu

    Ovaj korak je slična onoj iznad, osim stvaranja veze iz TestVNet4 za TestVNet1. Provjerite odgovara zajedničke tipke.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Trebali biste uspostaviti vezu nakon nekoliko minuta.

4. Provjerite vezu. U odjeljku [Provjera vezu](#verify).


## <a name="difsub"></a>Kako se povezati VNets koje su različite pretplate


![V2V dijagrama](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

U ovom scenariju smo TestVNet1 i povezivanje TestVNet5. TestVNet1 i TestVNet5 nalaze se u neku drugu pretplatu. Koraci za konfiguraciju dodajte dodatnu vezu VNet VNet radi povezivanja TestVNet1 sa TestVNet5. 

Razlika ovdje je dio navedeni koraci za konfiguraciju morate izvršiti u zasebnom sesiju ljuske PowerShell u kontekstu drugu pretplatu. Osobito kada pretplata na dva pripadaju različitim tvrtke ili ustanove. 

Upute i dalje iz prethodnih koraka koji je naveden. Morate dovršiti [Korak 1](#Step1) i [2 korak](#Step2) za stvaranje i konfiguriranje TestVNet1 i pristupnika VPN-a za TestVNet1. Kada dovršite korak 1 i 2 korak, nastavite s korak 5 da biste stvorili TestVNet5.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Drugi korak 5 – provjerite dodatne rasponi IP adresa

Nije važno da biste bili sigurni da prostor IP adrese novi virtualne mreže, TestVNet5, ne preklapa s bilo kojom VNet rasponi ili rasponi pristupnika lokalnoj mreži. 

U ovom primjeru virtualne mreže možda pripadaju različitim tvrtke ili ustanove. Za ovu vježbu za na TestVNet5 možete koristiti sljedeće vrijednosti:

**Vrijednosti za TestVNet5:**

- Naziv VNet: TestVNet5
- Grupa resursa: TestRG5
- Mjesto: Istok Japan
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- Sučelju: 10.51.0.0/24
- Pozadinskog: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS poslužitelj: 8.8.8.8
- GatewayName: VNet5GW
- Javnu IP: VNet5GWIP
- VPNType: RouteBased
- Veze: VNet5toVNet1
- Vrsta veze: VNet2VNet

**Dodatne vrijednosti za TestVNet1:**

- Veze: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Korak 6 – stvaranje i konfiguriranje TestVNet5

Ovaj korak mora poduzeti u kontekstu na novu pretplatu. Taj dio može se izvesti administrator u različitim tvrtki ili ustanovi koja je vlasnik pretplate.

1. Deklariranje varijabli

    Pripazite da zamijenite vrijednosti one koje želite koristiti za konfiguraciju.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Povezivanje s pretplatom 5

    Otvorite konzole za PowerShell i povezati s računom. Pomoću sljedeće ogledne možete povezati:

        Login-AzureRmAccount

    Provjerite pretplate za račun.

        Get-AzureRmSubscription 

    Navedite pretplatu u koju želite koristiti.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Stvorite novu grupu resursa

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Stvaranje konfiguracije podmreže za TestVNet4
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Stvaranje TestVNet5

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Zahtjev za javna IP adresa

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Stvaranje konfiguracije pristupnika

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. Stvaranje pristupnika za TestVNet5

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Korak 7 – povezivanje pristupnika

U ovom primjeru jer pristupnika u različite pretplate ne možemo ste podijelite ovaj korak dva sesije PowerShell označene kao [pretplata 1] i [pretplata 5].

1. **[Pretplata 1]** Dobivanje pristupnika virtualne mreže za pretplatu 1

    Provjerite je li prijaviti i povezivanje s pretplatom 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Kopirajte izlaz sljedeće elemente i pošaljite ih administrator pretplate 5 putem e-pošte ili neki drugi način.

        $vnet1gw.Name
        $vnet1gw.Id

    Ove dvije elementi će imati slično kao primjer rezultatu sljedeće vrijednosti:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Pretplata 5]** Dobivanje pristupnika virtualne mreže za pretplatu 5

    Provjerite je li prijaviti i povezati 5 pretplate.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Kopirajte izlaz sljedeće elemente i pošaljite ih administrator pretplate 1 putem e-pošte ili neki drugi način.

        $vnet5gw.Name
        $vnet5gw.Id

    Ove dvije elemenata će imati slično kao primjer rezultatu sljedeće vrijednosti:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Pretplata 1]** Stvaranje TestVNet1 TestVNet5 vezu

    U ovom ćete koraku koje stvorite vezu s TestVNet1 TestVNet5. Razlika je u tom $vnet5gw nije moguće dohvatiti izravno jer je u neku drugu pretplatu. Morat ćete Stvori novi objekt PowerShell s vrijednostima prenosi od 1 pretplate u gore navedene korake. Koristite primjeru u nastavku. Zamijenite naziv, Id i zajednički ključ vlastitih vrijednosti. Važno je da zajednički ključ mora odgovarati obje veze. Stvaranje veze može potrajati kratki da biste dovršili.

    Provjerite je li povezati 1 pretplate. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Pretplata 5]** Stvaranje TestVNet5 TestVNet1 vezu

    Ovaj korak je slična onoj iznad, osim stvaranja veze iz TestVNet5 za TestVNet1. Isti postupak stvaranja PowerShell objekt na temelju vrijednosti dobivenog od 1 pretplate primjenjuje ovdje kao i. U ovom ćete koraku provjerite odgovaraju zajedničke tipke.

    Provjerite je li povezati 5 pretplate.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Kako provjeriti veze


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Daljnji koraci

- Nakon dovršetka vezu, možete dodati virtualnim strojevima virtualne mreže. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Informacije o BGP potražite u članku [Pregled BGP](vpn-gateway-bgp-overview.md) i [konfiguriranju BGP](vpn-gateway-bgp-resource-manager-ps.md). 

