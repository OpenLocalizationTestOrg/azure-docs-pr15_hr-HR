<properties 
    pageTitle="Najčešća pitanja o | Microsoft Azure" 
    description="Najčešća pitanja" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>


#<a name="frequently-asked-questions"></a>Najčešća pitanja

##<a name="general-ams-faqs"></a>Najčešća pitanja AMS Općenito

P: kako ste skaliranje indeksiranje?

A: rezervirane su jedinice na isti način za kodiranje i indeksiranjem zadatke. Slijedite upute o [tome kako skaliranje kodiranje rezervirane jedinice](media-services-scale-media-processing-overview.md). **Napomena** da komponente za indeksiranje ne utječu na performanse rezervirane jedinica vrsta.

P: prenijeti, kodiranja i objavili videozapis. Što bi razloga videozapis reproducirati kada je strujanjem?

A: od najčešćih razloga je nemate najmanje jednu rezervirane strujanje jedinicu dodijeliti na strujanje krajnjoj točki iz kojeg želite reprodukcije.  Slijedite upute o [tome kako skaliranje strujanje rezervirane jedinice](media-services-portal-scale-streaming-endpoints.md).

P: učiniti uslojavanje na aktivno strujanje?

A: uslojavanje na uživo strujanja je trenutno ne nudi u servisa Azure Media Services da bi se trebali biste unaprijed sastavite na vašem računalu.

P: mogu koristiti Azure CDN s Live strujanje?

A: Media Services podržava Integracija s Azure CDN (Dodatne informacije potražite [u](media-services-portal-manage-streaming-endpoints.md)članku Upravljanje strujanje točke u računa za servise medijskih sadržaja).  Možete koristiti Live strujanje CDN. Azure Media Services nudi izglađenim izlaze strujeće, HLS i MPEG-crtica. Ove oblike koristiti HTTP-a za prijenos podataka i iskoristiti prednosti HTTP predmemoriranje. U stvarnom strujanje stvarnih podataka video i audiozapisa dijeli se fragmentirane i ovaj pojedinačne fragmentirane dobiti predmemorirani u CDN. Samo podataka mora biti osvježene su manifesta podaci. CDN povremeno osvježava manifesta podatke.

P: ne Azure Media services podržavaju pohranu slike?

A: Ako samo tražite za pohranu slika JPEG ili PNG, zadržati onima u spremište blobova platforme Azure. Nema smisla da biste ih stavljate na vašem računu Media Services, osim ako ne želite da ih vezane uz videozapis ili zvuka resursi. Ili ako je možda potrebno da biste koristili sliku kao slojeva u video kodiranje. Media Encoder standardne podržava preklapanje slike pri vrhu videozapisa, a to je što je popis JPEG i PNG kao što je podržano oblici za unos. Dodatne informacije potražite u članku [Stvaranje slojeva](media-services-custom-mes-presets-with-dotnet.md#overlay).

P: Kako mogu li kopirati imovine s jednog računa Media Services u drugu.

A: da biste kopirali imovine s jednog računa Media Services u drugu pomoću .NET, postupkom [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) proširenje dostupne u spremištu [Azure Media Services .NET SDK proširenja](https://github.com/Azure/azure-sdk-for-media-services-extensions/) . Dodatne informacije potražite u članku [ovaj](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) niti forum.

P: koje su podržane znakova za davanje naziva datoteke prilikom rada s AMS?

A: Media Services koristi vrijednost svojstva IAssetFile.Name kada sastavljanje URL-ovi za strujanje sadržaj (na primjer, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Zbog toga šifriranje u obliku postotaka nije dopušteno. Svojstvo **ime** ne smije sadržavati sljedeće [znakove postotak kodiranje-rezervirana](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Osim toga, može postojati samo jedan '. " za nastavak.


P: kako se povezati pomoću REST?

A: Nakon uspješnog povezivanja s https://media.windows.net, primit ćete 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI kao što je opisano u [povezuje se s Media Services pomoću REST API -JA](media-services-rest-connect-programmatically.md). 


P: kako rotirati videozapisa tijekom procesa kodiranja.

A: [Media Encoder standardne](media-services-dotnet-encode-with-media-encoder-standard.md) podržava zakretanje po kutove od 90/180/270. Zadano ponašanje je "Automatski", gdje pokuša otkriti zakretanje metapodataka u dolazne MP4/MOV datoteke i vaše za njega. Uključi sljedeći element **izvora** u jednu od na json unaprijed postavljeno definirani [ovdje](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
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




##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
