<properties
    pageTitle="Maksimiziranje obradu čvor uporabu paralelno zadaci | Microsoft Azure"
    description="Povećajte učinkovitost i manji trošak pomoću manje radi istovremene zadaci i računalnim čvorove na svaki čvor u programa grupe za Azure"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Maksimiziranje grupe za Azure računalnim resursa sa zadacima Istodobni čvor

Tako da pokrenete više zadataka istodobno na svakom računalnim čvor u svoje grupe za Azure, možete povećati korištenje resursa na manji broj čvorove u. Za neke radnih opterećenja, to može rezultirati kraće vrijeme za posao i donjem trošak.

Dok se nekim scenarijima prednosti put sve resurse u čvor izračunala jedan zadatak, nekoliko situacija prednosti dopuštanje višestrukih zadataka da biste zajednički koristili tih resursa:

 - **Minimiziranje prijenos podataka** kada se zadaci moći zajednički koristiti podatke. U ovom scenariju možete znatno smanjiti naknade za prijenos podataka kopiranjem podataka za zajedničko korištenje na manji broj čvorove i izvršavanja zadataka paralelno na svakom čvor. To se osobito vrijedi i ako podatke koje želite kopirati svaki čvor prenijeti između zemljopisna područja.

 - **Maximizing memorije** kada zadatke potrebne velike količine memorije, ali samo tijekom kratki razdoblja vremena i varijable vrijeme tijekom izvođenja. Možete koristiti manje, ali veće računalnim čvorove s dodatnu memoriju učinkovitije rukovati takve krivina. Ove čvorove imati više zadataka koji se izvode paralelno na svakom čvor, no svaki zadatak želite iskoristiti plentiful memorije u čvorove na različite vremenske.

 - Kada je potrebna unutar zajedničko područje komunikacije među čvor **mitigating čvor broj ograničenja** . Trenutno grupe konfigurirana za komunikaciju među čvor ograničeni su na 50 računalnim čvorove. Ako je svaki čvor u takvim skup moći izvršiti zadatke paralelno, veći broj zadataka možete izvršiti istodobno.

 - **Replikaciju lokalnog izračunati klaster**, kao što su kada prvi put premjestite okruženju računalnim Azure. Ako je trenutni rješenje lokalnog izvršava više zadataka po računalnim čvor, možete povećati maksimalni broj čvor zadaci koje ćete morati pobliže odražavati tu konfiguraciju.

## <a name="example-scenario"></a>Primjer scenarija

Kao primjer da biste ilustrirali prednosti izvođenja paralelno zadatka, recimo da ima li vaše aplikacije zadatka opterećenje procesora i memorije preduvjeti tako da [standardni\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) čvorove su potrebne. No da biste završili zadatak potrebna vremena, potrebni su 1000 te čvorove.

Umjesto standardu\_D1 čvorove koji imaju 1 core procesora, možete koristiti [standardni\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) čvorove koji su 16 jezgri i Omogućivanje izvođenja paralelno zadatka. Stoga *puta 16 manje čvorove* može koristiti – umjesto 1000 čvorove, samo 63 bio potreban. Osim toga, ako aplikacije velikih datoteka ili referenca podataka su potrebne za svaku čvor, trajanje zadatka i učinkovitost se ponovno poboljšane Budući da se podaci kopiraju se samo 16 čvorove.

## <a name="enable-parallel-task-execution"></a>Omogućivanje izvođenja paralelno zadatka

Konfiguriranje računalnim čvorove za izvršavanje paralelno zadatka na razini grupe aplikacija. S bibliotekom .NET grupe za postavljanje [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] svojstvo prilikom stvaranja zajedničko područje. Ako koristite obradu REST API-JA, postavite [maxTasksPerNode] [ rest_addpool] element u tijelu zahtjeva tijekom stvaranja grupe aplikacija.

Azure grupe omogućuje vam da biste postavili najveći zadataka po čvor do četiri puta (4 x) broj jezgri čvor. Na primjer, ako na resurse je konfiguriran za korištenje čvorove veličinu "Veliko" (četiri jezgri), zatim `maxTasksPerNode` može biti postavljena na 16. Detalje o broju jezgri za svaku veličina čvor potražite u članku [veličine za servise u Oblaku](../cloud-services/cloud-services-sizes-specs.md). Dodatne informacije o ograničenjima servisa potražite u članku [kvotama i ograničenja servisa Azure grupe](batch-quota-limit.md).

> [AZURE.TIP] Ne zaboravite da bi se u obzir u `maxTasksPerNode` vrijednost kad je sastavljanje [formula automatsko skaliranje] [ enable_autoscaling] za svoje područje. Primjerice, formula koja daje `$RunningTasks` može biti znatno utječe povećava zadataka po čvor. Dodatne informacije potražite [automatski skaliranje izračunati čvorove u programa Azure grupe](batch-automatic-scaling.md) .

