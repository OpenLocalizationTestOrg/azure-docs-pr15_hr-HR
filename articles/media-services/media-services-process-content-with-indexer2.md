<properties
    pageTitle="Indeksiranje medijskih datoteka uz pretpregled Azure Media indeksiranje 2 | Microsoft Azure"
    description="Azure Media indeksiranje omogućuje vam da biste sadržaj medijske datoteke mogu pretraživati i da biste generirali transkript cijelog teksta za skriveni titlovi i ključne riječi. U ovoj se temi objašnjava korištenje medijskih sadržaja za indeksiranje 2 Preview."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indeksiranje medijskih datoteka uz pretpregled Azure Media indeksiranje 2

##<a name="overview"></a>Pregled

Procesor medijskih sadržaja za **Azure Media indeksiranje 2 Preview** (MP) omogućuje vam da bi medijske datoteke i sadržaj koji se može pretraživati, kao i generiranje zatvorenih skrivene titlove prati. U usporedbi s prethodne verzije [Azure Media indeksiranje](media-services-index-content.md) **Azure Media indeksiranje 2 Preview** izvodi brže indeksiranja i nudi šira podrška za jezike. Podržani jezici obuhvaćaju engleski, španjolski, francuski, njemački, talijanskom, kineski, portugalski i arapski.

**Azure Media indeksiranje 2 Preview** MP trenutno u pretpregledu.

U ovoj se temi objašnjava da biste stvorili poslova indeksiranja **Azure Media indeksiranje 2 Preview**.

>[AZURE.NOTE]Primjena Imajte na umu sljedeće:
>
>Indeksiranje 2 nije podržan u Kini Azure i državne Azure.
>
>Pretpregled ograničeno je na ~ 10 minuta obrade, ali je besplatno svim kupcima.
>
>Kada indeksiranje sadržaja, provjerite je li za korištenje medijske datoteke koje ste vrlo Očisti govora (bez glazbe u pozadini, Šum, efekata ili mikrofona hiss). Neki primjeri odgovarajući sadržaj: snimljena sastanaka, predavanja ili prezentacije. Sljedeći sadržaj možda nije prikladna za indeksiranje: filmovi, prikazuje TV, bilo što s kombiniranim audio i zvučnih efekata loše snimljeni sadržaj s pozadinski Šum (hiss).


U ovoj se temi pojedinosti o **Azure Media indeksiranje 2 Preview** i prikazuje kako ga koristiti uz Media Services SDK za .NET

##<a name="input-and-output-files"></a>Ulazni i izlazni datoteke

###<a name="input-files"></a>Unos datoteke

Audio i videodatoteka

###<a name="output-files"></a>Izlazna datoteka

Indeksiranje posao možete generirati titlova datoteke sljedećih oblika:  

- **sami**
- **TTML**
- **WebVTT**

Nepravilan oblik zatvoren opisa (CC) datoteke u tim oblicima može se koristiti za audio i videodatoteka čine pristupačnim osobe oštećena sluha pročita.

##<a name="task-configuration-preset"></a>Konfiguriranje zadatka (Zadana Postava)

Prilikom stvaranja zadatka indeksiranja s **Azure Media indeksiranje 2 Preview**, morate navesti gotovu konfiguraciju.

Sljedeće JSON postavlja dostupnih parametara.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Podržani jezici  

Azure Media indeksiranje 2 Preview podržava govora u tekst za sljedeće jezike (kada je vrijednost koja određuje naziv jezika u konfiguraciji zadatka, koristite kod 4 znaka u uglatim zagradama kao što je prikazano u nastavku):

- Engleski [EnUs]
- Španjolski [EsEs]
- Kineski [ZhCn]
- Francuski [FrFr]
- Njemački [DeDe]
- Talijanski [ItIt]
- Portugalski [PtBr]
- Arapski (egipatska) [ArEg]


## <a name="sample-code"></a>Ogledni kod

Sljedeći program prikazuje kako:

1. Stvaranje sredstvo i prenijeti medijske datoteke u sredstava.
1. Stvara zadatak s zadatka indeksiranja na temelju konfiguracijska datoteka koja sadrži sljedeće json konfiguracije.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Preuzimanje Izlazna datoteka. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
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
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
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


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Srodne veze

[Azure Media Services pregled Analytics](media-services-analytics-overview.md)

[Azure Media analize pokazni programi](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)