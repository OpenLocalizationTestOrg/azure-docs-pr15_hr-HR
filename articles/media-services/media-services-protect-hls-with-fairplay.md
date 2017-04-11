<properties 
    pageTitle="Zaštita vaše HLS sadržaja s Apple FairPlay i/ili Microsoft PlayReady | Microsoft Azure" 
    description="U ovoj se temi omogućuje pregled i prikazuje kako dinamično šifriranje sadržaja HTTP Live strujanje (HLS) pomoću Apple FairPlay pomoću servisa Azure Media Services. Također se prikazuje kako koristiti servis za isporuku Media Services licencu za isporuku FairPlay licence za klijente." 
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
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Zaštita vaše HLS sadržaja s Apple FairPlay i/ili Microsoft PlayReady

Azure Media Services omogućuje dinamički Šifriraj sadržaj HTTP Live strujanje (HLS) pomoću sljedećih oblika:  

- **Očisti ključ AES 128 omotnice** 

    Cijeli bloka je šifriran pomoću **AES 128 CBC** način. Dešifriranje toka podržava iOS i OSX reproduktora nativno. Dodatne informacije potražite u [ovom članku](media-services-protect-with-aes128.md).

- **Apple FairPlay** 

    Pojedinačne uzoraka videosadržaj i audiosadržaj šifrirane načinu **AES 128 CBC** . **FairPlay strujanje** Operacijski sustavi uređaj s podrškom za izvorni sustavima iOS i Apple TV integriranom (FPS). Safari operacijskom sustavu OS X omogućuje FPS pomoću podršku sučelja šifrirane Media proširenja (EME).
- **Microsoft PlayReady**

Sljedeća slika prikazuje tijek rada **HLS + FairPlay i/ili PlayReady dinamički šifriranje** .

