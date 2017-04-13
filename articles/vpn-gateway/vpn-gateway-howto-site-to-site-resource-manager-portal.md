<properties
   pageTitle="Stvaranje virtualne mreže s web-mjesto VPN vezu pomoću upravitelja resursa Azure i portalu Azure | Microsoft Azure"
   description="Kako stvoriti VNet pomoću modela implementacije Voditelj resursa i povežite ga s svog lokalnog lokalne mreže pomoću S2S VPN veza pristupnika."
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

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Stvaranje na VNet s vezom za web-mjesto pomoću portala za Azure

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasični – klasični Portal](vpn-gateway-site-to-site-create.md)


U ovom se članku vodit će vas voditi kroz stvaranje virtualne mreže i web-mjesto VPN vezu pristupnika lokalne mreže pomoću model implementacije Voditelj resursa Azure i Azure portal. Veza web-mjesto koje se mogu koristiti za više lokalni i Hibridni konfiguracije.

![Dijagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Uvođenje modela i postupci za veze na web-mjesto

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Sljedeća tablica prikazuje trenutno dostupno implementacije modela i postupci za konfiguracije web-mjesto. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatna konfiguracija 

Ako želite da se povežu VNets, ali stvarate vezu s lokalne lokacije, potražite u članku [Konfiguriranje VNet VNet veze](vpn-gateway-vnet-vnet-rm-ps.md). Ako želite dodati vezu web-mjesto VNet koji već ima veze, potražite u članku [Dodavanje veze S2S VNet s postojeće veze pristupnika VPN-a](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Prije početka

Provjerite jeste li sljedeće stavke prije početka vašoj konfiguraciji:

- Kompatibilni uređaj VPN-a i nekome tko se može konfigurirati. Potražite u članku [o uređajima VPN-a](vpn-gateway-about-vpn-devices.md). Ako ne poznajete konfiguriranje uređaju VPN, ili nisu poznati s IP adresom raspona koji se nalazi u vaše lokalne mreže konfiguracije, potrebno je koordinaciji s nekim tko može zatražiti te detalje za vas.

- Vanjsko dostupnog javnu IP adresu za svoj uređaj VPN-a. U ovom IP adresa nije ga moguće pronaći iza na NAT.
    
- Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Ogledne vrijednosti konfiguracije za ovu vježbu


Kada koristite sljedeće korake kao za vježbu, možete koristiti ogledne vrijednosti konfiguracije:

- **VNet naziv:** TestVNet1
- **Adresa prostora:** 10.11.0.0/16 i 10.12.0.0/16
- **Podmreže:**
    - Sučelju: 10.11.0.0/24
    - Pozadinskog: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Grupa resursa:** TestRG1
- **Lokacije:** Istočni SAD-a
- **DNS poslužitelj:** 8.8.8.8
- **Naziv pristupnika:** VNet1GW
- **Javnu IP:** VNet1GWIP
- **Vrsta VPN:** Usmjeravanje-poštu
- **Vrsta veze:** Web-mjesta web-na-mjesta (IPsec)
- **Vrsta pristupnika:** VPN-A
- **Naziv pristupnika lokalne mreže:** Site2
- **Naziv veze:** VNet1toSite2


## <a name="CreatVNet"></a>1. stvaranje virtualne mreže 

Ako već imate VNet, provjerite jesu li postavke kompatibilne s dizajnom pristupnika VPN-a. Posebno obratite pažnju na bilo kojem podmreže koji može s njime preklapati s drugim mrežama. Ako imate podmreže koji se preklapaju, vezu neće pravilno funkcionirati. Ako je vaš VNet konfiguriran ispravnim postavkama, možete početi korake u odjeljku [Navedite DNS poslužitelj](#dns) .

### <a name="to-create-a-virtual-network"></a>Da biste stvorili virtualne mreže

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. dodavanje dodatnih adresa prostora i podmreže

Možete dodati prostor dodatne adrese i podmreže vaše VNet kada je stvorena.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Navedite DNS poslužitelj

### <a name="to-specify-a-dns-server"></a>Da biste odredili DNS poslužitelj

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. stvaranje podmreži pristupnika

Prije povezivanja virtualne mreže pristupnika, najprije morate stvoriti podmreže pristupnik za virtualne mreže na koju se želite povezati. Ako je to moguće, najbolje je da biste stvorili podmreži pristupnika pomoću blok CIDR /28 ili /27 omogućili dovoljno IP adrese kako bi odgovarao dodatne buduće konfiguracijske preduvjeti.

Ako stvarate tu konfiguraciju kao za vježbu, pročitajte ove [vrijednosti](#values) prilikom stvaranja vašoj podmreži pristupnika.

### <a name="to-create-a-gateway-subnet"></a>Da biste stvorili podmreži pristupnika


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. stvaranje pristupnika virtualne mreže

Ako stvarate tu konfiguraciju kao za vježbu, možete se referirati na [ogledne vrijednosti konfiguracije](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Da biste stvorili pristupnik virtualne mreže

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. stvaranje pristupnika za lokalnu mrežu

'Lokalne mreže pristupnik' se odnosi na lokalne lokacije. Naziv lokalne mreže pristupnika po kojoj Azure može se odnositi na njega. 

Ako stvarate tu konfiguraciju kao za vježbu, možete se referirati na [ogledne vrijednosti konfiguracije](#values).

### <a name="to-create-a-local-network-gateway"></a>Da biste stvorili pristupnik lokalne mreže

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. konfiguriranje uređaju VPN-a

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Stvori web-mjesto VPN vezu

Stvaranje web-mjesto VPN veza između pristupnikom virtualne mreže i uređaju VPN-a. Provjerite vrijednosti zamijeniti vlastitim. Zajednički ključ mora odgovarati vrijednost koja se koristi za konfiguraciju uređaj VPN-a. 

Prije početka Ova sekcija, provjerite je li da pristupnik za virtualne mreže i lokalne mreže pristupnika završili ste sa stvaranjem. Ako stvarate tu konfiguraciju kao programa vježbu, pročitajte ove [vrijednosti](#values) prilikom stvaranja vezu.

### <a name="to-create-the-vpn-connection"></a>Da biste stvorili vezu VPN-a


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Provjerite je li VPN veza

Možete provjeriti VPN veza na portalu ili pomoću komponente PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Daljnji koraci

- Nakon dovršetka vezu, možete dodati virtualnim strojevima virtualne mreže. Pogledajte na virtualnim strojevima [tečaj](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) dodatne informacije.

- Informacije o BGP potražite u članku [Pregled BGP](vpn-gateway-bgp-overview.md) i [konfiguriranju BGP](vpn-gateway-bgp-resource-manager-ps.md).
