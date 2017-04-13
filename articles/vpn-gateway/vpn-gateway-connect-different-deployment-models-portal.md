<properties 
   pageTitle="Upute za povezivanje classic virtualne mreže s resursima virtualne mreže na portalu | Microsoft Azure"
   description="Saznajte kako stvoriti VPN vezu između klasične VNets i resursima VNets pomoću pristupnika VPN-a i na portalu"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Povezivanje virtualne mreže iz različitih implementacije modela na portalu

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure trenutno sadrži dvije modela upravljanja: klasični i resursa Manager (Upravitelj Resursa). Ako ste koristili Azure neko vrijeme, vjerojatno imate Azure VMs i uloge instancu izvodi u klasični VNet. Novija VMs i uloga instance možda se izvode u VNet stvorena u upravitelju resursa. U ovom se članku vodit će vas kroz povezivanje classic VNets VNets Voditelj resursa da biste omogućili resursa koja se nalazi u zasebnom implementacije modelima za komunikaciju putem veze pristupnika. 

Možete stvoriti veze između VNets koji su u različite pretplate, a u različitim područjima. Možete povezati i VNets koji već imaju veze s lokalnim mrežama, pod uvjetom da je pristupnika koja ste je konfiguriran pomoću dinamičke ili usmjeravanje. Dodatne informacije o vezama VNet VNet potražite u članku [Najčešća pitanja vezana uz VNet VNet](#faq) pri kraju ovog članka.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Uvođenje modela i postupci za VNet VNet veze

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

U sljedećoj su tablici ažuriramo kao novih članaka i dodatne alate postaju dostupne za tu konfiguraciju. Kada članak nije dostupan, ne možemo veza izravno u nju iz tablice.<br><br>

**VNet VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Prije početka

Sljedeći koraci će vas voditi kroz potrebne za konfiguriranje pristupnika dinamički ili usmjeravanje za svaku VNet i Stvori VPN vezu između pristupnika postavke. Tu konfiguraciju ne podržava statički ili pravila pristupnika. 

U ovom se članku koristimo klasični portal, Azure portal i PowerShell. Trenutno nije moguće stvoriti pomoću samo Azure portala za konfiguraciju.

### <a name="prerequisites"></a>Preduvjeti

 - Oba VNets ste već stvorili.
 - Rasponi adresa za na VNets ne preklapa s drugom ili preklopite s bilo kojeg od raspona za druge veze koje pristupnika može biti povezani s.
 - Ste instalirali najnovija cmdleta ljuske PowerShell (1.0.2 ili noviji). Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) . Provjerite je li instalirati upravljanje servisa (US) i cmdleta resursa Manager (Upravitelj Resursa). 

### <a name="values"></a>Primjer postavke

Primjer postavke možete koristiti kao referencu.

**Klasični VNet postavke**

Naziv VNet = ClassicVNet <br>
Mjesto = Zapad SAD-a <br>
Virtualne mreže adresu razmake = 10.0.0.0/24 <br>
Podmreže 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Naziv lokalne mreže = RMVNetLocal <br>

**Voditelj resursa VNet postavke**

Naziv VNet = RMVNet <br>
Grupa resursa = RG1 <br>
Virtualne mreže IP adresa razmake = 192.168.0.0/16 <br>
Podmreže 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Mjesto = Istočni SAD-a <br>
Naziv pristupnika virtualne mreže = RMGateway <br>
Javnu IP naziv pristupnika = gwpip <br>
Vrsta pristupnika = VPN-a <br>
VPN-a vrsta = utemeljen na usmjeravanje <br>
Pristupnik za lokalnu mrežu = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Sekcija 1: Konfiguriranje postavki klasični VNet

U ovom ćete odjeljku ćemo stvoriti lokalnu mrežu i pristupnika za vaše klasični VNet. Upute u ovom odjeljku pomoću portala za klasični. Trenutno Azure portal nude sve postavke koje se odnose na klasični VNet.

### <a name="part-1---create-a-new-local-network"></a>Dio 1 – da biste stvorili novu lokalnu mrežu

