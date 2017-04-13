<properties 
   pageTitle="Upute za povezivanje classic virtualne mreže s resursima virtualne mreže pomoću komponente PowerShell | Microsoft Azure"
   description="Saznajte kako stvoriti VPN vezu između klasične VNets i resursima VNets pomoću pristupnika VPN-a i komponente PowerShell"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Povezivanje virtualne mreže iz različitih implementacije modela pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure trenutno sadrži dvije modela upravljanja: klasični i resursa Manager (Upravitelj Resursa). Ako ste koristili Azure neko vrijeme, vjerojatno imate Azure VMs i uloge instancu izvodi u klasični VNet. Novija VMs i uloga instance možda se izvode u VNet stvorena u upravitelju resursa. U ovom se članku vodit će vas kroz povezivanje classic VNets VNets Voditelj resursa da biste omogućili resursa koja se nalazi u zasebnom implementacije modelima za komunikaciju putem veze pristupnika.

Možete stvoriti veze između VNets koji su u različite pretplate, a u različitim područjima. Možete povezati i VNets koji već imaju veze s lokalnim mrežama, pod uvjetom da je pristupnika koja ste je konfiguriran pomoću dinamičke ili usmjeravanje. Dodatne informacije o vezama VNet VNet potražite u članku [Najčešća pitanja vezana uz VNet VNet](#faq) pri kraju ovog članka.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Modeli implementacije i postupci za povezivanje VNets u različitoj implementaciji modelima

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

U sljedećoj su tablici ažuriramo kao novih članaka i dodatne alate postaju dostupne za konfiguraciju. Kada članak nije dostupan, ne možemo veza izravno u nju iz tablice.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Prije početka

Sljedeći koraci će vas voditi kroz potrebne za konfiguriranje pristupnika dinamički ili usmjeravanje za svaku VNet i Stvori VPN vezu između pristupnika postavke. Tu konfiguraciju ne podržava statički ili pravila pristupnika.

### <a name="prerequisites"></a>Preduvjeti

 - Oba VNets ste već stvorili.
 - Rasponi adresa za na VNets ne preklapa s drugom ili preklopite s bilo kojeg od raspona za druge veze koje pristupnika može biti povezani s.
 - Ste instalirali najnovija cmdleta ljuske PowerShell (1.0.2 ili noviji). Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) . Provjerite je li instalirate upravljanje servisa (US) i cmdleta resursa Manager (Upravitelj Resursa). 

### <a name="exampleref"></a>Primjer postavke

Primjer postavke možete koristiti kao referencu.

**Klasični VNet postavke**

Naziv VNet = ClassicVNet <br>
Mjesto = Zapad SAD-a <br>
Virtualne mreže adresu razmake = 10.0.0.0/24 <br>
Podmreže 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Naziv lokalne mreže = RMVNetLocal <br>
GatewayType = DynamicRouting

**Voditelj resursa VNet postavke**

Naziv VNet = RMVNet <br>
Grupa resursa = RG1 <br>
Virtualne mreže IP adresa razmake = 192.168.0.0/16 <br>
Podmreže 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Mjesto = Istočni SAD-a <br>
Javnu IP naziv pristupnika = gwpip <br>
Pristupnik za lokalnu mrežu = ClassicVNetLocal <br>
Virtualna naziv mreže pristupnika = RMGateway <br>
Pristupnik IP adresa konfiguracija = gwipconfig


## <a name="createsmgw"></a>Sekcija 1 - konfiguriranje klasični VNet

### <a name="part-1---download-your-network-configuration-file"></a>Dio 1 – preuzimanje konfiguracijskoj datoteci mreže

1. Prijavite se na račun za Azure na konzoli PowerShell s dodatnim pravima. Sljedeći cmdlet traži vjerodajnice za prijavu za vaš račun za Azure. Nakon prijave u, ona preuzima postavki računa tako da su dostupni za Azure PowerShell. Će pomoću cmdleta ljuske PowerShell US da biste dovršili ovaj dio konfiguracije.

        Add-AzureAccount

