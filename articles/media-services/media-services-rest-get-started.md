<properties 
    pageTitle="Početak rada s isporuke sadržaja na zahtjev pomoću REST | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake implementacijom aplikaciju za isporuku sadržaja na zahtjev s pomoću REST API-JA servisa Azure Media Services." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Početak rada s isporuke sadržaja na zahtjev pomoću REST 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](/pricing/free-trial/?WT.mc_id=A261C142F). 


U ovom brzi početak rada vodit će vas kroz korake implementacijom aplikacije za isporuku sadržaja Video-on-Demand (VoD) pomoću servisa Azure Media Services (AMS) REST API-ji. 

Vodič predstavlja tijeka rada za osnovni Media Services i najčešće programiranje objekte i zadatke koji su potrebni za razvoj Media Services. Po završetku vodič, moći strujanje ili progresivno Preuzimanje oglednih medijsku datoteku prenijeli, kodiranja i preuzeti.  

## <a name="prerequisites"></a>Preduvjeti
U sljedećim preduvjeta da biste pokrenuli razvoja s Media Services s REST API-ji.

- Razumijevanje razvoju s Media Services REST API-JA. Dodatne informacije potražite u članku [media-usluge-ostale – pregled](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Aplikacija po izboru možete poslati HTTP zahtjeva i odgovora. Pomoću ovog praktičnog vodiča koristi [Fiddler](http://www.telerik.com/download/fiddler). 

U ovom brzi početak rada prikazane su sljedeće zadatke.

1.  Stvaranje računa Media Services (pomoću portala za Azure).
2.  Konfiguriranje strujanje krajnju točku (pomoću portala za Azure).
1.  Povezivanje s računom Media Services s REST API-JA.
1.  Stvorite novi resursa i prenesite videodatoteku s REST API-JA.
1.  Konfigurirajte strujanje jedinice REST API-JA.
2.  Kodiranje izvornu datoteku u skupu prilagodljivo brzina prijenosa MP4 datoteke s REST API-JA.
1.  Objavljivanje resursa i dobivanje strujanje i progresivno preuzimanje URL-ove s REST API-JA. 
1.  Reprodukcija sadržaja. 

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

## <a id="connect"></a>Povezivanje s računom Media Services s REST API-JA

Dvije su potrebni prilikom pristupanja servisa Azure Media Services: nudi Azure pristup kontrola Services (ACS) i servise za URI Media sam token za pristup. Možete koristiti bilo koji način želite da se prilikom stvaranja te zahtjeve kao navesti vrijednosti ispravno zaglavlja i prosljeđivanja u token za pristup pravilno prilikom pozivanja u Media Services.

Sljedeći koraci opisuju najčešći tijeka rada kada koristite Media Services REST API-JA za povezivanje s Media Services:

1. Početak token za pristup. 
2. Povezivanje s Media Services URI.  

    Imajte na umu da nakon uspješnog povezivanja s https://media.windows.net, primite 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI. Možda ćete dobiti odgovor HTTP/1.1 200 koji sadrži opis metapodataka ODATA API.
3. Objavljivanje naknadni pozivi API-JA na novi URL. 
    
    Na primjer, ako nakon korištenja probne verzije za povezivanje, koji ste nabavili sljedeće:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Trebali biste objaviti kasnije pozive API https://wamsbayclus001rest-hs.cloudapp.net/api/.

###<a name="getting-an-access-token"></a>Početak token za pristup

Da biste pristupili Media Services izravno putem REST API-JA, dohvatiti token za pristup s ACS te ga koristiti tijekom svaki zahtjev HTTP unesete na servis. Neki preduvjeti nije potrebno prije izravno povezivanja s Media Services.

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

Morate proved client_id i client_secret vrijednosti u tijelu zahtjeva; client_id i client_secret odgovaraju vrijednosti AccountName i AccountKey, odnosno. Ove vrijednosti su vam dodijelio Media Services prilikom postavljanja računa. 

AccountKey za vaš račun Media Services mora biti kodiran URL-om kada se koristi kao vrijednost client_secret u vašem tokena zahtjeva za pristup.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Preporučuje se u predmemoriju "access_token" i "expires_in" vrijednosti vanjskih za pohranu. Tokena podataka nije kasnije dohvaćeni iz prostora za pohranu i ponovno koristiti u pozive Media Services REST API-JA. To je posebno korisno za scenariji kojima token mogu sigurno zajednički koristiti više procesa ili računala.

Provjerite je li nadzora vrijednost "expires_in" token za pristup i ažurirati pozive REST API-JA novi tokeni prema potrebi.

###<a name="connecting-to-the-media-services-uri"></a>Povezivanje s Media Services URI-JA

Korijenska URI za Media Services je https://media.windows.net/. Na početku treba povezati ovu URI, a ako 301 preusmjeravanje se vratiti u odgovoru, morate biti naknadni pozivi za nove URI. Osim toga, nemojte koristiti bilo koji logike automatski-preusmjeravanje/za daljnji u zahtjevima za. Glagoli HTTP i tijela zahtjeva nije moguće proslijediti novi URI.

Korijenska URI-JA za prijenos i preuzimanje datoteka resursa je https://yourstorageaccount.blob.core.windows.net/ gdje naziv računa spremišta isti onim koji se koristi prilikom postavljanja računa Media Services.

Sljedeći primjer prikazuje HTTP zahtjev za korijenske Media Services URI (https://media.windows.net/). Zahtjev dohvaća 301 preusmjeravanje vratiti u odgovoru. Zahtjev za kasnije koristi novi URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP zahtjev**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Odsad nadalje novi URI koristi ovog praktičnog vodiča.

## <a id="upload"></a>Stvaranje novog resursa i prijenos videodatoteku s REST API-JA

U Media Services prenijeti digitalne datoteke na sredstava. Entitet **resursa** može sadržavati videozapis, zvuk, slike, minijatura zbirke, tekst zapise i titlova datoteke (i metapodataka o tim datotekama.)  Kada se datoteke prenose se u sredstvo, sadržaj pohranjen sigurno u oblaku za daljnje obrade i strujanje. 

Jedna od vrijednosti koje je potrebno unijeti prilikom stvaranja sredstvo je mogućnosti stvaranja resursa. Svojstvo **Mogućnosti** je vrijednost numeriranja koji opisuje mogućnosti šifriranja koje je moguće stvoriti imovine s. Valjana vrijednost jedan je od vrijednosti na popisu u nastavku ne kombinacija vrijednosti s tog popisa:

 
- **Nema** = **0** - nema šifriranja služi. Kada koristite tu mogućnost sadržaja nije zaštićena na putu ili na ostale u prostor za pohranu.
    Ako namjeravate održati programa MP4 korištenje progresivno preuzimanje koristiti tu mogućnost. 
- **StorageEncrypted** = **1** - šifrira Očisti sadržaj lokalno koristi AES 256 bitno šifriranje i prenosi Azure prostora za pohranu gdje je pohranjena šifriraju na ostale. Resursi koji su zaštićeni šifriranjem prostora za pohranu se automatski nešifrirani smjestiti u sustavu šifriranu datoteku prije no što kodiranja i po želji se ponovno šifrirane prije no što prenesete natrag kao novi izlaz resursa. Slučaj prvenstveno se koristi za pohranu šifriranje je kada želite sigurne visoke kvalitete unos medijske datoteke s šifriranjem na ostale na disku.
- **CommonEncryptionProtected** = **2** – korištenje tu mogućnost ako prenosite sadržaj koji je već šifrirane i zaštićen uobičajenih šifriranje ili PlayReady DRM (na primjer, izglađenim strujanje zaštićeni PlayReady DRM).
- **EnvelopeEncryptionProtected** = **4** – pomoću te mogućnosti prenosite HLS šifrirane i zaštićene AES. Datoteka je potrebno kodirani i šifrirane Upravitelj pretvorbe.

### <a name="create-an-asset"></a>Stvaranje imovine

Sredstva je spremnik za više vrsta ili skupa objekata u Media Services, uključujući videozapis, zvuk, slike, minijatura zbirke, prati tekst i titlova datoteke. U REST API-JA, stvaranje sredstvo zahtijeva slanje zahtjeva za objavu Media Services i postavite svojstvo podatke o vašem resursa u tijelu zahtjeva.

Sljedeći primjer pokazuje kako stvoriti sredstava.

**HTTP zahtjev**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

**HTTP odgovor**

Ako ne uspije, vraća se sljedeće:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Stvaranje programa AssetFile

Entitet [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) predstavlja videoisječka ili zvučnog datoteku pohranjenu u spremniku blob. Datoteku resursa uvijek je povezan s sredstvo, a sredstvo možda sadrži jedan ili više AssetFiles. Zadatak Media Encoder usluge neće uspjeti ako je objekt datoteka resursa nije povezan s digitalnih datoteka u spremniku blob.

Nakon vaše digitalnim medijima datoteku prenijeli u spremniku blob, upotrijebite HTTP zahtjev **SPAJANJE** da biste ažurirali na AssetFile s informacijama o medijske datoteke (kao što je prikazano u nastavku teme). 

**HTTP zahtjev**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP odgovor**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Stvaranje na AccessPolicy s dozvolu za pisanje. 

Prije prijenosa sve datoteke u spremište blobova platforme, postavite pristup pravilnik o pravima za pisanje na imovinu. Da biste to učinili, OBJAVITE HTTP zahtjev da biste postavili entitet AccessPolicies. Definiranje TrajanjeUMinutama vrijednost nakon stvaranja ili primite poruku o pogrešci sa internom poslužitelju za 500 vratiti u odgovoru. Dodatne informacije o AccessPolicies potražite u članku [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Sljedeći primjer pokazuje kako stvoriti programa AccessPolicy:
        
**HTTP zahtjev**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP odgovor**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>Dohvaćanje URL za prijenos

Da biste dobili URL za stvarni prijenos, stvaranje SAS Locator. Locators definirati vrijeme početka i vrstu veze krajnja točka za klijente koje želite da biste pristupili datotekama u sredstava. Možete stvoriti više Locator entiteti za dani par AccessPolicy i resursa za rukovanje zahtjeve za drugi klijent i potrebama. Svaki od tih Locators za određivanje trajanja URL-a mogu se koristi vrijednost StartTime plus TrajanjeUMinutama vrijednost na AccessPolicy. Dodatne informacije potražite u članku [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS URL-a sastoji se od sljedećih oblika:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Neke informacije vrijediti:

- Ne možete imati više od pet jedinstveni Locators odjednom povezan s određenom resursa. Dodatne informacije potražite u članku Locator.
- Ako je potrebno odmah prenesite datoteke postavite na vrijednost StartTime pet minuta prije trenutnog vremena. Razlog tomu može biti sat skew između klijentskom računalu i Media Services je. Također se vaš StartTime vrijednost mora biti u sljedećem obliku datuma i vremena: gggg-MM-DDTHH:mm:ssZ (Ako, na primjer, "2014.-05-23T17:53:50Z").   
- Možda postoje drugi 30 40 odgađanje nakon što je stvoren u Locator kada je dostupan za korištenje. Taj se problem odnosi SAS URL i Locators polazište.

Sljedeći primjer pokazuje kako stvoriti Locator za URL SAS, kako je definirano svojstvo vrsta u tijelu zahtjeva ("1" za SAS locator) i "2" za programa locator polazište na zahtjev. Svojstvo **put** vraćena sadrži URL-a koju morate koristiti da biste prenijeli datoteku.
    
**HTTP zahtjev**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP odgovor**

Ako ne uspije, vraća se sljedeće odgovor:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Prijenos datoteke u spremniku spremište blobova platforme
    
Nakon što dodate AccessPolicy i Locator postavljanje, samu datoteku se prenosi u spremniku spremište blobova platforme Azure pomoću REST API-ji za pohranu Azure. Možete prenijeti na stranici ili blokirati blob-ova. 

>[AZURE.NOTE] Morate dodati naziv datoteke za datoteku koju želite prenijeti na vrijednost Locator **put** primljene u prethodnom odjeljku. Na primjer, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Dodatne informacije o radu s blob-ova Azure prostora za pohranu potražite u članku [Blob servisa REST API -JA](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Ažuriranje na AssetFile 

Sad kad ste prenijeli datoteke, ažurirajte podatke o FileAsset veličina (i drugih). Ako, na primjer:
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP odgovor**

Ako je uspješan, sljedeće se vraća: HTTP/1.1 204 bez sadržaja

## <a name="delete-the-locator-and-accesspolicy"></a>Brisanje Locator i AccessPolicy 

**HTTP zahtjev**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP odgovor**

Ako ne uspije, vraća se sljedeće:

    HTTP/1.1 204 No Content 
    ...

**HTTP zahtjev**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP odgovor**

Ako ne uspije, vraća se sljedeće:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Konfiguriranje strujanje jedinice s REST API-JA

Prilikom rada s nešto Najčešći scenariji isporučuje prilagodljivu streaming brzina prijenosa u klijentima servisa Azure Media Services. S prilagodljivo brzina prijenosa strujanje, klijent možete se prebacivati viših ili nižih brzina prijenosa strujanje tijekom prikaza videozapisa na temelju trenutne propusnost mreže, procesora i ostali čimbenici. Media Services podržava sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo). 

Media Services nudi dinamički pakiranje koja vam omogućuje da izlaganje prilagodljivo brzina prijenosa MP4 ili Smooth Streaming kodirani sadržaj u strujanje oblici koje podržava Media Services (MPEG CRTICE, HLS, Smooth Streaming, HDS) bez potrebe za repackage u ovo strujanje oblici. 

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- Pronađite najmanje jednu strujanje jedinicu u **strujanje krajnjoj točki **iz koje namjeravate za isporuku sadržaja (opisani u ovom odjeljku).
- kodiranje ili transcode mezzanine (izvor) datoteke u skupu prilagodljivo brzina prijenosa MP4 datoteke ili prilagodljivo brzina prijenosa Smooth Streaming datoteke (kodiranja korake su što je prikazano u nastavku ovog praktičnog vodiča)  

Dinamični pakiranje samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services stvara i služi prikladan odgovor na temelju zahtjeve iz klijenta. 


>[AZURE.NOTE] Informacije o cijenama detalje potražite u članku [Media Services cijene pojedinosti](http://go.microsoft.com/fwlink/?LinkId=275107).

Da biste promijenili broj strujanje rezervirana jedinice, učinite sljedeće:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Početak strujanje krajnju točku koju želite ažurirati

Na primjer, recimo dobiti prvi strujanje krajnje točke na vašem računu (do dva strujanje krajnje točke u izvodi stanje istovremeno možete imati.)

**HTTP zahtjev**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP odgovor**
    
Ako ne uspije, vraća se sljedeće:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Promjena veličine strujanje krajnje točke
 
**HTTP zahtjev**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP odgovor**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Provjera stanja postupka dugoročnih

Dodjela sve nove jedinice traje oko 20 minuta da biste dovršili. Da biste provjerili status postupak korištenja metode **operacije** i navedite Id postupak. Operacija kao odgovor na zahtjev za **Skaliranje** vraćena je ID-a.

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP zahtjev**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP odgovor**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Kodiranje izvornu datoteku u skupu prilagodljivo brzina prijenosa MP4 datoteke

Nakon ingesting imovine u Media Services, medijskih sadržaja moguće kodirati, vodenim žigom, transmuxed i tako dalje prije no što je isporučeno klijentima. Te aktivnosti su zakazali i pokrenuti više instanci uloga pozadine da biste bili sigurni visoke performanse i dostupnost. Te aktivnosti nazivaju zadacima i svaki [zadatak](http://msdn.microsoft.com/library/azure/hh974289.aspx) sastoji se od atomske zadatke koje obaviti Stvaran rad na datoteci resursa. 

Kao što je navedeno ranije, prilikom rada s Azure Media Services nešto Najčešći scenariji isporučuje prilagodljivu streaming brzina prijenosa u klijentima. Media Services možete dinamički pakiranje skup prilagodljivo brzina prijenosa MP4 datoteka u jednu od sljedećih oblika: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo). 

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- kodiranje ili transcode mezzanine (izvor) datoteke u skupu prilagodljivo brzina prijenosa MP4 datoteke ili datoteke Smooth Streaming prilagodljivo brzina prijenosa,  
- Pronađite najmanje jednu strujanje jedinicu za strujanje krajnju točku iz koje namjeravate održati sadržaj. 

Sljedeći odjeljak pokazuje kako stvoriti zadatak koji sadrži jedan zadatak kodiranja. Zadatak određuje da biste transcode mezzanine datoteku u skupu prilagodljivo brzina prijenosa MP4s pomoću **Media Encoder Standard**. U odjeljku prikazuju i upute za praćenje posla obrada tijeka. Nakon dovršetka posla će moći stvoriti locators koja su potrebna za pristup vašem resursi. 

### <a name="get-a-media-processor"></a>Početak medija procesor

U Media Services media procesor je komponenta korisničkog sučelja koji se rukuje određenim obrada zadatka, kao što su kodiranja, pretvaranje oblika, medija šifriranja ili dešifriranja sadržaja. Kodiranja zadatka prikazane u ovom ćete praktičnom vodiču, ne možemo namjeravate koristiti Media Encoder standardne.

Sljedeći kod zahtjeve u encoder ID-a. 

**HTTP zahtjev**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP odgovor**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Stvaranje zadatka

Svaki zadatak može imati jedan ili više zadataka ovisno o vrsti obrada koju želite izvršiti. Kroz REST API-JA, možete stvoriti zadacima i njihovih povezani zadaci na jedan od dva načina: zadaci mogu biti definirani izravno putem svojstvo zadaci na entiteti posao ili putem OData skupna obrada. Media Services SDK koristi obrade. Međutim, zbog čitljivosti kod primjeri u ovoj temi, Zadaci su definirani u istoj razini. Informacije o obrade potražite u članku [Skupna obrada protokola Open Data (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Sljedeći primjer pokazuje kako stvarati i objavljivati posao jednim zadatka postavljena na kodiranje videozapisa na određene razlučivosti i kvalitete. Sljedeći odjeljak dokumentacija sadrži popis svih [Unaprijed postavljeno zadatka](http://msdn.microsoft.com/library/mt269960) podržava Media Encoder standardne procesor.  

**HTTP zahtjev**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP odgovor**

Ako ne uspije, vraća se sljedeće odgovor:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Stvari nekoliko važnih Napomena u bilo kojem zahtjev za zadatak:

- Svojstva TaskBody morate koristiti slovni XML da biste definirali broj unos ili izlaz sredstva koja koriste zadatak. Tema zadatka sadrži definicija sheme XML za XML.
- U definiciji TaskBody svaki inner vrijednost <inputAsset> i <outputAsset> mora biti postavljeno kao JobInputAsset(value) ili JobOutputAsset(value).
- Zadatak možete imati više izlaz resursi. Jedan JobOutputAsset(x) mogu se koristiti samo jednom kao na Izlaz zadatka na zadatak.
- Možete odrediti JobInputAsset ili JobOutputAsset kao sredstvo za unos zadatka.
- Zadaci moraju obrasca krugu.
- Vrijednost parametra koji proći JobInputAsset ili JobOutputAsset predstavlja vrijednost indeksa sredstava. Stvarni Resursi su definirani u navigaciju svojstva InputMediaAssets i OutputMediaAssets na entitet definiciju posla. 

>[AZURE.NOTE] Budući da je Media Services utemeljena na OData v3, pojedinačne imovine u InputMediaAssets i OutputMediaAssets zbirke svojstvo navigacijsko se pozivaju pomoću programa "__metadata: uri" par vrijednost za naziv. 

- InputMediaAssets mapira jednog ili više sredstava koji ste stvorili u Media Services. OutputMediaAssets stvaraju se u sustav. Koje se odnose postojeće resursa.
- OutputMediaAssets možete dobiti naziv na temelju atribut assetName. Ako je taj atribut postoji, a zatim naziv u OutputMediaAsset je bilo kakve unutarnji tekst vrijednost u <outputAsset> element je s sufiks naziv zadatka vrijednost ili vrijednost Id zadatka (u slučaju gdje nije definirano svojstvo naziv). Na primjer, ako postavite vrijednost za assetName da biste "Uzorak", zatim naziv OutputMediaAsset svojstvo bi biti postavljeno na "Uzorak". Međutim, ako ne postavite vrijednost za assetName, ali niste postavili naziv zadatka da biste "NewJob", zatim naziv OutputMediaAsset bi "_NewJob JobOutputAsset (vrijednost)". 

    Sljedeći primjer prikazuje način da biste postavili atribut assetName:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Da biste omogućili Ulančavanje zadatka:

    - Zadatak morate imati barem dva zadatka
    - Mora imati najmanje jedan zadatak je čiji unos Izlaz iz druge zadatke u sklopu posla.

Dodatne informacije potražite u člancima, [Stvaranje zadatak s Media Services REST API -JA za šifriranje](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Monitor obrada tijeka

Pomoću svojstva stanja možete dohvatiti statusu zadatka kao što je prikazano u sljedećem primjeru. 

**HTTP zahtjev**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP odgovor**

Ako ne uspije, vraća se sljedeće odgovor:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Otkazivanje posla

Media Services omogućuje vam da biste poništili izvodi zadatke pomoću funkcije CancelJob. U ovom poziva vraća 400 pogreška s kodom ako pokušate odustajanje od zadatka kada je stanje otkazana, poništavanje, pogreške ili dovršeno.

Sljedeći primjer prikazuje način da biste nazvali CancelJob.


**HTTP zahtjev**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Ako ne uspije, kod 204 odgovor, vraća se s tijela poruke.

>[AZURE.NOTE] Morate URL kodiranje id zadatka (obično nb:jid:UUID: somevalue) kada prosljeđivanje u kao parametar CancelJob.


### <a name="get-the-output-asset"></a>Početak izlaza resursa 

Sljedeći kod prikazuje kako zatražiti resursa izlaz ID-a. 


**HTTP zahtjev**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP odgovor**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Objavljivanje resursa i dobivanje strujanje i progresivno preuzimanje URL-ove s REST API-JA

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

U ovom se odjeljku objašnjava izvedite sljedeće zadatke potrebne za svoje imovine "Objavi".  

- Stvaranje na AccessPolicy dozvola za čitanje 
- Stvaranje SAS URL-a za preuzimanje sadržaja 
- Stvaranje polazište URL za strujanje sadržaja 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Stvaranje na AccessPolicy dozvola za čitanje

Prije preuzimanja ili strujanje sve medijske sadržaje, najprije definiranje programa AccessPolicy s dozvolama za čitanje i stvaranje odgovarajuće Locator onaj koji navodi vrstu mehanizam isporuke koji želite omogućiti za svoje klijente. Dodatne informacije o njihovim svojstvima potražite u članku [AccessPolicy entitet svojstva](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Sljedeći primjer pokazuje kako odrediti AccessPolicy za čitanje dozvole za dani resursa.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Ako ne uspije, vraća se 201 uspjeh kod s opisom entitet AccessPolicy koji ste stvorili. Zatim pomoću ID-a AccessPolicy uz identifikator resursa sredstva koja sadrži datoteku za koju želite izlaganje (kao što su sredstvo Izlaz) da biste stvorili entitet Locator.

>[AZURE.NOTE]
Ovaj osnovni tijek rada je isti kao prijenosa datoteke kada ingesting imovine (kao što je spominju u ovoj temi). Osim toga, kao što su prijenos datoteka, ako vi (ili klijentima) morate odmah pristupiti datotekama, postavite svoje StartTime vrijednost na pet minuta prije trenutnog vremena. Ova akcija nije potrebno jer se može biti sat skew između klijenta i Media Services. StartTime vrijednost mora biti u sljedećem obliku datuma i vremena: gggg-MM-DDTHH:mm:ssZ (na primjer, "2014.-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Stvaranje SAS URL-a za preuzimanje sadržaja 

Sljedeći kod prikazuje kako doći do URL-a koji se mogu koristiti da biste preuzeli medijske datoteke stvoriti i prenijeti prethodno. Na AccessPolicy pročitao skup dozvola i Locator put odnosi s URL-om za preuzimanje SAS.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Ako ne uspije, vraća se sljedeće odgovor:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Vraćeni svojstvo **put** sadrži SAS URL-a.

>[AZURE.NOTE]
Ako preuzimate šifrirane prostora za pohranu sadržaja, morate ručno dešifrirati prije nego što je vizualizacije, ili pomoću dešifriranje MediaProcessor prostora za pohranu u zadatku obrada izlaz obrađene datoteke u poništite da bi se OutputAsset, a zatim ga preuzmite iz tog resursa. Dodatne informacije o obrada potražite u članku Stvaranje zadatka za šifriranje s Media Services REST API-JA. Osim toga, SAS URL Locators nije moguće ažurirati nakon što ste je stvorili. Na primjer, ne može ponovno koristiti isti Locator ažurirane StartTime vrijednošću. To je zbog onako kako se stvaraju SAS URL-ova. Ako želite da biste pristupili sredstvo za preuzimanje kada je Locator istekla, morate stvoriti novi s nove StartTime.

###<a name="download-files"></a>Preuzimanje datoteka

Nakon što dodate AccessPolicy i Locator postavljanje, možete preuzeti datoteke pomoću REST API-ji za pohranu Azure.  

>[AZURE.NOTE] Morate dodati naziv datoteke za datoteku koju želite preuzeti vrijednost Locator **put** primljene u prethodnom odjeljku. Na primjer, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Dodatne informacije o radu s blob-ova Azure prostora za pohranu potražite u članku [Blob servisa REST API -JA](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Uslijed posao kodiranja koju ste stvorili ranije (kodiranja u skupu prilagodljivo MP4), imate više datoteka MP4 progresivno možete preuzeti. Ako, na primjer:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Stvaranje strujanje URL-a za strujanje sadržaja


Sljedeći kod prikazuje kako stvoriti strujanje Locator URL:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Ako ne uspije, vraća se sljedeće odgovor:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Za strujanje Smooth Streaming polazište URL-a u strujanje media player, morate dodati put svojstvo s nazivom Smooth Streaming datoteku, a zatim manifesta "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Za strujanje HLS dodati (oblik = m3u8 aapl) nakon na "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Za strujanje MPEG CRTICE, dodati (oblik = mpd, vrijeme i csf) nakon na "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Reprodukcija sadržaja  

Za strujanje videozapisa, koristite [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Da biste testirali progresivno preuzimanje, zalijepite URL u pregledniku (na primjer, preglednika Internet Explorer, Chrome, Safari).


##<a name="next-steps-media-services-learning-paths"></a>Sljedeći koraci: Media Services Tečajevi

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Tražite li nešto drugo?

Ako niste u ovoj se temi sadrže ono što ste očekivali, nema nešto ili u neki drugi način ne zadovoljava vaše potrebe, pošaljite nam s povratne informacije pomoću Disqus niti u nastavku.