Otvorite [portal za klasični](https://manage.windowsazure.com) i prijavite se pomoću računa za Azure.

1. U donjem lijevom kutu zaslona, kliknite **NOVO** > **Mrežnim servisima** > **Virtualne mreže** > **Dodaj lokalnoj mreži**.

2. U prozoru za **određivanje pojedinosti lokalne mreže** upišite naziv VNet upravitelja Resursa koji se želite povezati. U okvir **VPN uređaj IP adresa (neobavezno)** upišite bilo koji valjani javnu IP adresa. To je samo privremene rezerviranog mjesta. Kasnije promijeniti ovu IP adresu. U donjem desnom kutu prozora programa, kliknite gumb sa strelicom.
 
3. Na stranicu za **navođenje adrese prostor** u tekstni okvir **Pokretanje IP** upišite prefiks mreže i CIDR Blok za VNet Voditelj resursa koji se želite povezati. Ta postavka se koristi za određivanje prostor adrese za usmjeravanje VNet upravitelja Resursa.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Dio 2 – povežite lokalne mreže vaše VNet

1. Kliknite **Virtualne mreže** pri vrhu stranice da biste se prebacili na zaslon virtualne mreže, a zatim kliknite da biste odabrali na klasični VNet. Na stranici za vaše VNet kliknite **Konfiguriraj** da biste prešli na stranicu Konfiguriranje.

2. U odjeljku veza **s povezivanjem web-mjesto** odaberite potvrdni okvir **za povezivanje s lokalne mreže** . Odaberite lokalnu mrežu koju ste stvorili. Ako imate više mreža za lokalni koji ste stvorili, obavezno odaberite onaj koji ste stvorili za predstavljanje vaše VNet Voditelj resursa na padajućem izborniku.

3. Kliknite **Spremi** pri dnu stranice.

### <a name="part-3---create-the-gateway"></a>Dio 3 – stvaranje pristupnika

1. Kada spremite postavke, kliknite **nadzorna ploča** pri vrhu stranice da biste promijenili na stranicu nadzorne ploče. Na dnu stranice nadzorne ploče, kliknite **Stvaranje pristupnika**, a zatim kliknite **Dinamički usmjeravanja**. Kliknite **da** da biste započeli stvaranje pristupnikom. Za tu konfiguraciju potreban je pristupnika dinamički usmjeravanja.

2. Pričekajte pristupnika će biti stvoren. To može potrajati 45 minuta ili više da biste dovršili.

### <a name="ip"></a>Dio 4 - prikaz pristupnika javnu IP adresu

Nakon stvaranja pristupnika IP adresa pristupnika možete pregledati na stranicu **nadzorne ploče** . To je javnu IP adresu na pristupnika. Zapišite ili kopirajte na javnu IP adresu. Koristite ga u noviji korake kada stvarate lokalne mreže za konfiguraciju VNet Voditelj resursa.


## <a name="creatermgw"></a>Sekcija 2: Konfiguriranje postavki VNet Voditelj resursa

U ovom ćete odjeljku ćemo stvoriti pristupnika virtualne mreže i lokalne mreže za vaše VNet Voditelj resursa. Nemojte započeti na sljedeći način do nakon što ste dohvatili javnu IP adresu za klasični VNet pristupnik.

U snimke zaslona prikazuje se kao primjere. Provjerite vrijednosti zamijeniti vlastitim. Ako stvarate tu konfiguraciju kao programa vježbu, pročitajte ove [vrijednosti](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Dio 1 – stvaranje podmreži pristupnika

Prije povezivanja virtualne mreže pristupnika, najprije morate stvoriti podmreže pristupnik za virtualne mreže na koju se želite povezati. Stvaranje pristupnika podmreže s CIDR /28 ili veća (/ 27, / 26, itd.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

U pregledniku idite na [portal za Azure](http://portal.azure.com) i prijavite se pomoću računa za Azure.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Dio 2 – da biste stvorili pristupnik virtualne mreže


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Dio 3 – stvaranje pristupnika za lokalnu mrežu

'Lokalne mreže pristupnik' obično se odnosi na lokalne lokacije. Datotečni nastavak govori Azure rasponi koja IP adresa za usmjeravanje mjesto i javnu IP adresu uređaja za to mjesto. Međutim, u ovom slučaju upućuje na raspon adresu i javnu IP adresu povezanu sa klasični VNet i virtualne mreže pristupnika.

Naziv lokalne mreže pristupnika po kojoj Azure može se odnositi na njega. Pristupnik za lokalnu mrežu možete stvoriti tijekom stvaranja pristupnikom virtualne mreže. Za tu konfiguraciju koristite javnoj IP adresu koja je dodijeljena klasični VNet pristupnika u [prethodnom odjeljku](#ip).

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Dio 4 - Kopiraj na javnu IP adresu

Nakon dovršetka pristupnika virtualne mreže stvaranja, kopirajte javnu IP adresu koja je povezana s pristupnika. Kada konfigurirate postavke lokalne mreže vaše klasični VNet koristiti ga. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Odjeljak 3: Izmjena lokalne mreže za klasični VNet

Otvorite [portal za klasični](https://manage.windowsazure.com).

2. Na portalu klasični pomaknite se prema dolje na lijevoj strani, a zatim kliknite **mreže**. Na stranici **mreža** kliknite **Lokalne mreže** pri vrhu stranice. 

3. Kliknite da biste odabrali lokalnu mrežu koje ste konfigurirali u dijelu 1. Pri dnu stranice kliknite **Uređivanje**.

4. Na stranici **određivanje pojedinosti lokalne mreže** zamijenite IP adresa rezervirano mjesto javnu IP adresa za pristupnik za upravljanje resursima koji ste stvorili u prethodnom odjeljku. Kliknite strelicu da biste premjestili na sljedeću sekciju. Provjerite je li ispravan **Prostor adrese** , a zatim kliknite kvačicu da biste prihvatili promjene.

## <a name="connect"></a>Odjeljak 4: Stvaranje veze

U ovom ćete odjeljku ćemo stvoriti veze između sustava VNets. Upute za to potreban je PowerShell. Ne možete stvoriti ovu vezu u nekoj od na portalima. Provjerite je li ste preuzeli i instalirali klasični (US) i cmdleta ljuske PowerShell resursa Manager (Upravitelj Resursa).


1. Prijavite se na račun za Azure na konzoli PowerShell. Sljedeći cmdlet traži vjerodajnice za prijavu za vaš račun za Azure. Nakon prijave u, postavke računa se preuzimaju tako da su dostupni za Azure PowerShell.

        Login-AzureRmAccount 

    Ako imate više pretplata, dobit ćete popis pretplate Azure.

        Get-AzureRmSubscription

    Navedite pretplatu u koju želite koristiti. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Dodajte račun za Azure da biste pomoću klasične cmdleta ljuske PowerShell. Da biste to učinili, koristite sljedeću naredbu:

        Add-AzureAccount

3. Zajednički ključ postavite tako da pokrenete sljedeći primjer. U ovom primjeru `-VNetName` je naziv klasični VNet i `-LocalNetworkSiteName` je naziv koji ste naveli za lokalnu mrežu prilikom konfiguriran na portalu klasični. U `-SharedKey` je vrijednost koja možete stvoriti i navesti. Vrijednost koju navedete mora biti iste vrijednost koju navedete u sljedećem koraku kada stvorite vezu.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Stvaranje VPN veza ponovnim pokretanjem sljedeće naredbe:
    
    **Postavljanje varijable**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Stvaranje veze**<br> Imajte na umu da se `-ConnectionType` je "IPsec", ne 'Vnet2Vnet'. U ovom primjeru `-Name` je naziv koji želite pozvati vezu. Sljedeća Ogledna formula stvara vezu pod nazivom "*upravitelja resursa-na-klasični veze*".
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Provjerite je li vaša veza

Vezu možete provjeriti pomoću portala za klasični, Azure portal ili PowerShell. Možete koristiti sljedeće korake da biste provjerili vezu. Zamijenite vrijednosti vlastitu.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>Najčešća pitanja vezana uz VNet VNet

Dodatne informacije o vezama VNet VNet u detaljima najčešća Pitanja.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


