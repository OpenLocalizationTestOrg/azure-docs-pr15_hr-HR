<properties
   pageTitle="Akcija testability | Microsoft Azure"
   description="U ovom se članku govori o testability akcije u tkanina usluge za Microsoft Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Testability akcije
Da bi se zamjenu nepouzdanih infrastrukture tkanina servisa Azure omogućuje vam programer, načini kao zamjenu za različite stvarnog života pogrešaka i stanje prijelaze. Te su predstavljeni kao testability akcije. Akcije su API-ji najniže razine koji uzrokuju određene kvara unos, prijelaz stanje ili provjere valjanosti. Kombiniranjem ove akcije možete napisati scenariji potpun test za usluge.

Servis tkanina sadrži neke uobičajeni scenariji test sastoji od ove akcije. Preporučujemo koristite tih scenarija ugrađene koje pažljivo odabrati da biste testirali uobičajenih Prijelazi stanje i slučajeva nije uspjelo. Međutim, akcije se poslužite da biste stvorili prilagođeni test scenariji kada želite dodati opseg scenarija koji pokrivaju ugrađene scenariji još ili su prilagođene prilagođene aplikacije.

C# implementacije od akcija nalaze se u sklopu System.Fabric.dll. Modul ljuske PowerShell sustava tkanina nalazi se u sklopu Microsoft.ServiceFabric.Powershell.dll. Kao dio instalacije runtime modul ljuske ServiceFabric PowerShell je instaliran da biste omogućili radi jednostavnog korištenja.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Graceful nasuprot ungraceful kvara akcije
Akcije testability se podijeliti u dvije glavne memorijski blokovi:

* Ungraceful kvarove: te kvarove zamjenu pogreške kao što su ponovnog pokretanja računala i obrada ruši. U tim slučajevima neuspjeha kontekst izvođenja procesa naglo prekida. To znači da nema čišćenje stanja možete pokrenuti aplikaciju pokretanja ponovno.

* Graceful kvarove: ove kvarove kao zamjenu graceful akcije kao što su replike premješta i izostavlja opterećenja je pokrenuo. U tim slučajevima servis prima obavijesti o zatvaranju i možete očistiti stanje prije zatvaranja.

Provjere valjanosti bolje kvalitete, pokrenite radno opterećenje servisa i poslovne tijekom inducing različite kvarove graceful i ungraceful. Ungraceful kvarove Nekorištenje scenariji gdje se postupku servisa naglo zatvara sredini neki tijek rada. Ovo testiranje oporavak put kada replike servis je vratiti tkanina servisa. Na taj ćete testirati dosljednost podataka i je li stanje servisa se održava pravilno nakon neuspješnih pokušaja. Postavljanje neuspjeha (graceful neuspjeha) testirajte da servis pravilno reacts da biste replike koja se premješta tkanina servisa. To testira rukovanje od otkazivanja pretplate u način RunAsync. Da biste provjerili token opoziv nije moguće postaviti, ispravno spremiti stanje i izlaz iz načina RunAsync mora se servis.

## <a name="testability-actions-list"></a>Popis akcija testability