![Zaštita s FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

U ovoj se temi objašnjava kako pomoću servisa Azure Media Services dinamički šifriranje HLS sadržaja pomoću Apple FairPlay. Također se prikazuje kako koristiti servis za isporuku Media Services licencu za isporuku FairPlay licence za klijente.

>[AZURE.NOTE] Ako želite šifrirati HLS sadržaja pomoću PlayReady, morate stvoriti uobičajenih sadržaja ključ i povezati je s vašeg resursa. Morate konfigurirati pravila autorizacije ključ sadržaja, kao što je opisano u temi [pomoću PlayReady dinamički uobičajenih šifriranje](media-services-protect-with-drm.md) .

    
## <a name="requirements-and-considerations"></a>Preduvjeti i napomene

- U nastavku su potrebne prilikom korištenja AMS izlaganje HLS šifrirane i zaštićene FairPlay i izlaganje FairPlay licence.

    - Račun Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Na račun servisa Media Services. Stvaranje računa Media Services potražite u članku [Stvaranje računa](media-services-portal-create-account.md).
    - Prijavite se pomoću [Apple razvoj programa](https://developer.apple.com/).
    - Apple potreban je vlasnik sadržaja da biste dobili [paketa za implementaciju](https://developer.apple.com/contact/fps/). Navedite zahtjev već implementirana KSM (ključ sigurnosnom modulu) pomoću servisa Azure Media Services i koje ste zatražili konačni FPS paketa. Pojavit će se upute u paketu za stvaranje certifikata i dobiti ASK koje ćete koristiti da biste konfigurirali FairPlay konačni FPS. 

    - Azure Media Services .NET SDK verziju **3.6.0** ili noviji.

- Sljedeći elementi mora biti postavljeno na strani AMS ključa isporuke:
    - **Aplikacija certifikata (AC)** – .pfx datoteka koja sadrži privatni ključ. Datoteka je stvorena koje je korisnik odabrao te vlasnik isti kupac šifrirane lozinkom. 
        
        Kada je korisnik odabrao konfigurira pravila ključa isporuke, ih morate unijeti lozinku i .pfx u obliku base64.

        Sljedeći koraci opisuju kako generiranje pfx certifikat za FairPlay.
        
        1. Instalirajte OpenSSL https://slproweb.com/products/Win32OpenSSL.html
        
            Otvorite mapu u kojoj se certifikat FairPlay i druge datoteke isporučuju drugi Apple.
        
        2. Da biste pretvorili u cer pem naredbeni redak:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-obavijestili der-u fairplay.cer-out fairplay out.pem
        
        3. Naredbenog retka za pretvaranje pem pfx s privatni ključ (lozinke za datoteku pfx je pa od vas zatraži, OpenSSL).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-izvoz - out fairplay out.pfx-inkey privatekey.pem-u fairplay out.pem - passin file:privatekey-pem-pass.txt 
        
    - **Lozinka certifikata aplikacije** - kupca lozinku za stvaranje .pfx datoteka.
    - **ID aplikacije certifikata lozinke** – klijenta mora prijenos slične kako prenijeti ostale tipke za AMS i korištenje redni broj **ContentKeyType.FairPlayPfxPassword** lozinku. Kao rezultat, prikazat će im se AMS ID-a to je koje su im potrebne za korištenje unutar mogućnost pravila ključa isporuke.
    - **iv** - 16 bajta slučajnu vrijednost se mora podudarati iv pravila za isporuku resursa. Korisnička generira na IV i stavlja na oba mjesta: resursa isporuke pravila i ključ isporuke pravila mogućnost. 
    - **Postavljanje PITANJA** – ASK (aplikaciji tajna ključ) dobijete kada generiranje certifikata pomoću portala za razvojne inženjere Apple. Svaki tim za razvoj primit će jedinstveni ASK. Spremite kopiju na ASK i spremite ga na sigurnom mjestu. Morat ćete konfigurirati ASK kao FairPlayAsk za servisa Azure Media Services kasnije. 
    -  **ZAMOLITE ID** – dobiva se kada je korisnik odabrao prenosi ASk u AMS. Klijent morate prenijeti ASk pomoću redni broj **ContentKeyType.FairPlayASk** . Kao rezultat, AMS ID-a, vraća se, a to je što treba koristiti prilikom postavljanja pravilnika mogućnost ključa isporuke.

- Usporedno klijent FPS mora biti postavljeno sljedeće:
    - **Aplikacija certifikata (AC)** –.cer/.der datoteka koja sadrži javni ključ koji za šifriranje neke tereta koristi OS. AMS treba znati o njemu jer je potrebnih reproduktora. Servis za ključne isporuku dešifrira pomoću odgovarajući privatni ključ.

- Da biste reprodukcije šifrirane strujanje FairPlay, morate dobiti realni ASK prvi i generiranje realni certifikat. Taj postupak stvara svi dijelovi 3:

    -  .der, 
    -  .pfx i 
    -  lozinka na .pfx.
 
- Klijenti koji podržavaju HLS šifriranjem **AES 128 CBC** : Safari sustavu OS X, Apple TV iOS.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Upute za konfiguriranje FairPlay dinamički šifriranja i licence isporukom

Slijede općenite korake koje ćete morati izvršiti prilikom zaštite vaše imovine s FairPlay putem servisa za isporuku licence Media Services i koristi dinamički šifriranje.

1. Stvaranje sredstvo i prijenos datoteka u sredstava. 
1. Kodiranje resursa koja sadrži datoteku za prilagodljivo brzina prijenosa MP4 postavljanje.
1. Stvaranje sadržaja ključ i pridružiti kodiranih resursa.  
1. Konfiguriranje pravila autorizacije sadržaja ključ. Prilikom stvaranja pravila za sadržaja ključa autorizacije morate odrediti sljedeće: 
    
    - Način isporuke (u ovom slučaju, FairPlay) 
    - Konfiguracija mogućnosti pravila za FairPlay. Informacije o konfiguriranju FairPlay potražite u članku način ConfigureFairPlayPolicyOptions() u primjeru u nastavku.
    
        >[AZURE.NOTE] Obično činite želite konfigurirati mogućnosti pravila FairPlay samo jedanput, jer će imati samo jedan skup certifikata i ASK.
-ograničenja (Otvori ili tokena) – i informacije specifične za vrstu ključa isporuke koja definira kako se isporučuje tipku klijentu. 
    
2. Konfiguriranje pravilnika za isporuku resursa. Konfiguracija pravila za isporuku obuhvaća: 

    - protokol isporuke (HLS) 
    - Vrsta dinamički šifriranja (uobičajenih CBC šifriranje), 
    - URL za nabavu licence. 
    
    >[AZURE.NOTE]Ako želite izlaganje strujanje šifriranom pomoću FairPlay + drugi DRM, morate konfiguriranje pravilnika o zasebnom isporuke 
    >
    >- Jedan IAssetDeliveryPolicy da biste konfigurirali CRTICE CENC (PlayReady + WideVine) i prelazili putem glatke s PlayReady. 
    >- Drugi IAssetDeliveryPolicy da biste konfigurirali FairPlay za HLS

1. Stvaranje programa locator zahtjev da biste dobili strujanje URL-a.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>Pomoću tipki isporuke FairPlay reproduktora/klijentskim aplikacijama

Korisnici ne može razvijajte aplikacije reproduktora pomoću iOS SDK. Da bi se moći reproducirati sadržaj FairPlay klijenti imaju implementirati protokol licencu sustava exchange. Protokol licencu sustava exchange nije naveden, Apple. Je najviše svaku aplikaciju slanju zahtjeva za ključne isporuke. AMS FairPlay ključa isporukom očekuje SPC dolazi kao www-form-url kodiranih objave poruke u sljedećem obliku: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player ne podržava FairPlay reprodukcije iz okvir. Klijenti dobro da nabavite reproduktora uzorka s računa za razvojne inženjere Apple da biste dobili FairPlay reprodukcije MAC OSX. 
 
##<a name="streaming-urls"></a>Strujanje URL-ova

Ako je vaš resursa šifrirana s više od jedne DRM, koristite oznaku šifriranje u strujanje URL-: (oblik = 'm3u8-aapl' šifriranje = "xxx").

Primjena Imajte na umu sljedeće:

- Moguće je navesti samo nula ili jednu vrstu šifriranja.
- Vrsta šifriranja nema da bude navedena u URL-a ako samo jedan šifriranje je primijenjena na imovinu.
- Vrsta šifriranja je slova.
- Možete navesti sljedeće vrste šifriranja:  
    - **cenc**: uobičajene šifriranja (Playready ili Widevine)
    - **cbcs aapl**: Fairplay
    - **cbc**: AES šifriranje omotnice.


##<a name="net-example"></a>Primjer .NET


Sljedeći primjer pokazuje funkcionalnost koja je uvedena u Azure Media Services SDK za .net-verzije 3.6.0 (mogućnost korištenja servisa Azure Media Services za isporuku sadržaja šifrirane i zaštićene FairPlay). Sljedeća naredba paket Nuget koristila da biste instalirali paket:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Stvaranje projekta konzolu.
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
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Sljedeći koraci: Media Services Tečajevi

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
