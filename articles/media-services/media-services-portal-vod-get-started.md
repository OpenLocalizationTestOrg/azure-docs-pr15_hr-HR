<properties
    pageTitle=" Početak rada s isporuke sadržaja na zahtjev pomoću portala za Azure | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake implementacijom osnovni Video-on-Demand (VoD) servis za isporuku sadržaja s aplikacijom servisa Azure Media Services (AMS) pomoću portala za Azure."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Početak rada s isporuke sadržaja na zahtjev pomoću portala za Azure

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Pomoću ovog praktičnog vodiča vodit će vas kroz korake implementacijom osnovni Video-on-Demand (VoD) servis za isporuku sadržaja s aplikacijom servisa Azure Media Services (AMS) pomoću portala za Azure.

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 

Pomoću ovog praktičnog vodiča obuhvaća sljedeće zadatke:

1.  Stvorite račun servisa Azure Media Services.
2.  Konfiguriranje strujanje krajnjoj točki.
1.  Prijenos videozapisa datoteke.
1.  Kodiranje izvornu datoteku u skupu prilagodljivo brzina prijenosa MP4 datoteke.
1.  Objavljivanje resursa i dobivanje strujanje i progresivno preuzimanje URL-ova.  
1.  Reprodukcija sadržaja.


## <a name="create-an-azure-media-services-account"></a>Stvorite račun servisa Azure Media Services

