<properties
    pageTitle="Stvaranje videozapisa Summarization pomoću Azure Media Video minijature | Microsoft Azure"
    description="Video summarization omogućuju stvaranje sažetaka dugo videozapisa odabirom automatski zanimljive isječci iz izvora videozapisa. To je korisno kada želite li omogućiti Kratak pregled što mogu očekivati dugo videozapisa."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>

#<a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Stvaranje videozapisa Summarization pomoću Azure Media Video minijature
##<a name="overview"></a>Pregled

Procesor medijskih sadržaja za **Azure Media Video minijature** (MP) omogućuje vam stvaranje sažetak videozapisa koji je koristan korisnicima samo želite pretpregledati sažetak dugo videozapis. Na primjer, korisnici možda želite vidjeti kratki "sažetak video" kada se pokazivač miša postavite iznad minijature. Po tweaking parametre **Azure Media Video minijature** kroz gotovu konfiguraciju, možete koristiti u MP Napredna snimka otkrivanje i Ulančavanje tehnologija za algorithmically generiranje opisni subclip.  

**Azure Media Video minijaturu** MP trenutno u pretpregledu.

U ovoj se temi pojedinosti o **Azure Media Video minijatura** i prikazuje kako ga koristiti uz Media Services SDK za .NET

##<a name="video-summary-example"></a>Video primjer sažetka 

Evo nekoliko primjera medijske sadržaje procesor Azure Media Video minijature što možete učiniti:

###<a name="original-video"></a>Izvornog videozapisa

[Izvornog videozapisa](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

###<a name="video-thumbnail-result"></a>Video minijatura rezultata

[Video minijatura rezultata](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="task-configuration-preset"></a>Konfiguriranje zadatka (Zadana Postava)

Kada stvorite videozapis minijatura zadatak s **Azure Media Video minijature**, morate navesti gotovu konfiguraciju. Iznad minijature uzorka stvorena je pomoću sljedeće konfiguraciju JSON osnovni:

    {"version":"1.0"}

Trenutno, možete promijeniti sljedećih parametara:

Parametarskog|Opis
---|---
outputAudio|Određuje hoće li konačni videozapis sadrži neki audiozapis. <br/>Dopuštene su vrijednosti: True ili False. Zadana je vrijednost True.
fadeInFadeOut|Određuje hoće li se postupno prijelaze koriste između minijature zasebnom kretanja.  <br/>Dopuštene su vrijednosti: True ili False.  Zadana je vrijednost True.
maxMotionThumbnailDurationInSecs|Cijeli broj koji određuje koliko će cijeli konačni videozapis biti.  Zadani ovisi o izvornu video trajanje.


U sljedećoj tablici opisane zadano trajanje kada **maxMotionThumbnailInSecs** ne koristi.

||||
---|---|---|---|---
Video trajanje|d < 3 min|3 min < d < 15 min|15 min < d < 30 min| 30 min < d
Minijatura trajanje|15 sec (scena 2-3)| 30 sec (3 do 5 scena)|60 sec (5 10 scena)|90 sec (10 do 15 scena)


Sljedeće JSON postavlja dostupnih parametara.
    
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="sample-code"></a>Ogledni kod

Sljedeći program prikazuje kako:

1. Stvaranje sredstvo i prenijeti medijske datoteke u sredstava.
1. Stvara zadatak s video minijatura zadatka na temelju konfiguracijska datoteka koja sadrži sljedeće json konfiguracije. 
        
        {               
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }

1. Preuzimanje Izlazna datoteka. 

###<a name="net-code"></a>Kod .NET
     
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace VideoSummarization
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
            static void Main(string[] args)
            {
    
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    

                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");
    
                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }
    
            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);
    
                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");
    
                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";
    
                var processor = GetLatestMediaProcessorByName(MediaProcessorName);
    
                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);
    
                // Specify the input asset.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
    
                progressJobTask.Wait();
    
                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);
    
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);
    
                return asset;
            }
    
            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }
    
            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));
    
                return processor;
            }
    
            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
    
                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
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
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
        }
    }

###<a name="video-thumbnail-output"></a>Video minijatura Izlaz

[Video minijatura Izlaz](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Srodne veze

[Azure Media Services pregled Analytics](media-services-analytics-overview.md)

[Azure Media analize pokazni programi](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)