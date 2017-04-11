<properties
    pageTitle="Pokretanje aplikacije MPI u grupe za Azure sa zadacima više instanci | Microsoft Azure"
    description="Saznajte kako izvršiti sučelja prosljeđivanje poruke (MPI) aplikacije koje se koriste Vrsta zadatka više instanci u seriji Azure."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Korištenje više instanci zadataka za pokretanje aplikacije sučelja prosljeđivanje poruke (MPI) u grupe za Azure

Zadaci više instanci dopustiti izvođenje zadatka programa Azure grupe na više računalnim čvorove istodobno. Ove zadatke omogućiti visoke performanse računalstvo scenariji kao što je aplikacija sučelja prosljeđivanje poruke (MPI) u grupe. U ovom se članku saznat ćete kako izvršiti više instanci zadataka pomoću [Grupe za .NET] [ api_net] biblioteke.

>[AZURE.NOTE] Dok se primjerima u ovom članku usredotočite se na obradu .NET, MS-MPI i Windows izračunati čvorove, koncepata zadatak više instanci ovdje spominju primjenjuju se na drugim platformama i tehnologijama (Python i Intel MPI na Linux čvorove, na primjer).

## <a name="multi-instance-task-overview"></a>Pregled zadataka više instanci

U seriji, svaki zadatak obično se izvršava na jednom računalnim čvor – slanje više zadataka za posao i servis za obradu Zakazuje svaki zadatak za izvršavanje na čvor. Međutim, konfiguriranjem na zadatka **više instanci postavke**reći grupe umjesto stvaranja jedan zadatak primarnog i nekoliko podzadataka koja zatim izvršava na više čvorove.

![Pregled zadataka više instanci][1]

Kada pošaljete zadatak s više instanci postavkama za posao, obradu izvodi nekoliko koraka koji su jedinstveni više instanci zadacima:

1. Servis za obradu stvara jednu **primarnu** i nekoliko **podzadataka** na temelju više instanci postavke. Ukupan broj zadataka (primarni plus sve podzadatke) odgovara broj **instanci** (računalnim čvorove) koji ste naveli u više instanci postavke.
1. Obradu nešto čvorove računalnim označava kao **glavni**i rasporede primarni zadatak izvršiti na matrici. Ga rasporede podzadataka izvršiti na ostatak čvorove računalnim dodijeliti zadatak više instanci, jedan subtask po čvor.
1. Primarnih osnovnih stranica i sve podzadatke preuzeti sve **zajedničke datoteke resursa** koji ste naveli u postavkama više instanci.
1. Nakon uobičajenih resursa datoteka preuzeta, primarnih i podzadataka izvršiti **naredbu koordinaciji** koji ste naveli u više instanci postavke. Naredba koordinaciji obično koristi za pripremu čvorove za izvođenje zadatka. To može obuhvaćati pokretanje servisa pozadine (kao što je [Microsoft MPI][msmpi_msdn], `smpd.exe`) i provjera jesu li čvorove jeste li spremni za obradu među čvor poruke.
1. Primarni zadatka izvršava **naredbi aplikacije** na na glavni čvor *nakon* naredbu koordinaciji uspješno dovršen primarnih i sve podzadatke. Naredba aplikacije je naredbeni redak zadatka više instanci sam pa je izvršio samo primarni zadatka. U [MS MPI][msmpi_msdn]-temelji rješenja, to je gdje izvršavanje MPI je omogućen pomoću `mpiexec.exe`.

> [AZURE.NOTE] Iako se functionally distinct "zadatak više instanci" nije vrsta jedinstveni zadatka kao što su [StartTask] [ net_starttask] ili [JobPreparationTask][net_jobprep]. Zadatak više instanci je jednostavno standardni zadatak grupe ([CloudTask] [ net_task] u grupe za .NET) čije postavke za više instanci konfigurirana. U ovom se članku nazivamo taj **zadatak više instanci**.

## <a name="requirements-for-multi-instance-tasks"></a>Preduvjeti za zadatke više instanci

Više instanci zadatke potrebne skup **komunikacije među čvor omogućeno**i **Istodobni zadatka izvođenja onemogućene**. Ako pokušate pokrenuti zadatak više instanci u na s internode komunikacije onemogućena ili s *maxTasksPerNode* vrijednost veću od 1, zadatak nikada zakazano – beskonačno ona ostaje u stanju "aktivan". Ovaj isječak koda prikazuje stvaranja u grupu pomoću biblioteke .NET grupe.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Uz to, više instanci zadaci možete izvršiti *samo* na čvorove u **grupe stvorene nakon 14 prosinac 2015**.

### <a name="use-a-starttask-to-install-mpi"></a>Korištenje programa StartTask da biste instalirali MPI

