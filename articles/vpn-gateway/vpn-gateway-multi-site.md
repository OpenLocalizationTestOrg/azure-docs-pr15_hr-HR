<properties 
   pageTitle="Povezivanje virtualne mreže na više web-mjesta pomoću pristupnika VPN-a i PowerShell | Microsoft Azure"
   description="U ovom se članku će vas voditi kroz više web-mjesta lokalne lokalnog povezati virtualne mreže pomoću pristupnika VPN-a za model klasični implementacije."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Dodavanje veze za web-mjesto VNet s postojeće veze pristupnika VPN-a

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasični - PowerShell](vpn-gateway-multi-site.md)

U ovom se članku vodit će vas kroz pomoću komponente PowerShell da biste dodali veze web-mjesta web-na-mjesta (S2S) pristupnika VPN-a koji sadrži postojeće veze. Tu vrstu veze je često se nazivaju "više web-mjesta" konfiguracije. 

Ovaj se članak odnosi virtualne mrežama stvoren pomoću model klasični implementacije (poznat i kao servis za upravljanje). Ove korake ne primjenjuju se na ExpressRoute/web-mjesta-na-web-mjesta coexisting veze konfiguracije. Informacije o coexisting vezama potražite u članku [ExpressRoute/S2S coexisting veze](../expressroute/expressroute-howto-coexist-classic.md) .

### <a name="deployment-models-and-methods"></a>Uvođenje modela i postupci

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

U ovoj su tablici ažuriramo kao novih članaka i dodatne alate postaju dostupne za tu konfiguraciju. Kada je članak dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>O povezivanju

Više lokalnog web-mjesta možete povezati s jednom virtualne mreže. To je osobito privlačne za hibridno oblaka rješenja. Stvaranje veze s više web-mjesta u pristupnik za Azure virtualne mreže vrlo je sličan stvaranje ostale veze web-mjesto. Zapravo, možete koristiti postojeće Azure VPN pristupnika dok god je pristupnika dinamički (usmjeravanje sustavom).

Ako već imate statične pristupnika povezani s mrežom virtualne, možete promijeniti vrstu pristupnik za dinamičku bez obzira na to ponovno stvaranje virtualne mreže da biste omogućili više web-mjesta. Prije promjene usmjeravanje vrste, provjerite podržava li VPN pristupnikom lokalnim sustavom usmjeravanje konfiguracije VPN-a. 

![Dijagram više web-mjesta] (./media/vpn-gateway-multi-site/multisite.png "više web-mjesta")

## <a name="points-to-consider"></a>Točke treba uzeti u obzir

**Nećete moći koristiti Portal klasični Azure da biste promijenili virtualne mreže.** U ovom izdanju morat ćete promjene mreže konfiguracijska datoteka umjesto pomoću portala za klasični Azure. Ako promijenite na portalu klasični Azure, će prebrisati postavki reference na više web-mjesta za ovaj virtualni mrežu. 

Koje treba slobodno vrlo konfiguracijska datoteka mreže pomoću vrijeme dovršite postupak više web-mjesta. Međutim, ako imate više osoba koje rade na mrežnoj konfiguraciji, morat ćete da biste bili sigurni da svi znali o to ograničenje. To ne znači da ne možete koristiti Portal klasični Azure uopće. Možete ga koristiti za sve ostalo, osim promjene konfiguracije ovaj virtualne mreže.

## <a name="before-you-begin"></a>Prije početka

Prije nego počnete konfiguracije, provjerite jeste li sljedeće:

- Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).

- Kompatibilni hardver VPN-a za svaki lokalni mjesto. Provjerite [O VPN uređaji za virtualne mrežne veze](vpn-gateway-about-vpn-devices.md) da biste provjerili je li uređaj koji želite koristiti neki koji se zna da bi bio kompatibilan.

- Vanjsko dostupnog javno IPv4 IP adresa za svaki uređaj VPN-a. IP adresa nije ga moguće pronaći iza na NAT. To je potrebna.

- Morat ćete instalirati najnoviju verziju programa Azure PowerShell cmdleti. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

- Osobi koja se nalazi proficient pri konfiguraciji hardvera VPN-a. Nećete moći koristiti VPN skripte automatski generirani na portalu klasični Azure da biste konfigurirali uređajima VPN-a. To znači da morate imati istaknuti objašnjenje kako konfigurirati uređaju VPN-a ili rad s nekim tko ne.

- Na rasponi IP adresa koji želite koristiti za virtualne mreže (Ako već niste stvorili jedan). 

