<properties 
    pageTitle="Konfiguriranje pravilnika za isporuku resursa pomoću Media Services REST API-JA | Microsoft Azure" 
    description="U ovoj se temi objašnjava konfiguriranje pravilnika o isporuci različite resursa pomoću Media Services REST API-JA." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Konfiguriranje pravilnika o isporuci resursa

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Ako namjeravate održati dinamički šifrirane resursi, jedan od koraka u tijeku rada za isporuku sadržaja Media Services konfigurira isporuke pravila za resursi. Pravilnik za isporuku resursa govori Media Services način za vaše resursa za isporuku: u koje strujanje protokol treba vaše resursa dinamički pakirati (na primjer, MPEG CRTICE, HLS, Smooth Streaming ili sve), hoće li želite dinamički šifriranje vaše resursa i kako (omotnice ili uobičajenih šifriranje).

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
- Ako imate imovine s postojećeg strujanje locator, ne može povezivanje novog pravilnika imovine, prekinuti vezu postojeće pravilo iz imovine ili ažurirajte pridruženo imovine pravilo za isporuku.  Najprije morate uklonili strujanje locator, prilagodite pravila, a zatim ponovno stvorite strujanje locator.  Možete koristiti isti locatorId kada ponovno stvorite strujanje locator, ali potrebno je provjeriti koji se neće izazvati probleme sa za klijente jer sadržaj mogu predmemorirane izvor ili do CDN.

>[AZURE.NOTE] Rad s Media Services REST API-JA, primjenjuju se Imajte na umu sljedeće:
>
>Prilikom pristupanja entiteti u Media Services, morate postaviti određene zaglavlje polja i vrijednosti u zahtjevima za HTTP. Dodatne informacije potražite u članku [Postavljanje Media Services REST API razvoj](media-services-rest-how-to-use.md).

>Nakon uspješnog povezivanja s https://media.windows.net, primit ćete 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI kao što je opisano u [povezuje se s Media Services pomoću REST API -JA](media-services-rest-connect-programmatically.md).


##<a name="clear-asset-delivery-policy"></a>Pravila za isporuku Očisti resursa

###<a id="create_asset_delivery_policy"></a>Stvaranje pravila za isporuku resursa
Sljedeće HTTP zahtjev stvara pravilo isporuke resursa koji određuje da ne primjenjuju se dinamički šifriranja i izlaganje toka u nekom od sljedećih protokola: MPEG CRTICE, HLS i Smooth Streaming protokola. 

Informacije o onih vrijednosti koje možete odrediti kada stvarate programa AssetDeliveryPolicy, potražite u odjeljku [vrste koristi prilikom definiranja AssetDeliveryPolicy](#types) .   


Zahtjev:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Odgovor:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Veza resursa s pravilima isporuke resursa

Sljedeće HTTP zahtjev povezuje navedeni resursa pravilnik isporuke resursa.

Zahtjev:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Odgovor:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Pravila za isporuku DynamicEnvelopeEncryption resursa 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Stvaranje sadržaja ključa vrste EnvelopeEncryption i povezati ga s imovine

Prilikom određivanja DynamicEnvelopeEncryption isporuke pravilnika, morate da biste bili sigurni da biste se povezali vaše resursa sadržaja ključ vrste EnvelopeEncryption. Dodatne informacije potražite u članku: [Stvaranje sadržaja ključa](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Dohvaćanje URL-a za isporuku

Dohvaćanje URL-a isporuke način navedeni isporuku sadržaja ključa stvorili u prethodnom koraku. Klijent koristi vraćeni URL-a da biste zatražili ključa AES ili na PlayReady licence redoslijedom za reprodukciju zaštićenog sadržaja.

Određivanje vrste URL-a da biste u tijelu zahtjeva za HTTP. Ako su zaštita sadržaja pomoću PlayReady, zatražite Media Services PlayReady licence acquisition URL-a, pomoću 1 za na keyDeliveryType: {"keyDeliveryType": 1}. Ako su zaštita sadržaja pomoću šifriranja omotnice, zatražite URL za nabavu ključa navođenjem 2 za keyDeliveryType: {"keyDeliveryType": 2}.

Zahtjev:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Odgovor:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Stvaranje pravila za isporuku resursa

Sljedeće HTTP zahtjev stvara **AssetDeliveryPolicy** koji je konfiguriran da biste primijenili dinamički omotnica šifriranja (**DynamicEnvelopeEncryption**) **HLS** protocol (u ovom primjeru ostalih protokola bit će blokirane iz strujanje). 


Informacije o onih vrijednosti koje možete odrediti kada stvarate programa AssetDeliveryPolicy, potražite u odjeljku [vrste koristi prilikom definiranja AssetDeliveryPolicy](#types) .   

Zahtjev:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Odgovor:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Veza resursa s pravilima isporuke resursa

U odjeljku [resursa vezu s pravilima isporuke resursa](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Pravila za isporuku DynamicCommonEncryption resursa 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Stvaranje sadržaja ključa vrste CommonEncryption i povezati ga s imovine

Prilikom određivanja DynamicCommonEncryption isporuke pravilnika, morate da biste bili sigurni da biste se povezali vaše resursa sadržaja ključ vrste CommonEncryption. Dodatne informacije potražite u članku: [Stvaranje sadržaja ključa](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Dohvaćanje URL-a za isporuku

Dohvaćanje URL-a isporuke način isporuke PlayReady sadržaja ključa stvorili u prethodnom koraku. Klijent koristi vraćeni URL-a da biste zatražili licence PlayReady redoslijedom za reprodukciju zaštićenog sadržaja. Dodatne informacije potražite u članku [Početak isporuke URL-a](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Stvaranje pravila za isporuku resursa

Sljedeće HTTP zahtjev stvara **AssetDeliveryPolicy** koji je konfiguriran da biste primijenili dinamički uobičajenih šifriranja (**DynamicCommonEncryption**) **Smooth Streaming** protocol (u ovom primjeru ostalih protokola bit će blokirane iz strujanje). 

Informacije o onih vrijednosti koje možete odrediti kada stvarate programa AssetDeliveryPolicy, potražite u odjeljku [vrste koristi prilikom definiranja AssetDeliveryPolicy](#types) .   


Zahtjev:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Ako želite zaštiti sadržaja pomoću Widevine DRM ažuriranje vrijednosti AssetDeliveryConfiguration WidevineLicenseAcquisitionUrl (koja ima vrijednost od 7) i navedite URL servis za isporuku licence. Možete koristiti sljedeće AMS partnere da biste lakše izlaganje Widevine licenci: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

Ako, na primjer: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Prilikom šifriranja s Widevine, samo će moći izlaganje pomoću CRTICE. Provjerite je li potrebno navesti CRTICU (2) u protokol za isporuku resursa.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Veza resursa s pravilima isporuke resursa

U odjeljku [resursa vezu s pravilima isporuke resursa](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Vrste koje se koriste prilikom definiranja AssetDeliveryPolicy

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

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

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

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

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

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
