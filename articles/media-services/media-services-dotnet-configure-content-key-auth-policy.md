<properties 
    pageTitle="Konfiguriranje pravila sadržaja ključa autorizacije pomoću Media Services .NET SDK | Microsoft Azure" 
    description="Saznajte kako konfigurirati pravilo autorizacije za ključ sadržaja pomoću Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dinamični šifriranja: Konfiguriranje pravila sadržaja autorizacije ključa

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Pregled

Microsoft Azure Media Services omogućuje isporuku MPEG-CRTICE, Smooth Streaming i HTTP-Live-strujeće (HLS) strujanja zaštićena je Napredno šifriranje standardne (AES) (pomoću tipke 128-bitno šifriranje) ili [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS omogućuje i izlaganje CRTICE strujanja šifrirane i zaštićene Widevine DRM. PlayReady i Widevine šifrirane po specifikacija uobičajenih šifriranja (ISO IEC 23001 7 CENC).

Media Services omogućuje i **Servis za isporuku ključ/licence** iz kojeg klijente možete dobiti AES tipke PlayReady/Widevine licenci da biste reproducirali šifrirane sadržaj.

Ako želite Media Services da biste šifrirali sredstvo, morate pridružiti ključa za šifriranje (**CommonEncryption** ili **EnvelopeEncryption**) resursa (kao što je opisano [u nastavku](media-services-dotnet-create-contentkey.md)) i konfigurirati pravila za provjeru autentičnosti za ključ (kao što je opisano u ovom članku).

Strujanje primitku tako da je reproduktor Media Services koristi navedeni ključ za dinamično Šifriraj sadržaj šifriranjem AES ili DRM. Dešifrirati toka reproduktora će zatražiti tipku s servis za isporuku ključa. Odlučiti hoće li se korisnik ovlašteni da biste dobili ključ, servis vrednuje pravila za provjeru autentičnosti koji ste naveli za ključ.

Media Services podržava više načina provjere autentičnosti korisnika za upućivanje zahtjeva za ključa. Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: **Otvaranje** ili **tokena** ograničenja. Znak izdan tako da na sigurne tokena servisa (STS) mora biti praćeni tokena ograničena pravila. Media Services ne podržava tokeni u na **Jednostavne tokeni Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) i oblika **JSON Web tokena** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)).

Media Services ne nudi tokena servisa za sigurnu. Možete stvoriti prilagođeni STS ili pod utjecajem Microsoft Azure ACS da biste tokeni problem. Na STS mora biti konfigurirano za stvaranje tokena prijavljeni pomoću navedeni ključ i problem zahtjevima koji ste naveli u konfiguraciji tokena ograničenja (kao što je opisano u ovom članku). Servis za ključne isporuku Media Services će vratiti ključa za šifriranje klijent Ako vrijedi token i zahtjevima u token podudaraju s dimenzijama i konfiguriran za tipku sadržaja.

Dodatne informacije potražite u članku