- Rasponi IP adresa za svaku lokalne mreže web-mjesta koja će se povezujete s. Morat ćete da biste bili sigurni da rasponi IP adresa za svaku lokalne mreže web-mjesta koje želite povezati s njime preklapati. U suprotnom portalu klasični Azure ili REST API-JA odbacit konfiguracije prenosi. 

    Ako, na primjer, ako imate dvije lokalne mreže web-mjesta i sadrže na IP adresa raspon 10.2.3.0/24, a imate paket s odredišnu adresu 10.2.3.3, Azure ne informacije koje želite poslati paket jer su preklapajuće raspone adresa web-mjesta. Da biste spriječili usmjeravanje problema, Azure ne dopušta da biste prenijeli konfiguracijska datoteka koja sadrži preklapajuće raspone.



## <a name="1-create-a-site-to-site-vpn"></a>1. stvaranje web-mjesto VPN-a

Ako već imate VPN-a web-mjesto s dinamički usmjeravanje pristupnika great! Možete nastaviti [Izvoz postavki konfiguriranje virtualne mreže](#export). Ako nije, učinite sljedeće:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Ako već imate web-mjesto virtualne mreže, ali ima statične pristupnika usmjeravanje (pravila sustavom):

1. Promjena vrste vaš pristupnika dinamički postupku. Više web-mjesta VPN-a potreban dinamički pristupnika usmjeravanje (poznat i kao usmjeravanje sustavom). Da biste promijenili vrstu sustava pristupnika, morat ćete najprije izbrišite postojeće pristupnika, a zatim stvorite novi. Upute potražite u članku [kako promijeniti vrstu za usmjeravanje VPN-a za pristupnikom](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Konfiguriranje novog pristupnika i stvorite vaše tunelom VPN-a. Upute potražite u članku [Konfiguriranje VPN pristupnika na portalu klasični Azure](vpn-gateway-configure-vpn-gateway-mp.md). Najprije promijenite vrstu sustava pristupnika dinamički postupku. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Ako nemate web-mjesto virtualne mreže:

1. Stvaranje web-mjesto virtualne mrežom pomoću ove upute: [Stvaranje virtualne mreže s vezom za VPN-a web-mjesto na portalu klasični Azure](vpn-gateway-site-to-site-create.md).  

2. Konfiguriranje pristupnika dinamički usmjeravanje pomoću ove upute: [Konfiguriranje VPN pristupnika](vpn-gateway-configure-vpn-gateway-mp.md). Obavezno odaberite **dinamičke usmjeravanje** za vrstu sustava pristupnika.

## <a name="export"></a>2. Izvoz konfiguracijska datoteka mreže 

Izvoz konfiguracijskoj datoteci mreže. Datoteka izvesti koristit će konfigurirati nove postavke više web-mjesta. Ako vam je potrebna upute za izvoz u datoteku, pogledajte odjeljak u članku: [Stvaranje VNet pomoću datoteke konfiguracije mreže na portalu za Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. otvorite datoteku konfiguracije mreže

Otvorite datoteku konfiguracije mreže koju ste preuzeli u posljednjem koraku. Koristite li xml uređivaču. Datoteke trebao bi izgledati otprilike ovako:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. dodavanje više referenci web-mjesta

Kada dodate ili uklonite referentne informacije za web-mjesta, ConnectionsToLocalNetwork/LocalNetworkSiteRef ćete unijeti promjene konfiguracije. Dodavanje novog okidača lokalnog web-mjesta referenca Azure da biste stvorili novi tunelom. U primjeru u nastavku mrežnoj konfiguraciji je za vezu jednog web-mjesta. Kada završite s promjenama, spremite datoteku.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. uvezete datoteku konfiguracije mreže

Uvezete datoteku konfiguracije mreže. Kada uvezete datoteku s promjenama, dodat će se novi tunnels. Na tunnels će koristiti pristupnika dinamički koju ste ranije stvorili. Ako vam je potrebna upute za uvoz datoteke, pogledajte odjeljak u članku: [Stvaranje VNet pomoću datoteke konfiguracije mreže na portalu za Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. preuzmite tipke

Kada dodate na novu tunnels pomoću cmdleta ljuske PowerShell `Get-AzureVNetGatewayKey` da biste dobili IPsec-IKE zajednički tipke za svaku tunelom.

Ako, na primjer:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Ako želite, možete koristiti i *Dobiti virtualne mreže zajednički ključ pristupnika* REST API-JA da biste dobili zajednički tipke.

## <a name="7-verify-your-connections"></a>7. Provjerite svoje veze

Provjera stanja tunelom više web-mjesta. Nakon preuzimanja tipke za svaku tunelom ćete koji želite provjeriti veze. Korištenje `Get-AzureVnetConnection` da biste dobili popis tunnels virtualne mreže, kao što je prikazano u primjeru u nastavku. VNet1 je naziv u VNet.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o pristupnika VPN, potražite u članku [O pristupnika VPN-a](../vpn-gateway/vpn-gateway-about-vpngateways.md).

