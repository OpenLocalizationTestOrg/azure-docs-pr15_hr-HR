<properties
    pageTitle="Šifriranjem PlayReady i/ili Widevine dinamički uobičajenih | Microsoft Azure"
    description="Microsoft Azure Media Services omogućuje isporuku MPEG-CRTICE, Smooth Streaming i Http-Live-strujeće (HLS) strujanja zaštićena je Microsoft PlayReady DRM. Također omogućuje za isporuku CRTICE šifrirane i zaštićene Widevine DRM. U ovoj se temi objašnjava dinamički Šifriraj pomoću PlayReady i Widevine DRM."
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
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Šifriranjem PlayReady i/ili Widevine dinamički uobičajenih

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Services omogućuje isporuku MPEG-CRTICE, Smooth Streaming i HTTP-Live-strujeće (HLS) strujanja zaštićena je [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Također omogućuje izlaganje šifrirane CRTICE strujanja s Widevine DRM licence. PlayReady i Widevine šifrirane po specifikacija uobičajenih šifriranja (ISO IEC 23001 7 CENC). Da biste konfigurirali svoje AssetDeliveryConfiguration da biste koristili Widevine možete koristiti [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počevši od verzije 3.5.1) ili REST API-JA.

Media Services omogućuje uslugu za premještanje PlayReady i Widevine DRM licence. Media Services i omogućuje API-ji koji možete konfigurirati prava i ograničenja koja želite za vrijeme izvođenja PlayReady ili Widevine DRM da biste nametnuli kada korisnik reprodukcije zaštićeni sadržaj. Kad korisnik zatraži DRM zaštićeni sadržaj, aplikacija reproduktora zahtjev licence s AMS servisa za licencu. Servis za licence AMS će izdati licence reproduktora ako je ovlašten. Licenca za PlayReady ili Widevine sadrži ključ za dešifriranje koje je moguće koristiti Player klijenta za šifriranje i strujanje sadržaj.


Sljedeće AMS partnera možete koristiti i da biste lakše izlaganje Widevine licenci: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Dodatne informacije potražite u članku: Integracija sa [Axinom](media-services-axinom-integration.md) i [castLabs](media-services-castlabs-integration.md).

Media Services podržava više načina dopuštanja provjerite ključa zahtjeva za korisnike. Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: otvaranje ili token ograničenja. Znak izdan tako da na sigurne tokena servisa (STS) mora biti praćeni tokena ograničena pravila. Media Services ne podržava tokeni u na [Jednostavne tokeni Web](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) i oblika [JSON Web tokena](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Dodatne informacije potražite u članku konfiguriranje pravila autorizacije sadržaja ključ.

Da biste iskoristili dinamički šifriranja, morate koristiti sredstva koja sadrži skup višestruki – brzina prijenosa MP4 datoteke ili više – brzina prijenosa Smooth Streaming izvorne datoteke. Morate konfigurirati pravila isporuke za resursa (što je opisano u nastavku ovog članka). Pa na temelju obliku navedenom u strujanje URL poslužitelja strujanje na zahtjev će osigurati toka se isporučivati u protokol koji ste odabrali. Kao rezultat samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services će Sastavljanje i posluživanje odgovarajuće HTTP odgovor koji se temelji na zahtjev za svaku od klijenta.

U ovoj se temi će biti korisno za razvojne inženjere koji funkcioniraju na aplikacijama koje izlaganje media zaštićena je više DRMs, kao što su PlayReady i Widevine. U temi objašnjava konfiguriranje servisa za isporuku PlayReady licence pravilnike za autorizaciju tako da samo ovlašteni klijenti mogli primati PlayReady ili Widevine licenci. Također se prikazuje kako koristiti šifriranje dinamički šifriranja s PlayReady ili Widevine DRM putem CRTICE.

>[AZURE.NOTE]Da biste počeli koristiti dinamičke šifriranje, najprije morate dobiti najmanje jednu jedinicu mjerilo (poznat i kao strujanje jedinica). Dodatne informacije potražite u članku [upute za promjenu veličine servis za medijske sadržaje](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Preuzimanje oglednih

Možete preuzeti ogledne opisane u ovom članku iz [ovdje](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Konfiguriranje dinamičkih uobičajenih šifriranja i isporukom DRM licence

Slijede općenite korake koje ćete morati izvršiti prilikom zaštite vaše imovine s PlayReady putem servisa za isporuku licence Media Services i koristi dinamički šifriranje.

1. Stvaranje sredstvo i prijenos datoteka u sredstava.
1. Kodiranje resursa koja sadrži datoteku za prilagodljivo brzina prijenosa MP4 postavljanje.
1. Stvaranje sadržaja ključ i pridružiti kodiranih resursa. U Media Services tipku sadržaja sadrži sredstva ključa za šifriranje.
1. Konfiguriranje pravila autorizacije sadržaja ključ. Pravila za sadržaja ključa autorizacije mora biti konfigurirana tako da vam i ispunjen klijenta u redoslijedu tipke sadržaja za isporuku klijentu.

Prilikom stvaranja sadržaja ključa autorizacije pravila, morate navesti sljedeće: isporuke način (PlayReady ili Widevine), ograničenja (Otvori ili tokena), a informacije specifične za vrstu ključa isporuke koja definira kako tipku isporučen klijenta ([PlayReady](media-services-playready-license-template-overview.md) ili [Widevine](media-services-widevine-license-template-overview.md) licence predložak).
1. Konfiguriranje pravilnika isporuke imovine. Konfiguracija pravila za isporuku uključuje: web-mjesto isporuke protokol (na primjer, MPEG CRTICE, HLS, HDS, Smooth Streaming ili sve), u okvir za vrstu dinamički šifriranja (na primjer, uobičajeni šifriranje), PlayReady ili URL za nabavu Widevine licence.

Možete primijeniti različite pravila za svaki protokol na istom resursa. Na primjer, nije moguće primijeniti PlayReady šifriranje da biste prelazili putem glatke/CRTICE i AES omotnicu HLS. Bilo koji protokoli koji su definirani u pravilnik o isporuci (na primjer, dodate jedno pravilo koje samo određuje HLS kao protokol) će blokira strujanje. Iznimka je ako imate resursa pravila isporuke definirani uopće. Nakon toga svi protokoli dozvoljen u isključite.
1. Stvaranje programa locator zahtjev da biste dobili strujanje URL-a.

Tražit će dovršeno primjer .NET na kraju temu.

Sljedeća slika prikazuje prethodno opisan tijeka rada. Ovdje token koristi se za provjeru autentičnosti.

![Zaštita s PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Ostatak u ovoj se temi navode detaljna objašnjenja, primjere koda i veze na temu koja vam pokazati kako postići zadataka na prethodno opisan.

##<a name="current-limitations"></a>Trenutni ograničenja

Ako dodate ili ažurirate pravilo isporuke resursa, morate izbrisati povezane locator (ako ih ima) i stvaranje nove locator.

Ograničenja prilikom šifriranja s Widevine pomoću servisa Azure Media Services: trenutno nisu podržani više tipki sadržaja.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Stvaranje sredstvo i prijenos datoteka u imovine

Da biste mogli upravljati, kodiranje i strujanje videozapisa, prvo morate prenijeti sadržaj u servisa Microsoft Azure Media Services. Nakon prijenosa sadržaj pohranjen sigurno u oblaku za daljnje obrade i strujanje.

Detaljne informacije potražite u članku [Prijenos datoteke na račun servisa Media Services](media-services-dotnet-upload-files.md).

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Kodiranje resursa koja sadrži datoteku za prilagodljivo brzina prijenosa MP4 postavljanje

Dinamični šifriranjem, potreban vam je da biste stvorili sredstva koja sadrži skup višestruki-brzina prijenosa MP4 datoteke ili više – brzina prijenosa Smooth Streaming izvorne datoteke. Nakon toga na temelju navedenom obliku u zahtjevu za manifest i fragment poslužitelj za strujanje na zahtjev će osigurati primaju strujanje u protokol koji ste odabrali. Kao rezultat samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i servis Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta. Dodatne informacije u sintaksi [Dinamički pakiranje pregled](media-services-dynamic-packaging-overview.md) .

Upute za kodiranje, potražite [u](media-services-dotnet-encode-with-media-encoder-standard.md)članku kodiranje sredstvo pomoću standardnih kodiranje medijskih sadržaja.


##<a id="create_contentkey"></a>Stvaranje sadržaja ključ i povezivanje s kodiranih resursa

U Media Services tipku sadržaja sadrži ključ koji želite šifrirati imovine s.

Detaljne informacije potražite u članku [Stvaranje sadržaja ključ](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>Konfiguriranje pravila autorizacije sadržaja ključ

Media Services podržava više načina provjere autentičnosti korisnika za upućivanje zahtjeva za ključa. Pravila za sadržaja ključa autorizacije mora biti konfigurirana tako da vam i ispunjen klijenta (player) da bi ključ za isporuku klijentu. Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: otvaranje ili token ograničenja.

Detaljne informacije potražite u članku [Konfiguriranje sadržaja ključ pravila autorizacije](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

##<a id="configure_asset_delivery_policy"></a>Konfiguriranje pravilnika za isporuku resursa 

Konfiguriranje pravila isporuke za vaše resursa. Razlozi zbog kojih konfiguracija pravila za isporuku resursa obuhvaća:

- URL acquisition DRM licence. 
- Isporuka protokol resursa (na primjer, MPEG CRTICE, HLS, HDS, Smooth Streaming ili sve). 
- Vrsta dinamički šifriranja (u ovom slučaju uobičajenih šifriranje). 

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

##<a id="example"></a>Primjer


Sljedeći primjer pokazuje funkcionalnost koja je uvedena u Azure Media Services SDK za .net-verzije 3.5.2 (Konkretno, mogućnost da biste definirali predloška Widevine licence i zahtjev licence Widevine od servisa Azure Media Services). Sljedeća naredba paket Nuget koristila da biste instalirali paket:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Stvorite novi projekt konzolu.
1. Da biste instalirali i dodavanje Azure Media Services .NET SDK pomoću NuGet.
2. Dodavanje dodatnih reference: System.Configuration.
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

1. Pronađite barem jedan strujanje jedinica za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite u članku: [Konfiguriranje strujanje krajnje točke](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Kod u datoteci Program.cs prebrisati kôda prikazanog u ovom odjeljku.
    
    Provjerite je li za ažuriranje varijable tako da pokazuje na mape u kojoj se nalaze ulaznih datoteka.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
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
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
        
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


## <a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Vidi također

[CENC s više DRM i kontrola pristupa](media-services-cenc-with-multidrm-access-control.md)

[Konfiguriranje pakiranje Widevine s AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Objavljivanje Google Widevine licence isporukom u servisa Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
