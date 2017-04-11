<properties
   pageTitle="Usmjeravanje preduvjeti za ExpressRoute | Microsoft Azure"
   description="Ova stranica sadrži preduvjetima za konfiguriranje i upravljanje usmjeravanja za ExpressRoute krugova."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>Preduvjeti za usmjeravanje ExpressRoute  

Da biste se povezali s Microsoftovim pomoću ExpressRoute servisima u oblaku, morat ćete postaviti i upravljati usmjeravanja. Neki davatelji usluga povezivanje nude postavljanje i upravljanje usmjeravanje kao servis za upravljane. Obratite se davatelju povezivanje da biste vidjeli ako oni nudi taj servis. Ako oni ne moraju biti sljedeće preduvjete. 

Pogledajte članak [krugova i usmjeravanje domene](expressroute-circuit-peerings.md) opis usmjeravanje sesije koje je potrebno postaviti da biste olakšali povezivanje.

>[AZURE.NOTE] Microsoft ne podržava sve protokoli zalihosti usmjerivač (npr., HSRP, VRRP) za visoke dostupnosti konfiguracije. Ne možemo oslanjate suvišne par sesija BGP po peering za visoke dostupnosti.

## <a name="ip-addresses-used-for-peerings"></a>IP adresa koji se koriste za peerings

Morate rezervirati nekoliko blokova IP adrese da biste konfigurirali usmjeravanje između mreže i tvrtke Microsoft Enterprise rub (MSEEs) usmjerivača. U ovom se odjeljku nalaze se popis preduvjeti i opisuje pravila o kako te IP adresa mora biti dobivene i koristiti.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP adresa koji se koriste za Azure privatne peering

