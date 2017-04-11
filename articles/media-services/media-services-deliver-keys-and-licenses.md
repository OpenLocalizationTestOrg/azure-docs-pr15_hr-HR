<properties 
    pageTitle="Izlaganje DRM licence ili više njih AES pomoću servisa Azure Media Services" 
    description="U ovom se članku opisuje kako pomoću servisa Azure Media Services (AMS) izlaganje PlayReady i/ili Widevine licenci i AES tipke ali potražite ostale (kodiranje šifriranje, strujanje) koristeći lokalne poslužitelje." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="use-azure-media-services-to-deliver-drm-licenses-or-aes-keys"></a>Izlaganje DRM licence ili više njih AES pomoću servisa Azure Media Services

Azure Media Services (AMS) omogućuje ingest kodiranje, dodati zaštitu sadržaja i strujanje sadržaja (pogledajte [članak detalje](media-services-protect-with-drm.md) ). No postoje samo želite koristiti AMS za održavanje licenci i/ili tipke i kodiranje, šifriranje i strujanje pomoću njihovih lokalne poslužitelje korisnike. U ovom se članku opisuje kako koristiti AMS izlaganje PlayReady i/ili Widevine licence, ali ne ostatkom s lokalne poslužitelje. 


## <a name="overview"></a>Pregled

Media Services omogućuje servise za izlaganja PlayReady i Widevine DRM licenci i AES 128 tipke. Media Services i omogućuje API-ji koji možete konfigurirati prava i ograničenja za izvođenje DRM da biste nametnuli kada korisnik reprodukcije u DRM zaštićeni sadržaj. Kad korisnik zatraži zaštićenog sadržaja, aplikacija reproduktora zahtjev licence s AMS servisa za licencu. Servis za licence AMS će izdati licencu reproduktora (Ako je ovlašteni). Licence PlayReady i Widevine sadrže ključ za dešifriranje koje je moguće koristiti Player klijenta za šifriranje i strujanje sadržaj.

Media Services podržava više načina dopuštanja licence ili ključa zahtjeva za korisnike. Konfiguriranje pravila autorizacije sadržaja ključ i pravila može imati jednu ili više ograničenja: otvaranje ili token ograničenja. Znak izdan tako da na sigurne tokena servisa (STS) mora biti praćeni tokena ograničena pravila. Media Services ne podržava tokeni u na jednostavne tokeni Web (SWT) i oblika JSON Web tokena (JWT).


Sljedeći dijagram prikazuje glavne korake koje treba poduzeti da biste koristili AMS izlaganje PlayReady i/ili Widevine licence, ali nemojte ostatkom s lokalne poslužitelje.

![Zaštita s PlayReady](./media/media-services-deliver-keys-and-licenses/media-services-diagram1.png)

##<a name="download-sample"></a>Preuzimanje oglednih

Možete preuzeti ogledne opisane u ovom članku iz [ovdje](https://github.com/Azure/media-services-dotnet-deliver-drm-licenses).

##<a name="net-code-example"></a>Primjer koda .NET

Primjer koda u ovoj temi objašnjava stvaranje ključa uobičajenih sadržaja i PlayReady ili Widevine licence acquisition URL-ova. Morate se sljedeći dijelovi informacija iz AMS i konfiguriranje lokalnog poslužitelja: **sadržaja ključ**, **ključne id**, **licence acquisition URL**. Kada konfigurirate lokalnog poslužitelja, nije moguće strujanje s strujanje poslužitelja. Budući da šifrirane strujanje upućuje na AMS licence poslužitelja, player će zatražiti licence s AMS. Ako odaberete tokena provjeru autentičnosti, licencni poslužitelj AMS će provjeriti token šalje putem HTTP i (Ako je valjan) će isporučiti licencu natrag na player. (Primjer koda prikazuje samo kako stvoriti ključ uobičajenih sadržaja i dobiti PlayReady ili Widevine licence acquisition URL-ova. Ako želite isporuke AES 128 tipke, morate stvoriti ključa sadržaja omotnice i dobiti nabavu ključa URL-a i [u ovom](media-services-protect-with-aes128.md) se članku objašnjava kako to učiniti).
    
    
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
    
    
    namespace DeliverDRMLicenses
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
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                bool tokenRestriction = true;
                string tokenTemplateString = null;
    
    
                IContentKey key = CreateCommonTypeContentKey();
    
                // Print out the key ID and Key in base64 string format
                Console.WriteLine("Created key {0} with key value {1} ", 
                    key.Id, System.Convert.ToBase64String(key.GetClearKeyValue()));
    
                Console.WriteLine("PlayReady License Key delivery URL: {0}", 
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
    
                Console.WriteLine("Widevine License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine));
    
                if (tokenRestriction)
                    tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                else
                    AddOpenAuthorizationPolicy(key);
    
                Console.WriteLine("Added authorization policy: {0}", 
                    key.AuthorizationPolicyId);
                Console.WriteLine();
                Console.ReadLine();
            }
    
            static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
            {
    
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = 
                    new List<ContentKeyAuthorizationPolicyRestriction>
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
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = 
                    new List<ContentKeyAuthorizationPolicyRestriction>
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
    
                //The PlayReadyLicenseResponseTemplate class represents the template 
                //for the response sent back to the end user. 
                //It contains a field for a custom data string between the license server 
                //and the application (may be useful for custom app logic) 
                //as well as a list of one or more license templates.
    
                PlayReadyLicenseResponseTemplate responseTemplate = 
                    new PlayReadyLicenseResponseTemplate();
    
                // The PlayReadyLicenseTemplate class represents a license template 
                // for creating PlayReady licenses
                // to be returned to the end users. 
                // It contains the data on the content key in the license 
                // and any rights or restrictions to be 
                // enforced by the PlayReady DRM runtime when using the content key.
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
    
                // Configure whether the license is persistent 
                // (saved in persistent storage on the client) 
                // or non-persistent (only held in memory while the player is using the license).  
                licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
    
                // AllowTestDevices controls whether test devices can use the license or not.  
                // If true, the MinimumSecurityLevel property of the license
                // is set to 150.  If false (the default), 
                // the MinimumSecurityLevel property of the license is set to 2000.
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
    
                // IMPORTANT: These types of restrictions can be very powerful 
                // but can also affect the consumer experience. 
                // If the output protections are configured too restrictive, 
                // the content might be unplayable on some clients. 
                // For more information, see the PlayReady Compliance Rules document.
    
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
                            required_output_protection = 
                                new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
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
    
    
            static public IContentKey CreateCommonTypeContentKey()
            {
                // Create envelope encryption content key
                Guid keyId = Guid.NewGuid();
                byte[] contentKey = GetRandomBuffer(16);
    
                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);
    
                return key;
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
    

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Vidi također

[Pomoću PlayReady i/ili Widevine dinamičkih uobičajenih šifriranja](media-services-protect-with-drm.md)

[Korištenje AES 128 dinamički šifriranja i servis za isporuku ključa](media-services-protect-with-aes128.md)

[Korištenje partnere isporuka Widevine licenci servisa Azure Media Services](media-services-licenses-partner-integration.md)