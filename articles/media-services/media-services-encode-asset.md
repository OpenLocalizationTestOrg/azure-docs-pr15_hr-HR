<properties 
    pageTitle="Pregled i usporedbe Azure na zahtjev media koderi | Microsoft Azure" 
    description="Ova tema sadrži pregled i usporedbe Azure na zahtjev media koderi." 
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
    ms.author="juliako"/>

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Pregled i usporedbe Azure na zahtjev media koderi

##<a name="encoding-overview"></a>Pregled kodiranja

Azure Media Services pruža više mogućnosti za kodiranje medijskih sadržaja u oblak.

Kada počevši Media Services, važno je razlika između oblika kodecima i datoteke.
Kodeci su softver koji implementira algoritama za sažimanje/dekompresiju dok su formati spremnika koji sadrže Komprimirana videozapis.

Media Services nudi dinamički pakiranje koja vam omogućuje da izlaganje prilagodljivo brzina prijenosa MP4 ili Smooth Streaming kodirani sadržaj u strujanje oblici koje podržava Media Services (MPEG CRTICE, HLS, Smooth Streaming, HDS) bez potrebe za ponovno pakiranje u te strujanje oblici.

Da biste iskoristili [dinamički pakiranje](media-services-dynamic-packaging-overview.md), morate učinite sljedeće:

- Kodiranje datoteku mezzanine (izvor) u skupu prilagodljivo brzina prijenosa MP4 datoteke ili Smooth Streaming prilagodljivo brzina prijenosa datoteke (kodiranja korake su što je prikazano u nastavku ovog praktičnog vodiča).
- Pronađite najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite [u](media-services-portal-manage-streaming-endpoints.md)članku skaliranje na zahtjev strujanje rezervirane jedinice.

