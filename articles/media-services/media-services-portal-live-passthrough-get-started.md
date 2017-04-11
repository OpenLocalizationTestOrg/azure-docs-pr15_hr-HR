<properties 
    pageTitle="Kako izvesti uživo strujanje koderi na lokaciji pomoću portala za Azure | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvaranje kanala koji je konfiguriran za prolazni isporuke." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Kako izvesti uživo strujanje koderi na lokaciji pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [OSTALE]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Pomoću ovog praktičnog vodiča vodit će vas kroz korake za stvaranje **kanala** koji je konfiguriran za prolazni isporuke pomoću portala za Azure. 

##<a name="prerequisites"></a>Preduvjeti

Sljedeće su potrebne za dovršetak vodič:

- Račun Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 
- Na račun servisa Media Services. Stvaranje računa Media Services potražite u članku [Stvaranje računa za servise medijske sadržaje](media-services-portal-create-account.md).
- Web-kamere. Na primjer, [Telestream Wirecast kodiranje](http://www.telestream.net/wirecast/overview.htm).

Preporučujemo da pregledate u sljedećim člancima:

- [Azure Media Services RTMP podržavaju i Live koderi](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Pregled uživo strujanja pomoću servisa Azure Media Services](media-services-manage-channels-overview.md)
- [Live strujanje koderi na lokaciji koje stvorite strujanja višestruki-brzina prijenosa](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Uobičajeni scenarij strujanje uživo

Sljedeći koraci opisuju zadatke koji su uključeni u stvaranju uobičajenih uživo aplikacije za strujanje koje koriste kanalima za koje su konfigurirana za isporuku prolazni. Pomoću ovog praktičnog vodiča pokazuje kako stvarati i upravljati prolazni kanal i uživo događaja.

1. Povezivanje kameru s računalom. Pokretanje i konfiguriranje lokalnog uživo kodiranje koji proizvodi višestruki-brzina prijenosa RTMP ili fragmentirane MP4 strujanje. Dodatne informacije potražite u članku [RTMP podrška za Azure Media Services i Live koderi](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    U ovom koraku također može izvršiti nakon što stvorite kanal.

1. Stvaranje i pokretanje prolazni kanala.
1. Dohvaćanje kanal ingest URL-a. 

    Uživo kodiranje koristi ingest URL-a za slanje toka kanal.
1. Dohvaćanje URL-a za pregled kanala. 

    Pomoću ovog URL-a da biste potvrdili da kanala pravilno prima uživo toka.

3. Stvaranje uživo događaj ili programa. 

    Kada pomoću portala za Azure stvaranju uživo događaja i stvara sredstava. 
      
    >[AZURE.NOTE]Provjerite jeste li imati barem jedno strujanje rezervirane jedinice na strujanje krajnjoj točki iz kojeg želite strujanje sadržaj.
1. Pokrenite događaj/program kada ste spremni za početak strujanje i arhiviranja.
2. Po želji uživo kodiranje možete signaled da biste pokrenuli oglas. Na oglašavanje se umeće u izlaz toka.
1. Zaustavite događaj ili programa kad god želite zaustaviti strujanje i arhiviranja događaj.
1. Brisanje događaja ili programa (i po želji brisanje imovine).     

>[AZURE.IMPORTANT] Pregledajte [uživo strujanje koderi na lokaciji koje stvorite višestruki-brzina prijenosa strujanja](media-services-live-streaming-with-onprem-encoders.md) dodatne informacije o koncepata i napomene vezane uz uživo strujanje koderi na lokaciji i prolazni kanala.

##<a name="to-view-notifications-and-errors"></a>Da biste vidjeli obavijesti i pogreške

Ako želite da biste vidjeli obavijesti i pogreške koje je stvorio portala za Azure, kliknite ikonu obavijesti.

![Obavijesti](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Konfiguriranje strujanje krajnje točke 

Media Services omogućuje dinamički pakiranje, koji omogućuje izlaganja vaš višestruki-brzina prijenosa MP4s u sljedećim oblicima strujanje: MPEG CRTICE, HLS, Smooth Streaming ili HDS bez potrebe za repackage u ovo strujanje oblici. Dinamični pakiranje samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services stvara i služi prikladan odgovor na temelju zahtjeve iz klijenta.

Da biste iskoristili dinamički pakiranje, morate pribaviti najmanje jednu strujanje jedinicu strujanje krajnjoj točki iz koje namjeravate isporuku sadržaja.  

Stvaranje i promjena broja strujanja rezervirana jedinice, učinite sljedeće:

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
1. U prozoru **Postavke** kliknite **strujeće krajnje točke**. 

2. Kliknite zadani strujanje krajnjoj točki. 

    Otvara se prozor **ZADANI pojedinosti krajnjoj TOČKI strujanje** .

3. Da biste naveli broj strujanje jedinica, povucite klizač **strujeće jedinice** .

    ![Strujanje jedinice](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Kliknite gumb **Spremi** da biste spremili promjene.

    >[AZURE.NOTE]Dodjela sve nove jedinice može potrajati i do 20 minuta.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Stvaranje i pokretanje prolazni kanala i događaje

Kanal povezan je s događaje/programe koji omogućuju vam da biste odredili objavljivanje i prostora za pohranu segmente uživo strujanje. Kanala upravljali događajima. 
    
Možete navesti broj sati koje želite zadržati snimljeni sadržaj za program postavljanjem duljine **Prozor Arhiva** . Tu vrijednost postavite od najmanje pet minuta najviše 25 sati. Duljina prozor arhiva određuje i maksimalnu količinu vremena klijente možete traženje unatrag u vremenu od trenutnog mjesta uživo. Događaji koje možete pokrenuti putem određeno vrijeme, ali sadržaja koja se nalazi iza prozora duljine neprestano odbacuju. Ta vrijednost ovo svojstvo određuje i koliko manifesti klijenta možete povećavaju jedan za drugim.

Svaki događaj povezan je s sredstava. Da biste objavili događaj, morate stvoriti programa locator zahtjev za povezane resursa. Imate ovaj locator vam omogućuje sastavljanje strujanje URL koje nude klijentima.

Kanal podržava do tri istovremeno izvodi događaja da bi mogli stvarati više arhive isti dolazne toka. Time objaviti, a zatim arhiviranje različite dijelove događaja po potrebi. Na primjer, svojim potrebama tvrtke je za arhiviranje 6 sati programa, ali emitirati samo posljednjih 10 minuta. Da biste to postigli, morate stvoriti dva istovremeno pokrenute programe. Jedan program postavljena za arhiviranje 6 sati događaja, ali program nisu objavljene. U drugi program postavljeno za arhiviranje 10 minuta, a ovaj program je objavljen.

Nije potrebno ponovno koristiti postojeće događaje uživo. Umjesto toga, stvaranje i pokretanje novog događaja za svaki događaj.

Pokretanje događaja kada ste spremni za početak strujanje i arhiviranja. Zaustavite program kad god želite zaustaviti strujanje i arhiviranja događaj. 

Da biste izbrisali arhivirane sadržaj, zaustavljanje i brisanje događaja, a zatim izbrišite povezane resursa. Sredstva nije moguće izbrisati ako je koristi događaj; najprije potrebno je izbrisati događaj. 

Čak i kada zaustavite i brisanje događaja, korisnici će moći strujanje arhivirane sadržaja kao videozapisa na zahtjev, za sve dok ne izbrišete imovine.

Ako želite da biste zadržali arhivirane sadržaj, ali ne moraju biti dostupan za strujanje, izbrišite strujanje locator.

###<a name="to-use-the-portal-to-create-a-channel"></a>Da biste koristili portala za stvaranje kanala 

U ovom se odjeljku pokazuje kako koristiti mogućnost **Brzo stvaranje** stvaranje prolazni kanala.

Dodatne informacije o prolazni kanala potražite u članku [Live strujanje koderi na lokaciji koje stvorite strujanja višestruki-brzina prijenosa](media-services-live-streaming-with-onprem-encoders.md).

1. [Portal za Azure](https://portal.azure.com/)odaberite svoj račun servisa Azure Media Services.
2. U prozoru **Postavke** kliknite **Live strujanje**. 

    ![Početak rada](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Otvara se prozor **uživo strujanje** .

3. Kliknite da biste stvorili prolazni kanala na RTMP **Brzo stvaranje** ingest protokol.

    Otvara se prozor za **Stvaranje NOVOG kanala odgovora** .
4. Unesite naziv novog kanala, a zatim kliknite **Stvori**. 

    Time ste stvorili prolazni kanal s na RTMP ingest protokol.

##<a name="create-events"></a>Stvaranje događaja

1. Odabir kanala na koje želite dodati događaj.
2. Pritisnite gumb **Live događaj** .

![Događaja](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Dohvaćanje ingest URL-ova

Nakon stvaranja kanal, možete dobiti ingest URL-ovi koje će nude uživo kodiranje. Kodiranje koristi URL-ove za unos uživo strujanje.

![Stvorili](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Pogledajte na događaj

Gledati događaj, kliknite **nadzor** na portalu za Azure ili kopiranje strujanje URL-a i korištenje reproduktora po izboru. 
 
![Stvorili](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Uživo događaj automatski se pretvaraju nakon prestao sadržaj na zahtjev.

##<a name="clean-up"></a>Čišćenje

Dodatne informacije o prolazni kanala potražite u članku [Live strujanje koderi na lokaciji koje stvorite strujanja višestruki-brzina prijenosa](media-services-live-streaming-with-onprem-encoders.md).

- Kanal može se zaustaviti samo kada je zaustavljena sva/programa događaja na kanal.  Kada se Zaustavi kanal, plaćati bilo kakvih troškova. Kada je potrebno ponovno pokrenuti, imat će isti ingest URL pa nećete morati konfigurirajte vaše kodiranje.
- Kanal moguće je izbrisati samo kad je izbrisana Svi događaji uživo na kanal.

##<a name="view-archived-content"></a>Prikaz arhiviraju sadržaja

Čak i kada zaustavite i brisanje događaja, korisnici će moći strujanje arhivirane sadržaja kao videozapisa na zahtjev, za sve dok ne izbrišete imovine. Sredstva nije moguće izbrisati ako je koristi događaj; najprije potrebno je izbrisati događaj. 

Da biste upravljali vaše imovine, odaberite **postavku** , a zatim kliknite **Resursi**.

![Resursi](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
