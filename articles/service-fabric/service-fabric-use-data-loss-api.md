<properties
   pageTitle="Kako pozvati gubitka podataka na servisima tkanina servisa | Microsoft Azure"
   description="U članku se opisuje kako koristiti gubitka podataka ubuduće API-ja"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Kako pozvati gubitka podataka u uslugama

>[AZURE.WARNING] Ovaj dokument opisuje način dovesti do gubitka podataka u servisa, a želite koristiti s njega.

## <a name="introduction"></a>Uvod
Možete pozvati gubitka podataka na particije servis za servis tkanina po pozivanja StartPartitionDataLossAsync().  Taj api koristi kvara unos i servis za analizu za obavljanje posla uzrokuju uvjeti pad podataka.

## <a name="using-the-fault-injection-and-analysis-service"></a>Korištenje kvara unos i servis za analizu

Unos kvara i servis za analizu trenutno podržava sljedećih API-ji dolje na grafikonu.  Desno od grafikona prikazuje odgovarajuće cmdleta ljuske PowerShell.  Potražite u dokumentaciji msdn na svakom API dodatne informacije o svaki od njih.

|           C# API-JA                    |         Cmdlet ljuske PowerShell                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Početak ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Početak ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Početak ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Konceptualni pregled izvodi naredbu

Unos kvara i servis za analizu koristi asinkronog modela gdje pokrenite naredbu s jednog API-jem, se nazivaju API-JA "Pokreni" u ovom dokumentu, zatim provjerava tijek ta naredba pomoću programa "GetProgress" API dok ga je Pristigla terminal stanje ili dok je otkažete.
Da biste pokrenuli naredbu, nazovite "Pokreni" API-JA za odgovarajuće API-JA.  Vraća navedeni API kada se unos kvara i servis za analizu prihvatio zahtjev.  Međutim, on ne označava koliko je pokrenite naredbu ili čak i ako nije započelo.  Da biste provjerili tijek naredbe, nazovite API koja odgovara API "Pokreni" koji je prethodno pod nazivom "GetProgress".  "GetProgress" API će vratiti objekt koji označava trenutno stanje naredbu unutar njegovo svojstvo stanje.  Beskonačno dok se ne pokreće se naredbe:

1.  Uspješno Završi.  Ako poziv "GetProgress" na njemu u ovom slučaju, stanje tijeka objekt će biti dovršen.
2.  Naiđe kobne pogreške.  Ako poziv "GetProgress" na njemu u ovom slučaju, će pojavila stanje tijeka objekta
3.  Otkazivanje kroz [CancelTestCommandAsync]  [ cancel] API-JA ili [Stop ServiceFabricTestCommand]  [ cancelps] cmdlet ljuske PowerShell.  Ako poziv "GetProgress" na njemu u ovom slučaju, stanje tijeka objekt bit će Cancelled ili ForceCancelled, ovisno o argument taj API-JA.  Potražite u dokumentaciji [CancelTestCommandAsync]  [ cancel] više pojedinosti.


## <a name="details-of-running-a-command"></a>Detalje o izvodi naredbu

Da biste pokrenuli naredbu, nazovite API pokrenuti s očekivani argumenata.  Sve pokrenuti API-ji imaju Guid argument pod nazivom operationId.  Koje treba pratio operationId argument jer se koristi za praćenje napretka naredbe.  To mora proslijediti u API-JA "GetProgress" da biste pratili tijek naredbe.  Na operationId mora biti jedinstvena.

Nakon uspješno pozivanje Start API, GetProgress API treba pozvati u petlji dok se ne svojstvo Stanje vraćeni tijek objekta.  Sve [FabricTransientException,]  [ fte] i OperationCanceledException, mora se ponoviti.
Kada se naredba je dostigne terminal stanje (dovršeno, Faulted ili Cancelled), svojstvo rezultat objekta vraćeni tijeku dodijelit će dodatnih informacija.  Ako stanje dovrši, Result.SelectedPartition.PartitionId sadržavat će particija ID-a koji je odabran.  Result.Exception će biti null.  Ako je stanje u kojem se pojavila, Result.Exception će imati razlog unos kvara i servis za analizu pojavila naredbu.  Result.SelectedPartition.PartitionId će imati particija ID-a koji je odabran.  U nekim slučajevima naredbu možda neće imati grupnim dovoljno krajnjoj da biste odabrali particije.  U tom slučaju u PartitionId će biti 0.  Ako je stanje prekinuta, Result.Exception će biti null.  Kao što su predmet Faulted Result.SelectedPartition.PartitionId će imati particija ID-a koji nije odabran, ali ako naredbu sadrži grupnim dovoljno daleko da biste to učinili, ona će biti 0.  Također pogledajte uzorak u nastavku.

Uzorak koda pokazuje kako pokrenuti, a zatim potvrdite napredak naredba da bi se izgubiti podatke na određenim particije.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Uzorak u nastavku prikazuje kako koristiti u PartitionSelector da biste odabrali slučajni particija navedeni servisa:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Povijest i obrezivanje

Kada naredbe je dostigne terminal stanje, njegove metapodatke će ostati u kvara unos i servis za analizu u određeno vrijeme, prije nego što će se ukloniti da biste uštedjeli prostor.  Ako se zove "GetProgress" pomoću operationId naredbu kada je uklonjena, će vratiti FabricException pomoću programa KodPogreške od KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
