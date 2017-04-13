<properties
   pageTitle="Pregled Azure virtualne mreže (VNet)"
   description="Saznajte više o virtualne mreže (VNets) u Azure."
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

# <a name="virtual-network-overview"></a>Pregled virtualne mreže

Azure virtualne mreže (VNet) je prikaz vlastite mreže u oblaku.  Je logički odvajanja Azure oblaka namjenski za svoju pretplatu. Potpuno možete kontrolirati na IP adresa blokovi, DNS postavke, sigurnosne pravilnike i usmjeravanje tablice u ovu mrežu. Možete i dalje fazi vaše VNet u podmreže i pokretanje Azure IaaS virtualnim strojevima (VMs) i/ili [servise u Oblaku (PaaS uloga instance)](../cloud-services/cloud-services-choose-me.md). Osim toga, možete se povezati virtualne mreže s lokalnim mrežom koristite neku od [mogućnosti povezivanja](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) dostupne Azure. Zapravo, mreže možete proširiti Azure, s potpunu kontrolu na IP adresa blokova uz prednost enterprise skaliranje nudi Azure.

Da biste bolje razumjeli VNets, pogledajte na slici u nastavku, koji pokazuje pojednostavljeni lokalnu mrežu.

![Lokalne mreže](./media/virtual-networks-overview/figure01.png)

Na slici u iznad prikazan je povezan s Internetom javno putem usmjerivača lokalne mreže. Možete pogledati i vatrozid između usmjerivača i DMZ hosting DNS poslužitelj i farme poslužitelja web. Farme poslužitelja web je rasporediti pomoću raspoređivača opterećenja hardvera koji je izložen putem Interneta, a troši resurse iz internog podmreže opterećenje. Interna podmreže je odvojene iz na DMZ neki drugi vatrozid i poslužitelji hosts kontroler domene Active Directory, poslužitelja baze podataka i poslužitelji aplikacija.

U Azure mogu nalaziti na istoj mreži kao što je prikazano na slici u nastavku.

![Azure virtualne mreže](./media/virtual-networks-overview/figure02.png)

Obratite pozornost na to kako Azure infrastrukture vodi na ulozi usmjerivač dopuštanja pristupa iz vaše VNet javnog interneta bez potrebe sve konfiguracije. Vatrozidima se mogu međusobno zamijeniti tako da primijeniti na svaki pojedinačne podmreže mreže sigurnosnih grupa (NSGs). I balancers fizičke učitavanja su zamijenjen putem Interneta dostupnog i interne učitavanja balancers u Azure.

