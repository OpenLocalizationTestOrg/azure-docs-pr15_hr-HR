<properties 
   pageTitle="Migriranje iz grupe afinitet regionalne virtualne mrežom (VNet)"
   description="Saznajte kako migrirati iz grupe afinitet u regionalnim vnets"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Migriranje iz grupe afinitet Regional virtualne mrežom (VNet)

Grupe sustava afiniteta možete koristiti da biste bili sigurni da su fizički resursa stvorene u istoj grupi afinitet hostira tvrtka poslužitelje koji su bliže, omogućivanje ove resurse za komunikaciju brže. U prošlosti, afinitet grupe su potrebna za stvaranje virtualne mreže (VNets). Trenutku, mrežni servis Upravitelj kojim upravlja VNets nije moguće raditi samo, unutar skupa fizičke poslužitelja ili skaliranje jedinica. Arhitektonski poboljšanja ste povećati djelokrug mreži upravljanje u područje.

Kao rezultat ove arhitektonski poboljšanja afinitet grupe se više ne preporučuje ili potrebne za virtualne mreže. Korištenje afinitet grupe za VNets je zamijenjena po regijama. VNets koje su vezane uz područja nazivaju regionalne VNets.

Uz to, preporučujemo da ne koristite afinitet grupe Općenito. Osim preduvjeta VNet afinitet grupe i su važne koristite da biste bili sigurni resurse, kao što su računalnim i prostor za pohranu, bile smještene pri međusobno povezani. Međutim, s trenutnom arhitektura Azure mreže te preduvjete položaj više nisu potrebni. Potražite [afinitet grupe i VMs](#Affinity-groups-and-VMs) na nekoliko preostale nekim slučajevima gdje trebali biste koristiti grupe sustava afinitet.

## <a name="creating-and-migrating-to-regional-vnets"></a>Stvaranje i migriranje regionalne VNets

Odsad nadalje, prilikom stvaranja novog VNets, koristite *regija*. Primijetit ćete sljedeće kao mogućnost na portalu za upravljanje. Imajte na umu da u konfiguracijskoj datoteci mreže prikazuje kao *mjesto*.

>[AZURE.IMPORTANT] Iako je i dalje Tehnički moguće stvoriti virtualne mreže koja je povezana s grupom afinitet postoji nema dovoljno dobar razlog da biste to učinili. Mnogim novim značajkama, kao što su mreže sigurnosnih grupa, dostupne su samo kada se koristi regionalne VNet i nisu dostupne za virtualne mreže koje su vezane uz afinitet grupe.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>O VNets trenutno pridruženi afinitet grupe

VNets koji su trenutno povezani s grupama afinitet omogućena za migraciju u regionalnim VNets. Da biste migrirali regionalne VNet, slijedite ove korake:

1. Izvoz konfiguracije datoteke mreže. Možete koristiti PowerShell ili Portal za upravljanje. Upute za pomoću portala za upravljanje potražite u članku [Konfiguriranje vaše VNet pomoću datoteke konfiguracije mreže](virtual-networks-using-network-configuration-file.md).

1. Uređivanje konfiguracijskoj datoteci mreže, zamjenjuje stari vrijednosti nove vrijednosti. 

    > [AZURE.NOTE] **Mjesto** je regiju koju ste naveli za grupu afinitet koji je povezan s vašeg VNet. Na primjer, ako je vaš VNet povezana s grupom afinitet koja se nalazi u Zapad sad, prilikom migracije, lokacije moraju pokazivati Zapad SAD-a. 
    
    Uređivanje sadržan u konfiguracijskoj datoteci mreže, zamjenu vrijednosti s vlastitim: 

    **Staru vrijednost:** \<VirtualNetworkSitename = AffinityGroup "VNetUSWest" = "VNetDemoAG"\> 

    **Novu vrijednost:** \<VirtualNetworkSitename = mjesto "VNetUSWest" = "Zapad sad"\>

1. Spremite promjene i [Uvoz](virtual-networks-using-network-configuration-file.md) mrežna konfiguracija Azure.

>[AZURE.NOTE] U ovom migracije prouzročiti sve nedostupnost servisa.

## <a name="affinity-groups-and-vms"></a>Afinitet grupe i VMs

Kao što je već rečeno, grupe afinitet preporučuje se više ne obično za VMs. Grupe sustava afinitet trebali biste koristiti samo kad skup VMs mora imati apsolutne najniže mreže latencije između na VMs. Potvrđivanjem VMs u grupi afinitet na VMs će sve nalaziti u istoj računalni klaster ili skala jedinici.

Važno Imajte na umu da pomoću programa afinitet grupe možete imati dvije, vjerojatno negativan učinak je:

- Postavljanje veličine VM bit će ograničeni na postavljanje veličine VM nudi po jedinici skaliranje računalnim.

- Postoji veća vjerojatnost nemogućnošću dodijeliti novi VM. To se događa kada je jedinica određene skaliranje afinitet grupe iz kapaciteta.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Što učiniti ako ste na VM u grupi afinitet

VMs koji su trenutno u grupi afinitet ne moraju biti uklonjeni iz grupe afinitet.

Kada je implementiran u VM, je uveden u jednom skaliranje jedinicu. Afinitet grupe mogu ograničiti skupove veličine dostupne VM novi VM implementacije, ali bilo koje postojeće VM koja je postavila već je ograničeno na skup VM veličine dostupne u skaliranje jedinice u kojima je implementiran u VM. Zbog toga u VM uklanjanjem grupi afinitet neće imati utjecaja.
 