[JWT tokena authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrirati Azure Media Services OWIN MVC temelji aplikaciju pomoću servisa Azure Active Directory i ograničavanje isporuku sadržaja ključa koji se temelji na zahtjevima JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Korištenje Azure ACS da biste tokeni problem](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Neke informacije vrijediti:

- Da biste mogli koristiti dinamičke pakiranje i dinamični šifriranje, morate provjeriti da biste imati barem jedno strujanje rezervirane jedinicu. Dodatne informacije potražite u članku [upute za promjenu veličine servis za medijske sadržaje](media-services-portal-manage-streaming-endpoints.md).
- Vaš resursa mora sadržavati skup prilagodljivo brzina prijenosa MP4s ili datoteke Smooth Streaming prilagodljivo brzina prijenosa. Dodatne informacije potražite u članku [Kodiraj sredstava](media-services-encode-asset.md).
- Prijenos i kodiranje svoje imovine pomoću mogućnosti **AssetCreationOptions.StorageEncrypted** .
- Ako namjeravate sadrži više sadržaja tipke koje je potrebno ista pravila konfiguracija preporučuje da biste stvorili pravila jedan autorizacije i ponovno korištenje ključeve više sadržaja.
- Servis za isporuku ključ predmemorira ContentKeyAuthorizationPolicy i njegovih povezanih objekata (mogućnosti pravila i ograničenja) za 15 minuta.  Ako stvorite na ContentKeyAuthorizationPolicy i koristiti "Tokena" ograničenja, testirajte, i ažuriranje pravila "Otvori" ograničenja, trebat će otprilike 15 minuta prije pravila prebacuje na "Otvori" verziju pravila.
- Ako Dodavanje i ažuriranje pravila za isporuku vaše resursa, morate izbrisati postojeće locator (ako ih ima) i stvaranje novog locator.
- Trenutno ne možete šifrirati HDS streaming format ili Progresivni preuzimanja.


##<a name="aes-128-dynamic-encryption"></a>AES 128 dinamički šifriranje

###<a name="open-restriction"></a>Otvaranje ograničenja

Otvaranje ograničenja znači sustav ćete održati tipku svakome tko čini ključa zahtjev. Možda je to ograničenje korisno za testiranje.

Sljedeći primjer stvara pravilo Otvori autorizacije i dodaje tipku sadržaja.

statički javno Poništi AddOpenAuthorizationPolicy (IContentKey contentKey) {/ / stvaranje ContentKeyAuthorizationPolicy uz Otvori ograničenja / / i stvaranje pravila autorizacije pravila IContentKeyAuthorizationPolicy = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("pravila Otvori autorizacije"). Rezultat;

Popis<ContentKeyAuthorizationPolicyRestriction> ograničenja = novi popis<ContentKeyAuthorizationPolicyRestriction>();
    
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


###<a name="token-restriction"></a>Ograničenja tokena

U ovom se odjeljku opisuje kako stvoriti pravilo sadržaja ključa autorizacije i pridružiti tipku sadržaja. Pravila autorizacije opisuje što preduvjeti za autorizaciju mora biti zadovoljen da biste odredili ako korisnik ovlašten primanje ključ (na primjer, ne "potvrdu ključ" popis sadrži ključ koji je potpisan token).

Da biste konfigurirali mogućnost tokena ograničenja, morate koristiti XML opisuje na token autorizacije preduvjeti. Konfiguriranje tokena ograničenje XML mora biti usklađen s XML shemom.

####<a id="schema"></a>Shema tokena ograničenja
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Prilikom konfiguriranja na **tokena** ograničeno pravilnika, morate navesti primarni** ključ za potvrdu**, **izdavač** i **publike** parametre. **Provjera primarni ključ **sadrži ključ koji je potpisan token, **izdavač** je sigurna servis tokena koje izdaje token. **Ciljne skupine** (ponekad se zove i **opseg**) opisuje svrhu tokena ili resursa token neadministratorskog pristup. Servis za ključne isporuku Media Services Provjeri valjanost da te vrijednosti u token podudarati s vrijednostima u predlošku. 

Prilikom korištenja **Media Services SDK za .NET**klase **TokenRestrictionTemplate** možete koristiti za stvaranje tokena ograničenja.
Sljedeći primjer stvara pravilo za provjeru autentičnosti s tokena ograničenja. U ovom primjeru klijentu bit će potrebno izlaganje token koji sadrži: potpisivanje ključ (VerificationKey), tokena izdavača i potrebne zahtjevima.
    
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

####<a id="test"></a>Testiranje tokena

Da biste dobili test tokena na temelju tokena ograničenja koja se koristila za pravila ključa autorizacije, učinite sljedeće:
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Dinamični PlayReady šifriranje 

Media Services omogućuje vam da biste konfigurirali prava i ograničenja koja želite za vrijeme izvođenja PlayReady DRM da biste nametnuli kada korisnik pokušava da biste reproducirali zaštićenog sadržaja. 

Kada zaštita sadržaja pomoću PlayReady neke stvari koje morate odrediti u Pravilnik za autorizaciju je XML niz koji definira [PlayReady licence predložak](media-services-playready-license-template-overview.md). U Media Services SDK za .NET klase **PlayReadyLicenseResponseTemplate** i **PlayReadyLicenseTemplate** će vam pomoći definiranje predlošku PlayReady licence.

[U ovoj se temi](media-services-protect-with-drm.md) objašnjava Šifriraj sadržaj s **PlayReady** i **Widevine**.

###<a name="open-restriction"></a>Otvaranje ograničenja
    
Otvaranje ograničenja znači sustav ćete održati tipku svakome tko čini ključa zahtjev. Možda je to ograničenje korisno za testiranje.

Sljedeći primjer stvara pravilo Otvori autorizacije i dodaje tipku sadržaja.

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
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Ograničenja tokena

Da biste konfigurirali mogućnost tokena ograničenja, morate koristiti XML opisuje na token autorizacije preduvjeti. Konfiguracija tokena ograničenje XML mora biti usklađen s XML shemom prikazani u [ovom](#schema) odjeljku.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
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


Da biste dobili test token tokena ograničenja koja se koristila za ključne provjeru autentičnosti na temelju pravila pročitajte [ovaj](#test) odjeljak. 

##<a id="types"></a>Vrste koje se koriste prilikom definiranja ContentKeyAuthorizationPolicy

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Sljedeći korak
Sad kad ste konfigurirali pravila autorizacije sadržaja ključ, idite na temu [kako konfigurirati pravila za isporuku resursa](media-services-dotnet-configure-asset-delivery-policy.md) .
 
