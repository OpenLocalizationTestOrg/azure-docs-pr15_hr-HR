<properties
   pageTitle="Implementacijom arhitekturu mreže iznimno dostupna hibridnog | Microsoft Azure"
   description="Kako implementirati arhitekturu sigurna mreža web-mjesto koji obuhvaća Azure virtualne mreže i lokalne mreže povezani pomoću ExpressRoute prebacivanje pristupnika VPN-a."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Implementacijom arhitekturu iznimno dostupna hibridnog mreže

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje najbolje prakse za povezivanje lokalne mreže virtualne mreže na Azure pomoću ExpressRoute, na web-mjesto virtualne privatne mreže (VPN-a) kao prebacivanje vezu. Promet se odvija između lokalne mreže i programa Azure virtualne mreže (VNet) putem veze ExpressRoute.  Ako postoji gubitak povezivost u elektronička ExpressRoute, promet će usmjerena kroz programa tunelom IPSec VPN-a.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. U ovom nacrt koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Hibridno aplikacije koje su radnih opterećenja distributed između lokalne mreže i Azure.

- Aplikacija veliki, zaštita njihove privatnosti ovise ključnih radnih opterećenja koji zahtijevaju veliku skalabilnost.

- Veliki sigurnosnog kopiranja i vraćanja funkcije za podatke koji mora biti spremljena zvuka.

- Rukovanje radnih opterećenja velikih skupova podataka.

- Korištenje Azure kao Izrada oporavak web-mjesta.

Imajte na umu da ako elektronička ExpressRoute nije dostupan, usmjeravanje VPN će obrađivati samo privatne peering veze. Javni peering i Microsoft peering veze proći kroz putem Interneta.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

>[AZURE.NOTE] [Servis za Azure VPN pristupnika] [ azure-vpn-gateway] implementira dvije vrste pristupnika virtualne mreže; VPN virtualne mreže pristupnika i ExpressRoute virtualne mreže pristupnika. U ovom dokumentu termina *VPN pristupnika* se odnosi na servis za Azure, tijekom izraze *VPN virtualne mreže pristupnika* i *ExpressRoute virtualne mreže pristupnika* koriste se za odnosno uputiti implementacije VPN-a i ExpressRoute pristupnika.

Na sljedećem su dijagramu ističu važne komponente u toj arhitekturi:

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram je na "hibridnog mreži - ER VPN" stranice.

![[0]][0]

- **Azure virtualne mreže (VNets).** Svaki VNet nalazi se u jedno područje Azure, a možete hostirati više razine aplikacije. Razine aplikacije možete segmentirajući podmreže u svakom VNet i/ili mreže sigurnosnih grupa (NSGs).

- **Lokalnu mrežu tvrtke.** Ovo je mreža računala i uređaja povezanih putem lokalnog područje privatnom radi unutar tvrtke ili ustanove.

- **Potražite VPN-a.** Ovo je uređaj ili servis koji omogućuje vanjske povezivost za lokalnu mrežu. Potražite VPN-a može biti hardverski uređaj ili može biti rješenje softver kao što su usmjeravanje i daljinski pristup servisa (RRAS) u sustavu Windows Server 2012.

> [AZURE.NOTE] Popis podržanih aparata VPN-a i informacije o konfiguriranju odabrane aparata VPN-a za povezivanje programa Azure VPN pristupnika, potražite u članku upute za odgovarajući uređaj na [popisu uređaja VPN podržava Azure][vpn-appliance].

- **Pristupnik virtualne mreže VPN-a.** Pristupnik za VPN virtualne mreže omogućuje VNet za povezivanje s VPN-a potražite u lokalnu mrežu. Pristupnik za VPN virtualne mreže je konfiguriran za prihvaćanje zahtjeva iz lokalne mreže samo putem VPN-a potražite. Dodatne informacije potražite u članku [Povezivanje lokalne mreže za Microsoft Azure virtualne mreže][connect-to-an-Azure-vnet].

- **ExpressRoute virtualne mreže pristupnik.** Pristupnik virtualne mreže ExpressRoute omogućuje VNet povezati elektronička ExpressRoute koji se koristi za povezivanje s mrežom na lokalni.

- **Podmreže pristupnika.** Virtualne mreže pristupnika drže u istoj podmreži.

- **VPN veza.** Veza ima svojstva koja odredite vrstu veze (IPSec) i ključ koji se zajednički koriste s VPN-a potražite na lokalni šifriranje promet.

