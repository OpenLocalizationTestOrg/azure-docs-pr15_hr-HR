<properties 
   pageTitle="Konfiguriranje prisilne tuneliranje za web-mjesto veze pomoću modela implementaciju upravljanja resursima | Microsoft Azure"
   description="Upute za preusmjeravanje ili 'prisilno svih povezanih s Interneta promet natrag na lokalne lokacije."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Konfiguriranje prisilne tuneliranje pomoću modela implementaciju upravljanja resursima za Azure

> [AZURE.SELECTOR]
- [PowerShell – klasični](vpn-gateway-about-forced-tunneling.md)
- [PowerShell – Voditelj resursa](vpn-gateway-forced-tunneling-rm.md)

Prisilne tuneliranje omogućuje preusmjeravanje ili "prisilno" svih povezanih s Interneta promet natrag na lokalne lokacije putem web-mjesto VPN tunelom za provjeru i nadzor. Ovo je ključnih sigurnost preduvjeta za većinu tvrtki IT pravila.

Bez prisilne tuneliranje promet Internet povezanih s vašeg VMs u Azure će uvijek prolaziti iz Azure mrežne infrastrukture u izravno odgovor s Internetom, bez mogućnosti da biste dopustili Provjera ili revizije promet. Neovlašteni pristup Internetu potencijalno može dovesti do otkrivanja informacije ili druge vrste breaches sigurnosti

U ovom se članku vodit će vas kroz konfiguriranje prisilno tuneliranje za virtualne mreže stvoren pomoću model implementacije Voditelj resursa.

**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Alati za prisilne tuneliranje i implementaciju modela**

Prisilne tunneling veza moguće je konfigurirati za uvođenje klasičnog modela i model implementacije Voditelj resursa. U sljedećoj su tablici dodatne informacije potražite u članku. U ovoj su tablici ažuriramo kao novi članke, nove modele implementacije i dodatne alate postaju dostupne za tu konfiguraciju. Kada članak nije dostupan, ne možemo veza izravno u nju iz tablice.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Informacije o programu prisilno tuneliranja


Sljedeći dijagram prikazuje kako prisilne tuneliranje funkcionira. 

![Prisilno tuneliranja](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

U gornjem primjeru tunneled sučelju podmreži nije obavezno. Da biste prihvatili i odgovaranje na zahtjeve za klijenta s Interneta izravno možete nastaviti radnih opterećenja u sučelju podmreži. Podmreže usred sloju i pozadinskog su prisilno tunneled. Sve izlazne veze iz ove dvije podmreže s Internetom će biti obavezno ili preusmjeriti lokalno mjesto putem nešto tunnels S2S VPN-a.

Time da biste ograničili i provjera pristup Internetu s virtualnim strojevima ili cloud servisi u Azure, tijekom nastavka da biste omogućili vaša arhitektura više razina servis potrebna. Također možete primijeniti prisilne tuneliranje cijelu virtualne mrežama ako postoje nema radnih opterećenja mjesto na internetu u virtualne mreže.

## <a name="requirements-and-considerations"></a>Preduvjeti i napomene

Prisilne tuneliranja u Azure konfiguriran putem virtualne mreže korisnički definirane usmjerava. Preusmjeravanje prometa na lokalno mjesto izražen je zadani smjer pristupnika za Azure VPN-a. Dodatne informacije o usmjeravanju korisnički definirane i virtualne mreže potražite u članku [korisnički definirane usmjerava i IP prosljeđivanje](../virtual-network/virtual-networks-udr-overview.md).

- Svaki podmreže virtualne mreže sadrži ugrađeno, tablicu usmjeravanja sustava. Usmjeravanje tablica sustava sadrži sljedeće tri grupe usmjerava:

    - **Lokalni VNet usmjerava:** Izravno na odredište VMs u istom virtualne mreže
    
    - **Lokalni usmjerava:** Da biste pristupnika Azure VPN-a
    
    - **Zadani smjer:** Izravno s Internetom. Pakete namijenjene prikazivanju privatne IP adrese ne pokrivaju prethodna dva usmjerava će biti ispušteni.

-  Ovaj postupak koristi korisnički definirane usmjerava (UDR) da biste stvorili usmjeravanje tablicu da biste dodali zadani smjer i pridruživanje tablicu usmjeravanja vaše subnet(s) VNet da biste omogućili prisilne tuneliranje na te podmreže.

- Prisilne tuneliranje moraju biti povezane s VNet koja ima VPN pristupnik utemeljen na usmjeravanje. Morate postaviti na "Zadano web-mjesto" među web-lokacija web-mjesta lokalne povezani virtualne mrežom.

- ExpressRoute prisilno tuneliranje nije konfiguriran putem ovaj mehanizam, ali umjesto toga omogućena je prema oglašavanje zadani smjer putem sesije peering ExpressRoute BGP. Potražite u [Dokumentaciji ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) za dodatne informacije.

## <a name="configuration-overview"></a>Pregled konfiguracija

U nastavku omogućuju stvaranje grupa resursa i na VNet. Zatim će stvaranje pristupnika za VPN-a i konfiguriranje prisilne tuneliranje. U ovom postupku virtualne mreže "MultiTier VNet" je 3 podmreže: *sučelju*, *Midtier*i *pozadinskog*, s 4-lokacija veze: *DefaultSiteHQ*i 3 *grana*.

Koraka postupka postavljanje *DefaultSiteHQ* kao zadanu vezu web-mjesta za prisilno tuneliranje i konfiguriranje na Midtier i pozadinskog podmreže da biste koristili prisilno tuneliranje.

    
## <a name="before-you-begin"></a>Prije početka

Provjerite imate li sljedeće stavke prije početka konfiguraciju.

- Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).

- Morat ćete instalirati najnoviju verziju programa Azure resursima PowerShell cmdleti (1.0 ili noviji). Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Konfiguriranje prisilne tuneliranja

1. Na konzoli za PowerShell prijavite se na račun za Azure. Ovaj cmdlet traži vjerodajnice za prijavu za vaš račun za Azure. Nakon prijave u, ona preuzima postavki računa pa su dostupni za Azure PowerShell.

        Login-AzureRmAccount 

2. Dobit ćete popis pretplate Azure.

        Get-AzureRmSubscription

2. Navedite pretplatu u koju želite koristiti. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Stvorite grupu resursa.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Stvaranje virtualne mreže i navedite podmreže. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Stvaranje pristupnika lokalnoj mreži.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Stvaranje tablice usmjeravanje i pravila za usmjeravanje.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Povezivanje tablice usmjeravanje podmreže Midtier i pozadinskog.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Stvaranje pristupnika pomoću zadanog web-mjesta. Ovaj korak vodi neko vrijeme da biste dovršili, ponekad 45 minuta ili više njih, jer su stvaranje i konfiguriranje pristupnika.<br> U `-GatewayDefaultSite` je parametar cmdlet koji omogućuje prisilne usmjeravanje konfiguracije da biste zaobilaznim, stoga pobrinuti da biste konfigurirali tu postavku pravilno. Taj parametar nije dostupna u PowerShell 1.0 ili noviji.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Uspostavljanje veze VPN-a web-mjesto.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



