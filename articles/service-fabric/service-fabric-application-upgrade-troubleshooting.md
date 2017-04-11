<properties
   pageTitle="Otklanjanje poteškoća s nadogradnje aplikacije | Microsoft Azure"
   description="U ovom se članku opisuje neke uobičajene probleme s oko nadogradnje servisa tkanina aplikacije i kako biste ih riješili."
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

# <a name="troubleshoot-application-upgrades"></a>Otklanjanje poteškoća prilikom nadogradnje aplikacije

U ovom se članku opisuje neke od uobičajenih problema oko nadogradnje aplikacije tkanina servisa Azure i kako biste ih riješili.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Otklanjanje poteškoća s nije uspjelo aplikacije nadogradnje

Prilikom nadogradnje ne uspije, izlazna naredbu **Get-ServiceFabricApplicationUpgrade** sadrži dodatne informacije za ispravljanje pogrešaka pogreške.  Na sljedećem su popisu Navedi kako se mogu koristiti dodatne informacije:

1. Odredite vrstu pogreške.
2. Odredite razlog neuspjeha.
3. Izdvajanje komponente neuspjeh za daljnje istrage.

Ove informacije dostupna je kada je servis tkanina otkrije neuspjeh bez obzira na to je li **FailureAction** vratiti ili odgoditi nadogradnje.

### <a name="identify-the-failure-type"></a>Prepoznavanje vrstu pogreške

U rezultatu **Get-ServiceFabricApplicationUpgrade** **FailureTimestampUtc** označava vremenske oznake (po UTC-u) na kojoj se neuspio nadogradnje otkrio tkanina servisa i **FailureAction** ga je pokrenula. **FailureReason** označava jedan od tri uzroka više razine potencijalne pogreške:

1. UpgradeDomainTimeout – znači da određene domene nadogradnje traje predugo, a zatim je istekla **UpgradeDomainTimeout** .
2. OverallUpgradeTimeout – znači da cjelokupan nadogradnje traje predugo, a zatim je istekla **UpgradeTimeout** .
3. HealthCheck - pokazuje da nakon nadogradnje ažuriranje domene aplikacija pojavili dobro prema pravila navedeni stanja i **HealthCheckRetryTimeout** istekle.

Te stavke samo prikazuju se u izlaz prilikom nadogradnje ne uspije, a pokreće povratak. Daljnje informacije prikazuju se ovisno o vrsti pogreške.

### <a name="investigate-upgrade-timeouts"></a>Istražite nadogradnje vremensko ograničenje

Nadogradite vremenskog ograničenja problemi sa servisom dostupnost najčešće uzrokuju pogreške. Izlaz praćenja ovog odlomka je uobičajeni nadogradnje gdje replike servisa ili instance se neće pokrenuti u novoj verziji kod. Polje **UpgradeDomainProgressAtFailure** snima snimke napravili čekanju nadogradnje u trenutku pojavljivanja pogreške.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

U ovom primjeru Nadogradnja nije uspjela pri nadogradnji domene *MYUD1* i su zamrzne dvije particije (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* i *4b43f4d8-b26b-424e-9307-7a7a62e79750*). Particije su zamrzne jer je nije moguće postaviti primarni replike (*WaitForPrimaryPlacement*) na ciljnom čvorove *Node1* i *Node4*vremena izvođenja.

Naredba **Get-ServiceFabricNode** se može koristiti za provjerite jesu li ove dvije čvorove nadogradnje domene *MYUD1*. *UpgradePhase* piše *PostUpgradeSafetyCheck*, što znači da su pojavljivanja te provjere sigurnost nakon sve čvorove u nadogradnje domene dovršite nadogradnje. Ove informacije upućuje na potencijalnih problema s novom verzijom šifre aplikacije. Najčešći problemi su pogreške servisa u Otvori ili promotivne ponude primarni kod putovi.

*UpgradePhase* od *PreUpgradeSafetyCheck* znači došlo je do problema s Priprema nadogradnje domenu prije nego što je izvršena. Najčešći problemi u tom slučaju su pogreške servisa u Zatvori ili smanjivanje razine iz primarnog kod putovi.

Trenutni **UpgradeState** je *RollingBackCompleted*tako izvorne nadogradnje morate su izvršene s vraćanjem **FailureAction**, koji je vraćen automatski Nadogradnja nakon nije uspjelo. Ako izvorni nadogradnje izvršena s ručno **FailureAction**, zatim nadogradnje bi umjesto biti u obustavljenom stanju uživo dopustili ispravljanje pogrešaka aplikacije.

