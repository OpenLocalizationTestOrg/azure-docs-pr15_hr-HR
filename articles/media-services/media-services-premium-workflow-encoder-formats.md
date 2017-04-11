<properties 
    pageTitle="Tijek rada za Media Encoder Premium oblici i kodecima | Microsoft Azure" 
    description="Ova tema sadrži pregled Media Encoder Premium tijeka rada oblici oblici i kodeka" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Tijek rada za Media Encoder Premium oblici i kodeka


>[AZURE.NOTE]Premium encoder pitanja, e-pošte mepd na Microsoft.com.
>
>Tijek rada za Media Encoder Premium media procesor spominju u ovoj temi nije dostupna u Kini. 

Ovaj dokument sadrži popis ulazni i izlazni oblici datoteka i kodecima koje podržava javno pretpregled verziju **Tijeka rada za Media Encoder Premium** kodiranje.

[Media Encoder Premium Worflow unos oblici i kodeka](#input_formats)

[Media Encoder Premium Worflow izlaznih oblika i kodeka](#output_formats)

**Tijek rada za Media Encoder Premium** podržava titlove navedena u [ovom](#closed_captioning) odjeljku. 


##<a id="input_formats"></a>Tijek rada za Media Encoder Premium unos oblika i kodecima

U sljedećoj sekciji popis kodecima i datoteke oblika ovog procesora medijskih sadržaja podržava kao ulaz.

###<a name="input-containerfile-formats"></a>Spremnik/formata za unos

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- Strujanja prijenosa MPEG-2
- Strujanja Program MPEG-2
- MPEG-4/MP4
- Windows Media/ASF
- AVI (koje nisu komprimirane 8-bitni/10 bitova)

###<a name="input-video-codecs"></a>Unos Videokodeci

- AVC 8-bitnu/10-bitni, do 4:2:2, uključujući AVCIntra
- Avid DNxHD (u MXF)
- DVCPro/DVCProHD (u MXF)
- JPEG2000
- MPEG-2 (do 422 profila te visoku razinu; uključujući varijante kao što su XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Unos audiokodeke

- AES (SMPTE 331 M i 302 M, AES3 2003)
- Dolby® E
- Digitalni Dolby® (AC3)
- AAC (AAC-LC, HE za AAC i AAC-HEv2; do 5.1)
- MPEG sloja 2
- MP3 (MPEG 1 Audio Layer 3)
- Windows Media Audio
- WAV/UO
 
##<a id="output_format"></a>Media Encoder Premium tijeka rada izlaznih oblika i kodeka

Sljedeći odjeljak navode oblici kodecima i datoteke koje su podržane kao izlaz iz ovog procesora medijske sadržaje.

###<a name="output-containerfile-formats"></a>Izlazne oblike spremnik/datoteke

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM i AS02)
- DPP (uključujući AS11)
- GXF
- MPEG-4/MP4
- Windows Media/ASF
- AVI (koje nisu komprimirane 8-bitni/10 bitova)
- Oblik datoteke Smooth Streaming (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Izlaz Videokodeci

- AVC (H.264; 8-bitnu; najviše profila visoke razine 5.2; 4 K Ultra HD; AVC Intra)
- Avid DNxHD (u MXF)
- DVCPro/DVCProHD (u MXF)
- MPEG-2 (do 422 profila te visoku razinu; uključujući varijante kao što su XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)
- MPEG-1
- Windows Media Video/VC-1
- Stvaranja minijatura JPEG

###<a name="output-audio-codecs"></a>Izlaz audiokodeke

- AES (SMPTE 331 M i 302 M, AES3 2003)
- Digitalni Dolby® (AC3)
- Dolby® Digital Plus (E-AC3) do 7.1
- AAC (AAC-LC, HE za AAC i AAC-HEv2; do 5.1)
- MPEG sloja 2
- MP3 (MPEG 1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Podrška za titlove

Na ingest, **Tijek rada za Media Encoder Premium** podržava:

1. SCC datoteke
1. SMPTE TT datoteke
1. CEA-608/CEA-708 – prenose se kao korisničkih podataka (SEI poruke H.264 osnovne strujanja, ATSC/53, SCTE20) ili prenose se kao dodatnih podataka u datotekama MXF/GXF
1. STL podnaslov datoteke

Na Izlaz dostupne su sljedeće mogućnosti:

1. CEA 608 CEA 708 prijevoda
1. CEA-608/CEA-708 proći kroz (ugrađen u SEI poruke od H.264 osnovne strujanja ili prenose se kao dodatnih podataka u datotekama MXF)
1. SCC
1. SMPTE Timed tekst (iz izvora CEA 608 po SMPTE RP2052, uključujući stvaranje DFXP datoteke)
1. Podnaslov REDTI datoteka
1. DVB podnaslov strujanja

Napomena: sve iznad izlaznom obliku podržani su za isporuku putem strujanja u servisa Azure Media Services.

##<a name="known-issues"></a>Poznati problemi

Ako unos videozapis ne sadrži skriveni titlovi, izlaz resursa će i dalje sadržavati praznu TTML datoteku. 


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
