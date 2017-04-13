<properties 
   pageTitle="Što su korisnički definirana usmjerava i prosljeđivanje IP?"
   description="Saznajte kako koristiti korisnički definirana usmjerava (UDR) i prosljeđivanje IP za prosljeđivanje promet mrežnom virtualne aparata u Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Što su korisnički definirana usmjerava i prosljeđivanje IP?
Kada dodate virtualnim strojevima (VMs) virtualne mreže (VNet) u Azure, primijetit ćete da su u VMs moći komunikaciju putem mreže, automatski. Nije potrebno da biste odredili pristupnika, čak i ako su na VMs u različitim podmreže. Isto vrijedi za komunikaciju s na VMs javnog Interneta, a čak i za lokalnih postoji mreže veza hibridnog Azure vlastite podatkovnog centra.

Ovaj tijek komunikacije je moguće jer Azure koristi niz usmjerava sustava da biste definirali tok IP promet. Usmjerava sustava kontrolirati tijek komunikacije u sljedećim scenarijima:

- Iz u istoj podmreži.
- Iz podmreže na drugu unutar na VNet.
- Iz VMs s Internetom.
- Iz VNet drugi VNet putem VPN pristupnika.
- S VNet lokalne mreže putem VPN pristupnika.

Na slici u nastavku prikazuje jednostavne instalacijski program na VNet, dva podmreže i nekoliko VMs, zajedno s usmjerava sustav koji omogućuju IP promet na tijek pločica.

![Usmjerava sustava u Azure](./media/virtual-networks-udr-overview/Figure1.png)

Iako pomoću sustava usmjerava olakšava promet automatski za implementaciju sustava, postoje slučajevi u kojima želite upravljati usmjeravanje pakete putem virtualne uređaj. Pa tako da stvorite korisnički definirane usmjerava koje navodite sljedeći put za pakete slijedi određene podmreže da biste prešli na vaš uređaj virtualne umjesto i omogućavanja IP prosljeđivanja za VM izvodi kao virtualne potražite.

Na slici u nastavku prikazuje primjer usmjerava korisnički definirane i prosljeđivanje IP da biste nametnuli pakete poslani podmreže jedan od druge da bude obrađen virtualne potražite na treću podmreži.

