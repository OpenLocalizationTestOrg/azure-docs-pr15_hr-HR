<properties
    pageTitle="Posao i zadatak izlaz postojanost u seriji Azure | Microsoft Azure"
    description="Saznajte kako koristiti Azure prostora za pohranu, kao što je durable spremište za obradu zadatka i posla izlazne i omogućite prikaz ovaj postojanog Izlaz na portalu za Azure."
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Zadržava grupe za Azure posao i zadatak Izlaz

Zadaci obično pokrenete u seriji proizvesti izlaza koji moraju biti spremljene i kasnije dohvatiti koriste ostali zadaci u posla, klijentska aplikacija koja izvršava posao ili i jedno i drugo. Ovaj Izlaz možda datoteke stvorene u obradu ulaznih podataka ili datoteke koje su povezane s izvršavanje zadataka zapisnika. U ovom se članku predstavlja Biblioteka klasa .NET koja koristi tehnika utemeljen na konvencije održati takve izlaz zadatka u Azure blobova dostupnost ga čak i nakon Brisanje grupe, zadatke, a izračunati čvorove.

Pomoću predviđa u ovom članku možete vidjeti i izlaz zadatka u **zapisnicima spremljeno** [Azure portal]i **spremljeno Izlazna datoteka** [portal].

![Spremljeno Izlazna datoteka i Birači zapisnika spremljeni na portalu][1]

>[AZURE.NOTE] [Konvencije datoteka za obradu Azure] [ nuget_package] Biblioteka klasa .NET spominju u ovom članku je u pretpregledu. Prije no što Općenito dostupan može promijeniti neke od značajki opisanih u nastavku.

## <a name="task-output-considerations"></a>Razmatranja izlaz zadatka

Prilikom dizajniranja rješenje grupe, razmotrite nekoliko čimbenika vezane uz izlaze posao i zadatke.

* **Izračunati čvor vijek**: izračun čvorove često su tranzitne, posebice u automatsko skaliranje s omogućenim grupe. Izlaze zadataka koji se izvode na čvor dostupni su samo dok je čvor postoji, a samo unutar vremena zadržavanja datoteke postavljene za zadatak. Da biste osigurali zadržava se izlaz zadatka, zadataka morate stoga prenesite njihove Izlazna datoteka durable pohrana, na primjer, Azure prostora za pohranu.

* **Prostor za pohranu izlaz**: održati zadatka izlazne podatke durable za pohranu, možete koristiti [SDK Azure prostora za pohranu](../storage/storage-dotnet-how-to-use-blobs.md) u kodu zadatka da biste prenijeli izlaz zadatka spremniku spremište blobova platforme. Ako implementirate kontejner i konvencije imenovanja datoteka, klijentska aplikacija ili druge zadatke u sklopu posla možete pronaći i preuzmite ovaj Izlaz na temelju konvencije.

* **Dohvaćanje izlaz**: možete dohvatiti zadatka izlaz izravno s računalnim čvorove u svoje grupe aplikacija ili iz spremišta Azure ako zadataka zadržava njihove izlaz. Da biste dohvatili Izlaz na zadatak izravno s računalnim čvor, potreban je naziv datoteke i mjesta Izlaz na čvor. Ako zadržava Izlaz na Azure prostor za pohranu, do zadataka ili klijentska aplikacija mora imati cijeli put datoteke u odjeljku pohrana Azure da biste preuzeli pomoću SDK za pohranu Azure.

* **Prikaz izlaz**: kada dođite do grupe za zadatak na portalu za Azure i odaberite **datoteke na čvor**, predstavljanja sa svim datotekama pridružene zadatku, ne samo izlazna datoteka koja vas zanima. Ponovno datoteka na računalnim čvorove dostupni su samo dok je čvor postoji i samo u vremenu zadržavanja datoteke postavljene za zadatak. Da biste pogledali izlaza zadatka koji ste dosljedan Azure prostora za pohranu u portalu ili aplikacija kao što su [Azure prostora za pohranu Explorer][storage_explorer], morate znati mjesta, a zatim otvorite datoteku izravno.

## <a name="help-for-persisted-output"></a>Pomoć za postojanog Izlaz

Da biste lakše više jednostavno zadržava posao i zadatak Izlaz, tim grupe sadrži definirani i implementirati skup konvencije imenovanja, kao i biblioteka klasa .NET, [Konvencije datoteka za obradu Azure] [ nuget_package] biblioteke koje možete koristiti u grupe aplikacija. Osim toga, Azure portal je upoznat te konvencije imenovanja tako da možete lako pronaći datoteke koje ste pohranili pomoću biblioteke.

