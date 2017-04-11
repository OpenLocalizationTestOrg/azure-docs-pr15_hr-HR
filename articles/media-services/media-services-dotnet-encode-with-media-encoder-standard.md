<properties 
    pageTitle="Kodiranje imovine s Media Encoder standardne koji pomoću .NET | Microsoft Azure" 
    description="U ovoj se temi objašnjava pomoću .NET kodiranje sredstvo pomoću Media Encoder Strandard." 
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
    ms.date="09/19/2016"
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Kodiranje imovine s Media Encoder standardne koji pomoću .NET

Kodiranje poslove su jedna od najčešće obrada postupke u Media Services. Stvaranje kodiranja poslove pretvoriti medijske datoteke iz jedne kodiranje u drugu. Kada kodiranje, možete koristiti Media Encoder ugrađene Media Services. Možete koristiti i kodiranje koje ste dobili od partnera Media Services; trećih strana koderi dostupni su putem servisa Azure Marketplace. 

U ovoj se temi objašnjava pomoću .NET kodiranje imovine s Media Encoder standardne (TEMA). Media Encoder standardni je konfiguriran pomoću jednog od na kodiranje unaprijed postavljeno što je opisano [u nastavku](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Preporučuje se uvijek kodiranje mezzanine datoteka u programa prilagodljivo brzina prijenosa MP4 postavljanje, a zatim skup pretvorili u željeni oblik pomoću [Dinamičke pakiranje](media-services-dynamic-packaging-overview.md). Da biste iskoristili dinamički pakiranje, najprije se najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite [u](media-services-portal-manage-streaming-endpoints.md)članku skaliranje Media Services.

Ako je vaš izlaz resursa za pohranu šifrirane, morate konfigurirati pravila za isporuku resursa. Dodatne informacije potražite u članku [Konfiguriranje resursa isporuke pravila](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]TEMA daje Izlazna datoteka s nazivom koji sadrži 32 znakova naziva unosa datoteke. Naziv se temelji na što je naveden u unaprijed datoteku. Na primjer, "NazivDatoteke": "_ {Basename} {Index} {proširenje}". {Basename} zamijenjen je najprije 32 znakova naziva unosa datoteke.

###<a name="mes-formats"></a>Oblici Okvirima

[Oblici i kodeka](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>Unaprijed postavljeno Okvirima

Media Encoder standardni je konfiguriran pomoću jednog od na kodiranje unaprijed postavljeno što je opisano [u nastavku](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Ulazni i izlazni metapodataka

Kada linijski unos resursa (ili imovine) pomoću TEMA, dobiti sredstvo Izlaz na za uspješan dovršetak koji kodiranje zadatka. Izlaz resursa sadrži videozapis, zvuk, minijature, manifest itd kodiranja zadana postava koristite na temelju.

Izlaz resursa sadrži i datoteke s metapodacima o unos resursa. Naziv datoteke XML metapodatke sadrži sljedećem obliku: < asset_id > _metadata.xml (na primjer, 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), gdje je < asset_id > vrijednost idstavke sredstva za unos. Shema ovog unosa metapodataka XML opisan [u nastavku](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Izlaz resursa sadrži i datoteke s metapodacima o imovini izlaz. Naziv datoteke XML metapodatke sadrži sljedećem obliku: < source_file_name > _manifest.xml (na primjer, BigBuckBunny_manifest.xml). Sheme ovaj metapodataka izlaz XML opisan [u nastavku](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Ako želite da biste pregledali bilo koju od dvije metapodataka datoteke, možete stvoriti SAS locator i preuzeti datoteku s vašim lokalnim računalom. Primjer možete pronaći na stvaranju SAS locator i preuzimanje datoteke pomoću proširenja za SDK .NET Media Services.

##<a name="download-sample"></a>Preuzimanje oglednih

Početak i pokretanje uzorka s [ovdje](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Primjer

Sljedeći primjer koda koristi Media Services .NET SDK za izvođenje sljedećih zadataka:

- Stvaranje kodiranja posao.
- Se referenca Media Encoder standardne kodiranje.
- Odredite da biste koristili u "H264 više brzina prijenosa 720p" unaprijed postavljeni. Možete vidjeti sve unaprijed [ovdje](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Možete provjeriti i sheme na koji moraju biti usklađene ove konfiguracije [ovdje](https://msdn.microsoft.com/library/mt269962.aspx) temu.
- Dodavanje jednog kodiranja zadatka posla. 
- Navedite unos resursa za kodirati.
- Stvaranje izlaz sredstva koja će sadržavati kodiranih resursa.
- Dodavanje rukovatelja događajima tijek zadatka.
- Slanje zadatka.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
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


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vidi također 

[Upute za generiranje minijaturu pomoću Media Encoder standardne .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Kodiranje pregled Services za medijske sadržaje](media-services-encode-asset.md)
