<properties
   pageTitle="Scenariji za prilagođene test | Microsoft Azure"
   description="Kako poboljšati servisa protiv graceful i ungraceful pogreške."
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="anmola"/>

# <a name="simulate-failures-during-service-workloads"></a>Kao zamjenu pogreške tijekom radnih opterećenja servisa

Scenariji testability u tkanina servisa Azure omogućiti razvojnim inženjerima ne razmišljati o pojedinačnim kvarove. Postoje scenariji, međutim, gdje je izričito interleaving radno opterećenje klijenta i pogreške možda biti potrebno. Interleaving radno opterećenje klijenta i kvarove osigurava da servis zapravo izvršava neke akcije kada se dogodi pogreška. Zadana razina kontrole koje nudi testability, mogu biti pri precizno točke izvođenja radno opterećenje. U ovom induction kvarove na različiti statusi u aplikaciji možete pronaći programskih pogrešaka i poboljšati kvalitetu.

## <a name="sample-custom-scenario"></a>Uzorak prilagođene scenarija
Ovaj test prikazuje scenarij u kojem se interleaves radno opterećenje tvrtke s [graceful i ungraceful pogreške](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions). Na kvarove mora biti induced u sredini terenu ili računalnim da biste postigli najbolje rezultate.

Pogledajmo voditi kroz primjera servis koji se izlaže četiri radnih opterećenja: A, B, C i D. svaki odgovara skup tijekova rada, a može biti računalnim, prostor za pohranu ili različiti. Radi jednostavnosti, ne možemo će apstraktnog odgovor na radnih opterećenja u našem primjeru. Različite kvarove izvršava u ovom primjeru su:
  + RestartNode: Ungraceful kvara u programu Publisher ponovno pokretanje računala.
  + RestartDeployedCodePackage: Zatvara Ungraceful kvara u programu Publisher proces servis glavnog računala.
  + RemoveReplica: Graceful kvara u programu Publisher replike uklanjanje.
  + MovePrimary: Graceful kvara u programu Publisher premješta replike tkanina usluge raspoređivača opterećenja je pokrenuo.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