## <a name="using-the-file-conventions-library"></a>Korištenje biblioteka konvencije datoteka

[Azure konvencije datoteke grupe] [ nuget_package] je biblioteka klasa .NET .NET grupe aplikacija možete ga koristiti za pohranu i dohvaćanje izlaze zadatka i iz Azure prostora za pohranu. Je namijenjen za korištenje u kodu zadataka i klijenta – u kodu zadatak za održavanju datoteke, a zatim u kodu klijenta popisa te njihovo dohvaćanje. Zadacima možete koristiti u biblioteku za dohvaćanje izlaze upstream zadatke više razine, kao što su u scenariju [međuzavisnosti zadataka](batch-task-dependencies.md) .

Biblioteka konvencije vodi brigu o jamči da potpisu i zadataka Izlazna datoteka se nazivaju temelju konvencije i prenose se na pravom mjestu kada dosljedan Azure prostora za pohranu. Prilikom učitavanja izlaze izlaze za zadani posla ili zadatka brzo možete pronaći popis ili dohvaćanje izlaze ID i više nije potrebna, a da ne morate znati nazivi datoteka ili tamo gdje postoje u prostor za pohranu.

Ako, na primjer, možete koristiti u biblioteku "popis svih posredna datoteka za zadatak 7" ili "Dohvati me pretpregleda minijatura za posao *mymovie"*bez obzira na to znati nazivima datoteka ili mjesto unutar vašeg računa za pohranu.

### <a name="get-the-library"></a>Početak u biblioteku

Možete dobiti u biblioteku koja sadrži nove klase i proširuje [CloudJob] [ net_cloudjob] i [CloudTask] [ net_cloudtask] klase nove načine, iz [NuGet][nuget_package]. Možete dodati u Visual Studio projekt pomoću [Upravitelja paketima biblioteke NuGet][nuget_manager].

>[AZURE.TIP] Možete pronaći [izvornog koda] [ github_file_conventions] za Azure obradu datoteka konvencije biblioteku na GitHub u Microsoft Azure SDK za .NET spremište.

## <a name="requirement-linked-storage-account"></a>Zahtjevi: povezane prostora za pohranu računa

