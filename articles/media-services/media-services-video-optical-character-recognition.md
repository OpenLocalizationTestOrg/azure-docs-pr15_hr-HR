<properties
    pageTitle="Koristite Azure Media Analytics za pretvaranje tekstni sadržaj u videodatoteke u digitalni tekst | Microsoft Azure"
    description="Azure Media analize OCR-a (optičkog prepoznavanja znakova) omogućuje vam da biste pretvorili tekstni sadržaj u videodatoteke u digitalni tekst koji se može uređivati, koji se može pretraživati.  Omogućuje automatizaciju izdvajanje smisleni metapodataka iz videozapisa signal medijski sadržaj."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Korištenje Azure Media analize tekstni sadržaj u videodatoteke pretvoriti u digitalni tekst 

##<a name="overview"></a>Pregled

Ako vam je potrebna za izdvajanje tekstni sadržaj iz svoje videodatoteke i generiranje koji se može uređivati, pretraživati tekst digitalni, trebali biste koristiti Azure Media analize OCR-a (optičkog prepoznavanja znakova). Ovaj Azure Media procesor otkriva tekstni sadržaj u videodatoteke i generira tekstnih datoteka za upotrebu. OCR-a omogućuje vam da biste automatizirali izdvajanje smisleni metapodataka iz videozapisa signal medijski sadržaj.

Kada se koristi u kombinaciji s tražilice, možete jednostavno indeksirati medijskih sadržaja tako da tekst i poboljšati vidljivost sadržaj. Ovo je iznimno je korisna u iznimno tekstnih videozapisa, kao što su videosnimke ili snimiti prezentacije dijaprojekcije. Procesor medijskih sadržaja za Azure OCR-a optimizirana je za digitalni tekst.

Procesor media **Azure Media OCR** trenutno u pretpregledu.

U ovoj se temi daje pojedinosti o **Azure Media OCR -a** i prikazuje kako ga koristiti uz Media Services SDK za .NET. Dodatne informacije i primjeri potražite [u ovom blogu](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>Unos datoteke OCR-a

Videodatoteke. Trenutno podržava sljedećih oblika: MP4, MOV i WMV.

##<a name="task-configuration"></a>Konfiguriranje zadatka 

Konfiguriranje zadatka (Zadana Postava). Kada stvorite zadatak s **Azure Media OCR -a**, morate navesti konfiguracije unaprijed pomoću JSON ili XML. 

###<a name="attribute-descriptions"></a>Opisi atributa

Naziv atributa|Opis
---|---
Jezik|(neobavezno) opisuje jezika teksta koji se izgledati. Nešto od sljedećeg: automatsko otkrivanje (zadano), arapski, ChineseSimplified, ChineseTraditional, Češka danski, nizozemski, engleski, finski, francuski, njemački, grčki, mađarskom, talijanski, japanski, korejski, norveški, poljski, portugalski, rumunjski, ruski, SerbianCyrillic, SerbianLatin, slovačkom, španjolskom, švedski, turski.
TextOrientation|(neobavezno) opisuje usmjerenje teksta koji se izgledati.  "Lijevo", to znači da se na vrh sva slova pokazuje prema lijevoj strani.  Zadani tekst (kao što je koju je moguće pronaći u knjizi) možete pozvati "Gore" usmjeren.  Nešto od sljedećeg: automatsko otkrivanje (zadano), prema gore, desno, dolje, lijevo.
TimeInterval|(neobavezno) opisuje uzorkovanja.  Zadana vrijednost je 1/2 sekundi.<br/>JSON OSNOVNI oblik – hh. SSS (zadano 00:00:00.500)<br/>XML format – W3C XSD trajanje prim (zadano PT0.5)
DetectRegions|(neobavezno) Polje DetectRegion objekata koji određuje područja unutar okvira videozapisa u kojem možete otkriti tekst.<br/>DetectRegion objekt sastoji se od sljedećih četiri cijeli broj vrijednosti:<br/>Lijevo – piksela s lijeva margina<br/>Vrh – piksela od vrha margine<br/>Širina – širinu u pikselima regije<br/>Visina – visina regiju u pikselima

####<a name="json-preset-example"></a>Primjer unaprijed postavljeni JSON
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>XML unaprijed primjer

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>OCR-a Izlazna datoteka

Izlaz media procesor OCR-a je JSON datoteka.

###<a name="elements-of-the-output-json-file"></a>Elementi JSON Izlazna datoteka

Izlaz videozapisa OCR-a nudi vrijeme segmentirajući podataka za znakove koji se nalaze u videozapisu.  Atributi kao što su jezik ili usmjerenje možete koristiti za telefonskog razgovora na točno riječi koji vas zanima analiza. 

Izlaz sadrži sljedećim atributima:

Element|Opis
---|---
Vremensko mjerilo|"crtice na osi" sekundi videozapisa
Pomak|vrijeme pomak za vremenske oznake. U verziji 1.0 API-ji videozapis, to će uvijek biti 0.
Frekvenciju okvira|Slika u sekundi videozapisa
Širina|širinu u pikselima videozapisa
Visina|visinu u pikselima
Fragmentirane|raspon koji se temelji na vrijeme – blokova videozapis u kojem je chunked metapodataka
pokretanje|Početak fragment u "crtice na osi"
Trajanje|Duljina fragment u "crtice na osi"
Interval|Interval svaki događaj unutar zadanog djelić
događaji|polje koje sadrži područja
regija|objekt koji predstavlja otkriven riječi ili izraza
jezik|jezika teksta otkriven unutar područja
usmjerenje|usmjerenje teksta otkriven unutar područja
crta|raspon redaka teksta otkriven unutar područja
tekst|Stvarni tekst

###<a name="json-output-example"></a>Primjer JSON Izlaz

U sljedećem primjeru izlaz sadrži opće informacije videozapisa i nekoliko fragmentirane videozapisa. U svaku videozapisa fragment sadrži svaku regiju koji prepoznaje MP OCR-a s jezikom i njegov usmjerenje teksta. Područje sadrži i svaki redak riječi u tom području s tekst u retku, položaj u retku i svaku riječ informacije (sadržaja programa word, položaj i pouzdanosti) u retku. Slijedi primjer i postavljanja neke umetnute komentare.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Ogledni kod

Sljedeći program prikazuje kako:

1. Stvaranje sredstvo i prenijeti medijske datoteke u sredstava.
1. Stvara zadatak s datotekom konfiguracije/zadana postava OCR-a.
1. Preuzimanje datoteke JSON izlaz. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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
