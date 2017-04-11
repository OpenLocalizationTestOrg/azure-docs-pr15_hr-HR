<properties 
   pageTitle="O ExpressRoute virtualne mreže pristupnika | Microsoft Azure"
   description="Saznajte više o pristupnika virtualne mreže za ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>O pristupnika virtualne mreže za ExpressRoute


Pristupnik virtualne mreže koristi se za slanje mrežni promet između Azure virtualne mreže i lokalne lokacije. Kada konfigurirate vezu s ExpressRoute, morate stvoriti i konfigurirati pristupnik virtualne mreže i virtualne mrežne veze pristupnika.

Kada stvorite pristupnik virtualne mreže, navedite nekoliko postavki. Jedan od potrebne postavke određuje hoće li se pristupnik koristiti za promet ExpressRoute ili VPN-a web-mjesto. U model za uvođenje resursima je postavka "-GatewayType".

Kada se na namjenski privatne veze šalje mrežnog prometa, koristite vrstu pristupnik 'ExpressRoute'. To se naziva pristupnika za ExpressRoute. Kad mrežni promet se šalje šifrirane s Interneta javno, koristite vrstu pristupnik "Vpn". To se naziva pristupnika VPN-a. Web-mjesta-na-web-mjesta, točke web i VNet VNet veze sve koristite pristupnik VPN-a. 

Svaki virtualne mreže može imati samo jedan pristupnik virtualne mreže po vrsti pristupnika. Ako, na primjer, moguće je mreža virtualne pristupnik koji koristi - GatewayType VPN-a i onaj koji koristi - GatewayType ExpressRoute. U ovom se članku usredotočuje se na ExpressRoute pristupnika virtualne mreže.

## <a name="gwsku"></a>SKU-ove pristupnika

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ako želite nadograditi pristupnik jače pristupnika SKU u većini slučajeva koristite cmdlet PowerShell 'Promjene veličine AzureRmVirtualNetworkGateway'. Funkcionirat će o nadogradnjama standardnu i HighPerformance SKU-ove. Međutim, da biste nadogradili UltraPerformance SKU, morat ćete ponovno stvoriti pristupnika.

###  <a name="aggthroughput"></a>Procijenjena zbrajanja propusnost pristupnik SKU


U sljedećoj su tablici prikazane vrste pristupnika i Procijenjena zbrajanja propusnost. U ovoj su tablici primjenjuje se na Voditelj resursa i uvođenje klasičnog modela.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>Cmdleti za REST API-ji i komponente PowerShell

Dodatne tehničke resurse i preduvjeti određene sintakse prilikom korištenja REST API-ji i cmdleta ljuske PowerShell za konfiguracije pristupnika virtualne mreže potražite u članku sljedećim stranicama:

|**Klasični** | **Voditelj resursa**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API-JA](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API-JA](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o konfiguracije moguću vezu potražite u članku [Pregled ExpressRoute](expressroute-introduction.md) . 







 
