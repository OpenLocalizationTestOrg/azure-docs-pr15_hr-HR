<properties 
    pageTitle="Pomoću program za Azure Media Pakiranje za obavljanje zadatka u statični pakiranje | Microsoft Azure" 
    description="Ova tema prikazuje različite zadatke koji se izvode s program za Azure Media pakiranje." 
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
    ms.author="juliako"/>


# <a name="using-azure-media-packager-to-accomplish-static-packaging-tasks"></a>Da biste izvršili statične pakiranje zadatke pomoću program za Azure Media pakiranje

>[AZURE.NOTE]Krajnji rok trajanja za Microsoft Azure Media program za pakiranje i Microsoft Azure Media šifriranje proširen ožujak 1, 2017. Prije tog datuma radovi ti procesori dodat će se da biste Media Encoder standardne (TEMA). Klijenti objavit ćemo zajedno s uputama o tome kako migrirati njihove tijekova rada da biste poslali poslove Okvirima. Mogućnosti pretvorbe i šifriranje oblik može biti dostupne putem dinamičkih pakiranje i dinamični šifriranje.

## <a name="overview"></a>Pregled

Da bi se isporuku digitalni videozapise putem Interneta morate Komprimiranje medijskih sadržaja. Digitalne videodatoteke su vrlo velike i možda je prevelika da bi se isporučiti putem Interneta ili za uređaje vaših korisnika da bi se prikazao pravilno. Kodiranje je postupak sažimanje video i audiosadržaji da bi korisnici mogli vidjeti medijskih sadržaja. Kada videozapisa kodiran se može smjestiti u drugu datoteku spremnika. Postupak pri čemu kodiranih medijskih sadržaja u spremniku zove pakiranju. Na primjer, možete preuzeti datoteku MP4 i pretvorite ga u Smooth Streaming ili HLS sadržaja pomoću Azure Media program za pakiranje. 

Media Services podržava dinamične i statične pakiranju. Prilikom korištenja statične pakiranje ćete morati stvoriti kopiju sadržaj u svaki oblik koji je potreban klijenti. Dinamični pakiranje sve što trebate je da biste stvorili sredstva koja sadrži skup prilagodljivo brzina prijenosa MP4 ili Smooth Streaming datoteka. Nakon toga navedenom obliku u manifest ili fragment zahtjevu za strujanje na zahtjev na temelju poslužitelja će osigurati da korisnici primaju strujanje protokol koji su odlučili. Kao rezultat samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i servis Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta.

>[AZURE.NOTE]Preporučuje se da biste koristili [dinamički pakiranju](media-services-dynamic-packaging-overview.md).

No postoje neke scenarije koji zahtijevaju statične pakiranje: 

- Provjera valjanosti prilagodljivo brzina prijenosa MP4s šifriranih vanjski koderi (na primjer, pomoću koderi trećih strana).

Statički pakiranje možete koristiti i za izvođenje sljedećih zadataka. No preporučuje se da biste koristili dinamički šifriranje.

- Pomoću statičke šifriranja da biste prelazili putem glatke i MPEG CRTICE s PlayReady zaštiti
- Pomoću statičke šifriranja da biste zaštitili HLSv3 s AES 128
- Pomoću statičke šifriranja da biste zaštitili HLSv3 s PlayReady


## <a name="validating-adaptive-bitrate-mp4s-encoded-with-external-encoders"></a>Provjera valjanosti prilagodljivo MP4s brzina prijenosa šifriranih vanjski koderi

