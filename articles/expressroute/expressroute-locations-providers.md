<properties
   pageTitle="ExpressRoute mjesta | Microsoft Azure"
   description="Ovaj članak sadrži detaljne pregled od mjesta gdje se nude servise i kako se povezati sa Azure područja."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute partnera i peering mjesta

Tablice u ovom članku pronaći informacije o ExpressRoute povezivanje proizvođača, ExpressRoute zemljopisnim opseg, Microsoft cloud services podržano putem ExpressRoute i ExpressRoute sustava Integrators (SIs).

## <a name="partners"></a>Davatelji povezivanje ExpressRoute

ExpressRoute podržano je preko svih Azure područja i mjesta. Sljedeće karta daje popis te regije Azure ExpressRoute mjesta. ExpressRoute mjesta odnose se na one kojima Microsoft kolege s nekoliko davateljima usluga.

![Mjesto mape][0]

Ako ste povezani s najmanje jedno mjesto ExpressRoute unutar Geopolitički područja će imati pristup servisa Azure preko sva područja unutar Geopolitički područja. Sljedeća tablica sadrži kartu područja Azure ExpressRoute mjestima unutar Geopolitički područja.

|**Geopolitički regija**|**Azure regije**|**ExpressRoute mjesta**|
|---|---|---|
|**Sjeverna Amerika**|Istočni SAD-a, Zapad SAD-a, Istočni sad 2, središnje SAD-a, Južna središnje SAD-a, Sjeverna središnje SAD-a, Kanada središnje Kanadi Istok|Atlanta, Chicago, grada Dallasa, Las Vegas, Los Angeles, New York, Seattle, Silicon brijeg, Washington Kontroler, Montreal +, Grad Quebecu +, Toronto|
|**Južna Amerika**|Južna Brazil|Paulo Sveti|
|**Europa**|Sjeverna Europa, Zapad Europa, velika Britanija Zapad, Južna velika Britanija|Amsterdam, Dublin, London, Newport(Wales) +, Pariz|
|**Azija**|Istočnoazijski, Jugoistočne Azije|Hong Kong, Singapur|
|**Japan**|Japanska Zapad, Istok Japan|Osaka, Tokio|
|**Australija**|Australija Jugoistok, Istok Australija|Melbourne, Sydney|
|**Indija**|Indija Zapad, Južna središnje, Indija Indija|Chennai, Bombaj|



U tablici u nastavku navedene su informacije na područjima i Geopolitički ograničenja za nacionalna oblaka.

|**Geopolitički regija**|**Azure regije**|**ExpressRoute mjesta**|
|---|---|---|---|
|**Oblak vlade SAD-a**|SAD Gov Iowa, SAD Gov Virginia|Chicago, grada Dallasa, New York, Washington Kontroler|
|**Kina**|Kina Sjeverna, Kini Istok|Peking, Shanghai|
|**Njemačka**|Njemačka središnje, Njemačka Istok|Berlinski, Frankfurt|


Povezivanje preko Geopolitički regija nije podržana na standardni ExpressRoute SKU-om. Morat ćete omogućiti dodatak premium ExpressRoute za podršku globalni povezivanje. Veza s nacionalna oblaka okruženja nije podržana. Kod davatelja usluge povezivanja možete raditi ako kao što je potreba.


## <a name="connectivity-provider-locations"></a>Povezivanje davatelja mjesta