Media Services podržava sljedeće na zahtjev koderi koji su opisani u ovom članku:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Tijek rada za Media Encoder Premium](media-services-encode-asset.md#media-encoder-premium-workflow)

U ovom se članku daje Kratak pregled na zahtjev koderi medijske sadržaje i sadrži veze na članke koji dati više informacija. U temi omogućuje usporedbu u koderi.

Imajte na umu da po zadanom svaki račun Media Services može imati jednog aktivnog zadatka kodiranja odjednom. Možete rezervirati kodiranja jedinice koje omogućuju vam više kodiranja zadataka koji se izvode istovremeno, jedan za svaku kodiranja rezervirane jedinicu kupite. Informacije potražite u članku [Promjena veličine kodiranje jedinice](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media Encoder Standard

###<a name="how-to-use"></a>Upute za korištenje

[Kako kodiranje s Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Oblici

[Oblici i kodeka](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Unaprijed postavljeno

Media Encoder standardni je konfiguriran pomoću jednog od na kodiranje unaprijed postavljeno što je opisano [u nastavku](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Ulazni i izlazni metapodataka

Unos metapodatke koderi je opisan [u nastavku](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Izlaz metapodataka koderi je opisan [u nastavku](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Generiranje minijature

Informacije potražite u članku [Da biste generirali minijature pomoću standardnih kodiranje medijskih sadržaja](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Obrezivanje videozapisa (isječak)

Informacije potražite [u](media-services-custom-mes-presets-with-dotnet.md#trim_video)članku obrezivanje videozapisa u sustavu Media Encoder Standard.

###<a name="create-overlays"></a>Stvaranje prekrivanja

Informacije potražite u članku [kako stvoriti prekrivanja pomoću standardnih kodiranje medijskih sadržaja](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Vidi također

[Blog Media Services](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Tijek rada za Media Encoder Premium

###<a name="overview"></a>Pregled

[Uvod u Premium kodiranje Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Upute za korištenje

Tijek rada za Media Encoder Premium je konfiguriran pomoću složenih tijekova rada. Datoteka za tijek rada se može stvoriti i ažurirati pomoću alata za [Brzu analizu](media-services-workflow-designer.md) .

[Korištenje Premium kodiranje Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Poznati problemi

Ako unos videozapis ne sadrži skriveni titlovi, izlaz resursa će i dalje sadržavati praznu TTML datoteku. 


##<a id="compare_encoders"></a>Usporedba koderi

###<a id="billing"></a>Metar koristi svaki kodiranje za naplatu

Naziv procesor medija|Primjenjivo cijene|Bilješke
---|---|---
**Media Encoder Standard** |KODIRANJE|Kodiranje zadaci naplatiti skladu s veličinom izlaz resursa u GBytes brzinom naveden [ovdje][1], u stupcu kodiranje.
**Tijek rada za Media Encoder Premium** |KODIRANJE PREMIUM|Kodiranje zadaci naplatiti skladu s veličinom izlaz resursa u GBytes brzinom naveden [ovdje][1], u stupcu PREMIUM ENCODER.


U ovom se odjeljku uspoređuju kodiranja mogućnosti **Media Encoder standardne** i **Media Encoder Premium tijek**rada.


###<a name="input-containerfile-formats"></a>Spremnik/formata za unos

Spremnik/formata za unos|Media Encoder Standard|Tijek rada za Media Encoder Premium
---|---|---
Adobe® Flash® F4V           |Da|Da
MXF/SMPTE 377M              |Da|Da
GXF                         |Da|Da
Strujanja prijenosa MPEG-2    |Da|Da
Strujanja Program MPEG-2      |Da|Da
MPEG-4/MP4                  |Da|Da
Windows Media/ASF           |Da|Da
AVI (koje nisu komprimirane 8-bitni/10 bitova)|Da|Da
3GPP/3GPP2                  |Da|ne
Oblik datoteke Smooth Streaming (PIFF 1.3)|Da|ne
[Microsoft digitalni Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Da|ne
Matroska/WebM               |Da|ne
QuickTime (.mov) |Da|ne

###<a name="input-video-codecs"></a>Unos Videokodeci

Unos Videokodeci|Media Encoder Standard|Tijek rada za Media Encoder Premium
---|---|---
AVC 8-bitnu/10-bitni, do 4:2:2, uključujući AVCIntra   |8-bitno 4:2:0 i 4:2:2|Da
Avid DNxHD (u MXF)                                 |Da|Da
DVCPro/DVCProHD (u MXF)                            |Da|Da
JPEG2000                                            |Da|Da
MPEG-2 (do 422 profila te visoku razinu; uključujući varijante kao što su XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)|Do 422 profila|Da
MPEG-1                                              |Da|Da
Windows Media Video/VC-1                            |Da|Da
Canopus HQ HQX                                      |ne|ne
MPEG-4 dijela 2                                       |Da|ne
[Theora](https://en.wikipedia.org/wiki/Theora)      |Da|ne
Apple ProRes 422    |Da|ne
Apple ProRes 422 LT |Da|ne
Apple ProRes 422 HQ |Da|ne
Apple ProRes Proxy|Da|ne
Apple ProRes 4444 |Da|ne
Apple ProRes 4444 XQ |Da|ne

###<a name="input-audio-codecs"></a>Unos audiokodeke

Unos audiokodeke|Media Encoder Standard|Tijek rada za Media Encoder Premium
---|---|---
AES (SMPTE 331 M i 302 M, AES3 2003)        |ne|Da
Dolby® E                                    |ne|Da
Digitalni Dolby® (AC3)                        |ne|Da
Digitalni Dolby® Plus (E-AC3)                 |ne|Da
AAC (AAC-LC, HE za AAC i AAC-HEv2; do 5.1)|Da|Da
MPEG sloja 2|Da|Da
MP3 (MPEG 1 Audio Layer 3)|Da|Da
Windows Media Audio|Da|Da
WAV/UO|Da|Da
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Da|ne
[Opusu](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Da|ne
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Da|ne


###<a name="output-containerfile-formats"></a>Izlazne oblike spremnik/datoteke

Izlazne oblike spremnik/datoteke|Media Encoder Standard|Tijek rada za Media Encoder Premium
---|---|---
Adobe® Flash® F4V|ne|Da
MXF (OP1a, XDCAM i AS02)|ne|Da
DPP (uključujući AS11)|ne|Da
GXF|ne|Da
MPEG-4/MP4|Da|Da
MPEG-TS|Da|Da
Windows Media/ASF|ne|Da
AVI (koje nisu komprimirane 8-bitni/10 bitova)|ne|Da
Oblik datoteke Smooth Streaming (PIFF 1.3)|ne|Da

###<a name="output-video-codecs"></a>Izlaz Videokodeci

Izlaz Videokodeci|Media Encoder Standard|Tijek rada za Media Encoder Premium
---|---|---
AVC (H.264; 8-bitnu; najviše profila visoke razine 5.2; 4 K Ultra HD; AVC Intra)|Samo 8-bitno 4:2:0|Da
Avid DNxHD (u MXF)|ne|Da
MPEG-2 (do 422 profila te visoku razinu; uključujući varijante kao što su XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)|ne|Da
MPEG-1|ne|Da
Windows Media Video/VC-1|ne|Da
Stvaranja minijatura JPEG|ne|Da

###<a name="output-audio-codecs"></a>Izlaz audiokodeke

Izlaz audiokodeke|Media Encoder Standard|Tijek rada za Media Encoder Premium
---|---|---
AES (SMPTE 331 M i 302 M, AES3 2003)|ne|Da
Digitalni Dolby® (AC3)|ne|Da
Dolby® Digital Plus (E-AC3) do 7.1|ne|Da
AAC (AAC-LC, HE za AAC i AAC-HEv2; do 5.1)|Da|Da
MPEG sloja 2|ne|Da
MP3 (MPEG 1 Audio Layer 3)|ne|Da
Windows Media Audio|ne|Da


##<a name="error-codes"></a>Kodovi pogrešaka  

U sljedećoj su tablici navedeni šifre pogrešaka koje nije moguće vratiti u slučaju došlo je do pogreške tijekom kodiranja izvršavanja zadataka.  Da biste vidjeli detalje o pogrešci u kodu .NET, koristite [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) predmete. Da biste vidjeli detalje o pogrešci u kodu OSTALE, koristite [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API-JA.

ErrorDetail.Code|Mogući uzroci pogrešaka
-----|-----------------------
Nepoznato| Nepoznata pogreška pri izvođenju zadatak
ErrorDownloadingInputAssetMalformedContent|Kategorija pogrešaka koji prekriva pogrešaka u preuzimanje unos resursa kao što je neispravna nazivima, nulte duljine datoteke, koja je netočan oblikovanja i tako dalje.
ErrorDownloadingInputAssetServiceFailure|Kategorija pogreške koji prekriva probleme na stranu servisa – na primjer mreže ili prostora za pohranu pogrešaka prilikom preuzimanja.
ErrorParsingConfiguration|Kategorija pogrešaka gdje zadatka <see cref="MediaTask.PrivateData"/> (Konfiguracija) nije valjana, primjerice konfiguraciju nije valjani sustava unaprijed ili sadrži XML koji nije valjan.
ErrorExecutingTaskMalformedContent|Kategorija pogreške tijekom izvršavanja zadataka gdje problemi unutar unosa medijskih datoteka uzrokuju pogreške.
ErrorExecutingTaskUnsupportedFormat|Kategorija pogrešaka gdje media procesor ne može obraditi datoteke koje su-media oblik zapisa nije podržana ili odgovara konfiguraciji. Na primjer, pokušaja proizvesti samo zvuk Izlaz iz sredstva koja sadrži samo videozapis
ErrorProcessingTask|Kategorija drugih pogrešaka koje naiđe procesor medijskog sadržaja tijekom obrade zadatak koji nisu povezani sa sadržajem.
ErrorUploadingOutputAsset|Kategorija pogreške prilikom prijenos resursa za izlaz
ErrorCancelingTask|Kategorija pogrešaka da prekrije pogreške pri pokušaju da biste poništili zadatak
TransientError|Kategorija pogrešaka da prekrije tranzitne problema (npr. privremeni mrežnih problema s Azure prostora za pohranu)


Da biste dobili pomoć s tim **Media Services** , otvorite je [podržava karata](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Povezani članci

- [Izvođenje napredne kodiranja zadataka prilagodbom Media Encoder standardne unaprijed postavljeno](media-services-custom-mes-presets-with-dotnet.md)
- [Kvota i ograničenja](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