Ako želite koristiti skup datoteka MP4 prilagodljivo brzina prijenosa (između više – brzina prijenosa) koji su kodirani s koderi Media Services datoteka treba provjeriti prije daljnje obrade. Media Services Program za pakiranje možete provjeriti sredstva koja sadrži skup MP4 datoteke i provjerite je li se sredstva mogu pakirati Smooth Streaming ili HLS. Ako provjera valjanosti zadatak neuspješan, zadatak koji je obradu zadatka dovršite će poruku o pogrešci. XML koji definira zadana postava zadatka provjere valjanosti pronaći ćete u [Zadatka unaprijed postavljeni za program za Azure Media pakiranje](http://msdn.microsoft.com/library/azure/hh973635.aspx) temi.

>[AZURE.NOTE]Koristite Media Encoder Standard čime se dobiva ili na Media Services Program za pakiranje da biste provjerili valjanost sadržaja da biste izbjegli probleme prilikom izvođenja. Ako je poslužitelj strujanje na zahtjev ne mogu raščlaniti izvorne datoteke prilikom izvođenja, primite poruku o pogrešci "415 nepodržane vrsti medija" HTTP 1.1. Pritišćite uzrokuje poslužitelj tako da se neće raščlaniti izvorne datoteke utječu na performanse poslužitelja za strujanje na zahtjev, a može smanjiti propusnosti za posluživanje zahtjeve. Azure Media Services nudi na servis razinu ugovor (SLA) na njegove servise za strujanje osvježavati; Međutim, ovaj SLA ne može biti snazi ako je poslužitelj pogrešnog korištenja način prethodno opisan.

U ovom se odjeljku objašnjava obradi zadatka provjere valjanosti. Također se prikazuje kako provjeriti stanje i poruka o pogrešci posla koji izvršava s JobStatus.Error.

Da biste provjerili valjanost MP4 datoteka s program za Media Services pakiranje, morate stvoriti datoteku manifesta (.ism) i prijenos zajedno s izvorne datoteke u račun za Media Services. Ispod je uzorka .ism datoteke koje je stvorio Media Encoder Standard. U nazivima razlikuju velika i mala slova. Osim toga, provjerite je li tekst u datoteci .ism kodira s UTF-8.

    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
    <!-- Tells the server that these input files are MP4s – specific to Dynamic Packaging -->
        <meta name="formats" content="mp4" /> 
      </head>
      <body>
        <switch>
          <video src="BigBuckBunny_1000.mp4" />
          <video src="BigBuckBunny_1500.mp4" />
          <video src="BigBuckBunny_2250.mp4" />
          <video src="BigBuckBunny_3400.mp4" />
          <video src="BigBuckBunny_400.mp4" />
          <video src="BigBuckBunny_650.mp4" />
          <audio src="BigBuckBunny_400.mp4" />
        </switch>
      </body>
    </smil>

Nakon što dodate prilagodljivo brzina prijenosa MP4 postavljanje koje možete iskoristiti dinamički ambalaže. Dinamični pakiranje omogućuje izlaganje strujanja u navedeni protokol bez dodatno pakiranje. Dodatne informacije potražite u članku [dinamički pakiranju](media-services-dynamic-packaging-overview.md).

Sljedećim primjerom koda koristi Azure Media Services .NET SDK proširenja.  Provjerite je li da biste ažurirali kod tako da pokazuje na mapu u kojoj se nalazi unos MP4 datoteke i datoteke .ism. I i u kojem se nalazi datoteka MediaPackager_ValidateTask.xml. Ova XML datoteka je definiran u temi [Zadatka unaprijed postavljeni za program za Azure Media pakiranje](http://msdn.microsoft.com/library/azure/hh973635.aspx) .
    
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Xml.Linq;
    
    namespace MediaServicesStaticPackaging
    {
        class Program
        {
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
    
            // The MultibitrateMP4Files folder should also
            // contain the .ism manifest file.
            private static readonly string _multibitrateMP4s =
                Path.Combine(_mediaFiles, @"MultibitrateMP4Files");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations";
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Ingest a set of multibitrate MP4s.
                //
                // Use the SDK extension method to create a new asset by 
                // uploading files from a local directory.
                IAsset multibitrateMP4sAsset = _context.Assets.CreateFromFolder(
                    _multibitrateMP4s,
                    AssetCreationOptions.None,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
    
                // Use Azure Media Packager to validate the files.
                IAsset validatedMP4s =
                    ValidateMultibitrateMP4s(multibitrateMP4sAsset);
    
                // Publish the asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    validatedMP4s,
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                                     // Get the streaming URLs.
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(validatedMP4s.GetSmoothStreamingUri().ToString());
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(validatedMP4s.GetMpegDashUri().ToString());
                Console.WriteLine("HLS URL:");
                Console.WriteLine(validatedMP4s.GetHlsUri().ToString());
            }
    
            public static IAsset ValidateMultibitrateMP4s(IAsset multibitrateMP4sAsset)
            {
                // Set .ism as a primary file 
                // in a multibitrate MP4 set.
                SetISMFileAsPrimary(multibitrateMP4sAsset);
    
                // Create a new job.
                IJob job = _context.Jobs.Create("MP4 validation and converstion to Smooth Stream job.");
    
                // Read the task configuration data into a string. 
                string configMp4Validation = File.ReadAllText(Path.Combine(
                        _configurationXMLFiles,
                        "MediaPackager_ValidateTask.xml"));
    
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Create a task with the conversion details, using the configuration data. 
                ITask task = job.Tasks.AddNew("Mp4 Validation Task",
                    processor,
                    configMp4Validation,
                    TaskOptions.None);
    
                // Specify the input asset to be validated.
                task.InputAssets.Add(multibitrateMP4sAsset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted). 
                task.OutputAssets.AddNew("Validated output asset",
                        AssetCreationOptions.None);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
              
                // If the validation task fails and job completes with JobState.Error,
                // display the error message and throw an exception.
                if (job.State == JobState.Error)
                {
                    Console.WriteLine("  Job ID: " + job.Id);
                    Console.WriteLine("  Name: " + job.Name);
                    Console.WriteLine("  State: " + job.State);
    
                    foreach (var jobTask in job.Tasks)
                    {
                        Console.WriteLine("  Task Id: " + jobTask.Id);
                        Console.WriteLine("  Name: " + jobTask.Name);
                        Console.WriteLine("  Progress: " + jobTask.Progress);
                        Console.WriteLine("  Configuration: " + jobTask.Configuration);
                        Console.WriteLine("  Running time: " + jobTask.RunningDuration);
                        if (jobTask.ErrorDetails != null)
                        {
                            foreach (var errordetail in jobTask.ErrorDetails)
                            {
    
                                Console.WriteLine("  Error Message:" + errordetail.Message);
                                Console.WriteLine("  Error Code:" + errordetail.Code);
                            }
                        }
                    }
                    throw new Exception("The specified multi-bitrate MP4 set is not valid.");
                }
    
    
                return job.OutputMediaAssets[0];
            }
    
            static void SetISMFileAsPrimary(IAsset asset)
            {
                var ismAssetFiles = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();
    
                // The following code assigns the first .ism file as the primary file in the asset.
                // An asset should have one .ism file.  
                ismAssetFiles.First().IsPrimary = true;
                ismAssetFiles.First().Update();
            }
        }
    }

## <a name="using-static-encryption-to-protect-your-smooth-and-mpeg-dash-with-playready"></a>Pomoću statičke šifriranja da biste zaštitili prelazili putem glatke i MPEG CRTICE s PlayReady

Ako želite zaštititi sadržaja pomoću PlayReady imate mogućnost korištenja [dinamički šifriranja](media-services-protect-with-drm.md) (Preporučena mogućnost) ili statične šifriranja (kao što je opisano u ovom odjeljku).

Primjer u ovom odjeljku kodira mezzanine datoteke (u ovom slučaju MP4) u prilagodljivo brzina prijenosa MP4 datoteke. Zatim paketi MP4s u Smooth Streaming i šifrira Smooth Streaming s PlayReady. Kao rezultat vam se za strujanje Smooth Streaming ili MPEG CRTICE.

Media Services sada omogućuje usluge za Microsoft PlayReady licence. Primjer u ovom se članku prikazuje kako konfigurirati servis za isporuku Media Services PlayReady licence (pogledajte članak način ConfigureLicenseDeliveryService definirano u kodu niže). Dodatne informacije o servis za isporuku Media Services PlayReady licence potražite u članku [Korištenje PlayReady dinamički šifriranje i servis za isporuku licence](media-services-protect-with-drm.md).

>[AZURE.NOTE]Izlaganje MPEG CRTICE šifrirane i zaštićene PlayReady, provjerite je li da biste koristili mogućnosti CENC postavljanjem svojstava useSencBox i adjustSubSamples (opisana u temi [Zadatka unaprijed postavljeni za šifriranje Azure Media](http://msdn.microsoft.com/library/azure/hh973610.aspx) ) na true.  


Provjerite je li ažurirati sljedeći kod tako da pokazuje na mapu u kojoj se nalazi vaše ulazne MP4 datoteke.

I i gdje se nalaze datoteke MediaPackager_MP4ToSmooth.xml i MediaEncryptor_PlayReadyProtection.xml. MediaPackager_MP4ToSmooth.xml je definirano u [Zadatka unaprijed postavljeni za program za Azure Media pakiranje](http://msdn.microsoft.com/library/azure/hh973635.aspx) i MediaEncryptor_PlayReadyProtection.xml definiran u temi [Zadatka unaprijed postavljeni za Azure Media šifriranje](http://msdn.microsoft.com/library/azure/hh973610.aspx) . 

Primjer određuje način UpdatePlayReadyConfigurationXMLFile koje možete koristiti da biste dinamički ažurira datoteku MediaEncryptor_PlayReadyProtection.xml. Ako imate ključ polazne dostupan, možete koristiti metodu CommonEncryption.GeneratePlayReadyContentKey da biste generirali tipku sadržaja na temelju keySeedValue i KeyId vrijednosti.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace PlayReadyStaticEncryptAndKeyDeliverySvc
    {
        class Program
        {
           
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";
    
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Encoding and encrypting assets //////////////////////
                // Load a single MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);
    
                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset =
                    ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);
    
                // Create a common encryption content key that is used 
                // a) to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) to configure the license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;
    
                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);
    
                // The content key authorization policy must be configured by you 
                // and met by the client in order for the PlayReady license
                // to be delivered to the client. 
                // In this example the Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);
    
                // Get the Media Services PlayReady license delivery URL.
                // This URL will be assigned to the licenseAcquisitionUrl property 
                // of the MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
    
                // Update the MediaEncryptor_PlayReadyProtection.xml file with the key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);
    
    
                // Encrypt your clear Smooth Streaming to Smooth Streaming with PlayReady.
                IAsset outputAsset = CreateSmoothStreamEncryptedWithPlayReady(clearSmoothStreamAsset);
    
    
                // You can use the http://smf.cloudapp.net/healthmonitor player 
                // to test the smoothStreamURL URL.
                string smoothStreamURL = outputAsset.GetSmoothStreamingUri().ToString();
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(smoothStreamURL);
    
                // You can use the http://dashif.org/reference/players/javascript/ player 
                // to test the dashURL URL.
                string dashURL = outputAsset.GetMpegDashUri().ToString();
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(dashURL);
            }
    
            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");
    
                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Stream.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Get the output asset that contains the Smooth Streaming asset.
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Encrypts Smooth Stream with PlayReady.
            /// Then creates a Smooth Streaming Url.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateSmoothStreamEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                // Create a job.
                IJob job = _context.Jobs.Create("Encrypt to PlayReady Smooth Streaming.");
    
                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // The OutputMediaAssets[0] contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                return job.OutputMediaAssets[0];
            }
    
            /// <summary>
            /// Create a common encryption content key that is used 
            /// to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);
    
                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);
    
                return key;
            }
    
            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");
    
                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";
    
                // Prepare the encryption task template
                XDocument doc = XDocument.Load(xmlFileName);
    
                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();
    
                // Update the "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());
    
                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));
    
                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);
    
                doc.Save(xmlFileName);
            }
    
            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
    
                return asset;
            }
    
            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);
    
                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "H264 Multiple Bitrate 720p",
                   TaskOptions.None);
    
                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);
    
                return abrAsset;
            }
    
            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));
    
                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);
    
                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Stream",
                        AssetCreationOptions.None);
    
                return smoothOutputAsset;
            }
    
    
            /// <summary>
            /// Creates a task to encrypt Smooth Streaming with PlayReady.
            /// Note: To deliver DASH, make sure to set the useSencBox and adjustSubSamples 
            /// configuration properties to true. 
            /// In this example, MediaEncryptor_PlayReadyProtection.xml contains configuration.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);
    
                // Read the configuration XML.
                //
                // Note that the configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));
    
                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);
    
                playreadyTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);
    
                return playreadyAsset;
            }
    
            /// <summary>
            /// Configures authorization policy for the content key. 
            /// </summary>
            /// <param name="contentKey">The content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };
    
                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);
    
                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;
    
    
                contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }
    
            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // The following code configures PlayReady License Template using .NET classes
                // and returns the XML string.
    
                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
    
                responseTemplate.LicenseTemplates.Add(licenseTemplate);
    
                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }
    
            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];
    
                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }
    
                return returnValue;
            }
        }
    }

