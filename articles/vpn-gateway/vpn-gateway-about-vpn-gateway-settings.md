<properties 
   pageTitle="O postavkama pristupnika VPN-a za virtualne mreže pristupnika | Microsoft Azure"
   description="Saznajte više o postavkama pristupnika VPN-a za Azure virtualne mreže."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>O postavkama pristupnika VPN-a

Na VPN pristupnika veze rješenje ovisi konfiguracije višestrukih resursa da biste poslali mrežni promet između virtualne mreže i lokalne lokacije. Svaki resurs sadrži konfigurirati postavke. Kombinacija resursa i postavke određuje rezultat veze.

U odjeljcima u ovom se članku navode resurse i postavke koje se odnose na VPN pristupnika u model implementacije **Voditelj resursa** . Koje može biti korisno da biste vidjeli dostupne konfiguracije pomoću veze topologije dijagrama. Za svakog rješenja veza u članku [O VPN pristupnika](vpn-gateway-about-vpngateways.md) možete pronaći opise i dijagrami topologije. 

## <a name="gwtype"></a>Vrste pristupnika

Svaki virtualne mreže može imati samo jedan pristupnik virtualne mreže svake vrste. Kada stvarate pristupnik virtualne mreže, provjerite nalazi li se vrsta pristupnika točne za konfiguraciju.

Dostupne vrijednosti za - GatewayType su: 

- VPN-a
- ExpressRoute

Potreban je VPN pristupnika na `-GatewayType` *VPN-a*.  

Primjer:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>SKU-ove pristupnika


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Konfiguraciju pristupnika SKU

**Određivanje pristupnika SKU na portalu za Azure**

Ako koristite Azure portal za stvaranje pristupnika za upravljanje resursima virtualne mreže, možete odabrati pristupnika SKU pomoću padajući popis. Mogućnosti predstavljanja s koje odgovaraju vrsti pristupnika i Vrsta VPN-a koji ste odabrali.

Ako, na primjer, ako odaberete vrstu pristupnik "VPN" i VPN vrsta 'pravila-temeljenu na', vidjet ćete 'osnovni SKU jer je samo za VPN-ovima PolicyBased SKU. Ako ste odabrali 'Usmjeravanje sustavom', možete odabrati Basic, standardna i HighPerformance SKU-ove. 


**Određivanje pristupnika SKU pomoću komponente PowerShell**


Sljedeći primjer PowerShell određuje na `-GatewaySku` kao *standardni*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Promjena pristupnika SKU**

Želite li nadograditi pristupnika SKU na jače SKU (Basic/Standard za HighPerformance) možete koristiti u `Resize-AzureRmVirtualNetworkGateway` cmdlet ljuske PowerShell. Koje možete prijeći i na nižu pristupnika veličina SKU-om za ovaj cmdlet.

Sljedeći primjer PowerShell prikazuje pristupnika SKU promijeniti veličinu da biste HighPerformance.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Procijenjena zbrajanja propusnost pristupnik SKU i vrsta

U sljedećoj su tablici prikazane vrste pristupnika i Procijenjena zbrajanja propusnost. U ovoj su tablici primjenjuje se na Voditelj resursa i uvođenje klasičnog modela.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Vrsta veze

U model za uvođenje resursima svaki konfiguracije zahtijeva vrstu veze za pristupnik za određene virtualne mreže. Dostupne PowerShell resursima vrijednosti za `-ConnectionType` su:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

U sljedećem primjeru PowerShell ćemo stvoriti S2S veze koje je potrebno vrstu veze *IPsec*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>Vrsta VPN-a

Kada stvorite pristupnika virtualne mreže za konfiguraciju pristupnika VPN-a, morate navesti vrstu VPN-a. Vrsta VPN-a koji odaberete ovisi o topologiji vezu koju želite stvoriti. Na primjer, vezu P2S zahtijeva RouteBased VPN vrsta. Vrsta VPN također mogu ovisiti o hardver koji ćete koristiti. Konfiguracija S2S potreban je VPN uređaja. Neke VPN uređaje podržavaju samo određene vrste VPN-a.

