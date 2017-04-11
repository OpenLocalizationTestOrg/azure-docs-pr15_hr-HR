<properties 
    pageTitle="Kako stvoriti Media procesor | Microsoft Azure" 
    description="Saznajte kako stvoriti komponente media procesor kodiranje, pretvaranje oblika, šifriranje ili dešifriranje medijskih sadržaja za servisa Azure Media Services. Primjere koda zapisuju u C# i korištenje Media Services SDK za .NET." 
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


#<a name="how-to-get-a-media-processor-instance"></a>Kako: se s instancom procesor medijske sadržaje

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [OSTALE](media-services-rest-get-media-processor.md)


##<a name="overview"></a>Pregled

U Media Services media procesor je komponenta korisničkog sučelja koji se rukuje određenim obrada zadatka, kao što su kodiranja, oblikovati pretvorbe šifriranja ili dešifriranja medijskog sadržaja. Koje obično stvarate procesor medijskih sadržaja kada stvarate zadatak koji želite šifrirati, šifriranje ili pretvaranje oblika medijskih sadržaja.

Sljedeća tablica sadrži naziv i opis svakog procesora dostupne medijske sadržaje.

Naziv procesor medija|Opis|Dodatne informacije
---|---|---
Media Encoder Standard|Pruža standardne mogućnosti za šifriranje na zahtjev. |[Pregled i usporedbe Azure na zahtjev Media koderi](media-services-encode-asset.md)
Tijek rada za Media Encoder Premium|Omogućuje pokretanje kodiranja zadataka pomoću tijeka rada za Media Encoder Premium.|[Pregled i usporedbe Azure na zahtjev Media koderi](media-services-encode-asset.md)
Azure Media indeksiranje| Omogućuje vam da bi medijske datoteke i sadržaj koji se može pretraživati, kao i generiranje zatvorenih skrivene titlove zapise i ključne riječi.|[Azure Media indeksiranje](media-services-index-content.md)
Azure Media Hyperlapse (pretpregled)|Omogućuje glačanje out "udaraca" u videozapisu s video stabilization. Omogućuje da biste ubrzali sadržaj u potrošni isječka.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Amortizirana
Prostor za pohranu dešifriranje| Amortizirana|
Program za pakiranje Azure Media|Amortizirana|
Azure Media šifriranje|Amortizirana|

##<a name="get-media-processor"></a>Početak procesor medijske sadržaje

Sa sljedećim korakom prikazuje kako započeti s instancom procesor medijske sadržaje. Primjer koda pretpostavlja da koristite varijablu modul razinom pod nazivom **_context** referentni kontekst poslužitelja, kao što je opisano u odjeljku [Kako: povezivanje s Media Services Programski](media-services-dotnet-connect-programmatically.md).

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

##<a name="next-steps"></a>Daljnji koraci

Sada kada znate kako započeti s instancom procesor medijskih sadržaja, idite na temu [kako šifrirati sredstvo](media-services-dotnet-encode-with-media-encoder-standard.md) koji će vam pokazati kako pomoću Media Encoder standardne kodiranje sredstvo.