2. Izvoz konfiguracijskoj datoteci Azure mrežu tako da pokrenete sljedeću naredbu. Možete promijeniti mjesto datoteke koju želite izvesti na neko drugo mjesto prema potrebi. Će uređivati datoteku, a zatim je uvezite Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Otvorite .xml datoteci koju ste preuzeli radi uređivanja. Primjer mreže konfiguracijska datoteka potražite u članku [Sheme konfiguracije mreže](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Dio 2 – Provjerite podmreže pristupnika

Element **VirtualNetworkSites** dodati podmreži pristupnika na VNet ako neki već nije stvoren. Rad s mrežom konfiguracijska datoteka, podmreže pristupnika morate imenovani "GatewaySubnet" ili Azure ne prepoznaje i upotrijebite je kao podmreži pristupnika.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Primjer:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Dio 3 - mjesto dodati u lokalnoj mreži

Web-mjesto lokalne mreže dodate predstavlja VNet upravitelja Resursa na koji se želite povezati. Možda ćete morati dodati **LocalNetworkSites** element datoteku ako još ne postoji. Sada u konfiguraciji na VPNGatewayAddress može biti bilo koji valjani javnu IP adresu jer smo još niste stvorili pristupnik za upravljanje resursima VNet. Nakon što smo stvorili pristupnika, ne možemo zamijenite ovu IP adresu rezervirano mjesto ispravnu javnu IP adresu koja vam je dodijeljen pristupnika upravitelja Resursa.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Dio 4 - Pridruži VNet s web-mjesta lokalne mreže

U ovom ćete odjeljku smo navedite lokalne mreže web-mjesto koje se želite povezati VNet da biste. U ovom slučaju je VNet Voditelj resursa koje ranije poziva. Provjerite jesu li nazivi koji se podudaraju. Ovaj korak nije moguće stvoriti pristupnik. Određuje lokalnu mrežu koja će se povezati pristupnika.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Dio 5 - spremite datoteku i prijenos

Spremite datoteku, a zatim uvoza Azure ponovnim pokretanjem sljedeće naredbe. Provjerite je li promijeniti put datoteke prema potrebi okruženju sustava.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Vidjet ćete nešto slično kao sljedeći rezultat prikazuje da je uspjelo uvoz.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Dio 6 – stvaranje pristupnika 

VNet pristupnika možete stvoriti pomoću portala za klasični ili pomoću komponente PowerShell.

Da biste stvorili dinamičnu pristupnik za usmjeravanje koristite u sljedećem primjeru:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Status pristupnika možete provjeriti pomoću na `Get-AzureVNetGateway` cmdlet.


## <a name="creatermgw"></a>Sekcija 2: Konfigurirati pristupnik VNet upravitelja Resursa

Da biste stvorili pristupnik VPN-a za VNet upravitelja Resursa, slijedite sljedeće upute. Nemojte započeti koraka do nakon što ste dohvatili javnu IP adresu za klasični VNet pristupnik. 

1. **Prijavite se na račun za Azure** na konzoli PowerShell. Sljedeći cmdlet traži vjerodajnice za prijavu za vaš račun za Azure. Nakon prijave u, postavke računa se preuzimaju tako da su dostupni za Azure PowerShell.

        Login-AzureRmAccount 

    Ako imate više pretplata, dobit ćete popis pretplate Azure.

        Get-AzureRmSubscription

    Navedite pretplatu u koju želite koristiti. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Stvaranje pristupnika lokalnoj mreži**. U odjeljku virtualne mreža pristupnika lokalne mreže obično se odnosi lokalne lokacije. U ovom slučaju pristupnika lokalne mreže se odnosi na klasični VNet. Joj naziv koji Azure možete se referirati na nju, a i Navedite adresu prefiks prostora. Azure koristi prefiks IP adresa navedete da biste odredili koji promet da biste poslali lokalne lokacije. Po potrebi prilagodite informacije ovdje kasnije, prije stvaranja pristupnika, možete mijenjati vrijednosti i ponovno pokrenite uzorka.<br><br>
    - `-Name`je naziv koji želite dodijeliti da biste se pozvali u lokalnoj mreži pristupnik.<br>
    - `-AddressPrefix`je adresni prostor za vaše klasični VNet.<br>
    - `-GatewayIpAddress`je javnu IP adresu klasični VNet pristupnika. Pripazite da promijenite sljedeće ogledne u skladu s vizualnim ispravna IP adresa.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Zahtjev za javnu IP adresu** će se dodijeliti virtualne mreže pristupnika za upravljanje resursima VNet. Ne možete navesti IP adresa koji želite koristiti. Virtualne mreže pristupnika dinamički dodijeljen IP adresa. Međutim, to znači promijenit će se s IP adresom. Samo vrijeme promjenama virtualne mreže pristupnik IP adrese je kada pristupnika se briše i ponovno stvoriti. Neće se promijeniti preko promjene veličine, ponovno postavljanje ili druge interne održavanja/nadogradnje pristupnika.<br>U ovom ćete koraku ne možemo postaviti varijabla koja se koristi u noviji koraku.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Provjeri ima li virtualne mreže podmreži pristupnika**. Ako nema pristupnika podmreže postoji, dodajte ga. Provjerite je li pristupnik podmreži pod nazivom *GatewaySubnet*.

5. **Dohvaćanje podmreži** koji se koristi za pristupnika pokretanjem sljedeće naredbe. U ovom ćete koraku ne možemo postaviti varijabla koja će se koristiti u sljedećem koraku.
    - `-Name`je naziv vaše VNet Voditelj resursa.
    - `-ResourceGroupName`je grupa resursa koji je pridružen na VNet. Pristupnik podmreže mora postojati za ovaj VNet i mora imati naziv *GatewaySubnet* ispravno funkcionirao.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Stvaranje pristupnika IP adresa konfiguracija**. Konfiguraciju pristupnika definira podmreži i javnu IP adresu da biste koristili. Sljedeća Ogledna koristite da biste stvorili konfiguraciju pristupnika.<br>U ovom koraku u `-SubnetId` i `-PublicIpAddressId` parametara mora proslijediti svojstvo id podmreže i IP adrese objekte, odnosno. Ne možete koristiti jednostavni niz. Da biste zatražili javnu IP i korak za dohvaćanje podmreži tih varijabli se postavljaju u koraku.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Stvaranje pristupnika za upravljanje resursima virtualne mreže** pokretanjem sljedeće naredbe. Na `-VpnType` mora biti *RouteBased*. To može potrajati 45 minuta ili više za to da biste dovršili.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Kopirajte na javnu IP adresu** jednom pristupnika VPN je stvorena. Kada konfigurirate postavke lokalne mreže vaše klasični VNet koristiti ga. Koristite sljedeći cmdlet za dohvaćanje na javnu IP adresu. Povrat na javnu IP adresu popisu kao *IPAdresa*.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Odjeljak 3: Izmjena lokalne mreže za klasični VNet

To možete učiniti ovaj korak ili mreže konfiguracijska datoteka za izvoz, uređivanjem, i zatim spremanje i upute za uvoz datoteke natrag na Azure. Ili možete promijeniti ovu postavku u klasični portal. 

### <a name="to-modify-in-the-portal"></a>Da biste izmijenili na portalu

1. Otvorite [portal za klasični](https://manage.windowsazure.com).

2. Na portalu klasični pomaknite se prema dolje na lijevoj strani, a zatim kliknite **mreže**. Na stranici **mreža** kliknite **Lokalne mreže** pri vrhu stranice. 

3. Na stranici **Lokalne mreže** , kliknite da biste odabrali lokalnu mrežu koji konfiguriran u dijelu 1, a zatim idite na dno stranice i kliknite **Uređivanje**.

4. Na stranici **određivanje pojedinosti lokalne mreže** zamijenite IP adresa rezerviranom mjestu javnu IP adresa za pristupnik za upravljanje resursima koji ste stvorili u prethodnom odjeljku. Kliknite strelicu da biste premjestili na sljedeću sekciju. Provjerite je li ispravan **Prostor adrese** , a zatim kliknite kvačicu da biste prihvatili promjene.

### <a name="to-modify-using-the-network-configuration-file"></a>Da biste izmijenili pomoću konfiguracijska datoteka mreže

1. Izvoz konfiguracije datoteke mreže.
2. U uređivaču teksta, izmijenite VPN pristupnik IP adresa.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Spremite promjene i uvoz uređivati datoteke natrag na Azure.


## <a name="connect"></a>Odjeljak 4: Stvaranje veze između pristupnika

Stvaranje veze između pristupnika zahtijeva PowerShell. Možda ćete morati dodati račun za Azure da biste pomoću klasične cmdleta ljuske PowerShell. Da biste to učinili, koristite sljedeći cmdlet: 
        
    Add-AzureAccount

1. **Postavljanje zajednički ključ** pokretanjem sljedeće naredbe. U ovom primjeru `-VNetName` je naziv klasični VNet i `-LocalNetworkSiteName` je naziv koji ste naveli za lokalnu mrežu prilikom konfiguriran na portalu klasični. U `-SharedKey` je vrijednost koja možete stvoriti i navesti. Vrijednost koju navedete mora biti iste vrijednost koju navedete u sljedećem koraku kada stvorite vezu. Povrat ovaj primjer trebao pokazati **Status: uspješno**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Stvaranje VPN veza ponovnim pokretanjem sljedeće naredbe.

    **Postavljanje varijable**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Stvaranje veze**<br> Imajte na umu da se `-ConnectionType` je IPsec, ne Vnet2Vnet.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Možete vidjeti vezu ili portalu ili pomoću na `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>Najčešća pitanja vezana uz VNet VNet

Dodatne informacije o vezama VNet VNet u detaljima najčešća Pitanja.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


