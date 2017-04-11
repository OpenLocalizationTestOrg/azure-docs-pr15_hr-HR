<properties
    pageTitle="Korištenje AES 128 dinamički šifriranja i servis za isporuku ključa | Microsoft Azure"
    description="Servisa Microsoft Azure Media Services omogućuje isporuku sadržaja šifrirane i zaštićene AES 128-bitno ključeva za šifriranje. Media Services nudi i servis za isporuku ključ koji nudi ključeva za šifriranje ovlaštenim korisnicima. U ovoj se temi objašnjava dinamički Šifriraj pomoću AES 128 i korištenja servisa za ključne isporuke."
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

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Korištenje AES 128 dinamički šifriranja i servis za isporuku ključa

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Pregled

>[AZURE.NOTE] Pogledajte [ovaj](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) videozapis da biste saznali kako zaštititi svoje medijskih sadržaja s AES šifriranje.

Microsoft Azure Media Services omogućuje Http-Live-strujanje (HLS) i izglađenim strujanja šifrirane s Napredno šifriranje standardne (AES) (pomoću tipke 128-bitno šifriranje). Media Services nudi i servis za isporuku ključ koji nudi ključeva za šifriranje ovlaštenim korisnicima. Ako želite Media Services da biste šifrirali sredstvo, morate pridružiti ključa za šifriranje imovine i konfigurirati pravila za provjeru autentičnosti tipke. Strujanje primitku tako da je reproduktor Media Services koristi navedeni ključ za dinamično Šifriraj sadržaj koristi AES šifriranje. Dešifrirati toka reproduktora će zatražiti tipku s servis za isporuku ključa. Odlučite li korisnik ovlašten da biste dobili tipku servis vrednuje pravila za provjeru autentičnosti koji ste naveli za ključ.

