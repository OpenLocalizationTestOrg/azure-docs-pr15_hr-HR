<properties
   pageTitle="Implementacija aplikacije servisa tkanina | Microsoft Azure"
   description="Postupak implementacije i uklanjanje aplikacija u tkanina servisa"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Uvođenje i uklanjanje aplikacija pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Kada se [proizvodnje vrsta aplikacije][10], spremna je za implementaciju u programa klaster tkanina servisa Azure. Uvođenje obuhvaća sljedeća tri koraka:

1. Prijenos paketa aplikacije
2. Registrirajte se vrsta aplikacije
3. Stvaranje instance aplikacije

>[AZURE.NOTE] Ako koristite Visual Studio za implementaciju i ispravljanje pogrešaka aplikacije na svoj klaster lokalne razvoj, sljedeći koraci upravlja automatski kroz skriptu PowerShell nalazi u mapi skripte aplikacije projekta. Ovaj članak sadrži pozadine na ono te skripte izvršavate tako da možete izvesti iste radnje izvan Visual Studio.

## <a name="upload-the-application-package"></a>Prijenos paketa aplikacije

Prijenos Aplikacijski paket stavlja ga na mjesto kojemu je pristupačnog interni tkanina servisa komponente. Koristite PowerShell za provođenje prijenos. Prije pokretanja naredbe ljuske PowerShell u ovom članku uvijek pokrenuti pomoću [Povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) povezati klaster tkanina servisa.

Recimo da imate mapu pod nazivom *MyApplicationType* koja sadrži potrebne programski manifest, manifesti servisa i kod / / podatke o konfiguracijama paketa. Naredbe [Kopiraj ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) prenosi paket klaster pohrana slika. Cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** koja je dio modul ljuske PowerShell za SDK tkanina za servis, koristi se za dohvaćanje niza za povezivanje iz trgovine slike.  Da biste uvezli modul SDK, pokrenite:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Aplikacijski paket možete kopirati iz *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* *c:\temp\MyApplicationType* (Preimenuj direktorija "ispravljanje pogrešaka" u "MyApplicationType"). U sljedećem primjeru prenosi paket:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Registrirajte se Aplikacijski paket

Registracija Aplikacijski paket olakšava vrsta aplikacije i verziju deklarirani u manifestu aplikacije dostupne za korištenje. Sustav čita paketa prenijeli u prethodnom koraku, provjerite je li paket (jednako [Test ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) izvodi lokalno), obradu sadržaj paketa i kopirajte obrađeni paket na mjesto Interna sustava.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

Naredba [Register ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) vraća samo kada je uspješno kopiran Aplikacijski paket sustava. Koliko to trebalo ovisi o sadržaju paketa aplikacija. Ako je potrebno, parametar **- TimeoutSec** mogu se dohvaćati dulje vrijeme čekanja. (Zadana vremensko ograničenje je 60 sekundi).

[Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) -command dohvaća sve verzije vrsta uspješno registriranu aplikaciju.

## <a name="create-the-application"></a>Stvaranje aplikacije

Aplikaciju možete stvoriti pomoću bilo koja verzija vrsta aplikacije registrirani uspješno do naredbe [Nova ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) . Naziv svake aplikacije moraju započeti znakom u *tkanina:* shema i biti jedinstvena za svaku instancu aplikacije. Zadani servisi definirano u programski manifest vrsta ciljne aplikacije stvaraju se trenutno.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

[Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) -command dohvaća sve instance aplikacije stvorene uspješno, zajedno sa svojim ukupnog statusa.

[Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) -command dohvaća sve instance servisa uspješno stvorenih unutar danog aplikacije instance. Zadane servise (ako ih ima) su navedene ovdje.

Za sve navedene verziju vrste registriranu aplikaciju moguće stvoriti više instanci aplikacije. Svaku instancu aplikacije pokreće se u odvajanja s vlastitom imenika tvrtke i proces.

## <a name="remove-an-application"></a>Uklanjanje aplikacije

Kada aplikacija instance više nije potrebna, pomoću naredbe [Ukloni ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) trajno možete ukloniti ga. Ta se naredba automatski uklanja sve servise koji pripadaju kao i aplikacije trajno uklanjanje svih stanje servisa. Ovaj postupak ne može poništiti, a nije moguće oporaviti stanje aplikacije.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Kada određenu verziju vrstu aplikacije koju više nije potrebna, potrebno je odjaviti pomoću naredbe [Neregistriranja ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) . Poništavanje registriranja Neiskorišteni vrste izdaje prostora za pohranu koriste sadržaj paketa aplikacije tu vrstu slike spremište. Vrstu aplikacije mogu biti registriran kao nijedna aplikacija se instancirati slučajeve vezane uz njega i ne čeka aplikacije nadogradnje pozivate ga.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiraj ServiceFabricApplicationPackage traži programa ImageStoreConnectionString

Okruženje za servis tkanina SDK već moraju imati ispravne zadanih postavljanje. No ako je potrebno, ImageStoreConnectionString za sve naredbe moraju se podudarati vrijednost koja se koristi klaster tkanina servisa. Koje možete pronaći u manifest klaster dohvatiti putem naredbe [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) :

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Daljnji koraci

[Nadogradnja aplikacije servisa tkanina](service-fabric-application-upgrade.md)

[Uvod tkanina servis stanja](service-fabric-health-introduction.md)

[Dijagnosticiranje i otklanjanje poteškoća s usluge tkanina servisa](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Model aplikacije u tkanina servisa](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
