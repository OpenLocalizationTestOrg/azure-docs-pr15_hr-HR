<properties 
    pageTitle="Kako stvoriti Media procesor | Microsoft Azure" 
    description="Saznajte kako stvoriti komponente media procesor kodiranje, pretvaranje oblika, šifriranje ili dešifriranje medijskih sadržaja za servisa Azure Media Services." 
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

##<a name="get-mediaprocessor"></a>Početak MediaProcessor

>[AZURE.NOTE] Rad s Media Services REST API-JA, primjenjuju se Imajte na umu sljedeće:
>
>Prilikom pristupanja entiteti u Media Services, morate postaviti određene zaglavlje polja i vrijednosti u zahtjevima za HTTP. Dodatne informacije potražite u članku [Postavljanje Media Services REST API razvoj](media-services-rest-how-to-use.md).

>Nakon uspješnog povezivanja s https://media.windows.net, primit ćete 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI kao što je opisano u [povezuje se s Media Services pomoću REST API -JA](media-services-rest-connect-programmatically.md). 


Sljedeće poziv OSTALE pokazuje kako s instancom procesor medijskih sadržaja prema nazivu (u ovom slučaju **Media Encoder standardne**). 



    
Zahtjev:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Odgovor:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Daljnji koraci

Sada kada znate kako započeti s instancom procesor medijskih sadržaja, idite na temu [kako šifrirati sredstvo](media-services-rest-get-started.md) koji će vam pokazati kako pomoću Media Encoder standardne kodiranje sredstvo.