| Akcija | Opis | Upravljani API-JA | Cmdlet ljuske PowerShell | Graceful ungraceful kvarove |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Uklanja sve stanje test iz klaster u slučaju neispravni isključivanje upravljački program za testiranje. | CleanTestStateAsync | Uklanjanje ServiceFabricTestState | Nije primjenjivo |
| InvokeDataLoss | Induces gubitka podataka u servis particije. | InvokeDataLossAsync | Pozivanje ServiceFabricPartitionDataLoss | Graceful |
| InvokeQuorumLoss | Stavlja s praćenjem stanja servisu particije u kvorum gubitka. | InvokeQuorumLossAsync | Pozivanje ServiceFabricQuorumLoss | Graceful |
| Premještanje primarni | Premješta navedeni primarni replike s praćenjem stanja servisa čvor navedeno klaster. | MovePrimaryAsync | Premještanje ServiceFabricPrimaryReplica | Graceful |
| Premještanje sekundarni | Premješta trenutni sekundarnog replike s praćenjem stanja servisa različite čvor. | MoveSecondaryAsync | Premještanje ServiceFabricSecondaryReplica | Graceful |
| RemoveReplica | Simulira neuspješne replike tako da uklonite kopiju klaster. To će se zatvoriti replike i će prijelaz u ulogu 'Nijedna', uklonite sve stanje iz skupine. | RemoveReplicaAsync | Uklanjanje ServiceFabricReplica | Graceful |
| RestartDeployedCodePackage | Kod pogreške paketa postupak simulira ponovnim pokretanjem kod paketa u uveden u čvor klasteru. To prekida paketa procesa kod koji će se pokrenuti sve replike servisa korisnik nalazi u taj postupak. | RestartDeployedCodePackageAsync | Ponovno pokretanje ServiceFabricDeployedCodePackage | Ungraceful |
| RestartNode | Nije uspjelo servisa tkanina klaster čvor simulira ponovnim pokretanjem čvor. | RestartNodeAsync | Ponovno pokretanje ServiceFabricNode | Ungraceful |
| RestartPartition | Simulira podatkovnog centra blackout ili klaster blackout scenarij ponovnim pokretanjem replike neke ili sve particije. | RestartPartitionAsync | Ponovno pokretanje ServiceFabricPartition | Graceful |
| RestartReplica | Simulira neuspješne replike ponovnim postojanog replike klasteru, zatvaranjem replike, a zatim je ponovno otvoriti. | RestartReplicaAsync | Ponovno pokretanje ServiceFabricReplica | Graceful |
| StartNode | Pokreće čvor klasteru koji već prestao. | StartNodeAsync | Početak ServiceFabricNode | Nije primjenjivo |
| StopNode | Simulira neuspješne čvor po zaustavljanje čvor klasteru. Čvor će ostati prema dolje dok se zove StartNode. | StopNodeAsync | Zaustavi ServiceFabricNode | Ungraceful |
| ValidateApplication | Provjerava dostupnost i status sve servise tkanina servisa u aplikaciji, obično nakon inducing neke kvara u sustav. | ValidateApplicationAsync | Test ServiceFabricApplication | Nije primjenjivo |
| ValidateService | Provjerava dostupnost i status servisa tkanina servisa, obično nakon inducing neke kvara u sustav. | ValidateServiceAsync | Test ServiceFabricService | Nije primjenjivo |

## <a name="running-a-testability-action-using-powershell"></a>Pokretanje testability akcija pomoću komponente PowerShell

Pomoću ovog praktičnog vodiča pokazuje kako pomoću ljuske PowerShell pokrenite testability akcija. Ćete saznati kako pokrenuti testability akcija protiv lokalne klaster (jedan okvir) ili Azure klaster. Microsoft.Fabric.Powershell.dll – servis tkanina modul ljuske PowerShell sustava – se instalira automatski kada instalirate MSI tkanina za servis Microsoft. Modul učitava se automatski prilikom otvaranja odzivniku komponente PowerShell.

Praktični vodič segmenata:

