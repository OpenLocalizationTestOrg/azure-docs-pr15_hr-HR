<properties
    pageTitle="Upute za obrezivanje videozapisa | Microsoft Azure"
    description="U ovom se članku objašnjava obrezivanje videozapisa s Media Encoder Standard."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Obrezivanje videozapisa Media Encoder Standard

Media Encoder standardne (TEMA) možete koristiti da biste obrezali unos videozapisa. Obrezivanje je postupak odaberete pravokutni prozor unutar okvira videozapisa, a kodiranje samo piksela u tom prozoru. Na sljedećem su dijagramu olakšava ilustracija postupka.

![Obrezivanje videozapisa](./media/media-services-crop-video/media-services-crop-video01.png)

Pretpostavimo da imate kao ulaz videozapisa koji ima razlučivost 1920 x 1080 piksela (16:9 proporcije), ali nema crni trake (pillar okviri) na lijevo i desno, tako da samo prozor 4:3 ili 1440 x 1080 piksela sadrži aktivni videozapis. Korištenje Okvirima za obrezivanje ili uređivanje izgleda trake crno i kodiranje regija 1440 x 1080.

Obrezivanje u Okvirima je unaprijed obradi fazu, pa obrezivanje parametara u kodiranja zadana postava primjenjuju na izvornom unos videozapis. Kodiranje je kasnije fazu, a širina i visina postavke primjenjuju se na na *unaprijed obrađuju* videozapisa, a ne izvornog videozapisa. Prilikom dizajniranja na zadana postava trebali biste učiniti sljedeće: (a) odaberite izvornog unos videozapisa na temelju parametara Obreži i (b) odaberite vaše kodiranje postavke na temelju obrezane videozapis. Ako vam ne odgovaraju na kodiranje postavke obrezane videozapis, rezultat će biti kao što ste očekivali.

[Sljedeće](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) teme prikazuje kako ga stvoriti kodiranja zadatak s Okvirima i da biste odredili prilagođeni unaprijed postavljeni za kodiranja zadatak. 

## <a name="creating-a-custom-preset"></a>Stvaranje prilagođene zadana Postava

U primjeru u dijagramu:

1. 1920 x 1080 je izvorna unos
1. Treba se obrezati u programa izlaz 1440 x 1080 koji se nalazi u okvir za unos sredini
1. To znači da je X Pomak (1920 – 1440) / 2 = 240, a s Y pomak od nule
1. Širinu i visinu pravokutnika za obrezivanje su 1440 i 1080, odnosno
1. U fazi Kodiraj na ask je čime se dobiva tri slojeve, odnosno su rješenja 1440 x 1080 960 x 720 i 480 x 360

###<a name="json-preset"></a>Zadana Postava JSON


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Ograničenja za obrezivanje

Obrezivanje značajka je namijenjeni ručno. Morate učitati ulazni videozapis u odgovarajuću alat za uređivanje koji omogućuje odabir okvira koji vas zanimaju, postavite pokazivač da biste odredili pomake za obrezivanje pravokutnika, da biste odredili kodiranja zadana postava koje je postavljeno za taj određeni videozapis, itd. Značajka je namijenjena da biste omogućili elemente kao što su: automatsko otkrivanje i uklanjanje crna letterbox/pillarbox obruba u unos videozapisa.

Sljedeća ograničenja primjenjuju se na značajku obrezivanje. Ako to nije zadovoljen, Kodiraj zadatak možete neće uspjeti ili proizvesti neočekivani rezultati.

1. Suautorstvo ordinates i veličinu pravokutnika za obrezivanje morati prilagođava unos videozapis
1. Kao što je rečeno iznad širine i visine u postavkama Kodiraj morati odgovaraju obrezane videozapis
1. Obrezivanje odnosi se na videozapise u vodoravnom načinu (odnosno nije primjenjivo videozapise zapisane s Pametni telefon održava okomito ili u okomitom usmjerenju)
1. Najbolje funkcionira s progresivno videozapis zabilježene s kvadratne piksela

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Sljedeći korak
 
Pogledajte servisa Azure Media Services Tečajevi da biste lakše saznali o sjajne značajke koje nudi AMS.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
