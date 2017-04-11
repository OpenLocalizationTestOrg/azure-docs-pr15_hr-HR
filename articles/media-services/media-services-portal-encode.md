<properties
    pageTitle="Kodiranje sredstvo pomoću Media Encoder standardne Azure portal | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake kodiranja sredstvo pomoću Media Encoder standardne Azure portal."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Kodiranje sredstvo pomoću Media Encoder standardne portala za Azure

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 

Prilikom rada s nešto Najčešći scenariji isporučuje prilagodljivu streaming brzina prijenosa u klijentima servisa Azure Media Services. Media Services podržava sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo). Da biste pripremili videozapise za strujanje prilagodljivo brzina prijenosa, morate kodiranje izvora videozapisa u više – brzina prijenosa datoteke. Koristite **Media Encoder standardne** kodiranje za kodiranje videozapise.  

Media Services i omogućuje dinamički pakiranje koja vam omogućuje da izlaganja vaš višestruki-brzina prijenosa MP4s u sljedećim oblicima strujanje: MPEG CRTICE, HLS, Smooth Streaming ili HDS bez potrebe za ponovno pakiranje u te strujanje oblici. Dinamični pakiranje samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta.

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- Kodiranje izvornu datoteku u skupu višestruki-brzina prijenosa MP4 datoteke (kodiranja korake su planirati kasnije u ovom odjeljku).
- Pronađite barem jedan strujanje jedinica za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite u članku [Konfiguriranje strujanje krajnje točke](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

Da biste skalirali obrada medijskih sadržaja, pogledajte [ovu](media-services-portal-scale-media-processing.md) temu.

## <a name="encode-with-the-azure-portal"></a>Šifriraj pomoću portala za Azure

U ovom se odjeljku opisuju koraci koje možete poduzeti da biste kodiranje sadržaja pomoću Media Encoder Standard.

1.  [Portal za Azure](https://portal.azure.com/)odaberite svoj račun servisa Azure Media Services.
2.  U prozoru **Postavke** odaberite **Resursi**.  
2.  U prozoru **imovine** odaberite resursa koje želite šifrirati.
3.  Pritisnite gumb **Kodiraj** .
4.  U prozoru **Kodiraj sredstvo** odaberite "Media Encoder standardni" procesora i gotovu. Na primjer, ako znate da unos videozapis ima razlučivost 1920 x 1080 piksela, pa možete koristiti u "H264 više brzina prijenosa 1080p" unaprijed postavljeni. Dodatne informacije o unaprijed postavljeno, [pogledajte članak –](https://msdn.microsoft.com/library/azure/mt269960.aspx) je važno da biste odabrali zadana postava koji je najprikladniji za unos videozapisa. Ako imate videozapisa niske razlučivosti (640 x 360), a zatim treba ne koristite zadani "H264 više brzina prijenosa 1080p" unaprijed postavljeni.
    
    Radi lakšeg upravljanja imate mogućnost uređivanja naziv resursa izlazne i naziv zadatka.
        
    ![Kodiranje resursi](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Pritisnite **Stvaranje**.


##<a name="next-step"></a>Sljedeći korak

Možete nadzirati kodiranja napretka posla pomoću portala za Azure kao što je opisano u [ovom](media-services-portal-check-job-progress.md) članku.  

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