Da biste pokrenuli MPI aplikacije s više instanci zadatka, najprije morate instalirati implementacija MPI (MS MPI ili Intel MPI, na primjer) na čvorove računalnim u. Ovo je jedan dobar da biste koristili [StartTask][net_starttask], koja se izvršava svaki put kada čvor spaja u grupu ili ponovnog pokretanja. U ovom isječak koda stvara StartTask koji određuje instalacijski paket MS MPI kao [datoteka resursa][net_resourcefile]. Pokretanje zadatka naredbenog retka se izvršava nakon preuzimanja datoteke resursa za čvor. U ovom slučaju naredbenog retka izvodi nenadziranog instalaciju sustava MS MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Udaljena izravan pristup memoriji (RDMA)

Kad odaberete [RDMA zaslonom veličine](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) kao što su A9 za čvorove računalnim u svoje grupe, MPI aplikacija možete iskoristiti Azure, visokih performansi, nisko Latencija memorije udaljene Izravni pristup (RDMA) mreže.

Potražite veličine naveden kao "Koje je moguće povezati RDMA" u sljedećim člancima:

* **CloudServiceConfiguration** grupe

  * [Veličina za servise u Oblaku](../cloud-services/cloud-services-sizes-specs.md) (Samo za Windows)

* **VirtualMachineConfiguration** grupe

  * [Veličina za virtualnim strojevima servisu Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Veličina za virtualnim strojevima servisu Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Da biste iskoristili RDMA na [Linux izračunati čvorove](batch-linux-nodes.md), morate koristiti **Intel MPI** na čvorove. Dodatne informacije o CloudServiceConfiguration i VirtualMachineConfiguration grupe potražite u članku u odjeljku [skup](batch-api-basics.md#Pool) značajki pregled obrade.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Stvorite zadatak s više instanci s grupe za .NET

Sad kad ste smo prekriveno skup preduvjeti i instalaciju paketa MPI, recimo stvorite zadatak s više instanci. U ovom isječaka ćemo stvoriti standardni [CloudTask][net_task], konfiguriranje [MultiInstanceSettings] [ net_multiinstance_prop] svojstvo. Kao što je rečeno ranije, zadatak više instanci nije vrsta distinct zadatka, ali standardni zadatak grupe s više instanci postavke.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primarni zadataka i podzadataka

Kada stvarate više instanci postavke zadatka, navedite broj čvorove računalnim koji su za izvođenje zadatka. Kada pošaljete zadatak posla, servis za obradu stvara jedan zadatak **primarnog** i dovoljno **podzadataka** koji zajedno podudaran s brojem čvorove koju ste naveli.

Ove se zadaci dodjeljuju id cijeli broj u rasponu od 0 *numberOfInstances* - 1. Zadatak s ID-om 0 je primarni zadatak, a sve druge ID-ove su podzadataka. Ako, na primjer, stvorite sljedeće postavke više instanci za zadatak, primarni zadatka promijenile identifikacijskim brojem 0, a podzadataka promijenile ID-a 1 do 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Glavni čvor

Kada pošaljete više instanci zadatka, servis za obradu jedan čvorove računalnim označava kao "osnovnu" čvor i Zakazuje primarni zadatak izvršiti na glavni čvor. Podzadataka zakazani izvršiti na ostatak čvorove dodijeliti zadatak više instanci.

## <a name="coordination-command"></a>Naredba koordinaciji

**Naredba koordinaciji** je izvršio oba primarnih i podzadataka.

Pozivanje naredbe koordinaciji blokira – obradu izvršiti naredbu aplikacije dok naredbu koordinaciji uspješno je vratio za sve podzadatke. Naredba koordinaciji treba stoga pokrenuti sve potrebne u pozadini, provjerite jesu li spremni za korištenje, a zatim zatvorite. Na primjer, tu naredbu koordinaciji rješenja pomoću MS MPI verzije 7 pokrenuti servis SMPD na čvor, zatim izlazi iz:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Imajte na umu korištenje `start` u toj naredbi koordinaciji. To je potrebna jer se `smpd.exe` aplikacija ne vraća odmah nakon izvođenja. Bez korištenja [pokretanje] [ cmd_start] naredbu, ta se naredba koordinaciji bi vratilo i stoga želite blokirati naredbe aplikacije pokretanje.

## <a name="application-command"></a>Naredba aplikacije

Kada primarni zadatka i sve podzadatke dovršite izvodi naredbu koordinaciji, zadatak više instanci naredbenog retka je izvršio u primarni zadatka *samo*. Ne možemo to poziva **naredbe aplikacije** da bi se razlikovao od naredbe koordinaciji.

Za MS MPI aplikacije, upotrijebite naredbu aplikacije za izvođenje aplikacije s omogućenim MPI `mpiexec.exe`. Na primjer, Evo i naredbi aplikacije za rješenje pomoću MS-MPI verzije 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Budući da MS-MPI- `mpiexec.exe` koristi u `CCP_NODES` varijabla po zadani (pogledajte [varijable okruženja](#environment-variables)) u primjeru aplikacije naredbenog retka iznad isključuje ga.

## <a name="environment-variables"></a>Varijable okruženja

Obradu stvara više [varijabli okruženja] [ msdn_env_var] specifične za više instanci zadaci na čvorove računalnim dodijeliti zadatak više instanci. Koordinaciji i aplikacije retke naredbe možete referencirati ove varijable okruženja kao moguće skripte i programi mogu izvršiti.

Sljedeće varijable okruženja je stvorio za korištenje servisa obrade tako da više instanci zadataka:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Sve detalje na te i druge grupe računalnim čvor varijable okruženja, uključujući njihov sadržaj i vidljivost potražite [čvor varijable okruženja za izračun][msdn_env_var].

>[AZURE.TIP] Uzorak koda za obradu Linux MPI sadrži primjera kako nekoliko ove varijable okruženja može se koristiti. [Koordinaciji cmd] [ coord_cmd_example] tulumu skripte preuzimanja uobičajena aplikacija i unos datoteke sa servisa za pohranu Azure, omogućuje zajednički sustava datoteka na mreži (NFS) čvor matrice i konfigurira ostale čvorove dodijeliti zadatak više instanci kao NFS klijente.

## <a name="resource-files"></a>Datoteka resursa

Postoje dva skupa datoteka resursa treba uzeti u obzir za zadatke više instanci: **zajedničke datoteke resursa** preuzimanje *svih* zadataka (obje primarni i podzadataka), i **datoteke resursa** za više instanci zadatka sam zadataka koje *samo primarni* preuzimanja.

Možete odrediti jednu ili više **standardnih datoteka resursa** u više instanci postavke zadatka. Ove zajedničke datoteke resursa se preuzimaju s [Azure prostora za pohranu](./../storage/storage-introduction.md) u svaki čvor **imeniku za zajedničko korištenje zadataka** po primarnih i sve podzadatke. Zadatak imeniku za zajedničko korištenje možete pristupiti iz aplikacije i koordinaciji naredba redaka pomoću na `AZ_BATCH_TASK_SHARED_DIR` varijabla okruženja. Na `AZ_BATCH_TASK_SHARED_DIR` put je jednake na svakoj čvor dodijeliti zadatak više instanci, stoga možete zajednički koristiti naredbu jedan koordinaciji između primarnih i sve podzadatke. Obradu nije "objava" direktorija u smislu daljinski pristup, ali možete ga koristiti kao iznosom ili točku kao spomenuli u savjet na varijable okruženja za zajedničko korištenje.

Datoteka resursa koje navedete za zadatak više instanci sam preuzimaju se zadatka radni direktorij `AZ_BATCH_TASK_WORKING_DIR`, po zadanom. Kao što je rečeno, za razliku od uobičajenih datoteke resursa, primarni zadatka preuzimanja datoteka resursa za zadatak više instanci sam.

> [AZURE.IMPORTANT] Uvijek koristi varijable okruženja `AZ_BATCH_TASK_SHARED_DIR` i `AZ_BATCH_TASK_WORKING_DIR` da biste se pozvali te direktorija u vašem naredba retke. Pokušajte ručno sastavljanje putova.

## <a name="task-lifetime"></a>Vijek trajanja zadataka

Vijek trajanja primarni zadatka kontrole života zadatak za cijelu više instanci. Kada primarni zatvara, sve podzadataka su prekinuti. Kod izlaz za primarni je izlazni kod zadatka i zato upotrijebljena za određivanje uspjelo ili nije zadatka u svrhu pokušajte ponovno.

Ako bilo koji od podzadataka uspjeti izlaska iz njega s kodom povratnu vrijednost različita od nule, ako, na primjer, zadatak za cijelu više instanci neće uspjeti. Zadatak s više instanci je prekinut i ponoviti najviše ograničenje pokušajte ponovno.

Kada izbrišete više instanci zadatka, primarnih osnovnih stranica i sve podzadatke brišu se i grupe za servis. Sve subtask direktorija i njihove datoteke brišu se iz čvorove računalnim samo kao standardni zadatak.

[TaskConstraints] [ net_taskconstraints] za više instanci zadatka, kao što su [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], a [RetentionTime] [ net_taskconstraint_retention] svojstva su na snazi su za standardni zadatak, a odnose se na primarni i sve podzadatke. Međutim, ako promijenite [RetentionTime] [ net_taskconstraint_retention] svojstvo nakon dodavanja zadataka više instanci poslu ta promjena primjenjuje se samo na primarni zadatka. Sve podzadataka nastaviti koristiti izvorni [RetentionTime][net_taskconstraint_retention].

Popis nedavno korištenih zadatka čvor računalnim odražava id programa subtask ako nedavne zadatak je dio više instanci zadatka.

## <a name="obtain-information-about-subtasks"></a>Prikupite informacije o podzadatke

Da biste dobili informacije o podzadataka pomoću grupe za .NET biblioteke, nazovite [CloudTask.ListSubtasks] [ net_task_listsubtasks] način. Ta metoda vraća informacije o sve podzadatke i informacije o čvor računalnim koja izvršava zadatke. Iz ove informacije možete odrediti svaki subtask korijenski direktorij, id grupe aplikacija, njegovu trenutnom stanju, izlazni kod i drugo. Ove informacije možete koristiti u kombinaciji s [PoolOperations.GetNodeFile] [ poolops_getnodefile] način za dohvaćanje datoteka na subtask. Imajte na umu da ova metoda vraćati informacija za primarni zadatak (id 0).

> [AZURE.NOTE] Osim ako, načina obrade .NET koji rade na više instanci [CloudTask] [ net_task] sam primijeniti *samo* primarni zadatka. Na primjer, kada se uključujete [CloudTask.ListNodeFiles] [ net_task_listnodefiles] način za zadatak više instanci vraćaju samo primarni zadatka datoteke.

Sljedeći isječak koda pokazuje kako nabaviti subtask informacije, kao i zahtijevanje sadržaj datoteke iz čvorove na kojemu se izvršava.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Uzorak koda

[MultiInstanceTasks] [ github_mpi] uzorak koda na GitHub pokazuje kako koristiti više instanci zadatka da biste pokrenuli [MS MPI] [ msmpi_msdn] aplikaciju na obradu izračunati čvorove. Slijedite korake u [Priprema](#preparation) i [izvođenja](#execution) da biste pokrenuli uzorka.

### <a name="preparation"></a>Priprema

1. Prva dva koraka navedenih u [Kompiliranje i pokrenuli program jednostavan MS MPI][msmpi_howto]. To zadovoljava prerequesites za sljedeći korak.
1. Sastavljanje *izdanju programa [MPIHelloWorld] * [ helloworld_proj] program MPI uzorka. Ovo je program koji će se izvoditi na računalnim čvorove tako da zadatak više instanci.
1. Stvaranje zip datoteke sa `MPIHelloWorld.exe` (koju ste ugrađeni korak 2) i `MSMpiSetup.exe` (koji ste preuzeli korak 1). Ćete prenijeti ovu datoteku zip kao paketa aplikacije u sljedećem koraku.
1. Pomoću [portala za Azure] [ portal] možete stvarati grupe [aplikacija](batch-application-packages.md) pod nazivom "MPIHelloWorld" i navedite zip datoteke koji ste stvorili u prethodnom koraku kao verzija "1.0" paketa aplikacija. Dodatne informacije potražite [prijenos i upravljanje aplikacijama](batch-application-packages.md#upload-and-manage-applications) .

>[AZURE.TIP] Sastavljanje *izdanju programa* `MPIHelloWorld.exe` tako da ne morate uključiti sve dodatne zavisnosti (na primjer, `msvcp140d.dll` ili `vcruntime140d.dll`) u Aplikacijski paket.

### <a name="execution"></a>Izvršavanje

1. Preuzimanje [azure, grupe i uzorke] [ github_samples_zip] iz GitHub.
1. Otvorite MultiInstanceTasks **rješenja** u Visual Studio 2015. Na `MultiInstanceTasks.sln` rješenje datoteka nalazi u:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Unesite vjerodajnice račun grupe i prostora za pohranu u `AccountSettings.settings` u programu project **Microsoft.Azure.Batch.Samples.Common** .
1. **Stvaranje i pokretanje** MultiInstanceTasks rješenja za izvršavanje na MPI poslušajte aplikacije na izračunati čvorove u grupe za skup.
1. *Neobavezno*: pomoću [portala za Azure] [ portal] ili [Obradu Explorer] [ batch_explorer] da biste pregledali ogledne skup posla, a zadatak ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") prije nego što brisanje resurse.

>[AZURE.TIP] Možete preuzeti [Iz zajednice za Visual Studio] [ visual_studio] besplatno ako nemate Visual Studio.

Izlaz iz `MultiInstanceTasks.exe` je slično sljedećem:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Daljnji koraci

- Microsoft HPC & Azure obradu timskog bloga govori o [MPI podrške za Linux seriju Azure][blog_mpi_linux], a sadrži informacije o korištenju [OpenFOAM] [ openfoam] pomoću obrade. Primjere koda Python možete pronaći na [primjer OpenFOAM na GitHub][github_mpi].

- Saznajte kako [stvoriti grupe Linux računalnim čvorove](batch-linux-nodes.md) za korištenje u vašem rješenja Azure obradu MPI.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Pregled više instanci"
