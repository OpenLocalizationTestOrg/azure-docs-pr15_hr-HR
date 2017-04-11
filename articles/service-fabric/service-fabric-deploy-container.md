<properties
   pageTitle="Servis tkanina i implementacija spremnika | Microsoft Azure"
   description="Servisa tkanina i korištenje spremnika za implementaciju aplikacije microservice. U ovom se članku opisuju mogućnosti koje tkanina servis nudi za spremnika i kako implementirati spremnik sliku sustava Windows u klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Pregled: Implementacija spremniku Windows tkanina servisa

> [AZURE.SELECTOR]
- [Implementacija spremnik za Windows](service-fabric-deploy-container.md)
- [Implementacija Docker spremnik](service-fabric-deploy-container-linux.md)

U ovom se članku vodit će vas kroz stvaranje containerized services u spremnicima za Windows. 

>[AZURE.NOTE] Ta je značajka u pretpregledu Linux i trenutno nije dostupno za korištenje sa sustavom Windows Server 2016. To će biti dostupno u pretpregledu za Windows Server 2016 u sljedećem izdanju tkanina servisa i podržane u sljedećem izdanju nakon toga.

Servis tkanina ima nekoliko mogućnosti spremnik koji vam pomoći s stvaranje aplikacija koja se sastoji od microservices koji su containerized. One se nazivaju containerized services. 

Mogućnosti uključuju;

- Spremnik slika implementacije i aktivacijom
- Upravljanje resursa
- Provjera autentičnosti spremište
- Spremnik priključak za mapiranje priključak glavnog računala
- Spremnik spremnik otkrivanje i komunikacija
- Mogućnost konfiguracija i postavljanje varijable okruženja

Omogućuje pregled svih mogućnosti shodno kada pakiranje containerized usluga da bi bio uvršten u aplikaciji.

## <a name="packaging-a-windows-container"></a>Pakiranje spremnik za Windows

