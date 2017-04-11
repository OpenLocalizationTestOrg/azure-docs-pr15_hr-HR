<properties 
    pageTitle="Media Encoder standardne oblike i kodeka" 
    description="Ova tema sadrži pregled Media Encoder standardne oblike i kodecima." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder standardne oblike i kodeka


Ovaj dokument sadrži popis Najčešći uvoz i izvoz oblici datoteka koje možete koristiti s Media Encoder Standard.


##<a name="input-containerfile-formats"></a>Spremnik/formata za unos

Oblici datoteka (datotečnih nastavaka)|Podržana
---|---|---|---
FLV (s H.264 i AAC kodeci) (.flv)          |Da 
MXF (.mxf)                  |Da 
GXF (.gxf)                  |Da 
MPEG2-PS, MPEG2 TS 3GP (.ts, .ps, .3gp, .3gpp, .mpg)   |Da 
Windows Media Video (WMV) / ASF (.wmv, .asf) |Da 
AVI (koje nisu komprimirane 8-bitni/10 bitova) (.avi)|Da 
MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)|Da 
[Microsoft digitalni Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Da 
Matroska/WebM (.mkv)        |Da 
VAL/WAV (.wav) |Da 
QuickTime (.mov) |Da

>[AZURE.NOTE] Noviji je popis obično je naišao na datotečni nastavak. Media Encoder standardne podržava mnoge druge (na primjer: .m2ts, .mpeg2video, .qt). Ako pokušate kodiranje datoteke i dobijete poruku o pogrešci o obliku koji se ne podržava, povratne informacije u [nastavku](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Oblici audiozapisa u spremnicima za unos 

Media Encoder standardne podržava koji ima sljedeći oblici audiozapisa u spremnicima za unos:

- MXF, GXF i QuickTime datoteke koje ste audiozapisa s interleaved stereozvuk ili 5.1 uzorka

ili

- Datoteke MXF, GXF i QuickTime kojima zvuk prenose se kao zasebna uo zapise, ali kanala mapiranje (stereozvuk ili 5.1) koje se mogu deduced iz datoteke metapodataka

Napomena koji podržavaju za mapiranje eksplicitnih/korisnik-navedeni kanala objavit ćemo zajedno u skorijoj budućnosti.


##<a name="input-video-codecs"></a>Unos Videokodeci

Unos Videokodeci|Podržana
---|---|---|---
AVC 8-bitnu/10-bitni, do 4:2:2, uključujući AVCIntra   |8-bitno 4:2:0 i 4:2:2 
Avid DNxHD (u MXF)                                 |Da 
DVCPro/DVCProHD (u MXF)                            |Da 
Digitalni video (DV) (u AVI datoteke)                   |Da
JPEG 2000                                           |Da 
MPEG-2 (do 422 profila te visoku razinu; uključujući varijante kao što su XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10)|Do 422 profila 
MPEG-1                                              |Da 
VC-1/WMV9                                           |Da 
Canopus HQ HQX                                      |ne 
MPEG-4 dijela 2                                       |Da 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Da 
YUV420 dekomprimirati ili mezzanine                   |Da
Apple ProRes 422                                    |Da
Apple ProRes 422 LT |Da
Apple ProRes 422 HQ |Da
Apple ProRes Proxy|Da
Apple ProRes 4444 |Da
Apple ProRes 4444 XQ |Da



##<a name="input-audio-codecs"></a>Unos audiokodeke

Unos audiokodeke|Podržana
---|---|---|---
AAC (AAC-LC, HE za AAC i AAC-HEv2; do 5.1)|Da 
MPEG sloja 2|Da 
MP3 (MPEG 1 Audio Layer 3)|Da 
Windows Media Audio|Da 
WAV/UO|Da 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Da 
[Opusu](http://go.microsoft.com/fwlink/?LinkId=822667) |Da 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Da 
AMR (stopa prilagodljivo više)|Da
AES (SMPTE 331 M i 302 M, AES3 2003)        |ne 
Dolby® E                                    |ne 
Digitalni Dolby® (AC3)                        |ne 
Digitalni Dolby® Plus (E-AC3)                 |ne 


##<a name="output-formats-and-codecs"></a>Izlaznih oblika i kodeka

U sljedećoj su tablici navedeni oblici kodecima i datoteke koje su podržane za izvoz.


Oblik datoteke|Video kodek|Audio kodek
---|---|---
MP4 <br/><br/>(uključujući višestruki-brzina prijenosa MP4 spremnika) |H.264 (najviša, glavni i osnovne profili)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2 TS |H.264 (najviša, glavni i osnovne profili)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vidi također

[Kodiranje osvježavati sadržaja pomoću servisa Azure Media Services](media-services-encode-asset.md)

[Kako kodiranje s Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)
