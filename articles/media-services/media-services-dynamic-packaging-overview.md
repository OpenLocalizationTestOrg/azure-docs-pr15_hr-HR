<properties
    pageTitle="Pregled dinamički pakiranje | Microsoft Azure"
    description="Tema daje i pregled dinamički pakiranju."
    authors="Juliako"
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
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dinamični pakiranje

##<a name="overview"></a>Pregled

Zaštitu sadržaja oblike s raznim tehnologije klijenta i servisa Microsoft Azure Media Services može se koristiti za izlaganje više izvora oblike medijskih datoteka, oblike, strujanja medijskih sadržaja (na primjer, iOS XBOX, Silverlight, Windows 8). Ove klijenti razumijevanje različitih protokola, primjerice iOS zahtijeva obliku V4 HTTP Live strujanje (HLS) i Silverlight i Xbox potreban Smooth Streaming. Ako imate skup prilagodljivo brzina prijenosa (između više – brzina prijenosa) MP4 (ISO osnovni medijske 14496 12) datoteke ili grupe prilagodljivo brzina prijenosa datoteka u Smooth Streaming u koji želite da vam poslužiti da klijenti za razumijevanje MPEG CRTICE, HLS ili Smooth Streaming, trebali biste iskoristite prednost pakiranja za dinamičku Media Services.

Dinamični pakiranje sve što trebate je da biste stvorili sredstva koja sadrži skup prilagodljivo brzina prijenosa MP4 datoteke ili datoteke Smooth Streaming prilagodljivo brzina prijenosa. Nakon toga navedenom obliku u manifest ili fragment zahtjevu za strujanje na zahtjev na temelju poslužitelja će osigurati primaju strujanje u protokol koji ste odabrali. Kao rezultat samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i servis Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta.

Sljedeći dijagram prikazuje tradicionalni kodiranja i statične pakiranje tijeka rada.

![Statički kodiranja](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Sljedeći dijagram prikazuje dinamični pakiranje tijeka rada.

![Dinamični kodiranja](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Da biste iskoristili dinamički pakiranje, najprije se najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite [u](media-services-portal-manage-streaming-endpoints.md)članku skaliranje Media Services.

##<a name="common-scenario"></a>Uobičajeni scenariji

1. Prijenos datoteke unosa (naziva se mezzanine datoteka). Na primjer, H.264, MP4 ili WMV (za popis Podržani oblici potražite u članku [Oblici podržava Media Encoder Standard](media-services-media-encoder-standard-formats.md).

1. Kodiranje datoteku mezzanine skupovima H.264 MP4 prilagodljivo brzina prijenosa.

1. Objavljivanje resursa koji sadrži prilagodljivo brzina prijenosa MP4 postaviti tako da stvorite Locator na zahtjev.

1. Sastavljanje strujanje URL-ovi za pristup i strujanje sadržaj.


##<a name="preparing-assets-for-dynamic-streaming"></a>Priprema resursi za dinamičku strujanje

Da biste pripremili vaše resursa za dinamičku strujanje imate dvije mogućnosti:

1. [Prijenos osnovne datoteke](media-services-dotnet-upload-files.md).
2. [Koristiti Media Encoder standardne encoder čime se dobiva H.264 MP4 prilagodljivo brzina prijenosa skupove](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Strujanje sadržaja](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Oblici koji ne podržava dinamičke pakiranje

Dinamični pakiranje ne podržava sljedeće oblike datoteka izvora.

- Dolby digitalni mp4 datoteke.
- Dolby digitalne izglađenim datoteke.

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
