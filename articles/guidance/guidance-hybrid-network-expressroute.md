<properties
   pageTitle="Implementacijom arhitekturu hibridnog mreže s Azure ExpressRoute | Referenca arhitektura | Microsoft Azure"
   description="Kako implementirati arhitekturu sigurna mreža web-mjesto koji obuhvaća Azure virtualne mreže i lokalne mreže povezano s pomoću Azure ExpressRoute."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Implementacijom arhitekturu hibridnog mreže s Azure ExpressRoute

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

U ovom se članku opisuje najbolje prakse za povezivanje lokalne mreže virtualne mreže na Azure pomoću ExpressRoute. ExpressRoute veze vrše pomoću privatne veze namjenski putem davatelja povezivanje drugih proizvođača. Privatna veza proširuje lokalne mreže u Azure koja omogućuje pristup vlastite IaaS infrastrukture u Azure, javno krajnje točke koriste u PaaS i Office 365 SaaS services. Ovaj dokument usredotočuje se na korištenje ExpressRoute za povezivanje s jedne Azure virtualne mreže (VNet) pomoću pod nazivom privatne peering.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. U ovom nacrt koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Tipična Upotreba slučajeva za tu arhitekturu obuhvaćaju sljedeće:

- Hibridno aplikacije koje su radnih opterećenja distributed između lokalne mreže i Azure.

- Aplikacija veliki, zaštita njihove privatnosti ovise ključnih radnih opterećenja koji zahtijevaju veliku skalabilnost.

- Veliki sigurnosnog kopiranja i vraćanja funkcije za podatke koji mora biti spremljena zvuka.

- Rukovanje radnih opterećenja velikih skupova podataka.

- Korištenje Azure kao Izrada oporavak web-mjesta.

> [AZURE.NOTE] [Tehnički pregled ExpressRoute] [ expressroute-technical-overview] pruža Uvod u ExpressRoute.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Na sljedećem su dijagramu ističu važne komponente u toj arhitekturi:

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram nalazi se na stranici "Hibridnog mreže - a".

![[0]][0]

- **Azure virtualne mreže (VNets).** Svaki VNet nalazi se u jedno područje Azure, a možete hostirati više razine aplikacije. Razine aplikacije možete segmentirajući podmreže u svakom VNet i/ili mreže sigurnosnih grupa (NSGs). 

- **Azure javnog servisa.** To su Azure servise koje možete iskoristiti unutar hibridnog aplikacije. Ove usluge dostupne su i putem javnog Interneta, ali pristupanje njima putem programa ExpressRoute elektronička nudi niske latencije i predvidljivi performanse jer promet otvorite putem Interneta. Veza se izvode pomoću **javnog peering**, s adresama koje su čiji je vaša tvrtka ili ustanova ili koji vam je dao vaš davatelj usluga za povezivanje. 

- **Servisi za Office 365.** To su javno dostupnih aplikacija sustava Office 365 i usluge Microsoft. Veza se izvode pomoću programa **Microsoft peering**, adrese koje su čiji je vaša tvrtka ili ustanova ili koji vam je dao vaš davatelj usluga za povezivanje.

>[AZURE.NOTE] Možete povezati i izravno u programu Microsoft CRM Online putem Microsoft peering.

- **Lokalnu mrežu tvrtke.** Ovo je mreža računala i uređaja povezanih putem lokalnog područje privatnom radi unutar tvrtke ili ustanove.

- **Lokalni rub usmjerivača.** To su usmjerivača koji lokalne mreže povezati elektronička upravlja davatelj usluge. Ovisno o tome kako je dodjeli vezu, morate unijeti javnu IP adrese koristi u usmjerivača. 

- **Microsoft edge usmjerivača.** Slijede dva usmjerivača u konfiguraciji iznimno dostupna aktivno aktivno. Ove usmjerivača Omogući povezivanje davatelja njihove krugova izravno povezivanje s njihovim podatkovnim centrom. Ovisno o tome kako je dodjeli vezu, morate unijeti javnu IP adrese koristi u usmjerivača.

- **Elektronička ExpressRoute.** To je sloja 2 ili elektronička layer 3 koji vam je dao davatelj usluga za povezivanje tog spojevi u lokalni mreža s Azure preko ruba usmjerivača. Na elektronička koristi infrastrukture hardver upravlja davatelj usluga za povezivanje.