## <a name="using-static-encryption-to-protect-hlsv3-with-aes-128"></a>Pomoću statičke šifriranja da biste zaštitili HLSv3 s AES 128

Ako želite šifrirati vaše HLS s AES 128 imate mogućnost korištenja dinamički šifriranja (Preporučena mogućnost) ili statične šifriranja (kao što je prikazano u ovom odjeljku). Ako odlučite da biste koristili dinamički šifriranje, potražite u članku [Korištenje AES 128 dinamički šifriranja i servis za isporuku ključ](media-services-protect-with-aes128.md).

>[AZURE.NOTE]Da biste pretvorili sadržaj u HLS, morate najprije Pretvori/linijski sadržaj u Smooth Streaming.
>Osim toga, za HLS da biste dobili šifrirane i zaštićene AES provjerite je li u datoteci MediaPackager_SmoothToHLS.xml postaviti sljedeća svojstva: postavite svojstvo šifriranje true, postavite vrijednost ključa i vrijednost keyuri tako da pokazuje na poslužitelj za authentication\authorization.
Media Services će stvoriti datoteku ključa i njegovo stavljanje u spremniku resursa. Kopirajte na /asset-containerguid /*.key datoteke na poslužitelju (ili stvorite ključa datoteku), a zatim izbrišite na *.key datoteke iz spremnik resursa.

Primjer u ovom odjeljku kodira mezzanine datoteke (u ovom slučaju MP4) u multibitrate MP4 datoteke, a zatim u Smooth Streaming paketi MP4s. On zatim paketi Smooth Streaming u HTTP Live strujanje (HLS) šifrirane 128-bitno strujanje šifriranjem dodatno šifriranje standardne (AES). Provjerite je li ažurirati sljedeći kod tako da pokazuje na mapu u kojoj se nalazi vaš unos MP4 datoteka. I i gdje se nalaze MediaPackager_MP4ToSmooth.xml i MediaPackager_SmoothToHLS.xml konfiguracijske datoteke. Definicija za te datoteke možete pronaći u temi [Zadatka unaprijed postavljeni za program za Azure Media pakiranje](http://msdn.microsoft.com/library/azure/hh973635.aspx) .
    
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    
    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths to support files (within the above base path). You can use 
            // the provided sample media files from the "SupportFiles" folder, or 
            // provide paths to your own media files below to run these samples.
    
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
            
            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName = 
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey = 
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName, 
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Encoding and encrypting assets //////////////////////
    
                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);
    
                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);
    
                // Create HLS encrypted with AES.
                IAsset HLSEncryptedWithAESAsset = CreateHLSEncryptedWithAES(clearSmoothStreamAsset);
    
                // You can use the following player to test the HLS with AES stream.
                // http://apps.microsoft.com/windows/app/3ivx-hls-player/f79ce7d0-2993-4658-bc4e-83dc182a0614 
                string hlsWithAESURL = HLSEncryptedWithAESAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with AES URL:");
                Console.WriteLine(hlsWithAESURL);
            }
    
    
            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");
    
                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Get the output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Encrypts an HLS with AES-128.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateHLSEncryptedWithAES(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt to HLS with AES.");
    
                // Add task 1 - Package clear Smooth Streaming to HLS with AES.
                PackageSmoothStreamToHLS(job, clearSmoothStreamAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // The OutputMediaAssets[0] contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                return job.OutputMediaAssets[0];
            }
    
            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
     
                return asset;
            }
    
            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);
    
                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "H264 Multiple Bitrate 720p",
                   TaskOptions.None);
    
                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s", 
                                        AssetCreationOptions.None);
    
                return abrAsset;
            }
    
            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles, 
                            "MediaPackager_MP4toSmooth.xml"));
    
                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);
    
                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset = 
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming", 
                        AssetCreationOptions.None);
    
                return smoothOutputAsset;
            }
    
            /// <summary>
            /// Converts Smooth Streaming to HLS.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The Smooth Streaming asset.</param>
            /// <returns>The asset that was packaged to HLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Read the configuration data into a string. 
                // For the HLS to get encrypted with AES make sure to set the
                // encrypt configuration property to true.
                //
                // In production, it is recommended to do the following:
                //    Set a Key url for your authn/authz server.
                //    Copy the /asset-containerguid/*.key file to your server (or craft a key file for yourself).
                //    Delete *.key from the asset container.
                //
                string configuration = File.ReadAllText(Path.Combine(_configurationXMLFiles, @"MediaPackager_SmoothToHLS.xml"));
    
                // Create a task with the encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth Streaming to HLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(smoothStreamAsset);
    
                // Add an output asset to contain the results of the job. 
                IAsset outputAsset = 
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);
    
    
                return outputAsset;
            }
        }
    }

## <a name="using-static-encryption-to-protect-hlsv3-with-playready"></a>Pomoću statičke šifriranja da biste zaštitili HLSv3 s PlayReady

Ako želite zaštititi sadržaja pomoću PlayReady imate mogućnost korištenja [dinamički šifriranja](media-services-protect-with-drm.md) (Preporučena mogućnost) ili statične šifriranja (kao što je opisano u ovom odjeljku).

>[AZURE.NOTE] Da bi se zaštiti sadržaja pomoću PlayReady morate najprije Pretvori/linijski sadržaj u obliku Smooth Streaming.

Primjer u ovom odjeljku kodira mezzanine datoteke (u ovom slučaju MP4) u multibitrate MP4 datoteke. Datoteka, a zatim paketi MP4s u Smooth Streaming i šifrira Smooth Streaming s PlayReady. Čime se dobiva HTTP Live strujanje (HLS) šifrirane i zaštićene PlayReady resursa PlayReady Smooth Streaming mora biti pakirat u HLS. U ovoj se temi objašnjava kako izvršiti ove korake.

Media Services sada omogućuje usluge za Microsoft PlayReady licence. Primjer u ovom se članku prikazuje kako konfigurirati servis za isporuku Media Services PlayReady licence (pogledajte članak način **ConfigureLicenseDeliveryService** definirano u kodu niže). 

Provjerite je li ažurirati sljedeći kod tako da pokazuje na mapu u kojoj se nalazi vaš unos MP4 datoteka. I i gdje se nalaze datoteke MediaPackager_MP4ToSmooth.xml, MediaPackager_SmoothToHLS.xml i MediaEncryptor_PlayReadyProtection.xml. MediaPackager_MP4ToSmooth.xml i MediaPackager_SmoothToHLS.xml su definirani u [Zadatka unaprijed postavljeni za program za Azure Media pakiranje](http://msdn.microsoft.com/library/azure/hh973635.aspx) i MediaEncryptor_PlayReadyProtection.xml definirano u temi [Zadatka unaprijed postavljeni za Azure Media šifriranje](http://msdn.microsoft.com/library/azure/hh973610.aspx) .
    
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths to support files (within the above base path). You can use 
            // the provided sample media files from the "SupportFiles" folder, or 
            // provide paths to your own media files below to run these samples.
    
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";
    
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);
    
                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);
    
                // Create a common encryption content key that is used 
                // a) to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) to configure the license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;
    
                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);
    
                // The content key authorization policy must be configured by you 
                // and met by the client in order for the PlayReady license
                // to be delivered to the client. 
                // In this example the Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);
    
                // Get the Media Services PlayReady license delivery URL.
                // This URL will be assigned to the licenseAcquisitionUrl property 
                // of the MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
    
                // Update the MediaEncryptor_PlayReadyProtection.xml file with the key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);
    
                // Create HLS encrypted with PlayReady.
                IAsset playReadyHLSAsset = CreateHLSEncryptedWithPlayReady(clearSmoothStreamAsset);
                //
                string hlsWithPlayReadyURL = playReadyHLSAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with PlayReady URL:");
                Console.WriteLine(hlsWithPlayReadyURL);
            }
    
            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");
    
                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Get the output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Create a common encryption content key that is used 
            /// to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);
    
                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);
    
                return key;
            }
    
            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");
    
                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";
    
                // Prepare the encryption task template
                XDocument doc = XDocument.Load(xmlFileName);
    
                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();
    
                // Update the "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());
    
                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));
    
                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);
    
                doc.Save(xmlFileName);
            }
    
            /// <summary>
            // Encrypts clear Smooth Streaming to Smooth Streaming with PlayReady.
            // Then, packages the PlayReady Smooth Streaming to HLS with PlayReady.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateHLSEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt to HLS with PlayReady.");
    
                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);
    
                // Add task 2 - Package to HLS with PlayReady.
                PackageSmoothStreamToHLS(job, encryptedSmoothAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Since we had two tasks, the OutputMediaAssets[1]
                // contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[1],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
    
    
                return asset;
    
            }
            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);
    
                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "H264 Multiple Bitrate 720p",
                   TaskOptions.None);
    
                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);
    
                return abrAsset;
            }
    
            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));
    
                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);
    
                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming",
                        AssetCreationOptions.None);
    
                return smoothOutputAsset;
            }
    
    
            /// <summary>
            /// Converts Smooth Stream to HLS.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The Smooth Stream asset.</param>
            /// <returns>The asset that was packaged to HLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Read the configuration data into a string. 
                //
                string configuration = File.ReadAllText(
                            Path.Combine(_configurationXMLFiles,
                                        @"MediaPackager_SmoothToHLS.xml"));
    
                // Create a task with the encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth to HLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(smoothStreamAsset);
    
                // Add an output asset to contain the results of the job. 
                IAsset outputAsset =
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);
    
    
                return outputAsset;
            }
    
            /// <summary>
            /// Creates a task to encrypt Smooth Streaming with PlayReady.
            /// Note: Do deliver DASH, make sure to set the useSencBox and adjustSubSamples 
            /// configuration properties to true.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);
    
                // Read the configuration XML.
                //
                // Note that the configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));
    
                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);
    
                playreadyTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);
    
    
                return playreadyAsset;
            }
    
    
            /// <summary>
            /// Configures authorization policy for the content key. 
            /// </summary>
            /// <param name="contentKey">The content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };
    
                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);
    
                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;
    
    
                contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }
    
            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // The following code configures PlayReady License Template using .NET classes
                // and returns the XML string.
    
                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
    
                responseTemplate.LicenseTemplates.Add(licenseTemplate);
    
                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }
            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];
    
                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }
    
                return returnValue;
            }
    
        }
    }

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
