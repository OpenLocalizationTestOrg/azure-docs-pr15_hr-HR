<properties
    pageTitle="Zadatak ovisnosti u seriji Azure | Microsoft Azure"
    description="Stvaranje zadataka koje ovise o uspješan dovršetak druge zadatke za obradu MapReduce stil i slično velikih skupova podataka radnih opterećenja u seriji Azure."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Međuzavisnosti zadataka u grupe za Azure

Značajka zavisnosti zadatka Azure serije je dobro rješenje ako želite obrađivati:

- MapReduce stil radnih opterećenja u oblaku.
- Zadatke čiji zadatke za obradu podataka može se izraziti kao usmjerenog acyclic grafikon (DAG).
- Bilo koji drugi posao u kojem do zadaci ovise o rezultatu upstream zadataka.

Međuzavisnosti zadataka grupe omogućuju stvaranje zadataka koji su zakazani izvođenja na računalnim čvorove tek nakon što uspješan dovršetak jedan ili više zadataka. Ako, na primjer, možete stvoriti zadatak koji prikazuje svaki okvir 3D film s zasebne, paralelnog zadaci. Konačnih zadataka – "spajanje zadatak" – spaja prikazanih slika u čitav film tek nakon što se sve okvire ste uspješno prikazati.

Možete stvoriti zadatke koje ovise o druge zadatke u jedan ili jedan-prema-više odnosa. Možete čak stvoriti raspon ovisnost gdje zadatka ovisi o uspješan dovršetak grupu zadataka unutar određenog raspona zadatka ID-ova. Možete kombinirati tri osnovna scenarija da biste stvorili odnose više-prema-više.

## <a name="task-dependencies-with-batch-net"></a>Međuzavisnosti zadataka s grupe za .NET

