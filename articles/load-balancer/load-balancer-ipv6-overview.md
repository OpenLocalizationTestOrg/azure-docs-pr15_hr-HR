<properties
    pageTitle="Pregled IPv6 za Azure opterećenja | Microsoft Azure"
    description="Objašnjenje podrška za IPv6 za Azure raspoređivača opterećenja i uravnoteženja VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, azure opterećenja, dvostruki snop, javnu ip, izvorni ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Pregled IPv6 za Azure opterećenja

Mjesto na Internetu učitavanja balancers uvode se sa IPv6 adrese. Osim Povezivost putem IPv4 to omogućuje sljedeće mogućnosti:

* Izvorni završetka do kraja IPv6 veze između javno Internet klijenata i virtualnim računalima sustava Azure (VMs) do raspoređivača opterećenja.
* Izvorni završetka do kraja izlaznog povezivanje putem protokola IPv6 između VMs i javni Internet IPv6 omogućen klijente.

Sljedeća slika prikazuje funkciju IPv6 za Azure raspoređivača opterećenja.

![Azure opterećenja s IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Kada implementiran, klijent za IPv4 ili IPv6 omogućen Internet mogu komunicirati s javnim IPv4 ili IPv6 adrese (ili hostnames) od opterećenja mjesto na Internetu Azure. Raspoređivača opterećenja preusmjerava pakete IPv6 privatne IPv6 adrese VMs korištenju prevođenja mrežnih adresa (NAT). Klijenta za IPv6 internetskih komunikacija izravno s IPv6 adrese na VMs.

## <a name="features"></a>Značajke

Ugrađena podrška za IPv6 za VMs implementiran putem upravitelja za Azure resursa omogućuje:

1. Uravnoteženja IPv6 servise za IPv6 klijente na Internetu
2. Izvorni IPv6 i IPv4 krajnje točke na VMs ("lišće složeni")
3. Ulazni i izlazni pokrenut nativni IPv6 veze
4. Podržani protokoli kao što su TCP i UDP HTTP (S) Omogućivanje širok raspon arhitekturi servisa

## <a name="benefits"></a>Prednosti

Ta je funkcija omogućuje sljedeće ključne prednosti:

* Upoznavanje pravilnicima za državne ustanove koje je obavezna da nova aplikacija biti dostupna samo za IPv6 klijenti
* Omogući mobile i internetske stvari (IOT) razvojnim inženjerima lišće složeni (IPv4 + IPv6) virtualnim računalima sustava Azure adrese sve veći mobile i tržištima IOT

## <a name="details-and-limitations"></a>Detalji i ograničenja

Pojedinosti

* Azure DNS servis sadrži odgovora IPv4 i IPv6 AAAA zapisa naziva i Odgovori s oba zapisa za raspoređivača opterećenja. Klijent odabire koju adresu (IPv4 ili IPv6) u komunikaciju.
* Kada je VM pokreće vezu javni uređaj povezan s Internetom IPv6, mrežne adrese prevesti (NAT) javno IPv6 address raspoređivača opterećenja je na VM izvor IPv6 adrese.
* Da biste primali IPv6 IP adrese putem DHCP mora biti konfigurirano VMs operacijski sustav Linux. Mnoge Linux slike u galeriji Azure već konfigurirani tako da podržava IPv6 bez izmjene. Dodatne informacije potražite u članku [Konfiguriranje DHCPv6 za Linux VMs](load-balancer-ipv6-for-linux.md)
* Ako se odlučite za uporabu stanja probni raspoređivača opterećenja, stvorite probni na IPv4 i koristiti s IPv4 i IPv6 krajnje točke. Ako funkcionira servis sustava VM, krajnje točke IPv4 i IPv6 uzimaju se iz rotacije.

Ograničenja

* Na portalu za Azure ne možete dodati pravila za ujednačavanje opterećenja IPv6. Pravila moguće je stvoriti samo putem predložak, EŽA, PowerShell.
* Ne može nadograditi postojeće VMs koristiti IPv6 adrese. Morate uvesti nove VMs.
* Jedan IPv6 address se mogu dodijeliti jedne mreže sučelja u svakom VM.
* IPv6 adrese javnog ne može biti dodijeljena na VM. Ih možete samo dodijeliti raspoređivača opterećenja.
* Ne možete konfigurirati obrnuto pretraživanje DNS-a za javno IPv6 adrese.
* VMs s IPv6 adrese ne mogu biti članovi programa Azure u Oblaku. Može se povezati s mrežom virtualne Azure (VNet) i komunikaciju putem njihove IPv4 adrese.
* Privatna IPv6 adrese može uvesti na pojedinačne VMs u grupu resursa, ali ne može biti implementirano u grupu resursa putem skaliranje skupove.
* Azure VMs ne može povezati putem IPv6 s drugim VMs, drugim servisa Azure ili lokalnim uređajima. Mogli samo komunicirati s Azure opterećenja putem protokola IPv6. Međutim, mogli komunicirati sa sljedećim resursima pomoću IPv4.
* Zaštita mreže sigurnosne grupe (NSG) za IPv4 podržana u stogu lišće (IPv4 + IPv6) implementacijama. NSGs ne primjenjuju se na krajnje točke IPv6.
* IPv6 krajnjoj točki na VM ne prikazuje se izravno s Internetom. Preporučuje se iza raspoređivača opterećenja. Samo priključke naveden u pravila raspoređivača opterećenja može se pristupiti putem protokola IPv6.
* Promjena parametara IdleTimeout za IPv6 je **trenutno nije podržano**. Zadano je četiri minute.

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako implementirati opterećenja s IPv6.

* [Raspoloživost IPv6 po regijama](https://go.microsoft.com/fwlink/?linkid=828357)
* [Implementacija raspoređivača opterećenja s IPv6 pomoću predloška](load-balancer-ipv6-internet-template.md)
* [Implementacija raspoređivača opterećenja s IPv6 pomoću komponente PowerShell Azure](load-balancer-ipv6-internet-ps.md)
* [Implementacija raspoređivača opterećenja s IPv6 pomoću EŽA Azure](load-balancer-ipv6-internet-cli.md)
