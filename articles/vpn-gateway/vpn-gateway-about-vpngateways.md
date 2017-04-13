<properties 
   pageTitle="O VPN pristupnika | Microsoft Azure"
   description="Saznajte više o pristupnika VPN veza za Azure virtualne mreže."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>O pristupnika VPN-a


Pristupnik virtualne mreže koristi se za slanje mrežni promet između Azure virtualne mreže i lokalne lokacije i i virtualne mreže unutar Azure (VNet-VNet). Kada konfigurirate pristupnik VPN-a, morate stvoriti i konfigurirati pristupnik virtualne mreže i virtualne mrežne veze pristupnika.

U model za uvođenje resursima prilikom stvaranja virtualne mrežnog resursa za pristupnik navedite nekoliko postavki. Jedna je od potrebne postavke "-GatewayType". Postoje dvije vrste pristupnika virtualne mreže: VPN-a i ExpressRoute. 

Kada se na namjenski privatne veze šalje mrežnog prometa, koristite vrstu pristupnik 'ExpressRoute'. To se naziva pristupnika za ExpressRoute. Kad mrežni promet poslane šifrirane putem javne veze, koristite vrstu pristupnik "Vpn". To se naziva pristupnika VPN-a. Web-mjesta-na-web-mjesta, točke web i VNet VNet veze sve koristite pristupnik VPN-a.

