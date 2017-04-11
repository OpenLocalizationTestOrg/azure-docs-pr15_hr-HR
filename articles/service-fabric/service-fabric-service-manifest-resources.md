<properties
   pageTitle="Određivanje krajnje točke servisa servis tkanina | Microsoft Azure"
   description="Kako se opisuju krajnjoj točki resursa u manifest servisa, uključujući upute za postavljanje HTTPS krajnje točke"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Određivanje resursa u manifest servisa

## <a name="overview"></a>Pregled

Manifest servisa omogućuje resursa koji se koriste servis tako da bude deklarirane/promijeniti bez promjena kompilirani kod. Azure servisa tkanina podržava konfiguraciju krajnju točku resursa za uslugu. Pristup resurse koji su navedeni u manifestu servis je moguće prilagoditi putem SecurityGroup u manifestu aplikacije. Deklariranje resursa omogućuje ovih resursa moguća trenutku implementaciju, što znači da je servis nije potrebno predstavljanje novi mehanizam konfiguracije. Definicija sheme datoteke ServiceManifest.xml instaliran je uz servis tkanina SDK i alate za *C:\Programske datoteke\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Krajnje točke

Kada na krajnjoj točki resursa definiran u manifest servisa, tkanina servisa dodjeljuje priključke iz raspona priključak rezervirane aplikacije kada priključak izričito nije naveden. Na primjer, pogledajte krajnjoj točki *ServiceEndpoint1* naveden u manifesta isječak dobili nakon ovog odlomka. Uz to, services možete zatražiti i određeni priključak u resursu. Servis replike koji se izvode na različite čvorove se mogu dodijeliti drugi priključak brojeve, dok je priključak za zajedničko korištenje replike servisa radi na istom čvor. Servis replike možete koristiti sljedeće priključke potrebi za replikaciju i slušanje klijent zahtjeva za.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Pogledajte [Konfiguriranje s praćenjem stanja pouzdanog Services](service-fabric-reliable-services-configuration.md) da biste pročitali dodatne informacije o pozivanju krajnje točke iz postavke paketa config datoteke (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Primjer: Određivanje krajnje HTTP servisa

Sljedeće manifest servisa definira jedan resurs TCP krajnjoj točki i dva resurse HTTP krajnje točke u na &lt;resursi&gt; element.

HTTP krajnje točke se automatski ACL način tako tkanina servisa.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Primjer: Određivanje krajnje HTTPS servisa

HTTPS protokola nudi provjera autentičnosti poslužitelja i služi za šifriranje komunikacije klijent-poslužitelj. Da biste omogućili HTTPS servis za servis tkanina, navedite protokol u na *Resursi -> krajnje točke -> krajnje točke* dio manifest servisa, kao što je prikazano na prethodnom za krajnju točku *ServiceEndpoint3*.

>[AZURE.NOTE] Protokol na servis nije moguće promijeniti tijekom nadogradnje računala na kojemu je constituting na važne promjena.


Slijedi primjer ApplicationManifest koje su vam potrebne da biste postavili za HTTPS. Potrebno je navesti otisak prsta za potvrdu. U EndpointRef je referenca na EndpointResource u ServiceManifest, za koju postavljate HTTPS protokola. Možete dodati više EndpointCertificate.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
