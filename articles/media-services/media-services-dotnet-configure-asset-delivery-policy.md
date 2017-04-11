<properties 
    pageTitle="Konfiguriranje pravilnika za isporuku resursa s .NET SDK | Microsoft Azure" 
    description="U ovoj se temi objašnjava konfiguriranje pravilnika za isporuku različite resursa s Azure Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>Konfiguriranje pravilnika za isporuku resursa s .NET SDK
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Pregled

Ako planirate isporuke šifrirane resursi, jedan od koraka u tijeku rada za isporuku sadržaja Media Services konfigurira isporuke pravila za resursi. Pravilnik za isporuku resursa govori Media Services način za vaše resursa za isporuku: u koje strujanje protokol treba vaše resursa dinamički pakirati (na primjer, MPEG CRTICE, HLS, Smooth Streaming ili sve), hoće li želite dinamički šifriranje vaše resursa i kako (omotnice ili uobičajenih šifriranje).

U ovoj se temi objašnjava zašto te upute za stvaranje i konfiguriranje pravilnika o isporuci resursa.

>[AZURE.NOTE]Da biste mogli koristiti dinamičke pakiranje i dinamični šifriranje, morate provjeriti imati najmanje jednu jedinicu mjerilo (poznat i kao strujanje jedinica). Dodatne informacije potražite u članku [upute za promjenu veličine servis za medijske sadržaje](media-services-portal-manage-streaming-endpoints.md).
>
>Osim toga, vaš resursa mora sadržavati skup prilagodljivo brzina prijenosa MP4s ili datoteke Smooth Streaming prilagodljivo brzina prijenosa.

Da biste isti resursa možete primijeniti različita pravila. Ako, na primjer, nije moguće primijenite PlayReady šifriranje Smooth Streaming i omotnica AES šifriranje MPEG CRTICE i HLS. Bilo koji protokoli koji su definirani u pravilnik o isporuci (na primjer, dodate jedno pravilo koje samo određuje HLS kao protokol) će blokira strujanje. Iznimka je ako imate resursa pravila isporuke definirani uopće. Nakon toga svi protokoli dozvoljen u isključite.

Ako želite izlaganje resursa za šifriranu prostora za pohranu, morate konfigurirati pravila za isporuku sredstava. Prije moguće je strujanjem vaše resursa, strujanje poslužitelja uklanja šifriranje prostora za pohranu i strujanja putem pravila za isporuku navedeni sadržaj. Ako, na primjer, prilikom izlaganja vaš resursa šifrirane ključem za šifriranje omotnica Napredno šifriranje standardne (AES), postaviti vrstu pravila **DynamicEnvelopeEncryption**. Da biste uklonili šifriranje prostora za pohranu i strujanje resursa u isključite, postaviti vrstu pravila na **NoDynamicEncryption**. Primjeri koji pokazuju kako konfigurirati ove vrste pravila slijedite.

Ovisno o kako konfigurirati pravila za isporuku resursa moći dinamički pakiranje, dinamički šifriranje i strujanje sljedećih protokola za strujanje: Smooth Streaming HLS, MPEG CRTICE te HDS strujanja.

Na sljedećem popisu navedene u formate koje koristite za strujanje bila glatka, HLS, crta i HDS.

Smooth Streaming:

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG CRTICA

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Upute za objavljivanje imovina i stvaranje strujanje URL-a, potražite u članku [Sastavljanje strujanje URL-a](media-services-deliver-streaming-content.md).

##<a name="considerations"></a>Razmatranja

