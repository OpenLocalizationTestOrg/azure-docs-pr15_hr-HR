<properties
    pageTitle="Praktični vodič – početak rada s bibliotekom Azure grupe za .NET | Microsoft Azure"
    description="Saznajte osnovni koncepti grupe za Azure i razvoju servisa za obradu s na primjeru scenarija."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Početak rada s bibliotekom Azure grupe za .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Informirajte se o osnovama [Grupe za Azure] [ azure_batch] i [Grupe za .NET] [ net_api] biblioteke u ovom članku kao ćemo razmotriti u C# uzorka aplikaciju korak po korak. Ne možemo ćete pogledajte kako u ovom primjeru aplikacije upravlja grupe servisa za obradu paralelne radno opterećenje u oblaku, kao i kako ga stupi u interakciju s [Azure prostora za pohranu](../storage/storage-introduction.md) za pripremna datoteka i dohvaćanja. Ćete saznati uobičajenih tehnike obradu aplikacije tijeka rada. Ćete dobiti osnovni razumijevanja glavne komponente obrade, kao što su zadacima, zadatke, a zatim grupe, i izračunati čvorove.

![Obradu rješenja tijeka rada (basic)][11]<br/>

## <a name="prerequisites"></a>Preduvjeti

U ovom se članku pretpostavlja da imate rad znanja C# i Visual Studio. Također pretpostavlja da ste moći zadovoljava preduvjete za stvaranje računa koje su navedene u nastavku za Azure i u grupe i servisi za pohranu.

### <a name="accounts"></a>Poslovni subjekti

