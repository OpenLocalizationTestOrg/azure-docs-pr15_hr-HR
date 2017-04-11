<properties 
    pageTitle="Stvaranje napredne kodiranja tijekovi rada s programom dizajner tijeka rada | Microsoft Azure" 
    description="Naučite kako stvoriti dodatne kodiranja tijekovi rada s programom dizajner tijeka rada." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Stvaranje napredne kodiranja tijekovi rada s programom dizajner tijeka rada

##<a name="overview"></a>Pregled

U **Dizajneru tijeka rada** je alat za radnu površinu sustava Windows koja se koristi za dizajniranje i stvaranje prilagođene tijekove rada za šifriranje s **Tijekom rada sustava Media Encoder Premium**.
Pomoću power dizajner alata za tijek rada možete dizajnirati i stvaranje složenih tijekova rada koji će se pokrenuti u **Media Encoder Premium**.  

Tijekovi rada mogu sadržavati klijenta odluka logike te grananja prema svojstvima unos izvornu datoteku. Možete stvoriti tijekove rada s može odbaciti svojstva i dinamičke vrijednosti da biste se lakše ponovite i Prilagodba u oblaku čak i najčešće složene kodiranja zadatke.

Primjer tijekovi rada koje možete stvoriti obuhvaćaju sljedeće:

- Odluka temelji tijekova rada koji Provjera izvora sadržaja za rješavanje i kodiranje samo zapise željeni izlazni rezultat.  Ovo je helfpul uklanjanjem utrošeno zapise koje želite generira upscaling primijenite sadržaja izvora.
- Više datoteka za unos se može koristiti za podršku opise, slojeva i Šivanje zajedno sadržaj. 

Ovaj alat mogu također se mijenjati sve naše [objavljene tijekova rada](media-services-workflow-designer.md#existing_workflows). 

>[AZURE.NOTE]Da biste svoj primjerak sustava alat za brzu analizu, obratite se mepd@microsoft.com.


Nakon stvaranja datoteke tijeka rada je moguće prenijeti kao sredstvo, a zatim može koristiti za kodiranje medijskih datoteka. Informacije o kodiranje **Premium tijeka rada za Media Encoder** pomoću **.NET**, potražite u članku [Napredno kodiranje Premium Media Encoder tijekom rada](media-services-encode-with-premium-workflow.md).

##<a id="existing_workflows"></a>Izmjena postojećeg tijekova rada

Zadane [objavljivanja tijekovi rada](media-services-workflow-designer.md#existing_workflows) mogu mijenjati pomoću alata za dizajnera. Možete dobiti zadana tijeka rada datoteke [ovdje](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Mapa sadrži i opis te datoteke.

Sljedeći videozapisi Demonstracija pomoću dizajnera.

###<a name="day-1--getting-started"></a>Dan 1 – prvi koraci

Videozapis 1 dan obuhvaća:

- Dizajner pregled
- Osnovni tijekova rada – "Zdravo svijete"
- Stvaranje višestrukih izlaz MP4 datoteke za korištenje s strujanje servisa Azure Media Services

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>2 dana

Videozapis 2 dana obuhvaća:

- Promjenjivih izvorne datoteke scenariji – zadužen za zvuk
- Tijekovi rada s napredne logike
- Faza grafikonu

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>3 dana

Videozapis 3 dana obuhvaća:

- Skriptiranje unutar tijekovi rada/Blueprints
- Ograničenja s trenutnom Encoder
- Značajka pitanja i odgovora
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Ako trebate podršku ili imate pitanja o stvaranju prilagođene tijekove rada u alatu za dizajner tijeka rada, pošaljite e-poštu na mepd@microsoft.com.

##<a name="see-also"></a>Vidi također

[Azure Premium Encoder tijeka rada dizajnera Tečajevi](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
