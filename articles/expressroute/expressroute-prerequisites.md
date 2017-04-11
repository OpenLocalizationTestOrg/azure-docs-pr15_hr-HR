<properties
   pageTitle="Preduvjeti za ExpressRoute prihvaćanja | Microsoft Azure"
   description="Ova stranica sadrži popis preduvjete za mora zadovoljavati redoslijed možete je elektronička Azure ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>Preduvjeti za ExpressRoute & kontrolni popis  

Da biste se povezali s Microsoftovim pomoću ExpressRoute servisima u oblaku, morat ćete da biste potvrdili da sljedeće preduvjete navedene u sljedećim odjeljcima zadovolje.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Račun za Azure

- Račun sustava Microsoft Azure valjan i aktivni. Potrebna je za postavljanje elektronička ExpressRoute. ExpressRoute krugova su resursi unutar Azure pretplate. Azure pretplate preduvjet je čak i ako povezivanje ograničeno je na servise u oblaku Azure proizvođača, kao što su servisima sustava Office 365 i CRM online.
- Aktivni pretplatu na Office 365 (Ako je pomoću servisa sustava Office 365). U odjeljku [određene preduvjeta za Office 365](#office-365-specific-requirements) ovog članka dodatne informacije.

## <a name="connectivity-provider"></a>Davatelj usluga za povezivanje
- Možete raditi s [partnera za povezivanje ExpressRoute](expressroute-locations.md#partners) za povezivanje s Microsoft cloud. Možete postaviti vezu između lokalne mreže i Microsoft na [tri načina](expressroute-introduction.md#howtoconnect). 
- Ako vaš davatelj usluga nije partnera za povezivanje programa ExpressRoute, i dalje možete povezati Microsoft cloud putem [oblaka davatelj usluga za exchange](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Preduvjeti za mrežu
- **Povezivanje suvišnih**: postoji bez zahtjeva zalihosti na fizičke veze između vi i vaš davatelj usluga. Microsoft zahtijevaju suvišnih BGP sesije postaviti između usmjerivača tvrtke Microsoft i peering usmjerivača, čak i kad imate samo [jedan fizičke veze sa sustavom exchange oblaka](expressroute-faqs.md#onep2plink). 
- **Usmjeravanje**: ovisno o tome kako se povezati s Microsoft Cloud, vi ili vaš davatelj usluga morate postaviti i upravljati BGP sesije za [usmjeravanje domene](expressroute-circuit-peerings.md). Nekih davatelja povezivanje Ethernet ili davatelj usluga za exchange oblaka može ponuditi upravljanja BGP kao na vrijednost Dodavanje servisa.
- **NAT**: Microsoft prihvaća samo javnu IP adrese kroz Microsoft peering. Ako koristite privatne IP adresa u lokalne mreže ili davatelja usluge vam upute za prevođenje privatne IP adresama na javnu IP adrese [pomoću na NAT](expressroute-nat.md).
- **QoS**: Skype za tvrtke sadrži različite servise (npr. govorne pozive i videozapis, tekst) koje je potrebno razlikuju QoS pokusa. Vi i vaš davatelj usluga morate slijediti [QoS preduvjeti](expressroute-qos.md).
- **Zaštita mreže**: razmislite o [sigurnosti mreže](../best-practices-network-security.md) prilikom povezivanja s Microsoft Cloud putem ExpressRoute.
 
## <a name="office-365"></a>Office 365

Ako namjeravate omogućiti sustava Office 365 na ExpressRoute, pregledajte sljedeće dokumente za dodatne informacije o preduvjetima za Office 365.


- [Pregled ExpressRoute za Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Usmjeravanje s ExpressRoute za Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365 URL-ovi i rasponi IP adresa](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Planiranje mreže i ugađanje performansi za Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Alatima i kalkulatorima propusnosti mreže](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Integracija sustava Office 365 s lokalnim okruženjima](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Ako namjeravate omogućiti CRM Online na ExpressRoute, pregledajte sljedeće dokumente dodatne informacije o CRM Online

- [CRM Online URL-ovi](https://support.microsoft.com/kb/2655102) i [rasponi IP adresa](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o ExpressRoute potražite u članku [Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md).
- Pronađite davatelja usluge razmjene ExpressRoute povezivanje. Potražite [partnera ExpressRoute i peering mjesta](expressroute-locations.md).
- Pogledajte preduvjete za [usmjeravanje](expressroute-routing.md), [NAT](expressroute-nat.md) i [QoS](expressroute-qos.md).
- Konfiguriranje veza s ExpressRoute.
    - [Stvaranje je elektronička ExpressRoute](expressroute-howto-circuit-classic.md)
    - [Konfiguriranje usmjeravanja](expressroute-howto-routing-classic.md)
    - [Povezivanje s VNet je elektronička ExpressRoute](expressroute-howto-linkvnet-classic.md)