- **Račun za Azure**: Ako još nemate Azure pretplatu, [stvorite račun za Azure besplatne][azure_free_account].
- **Račun za obradu**: kada imate Azure pretplatu, [stvorite račun za Azure grupe](batch-account-create-portal.md).
- **Račun za pohranu**: potražite u članku [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) u [računi o Azure prostora za pohranu](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Obradu trenutno podržava *samo* vrstu računa **opće namjene** prostora za pohranu, kao što je opisano u koraku #5 [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) u [računi o Azure prostora za pohranu](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

Morate imati **Visual Studio 2015** da biste sastavili projekta uzorka. [Pregled proizvoda Visual Studio 2015]možete pronaći besplatnu probnu verziju Visual Studio[visual_studio].

### <a name="dotnettutorial-code-sample"></a>Uzorak *DotNetTutorial* koda

[DotNetTutorial] [ github_dotnettutorial] uzorka jedan je od mnogo primjere koda u [azure, grupe i uzorke] [ github_samples] spremište na GitHub. Uzorak možete preuzeti klikom na gumb **Preuzimanje ZIP** na početnoj stranici spremište ili tako da kliknete [azure-obradu-uzorka-master.zip] [ github_samples_zip] vezu za preuzimanje izravno. Nakon što ste dobivenih sadržaj ZIP datoteke, vidjet ćete rješenja u sljedećoj mapi:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure obradu Explorer (nije obavezno)

[Azure obradu Explorer] [ github_batchexplorer] je besplatan uslužni program koji se isporučuje u [azure, grupe i uzorke] [ github_samples] spremište na GitHub. Dok ne potrebne za dovršetak ovog praktičnog vodiča, može biti korisna prilikom razvoja i ispravljanje pogrešaka na obradu rješenja.

## <a name="dotnettutorial-sample-project-overview"></a>Pregled projekta uzorka DotNetTutorial

Uzorak koda *DotNetTutorial* je rješenje za Visual Studio 2015 koji se sastoji od dvaju projekata: **DotNetTutorial** i **TaskApplication**.

- **DotNetTutorial** je klijentska aplikacija koji podržava interakciju sa servisima za obradu i pohranu izvršiti paralelno radno opterećenje na izračunati čvorove (virtualnim strojevima). DotNetTutorial se izvršava na vašem lokalnom radne stanice.

- **TaskApplication** je program koji se izvodi na računalnim čvorove u Azure za izvođenje stvarni posao. U uzorku, `TaskApplication.exe` raščlanjuje tekst iz datoteke preuzete iz Azure prostora za pohranu (ulazni datoteka). Zatim daje tekstnu datoteku (Izlazna datoteka) koja sadrži popis gornje tri riječi koje se prikazuju u ulaznoj datoteci. Nakon što ga Stvori Izlazna datoteka, TaskApplication prenese datoteka Azure prostora za pohranu. To čini dostupnim klijentska aplikacija za preuzimanje. TaskApplication pokreće paralelno više računalnim čvorove u servisu grupe.

Sljedeći dijagram prikazuje primarni postupaka koji se izvode klijentska aplikacija, *DotNetTutorial*i aplikacije koja se izvršava tako da se zadaci *TaskApplication*. Uobičajeni mnogo računalnim rješenja koje su stvorene pomoću obrade je osnovni tijek rada. Dok ne sadrži sve značajke dostupne u servisu grupe, skoro sve grupe scenarij sadrži slične procesa.

![Grupe za tijek rada][8]<br/>

[**Korak 1.**](#step-1-create-storage-containers) Stvaranje **spremnika** u spremište blobova platforme Azure.<br/>
[**Korak 2.**](#step-2-upload-task-application-and-data-files) Prenesite datoteke aplikacije zadatka i unos spremnika.<br/>
[**Korak 3.**](#step-3-create-batch-pool) Stvaranje grupe za **skup**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Skup **StartTask** preuzima zadatka binarne datoteke (TaskApplication) da biste čvorove kako se uključiti u grupu.<br/>
[**Korak 4.**](#step-4-create-batch-job) Stvaranje grupe za **posao**.<br/>
[**Korak 5.**](#step-5-add-tasks-to-job) Dodavanje **zadataka** na posao.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Zadaci koji su zakazane izvršiti na čvorove.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Svaki zadatak preuzimanja ulazne podatke iz spremišta Azure, a zatim počinje izvršavanja.<br/>
[**Korak 6.**](#step-6-monitor-tasks) Praćenje zadataka.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Zadaci se dovršenim prijenosa svoje podatke za pohranu Azure.<br/>
[**Korak 7.**](#step-7-download-task-output) Preuzmite zadatka Izlaz iz prostora za pohranu.

Kao što je rečeno, ne svake grupe rješenje izvodi te točno korake, a mogu sadržavati mnogo više, ali primjer aplikacije *DotNetTutorial* pokazuje uobičajenih procesa koji se nalaze u rješenje grupe.

## <a name="build-the-dotnettutorial-sample-project"></a>Stvaranje projekta uzorka *DotNetTutorial*

Mogli uspješno pokrenuti uzorka, morate navesti obradu i pohranu vjerodajnica računa u programu project *DotNetTutorial* `Program.cs` datoteku. Ako ste još niste učinili, otvorite rješenje u Visual Studio dvoklikom na `DotNetTutorial.sln` datoteku rješenja. Ili je otvoriti iz programa Visual Studio u programu na **Datoteka > Otvaranje > projekta/rješenje** izbornik.

Otvaranje `Program.cs` u projektu *DotNetTutorial* . Zatim dodajte vjerodajnice kao što je navedeno pri vrhu datoteke:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Kao što je rečeno iznad, trenutno u morate navesti vjerodajnice za pohranu račun **opće namjene** Azure prostora za pohranu. Grupe aplikacija pomoću spremište blobova platforme za pohranu subjekta **opće namjene** . Navedite vjerodajnice za pohranu račun koji je stvoren tako da odaberete vrstu računa *spremišta blobova* .

Vjerodajnice račun grupe i pohranu unutar plohu računa za svaki servis možete pronaći na [portal za Azure][azure_portal]:

![Obrada vjerodajnice na portalu][9]
![vjerodajnice za pohranu na portalu][10]<br/>

Sad kad ste ažurirali projekt vjerodajnice, desnom tipkom miša kliknite rješenje u pregledniku rješenja, a zatim kliknite **Stvaranje rješenja**. Ako se od vas zatraži, provjerite vraćanje svih NuGet paketa.

> [AZURE.TIP] Paketi NuGet ne automatski obnoviti ili vam se pojave pogreške o pogreška da biste vratili pakete, provjerite imate li [Upravitelj paketa NuGet] [ nuget_packagemgr] instaliran. Zatim omogućiti preuzimanje paketa nedostaju. U odjeljku [Omogućivanje paketa vraćanje tijekom stvaranja] [ nuget_restore] da biste omogućili preuzimanje paketa.

U sljedećim se odjeljcima smo raščlanjuju primjer aplikacije u korake koji se izvodi za obradu radno opterećenje u servisu grupe, a govori o tim koracima detaljno. Pozivamo vas da biste se pozvali Otvori rješenje u Visual Studio dok radite na način kroz sve ostale u ovom se članku jer ne svaki redak koda u uzorku govori.

Dođite do vrha na `MainAsync` način u programu project *DotNetTutorial* `Program.cs` da biste započeli s korak 1. Sve korake u nastavku, a zatim otprilike slijedi napretka način pozive u `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Korak 1: Stvaranje potpisu

![Stvaranje spremnika u prostor za pohranu za Azure][1]
<br/>

Obradu uključuje ugrađenu podršku za interakciju s Azure prostora za pohranu. Spremnika u vašem računu za pohranu dat će datoteke potrebne za zadatke koji se izvode na vašem računu grupe. Spremnike omogućuje i mjesto za pohranu za izlazne podatke koji daju zadaci. Najprije *DotNetTutorial* klijentska aplikacija ne je stvorite tri spremnika u [Spremište blobova platforme Azure](../storage/storage-introduction.md):

- **aplikacija**: ovaj spremnik će sadržavati aplikacije izvođenje zadataka, kao i bilo koji od njezine ovisnosti, kao što su DLL bibliotekama.
- **unos**: zadaci će preuzimati podatkovnih datoteka za obradu iz *ulaznog* spremnik.
- **Izlaz**: kada zadaci dovršiti obrada ulazne datoteke, će prijenosa rezultate spremniku *Izlaz* .

Da biste mogli raditi s računom za pohranu i stvaranje spremnika, koristimo [Azure prostora za pohranu klijenta biblioteke za .NET][net_api_storage]. Ne možemo stvaranje reference na račun s [CloudStorageAccount][net_cloudstorageaccount], i iz koje stvorite [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Koristimo na `blobClient` upućuju na cijeloj aplikacije i prenesite ga kao parametar na nekoliko načina. Primjer to je u bloku kod koji slijedi navedenog, gdje ćemo poziva `CreateContainerIfNotExistAsync` zapravo stvaranje spremnike.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Kada stvorite spremnike aplikacije sada možete prenijeti datoteke koje će se koristiti tako da se zadaci.

> [AZURE.TIP] [Upute za korištenje spremišta blobova iz .NET](../storage/storage-dotnet-how-to-use-blobs.md) omogućuje dobar pregled rad s Azure potpisu i blob-ova. Pri vrhu popisa čitanje mora biti dok radite s grupe.

## <a name="step-2-upload-task-application-and-data-files"></a>Korak 2: Prijenos zadatka aplikacije i podatkovne datoteke

![Prijenos aplikacije zadatka i unos (podaci) datoteke spremnika][2]
<br/>

Prijenos operacija datoteke *DotNetTutorial* najprije definira zbirke putova datoteka **aplikacije** i **unos** kao postoje na lokalnom računalu. Zatim ga prenosite te datoteke spremnika koji ste stvorili u prethodnom koraku.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Postoje dva načina u `Program.cs` koje su povezane s prijenos postupak:

- `UploadFilesToContainerAsync`: Ova metoda vraća skup [ResourceFile] [ net_resourcefile] objekte (opisan u nastavku) i interno pozive `UploadFileToContainerAsync` da biste prenijeli svaku datoteku koja se prenosi u parametru *filePaths* .
- `UploadFileToContainerAsync`: To je način na koji se zapravo izvodi prijenosa datoteka i stvara [ResourceFile] [ net_resourcefile] objekte. Nakon prenošenja datoteku ga dohvaća zajednički pristup potpis (SAS) datoteke i vraća ResourceFile objekt koji je predstavlja. Zajedničko korištenje programa access potpisi također se spominju ispod.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ net_resourcefile] sadrži zadatke u seriji URL-om s datotekom u Azure prostora za pohranu koji se preuzima računalnim čvor prije pokretanja tom zadatku. [ResourceFile.BlobSource] [ net_resourcefile_blobsource] svojstvo određuje cijeli URL datoteke kao ona nalazi Azure prostora za pohranu. URL-a može obuhvaćati i zajednički pristup potpis (SAS) koji omogućuje siguran pristup datoteci. Većinu zadataka unutar grupe za .NET uključiti svojstvo *ResourceFiles* , uključujući:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

Primjer aplikacije DotNetTutorial koristiti vrste zadataka JobPreparationTask ili JobReleaseTask, ali potražite dodatne informacije o njima u [Pokreni posao Priprema i dovršetka zadaci na obradu Azure izračunati čvorove](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Zajednički pristup potpis (SAS)

Zajednički pristup potpisi su nizovi koji – kada je uključena kao dio URL-a – Omogućivanje sigurnog pristupa spremnika i blob-ova u Azure prostora za pohranu. DotNetTutorial aplikacija koristi blob i spremnik zajednički pristup potpis URL-ovi i pokazuje kako nabaviti Ti nizovi potpis zajednički pristup servisu za pohranu.

- **Blob zajednički pristup potpisi**: StartTask na resurse u DotNetTutorial koristi blob zajednički pristup potpisa kada se preuzima binarne datoteke iz aplikacije i unos podatkovne datoteke sa servisa za pohranu (pročitajte članak korak #3 ispod). Na `UploadFileToContainerAsync` metoda u DotNetTutorial, `Program.cs` sadrži kod koji dohvaća svaki blob zajednički pristup potpis. To čini tako da nazovete [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Spremnik zajednički pristup potpisa**: kao svaki zadatak završi rad na čvor računalnim, prenosi njegov Izlazna datoteka spremniku *Izlaz* u Azure prostora za pohranu. Da biste to učinili, TaskApplication koristi potpis zajednički pristup spremnik koji omogućuje pristup zapisivanju spremnik kao dio put kada se prenese datoteka. Nabavljanje zajednički pristup potpis spremnik obavlja na sličan način kao kada stjecanje blob-om zajedničko korištenje potpisa u programu access. U DotNetTutorial, vidjet ćete koji se `GetContainerSasUrl` Pomoćnik za metodu poziva [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] da biste to učinili. Ćete Pročitajte više o tome kako TaskApplication koristi spremnik zajednički pristup potpis u "koraku 6: praćenje zadataka."

> [AZURE.TIP] Pogledajte Dvodijelno niza na zajednički pristup potpise, [dio 1: osnove modela potpis (SAS) zajednički pristup](../storage/storage-dotnet-shared-access-signature-part-1.md) i [dio 2: Stvaranje i korištenje potpisa zajednički pristup (SAS) s blobova](../storage/storage-dotnet-shared-access-signature-part-2.md), da biste saznali više o slanju siguran pristup podacima na vašem računu za pohranu.

## <a name="step-3-create-batch-pool"></a>Korak 3: Stvaranje grupe za grupu

![Stvaranje grupe za grupu][3]
<br/>

Obradu **skup** je zbirka računalnim čvorove (virtualnim strojevima) na kojem obradu izvršava zadataka s posla.

Nakon što ga prenosi datoteke i podataka na račun za pohranu, *DotNetTutorial* pokreće njegov interakcija sa servisom za obradu pomoću biblioteke .NET grupe. Da biste to učinili, [BatchClient] [ net_batchclient] stvoreno:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Nakon toga će se skup računalnim čvorove stvorene u račun za obradu s pozivom `CreatePoolAsync`. `CreatePoolAsync`koristi [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metoda zapravo stvaranja zajedničko područje u servisu grupe.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Prilikom stvaranja zajedničko područje s [CreatePool][net_pool_create], potrebno je navesti nekoliko parametre kao što su broj računalnim čvorove, [Veličina čvorove](../cloud-services/cloud-services-sizes-specs.md)i na čvorove operacijski sustav. U *DotNetTutorial*koristimo [CloudServiceConfiguration] [ net_cloudserviceconfiguration] da biste odredili Windows Server 2012 R2 servisa u [Oblaku](../cloud-services/cloud-services-guestos-update-matrix.md). Međutim, navođenjem [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] umjesto toga, možete stvoriti grupe čvorove stvorene iz trgovine slike, koji sadrži slike sustava Windows i Linux – dodatne informacije potražite [Linux dodjele resursa za izračun čvorove u grupe za Azure grupe](batch-linux-nodes.md) .

> [AZURE.IMPORTANT] Vam se naplatiti za računalnim resursa u grupe. Da biste minimizirali troškove, možete donji `targetDedicated` 1 prije pokretanja uzorka.

Uz tih svojstava fizičke čvor mogu odrediti [StartTask] [ net_pool_starttask] za na resurse. U StartTask se izvršava na svakom čvor taj čvor spaja na resurse i svaki put čvor ponovnog pokretanja. U StartTask je posebno korisno za instalaciju aplikacije na računalnim čvorove prije izvođenja zadataka. Na primjer, zadataka obradu podataka putem skripti Python nije moguće koristiti u StartTask da biste instalirali Python na čvorove računalnim.

U ovoj aplikaciji uzorak u StartTask kopira datoteke koje se preuzima iz spremišta (koji su navedeni pomoću [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] svojstvo) iz imenika StartTask rad u imeniku za zajedničko korištenje koji mogu pristupiti *svim* zadataka koji se izvode na čvor. Zapravo, to kopira `TaskApplication.exe` i njezine ovisnosti u imeniku za zajedničko korištenje na svakom čvor kao čvor spaja na resurse tako da se svi zadaci koji se pokreću na čvor joj možete pristupiti.

> [AZURE.TIP] Značajka **paketa aplikacije** grupe za Azure omogućuje drugi način da biste dobili aplikacija na čvorove računalnim u zajedničko područje. Detalje potražite u članku [Implementacija aplikacije s paketa aplikacije Azure grupe](batch-application-packages.md) .

Također primjećuje u isječak koda iznad je korištenje dvije varijable okruženja u svojstvu *naredbenog retka* na StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` i `%AZ_BATCH_NODE_SHARED_DIR%`. Svaki čvor računalnim unutar grupe aplikacija za obradu automatski se konfigurira pomoću nekoliko varijable okruženja specifične za obradu. Izvođenja procesa koje je izvršio zadatak ima pristup te varijable okruženja.

> [AZURE.TIP] Da biste saznali više o okruženje varijabli koje su dostupne na računalnim čvorove u skup grupe i informacije o zadatka rad direktorija, potražite u članku sekcije [postavki okruženja za zadatke](batch-api-basics.md#environment-settings-for-tasks) i [datoteke i mape](batch-api-basics.md#files-and-directories) u [grupe za pregled značajke za razvojne inženjere](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Korak 4: Stvaranje obrade

![Stvaranje obrade][4]<br/>

Obrade **posao** zbirku zadataka, a pridruženi skup računalnim čvorove. Zadatke u posao izvršiti na pridruženi skup računalnim čvorove.

Možete koristiti za posao ne samo za organiziranje i praćenje zadataka u povezanih radnih opterećenja, nego i za nametanja ograničenja – kao što je maksimalna runtime za posao (i nastavak, njegov zadaci) te prioritet posao odnosu druge zadatke na računu za obradu. U ovom primjeru no posao pridruženo samo skup koja je stvorena u koraku #3. Konfigurirana bez dodatna svojstva.

Sve obrade su vezane uz određenu grupu. To pridruživanje upućuje na to koji se čvorovi zadataka s posla će se izvoditi na. To navesti pomoću [CloudJob.PoolInformation] [ net_job_poolinfo] svojstvo, kao što je prikazano u nastavku isječak koda.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Sad kad je stvoren posao, zadaci dodaju se za obavljanje posla.

## <a name="step-5-add-tasks-to-job"></a>Korak 5: Dodavanje zadataka posla

![Dodavanje zadataka posla][5]<br/>
*(1) zadaci dodat će se na poslu, (2) zadaci zakazuju pokrenuti čvorove i (3) zadaci preuzimanje podatkovne datoteke za obradu*

Obradu **Zadaci** su pojedinačne jedinice rada izvršiti na čvorove računalnim. Zadatak je naredbenog retka i pokreće skripte ili izvršne datoteke koje ste odredili u tom naredbenog retka.

Da biste zapravo izvršiti, zadaci mora biti dodan na posao. Svaki [CloudTask] [ net_task] je konfiguriran pomoću svojstvo naredbenog retka i [ResourceFiles] [ net_task_resourcefiles] (kao i na resurse StartTask) koje zadatak preuzima čvor prije njegove naredbenog retka automatski izvršava. U programu project uzorka *DotNetTutorial* , svaki zadatak obrađuje samo jednu datoteku. Dakle, njegov zbirke ResourceFiles sadrži jedan element.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Kada pristupi varijable okruženja kao što su `%AZ_BATCH_NODE_SHARED_DIR%` ili izvoditi aplikacija nije pronađen u na čvor `PATH`, reci naredba zadatka mora biti mjestu s `cmd /c`. To izričito će izvršiti naredbu tumačenja i uputite ga da biste prekinuli nakon izvršavanja naredbu. Taj zahtjev je nepotreban zadataka izvršavanje aplikacije u na čvor `PATH` (primjerice *robocopy.exe* ili *powershell.exe*) i koriste bez varijable okruženja.

Unutar na `foreach` petlje u isječak koda iznad, vidjet ćete naredbenog retka za zadatak konstruirana tako da se *TaskApplication.exe*prosljeđuju se tri argumenta naredbenog retka:

1. **Prvi argument** je put datoteke za obradu. To je lokalni put do datoteke kao ona se nalazi na čvor. Kada se u ResourceFile objekt u `UploadFileToContainerAsync` je stvoreno noviji, naziv datoteke korišten za to svojstvo (kao parametar Graditelj ResourceFile). To znači da se datoteku možete pronaći u imeniku isti kao *TaskApplication.exe*.

2. **Drugi argument** određuje gornju *N* riječi moraju biti napisani za izlazna datoteka. U uzorku, to je programiranih tako da se gornji tri riječi zapisuju se Izlazna datoteka.

3. **Treći argument** je zajednički pristup potpis (SAS) koji omogućuje pristup zapisivanju spremnik **Izlaz** iz spremišta Azure. *TaskApplication.exe* koristi ovaj URL potpis zajednički pristup kada se prenese Izlazna datoteka Azure prostora za pohranu. Za to možete pronaći kod u `UploadFileToContainer` način u programu project TaskApplication `Program.cs` datoteke:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Korak 6: Praćenje zadataka

![Praćenje zadataka][6]<br/>
*Klijentska aplikacija (1) nadzire zadaci dovršetka i status uspjeh i (2) zadaci prenijeti podatke za rezultat Azure prostora za pohranu*

Kada se zadaci dodaju na poslu, automatski u redu čekanja i im zakazano za izvršavanje na računalnim čvorove unutar skup povezan s posla. Na temelju postavke koje navedete obradu rukuje sve stavljanje zadatka, Planiranje, implementacijom i drugih zadataka administracije obavezama koje imate umjesto vas. Postoji mnogo postupke za nadzor izvršavanje zadataka. DotNetTutorial prikazuje jednostavnog primjera u kojem se samo na web-mjesto nije uspjelo dovršetka i zadataka ili u okvir za uspjeh izvješća stanja.

Unutar na `MonitorTasks` metoda u DotNetTutorial, `Program.cs`, postoje tri grupe za .NET koncepata koji jamči rasprave. Oni su navedene u njihov redoslijed izgleda:

1. **ODATADetailLevel**: Određivanje [ODATADetailLevel] [ net_odatadetaillevel] popisu operacija (kao što su stjecanje popis zadataka s posla) ključan u osiguravanje performanse aplikacije grupe. Dodajte [učinkovito upita servisa Azure grupe](batch-efficient-list-queries.md) na popis za čitanje Ako planirate ploče sortiranje statusa nadzor unutar grupe aplikacija.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] nudi .NET grupe aplikacija pomoću preglednika uslužni programi za praćenje stanja zadatka. U `MonitorTasks`, *DotNetTutorial* čeka za sve zadatke dosegne [TaskState.Completed] [ net_taskstate] unutar ograničenja za vrijeme. Zatim završava posao.

3. **TerminateJobAsync**: prekidanje zadatak s [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (ili blokiranja JobOperations.TerminateJob) označava taj zadatak kao dovršen. Ključan da biste to učinili Ako rješenje obrade koristi [JobReleaseTask][net_jobreltask]. To je posebnoj vrsti zadatak koji je opisan u [zadacima Priprema i dovršetka posla](batch-job-prep-release.md).

Na `MonitorTasks` način iz *DotNetTutorial*, `Program.cs` pojavljuje se ispod:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Korak 7: Preuzimanje izlaz zadatka

![Preuzimanje zadatka Izlaz iz spremišta][7]<br/>

Sad kad dovršetka posla Izlaz iz zadaci možete preuzeti s Azure prostora za pohranu. To možete učiniti s pozivom `DownloadBlobsFromContainerAsync` u *DotNetTutorial*, `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Poziv na `DownloadBlobsFromContainerAsync` u *DotNetTutorial* aplikacije određuje se da se datoteka preuzeti na vašem `%TEMP%` mapu. Slobodno izmjena ovo mjesto za izlazne podatke.

## <a name="step-8-delete-containers"></a>Korak 8: Spremnici Izbriši

Budući da vam se naplatiti za podatke koji se nalazi u odjeljku pohrana Azure, preporučuje se uvijek u da biste uklonili sve blob-ova koje više nisu potrebne za vaše obrade. U DotNetTutorial, `Program.cs`, to možete učiniti s tri pozive na način Pomoćnik za `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Način sam samo dohvaća referenca spremnik i poziva [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Korak 9: Brisanje posla i na resurse

U posljednjem koraku korisnika se to od vas zatraži da biste izbrisali zadatak i grupe aplikacija koje su stvorene pomoću aplikacije DotNetTutorial. Iako vam se naplatiti za zadatke i zadacima, *su* naplatiti računalnim čvorove. Stoga preporučujemo da dodijeliti čvorove samo po potrebi. Brisanje nekorištenih grupe može biti dio održavanja procesa.

U BatchClient [JobOperations] [ net_joboperations] i [PoolOperations] [ net_pooloperations] obje imaju odgovarajuće načine brisanja, koji se nazivaju ako korisnik potvrditi brisanje:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Imajte na umu da vam se naplatiti za resurse računalnim – brisanje nekorištenih grupe minimiziranje trošak. Osim toga, imajte na umu da brisanje zajedničko područje briše sve čvorove računalnim unutar taj skup, a da sve podatke na čvorove bit će nepopravljiva nakon brisanja na resurse.

## <a name="run-the-dotnettutorial-sample"></a>Pokrenite *DotNetTutorial* uzorka

Kada pokrenete aplikaciju uzorka, izlazna konzole bit će otprilike ovako. Tijekom izvođenja, će se dogoditi Pauziraj pri `Awaiting task completion, timeout in 00:30:00...` dok su pokrenuti na resurse računalnim čvorove. Pomoću [portala za Azure] [ azure_portal] da biste pratili vaše skup, izračunati čvorove, posla i zadatke tijekom i nakon izvršavanja. Pomoću [portala za Azure] [ azure_portal] ili [Azure prostora za pohranu Explorer] [ storage_explorers] da biste pogledali resursa za pohranu (spremnika i blob-ova) koje su stvorene u aplikaciji.

Uobičajeni vrijeme izvođenja je **otprilike 5 minuta** prilikom pokretanja aplikacije njegov zadanoj konfiguraciji.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Daljnji koraci

Slobodno promjene *DotNetTutorial* i *TaskApplication* Eksperimentirajte s računalnim različitim scenarijima. Ako, na primjer, pokušajte dodati programa odgode izvođenja *TaskApplication*, kao što su s [Thread.Sleep][net_thread_sleep], za zamjenu dugoročnih zadaci i praćenje ih na portalu. Pokušajte dodavanjem više zadataka ili Prilagodba broj računalnim čvorove. Dodavanje logike provjeru i omogućuje korištenje postojeće grupe aplikacija za vrijeme izvođenja brzine (*podsjetnik*: pogledajte `ArticleHelpers.cs` u [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] projekt [azure, grupe i uzorke][github_samples]).

Sad kad ste upoznati s osnovni tijek rada rješenja za obradu, vrijeme je za istražujte dodatnim značajkama servisa za grupu.

- Pročitajte [Pregled značajki grupe za razvojne inženjere](batch-api-basics.md), koji se preporučuje za sve nove grupe za korisnike.
- Pokretanje na druge grupe za razvoj članaka u **razvoju detaljnije** u [grupe tečaj][batch_learning_path].
- Pogledajte drugoj implementaciji obrade radno opterećenje "N najgornjih riječi" pomoću grupe u [TopNWords] [ github_topnwords] uzorka.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Stvaranje spremnika u prostor za pohranu za Azure"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Prijenos aplikacije zadatka i unos (podaci) datoteke spremnika"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Stvaranje grupe za grupu"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Stvaranje obrade"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Dodavanje zadataka posla"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Praćenje zadataka"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Preuzimanje zadatka Izlaz iz spremišta"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Obradu rješenja tijeka rada (cijeli dijagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Vjerodajnice za obradu portalu"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Vjerodajnice za pohranu na portalu"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Obradu rješenja tijeka rada (minimalnog dijagram)"
