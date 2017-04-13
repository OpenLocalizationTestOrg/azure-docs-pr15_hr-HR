<properties
   pageTitle="Javne i privatne IP adresiranja (klasični) Azure | Microsoft Azure"
   description="Dodatne informacije o javnim i privatnim IP adresiranja servisu Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP adrese (klasični) u Azure
IP adrese možete dodijeliti Azure resursa možete komunicirati s ostalim resursima Azure, lokalne mreže i s Internetom. Postoje dvije vrste IP adresa možete koristiti u Azure: javnim i privatnim.

Javnu IP adrese koriste se za komunikaciju s Internetom, uključujući servisa Azure dostupnom javnosti.

Kada koristite pristupnika VPN-a ili ExpressRoute elektronička da biste proširili mreže za Azure privatno IP adrese koriste se za komunikacije unutar Azure virtualne mreže (VNet), servis u oblaku i lokalne mreže.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvršiti ove korake pomoću modela implementacije Voditelj resursa](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Javnu IP adresa
Javnu IP adrese omogućuju Azure resursa možete komunicirati s Internetom i Azure dostupnog javnosti servise kao što je [Azure Redis predmemorije](https://azure.microsoft.com/services/cache/), [Azure događaj koncentratora](https://azure.microsoft.com/services/event-hubs/), [baze podataka SQL](../sql-database/sql-database-technical-overview.md)i [Azure prostora za pohranu](../storage/storage-introduction.md).

Javna IP adresa je povezan sa sljedećim vrstama resursa:

- Servisi u oblaku
- IaaS virtualnim strojevima (VMs)
- Instance PaaS uloga
- Pristupnika VPN-a
- Aplikacija pristupnika

### <a name="allocation-method"></a>Način dodjele
Kad javnu IP adresa mora biti dodijeljena Azure resursa, preporučuje se *dinamički* dodijeliti iz skupa dostupna javnu IP adresu unutar mjesta stvara se resurs. U ovom IP adresa je objavio kada je zaustavljena resursa. U slučaju da od servis u oblaku, to se događa kada su zaustavljena sve instance uloge, koji mogu biti izbjegava pomoću *statičke* (rezervirane) IP adresu (pogledajte [Servise u Oblaku](#Cloud-services)).

>[AZURE.NOTE] Popis IP rasponi javnu IP adrese iz kojeg se dodijeliti Azure resursima je objavljeno na [Azure podatkovnog centra IP raspone](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>Razrješavanje DNS naziv glavnog računala
Kada stvorite servis u oblaku ili na IaaS VM, morate Navedite naziv DNS servis oblaka koja je jedinstvena preko svih resursa u Azure. Time ste stvorili mapiranja u Azure upravlja DNS poslužitelji za *dnsname*. cloudapp.net na javnu IP adresu resursa. Ako, primjerice, prilikom stvaranja servis u oblaku s nazivom DNS servis oblaka od **tvrtke contoso**, naziv (FQDN) u potpunosti kvalificirana domene **contoso.cloudapp.net** će riješiti na javnu IP adresi (VIP) servisa u oblaku. U ovom FQDN možete koristiti da biste stvorili prilagođenu domenu CNAME zapis koja pokazuje na javnu IP adresa u Azure.

### <a name="cloud-services"></a>Servisi u oblaku
Servis u oblaku uvijek ima javnu IP adresu se nazivaju virtualna IP adresa (VIP). Možete stvoriti krajnje točke u oblaku za pridruživanje različite priključke VIP interne priključcima na VMs i uloga instance unutar servisa u oblaku. 

Servis u oblaku mogu sadržavati više IaaS VMs ili PaaS uloga instance, sve prikazanog kroz isti VIP servisa oblaka. Možete dodijeliti i [više VIPs na servis u oblaku](../load-balancer/load-balancer-multivip.md), koji omogućuje višestruki VIP scenariji kao što je više klijentu okruženje s operacijskim sustavom SSL web-mjesta.

Možete biti sigurni javnu IP adresu servis u oblaku ostaje, čak i kad su sve instance uloga zaustavljena, pomoću *statičke* javnu IP adrese, se nazivaju [Rezervirana IP](virtual-networks-reserved-public-ip.md). Možete stvoriti (rezervirane) statičke IP resursa na određenom mjestu i dodjeljivanje na neki servis u oblaku na tom mjestu. Ne možete navesti stvarni IP adresa za rezervirana IP, Dodijeljeno iz skupa dostupna IP adrese na mjestu stvara se. U ovom IP adresa je objavio dok izričito brisanje.

Statične (rezervirane) javnu IP adrese obično se koristi u scenarijima gdje je servis u oblaku:

- potreban je pravila vatrozida biti postavljanje po krajnjim korisnicima.
- ovisi o vanjskim Razrješavanje DNS naziva, i dinamičke IP bi zahtijevaju ažuriranje na zapisa.
- troši vanjskih web services koje koriste model sigurnosti IP temelji.
- Koristi SSL certifikata koji su povezani s IP adresom.

>[AZURE.NOTE] Prilikom stvaranja klasični VM spremnika *u oblaku* je koji je stvorio Azure koja ima virtualna IP adresa (VIP). Stvaranje završetku putem portala zadane RDP ili SSH *krajnje* je konfiguriran na portalu tako da se možete povezati s VM putem servisa VIP oblaka. Ovaj VIP servisa oblaka može biti rezervirana, koja omogućuje učinkovito Rezervirana IP adresa da biste se povezali s VM. Možete otvoriti dodatne priključke konfiguriranjem više krajnje točke.

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs i PaaS instance uloga
Možete dodijeliti javnu IP adrese izravno IaaS [VM](../virtual-machines/virtual-machines-linux-about.md) ili PaaS uloga instance unutar servis u oblaku. To se naziva instancu razinom javnu IP adresu ([ILPIP](virtual-networks-instance-level-public-ip.md)). Javnu IP adresu može biti samo dinamičke.

>[AZURE.NOTE] Time se razlikuje od VIP servis za oblak koji je spremnik za IaaS VMs ili PaaS uloga instance, budući da neki servis u oblaku mogu sadržavati više IaaS VMs ili PaaS uloga instance, sve vidljiva kroz isti VIP servisa oblaka.

### <a name="vpn-gateways"></a>Pristupnika VPN-a
[Pristupnik za VPN-a](../vpn-gateway/vpn-gateway-about-vpngateways.md) mogu se povezati programa Azure VNet Azure VNets drugih lokalne mreže. Pristupnik za VPN-a na javnu IP adresa *dinamički*, koja omogućuje komunikaciju s daljinski mrežni je dodijeljena.

### <a name="application-gateways"></a>Aplikacija pristupnika
Azure [aplikacije pristupnika](../application-gateway/application-gateway-introduction.md) može se koristiti za Layer7 ujednačavanje opterećenja za usmjeravanje mrežni promet na temelju HTTP. Pristupnik za aplikaciju na javnu IP adresu *dinamički*, koje služi kao VIP uravnoteženja je dodijeljena.

### <a name="at-a-glance"></a>Na prvi pogled
U sljedećoj tablici prikazane svaku vrstu resursa s načine moguće dodijeljeni (dinamičkih na statički) i mogućnost da biste dodijelili više javnu IP adresa.

|Resurs|Dinamični|Statički|Više IP adresa|
|---|---|---|---|
|Servis u oblaku|Da|Da|Da|
|Uloga instance IaaS VM ili PaaS|Da|ne|ne|
|Pristupnik za VPN-a|Da|ne|ne|
|Pristupnik za aplikaciju|Da|ne|ne|

## <a name="private-ip-addresses"></a>Privatno IP adresa
Privatno IP adrese omogućuju Azure resursa možete komunicirati s drugih resursa u neki servis u oblaku ili [virtualne mreže](virtual-networks-overview.md)(VNet) ili lokalne mreže (putem pristupnika VPN-a ili ExpressRoute elektronička), bez korištenja Internet dostupna IP adresa.

U modelu Azure uvođenje klasičnog mogu dodijeliti privatne IP adrese u sljedećim resursima Azure:

- IaaS VMs i PaaS instance uloga
- Interna opterećenja
- Pristupnik za aplikaciju

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs i PaaS instance uloga
Virtualnim strojevima (VMs) stvorene pomoću model klasični implementacije se uvijek smještaju u oblaku, slično PaaS uloga instance. Ponašanje privatne IP adrese stoga su slične za ove resurse.

Važno je da Imajte na umu da je servis u oblaku može uvesti na dva načina:

- Kao *samostalni* servise u oblaku, gdje nije unutar virtualne mreže.
- Kao dio virtualne mreže.

#### <a name="allocation-method"></a>Način dodjele
U slučaju servise u oblaku *samostalne* resursi zatražite u privatnu IP adresa dodijeliti *dinamički* od Azure podatkovnog centra privatne IP adresa raspon. Može se koristiti samo za komunikaciju s drugim VMs unutar iste servis u oblaku. U ovom IP adresa možete promijeniti kada resurs je zaustavio i rada.

U slučaju u uveden u virtualne mreže servis u oblaku, resursi se privatni adrese poruke IP dodijeliti iz adresa raspon povezane subnet(s) (kao što je navedeno u njegov mrežna konfiguracija). U ovom privatne IP adrese poruke može se koristiti za komunikaciju između svih VMs unutar na VNet.

Uz to, u slučaju servise u oblaku unutar na VNet, privatne IP adrese dodijeljeno *dinamički* (pomoću DHCP) prema zadanim postavkama. Možete promijeniti kada je resurs zaustavio i rada. Da biste osigurali IP adresa ostaje, morate postaviti način dodjele u *statični*i Navedite valjanu IP adresu rasponu odgovarajuće adrese.

Statičke IP adrese privatne najčešće koriste se za:

 - VMs funkcionirati kao kontrolera domene ili DNS poslužitelji.
 - VMs koje je potrebno pravila vatrozida pomoću IP adresa.
 - Pokretanje servisa pristupiti pomoću drugih aplikacija do IP adresu VMs.

#### <a name="internal-dns-hostname-resolution"></a>Interna Razrješavanje DNS naziv glavnog računala
Sve Azure VMs i PaaS uloga instance konfiguriranih s [Azure upravlja DNS poslužitelji](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) prema zadanim postavkama, osim ako izričito konfiguriranje prilagođene DNS poslužitelji. Ove DNS poslužitelji Navedite naziv internog razlučivost za VMs i instance uloge koje se nalaze u istom VNet ili oblaka servis.

Kada stvorite na VM, mapiranja za naziv glavnog računala na njegovu privatne IP adresu dodaje se Azure upravlja DNS poslužitelji. U slučaju višestruki NIC VM, glavnog računala mapirati je privatne IP adrese primarni NIC. Međutim, ovaj informacije o mapiranju ograničen je na resurse unutar iste servis u oblaku ili VNet.

U slučaju servise u oblaku *samostalne* moći da biste riješili hostnames sve instance VMs/uloga unutar iste servisa u oblaku samo. U slučaju servise u oblaku unutar na VNet moći da biste riješili hostnames sve instance VMs/uloga unutar na VNet.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interna učitavanja balancers (ILB) i aplikacije pristupnika
Konfiguracija **sučelje** programa [Azure interne raspoređivača opterećenja](../load-balancer/load-balancer-internal-overview.md) (ILB) ili [Azure aplikacije pristupnika](../application-gateway/application-gateway-introduction.md)možete dodijeliti privatne IP adrese. Privatne IP adrese služi kao Interna krajnje, dostupna samo za resurse unutar virtualne mrežom (VNet) i udaljene mreže povezani s VNet. Konfiguriranje sučelje možete dodijeliti ili na dinamičke ili statičke privatne IP adrese. Možete dodijeliti i više privatne IP adrese da biste omogućili višestruki vip scenarijima.

### <a name="at-a-glance"></a>Na prvi pogled
U sljedećoj tablici prikazane svaku vrstu resursa s načine moguće dodijeljeni (dinamičkih na statički) i mogućnost da biste dodijelili više privatne IP adresa.

|Resurs|Dinamični|Statički|Više IP adresa|
|---|---|---|---|
|VM (u oblaku *samostalne* )|Da|Da|Da|
|PaaS uloga instancu (u oblaku *samostalne* )|Da|ne|Da|
|VM ili PaaS uloga instancu (u VNet)|Da|Da|Da|
|Interna učitavanja opterećenja sučelje|Da|Da|Da|
|Aplikacija pristupnika sučelje|Da|Da|Da|

## <a name="limits"></a>Ograničenja

U sljedećoj tablici prikazane ograničenja zadana na IP adresiranja u Azure po pretplati. Možete se [obratiti službi za podršku](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) da biste povećali ograničenja zadani najviše Maksimalna ograničenja koja se temelji na svoje poslovne potrebe.

||Zadano ograničenje|Maksimalno ograničenje|
|---|---|---|
|Javnu IP adrese (dinamički)|5|Kontaktiranje službe za podršku|
|Rezervirane javnu IP adresa|20|Kontaktiranje službe za podršku|
|Javni VIP po implementacije (u oblaku)|5|Kontaktiranje službe za podršku|
|Privatni VIP (ILB) po implementacije (u oblaku)|1|1|

Provjerite je li čitati potpunog skupa Azure [ograničenjima mreže](azure-subscription-service-limits.md#networking-limits) .

## <a name="pricing"></a>Cijene

U većini slučajeva javnu IP adrese nisu besplatno. Postoji nominal trošak koristiti dodatne i/ili statične javnu IP adresa. Provjerite je li razumijevanje [cijene strukturu javnu IP-ovi](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Razlike između Voditelj resursa i klasični implementacije
Slijedi Usporedba značajki IP adresa u Voditelj resursa i model klasični implementacije.

||Resurs|Klasični|Voditelj resursa|
|---|---|---|---|
|**Javnu IP adresa**|VM|Se nazivaju ILPIP (samo za dinamički)|Se nazivaju javnu IP (dinamičnu ili statičnu)|
|||Dodijeljena programa VM IaaS ili instancu komponente PaaS uloga|Povezana SLIKA na VM|
||Dostupnog opterećenja Internet|Naziva VIP (dinamički) ili rezervirana IP (statičnu)|Se nazivaju javnu IP (dinamičnu ili statičnu)|
|||Dodijeljene servis u oblaku|Pridruženi raspoređivača opterećenja sučelje config|
||||
|**Privatna IP adresa**|VM|Se nazivaju u DIP|Se nazivaju privatne IP adrese|
|||Dodijeljena programa VM IaaS ili instancu komponente PaaS uloga|Dodijeljene se VM NIC|
||Interna opterećenja (ILB)|Dodijeljene ILB (dinamičnu ili statičnu)|Dodijeljene se ILB sučelje konfiguracije (dinamičke ili statičke)|

## <a name="next-steps"></a>Daljnji koraci
- [Uvođenje VM s statičke privatne IP adrese](virtual-networks-static-private-ip-classic-pportal.md) pomoću portala za klasični.
