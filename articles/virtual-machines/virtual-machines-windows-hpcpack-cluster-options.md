<properties
 pageTitle="Mogućnosti klaster paketa Windows HPC u oblaku | Microsoft Azure"
 description="Dodatne informacije o mogućnostima Microsoft HPC paket za stvaranje i upravljanje Windows visoke performanse računalstvo klasteru (HPC) u oblaku Azure"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Mogućnosti HPC paket za stvaranje i upravljanje Windows HPC klaster servisu Azure

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

U ovom članku fokus je na mogućnosti da biste stvorili klastere HPC paket da biste pokrenuli radnih opterećenja sustava Windows. Postoje i mogućnosti za stvaranje klastere da biste pokrenuli [radnih opterećenja Linux HPC HPC paketom](virtual-machines-linux-hpcpack-cluster-options.md).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Pokretanje programa paketa HPC klaster u Azure VMs

### <a name="azure-templates"></a>Predlošci za Azure

* (Trgovine) [Klaster HPC paket za radnih opterećenja sustava Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Trgovine) [Klaster HPC paket za radnih opterećenja programa Excel](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Brzi početak rada) [Stvaranje programa klasteru HPC](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Brzi početak rada) [Stvaranje programa klasteru HPC sa slikom prilagođene računalnim čvor](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure VM slike

* [Paket HPC na Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [Paket HPC računalnim čvor na Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [Paket HPC izračunati čvor s programom Excel na Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>Implementacijsku skriptu PowerShell

* [Stvaranje programa HPC klaster s implementacijsku skriptu HPC paket IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Vodiči za

* [Praktični vodič: Početak rada s programa paketa HPC klaster u Azure da biste pokrenuli Excel i SOA radnih opterećenja](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Ručno implementacija s portala za Azure

* [Postavljanje čvor glavni programa paketa HPC klaster u VM programa Azure](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Upravljanje klaster

* [Upravljanje računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Povećaj i Smanji Azure računalnim resursa u programa paketa HPC klaster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Slanje poslove programa paketa HPC klaster servisu Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Upravljanje u paketu HPC](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Dodavanje čvorove ulogu suradnika programa paketa HPC klaster


* [Brzi niz za Azure tempiranja instance HPC paketa](https://technet.microsoft.com/library/gg481749.aspx)

* [Praktični vodič: Postavljanje klaster hibridne paketom HPC servisu Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Dodavanje Azure "neprekidnog rada" čvorove HPC paket glavni čvor u Azure](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integracija s grupe za Azure 

* [Brzi niz za Azure obradu HPC paketom](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Stvaranje RDMA klastere za radnih opterećenja MPI

* [Postavljanje Windows RDMA klaster HPC paket za pokretanje aplikacije MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