Kada pakiranje spremniku, možete odabrati bilo da biste koristili predložak projekta za Visual Studio ili [ručno stvaranje paketa aplikacije](#manually). Koristite Visual Studio, strukture paketa aplikacije i datotekama manifesta stvaraju novi projekt Čarobnjak za vas (to je uskoro u novo izdanje).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Pakiranje postojeće slike spremnik pomoću Visual Studio

>[AZURE.NOTE] U budućim izdanju programa Visual Studio tooling SDK moći da biste dodali spremnika u aplikaciju na isti način možete dodati gost izvršna danas. Pogledajte temu [uvođenja izvršne datoteke na servis tkanina gost](service-fabric-deploy-existing-app.md) . Trenutno to morate učiniti ručno pakiranje prema uputama u nastavku.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Ručno pakiranje i implementacija spremnik
Postupak ručno pakiranje containerized servisa temelji se na sljedeće korake:

1. Objaviti spremnike vaše spremište.
2. Stvorite strukturu direktorija paketa.
3. Uređivanje datoteke manifesta servisa.
4. Uređivanje datoteke manifesta aplikacije.

## <a name="container-image-deployment-and-activation"></a>Spremnik slika implementacije i aktivacija.
U servis tkanina [modelu](service-fabric-application-model.md)spremnik predstavlja aplikacije glavnog računala smještaju replike većem broju servisa. Implementacije i aktiviranje spremniku, stavite naziv spremnik sliku u `ContainerHost` element u manifestu servisa.

U manifestu servisa dodali na `ContainerHost` stavka pokažite i postavite na `ImageName` se naziv spremnik spremište i slike. Sljedeće djelomično manifest prikazuje primjer implementacija spremnika koji se naziva *myimage:v1* iz spremišta naziva *myrepo*

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Možete unijeti unos naredbe sa slikom spremnik navođenjem neobavezna `Commands` element zarezom razgraničeno skupa naredbi za pokretanje u kontejneru. 

## <a name="resource-governance"></a>Upravljanje resursa
Resurs upravljanja je mogućnost spremnika i ograničava resursa koje spremnik možete koristiti na glavnom računalu. U `ResourceGovernancePolicy`, naveden u programski manifest, omogućuje deklarirati ograničenja resursa za paket kod usluge. Ograničenja resursa možete postaviti za;

- Memorije
- MemorySwap
- CpuShares (CPU relativni težina)
- MemoryReservationInMB  
- BlkioWeight (relativni Debljina BlockIO). 

>[AZURE.NOTE] U buduće izdanje podrške za određivanje određeni blok IO ograničenja kao što su IOPs, čitanje/pisanje BPS, a drugi će biti moguće.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Provjera autentičnosti spremište
Da biste preuzeli spremniku, možda ćete morati unijeti vjerodajnice za prijavu u spremniku spremište. Vjerodajnice za prijavu, naveden u *programski* manifest koriste se za određivanje podatke za prijavu ili SSH ključ za preuzimanje slike spremnika u spremištu slike.  Sljedeći primjer prikazuje račun pod nazivom *TestUser* uz lozinku u običan tekst. To se **ne** preporučuje.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Lozinke možete i trebali biste šifriran pomoću certifikata koji je implementiran na računalu.

Sljedeći primjer prikazuje račun pod nazivom *TestUser* lozinkom šifriran pomoću certifikata pod nazivom *MyCert*. Možete koristiti u `Invoke-ServiceFabricEncryptText` Powershell naredbu da biste stvorili tajnu snaga tekst za lozinku. Pročitajte ovaj članak [Upravljanje tajne u aplikacijama servisa tkanina](service-fabric-application-secret-management.md) detalje o tome. Privatni ključ certifikata dešifrirati lozinku mora biti implementirano u lokalnom stroju u način grupiranje (u Azure to je putem OKVIRA). Pa kad servisa tkanina uvodi paket servisa s računalom, preporučuje se moći dešifrirati na tajna i uz naziv računa uspješnoj spremište spremnik pomoću vjerodajnica.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Spremnik priključak za mapiranje priključak glavnog računala
Glavno računalo priključak koji se koristi za komunikaciju s spremnik navođenjem možete konfigurirati na `PortBinding` u manifestu aplikacije. Povezivanje priključak karata priključka koji servis priključuje na u kontejneru s priključkom na glavnom računalu.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Spremnik spremnik otkrivanje i komunikacija
Korištenje na `PortBinding` pravila možete mapirati priključak spremnik za programa `Endpoint` u manifestu servisa kao što je prikazano u sljedećem primjeru. Krajnja točka `Endpoint1` možete navesti fixed priključak, na primjer priključak 80 ili navedite priključak nije uopće, u tom slučaju slučajni priključak iz raspona priključak za aplikaciju za klastere odabran umjesto vas.

Za goste spremnika, pri određivanju programa `Endpoint` kao što je to u manifestu servisa omogućuje servisa tkanina automatski objaviti ovaj krajnje točke na servis za imenovanje tako da u klasteru ostale servise mogu otkriti ovaj spremnik za korištenje upita za OSTALE Razriješi servisa. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Po Registracija sa servisom imenovanje, jednostavno možete učiniti spremnik spremnik komunikacije kod unutar vaše kontejnera pomoću [povratnog proxy](service-fabric-reverseproxy.md). Sve što trebate napraviti je unijeti povratnog proxy http listening priključak i naziv servise koje želite komunicirati s postavljanjem kao varijable okruženja. U sljedećem odjeljku kako to učiniti.  

## <a name="configure-and-set-environment-variables"></a>Konfiguriranje i postavljanje varijable okruženja
Varijable okruženja može biti navedeni foe svaki paket kod u servisu manifesta za oba servisa u uveden u spremnika ili kao procesa/goste izvršne datoteke. Ove vrijednosti varijable okruženje možete nadjačati posebno u manifestu aplikacije ili navedeni tijekom implementacije kao parametar aplikacije.

Sljedeće servisa manifest XML isječak prikazani Primjeri kako odrediti varijabli okruženja za paket kod. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Ove varijable okruženja moguće poništiti na razini manifesta aplikacije:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

U primjeru iznad smo ste naveli eksplicitnih vrijednost za na `HttpGateway` varijablu okruženja (19000) tijekom vrijednost za `BackendServiceName` je parametar postavljen putem na `[BackendSvc]` parametar aplikacije. To vam omogućuje da odredite vrijednost za `BackendServiceName`vrijednost u vrijeme implementacija aplikacije i nemate Fiksna vrijednost u manifestu. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Dovršavanje primjeri za računala i servisa manifest
Slijedi manifest aplikacije za primjer koji prikazuje spremnik značajke.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Slijedi primjer servisa manifest (koji je naveden u prethodnom programski manifest), a prikazuje značajke spremnik

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
