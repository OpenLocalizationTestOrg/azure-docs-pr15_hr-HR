<properties
   pageTitle="Skriptu PowerShell za implementaciju Linux HPC klaster | Microsoft Azure"
   description="Pokrenuti skriptu PowerShell za implementaciju klaster Linux HPC paket na Azure virtualnim računalima sustava"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Stvaranje Linux visoke performanse računalstvo klasteru (HPC) s implementacijsku skriptu HPC paket IaaS

Pokrenite implementaciju HPC paket IaaS skriptu PowerShell za implementaciju dovršeno klasteru HPC za radnih opterećenja Linux na Azure virtualnim računalima sustava. Klaster sastoji se od sustava Active Directory pridruženo glavni čvor izvodi Windows Server i Microsoft HPC Pack i računalnim čvorovi koji se izvode jednu od distribucija Linux podržava HPC paket. Ako želite implementirati programa paketa HPC klaster u radnih opterećenja Azure za Windows potražite u članku [Stvaranje klaster Windows HPC s implementacijsku skriptu HPC paket IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Predložak programa Azure Voditelj resursa možete koristiti i za implementaciju sustava klaster HPC paket. Primjer potražite u članku [Stvaranje programa HPC klaster s Linux čvorove za izračun](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Primjer konfiguracijska datoteka

Sljedeće konfiguracijska datoteka stvara novi kontroler i skup stabala domene i implementira programa paketa HPC klaster koja ima 1 glavni čvora s lokalne baze podataka i 10 čvorove računalnim Linux. Sve servise oblaka stvaraju se izravno na mjestu istočnoazijski. Čvorovi računalnim Linux stvaraju se u 2 servise u oblaku i 2 računa za pohranu (odnosno _MyLnxCN 0001_ da biste _MyLnxCN 0005_ u _MyLnxCNService01_ i _mylnxstorage01_) i _MyLnxCN 0006_ _MyLnxCN 0010_ u _MyLnxCNService02_ i _mylnxstorage02_. Čvorovi računalnim stvaraju se slike OpenLogic CentOS verzije 7.0 Linux. 

Zamijenite vlastitim vrijednosti za naziv svoje pretplate i nazive račun i servis.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Otklanjanje poteškoća

* **Pogreška "VNet ne postoji"** – ako pokrenete implementacijsku skriptu IaaS HPC paketa za implementaciju više klastere Azure istovremeno u odjeljku pretplata, jedan ili više implementacijama možda neće funkcionirati uz pogreške "VNet *VNet\_naziv* ne postoji".
Ako se pojavi ta pogreška, ponovno pokrenuti skriptu nije uspjelo implementacije.

* **Problem pristup Internetu iz Azure virtualne mreže** – ako stvorite programa paketa HPC klaster s novi kontroler implementacijsku skriptu ili ručno Promicanje glavni čvor VM s kontrolerom domene, mogli biste primijetiti problema s povezivanjem VMs u Azure virtualne mreže s Internetom. To se može dogoditi ako DNS poslužitelju forwarder automatski konfigurirana na kontrolerom domene, a forwarder DNS poslužitelj ne otkloni pravilno.

    Da biste riješili taj problem, prijavite se u kontrolerom domene i uklanjanje postavki konfiguriranje forwarder ili konfiguriranje DNS poslužitelja za valjane forwarder. Da biste to učinili, u upravitelju poslužitelja kliknite **Alati** >
    **DNS-a** da biste otvorili Upravitelj DNS-a, a zatim dvokliknite **prosljeđivanja**.
    
## <a name="next-steps"></a>Daljnji koraci

* Pročitajte članak [Početak rada s Linux računalnim čvorove programa klasteru HPC paketa u Azure](virtual-machines-linux-classic-hpcpack-cluster.md) informacije o podržanim Linux distribucija premještanje podataka i slanje zadatke programa paketa HPC klaster s Linux čvorove za izračun.
* Vodiči za koje koriste skriptu da biste stvorili klaster i pokrenite radno opterećenje Linux HPC, potražite u članku:
    * [Mada NAMD s paket Microsoft HPC Linux računalnim čvorove u Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Mada OpenFOAM s paket Microsoft HPC Linux računalnim čvorove u Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Pokretanje ZVIJEZDA-CCM + s paket Microsoft HPC na Linux izračunati čvorove u Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
