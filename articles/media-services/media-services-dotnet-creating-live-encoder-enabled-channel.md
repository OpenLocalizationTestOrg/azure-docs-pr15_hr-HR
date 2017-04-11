<properties 
    pageTitle="Kako izvesti pomoću servisa Azure Media Services da biste stvorili višestruki-brzina prijenosa strujanja .NET uživo strujanje | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvaranje kanala prima jedan-brzina prijenosa aktivno strujanje i kodira strujanje višestruki-brzina prijenosa pomoću .NET SDK-a." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Kako izvesti uživo strujanje pomoću servisa Azure Media Services da biste stvorili višestruki-brzina prijenosa strujanja .NET

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API-JA](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvaranje **kanala** koja prima aktivno strujanje jednostruko-brzina prijenosa i kodira strujanje višestruki-brzina prijenosa.

Dodatne konceptualnih informacija vezanih uz kanalima za koje je omogućena za šifriranje uživo, potražite u članku [Live strujanje pomoću servisa Azure Media Services da biste stvorili strujanja višestruki-brzina prijenosa](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Uobičajeni scenarij strujanje uživo

Sljedeći koraci opisuju zadatke koji su uključeni u stvaranju najčešćih aplikacija uživo strujanje.

>[AZURE.NOTE] Trenutno Maks preporučene uživo događaj traje osam sati. Obratite se amslived na Microsoft.com ako morate pokrenuti kanal za duljeg vremenskog razdoblja.

1. Povezivanje kameru s računalom. Pokretanje i konfiguriranje lokalnog uživo kodiranje koje možete poslati u jednom brzina prijenosa strujanje na jedan od sljedećih protokola: RTMP, Smooth Streaming ili RTP (MPEG-TS). Dodatne informacije potražite u članku [RTMP podrška za Azure Media Services i Live koderi](http://go.microsoft.com/fwlink/?LinkId=532824).

U ovom koraku također može izvršiti nakon što stvorite kanal.

1. Stvaranje i pokretanje kanala.

1. Dohvaćanje kanal ingest URL-a.

Uživo kodiranje koristi ingest URL-a za slanje toka kanal.

1. Dohvaćanje URL-a za pregled kanala.

Pomoću ovog URL-a da biste potvrdili da kanala pravilno prima uživo toka.

2. Stvaranje sredstava.
3. Ako želite resursa dinamički šifriranje tijekom reprodukcije, učinite sljedeće:
1. Stvaranje sadržaja ključa.
1. Konfiguriranje pravila autorizacije sadržaja ključ.
1. Konfiguriranje pravila za isporuku resursa (koristi se dinamički pakiranje i dinamični šifriranje).
3. Stvaranje programa i koristiti resursa koji ste stvorili.
1. Objavljivanje resursa koji je povezan s programom stvaranjem programa locator zahtjev.

Provjerite jeste li imati barem jedno strujanje rezervirane jedinice na strujanje krajnjoj točki iz kojeg želite strujanje sadržaj.

1. Kada ste spremni za početak strujanje i arhiviranja, pokrenite program.
2. Po želji uživo kodiranje možete signaled da biste pokrenuli oglas. Na oglašavanje se umeće u izlaz toka.
1. Zaustavite program kad god želite zaustaviti strujanje i arhiviranja događaj.
1. Brisanje Program (i po želji brisanje imovine).

## <a name="what-youll-learn"></a>Saznat ćete

U ovoj se temi objašnjava dopušta izvršavanje operacija različite kanala i programa pomoću Media Services .NET SDK. Jer mnogi operacije su dugoročnih .NET API-ji koji upravljaju dugotrajne operacije koriste.

U temi objašnjava učinite sljedeće:

1. Stvaranje i pokretanje kanala. API-ji dugoročnih koriste.
1. Početak kanale ingest (ulazni) krajnjoj točki. U ovom krajnje točke biti dostupni za kodiranje koje možete poslati u jednom brzina prijenosa uživo strujanje.
1. Pronađite krajnja točka za pretpregled. Da biste pretpregledali vaše strujanje koristi se ovaj krajnje točke.
1. Stvaranje sredstva koja će se koristiti za pohranu sadržaja. Pravila za isporuku resursa trebali biste konfigurirati kao i, kao što je prikazano u ovom primjeru.
1. Stvaranje programa i koristiti resursa koje ste prethodno stvorili. Pokrenite program. API-ji dugoročnih koriste.
1. Stvorite locator za resursa, da bi se sadržaj dobiva objaviti i može biti kvalitetom strujanja klijentima.
1. Prikaz i skrivanje slates. Pokretanje i zaustavljanje oglasa. API-ji dugoročnih koriste.
1. Čišćenje kanal i sve pridružene resurse.


##<a name="prerequisites"></a>Preduvjeti

Sljedeće su potrebne za dovršetak vodič.

- Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure.

Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F). Dobit ekipa koje je moguće koristiti da biste isprobali plaćenu servisa Azure. Čak i kada koriste na kredita, možete zadržati račun i pomoću besplatnog servisa Azure i značajke, kao što je značajka web-aplikacije u aplikacije servisa za Azure.
- Na račun servisa Media Services. Stvaranje računa Media Services potražite u članku [Stvaranje računa](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate ili Express) ili novije verzije.
- Morate koristiti Media Services .NET SDK verzija 3.2.0.0 ili novija verzija.
- Web-kamere i kodiranje koje možete poslati u jednom brzina prijenosa uživo strujanje.

##<a name="considerations"></a>Razmatranja

- Trenutno Maks preporučene uživo događaj traje osam sati. Obratite se amslived na Microsoft.com ako morate pokrenuti kanal za duljeg vremenskog razdoblja.
- Provjerite jeste li imati barem jedno strujanje rezervirane jedinice na strujanje krajnjoj točki iz kojeg želite strujanje sadržaj.

##<a name="download-sample"></a>Preuzimanje oglednih

Početak i pokretanje uzorka s [ovdje](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Postavljanje za razvoj s Media Services SDK za .NET

1. Stvaranje aplikacije konzole pomoću Visual Studio.
1. Dodajte Media Services SDK za .NET u aplikaciji konzole za korištenje paketa Media Services NuGet.

##<a name="connect-to-media-services"></a>Povezivanje s Media Services
Kao preporučenim načinom rada koristite datoteku app.config za pohranu tipku Media Services imenom i računom.

>[AZURE.NOTE]Da biste pronašli naziv i ključ vrijednosti, idite na portal za Azure i odaberite svoj račun. Pojavit će se prozor postavke na desnoj strani. U prozoru postavke odaberite tipke. Klikom na ikonu pokraj svake tekstni okvir kopira vrijednost u međuspremnik sustava.

Dodavanje sekcije appSettings app.config datoteku i postavite vrijednosti za Media Services imenom i računom ključ za račun.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Primjer koda

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Tražite li nešto drugo?

Ako niste u ovoj se temi sadrže ono što ste očekivali, nema nešto ili u neki drugi način ne zadovoljava vaše potrebe, povratne informacije nam s vama pomoću Disqus niti u nastavku.
