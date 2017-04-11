<properties
   pageTitle="Povezivanje lokalne mreže Azure | Microsoft Azure"
   description="U članku se objašnjava i uspoređuje različite metode dostupne za povezivanje s Microsoftovim servisima u oblaku kao što su Azure, Office 365 i Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Povezivanje lokalne mreže Azure

Microsoft pruža nekoliko vrsta servise u oblaku. Dok se možete povezati sve servise s Interneta javno, možete povezati s nekim od servisa putem virtualne privatne mreže (VPN-a) tunelom putem Interneta ili putem izravne, privatne veze Microsoftu. U ovom se članku pomaže vam da odredite koju mogućnost povezivanje će najbolje odgovara vašim potrebama na temelju vrste Microsoftovim servisima u oblaku koji ste zauzeti. Većina tvrtki ili ustanova koristi funkciju opisanim više vrsta veze.

## <a name="connecting-over-the-public-internet"></a>Povezivanje putem javnog Interneta

Tu vrstu veze omogućuje pristup Microsoftovim servisima u oblaku izravno putem Interneta, kao što je prikazano u nastavku.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Ova je veza obično prva vrsta koja se koristi za povezivanje s Microsoftovim servisima u oblaku. U tablici u nastavku navedeni argumente za i protiv tu vrstu veze.



| **Prednosti**| **Razmatranja**|
|---------|---------|
|Zahtijeva bez promjena za lokalnu mrežu pod uvjetom da imate sve o klijentskim uređajima Neograničeni pristup svim IP adresa i priključaka na Internetu.|Iako promet često šifriran pomoću HTTPS, ga može biti intercepted na putu jer je putuje, a javnog Interneta.|
|Možete povezati s sve Microsoftovim servisima u oblaku javnog Interneta.|Nepredvidljive Latencija jer veza putuje, a na Internetu.|
|Koristi postojeću vezu s Internetom.||
|Upravljanje povezivanje uređaja nije potreban.||

Ovu vezu ne sadrži povezivanje ili propusnost troškove Budući da koristite postojeću vezu s Internetom. 

## <a name="connecting-with-a-point-to-site-connection"></a>Povezivanje s vezom za točku mjesta

Tu vrstu veze omogućuje pristup nekim Microsoftovim servisima u oblaku tunelom Secure Socket Tunneling Protocol (SSTP) putem Interneta, kao što je prikazano u nastavku.

![P2S] Veza (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "točke-na-web-mjesta")

Veza postane putem postojeće internetske veze, ali zahtijeva korištenje programa Azure VPN pristupnika. U tablici u nastavku navedeni argumente za i protiv tu vrstu veze.

| **Prednosti**| **Razmatranja**|
|---------|---------|
|Zahtijeva bez promjena za lokalnu mrežu pod uvjetom da imate sve o klijentskim uređajima Neograničeni pristup svim IP adresa i priključaka na Internetu.|Iako promet šifriran pomoću IPSec, ga može biti intercepted na putu jer je putuje, a javni Internet.|
|Koristi postojeću vezu s Internetom.|Nepredvidljive Latencija jer veza putuje, a na Internetu.|
|Propusnost do 200 Mb/s po pristupnika.|Zahtijeva stvaranje i upravljanje zasebnom veza između svakom uređaju u lokalnu mrežu i svaki pristupnika svaki uređaj treba povezati.|
|Možete se povezati sa servisom Azure services koje se mogu povezati s programa Azure virtualne mrežama (VNet) kao što su virtualnim računalima sustava Azure i Azure servise u Oblaku.|Potreban je minimalnog tijeku administraciji programa Azure VPN pristupnika.|
||Nije moguće koristiti za povezivanje sa sustavom Office 365 ili Dynamics CRM Online.
||Nije moguće koristiti za povezivanje s Azure servise koje ne mogu se povezati s VNet.|

