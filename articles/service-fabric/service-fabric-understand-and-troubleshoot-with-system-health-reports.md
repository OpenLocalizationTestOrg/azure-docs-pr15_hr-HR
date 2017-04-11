<properties
   pageTitle="Rješavanje problema s izvješća o stanju sustava | Microsoft Azure"
   description="U članku se opisuje izvješća o stanju poslao komponente tkanina servisa Azure i njihove upotrebe za otklanjanje poteškoća klaster ili problema s računala."
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

# <a name="use-system-health-reports-to-troubleshoot"></a>Korištenje izvješća o stanju sustava da biste otklonili

Azure tkanina servisa komponente izvješća iz okvir na Svi entiteti u klasteru. [Spremanje stanja](service-fabric-health-introduction.md#health-store) stvara i briše entiteti koji se temelji na sustavu izvješća. Također ih organizira u hijerarhiji koji opisuje entitet interakcije.

> [AZURE.NOTE] Da biste shvatili koncepte vezanih stanju, pročitajte više u [modelu stanje servisa tkanina](service-fabric-health-introduction.md).

Izvješća o stanju sustava omogućuju uvid u klaster i funkcionalnosti aplikacije i zastavice poteškoća pomoću stanja. Za aplikacije i servise, izvješća o stanju sustava provjerite entiteti primjenjuju te se ponaša pravilno iz perspektive tkanina servisa. Izvješća pružaju sve nadzor stanja poslovne logike servisa ili otkrivanje procesa koji se ne reagira. Korisnički servisi možete obogaćivanje podataka stanja informacije specifične za svoje logike.

> [AZURE.NOTE] Izvješća o stanju watchdogs su vidljivi samo *Kada* komponente sustava stvoriti entitet. Nakon brisanja entitet spremište stanja automatski briše sve izvješća o stanju koje su povezane s njom. Isto vrijedi i stvaranja nove instance programa entitet (na primjer, nove instance replike servisa je stvorena). Sva izvješća pridružene stare instancu se briše i očistiti iz trgovine.

Izvješća komponente sustava prepoznaju izvor započinje s "**sustava.**" prefiks. Watchdogs nije moguće koristiti isti prefiks izvora, kao što su izvješća s neispravni parametri odbijena.
Pogledajmo neke izvješća sustava da biste razumjeli što ih pokreće i kako ispraviti moguće probleme predstavljaju.

> [AZURE.NOTE] Tkanina servisa i dalje da biste dodali izvješća na određene uvjete koji vas zanimaju poboljšati uvid u što se događa u klaster te aplikacije.

## <a name="cluster-system-health-reports"></a>Skupine izvješća o stanju sustava
Stanje entitet klaster automatski se stvara u spremištu stanja. Ako sve radi ispravno, ne obuhvaća izvješća sustava.

### <a name="neighborhood-loss"></a>Gubitak susjedstvo
**System.Federation** prijavljuje pogrešku kada utvrdi susjedstvo gubitka. Izvješće je poslao pojedinačne čvorove i ID čvor je sve obuhvaćeno naziv svojstva. Ako jedan susjedstvo gube se na cijelu tkanina servisa, obično možete očekivati dva događaja (obje strane rupe izvješće). Ako se izgube više susjedstva, nema više događaja.

Izvješće navodi vremenskog ograničenja globalni Zakup kao vrijeme Live. Izvješće je ponovno poslan svakih pola TTL trajanje pod uvjetom da uvjet ostaje aktivan. Događaj se uklanjaju se automatski nakon isteka. Uklonite kada istekle ponašanje osigurava da izvješće je očistiti iz trgovine stanja pravilno, čak i ako je čvor izvješćivanja prema dolje.

- **ID-a izvora**: System.Federation
- **Svojstvo**: započinje **susjedstvo** i uključuje informacije čvor
- **Sljedeći koraci**: istražiti Zašto se gube četvrti (Ako, na primjer, provjerite i komunikaciju između klaster čvorove).

## <a name="node-system-health-reports"></a>Izvješća o stanju sustava za čvor
**System.FM**, koji predstavlja servis upravitelja za prebacivanje je izdavanje koja upravlja informacije o klaster čvorove. Svaki čvor mora sadržavati jedno izvješće iz System.FM prikazuje stanje. Entiteti čvor uklanjaju se u stanju čvor nestaje (pogledajte [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Čvor gore/dolje
System.FM izvješća kao u redu kada čvor spaja Nazovi (to je s radom). Prijavljuje pogrešku kada čvor departs pozivanje (je prema dolje ili za nadogradnju ili jednostavno jer je nije uspio). Hijerarhija stanja ugrađen u spremištu stanja poduzima akcije na distribuiranih entiteti u korelacije System.FM čvor izvješća. Čvor se smatra pri virtualne nadređenog svi distribuiranih entiteti. Distribuiranih entiteti na tom čvor su vidljiva kroz upita ako čvor prijavljuje kao prema gore po System.FM s istoj instanci kao instanca pridružene entiteti. Kada System.FM izvješća da čvor prema dolje ili ponovno pokrenuti (novu instancu), u trgovini stanja automatski čisti distribuiranih entiteti koji se mogu nalaziti samo na čvor prema dolje ili prethodna instanca čvor.

- **ID-a izvora**: System.FM
- **Svojstvo**: stanje
- **Sljedeći koraci**: Ako je čvor isključen za nadogradnju, ga treba pojavljuju ponovno kada je nadograđena. U ovom slučaju stanja treba promijenio natrag na u redu. Ako čvor ne vratite ili ga ne uspije, problem potrebno više istrage.

Sljedeći primjer prikazuje System.FM događaj na stanja u redu za čvor:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Istek certifikata
**System.FabricNode** izvješća upozorenje kada su certifikati koriste čvor u blizini isteka. Postoje tri certifikati po čvor: **Certificate_cluster**, **Certificate_server**i **Certificate_default_client**. Kada je istek barem dva tjedna, izvješća stanja je u redu. Kada je istek unutar dva tjedna, vrstu izvješće će se upozorenje. TTL te događaja je beskonačno, a oni se uklanjaju kada čvor napusti klaster.

- **ID-a izvora**: System.FabricNode
- **Svojstvo**: započinje **certifikat** , a sadrži dodatne informacije o vrsta certifikata
- **Sljedeći koraci**: ažuriranje certifikate ako su blizu isteka.

### <a name="load-capacity-violation"></a>Učitavanje kršenja kapaciteta
Opterećenja servisa tkanina izvješća upozorenje otkrije čvor kapaciteta kršenja prava.

 - **ID-a izvora**: System.PLB
 - **Svojstvo**: započinje **kapaciteta**
 - **Sljedeći koraci**: Potvrdite navedene metriku, a zatim prikaz trenutnog kapaciteta na čvor.

## <a name="application-system-health-reports"></a>Izvješća o stanju sustava za aplikaciju
**System.CM**, koji predstavlja klaster Upravitelj servisa je izdavanje koja upravlja informacije o aplikacije.

### <a name="state"></a>Stanje
System.CM izvješća kao u redu kada Stvori ili ažurira aplikaciju. Obavještava spremište stanje nakon brisanja aplikacija tako da je uklonjena iz trgovine.

- **ID-a izvora**: System.CM
- **Svojstvo**: stanje
- **Sljedeći koraci**: Ako aplikacija je stvoren, trebali biste uključiti izvješće o stanju klaster Manager. U suprotnom provjeriti stanje aplikacije izdavanja upita (na primjer, cmdlet ljuske PowerShell * *Get-ServiceFabricApplication ApplicationName *applicationName***).

Sljedeći primjer prikazuje stanje događaja na **tkanina: / WordCount** aplikacije:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Izvješća o stanju sustava za servis
**System.FM**, koji predstavlja servis upravitelja za prebacivanje je izdavanje koja upravlja informacije o servisima.

### <a name="state"></a>Stanje
System.FM izvješća kao u redu kada je stvoreno je servis. Briše entitet iz trgovine stanja kada je servis koji je izbrisan.

- **ID-a izvora**: System.FM
- **Svojstvo**: stanje

Sljedeći primjer prikazuje stanje događaja na servis **tkanina: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Unplaced replike kršenja prava
**System.PLB** izvješća upozorenje ako formula ne može pronaći smještaj za jednu ili više usluga replike. Nakon što istekne, uklanjaju se izvješće.

- **ID-a izvora**: System.FM
- **Svojstvo**: stanje
- **Sljedeći koraci**: Provjera ograničenjima servisa i trenutno stanje položaj.

Sljedeći primjer prikazuje kršenja prava za uslugu s 7 replike cilj klasteru s 5 čvorove:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
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
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Izvješća o stanju sustava za partition
**System.FM**, koji predstavlja servis upravitelja za prebacivanje je izdavanje koja upravlja informacije o particije servisa.

### <a name="state"></a>Stanje
System.FM izvješća kao u redu, kada particija stvorena bude dobar. Briše entitet iz trgovine stanje nakon brisanja particije.

Ako je particija ispod replike Minimalni broj, javlja se pogreška. Ako particija nije ispod replike Minimalni broj, ali je ispod replike count cilj, izvješća će se upozorenje. Ako je particija gubitak kvorum, System.FM prijavljuje pogrešku.

Druge važne događaje obuhvaćaju upozorenje kada se ponovno konfiguriranje traje dulje nego je očekivano, a sastavljanje traje dulje nego je očekivano. Polje očekivane vremena za sastavljanje i ponovno konfiguriranje su konfigurirati na temelju scenarije servisa. Ako, na primjer, ako je neki servis terabajta stanja, kao što su baze podataka SQL sastavljanje traje dulje nego za servis mali stanja.

- **ID-a izvora**: System.FM
- **Svojstvo**: stanje
- **Sljedeći koraci**: Ako stanja nije u redu, moguće je da neke replike nisu je stvorili, otvoriti ili pravilno ulogu primarnog ili sekundarni. U više instanci uzrok je servis pogrešku u implementaciji Otvori ili promjena uloge.

Sljedeći primjer prikazuje dobar particije:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Sljedeći primjer prikazuje stanje particija ispod cilj replike count. Sljedeći je korak da biste dobili opis particije koji pokazuje kako je konfigurirano: **MinReplicaSetSize** je dva, a **TargetReplicaSetSize** brošure. Zatim se broj čvorovi u klasteru: pet. Stoga u tom slučaju dvije replike ne može smjestiti.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Replike povrede ograničenja
**System.PLB** izvješća upozorenje se on otkrije replike povrede ograničenja i ne možete smjestiti replike particije.

- **ID-a izvora**: System.PLB
- **Svojstvo**: započinje **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Izvješća o stanju sustava za replike
**System.RA**, koji predstavlja komponentu agent ponovno konfiguriranje je izdavanje za stanje replike.

### <a name="state"></a>Stanje
**System.RA** izvješća kao u redu kada je stvorena replike.

- **ID-a izvora**: System.RA
- **Svojstvo**: stanje

Sljedeći primjer prikazuje dobar replike:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Otvaranje status replike
Opis ovo izvješće o stanju sadrži početno vrijeme (Koordinirano univerzalno vrijeme) kada je poziv API pozvan.

**System.RA** izvješća upozorenje ako otvaranje replike traje dulje od konfigurirani razdoblja (zadani: 30 minuta). Ako se API utječe dostupnost servisa, izvješće izdaje mnogo brže (konfigurirati interval, sa zadanim od 30 sekundi). Vrijeme mjeri obuhvaća na vrijeme potrebno za otvorenu umnažanje i Otvori servisa. Ako Otvori dovrši, svojstvo se mijenja u u redu.

- **ID-a izvora**: System.RA
- **Svojstvo**: **ReplicaOpenStatus**
- **Sljedeći koraci**: stanja nije u redu, istražili Zašto replike Otvori traje dulje nego je očekivano.

### <a name="slow-service-api-call"></a>Poziv API servisa sporo
**System.RAP** i **System.Replicator** izvješće upozorenje ako poziv kod usluge korisnika traje dulje nego konfiguriranog vremena. Poništen je upozorenje kada se završi poziv.

- **ID-a izvora**: System.RAP ili System.Replicator
- **Svojstvo**: naziv sporo API-JA. Opis navedeni su dodatni Detalji o vremenu u API je na čekanju.
- **Sljedeći koraci**: istražiti Zašto traje dulje nego je očekivano poziv.

Sljedeći primjer prikazuje particije u kvorum gubitka i istrage korake gotovo da biste utvrdili zašto. Jedno od na replike je stanja upozorenje, da biste dobili njegov stanja. Pokazuje da postupak usluge traje dulje nego je očekivano događaja dojavi System.RAP. Nakon primitka te podatke na sljedeći korak je pogledati kod usluge i istražiti postoji. U ovom slučaju **RunAsync** implementaciju s praćenjem stanja servisa throws neobrađenu iznimku. Na replike su recikliranje tako da se možda nećete vidjeti sve replike u stanju upozorenje. Možete pokušajte početak stanja, a zatim potražite eventualne razlike u replike ID-u U određenim slučajevima u ponovne pokušaje mogu vam dati clues.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Kada pokrenete neispravan aplikacije u odjeljku program za ispravljanje pogrešaka, windows dijagnostičkih događaja Prikaži izbačena iz RunAsync:

![Visual Studio 2015 dijagnostičkih događaja: pogreška RunAsync u tkanina: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 dijagnostičkih događaja: pogreška RunAsync u **tkanina: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Red čekanja replikacije puni
**System.Replicator** izvješća upozorenje ako red replikacije pun. Na primarni, to obično događa jer je jedan ili više sekundarnog replike sporo svjesni operacije. Na sekundarnom, to obično događa kada servis je spor da biste primijenili operacije. Poništen je upozorenje kada reda čekanja više nije cijeli.

- **ID-a izvora**: System.Replicator
- **Svojstvo**: **PrimaryReplicationQueueStatus** ili **SecondaryReplicationQueueStatus**, ovisno o replike uloga

### <a name="slow-naming-operations"></a>Sporo imenovanja operacije

**System.NamingService** izvješća o stanju na njegov primarni replike kada imenovanje postupak traje dulje nego prihvatljiva. Primjeri imenovanje operacije su [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) ili [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Dodatne načine možete pronaći u odjeljku FabricClient, primjerice u odjeljku [servis za upravljanje metode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) ili [načina upravljanja svojstva](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] Servis za imenovanje razrješava nazivi servisa na mjesto u klasteru i korisnicima omogućuje upravljanje nazivi servisa i svojstva. Je servis tkanina particije ista i usluge. Jedna particija predstavlja vlasnik za izdavanje certifikata koje sadrži metapodatke o sva imena tkanina usluge i usluge. Nazivi servisa tkanina mapiraju se različite particije naziva particije ime vlasnika, pa je servis extensible. Saznajte više o [servisu za imenovanje](service-fabric-architecture.md).

Kada imenovanje postupak traje dulje nego je očekivano, postupak označit će se s izvješćem upozorenje na *primarni replike particije servisa imenovanje koji služi postupak*. Ako se operacija uspješno završi, upozorenje isključen. Ako dovršetak poruku o pogrešci, izvješće o stanju sadrži detalje o pogrešci.

- **ID-a izvora**: System.NamingService
- **Svojstvo**: započinje s prefiks **Duration_** i služi za identifikaciju spora operacije, a zatim naziv tkanina servis na kojemu se postupak primjenjuje. Na primjer, ako stvaranje servisa na naziv tkanina: / MyApp/MyService traje predugo, svojstvo je Duration_AOCreateService.fabric:/MyApp/MyService. AO upućuje na ulogama particije imenovanje za ovaj naziv i postupak.
- **Sljedeći koraci**: Potvrdite Zašto imenovanje operacija neće uspjeti. Svaka operacija može imati uzroci drugi korijen. Primjerice, izbrišite servis ostaju na čvor jer zadržava glavnog računala ruši na čvor pogreška korisnika u kod usluge.

Sljedeći primjer prikazuje operaciju servisa. Postupak traje dulje od trajanja konfigurirani. AO ponovnih pokušaja i šalje rad na ne. NE dovršiti zadnja operacija s vremenskog ograničenja. U ovom slučaju isti replike je primarni na AO i nema uloga.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>Izvješća o stanju sustava za DeployedApplication
**System.Hosting** je izdavanje na distribuiranih entiteti.

### <a name="activation"></a>Aktivacija
System.Hosting izvješća kao u redu kada aplikacija uspješno aktiviran na čvor. U suprotnom prijavljuje pogrešku.

- **ID-a izvora**: System.Hosting
- **Svojstvo**: aktivacija, uključujući uvođenje verziju
- **Sljedeći koraci**: Ako aplikacija nije dobro, istražili Zašto nije uspjela aktivaciju.

Sljedeći primjer prikazuje uspješno aktivaciju:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Preuzimanje
**System.Hosting** prijavljuje pogrešku ako ne uspije preuzimanje paketa aplikacije.

- **ID-a izvora**: System.Hosting
- **Svojstvo**: * *Preuzimanje:*RolloutVersion***
- **Sljedeći koraci**: istražiti Zašto preuzimanje nije uspjela čvor.

## <a name="deployedservicepackage-system-health-reports"></a>Izvješća o stanju sustava za DeployedServicePackage
**System.Hosting** je izdavanje na distribuiranih entiteti.

### <a name="service-package-activation"></a>Aktivacija servisa paketa
System.Hosting izvješća kao u redu ako aktivaciju paket servisa na čvor je uspješan. U suprotnom prijavljuje pogrešku.

- **ID-a izvora**: System.Hosting
- **Svojstvo**: aktivaciju
- **Sljedeći koraci**: istražiti Zašto nije uspjela aktivaciju.

### <a name="code-package-activation"></a>Aktivacija paketa kod
**System.Hosting** izvješća kao u redu za svaki paket kod ako aktivaciju je uspješan. Ako aktivaciju ne uspije, izvješća upozorenje konfigurirani. Ako **CodePackage** ne uspije da biste aktivirali ili završava s pogreškom veći od konfiguriranog **CodePackageHealthErrorThreshold**, hosting prijavljuje pogrešku. Ako je paket servisa sadrži više paketa kod, izvješće o aktivaciji se generira za svaki od njih.

- **ID-a izvora**: System.Hosting
- **Svojstvo**: koristi prefiks **CodePackageActivation** te sadrži naziv paketa kod i točka unosa kao * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/ulazna* ** (na primjer, **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Registracija vrsta usluge
**System.Hosting** izvješća kao u redu ako vrsta servisa registrirani uspješno. Ako registracija nije gotovo u vremenu (kao što je konfiguriran pomoću **ServiceTypeRegistrationTimeout**), prijavljuje pogrešku. Ako je vrsta servisa registriran s čvor, to je jer je zatvoren vrijeme izvođenja. Upozorenje se nalaze izvješća.

- **ID-a izvora**: System.Hosting
- **Svojstvo**: koristi prefiks **ServiceTypeRegistration** te sadrži naziv vrste usluge (primjerice, **ServiceTypeRegistration:FileStoreServiceType**)

Sljedeći primjer prikazuje paket dobar distribuiranih servisa:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Preuzimanje
**System.Hosting** prijavljuje pogrešku ako ne uspije preuzimanje paketa za servis.

- **ID-a izvora**: System.Hosting
- **Svojstvo**: * *Preuzimanje:*RolloutVersion***
- **Sljedeći koraci**: istražiti Zašto preuzimanje nije uspjela čvor.

### <a name="upgrade-validation"></a>Nadogradnja provjere valjanosti
**System.Hosting** prijavljuje pogrešku ako neuspjele provjere tijekom nadogradnje ili ne uspijete nadogradnju na čvor.

- **ID-a izvora**: System.Hosting
- **Svojstvo**: koristi prefiks **FabricUpgradeValidation** te sadrži verzija za nadogradnju
- **Opis**: upućuje na došlo je do pogreške

## <a name="next-steps"></a>Daljnji koraci
[Prikaz izvješća o stanju servisa tkanina](service-fabric-view-entities-aggregated-health.md)

[Upute za izvješća i provjerite stanje servisa](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Praćenje i dijagnosticiranje lokalno services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Nadogradnja aplikacije servisa tkanina](service-fabric-application-upgrade.md)
