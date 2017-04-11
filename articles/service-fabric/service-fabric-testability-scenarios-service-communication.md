<properties
   pageTitle="Testability: Servisa komunikacije | Microsoft Azure"
   description="Servis za servis komunikacije je točka ključnih integraciju servisa tkanina aplikacije. U ovom se članku govori o značajkama dizajna i testiranja tehnike."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Servis tkanina testability scenarije: servisa komunikacije

Microservices i plošni vezanima uz servis arhitektonski stilovi prirodan u tkanina servisa Azure. U tim vrstama raspodijeljeno arhitekturi componentized microservice aplikacije obično se sastoje od više usluga koje je potrebno obratiti međusobno. U slučajevima čak i najjednostavnije, obično imate barem bez praćenja stanja web-servisa i servis za pohranu s praćenjem stanja podatke koji su potrebni za komunikaciju.

Servis za servis komunikacije je točka ključnih integraciju aplikacije, jer se svaki servis izlaže udaljene API-JA za ostale servise. Rad sa skupom API ograničenja koja obuhvaća/i obično zahtijeva neke pažnju s dobar količinu testiranja i provjere valjanosti.

Da biste prilikom ove usluge ograničenja zajedno wired raspodijeljeno sustavu na brojne obzir:

 - *Protokol za prijenos*. Hoćete li koristiti HTTP povećana komunikaciji ili prilagođeni binarni protokol za maksimalnu propusnost?
 - *Rukovanje pogreškama*. Kako će trajno i tranzitne pogrešaka obraditi? Što će se dogoditi kada servisa prelazi na različitim čvor?
 - *Vremenska ograničenja i kašnjenje*. U aplikacijama multitiered kako će svaki servis sloja obrađivati Latencija kroz stog i korisniku?

Koristite neku od komunikacijske komponente ugrađene servis nudi tkanina servis je li ili stvaranja vlastitog, testiranje interakcije između servisa ključnih zajamčiti otpornost u aplikaciji.

## <a name="prepare-for-services-to-move"></a>Priprema za servise za premještanje

Možda instanci servisa kretanje tijekom vremena. To vrijedi osobito kada su konfigurirani s učitavanja metriku Prilagođeno konkretne optimalnih resursa za ujednačavanje. Servis tkanina premješta na instanci servisa za maksimiziranje njihove dostupnosti čak i tijekom nadogradnje, failovers, skala Izlaz i drugim situacijama koji se pojavljuju tijekom života raspodijeljeno sustava.

Kao usluge premještali u klasteru, klijentima i ostale servise treba se pripremite obrađuje dva scenarija kada ih razgovarati s uslugom:

- Servis instanci ili particije replike premještena od posljednjeg bila riječ na njega. Ovo je običnom dio životni ciklus servisa, a trebali biste očekivali da se dogodi tijekom života aplikacije.
- U tijeku premještanje je servis replike instanci ili particije. Premda prebacivanje servisa čvor jedne na drugu pojavljuje vrlo brzo tkanina servisa, mogu postojati Odgoda dostupnosti ako je sporo pokreće komponenta komunikacije servisa.

Obavljanje rukovanje scenarija je važno prelazili putem glatke pokretanje sustava. Da biste to učinili, imajte na umu:

- Svaki servis koji se mogu povezati s sadrži *adresu* koja se očekuje podatke (na primjer, HTTP ili WebSockets). Kada je instanca servisa ili particije premješta, mijenja se njezin krajnju točku adresu. (Prelazi na različitim čvor s različitim IP adresom.) Ako koristite ugrađeni komunikacije komponente, će obrađivati ponovno rješavanje adrese servisa za vas.
- Možda postoje privremeni povećava Latencija servisa kao pokretanja instance servisa gore njegov ga slušatelj ponovno. To ovisi o brzinu servis otvara ga slušatelj nakon premještanja instanca servisa.
- Sve postojeće veze morate zatvoriti i ponovno otvorite nakon servis otvorit će se novi čvor. Ponovno pokretanje ili isključivanje graceful čvor omogućuje vremena za postojeće veze isključiti bez poteškoća.

### <a name="test-it-move-service-instances"></a>Ga testirate: premještanje instanci servisa

Pomoću alata za testability servisa tkanina možete autor scenarij test da biste testirali ovih situacija na različite načine:

1. Premještanje primarni replike s praćenjem stanja servisa.

    Primarni replike particije s praćenjem stanja servisa, možete premjestiti bilo koji broj razloga. Ta postavka omogućuje ciljani primarni replike određene particije da biste vidjeli kako se servisa Upoznajte za premještanje u vrlo kontrolirano.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Zaustavite čvor.

    Kada se čvor Zaustavi, servis tkanina premješta sve instance servisa ili particije koji su na tom čvor na jedan od drugih dostupnih čvorove u klasteru. Koristi se za testiranje situacija u kojima čvor gube se iz svoj klaster, a sve instance servisa i replike na tom čvor morati pomaknuti.

    Pomoću cmdleta ljuske PowerShell **Zaustavi ServiceFabricNode** , možete isključiti čvor:

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Održavanje dostupnost usluge

Kao na platformi servisa tkanina namijenjen je pružaju visoke dostupnosti servisa. No u ekstremne slučajeva podlozi infrastrukture probleme i dalje mogu prouzročiti nedostupnosti. Važno je da to testiranje za tih scenarija.

S praćenjem stanja servisa korištenje kvorum-sustava za replikaciju stanja visoke dostupnosti. To znači da kvorum replike mora biti dostupan za izvođenje operacije pisanja. U rijetko slučajevima, kao što su neuspješne Primamo hardver kvorum replike možda neće biti dostupne. U tim slučajevima, nećete moći izvršiti operacije pisanja, ali i dalje moći izvršiti operacije čitanja.

### <a name="test-it-write-operation-unavailability"></a>Ga testirate: pisanje nedostupnosti operacije

Pomoću alata za testability servisa tkanina možete ubaciti kvara koji induces kvorum gubitka test. Iako se rijetko takve scenarij, važno je su pripremljeni u raznim situacijama ih ne možete odabirati pisati zahtjeve klijenata i servise koje ovise o tome s praćenjem stanja servisa. Također važno je sam s praćenjem stanja servis zna tu mogućnost i mogli bez poteškoća komunicirati ga za pozivatelje.

Pomoću cmdleta ljuske PowerShell **Pozovite ServiceFabricPartitionQuorumLoss** možete induce kvorum gubitak:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

U ovom se primjeru ne možemo postaviti `QuorumLossMode` da biste `QuorumReplicas` da biste naznačili želimo induce kvorum gubitka bez poduzimanja dolje sve replike. Na taj način moguće su operacije čitanja i dalje. Da biste testirali scenarij u kojem nije dostupan cijeli particija, možete postaviti taj parametar `AllReplicas`.

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o testability akcije](service-fabric-testability-actions.md)

[Dodatne informacije o testability scenariji](service-fabric-testability-scenarios.md)
