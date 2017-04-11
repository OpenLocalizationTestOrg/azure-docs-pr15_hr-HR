
<properties 
    pageTitle="Upravljanje popisom inventara i povezani entiteti s Media Services .NET SDK" 
    description="Informirajte se o upravljanju imovine i povezani entiteti s Media Services SDK za .NET." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Upravljanje popisom inventara i povezani entiteti s Media Services .NET SDK


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [OSTALE](media-services-rest-manage-entities.md)


U ovoj se temi objašnjava izvršiti sljedeće zadatke upravljanja Media Services:

- Početak referencu resursa 
- Upućivanje posla 
- Popis svih resursi 
- Popis zadataka i resursi 
- Popis svih pravila programa access 
- Popis svih Locators
- Prebrojavanje kroz velikih zbirki entiteti
- Brisanje imovine 
- Brisanje posla 
- Brisanje pravila programa access 

##<a name="prerequisites"></a>Preduvjeti 

U odjeljku [Postavljanje okruženja](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Početak referencu resursa

Česti zadatak je referenca postojeće resursa u Media Services. Sljedeći primjer koda pokazuje kako referencu resursa iz zbirke imovine na poslužitelju Kontekstni objekt koji se temelji na resursa ID-a.
Sljedeći primjer koda koristi Linq upit da biste dobili referenca postojećeg IAsset objekta.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Upućivanje posla

Prilikom rada sa zadacima u kodu Media Services za obradu, često morate nabaviti reference na postojeći posao na temelju Id. Sljedeći primjer koda prikazuje kako se referenca na objekt IJob iz zbirke zadatke.
WarningWarning ćete morati dolaznog reference posao prilikom pokretanja dugoročnih kodiranja posla, a zatim morate Provjera stanja zadatka na niti. U slučajevima ovako kada metodu vraća iz niti, morate dohvatiti osvježene referenca posao.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Popis svih resursi

Kao što je broj imovine u prostor za pohranu poveća, korisno je popis svoje imovine. Sljedeći primjer koda prikazuje kako iteracija putem zbirke imovine na poslužitelju Kontekstni objekt. Sa svakom resursa primjer koda i zapisuje neke od vrijednosti svojstva na konzoli sustava. Svaki resursa, na primjer, mogu sadržavati mnogo medijske datoteke. Primjer koda piše out sve datoteke koje su povezane sa svakom resursa.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Popis zadataka i resursi

Važno povezani zadatak je popis imovine s njihove povezani zadatak u Media Services. Sljedeći primjer koda prikazuje kako popis svaki objekt IJob i zatim za svaki zadatak prikazuje svojstva o posla, sve povezane zadatke, sve unos imovine i imovini izlaz. Kod u ovom primjeru može biti korisno za brojne zadatke. Na primjer, ako želite da biste dobili popis resursi za izlaz iz jednog ili više kodiranja poslove koje ste već pokrenuli, kod prikazuje kako pristupiti imovine izlaz. Kada imate referenca sredstvo Izlaz, pa možete isporučiti sadržaj s drugim korisnicima ili aplikacije preuzimanja ili pruža URL-ova. 

Dodatne informacije o mogućnostima za resursima potražite u članku [Izlaganje imovine s Media Services SDK za .NET](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Popis svih pravila programa Access

U Media Services možete definirati pravila programa access na sredstvo ili njezine datoteke. Pravilnik za pristup definira dozvole za datoteku ili resursa (vrste programa access i trajanje). U kodu Media Services obično definirati pravilo pristup stvaranjem IAccessPolicy objekt, a zatim ga Pridruživanjem postojeće resursa. Zatim stvorite ILocator objektom, čime se omogućuje izravan pristup imovini iz Media Services. Projekt Visual Studio koja se isporučuje se uz ovaj niz dokumentacija sadrži nekoliko primjera kod koji pokazuju kako stvoriti i pravilnicima za pristup i locators dodijeliti resursi.

Sljedeći primjer koda prikazuje kako popis svih pravila pristupa na poslužitelju i prikazuje vrstu dozvole povezane sa svakom. Drugi koristan način za prikaz pravilnike za pristup jest popis svih objekata ILocator na poslužitelju, a zatim za svaki locator možete popisu politiku pridruženi pristup putem njegovo svojstvo AccessPolicy.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Popis svih Locators

Na locator je URL koji daje puta izravno pristupiti resursa, uz dozvole za sredstvo kako je definirano u locator pridruženi pristup pravila. Svaki resursa možete imati zbirku objekata ILocator pridružene je njegovo svojstvo Locators. Kontekst poslužitelja ima Locators zbirku koja sadrži sve locators.

Sljedeći primjer koda sadrži popis svih locators na poslužitelju. Za svaki locator prikazuje Id povezanih resursa i pristup pravila. Također prikazuje vrstu dozvole, datum isteka i cijeli put na imovinu.

Imajte na umu da locator put do sredstvo samo osnovni URL-a sredstava. Da biste stvorili izravno put do pojedinačne datoteke koje nije moguće potražite ime korisnika ili naziv aplikacije, kod morate dodati određene put locator put. Dodatne informacije o tome kako to učiniti potražite u temi [Izlaganje imovine s Media Services SDK za .NET](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Prebrojavanje kroz velikih zbirki entiteti

Prilikom postavljanja upita entiteti, postoji ograničenje od 1000 entiteti vraća odjednom jer javno OSTALE v2 Ograničava rezultate upita 1000 rezultate. Morate koristiti Preskoči i preuzimanje kada prebrojavanje kroz velikih zbirki entiteti. 
    
Sljedeće funkcije petlje kroz sve zadatke u navedeni računa za servise medijske sadržaje. Media Services vraća 1000 zadataka u zadacima zbirke. Funkcija omogućuje korištenje Preskoči i preuzimanje da biste bili sigurni da svi zadaci su videouređaja (u slučaju da imate više od 1000 zadataka na vašem računu).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Brisanje imovine

U sljedećem primjeru briše sredstava.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Brisanje posla

Da biste izbrisali zadatak, morate Provjera stanja zadatka kao što je naznačeno u svojstvu stanje. Moguće je izbrisati zadatke koji su Završi ili otkazana dok zadatke koji su u određenim drugim državama, kao što je u redu čekanja, zakazano ili obradi, najprije morate se otkazati, a zatim moguće je izbrisati.
Sljedeći primjer koda prikazuje način za brisanje posla tako da Provjera stanja zadatka, a zatim izbrišete kada je stanje Završi ili otkazan. Kod ovisi o u prethodnom odjeljku u ovoj temi za početak reference na poslu: upućivanje posao.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Brisanje pravila programa Access

Sljedeći primjer koda pokazuje kako referenca pravilnik pristupa na temelju pravila ID-a, a zatim da biste izbrisali pravilo.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
