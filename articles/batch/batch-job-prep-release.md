<properties
    pageTitle="Zadatak Priprema i čišćenje u seriji | Microsoft Azure"
    description="Zadaci pripreme za posao razinom koristite da biste minimizirali prijenos podataka grupom za Azure izračunati čvorove i otpustite zadaci za čišćenje čvor pri dovršetka posla."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Izvođenje zadataka Priprema i dovršetka posla na čvorove računalnim grupe za Azure

 Azure obradu često zahtijeva neki oblik postavljanje prije njegove zadaci se provode i naknadno zadatak održavanja nakon njegova zadaci. Možda ćete morati preuzeti uobičajenih zadataka ulazne podatke na vašem čvorove računalnim ili je prenesite zadatka izlazne podatke za pohranu Azure nakon dovršetka posla. **Pripremu za posao** i **oslobodi zadatak** zadaci možete koristiti za izvođenje te operacije.

## <a name="what-are-job-preparation-and-release-tasks"></a>Što su pripremu za posao i pustite zadataka?

Prije pokretanja zadataka s posla, pripremu zadatak se izvršava na sve računalnim čvorove zakazano izvođenje najmanje jedan zadatak. Nakon dovršetka posla izdanje zadatak pokreće svaki čvor u grupu koja izvršava najmanje jedan zadatak. Kao i kod normalni obradu zadatke više razine, možete odrediti naredbenog retka treba pozvati kada se zadatak pripreme ili izdanje.

Priprema i izdanje zadataka posla nude poznate značajke obradu zadatka kao što su preuzimanje datoteke ([datoteke resursa][net_job_prep_resourcefiles]), razinom izvođenja, varijable prilagođene okruženja, trajanje najveći izvršavanja, pokušaj count i vrijeme zadržavanja datoteke.

U sljedećim se odjeljcima ćete saznati kako koristiti [JobPreparationTask] [ net_job_prep] i [JobReleaseTask] [ net_job_release] klase pronađeni u [Grupe za .NET] [ api_net] biblioteke.

> [AZURE.TIP] Priprema i izdanje zadataka posla su osobito korisni u "zajednički se koristi skup" okruženja, u koji je skup računalnim čvorove nastavi između pokretanja posla, a koristi mnogo zadataka.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Kada koristite pripremu za posao i otpustite zadataka

Priprema za posao i zadatke posla izdanje su dobro rješenje za u sljedećim situacijama:

**Preuzimanje podataka uobičajenih zadataka**

Obrada često potrebno zajednički skup podataka kao ulaznih za zadatke posla. Ako, na primjer, izračunom dnevnih rizika analizu, tržištu podaci specifičnu za posao, još čestim za sve zadatke u sklopu posla. Ove tržištu podatke, često nekoliko gigabajta veličine, trebali biste može preuzeti svaki čvor računalnim samo jednom tako da ga možete koristiti za svaki zadatak koji se izvodi na čvor. Pomoću **Priprema zadatak** možete preuzeti ove podatke na svakom čvor prije izvođenja posla je Ostali zadaci.

**Brisanje izlaz za posao i zadatke**

U "zajednički se koristi skup" okruženju, gdje je skup računalnim čvorove nisu decommissioned između zadataka, morate izbrisati podatke posao između pokretanja. Možda ćete morati Štednja prostora na disku na čvorove ili zadovoljavaju pravila zaštite vaše tvrtke ili ustanove. Koristite **izdanje zadatak** da biste izbrisali podatke koji je preuzeti po Priprema zadatak ili generiraju tijekom izvođenja zadatka.

**Zadržavanje evidencije**

Možda ćete morati Zadrži kopiju datoteke zapisnika koji stvaraju zadataka ili možda pasti datoteke ispisa koji se mogu stvoriti neuspjelih aplikacija. Pomoću **izdanje zadatak** u tim slučajevima komprimiranje te podatke da biste prenijeli [Spremištem Azure] [ azure_storage] računa.

>[AZURE.TIP] Drugi način održati zapisnika i druge posao i zadatak izlaz podaci za promjenu veličine da biste koristili biblioteku [Konvencije datoteka za obradu Azure](batch-task-output.md) .

## <a name="job-preparation-task"></a>Priprema zadatak

Prije izvođenja zadataka s posla, obradu izvršava pripremu zadatak na svakom računalnim čvor koji je zakazano izvođenje zadatka. Prema zadanim postavkama, servis za obradu čeka za pripremu zadatak izvršiti prije pokretanja zadataka zakazano izvršiti na čvor. Međutim, možete konfigurirati servis nije na čekanje. Ako ponovnog pokretanja čvor Priprema zadatak pokreće ponovno, no možete onemogućiti takvo ponašanje.

Priprema zadatak se izvršava samo na čvorove koji su zakazani pokretanja zadatka. To sprječava nepotrebne izvršavanje zadataka za pripremu za slučaj da čvor se dodijeli zadatak. To se može dogoditi kada je broj zadataka za posao manji broj čvorove u zajedničko područje. Ograničenje se primjenjuje kada [izvođenja Istodobni zadatak](batch-parallel-node-tasks.md) je omogućen, koji ostavlja neke čvorove neaktivnosti ako je broj zadataka manja od broj mogućih Istodobni zadataka. Tako da nije pokrenut Priprema zadatak na Neaktivno čvorove, potrošiti manje novca na naknade za prijenos podataka.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] razlikuje se od [CloudPool.StartTask] [ pool_starttask] u tom JobPreparationTask se izvršava na početku svakog posla dok StartTask izvršava samo kada računalnim čvor prvo spaja u grupu ili ponovnog pokretanja.

