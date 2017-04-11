<properties 
    pageTitle="Stvaranje filtara pomoću servisa Azure Media Services .NET SDK" 
    description="U ovoj se temi opisuje kako stvoriti filtri tako da ih klijent možete koristiti za strujanje određene dijelove strujanje. Media Services stvara dinamički manifesti da biste postigli ovaj selektivno strujanje." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Stvaranje filtara pomoću servisa Azure Media Services .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [OSTALE](media-services-rest-dynamic-manifest.md)

Počevši od 2.11 izdanje, Media Services omogućuje definiranje filtara za imovinu. Ovi filtri su pravila na strani poslužitelja omogućit će klijentima da biste odabrali radnje kao što su: reprodukcija samo dio videozapisa (umjesto reproducira cijeli videozapis), ili samo podskup audio i video vizualizacija koji uređaj klijenta možete rukovati (umjesto sve vizualizacije koje su vezane uz imovine). Postići filtriranja imovine kroz **Dinamički manifesta**s stvorene nakon klijentovim zahtjeva za strujanje videozapisa koji se temelji na navedeni filter(s).

Detaljnije informacije vezane uz filtre i dinamični manifesta potražite u članku [Pregled dinamički manifesti](media-services-dynamic-manifest-overview.md).

U ovoj se temi objašnjava Media Services .NET SDK omogućuje stvaranje, ažuriranje i brisanje filtara. 


Napomena Ako ažurirate filtra može potrajati do dvije minute za strujanje krajnja točka za osvježavanje pravila. Ako sadržaj je poslužena pomoću ovog filtra (i predmemorirani proxy poslužitelji i CDN predmemorije), ažuriranje filtar može uzrokovati pogreške reproduktora. Nije preporučeno da biste očistili predmemoriju nakon ažuriranja filtar. Ako ta mogućnost nije moguće, preporučujemo da koristite drugačiji filtar. 

##<a name="types-used-to-create-filters"></a>Vrste koje se koriste za stvaranje filtara

Sljedeće se koriste prilikom stvaranja filtre: 

- **IStreamingFilter**.  Ta vrsta temelji se na sljedeće REST API-JA [filtra](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Ta vrsta temelji se na sljedeće REST API-JA [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Ta vrsta temelji se na sljedeće REST API-JA [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** i **IFilterTrackPropertyCondition**. Ove vrste temelje se na sljedeće REST API-ji [FilterTrackSelect i FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Stvaranje/ažuriranje/pročitano/brisanje globalni filtara

Sljedeći kod prikazuje kako .NET omogućuje stvaranje, ažuriranje, čitati i brisanje filtara resursa.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Stvaranje/ažuriranje/pročitano/brisanje resursa filtara

Sljedeći kod prikazuje kako .NET omogućuje stvaranje, ažuriranje, čitati i brisanje filtara resursa.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Sastavljanje strujanje URL-ova pomoću filtara

Informacije o tome kako objaviti i izlaganje svoje imovine potražite u članku [Izlaganja sadržaja za pregled klijentima](media-services-deliver-content-overview.md).


Sljedeći primjeri pokazuju kako dodati filtri strujanje URL-ova.

**MPEG CRTICA** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live strujanje V4 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live strujanje V3 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Vidi također 

[Pregled dinamički manifesti](media-services-dynamic-manifest-overview.md)
 

