<properties
   pageTitle="Javne i privatne IP adresiranja u Azure Voditelj resursa | Microsoft Azure"
   description="Dodatne informacije o javnim i privatnim IP adresiranja u Azure Voditelj resursa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>IP adrese u Azure
IP adrese možete dodijeliti Azure resursa možete komunicirati s ostalim resursima Azure, lokalne mreže i s Internetom. Postoje dvije vrste IP adresa u Azure možete koristiti:

- **Javnu IP adrese**: koristi za komunikaciju s Internetom, uključujući servisa Azure dostupnog javnosti
- **Privatno IP adrese**: koristi za komunikaciju unutar Azure virtualne mreže (VNet), a vaše lokalne mreže kada koristite pristupnika VPN-a ili ExpressRoute elektronička da biste proširili mreže za Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](virtual-network-ip-addresses-overview-classic.md).

Ako ste upoznati s modelom klasični implementaciju, potvrdite okvir na [razlike u IP adresama između klasični i resursima](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Javnu IP adresa
Javnu IP adrese omogućuju Azure resursa možete komunicirati s Internetom i Azure dostupnog javnosti servise kao što je [Azure Redis predmemorije](https://azure.microsoft.com/services/cache/), [Azure događaj koncentratora](https://azure.microsoft.com/services/event-hubs/), [baze podataka SQL](../sql-database/sql-database-technical-overview.md)i [Azure prostora za pohranu](../storage/storage-introduction.md).

U upravitelju Azure resursa, [javnu IP](resource-groups-networking.md#public-ip-address) adresa je resurs koji ima vlastitu svojstva. Možete pridružiti javno resursa IP adresa bilo koji od sljedećih resursa:

- Virtualnim strojevima (VM)
- Balancers učitavanja mjesto na Internetu
- Pristupnika VPN-a
- Aplikacija pristupnika

### <a name="allocation-method"></a>Način dodjele
U kojem je IP adresa je dodijeljen *javnu* IP resurs - *dinamičke* ili *statičke*na dva načina. Zadani način dodijeljeni je *dinamički*, gdje je IP adresa **nije** dodijeljeno prilikom njegova stvaranja. Umjesto toga na javnu IP adresu je dodijeliti kada pokrenete (ili stvaranje) povezane resursa (kao što je VM ili učitavanje raspoređivača). IP adresa je objavio kada Zaustavi (ili brisanje) resursa. Zbog toga IP adresa da biste promijenili kada zaustaviti i pokrenuti resurs.

Da biste osigurali IP adresa za povezane resursa ostaje, možete postaviti način dodjele izričito u *statični*. U ovom slučaju IP adresu dodjeljuje odmah. Objavljivanja samo kada je izbrišite resursa ili promijeniti njegov način dodjele *dinamičke*.

>[AZURE.NOTE] Čak i kada postavite način dodjele *statične*, ne možete navesti stvarni IP adresa dodijeljena *javnu IP resursa*. Umjesto toga ga može vidjeti dodijeliti iz skupa dostupna IP adrese Azure mjestu resurs je stvoren u.

Statičke IP adrese javnog najčešće koriste se u sljedećim scenarijima:

- Krajnji korisnici morate ažurirati pravila vatrozida za komunikaciju s Azure resurse.
- Razrješavanje DNS naziva, gdje je promjene u IP adresa potrebna ažuriranje na zapisa.
- Resursi za Azure komunicirati s druge aplikacije ili servise koje koristite s modelom IP adresa temeljenu na.
- Korištenje SSL certifikata koji su povezani s IP adresom.

>[AZURE.NOTE] Na popisu IP rasponi iz kojeg se javnu IP adrese (dinamičkih na statički) dodijeliti Azure resursi se objavljuje na [Azure podatkovnog centra IP raspone](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>Razrješavanje DNS naziv glavnog računala
Možete navesti oznaku naziv domene DNS-a za javno IP resursa, čime se mapiranje za *domainnamelabel*. *lokacija*. cloudapp.azure.com na javnu IP adresu u Azure upravlja DNS poslužitelji. Na primjer, ako ste stvorili javnu IP resursa pomoću **contoso** kao *domainnamelabel* u **SAD -a Zapad** Azure *mjesto*, naziv (FQDN) u potpunosti kvalificirana domene **contoso.westus.cloudapp.azure.com** će riješiti na javnu IP adresu resursa. U ovom FQDN možete koristiti da biste stvorili prilagođenu domenu CNAME zapis koja pokazuje na javnu IP adresa u Azure.

>[AZURE.IMPORTANT] Oznake naziva domene stvorili mora biti jedinstvena u položaju Azure.  

### <a name="virtual-machines"></a>Virtualnim strojevima
Javna IP adresa možete pridružiti sa [sustavom Windows](../virtual-machines/virtual-machines-windows-about.md) ili [Linux](../virtual-machines/virtual-machines-linux-about.md) VM dodjeljivanjem **sučelje mreže**. Ako više mrežno sučelje VM možete dodijeliti je *primarni* mrežnog sučelja samo. Dinamičnu ili statičke javnu IP adrese možete dodijeliti u VM.

### <a name="internet-facing-load-balancers"></a>Balancers učitavanja mjesto na Internetu
Javna IP adresa možete pridružiti programa [Azure opterećenja](../load-balancer/load-balancer-overview.md)dodjelom **sučelju** konfiguracije raspoređivača opterećenja. Javnu IP adresu služi kao uravnoteženja virtualne IP adresa (VIP). Dinamičnu ili statičke javnu IP adrese možete dodijeliti raspoređivača opterećenja sučelja. Da biste na opterećenja sučelja, koja omogućuje [Višestruki VIP](../load-balancer/load-balancer-multivip.md) scenariji kao što su okruženju više klijentu s operacijskim sustavom SSL web-mjesta možete dodijeliti i više javnu IP adresa.

### <a name="vpn-gateways"></a>Pristupnika VPN-a
[Pristupnik za VPN Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md) se koristi za povezivanje Azure virtualne mreže (VNet) da biste druge Azure VNets ili lokalne mreže. Trebate dodijeliti javnu IP adresu na **IP konfiguracija** da ga možete komunicirati s daljinski mrežni Omogući. Trenutno možete dodijeliti *dinamički* javnu IP adresu samo pristupnik VPN-a.

### <a name="application-gateways"></a>Aplikacija pristupnika
Javna IP adresa možete pridružiti programa Azure [Aplikacije pristupnika](../application-gateway/application-gateway-introduction.md)dodjelom konfiguraciju pristupnika u **sučelju** . Javnu IP adresu služi kao VIP za uravnoteženja. Trenutno možete dodijeliti *dinamički* javnu IP adresu samo na sučelju konfiguracije pristupnika za aplikacije.

### <a name="at-a-glance"></a>Pri kratak
U sljedećoj tablici prikazane svojstvo određene kroz koje mogu se povezati najviše razine resursa i načine moguće dodijeljeni (dinamičke ili statičke) koje je moguće koristiti javnu IP adresa.

|Najviše razine resursa|Pridruživanje IP adresa|Dinamični|Statički|
|---|---|---|---|
|Virtualnog računala|Mrežno sučelje|Da|Da|
|Opterećenja|Konfiguriranje sučelje|Da|Da|
|Pristupnik za VPN-a|Pristupnik IP konfiguracija|Da|ne|
|Pristupnik za aplikaciju|Konfiguriranje sučelje|Da|ne|

## <a name="private-ip-addresses"></a>Privatno IP adresa
Privatno IP adrese omogućuju Azure resursa možete komunicirati s drugih resursa u [virtualne mreže](virtual-networks-overview.md) ili lokalne mreže putem VPN pristupnika ili elektronička ExpressRoute bez korištenja Internet dostupna IP adresa.

U model za uvođenje Voditelj resursa Azure privatne IP adrese je povezan sa sljedećim vrstama Azure resursa:

- VMs
- Interna učitavanja balancers (ILBs)
- Aplikacija pristupnika

### <a name="allocation-method"></a>Način dodjele
Privatne IP adrese Dodijeljeno iz raspona adresa podmreže koji je pridružen resursa. Adresa raspon podmreže sam je dio na VNet adresni raspon.

Postoje dva načina u kojem je dodijeliti privatne IP adrese: *dinamičke* ili *statičke*. Zadani način dodijeljeni je *dinamički*, gdje se s IP adresom automatski dodijeliti iz podmreže resursa (pomoću DHCP). U ovom IP adresa možete promijeniti kada zaustaviti i pokrenuti resurs.

Način dodjele možete postaviti u *statički* da biste bili sigurni IP adresa ostaje isto. U ovom slučaju morate unijeti valjanu IP adresu koja je dio podmreže resursa.

Statičke IP adrese privatne najčešće koriste se za:

- VMs funkcionirati kao kontrolera domene ili DNS poslužitelji.
- Resursi koji zahtijevaju pravila vatrozida pomoću IP adresa.
- Resursi pristupiti pomoću drugih aplikacija/resursa putem IP adresa.

### <a name="virtual-machines"></a>Virtualnim strojevima
**Mrežno sučelje** sustava [Windows](../virtual-machines/virtual-machines-windows-about.md) ili [Linux](../virtual-machines/virtual-machines-linux-about.md) VM je dodijeljen privatne IP adrese. U slučaju više mrežno sučelje VM svaki sučelja dohvaća privatne IP adrese dodijeljeni. Možete odrediti način dodjele kao dinamičke ili statičke za mrežno sučelje.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Interni DNS naziv glavnog računala razlučivost (VMs)
Sve VMs Azure konfiguriranih s [Azure upravlja DNS poslužitelji](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) prema zadanim postavkama, osim ako izričito konfiguriranje prilagođeni DNS poslužitelji. Ove DNS poslužitelji Navedite naziv internog rješenja za VMs koje se nalaze u istom VNet.

Kada stvorite na VM, mapiranja za naziv glavnog računala na njegovu privatne IP adresu dodaje se Azure upravlja DNS poslužitelji. U slučaju više mrežno sučelje VM privatne IP adrese primarni mrežnog sučelja je mapirati glavnog računala.

VMs konfiguriran pomoću Azure upravlja DNS poslužitelji moći da se raščlanjuje hostnames sve VMs unutar svoje VNet privatne IP adrese.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interna učitavanja balancers (ILB) i aplikacije pristupnika
Konfiguracija **sučelje** programa [Azure interne raspoređivača opterećenja](../load-balancer/load-balancer-internal-overview.md) (ILB) ili [Azure aplikacije pristupnika](../application-gateway/application-gateway-introduction.md)možete dodijeliti privatne IP adrese. Privatne IP adrese služi kao interne krajnje, dostupna samo za resurse unutar virtualne mrežom (VNet) i udaljene mreže povezani s VNet. Konfiguriranje sučelje možete dodijeliti ili na dinamičke ili statičke privatne IP adrese.

### <a name="at-a-glance"></a>Pri kratak
U sljedećoj tablici prikazane određenim svojstvo kroz koje mogu se povezati najviše razine resursa i načine moguće dodijeljeni (dinamičke ili statičke) koje je moguće koristiti privatne IP adrese.

|Najviše razine resursa|Pridruživanje IP adresa|Dinamični|Statički|
|---|---|---|---|
|Virtualnog računala|Mrežno sučelje|Da|Da|
|Opterećenja|Konfiguriranje sučelje|Da|Da|
|Pristupnik za aplikaciju|Konfiguriranje sučelje|Da|Da|

## <a name="limits"></a>Ograničenja

Ograničenja zadana na IP adresiranja označene su u potpunog skupa Azure [ograničenjima mreže](azure-subscription-service-limits.md#networking-limits) . Ta ograničenja su po regijama i po pretplati. Možete se [obratiti službi za podršku](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) da biste povećali ograničenja zadani najviše Maksimalna ograničenja koja se temelji na svoje poslovne potrebe.

## <a name="pricing"></a>Cijene

Javnu IP adrese možda nominal trošak. Dodatne informacije o IP adresi cijene u Azure, pregledajte stranicu [cijene za IP adresa](https://azure.microsoft.com/pricing/details/ip-addresses) .

## <a name="next-steps"></a>Daljnji koraci
- [Implementacija VM pomoću statičke IP javno pomoću portala za Azure](virtual-network-deploy-static-pip-arm-portal.md)
- [Implementacija VM pomoću statičke IP javno pomoću predloška](virtual-network-deploy-static-pip-arm-template.md)
- [Implementacija u VM pomoću statičke privatne IP adrese pomoću portala za Azure](virtual-networks-static-private-ip-arm-pportal.md)
