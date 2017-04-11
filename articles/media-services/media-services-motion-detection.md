<properties
    pageTitle="Otkrivanje pokreti s Azure Media analize | Microsoft Azure"
    description="Procesor medijskih sadržaja za Azure Media kretanja otkrivatelja (MP) omogućuje učinkovito prepoznavanje sekcije koje vas zanimaju unutar u suprotnom dugi i uneventful videozapis."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Otkrivanje pokreti s Azure Media Analytics

##<a name="overview"></a>Pregled

Procesor medijskih sadržaja za **Azure Media kretanja otkrivatelja** (MP) omogućuje učinkovito prepoznavanje sekcije koje vas zanimaju unutar u suprotnom dugi i uneventful videozapis. Otkrivanje kretanja se može koristiti na materijal statične kameru da biste odredili sekcije videozapisa u kojima se pojavljuje kretanja. Generira JSON datoteku koja sadrži metapodatke s vremenske oznake i opisanog regije gdje došlo je do događaja.

Namijenjena sigurnost videozapisa sažetke sadržaja, tehnologije je Kategorizirajte kretanja u odgovarajuću događaja, a false positives kao što su sjene, a promjene osvjetljenja. Omogućuje stvaranje sigurnosnih upozorenja iz kamere sažetaka sadržaja bez povećavate s neograničene nevažnih događaje, uz mogućnost da biste izdvojili uvodnih kamata iznimno dugo surveillance videozapisi.

**Azure Media kretanja otkrivatelja** MP trenutno u pretpregledu.

U ovoj se temi pojedinosti o **Azure Media kretanja otkrivatelja** i prikazuje kako ga koristiti uz Media Services SDK za .NET


##<a name="motion-detector-input-files"></a>Kretanja otkrivatelja ulaznih datoteka

Videodatoteke. Trenutno podržava sljedećih oblika: MP4, MOV i WMV.

##<a name="task-configuration-preset"></a>Konfiguriranje zadatka (Zadana Postava)

Kada stvorite zadatak s **Azure Media kretanja otkrivatelja**, morate navesti gotovu konfiguraciju. 

###<a name="parameters"></a>Parametri

Možete koristiti sljedeće argumente:

Ime|Mogućnosti|Opis|Zadani
---|---|---|---
sensitivityLevel|Niz: "niska", "Srednje", "Visoka"|Skupovi razina osjetljivosti pri koje pokreti prijavljuje. Da biste prilagodili količinu false positives prilagoditi.|"Srednje"
frameSamplingValue|Pozitivni cijeli broj|Postavlja učestalost na koji se pokreće algoritam. svaki okvir jednako je 1, 2 znači da svaki okvir 2 i tako dalje.|1
detectLightChange|Booleova: "true", "false"|Određuje hoće li se osnovna promjene prijavljene u rezultatima|"False"
mergeTimeThreshold|O. vremena: HH<br/>Primjer: 00:00:03|Određuje termin između kretanja događaja gdje kombinirati i prijavili se kao 1 2 događaja.|00:00:00
detectionZones|Polje zona otkrivanja:<br/>-Zone otkrivanje je polje 3 ili više točaka<br/>-Točka je x i y koordinata od 0 do 1.|U članku se opisuje popis mnogokuta otkrivanje zona će se koristiti.<br/>Rezultati će biti prijavljena s zona kao ID, s najprije jedan "id" se: 0|Jedan zona kojemu su opisane čitavog okvira.

###<a name="json-example"></a>Primjer JSON

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Kretanja otkrivatelja Izlazna datoteka

Otkrivanje posao kretanja će vratiti datoteke JSON u izlaz resursa koji opisuje kretanja upozorenja i njihovih kategorija, unutar njega. Datoteka će sadrži informacije o vrijeme i trajanje kretanja otkriven u videozapisu.

API otkrivatelja kretanja nudi pokazatelja kada su objekti kretanja u fixed pozadine videozapisa (npr. na surveillance videozapisa). Da biste smanjili alarmi false, kao što su osvjetljenja i promjene sjene je put uvježbavan otkrivatelja kretanja. Trenutni ograničenja algoritmima pomoću obuhvaćaju noći vidom videozapisa, djelomično prozirnih objekata i malih objekata.