Koraci u ovom odjeljku pokazati kako stvoriti račun AMS.

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **+ Novo** > **Web + Mobile** > **Media Services**.

    ![Stvaranje Media Services](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. U **Stvaranje RAČUNA za SERVISE MEDIA** unesite tražene vrijednosti.

    ![Stvaranje Media Services](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. U **Naziv računa**, unesite naziv novog računa AMS. Naziv računa za Media Services sve malo brojeva ili slova bez razmaka, a 3 i 24 znakova.
    2. U pretplatu, odaberite između različitih Azure pretplate koje imate pristup.
    
    2. U **Grupi resursa**, odaberite novi ili postojeći resurs.  Grupa resursa je skup resursa koji životni ciklus, dozvolama i pravilnicima za zajedničko korištenje. Dodatne informacije [u nastavku](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Mjesto**, odaberite u regiji se koristi za pohranu zapisa mediji i metapodataka za vaš račun Media Services. Ovo područje koristi se za obradu i strujanje medijskih sadržaja. Samo dostupne Media Services područja pojavljuju se u okviru padajućeg popisa. 
    
    3. U **Račun za pohranu**, odaberite račun za pohranu možete unijeti blobova medijskog sadržaja s računa Media Services. Možete odabrati postojeći račun za pohranu u istom regiji kao račun Media Services ili možete stvoriti račun za pohranu. U istom području stvaranja novog računa za pohranu. Pravila za nazive računa za pohranu isti su kao računi Media Services.

        Dodatne informacije o prostora za pohranu [u nastavku](storage-introduction.md).

    4. Odaberite **Prikvači na nadzornu ploču** da biste vidjeli tijeku implementacije računa.
    
7. Kliknite **Stvori** pri dnu obrasca.

    Kada je račun uspješno stvorili, status mijenja se u **pokrenut**. 

    ![Postavke Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Da biste upravljali računa AMS (na primjer, prijenos videozapisa, kodiranje resursi, praćenje napretka posla) pomoću prozora **Postavke** .

## <a name="manage-keys"></a>Upravljanje tipke

Potreban je naziv računa i informacija primarnog ključa za programatski pristup računu Media Services.

1. Na portalu Azure odaberite svoj račun. 

    Pojavit će se prozor **Postavke** na desnoj strani. 

2. U prozoru **Postavke** odaberite **tipke**. 

    **Upravljanje ključevima** windows prikazuje naziv računa i prikazuju se primarnih i sekundarnih ključeva. 
3. Pritisnite gumb Kopiraj da biste kopirali vrijednosti.
    
    ![Media Services tipke](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Konfiguriranje strujanje krajnje točke

Prilikom rada s nešto Najčešći scenariji isporučuje videozapis putem prilagodljivu streaming brzina prijenosa u klijentima servisa Azure Media Services. Media Services podržava sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo).

Media Services nudi dinamički pakiranje koja vam omogućuje da izlaganja vaš prilagodljivo brzina prijenosa MP4 kodirani sadržaja u strujanje oblici koje podržava Media Services (MPEG CRTICE, HLS, Smooth Streaming, HDS) samo-u – vrijeme, bez potrebe za pohranu unaprijed zapakirani verzije svaki od ovih strujanje oblici.

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- Kodiranje datoteku mezzanine (izvor) u skupu prilagodljivo brzina prijenosa MP4 datoteke (kodiranja korake su što je prikazano u nastavku ovog praktičnog vodiča).  
- Stvorite najmanje jednu strujanje jedinicu u *strujanje krajnjoj točki* iz koje namjeravate isporuku sadržaja. Koraci u nastavku pokazuju kako da biste promijenili broj strujanje jedinica.

Dinamični pakiranje samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services stvara i služi prikladan odgovor na temelju zahtjeve iz klijenta.

Stvaranje i promjena broja strujanja rezervirana jedinice, učinite sljedeće:


1. U prozoru **Postavke** kliknite **strujeće krajnje točke**. 

2. Kliknite zadani strujanje krajnjoj točki. 

    Otvara se prozor **ZADANI pojedinosti krajnjoj TOČKI strujanje** .

3. Da biste naveli broj strujanje jedinica, povucite klizač **strujeće jedinice** .

    ![Strujanje jedinice](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknite gumb **Spremi** da biste spremili promjene.

    >[AZURE.NOTE]Dodjela sve nove jedinice može potrajati i do 20 minuta.

## <a name="upload-files"></a>Prijenos datoteka

Strujanje videozapisa pomoću servisa Azure Media Services, morate prijenos videozapisa izvora, kodiranje ih u više bitrates i objavljivanje rezultat. Prvi korak opisana u ovom odjeljku. 

1. U prozoru **Postavke** kliknite **Resursi**.

    ![Prijenos datoteka](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Kliknite gumb **Prenesi** .

    Otvara se prozor za **Prijenos videozapisa resursa** .

    >[AZURE.NOTE] Postoji nema ograničenja veličine datoteka.
    
4. Pronađite željenu videozapis na vašem računalu, odaberite ga i kliknite u redu.  

    Prijenos pokreće i vidjet ćete napredak u odjeljak naziv datoteke.  

Kada prijenos završi, vidjet ćete novi resursa koji je naveden u prozoru **Resursi** . 

## <a name="encode-assets"></a>Kodiranje resursi

Prilikom rada s nešto Najčešći scenariji isporučuje prilagodljivu streaming brzina prijenosa u klijentima servisa Azure Media Services. Media Services podržava sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo). Da biste pripremili videozapise za strujanje prilagodljivo brzina prijenosa, morate kodiranje izvora videozapisa u više – brzina prijenosa datoteke. Koristite **Media Encoder standardne** kodiranje za kodiranje videozapise.  

Media Services i omogućuje dinamički pakiranje, koji omogućuje izlaganja vaš višestruki-brzina prijenosa MP4s u sljedećim oblicima strujanje: MPEG CRTICE, HLS, Smooth Streaming ili HDS bez potrebe za repackage u te strujanje oblici. Dinamični pakiranje samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services stvara i služi prikladan odgovor na temelju zahtjeve iz klijenta.

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- Kodiranje izvornu datoteku u skupu višestruki-brzina prijenosa MP4 datoteke (kodiranja korake su planirati kasnije u ovom odjeljku).
- Pronađite barem jedan strujanje jedinica za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite u članku [Konfiguriranje strujanje krajnje točke](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### <a name="to-use-the-portal-to-encode"></a>Da biste koristili na portalu za kodiranje

U ovom se odjeljku opisuju koraci koje možete poduzeti da biste kodiranje sadržaja pomoću Media Encoder Standard.

1.  U prozoru **Postavke** odaberite **Resursi**.  
2.  U prozoru **imovine** odaberite resursa koje želite šifrirati.
3.  Pritisnite gumb **Kodiraj** .
4.  U prozoru **Kodiraj sredstvo** odaberite "Media Encoder standardni" procesora i gotovu. Na primjer, ako znate da unos videozapis ima razlučivost 1920 x 1080 piksela, pa možete koristiti u "H264 više brzina prijenosa 1080p" unaprijed postavljeni. Dodatne informacije o unaprijed postavljeno, [pogledajte članak –](https://msdn.microsoft.com/library/azure/mt269960.aspx) je važno da biste odabrali zadana postava koji je najprikladniji za unos videozapisa. Ako imate videozapisa niske razlučivosti (640 x 360), a zatim treba ne koristite zadani "H264 više brzina prijenosa 1080p" unaprijed postavljeni.
    
    Radi lakšeg upravljanja imate mogućnost uređivanja naziv resursa izlazne i naziv zadatka.
        
    ![Kodiranje resursi](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Pritisnite **Stvaranje**.

### <a name="monitor-encoding-job-progress"></a>Praćenje napretka kodiranja posla

Praćenje napretka kodiranja posla, kliknite **Postavke** (pri vrhu stranice), a zatim odaberite **Zadaci**.

![Zadaci](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Objavljivanje sadržaja

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

>[AZURE.NOTE] Ako ste koristili portalu da biste stvorili locators prije ožujak 2015, locators s datumom isteka dvije godine su stvorene.  

Da biste ažurirali datum isteka na na locator, koristite [OSTALE](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) ili [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-ji. Kada ažurirate datum isteka SAS locator, mijenja se URL-a.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Da biste koristili portal za objavljivanje imovine

Da biste koristili portal za objavljivanje sredstvo, učinite sljedeće:

1. Odaberite **Postavke** > **Resursi**.
1. Odaberite resursa koje želite objaviti.
1. Kliknite gumb **Objavi** .
1. Odaberite vrstu locator.
2. Pritisnite **Dodaj**.

    ![Objavljivanje](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL se dodaje na popis **Objavljene**URL-ova.

## <a name="play-content-from-the-portal"></a>Reprodukcija sadržaja s portala

Azure portal predstavlja sadržaja reproduktoru koji možete koristiti za testiranje videozapis.

Kliknite željeni videozapis, a zatim kliknite gumb **Reproduciraj** .

![Objavljivanje](./media/media-services-portal-vod-get-started/media-services-play.png)

Neke informacije vrijediti:

- Provjerite je li videozapis je objavljen.
- **Reproduktor** reproducira zadanu strujanje krajnjoj točki. Ako želite reproducirati na krajnjoj točki strujanje koji nisu zadani, kliknite da biste kopirali URL-a i koristi neki drugi program za reproduciranje. Na primjer, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##<a name="next-steps"></a>Daljnji koraci

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