## <a name="distribution-of-tasks"></a>Distribucija zadataka

Kada čvorove računalnim u zajedničko područje možete istovremeno izvršavanje zadataka, važno je da biste odredili način na koji želite da se raspodijeliti čvorove u zadaci.

Pomoću [CloudPool.TaskSchedulingPolicy] [ task_schedule] svojstvo, možete odrediti da zadaci trebaju biti dodijeljeni ravnomjerno preko sve čvorove u ("šire"). Ili možete odrediti da proizvoljan broj zadatke kao što su mogući treba dodijeliti svakom čvor prije nego što se zadaci dodjeljuju drugi čvor u ("spremanje").

Kao primjer kako značajka je koristan, razmislite o na skup [standardni\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) čvorove (u primjeru iznad) koji je konfiguriran za korištenje [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vrijednost od 16. Ako [CloudPool.TaskSchedulingPolicy] [ task_schedule] je konfiguriran za korištenje [ComputeNodeFillType] [ fill_type] *paket*ga želite Maksimiziranje korištenje svih 16 jezgri svaki čvora i Dopusti [autoscaling skup](batch-automatic-scaling.md) za prune Neiskorišteni čvorove iz grupe aplikacija (čvorove bez dodijeljenih zadataka). Time se minimizira korištenje resursa i sprema novca.

## <a name="batch-net-example"></a>Primjer grupe za .NET

[Grupe za .NET] [ api_net] isječak koda API prikazuje zahtjev da biste stvorili grupu koja sadrži četiri velike čvorove s najviše četiri zadataka po čvor. Određuje zakazivanje pravila koja će unositi svaku čvor sa zadacima prije no što dodjeljivanje zadataka na drugi čvor u zadataka. Dodatne informacije o dodavanju grupe pomoću .NET API grupe potražite u članku [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Primjer grupe za OSTALE

[Obradu ostalih] [ api_rest] API isječak prikazuje zahtjev da biste stvorili grupu koja sadrži dvije velike čvorove s najviše četiri zadataka po čvor. Dodatne informacije o dodavanju grupe pomoću REST API-JA potražite u članku [Dodavanje grupe aplikacija s poslovnim subjektom][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Možete postaviti na `maxTasksPerNode` element i [MaxTasksPerComputeNode] [ maxtasks_net] svojstvo samo na vrijeme stvaranja grupe aplikacija. Nije moguće promijeniti nakon što je skup već stvorili.

## <a name="code-sample"></a>Uzorak koda

[ParallelNodeTasks] [ parallel_tasks_sample] projekt na GitHub ilustrira korištenje [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] svojstvo.

Ove aplikacije konzole za C# koristi [Obradu .NET] [ api_net] biblioteke da biste stvorili zajedničko područje s jednog ili više računalnim čvorove. Prilagodljivo broj zadataka je izvršava na te čvorove u programu Publisher varijable Učitaj. Izlaz iz aplikacije određuje koji se čvorovi izvršava svakog zadatka. Aplikacija se nalaze sažetak zadatka Parametri i trajanje. Sažetak dio Izlaz iz dva pokretanja aplikacije uzorka pojavljuje se ispod.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Prvi izvršavanje primjer aplikacije prikazuje koje s jedan čvor u na resurse i po zadanim postavkama jedan zadatak po čvor, posao traje dulje od 30 minuta.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Drugi pokrenuti uzorak prikazuje se trajanje zadatka znatno smanjiti. Ovo je jer na resurse je konfiguriran pomoću četiri zadataka po čvor koja omogućuje izvođenja paralelno zadatka da biste dovršili zadatak gotovo tromjesečja vrijeme.

> [AZURE.NOTE] Trajanje zadatka u sažetaka iznad Uključi vrijeme stvaranja grupe aplikacija. Svaki od iznad poslove poslan prethodno stvorena grupe čiji čvorove računalnim su u stanju *stanje neaktivnosti* vrijeme slanja.

## <a name="next-steps"></a>Daljnji koraci

### <a name="batch-explorer-heat-map"></a>Karte Ekstrema Explorer grupe

[Azure obradu Explorer][batch_explorer], jednu od Azure serije [probne aplikacije][github_samples], sadrži *Karte Ekstrema* značajka koja omogućuje vizualizacije izvršavanje zadataka. Kada se izvršava [ParallelTasks] [ parallel_tasks_sample] primjer aplikacije, možete koristiti značajku karte Ekstrema da biste jednostavno vizualizirati izvođenja paralelno zadaci na svakom čvor.

![Karte Ekstrema Explorer grupe][1]

*Karte Ekstrema Explorer za obradu prikazuje skup četiri čvorove, uz svaki čvor trenutno izvršavaju četiri zadataka*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
