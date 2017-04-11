<properties 
    pageTitle="Preuzimanje medijskih sadržaja" 
    description="Dodatne informacije o programu da biste preuzeli imovine s računalom. Primjere koda zapisuju u C# i korištenje Media Services SDK za .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Kako: izlaganje sredstvo preuzeti

U ovoj se temi opisuje mogućnosti izlaganja medijskih sadržaja prenijeli Media Services. Izlažete Media Services sadržaj u brojne aplikacije scenarijima. Možete preuzeti medijskih sadržaja ili im pristupiti pomoću programa locator. Medijski sadržaj možete poslati u neku drugu aplikaciju ili nekog drugog davatelja usluga sadržaja. Bolje performanse i skalabilnost, možete izlaganje sadržaja pomoću sadržaj mreže isporuke (CDN).

U ovom se primjeru pokazuje kako preuzeti medijskih sadržaja s Media Services s vašim lokalnim računalom. Kod upiti poslove povezanu s računom Media Services po ID zadatka i pristupa njegov zbirke **OutputMediaAssets** (što je skup jedan ili više izlaz medijskih sadržaja koja je rezultat pokrenut posao). U ovom se primjeru prikazuju kako preuzeti medijskih sadržaja za izlaz iz posla, ali možete primijeniti na isti način da biste preuzeli drugih sredstava.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Vidi također 

[Izlaganje strujanja medijskih sadržaja](media-services-deliver-streaming-content.md)

