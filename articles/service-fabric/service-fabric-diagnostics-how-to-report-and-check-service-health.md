<properties
   pageTitle="Izvješća i provjera stanja tkaninom servisa Azure | Microsoft Azure"
   description="Saznajte kako za slanje izvješća o stanju s kod usluge i provjera stanja servisa pomoću alata za nadzor stanja koja omogućuje tkanina servisa Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Izvješća i provjerite stanje servisa
Kada servisa pojaviti problemi, mogućnost odgovoriti i ispravljanje kupljenih i kvarove ovisi o mogućnost da biste brzo otkrili probleme. Ako prosljeđujete probleme i pogreške stanja Upravitelj tkanina servisa Azure iz koda servisa, možete koristiti standardne stanja Alati koji omogućuje tkanina servisa da biste provjerili status stanja za nadzor.

Prijaviti stanje servisa na dva načina:

- Koristite [particije](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) ili [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) objekata.  
Možete koristiti u `Partition` i `CodePackageActivationContext` objekata o stanju elemente koji su dio trenutačnom kontekstu. Na primjer, kod koji se izvodi kao dio kopiju mogu prijaviti stanje samo na taj replike, particije koja pripada i aplikaciju koja je dio.

- Korištenje `FabricClient`.   
Možete koristiti `FabricClient` stanje izvješća iz kod usluge ako skupine nije [sigurnih](service-fabric-cluster-security.md) i je li servis pokrenut s administratorskim ovlastima. To neće biti true u većini slučajeva stvarnog života. S `FabricClient`, možete je prijaviti stanja na bilo kojem entitet koja je dio klaster. Najbolje, međutim, kod usluge mora poslati samo izvješća koji se odnose na vlastitu stanje.

U ovom se članku vodit će vas kroz primjer u kojem se izvješća stanja iz kod usluge. U primjeru prikazano i kako koristiti alate koje nudi tkanina servisa da biste provjerili status stanja. Ovaj članak namijenjen je biti kratkog uvoda stanja mogućnosti tkanina servisa za nadzor. Detaljnije informacije može čitati niz detaljnije članci o stanju koje započinju s vezom na kraju ovog članka.

## <a name="prerequisites"></a>Preduvjeti
Mora imati instalirano sljedeće:

   * Visual Studio 2015.
   * Servis tkanina SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Da biste stvorili lokalnu sigurne razvojni klaster
- Otvorite PowerShell s administratorskim ovlastima i pokrenite sljedeće naredbe.