Da biste konfigurirali na peerings možete koristiti privatne IP adresa ili javnu IP adrese. Adresni raspon koji se koristi za konfiguriranje usmjerava se mora preklapati s adresa raspona koji se koriste za stvaranje virtualne mreže u Azure. 

 - Morate rezervirati na /29 podmreži ili dva /30 podmreže za usmjeravanje sučelja.
 - Podmreže koristi za usmjeravanje može biti privatne IP adresa ili javnu IP adrese.
 - Na podmreže mora nisu u sukobu s rasponom rezervirane koje je korisnik odabrao za korištenje u programu Microsoft Excel.
 - Ako je /29 koristi podmreže, ona će se podijeliti na dva /30 podmreže. 
     - Prvi /30 podmreže će se koristiti za primarni vezu, a drugi/30 podmreže će se koristiti za sekundarne veze.
     - Za svaki unos u /30 podmreže, morate koristiti prvi IP adresu na /30 podmreže na usmjerivač. Microsoft će koristiti drugi IP adresu na /30 podmreže da biste postavili BGP sesiju.
     - Morate postaviti i BGP sesije za naše [dostupnost SLA](https://azure.microsoft.com/support/legal/sla/) moraju biti valjane.  

#### <a name="example-for-private-peering"></a>Primjer privatne peering

Ako odlučite koristiti a.b.c.d/29 da biste postavili na peering će se podijeliti u dvije /30 podmreže. U primjeru u nastavku ćemo izgledat će na kako se koristi podmreže a.b.c.d/29. 

a.b.c.d/29 bit će Podjela a.b.c.d/30 i a.b.c.d+4/30 i Microsoft kroz dodjele resursa API-ji proslijeđena prema dolje. A.b.c.d+1 će koristiti kao VRF IP za primarni PE i Microsoft zauzeti će a.b.c.d+2 kao IP VRF za primarni MSEE. A.b.c.d+5 će koristiti kao VRF IP za sekundarne PE i Microsoft koristit će a.b.c.d+6 kao VRF IP sekundarne MSEE.

Razmislite o slučaj kojem odabrati 192.168.100.128/29 da biste postavili privatne peering. 192.168.100.128/29 sadrži adrese iz 192.168.100.128 192.168.100.135 među koji:

- 192.168.100.128/30 dodjeljuju link1, kod davatelja usluge korištenja 192.168.100.129 i Microsoft pomoću 192.168.100.130.
- 192.168.100.132/30 dodjeljuju link2, kod davatelja usluge korištenja 192.168.100.133 i Microsoft pomoću 192.168.100.134.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP adresa koji se koriste za Azure javno i Microsoft peering

Javnu IP adrese da ste vlasnik morate koristiti za postavljanje BGP sesije. Microsoft moraju imati mogućnost potvrditi vlasništvo nad IP adrese putem usmjeravanje Registries Internet i Registries usmjeravanje Internet. 

- Morate koristiti jedinstvenih/29 podmreže ili dva /30 podmreže da biste postavili BGP peering za svaku peering po ExpressRoute sklopovske (Ako imate više od jedne). 
- Ako je /29 koristi podmreže, ona će se podijeliti na dva /30 podmreže. 
    - Prvi /30 podmreže će se koristiti za primarni vezu, a drugi/30 podmreže će se koristiti za sekundarne veze.
    - Za svaki unos u /30 podmreže, morate koristiti prvi IP adresu na /30 podmreže na usmjerivač. Microsoft će koristiti drugi IP adresu na /30 podmreže da biste postavili BGP sesiju.
    - Morate postaviti i BGP sesije za naše [dostupnost SLA](https://azure.microsoft.com/support/legal/sla/) moraju biti valjane.

## <a name="public-ip-address-requirement"></a>Javnu IP adresa zahtjeva 

### <a name="private-peering"></a>Privatni Peering 

Možete odabrati da biste koristili javna ili privatna IPv4 adrese za privatne peering. Dajemo završetka do kraja odvajanja prometa da adresa za druge korisnike koji se preklapaju nije moguće u slučaju privatne peering. Ove adrese se su objavljeno na Internetu. 

### <a name="public-peering"></a>Javni Peering

Azure javno peering put omogućuje povezivanje na sve usluge u Azure na javnu IP adrese. To obuhvaća navedenih u [Najčešćim Pitanjima ExpessRoute](expressroute-faqs.md) i sve services hostira tvrtka ISV-ovi na Microsoft Azure. Povezivanje servisa Microsoft Azure na javno peering uvijek pokrene u mreži u mrežu na Microsoft. Morate koristiti javnu IP adresa za promet namijenjene prikazivanju na mrežu tvrtke Microsoft.

### <a name="microsoft-peering"></a>Microsoft Peering

Microsoft peering put omogućuje povezivanje s Microsoftovim servisima u oblaku koji nisu podržani kroz Azure javno peering put. Na popisu usluga obuhvaća servisima sustava Office 365, kao što je Exchange Online, sustava SharePoint Online, Skype za tvrtke i CRM Online. Microsoft podržava povezivanje dvosmjerni na Microsoft peering. Promet namijenjene prikazivanju u Microsoftovim servisima u oblaku morate koristiti valjani javno IPv4 adrese prije unesu Microsoftovoj mreži.

Provjerite je li vaša IP adresa i kao broj bilježe se u jednu od registries navedena u nastavku.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Objavljeno Microsoftu putem ExpressRoute javnu IP adresa mora biti objavljeno na Internetu. To može prekinuti vezu s druge Microsoftove servise. Međutim, javnu IP adrese koristi poslužitelji u vašoj mreži komunicirati s krajnje točke O365 u programu Microsoft možda objavljeno putem ExpressRoute. 

## <a name="dynamic-route-exchange"></a>Dinamični usmjeravanje exchange

Usmjeravanje exchange bit će putem protokola eBGP. Sesije EBGP se uspostavili između na MSEEs i vaše usmjerivača. Provjera autentičnosti BGP sesija nije preduvjet. Ako je potrebno, u raspršivanje MD5 mogu konfigurirati. Potražite u članku [Konfiguriranje servis za usmjeravanje](expressroute-howto-routing-classic.md) i [elektronička dodjeljivanje tijekovi rada i država elektronička](expressroute-workflows.md) informacije o konfiguriranju BGP sesije.

## <a name="autonomous-system-numbers"></a>Autonomna sustava brojeva

Microsoft će koristiti kao 12076 za Azure javno dostupnom Azure privatne i Microsoft peering. Da biste 65520 za internu upotrebu smo su rezervirana ASNs iz 65515. Podržane su 16 i 32 bita kao brojevi.

Ne postoje pravila oko simetrije prijenos podataka. Prosljeđivanje i povrata putove možda prolaziti parove različite usmjerivač. Jednake usmjerava morate objavljeno s obje strane preko više elektronička parove koje su vam djelatnici. Usmjeravanje metriku ne moraju biti jednake.

## <a name="route-aggregation-and-prefix-limits"></a>Usmjeravanje zbrajanja i prefiks ograničenja

Podržavamo do 4000 prefiksi objavljeno nam kroz na Azure privatne peering. To možete povećati do 10 000 prefiksi ako je omogućen dodatak premium ExpressRoute. Ne možemo prihvatiti do 200 prefiksi po sesiji BGP za Azure javno te Microsoft peering. 

Sesije BGP će biti ispušteni ako je broj prefiksi premašuje ograničenje. Ne možemo prihvaća zadani usmjerava privatne peering vezu samo. Davatelj usluge mora filtriraju zadani smjer i privatni IP adrese (RFC 1918) iz javne Azure i Microsoft peering putovi. 

## <a name="transit-routing-and-cross-region-routing"></a>Usmjeravanje prijenosa i regije-preusmjeravanje

ExpressRoute ne može konfigurirati kao usmjerivača prijenosa. Morat ćete je za davatelja usluge povezivanja za servise za usmjeravanje prijenosa.

## <a name="advertising-default-routes"></a>Oglašavanje zadani usmjerava

Zadani usmjerava dopušteno samo na Azure privatne sesije peering. U tom slučaju smo će sve promet usmjerili iz povezane virtualne mreža s mrežom. Oglašavanje zadani usmjerava u privatnu peering rezultirat će internet put od Azure blokiranja. Potrebno je za vaše tvrtke rub da biste usmjerili promet na i s Internetom za servise smješten u Azure. 

 Da biste omogućili veza s drugih servisa Azure i servisi infrastrukturu, morate provjeriti je jedna od sljedećih stavki na mjestu:

 - Azure javno peering je omogućen za usmjeravanje prometa javno krajnje točke
 - Korištenje korisnički definirane usmjeravanja da biste omogućili internetska veza za svaki podmreže potrebno s Internetom.
 
>[AZURE.NOTE] Oglašavanje usmjerava zadana će prekinuti Windows i druge VM Aktivacija licence. Slijedite upute [u nastavku](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) da biste to riješili.

## <a name="support-for-bgp-communities-preview"></a>Podrška za BGP zajednica (pretpregled)


Ovo poglavlje sadrži pregled kako BGP zajednica koristit će se s ExpressRoute. Microsoft će Oglasite usmjerava u javno i Microsoft peering putovi s usmjerava označeni zajednice odgovarajuće vrijednosti. Rationale za to detalje o zajednice vrijednosti su opisane ispod. Microsoft, međutim, će poštovati sve vrijednosti zajednice označili usmjerava objavljeno Microsoftu.

Ako se povezujete s Microsoft kroz ExpressRoute na bilo kojem mjestu jedan peering unutar Geopolitički područja, imat ćete pristup svim servisima Microsoft cloud preko svih područja unutar Geopolitički granicu. 

Ako, na primjer, ako povezani Microsoftu Amsterdam kroz ExpressRoute će imati pristup sve Microsoftovim servisima u oblaku smješten u Sjevernoj Europe i Zapad Europe. 

Odnose se na stranicu [ExpressRoute partnera i peering mjesta](expressroute-locations.md) detaljni popis Geopolitički područja, pridruženi Azure regije i odgovarajuće ExpressRoute peering mjesta.

Možete kupiti više ExpressRoute elektronička po Geopolitički regija. Imate više veza nudi brojne prednosti na visoke dostupnosti zbog zemlj zalihosti. U slučajevima gdje imate više ExpressRoute krugova, primit ćete isti skup prefiksi objavljeno iz Microsoft na javno peering i Microsoft peering putovi. To znači da će imati više putova u mreži u Microsoft. To može uzrokovati potencijalno podgrupama optimalnih usmjeravanje odluke želite učiniti u mreži. Kao rezultat, mogli biste primijetiti podgrupama optimalnih povezivanje sučelja za različite servise. 

Microsoft će oznaci prefiksi objavljeno putem javne peering i Microsoft peering vrijednostima odgovarajuće BGP zajednice koji označava područja prefiksa nalaze se u. Možete oslanjate vrijednosti zajednice odgovarajuće usmjeravanje odluke nudi [optimalnih usmjeravanje klijentima](expressroute-optimize-routing.md).

| **Geopolitički regija** | **Područje Microsoft Azure** | **Vrijednost BGP zajednice** |
|---|---|---|
| **Sjeverna Amerika** |    |  |
|    | Istočni SAD-a | 12076:51004 |
|    | Istočni sad 2 | 12076:51005 |
|    | Zapad SAD-a | 12076:51006 |
|    | Zapad sad 2 | 12076:51026 |
|    | Zapad središnje SAD-a | 12076:51027 |
|    | Sjeverna središnje SAD-a | 12076:51007 |
|    | Južna središnje SAD-a | 12076:51008 |
|    | Središnje SAD-a | 12076:51009 |
|    | Središnja Kanada | 12076:51020 |
|    | Istok Kanada | 12076:51021 |
| **Južna Amerika** |  |  |
|    | Južna Brazil | 12076:51014 |
| **Europa** |    |  |
|    | Sjeverna Europa | 12076:51003 |
|    | Europa Zapad | 12076:51002 |
| **Države Pacifičke** |    |   |
|    | Istočnoazijski | 12076:51010 |
|    | Jugoistočne Azije | 12076:51011 |
| **Japan** |     |   |
|    | Istok Japan | 12076:51012 |
|    | Japan Zapad | 12076:51013 |
| **Australija** |    |   | 
|    | Istok Australija | 12076:51015 |
|    | Australija Jugoistok | 12076:51016 |
| **Indija** |    |   |
|    | Južna Indija | 12076:51019 |
|    | Indija Zapad | 12076:51018 |
|    | Središnja Indija | 12076:51017 |

Svi putovi objavljeno od Microsofta će označeni vrijednost odgovarajuće zajednice. 

>[AZURE.IMPORTANT] Globalni prefiksi će označeni vrijednost odgovarajuće zajednice i će objavljeno samo kada je omogućen dodatak ExpressRoute premium.


Osim navedenog, Microsoft će označavanje i prefiksi koji se temelji na usluzi pripadaju. To se odnosi samo na Microsoft peering. U tablici u nastavku nudi mapiranja servisa BGP zajednice vrijednost.

| **Servis** | **Vrijednost BGP zajednice** |
|---|---|
| **Exchange** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype za tvrtke** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Druge servise sustava Office 365** | 12076:5100 |

>[AZURE.NOTE] Microsoft poštovati sve vrijednosti zajednice BGP koju ste postavili za usmjerava objavljeno Microsoftu.

## <a name="next-steps"></a>Daljnji koraci

- Konfiguriranje veza s ExpressRoute.

    - [Stvaranje je elektronička ExpressRoute za uvođenje klasičnog model](expressroute-howto-circuit-classic.md) ili [Stvaranje i izmjena je elektronička ExpressRoute pomoću upravitelja resursa Azure](expressroute-howto-circuit-arm.md)
    - [Konfiguriranje usmjeravanja za uvođenje klasičnog modela](expressroute-howto-routing-classic.md) ili [Konfiguriraj usmjeravanja za implementaciju modela Voditelj resursa](expressroute-howto-routing-arm.md)
    - [Klasični VNet da biste je elektronička ExpressRoute vezu](expressroute-howto-linkvnet-classic.md) ili [Voditelj resursa VNet da biste je elektronička ExpressRoute](expressroute-howto-linkvnet-arm.md)


