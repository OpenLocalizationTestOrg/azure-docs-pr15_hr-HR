<properties
   pageTitle="Voditelj resursa podržava usluga | Microsoft Azure"
   description="U članku se opisuje davatelji resursa koji podržavaju Voditelj resursa, osobe sheme i verzija API-JA i regije koje možete hostirati resurse."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Voditelj resursa davatelji, područja, verzije API-JA i sheme

Azure Voditelj resursa daje novi način implementacije i upravljanje uslugama koji čine aplikacija. Većina, ali ne svim, services podržava Voditelj resursa, a neke usluge podrške resursima samo djelomično. Ova tema sadrži popis davatelja podržani resursa za Azure Voditelj resursa.

Kada implementirate resursa, također morate znati koji regije podržavaju tih resursa i koje verzije API su dostupne za resurse. U odjeljku [podržani područja](#supported-regions) objašnjava da biste saznali koji posla područja za svoju pretplatu i resursa. U odjeljku [podržani API verzije](#supported-api-versions) pokazuje kako odrediti koje verzije API-JA možete koristiti.

Usluga koje su podržane u Azure portal i klasični portal potražite [Azure portala dostupnost grafikona](https://azure.microsoft.com/features/azure-portal/availability/). Premještanje resursi podrške koje se servise potražite [Premještanje resursa ili pretplatu za novu grupu resursa](resource-group-move-resources.md).

U sljedećim tablicama navedeni koji Microsoft usluge podrške implementacije i upravljanja putem upravitelja resursa i koji ne. Vezu u stupcu **Predložaka za brzi početak rada** šalje upit u spremište Azure predložaka za brzi početak rada za davatelja usluga za određeni resurs. Predlošci za brzi početak rada dodaju i ažuriraju često. Podaci o prisutnosti veze servisa za određeni ne znači nužno upit vraća predložaka u spremištu. Postoje i brojne davatelje usluga drugih proizvođača resursa koji podržavaju Voditelj resursa. Saznajte kako da biste vidjeli sve davatelji resursa u odjeljku [davatelji resursa i vrste](#resource-providers-and-types) .


## <a name="compute"></a>Izračun

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------------------------ |-------- | ------ | ------ |
| Grupe   | Da     | [Obradu ostalih](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015 01 – 12](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Spremnik | Da | [Spremnik servisa REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016 03 30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Usluge Dynamics životni ciklus | Da  |      |        |  |
| Promjena veličine skupova | Da | [Promjena veličine postavite OSTALE](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Tkanina servisa | Da  | [Ostale tkanina servisa](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtualnim strojevima | Da | [OSTALE VM](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtualnim strojevima (classic) | Ograničeno | - | - | - |
| Aplikacija Remote | ne      | -  | - | - |
| Servisi u oblaku (classic) | Ograničeno (pogledajte informacije u nastavku) | - | - | - |

Virtualnim strojevima (klasični) se odnosi na resurse koji su implementiran putem model klasični implementacije umjesto putem model implementacije Voditelj resursa. Općenito govoreći, te resurse ne podržavaju operacije Voditelj resursa, ali su neki postupci kojima je omogućeno. Dodatne informacije o ove implementacije modelima potražite u članku [Implementacija upravljanja resursima za razumijevanje i uvođenje klasičnog](resource-manager-deployment-model.md). 

Servisi u oblaku (klasični) mogu se koristiti s drugim klasični resursima; Međutim, klasični resursi iskoristite prednost sve značajke upravljanja resursima i nisu dobar izbor za buduće rješenja. Umjesto toga, razmislite o promjeni infrastruktura za aplikaciju za korištenje resursa iz prostori Microsoft.Compute, Microsoft.Storage i Microsoft.Network.


## <a name="networking"></a>Povezivanje s mrežom

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | -------  | -------- | ------ | ------ |
| Pristupnik za aplikaciju | Da | [Pristupnik za aplikaciju REST](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS-A     | Da | [OSTALE DNS-A](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Da  | [ExpressRoute OSTALE](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Opterećenja | Da  | [Opterećenja REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Upravitelj promet | Da | [OSTALE Upravitelj promet](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015 11 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtualne mreže | Da| [Virtualne mreže REST](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| Pristupnik za VPN-a | Da | [Mrežni pristupnik REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[veze](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Prostor za pohranu

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Prostor za pohranu | Da  | [Prostor za pohranu REST](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Račun za pohranu](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Da  |         |        |  |

## <a name="databases"></a>Baza podataka

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Da  | [DocumentDB OSTALE](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis predmemorije | Da |   | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL baze podataka | Da | [OSTALE baze podataka SQL](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014.-04-01 – pregled](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL Data Warehouse | Da |   |      |


## <a name="web--mobile"></a>Web- & Mobile

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------ |
| API aplikacije | Da |   | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API aplikacije](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| Upravljanje API-JA | Da  | [Upravljanje API REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Sadržaja moderatora | Da |   |   |   |
| Funkcija aplikacije | Da |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Logika aplikacije | Da   | [Tijek rada servisa REST API-JA](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobilne aplikacije | Da |  | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobilni obaveze | Da  |  [Mobilni radnje REST](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Pretraživanje | Da  | [Pretraživanje REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Web-aplikacije | Da |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Analytics

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | -------  | -------- | ------ | ------ |
| Katalog podataka | Da  | [Katalog podataka REST](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016 03 30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Tvorničke podataka | Da | [Podaci tvorničke REST](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Analitički Lake podataka | Da |   |   [2015-10-01 – pregled](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Spremište Lake podataka | Da  | [OSTALE spremišta Lake podataka](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01 – pregled](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Da | [HDInsights OSTALE](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Upoznavanje s računala | Da | [Strojnog učenja REST](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016-05-01 – pregled](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Strujanje Analytics | Da  | [Analitički pare REST](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Da | [Power BI ugrađene REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016 01 29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Obavještavanje

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------ |
| Kognitivni usluge | Da |  | [2016-02-01 – pregled](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internetske mogućnosti

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------ |
| Koncentrator za događaj | Da  | [OSTALE koncentratora za događaj](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Da | [Koncentrator IoT OSTALE](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016 02 03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Obavijest o koncentratora | Da | [Obavijest o koncentrator REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Mediji i CDN

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------ |
| CDN | Da | [OSTALE CDN](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016 04 02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Servis za medijske sadržaje | Da | [Media Services REST](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015 10 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Integracija hibridnog

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk Services | Da |          | [2014.-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Vraćanje servisa | Da | [Oporavak REST web-mjesta](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Servis Bus | Da | [Servis Bus REST](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identitet i upravljanje pristupom 

Azure Active Directory funkcionira s Voditelj resursa da biste omogućili kontrolu pristupa na temelju uloga za vašu pretplatu. Informacije o korištenju kontrola pristupa na temelju uloga i servisa Active Directory potražite u članku [Utemeljeno na ulogama Azure kontrola pristupa](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Servisi za razvojne inženjere 

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------ |
| Aplikacija uvida | Da  | [OSTALE uvida](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014.-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing karte | Da  |          |        |  |
| DevTest Labs | Da |   | [2016 05 15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Račun za Visual Studio | Da   |          | [2014. 02 26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Upravljanje i sigurnost

| Servis | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------- | ------ | ------ |
| Automatizacija | Da | [Automatizacija REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10 – 31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Ključni zbirke ključeva | Da | [OSTALE sigurnog ključa.](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Ključni zbirke ključeva](resource-manager-template-keyvault.md)<br />[Tajna ključa zbirke ključeva](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Radu uvida | Da |          |        |  |
| Raspored | Da  | [Raspored REST](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016 03 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Sigurnost (pretpregled) | Da | [OSTALE sigurnosne](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Voditelj resursa

| Značajka | Voditelj resursa omogućeno | REST API-JA | Shema | Predlošci za brzi početak rada |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Autorizacija | Da | [Upravljanje zaključavanja](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Kontrola pristupa na temelju uloga](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Zaključavanje resursa](resource-manager-template-lock.md)<br />[Dodjele uloga](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Resursi | Da | [Povezani resursi](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Veza resursa](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Vrste i davatelji resursa

Kada implementirate resursa, često potrebno dohvatiti podatke o davatelji resursa i vrste. Može dohvatiti te informacije kroz REST API-JA, Azure PowerShell ili Azure EŽA.

Da biste radili s davatelja resursa, taj davatelj resursa mora biti registriran u svoj račun. Prema zadanim postavkama, brojne davatelje usluga resursa su automatski registered; Međutim, možda ćete morati ručno registrirali Neki davatelji resursa. Primjere u nastavku pokazuju kako se porezni status davatelja resursa i registrirati davatelja resursa po potrebi.

### <a name="portal"></a>Portal

Jednostavno možete vidjeti popis podržanih resursi davatelja sa sljedećim koracima:

1. Odaberite **Moje dozvole**.

    ![Moje dozvole](./media/resource-manager-supported-services/my-permissions.png)

2. Odaberite **Stanje davatelja resursa**

    ![status davatelja resursa](./media/resource-manager-supported-services/resource-provider-status.png)

3. Pogledajte cjelovit popis resursa davatelja usluga za vašu pretplatu.

    ![Davatelji popis resursa](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST API-JA

Da biste sve dostupne resursa proizvođača, uključujući njihove vrste, mjesta, verzije API-JA i porezni status, koristite postupak [popis svih proizvođača resursa](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Ako morate registrirati davatelja resursa, potražite u članku [registrirati pretplatu pomoću davatelja resursa](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>PowerShell

Sljedeći primjer pokazuje kako sve davatelje usluga dostupnih resursa.

    Get-AzureRmResourceProvider -ListAvailable
    

U sljedećem primjeru pokazuje kako vrste resursa za davatelja usluga za određeni resurs.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
Izlaz slično je:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Da biste registrirali davatelja resursa, navedite naziva:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure EŽA

Sljedeći primjer pokazuje kako sve davatelje usluga dostupnih resursa.

    azure provider list
    
Izlaz slično je:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Informacije za davatelja usluga za određeni resurs možete spremiti u datoteku pomoću sljedeće naredbe.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Da biste registrirali davatelja resursa, navedite naziva:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Podržanih regija

Kada implementirate resursa, obično ćete morati navedite regiju za resurse. Voditelj resursa je podržano u svim regijama, no resursi implementacije možda neće biti podržane u svim regijama. Osim toga, možda postoje ograničenja vezana uz pretplatu zbog kojih ste pomoću neke regije koje podržavaju resurs. Ta ograničenja može biti vezano uz porez problemi za vašu državu kućni ili rezultat pravila koja je postavila pretplatu administrator da biste koristili samo određenih područja. 

Cjelovit popis svih podržanih regija za sve servise Azure potražite u članku [Servisi po regijama](https://azure.microsoft.com/regions/#services); Međutim, ovaj popis može obuhvaćati i regije koje ne podržava pretplate. Možete odrediti područja za vrstu određeni resurs koji podržava vaša pretplata putem portal, REST API-JA, PowerShell ili Azure EŽA.

### <a name="portal"></a>Portal

Možete vidjeti podržanih regija se radi o vrsti resursa kroz sljedeće korake:

1. Odaberite **Dodatne usluge** > **Explorer resursa**.

    ![explorer resursa](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Otvorite čvora **davatelji usluga** .

    ![Odaberite davatelja usluga](./media/resource-manager-supported-services/select-providers.png)

3. Odaberite davatelja resursa i prikaz podržanih regija i verzije API-JA.

    ![Davatelj usluga za prikaz](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST API-JA

Da biste otkrili koje područja su dostupne za vrstu određeni resurs za pretplatu, koristite postupak [popis svih proizvođača resursa](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>PowerShell

Sljedeći primjer pokazuje kako se nabavljaju podržanih regija za web-mjesta.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
Izlaz slično je:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure EŽA

Sljedeći primjer vraća cjelokupan podržani mjesta za svaku vrstu resursa.

    azure location list

Možete filtrirati i mjesto rezultata pomoću programa za JSON kao što je [jq](https://stedolan.github.io/jq/).

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Koja vraća:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Podržane verzije API-JA

Kada uvođenje predloška, morate navesti API verzija namijenjen stvaranju svakog resursa. API verzija odgovara verziji REST API-JA operacije koje su objavio davatelja resursa. Kao što je davatelj usluga resursa omogućuje nove značajke, izda nova verzija REST API-JA. Stoga verziju API koji ste naveli u predlošku utječe na svojstva koja možete odrediti u predlošku. Općenito govoreći, želite odabrati najnoviju verziju API pri stvaranju predložaka. Za postojeće predloške možete odlučiti hoće li želite nastaviti koristiti starije verzije API-JA ili ažurirali predložak za najnoviju verziju da iskoristite prednost novih značajki.

### <a name="portal"></a>Portal

Odredite podržanim verzijama API-JA na isti način kao i odrediti podržane regije (prethodno prikazanom).

### <a name="rest-api"></a>REST API-JA

Da biste otkrili koje verzije API su dostupne za vrste resursa, koristite postupak [popis svih proizvođača resursa](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>PowerShell

Sljedeći primjer pokazuje kako se nabavljaju dostupnih verzija API-JA za vrstu određeni resurs.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
Izlaz slično je:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure EŽA

Za davatelja resursa u datoteku pomoću sljedeće naredbe možete spremiti podatke (uključujući dostupnih verzija API-JA).

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Otvorite datoteku i traženje **apiVersions** element

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o stvaranju predložaka Voditelj resursa, potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).
- Da biste saznali više o implementaciji resursa, potražite u članku [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).