![Naredbe koje pokazuju kako stvoriti sigurne razvojni klaster](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Implementacija aplikacije i provjeriti njezinu stanja

1. Da biste otvorili Visual Studio kao administrator.

2. Stvaranje projekta pomoću predloška **s praćenjem stanja servisa** .

    ![Stvaranje aplikacije servisa tkanina sa servisom s praćenjem stanja](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Pritisnite **F5** da biste pokrenuli program u načinu rada za ispravljanje pogrešaka. Aplikacija će biti implementirano u lokalnom klaster.

4. Kada program radi, desnom tipkom miša kliknite ikonu upravitelja klaster lokalnih u području obavijesti i odaberite **Upravljanje lokalne klaster** na izborniku prečaca da biste otvorili servis tkanina Explorer.

    ![Otvorite Eksplorer za servis tkanina iz područja obavijesti](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Stanje aplikacije treba prikazati kao na ovoj se slici. Trenutačno aplikacije mora biti dobar bez pogrešaka.

    ![Dobar aplikacije u programu Explorer tkanina servisa](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Stanja možete provjeriti i pomoću komponente PowerShell. Možete koristiti ```Get-ServiceFabricApplicationHealth``` možete koristiti da biste provjerili stanje aplikacije i ```Get-ServiceFabricServiceHealth``` da biste provjerili stanje na servis. Izvješće o stanju za iste aplikacije u ljusci PowerShell je na ovoj slici.

    ![Dobar aplikacije komponente PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Da biste dodali prilagođenu stanja događaja kod usluge
Servis tkanina predlošcima projekata u Visual Studio sadrže kod uzorka. Na sljedeći način prikažite kako prijaviti prilagođene stanja događaje iz servisa koda. Takve izvješća će se automatski prikazati u standardni alate za nadzor stanja da tkanina servis nudi, kao što je servis tkanina Explorer, prikaz Azure portala stanja i PowerShell.

1. Ponovno otvorite aplikaciju koju ste prethodno stvorili u Visual Studio ili stvorite novu aplikaciju pomoću predloška za Visual Studio **S praćenjem stanja servisa** .

2. Otvorite datoteku Stateful1.cs i pronaći u `myDictionary.TryGetValueAsync` poziva na `RunAsync` način. Vidjet ćete da je ova metoda vraća na `result` koji sadrži trenutnu vrijednost od brojač jer je ključa logike u ovoj aplikaciji da biste zadržali count pokrenut. Ako se realni aplikacije, a nedostatak rezultat predstavlja pogreške, bi koji želite označiti taj događaj.

3. Da biste prijavili stanja događaja kada nedostatak rezultat predstavlja pogreške, dodajte na sljedeći način.

    na. Dodavanje u `System.Fabric.Health` prostor naziva Stateful1.cs datoteku.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Dodajte sljedeći kôd nakon na `myDictionary.TryGetValueAsync` poziva

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Ne možemo prijaviti replike stanja jer je u tijeku je s s praćenjem stanja servisa. Na `HealthInformation` parametar pohranjuje informacije o stanju problem se prijavljena.

    Ako ste stvorili bez praćenja stanja servisa, koristite sljedeći kod

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Ako vaš servis je pokrenut s administratorskim ovlastima ili ako je klaster [sigurne](service-fabric-cluster-security.md), možete koristiti `FabricClient` stanje izvješća, kao što je prikazano u sljedećim koracima.  

    na. Stvaranje na `FabricClient` instancu nakon na `var myDictionary` deklariranje.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Dodajte sljedeći kôd nakon na `myDictionary.TryGetValueAsync` poziva.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Pogledajmo kao zamjenu za tog problema i vidjeti prikazuju se u odjeljku Alati za nadzor stanja. U programu Publisher neuspjeh komentar izgleda u prvom retku u kodu izvješćivanja stanja koju ste prethodno dodali. Nakon komentar izgleda u prvom retku, kod će izgledati kao u sljedećem primjeru.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Kod će odmah pokrenuti izvješća o stanju svaki put `RunAsync` izvršava. Kad unesete promjene, pritisnite **F5** da biste pokrenuli aplikaciju.

6. Kada je pokrenut aplikaciju, otvorite Eksplorer za tkanina servisa da biste provjerili stanje aplikacije. Ovaj put servisa tkanina Explorer prikazivat će se aplikacija je dobro. To je zbog pogreške koje je prijavili iz kod da dodali smo prethodno.

    ![Dobro aplikacije u programu Explorer tkanina servisa](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Ako ste odabrali primarni replike u prikazu stabla programa Explorer tkanina servisa, vidjet ćete da **Stanja** ukazuje na pogrešku previše. Servis tkanina Explorer prikazuje i Detalji izvješća o stanju koje su dodane u `HealthInformation` parametar kod. Vidjet ćete iste izvješća o stanju u PowerShell i Azure portal.

    ![Replike stanja u programu Explorer tkanina servisa](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Izvješće će ostati u upravitelju stanja dok je zamijenjena izvještaja ili dok se ne briše se ova replike. Jer smo nije postavljen `TimeToLive` za izvješće o stanju u na `HealthInformation` objekta, izvješće će nikada ne istječe.

Preporučujemo da stanja treba se prijavljivati na razinu najčešće zrnastog koja je u tom slučaju je replike. Stanje se možete prijaviti i na `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Da biste izvješće o stanju sustava na `Application`, `DeployedApplication`, i `DeployedServicePackage`, koristite `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Daljnji koraci
[Precizno dive na stanje servisa tkanina](service-fabric-health-introduction.md)
