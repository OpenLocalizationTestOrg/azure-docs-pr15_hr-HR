<properties 
   pageTitle="Mrežni sučelja | Microsoft Azure"
   description="Saznajte više o sučelje Azure mreže u Azure Voditelj resursa."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Sučelje mreže

Mrežnog sučelja (NIC) je interconnection između programa virtualnog računala (VM) i softver mreže. U ovom se članku objašnjava što mrežno sučelje je i kako se koristi u modelu implementaciju upravljanja resursima Azure.

Microsoft preporučuje implementacija nove resurse pomoću modela za implementaciju upravljanja resursima, ali možete i implementirati VMs s veza s mrežom u [Klasični](virtual-network-ip-addresses-overview-classic.md) model implementacije. Ako ste upoznati s klasični model, postoje važne razlike u VM mreže u modelu implementaciju upravljanja resursima. Dodatne informacije o razlikama između tako da pročitate članak [virtualnog računala umrežavanje - klasični](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) .

U Azure, mrežnog sučelja:

1. Resurs koji se može stvoriti, izbrisati i imaju vlastiti konfigurirati postavke.
2. Morate biti povezani s jednog podmreže u jedan Azure virtualne mreže (VNet) prilikom stvaranja. Ako niste upoznati s VNets, Saznajte više o ih tako da pročitate članak [Pregled virtualne mreže](virtual-networks-overview.md) . NIC-a mora biti povezano s VNet koji postoji u istoj Azure [mjesto](https://azure.microsoft.com/regions) i [pretplate](../azure-glossary-cloud-terminology.md#subscription) kao u NIC. Nakon stvaranja na NIC možete promijeniti podmreže je povezano s, ali ne možete promijeniti VNet je povezano s.
3. Naziv dodijelio koje nije moguće promijeniti nakon stvaranja NIC-a. Naziv mora biti jedinstvena u Azure [grupa resursa](../azure-resource-manager/resource-group-overview.md#resource-groups), ali ne mora biti jedinstvena u pretplatu, mjesto Azure stvara se u ili VNet je povezano s. NIC-ovi nekoliko obično se stvaraju unutar Azure pretplate. Preporučuje se da razviju konvencija imenovanja koji omogućuje upravljanje više NIC-ovi lakše nego pomoću zadane nazive. Potražite u članku [preporučeni konvencije imenovanja za Azure resurse](../guidance/guidance-naming-conventions.md) za prijedloge.
4. Može se priložiti u VM, ali se može priložiti samo jedan VM koji postoji u na isto mjesto na NIC.
5. Ima MAC adresu koja je ista i s NIC-a za sve dok se ona ostaje priloženi na VM. MAC adresa je ista i hoće li se pokrene u VM (iz unutar operacijski sustav) ili zaustavljen (deaktivirali dodijeljene) i pomoću portala za Azure, Azure PowerShell ili Azure sučelja naredbenog retka. Ako je ima odvojene iz programa VM i priložiti različite VM, NIC-a prima različite MAC adresa. Ako je izbrisana NIC-a, MAC adresa je dodijeljen druge mrežne kartice.
6. Potrebno je jednu primarnu **privatne** *IPv4* statički ili dinamički IP adresu dodijeliti na njega.
8. Možda ste jedan javnu IP adresa resurs povezali s njim.
9. Podržava ubrzana mreže s jednim korijenski/i virtualizacije (SR IOV) za određene veličine VM koji se izvodi određenih verzija operacijskog sustava Microsoft Windows Server. Da biste saznali više o toj značajci PRETPREGLED, pročitajte članak [Accelerated umrežavanje za virtualnog računala](virtual-network-accelerated-networking-powershell.md) .
10. Možete primati promet nisu namijenjene prikazivanju da biste joj dodijelili ako je prosljeđivanje IP omogućen u NIC. privatne IP adrese Ako je VM pokrenut vatrozid, primjerice, usmjerava pakete nisu namijenjene prikazivanju na vlastitu IP adrese. Pokrenite i dalje u VM softvera instaliranog usmjeravanje ili prosljeđivanje promet, ali da biste to učinili, prosljeđivanje IP mora biti omogućen za na NIC.
11. Često stvoriti u istoj grupi resursa kao VM je pridružen ili isti VNet koji je povezan s, iako nije obavezna biti.

## <a name="vms-with-multiple-network-interfaces"></a>VMs s više mreža sučelja

NIC-ovi više mogu priložiti u VM, ali kada to učinite, imajte na umu sljedeće:  

- Veličina VM mora podržavati više NIC-ovi. Da biste saznali više o veličinama VM podržava više NIC-ovi, pročitajte članke [Windows Server VM veličine](../virtual-machines/virtual-machines-windows-sizes.md) ili [veličine Linux VM](../virtual-machines/virtual-machines-linux-sizes.md) .   
- Na VM moraju se stvoriti s najmanje dva NIC-ovi. Ako na VM stvara se pomoću samo jedan NIC, čak i ako je veličina VM podržava više od jedne, ne možete priložiti dodatne NIC-ovi u VM nakon stvaranja. Pod uvjetom da se VM stvorena je pomoću najmanje dva NIC-ovi, možete priložiti dodatne NIC-ovi u VM nakon stvaranja, pod uvjetom da veličina VM podržava više od dva NIC-ovi.  
- Iz programa VM možete odvojiti sekundarne NIC-ovi (primarni NIC ne može biti odvojene) ako na VM ima najmanje tri NIC-ovi pridružen. NIC-ovi nije moguće odvojiti ako na VM ima dvije ili manje NIC-ovi priključen na njega.  
- Redoslijed mrežne kartice iz unutar na VM bit će slučajni, a može promijeniti i preko Azure infrastrukture ažuriranja. Međutim, IP adresa i odgovarajuće ethernet MAC adrese, ostati isto. Ako, na primjer, pretpostavimo operacijski sustav prepoznaje Azure NIC1 kao Eth1. Eth1 je IP adresa 10.1.0.100 i MAC adresa 00-0D-3A-B0-39-0D. Nakon Azure infrastrukture ažurirajte i ponovno, operacijski sustav sada otkriti Azure NIC1 kao Eth2, ali IP i MAC adrese će biti isti kao što su kada operacijski sustav označena Azure NIC1 kao Eth1. Kada je pokrenut klijenta, redoslijed NIC će ostati u operacijski sustav.  
- Ako je na VM član za [Postavljanje dostupnosti](../azure-glossary-cloud-terminology.md#availability-set), sve VMs unutar skupa dostupnost mora imati jedan GUMB ili više NIC-ovi. Ako na VMs više NIC-ovi, broj svaki imaju nije potrebno imati isti, pod uvjetom da svaki VM ima najmanje dva NIC-ovi.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako stvoriti na VM s jednom NIC tako da pročitate članak [Stvaranje na VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Saznajte kako stvoriti na VM s više NIC-ovi tako da pročitate članak [uvođenja VM s više SLIKA](virtual-network-deploy-multinic-arm-ps.md) .
- Saznajte kako stvoriti na NIC s više IP konfiguracijama tako da pročitate članak [više IP adresa za Azure virtualnih računala](virtual-network-multiple-ip-addresses-powershell.md) .
