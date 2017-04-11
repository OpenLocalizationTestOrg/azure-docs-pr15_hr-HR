<properties
 pageTitle="O računalnim ćete morati usko VMs sa sustavom Windows | Microsoft Azure"
 description="Dodatne informacije i pitanja vezana uz za korištenje Azure veličine računalnim ćete morati usko H niza i A8, A9, A10 i A11 za servise sustava Windows VMs i oblaka"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>O H niza i računalnim ćete morati usko VMs odgovora niza

Evo osnovne informacije i što valja za korištenje novijom Azure H-niza i starijim A8 A9, A10 i A11 pojavljivanja, poznata i kao *računalnim ćete morati usko* instanci. U ovom članku fokus je na pomoću ove instance za Windows VMs. U ovom se članku i dostupna je za [Linux VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md).


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Pristup mreži RDMA

Možete stvoriti klastere zaslonom RDMA Windows Server instancama i implementirati jednu od podržanih implementacije MPI da iskoristite prednost Azure RDMA mreže. Niska Latencija, Visoko propusnost mreže je rezervirana za MPI promet samo.

* **Operacijski sustav**
    * **Virtualnim strojevima** – Windows Server 2012 R2, Windows Server 2012
    * **Servisi u oblaku** – Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 goste OS obitelj

* **MPI** – Microsoft MPI (MS-MPI) 2012 R2 ili novijim verzijama biblioteke MPI Intel 5.x

Podržani MPI implementacije koristite mrežni izravno sučelje za komunikaciju između instance. Potražite u članku [Postavljanje Windows RDMA klaster HPC paket za pokretanje aplikacije MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) i [Korištenje više instanci izvođenje zadataka aplikacije sučelja prosljeđivanje poruke (MPI) u grupe za Azure](../batch/batch-mpi.md) za mogućnosti implementacije i navedeni koraci za konfiguraciju uzorka.


>[AZURE.NOTE]Proširenje HpcVmDrivers na zaslonom RDMA VMs računalnim ćete morati usko, mora biti dodan VMs da biste instalirali Windows mreže upravljačkih koje su vam potrebne za povezivanje RDMA. U većini implementacijama proširenje HpcVmDrivers se automatski dodaje. Ako je potrebno dodati proširenje potražite u članku [Upravljanje VM proširenja](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Zahtjevi za HPC paket i Windows

Za korištenje instance računalnim ćete morati usko sa sustavom Windows Server potreban je [Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), besplatne klasteru HPC tvrtke Microsoft i rješenje za upravljanje posao. Međutim, je jedna od mogućnosti za stvaranje računalnim klasterima u Azure pokretanje aplikacije utemeljen na sustavu Windows MPI i drugih radnih opterećenja HPC. HPC paket 2012 R2 i novijim verzijama obuhvaćaju okruženje za izvođenje za MS-MPI koje možete koristiti s mrežom Azure RDMA kada implementiran na zaslonom RDMA VMs.

Dodatne informacije i kontrolne popise za uporabu instance računalnim ćete morati usko HPC paket na Windows Server potražite u članku [Postavljanje Windows RDMA klaster HPC paket za pokretanje aplikacije MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Daljnji koraci

* Detalje o dostupnosti i cijene veličina računalnim ćete morati usko potražite u članku [virtualnim strojevima cijene](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) i [cijene za servise u Oblaku](https://azure.microsoft.com/pricing/details/cloud-services/).

* Prostor za pohranu kapaciteta i detalja na disku, potražite u članku [veličine za virtualnim računalima](virtual-machines-linux-sizes.md).

* Za početak implementacija i računalnim ćete morati usko instance pomoću HPC paketa u sustavu Windows potražite u članku [Postavljanje Windows RDMA klaster HPC paket za pokretanje aplikacije MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Informacije o korištenju A8 i A9 instanci za pokretanje aplikacije MPI pomoću obrade Azure, potražite u članku [Korištenje više instanci zadaci za pokretanje aplikacije sučelja prosljeđivanje poruke (MPI) u grupe za Azure](../batch/batch-mpi.md).
