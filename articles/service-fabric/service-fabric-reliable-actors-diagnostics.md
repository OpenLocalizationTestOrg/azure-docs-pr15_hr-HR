<properties
   pageTitle="Dijagnostika Glumci te praćenje | Microsoft Azure"
   description="U ovom se članku opisuju Dijagnostika i performanse nadzor značajke servisa tkanina pouzdanog Glumci izvođenja, uključujući događaja i mjerača performansi čuje tako da ga."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Za dijagnostiku i praćenje performansi za pouzdan Glumci
Izvođenje pouzdanog Glumci emits [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) događaja i [mjerača performansi](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Te navedite uvida u koliko je vremena izvođenja radi i pomoć pri otklanjanju poteškoća i praćenje performansi.

## <a name="eventsource-events"></a>EventSource događaja
Naziv davatelja EventSource za izvođenje pouzdanog Glumci je "Microsoft-ServiceFabric-Glumci". Događaje iz ovog izvora događaj prikazuju se u prozoru [Dijagnostika događaja](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) kada aplikacija glumca se [debugged u Visual Studio](service-fabric-debugging-your-application.md).

Primjeri Alati i tehnologije koji pomažu u prikupljanje i/ili prikaz događaji EventSource su [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Dijagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md), [Semantičkog zapisivanje](https://msdn.microsoft.com/library/dn774980.aspx)i [U biblioteci Microsoft TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Ključne riječi
Svi događaji koje pripadaju pouzdanog EventSource Glumci povezane su s jednog ili više ključnih riječi. Time se omogućuje filtriranje događaja koji se prikupljaju. Definirani su sljedeće bitova ključnih riječi.

|Bitne|Opis|
|---|---|
|0x1|Skup važnim događajima koji daju sažeti operacija runtime tkanina Glumci.|
|0x2|Postavljanje događaja koji se opisuju glumca način pozive. Dodatne informacije potražite u članku [uvodnu temu na Glumci](service-fabric-reliable-actors-introduction.md#actors).|
|0x4|Skup događaje vezane uz stanje glumca. Dodatne informacije potražite u temi [Upravljanje glumca stanje](service-fabric-reliable-actors-state-management.md).|
|0x8|Skup događaje vezane uz utemeljen na Uključi istodobnosti u na glumca. Dodatne informacije potražite u temi [istodobnosti](service-fabric-reliable-actors-introduction.md#concurrency).|

## <a name="performance-counters"></a>Mjerača performansi
Izvođenje pouzdanog Glumci definira sljedeće kategorije brojač performansi.

|Kategorija|Opis|
|---|---|
|Servis tkanina glumca|Mjerača specifične za tkanina servisa Azure Glumci, primjerice vremena za spremanje glumca stanja|
|Način glumca tkanina servisa|Mjerača određene metode Glumci tkanina servisa, npr učestalosti način glumca se poziva|

Svaki od iznad kategorije ima jednu ili više mjerača.

Prikupljanje i prikaz podataka o performansama brojač može se koristiti aplikacije [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) koji je dostupan po zadanom u operacijskom sustavu Windows. [Dijagnostika Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) je neku drugu mogućnost za prikupljanje podataka o performansama brojač i prijenos Azure tablice.

### <a name="performance-counter-instance-names"></a>Performanse brojač instance imena
Klaster koja sadrži velik broj glumca servise ili glumca servisa particije će imati velikog broja glumca performanse brojač instance. Instance imena brojač performanse mogu olakšati prepoznavanje određeni način [particija](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) i glumca (Ako je primjenjivo) brojač instancu izvedbe pridruženo.

#### <a name="service-fabric-actor-category"></a>Servis tkanina glumca kategorija
Kategorije `Service Fabric Actor`, brojač instance imena su u sljedećem obliku:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* je prikaz niza servisa tkanina particija ID-a koji je pridružen brojač instancu izvedbe. ID particije je GUID i njegov prikaz niza generira se kroz na [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) način specifikatorom oblik "D".

*ActorRuntimeInternalID* je prikaz niza 64-bitnu cjelobrojnu koji je generirao runtime tkanina Glumci za njegov internu upotrebu. To je sve obuhvaćeno naziv instance brojač performanse da biste osigurali njegov jedinstvenosti i izbjegli sukob s druge nazive za performanse brojač instance. Korisnici moraju pokušajte tumačenje ovaj dio naziv instance brojač performansi.

Slijedi primjer naziv instance brojač brojač koji pripada u `Service Fabric Actor` kategorija:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

U primjeru iznad, `2740af29-78aa-44bc-a20b-7e60fb783264` je prikaz niza particija ID tkanina servisa i `635650083799324046` je 64-bitni ID koji je generirao za vremena izvođenja je interna koristiti.

#### <a name="service-fabric-actor-method-category"></a>Način glumca tkanina servisa kategorija
Kategorije `Service Fabric Actor Method`, brojač instance imena su u sljedećem obliku:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*Naziva metode* je naziv glumca način koji je pridružen brojač instancu izvedbe. Oblikovanje prema nazivu metode određuje na temelju neke logike u vrijeme izvođenja tkanina Glumci koji salda čitljivosti naziv s ograničenja Maksimalna duljina performanse brojač instance imena u sustavu Windows.

*ActorsRuntimeMethodId* je prikaz niza 32-bitnu cjelobrojnu koji je generirao runtime tkanina Glumci za njegov internu upotrebu. To je sve obuhvaćeno naziv instance brojač performanse da biste osigurali njegov jedinstvenosti i izbjegli sukob s druge nazive za performanse brojač instance. Korisnici moraju pokušajte tumačenje ovaj dio naziv instance brojač performansi.

*ServiceFabricPartitionID* je prikaz niza servisa tkanina particija ID-a koji je pridružen brojač instancu izvedbe. ID particije je GUID i njegov prikaz niza generira se kroz na [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) način specifikatorom oblik "D".

*ActorRuntimeInternalID* je prikaz niza 64-bitnu cjelobrojnu koji je generirao runtime tkanina Glumci za njegov internu upotrebu. To je sve obuhvaćeno naziv instance brojač performanse da biste osigurali njegov jedinstvenosti i izbjegli sukob s druge nazive za performanse brojač instance. Korisnici moraju pokušajte tumačenje ovaj dio naziv instance brojač performansi.

Slijedi primjer naziv instance brojač brojač koji pripada u `Service Fabric Actor Method` kategorija:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

U primjeru iznad, `ivoicemailboxactor.leavemessageasync` je naziv metode `2` je ID za 32-bitni za internu upotrebu u runtime generira `89383d32-e57e-4a9b-a6ad-57c6792aa521` je prikaz niza particija ID tkanina servisa i `635650083804480486` je Identifikator 64-bitni generira za vremena izvođenja je interna koristiti.

## <a name="list-of-events-and-performance-counters"></a>Popis događaja i mjerača performansi

### <a name="actor-method-events-and-performance-counters"></a>Događaji način glumca i mjerača performansi
Izvođenje pouzdanog Glumci emits sljedeće događaje vezane uz [glumca metode](service-fabric-reliable-actors-introduction.md#actors).

|Naziv događaja|ID događaja|Razina|Ključne riječi|Opis|
|---|---|---|---|---|
|ActorMethodStart|7|Opširno|0x2|Izvođenje Glumci uskoro će pozivanje način glumca.|
|ActorMethodStop|8|Opširno|0x2|Način provjere glumca je završio izvršavanja. To jest, na runtime asinkronog poziva glumca način je vratio, a zadatak koji je vratio glumca način je završio.|
|ActorMethodThrewException|9|Upozorenje|0x3|Izbačena je iznimka tijekom izvođenja način glumca tijekom poziva na runtime asinkronog korakom glumca ili tijekom izvođenja zadatka vratio glumca način. Ovaj događaj označava neke sortiranje neuspješna instalacija u glumca kod koji je potrebno istrage.|

Izvođenje pouzdanog Glumci objavljuje sljedeće mjerača performansi vezane uz izvođenja glumca metode.

|Naziv kategorije|Naziv brojač|Opis|
|---|---|---|
|Način glumca tkanina servisa|Instanci/Sec|Broj koji se način servisa glumca poziva u sekundi|
|Način glumca tkanina servisa|Prosječna milisekundi po poziva|Vrijeme za izvršavanje način servisa glumca u milisekundama|
|Način glumca tkanina servisa|Izbačena iznimke/Sec|Koliko je puta na glumca servis je način izbacio je iznimku u sekundi|

### <a name="concurrency-events-and-performance-counters"></a>Događaji istodobnosti i mjerača performansi
Izvođenje pouzdanog Glumci emits sljedeće događaje vezane uz [istodobnosti](service-fabric-reliable-actors-introduction.md#concurrency).

|Naziv događaja|ID događaja|Razina|Ključne riječi|Opis|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Opširno|0x8|Na početku svake nove uključivanje u glumca napisan ovaj događaj. Sadrži broj na čekanju glumca pozive koji čekaju na zaključati po glumca koji nameće utemeljen na Uključi istodobnosti.|

Izvođenje pouzdanog Glumci objavljuje sljedeće mjerača performansi vezane uz istodobnosti.

|Naziv kategorije|Naziv brojač|Opis|
|---|---|---|
|Servis tkanina glumca|# glumca poziva na čekanje glumca lock|Broj na čekanju glumca poziva na čekanju u zaključati po glumca koji nameće utemeljen na Uključi istodobnosti|
|Servis tkanina glumca|Pričekajte AVERAGE milisekundama po lock|Vrijeme (u milisekundama) u zaključati po glumca koji nameće utemeljen na Uključi istodobnosti|
|Servis tkanina glumca|Sadrži zaključavanje glumca AVERAGE milisekundama|Vrijeme (u milisekundama) za koje je sadrži zaključavanje po glumca|

### <a name="actor-state-management-events-and-performance-counters"></a>Glumca stanje upravljanja događaja i mjerača performansi
Izvođenje pouzdanog Glumci emits sljedeće događaje vezane uz [Upravljanje glumca stanje](service-fabric-reliable-actors-state-management.md).

|Naziv događaja|ID događaja|Razina|Ključne riječi|Opis|
|---|---|---|---|---|
|ActorSaveStateStart|10|Opširno|0x4|Izvođenje Glumci uskoro će spremiti glumca stanje.|
|ActorSaveStateStop|11|Opširno|0x4|Izvođenje Glumci je završio spremanja glumca stanje.|

Izvođenje pouzdanog Glumci objavljuje sljedeće mjerača performansi vezane uz upravljanje glumca stanje.

|Naziv kategorije|Naziv brojač|Opis|
|---|---|---|
|Servis tkanina glumca|Prosječna milisekundama po spremanje stanja operacije|Vrijeme za spremanje glumca stanja u milisekundama|
|Servis tkanina glumca|Prosječna milisekundama po operaciji stanje učitavanja|Vrijeme za učitavanje glumca stanja u milisekundama|

### <a name="events-related-to-actor-replicas"></a>Događaje vezane uz glumca replike
Izvođenje pouzdanog Glumci emits sljedeće događaje vezane uz [glumca replike](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors).

|Naziv događaja|ID događaja|Razina|Ključne riječi|Opis|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Informativna|0x1|Replike glumca promijeniti uloga u primarni. To podrazumijeva da Glumci za particija stvorit će se unutar ovaj replike.|
|ReplicaChangeRoleFromPrimary|2|Informativna|0x1|Replike glumca promijeniti uloge koje nisu primarni. To podrazumijeva da Glumci za particija više ne mogu stvarati unutar ovaj replike. Bez novih zahtjeva za isporuku će se Glumci već stvorene u ovom replike. Na Glumci će uništavanje kada se dovrše sve zahtjeve u tijeku.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktivacija i deaktivacija događaje glumca i mjerača performansi
Izvođenje pouzdanog Glumci emits sljedeće događaje vezane uz [glumca Aktivacija i deaktivacija](service-fabric-reliable-actors-lifecycle.md).

|Naziv događaja|ID događaja|Razina|Ključne riječi|Opis|
|---|---|---|---|---|
|ActorActivated|5|Informativna|0x1|Glumca je aktiviran.|
|ActorDeactivated|6|Informativna|0x1|Glumca je deaktiviran.|

Izvođenje pouzdanog Glumci objavljuje sljedeće mjerača performansi vezane uz glumca Aktivacija i deaktivacija.

|Naziv kategorije|Naziv brojač|Opis|
|---|---|---|
|Servis tkanina glumca|Prosječna OnActivateAsync milisekundama|Vrijeme za izvršavanje OnActivateAsync način u milisekundama|

### <a name="actor-request-processing-performance-counters"></a>Obrada zahtjeva za glumca mjerača performansi
Kada klijent pozove metode putem proxy objekta glumca, rezultat u poruci zahtjeva za usluge glumca šalje putem mreže. Servis obrađuje poruci zahtjeva i šalje odgovor klijent. Izvođenje pouzdanog Glumci objavljuje sljedeće mjerača performansi vezane uz obrada zahtjeva za glumca.

|Naziv kategorije|Naziv brojač|Opis|
|---|---|---|
|Servis tkanina glumca|# preostalu zahtjeva|Broj zahtjeva koji se obrađuju u servisu|
|Servis tkanina glumca|Prosječna milisekundama po zahtjev|Vrijeme (u milisekundama) servis obrade zahtjeva|
|Servis tkanina glumca|Prosječna MS deserijalizacija zahtjev|Vrijeme (u milisekundama) za ukloniti serijski broj poruka sa zahtjevom za glumca se po primitku na servis|
|Servis tkanina glumca|Prosječna MS serijalizacije odgovor|Vrijeme (u milisekundama) serijalizirati glumca poruku odgovora na servis prije slanja odgovora na klijentu|

## <a name="next-steps"></a>Daljnji koraci
 - [Kako koristiti pouzdanog Glumci tkanina servisa platforme](service-fabric-reliable-actors-platform.md)
 - [Dokumentacija referenca glumca API-JA](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Ogledni kod](https://github.com/Azure/servicefabric-samples)