## <a name="job-release-task"></a>Zadatak izdanje

Nakon što zadatak je označen kao dovršen, izdanje zadatak se izvršava na svakom čvor u grupu koja izvršava najmanje jedan zadatak. Označili zadatak kao dovršen slanjem zahtjeva za prekinite. Servis za obradu zatim *prekidanje*postavlja stanje posla, prekida bilo koje aktivne ili izvodi zadataka povezan s posla i pokreće izdanje zadatak. Posao prelazi u stanje *Dovršeno* .

> [AZURE.NOTE] Brisanje posla izvršava i izdanje zadatak. Međutim, ako posla već prekinuta, oslobodi zadatak se neće pokrenuti drugi put posao kasnije brisanja.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Zadatak avansne i otpustite zadacima s grupe za .NET

Da biste koristili pripremu zadatak, dodijelite [JobPreparationTask] [ net_job_prep] objekt vaš posao [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] svojstvo. Isto tako, Inicijalizacija [JobReleaseTask] [ net_job_release] i njezino dodavanje vaš posao [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] svojstva koje želite postaviti zadatak izdanje.

U ovom koda `myBatchClient` je instance komponente [BatchClient][net_batch_client], i `myPool` je postojeće grupe aplikacija unutar grupe za račun.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Kao što je rečeno ranije, oslobodi zadatak se izvršava kada je posao prekinuta ili izbrisati. Prekid zadatak s [JobOperations.TerminateJobAsync][net_job_terminate]. Brisanje zadatak s [JobOperations.DeleteJobAsync][net_job_delete]. Obično prekid ili izbrišite posao nakon njegova zadataka ili kada do vremenskog ograničenja koje ste definirali.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Uzorak koda na GitHub

Da biste potražite u članku Priprema za posao i pustite zadataka u praksi, pogledajte [JobPrepRelease] [ job_prep_release_sample] uzorka projekt na GitHub. Ove aplikacije konzole čini sljedeće:

1. Stvara zajedničko područje s dva čvorove "mala".
2. Stvara zadatak s posla Priprema, izdanje i standardne zadatke.
3. Priprema za zadatak, koja najprije zapisuje ID čvor tekstnu datoteku u imeniku za "zajedničko korištenje" na čvor se pokreće.
4. Pokreće se zadatak na svaki čvor koji zapisuje njegov ID zadatka na istom tekstne datoteke.
5. Kada su svi zadaci dovršeni (ili se dosegne vremensko ograničenje), ispisuje sadržaj svaki čvor tekstnu datoteku na konzolu.
6. Nakon dovršetka posla pokreće izdanje zadatak da biste izbrisali datoteku s čvor.
6. Ispisuje kodove izlaz Priprema i izdanje zadataka posla za svaki čvor na kojemu se izvršava.
7. Izvršavanje stanke da biste omogućili potvrdu brisanja zadatka i/ili grupu.

Izlaz iz aplikacije uzorka je slična sljedećoj:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Zbog varijable stvaranje i pokretanje vrijeme čvorove u novu grupu (neke čvorove spremni za zadatke prije drugima), možda ćete vidjeti različite izlaz. Konkretno, jer brzo Dovršavanje zadataka, jedan čvorove na resurse može izvršiti Svi zadaci posla. Ako se to dogodi, primijetit ćete da zadatak Priprema i zadaci izdanje ne postoje za čvor koja izvršava nema zadataka.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Provjera izdanje zadaci na portalu za Azure i Priprema za posao

Kada pokrenete aplikaciju uzorka, možete koristiti za [Azure portal] [ portal] Prikaz svojstava posla i njegov zadataka ili čak i preuzimanje zajedničke tekstnu datoteku koju je izmijenio zadataka s posla.

Snimka zaslona ispod prikazuje **plohu zadatke za pripremu** na portalu za Azure nakon pokretanja aplikacije uzorka. Otvorite svojstva *JobPrepReleaseSampleJob* nakon dovršetka zadataka (ali prije brisanja zadatka i skup) te kliknite **Zadaci pripreme** ili **izdanje zadatke** da biste vidjeli njihova svojstva.

![Svojstva za pripremu zadatka Azure portalu][1]

## <a name="next-steps"></a>Daljnji koraci

### <a name="application-packages"></a>Pakete aplikacija

Osim Priprema zadatak možete koristiti značajku [pakete](batch-application-packages.md) serije Priprema računalnim čvorove za izvršavanje zadataka. Značajka je posebno korisno za implementaciju aplikacije koji zahtijevaju instalirano instalacijski program, aplikacije koje sadrže mnogo datoteka (100 +) ili aplikacije koje zahtijevaju kontrole strict verzija.

### <a name="installing-applications-and-staging-data"></a>Instaliranje aplikacije i pripremnih podataka

Forum oglas MSDN sadrži pregled Pripremanje zadataka koji se izvode na čvorove nekoliko načina:

[Instaliranje aplikacije i pripremnih podataka na obradu izračunati čvorove][forum_post]

Napisao nešto članovi grupe za Azure, ga govori o nekoliko postupaka koje možete koristiti za implementaciju aplikacije i podataka za izračunavanje čvorove.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