Za pohranu izlaze durable koristi biblioteka konvencije datoteka za pohranu i njihov prikaz na portalu za Azure, morate [vezu račun za Azure prostora za pohranu](batch-application-packages.md#link-a-storage-account) na račun za obradu. Ako to već niste učinili, veza s računom za pohranu na račun za obradu pomoću portala za Azure:

**Račun za obradu** plohu > **Postavke** > **Računa za pohranu** > **Računa za pohranu** (ništa) > odaberite račun za pohranu u pretplatu

Detaljnije walk-through na povezivanja s računom za pohranu potražite u članku [Implementacija aplikacije s paketa aplikacije Azure grupe](batch-application-packages.md).

## <a name="persist-output"></a>Zadržava Izlaz

Postoje dvije primarni akcije da biste izvršili prilikom spremanja izlaz za posao i zadatke s bibliotekom konvencije datoteke: Stvaranje kontejnera za pohranu i spremanje izlaz spremniku.

>[AZURE.WARNING] Budući da sve posao i zadatak izlaze spremaju se u istu spremnika, [za pohranu ograničavanje ograničenja](../storage/storage-performance-checklist.md#blobs) možda provesti ako zadržava datoteka u isto vrijeme velikog broja zadataka.

### <a name="create-storage-container"></a>Stvaranje kontejnera za pohranu

Prije zadataka održavanju izlaz za pohranu, morate stvoriti spremnik za pohranu blob u koju ćete prijenosa njihove izlaz. To tako da nazovete [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Ovu metodu proširenje vodi [CloudStorageAccount] [ net_cloudstorageaccount] objekt kao parametar i stvara spremnik pod nazivom na način da je njen sadržaj eventualna Azure portal i dohvaćanja metode navedene u nastavku članka.

Obično postaviti kod u klijentskoj aplikaciji – aplikacije koji stvara sustava grupe, zadatke i zadatke.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Spremanje izlaze zadatka

Sad kad ste spremni spremnika u spremište blobova platforme, zadaci možete spremiti izlaz spremnik pomoću [TaskOutputStorage] [ net_taskoutputstorage] predmete pronaći u biblioteci konvencije datoteke.

U kodu zadatka stvaranja [TaskOutputStorage] [ net_taskoutputstorage] objekt, a zatim kada je zadatak dovršen rad, poziva [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] način da biste spremili rezultat Azure prostora za pohranu.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Parametar "Izlaz vrstu" kategorizira postojanog datoteke. Postoje četiri unaprijed definirane [TaskOutputKind] [ net_taskoutputkind] vrste: "TaskOutput", "TaskPreview", "TaskLog" i "TaskIntermediate." Možete definirati i prilagođene vrste ako želite biti korisno je u tijeku rada.

Ove vrste izlaz omogućuju da biste odredili vrstu izlaze popis kada kasnije upit grupe za postojanog izlaze danog zadatka. Drugim riječima, kada popis izlaza za zadatak, možete filtrirati popis na jednu od vrsta izlaz. Ako, na primjer, "mi dali izlaz *Pretpregled* za zadatak *109*." Dodatne informacije na popis te dohvaćanje izlaze pojavljuje se u [dohvatiti izlaz](#retrieve-output) u nastavku članka.

>[AZURE.TIP] Vrsta izlazne i označava mjesto na portalu za Azure određenu datoteku pojavit će se: *TaskOutput*-kategorizirane datoteke pojavit će se u "Zadatka Izlazna datoteka" i *TaskLog* datoteke pojavit će se u "Zadatka zapisnicima."

### <a name="store-job-outputs"></a>Spremanje izlaze poslova

Osim pohrani izlaze zadatka, možete spremiti izlaze pridružene cijeli posao. Na primjer, u zadatku spajanja posla vizualizacije film nije zadržava potpuno prikazanih film kao rezultat za posao. Nakon dovršetka posla klijentska aplikacija možete jednostavno popisa i dohvaćanje izlaze posla, a morati upit pojedine zadatke.

Spremanje poslova izlaz tako da nazovete [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] metoda i navedite [JobOutputKind] [ net_joboutputkind] i naziv datoteke:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Kao što je s TaskOutputKind za zadatak, koristite [JobOutputKind] [ net_joboutputkind] parametar kategorizirati posao je ista i datoteke. Ovaj parametar omogućuje kasnije upit za (popis) određenu vrstu izlaz. Na JobOutputKind obuhvaća vrste izlazne i pregled, a podržava stvaranje prilagođene vrste.

### <a name="store-task-logs"></a>Spremanje zapisnika zadatka

Osim persisting datoteke durable za pohranu kada se Završi zadatak ili posao, možda će vam trebati zadržava datoteke koje su ažurirane tijekom izvršavanja zadataka – datoteke zapisnika ili `stdout.txt` i `stderr.txt`, na primjer. U tu svrhu biblioteku Azure obradu datoteka konvencije nudi [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] način. S [SaveTrackedAsync][net_savetrackedasync], možete pratiti ažuriranja u datoteku na čvor (u intervalu koji navedete) i zadržava tim ažuriranjima prostora za pohranu Azure.

U sljedećim isječak koda koristimo [SaveTrackedAsync] [ net_savetrackedasync] da biste ažurirali `stdout.txt` u spremište Azure svakih 15 sekundi tijekom izvođenja zadatka:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`je rezervirano mjesto za kod koji se obično obavljate zadatka. Na primjer, možda kod koji se preuzima podatke iz spremišta Azure i izvodi transformacije ili izračuna na njemu. Važan dio ovog isječaka takvi kako prelamanje takve kod u na `using` da povremeno ažurirati datoteke [SaveTrackedAsync]blok[net_savetrackedasync].

U `Task.Delay` je potrebna na kraju ovog `using` blok da biste bili sigurni da agent čvor ima vrijeme pražnjenje sadržaj iz standardnog stdout.txt datoteku na čvor (agent čvor je program koji pokreće svaki čvor u i nudi sučelje naredba i kontrola između čvor i servis za obradu). Bez odgode, moguće je propustio posljednje nekoliko sekundi izlaz. Odgode možda neće biti potrebni za sve datoteke.

>[AZURE.NOTE] Kada omogućite datoteke praćenje sa SaveTrackedAsync samo evidentirane datoteke *dodaje* se dosljedan Azure prostora za pohranu. Koristite ovu metodu samo za praćenje koji nisu rotiranje zapisničke datoteke ili druge datoteke koje se dodaju, odnosno, podatke samo se dodaje na kraj datoteke kada je ažurira.

## <a name="retrieve-output"></a>Dohvaćanje Izlaz

Prilikom učitavanja postojanog izlaz koristi biblioteka Azure obradu datoteka konvencije to zadatka i posao-usmjereni na način. Možete zatražiti izlaz za dani zadatak ili posao bez obzira na to znaju svoju putanju blob prostora za pohranu ili čak i njen naziv datoteke. Možete jednostavno reći "Daj mi Izlazna datoteka za zadatak *109*."

Sljedeće koda zadanu ponovno izračunava kroz sve zadatke s posla, ispisuje neke informacije o Izlazna datoteka za zadatak, a zatim preuzimanja njezine datoteke sa servisa za pohranu.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Zadataka izlaze i portala za Azure

Portal za Azure prikazuje izlaze zadatka i zapisnika koji su ista i povezanih računa Azure prostora za pohranu pomoću konvencije imenovanja pronađeni u [Azure obradu datoteka konvencije README][github_file_conventions_readme]. Možete implementirati te konvencije sami jeziku vašem odabiru ili koristite biblioteku konvencije datoteka u .NET aplikacijama.

### <a name="enable-portal-display"></a>Omogućivanje portala prikaza

Da biste omogućili prikaz vaše izlaze na portalu, mora zadovoljavati sljedeće preduvjete:

 1. [Veza račun za Azure prostora za pohranu](#requirement-linked-storage-account) na račun za obradu.
 2. Drži unaprijed definirane konvencije imenovanja za potpisu i datoteke kada održavanju izlaze. Definicija te konvencije možete pronaći u biblioteci konvencije datoteke [README][github_file_conventions_readme]. Ako koristite [Azure obradu datoteke konvencije] [ nuget_package] zadovoljeni biblioteke održati Izlaz, taj zahtjev.

### <a name="view-outputs-in-the-portal"></a>Prikaz izlaze na portalu

Da biste pogledali izlaze zadatka i zapisnike na portalu za Azure, pomaknite se do zadatak čiji je izlaz koje vas zanimaju, a zatim kliknite **spremljeni Izlazna datoteka** ili **spremili zapisnike**. Na ovoj se slici prikazuje **spremljeni Izlazna datoteka** za zadatak s ID-om "007":

![Zadatak izlaze plohu na portalu za Azure][2]

## <a name="code-sample"></a>Uzorak koda

[PersistOutputs] [ github_persistoutputs] uzorka projekta jedan je od [primjere koda za obradu Azure] [ github_samples] na GitHub. Rješenje Visual Studio 2015 prikazano kako koristiti biblioteku Azure obradu datoteka konvencije održati izlaz zadatka u durable prostora za pohranu. Da biste pokrenuli uzorka, slijedite ove korake:

1. Otvaranje projekta u **Visual Studio 2015**.
2. Dodavanje grupe i pohranu **vjerodajnica računa** **AccountSettings.settings** u programu project Microsoft.Azure.Batch.Samples.Common.
3. **Sastavljanje** (ali ne izvode) rješenja. Ako se to od vas zatraži, vraćanje svih NuGet paketa.
4. Prenesite [Aplikacijski paket](batch-application-packages.md) za **PersistOutputsTask**pomoću portala za Azure. Uključiti u `PersistOutputsTask.exe` i njegov zavisne sklopova u paketu .zip postavite ID aplikacije za "PersistOutputsTask" i verzija paketa aplikacije za "1.0".
5. **Pokretanje** projekt (Pokreni) **PersistOutputs** .

## <a name="next-steps"></a>Daljnji koraci

### <a name="application-deployment"></a>Implementacija aplikacije

Značajka [pakete](batch-application-packages.md) grupe omogućuje jednostavnu i implementacija i verzija aplikacije koje izvršavanje zadataka na izračunati čvorove.

### <a name="installing-applications-and-staging-data"></a>Instaliranje aplikacije i pripremnih podataka

Potražite u članku [Instaliranje aplikacije i pripremna podatke na obradu izračunati čvorove] [ forum_post] objaviti na forumu za obradu Azure pregled različite metode Pripremanje zadataka koji se izvode na čvorove. Napisao nešto članovi grupe za Azure, oglas je dobro primer na različite načine za dohvaćanje datoteka (uključujući aplikacije i unos podataka zadataka) na vašem čvorove računalnim i neke posebne napomene za svaki način.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Spremljeno Izlazna datoteka i Birači zapisnika spremljeni na portalu"
[2]: ./media/batch-task-output/task-output-02.png "Zadatak izlaze plohu na portalu za Azure"