- Nije moguće izbrisati programa AssetDeliveryPolicy pridružene sredstvo dok je locator zahtjev (strujanje) postoji za tog resursa. Na preporuka je da biste uklonili pravila imovine prije brisanja pravila.
- Strujanje Lokator nije moguće stvoriti na pohranu resursa za šifriranu kada nema pravila za isporuku resursa postavljen.  Ako sredstva ne šifrirane prostora za pohranu, u sustavu omogućuju stvaranje na locator i strujanje resursa u Očisti bez pravilo isporuke resursa.
- Imate više pravila isporuke resursa povezan s jednom resursa, ali možete navesti samo jedan od načina rukovanja navedeni AssetDeliveryProtocol.  Što znači ako pokušate povezati dva isporuke pravila navedite protokol AssetDeliveryProtocol.SmoothStreaming koji će uzrokovati pogrešku jer je sustav znati koje jedan koji želite primijeniti kada klijentsko čini Smooth Streaming zahtjev.
- Ako imate imovine s postojećeg strujanje locator, ne možete povezati novog pravilnika imovine (možete prekinuti vezu postojeće pravilo iz sredstvo, ili ažuriranje pridruženo imovine pravilo za isporuku).  Najprije morate uklonili strujanje locator, prilagodite pravila, a zatim ponovno stvorite strujanje locator.  Možete koristiti isti locatorId kada ponovno stvorite strujanje locator, ali potrebno je provjeriti koji se neće izazvati probleme sa za klijente jer sadržaj mogu predmemorirane izvor ili do CDN.


##<a name="clear-asset-delivery-policy"></a>Pravila za isporuku Očisti resursa

Određuje sljedeću metodu **ConfigureClearAssetDeliveryPolicy** ne primjenjuju se dinamički šifriranja i izlaganje toka u nekom od sljedećih protokola: MPEG CRTICE, HLS i Smooth Streaming protokola. Možda želite primijeniti ovo pravilo za svoje imovine šifrirane prostora za pohranu.

Informacije o onih vrijednosti koje možete odrediti kada stvarate programa AssetDeliveryPolicy, potražite u odjeljku [vrste koristi prilikom definiranja AssetDeliveryPolicy](#types) .

statički javno Poništi ConfigureClearAssetDeliveryPolicy (IAsset resursa) {IAssetDeliveryPolicy pravila = _context. AssetDeliveryPolicies.Create ("Očisti pravila", AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

resursa. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Pravila za isporuku DynamicCommonEncryption resursa


Sljedeći način **CreateAssetDeliveryPolicy** stvara **AssetDeliveryPolicy** koji je konfiguriran za primjenu dinamički uobičajenih šifriranja (**DynamicCommonEncryption**) izglađenim strujanje protokola (drugi protokoli bit će blokirane iz strujanje). Način vodi dva parametra: **resursa** (resursa na koji želite primijeniti Pravilnik za isporuku) i **IContentKey** (sadržaja tipku vrste **CommonEncryption** , dodatne informacije potražite u članku: [Stvaranje sadržaja ključa](media-services-dotnet-create-contentkey.md#common_contentkey)).

Informacije o onih vrijednosti koje možete odrediti kada stvarate programa AssetDeliveryPolicy, potražite u odjeljku [vrste koristi prilikom definiranja AssetDeliveryPolicy](#types) .


statički javno CreateAssetDeliveryPolicy Poništi (IAsset resursa, IContentKey ključa) {Uri acquisitionUrl = ključ. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

AssetDeliveryPolicyConfiguration rječnik < AssetDeliveryPolicyConfigurationKey, niz > novi rječnik < AssetDeliveryPolicyConfigurationKey niz > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services omogućuje vam da biste dodali Widevine šifriranje. Sljedeći primjer prikazuje PlayReady i Widevine dodat pravila za isporuku resursa.


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
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Prilikom šifriranja s Widevine, samo će moći izlaganje pomoću CRTICE. Provjerite je li potrebno navesti crtica u protokol za isporuku resursa.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Pravila za isporuku DynamicEnvelopeEncryption resursa 

Sljedeću metodu **CreateAssetDeliveryPolicy** stvara **AssetDeliveryPolicy** koji je konfiguriran da biste primijenili dinamički omotnica šifriranja (**DynamicEnvelopeEncryption**) protokoli Smooth Streaming, HLS i CRTICU (Ako odlučite da ne navedete neki protokoli, oni će biti blokira strujanje). Način vodi dva parametra: **resursa** (resursa na koji želite primijeniti Pravilnik za isporuku) i **IContentKey** (sadržaja tipku vrste **EnvelopeEncryption** , dodatne informacije potražite u članku: [Stvaranje sadržaja ključa](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Informacije o onih vrijednosti koje možete odrediti kada stvarate programa AssetDeliveryPolicy, potražite u odjeljku [vrste koristi prilikom definiranja AssetDeliveryPolicy](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
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
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Vrste koje se koriste prilikom definiranja AssetDeliveryPolicy

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


