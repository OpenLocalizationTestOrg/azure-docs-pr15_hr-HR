<properties 
    pageTitle="Napredno kodiranje s Media Encoder Standard" 
    description="U ovoj se temi objašnjava izvođenje napredne kodiranje prilagodbom Media Encoder standardne unaprijed postavljeno zadatka. U temi saznati kako koristiti Media Services .NET SDK da biste stvorili kodiranja zadataka i zadatak. Također se prikazuje kako navesti gotove kodiranja posao." 
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
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Napredno kodiranje s Media Encoder Standard

##<a name="overview"></a>Pregled

U ovoj se temi objašnjava izvođenje napredne kodiranja zadataka s Media Encoder Standard. U temi saznati [kako koristiti .NET da biste stvorili zadatak programa kodiranja i posla koji izvršava ovaj zadatak](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Također se prikazuje kako navesti gotove kodiranja zadatak. Opis elemente koji se koriste unaprijed, potražite u članku [ovaj dokument](https://msdn.microsoft.com/library/mt269962.aspx). 

Gotove izvedite sljedeće zadatke kodiranja su planirati:

- [Generiranje minijature](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Obrezivanje videozapisa (isječak)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Stvaranje pomoću preklapanja](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Umetanje tihu audiozapis kada unos ima nema zvuka](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Onemogućivanje automatskog deaktivirali preplitanje](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Unaprijed postavljeno samo za zvuk](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [Spajanje dvaju ili više video datoteka](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Obrezivanje videozapisa Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Umetanje video zapis kada unos sadrži nijedan videozapis](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Rotiranje videozapisa](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Kodiranje s Media Services .NET SDK

Sljedeći primjer koda koristi Media Services .NET SDK za izvođenje sljedećih zadataka:

- Stvaranje kodiranja posao.
- Se referenca Media Encoder standardne kodiranje.
- Učitavanje prilagođeni XML ili JSON unaprijed. Uštedjet ćete XML ili JSON (Ako, na primjer, [XML](media-services-custom-mes-presets-with-dotnet.md#xml) ili [JSON](media-services-custom-mes-presets-with-dotnet.md#json) u datoteku i koristite sljedeći kod učitati datoteku.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Dodajte kodiranja zadatka posla. 
- Navedite unos resursa za kodirati.
- Stvaranje izlaz sredstva koja sadrži kodiranih resursa.
- Dodavanje rukovatelja događajima tijek zadatka.
- Slanje zadatka.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Podrška za relativni veličine

Pri stvaranju minijature ga ne morate navesti izlaz širinu i visinu u pikselima. Možete ih odrediti u postocima u rasponu [1%,..., 100%].

###<a name="json-preset"></a>Zadana Postava JSON 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>Zadana Postava XML
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Generiranje minijature

U ovom se odjeljku objašnjava da biste prilagodili gotovu koje generira minijature. Zadana Postava definirani ispod sadrži informacije o kako želite kodiranje vaše datoteke kao i podatke koji su potrebni za generiranje minijature. Možete poduzeli bilo koju od u Okvirima unaprijed postavljeno dokumentirane [ovdje](https://msdn.microsoft.com/library/mt269960.aspx) i dodati kod koji generira minijature.  

>[AZURE.NOTE]Postavka **SceneChangeDetection** u sljedeće unaprijed mogu samo biti postavljena na true ako su kodiranje koje će se u jednom brzina prijenosa videozapisa. Ako su kodiranje koje će se višestruki-brzina prijenosa videozapisa, a **SceneChangeDetection** postavljen na true, kodiranje vraća pogrešku.  


Informacije o shemi, pogledajte [ovu](https://msdn.microsoft.com/library/mt269962.aspx) temu.

Svakako pročitajte odjeljak [pitanja vezana uz](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>Zadana Postava JSON


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>Zadana Postava XML


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Razmatranja

Primjena Imajte na umu sljedeće:

- Korištenje eksplicitnih vremenske oznake za početak/korak/raspon pretpostavlja da je ulazni izvor dugi najmanje 1 minute.
- Elementi Png/jpg/BmpImage ste Start, a zatim ćemo koraku i atribute niza u rasponu – te moguće ga je protumačiti kao:

    - Okvir broj ako su cijelim brojevima koji nije negativan, primjerice "Pokreni": "120,"
    - U odnosu na izvor trajanje ako izražen kao % – suffixed, primjerice "Pokreni": "15%", ili
    - Vremenska oznaka ako izražen kao hh... Oblikovanje, na primjer "Pokreni": "00: 01:00"

    Možete kombinirati i provjerite podudara notacije tijekom.
    
    Uz to, Start i podržava posebno makronaredbe: {najbolje}, koje pokušava da biste odredili prvi okvir "zanimljive" sadržaja bilješke: (korak, a raspon zanemaruju se kada Start postavljen na {najbolje})
    
    - Zadane postavke: Start: {najbolje}
- Izlazni oblik mora biti izričito dano za svaki oblik slike: Jpg/Png/BmpFormat. Ako postoje, Okvirima bit će jednak JpgVideo za JpgFormat i tako dalje. OutputFormat predstavlja nove makronaredbe određene slike kodek: {Index}, koje mora biti prezentacije (jednom i samo jedanput) za sliku izlaznih oblika.

##<a id="trim_video"></a>Obrezivanje videozapisa (isječak)

U ovom se odjeljku govori o izmjeni unaprijed encoder isječak ili obrezivanje videozapisa unosa gdje je unos so-called mezzanine datoteke ili datoteke na zahtjev. Kodiranje može se koristiti u isječak ili obrezivanje resursa koji je zabilježene ili arhiviraju iz uživo toka – detalja za tu su dostupne u [ovom blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Da biste izdvojili videozapise poduzeli bilo koju od u Okvirima unaprijed postavljeno dokumentirane [ovdje](https://msdn.microsoft.com/library/mt269960.aspx) i izmjena element **izvora** (kao što je prikazano u nastavku). Vrijednost StartTime mora se podudarati apsolutne vremenske oznake unos videozapisa. Na primjer, ako je prvi okvir ulazni videozapis vremenskog pečata 12:00:10.000, zatim StartTime mora biti najmanje 12:00:10.000 i veće. U primjeru u nastavku ćemo pretpostavlja da unos videozapis ima početni vremenske oznake nula. **Izvori** se mora nalaziti na početku na zadana Postava. 
 
###<a id="json"></a>Zadana Postava JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>Zadana Postava XML
    
Da biste izdvojili videozapise poduzeli bilo koju od u Okvirima unaprijed postavljeno dokumentirane [ovdje](https://msdn.microsoft.com/library/mt269960.aspx) i izmjena element **izvora** (kao što je prikazano u nastavku).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Stvaranje pomoću preklapanja

Media Encoder standardne omogućuje preklapanja sliku na postojećeg videozapisa. Trenutno podržava sljedećih oblika: png, jpg, gif i bmp. Zadana Postava definirani ispod je osnovni primjera videozapisa preklapanje.

Osim definiranje unaprijed datoteke, morate omogućiti Media Services znati koja će se datoteka sredstvo je prekriveno slika i datoteka koje je izvor videozapisa na koji želite preko slike. Video datoteka mora biti **primarni** datoteka. 

Gornji primjer .NET definira dvije funkcije: **UploadMediaFilesFromFolder** i **EncodeWithOverlay**. Funkcija UploadMediaFilesFromFolder prenosi datoteke iz mape (na primjer, BigBuckBunny.mp4 i Image001.png) i postavlja mp4 datoteku koja će biti primarni datoteka u sredstava. Funkcija **EncodeWithOverlay** koristi prilagođene unaprijed datoteke proslijeđen ga (na primjer, na zadana postava koji se nalazi iza) da biste stvorili kodiranja zadatak. 

>[AZURE.NOTE]Trenutni ograničenja:
>
>Postavka neprozirnost preklapanja nije podržana.
>
>Izvornu datoteku videozapisa i slikovne datoteke prekriveno moraju biti u istoj resursa i videodatoteka mora biti postavljena kao primarni datoteka u ovom resursa.

###<a name="json-preset"></a>Zadana Postava JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>Zadana Postava XML
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Umetanje tihu audiozapis kada unos ima nema zvuka

Po zadanom ako pošaljete ulaz encoder koja sadrži samo videozapis i nema zvuka zatim izlaz resursa sadrži datoteke koje sadrže samo videozapisa podatke. Neki čitači možda nećete moći rukovati takve strujanja izlaz. Da biste nametnuli encoder da biste dodali tihu audiozapis na Izlaz u tom slučaju možete koristiti tu postavku.

Da biste nametnuli encoder čime se dobiva sredstva koja sadrži tihu audiozapis kada unos ima nema zvuka, odrediti vrijednost "InsertSilenceIfNoAudio".

Poduzeli bilo koju od u Okvirima unaprijed postavljeno dokumentirane [ovdje](https://msdn.microsoft.com/library/mt269960.aspx)i napravite sljedeće izmjene:

###<a name="json-preset"></a>Zadana Postava JSON

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>Zadana Postava XML

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Onemogućivanje automatskog deaktivirali preplitanje

Korisnici ne morate ništa učiniti ako žele interlace sadržaj se automatski deaktivirali isprepleteno. Kada je automatski deaktivirali preplitanje uključen (zadano) u Okvirima ne automatsko otkrivanje isprepleteno okvira i samo njezini interlaces okvira označen kao prepleteno.

Možete isključiti automatsko deaktivirali preplitanje. Ne preporučuje se tu mogućnost.

###<a name="json-preset"></a>Zadana Postava JSON
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>Zadana Postava XML
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Unaprijed postavljeno samo za zvuk

U ovom se odjeljku objašnjavaju dva samo zvuk Okvirima unaprijed postavljeno: AAC Audio i AAC dobar kvalitete zvuka.

###<a name="aac-audio"></a>AAC zvuka 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>AAC dobru kvalitetu zvuka

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>Spajanje dvaju ili više video datoteka

Sljedeći primjer prikazuje kako možete generirati zadana postava za spajanje dva ili više videodatoteke. Najčešći scenarij je kada želite dodati zaglavlja ili je Filmska glazba na glavnom videozapis. Namjena je kada videodatoteke uređuje zajedno zajedničko korištenje svojstva (razlučivost sekundi, Brojanje audiozapis, itd.). Trebali biste voditi brigu ne želite kombinirati videozapise u drugom okviru stope ili pak s različit broj audiozapisa.

###<a name="requirements-and-considerations"></a>Preduvjeti i napomene

- Unos videozapisi treba imati samo jedan audiozapis.
- Unos videozapisi moraju imati isti sekundi.
- Morate prenijeti videozapise u zasebnom resursi i postavite videozapisa kao primarni datoteke u svakom resursa.
- Morate znati trajanje videozapise.
- Unaprijed postavljeni primjere u nastavku pretpostavlja unos videozapisi počinju vremenskog pečata nula. Ćete morati izmijeniti vrijednosti StartTime ako videozapise imaju različite početne vremenske oznake, kao što je obično u slučaju uživo arhiva.
- Zadana Postava JSON čini eksplicitne reference na vrijednosti idstavke unos amortizacije.
- Ogledni kod pretpostavlja da zadana postava JSON spremanja u lokalnu datoteku, kao što su "C:\supportFiles\preset.json". Također pretpostavlja se da dva imovine je stvorio prijenos dva videodatoteke, a znate konačni vrijednosti idstavke.
- Isječak koda i JSON zadana postava prikazuje primjer Ulančavanje dva videodatoteke. Proširivanje ga na više od dva videozapise prema:

    1. Pozivanje zadatak. InputAssets.Add() pritišćite tipku TAB da biste dodali dodatne videozapise u redoslijedu.
    2. Uređuje element "Izvora" u JSON, dodavanjem više stavki u istoj narudžbi upućivanje odgovara. 


###<a name="net-code"></a>Kod .NET

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>Zadana Postava JSON

Ažurirajte prilagođene konfiguracije ID-a imovine koju želite concatenate i segment odgovarajuće vremena za svaki videozapis.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Obrezivanje videozapisa Media Encoder Standard

Potražite u temi [obrezivanje videozapisa Media Encoder Standard](media-services-crop-video.md) .

##<a id="no_video"></a>Umetanje video zapis kada unos sadrži nijedan videozapis

Po zadanom ako pošaljete ulaz encoder koja sadrži samo audio i nijedan videozapis, zatim resursa za izlaz sadrži datoteke koje sadrže samo audio podatke. Neke uređaje uključujući Azure Media Player (pogledajte [ovaj](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) možda nećete moći učiniti takve strujanja. Da biste nametnuli encoder da biste dodali jednobojne videozapis na Izlaz u tom slučaju možete koristiti tu postavku. 

>[AZURE.NOTE]Prisilno encoder da biste umetnuli videozapis koji izlaz povećava veličinu izlaz resursa, i time trošak nastale kodiranja zadatka. Pokrećite testovi Provjerite postoji li ovaj konačni povećava samo modest utjecaj na mjesečni troškovi.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Umetanje videozapisa na samo najniže brzina prijenosa

Pretpostavimo da koristite na više brzina prijenosa kodiranje unaprijed postavljeni kao što su ["H264 više brzina prijenosa 720p"](https://msdn.microsoft.com/library/mt269960.aspx) za kodiranje cijeli unos katalog za strujanje, koji sadrži videodatoteke i datoteke samo za zvuk. U ovom scenariju kada unos sadrži nijedan videozapis, trebali biste prisilno encoder da biste umetnuli jednobojne videozapis na samo na najniže brzina prijenosa, umjesto umetnete videozapis na svaku izlaz brzina prijenosa. Da biste to postigli, morate navesti zastavice "InsertBlackIfNoVideoBottomLayerOnly".

Poduzeli bilo koju od u Okvirima unaprijed postavljeno dokumentirane [ovdje](https://msdn.microsoft.com/library/mt269960.aspx)i napravite sljedeće izmjene:

#### <a name="json-preset"></a>Zadana Postava JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Zadana Postava XML

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Umetanje videozapisa uopće izlaz bitrates

Pretpostavimo da koristite na više brzina prijenosa kodiranje unaprijed postavljeni kao što su ["H264 više brzina prijenosa 720p](https://msdn.microsoft.com/library/mt269960.aspx) za kodiranje cijeli unos katalog za strujanje, koji sadrži videodatoteke i datoteke samo za zvuk. U ovom scenariju kada unos sadrži nijedan videozapis, trebali biste prisilno encoder da biste umetnuli jednobojne videozapis uopće bitrates izlaz. To osigurava izlaz Resursi su sve homogenous vezana uz broj prati video i audiozapisa. Da biste to postigli, morate navesti zastavice "InsertBlackIfNoVideo".

Poduzeli bilo koju od u Okvirima unaprijed postavljeno dokumentirane [ovdje](https://msdn.microsoft.com/library/mt269960.aspx)i napravite sljedeće izmjene:

#### <a name="json-preset"></a>Zadana Postava JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Zadana Postava XML
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Rotiranje videozapisa

[Media Encoder standardne](media-services-dotnet-encode-with-media-encoder-standard.md) podržava zakretanje po kutove od 0/90/180/270. Zadano ponašanje je "Automatski", gdje pokuša Odredi metapodataka za zakretanje u dolazne video datoteka, a vaše za njega. Uključi sljedeći element **izvora** u jednu od na unaprijed postavljeno definirani [ovdje](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>Zadana Postava JSON

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>Zadana Postava XML

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Pogledajte [u ovoj](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) se temi dodatne informacije na kako kodiranje tumači postavki širine i visine u zadana Postava, kada se pokrene plaćati rotacije.

Da biste naznačili encoder da biste zanemarili metapodataka za zakretanje, ako postoji u unos videozapisa, poslužite se vrijednost "0". 


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vidi također 

[Media Services kodiranja pregled](media-services-encode-asset.md)
