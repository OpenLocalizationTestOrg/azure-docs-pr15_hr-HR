<properties
 pageTitle="Mogućnosti klaster Linux HPC paketa u oblaku | Microsoft Azure"
 description="Dodatne informacije o mogućnostima Microsoft HPC paket za stvaranje i upravljanje Linux visoke performanse računalstvo klasteru (HPC) u oblaku Azure"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Mogućnosti HPC paket za stvaranje i upravljanje programa HPC klaster u Azure za radnih opterećenja Linux

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

U ovom članku fokus je na mogućnosti za korištenje paketa HPC da biste pokrenuli radnih opterećenja Linux. Postoje i mogućnosti pokretanja [radnih opterećenja sustava Windows HPC HPC paketom](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Pokretanje programa paketa HPC klaster u Azure VMs

### <a name="azure-templates"></a>Predlošci za Azure


* (Trgovine) [Klaster HPC paket za radnih opterećenja Linux](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Brzi početak rada) [Stvaranje programa HPC klaster s Linux izračunati čvorove](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>Implementacijsku skriptu PowerShell

* [Stvaranje Linux HPC klaster s implementacijsku skriptu HPC paket IaaS](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Vodiči za

* [Praktični vodič: Početak rada s Linux računalnim čvorovi u klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Praktični vodič: Pokreni NAMD s paket Microsoft HPC na Linux izračunati čvorove u Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Praktični vodič: Pokreni OpenFOAM s paket Microsoft HPC na Linux RDMA klaster servisu Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Praktični vodič: Pokreni ZVIJEZDA-CCM + s paket Microsoft HPC na Linux RDMA skupine servisu Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Upravljanje klaster

* [Slanje poslove programa paketa HPC klaster servisu Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Upravljanje u paketu HPC](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>Stvaranje RDMA klastere za radnih opterećenja MPI

* [Praktični vodič: Pokreni OpenFOAM s paket Microsoft HPC na Linux RDMA klaster servisu Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Postavljanje Linux RDMA klaster za pokretanje aplikacije MPI](virtual-machines-linux-classic-rdma-cluster.md)