![Usmjerava sustava u Azure](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Korisnički definirane usmjerava samo primjenjuju se na promet ostavite podmreži. ne možete stvoriti usmjerava da biste odredili kako promet stupa podmreži s Interneta, na primjer. Osim toga, potražite prosljeđujete promet ne mogu biti u istoj podmreži gdje potječe promet. Uvijek možete stvoriti zaseban podmreže za svoje aparata. 

## <a name="route-resource"></a>Usmjeravanje resursa
Pakete usmjeruje putem mreže TCP/IP temeljen na tablici usmjeravanje definirani pri svakom čvor fizičke mreži. Usmjeravanje tablica je skup pojedinačnih smjerova služi za odabir mjesta Proslijedi pakete temelju odredišnu IP adresu. Smjer sastoji se od sljedećeg:

|Svojstvo|Opis|Ograničenja|Razmatranja|
|---|---|---|---|
| Adresa prefiksa | Odredište CIDR na koju smjer odnosi, kao što su 10.1.0.0/16.|Mora biti valjani raspon CIDR koji predstavlja adrese javnog Interneta, Azure virtualne mreže ili lokalni podatkovnog centra.|Provjerite je li **Adresa prefiks** sadrže adresu za **sljedeći put adresa**, u suprotnom će vaše pakete unijeti u petlji prijelaz s izvorišnog web-mjesta na sljedeći put bez ikad Dostizanje odredište. |
| Sljedeće vrste skoka | Vrsta Azure skoka paket moraju se poslati u. | Mora biti u jednom od sljedećih vrijednosti: <br/> **Virtualne mreže**. Predstavlja lokalne virtualne mreže. Ako, primjerice, ako imate dvije podmreže, 10.1.0.0/16 i 10.2.0.0/16 u istom virtualne mreže, usmjeravanje za svaki podmreže u tablici usmjeravanje neće imati vrijednost sljedećeg skoka *Virtualne*mreže. <br/> **Pristupnik virtualne mreže**. Predstavlja programa Azure S2S VPN pristupnika. <br/> **Internet**. Predstavlja zadani pristupnik Internet nudi infrastrukturu Azure. <br/> **Virtualna uređaj**. Predstavlja virtualne potražite dodali Azure virtualne mreže. <br/> **Ništa**. Predstavlja crno rupe. Pakete proslijediti crno rupe se uopće ne prosljeđuju.| Razmislite o korištenju vrstu **ništa** da biste prestali pakete iz slijedi navedene odredište. | 
| Sljedeći put adresa | Sljedeći adresa skoka sadrži IP adresa pakete treba proslijediti. Sljedeći put vrijednosti dopuštene su samo u usmjerava gdje se nalazi sljedeće vrsti skoka *Virtualne potražite*.| Mora biti IP adresu koja je dostupna u virtualne mreže na kojima se primjenjuje usmjeravanje korisnički definirane. | Ako se s IP adresom predstavlja na VM, provjerite je li omogućite Azure [Prosljeđivanje IP](#IP-forwarding) za na VM. |

U ljusci PowerShell Azure neke vrijednosti "NextHopType" imaju različite nazive:
- Virtualna mreža nije VnetLocal
- Virtualna mrežni pristupnik je VirtualNetworkGateway
- Virtualna potražite je VirtualAppliance
- Internet je Internet
- Ništa nema

### <a name="system-routes"></a>Usmjerava sustava
Svaki podmreže stvorene u virtualne mreže je automatski povezuju s usmjeravanje tablicu koja sadrži sljedeća pravila za usmjeravanje sustava:

- **Lokalni Vnet pravilo**: Ovo pravilo automatski se stvara za svaki podmreže u virtualne mreže. Navodi da postoji izravnu vezu između VMs u na VNet i postoji bez Srednja sljedeći put.
- **Lokalni pravilo**: Ovo pravilo primjenjuje na sve promet namijenjene prikazivanju raspon adresa lokalnog i koristi pristupnik VPN-a kao sljedeći željeno mjesto.
- **Pravilo Internet**: pravilo ručice sav promet namijenjene prikazivanju javni Internet (adresu prefiks 0.0.0.0/0) i koristi Internetski pristupnik infrastrukture kao sljedeći put sve promet namijenjene prikazivanju s Internetom.

### <a name="user-defined-routes"></a>Korisnički definirane usmjerava
Za većinu okruženja samo ćete usmjerava sustava već definira Azure. Međutim, možda ćete morati stvoriti tablicu usmjeravanje i dodati jedan ili više usmjerava u nekim slučajevima, kao što su:

- Prisilna Primjena tuneliranje na Internetu putem lokalne mreže.
- Korištenje virtualnih aparata u okruženju sustava Azure.

U gornjem slučajevima morat ćete stvaranje usmjeravanje tablice i dodajte usmjerava korisnički definirane. Imate više tablica usmjeravanje, a isti smjer tablica može biti povezan jedan ili više podmreže. I svaki podmreže mogu se povezati samo jedan usmjeravanje tablicu. Sve VMs i servise u oblaku podmreže koristi tablici usmjeravanje povezan te podmreže.

Podmreže oslanjate usmjerava sustava dok je povezan sa podmreži usmjeravanje tablice. Nakon pridruživanja postoji, usmjeravanje obavlja utemeljene na najdulje prefiks Match (LPM) između usmjerava korisnički definirane i usmjerava sustava. Ako postoji više od jedne usmjeravanje s istom match LPM zatim rutu je odabran na temelju njezinu izvoru sljedećim redoslijedom:

1. Korisnički definirane usmjeravanje
1. Usmjeravanje BGP (kada se koristi ExpressRoute)
1. Usmjeravanje sustava

Da biste saznali kako stvoriti korisnički definirane usmjerava potražite u [članku Stvaranje usmjerava i omogućivanje IP prosljeđivanja u Azure](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Korisnički definirane usmjerava primjenjuju samo servisa Azure VMs i oblaka. Ako, primjerice, ako želite dodati vatrozid virtualne potražite između lokalne mreže i Azure, morat ćete stvoriti korisnički definirane usmjeravanje za Azure usmjeravanje tablice koje prosljeđuje sve promet na prostoru adresa lokalnog virtualne potražite. Da biste GatewaySubnet proslijediti sav promet lokalnim Azure putem virtualne potražite možete dodati i korisnički definirane usmjeravanje (UDR). Ovo je nedavno zbrajanja.

### <a name="bgp-routes"></a>Usmjerava BGP
Ako imate vezu s ExpressRoute između lokalne mreže i Azure, možete omogućiti BGP proširiti usmjerava iz lokalne mreže Azure. Ove BGP usmjerava se koriste na isti način kao usmjerava sustava i usmjerava korisnički definirano u svakom Azure podmreže. Dodatne informacije potražite u članku [Uvod ExpressRoute](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Možete konfigurirati okruženje sustava Azure da biste koristili prisilno tuneliranja kroz lokalnu mrežu tako da stvorite korisnički definirane smjer podmreže 0.0.0.0/0 koji koristi pristupnik za VPN-a kao sljedeći put. No to funkcionira samo ako koristite VPN pristupnik nije ExpressRoute. Za ExpressRoute, prisilne tuneliranje konfiguriran putem BGP.

## <a name="ip-forwarding"></a>Prosljeđivanje IP
Kao opisuju iznad, jedna od glavna razloga zbog kojih za stvaranje korisnički definiranih smjer jest da preusmjerava promet na virtualne potražite. Virtualna potražite je ništa više VM koja se pokreće aplikacija korištena za rukovanje mrežni promet na mobitel, kao što su vatrozid ili NAT uređaja.

Ovaj virtualni potražite VM moraju imati mogućnost primanja dolazne prometa čije je adresirana sa sobom. Da biste omogućili VM prima promet adresirana drugih odredišta, morate omogućiti IP prosljeđivanje za na VM. Ovo je programa Azure postavljanje nije postavku u operacijskom sustavu gost.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [stvoriti usmjerava u modelu implementacije Voditelj resursa](virtual-network-create-udr-arm-template.md) i povezati ih podmreže. 
- Saznajte kako [stvoriti usmjerava u modelu uvođenje klasičnog](virtual-network-create-udr-classic-ps.md) i povezati ih podmreže.
