<properties
    pageTitle="Licem redaktorskih s Azure media analize | Microsoft Azure"
    description="U ovoj se temi objašnjava kako Redaktura smješka s Azure media analize."
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
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Lice redaktorskih s Azure media analytics

##<a name="overview"></a>Pregled

**Azure Media Redactor** je programa [Azure Media analize](media-services-analytics-overview.md) media procesor (MP) koja nudi redaktorskih skalabilni licem u oblaku. Nominalne redaktorskih omogućuje vam da biste izmijenili videozapis da biste Zamagljenost smješka odabrane pojedinaca. Trebali biste koristiti servis redaktorskih lice u javnom scenariji za sigurnost i medijskih sadržaja vijesti. Nekoliko minuta materijal koja sadrži više smješka može potrajati sata za redakturu ručno, ali uz taj servis postupka redaktorskih nominalne potrebno je samo nekoliko jednostavnih koraka. Dodatne informacije potražite u članku [ovom](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

U ovoj se temi daje pojedinosti o **Azure Media Redactor** i prikazuje kako ga koristiti uz Media Services SDK za .NET.

**Azure Media Redactor** MP trenutno u pretpregledu.

## <a name="face-redaction-modes"></a>Načini rada za redakturu lice

Putem redaktorskih funkcionira otkrivanje smješka u svaki okvir videozapisa i praćenjem nominalne objekt i prosljeđivanja i unatrag u vremenu, tako da se isti pojedinačne koje se mogu blurred iz drugih kutova. Postupak automatskog redaktorskih je vrlo složenim i ne nije uvijek povrće 100% željeni izlazni rezultat, zbog toga Media analize pruža na nekoliko načina za izmjenu konačni izlaz.

Osim potpuno automatsko načinu, postoji tijeka rada s dvije pristupni koji omogućuje na odabira/de-selection od pronađenih smješka putem popis ID-a. Osim toga, da biste proizvoljne po prilagođavanja okvira na MP koristi datoteku metapodataka u JSON OSNOVNI oblik. Ovaj tijek rada je podijelite **Analiza** i **Redact** načina. Možete kombinirati dva načina rada u jednom računanja koji se izvodi oba zadatka u jedan zadatak; u ovom načinu zove **Combined**.

###<a name="combined-mode"></a>Kombinirani načinu rada

To će stvoriti redigirano mp4 automatski bez sve Ručni unos.

Faza|Naziv datoteke|Bilješke
---|---|---
Unos resursa|foo.bar|Videozapis u obliku WMV, MOV i MP4
Konfiguracija za unos|Postavljanje konfiguracije zadatka|{"verzija": "1.0', 'odrednice': {'način':"kombinirati"}}
Izlaz resursa|foo_redacted.MP4|Videozapis s blurring primijeniti

####<a name="input-example"></a>Unos primjer:

[Pogledajte videozapis](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Primjer izlaz:

[Pogledajte videozapis](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Analiza načinu rada

Pristupni **analizirati** tijeka rada za dva fazu uzima videozapisa unos i stvara datoteku JSON nominalne mjesta i jpg slika svakog otkriven nominalne.

Fazu|Naziv datoteke|Bilješke
---|---|----
Unos resursa|foo.bar|Videozapis u obliku WMV, MPV ili MP4
Konfiguracija za unos|Postavljanje konfiguracije zadatka|{"verzija": "1.0', 'odrednice': {'način':"analize"}}
Izlaz resursa|foo_annotations.JSON|Podaci opaske nominalne mjesta u JSON OSNOVNI oblik. To se može uređivati korisnika da biste izmijenili blurring opisani okvire. Pogledajte primjer u nastavku.
Izlaz resursa|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Obrezane jpg svakog otkriven nominalne, gdje je broj označava labelId i

####<a name="output-example"></a>Primjer izlaz:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… Odbacuje decimalni dio broja


###<a name="redact-mode"></a>Redaktura načinu rada

Drugi računanja tijek rada traje veći broj ulaza koje morate posložene jedan resursa.

To obuhvaća popis ID-a za Zamagljenost, izvornog videozapisa i primjedbe JSON. Ovaj način koristi primjedbe da biste primijenili blurring na unos videozapisa.

Izlaz iz računanja analiza ne uključuje izvornog videozapisa. Videozapis mora prenijeti u unos resursa za zadatak način Redact i odabrali kao primarni datoteku.

Faza|Naziv datoteke|Bilješke
---|---|---
Unos resursa|foo.bar|Videozapis u obliku WMV, MPV ili MP4. Isto videozapisa kao korak 1.
Unos resursa|foo_annotations.JSON|datoteku metapodataka primjedbe iz faza onaj s neobavezno izmjene.
Unos resursa|foo_IDList.txt (neobavezno)|Neobavezni novi redak odvojene popis nominalne ID-a za redakturu. Ako ostavite prazno, to blurs sve smješka.
Konfiguracija za unos|Postavljanje konfiguracije zadatka|{"verzija": "1.0', 'odrednice': {'način':"Redaktura"}}
Izlaz resursa|foo_redacted.MP4|Videozapis s blurring primjenjuje na temelju primjedbe

####<a name="example-output"></a>Primjer Izlaz

Ovo je izlaz iz programa IDList s odabranom jednom ID-om.

[Pogledajte videozapis](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Opisi atributa

MP redaktorskih nudi visoke preciznosti nominalne mjesto otkrivanje i praćenje koji može prepoznati do 64 Ljudski smješka u sličicu videozapisa. Frontal smješka pružaju najbolje rezultate, tijekom smješka strani i male smješka (manje od ili jednako 24 x 24 piksela) zahtjevne.

Smješka otkriven i evidentirane se vraćaju s koordinate koji označava mjesto smješka, kao i lice ID broj koji označava praćenje to pojedinačne. Nominalne ID su brojevi podložni da biste ponovno postavili okolnostima kada frontal nominalne izgubite ili Preklapajući u okvir posljedica su neke osobe početak dodijeljeni većeg broja ID-a.

Detaljna objašnjenja za atribute sintaksi [otkriti nominalne i emocije s Azure Media analize](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Ogledni kod

Sljedeći program prikazuje kako:

1. Stvaranje sredstvo i prenijeti medijske datoteke u sredstava.
1. Stvorite zadatak s zadatka redaktorskih nominalne konfiguracijska datoteka koja sadrži sljedeće konfiguracije json na temelju. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Preuzimanje datoteka JSON izlaz. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Srodne veze

[Azure Media Services pregled Analytics](media-services-analytics-overview.md)

[Azure Media analize pokazni programi](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