Svaki virtualne mreže može imati samo jedan pristupnik virtualne mreže po vrsti pristupnika. Ako, na primjer, moguće je mreža virtualne pristupnik koji koristi - GatewayType ExpressRoute i onaj koji koristi - GatewayType VPN-a. U ovom se članku usredotočuje se prvenstveno na VPN pristupnika. Dodatne informacije o ExpressRoute potražite u članku [ExpressRoute Tehnički pregled](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Cijene

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>SKU-ove pristupnika

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Dodatne informacije o pristupnika SKU-ove potražite u članku [Pristupnika SKU-ove](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Procijenjena propusnost zbrajanja po SKU

U sljedećoj su tablici prikazane vrste pristupnika i Procijenjena zbrajanja propusnost. U ovoj su tablici primjenjuje se na Voditelj resursa i uvođenje klasičnog modela.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>Konfiguriranje pristupnik VPN-a

Kada konfigurirate pristupnik VPN, upute koristite ovise o model implementacije koje ste koristili za stvaranje virtualne mreže. Na primjer, ako ste stvorili na VNet pomoću modela uvođenje klasičnog, koristite upute i upute za uvođenje klasičnog model za stvaranje i konfiguriranje postavki pristupnika VPN-a. Dodatne informacije o implementaciji modelima potražite u članku [Upravitelj resursa za razumijevanje i uvođenje klasičnog modela](../resource-manager-deployment-model.md).

VPN veza pristupnika ovisi o višestrukih resursa konfiguriranih pomoću određene postavke. Većina resurse koji se može konfigurirati zasebno, iako mora biti konfigurirana u određenom redoslijedu u nekim slučajevima. Možete početi stvaranje i konfiguriranje resurse pomoću jedne alat za konfiguraciju, kao što su Azure portal. Možete pa naknadno odlučite da biste se prebacili na neki drugi alat, kao što su komponente PowerShell da biste konfigurirali dodatne resurse ili da biste izmijenili postojeći resursi kada je primjenjivo. Trenutno ne možete konfigurirati svaki resursa i postavke resursa na portalu za Azure. Upute u člancima za svaku vezu topologije navedite kad je potreban alat za određenu konfiguraciju. Informacije o pojedinačnih resursa i postavke za pristupnik za VPN, potražite u članku [o pristupnika VPN postavke](vpn-gateway-about-vpn-gateway-settings.md).

Sljedeće sekcije sadrže tablice koje popisa:

- model dostupna implementacije
- Alati za konfiguraciju dostupna
- veze koje će vas odvesti izravno na članak, ako je dostupna

Pomoću dijagrame i opise odaberite topologije veza u skladu s potrebama. Dijagrami prikazivati topologija glavni osnovne, ali je moguće da biste sastavili složenije konfiguracije pomoću dijagrame kao s većinom.

## <a name="site-to-site-and-multi-site"></a>Web-mjesto i više web-mjesta

### <a name="site-to-site"></a>Web-mjesto

Web-mjesta web-na-mjesta (S2S) VPN veza pristupnik je veza putem tunelom VPN IPsec/IKE (IKEv1 ili IKEv2). Tu vrstu veze potreban je VPN uređaju nalazi lokalnog koja ima javnu IP adresa dodijeljena i se nalazi iza na NAT. S2S veze koje se mogu koristiti za više lokalni i Hibridni konfiguracije.   

![S2S veze] (./media/vpn-gateway-about-vpngateways/demos2s.png "web-mjesto")


### <a name="multi-site"></a>Više web-mjesta

Možete stvoriti i konfiguriranje VPN pristupnika veze između sustava VNet i više mreža za lokalni. Rad s više veza, morate koristiti vrstu RouteBased VPN-a (dinamički pristupnik za klasični VNets). Budući da je VNet može imati samo jedan pristupnik VPN, sve veze do pristupnika dijelite raspoložive propusnosti. To se često naziva veze "više web-mjesta".
 

![Veza na više web-mjesta] (./media/vpn-gateway-about-vpngateways/demomulti.png "više web-mjesta")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Uvođenje modela i postupci za web-mjesto i više web-mjesta

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet VNet

Povezivanje virtualne mreže s drugom virtualne mreže (VNet-VNet) slično je povezivanja s VNet na mjesto lokalnog web-mjesta. Pristupnik za VPN obje vrste povezivanja koristite sigurne tunelom pomoću IPsec-IKE. Možete čak i kombinirati VNet VNet komunikacije s konfiguracijama veze više web-mjesta. Omogućuje uspostavljanje topologija mreže kojima se kombiniraju više lokacija povezivanje s povezivanjem inter-virtualne mreže.

VNets povežete mogu biti:

- u istom ili drugom regijama
- u istom ili drugom pretplate 
- u istoj modelima različite implementacije


![VNet VNet vezu] (./media/vpn-gateway-about-vpngateways/demov2v.png "vnet vnet")

#### <a name="connections-between-deployment-models"></a>Veza između modelima implementacije

Azure trenutno sadrži dvije implementacije modela: klasični i resursima. Ako ste koristili Azure neko vrijeme, vjerojatno imate Azure VMs i uloge instancu izvodi u klasični VNet. Novija VMs i uloga instance možda se izvode u VNet stvorena u upravitelju resursa. Možete stvoriti veze između VNets da biste omogućili resursa u jedan VNet izravno komunicirati s resursa u drugu.

#### <a name="vnet-peering"></a>VNet peering

Možda ćete moći koristiti VNet peering da biste stvorili vezu, pod uvjetom da virtualne mreže zadovoljava određene uvjete. VNet peering pomoću pristupnika virtualne mreže. Dodatne informacije potražite u članku [VNet peering](../virtual-network/virtual-network-peering-overview.md).


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Uvođenje modela i postupci za VNet VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Točka-mjesta

Veza za pristupnik za VPN točke-na-web-mjesta (P2S) omogućuje vam da stvorite sigurno povezivanje s mrežom virtualne s pojedinačne klijentskog računala. P2S je VPN veza putem SSTP (Secure Socket protokol preusmjeravanja). P2S veze ne zahtijeva VPN uređaja ili dostupnog javnosti IP adresa za rad. Uspostavljanje VPN vezu tako da pokrenete s klijentskog računala. Ovo je rješenje je korisno kada želite povezati svoje VNet s udaljenog mjesta, kao što su od kuće ili konferencije, ili kada imate samo nekoliko klijenti koje morate se povezati s VNet. P2S veze može se koristiti u kombinaciji s vezama S2S putem isti pristupnika VPN, pod uvjetom da se sve konfiguracije preduvjeti za oba veze su kompatibilne.


Veza ![točke-na-web-mjesta] (./media/vpn-gateway-about-vpngateways/demop2s.png "točka-mjesta")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Modeli implementacije i postupci za točku web

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

U vezu s ExpressRoute virtualne mreže pristupnik konfiguriran s vrstom pristupnik "ExpressRoute" umjesto "Vpn". Dodatne informacije o ExpressRoute potražite u članku [ExpressRoute Tehnički pregled](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Web-mjesto i ExpressRoute coexisting veze

ExpressRoute je Izravni, namjenski veza na WAN (ne putem javnog Interneta) Microsoft Services, uključujući Azure. Web-mjesto VPN promet si olakšali putovanje šifrirane putem javnog Interneta. Moći Konfiguriranje VPN-a web-mjesto i ExpressRoute veza za istu virtualne mreže ima nekoliko prednosti.

Konfiguriranje web-mjesto VPN-a kao sigurnu prebacivanje put za ExpressRoute ili korištenje VPN-ovi web-mjesto za povezivanje s web-mjestima koja nisu dio vaše mreže, ali koji su povezani putem ExpressRoute. Obavijesti za tu radnju dva pristupnika virtualne mreže za istu virtualne mreže ga pomoću - GatewayType VPN-a, a drugo pomoću - GatewayType ExpressRoute.


![Coexist veze] (./media/vpn-gateway-about-vpngateways/demoer.png "expressroute site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Uvođenje modela i postupci za S2S i ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Daljnji koraci

Planiranje konfiguraciju pristupnika VPN-a. U odjeljku [VPN pristupnika planiranja i dizajna](vpn-gateway-plan-design.md) i [Povezivanje lokalne mreže za Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
