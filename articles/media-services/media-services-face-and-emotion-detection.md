<properties
    pageTitle="Otkrivanje nominalne i emocije s analize Azure Media | Microsoft Azure"
    description="U ovoj se temi objašnjava kako prepoznati smješka i nalaze s Azure Media analize."
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
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Otkrivanje nominalne i emocije s Azure Media Analytics

##<a name="overview"></a>Pregled

Procesor medijskih sadržaja za **Azure Media nominalne otkrivatelja** (MP) omogućuje vam Brojanje, praćenje premještanja, a čak i procijeni sudjelovanje ciljne skupine i izvršavanju putem putem izraza. Taj servis sadrži dvije značajke: 

- **Otkrivanje nominalne**

    Otkrivanje nominalne pronalazi i prati Ljudski smješka unutar videozapisa. Više smješka vidljivo i naknadno pratiti kao što su pomicati, s vremenom i mjestom metapodacima vraća u datoteci JSON. Tijekom praćenje će pokušati dati dosljedan ID isti nominalne dok je osoba koja je kretanje na zaslonu, čak i ako se mogu narušiti ili nakratko ostavite okvir.

    >[AZURE.NOTE]Ovaj services izvršiti putem prepoznavanja. Osobe koja ostavlja okvira ili postaje mogu narušiti za predugo će dobiti novi ID kada se vratite.

- **Otkrivanje emocije**
    
    Otkrivanje emocije je neobavezna komponenta procesora nominalne otkrivanje medijske sadržaje koji vraća analizu na više emocionalne atributa iz smješka otkrije, uključujući sreća, sadness, problema, anger i više. 

**Azure Media nominalne otkrivatelja** MP trenutno u pretpregledu.

U ovoj se temi daje pojedinosti o **Azure Media nominalne otkrivatelja** i prikazuje kako ga koristiti uz Media Services SDK za .NET.

##<a name="face-detector-input-files"></a>Licem otkrivatelja ulaznih datoteka

Videodatoteke. Trenutno podržava sljedećih oblika: MP4, MOV i WMV.

##<a name="face-detector-output-files"></a>Licem otkrivatelja Izlazna datoteka

API za otkrivanje i praćenje nominalne nudi visoke preciznosti nominalne mjesto otkrivanje i praćenje koji mogu otkriti do 64 Ljudski smješka u videozapisu. Frontal smješka pružaju najbolje rezultate, tijekom smješka strani i small smješka (manje od ili jednako 24 x 24 piksela) možda neće biti toliko točan.

Otkriven i evidentirane smješka se vraćaju s koordinate (lijevo, gore, širina i visina) koji označava mjesto smješka u slike u pikselima, kao i nominalne ID broj koji označava praćenje to pojedinačne. Nominalne ID su brojevi podložni da biste ponovno postavili okolnostima kada frontal nominalne izgubite ili Preklapajući u okvir posljedica su neke osobe početak dodijeljeni većeg broja ID-a.

###<a id="output_elements"></a>Elementi JSON Izlazna datoteka

Za otkrivanje nominalne i postupak praćenja izlazni rezultat sadrži metapodatke iz smješka unutar zadanog datoteke u obliku JSON.

Otkrivanje nominalne i praćenje JSON obuhvaća sljedećim atributima:

Element|Opis
---|---
Verzija|To se odnosi na verziju API videozapis.
Vremensko mjerilo|"Crtice na osi" sekundi videozapisa.
Pomak|Ovo je vrijeme pomak za vremenske oznake. U verziji 1.0 API-ji videozapis, to će uvijek biti 0. U budućnosti scenarija podržavamo tu vrijednost može se promijeniti.
Frekvenciju okvira|Slika u sekundi videozapisa.
Fragmentirane|Metapodaci je chunked prema gore u različitim segmente naziva fragmentirane. Svaki dio sadrži početka, trajanje, broj intervala i event(s).
Pokretanje|Vrijeme početka prvi događaja u "crtice na osi".
Trajanje|Duljina fragment u "crtice na osi".
Interval|Interval za svaku stavku događaj unutar fragment u "crtice na osi".
Događaji|Svaki događaj sadrži smješka provjeravati i pratiti unutar te vrijeme trajanja. To je polje polja događaja. Vanjski polja predstavlja jednu vremensko razdoblje. Unutarnji polja sastoji se od 0 ili više događaje u tom trenutku u vrijeme. U prazan uglate zagrade [] znači da nije otkriven nijedan smješka.
ID-A| ID nominalne koji se prati. Taj broj nenamjerno možete promijeniti ako lice postane neprepoznate. Dani osoba mora imati isti ID cijeloj cjelokupan videozapis, no to ne može se zajamčiti zbog ograničenja algoritam prepoznavanja (occlusion itd.)
X, Y|Gornji lijevi kut X i Y koordinate nominalne opisani u okvir skalu normaliziranu 0.0 1.0. <br/>-X i Y koordinata su u odnosu na vodoravno uvijek, pa ako imate okomito videozapisa (ili okrenut prema dolje, u slučaju iOS), morat ćete promijeniti raspored koordinate sukladno tome.
Širina, visina|Širinu i visinu nominalne opisani u okvir skalu normaliziranu 0.0 1.0.
facesDetected|Nalazi li se na kraju rezultata JSON i navedene su broj smješka koji algoritam otkrivenog videozapis. Jer ID-ove možete se vratiti slučajno ako lice postane neprepoznate (npr. nominalne vodi izvan zaslona, a zatim traži Odsutan), taj broj može uvijek jednako true broj smješka u videozapisu.