- **Povezivanje davatelje usluga.** To su tvrtke koje nude veze ili pomoću sloja 2 ili layer 3 veze između sustava podatkovnog centra i Azure podatkovnog centra.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji ne stane i preporuke.

### <a name="connectivity-providers"></a>Povezivanje davatelja usluga

Odaberite odgovarajuću davatelja povezivanje ExpressRoute za svoju lokaciju. Da biste dobili popis povezivanje davatelje usluga dostupni na vašoj lokaciji, koristite sljedeću naredbu Azure PowerShell:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

Davatelji povezivanje ExpressRoute povezati svoje podatkovnog centra Microsoftu na sljedeće načine:

- **Suradnja nalazi se na sustavu exchange oblaka.** Ako zajednički koristite nalazi u funkciji sa sustavom exchange oblaka, redoslijed možete virtualne unakrsno-veze Microsoft cloud putem davatelja zajednički mjesto Ethernet exchange. Suautorstvo mjesto možete nudi više veza sloja 2 ili upravljanih Layer 3 unakrsno-veze između preduvjete infrastrukture u funkciji zajednički mjesto i Microsoft cloud...

- **Point-to-Point Ethernet veze.** Lokalni podatkovnim centrima/ureda možete povezati s Microsoft cloud kroz point-to-point Ethernet veze. Davatelji Ethernet Point-to-Point može ponuditi sloja 2 veze ili upravlja Layer 3 veze između web-mjesta i Microsoft cloud.

- **Bilo koji-na-bilo koje mreže (IPVPN).** Vaš WAN možete integrirati s Microsoft cloud. IPVPN (obično MPLS VPN-a) nudi bilo koje-na-bilo koje veze između podružnicama i podatkovnim centrima. Microsoft cloud može biti povezana za vaše WAN kako bi izgledao baš kao i sve druge podružnici. WAN davatelji usluga obično nude upravljanih povezivanje Layer 3.

Dodatne informacije o davateljima povezivanje potražite u članku [ExpressRoute krugova i usmjeravanje domene][connectivity-providers].

### <a name="expressroute-circuit"></a>Elektronička ExpressRoute

Provjerite je li vaša tvrtka ili ustanova ima ispunjen [ExpressRoute pripremni preduvjeti] [ expressroute-prereqs] o povezivanju sa servisom Azure.

Ako to još niste učinili, dodajte podmreže pod nazivom `GatewaySubnet` za svoje VNet Azure i stvaranje pristupnika virtualne mreže ExpressRoute koja se pomoću servisa Azure VPN pristupnika. Dodatne informacije o tom se postupku potražite u članku [Tijekovi rada ExpressRoute za dodjelu resursa elektronička i država elektronička][ExpressRoute-provisioning].

Stvaranje je elektronička ExpressRoute na sljedeći način:

1. Pokrenite sljedeću naredbu komponente PowerShell:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Slanje u `ServiceKey` za nove elektronička davatelju usluge.

3. Pričekajte Dodjela resursa u elektronička davatelja usluga. Možete provjeriti stanje dodjele resursa u elektronička pomoću sljedeće naredbe ljuske PowerShell:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

Na `Provisioning state` u na `Service Provider` dio izlaz promijenit će se iz `NotProvisioned` da biste `Provisioned` kada je spremna za elektronička.

>[AZURE.NOTE]Ako koristite vezu layer 3, davatelj trebali biste konfigurirati i upravljati njima usmjeravanje umjesto vas; Navedite podatke potrebne za omogućivanje davatelj usluga za implementaciju odgovarajuće usmjerava.

Ako koristite vezu sloja 2, slijedite ove korake:

1. Rezerviranje dva/30 podmreže sastoji od valjani javnu IP adresa za svaku vrstu peering želite implementirati. Te/30 podmreže koristit će se omogućuje IP adresa za usmjerivača koji se koriste za na elektronička. Ako su implementacijom privatno, javno i Microsoft peering, morat ćete 6 /30 podmreže s valjanim javnu IP adresama.     

