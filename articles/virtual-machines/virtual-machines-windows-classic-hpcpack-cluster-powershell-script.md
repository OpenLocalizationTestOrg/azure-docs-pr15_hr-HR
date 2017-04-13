<properties
   pageTitle="Implementacija klasteru Windows HPC skriptu PowerShell | Microsoft Azure"
   description="Pokrenuti skriptu PowerShell za implementaciju paketa Windows HPC klaster na Azure virtualnim računalima sustava"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Stvaranje Windows računalstvo visokih performansi (HPC) skupine s implementacijsku skriptu HPC paket IaaS

Pokrenite implementaciju HPC paket IaaS skriptu PowerShell za implementaciju dovršeno klasteru HPC za radnih opterećenja sustava Windows na Azure virtualnim računalima sustava. Klaster se sastoji od servisa Active Directory pridruženo glavni čvora izvodi Windows Server i Microsoft HPC Pack i dodatne prozore izračunati resursi koji navedete. Ako želite implementirati programa paketa HPC klaster u Azure za radnih opterećenja Linux potražite u članku [Stvaranje Linux HPC klaster s implementacijsku skriptu HPC paket IaaS](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Predložak programa Azure Voditelj resursa možete koristiti i za implementaciju sustava klaster HPC paket. Primjeri, potražite u članku [Stvaranje programa klasteru HPC](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) i [Stvaranje programa klasteru HPC s prilagođeni izračun čvor slike](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Primjer konfiguracijske datoteke

U sljedećim primjerima zamijeniti vlastitim vrijednosti za vašu pretplatu Id ili naziv i nazive račun i servis.

### <a name="example-1"></a>Primjer 1

Nakon odjeljka datoteka konfiguracije uvodi se klaster HPC paket koji sadrži glavni čvora s lokalne baze podataka, a zatim pet izračunati čvorove operacijskog sustava Windows Server 2012 R2. Sve servise oblaka stvaraju se izravno na mjestu Zapad SAD-a. Glavni čvor ponaša se kao kontroler domene skupa stabala domene.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Primjer 2

Nakon odjeljka datoteka konfiguracije uvodi se klaster HPC paketa u postojeći skup stabala domene. Klaster je 1 glavni čvora s lokalne baze podataka i 12 izračunati čvorove s nastavkom BGInfo VM primijeniti.
Automatsko instaliranje ažuriranja sustava Windows nije omogućen za sve VMs skupa stabala domene. Sve servise oblaka stvaraju se izravno na mjestu istočnoazijski. Čvorovi računalnim stvaraju se u tri servisa putem oblaka i tri računa za pohranu: _MyHPCCN 0001_ _MyHPCCN 0005_ u _MyHPCCNService01_ i _mycnstorage01_; _MyHPCCN 0006_ _MyHPCCN0010_ u _MyHPCCNService02_ i _mycnstorage02_; i _MyHPCCN 0011_ _MyHPCCN 0012_ u _MyHPCCNService03_ i _mycnstorage03_). Čvorovi računalnim se stvaraju iz postojeće slike privatnim zabilježene s računalnim čvor. Automatsko Povećaj i Smanji sa zadanom je omogućena usluga Povećaj i Smanji vremenskim razmacima.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Primjer 3

Nakon odjeljka datoteka konfiguracije uvodi se klaster HPC paketa u postojeći skup stabala domene. Klaster sadrži jedan glavni čvor, jedan poslužitelj baze podataka na disku 500 GB podataka, 2 broker čvorovi operacijskog sustava Windows Server 2012 R2 i pet računalnim čvorovi operacijskog sustava Windows Server 2012 R2. U grupi afinitet *MyIBAffinityGroup*stvara se servis u oblaku MyHPCCNService i drugih servisa u oblaku stvaraju se u grupi afinitet *MyAffinityGroup*. Na glavni čvor je omogućeno HPC posao raspored REST API-JA i HPC web-portalu.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Primjer 4

Nakon odjeljka datoteka konfiguracije uvodi se klaster HPC paketa u postojeći skup stabala domene. Klaster sadrži dvije glavni čvora s lokalne baze podataka, stvaraju se dva Azure čvor predloške i tri čvorove Azure srednje veličine stvaraju predloška Azure čvor _AzureTemplate1_. Datoteka skripte pokreće čvor glavni kada je konfiguriran čvor glavni.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Otklanjanje poteškoća


* **Pogreška "VNet ne postoji"** – ako pokrenete skripte za implementaciju više klastere Azure istovremeno u odjeljku pretplata, jedan ili više implementacijama možda neće funkcionirati uz pogreške "VNet *VNet\_naziv* ne postoji".
Ako se pojavi ta pogreška, pokrenite skriptu ponovno za nije uspjelo.

* **Problem pristup Internetu iz Azure virtualne mreže** – ako stvorite klaster s novi kontroler implementacijsku skriptu ili ručno Promicanje glavni čvor VM s kontrolerom domene, mogli biste primijetiti probleme s VMs povezivanja s Internetom. Taj se problem pojavljuje ako DNS poslužitelju forwarder automatski konfigurirana na kontrolerom domene, a forwarder DNS poslužitelj ne otkloni pravilno.

    Da biste riješili taj problem, prijavite se u kontrolerom domene i uklanjanje postavki konfiguriranje forwarder ili konfiguriranje DNS poslužitelja za valjane forwarder. Da biste konfigurirali tu postavku, u upravitelju poslužitelja kliknite **Alati** >
    **DNS-a** da biste otvorili Upravitelj DNS-a, a zatim dvokliknite **prosljeđivanja**.

* **Problem pristupa RDMA mreže s računalnim ćete morati usko VMs** – ako dodate računalnim Windows Server ili vi posrednik čvor VMs pomoću RDMA zaslonom veličine kao što su A8 ili A9, mogli biste primijetiti problema s povezivanjem te VMs s mrežom RDMA aplikacije. Jedan od razloga taj se problem pojavljuje se ako proširenje HpcVmDrivers nije ispravno instaliran na VMs je netko dodao na klaster. Ako, na primjer, proširenje možda zamrzne u instalaciji stanju.

    Da biste riješili taj problem, najprije provjerite stanje datotečni nastavak u na VMs. Ako proširenje nije ispravno instaliran, pokušajte ukloniti čvorove iz klasteru HPC, a zatim ponovno dodajte čvorove. Ako, na primjer, možete dodati računalnim čvor VMs ponovnim pokretanjem skripte Dodaj HpcIaaSNode.ps1 na glavni čvor.
    
## <a name="next-steps"></a>Daljnji koraci

* Pokušajte pokrenuti test radno opterećenje na klaster. Na primjer, pročitajte članak HPC paket [Vodič za početak rada](https://technet.microsoft.com/library/jj884144).

* Vodič za skripte implementacije klaster i pokretanje programa HPC radno opterećenje, potražite u članku [Početak rada s programa paketa HPC klaster u Azure da biste pokrenuli Excel i SOA radnih opterećenja](virtual-machines-windows-excel-cluster-hpcpack.md).

* Isprobajte HPC paket alate za pokretanje, zaustavljanje, dodajte i uklonite računalnim čvorove iz klaster stvarate. Potražite u članku [Upravljanje izračunati čvorove u programa paketa HPC klaster u Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Postavljanje slanje zadataka na klaster s lokalnog računala, potražite u članku [Slanje HPC poslove s lokalnog računala programa paketa HPC klaster u Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