> [AZURE.SELECTOR]
[Mjesta davatelj](expressroute-locations.md#connectivity-provider-locations)
[davatelji po lokaciji](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Proizvodne Azure
| **Mjesto**  | **Davatelji usluga** |
|---------------|-----------------------|
| **Amsterdam** | Mreža Aryaka AT & T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, a zatim internetske rješenja - povezati oblaka, Interxion, Verizon komunikacije, narančastu, Tata komunikacije, TeleCity grupa, Telenor, razinu 3 |
| **Atlanta** | Equinix |
| **Chennai** | Tata komunikacije |
| **Chicago** | AT & T NetBond, Comcast, Equinix, razinu 3 komunikacije, Zayo grupa |
| **Grada Dallasa** | AT & T NetBond, Cologix, Equinix, razina 3 komunikacije, Megaport |
| **Dublin** | Colt, Telecity grupa |
| **Hong Kong** | Telecom British Telecom, Kini globalni, Equinix, Megaport, narančastu, PCCW globalni ograničen, Tata komunikacije, Verizon |
| **London** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, rješenja Internet – povezivanje oblaka, Interxion, Jisc, razinu 3 komunikacije, MTN, NTT komunikacije, narančastu, Tata komunikacije, Telecity grupa, Telenor, Verizon, Vodafone |
| **Las Vegas** | Razina 3 komunikacije +, Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo grupa |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo grupa |
| **Newport(Wales) +** | Sljedeći generacije podataka + |
| **Montreal** | Cologix + |
| **Bombay** | Tata komunikacije |
| **Osaka** | Equinix, Internet inicijativa Japan Inc. - IIJ, NTT komunikacije, Softbank |
| **Pariz** | Interxion, Equinix + |
| **Paulo Sveti** | Equinix, Telefonica |
| **Iz Seattlea** | Megaport Equinix, komunikacije razinu 3 |
| **Silicon udubljenje** | Mreža Aryaka AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, razinu 3 komunikacije, narančastu, Tata komunikacije, Verizon, Zayo grupa |
| **Singapur** | Mreža Aryaka AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, narančastu, SingTel, Tata komunikacije, Verizon |
| **Sydney** | AARNet, AT & T NetBond, britansku Telecom, Equinix, Megaport, NEXTDC, narančastu, Telstra Corporation, Verizon |
| **Tokio** | Aryaka mrežama, britansku Telecom, Colt, Equinix, Internet inicijativa Japan Inc. - IIJ, NTT komunikacije, Softbank, Verizon |
| **Toronto** | Cologix, Equinix, Zayo grupa |
| **Kontroler Washington** | Mreža Aryaka AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, razinu 3 komunikacije, Megaport, narančastu, Tata komunikacije, Verizon, Zayo grupa |

 **+**označava uskoro dostupno

### <a name="national-cloud-environments"></a>Nacionalni oblaka okruženja

#### <a name="us-government-cloud"></a>Oblak vlade SAD-a

| **Mjesto**  |**Davatelji usluga** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond, Equinix, razina 3 komunikacije, Verizon |
| **Grada Dallasa** |  Equinix, Verizon + |
| **New York** | Verizon Equinix, razinu 3 komunikacije +, |
| **Kontroler Washington** | AT & T NetBond, Equinix, razina 3 komunikacije, Verizon |

#### <a name="china"></a>Kina

| **Mjesto**  | **Davatelji usluga** |
|---------------|-----------------------|
| **Peking** | Kina Telecom |
| **Shanghai** |  Kina Telecom |
Dodatne informacije potražite u članku [ExpressRoute u Kini](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Njemačka

| **Mjesto**  | **Davatelji usluga** |
|---------------|-----------------------|
| **Berlinski** | Colt +, e-shelter |
| **Frankfurt** | Colt, Equinix, Interxion |

## <a name="nonpartners"></a>Povezivanje putem usluga nije naveden na popisu

Ako vaš davatelj usluga za povezivanje nije naveden u prethodnom odjeljku, i dalje možete stvoriti vezu.

- Obratite se davatelju povezivanje jesu li u bilo koji od razmjena iz gornje tablice koji su povezani. Možete provjeriti sljedeće veze da biste prikupili dodatne informacije o servise koje nudi davatelje usluga sustava exchange. Nekoliko povezivanje davatelji već niste povezani s Ethernet razmjena.

    - [Oblak Equinix sustava Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Imati davatelja connectivity Proširi mrežom peering mjesto po izboru.
    - Provjerite je li da vaš davatelj usluga za povezivanje proširuje stanje veze iznimno dostupna način tako da nema jedan točaka kvara.
- Redoslijed je elektronička ExpressRoute s exchange kao davatelja povezivanje da biste se povezali Microsoftu.
    - Slijedite korake u odjeljku [Stvaranje je elektronička ExpressRoute](expressroute-howto-circuit-classic.md) za postavljanje povezivanja.

|**Mjesto**|**Exchange**|**Povezivanje davatelja usluga**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Iz Seattlea** | Equinix | Aljasku komunikacije |
| **Silicon udubljenje** | Equinix | XO komunikacije |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Kontroler Washington** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute sustava integrators

Omogućivanje privatne povezani s vašim potrebama može biti zahtjevne, koji se temelji na skali mreže. Možete raditi s bilo kojom integrators sustava navedene u tablici u nastavku radi jednostavnijeg za uhodavanje u ExpressRoute.

|**Kontinent**|**Integrators sustava**|
|-------------|---------------------|
| **Azija** | OneAs1a Avanade Inc.|
| **Europa** | Rješenja Dotnet Avanade Inc.|
| **SAD** | Avanade Inc., Equinix profesionalne usluge, Perficient, vodstvo projekta|

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o ExpressRoute potražite u članku [Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md).
- Provjerite je li sve preduvjete zadovoljeni. Pogledajte [preduvjete ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mjesto mape"
