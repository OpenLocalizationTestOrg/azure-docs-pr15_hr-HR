<properties 
    pageTitle="Uvod u okruženje za aplikacije servisa" 
    description="Saznajte više o značajka okruženja aplikacije servisa koja omogućuje sigurne VNet pridružili, namjenski skaliranje jedinice za pokretanje sve aplikacije." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="stefsch"/>

# <a name="introduction-to-app-service-environment"></a>Uvod u okruženje za aplikacije servisa

## <a name="overview"></a>Pregled ##
Okruženju servisa aplikacija je [Premium] [ PremiumTier] plan mogućnost aplikacije servisa Azure kojoj se navode u potpunosti Izolirani i namjenski okruženje za sigurno pokretanje aplikacije servisa za Azure aplikacije na visok mjerilo, uključujući [Web-aplikacije]za servis[WebApps], [Mobilne aplikacije][MobileApps], i [Aplikacije API][APIApps].  

Aplikacije servisa za okruženja idealne su za radnih opterećenja aplikacije koje je obavezna:

- Vrlo visoka skala
- Odvajanja i sigurne mrežni pristup

Korisnici možete stvoriti više okruženja aplikacije servisa unutar jedno područje Azure, kao i preko više Azure regija.  Ovime okruženja aplikacije servisa idealna za vodoravno skaliranje razine Savezna manje aplikacije radi ispunjavanja visoke radnih opterećenja RPS.

Okruženja aplikacije servisa za su Izolirani izvodi samo jednog klijenta aplikacije, a uvijek implementiran u virtualne mreže.  Klijenti imaju preciznije kontrolirati oba mrežni promet ulazni i izlazni aplikacije, a aplikacija možete uspostaviti velika brzina sigurnu vezu putem virtualne mreže na lokalni tvrtke resurse.

Svih članaka i kako – da biste je o okruženja aplikacije servisa dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Da biste saznali kako okruženja aplikacije servisa omogućiti visoke skaliranje i sigurne mrežni pristup, pročitajte članak [Precizno Dive AzureCon] [ AzureConDeepDive] na okruženja aplikacije servisa!

Na dive obuhvaća više razina na vodoravno skaliranje pomoću više okruženja aplikacije servisa potražite u članku kako [ostavlja manji trag pri zemlj distribuirati aplikaciju]za postavljanje[GeodistributedAppFootprint].

Da biste vidjeli konfiguraciji sigurnosna arhitektura prikazano Dive precizno AzureCon, potražite u članku na implementacije u [slojevima sigurnosna arhitektura](app-service-app-service-environment-layered-security.md) s okruženjima aplikacije servisa.

Aplikacije koje se izvode na okruženja aplikacije servisa možete pristupiti svojim gated po upstream uređajima kao što su vatrozidima aplikaciju za web (WAF).  Članak o [konfiguriranju WAF za aplikaciju servisa okruženja](app-service-app-service-environment-web-application-firewall.md) pokriva scenarij. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Namjenski računalnim resursi ##
Svi resursi računalnim u okruženju aplikacije servisa su trakom isključivo za jednu pretplate, a okruženju servisa aplikacije mogu konfigurirati s do 50 (50) računalnim resursima za isključivo korištenje aplikacija za jedinstvenu.

Servis okruženju aplikacije sastoji se od sučelja računalnim skup resursa, kao i jedan do tri tempiranja računalnim resursa grupe. 

Pristupne skup sadrži računalnim resurse odgovoran za prekid SSL kao i automatsko opterećenja zahtjeva za aplikacije u okruženju aplikacije servisa. 

Svaki skup tempiranja sadrži resurse računalnim dodijeliti [Tarife za aplikaciju servisa][AppServicePlan], koji sadrže jedan ili više aplikacije servisa za Azure aplikacije.  Budući da u okruženju servisa aplikacija može biti do tri različite tempiranja grupe, imate fleksibilnost da biste odabrali drugi računalnim resursi za svaki skup tempiranja.  

Ako, na primjer, to vam omogućuje da biste stvorili jedan skup tempiranja s manje Napredna računalnim resursima za aplikaciju servisa tarife za razvoj ili probno aplikacije.  Second (ili čak i treće) tempiranja skup može koristiti jače računalnim resursi za aplikaciju servisa tarife pokretati aplikacije radnog.

Dodatne informacije o količinu računalnim resurse koji su dostupni za grupe prednje i tempiranja potražite [u]članku konfiguriranje okruženja aplikacije servisa[HowToConfigureanAppServiceEnvironment].  

Pojedinosti o dostupni resursa veličine podržanih u okruženju aplikacije servisa za izračun, obratite se [Cijene za aplikaciju servisa] [ AppServicePricing] stranicu i pregledajte dostupne mogućnosti za aplikaciju servisa okruženja u Premium cijene sloju.

## <a name="virtual-network-support"></a>Podrška za virtualne mreže ##
Moguće je stvoriti okruženju aplikacije servisa u **svakom** Voditelj resursa Azure virtualne mreže, **ili** klasični implementaciju modela virtualne mreže ([Dodatne informacije o mrežama virtualne][MoreInfoOnVirtualNetworks]).  Budući da okruženju aplikacije servisa uvijek postoji u virtualne mreže i preciznije unutar podmreži virtualne mreže, možete koristiti značajke zaštite virtualne mreže da biste odredili oba komunikacije ulazni i izlazni mreže.  

Servis okruženju aplikacije može biti bilo Internetom nasuprotne s javnu IP adresa ili interne nasuprotne s samo adrese Azure Interna učitavanja opterećenja (ILB).

Koristite [mrežni sigurnosne grupe] [ NetworkSecurityGroups] da biste ograničili ulaznog mrežnu komunikaciju u podmreži gdje se nalazi okruženju aplikacije servisa.  Omogućuje vam pokretanje aplikacija iza upstream uređajima i servisima kao što su vatrozidima web aplikacije i davatelja SaaS mreže.

Aplikacija često moraju imati pristup tvrtke kao što je baza podataka za interne i web-servisa.  Zajednički pristup je te krajnje točke dostupna samo za interne mrežni promet slijedi unutar Azure virtualne mreže.  Kada okruženju aplikacije servisa pridruženo isti virtualne mreže kao Interna servisi, aplikacije koji se izvodi u okruženju im mogli pristupati, uključujući krajnje točke dostupno putem [Web-mjesto] [ SiteToSite] i [Azure ExpressRoute] [ ExpressRoute] veze.

Za dodatne informacije o funkcioniranje aplikacije servisa okruženjima s virtualne mreže i lokalne mreže potražite u sljedećim člancima na [Mreži arhitektura][NetworkArchitectureOverview], [Kontrola dolazni promet][ControllingInboundTraffic], i [Sigurno povezivanje s pozadinskom sustavu][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Početak rada

Da biste započeli aplikacije servisa okruženja, potražite u članku [Kako da biste stvorili programa aplikacije servisa okruženje][HowToCreateAnAppServiceEnvironment]

Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure][AzureAppService].

Pregled mreže arhitektura okruženja aplikacije servisa, potražite u članku [Pregled arhitektura mrežni] [ NetworkArchitectureOverview] članka.

Informacije o korištenju okruženja aplikacije servisa s ExpressRoute, potražite u sljedećem članku [usmjeravanje Express]i okruženja aplikacije servisa[NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
