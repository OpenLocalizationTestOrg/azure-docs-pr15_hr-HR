<properties
    pageTitle="  Objavljivanje sadržaja pomoću portala za Azure | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake za objavljivanje sadržaja pomoću portala za Azure."
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

# <a name="publish-content-with-the-azure-portal"></a>Objavljivanje sadržaja pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [OSTALE](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Pregled

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 

Da bi korisnički URL-om na koje je moguće koristiti za strujanje ili preuzimanje sadržaja, najprije morate "Objavi" vaše resursa stvaranjem na locator. Locators omogućuje pristup datotekama koje se nalaze u sredstava. Media Services podržava dvije vrste locators: 

- Strujanje locators (OnDemandOrigin), koristi za prilagodljivu streaming (Ako, na primjer, da biste strujanje MPEG CRTICE, HLS ili Smooth Streaming). Da biste stvorili strujanje locator vaše resursa mora sadržavati datoteku .ism. 
- Progresivno (SAS) locators, koristi za isporuku videozapise putem progresivno preuzimanje.


Strujanje URL ima sljedeći oblik i koristite da biste reproducirali Smooth Streaming resursi.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Da biste sastavili programa HLS strujanje URL-a, dodati (oblik = m3u8 aapl) na URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Da biste sastavili dugu CRTICU MPEG strujanje URL-a, dodati (oblik = mpd, vrijeme i csf) na URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

URL-a SAS ima sljedeći oblik.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Dodatne informacije potražite u članku [Pregled sadržaja isporuka](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Ako ste koristili portalu da biste stvorili locators prije ožujak 2015, locators s datumom isteka dvije godine su stvorene.  

Da biste ažurirali datum isteka na na locator, koristite [OSTALE](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) ili [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-ji. Imajte na umu da ažurirate datum isteka SAS locator URL promijeni.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Da biste koristili portal za objavljivanje imovine

Da biste koristili portal za objavljivanje sredstvo, učinite sljedeće:

1. [Portal za Azure](https://portal.azure.com/)odaberite svoj račun servisa Azure Media Services.
1. Odaberite **Postavke** > **Resursi**.
1. Odaberite resursa koje želite objaviti.
1. Kliknite gumb **Objavi** .
1. Odaberite vrstu locator.
2. Pritisnite **Dodaj**.

    ![Objavljivanje](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL će se dodaje na popis **Objavljene**URL-ova.

## <a name="play-content-from-the-portal"></a>Reprodukcija sadržaja s portala

Azure portal predstavlja sadržaja reproduktoru koji možete koristiti za testiranje videozapis.

Kliknite željeni videozapis, a zatim kliknite gumb **Reproduciraj** .

![Objavljivanje](./media/media-services-portal-vod-get-started/media-services-play.png)

Neke informacije vrijediti:

- Provjerite je li videozapis je objavljen.
- **Reproduktor** reproducira zadanu strujanje krajnjoj točki. Ako želite reproducirati na krajnjoj točki strujanje koji nisu zadani, kliknite da biste kopirali URL-a i koristi neki drugi program za reproduciranje. Na primjer, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Strujanje krajnjoj točki iz kojeg strujanje mora biti pokrenut.  
- Za strujanje iz strujanje krajnje točke, trebali biste dodati najmanje jednu strujanje jedinicu. Dodatne informacije potražite u članku [u ovoj](media-services-portal-scale-streaming-endpoints.md) se temi.   

##<a name="next-steps"></a>Daljnji koraci

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


