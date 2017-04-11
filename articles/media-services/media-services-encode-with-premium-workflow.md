<properties 
    pageTitle="Napredno kodiranje s tijekom rada sustava Media Encoder Premium | Microsoft Azure" 
    description="Saznajte kako šifrirati Premium Media Encoder tijekom rada. Primjere koda zapisuju u C# i korištenje Media Services SDK za .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Napredno kodiranje Premium Media Encoder tijekom rada

>[AZURE.NOTE] Tijek rada za Media Encoder Premium media procesor spominju u ovoj temi nije dostupna u Kini.

Premium encoder pitanja, e-pošte mepd na Microsoft.com.

##<a name="overview"></a>Pregled

Microsoft Azure Media Services je Uvod media procesor **Tijeka rada za Media Encoder Premium** . U ovom kodiranje mogućnosti za tijekove rada osvježavati premium ponuda advance "procesor". 

U sljedećim temama strukture detalja vezanih uz **Tijeka rada za Media Encoder Premium**: 

- [Podržani oblici tako da se tijek rada Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) – Discusses datoteku oblici i kodecima podržava **Media Encoder Premium tijeka**.

- U odjeljku [Usporedba koderi](media-services-encode-asset.md#compare_encoders) uspoređuje kodiranja mogućnosti **Media Encoder Premium tijek** rada i **Media Encoder Standard**.

U ovoj se temi objašnjava kako kodiranje **Premium tijeka rada za Media Encoder** pomoću .NET.

Kodiranja zadaci **Tijeka rada za Media Encoder Premium** zahtijeva zasebne konfiguracijska datoteka, pod nazivom datoteke tijeka rada. Te datoteke s nastavkom .workflow i stvaraju se pomoću alata za [Brzu analizu](media-services-workflow-designer.md) .

##<a name="encode"></a>Kodiranje

Kodiranja zadaci **Tijeka rada za Media Encoder Premium** zahtijeva zasebne konfiguracijska datoteka, pod nazivom datoteke tijeka rada. Te datoteke s nastavkom .workflow i stvaraju se pomoću alata za [Brzu analizu](media-services-workflow-designer.md) .


Možete pristupati i Zadana tijeka rada datoteke [ovdje](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Mapa sadrži i opis te datoteke.

Datoteka za tijek rada moraju prenijeti na račun servisa Media Services kao sredstvo, a ovaj resursa moraju se proslijediti u kodiranja zadatka.

Sljedeći primjer pokazuje kako šifrirati **Premium Media Encoder**tijekom rada. 

Izvodi se na sljedeći način: 
 
1. Stvaranje sredstvo i prijenos datoteka za tijek rada. 
2. Stvaranje sredstvo i medijsku datoteku izvora.
3. Pronađite procesor medijskih sadržaja za "Media Encoder Premium tijeka rada".
4. Stvorite zadatak i zadatak. 

    U većini slučajeva je prazan niz konfiguraciju za zadatak (kao u sljedećem primjeru). Postoje neke Napredne scenarije (zbog kojih morate za postavljanje svojstava za vrijeme izvođenja dinamički) u tom slučaju bi im XML niz kodiranja zadatak. Primjeri takve scenarije: Stvaranje preklapanja, paralelni ili uzastopnih media Šivanje, subtitling.
5. Dodajte dva resursi za unos zadatka.
    
    na. 1st – resursa tijeka rada.

    b. 2. – videozapis resursa.
    
    **Napomena**: resursa tijek rada mora biti dodan zadatku prije medijskih resursa. Konfiguriranje niz za taj zadatak mora biti prazna. 

6. Slanje kodiranja posla.

Slijedi primjer s dovršeno. Informacije o tome kako postaviti s razvoj Media Services .NET potražite u članku [razvoja Media Services s .NET](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
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
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
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
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
            }
    
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }


Premium encoder pitanja, e-pošte mepd na Microsoft.com.

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