2. Konfiguriranje usmjeravanja za elektronička ExpressRoute. Morate pokrenuti naredbu ispod za svaku vrstu peering želite konfigurirati (privatno, javno i Microsoft).

    >[AZURE.NOTE]U odjeljku [Stvaranje i izmjena usmjeravanja za je elektronička ExpressRoute] [ configure-expressroute-routing] detalje. Da biste dodali mreže peering za usmjeravanje prometa, koristite sljedeće naredbe ljuske PowerShell:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Rezerviranje skup valjani javnu IP adrese namijenjen je za NAT javno i Microsoft peering. Preporučuje se da bi drugi skup za svaku peering. Navedite skup davatelju usluge povezivanja tako da ih možete konfigurirati BGP reklame tih raspona.

[Veza na privatne VNet(s) u oblaku na sklopovske ExpressRoute][link-vnet-to-expressroute]. Koristite sljedeće naredbe ljuske PowerShell:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Imajte na umu sljedeće točke:

- ExpressRoute koristi protokol pristupnika obruba (BGP) za razmjenu usmjeravanje između mreže i Azure.

- Možete se povezati s više VNets koja se nalazi u različite regije na istom elektronička ExpressRoute sve dok se sve VNets i ExpressRoute elektronička se nalaze unutar iste Geopolitički područja.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

ExpressRoute krugova Navedite put visoke propusnosti između mreže. Općenito govoreći, viša propusnosti je veća trošak. Iako Neki davatelji omogućuju promjenu vaše propusnosti, provjerite je li odaberite početni podatkovnog prometa koji surpasses vaše potrebe, a omogućuje prostor za rast. U slučaju da trebate da biste povećali propusnost u budućnosti, ostaju dvije mogućnosti.

Povećanje propusnosti. Imajte na umu sve davatelje usluga mogli da to učinite dinamički. I izbjegavajte potrebe za učinite ovo najveću moguću. No ako je potrebno, nakon provjere kod davatelja usluge naredbe pokrenite u nastavku.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Možete povećati propusnosti bez gubitka veze. Prijelaz na stariju propusnosti rezultirat će u prekidu povezivanje. Imate da biste izbrisali s instalacijom i ponovno ga stvorite novu konfiguraciju.

