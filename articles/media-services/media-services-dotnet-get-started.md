<properties
    pageTitle="Početak rada s isporuke sadržaja na zahtjev pomoću .NET | Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake implementacijom aplikaciju za isporuku sadržaja na zahtjev s .NET pomoću servisa Azure Media Services."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Početak rada s isporuke sadržaja na zahtjev pomoću .NET SDK-a

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Pregled 

Pomoću ovog praktičnog vodiča vodit će vas kroz korake implementacijom aplikacije za isporuku sadržaja Video-on-Demand (VoD) pomoću servisa Azure Media Services (AMS) SDK za .NET.


Vodič predstavlja tijeka rada za osnovni Media Services i najčešće programiranje objekte i zadatke koji su potrebni za razvoj Media Services. Po završetku vodič, moći strujanje ili progresivno Preuzimanje oglednih medijsku datoteku prenijeli, kodiranja i preuzeti.

## <a name="what-youll-learn"></a>Saznat ćete

Vodič prikazuje kako izvršiti sljedeće zadatke:

1.  Stvaranje računa Media Services (pomoću portala za Azure).
2.  Konfiguriranje strujanje krajnju točku (pomoću portala za Azure).
3.  Stvaranje i konfiguriranje projekta za Visual Studio.
5.  Povezivanje s računom za Media Services.
6.  Stvorite novi resursa i Učitavanje videodatoteke.
7.  Kodiranje izvornu datoteku u skupu prilagodljivo brzina prijenosa MP4 datoteke.
8.  Objavljivanje sredstva te pronađite URL-ovi za strujanje i progresivno preuzimanje.
9.  Testirajte dodavanjem sadržaj koji se reproducira.

## <a name="prerequisites"></a>Preduvjeti

Sljedeće su potrebne za dovršetak vodič.

- Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. 
    
    Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F). Dobit ekipa koje je moguće koristiti da biste isprobali plaćenu servisa Azure. Čak i kada koriste na kredita, možete zadržati račun i pomoću besplatnog servisa Azure i značajke, kao što je značajka web-aplikacije u aplikacije servisa za Azure.
- Operacijske sustave: Windows 8 ili noviji, Windows 2008 R2, Windows 7.
- .NET framework 4.0 ili noviji
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate ili Express) ili novije verzije.


##<a name="download-sample"></a>Preuzimanje oglednih

