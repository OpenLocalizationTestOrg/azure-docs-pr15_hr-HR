<properties 
   pageTitle="Konfiguriranje prisilne tuneliranje za web-mjesto veze pomoću modela uvođenje klasičnog | Microsoft Azure"
   description="Upute za preusmjeravanje ili 'prisilno svih povezanih s Interneta promet natrag na lokalne lokacije."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Konfiguriranje prisilne tuneliranje pomoću modela klasični implementacije

> [AZURE.SELECTOR]
- [PowerShell – klasični](vpn-gateway-about-forced-tunneling.md)
- [PowerShell – Voditelj resursa](vpn-gateway-forced-tunneling-rm.md)

Prisilne tuneliranje omogućuje preusmjeravanje ili "prisilno" svih povezanih s Interneta promet natrag na lokalne lokacije putem web-mjesto VPN tunelom za provjeru i nadzor. Ovo je ključnih sigurnost preduvjeta za većinu tvrtki IT pravila. 

Bez prisilne tuneliranje promet Internet povezanih s vašeg VMs u Azure će uvijek prolaziti iz Azure mrežne infrastrukture u izravno odgovor s Internetom, bez mogućnosti da biste dopustili Provjera ili revizije promet. Neovlašteni pristup Internetu potencijalno može dovesti do otkrivanja informacije ili druge vrste breaches sigurnost.

U ovom se članku će vas voditi kroz konfiguriranje prisilno tuneliranje za virtualne mreže stvoren pomoću model klasični implementacije. 

**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Alati za prisilne tuneliranje i implementaciju modela**

Prisilne tunneling veza moguće je konfigurirati za uvođenje klasičnog modela i model implementacije Voditelj resursa. U sljedećoj su tablici dodatne informacije potražite u članku. U ovoj su tablici ažuriramo kao novi članke, nove modele implementacije i dodatne alate postaju dostupne za tu konfiguraciju. Kada članak nije dostupan, ne možemo veza izravno u nju iz tablice.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Preduvjeti i napomene

Prisilne tuneliranja u Azure konfiguriran putem virtualne mreže korisnički definirane usmjerava (UDR). Preusmjeravanje prometa na lokalno mjesto izražen je zadani smjer pristupnika za Azure VPN-a. U sljedećoj sekciji popis trenutno ograničenje usmjeravanje tablice i usmjerava za Azure virtualne mreže:


-  Svaki podmreže virtualne mreže sadrži ugrađeno, tablicu usmjeravanja sustava. Usmjeravanje tablica sustava sadrži sljedeće tri grupe usmjerava:

    - **Lokalni VNet usmjerava:** Izravno na odredište VMs u istom virtualne mreže
    
    - **Na lokalni usmjerava:** Da biste pristupnika Azure VPN-a
    
    - **Zadani smjer:** Izravno s Internetom. Pakete namijenjene prikazivanju privatne IP adrese ne pokrivaju prethodna dva usmjerava će biti ispušteni.


-  Uz izdanje usmjerava korisnički definirane, možete stvoriti usmjeravanje tablice da biste dodali zadani smjer i zatim povezati tablicu usmjeravanja na vašem subnet(s) VNet da biste omogućili prisilne tuneliranje na te podmreže.

- Morate postaviti na "Zadano web-mjesto" među web-lokacija web-mjesta lokalne povezani virtualne mrežom.

- Prisilne tuneliranje moraju biti povezane s VNet koja sadrži dinamički usmjeravanje VPN pristupnika (nije statične pristupnik).
 
- ExpressRoute prisilno tuneliranje nije konfiguriran putem ovaj mehanizam, ali umjesto toga omogućena je prema oglašavanje zadani smjer putem sesije peering ExpressRoute BGP. Potražite u [Dokumentaciji ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) za dodatne informacije.



## <a name="configuration-overview"></a>Pregled konfiguracija

U sljedećem primjeru tunneled Frontend podmreže nije obavezno. Da biste prihvatili i odgovaranje na zahtjeve za klijenta s Interneta izravno možete nastaviti radnih opterećenja u sučelju podmreži. Podmreže usred sloju i pozadinskog su prisilno tunneled. Sve izlazne veze iz ove dvije podmreže s Internetom će biti obavezno ili preusmjeriti lokalno mjesto putem nešto tunnels S2S VPN-a.

Time da biste ograničili i provjera pristup Internetu s virtualnim strojevima ili cloud servisi u Azure, tijekom nastavka da biste omogućili vaša arhitektura više razina servis potrebna. Također možete primijeniti prisilne tuneliranje cijelu virtualne mrežama ako postoje nema radnih opterećenja mjesto na internetu u virtualne mreže.


![Prisilno tuneliranja](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Prije početka

Provjerite imate li sljedeće stavke prije početka konfiguracije.

- Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).

- Konfigurirani virtualne mreže. 

- Najnovija verzija cmdleta Azure PowerShell. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Konfiguriranje prisilne tuneliranja

Sljedeći postupak će vam pomoći odrediti prisilne tuneliranje virtualne mreže. Navedeni koraci za konfiguraciju odgovaraju VNet mreže konfiguracijskoj datoteci.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

U ovom se primjeru virtualne mreže "MultiTier VNet" ima tri podmreže: *sučelju*, *Midtier*i *pozadinskog* podmreže s vezama četiri unakrsni lokalni: *DefaultSiteHQ*i tri *grana*. 

Korake će postaviti *DefaultSiteHQ* kao zadanu vezu web-mjesta za prisilno tuneliranje i konfiguriranje na Midtier i pozadinskog podmreže da biste koristili prisilno tuneliranje.


1. Stvorite tablicu usmjeravanja. Koristite sljedeći cmdlet za stvaranje tablice usmjeravanje.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Zadani smjer dodati u tablicu usmjeravanja. 

    Sljedeći primjer dodaje zadani smjer tablicu usmjeravanja stvorene u koraku 1. Imajte na umu da je podržana samo usmjeravanje je odredište prefiks "0.0.0.0/0" NextHop "VPNGateway".
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Pridruživanje usmjeravanje tablice u podmreže. 

    Nakon usmjeravanja tablica stvara i rutu dodavanja, koristite sljedeći primjer da biste dodali ili pridruživanje tablice usmjeravanje podmreži VNet. Primjer dodaje tablici usmjeravanje "MyRouteTable" podmreže Midtier i pozadinskog sustava VNet MultiTier-VNet.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Dodijelite zadano web-mjesto za prisilno tuneliranje. 

    U prethodnom koraku cmdlet ogledne skripte stvorili tablicu usmjeravanja i povezane tablice usmjeravanje dva podmreže VNet. Da biste odabrali lokalnog web-mjesta među veze više web-mjesta virtualne mreže kao zadanog web-mjesta ili tunelom je se preostale korak.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Dodatni cmdleta ljuske PowerShell

### <a name="to-delete-a-route-table"></a>Da biste izbrisali tablicu usmjeravanje

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Na popisu usmjeravanje tablice

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Da biste izbrisali rutu iz tablice usmjeravanje

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Da biste uklonili rutu podmreži

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Na popisu usmjeravanje tablice povezane s podmreži
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Da biste uklonili zadano web-mjesto s jednog pristupnika VNet VPN-a

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






