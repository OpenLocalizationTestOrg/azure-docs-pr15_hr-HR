<properties
 pageTitle="O računalnim ćete morati usko VMs s Linux | Microsoft Azure"
 description="Dodatne informacije i pitanja vezana uz za korištenje računalnim ćete morati usko veličine H niza i A8, A9, A10 i A11 za Linux VMs"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>O H niza i računalnim ćete morati usko VMs odgovora niza 

Evo osnovne informacije i što valja za korištenje novijom Azure H-niza i starijim A8 A9, A10 i A11 veličine, poznata i kao *računalnim ćete morati usko* instance. U ovom se članku usredotočuje se na pomoću ove veličine za Linux VMs. Ovaj se članak odnosi dostupne za [Windows VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Pristup mreži RDMA

Možete stvoriti klastere zaslonom RDMA Linux VMs izvođenja jedan od sljedećih podržane Linux HPC distribucija i podržani implementaciju MPI da iskoristite prednost Azure RDMA mreže. Potražite u članku [Postavljanje Linux RDMA klaster da biste pokrenuli MPI aplikacije](virtual-machines-linux-classic-rdma-cluster.md) za mogućnosti implementacije i navedeni koraci za konfiguraciju uzorka.

* **Distribucija** - mora implementirati VMs sa zaslonom RDMA SUSE Linux Enterprise Server (SLES) ili utemeljen na OpenLogic CentOS HPC slike u Azure Marketplace. Samo sljedeće slike trgovine podržavaju potrebne Linux RDMA upravljačke programe:

    * SLES 12 SP1 za HPC, SLES 12 SP1 za HPC (Premium)
    
    * SLES 12 za HPC, SLES 12 za HPC (Premium)
    
    * Utemeljen na centOS 7.1 HPC
    
    * Utemeljen na centOS 6.5 HPC
    
    >[AZURE.NOTE]H niz VMs, preporučujemo ili SP1 za 12 SLES HPC slike ili utemeljen na CentOS 7.1 HPC slike.
    >
    >Na slikama utemeljen na CentOS HPC otklanjanje ažuriranja su onemogućene u konfiguracijskoj datoteci **yum** . To je jer su upravljačke programe za Linux RDMA distributed kao paketa RPM i ažuriranja za upravljačke programe možda neće funkcionirati ako se ažurira Jezgra sustava.

* **MPI** - Intel MPI biblioteke 5.x

    Ovisno o slika trgovine odaberete, posebnu licenciranja, instalaciju ili konfiguracije Intel MPI možda će biti potrebno, na sljedeći način: 
    
    * **SLES 12 SP1 za HPC sliku** – instalaciju paketa Intel MPI distribuira na na VM pokretanjem sljedeće naredbe:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **Za HPC slika SLES 12** - zasebno morate se registrirati za preuzimanje i instalaciju Intel MPI. Potražite u [vodiču za instalaciju Intel MPI biblioteke](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **Slika utemeljen na centOS HPC** - Intel MPI 5.1 već instaliran.  

    Da biste pokrenuli MPI zadatke na grupirani VMs nije potrebno dodatne podatke o konfiguraciji sustava. Ako, na primjer, na klaster VMs ćete morati uspostaviti pouzdanost među čvorove računalnim. Standardne postavke potražite u članku [Postavljanje Linux RDMA klaster da biste pokrenuli MPI aplikacije](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>Zahtjevi za HPC komercijalni ispis Linux

[Paket HPC](https://technet.microsoft.com/library/jj899572.aspx), besplatne klasteru HPC tvrtke Microsoft i rješenje za upravljanje posla sadrži jednu mogućnost za korištenje instance računalnim ćete morati usko s Linux. Najnovija izdanja HPC paket 2012 R2 podrške nekoliko distribucija Linux pokrenuti izračun čvorove u uveden u VMs Azure upravlja čvor glavni Windows Server. Sa zaslonom RDMA Linux računalnim čvorove izvodi Intel MPI, HPC paket možete zakazivanje i pokretanje aplikacije koje pristup mreži RDMA Linux MPI. Početak rada potražite u članku [Početak rada s Linux računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Razmatranja topologija mreže

* Na RDMA omogućeno Linux VMs u Azure, Eth1 je rezervirana za RDMA mrežni promet. Promijenite željene postavke Eth1 ili podatke u konfiguracijskoj datoteci koje upućuju na mrežu. Eth0 je rezervirana za obične Azure mrežni promet.

* IP iznad InfiniBand (IB) nije podržan u Azure. Podržana je samo RDMA putem IB.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA ažuriranja za upravljačke programe za SLES 12

Nakon što stvorite VM na osnovi SLES 12 HPC slike, možda ćete morati ažuriranje upravljačkih programa RDMA na VMs za RDMA mrežne veze. 

>[AZURE.IMPORTANT]Ovaj korak nije **potreban** za SLES 12 za HPC VM implementacije u svim regijama Azure. 
>Ovaj korak nije potreban ako pokrenete SP1 za 12 SLES HPC, utemeljen na CentOS HPC 7.1 ili utemeljen na CentOS 6.5 HPC VM. 

Prije nego što ažurirate upravljačke programe, zaustavite sve procese **zypper** ili sve procese koji zaključavanja SUSE repo baze podataka na na VM. U suprotnom se upravljačke programe možda neće ispravno ažurirati.  

Da biste ažurirali upravljačke programe za Linux RDMA na svakom VM, pokrenite jedan od sljedećih skupova Azure EŽA naredbe od klijentskog računala.

**SLES 12 HPC VM dodjeli u modelu klasični implementacije**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**SLES 12 HPC VM dodjeli u modelu implementacije Voditelj resursa**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]To može potrajati neko vrijeme da instalirate upravljačke programe i naredbu vraća bez izlaz. Nakon ažuriranja, vaše VM će se pokrenuti i mora biti spreman za korištenje u nekoliko minuta.

### <a name="sample-script-for-driver-updates"></a>Ogledne skripte ažuriranja upravljački program

Ako imate Klaster od SLES 12 za HPC VMs, možete skripte ažuriranje upravljačkog programa preko sve čvorove u svoj klaster. Na primjer, sljedeću skriptu ažurira upravljačke programe sustava klasteru 8 čvor.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Daljnji koraci

* Detalje o dostupnosti i cijene veličina računalnim ćete morati usko potražite u članku [virtualnim strojevima cijene](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Prostor za pohranu kapaciteta i detalja na disku, potražite u članku [veličine za virtualnim strojevima](virtual-machines-linux-sizes.md).

* Za početak implementacija i veličine računalnim ćete morati usko pomoću RDMA na Linux potražite u članku [Postavljanje Linux RDMA klaster da biste pokrenuli MPI aplikacije](virtual-machines-linux-classic-rdma-cluster.md).


