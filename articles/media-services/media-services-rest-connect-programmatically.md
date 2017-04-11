<properties 
    pageTitle="Povezivanje s računa za servise medijskih sadržaja pomoću REST API-JA | Microsoft Azure" 
    description="U ovoj se temi objašnjava kako se povezati sa Media Services uisng REST API-JA." 
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
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Povezivanje s računa za servise medijskih sadržaja pomoću Media Services REST API-JA

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [OSTALE](media-services-rest-connect-programmatically.md)

U ovoj se temi objašnjava kako nabaviti programski veze sa servisa Microsoft Azure Media Services kada su programiranje Media Services REST API-JA.

Dvije su potrebni prilikom pristupanja servisa Microsoft Azure Media Services: nudi Azure pristup kontrola Services (ACS) i servise za URI Media sam token za pristup. Možete koristiti bilo koji način želite da se prilikom stvaranja te zahtjeve kao navesti vrijednosti ispravno zaglavlja i prosljeđivanja u token za pristup pravilno prilikom pozivanja u Media Services.

Sljedeći koraci opisuju najčešći tijeka rada kada koristite Media Services REST API-JA za povezivanje s Media Services:

1. Početak token za pristup 
2. Povezivanje s Media Services URI-JA 

    >[AZURE.NOTE] Nakon uspješnog povezivanja s https://media.windows.net, primit ćete 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI.
Možda ćete dobiti odgovor HTTP/1.1 200 koji sadrži opis metapodataka ODATA API.

3. Objavite naknadni pozivi API-JA na novi URL. 

    Na primjer, ako nakon korištenja probne verzije za povezivanje, koji ste nabavili sljedeće:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Trebali biste objaviti kasnije pozive API https://wamsbayclus001rest-hs.cloudapp.net/api/.

##<a name="access-control-address"></a>Adresa za kontrolu pristupa

Adresa za kontrolu pristupa Media Services je https://wamsprodglobal001acs.accesscontrol.windows.net, osim Sjeverna Kina regiju, pri čemu je https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Početak token za pristup

Da biste pristupili Media Services izravno putem REST API-JA, dohvatiti token za pristup s ACS te ga koristiti tijekom svaki zahtjev HTTP unesete na servis. Ovaj token je slična druge tokeni koje ste dobili od ACS na temelju navedenih u zaglavlju HTTP zahtjev i pomoću protokola v2 OAuth zahtjevima za pristup. Neki preduvjeti nije potrebno prije izravno povezivanja s Media Services.

Sljedeći primjer prikazuje HTTP zahtjev za zaglavlja i tijela koji se koristi za dohvaćanje token.

**Zaglavlja**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Tijelo**:

Morate biti client_id i client_secret vrijednosti u tijelu zahtjeva; client_id i client_secret odgovaraju vrijednosti AccountName i AccountKey, odnosno. Ove vrijednosti su vam dodijelio Media Services prilikom postavljanja računa. 

Imajte na umu AccountKey za vaš račun Media Services mora biti kodiran URL-om (pogledajte [Šifriranje u obliku postotaka](http://tools.ietf.org/html/rfc3986#section-2.1) kada se koristi kao vrijednost client_secret u vašem tokena zahtjeva za pristup.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Ako, na primjer: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Sljedeći primjer prikazuje HTTP odgovor koji sadrži pristup tokena u tijelu odgovor.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Preporučuje se u predmemoriju "access_token" i "expires_in" vrijednosti vanjskih za pohranu. Tokena podataka nije kasnije dohvaćeni iz prostora za pohranu i ponovno koristiti u pozive Media Services REST API-JA. To je posebno korisno za scenariji kojima token mogu sigurno zajednički koristiti više procesa ili računala.

Provjerite je li nadzora vrijednost "expires_in" token za pristup i ažurirati pozive REST API-JA novi tokeni prema potrebi.

###<a name="connecting-to-the-media-services-uri"></a>Povezivanje s Media Services URI-JA

Korijenska URI za Media Services je https://media.windows.net/. Na početku treba povezati ovu URI, a ako 301 preusmjeravanje se vratiti u odgovoru, morate biti naknadni pozivi za nove URI. Osim toga, nemojte koristiti bilo koji logike automatski-preusmjeravanje/za daljnji u zahtjevima za. Glagoli HTTP i tijela zahtjeva nije moguće proslijediti novi URI.

Imajte na umu da korijenski URI-JA za prijenos i preuzimanje datoteka resursa https://yourstorageaccount.blob.core.windows.net/ gdje naziv računa spremišta ista je jedan koji ste koristili prilikom postavljanja računa Media Services.

Sljedeći primjer prikazuje HTTP zahtjev za korijenske Media Services URI (https://media.windows.net/). Zahtjev dohvaća 301 preusmjeravanje vratiti u odgovoru. Zahtjev za kasnije koristi novi URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP zahtjev**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP odgovor**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP zahtjev** (pomoću novog URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP odgovor**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Kad primite novu URI, to je URI koji će se koristiti za komunikaciju s Media Services. 


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
