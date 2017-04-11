<properties
   pageTitle="Induce Chaos u servis tkanina klastere | Microsoft Azure"
   description="Unos kvara i klaster Analysis servisa API-ji upravljate pomoću Chaos u klasteru."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Induce nadziranim Chaos u klastere tkanina servisa
Veliki distributed sustave kao što su infrastructures oblaka su čini nepouzdanih. Azure tkanina servisa omogućuje razvojnim inženjerima pisati pouzdanog services pri vrhu nepouzdanih infrastrukture. Da biste napisali robusne services, razvojni inženjeri morat ćete moći induce kvarove protiv takve nepouzdanih infrastrukture za testiranje stabilnosti svoje usluge.

Unos kvara i servis za analizu klaster (poznat i kao servis analizu kvara) omogućuje razvojnim inženjerima induce kvara akcije da biste testirali services. Međutim, ciljano Simulirani kvarove vam samo dosad. Da biste dodatno preuzeli testiranja, možete koristiti Chaos.

Chaos simulira neprekinuti, interleaved kvarove (graceful i ungraceful) cijeloj klaster putem prošireni vremenskog razdoblja. Kada završite s stopu i vrste kvarove konfiguriranje Chaos, možete započeti i prekid putem C# API-ji ili PowerShell da biste generirali kvarove klaster i usluge.

Dok se izvodi Chaos, ona će dati različitim događajima koji snimanja stanja Pokreni u trenutku. Na primjer, programa ExecutingFaultsEvent sadrži kvarove koji se izvršavaju u tom iteracije. Na ValidationFailedEvent sadrži detalje o neuspješne pronađen tijekom provjere valjanosti klaster. Možete pozvati API GetChaosReportAsync da biste dobili izvješće Chaos se pokreće.

## <a name="faults-induced-in-chaos"></a>Kvarove induced u Chaos
Chaos generira kvarove preko cijele klaster tkanina servisa te sažima kvarove koji možete vidjeti u mjeseci ili godina u nekoliko sati. Kombinacija interleaved kvarove visoko kvara brzine traži slučajeve kut u suprotnom Propušteni. Ovu vježbu Chaos potencijalnih klijenata za značajan poboljšavanja kvalitete kod usluge.

Chaos induces kvarove iz sljedećih kategorija:

 - Ponovno pokrenite čvor
 - Ponovno pokrenite paket distribuiranih kod
 - Uklanjanje kopiju
 - Ponovno pokrenite kopiju
 - Premještanje primarni replike (konfigurirati)
 - Premještanje sekundarnog replike (konfigurirati)

Chaos pokreće se u većeg broja iteracija. Iteracija sastoji se od kvarove i klaster provjere valjanosti za navedeno razdoblje. Možete konfigurirati vrijeme utrošeno klaster Stabiliziranje i provjere valjanosti da. Ako je pogreške u provjeri valjanosti klaster, Chaos stvara i nastavi ValidationFailedEvent s vremenskom oznakom UTC-a i detalja pogreške.

Na primjer, razmotrite instance kaosa koja je postavljena za pokretanje programa h s najviše tri Istodobni kvarove. Chaos induces tri kvarove, a zatim Provjeri valjanost stanja klaster. Ga zadanu ponovno izračunava pomoću prethodnih koraka dok je izričito prestao kroz StopChaosAsync API-JA ili jedan sat ne prosljeđuje. Ako klaster postane dobro u bilo kojem iteracije (to jest, on neće Stabiliziraj unutar konfiguriranog vremena), Chaos generira na ValidationFailedEvent. Ovaj događaj pokazuje da nešto je nestalo pogrešna i možda će biti potrebno daljnje istrage.

U trenutnom obliku, Chaos induces sigurni kvarove. To podrazumijeva da u odsutnost vanjskih kvarove kvorum gubitka ili gubitka podataka nikad ne dogodi.

## <a name="important-configuration-options"></a>Mogućnosti važne konfiguracije
 - **TimeToRun**: Ukupno vrijeme te se pokreće Chaos prije nego što završi s uspjeh. Prije pokretanja za razdoblje TimeToRun kroz StopChaos API, Chaos možete isključiti.
 - **MaxClusterStabilizationTimeout**: maksimalnu količinu vremena čekanja klaster postati dobar prije traženja na njemu. U ovom čekanja je da biste smanjili opterećenje klaster dok ga se oporaviti. Provjera izvršiti su:
    - Ako je stanje klaster u redu
    - Ako je stanje servisa u redu
    - Ako replike cilj postavite veličinu se postiže particije servisa
    - Postoji bez replike InBuild
 - **MaxConcurrentFaults**: maksimalni broj Istodobni kvarove koji su induced u iteracija. Veći broj, više izrazito Chaos je. Rezultat u složenije failovers i kombinacije prijelaz. Chaos jamčiti da u odsutnost vanjskih kvarove postoji bez gubitka kvorum ili gubitka podataka ubuduće, bez obzira na to kako visoke vrijednosti sadrži tu konfiguraciju.
 - **EnableMoveReplicaFaults**: omogućuje ili onemogućuje kvarove koji uzrokuju primarni ili sekundarnog replike da biste premjestili. Po zadanom su onemogućeni te kvarove.
 - **WaitTimeBetweenIterations**: vrijeme čekanja između iteracija, odnosno nakon round kvarove i odgovarajuće provjere valjanosti.
 - **WaitTimeBetweenFaults**: vrijeme čekanja između dva uzastopna kvarove u programa iteracije.

## <a name="how-to-run-chaos"></a>Pokretanje Chaos
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
