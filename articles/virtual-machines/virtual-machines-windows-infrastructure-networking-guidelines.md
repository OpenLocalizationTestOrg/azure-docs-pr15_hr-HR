<properties
    pageTitle="Smjernice za infrastrukturu za povezivanje s mrežom | Microsoft Azure"
    description="Saznajte više o ključa dizajna i implementaciju smjernice za implementaciju virtualne mreže servisa Azure Infrastruktura sustava."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Smjernice za mrežne infrastrukture

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

U ovom se članku usredotočuje se na objašnjenje potrebna planiranja korake za virtualne mreže unutar Azure i povezivanjem između postojeće lokalno okruženja.


## <a name="implementation-guidelines-for-virtual-networks"></a>Smjernice za implementaciju za virtualne mreže

Odluka:

- Koje vrste virtualne mreže morate za hostiranje radno opterećenje IT ili infrastrukture (samo oblak ili više lokacija)?
- Za više lokacija virtualne mreže, koliko prostora na adresi potrebne za hostiranje podmreže i VMs sada i u budućnosti pametnije proširenja?
- Koje namjeravate stvoriti središnje virtualne mreže ili stvoriti pojedinačne virtualne mreže za svaku grupu resursa?

Zadaci:

- Definiranje prostor adrese za virtualne mreže će biti stvoren.
- Definirati skup podmreže i prostor adrese za svaki unos.
- Za više lokacija virtualne mreže definirati skup razmake adresu lokalne mreže za lokalni mjesta koje je potrebno dosegne VMs u virtualne mreže.
- Rad s lokalnim umrežavanje tima da biste bili sigurni odgovarajuće usmjeravanje konfiguriran pri stvaranju-lokacija virtualne mreže.
- Stvaranje virtualne mreže pomoću svoje konvencija imenovanja.


## <a name="virtual-networks"></a>Virtualne mreže

Virtualne mreže su potrebne za podršku komunikaciju između virtualnim strojevima (VMs). Možete definirati podmreže, prilagođene IP adresa, postavke DNS-a, sigurnost filtriranja i uravnoteženje opterećenja, kao i u slučaju fizičke mrežama. Pomoću [pristupnika VPN-a](../vpn-gateway/vpn-gateway-about-vpngateways.md) ili [Express usmjeravanje elektronička](../expressroute/expressroute-introduction.md)možete se povezati Azure virtualne mreže vaše lokalne mreže. Dodatne informacije o [virtualne mreže i njihovih komponenti](../virtual-network/virtual-networks-overview.md).

Pomoću grupa resursa imate fleksibilnosti u način dizajna virtualne mrežne komponente. VMs možete povezati s virtualne mreže izvan vlastitu grupu resursa. Uobičajeni način dizajna bi da biste stvorili grupa središnje resursa koje sadrže preduvjete core mrežne infrastrukture koje možete upravljati njime mogu uobičajenih tima, a zatim VMs i njihovim aplikacijama implementiran na zasebnom resursa grupe. Taj se način omogućuje pristup vlasnici aplikacije resursa grupu koja sadrži njihove VMs bez otvaranja pristup konfiguracije šire virtualne mrežni resursi.

## <a name="site-connectivity"></a>Povezivanje web-mjesta

### <a name="cloud-only-virtual-networks"></a>Samo oblak virtualne mreže
Ako lokalne korisnike i računala ne zahtijeva tijeku veza sa sustavom VMs u Azure virtualne mreže, dizajnu virtualne mreže je izravno naprijed:

![Osnovni samo oblak virtualne mrežni dijagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Taj se način je obično radnih opterećenja mjesto na Internetu, kao što je poslužitelj za web-mjesto utemeljeno na Internet. Možete upravljati te VMs pomoću RDP ili točke na mjestu VPN veze.

Budući da se mogu povezati s lokalne mreže, samo za Azure virtualne mreže možete koristiti bilo koji dio privatne razmak IP adresa, čak i ako se isti prostor za privatne koristiti lokalnog.


### <a name="cross-premises-virtual-networks"></a>Više lokacija virtualne mreže
Ako lokalne korisnike i računala potreban tijeku veza sa sustavom VMs u Azure virtualne mreže, stvorite više lokacija virtualne mreže.  Povezivanje s mrežom na lokalni ExpressRoute ili web-mjesto VPN veza.

![Više lokacija virtualne mrežni dijagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

U ovoj konfiguraciji Azure virtualne mreže je zapravo oblaku nastavkom lokalne mreže.

Budući da se povežu s mrežom na lokalni-lokacija virtualne mreže morate koristiti dio prostor adrese koristi vaša tvrtka ili ustanova koja je jedinstvena. Na isti način različitih mjesta tvrtke dodjeljuju se određene IP podmreže, Azure postaje neko drugo mjesto, kao što je proširiti izvan mreže.

Da biste omogućili pakete morali putovati u mreži virtualne više lokacija lokalne mreže, morate konfigurirati skup odgovarajuću lokalnog adresu prefiksi kao dio definicije lokalne mreže za virtualne mreže. Ovisno o prostor adrese virtualne mreže i skup relevantnih lokalnog mjesta, može postojati više adresa prefiksi u lokalnoj mreži.

Samo oblak virtualne mreže možete pretvoriti u više lokacija virtualne mreže, ali najvjerojatnije potrebno da biste ponovno IP prostor adrese virtualne mreže i Azure resursi. Stoga Pažljivo razmotrite ako virtualne mreže mora biti povezani s mrežom na lokalni Kada dodjeljujete programa IP podmreže.

## <a name="subnets"></a>Podmreže
Podmreže omogućuju organiziranje resursa koje su povezane ili logički (na primjer, jedan podmreže za VMs povezane iste aplikacije), ili fizički (na primjer, jedan podmreže po grupa resursa). Možete koristiti i podmreže odvajanja tehnika radi dodatne sigurnosti.

Za više lokacija virtualne mreže, trebali biste dizajnirati podmreže s istom konvencijama koju koristite za Lokalni resursi. **Azure uvijek koristi prva tri IP adrese adresnog prostora za svaki podmreže**. Da biste utvrdili broj adresa koja su potrebna za podmreži, najprije prebrojavanje VMs koje su vam potrebne sada. Procjenu za buduće rasta, a zatim koristite sljedeću tablicu za određivanje veličine podmreži.

Broj VMs potrebno | Broj bitova glavno računalo potrebno | Veličina podmreži
--- | --- | ---
1 – 3 | 3 | / 29
4 – 11     | 4 | / 28
12-27 | 5 | / 27
28 – 59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Maksimalni broj adrese glavno računalo za podmreži s bitova n glavno računalo podmreže normalni lokalnog je 2<sup>n</sup> – 2. Za Azure podmreže, maksimalni broj adrese glavno računalo za podmreže s bitova n glavno računalo je 2<sup>n</sup> – 5 (2 i 3 za adrese koje koristi Azure na svakom podmreži).

Ako odaberete veličinu podmreže premalen, morate ponovno IP i ponovno implementirate VMs u podmreži.


## <a name="network-security-groups"></a>Mrežni sigurnosne grupe
Filtriranje pravila možete primijeniti da biste promet koji se piše putem virtualne mreže pomoću mreže sigurnosne grupe. Možete sastaviti zrnastog filtriranja pravila radi zaštite vaše virtualne umrežavanje okruženje kontroliranje ulazni i izlazni promet, izvorišni i odredišni IP raspona, dopušteni priključke, itd. Mrežni sigurnosne grupe mogu primijeniti podmreže unutar virtualne mreže ili izravno sučelje za dani virtualne mreže. Preporučuje se da bi se neke razine mreže sigurnosne grupe filtriranje promet na vaše virtualne mreže. Dodatne informacije o [Sigurnosnim grupama s mrežom](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Dodatne komponente mreže
Kao i kod programa lokalnog fizičke mrežne infrastrukture, Azure virtualne mreže mogu sadržavati više od podmreže i odabiru IP adresa. Dizajnirate infrastruktura za aplikaciju, trebali biste uključivanje neke od ovih dodatnih komponenti:

- [Pristupnika VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) - povezivanje Azure virtualne mreže s drugim mrežama Azure virtualne ili povezivanje s lokalnim mrežama putem veze za VPN-a web-mjesto. Implementacija Express usmjeravanje veze za namjenski, sigurne veze. Možete unijeti i korisnici izravan pristup s vezama VPN točke na web.
- [Učitavanje opterećenja](../load-balancer/load-balancer-overview.md) - nudi opterećenja prometa za vanjskih i internih promet po želji.
- [Pristupnik za aplikaciju](../application-gateway/application-gateway-introduction.md) - HTTP ujednačavanje opterećenja na aplikacijskom sloju, koja omogućuje neke od dodatnih pogodnosti za web-aplikacije umjesto implementacija u Azure učitavanje raspoređivača.
- [Upravitelj promet](../traffic-manager/traffic-manager-overview.md) - promet koji se temelji na DNS distribucije za usmjeravanje krajnji korisnici najbliže krajnja točka dostupne aplikacije koji hostira vaša aplikacija iz Azure podatkovnim centrima u različitim područjima omogućava.


## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 