###<a id="output_elements"></a>Elementi JSON Izlazna datoteka

>[AZURE.NOTE]U najnovijem izdanju, izlazna JSON OSNOVNI oblik promijenio i mogu predstavljati promjena prijelom za neke korisnike.

U sljedećoj tablici opisane elemenata JSON Izlazna datoteka.

Element|Opis
---|---
Verzija|To se odnosi na verziju API videozapis. Trenutna verzija nije 2.
Vremensko mjerilo|"Crtice na osi" sekundi videozapisa.
Pomak|Offset vremena za vremenske oznake u "crtice na osi". U verziji 1.0 API-ji videozapis, to će uvijek biti 0. U budućnosti scenarija podržavamo tu vrijednost može se promijeniti.
Frekvenciju okvira|Slika u sekundi videozapisa.
Širina, visina|Odnosi se na širinu i visinu u pikselima.
Pokretanje|Na početak vremenske oznake u "crtice na osi".
Trajanje|Duljina događaj "crtice na osi".
Interval|Interval za svaku stavku u događaj "crtice na osi".
Događaji|Fragment svaki događaj sadrži kretanja otkriven u tom trajanje.
Vrsta|U trenutnoj verziji, to je uvijek 2 za generički kretanja. Time vam se natpis videozapis API-ji fleksibilnost za kategorizaciju kretanja u budućim verzijama.
RegionID|Kao što je opisano iznad, to će uvijek biti 0 u ovoj verziji. Ova etiketa daje videozapis API fleksibilnost da biste pronašli kretanja u različitim područjima u budućim verzijama.
Područja|Odnosi se na područje na videozapisu na mjesto koje vas zanimaju kretanja. <br/><br/>-"id" predstavlja područje regija – u ovoj verziji postoji samo jedna ID 0. <br/>-"Vrsta" predstavlja oblika regije koje vas zanimaju za kretanja. Trenutno "pravokutnik" i "poligona" podržani.<br/> Ako ste odredili "pravokutnik", područje sadrži dimenzije X, Y, širinu i visinu. Koordinate X i Y predstavljaju XY koordinata gornjoj lijevoj strani područja u normaliziranu skale 0.0 1.0. Širina i visina predstavljaju veličinu regija u normaliziranu skale 0.0 1.0. U trenutnoj verziji, X, Y, širinu i visinu uvijek rješava na 0, 0 i 1, 1. <br/>Ako ste odredili "poligona", područje sadrži dimenzije u točkama. <br/>
Fragmentirane|Metapodaci je chunked prema gore u različitim segmente naziva fragmentirane. Svaki dio sadrži početka, trajanje, broj intervala i event(s). Fragment s bez događajima znači da nema kretanja otkriven tijekom te vrijeme početka i trajanje.
Uglate zagrade]|Svaki uglate zagrade predstavlja jedan interval u događaj. Pražnjenje uglatih zagrada za taj interval znači da je otkriven nijedan kretanja.
mjesta|Taj novi unos u odjeljku događaji navedeni na mjesto gdje se pojavila na kretanja. To je određeno otkrivanje zone.

Slijedi primjer za izlaz JSON

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Ograničenja

- Podržani oblici videozapisa unos obuhvaćaju MP4, MOV i WMV.
- Otkrivanje kretanja optimiziran je za stolna pozadine videozapisa. Algoritam usredotočuje se na smanjivanje alarmi false, kao što su promjene osvjetljenja i sjene.
- Neke kretanja se ne može otkriti zbog tehničke izazove; Npr noći vidom videozapise, djelomično prozirnih objekata i malih objekata.


## <a name="sample-code"></a>Ogledni kod

Sljedeći program prikazuje kako:

1. Stvaranje sredstvo i prenijeti medijske datoteke u sredstava.
1. Stvara zadatak sa zadatkom otkrivanje videozapisa kretanja, na temelju konfiguracijska datoteka koja sadrži sljedeće json konfiguracije. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Preuzimanje datoteke JSON izlaz. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Srodne veze
[Azure Media Services kretanja otkrivatelja bloga](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services pregled Analytics](media-services-analytics-overview.md)

[Azure Media analize pokazni programi](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
