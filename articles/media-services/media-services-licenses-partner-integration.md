<properties 
    pageTitle="Korištenje partnere isporuka Widevine licenci servisa Azure Media Services | Microsoft Azure" 
    description="U ovom se članku opisuje kako pomoću servisa Azure Media Services (AMS) izlaganje strujanje dinamički šifrirane po AMS s PlayReady i Widevine DRMs. Licenca PlayReady isporučuje se s poslužitelja licence Media Services PlayReady i Widevine licence je isporučen castLabs licence poslužitelj." 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Korištenje partnere isporuka Widevine licenci servisa Azure Media Services

##<a name="overview"></a>Pregled

Microsoft Azure Media Services omogućuje isporuku MPEG-CRTICOM zaštićen DRM Widevine koji je šifriran po specifikacija uobičajenih šifriranja (CENC).

Počevši od Media Services .NET SDK verzija 3.5.2, Media Services omogućuje konfiguriranje predloška Widevine licence te pronađite Widevine licence. Sljedeće AMS partnera možete koristiti i da biste lakše izlaganje Widevine licenci: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

##<a name="castlabs"></a>castLabs

Možete koristiti [castLabs](http://castlabs.com/company/partners/azure/) izlaganje Widevine licence. Dodatne informacije potražite u članku [Korištenje castLabs izlaganje DRM licence servisa Azure Media Services](media-services-castlabs-integration.md)

##<a name="axinom"></a>Axinom

Možete koristiti [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) izlaganje Widevine licence. Dodatne informacije potražite u članku [Korištenje Axinom izlaganje DRM licence servisa Azure Media Services](media-services-axinom-integration.md)


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vidi također

[Šifriranjem PlayReady i/ili Widevine dinamički uobičajenih](media-services-protect-with-drm.md)

[Mingfei na blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

