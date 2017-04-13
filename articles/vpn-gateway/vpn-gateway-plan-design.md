<properties 
   pageTitle="Pristupnik za VPN planiranja i dizajna | Microsoft Azure"
   description="Dodatne informacije o VPN pristupnika planiranja i dizajna za više lokacija, hibridnog i VNet VNet veze"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Planiranje i dizajniranje za pristupnik za VPN-a

Planiranje i dizajniranje više lokacija i konfiguracija VNet VNet može biti jednostavne ili složene, ovisno o vašim potrebama za umrežavanje. U ovom se članku vodit će vas kroz osnovne razmatranja planiranja i dizajna.

## <a name="planning"></a>Planiranje


### <a name="compare"></a>Mogućnosti za povezivanje s više lokacija

Želite li sigurno povezivanje web-mjesta na lokalni virtualne mrežom, imate tri načina da biste to učinili: web-mjesta-na-web-mjesta, točke na mjesta i ExpressRoute. Usporedba različitih više lokacija veze koje su dostupne. Mogućnost odaberete mogu ovisiti o raznim pitanja vezana uz kao što su:


- Kakvu vrstu propusnost potreban rješenje?
- Želite li komunikaciju putem Interneta javno putem sigurne VPN-a ili putem privatne veze?
- Imate li javna IP adresa dostupan za korištenje?
- Namjeravate li VPN uređaja? Ako je tako, je li kompatibilan?
- Se povezujete samo nekoliko računala ili želite Neprekidna veza web-mjesta?
- Koju vrstu VPN pristupnika je potrebno za koju želite stvoriti rješenje?
- Pristupnik SKU morate koristiti?