- [Pokretanje akcije na temelju jednog okvira klaster](#run-an-action-against-a-one-box-cluster)
- [Pokrećete akciju za Azure klaster](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Pokretanje akcije na temelju jednog okvira klaster

Da biste pokrenuli testability akcija protiv lokalne klaster, najprije povezati klaster te otvorite PowerShell upita u načinu za administratora. Javite nam pogledajte **Ponovno pokretanje ServiceFabricNode** akciju.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Ovdje akciju **Ponovno pokretanje ServiceFabricNode** je pokrenut na čvor naziva "Node1". Dovršetak načina rada određuje da ga treba provjeriti li akciju ponovno pokretanje čvor zapravo je uspjelo. Koji određuje način dovršetka kao "Provjerite je li" uzrokovat će ga da biste provjerili je li akciju ponovno pokretanje zapravo je uspjelo. Umjesto izravno određivanja čvor prema nazivu, možete odrediti je putem ključa particija i vrste replike, na sljedeći način:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Ponovno pokretanje ServiceFabricNode** treba koristiti da biste ponovno pokrenuli servis tkanina čvor klasteru. To će prekinuti Fabric.exe procesa koji će se pokrenuti sve na sustav servisa i korisnik servisa replike hostirane na tom čvor. Pomoću ovog API-JA za vaš servis za testiranje olakšava otkrivanje programskih pogrešaka duž oporavak putova prebacivanje. Omogućuje zamjenu čvor pogrešaka u klasteru.

Sljedeće snimka zaslona prikazuje naredbu testability **Ponovno pokretanje ServiceFabricNode** u akciji.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Izlaz iz prvog **Get-ServiceFabricNode** (cmdlet iz modula servisa tkanina PowerShell) pokazuje da lokalne klaster ima pet čvorove: Node.1 za Node.5. Nakon izvršenja akcije testability (cmdlet) **Ponovno pokretanje ServiceFabricNode** na čvor naziva Node.4, možemo vidjeti da vrijeme aktivnosti u čvor vraćena.

### <a name="run-an-action-against-an-azure-cluster"></a>Pokrećete akciju za Azure klaster

Izvodi akciju za testability (pomoću komponente PowerShell) protiv Azure klaster je slična izvodi akciju protiv lokalne klaster. Jedina razlika je da biste mogli pokrenuti akciju, umjesto povezivanje s lokalnom klaster, morate se najprije povezati s Azure klaster.

## <a name="running-a-testability-action-using-c35"></a>Pokretanje testability akcija pomoću C# 35;

Da biste pokrenuli testability akcija pomoću C#, prvo morate povezati s klaster pomoću FabricClient. Zatim nabavite parametara potrebno za pokretanje akcije. Da biste pokrenuli akciju isti se poslužite druge parametre.
Pogled na RestartServiceFabricNode akciju, da biste ga pokrenuti je pomoću podataka čvor (čvor naziva i ID instance čvora) u klasteru.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Objašnjenje parametar:

- **CompleteMode** određuje da načina rada treba provjeriti li akciju ponovno pokretanje zapravo je uspjelo. Koji određuje način dovršetka kao "Provjerite je li" uzrokovat će ga da biste provjerili je li akciju ponovno pokretanje zapravo je uspjelo.  
- **OperationTimeout** postavlja količinu vremena za postupak da biste dovršili prije nego što je TimeoutException Izbačena je.
- **CancellationToken** omogućuje se otkazati poziv na čekanju.

Umjesto izravno određivanja čvor prema nazivu, možete je odrediti putem ključa particija i vrste replike.

Dodatne informacije potražite u članku [PartitionSelector i ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector i ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector je preglednika koji se prikazuje u testability i koristi se za odabir određene particije na kojem želite izvršiti bilo koju od akcija testability. To se može koristiti da biste odabrali određene particije ako ID particije prije toga poznati. Ili možete unijeti ključ particija i postupak će riješiti ID particije interno. Imate mogućnost odabira slučajni particije.

Da biste koristili ovu preglednika, stvorite objekt PartitionSelector i odaberite particije na jedan od načina Select *. Zatim prenesite u objektu PartitionSelector API koje je potrebno je. Ako je odabrana mogućnost bez, po zadanom je odabrana slučajni particije.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector je preglednika koji se prikazuje u testability i pridonose odaberite replike po kojem želite izvršiti bilo koju od akcija testability. To se može koristiti da biste odabrali određene replike Ako ID replike prije toga poznati. Uz to, imate mogućnost odabira primarni replike ili izravnim sekundarni. ReplicaSelector rezultat PartitionSelector, pa ćete morati odabrati replike i particije na kojem želite izvođenje operacije testability.

Da biste koristili ovu preglednika, stvorite ReplicaSelector objekta i postavite onako kako želite da biste odabrali replike i particije. Možete je zatim proslijediti u API koje je potrebno je. Ako je odabrana mogućnost bez, po zadanom je odabrana slučajni replike i izravnim particije.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Daljnji koraci

- [Scenariji testability](service-fabric-testability-scenarios.md)
- Kako na servis za testiranje
   - [Kao zamjenu pogreške tijekom radnih opterećenja servisa](service-fabric-testability-workload-tests.md)
   - [Servis za servis komunikacije neuspjeha](service-fabric-testability-scenarios-service-communication.md)
