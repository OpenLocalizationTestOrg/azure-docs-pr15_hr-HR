<properties 
    pageTitle="Kako izvesti pomoću servisa Azure Media Services da biste stvorili višestruki-brzina prijenosa strujanja s portala za Azure uživo strujanje | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvaranje kanala koji prima aktivno strujanje jedne-brzina prijenosa i kodira strujanje višestruki-brzina prijenosa pomoću portala za Azure." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Kako izvesti uživo strujanje pomoću servisa Azure Media Services da biste stvorili višestruki-brzina prijenosa strujanja s portala za Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API-JA](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvaranje **kanala** koji prima jedan-brzina prijenosa aktivno strujanje kodira strujanje višestruki – brzina prijenosa.

>[AZURE.NOTE]Dodatne konceptualne informacije vezane uz kanalima za koje je omogućena za šifriranje uživo, potražite u članku [Live strujanje pomoću servisa Azure Media Services da biste stvorili strujanja višestruki-brzina prijenosa](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Uobičajeni scenarij strujanje uživo

Slijede općenite korake koji je uključen u stvaranju najčešćih aplikacija uživo strujanje.

>[AZURE.NOTE] Trenutno Maks preporučene uživo događaj traje osam sati. Obratite se amslived na Microsoft.com ako morate pokrenuti kanal za duljeg vremenskog razdoblja.

1. Povezivanje kameru s računalom. Pokretanje i konfiguriranje lokalnog uživo kodiranje koje možete poslati u jednom brzina prijenosa strujanje na jedan od sljedećih protokola: RTMP, Smooth Streaming ili RTP (MPEG-TS). Dodatne informacije potražite u članku [RTMP podrška za Azure Media Services i Live koderi](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    U ovom koraku također može izvršiti nakon što stvorite kanal.

1. Stvaranje i pokretanje kanala. 

1. Dohvaćanje kanal ingest URL-a. 

    Uživo kodiranje koristi ingest URL-a za slanje toka kanal.
1. Dohvaćanje URL-a za pregled kanala. 

    Pomoću ovog URL-a da biste potvrdili da kanala pravilno prima uživo toka.

3. Stvaranje događaja ili programa (koja također će se stvoriti imovine). 
1. Objavljivanje događaj (koji će se stvoriti u locator zahtjev za povezane resursa).  

    Provjerite jeste li imati barem jedno strujanje rezervirane jedinice na strujanje krajnjoj točki iz kojeg želite strujanje sadržaj.
1. Pokretanje događaja kada ste spremni za početak strujanje i arhiviranja.
2. Po želji uživo kodiranje možete signaled da biste pokrenuli oglas. Na oglašavanje se umeće u izlaz toka.
1. Zaustavite događaj kad god želite zaustaviti strujanje i arhiviranja događaj.
1. Brisanje događaja (i po želji brisanje imovine).   

##<a name="in-this-tutorial"></a>Pomoću ovog praktičnog vodiča

U ovom ćete praktičnom vodiču Azure portal se izvršiti sljedeće zadatke: 

2.  Konfiguriranje strujanje krajnje točke.
3.  Stvaranje kanala koji je omogućen za izvođenje uživo kodiranja.
1.  Da biste ga Live encoder navesti se Ingest URL. Pomoću ovog URL-a uživo encoder će ingest strujanje u kanal. .
1.  Stvaranje događaja ili programa (i sredstvo)
1.  Objaviti imovina i dobiti strujanje URL-ova  
1.  Reprodukcija sadržaja 
2.  Čišćenje

##<a name="prerequisites"></a>Preduvjeti

Sljedeće su potrebne da biste dovršili vodič.

- Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- Na račun servisa Media Services. Stvaranje računa Media Services potražite u članku [Stvaranje računa](media-services-portal-create-account.md).
- Web-kamere i kodiranje koje možete poslati u jednom brzina prijenosa uživo strujanje.

##<a name="configure-streaming-endpoints"></a>Konfiguriranje strujanje krajnje točke 

Media Services omogućuje dinamički pakiranje koja vam omogućuje da izlaganja vaš višestruki-brzina prijenosa MP4s u sljedećim oblicima strujanje: MPEG CRTICE, HLS, Smooth Streaming ili HDS bez potrebe za ponovno pakiranje u te strujanje oblici. Dinamični pakiranje samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta.

Da biste iskoristili dinamički pakiranje, morate pribaviti najmanje jednu strujanje jedinicu strujanje krajnjoj točki iz koje namjeravate isporuku sadržaja.  

Stvaranje i promjena broja strujanja rezervirana jedinice, učinite sljedeće:

1. Prijavite se na [portal za Azure](https://portal.azure.com/) i odaberite svoj račun AMS.
1. U prozoru **Postavke** kliknite **strujeće krajnje točke**. 

2. Kliknite zadani strujanje krajnjoj točki. 

    Otvara se prozor **ZADANI pojedinosti krajnjoj TOČKI strujanje** .

3. Da biste naveli broj strujanje jedinica, povucite klizač **strujeće jedinice** .

    ![Strujanje jedinice](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Kliknite gumb **Spremi** da biste spremili promjene.

    >[AZURE.NOTE]Dodjela sve nove jedinice može potrajati i do 20 minuta.

##<a name="create-a-channel"></a>Stvaranje kanala

1. [Portal za Azure](https://portal.azure.com/)Media Services odaberite, a zatim kliknite na naziv računa Media Services.
2. Odaberite **Live strujanje**.
3. Odaberite **Stvaranje Prilagođeno**. Ta mogućnost poslat će vam stvaranje kanala koji je omogućen za uživo kodiranja.

    ![Stvaranje kanala](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Kliknite **Postavke**.
    
    1.  Odaberite vrstu kanala **Live kodiranja** . Ta vrsta određuje želite li stvoriti kanala koji je omogućen za uživo kodiranja. To znači da se dolazni jedan brzina prijenosa strujanje se šalje na kanal i kodira u višestruki-brzina prijenosa strujanje pomoću postavki navedeni encoder uživo. Dodatne informacije potražite u članku [Live strujanje pomoću servisa Azure Media Services da biste stvorili strujanja višestruki-brzina prijenosa](media-services-manage-live-encoder-enabled-channels.md). Kliknite u redu.
    2. Navedite naziv na kanal.
    3. Kliknite u redu pri dnu zaslona.
    
5. Odaberite karticu **Ingest** .

    1. Na ovoj stranici možete odabrati protokol za strujanje. Za vrstu kanala **Live kodiranje** su valjani protokol mogućnosti:
        
        - Jedan brzina prijenosa Fragmented MP4 (Smooth Streaming)
        - Jedan brzina prijenosa RTMP
        - (MPEG-TS) RTP: Strujanje prijenosa MPEG-2 putem RTP.
        
        Detaljno objašnjenje svaki protokol, potražite u članku [Live strujanje pomoću servisa Azure Media Services da biste stvorili strujanja višestruki-brzina prijenosa](media-services-manage-live-encoder-enabled-channels.md).
    
        Ne možete promijeniti mogućnosti protokol tijekom kanal ili koristite njegove pridružene/programa događaja. Ako zahtijevaju različite protokola, potrebno je stvoriti zasebnu kanalima za svaki strujanje protokol.  

    2. Ograničenja IP možete primijeniti na na ingest. 
    
        Možete definirati IP adrese dopušteni ingest videozapis da biste kanala. Dopuštene IP adrese možete navesti kao jedan IP adresa (npr. "10.0.0.1"), raspon IP pomoću IP adresa i masku CIDR (npr. "10.0.0.1/22") ili raspona IP pomoću IP adresa i istočkanom decimalni masku (npr. "10.0.0.1(255.255.252.0)').

        Ako su navedeni bez IP adresa i postoji bez definicija pravila pa nema IP adresa dozvoljen. Da biste omogućili bilo koje IP adrese, stvorite pravilo i postavite 0.0.0.0/0.

6. Na kartici **Pretpregled** primijeniti IP ograničenja pretpregled.
7. Na kartici **kodiranje** navedite kodiranja zadana Postava. 

    Trenutno samo sustava je gotova konfiguracija možete odabrati **zadano 720 p**. Da biste odredili prilagođene zadana Postava, otvorite u programu Microsoft zahtjev za podršku možete. Nakon toga unesite naziv zadana postava za vas stvara. 

>[AZURE.NOTE] Trenutno start kanala može potrajati i do 30 minuta. Vrati izvorne postavke kanala može potrajati i do pet minuta.

Nakon stvaranja kanal, možete na kanal kliknite i odaberite **Postavke** gdje može vidjeti vaše konfiguracije kanala. 

Dodatne informacije potražite u članku [Live strujanje pomoću servisa Azure Media Services da biste stvorili strujanja višestruki-brzina prijenosa](media-services-manage-live-encoder-enabled-channels.md).


##<a name="get-ingest-urls"></a>Dohvaćanje ingest URL-ova

Nakon stvaranja kanal, možete dobiti ingest URL-ovi koje će nude uživo kodiranje. Kodiranje koristi URL-ove za unos uživo strujanje.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Stvaranje i upravljanje događaja

###<a name="overview"></a>Pregled

Kanal povezan je s događaje/programe koji omogućuju vam da biste odredili objavljivanje i prostora za pohranu segmente uživo strujanje. Kanala upravljanje/programa događaja. Odnos kanal i Program vrlo je sličan tradicionalni media gdje kanala sadrži konstante strujanje sadržaja i programa za implementaciju ograničen je na nekim vremenskim događaja na tom kanalu.

Možete navesti broj sati koje želite zadržati snimljeni sadržaj za događaj postavljanjem duljine **Prozor Arhiva** . Tu vrijednost postavite od najmanje pet minuta najviše 25 sati. Duljina prozor arhiva određuje i maksimalnu količinu vremena klijente možete traženje unatrag u vremenu od trenutnog mjesta uživo. Događaji koje možete pokrenuti putem određeno vrijeme, ali sadržaja koja se nalazi iza prozora duljine neprestano odbacuju. Ta vrijednost ovo svojstvo određuje i koliko manifesti klijenta možete povećavaju jedan za drugim.

Svaki događaj povezan je s sredstava. Da biste objavili događaj stvarate programa locator zahtjev za povezane resursa. Imate ovaj locator će vam omogućiti da biste sastavili strujanje URL koje nude klijentima.

Kanal podržava do tri istovremeno izvodi događaja da bi mogli stvarati više arhive isti dolazne toka. Time objaviti, a zatim arhiviranje različite dijelove događaja po potrebi. Na primjer, svojim potrebama tvrtke je za arhiviranje 6 sati događaja, no emitirati samo posljednjih 10 minuta. Da biste to postigli, morate stvoriti dvije istovremeno radi događaj. Jedan događaj postavljen za arhiviranje 6 sati događaja, no program je objavljen. Drugi događaj postavljeno za arhiviranje 10 minuta, a ovaj program je objavljen.

Nije potrebno ponovno koristiti postojeće programe za nove događaje. Umjesto toga, stvaranje i pokretanje novog programa za svaki događaj.

Kada ste spremni za početak strujanje i arhiviranja, pokrenite/programa događaja. Zaustavite događaj kad god želite zaustaviti strujanje i arhiviranja događaj. 

Da biste izbrisali arhivirane sadržaj, zaustavljanje i brisanje događaja, a zatim izbrišite povezane resursa. Sredstva ne može se izbrisati ako se koristi događaj; najprije potrebno je izbrisati događaj. 

Čak i kada zaustavite i brisanje događaja, korisnici će moći strujanje arhivirane sadržaja kao videozapisa na zahtjev, za sve dok ne izbrišete imovine.

Ako želite da biste zadržali arhivirane sadržaj, ali ne moraju biti dostupan za strujanje, izbrišite strujanje locator.

###<a name="createstartstop-events"></a>Stvaranje/početak i kraj događaja

Nakon što dodate strujanje slijedi u kanal stvaranjem programa resursa, programa i strujanje Locator možete početi strujanje događaja. To će arhiviranje toka i učiniti je dostupnom udaljenim gledateljima kroz krajnja točka za strujanje. 

Da biste pokrenuli događaja na dva načina: 

1. Na stranici **kanal** pritisnite **Live događaja** da biste dodali novi događaj.

    Odredite: naziv događaja, naziv resursa, prozor arhiva i mogućnost šifriranja.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Ako ostane potvrđen okvir **Objavi sada se uživo događaj** , stvorit će se događaj OBJAVLJIVANJE URL-ova.
    
    Možete pritisnuti **pokretanje**, svaki put kada budete spremni za strujanje događaja.

    Kada počnete događaj, možete pritisnuti **pogledajte** da počnu sadržaj.

2. Osim toga, možete koristiti prečac i pritisnite tipku **Live idite** na **kanal** stranica. To će stvoriti zadani resursa, programa i strujanje Locator.

    Događaj pod nazivom **zadane** i prozor arhiva postavljen na osam sati.

Možete gledati objavljene događaja na stranici **Live događaj** . 

Ako kliknete **Zraka za isključivanje**, će prekinuti Svi događaji uživo. 


##<a name="watch-the-event"></a>Pogledajte na događaj

Gledati događaj, kliknite **nadzor** na portalu za Azure ili kopiranje strujanje URL-a i korištenje reproduktora po izboru. 
 
![Stvorili](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Uživo događaj automatski pretvara događaje sadržaj na zahtjev kada se zaustaviti.

##<a name="clean-up"></a>Čišćenje

Ako gotovi strujanje događaja, a želite očistiti resursi dodjeli ranije, slijedite postupak u nastavku.

- Zaustavite margina toka iz kodiranje.
- Zaustavite kanal. Kada se Zaustavi kanal, će uzrokovati bilo kakvih troškova. Kada je potrebno ponovno pokrenuti, imat će isti ingest URL pa nećete morati konfigurirajte vaše kodiranje.
- Ako ne želite nastaviti omogućuje arhiviranje uživo događaja kao strujanje sustava na zahtjev, vaše strujanje krajnje točke, možete isključiti. Ako je kanal prestao stanje, će uzrokovati bilo kakvih troškova.
  
##<a name="view-archived-content"></a>Prikaz arhiviraju sadržaja

Čak i kada zaustavite i brisanje događaja, korisnici će moći strujanje arhivirane sadržaja kao videozapisa na zahtjev, za sve dok ne izbrišete imovine. Sredstva nije moguće izbrisati ako je koristi događaj; najprije potrebno je izbrisati događaj. 

Da biste upravljali vaše imovine, odaberite **postavku** , a zatim kliknite **Resursi**.

![Resursi](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Razmatranja

- Trenutno Maks preporučene uživo događaj traje osam sati. Obratite se amslived na Microsoft.com ako morate pokrenuti kanal za duljeg vremenskog razdoblja.
- Provjerite jeste li imati barem jedno strujanje rezervirane jedinice na strujanje krajnjoj točki iz kojeg želite strujanje sadržaj.


##<a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