U sljedećoj su tablici pomoći da odlučite najbolja mogućnost za povezivanje za rješenje.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Preduvjeti za pristupnik Vrsta VPN-a i SKU

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Dodatne informacije o pristupnika SKU-ove potražite u članku [Postavke pristupnika VPN-a](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Zbrajanja propusnost po vrsti SKU i VPN-a

U sljedećoj su tablici prikazane vrste pristupnika i Procijenjena zbrajanja propusnost. Procijenjena zbrajanja propusnost možda deciding faktor za dizajn.
Cijene razlikuju pristupnika SKU-ove. Informacije o cijenama, potražite u članku [Cijene za VPN pristupnika](https://azure.microsoft.com/pricing/details/vpn-gateway/). U ovoj su tablici primjenjuje se na Voditelj resursa i uvođenje klasičnog modela.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Podržani konfiguracije po vrsti SKU i VPN-a

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Tijek rada

Sljedeći popis opisuje uobičajene tijeka rada za oblak povezivanje:

1.  Dizajniranje i planiranje topologiji povezivanje i popis adresa razmake za sve mreže se želite povezati.
2.  Stvaranje Azure virtualne mreže. 
3.  Stvaranje pristupnika VPN-a za virtualne mreže.
4.  Stvaranje i konfiguriranje veze lokalne mreže ili druge virtualne mreže (po potrebi).
5.  Stvaranje i konfiguriranje točke web vezu za Azure VPN pristupnika (po potrebi).
 

## <a name="design"></a>Dizajn

### <a name="topologies"></a>Topologija veze

Započnite tako da pogledate dijagrama u članku [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md) . Članak sadrži osnovnih dijagrama, modele implementacije za svaki topologije (Voditelj resursa ili klasičnu), a koje implementacije alata koje možete koristiti za implementaciju konfiguraciju.   

### <a name="designbasics"></a>Osnove dizajna

U sljedećim se odjeljcima navode pristupnika osnove VPN-a. Osim toga, razmislite o [mrežnim servisima ograničenja](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>O podmreže

Prilikom stvaranja veze, morate uzeti u obzir podmreže raspone. Ne mogu imati preklapajuće raspone podmreže adresu. Preklapanje podmreže je kada jedan virtualne mreže ili lokalne lokacije sadrže isti prostor za adresu koja sadrži na drugo mjesto. To znači da morate mreža inženjeri za vaše lokalne lokalne mreže da biste carve izvan raspona za koji želite koristiti za Azure IP adresiranja prostora/podmreže. Potreban prostor adresa koji se ne koriste na lokalni lokalnu mrežu. 

Izbjegavanje koji se preklapaju podmreže je važno kada radite s vezama VNet VNet. Ako vaš podmreže se s njime preklapati, a IP adresu postoji i slanja i odredišni VNets, VNet VNet veze neće uspjeti. Azure ne usmjeravate podatke u VNet jer je odredišnu adresu dio slanje VNet. 

Pristupnika VPN-a potreban je određeni podmreže naziva podmreži pristupnika. Sve podmreže pristupnika moraju biti imenovane GatewaySubnet ispravno funkcionirao. Obavezno ne naziv vašoj podmreži pristupnika na drugi, a ne uvesti VMs ili nešto drugo podmreže pristupnika. U odjeljku [podmreže pristupnika](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>O lokalnoj mreži pristupnika

Pristupnik za lokalne mreže obično se odnosi na lokalne lokacije. U modelu uvođenje klasičnog pristupnika lokalne mreže se naziva lokalne mreže web-mjesta. Kada konfigurirate pristupnik lokalne mreže, koje joj dodijeliti naziv, odredite javnu IP adresu uređaja VPN-a za lokalni i Navedite adresu prefiksi koje su lokalne lokacije. Azure razmatra prefiksa odredišnu adresu za mrežni promet, consults konfiguraciju koje ste naveli za pristupnik za lokalnu mrežu i sukladno tome usmjerava pakete. Ove prefiksi adresa možete izmijeniti prema potrebi. Dodatne informacije potražite u članku [pristupnika lokalnoj mreži](vpn-gateway-about-vpn-gateway-settings.md#lng).


#### <a name="gwtype"></a>O vrstama pristupnika

Odabir vrste točan pristupnik za topologiji je važnosti. Ako ste odabrali pogrešnu vrstu, pristupnikom neće pravilno funkcionirati. Vrsta pristupnika određuje način na koji pristupnika sam povezuje i je potrebna konfiguracija postavka za implementaciju model upravljanja resursima.

Vrste pristupnika su sljedeće:

- VPN-a
- ExpressRoute

#### <a name="connectiontype"></a>O vrsta veze

Svaki konfiguracije zahtijeva vrstu određenu vezu. Vrsta veze su:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>O vrstama VPN-a

Svaki konfiguracije zahtijeva određenu vrstu VPN-a. Ako su kombiniranje dvije konfiguracijama, kao što su stvaranje na web-mjesto i veze točke na mjesto na istom VNet morate koristiti vrstu VPN-a koji zadovoljava preduvjete i veze.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

U sljedećoj tablici prikazane vrste VPN-a kao što je karte svaki konfiguraciju veze. Provjerite je li vrstu VPN-a za pristupnikom odgovara konfiguracije koji želite stvoriti. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>Uređaji za VPN-a za veze na web-mjesto

Konfiguriranje veze web-mjesto, bez obzira na to model implementacije, potrebne su vam sljedeće stavke:

- VPN uređaja koji je kompatibilan s Azure VPN pristupnika
- Na dostupnom javnosti IPv4 IP adresa koji se ne nalazi iza NAT

Morate imati iskustvo konfiguriranje uređaju VPN-a ili vam netko možete konfigurirati za vas. Dodatne informacije o VPN uređajima potražite u članku [o VPN uređaja](vpn-gateway-about-vpn-devices.md). U članku uređaji VPN sadrži informacije o provjerene uređaja i preduvjeti za uređaje koji se ne provjerava i veze na dokumente Konfiguracija uređaja ako je dostupna.

### <a name="forcedtunnel"></a>Razmislite o usmjeravanju prisilne tunelom

Za većinu konfiguracije možete konfigurirati prisilne tuneliranje. Prisilne tuneliranje omogućuje preusmjeravanje ili "prisilno" svih povezanih s Interneta promet natrag na lokalne lokacije putem web-mjesto VPN tunelom za provjeru i nadzor. Ovo je ključnih sigurnost preduvjeta za većinu tvrtki IT pravila. 

Bez prisilne tuneliranje promet Internet povezanih s vašeg VMs u Azure će uvijek prolaziti iz Azure mrežne infrastrukture u izravno odgovor s Internetom, bez mogućnosti da biste dopustili Provjera ili revizije promet. Neovlašteni pristup Internetu potencijalno može dovesti do otkrivanja informacije ili druge vrste breaches sigurnost.

**Prisilno tuneliranje dijagrama**

![Prisilno tuneliranje veze] (./media/vpn-gateway-plan-design/forced-tunnel.png "prisilno tuneliranja")

Prisilne tunneling veza moguće je konfigurirati u oba modelima implementacije i pomoću različitih alata. U sljedećoj su tablici dodatne informacije potražite u članku. U ovoj su tablici ažuriramo kao novi članke, nove modele implementacije i dodatne alate postaju dostupne za tu konfiguraciju. Kada članak nije dostupan, ne možemo veza izravno u nju iz tablice.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Daljnji koraci

Pogledajte na [Najčešća pitanja vezana uz VPN pristupnika](vpn-gateway-vpn-faq.md) i [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md) dodatne informacije potražite u člancima će vam pomoći s dizajnom.

Dodatne informacije o postavkama određene pristupnika potražite u članku [O postavki za VPN pristupnika](vpn-gateway-about-vpn-gateway-settings.md).