### <a name="investigate-health-check-failures"></a>Istražite stanja provjere pogrešaka

Do neuspjele provjere stanja može aktivirati različite problemi koji se može dogoditi kada sve čvorove u nadogradnje domene dovršili nadogradnje i prosljeđivanje sve provjere sigurnost. Karakteristično nadogradnje kvara zbog provjere stanja nije uspjelo je rezultatu praćenja ovog odlomka. Polje **UnhealthyEvaluations** snima snimke stanja provjere koje nije uspjela prilikom nadogradnje prema navedenom [pravilo stanja](service-fabric-health-introduction.md).

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Najprije istražuje neuspjele provjere stanja, potrebno je poznavati modela stanje servisa tkanina. No bez kao što je detaljnije poznavati, možemo vidjeti da su dva servisa dobro: *tkanina: / DemoApp/Svc3* i *tkanina: / DemoApp/Svc2*, uz izvješća o stanju pogreške ("InjectedFault" u ovom slučaju). U ovom primjeru dva od tri su servisi dobro, koji se nalazi ispod zadani cilj 0% dobro (*MaxPercentUnhealthyServices*).

Nadogradnja je obustavljena nakon neuspjeh navođenjem **FailureAction** ručno prilikom pokretanja nadogradnje. Taj način omogućuje nam da biste istražili uživo sustavu nije uspjelo stanje prije poduzimanja akcija izvući.

### <a name="recover-from-a-suspended-upgrade"></a>Oporavak obustavljenom nadogradnje

S vraćanjem **FailureAction**postoji bez oporavak potrebne nakon nadogradnje automatski će vratiti nakon neuspjeh. Pomoću ručnog **FailureAction**postoji nekoliko mogućnosti za oporavak:

1. Ručno pokretanje vraćanje
2. Ručno prijeđite do ostatak nadogradnje
3. Nastavak nadziranim nadogradnje

Da biste pokrenuli vraćanje aplikacije mogu se naredba **Pokreni ServiceFabricApplicationRollback** u bilo kojem trenutku. Kada se naredba vraća uspješno, zahtjev za vraćanje registrirani u sustavu i pokreće uskoro nakon toga.

Naredba **Životopis ServiceFabricApplicationUpgrade** može se koristiti za ručno, nastavite kroz ostatak nadogradnje jednu domenu nadogradnje odjednom. U tom načinu samo sigurnost provjerava se izvode sustav. Nema više provjere stanja se izvode. Ta se naredba može koristiti samo kada *UpgradeState* prikazuje *RollingForwardPending*, što znači da je trenutnu domenu nadogradnje završio nadogradnje, ali sljedeći je počeo (na čekanju).

Naredba **Update ServiceFabricApplicationUpgrade** može se koristiti za nastavak nadziranim nadogradnje za oba sigurnost i stanje provjerava se izvodi.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Nadogradnja i dalje od nadogradnje domene gdje je zadnji put obustavljeno i koristi iste nadograditi parametara i pravila stanja kao prije. Ako je potrebno, bilo koji od nadogradnje parametara i stanje pravila prikazana u prethodnom izlaz može promijeniti u ista naredba prilikom nadogradnje primijenila. U ovom primjeru Nadogradnja je vraćen u načinu nadziranih s parametre i pravila stanja ne mijenja.

## <a name="further-troubleshooting"></a>Daljnje rješavanje problema

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Servis tkanina ne pratite pravila navedeni stanja

Mogući uzrok 1:

Servis tkanina prevodi sve postotaka u stvarni broj entiteti (Ako, na primjer, replike, particije i usluge) za procjenu stanja i uvijek zaokružuje na cijelu entiteti. Ako, na primjer, ako maksimalno _MaxPercentUnhealthyReplicasPerPartition_ je 21%, a postoji pet replike, tkanina servisa omogućuje do dva dobro replike (taj is,'Math.Ceiling (5\*0.21)). Stoga se pravila stanja treba sukladno tome postaviti.

Mogući uzrok 2:

Stanje pravila određeni su klauzulom pomoću postotaka ukupni servise i instanci nisu specifične servisa. Na primjer, prije nadogradnje, ako aplikacija ima četiri servisa instance A, B, C i D, pri čemu je servis D dobro, ali s učinka na aplikaciju. Želimo da biste zanemarili poznati dobro servis D tijekom nadogradnje i postavljanje parametar *MaxPercentUnhealthyServices* biti 25%, uz pretpostavku da samo A, B i C moraju biti dobar.

Međutim, tijekom nadogradnje, D može postati dobar dok C postaje dobro. Nadogradnja će i dalje uspjeti jer se samo 25% od servisa su dobro. Međutim, to može dovesti neočekivana pogreške zbog C se neočekivano dobro umjesto D. U tom slučaju D mora biti katalog modeliran kao servis za različite vrste iz A, B i C. Budući da pravila stanja određeni su klauzulom po vrsti servisa, drugi dobro postotak pragovi se mogu primijeniti različite servise. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>I navedite pravilo stanja za nadogradnju aplikacije, ali nadogradnje i dalje ne uspijeva za neke istek koja nikad ne određena

Kada je stanje pravila nisu navedene u zahtjev za nadogradnju, oni uzimaju se s *ApplicationManifest.xml* trenutne verzije aplikacija. Na primjer, Ako nadograđujete X aplikacije iz verzije 1.0 verzije 2.0, pravila stanja za programe navedene za u verziji 1.0 koriste. Ako pravilo stanja različite treba koristiti za nadogradnju, zatim pravilo mora biti navedena kao dio aplikacije nadogradnje API poziva. Pravila navedenom u sklopu API poziva mogu primijeniti samo tijekom nadogradnje. Nakon dovršetka nadogradnje pravila naveden u *ApplicationManifest.xml* koriste.

### <a name="incorrect-time-outs-are-specified"></a>Netočan istek određeni su klauzulom

Možda ste wondered o što se događa kada je istek nedosljedno. Na primjer, možda je *UpgradeTimeout* koja je manja od *UpgradeDomainTimeout*. Odgovor je vraća se pogreška. Ako *UpgradeDomainTimeout* je manji od zbroja *HealthCheckWaitDuration* i *HealthCheckRetryTimeout*ili *UpgradeDomainTimeout* je manji od zbroja *HealthCheckWaitDuration* i *HealthCheckStableDuration*, vraćaju se pogreške.

### <a name="my-upgrades-are-taking-too-long"></a>Moje nadogradnje su predugo traje

Vrijeme za nadogradnju da biste dovršili ovisi o provjere stanja i istek naveden. Provjera stanja i istek ovise o tome koliko dugo traje kopirajte, uvođenje i Stabiliziraj aplikacije. Previše izrazito s istek koja se katkad znači više nije uspjelo nadogradnje, stoga preporučujemo da conservatively počevši od više istek.

Evo brzi podsjetnik na način na koji se na istek rade s nadogradnjom vremena:

Nadograđuje za nadogradnju domene nije moguće završiti brže nego *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Pogreška nadogradnje ne može biti brže nego *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Nadogradnja vremena za nadogradnju domene ograničen *UpgradeDomainTimeout*.  Ako *HealthCheckRetryTimeout* i *HealthCheckStableDuration* su od nule, a stanje aplikacije stalno natrag i naprijed promjene, zatim nadogradnje naposljetku ističe na *UpgradeDomainTimeout*. *UpgradeDomainTimeout* pokreće brojeći nakon nadogradnje za trenutnu domenu nadogradnje započinje.

## <a name="next-steps"></a>Daljnji koraci

[Nadogradnja vašeg računala pomoću Visual Studio](service-fabric-application-upgrade-tutorial.md) vodit će vas kroz nadogradnju aplikacije pomoću Visual Studio.

[Nadogradnja aplikacije pomoću ljuske](service-fabric-application-upgrade-tutorial-powershell.md) vodit će vas kroz nadogradnju aplikacije pomoću komponente PowerShell.

Upravljanje kako nadograđuje aplikacije pomoću [Nadograditi parametara](service-fabric-application-upgrade-parameters.md).

Provjerite svoje nadogradnje aplikacije kompatibilne Naučite koristiti [Serijalizacije podataka](service-fabric-application-upgrade-data-serialization.md).

Saznajte kako koristiti napredne funkcije tijekom nadogradnje aplikacije za [Dodatne teme](service-fabric-application-upgrade-advanced.md).

Riješite uobičajene probleme u aplikaciji nadogradnje tako da upućuju na korake [Za otklanjanje poteškoća nadogradnje aplikacije](service-fabric-application-upgrade-troubleshooting.md).
 
