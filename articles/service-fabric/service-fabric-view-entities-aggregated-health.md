<properties
   pageTitle="Kako prikazati tkanina servisa Azure entiteti pridružuje stanja | Microsoft Azure"
   description="U članku se opisuje kako upit, prikaz i analiza tkanina servisa Azure entiteti Zbrojeno stanja, kroz stanja i Općenito upiti."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="view-service-fabric-health-reports"></a>Prikaz izvješća o stanju servisa tkanina
Azure tkanina servisa predstavlja [stanja modela](service-fabric-health-introduction.md) koji se sastoji se od stanja entiteti na koje sustav komponente watchdogs možete izvješća i lokalne uvjete koje oni su nadzor. [Spremanje stanja](service-fabric-health-introduction.md#health-store) objedinjuje sve podatke o stanju bi se utvrdilo dobar entiteti.

U okvir klaster je popunjen izvješća o stanju poslao komponente sustava. Dodatno se informirajte na [Korištenje izvješća o stanju sustava da biste otklonili](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Servis tkanina omogućuje više načina da biste dobili Zbrojeno stanju entiteti:

- [Servis tkanina Explorer](service-fabric-visualizing-your-cluster.md) ili druge alate za vizualizaciju

- Stanje upita (putem komponente PowerShell, API-JA ili REST)

- Općenito upiti koji povrata popis entiteti koji imaju stanje kao jedno od svojstava (putem komponente PowerShell, API-JA ili REST)

Da bismo pokazali ove mogućnosti, recimo koristiti lokalne klaster s pet čvorove. Uz stavku na **tkanina: / sustava** aplikacije (koji postoji iz okvir) uvode se neke druge aplikacije. Jedna je od te aplikacije **tkanina: / WordCount**. Ove aplikacije sadrži konfiguriran pomoću sedam replike s praćenjem stanja servisa. Jer su samo pet čvorove, komponente sustava Pokaži upozorenje da je particija ispod count cilj.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Stanje u programu Explorer tkanina servisa
Servis tkanina Explorer omogućuje vizualni prikaz klaster. Na slici u nastavku, možete vidjeti koje:

- Aplikacija **tkanina: / WordCount** se crvena (u pogreška) jer je događaj pogreške dojavi **MyWatchdog** za svojstvo **dostupnost**.

- Neke njegove servise **tkanina: / WordCount/WordCountService** žuti (u upozorenje). Budući da je servis konfiguriran za korištenje sedam replike, oni ne sve može smjestiti, kao što su samo pet čvorove. Iako je ovdje nije prikazana, servis je particija žuta zbog izvješće sustava. Žuti particija pokreće servis žuta.

- Klaster je crveni crvena aplikacije.

Analizu koristi zadane pravilnike klaster manifest i programski manifest. Točno pravila i im tolerate sve pogreške.

Prikaz klaster pomoću programa Explorer tkanina servisa:

![Prikaz klaster pomoću programa Explorer tkanina servisa.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Dodatne informacije o [Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Stanje upita
Servis tkanina izlaže stanje upita za sve podržane [vrste entitet](service-fabric-health-introduction.md#health-entities-and-hierarchy). Može se pristupiti putem API-JA (metode možete pronaći na **FabricClient.HealthManager**), cmdleta ljuske PowerShell i OSTALE. Ove upiti vraćaju dovršeno stanja informacije o entitet: Zbrojeno stanja, događaji stanja entitet, podređeni stanja državama (kada je primjenjivo) i dobro procjene kada je entitet dobar.

> [AZURE.NOTE] Stanje entitet, vraća se kada potpuno unose u trgovini stanja. Entitet mora biti aktivna (ne brišu se) i imati sustav izvješća. Njegov nadređeni entiteti na hijerarhiju lanac mora imati sustav izvješća. Ako neki od ovih uvjeta nisu zadovoljeni, upiti stanja vratiti iznimku koja prikazuje Zašto se ne vraća entitet.

Stanje upita mora proći u identifikator entitet, a to ovisi o vrsti entitet. Upiti koriste neobavezno stanja pravila parametre. Ako su navedeni nema pravila stanja, [pravila stanja](service-fabric-health-introduction.md#health-policies) iz manifesta klaster ili aplikacije koriste se za procjenu. Upiti prihvatiti i filtre za vraćanje samo djelomična djece ili događaja – koji se poštovati navedenim filtrima.

> [AZURE.NOTE] Izlaz filtri primjenjuju se na strani poslužitelja tako da smanjite veličinu poruku odgovora. Preporučujemo da ste pomoću filtara za izlaz za ograničavanje vraćenih podataka, a ne primijenite filtre na klijentskoj strani.

Stanje neki entitet sadrži:

- Zbrojeno stanja entitet. Izračunati u spremištu stanja na temelju izvješća o stanju entitet, podređeni stanja državama (kada je primjenjivo) i pravila stanja. Dodatne informacije o [procjenu entitet stanja](service-fabric-health-introduction.md#entity-health-evaluation).  

- Stanje događaji na entitet.

- Zbirka stanja savezne države sve podređene sinkronizacije koji mogu sadržavati djece. Stanje stanja sadrže identifikatora entitet i Zbrojeno stanja. Da biste dovršeno stanje pribaviti podređeni, poziva stanje upita za podređene vrste entitet, a zatim prenesite identifikator podređene.

- Dobro procjene koji vode do izvješća koja ga je pokrenula stanje entitet, ako je entitet dobar.

## <a name="get-cluster-health"></a>Dohvaćanje stanja klaster
Vraća stanja klaster entitet, a sadrži stanje savezne države aplikacije i čvorove (podređenih klaster). Unos:

- [Neobavezni] Stanje klaster pravila za procjenu čvorove i događaje klaster.

- [Neobavezni] Aplikacija stanja pravila karti s pravilima o stanju za nadjačati pravilnike manifesta aplikacije.

- [Neobavezni] Filtri za događaje, čvorove i aplikacije koje odredite koje stavke su od interesa i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji, čvorove i aplikacijama koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste dobili klaster stanja, stvorite je `FabricClient` i pozivanje metodu [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) na **HealthManager**.

Sljedeće poziva korisniku klaster stanja:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Sljedeći kod dohvaća stanja klaster pomoću prilagođene klaster pravilo stanja i filtre za čvorove i aplikacije. Stvara [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), koji sadrži ulazne podatke.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanja klaster je [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Stanje klaster je pet čvorove, aplikacija sustava i tkanina: / WordCount konfigurirati kao što je opisano.

Sljedeći cmdlet dohvaća klaster stanja pomoću zadane pravilnike o stanju sustava. Zbrojeno stanja se upozorenje, jer se tkanina: / WordCount aplikacije se upozorenje. Imajte na umu kako dobro procjene zatražiti detalje na uvjetima koji se aktivira Zbrojeno stanja.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Sljedeći cmdlet PowerShell dohvaća stanja klaster pomoću pravila za prilagođene aplikacije. Filtriraju rezultata da biste dobili samo pogreške ili upozorenje o aplikacijama i čvorove. Zbog toga ne čvorove vraćaju, kao što su sve dobar. Samo tkanina: / WordCount aplikacije poštuje filtar za aplikacije. Jer prilagođenog pravilnika određuje treba uzeti u obzir upozorenja kao pogreške za na tkanina: / WordCount aplikacije, aplikacija procjenjuje kao pogreška te se klaster.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>OSTALE
Možete dobiti klaster stanja na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707669.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707696.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="get-node-health"></a>Početak čvor stanja
Vraća stanja čvor entitet, a sadrži stanje događaje koje se prijavili na čvor. Unos:

- [Traženi] Čvor naziva koja služi za identifikaciju čvor.

- [Neobavezni] Klaster stanja pravila postavke koje se koriste za procjenu stanja.

- [Neobavezni] Filtri za događaje koje odredite koje stavke su od interesa i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste dobili čvor stanja kroz na API-JA, stvorite je `FabricClient` i pozivanje metodu [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) na njegov HealthManager.

Sljedeći kod dobiva čvor stanja za navedeni čvor naziva:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Sljedeći kod dobiva čvor stanja za navedeni čvor naziva i prolaska u filtar događaja i prilagođenog pravilnika kroz [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanja čvor je [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .
Sljedeći cmdlet dobiva stanja čvor pomoću zadanog stanja pravila:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Sljedeći cmdlet dobiva stanja sve čvorove u klasteru sljedeće:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>OSTALE
Možete dobiti čvor stanja na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707650.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707665.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="get-application-health"></a>Dohvaćanje aplikacije stanja
Vraća stanja entitet aplikacije. Sadrži stanje savezne države distribuiranih aplikacija i servisa djece. Unos:

- [Traženi] Naziv aplikacije (URI) koja služi za identifikaciju aplikacije.

- [Neobavezni] Pravilnik za aplikacije stanja za nadjačati pravilnike manifesta aplikacije.

- [Neobavezni] Filtri za događaje, servise i distribuiranih aplikacije koje odredite koje stavke su od interesa i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji, servise i distribuiranih aplikacije koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste nabavili aplikaciju stanja, stvorite je `FabricClient` i pozivanje metodu [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) na njegov HealthManager.

Sljedeći kod dobiva stanja aplikacije za navedeni naziv aplikacije (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Sljedeći kod dohvaća stanja aplikacije za navedeni naziv aplikacije (URI), s filtrima i prilagođena pravila navedeno putem [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanja aplikacija je [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Sljedeći cmdlet vraća stanju na **tkanina: / WordCount** aplikacije:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Sljedeći cmdlet PowerShell prosljeđuje za prilagođene pravila. Također filtrira djece i događaje.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>OSTALE
Možete dobiti stanja aplikacije na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707681.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707643.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="get-service-health"></a>Početak stanje servisa
Vraća stanje servisa entitet. Sadrži stanje stanja particije. Unos:

- [Traženi] Naziv usluge (URI) koja služi za identifikaciju servis.

- [Neobavezni] Pravilnik za aplikacije stanja koristi da biste nadjačali pravilnik manifesta aplikacije.

- [Neobavezni] Filtri za događaja i particije koji određuju koji stavke koja vas zanima i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji i particije koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste dobili stanje servisa putem na API-JA, stvorite je `FabricClient` i pozivanje metodu [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) na njegov HealthManager.

Sljedeći primjer dobiva stanje servisa s nazivom navedeni servis (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Sljedeći kod dobiva stanje servisa za navedenog naziva servisa (URI), navodeći filtre i prilagođenog pravilnika putem [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)sljedeće:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanje servisa je [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Sljedeći cmdlet dobiva stanje servisa pomoću zadanog stanja pravila:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>OSTALE
Možete dobiti stanje servisa na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707609.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707646.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="get-partition-health"></a>Početak particija stanja
Vraća stanja particija entitet. Sadrži stanja replike stanja. Unos:

- [Traženi] Particija ID (GUID) koja služi za identifikaciju particije.

- [Neobavezni] Pravilnik za aplikacije stanja koristi da biste nadjačali pravilnik manifesta aplikacije.

- [Neobavezni] Filtri za događaja i replike koji određuju koji stavke koja vas zanima i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji i replike koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste dobili particija stanja kroz na API-JA, stvorite je `FabricClient` i pozivanje metodu [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) na njegov HealthManager. Da biste odredili neobavezni parametri, stvorite [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanja particija je [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Sljedeći cmdlet dobiva stanja za sve particije od na **tkanina: / WordCount/WordCountService** servisa:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>OSTALE
Možete dobiti stanja particije na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707683.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707680.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="get-replica-health"></a>Početak replike stanja
Vraća stanju s praćenjem stanja servisa replike ili instancu bez praćenja stanja servisa. Unos:

- [Traženi] Particija ID (GUID) i replike ID koja služi za identifikaciju replike.

- [Neobavezni] Aplikacija stanja pravila parametre koristiti da biste nadjačali manifesta pravila za programe.

- [Neobavezni] Filtri za događaje koje odredite koje stavke su od interesa i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste dobili stanja replike kroz na API-JA, stvorite je `FabricClient` i pozivanje metodu [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) na njegov HealthManager. Da biste odredili napredne parametara, koristite [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanja replike je [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Sljedeći cmdlet dobiva stanja primarni replike za sve particije servisa:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>OSTALE
Možete dobiti replike stanja na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707673.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707641.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="get-deployed-application-health"></a>Dohvaćanje aplikacije distribuiranih stanja
Vraća stanja aplikacije implementiran na čvor entitet. Sadrži stanja stanje servisa distribuiranih paketa. Unos:

- [Traženi] Naziv aplikacije (URI) i naziv čvora (niz) koje označavaju distribuiranih aplikacije.

- [Neobavezni] Pravilnik za aplikacije stanja za nadjačati pravilnike manifesta aplikacije.

- [Neobavezni] Filtri za događaja i paketi distribuiranih servis koji odredite koje stavke su od interesa i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji i distribuiranih servisa paketa koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste dobili stanja aplikacije implementiran na čvor kroz na API-JA, stvorite je `FabricClient` i pozivanje metodu [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) na njegov HealthManager. Da biste odredili neobavezni parametri, koristite [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanja distribuiranih aplikacija je [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Da biste saznali gdje je implementirati aplikaciju, pokrenite [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) i pogledajte podređenih distribuiranih aplikacije.

Sljedeći cmdlet dobiva stanju na **tkanina: / WordCount** aplikacije u uveden u **_Node_2**.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>OSTALE
Možete dobiti stanja distribuiranih aplikacije na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707644.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707688.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="get-deployed-service-package-health"></a>Početak stanje paketa distribuiranih servisa
Vraća stanja entitet paketa distribuiranih servisa. Unos:

- [Traženi] Naziv aplikacije (URI), čvor naziva (niz) i servis manifesta naziv (niz) koje označavaju paketa distribuiranih servisa.

- [Neobavezni] Pravilnik za aplikacije stanja koristi da biste nadjačali pravilnik manifesta aplikacije.

- [Neobavezni] Filtri za događaje koje odredite koje stavke su od interesa i bi trebala biti vraćena u rezultatu (Ako, na primjer, samo pogreške ili upozorenja i pogreške). Svi događaji koriste se za procjenu stanja entitet pridružuje, bez obzira na to filtar.

### <a name="api"></a>API-JA
Da biste dobili stanja paket distribuiranih servisa putem na API-JA, stvorite je `FabricClient` i pozivanje metodu [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) na njegov HealthManager. Da biste odredili neobavezni parametri, koristite [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili paket stanje distribuiranih servisa je [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Da biste vidjeli gdje je implementirati aplikaciju, pokrenite [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) i pogledajte distribuiranih aplikacije. Da biste vidjeli koji servis za pakete su u aplikaciji, pogledajte podređenih paketa distribuiranih servisa za [Dohvaćanje ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) izlaz.

Sljedeći cmdlet dobiva stanja **WordCountServicePkg** servisa paket na **tkanina: / WordCount** aplikacije u uveden u **_Node_2**. Entitet ima **System.Hosting** izvješća za uspješan paketa za usluge i točka unosa aktivaciju i uspješno Registracija vrsta servisa.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>OSTALE
Možete dobiti stanje paketa distribuiranih servisa na [Početak zahtjev](https://msdn.microsoft.com/library/azure/dn707677.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/dn707689.aspx) koja sadrži pravila stanja opisane u tijelu.

## <a name="health-chunk-queries"></a>Stanje bloka upita
Upiti bloka stanja možete se vratiti na više razina klaster djece (rekurzivno) po unos filtre. Podržava dodatne filtre koji omogućuju koliko fleksibilnost da biste izrazili koje određene djece se vraćaju otkrije njihove Jedinstveni identifikator ili druge identifikator grupe i/ili stanja. Po zadanom uključene, umjesto stanja naredbe koje uvijek uvrstite djece prve razine su nema podređenih.

[Stanje upita](service-fabric-view-entities-aggregated-health.md#health-queries) vratiti samo prve razine djece navedeni entitet po potrebne filtre. Da biste dobili djece podređenih, morate pozovite dodatne stanja API-ji za svaki entitet koji vas zanimaju. Isto tako, da biste dobili stanja određene entiteti, morate nazovite jedan stanja API-JA za svaki željeni entitet. U bloku upit Napredno filtriranje omogućuje da biste zatražili više stavki od interesa upita, minimiziranje veličinu poruke i broj poruka.

Vrijednost parametra upita bloka je koje možete dobiti stanja za dodatne klaster entiteti (potencijalno sve klaster entiteti počevši od obavezan korijenski) u jedan poziv. Složena stanje upita možete express kao što su:

- Vraćanje samo aplikacije na pogreške, kao i za tim aplikacijama sadržavati sve servise na upozorenje | pogreške. Za vraćeni servise obuhvaća sve particije.

- Vratite stanje 4 aplikacija određen njihova imena.

- Vratite stanje aplikacija vrste željenu aplikaciju.

- Da biste vratili sve distribuiranih entiteti na čvor. Vraća sve aplikacije, sve implementiran aplikacije na navedeni čvor i sve pakete distribuiranih servisa na tom čvor.

- Vrati sve replike kod pogreške. Vraća sve aplikacije, usluge, particije i replike kod pogreške.

- Vrati sve aplikacije. Za navedeni servis obuhvaća sve particije.

Trenutno stanje upita bloka se prikazuju samo za entitet klaster. Vraća bloka stanja klaster koja sadrži:

- Klaster pridružuje stanja.

- Stanje stanje bloka popis čvorove koji poštuje unos filtre.

- Stanje stanje bloka popis aplikacije koje poštuje unos filtre. Svaki blok za aplikaciju stanja stanje sadrži popis bloka sa svim servisima poštuje unos filtre i bloka popis svih distribuiranih aplikacije koje poštuje filtre. Isto za djecu servisa i distribuiranih aplikacije. Na taj način, svi entiteti u klasteru može potencijalno vratiti ako ste tražili, hijerarhijski način.

### <a name="cluster-health-chunk-query"></a>Klaster stanja bloka upita
Vraća stanja klaster entitet, a sadrži blokova stanje hijerarhijski stanja potrebne komponente. Unos:

- [Neobavezni] Stanje klaster pravila za procjenu čvorove i događaje klaster.

- [Neobavezni] Aplikacija stanja pravila karti s pravilima o stanju za nadjačati pravilnike manifesta aplikacije.

- [Neobavezni] Filtri za čvorove i aplikacije koje odredite koje stavke su od interesa i bi trebala biti vraćena u rezultatu. Filtri karakteristične za entitet/grupe entiteti ili se primijeniti na sve entiteti te razine. Popis filtara može sadržavati jednim filtrom Općenito i/ili filtri za određene identifikatora za precizno Žitne entiteti vratio upit. Ako je prazan, podređenih ne vraćaju se po zadanom.
Saznajte više o primjeni filtra na [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) i [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). U aplikaciji filtara može rekurzivno navedite dodatne filtre za djecu.

Rezultat bloka obuhvaća podređenih koji poštuje filtre.

Trenutno bloka upit vraća dobro procjene ili entitet događaja. Koje dodatne informacije mogu dobiveni pomoću postojeći upit stanja klaster.

### <a name="api"></a>API-JA
Da biste dobili klaster stanja bloka, stvorite je `FabricClient` i pozivanje metodu [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) na **HealthManager**. Možete proslijediti u [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) opisuje pravila stanja i napredni filtri.

Sljedeći kod dohvaća klaster stanja bloka s dodatne filtre.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet da biste dobili stanja klaster je [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Najprije povezati s klaster pomoću cmdleta [Za povezivanje ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

Sljedeći kod dohvaća čvorove samo ako su na pogreške osim određene čvor koje uvijek bi trebala biti vraćena.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Sljedeći cmdlet dohvaća klaster bloka s filtrima aplikacije.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Sljedeći cmdlet vraća svi distribuiranih entiteti čvor.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>OSTALE
Možete dobiti klaster stanja bloka na [Početak zahtjev](https://msdn.microsoft.com/library/azure/mt656722.aspx) ili [zahtjev za objavu](https://msdn.microsoft.com/library/azure/mt656721.aspx) koja sadrži pravila stanja i naprednih filtara opisana u tijelu.

## <a name="general-queries"></a>Općenito upita
Općenito upiti vratiti popis servisa tkanina entiteti navedene vrste. Oni su vidljiva kroz API-JA (putem metode na **FabricClient.QueryManager**), PowerShell cmdleti i OSTALE. Te upite aggregate podupiti iz više komponenti. Jedan od njih je na [Stanje pohraniti](service-fabric-health-introduction.md#health-store), koja popunjava Zbrojeno stanja za svaki rezultata upita.  

> [AZURE.NOTE] Općenito upiti vratili Zbrojeno stanja entitet i sadrže podatke obogaćenog stanja. Ako je entitet dobar, možete se daljnji rad s upitima stanja da biste saznali sve njegove stanja, uključujući događaje, podređeni stanja Državama i dobro procjene.

Ako općenito upiti vratili Nepoznato stanja za entitet, moguće je spremište stanja nema dovršeno podatke o entitet. Moguće je i nije li podupita spremište stanja uspješno (na primjer, pojavila se pogreška komunikacije ili ograničio je vrijeme spremišta stanja). Upute za daljnji rad s upitom o stanju za entitet. U slučaju podupit tranzitne pogreške, kao što su problemi s mrežom, možda će uspjeti ovaj upit za daljnji rad. Možda i omogućit vam više detalja iz trgovine stanja o Zašto entitet ne prikazuje.

Upiti koji sadrže **HealthState** za entiteti su:

- Popis čvor: vraća čvorove popisa u skupini (Straničeno).
  - API-JA: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShell: Get-ServiceFabricNode
- Popis aplikacija: vraća popis aplikacija u skupini (Straničeno).
  - API-JA: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricApplication
- Popis servisa: vraća popis servisa u aplikaciji (Straničeno).
  - API-JA: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShell: Get-ServiceFabricService
- Popis particija: vraća popis razdjeljivanja za uslugu (Straničeno).
  - API-JA: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShell: Get-ServiceFabricPartition
- Popis replike: vraća popis replike particija (Straničeno).
  - API-JA: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShell: Get-ServiceFabricReplica
- Uvesti popis aplikacija: vraća popis distribuiranih aplikacija na čvor.
  - API-JA: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication
- Uvesti popis servisnih paketa: vraća popis servisa paketa u distribuiranih računala.
  - API-JA: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Neke od upiti vratiti Straničeno rezultate. Vraćanje tih upita je popis izvedene iz [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Ako rezultate stane poruke, vraća se na stranicu, a na ContinuationToken koja prati gdje se zaustavio numeriranje. Nastavite da biste pozvali istog upita ili prenesite u nastavku tokena iz prethodne upit da biste se sljedeći rezultati.

### <a name="examples"></a>Primjeri

Sljedeći kod dobiva dobro aplikacije u klasteru sljedeće:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Sljedeći cmdlet dobiva detalje aplikacije za na tkanina: / WordCount aplikacije. Primijetite da stanja pri upozorenje.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Sljedeći cmdlet dobiva usluge s ugovorima stanja Upozorenje:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Nadograđuje klaster i aplikacija
Tijekom nadogradnje nadzirane klaster i aplikacije servisa tkanina provjere stanja da biste bili sigurni da će sve ostane dobar. Ako je entitet dobro kao procjenjuju pomoću pravila konfigurirani stanja nadogradnje primjenjuje specifične za nadogradnju pravila da biste odredili sljedeću akciju. Nadogradnja je možda zaustavljena dopustili interakcije s korisnikom (primjerice rješavanje stanja pogreške ili promjena pravila) ili ga može automatski vratiti na prethodnu verziju dobro.

Tijekom nadogradnje *klaster* , možete dobiti stanja nadogradnje klaster. Status nadogradnje sadrži dobro procjene koji pokazuju što je dobro u klasteru. Ako je Nadogradnja je vraćen zbog problema s stanja, stanja nadogradnje pamti posljednje dobro razloga. Ove informacije omogućuju administratorima istražiti čemu je problem nakon nadogradnje vraćen ili prekinuli.

Isto tako, tijekom nadogradnje *aplikacije* , sve dobro procjene nalaze se u stanja nadogradnje aplikacije.

Sljedeće prikazuje stanja nadogradnje aplikacije za izmijenjenu tkanina: / WordCount aplikacije. Na watchdog prijavio pogrešku na jednom od njezinih replike. Nadogradnja je vodoravnim jer provjere stanja su poštuju.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Saznajte više o [nadogradnje servisa tkanina aplikacije](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Rješavanje problema pomoću procjene stanja
Kad god je došlo do problema s klaster ili aplikaciju, pogledajte stanje klaster ili aplikacije da biste pinpoint što nije u redu. Dobro procjene sadrže pojedinosti o što pokrene trenutno stanje dobro. Ako je potrebno, možete se naniže u dobro podređeni entiteti da biste odredili uzrok.

> [AZURE.NOTE] Dobro procjene prikazuje prvi razloga entitet procjenjuje za trenutnog stanja. Mogu postojati više događaje koje aktiviraju to stanje, ali ih nije moguće odražavaju se u procjene. Da biste saznali više, naniže u entiteti stanja da biste utvrdili dobro izvješća u klasteru.

## <a name="next-steps"></a>Daljnji koraci
[Korištenje izvješća o stanju sustava da biste otklonili](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Dodavanje prilagođenih izvješća o stanju servisa tkanina](service-fabric-report-health.md)

[Upute za izvješća i provjerite stanje servisa](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Praćenje i dijagnosticiranje lokalno services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Nadogradnja aplikacije servisa tkanina](service-fabric-application-upgrade.md)
