<properties 
    pageTitle="Upravljanje Media Services imovine u više prostora za pohranu računa | Microsoft Azure" 
    description="Članci dajte upute o upravljanju medijskih sadržaja za servise u više računa za pohranu." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako"/>


#<a name="managing-media-services-assets-across-multiple-storage-accounts"></a>Upravljanje Media Services imovine u više prostora za pohranu računa

Počevši od Microsoft Azure Media Services 2.2, možete priložiti više prostora za pohranu računa jednog računa Media Services. Mogućnost da biste priložili više prostora za pohranu računa na račun servisa Media Services pruža sljedeće prednosti:

- Uravnoteženje opterećenja svoje imovine u više računa za pohranu.
- Skaliranja Media Services za velike količine obrade sadržaja (kao što je trenutno prostora za pohranu za jedan račun Maks ograničenje od 500 TB). 

U ovoj se temi objašnjava kako priložiti više prostora za pohranu računa Media Services računom putem Azure usluga upravljanja REST API-JA. Prikazuje i određivanje prostora za pohranu za različite račune prilikom stvaranja koju koristite Media Services SDK. 

##<a name="considerations"></a>Razmatranja

Prilikom pridruživanja više prostora za pohranu računa na račun servisa Media Services, primjenjuju se Imajte na umu sljedeće:

- Prostor za pohranu računi povezan s računom Media Services mora biti u istoj podatkovnog centra kao račun Media Services.
- Trenutno kada je prostor za pohranu račun pridružen navedeni račun Media Services, ga ne može biti odvojena.
- Primarno pohranjivanje računa je označen vrijeme stvaranja računa Media Services. Trenutno nije moguće promijeniti zadani račun za pohranu. 

Druge napomene:

Media Services koristi vrijednost svojstva **IAssetFile.Name** kada sastavljanje URL-ovi za strujanje sadržaj (na primjer, http://{WAMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Zbog toga šifriranje u obliku postotaka nije dopušteno. Svojstvo ime ne smije sadržavati sljedeće [znakove postotak kodiranje-rezervirana](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Osim toga, može postojati samo jedan '. " za nastavak.

##<a name="to-attach-a-storage-account-with-azure-service-management-rest-api"></a>Da biste priložili stvorite račun za pohranu s Azure usluga upravljanja REST API-JA

Trenutno je jedini način pridruživanja više prostora za pohranu računa pomoću [Azure usluga upravljanja REST API -JA](http://msdn.microsoft.com/library/azure/dn167014.aspx). Uzorak koda u na [Kako: korištenje Media Services upravljanje REST API-JA](https://msdn.microsoft.com/library/azure/dn167656.aspx) tema određuje način **AttachStorageAccountToMediaServiceAccount** koji se pridružuje s računom za pohranu navedeni račun Media Services. Kod u istom članku određuje način **ListStorageAccountDetails** koji navodi sve račune za pohranu priložiti navedeni račun Media Services.


##<a name="to-manage-media-services-assets-across-multiple-storage-accounts"></a>Da biste upravljali imovine Media Services u više prostora za pohranu računa

Sljedeći kod koristi najnovije Media Services SDK za izvođenje sljedećih zadataka:

1. Prikaz svih računa za pohranu povezani s računom za navedeni Media Services.
1. Dohvaćanje naziva zadanog računa za pohranu.
1. Stvaranje novog resursa u zadani račun za pohranu.
1. Stvaranje sredstvo izlaz kodiranja posla u navedenom prostora za pohranu računa.
    
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace MultipleStorageAccounts
        {
            class Program
            {
                // Location of the media file that you want to encode. 
                private static readonly string _singleInputFilePath =
                    Path.GetFullPath(@"../..\supportFiles\multifile\interview2.wmv");
        
                private static readonly string MediaServicesAccountName = 
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string MediaServicesAccountKey = 
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static CloudMediaContext _context;
                private static MediaServicesCredentials _cachedCredentials = null;
    
                static void Main(string[] args)
                {
    
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    MediaServicesAccountName,
                                    MediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
    
        
                    // Display the storage accounts associated with 
                    // the specified Media Services account:
                    foreach (var sa in _context.StorageAccounts)
                        Console.WriteLine(sa.Name);
        
                    // Retrieve the name of the default storage account.
                    var defaultStorageName = _context.StorageAccounts.Where(s => s.IsDefault == true).FirstOrDefault();
                    Console.WriteLine("Name: {0}", defaultStorageName.Name);
                    Console.WriteLine("IsDefault: {0}", defaultStorageName.IsDefault);
        
                    // Retrieve the name of a storage account that is not the default one.
                    var notDefaultStroageName = _context.StorageAccounts.Where(s => s.IsDefault == false).FirstOrDefault();
                    Console.WriteLine("Name: {0}", notDefaultStroageName.Name);
                    Console.WriteLine("IsDefault: {0}", notDefaultStroageName.IsDefault);
                    
                    // Create the original asset in the default storage account.
                    IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, 
                        defaultStorageName.Name, _singleInputFilePath);
                    Console.WriteLine("Created the asset in the {0} storage account", asset.StorageAccountName);
                    
                    // Create an output asset of the encoding job in the other storage account.
                    IAsset outputAsset = CreateEncodingJob(asset, notDefaultStroageName.Name, _singleInputFilePath);
                    if(outputAsset!=null)
                        Console.WriteLine("Created the output asset in the {0} storage account", outputAsset.StorageAccountName);
        
                }
        
                static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string storageName, string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    
                    // If you are creating an asset in the default storage account, you can omit the StorageName parameter.
                    var asset = _context.Assets.Create(assetName, storageName, assetCreationOptions);
        
                    var fileName = Path.GetFileName(singleFilePath);
        
                    var assetFile = asset.AssetFiles.Create(fileName);
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    return asset;
                }
        
                static IAsset CreateEncodingJob(IAsset asset, string storageName, string inputMediaFilePath)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My encoding job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.ProtectedConfiguration);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset", storageName,
                        AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish. 
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();
        
                    // Get an updated job reference.
                    job = GetJob(job.Id);
        
                    // If job state is Error the event handling 
                    // method for job progress should log errors.  Here we check 
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        Console.WriteLine("\nExiting method due to job error.");
                        return null;
                    }
        
                    // Get a reference to the output asset from the job.
                    IAsset outputAsset = job.OutputMediaAssets[0];
        
                    return outputAsset;
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                private static void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("********************");
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine("Please wait while local tasks or downloads complete...");
                            Console.WriteLine("********************");
                            Console.WriteLine();
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            Console.WriteLine("An error occurred in {0}", job.Id);
                            break;
                        default:
                            break;
                    }
                }
        
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
            }
        }
 

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
