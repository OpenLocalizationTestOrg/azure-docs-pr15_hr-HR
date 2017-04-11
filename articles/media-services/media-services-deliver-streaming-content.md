<properties 
    pageTitle="Objavljivanje web-servisa Azure Media Services sadržaja pomoću .NET" 
    description="Saznajte kako stvoriti locator koja se koristi za stvaranje strujanje URL-a. Primjere koda zapisuju u C# i korištenje Media Services SDK za .NET." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-net"></a>Objavljivanje web-servisa Azure Media Services sadržaja pomoću .NET
 
> [AZURE.SELECTOR]
- [OSTALE](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Portal](media-services-portal-publish.md)

##<a name="overview"></a>Pregled

Strujanja prilagodljivo brzina prijenosa MP4 postavljanje stvaranjem locator strujanje na zahtjev i stvaranje strujanje URL-a. Tema [kodiranje sredstvo](media-services-encode-asset.md) prikazuje kako kodiranje u prilagodljivo brzina prijenosa MP4 postavljanje. 

>[AZURE.NOTE]Ako je šifrirana sadržaj, konfigurirati pravila za isporuku resursa (kao što je opisano u [nastavku](media-services-dotnet-configure-asset-delivery-policy.md) ) prije stvaranja na locator. 

Na zahtjev strujanje locator možete koristiti i da biste sastavili URL-ovi koji vode do MP4 datoteke koju je moguće progresivno preuzeti.  

U ovoj se temi objašnjava da biste stvorili na zahtjev strujanje locator da biste mogli objaviti svoje resursa i sastavljanje bila glatka, MPEG CRTICE i HLS strujanje URL-ova. Također prikazuje tipkovni da biste sastavili progresivno preuzimanje URL-ova. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Stvaranje programa zahtjev strujanje locator

Da biste stvorili zahtjev strujanje locator i dohvaćanje URL-ovi morate učiniti sljedeće:

   1. Ako je šifrirana sadržaj, definirati pravilo programa access.
   2. Stvorite na zahtjev strujanje locator.
   3. Ako namjeravate strujanje dobiti strujanje manifesta datoteku (.ism) u sredstava. 
        
    Ako namjeravate progresivno preuzimanje se imena MP4 datoteke u sredstava.  
   4. Sastavljanje URL-ovi za manifesta datoteku ili MP4 datoteke. 
   

###<a name="use-media-services-net-sdk"></a>Korištenje Media Services .NET SDK 

Sastavljanje strujanje URL-ova 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Izlaze kod:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Sadržaj možete strujanje i putem SSL veze. Da biste to učinili, provjerite je li strujanje URL-ovi počinju HTTPS. 

Sastavljanje progresivno preuzimanje URL-ova 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Izlaze kod:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Korištenje Media Services .NET SDK proširenja

Sljedeći kod poziva .NET SDK proširenja metode koje stvorite u locator i generiranje na Smooth Streaming, HLS i MPEG-CRTICOM URL-ovi za prilagodljivu streaming.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vidi također

[Preuzimanje imovine](media-services-deliver-asset-download.md)
[Konfiguriraj resursa isporuke pravila](media-services-dotnet-configure-asset-delivery-policy.md)