Media Services podržava više načina provjere autentičnosti korisnika za upućivanje zahtjeva za ključa. Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: otvaranje ili token ograničenja. Znak izdan tako da na sigurne tokena servisa (STS) mora biti praćeni tokena ograničena pravila. Media Services ne podržava tokeni u na [Jednostavne tokeni Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) i oblika [JSON Web tokena](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Dodatne informacije potražite u članku [Konfiguriranje pravila autorizacije sadržaja ključ](media-services-protect-with-aes128.md#configure_key_auth_policy).

Da biste iskoristili dinamički šifriranja, morate koristiti sredstva koja sadrži skup višestruki-brzina prijenosa MP4 datoteke ili više – brzina prijenosa Smooth Streaming izvorne datoteke. Morate konfigurirati pravila isporuke za resursa (što je opisano u nastavku ovog članka). Pa na temelju obliku navedenom u strujanje URL poslužitelja strujanje na zahtjev će osigurati toka se isporučivati u protokol koji ste odabrali. Kao rezultat samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i servis Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta.

U ovoj se temi će biti korisno razvojni inženjeri koji rade na aplikacije koje izlaganje zaštićeni medijske sadržaje. U temi objašnjava konfiguriranje servisa za ključne isporuke s pravilima za provjeru autentičnosti tako da samo ovlašteni klijenti može primati ključeve za šifriranje. Također se prikazuje kako koristiti dinamičke šifriranje.

>[AZURE.NOTE]Da biste počeli koristiti dinamičke šifriranje, najprije morate dobiti najmanje jednu jedinicu mjerilo (poznat i kao strujanje jedinica). Dodatne informacije potražite u članku [upute za promjenu veličine servis za medijske sadržaje](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Dinamični šifriranje AES 128 i ključne isporuke servisa tijek rada

Slijede općenite korake koje ćete morati izvršiti prilikom šifriranja svoje imovine s AES putem servisa za ključne isporuke Media Services i koristi dinamički šifriranje.

1. [Stvaranje sredstvo i prijenos datoteka u sredstava](media-services-protect-with-aes128.md#create_asset).
1. [Kodiraj resursa koja sadrži datoteku za prilagodljivo brzina prijenosa MP4 postavljanje](media-services-protect-with-aes128.md#encode_asset).
1. [Stvaranje sadržaja ključa i povezati je s kodiranih resursa](media-services-protect-with-aes128.md#create_contentkey). U Media Services tipku sadržaja sadrži sredstva ključa za šifriranje.
1. [Konfiguriranje pravila autorizacije sadržaja ključ](media-services-protect-with-aes128.md#configure_key_auth_policy). Pravila za sadržaja ključa autorizacije mora biti konfigurirana tako da vam i ispunjen klijenta u redoslijedu tipke sadržaja za isporuku klijentu.
1. [Konfiguriranje pravilnika isporuke imovine](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Konfiguracija pravila za isporuku uključuje: ključnog URL za nabavu i vektorski inicijalizaciju (IV) (AES 128 zahtijeva isti IV na navedeni kada šifriranje i dešifriranje), web-mjesto isporuke protokol (na primjer, MPEG CRTICE, HLS, HDS, Smooth Streaming ili sve), u okvir za vrstu dinamički šifriranja (Ako, na primjer, omotnice ili nema dinamički šifriranja).

Možete primijeniti različite pravila za svaki protokol na istom resursa. Na primjer, nije moguće primijeniti PlayReady šifriranje da biste prelazili putem glatke/CRTICE i AES omotnicu HLS. Bilo koji protokoli koji su definirani u pravilnik o isporuci (na primjer, dodate jedno pravilo koje samo određuje HLS kao protokol) će blokira strujanje. Iznimka je ako imate resursa pravila isporuke definirani uopće. Nakon toga svi protokoli dozvoljen u isključite.

1. [Stvaranje programa locator zahtjev](media-services-protect-with-aes128.md#create_locator) da biste dobili strujanje URL-a.

U temi također prikazuje [kako klijentska aplikacija možete zatražiti ključ iz servisa ključa isporuke](media-services-protect-with-aes128.md#client_request).

Tražit će dovršeno.NET [primjer](media-services-protect-with-aes128.md#example) na kraju temu.

Sljedeća slika prikazuje prethodno opisan tijeka rada. Ovdje token koristi se za provjeru autentičnosti.

![Zaštita s AES 128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Ostatak u ovoj se temi navode detaljna objašnjenja, primjere koda i veze na temu koja vam pokazati kako postići zadaci na prethodno opisan.

##<a name="current-limitations"></a>Trenutni ograničenja

Ako Dodavanje i ažuriranje pravila za isporuku vaše resursa, morate izbrisati postojeće locator (ako ih ima) i stvaranje novog locator.

##<a id="create_asset"></a>Stvaranje sredstvo i prijenos datoteka u imovine

Da biste mogli upravljati, kodiranje i strujanje videozapisa, prvo morate prenijeti sadržaj u servisa Microsoft Azure Media Services. Nakon prijenosa sadržaj pohranjen sigurno u oblaku za daljnje obrade i strujanje. 

Detaljne informacije potražite u članku [Prijenos datoteke na račun servisa Media Services](media-services-dotnet-upload-files.md).

##<a id="encode_asset"></a>Kodiranje resursa koja sadrži datoteku za prilagodljivo brzina prijenosa MP4 postavljanje

Dinamični šifriranjem, potreban vam je da biste stvorili sredstva koja sadrži skup višestruki-brzina prijenosa MP4 datoteke ili više – brzina prijenosa Smooth Streaming izvorne datoteke. Nakon toga navedenom obliku u manifest ili fragment zahtjevu za strujanje na zahtjev na temelju poslužitelja će osigurati primaju strujanje u protokol koji ste odabrali. Kao rezultat samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i servis Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta. Dodatne informacije u sintaksi [Dinamički pakiranje pregled](media-services-dynamic-packaging-overview.md) .

Upute za kodiranje, potražite [u](media-services-dotnet-encode-with-media-encoder-standard.md)članku kodiranje sredstvo pomoću standardnih kodiranje medijskih sadržaja.

##<a id="create_contentkey"></a>Stvaranje sadržaja ključ i povezivanje s kodiranih resursa

U Media Services tipku sadržaja sadrži ključ koji želite šifrirati imovine s.

Detaljne informacije potražite u članku [Stvaranje sadržaja ključ](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>Konfiguriranje pravila autorizacije sadržaja ključ

Media Services podržava više načina provjere autentičnosti korisnika za upućivanje zahtjeva za ključa. Pravila za sadržaja ključa autorizacije mora biti konfigurirana tako da vam i ispunjen klijenta (player) da bi ključ za isporuku klijentu. Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: otvaranje, token ograničenja ili IP ograničenja.

Detaljne informacije potražite u članku [Konfiguriranje sadržaja ključ pravila autorizacije](media-services-dotnet-configure-content-key-auth-policy.md).

##<a id="configure_asset_delivery_policy"></a>Konfiguriranje pravilnika za isporuku resursa 

Konfiguriranje pravila isporuke za vaše resursa. Razlozi zbog kojih konfiguracija pravila za isporuku resursa obuhvaća:

- URL za nabavu ključa. 
- Na Inicijalizacija vektorski (IV) da biste koristili za šifriranje omotnice. AES 128 zahtijeva isti IV na navedeni kada šifriranje i dešifriranje. 
- Isporuka protokol resursa (na primjer, MPEG CRTICE, HLS, HDS, Smooth Streaming ili sve).
- Vrsta dinamički šifriranja (na primjer, AES omotnica) ili nema dinamički šifriranja. 

Detaljne informacije potražite u članku [Konfiguriranje resursa isporuke pravila ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Stvaranje programa zahtjev strujanje locator da biste dobili strujanje URL-a

Morat ćete navesti korisničko strujanje URL-om za bila glatka, CRTICE ili HLS.

>[AZURE.NOTE]Ako Dodavanje i ažuriranje pravila za isporuku vaše resursa, morate izbrisati postojeće locator (ako ih ima) i stvaranje novog locator.

Upute za objavljivanje imovina i stvaranje strujanje URL-a, potražite u članku [Sastavljanje strujanje URL-a](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>Početak testiranja tokena

Početak testiranja tokena na temelju tokena ograničenja koja se koristila za pravila ključa autorizacije.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

[Reproduktor AMS](http://amsplayer.azurewebsites.net/azuremediaplayer.html) možete koristiti za testiranje sustava strujanje.

##<a id="client_request"></a>Kako mogu klijent zahtjev ključa od servis za ključne isporuku?

U prethodnom koraku konstruirana URL-a koji vodi do manifesta datoteke. Klijent treba izdvojiti potrebne podatke iz strujanje manifesta datoteka da biste omogućili zahtjeva za uslugu ključa isporuke.

###<a name="manifest-files"></a>Manifest datoteka

Klijent treba izdvojiti URL (koji sadrži i sadržaja ključ Id (dijete)) vrijednosti iz manifesta datoteke. Klijent zatim pokušate usluge ključa isporuke dobiti ključa za šifriranje. Klijent i treba izdvojiti IV vrijednost i korištenje ga dešifriranje toka. U sljedećem isječak u <Protection> element manifesta Smooth Streaming.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

U slučaju HLS, manifest korijenski dolazi do oštećenja u segmentu datoteke. 

Na primjer, je manifest korijenski: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) i nalazi se popis naziva datoteka segmenta.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Ako otvorite neku od datoteka segmenta u uređivaču teksta (na primjer, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), ona mora sadržavati #EXT X-ključa koja pokazuje da datoteka je šifrirana.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Zahtjev za tipku od servis za isporuku ključa

Sljedeći kod prikazuje kako pošaljite zahtjev za servis za isporuku tipki na Media Services pomoću ključa isporuke Uri (koji je izvedena iz manifest) i token (u ovoj se temi nisu objasniti kako jednostavno tokeni Web iz sigurne tokena servisa).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Primjer

1. Stvorite novi projekt konzolu.
1. Da biste instalirali i dodavanje Azure Media Services .NET SDK proširenja pomoću NuGet. Instalirate paket, instalira Media Services .NET SDK i dodaje ostale potrebne ovisnosti.
2. Dodajte konfiguracijska datoteka koja sadrži naziv računa i informacija o ključu:

    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. Kod u datoteci Program.cs prebrisati kôda prikazanog u ovom odjeljku.
    
    Provjerite je li za ažuriranje varijable tako da pokazuje na mape u kojoj se nalaze ulaznih datoteka.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    // In this case "H264 Multiple Bitrate 720p" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        TaskOptions.None);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "HLS Open Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null // no requirements needed for HLS
                                };
        
                    restrictions.Add(restriction);
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                        "policy",
                        ContentKeyDeliveryType.BaselineHttp,
                        restrictions,
                        "");
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                            new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                            new ContentKeyAuthorizationPolicyRestriction
                            {
                                Name = "Token Authorization Policy",
                                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                Requirements = tokenTemplateString
                            };
        
                    restrictions.Add(restriction);
        
                    //You could have multiple options 
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                            "Token option for HLS",
                            ContentKeyDeliveryType.BaselineHttp,
                            restrictions,
                            null  // no key delivery data is needed for HLS
                            );
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        
                    return tokenTemplateString;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
        
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