Ako koristite standardne SKU-om za ExpressRoute i morate nadograditi na Premium ili promjena koje ste vaše cijene planiranje (s ograničenim prometom ili neograničeno), naredbe pokrenite u nastavku. Na `Sku.Tier` svojstvo može biti `Standard` ili `Premium`; na `Sku.Name` svojstvo može biti `MeteredData` ili `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Provjerite je li u `Sku.Name` svojstvo podudaranja s `Sku.Tier` i `Sku.Family`. Ako promijenite obitelji i sloju, ali ne naziv, veza će biti onemogućene.

Možete nadograditi SKU bez prekidu, ali ne možete promijeniti iz da s ograničenim prometom neograničeno cijene plan. Kada se SKU prijelaz na stariju, vaše utrošak propusnosti mora biti unutar ograničenja za zadani standardni SKU-om.

> [AZURE.NOTE] ExpressRoute nudi dva cijene plana klijentima, na temelju mjerne ili neograničeno podataka. U odjeljku [ExpressRoute cijene] [ expressroute-pricing] detalje. Troškovi razlikuju se ovisno o propusnosti elektronička. Raspoložive propusnosti vjerojatno razlikuju se od davatelja davatelja usluga. Korištenje na `Get-AzureRmExpressRouteServiceProvider` cmdlet da biste vidjeli davatelji dostupna u vašoj regiji i bandwidths koje se nude.

Jedan ExpressRoute elektronička podržavaju broj peerings i VNet veze. Potražite u članku [ograničenja ExpressRoute] [ expressroute-limits] dodatne informacije.

Trošak dodatni ExpressRoute Premium dodatak predstavlja:

- Do 10 000 usmjerava po elektronička. Bez ExpressRoute Premium dodatak, ograničenje iznosi 4000 usmjerava po elektronička.

- Globalni povezivanje, omogućivanje programa ExpressRoute elektronička smještene bilo gdje u svijetu pristupa resursima servisa sve regije umjesto samo regije u istom kontinent.

- Povećava broj veza VNet po sklopovske od 10 veća ograničenja, ovisno o propusnosti s instalacijom.

ExpressRoute krugova su osmišljeni tako da omogućuje privremeni problem s mrežom bursts do dva puta propusnosti granicu nabavljaju bez dodatnih troškova. To možete postići pomoću suvišnih veze. Međutim, nije svih proizvođača povezivanje to podržava; Provjerite je li vaš davatelj usluga za povezivanje omogućuje je značajka prije ovisno o tome je.

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Visoke dostupnosti možete konfigurirati za Azure vezu na različite načine, ovisno o vrsti davatelja koristite i broj ExpressRoute krugova i virtualne mreže pristupnika veze želite konfigurirati. Točke ispod sažetak dostupnosti mogućnosti:

- ExpressRoute podržava usmjerivač zalihosti protokole kao što su HSRP i VRRP implementaciju visoke dostupnosti. Umjesto toga, koristi suvišnih par sesija BGP po peering. Da biste olakšali iznimno dostupna veza s mrežom, Microsoft Azure dodjeljuje vam dvije suvišnih priključke na dva usmjerivača (dio Microsoft edge) u konfiguraciji aktivno aktivno.

- Ako koristite vezu sloja 2, implementirati suvišnih usmjerivača u lokalnu mrežu u konfiguraciji aktivno aktivno. Primarni elektronička se povezati s jednog usmjerivača i sekundarne elektronička na drugi. To će vam iznimno dostupna veza na oba kraja vezu. To je potrebno ako su vam potrebne ExpressRoute SLA. U odjeljku [SLA za Azure ExpressRoute] [ sla-for-expressroute] detalje.

Prikazano u sljedećem dijagramu u konfiguraciji s suvišnih lokalnog usmjerivača povezani krugova primarnih i sekundarnih. Svaki elektronička rukuje promet za javne peering i privatni peering (svaki peering je označen par /30 adresa razmake, kao što je opisano u prethodnom odjeljku).

![[1]][1]

Ako koristite layer 3 veze, provjerite je li u njoj nalaze suvišnih BGP sesije koji upravljaju dostupnost umjesto vas.

Virtualne mreže može se povezati s više ExpressRoute krugova i svaki krugova mogu biti iz različitih davateljima usluga. U ovom strategije pruža dodatne visoku dostupnost i Izrada oporavak mogućnosti.

Konfiguriranje web-mjesto VPN-a kao put prebacivanje za ExpressRoute. Samo je dodijeljen privatne peering. Za Azure i Office 365 Internet je put samo prebacivanje.

Prema zadanim postavkama BGP sesije pomoću vrijednost neaktivnosti vremensko ograničenje od 60 sekundi. Nakon što sesije timed out 3 puta, smjer je označena kao nije dostupan, a sve promet se preusmjerava na preostale usmjerivač. Možda je ovog vremenskog ograničenja 180 druge predug za ključne aplikacije. U tom slučaju možete promijeniti postavke isteka BGP na lokalni usmjerivač u manju vrijednost.

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

Pomoću [Alata za povezivanje Azure (AzureCT)] [ azurect] praćenje veze između lokalnog podatkovnog centra i Azure.

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Možete konfigurirati sigurnosne mogućnosti za Azure vezu na različite načine, ovisno o problemima sigurnosti i usklađenost potrebama. Točke ispod sažetak sigurnosne mogućnosti.

ExpressRoute funkcionira u layer 3. Prijetnji u aplikacijskom sloju spriječit se pomoću potražite sigurnost mreže koji ograničava promet valjane resursi. Uz to, ExpressRoute veze pomoću javnog peering se inicirati samo iz lokalnog. To sprječava rogue servisa pristupa i ugrozi lokalnih podataka s Interneta javno.

Da biste maksimizirali sigurnost, dodajte aparata sigurnost mreže između lokalne mreže i ruba usmjerivača davatelja usluga. To će vam pomoći da biste ograničili inflow neovlašteno promet iz na VNet:

![[2]][2]

Za nadzor i usklađenost svrhe, možda će biti potrebno sprječavaju izravan pristup iz komponente koji se izvodi u na VNet s Internetom i implementirati [prisilno tuneliranje][forced-tuneling]. U tom slučaju internetski promet mora biti preusmjereni unatrag kroz proxy radi lokalnog koju je moguće nadzirati. Blokiranje neovlašteno promet slijedi izgleda i filtriranje potencijalno štetne dolazni promet moguće je konfigurirati proxy poslužitelj.

![[3]][3]

Prema zadanim postavkama Azure VMs izložiti krajnje točke za osiguravanja pristupa za prijavu radi upravljanja - RDP i udaljene ljuske Powershell za Windows VMs i SSH za sustavom Linux VMs kada implementiran putem portala za Azure. Da biste maksimizirali sigurnost, omogućite javnu IP adresa za vaše VMs i koristite NSGs da biste bili sigurni da nisu te VMs javno dostupno. VMs samo mora biti dostupan interne IP adrese. Ove adrese možete mogu učiniti pristupačnima putem mreže ExpressRoute, omogućivanje lokalnog DevOps osoblja za izvođenje potrebnih konfiguracija ni održavanja.

Ako morate izložiti upravljanje krajnje točke za VMs u vanjskoj mreži, koristite NSGs i/ili pristupiti popisi za upravljanje da biste ograničili vidljivost priključke whitelist IP adresa ili mreže.

## <a name="troubleshooting-considerations"></a>Napomene za otklanjanje poteškoća

Prethodno radi ExpressRoute elektronička sada ne uspije povezati, za izostanak sve konfiguracije promjene na lokalni ili unutar vaše privatne VNet možda ćete morati obratite se davatelju povezivanje i raditi s njima da biste riješili problem. Sljedeće naredbe Azure PowerShell možete koristiti i za izvođenje neke ograničeni Provjera i utvrditi gdje može biti problema:

- Provjerite je li u elektronička je stvorena:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Izlaz iz ta naredba prikazuje nekoliko svojstava elektronička, uključujući `ProvisioningState`, `CircuitProvisioningState`, a `ServiceProviderProvisioningState` kao što je prikazano u nastavku.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Ako u `ProvisioningState` nije postavljena na `Succeeded` nakon što ste pokušali stvoriti novi elektronička, uklonite na elektronička pomoću naredbe "ispod" i pokušajte ponovno stvoriti.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Ako je vaš davatelj usluga imali već dodjeli elektronička, i `ProvisioningState` postavljen na `Failed`, ili `CircuitProvisioningState` nije `Enabled`, pomoć zatražite od davatelja usluga.

## <a name="solution-deployment"></a>Uvođenje rješenja

Ako imate postojeću lokalnog infrastrukturu konfigurirane s odgovarajuću mreže potražite, možete implementirati arhitektura referenca na sljedeći način:

Ako imate postojeću lokalnog infrastrukturu konfigurirane s VPN-a potražite, možete implementirati arhitektura referenca na sljedeći način:

1. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Pričekajte vezu da biste otvorili na portalu za Azure, a zatim slijedite ove korake: 
  - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-hybrid-er-rg` u tekstni okvir.
  - **Mjesto** padajućem okviru odaberite regiju.
  - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
  - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
  - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

4. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru":  
[![Implementacija Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Pričekajte vezu da biste otvorili na portalu za Azure, a zatim slijedite ove korake: 
  - Odaberite **Koristi postojeći** u odjeljku **grupa resursa** i unesite `ra-hybrid-er-rg` u tekstni okvir.
  - **Mjesto** padajućem okviru odaberite regiju.
  - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
  - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
  - Kliknite gumb **za kupnju** .

6. Pričekajte implementaciju da biste dovršili.

## <a name="next-steps"></a>Daljnji koraci

- Potražite u članku [implementacijom arhitekturu mreže iznimno dostupna hibridnog] [ highly-available-network-architecture] informacije o povećanje dostupnosti mreže hibridnog koji se temelje na ExpressRoute neuspjeh putem VPN veza.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hibridno mreže arhitektura pomoću Azure ExpressRoute"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Pomoću suvišnih usmjerivača ExpressRoute primarnih i sekundarnih krugova"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Dodavanje sigurnosni uređaji lokalne mreže"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Korištenje prisilno tuneliranja u nadzornom promet povezanih s Interneta"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Pronalaženje ServiceKey je elektronička ExpressRoute"
