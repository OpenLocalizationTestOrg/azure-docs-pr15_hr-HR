<properties
   pageTitle="Napredno korištenje pouzdanog servisa | Microsoft Azure"
   description="Saznajte više o Napredno korištenje servisa tkanina pouzdanog servisa za dodatnu fleksibilnost pri servisa."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Napredno korištenje pouzdanog servisa programiranje modela
Azure servisa tkanina pojednostavljuje pisanje i upravljanje pouzdanog bez praćenja stanja i s praćenjem stanja servisa. Ovaj vodič objasniti će napredni upotrebe pouzdanog servisa da biste dobili dodatne kontrole i fleksibilnost putem servisa. Prije no što ovaj vodič za čitanje, upoznajte se sa [Servisima pouzdanog programiranje modela](service-fabric-reliable-services-introduction.md).

S praćenjem stanja i bez praćenja stanja servisa postoje dvije točke primarni unos koda korisnika:

 - `RunAsync`je općenite namjene ulaza za svoj servis kod.
 - `CreateServiceReplicaListeners`i `CreateServiceInstanceListeners` je za otvaranje komunikacije slušače klijent zahtjeva za.
 
Za većinu servise, potrebne su ove dvije ulazne točke. U slučajevima rijetko kada veću kontrolu nad životni ciklus na servis je obavezna, događaji dodatne životni ciklus su dostupne.

## <a name="stateless-service-instance-lifecycle"></a>Životni ciklus instancu bez praćenja stanja servisa

Životni ciklus bez praćenja stanja servisa je vrlo jednostavne. Bez praćenja stanja servisa može samo biti otvorena, zatvorite, ili je prekinuta. `RunAsync`u bez praćenja stanja servisa se izvršava kada se instanca servisa otvoriti, a prekinuto kada je instanca servisa zatvoren ili prekinuta. 

Iako `RunAsync` da bi se u gotovo svakom slučaju, otvaranje, zatvaranje i prekid događaja u bez praćenja stanja servisa su dostupne:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync naziva kada instancu bez praćenja stanja servisa uskoro će se koristiti. Zadaci Inicijalizacija prošireni servisa može se pokrenuti trenutno.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync naziva kada će biti bez poteškoća zatvoren instancu bez praćenja stanja servisa. To se može dogoditi prilikom nadogradnje servisa kod, instanca servisa premješten zbog opterećenja ili otkrije tranzitne kvara. OnCloseAsync se može koristiti za sigurno zatvorite sve resursa, prekinuti sve pozadinska obrada, završi spremanje vanjskih stanje ili zatvorite postojeće veze.

- `void OnAbort()`
  OnAbort naziva kada instancu bez praćenja stanja servisa se program prisilno isključuje. To se obično naziva kada se trajno kvara otkrije na čvor ili kada na servis tkanina pouzdano nije moguće upravljati životni ciklus instanca servisa zbog interne pogreške.

## <a name="stateful-service-replica-lifecycle"></a>Životni ciklus replike s praćenjem stanja servisa

Životni ciklus s praćenjem stanja servisa replike je puno složenije od instancu bez praćenja stanja servisa. Uz to za otvaranje, zatvaranje i prekid događaje, s praćenjem stanja servisa replike prolazi kroz promjena uloga tijekom njegova života. Kada se s praćenjem stanja servisa replike Promijeni ulogu, u `OnChangeRoleAsync` događaj aktivira:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync naziva kada replike s praćenjem stanja servisa se mijenja ulogu, primjerice primarni ili sekundarni. Primarni replike ponudit će vam pisanje status (dopušteno stvaranje i pisanje pouzdanog zbirke). Sekundarnog replike ponudit će statusa pročitanosti (može čitati samo iz postojećeg pouzdanog zbirki). Većina rada s praćenjem stanja servisa izvodi se na primarni replike. Sekundarnog replike mogu izvršiti samo za čitanje provjere valjanosti, generiranje izvješća, dubinsko pretraživanje podataka ili druge zadatke samo za čitanje.

U s praćenjem stanja servisa primarni replike dozvolu za pisanje stanje i stoga je obično kada servis izvršava stvarni posao. Na `RunAsync` način s praćenjem stanja servisu se izvršava samo kad je primarni replike s praćenjem stanja servisa. Na `RunAsync` način je otkazan promjenama primarni replike uloga izvan primarni, kao i tijekom događaja Zatvori i prekid. 

Korištenje na `OnChangeRoleAsync` događaj omogućuje izvođenje rada ovisno o replike uloga u kao i kao odgovor na promjena uloga.

S praćenjem stanja servisa i nudi iste četiri životni ciklus događaje kao bez praćenja stanja servisa s istu semantiku i korištenje slučajeva:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Daljnji koraci
Dodatne teme vezane uz servis tkanina, potražite u sljedećim člancima:

- [Konfiguriranje s praćenjem stanja servisa pouzdan](service-fabric-reliable-services-configuration.md)

- [Uvod tkanina servis stanja](service-fabric-health-introduction.md)

- [Pomoću izvješća o stanju sustava za otklanjanje poteškoća](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Konfiguriranje usluge s tkanina klaster resursa Upravitelj servisa](service-fabric-cluster-resource-manager-configure-services.md)