- **Elektronička ExpressRoute.** To je sloja 2 ili elektronička layer 3 koji vam je dao davatelj usluga za povezivanje tog spojevi u lokalni mreža s Azure preko ruba usmjerivača. Na elektronička koristi infrastrukture hardver upravlja davatelj usluga za povezivanje.

- **Oblak N sloja aplikacija.** Ovo je aplikacija smješten u Azure. On može sadržavati više razine, s više podmreže povezanih putem balancers Azure Učitaj. Promet u svakom podmreže mogu se primjenjivati pravila definirane pomoću [Azure mreže sigurnosne grupe][azure-network-security-group](NSGs). Dodatne informacije potražite u članku [Uvod u Microsoft Azure sigurnost][getting-started-with-azure-security].

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji ne stane i preporuke.

### <a name="vnet-and-gatewaysubnet"></a>VNet i GatewaySubnet

Stvaranje pristupnika ExpressRoute virtualne mreže i virtualne mreže pristupnika VPN-a u istu VNet. To znači da trebaju dijeliti istoj podmreži pod nazivom **GatewaySubnet**

Ako na VNet već sadrži podmreže pod nazivom **GatewaySubnet**, provjerite sadrži li na /27 ili veći prostor adrese. Ako je postojeći podmreže presitan, ukloniti na sljedeći način i stvorite novu, kao što je prikazano na sljedeću grafičku oznaku:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Ako na VNet sadrže podmreže pod nazivom **GatewaySubnet**, zatim stvorite novi na sljedeći način:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN-a i ExpressRoute pristupnik

Provjerite ispunjava li vaša organizacija [ExpressRoute pripremni preduvjeti] [ expressroute-prereq] o povezivanju sa servisom Azure.

Ako već imate pristupnik virtualne mreže VPN-a u vašem VNet Azure, uklonite ga, kao što je prikazano u nastavku.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Slijedite upute u [implementacijom arhitekturu hibridnog mreže s Azure ExpressRoute] [ implementing-expressroute] da biste uspostavili vezu s ExpressRoute.

Slijedite upute u [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN] [ implementing-vpn] da biste uspostavili VPN veza pristupnika virtualne mreže.

Nakon što ste uspostavili virtualne mrežnih veza pristupnika, testirajte okruženje kao sljedeće:

1. Provjerite je li kojima se možete povezati s lokalne mreže vaše VNet Azure.

2. Obratite se davatelju da biste prestali s povezivanjem ExpressRoute za testiranje.

3. Provjerite koje možete i dalje se povezati iz lokalne mreže za vaše VNet Azure pomoću VPN veza virtualne mreže pristupnika.

4. Obratite se davatelju za reestabilish ExpressRoute povezivanje.

## <a name="considerations"></a>Razmatranja

Razmatranja ExpressRoute potražite u članku [implementacijom arhitekturu hibridnog mreže s Azure ExpressRoute] [ guidance-expressroute] upute.

Pitanja vezana uz web-mjesto VPN, u odjeljku [implementacijom arhitekturu hibridnog mreže s Azure i lokalnih VPN] [ guidance-vpn] upute.

Općenito Azure sigurnosna pitanja vezana uz, potražite u članku [Microsoftovim servisima u oblaku i sigurnost mrežne][best-practices-security].

## <a name="solution-deployment"></a>Uvođenje rješenja

Ako imate postojeću lokalnog infrastrukturu konfigurirane s odgovarajuću mreže potražite, možete implementirati arhitektura referenca na sljedeći način:

1. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Pričekajte vezu da biste otvorili na portalu za Azure, a zatim slijedite ove korake: 
  - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-hybrid-vpn-er-rg` u tekstni okvir.
  - **Mjesto** padajućem okviru odaberite regiju.
  - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
  - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
  - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

4. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Pričekajte vezu da biste je otvorili na portalu Azure, a zatim unesite a zatim slijedite ove korake: 
  - Odaberite **Koristi postojeći** u odjeljku **grupa resursa** i unesite `ra-hybrid-vpn-er-rg` u tekstni okvir.
  - **Mjesto** padajućem okviru odaberite regiju.
  - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
  - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
  - Kliknite gumb **za kupnju** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Arhitektura arhitekturu iznimno dostupna hibridnog mreže pomoću ExpressRoute i VPN pristupnika"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