U ovom se članku ćemo razmotriti kako konfigurirati međuzavisnosti zadataka pomoću [Grupe za .NET] [ net_msdn] biblioteke. Najprije Pokazat ćemo vam kako [omogućiti zadatka ovisnost](#enable-task-dependencies) zadataka, a zatim demonstrirati kako [konfigurirati zadatak s ovisnosti](#create-dependent-tasks). Na kraju, ne možemo govori o [ovisnosti scenarija](#dependency-scenarios) koji podržava grupe.

## <a name="enable-task-dependencies"></a>Omogućivanje međuzavisnosti zadataka

Da biste koristili međuzavisnosti zadataka u aplikaciji za obradu, morate najprije recite servis za obradu da posao koristi međuzavisnosti zadataka. U .NET grupe omogućuju na [CloudJob] [ net_cloudjob] postavljanjem [UsesTaskDependencies] [ net_usestaskdependencies] svojstvo `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

U prethodnom isječak koda "batchClient" je instance komponente [BatchClient] [ net_batchclient] predmete.

## <a name="create-dependent-tasks"></a>Stvaranje zavisne zadataka

Da biste stvorili zadatak koji je ovisna o uspješan dovršetak jedan ili više zadataka, recite obradu da zadatak "ovisi o" druge zadatke. .NET obradu konfigurirati [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] svojstvo s instance komponente [TaskDependencies] [ net_taskdependencies] klasa:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

U ovom isječak koda stvara zadatak s ID-om "Cvijeća", koji će pokrenuti računalnim čvor tek nakon zadataka pomoću ID-a "Kiša" i "Ned" uspješno je dovršena.

 > [AZURE.NOTE] Zadatak smatra se da se dovršiti kad je u stanju **dovršiti** i njegov **Izlazni kod** `0`. U .NET grupe, to znači da [CloudTask][net_cloudtask]. [Stanje] [net_taskstate] vrijednost svojstva `Completed` i u CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] svojstva vrijednost je `0`.

## <a name="dependency-scenarios"></a>Scenariji ovisnost

Postoje tri scenarija ovisnost osnovnih zadataka koje možete koristiti u seriji Azure: ikone, jedan-prema-više, a ID zadatka u rasponu ovisnosti. Možete kombinirati možete unijeti četvrti scenarij više-prema-više.

 Scenarij&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Primjer | |
 :-------------------: | ------------------- | -------------------
 [Jedna prema jednoj vrijednosti](#one-to-one) | *taskB* ovisi o *taskA* <p/> *taskB* će se zakazati za izvršavanje dok *taskA* uspješno dovršeno | ![Dijagram: jedan zadatak ovisnost][1]
 [Jedan-prema-više](#one-to-many) | *taskC* ovisi o *taskA* i *taskB* <p/> *taskC* će se zakazati za izvršavanje dok *taskA* i *taskB* je uspješno dovršeno | ![Dijagram: ovisnost jedan-prema-više zadataka][2]
 [Raspon ID zadatka](#task-id-range) | *taskD* ovisi o rasponu zadataka <p/> *taskD* će se zakazati za izvršavanje dok zadataka s ID-a od *1* do *10* je uspješno dovršeno | ![Dijagram: Zadatka id raspon ovisnost][3]

>[AZURE.TIP] Možete stvoriti odnos **više-prema-više** , kao što su gdje zadaci C, D, E, a F svaki ovise o zadacima A i B. To je korisno ako, na primjer, u parallelized pretprocesnih scenarijima kojima do zadataka ovisi o izlaz više upstream zadataka.

### <a name="one-to-one"></a>Jedna prema jednoj vrijednosti

Da biste stvorili zadatak koji je u opsegu uspješan dovršetak jedan zadatak, unesete ID jedan zadatak za [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] statične način kada popunjavanje [DependsOn] [ net_dependson] svojstvo [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Jedan-prema-više

Da biste stvorili zadatak koji je u opsegu uspješan dovršetak više zadataka, unesete skup zadatka ID-a za [TaskDependencies][net_taskdependencies]. [OnIds] [net_onids] statične način kada popunjavanje [DependsOn] [ net_dependson] svojstvo [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Raspon ID zadatka

Da biste stvorili zadatak koji je u opsegu uspješan dovršetak grupu zadataka čije ID-a biti unutar raspona, navedite prvi i zadnji zadatka ID-a u rasponu između [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] statične način kada popunjavanje [DependsOn] [ net_dependson] svojstvo [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Kada koristite raspona ID zadatka vaše ovisnost, zadatak ID-a u rasponu *mora* biti niz reprezentacije vrijednosti cijeli broj. Uz to, svaki zadatak u rasponu mora uspješno dovršeno za zavisne zadatak za planiranje za izvršavanje.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Uzorak koda

[TaskDependencies] [ github_taskdependencies] uzorka projekta jedan je od [primjere koda za obradu Azure] [ github_samples] na GitHub. Rješenje Visual Studio 2015 pokazuje kako omogućiti ovisnost zadatka na zadatak, stvorite zadatke koje ovise o druge zadatke i izvršiti te zadatke na skup računalnim čvorove.

## <a name="next-steps"></a>Daljnji koraci

### <a name="application-deployment"></a>Implementacija aplikacije

Značajka [pakete](batch-application-packages.md) grupe omogućuje jednostavnu i implementacija i verzija aplikacije koje izvršavanje zadataka na izračunati čvorove.

### <a name="installing-applications-and-staging-data"></a>Instaliranje aplikacije i pripremnih podataka

Potražite u članku [Instaliranje aplikacije i pripremna podatke na obradu izračunati čvorove] [ forum_post] objaviti na forumu za obradu Azure pregled različite metode da biste pripremili vaše čvorove za izvođenje zadataka. Napisao nešto članovi grupe za Azure, oglas je dobro primer na različite načine za dohvaćanje datoteka (uključujući aplikacije i unos podataka zadataka) na vašem računalnim čvorove.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Dijagram: ikone ovisnost"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Dijagram: ovisnost jedan-prema-više"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Dijagram: zadatka id raspon ovisnost"
