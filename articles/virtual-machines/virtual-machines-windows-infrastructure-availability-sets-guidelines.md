<properties
    pageTitle="Upute za postavljanje dostupnosti | Microsoft Azure"
    description="Saznajte više o ključa dizajna i implementaciju smjernice za implementaciju skupove dostupnost servisa Azure Infrastruktura sustava."
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

# <a name="availability-sets-guidelines"></a>Dostupnost postavlja smjernice

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

U ovom se članku usredotočuje se na objašnjenje potrebne korake za planiranje za skupove dostupnost da biste bili sigurni aplikacije ostaju dostupna tijekom planiranog ili neplanirano događaja.

## <a name="implementation-guidelines-for-availability-sets"></a>Smjernice za implementaciju za skupove dostupnosti

Odluka:

- Koliko dostupnost skupovima vam je potrebno za različite uloge i razine u sustavu aplikacije?

Zadaci:

- Definiranje broj VMs u svakom razina aplikacije ako su vam potrebne.
- Utvrđivanje potrebe za podesiti broj kvara ili ažuriranje domene koja će se koristiti za svoju aplikaciju.
- Definiranje potrebna dostupnosti skupa pomoću konvencije imenovanja i što VMs nalaze se u njima. Na VM samo mogu se nalaziti u jednom dostupnost postavljanje. 

## <a name="availability-sets"></a>Skupovi dostupnosti

U Azure, virtualnim strojevima (VMs) možete staviti u za logičke grupiranja naziva skupa dostupnost. Kada stvorite VMs unutar skupa dostupnost, Azure platforme distribuira položaj te VMs preko podlozi infrastrukture. Mora postojati planiranom održavanju platforme Azure ili podlozi hardver / infrastrukture kvara, korištenje skupova dostupnost osigurava ostaje li barem jedan VM izvodi.

Kao preporučenim načinom rada aplikacije treba nalazi se na jednom VM. Skup dostupnost koja sadrži jedan VM ne dobiti sve Zaštita od planirane ili neplanirano događaje unutar Azure platforme. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) potrebna dva ili više VMs unutar raspoloživost dopustiti distribucija VMs preko podlozi infrastrukture.

Temeljni infrastrukture u Azure je podijeljen u da biste ažurirali domene i kvara domene. Ove domene definiraju tako što domaćini zajedničko korištenje uobičajenih ciklusa ažuriranje ili zajedničko korištenje slične fizičke infrastrukture kao što su power i povezivanje s mrežom. Azure automatski distribuira vaše VMs unutar raspoloživost postavite svim domenama da biste zadržali dostupnosti i odstupanje kvara. Ovisno o veličini vaše aplikacije i broj VMs unutar skupa dostupnost, možete podesiti broj domene koju želite koristiti. Dodatne informacije o [upravljanju dostupnosti i korištenje ažuriranja i kvara domene](virtual-machines-windows-manage-availability.md).

Prilikom dizajniranja infrastruktura za aplikaciju, trebali biste planiranje i razine aplikacije koju koristite. Grupa VMs koji vam poslužiti dostupan u istu svrhu u postavlja se kao što je postavljena za vaš sučelja VMs pokrenuti IIS raspoloživost. Stvorite zaseban dostupnost postavljena za vaš VMs pozadinsku izvodi SQL Server. Cilj je da biste bili sigurni da svaki dio aplikacije zaštićen skupa dostupnosti i barem jednom instancu uvijek ostaje izvodi.

Učitavanje balancers koristi se ispred svakog razina aplikacije za rad uz skupa dostupnosti i provjerite je li promet uvijek moguće usmjeriti pokrenute instance. Bez raspoređivača opterećenja sustava VMs nastaviti pokretanje cijeloj planirane i Neplanirana održavanja događaja, ali krajnji korisnici možda nećete moći riješiti ih ako primarni VM nije dostupan.


## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 