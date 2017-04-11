<properties 
    pageTitle="Prijenos datoteka na račun servisa Media Services pomoću REST | Microsoft Azure" 
    description="Saznajte kako pronaći i medijskih sadržaja u Media Services stvaranju prenošenje imovine." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Prijenos datoteka na račun servisa Media Services pomoću REST

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [OSTALE](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

U Media Services prenijeti digitalne datoteke na sredstava. Entitet [resursa](https://msdn.microsoft.com/library/azure/hh974277.aspx) može sadržavati videozapis, zvuk, slike, minijatura zbirke, tekst zapise i titlova datoteke (i metapodataka o tim datotekama.)  Kada se datoteke prenose se u sredstvo, sadržaj pohranjen sigurno u oblaku za daljnje obrade i strujanje. 

>[AZURE.NOTE]Kada odaberete naziva datoteke programa resursa, primjenjuju se Imajte na umu sljedeće:
>
>- Media Services koristi vrijednost svojstva IAssetFile.Name kada sastavljanje URL-ovi za strujanje sadržaj (na primjer, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Zbog toga šifriranje u obliku postotaka nije dopušteno. Svojstvo **ime** ne smije sadržavati sljedeće [znakove postotak kodiranje-rezervirana](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Osim toga, može postojati samo jedan '. "za nastavak.
>
>- Duljina naziva ne smije biti veća od 260 znakova.

Osnovni tijek rada za prijenos imovine je podijeljen u sljedeće odjeljke:

- Stvaranje imovine
- Šifriranje imovine (nije obavezno)
- Prijenos datoteke u spremište blobova platforme

AMS omogućuje vam da biste prenijeli imovine masovno. Dodatne informacije potražite u članku [ovaj](media-services-rest-upload-files.md#upload_in_bulk) odjeljak.

##<a name="upload-assets"></a>Prijenos resursi

###<a name="create-an-asset"></a>Stvaranje imovine

>[AZURE.NOTE] Rad s Media Services REST API-JA, primjenjuju se Imajte na umu sljedeće:
>
>Prilikom pristupanja entiteti u Media Services, morate postaviti određene zaglavlje polja i vrijednosti u zahtjevima za HTTP. Dodatne informacije potražite u članku [Postavljanje Media Services REST API razvoj](media-services-rest-how-to-use.md).

>Nakon uspješnog povezivanja s https://media.windows.net, primit ćete 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI kao što je opisano u [povezuje se s Media Services pomoću REST API -JA](media-services-rest-connect-programmatically.md). 
 
Sredstva je spremnik za više vrsta ili skupa objekata u Media Services, uključujući videozapis, zvuk, slike, minijatura zbirke, prati tekst i titlova datoteke. U REST API-JA, stvaranje sredstvo zahtijeva slanje zahtjeva za objavu Media Services i postavite svojstvo podatke o vašem resursa u tijelu zahtjeva.

Jedna od svojstava koja možete odrediti kada stvarate sredstvo je **Mogućnosti**. **Mogućnosti** je vrijednost numeriranja koji opisuje mogućnosti šifriranja koje je moguće stvoriti imovine s. Valjana vrijednost je jedna od vrijednosti na popisu u nastavku ne kombinacija vrijednosti. 

- **Nema** = **0**: nema šifriranja će se koristiti. To je zadana vrijednost. Imajte na umu da koristite tu mogućnost sadržaja nije zaštićena na putu ili na ostale u prostor za pohranu.
    Ako namjeravate održati programa MP4 korištenje progresivno preuzimanje koristiti tu mogućnost. 

- **StorageEncrypted** = **1**: Navedite želite li datoteke šifriranje s bitno AES 256 šifriranje za prijenos i prostora za pohranu.

    Ako je vaš resursa za pohranu šifrirane, morate konfigurirati pravila za isporuku resursa. Dodatne informacije potražite u članku [Konfiguriranje resursa isporuke pravila](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: Navedite prenosite datoteka zaštićena je na uobičajeni način šifriranja (kao što su PlayReady). 

- **EnvelopeEncryptionProtected** = **4**: odredite prenosite HLS šifrirane i zaštićene AES datoteke. Imajte na umu da datoteke mora su kodirani i šifrirane Upravitelj pretvorbe.

>[AZURE.NOTE]Ako vaš resursa koristi šifriranje, morate stvoriti **ContentKey** i veza na vašem resursa kao što je opisano u temi:[kako stvoriti na ContentKey](media-services-rest-create-contentkey.md). Imajte na umu da nakon prenijeti datoteke u sredstvo, potrebno je ažuriranje svojstava šifriranje na entitet **AssetFile** s vrijednostima koji ste nabavili tijekom šifriranja **resursa** . Stvoriti pomoću **SPAJANJA** HTTP zahtjev. 


Sljedeći primjer pokazuje kako stvoriti sredstava.

**HTTP zahtjev**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Stvaranje programa AssetFile

Entitet [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) predstavlja videoisječka ili zvučnog datoteku pohranjenu u spremniku blob. Datoteku resursa uvijek je povezan s sredstvo, a sredstvo možda sadrži jednu ili više datoteka resursa. Zadatak Media Encoder usluge neće uspjeti ako je objekt datoteka resursa nije povezan s digitalnih datoteka u spremniku blob.

Imajte na umu da instancu **AssetFile** i stvarne medijske datoteke dva različita objekta. AssetFile instance sadrži metapodatke o medijske datoteke dok medijske datoteke sadrži stvarni medijskog sadržaja.

Nakon vaše digitalnim medijima datoteku prenijeli u spremniku blob, koristit ćete HTTP zahtjev **SPAJANJE** da biste ažurirali na AssetFile s informacijama o medijske datoteke (kao što je prikazano u nastavku teme). 

**HTTP zahtjev**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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

Prije prijenosa sve datoteke u spremište blobova platforme, postavite pristup pravilnik o pravima za pisanje na imovinu. Da biste to učinili, OBJAVITE HTTP zahtjev da biste postavili entitet AccessPolicies. Definiranje TrajanjeUMinutama vrijednost nakon stvaranja ili dobit ćete 500 poruka o pogrešci internom poslužitelju vratiti u odgovoru. Dodatne informacije o AccessPolicies potražite u članku [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Sljedeći primjer pokazuje kako stvoriti programa AccessPolicy:
        
**HTTP zahtjev**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP zahtjev**

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

###<a name="get-the-upload-url"></a>Dohvaćanje URL za prijenos

Da biste dobili URL za stvarni prijenos, stvaranje SAS Locator. Locators definirati vrijeme početka i vrstu veze krajnja točka za klijente koje želite da biste pristupili datotekama u sredstava. Možete stvoriti više Locator entiteti za dani par AccessPolicy i resursa za rukovanje zahtjeve za drugi klijent i potrebama. Svaki od tih Locators pomoću vrijednost StartTime plus TrajanjeUMinutama vrijednost u AccessPolicy određuje duljinu URL-a može se koristiti. Dodatne informacije potražite u članku [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS URL-a sastoji se od sljedećih oblika:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Neke informacije vrijediti:

- Ne možete imati više od pet jedinstveni Locators odjednom povezan s određenom resursa. Dodatne informacije potražite u članku Locator.
- Ako je potrebno odmah prenesite datoteke postavite na vrijednost StartTime pet minuta prije trenutnog vremena. Razlog tomu može biti sat skew između klijentskom računalu i Media Services je. Također se vaš StartTime vrijednost mora biti u sljedećem obliku datuma i vremena: gggg-MM-DDTHH:mm:ssZ (Ako, na primjer, "2014.-05-23T17:53:50Z").   
- Možda postoje drugi 30 40 odgađanje nakon što je stvoren u Locator kada je dostupan za korištenje. Taj se problem odnosi SAS URL i Locators polazište.

Sljedeći primjer pokazuje kako stvoriti Locator za URL SAS, kako je definirano svojstvo vrsta u tijelu zahtjeva ("1" za SAS locator) i "2" za programa locator polazište na zahtjev. Svojstvo **put** vraća sadrži URL-a koju morate koristiti da biste prenijeli datoteku.
    
**HTTP zahtjev**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP odgovor**

Ako je uspješan, sljedeće se vraća: HTTP/1.1 204 bez sadržaja

### <a name="delete-the-locator-and-accesspolicy"></a>Brisanje Locator i AccessPolicy 

**HTTP zahtjev**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP odgovor**

Ako ne uspije, vraća se sljedeće:

    HTTP/1.1 204 No Content 
    ...

**HTTP zahtjev**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP odgovor**

Ako ne uspije, vraća se sljedeće:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Prijenos imovine masovno

###<a name="create-the-ingestmanifest"></a>Stvaranje na IngestManifest

U IngestManifest je spremnik za skup resursi, datoteka resursa i statističke podatke koje je moguće koristiti da biste odredili tijeku skupno ingesting za skup.


**HTTP zahtjev**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Stvaranje resursi

Prije stvaranja na IngestManifestAsset potrebnih za stvaranje resursa koji će se dovršiti pomoću masovnog ingesting. Sredstva je spremnik za više vrsta ili skupa objekata u Media Services, uključujući videozapis, zvuk, slike, minijatura zbirke, prati tekst i titlova datoteke. U REST API-JA, stvaranje sredstvo zahtijeva pošaljete zahtjev HTTP POST na servisa Microsoft Azure Media Services i postavite svojstvo podatke o vašem resursa u tijelu zahtjeva. U ovom primjeru imovine se stvara pomoću mogućnosti StorageEncrption(1) ugrađenima u tijelu zahtjeva.

**HTTP odgovor**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Stvaranje na IngestManifestAssets

IngestManifestAssets predstavljaju imovine unutar programa IngestManifest koja se koristi s ingesting skupno. Na osnovi imovine povezati manifest. Azure Media Services interno nadzirane prijenosa datoteka na temelju IngestManifestFiles zbirke povezan s IngestManifestAsset. Nakon prijenosa datoteke imovine ne završi. Možete stvoriti novu IngestManifestAsset sa zahtjevom za HTTP POST. U tijelu zahtjeva obuhvaćaju IngestManifest Id i Id resursa koji na IngestManifestAsset treba povezivati za skupno ingesting.

**HTTP odgovor**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Stvaranje na IngestManifestFiles za svaku imovine

Na IngestManifestFile predstavlja stvarni videoisječka ili zvučnog blob objekt koji će biti prenesene kao dio masovnog ingesting imovine. Šifriranje povezane svojstva nisu potrebni osim ako imovine koristi mogućnost šifriranja. Primjer koristi u ovom odjeljku prikazuje stvaranje programa IngestManifestFile koji koristi StorageEncryption za resursa koje ste prethodno stvorili.


**HTTP odgovor**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Prijenos datoteke sa spremištem blobova

Možete koristiti bilo koji velika brzina klijentska aplikacija koje je moguće povezati s prijenosom datoteka resursa spremniku spremišta blobova platforme Uri nudi svojstvo BlobStorageUriForUpload na IngestManifest. Jedan najvažnije velika brzina prijenos servis je [Aspera na zahtjev za Azure aplikaciju](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Skupno monitor Ingest tijeku

Možete pratiti napredak skupno ingesting operacije za programa IngestManifest tako da svojstvo Statistika na IngestManifest provjere. Svojstvo je Složena vrsta [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Za svojstvo Statistika, pošaljite zahtjev za HTTP GET prosljeđivanje IngestManifest Id.
 

##<a name="create-contentkeys-used-for-encryption"></a>Stvaranje ContentKeys koji se koristi za šifriranje

Ako vaš resursa će koristiti šifriranje, morate stvoriti ContentKey koja će se koristiti za šifriranje prije stvaranja datoteke resursa. Za šifriranje prostora za pohranu, sljedeća svojstva želite uvrstiti u tijelu zahtjeva.
 
Svojstvo tijelu zahtjeva   | Opis
---|---
ID-a | ContentKey Id koje ćemo nas generiranje pomoću sljedećih oblika "nb:kid:UUID:<NEW GUID>".
ContentKeyType | To je vrsta sadržaja ključa kao cijeli tog sadržaja ključa. Ne možemo proslijedite vrijednost 1 za šifriranje prostora za pohranu.
EncryptedContentKey | Ćemo stvoriti novu vrijednost sadržaja ključa koji je vrijednost 256-bitni (32 bajta). Ključ je šifriran pomoću certifikata za X.509 šifriranje prostora za pohranu koji ne možemo dohvatiti iz servisa Microsoft Azure Media Services izvršavanjem zahtjev HTTP GET za GetProtectionKeyId i GetProtectionKey metode.
ProtectionKeyId | To je id ključa zaštite u prostor za pohranu šifriranje certifikat x.509 koji je korišten za šifriranje naš sadržaja ključ.
ProtectionKeyType | To je vrsta šifriranja za zaštitu ključ koji je korišten za šifriranje tipku sadržaja. U ovom se vrijednost koristi StorageEncryption(1) našeg primjera.
Kontrolni zbroj |MD5 izračunatom kontrolnom zbroju tipke sadržaja. Izračunati je šifrira sadržaja ID-a pomoću tipku sadržaja. Kod primjer pokazuje kako izračunati na kontrolni zbroj.


**HTTP odgovor**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Veza na ContentKey na imovinu

Na ContentKey povezan je jedan ili više imovini slanjem zahtjeva za HTTP POST. Zahtjev za sljedeće se primjer da biste se povezali primjer ContentKey resursa primjer prema ID-a.

**HTTP odgovor**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP odgovor**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