Vrsta VPN-a odaberete mora zadovoljavati sve veze uvjete rješenja želite stvoriti. Ako, na primjer, ako želite stvoriti S2S VPN veza pristupnika i P2S VPN veza pristupnik za istu virtualne mreže, koristit ćete VPN vrsta *RouteBased* jer P2S zahtijeva RouteBased VPN vrsta. I trebali biste provjerite je li uređaj VPN podržano je RouteBased VPN veza. 

Nakon što pristupnik virtualne mreže, ne možete promijeniti vrstu VPN-a. Imate da biste izbrisali pristupnik virtualne mreže i stvorite novi. Postoje dvije vrste VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Sljedeći primjer PowerShell određuje na `-VpnType` kao *RouteBased*. Kada stvarate pristupnika, provjerite nalazi li se-VpnType točne za konfiguraciju. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Preduvjeti za pristupnika

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Podmreže pristupnika

Da biste konfigurirali pristupnik virtualne mreže, najprije morate stvoriti podmreži pristupnik za vaše VNet. Pristupnik podmreži moraju biti imenovane *GatewaySubnet* ispravno funkcionirao. Ovaj naziv omogućuje Azure da se ovaj podmreže koristiti pristupnika.

Minimalna veličina vašoj podmreži pristupnika u cijelosti ovisi konfiguracije koji želite stvoriti. Iako je moguće da biste stvorili pristupnik podmreže što manje /29, preporučujemo stvorite podmreži pristupnika /28 ili veća (/ 28, /27, /26, itd.). 

Stvaranje pristupnika većom spriječiti pokretanje prema gore od ograničenja veličine pristupnika. Ako, na primjer, možda ste stvorili pristupnik virtualne mreže veličine pristupnika podmreži /29 S2S veze. Koje sada želite konfigurirati S2S/ExpressRoute supostojanje konfiguracije. Tu konfiguraciju potreban je pristupnika podmreže Minimalna veličina /28. Da biste stvorili konfiguraciju, promijenile izmjena podmreže pristupnika kako bi odgovarao minimalni preduvjet veze, što je /28.

Sljedeći primjer Voditelj resursa PowerShell prikazuje podmreži pristupnika pod nazivom GatewaySubnet. Vidjet ćete notaciju CIDR određuje /27, koja omogućuje dovoljno IP adresa za većinu konfiguracije koji trenutno postoje.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Lokalne mreže pristupnika

Prilikom stvaranja konfiguraciju pristupnika VPN pristupnika lokalne mreže često predstavlja lokalne lokacije. U modelu uvođenje klasičnog pristupnik za lokalnu mrežu je se nazivaju lokalnog web-mjesta. 

Naziv pristupnika lokalne mreže, javnu IP adresu VPN uređaja lokalnog i Navedite adresu prefiksi koje se nalaze na lokalne lokacije. Azure razmatra prefiksa odredišnu adresu za mrežni promet, consults konfiguraciju koje ste naveli za pristupnik za lokalnu mrežu i sukladno tome usmjerava pakete. Možete odrediti i lokalne mreže pristupnika za VNet VNet konfiguracije koji koriste VPN veza pristupnika.

Sljedeći primjer PowerShell stvara novi pristupnik za lokalnu mrežu:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Ponekad ćete morati izmijeniti pristupnika postavke lokalne mreže. Ako, na primjer, kada Dodavanje ili izmjena raspon adresu ili ako se mijenja IP adresa uređaja za VPN-a. Za klasični VNet, možete promijeniti postavke na portalu klasični na stranici lokalne mreže. Za Voditelj resursa potražite u članku [Postavke pristupnika Izmijeni lokalne mreže pomoću komponente PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>Cmdleti za REST API-ji i komponente PowerShell

Dodatne tehničke resurse i preduvjeti određene sintakse prilikom korištenja REST API-ji i cmdleta ljuske PowerShell za konfiguracije pristupnika za VPN-a potražite u članku sljedećim stranicama:

|**Klasični** | **Voditelj resursa**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API-JA](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API-JA](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o konfiguracije moguću vezu potražite u članku [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md) . 







 