Saznajte više o servisom [Pristupnika za VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) , [cijene](https://azure.microsoft.com/pricing/details/vpn-gateway)i izlazni podataka prijenos [cijene](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Povezivanje s vezom za web-mjesto

Tu vrstu veze omogućuje pristup nekim Microsoftovim servisima u oblaku putem programa tunelom IPSec putem Interneta, kao što je prikazano u nastavku.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Povezivanje web-mjesto")

Veza postane putem postojeće internetske veze, ali zahtijeva korištenje pristupnika VPN-a za Azure s pridružene cijene i cijene prijenos za izlazne podatke. U tablici u nastavku navedeni argumente za i protiv tu vrstu veze.

| **Prednosti**| **Razmatranja**|
|---------|---------|
|Svim uređajima na lokalnu mrežu možete komunicirati s Azure servise koji su povezani s VNet tako da nema potrebe da biste konfigurirali pojedinačne veza za svaki uređaj.|Iako promet šifriran pomoću IPSec, ga može biti intercepted na putu jer je putuje, a javni Internet.|
|Koristi postojeću vezu s Internetom.|Nepredvidljive Latencija jer veza putuje, a na Internetu.|
|Možete se povezati s Azure servise koji se mogu povezati s VNet kao što su virtualnim strojevima i servise u Oblaku.|Morate konfigurirati i upravljati njima u provjerene VPN uređaj * lokalnog.|
|Propusnost do 200 Mb/s po pristupnika.|Potreban je minimalnog tijeku administraciji programa Azure VPN pristupnika.|
|Možete pokrenuti odlazni promet pokrenuto iz oblaka virtualnim strojevima putem lokalne mreže za ispitivanje i zapisivanje pomoću korisnički definiranih usmjerava ili obrub pristupnika Protocol (BGP) **.|Nije moguće koristiti za povezivanje sa sustavom Office 365 ili Dynamics CRM Online.|
||Nije moguće koristiti za povezivanje s Azure servise koje ne mogu se povezati s VNet.|
||Ako koristite servise koje pokretanje veze natrag na lokalni uređaji i sigurnosna pravila potreban je, morat ćete vatrozid između lokalne mreže i Azure.|

- * Prikaz popisa [provjeriti VPN uređaja](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices).
- ** Dodatne informacije o korištenju [korisnički definirane usmjerava](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) ili [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) da biste nametnuli usmjeravanje iz Azure VNets lokalnog uređaju.

## <a name="connecting-with-a-dedicated-private-connection"></a>Povezivanje s namjenski privatne veze

Tu vrstu veze omogućuje pristup svim Microsoftovim servisima u oblaku putem namjenski privatne veze Microsoftu koji prolaziti s Internetom, kao što je prikazano u nastavku.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute veze")

Veza zahtijeva korištenje servisa ExpressRoute i veze za davatelja usluga za povezivanje. U tablici u nastavku navedeni argumente za i protiv tu vrstu veze.

| **Prednosti**| **Razmatranja**|
|---------|---------|
|Promet ne može biti intercepted na putu s Interneta javno jer se koristi namjenski vezu putem davatelja usluge.|Potreban je upravljanje lokalnim usmjerivač.|
|Propusnosti do 10 Gb/s po ExpressRoute elektronička i propusnost do 2 Gb/s da biste svaki pristupnika.|Zahtijeva namjenski vezu davatelju usluge povezivanja.|
|Predvidljivi Latencija jer je namjenski vezu Microsoft koji prolaziti s Internetom.|Možda ćete morati minimalnog tijeku Administracija jedan ili više pristupnika VPN Azure (Ako se povezujete s sklopovske VNets).|
|Ne zahtijeva šifriranu komunikacije, iako Šifrirajte promet, po želji.| Ako koristite servise u oblaku koji pokretanje veze natrag na lokalni uređaje, morat ćete vatrozid između lokalne mreže i Azure.|
|Možete izravno povezati sve Microsoftovim servisima u oblaku, s nekoliko iznimki *.|Lokalni IP adrese unos podataka centre za Microsoft za usluge koje se ne može povezati s VNet.* * zahtijeva prevođenja mrežnih adresa (NAT)|
|Možete pokrenuti odlazni promet pokrenuto iz oblaka virtualnim strojevima putem lokalne mreže za ispitivanje i zapisivanje pomoću BGP.|

- * Prikazati [popis servisa](../expressroute/expressroute-faqs.md#supported-services) koje nije moguće koristiti s ExpressRoute. Pretplate Azure mora odobriti za povezivanje sa sustavom Office 365.  Detalje potražite u članku [Azure ExpressRoute za Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) .
- ** Dodatne informacije o ExpressRoute [NAT](../expressroute/expressroute-nat.md) preduvjeti.

Dodatne informacije o [ExpressRoute](../expressroute/expressroute-introduction.md)te pridružene [cijene](https://azure.microsoft.com/pricing/details/expressroute)i [Povezivanje davatelja usluga](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Dodatne napomene

- Više mogućnosti iznad imaju različite Maksimalna ograničenja oni podržavaju za VNet veze, kao i veze pristupnika i drugim kriterijima. Preporučuje se da pregledate na Azure [ograničava mreže](../azure-subscription-service-limits.md#networking-limits) da biste shvatili ako ih utjecati na povezivanje vrste odlučite koristiti. 
- Ako namjeravate povežite pristupnik VPN veza web-mjesto s istom VNet kao pristupnik za ExpressRoute ste upoznati s važne ograničenja najprije. Potražite u članku [Konfiguriranje ExpressRoute i coexisting veze web-mjesto](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) za više detalja.

## <a name="next-steps"></a>Daljnji koraci

Ispod resursi objašnjavaju kako implementirati vrsta veze u ovom članku.

-   [Implementacija veze točke web](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Implementacija veze web-mjesto](guidance-hybrid-network-vpn.md)
-   [Implementacija namjenski privatne veze](guidance-hybrid-network-expressroute.md)
-   [Implementacija namjenski privatne veze s vezom za web-mjesto za visoke dostupnosti](guidance-hybrid-network-expressroute-vpn-failover.md)