Početak i pokretanje uzorka s [ovdje](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Stvorite račun servisa Azure Media Services pomoću portala za Azure

Koraci u ovom odjeljku pokazati kako stvoriti račun AMS.

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **+ Novo** > **Media + CDN** > **Media Services**.

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

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfiguriranje strujanje krajnje točke pomoću portala za Azure

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

##<a name="create-and-configure-a-visual-studio-project"></a>Stvaranje i konfiguriranje projekta za Visual Studio

1. Stvaranje nove C# konzole aplikacije Visual Studio 2013 i Visual Studio 2012 ili u Visual Studio 2010 SP1. Unesite **naziv**, **mjesto**i **naziv rješenja**, a zatim kliknite **u redu**.

2. Pomoću značajke pakiranja NuGet [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) da biste instalirali **Azure Media Services .NET SDK proširenja**.  Media Services .NET SDK proširenja je skup Pomoćnik za funkcije koje će Pojednostavnite kodu i lakše razviti s Media Services i nastavak metode. Instalirate paket, instalira **Media Services.NET SDK** i dodaje ostale potrebne ovisnosti.

3. Dodavanje reference u sklopu System.Configuration. Ovaj skup sadrži klasa **System.Configuration.ConfigurationManager** koja se koristi za pristup konfiguracijske datoteke, na primjer, App.config.

4. Otvorite datoteku App.config (Dodavanje datoteke u projekt ako je dodana po zadanom) i dodajte odjeljak *appSettings* u datoteku. Postavljanje vrijednosti za servisa Azure Media Services imenom i računom ključ računa, kao što je prikazano u sljedećem primjeru. Da biste dobili naziv računa i informacija o ključu, idite na [portal za Azure](https://portal.azure.com/) i odaberite svoj račun AMS. Zatim odaberite **Postavke** > **tipke**. Tipke windows upravljanje prikazuje naziv računa i prikazuju se primarnih i sekundarnih ključeva.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Prebriši postojeće **pomoću** naredbe na početku datoteke Program.cs sljedeći kod.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Stvorite novu mapu pod imenik projekata i kopirajte .mp4 ili .wmv datoteke koju želite šifrirati i strujanje ili progresivno preuzimanje. U ovom se primjeru koristi put "C:\VideoFiles".

##<a name="connect-to-the-media-services-account"></a>Povezivanje s računom Media Services

Kada pomoću .NET Media Services, morate koristiti klase **CloudMediaContext** za većinu Media Services programiranje zadataka: povezivanje s računom Media Services; Stvaranje, ažuriranje, pristup i brisanje sljedeće objekte: resursi, datoteka resursa, zadatke, access pravila, locators itd.

Prebrisati zadanu klasu Program sljedeći kod. Kod pokazuje kako čitati vrijednosti veze iz datoteke App.config i kako stvoriti objekt **CloudMediaContext** radi povezivanja sa Media Services. Dodatne informacije o povezivanju s Media Services potražite u članku [Povezivanje sa Media Services s Media Services SDK za .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Funkcija **glavne** poziva metode koje će se definirati daljnje u ovom odjeljku.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Stvaranje novog resursa i prijenos video datoteka

Media Services koje prijenos (ili ingest) digitalni datoteke u sredstava. Entitet **resursa** može sadržavati videozapis, zvuk, slike, minijatura zbirke, tekst zapise, i titlova datoteke (i metapodataka o tim datotekama.)  Nakon prijenosa datoteka sadržaj pohranjen sigurno u oblaku za daljnje obrade i strujanje. Datoteke imovine nazivaju **Datoteke resursa**.

**UploadFile** način definiran ispod pozive **CreateFromFile** (definirano u .NET SDK Extensions). **CreateFromFile** stvara novi resursa u kojem se prenese datoteka navedeni izvor.

Način **CreateFromFile** uzima **AssetCreationOptions** koji omogućuje vam da navedete jednu od sljedećih mogućnosti za stvaranje resursa:

- Koristi se **ništa** – nema šifriranja. To je zadana vrijednost. Imajte na umu da kada koristite tu mogućnost, sadržaj nije zaštićena na putu ili na ostale u prostor za pohranu.
Ako namjeravate održati programa MP4 korištenje progresivno preuzimanje koristiti tu mogućnost.
- **StorageEncrypted** – korištenje šifriraju tu mogućnost da biste šifrirali Očisti sadržaj lokalno koristi Napredno šifriranje standardne (AES)-256 bitne šifriranje, koji se prenosi prostora za pohranu Azure na kojem je pohranjena na ostale. Resursi koji su zaštićeni šifriranjem prostora za pohranu se automatski nešifrirani smjestiti u sustavu šifriranu datoteku prije no što kodiranja i po želji se ponovno šifrirane prije no što prenesete natrag kao novi izlaz resursa. Slučaj prvenstveno se koristi za pohranu šifriranje je kada želite sigurne visoke kvalitete unos medijske datoteke s šifriranjem na ostale na disku.
- **CommonEncryptionProtected** - koristite ovu mogućnost ako prenosite sadržaj koji je već šifrirane i zaštićen uobičajenih šifriranje ili PlayReady DRM (na primjer, izglađenim strujanje zaštićeni PlayReady DRM).
- **EnvelopeEncryptionProtected** – pomoću te mogućnosti prenosite HLS šifrirane i zaštićene AES. Imajte na umu da datoteke mora su kodirani i šifrirane Upravitelj pretvoriti.

Metodu **CreateFromFile** omogućuje određivanje na povratnog da bi se izvješćivanje o tijeku prijenos datoteke.

U sljedećem primjeru, ne možemo navedite **nema** mogućnosti resursa.

Dodajte na sljedeći način predmete Program.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Kodiranje izvornu datoteku u skupu prilagodljivo brzina prijenosa MP4 datoteke

Nakon ingesting imovine u Media Services, mogu biti media kodirani, transmuxed i vodenim žigom, itd., prije nego što je isporučen klijente. Te aktivnosti su zakazali i pokrenuti više instanci uloga pozadine da biste bili sigurni visoke performanse i dostupnost. Te aktivnosti nazivaju zadatke, a svaki zadatak se sastojati od atomske zadatke koje obaviti Stvaran rad na datoteci resursa.

Kao što je navedeno ranije, prilikom rada s web-servisa Azure Media Services, jednu od Najčešći scenariji isporučuje prilagodljivu streaming klijentima brzina prijenosa. Media Services možete dinamički pakiranje skup prilagodljivo brzina prijenosa MP4 datoteka u jednu od sljedećih oblika: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo).

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- Kodiranje ili transcode u skupu prilagodljivo brzina prijenosa MP4 datoteke ili datoteke Smooth Streaming prilagodljivo brzina prijenosa datoteka mezzanine (izvor).  
- Pronađite barem jedan strujanje jedinica za strujanje krajnju točku iz koje namjeravate isporuku sadržaja.

Sljedeći kod prikazuje kako slanje kodiranja posla. Posao sadrži jedan zadatak koji određuje da biste transcode mezzanine datoteku u skupu prilagodljivo brzina prijenosa MP4s pomoću **Media Encoder Standard**. Šifra šalje posla i čeka se dovrši.

Nakon dovršetka posla će se moći strujanje sustava resursa ili progresivno preuzimanje datoteka MP4 stvorenih kao rezultat prekodiranje.
Imajte na umu da ne morate imati više od 0 strujanje jedinice da bi se progresivno preuzimanje MP4 datoteke.

Dodajte na sljedeći način predmete Program.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Objavite imovina i Pribavljanje URL-ovi za strujanje i progresivno preuzimanje

Strujanje ili preuzimanje sredstvo, najprije morate "" ga objavite tako da stvorite na locator. Locators omogućuje pristup datotekama koje se nalaze u sredstava. Podržava Media Services dvije vrste locators: OnDemandOrigin locators, koji se koriste za strujanje medijskih sadržaja (na primjer, MPEG CRTICE, HLS ili Smooth Streaming) i locators pristup potpis (SAS) koristiti za preuzimanje medijskih datoteka.

Kada stvorite na locators, možete sastaviti URL-ovi koji se koriste za strujanje ili preuzimanje datoteka.


Strujanje URL-a za Smooth Streaming sastoji se od sljedećih oblika:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Strujanje URL-a za HLS sastoji se od sljedećih oblika:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Strujanje URL-a za MPEG CRTICE sadrži sljedećem obliku:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS URL-a za preuzimanje datoteka sastoji se od sljedećih oblika:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK proširenja pružaju preglednika praktične metode koje vraćaju oblikovani URL-ova za objavljene resursa.

Sljedeći kod koristi .NET SDK proširenja za stvaranje locators i dobiti strujanje i progresivno preuzimanje URL-ova. Kod prikazuje i upute za preuzimanje datoteka u lokalnu mapu.

Dodajte na sljedeći način predmete Program.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Testirajte reproducira sadržaja  

Kad pokrenete program definirano u prethodnom odjeljku, URL-ovi sličnu ovoj prikazat će se u prozoru konzolu.

Prilagodljivo strujanje URL-ova:

Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG CRTICA

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Progresivno preuzimanje URL-ovi (zvuk i videozapis).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Za strujanje videozapisa, koristite [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Da biste testirali progresivno preuzimanje, zalijepite URL u pregledniku (na primjer, Internet Explorer, Chrome ili Safari).


##<a name="next-steps-media-services-learning-paths"></a>Sljedeći koraci: Media Services Tečajevi

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Tražite li nešto drugo?

Ako niste u ovoj se temi sadrže ono što ste očekivali, nema nešto ili u neki drugi način ne zadovoljava vaše potrebe, pošaljite nam s povratne informacije pomoću Disqus niti u nastavku.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
