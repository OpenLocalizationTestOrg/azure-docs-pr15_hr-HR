<properties 
    pageTitle="Reprodukcija sadržaj | Microsoft Azure" 
    description="Ova tema sadrži popis postojeće reproduktora koje možete koristiti za reprodukciju sadržaj." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Reprodukcija sadržaja pomoću postojeće igrača

Azure Media Services podržava mnoge popularne strujanje oblike, kao što su Smooth Streaming, HTTP Live strujanje i MPEG-crtica. Ova tema ukazuje postojeće reproduktora koje možete koristiti da biste testirali strujanje.

>[AZURE.NOTE]Da biste reprodukciju dinamički pakirat ili dinamički šifrirane sadržaja, provjerite jeste li dobili najmanje jednu strujanje jedinicu za strujanje krajnjoj točki iz koje namjeravate izlaganje sadržaja. Informacije o skaliranje strujanje jedinice potražite u članku: [kako skaliranje strujanje jedinice](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure portala Media Services sadržaja reproduktora

Portal za **Azure** predstavlja sadržaja reproduktoru koji možete koristiti za testiranje videozapis.

Kliknite željeni videozapis (Provjerite je li je [objavljen](media-services-portal-publish.md)) i kliknite gumb **Reproduciraj** pri dnu portalu.

Neke informacije vrijediti:

- **Usluge sadržaja REPRODUKTOR** reproducira zadanu strujanje krajnjoj točki. Ako želite reproducirati na krajnjoj točki strujanje koji nisu zadani, koristite neki drugi player. Na primjer, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure Media Player

[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) omogućuje reprodukciju sadržaja (Očisti ili zaštićeni) u nekom od sljedećih oblika:

- Smooth Streaming
- MPEG CRTICA
- HLS
- MP4 progresivno


###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>AES šifrirane s tokena

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight reproduktori

####<a name="monitoring"></a>Nadzor

[http://smf.cloudapp.NET/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady s tokena

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>Čitači crtica

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Drugi

Da biste testirali HLS URL-ova možete koristiti:

- **Safari** na uređaju sa sustavom iOS ili
- **reproduktor HLS 3ivx** u sustavu Windows.

##<a name="developing-video-players"></a>Razvoj telefone

Informacije o razvoju vlastite reproduktora potražite u članku [razvojnom programiranju telefone](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