>[AZURE.NOTE] Postoje dva načina uvođenja Azure: klasični (poznat i kao Upravljanje servisom) i Upravitelj Azure resursa (ARM). Klasični VNets nije moguće dodati u grupu afinitet ili stvoreni kao regionalne VNet. Ako imate na VNet u grupi afinitet, preporučuje se da biste [migrirali da regionalne VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>Prednosti virtualne mreže

- **Odvajanja**. VNets su potpuno Izolirani međusobno. Koji omogućuje stvaranje zasebnim mrežama za razvoj i testiranje radnog koje koriste isti CIDR adresni blokovi.

- **Pristup javnim Internet**. Sve IaaS VMs i PaaS instance uloga u VNet možete pristupiti javni Internet prema zadanim postavkama. Možete nadzirati pristup putem mreže sigurnosnih grupa (NSGs).

- **Pristup VMs unutar na VNet**. Instance PaaS uloga i IaaS VMs moguće pokrenuti u istom virtualne mreže i mogu povezati međusobno pomoću privatne IP adrese, čak i ako se ona nalazi u različitim podmreže bez potrebe za konfiguraciju pristupnika ili koristite javnoj IP adrese.

- **Razlučivanje naziva**. Azure nudi rješenje naziv internog IaaS VMs i PaaS uloga instance implementiran u vašem VNet. Implementacija DNS poslužitelja i konfiguriranje VNet ih koristiti.

- **Sigurnost**. Promet unos i izlazak iz virtualnim strojevima i PaaS uloga instance programa VNet možete kontrolirati pomoću mreže sigurnosne grupe.

- **Povezivanje**. VNets moguće je povezati međusobno putem mreže pristupnika ili VNet peering. VNets moguće je povezati lokalnih podataka centrima putem mreže VPN-a web-mjesto ili Azure ExpressRoute. Da biste saznali više o web-mjesto grupa podaci, posjetite [pristupnika o VPN-a](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Da biste saznali više o ExpressRoute, posjetite [ExpressRoute Tehnički pregled](../expressroute/expressroute-introduction.md). Da biste saznali više o VNet peering, posjetite [VNet peering](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Provjerite je li stvoriti na VNet prije no što implementirate IaaS VMs ni PaaS uloga instance okruženje za Azure. ARM temelji VMs potrebna je VNet, a ako ne navedete postojeće VNet, Azure stvara zadani VNet koje možda imaju clash Blok adresa CIDR s lokalne mreže. Zbog čega je moguće povezati svoje VNet lokalne mreže.

## <a name="subnets"></a>Podmreže

Podmreže je raspon IP adresa u na VNet, možete podijeliti na VNet u više podmreže za tvrtke ili ustanove i sigurnost. VMs i PaaS uloga instance implementiran na podmreže (istom ili drugom) unutar na VNet međusobno možete komunicirati bez sve dodatna konfiguracija. Možete konfigurirati i tablice usmjeravanja i NSGs da biste podmreži.

## <a name="ip-addresses"></a>IP adrese


Postoje dvije vrste IP adrese dodijeljene resursa u Azure: *javnim* i *privatnim*. Javnu IP adrese omogućuju Azure resursa možete komunicirati s internetske i drugih Azure dostupnog javnosti servisa kao što je [Azure Redis predmemorije](https://azure.microsoft.com/services/cache/), [Azure koncentratora za događaj](https://azure.microsoft.com/documentation/services/event-hubs/). Privatno IP adrese omogućuje komunikaciju između resursa u virtualne mreže, zajedno s onima povezivanje putem virtualne privatne MREŽE, bez korištenja Internet usmjeravati IP adrese.

Dodatne informacije o IP adresa u Azure, posjetite [IP adresa u virtualne mreže](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Balancers Azure učitavanja

Virtualnim strojevima i servise u oblaku u mreži virtualne možete izložiti interneta pomoću balancers Azure Učitaj. Poslovnim aplikacijama koje su vidljivim internim korisnicima samo može biti rasporediti pomoću internog opterećenja opterećenje.

- **Vanjski opterećenja**. Vanjski opterećenja možete koristiti da biste prikazali visoke dostupnosti IaaS VMs i PaaS instance uloge kojima se pristupa putem javnog Interneta.

- **Internog učitati opterećenja**. Interna opterećenja možete koristiti da biste prikazali visoke dostupnosti IaaS VMs i PaaS uloga instance pristupiti iz drugih servisa u vašem VNet.

Da biste saznali više o servisu Azure za ujednačavanje opterećenja, posjetite [učitati opterećenja pregled](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Mrežni sigurnosne grupe (NSG)

Možete stvoriti NSGs za kontrolu pristupa ulazni i izlazni mrežom sučelja (NIC-ovi), VMs i podmreže. Svaki NSG sadrži jednu ili više pravila vrijednost koja određuje hoće li promet odobrene ili zabranjen na temelju IP adresu izvora, izvorni Priključak, odredišni IP adresa i odredišni priključak. Da biste saznali više o NSGs, posjetite [što je mreža sigurnosne grupe](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtualna aparata

Virtualna potražite je samo drugi VM u vašem VNet koja se pokreće softver koji se temelje potražite funkcija, kao što je vatrozid, WAN optimizacije ili otkrivanje podataka. Stvorite rutu Azure da biste usmjerili promet za svoju VNet putem virtualne uređaj da biste koristili njegove mogućnosti.

Na primjer, NSGs može se koristiti za navođenje sigurnosti na vašem VNet. No NSGs nude sloja 4 popis kontrola na Access (ACL) da biste dolazne i odlazne pakete. Ako želite koristiti 7 sigurnosnih modela sloja trebate koristiti vatrozid uređaj.

Virtualna aparata ovise o [korisnički definirane usmjerava i IP prosljeđivanje](virtual-networks-udr-overview.md).

## <a name="limits"></a>Ograničenja
Nema ograničenja broja virtualne mreže dopuštena za pretplatu, pogledajte [Azure umrežavanje ograničenja](../azure-subscription-service-limits.md#networking-limits) dodatne informacije.

## <a name="pricing"></a>Cijene
Došlo je vrlo ne troška za korištenje virtualne mreže u Azure. Instance računalnim pokretanja unutar na Vnet naplatiti standardne stope kao što je opisano u [Azure VM cijene](https://azure.microsoft.com/pricing/details/virtual-machines/). [VPN pristupnika](https://azure.microsoft.com/pricing/details/vpn-gateway/) i [javnu IP adrese] (https://azure.microsoft.com/pricing/details/ip-addresses/) koristi u na VNet će biti naplaćena standardne stope.

## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje na VNet](virtual-networks-create-vnet-arm-pportal.md) i podmreže.
- [Stvaranje VM u na VNet](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Saznajte više o [NSGs](virtual-networks-nsg.md).
- Saznajte više o [korisnički definirane usmjerava i IP prosljeđivanje](virtual-networks-udr-overview.md).