Nominalne otkrivatelja koristi tehnike fragmentacija (gdje metapodatke može biti do oštećenja prema gore u temeljene blokova i može preuzeti samo što vam je potrebno) i segmente (gdje događaje su do oštećenja prema gore u slučaju da bi im se prevelika). Nekoliko jednostavnih izračuna može vam pomoći pretvaranje podataka. Na primjer, ako događaj započeto 6300 (crtice na osi), s Vremensko mjerilo od 2997 (crtice na osi na sec) i frekvenciju okvira od 29.97 (okvira/sec) pa:

- Početak/Vremensko mjerilo = 2.1 sekundi
- Sekundi x (frekvenciju okvira/Vremensko mjerilo) = 63 okvira

U nastavku je jednostavnog primjera izdvajanje JSON u na po okvira oblik za otkrivanje nominalne i praćenje:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Licem otkrivanje ulazni i izlazni primjer

###<a name="input-video"></a>Unos videozapis

[Unos videozapis](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Konfiguriranje zadatka (Zadana Postava)

Kada stvorite zadatak s **Azure Media nominalne otkrivatelja**, morate navesti gotovu konfiguraciju. Sljedeća zadana postava za konfiguraciju je samo za otkrivanje lice.

    {"version":"1.0"}

###<a name="json-output"></a>JSON Izlaz

U sljedećem primjeru JSON izlaza je odrezana.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Otkrivanje emocije ulazni i izlazni primjer


###<a name="input-video"></a>Unos videozapis

[Unos videozapis](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Konfiguriranje zadatka (Zadana Postava)

Kada stvorite zadatak s **Azure Media nominalne otkrivatelja**, morate navesti gotovu konfiguraciju. Sljedeća zadana postava za konfiguraciju određuje se da biste stvorili JSON koji se temelji na otkrivanje emocije.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Opisi atributa

Naziv atributa|Opis
---|---
Način rada|Smješka: Samo licem otkrivanje <br/>AggregateEmotion: Povrata average emocije vrijednosti za sve smješka u okvir.
AggregateEmotionWindowMs|Koristite odaberete način AggregateEmotion. Određuje duljinu videozapis koji se koristi da bi proizveo svaki rezultat zbrajanja u milisekundama.
AggregateEmotionIntervalMs|Koristite odaberete način AggregateEmotion. Određuje koje učestalost čime se dobiva rezultat zbrajanja.

####<a name="aggregate-defaults"></a>Zbrajanja zadanih postavki

Ispod preporučuje se vrijednosti zbroja prozor i interval postavki. AggregateEmotionWindowMs mora biti dulji od AggregateEmotionIntervalMs.

   |Zadane postavke (s)|MAX(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON Izlaz

JSON izlaz za zbrajanja emocije (decimale):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Ograničenja

- Podržani oblici videozapisa unos obuhvaćaju MP4, MOV i WMV.
- Veličina raspon nalaziti neprepoznatljive nominalne je 24 x 24 da biste 2048 x 2048 piksela. Neće biti otkriveni smješka iz ovog raspona.
- Za svaki videozapis maksimalni broj smješka vraća iznosi 64.
- Neke vrste se ne može otkriti zbog tehničke izazove; primjerice vrlo velike nominalne kutova (glave-predstavljati), a zatim velike occlusion. Frontal i pri frontal smješka imati najbolje rezultate.


## <a name="sample-code"></a>Ogledni kod

Sljedeći program prikazuje kako:

1. Stvaranje sredstvo i prenijeti medijske datoteke u sredstava.
1. Stvara zadatak sa zadatkom otkrivanje nominalne, ovisno o konfiguraciji datoteku koja sadrži sljedeće konfiguracije json. 
                    
        {
            "version": "1.0"
        }

1. Preuzimanje datoteke JSON izlaz. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

[Azure Media Services pregled Analytics](media-services-analytics-overview.md)

[Azure Media analize pokazni programi](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)