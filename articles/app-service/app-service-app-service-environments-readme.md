<properties 
    pageTitle="Okruženje za aplikacije servisa za | Microsoft Azure" 
    description="Što je okruženja aplikacije servisa Azure? Uvod u okruženje za aplikaciju servisa." 
    keywords="Azure aplikacije servisa okruženju, virtualne mreže sigurne mreže"
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

# <a name="app-service-environment-documentation"></a>Dokumentacija okruženja aplikacije servisa

Aplikacije servisa okruženje je [Premium] [ PremiumTier] plan mogućnost aplikacije servisa Azure kojoj se navode u potpunosti Izolirani i namjenski okruženje za sigurno pokretanje aplikacije servisa za Azure aplikacije na visok razini, uključujući [Web-aplikacije]za servis[WebApps], [Mobilne aplikacije][MobileApps], i [Aplikacije API][APIApps].  

Aplikacije servisa okruženja idealne su za radnih opterećenja aplikacije koje je obavezna:

- Vrlo visoka skala
- Odvajanja i sigurne mrežni pristup

Korisnici možete stvoriti više okruženja aplikacije servisa unutar jedno područje Azure, kao i preko više Azure regija.  Ovime okruženja aplikacije servisa idealna za vodoravno skaliranje razine Savezna manje aplikacije radi ispunjavanja visoke radnih opterećenja RPS.

Okruženja aplikacije servisa za su Izolirani izvodi samo jednog klijenta aplikacije, a uvijek uveden u virtualne mreže.  Klijenti imaju preciznije kontrolirati oba mrežni promet ulazni i izlazni aplikacije pomoću [mreže sigurnosne grupe][NetworkSecurityGroups].  Aplikacije možete uspostaviti velika brzina sigurnu vezu putem virtualne mreže na lokalni tvrtke resurse.

Aplikacija često moraju imati pristup tvrtke kao što je baza podataka za interne i web-servisa.  Aplikacijama koje su pokrenute na okruženja aplikacije servisa može pristupiti resursima dostupno putem [Web-mjesto] [ SiteToSite] VPN-a i [Azure ExpressRoute] [ ExpressRoute] veze.

* [Što je servis okruženju aplikacije?](../app-service-web/app-service-app-service-environment-intro.md)
* [Stvaranje aplikacije servisa za okruženja](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Stvaranje aplikacije u okruženju aplikacije servisa](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Stvaranje i korištenje Interna opterećenja pomoću aplikacije servisa za okruženja](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Konfiguriranje okruženja aplikacije servisa](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Promjena veličine aplikacije u okruženju aplikacije servisa](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Zaštita mreže i arhitekturu](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Upute za korisnika

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Videozapisi
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
