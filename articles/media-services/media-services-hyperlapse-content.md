<properties
    pageTitle="Hyperlapse medijskih datoteka s Azure Media Hyperlapse | Microsoft Azure"
    description="Azure Media Hyperlapse stvara izglađenim vrijeme lapsed videozapisa iz prve osobe ili akcija kamere sadržaj. U ovoj se temi objašnjava korištenje medijskih sadržaja za indeksiranje."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapse medijske datoteke s Hyperlapse Azure Media

Azure Media Hyperlapse je na Media procesor (MP) koji stvara izglađenim vrijeme lapsed videozapisi iz prve osobe ili akcija kamere sadržaj.  U oblaku iste razine [Hyperlapse Pro web-mjesta Microsoft Research radne površine](http://aka.ms/hyperlapse)i utemeljenu telefonu Hyperlapse Mobile, Microsoft Hyperlapse za servisa Azure Media Services koristi pretraživanje velikog skale platforme Azure Media Services Media obrade vodoravno skaliranje i parallelize skupno Hyperlapse obrade.

>[AZURE.IMPORTANT]Microsoft Hyperlapse je osmišljeno najbolje na prvoj osobi sadržaja za premještanje kamere.  Iako se i dalje možete raditi i dalje kamere materijal, performanse i kvalitetu Azure Media Hyperlapse Media procesor ne može se zajamčiti za druge vrste sadržaja.  Dodatne informacije o programu Microsoft Hyperlapse za servisa Azure Media Services i pogledajte neki videozapisi primjer, potražite u [uvodnom bloga](http://aka.ms/azurehyperlapseblog) iz javne pretpregled.

Programa Azure Hyperlapse medija posao uzima kao unos resursa datoteku MP4, MOV ili WMV uz konfiguracijska datoteka koji određuje koje okvira iz videozapisa mora biti vrijeme lapsed i koje brzinu (npr. prvi 10 000 okvira na 2 x).  Rezultat je stabilized i vrijeme lapsed vizualizacije unosa videozapisa.

Najnovija ažuriranja za Azure Media Hyperlapse potražite u članku [blogovi Media Services](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse imovine

Najprije morate prenijeti željeni Ulazna datoteka servisa Azure Media Services.  Da biste saznali više o konceptima vezanih uz prijenos i upravljanje sadržajem, pročitajte [članak upravljanje sadržajem](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Zadana Postava konfiguracije za Hyperlapse

Kada je sadržaj na vašem računu Media Services, morat ćete sastavljanje na zadana postava konfiguracije.  U sljedećoj su tablici objašnjavaju polja korisnik:

 Polje | Opis
-------|-------------
StartFrame|Okvir na kojem je Microsoft Hyperlapse obrada započeti.
NumFrames|Broj slika za obradu
Brzina|Faktor kojom da biste ubrzali unos videozapis.

Slijedi primjer datoteke za konfiguraciju conformant u XML i JSON:

**Zadana Postava XML:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**Zadana Postava JSON:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse s AMS .NET SDK

Sa sljedećim korakom prenosi medijske datoteke kao sredstvo i stvara zadatak s procesorom medijskih sadržaja za Azure Media Hyperlapse.

> [AZURE.NOTE] Već imat ćete je CloudMediaContext u opsegu s nazivom "kontekst" za kod za rad.  Dodatne informacije o tome pročitajte u [članku Upravljanje sadržajem](media-services-dotnet-get-started.md).

> [AZURE.NOTE] Argument niz "hyperConfig" očekuje conformant konfiguracije unaprijed JSON ili u XML prethodno opisan.

statički booleovom RunHyperlapseJob (unos niza, izlazna niz, hyperConfig niz) {/ / stvaranje resursa s Ulazna datoteka IAsset resursa = kontekst. Resursi. CreateAssetAndUploadSingleFile (unos "Moje unos Hyperlapse", AssetCreationOptions.None);

privucite pojavljivanja Azure Media Hyperlapse MP IMediaProcessor mp = kontekst. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Stvaranje zadatka s Hyperlapse zadatka IJob posao = kontekst. Zadatke. Stvaranje (String.Format ("Hyperlapse {0}", unos));

Ako (String.IsNullOrEmpty(hyperConfig)) {/ / config ne može biti prazna povrata false;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = posao. Tasks.AddNew ("Hyperlapse zadatka", mp, hyperConfig, TaskOptions.None) hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse Izlaz" AssetCreationOptions.None);


zadatak. Submit();

Stvaranje tijeka ispis i postavljanje upita progressPrintTask zadatka zadaci = novi Task(() = > {

IJob jobQuery = null; učinite {var progressContext = kontekst; jobQuery = progressContext.Jobs. Gdje (j = > j.Id == posao. ID-ja). First(); Console.WriteLine (niz. Oblikovanje ("\t \t {1} {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Tijeku)) Thread.Sleep(10000); } dok (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Podržane vrste datoteka

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Srodne veze

[Azure Media Services pregled Analytics](media-services-analytics-overview.md)

[Azure Media analize pokazni programi](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
