<properties 
    pageTitle="Šifriranje sadržaja pomoću prostora za pohranu šifriranje pomoću AMS REST API-JA" 
    description="Saznajte kako šifrirati sadržaja pomoću AMS REST API-ji šifriranjem prostora za pohranu." 
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


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Šifriranje sadržaja pomoću prostora za pohranu šifriranje pomoću AMS REST API-JA

Preporučljivo je Šifriraj sadržaj lokalno koristi AES 256 bitno šifriranje, a zatim je prijenos na Azure spremište gdje će biti pohranjeno šifriraju na ostale.

U ovom se članku daje pregled šifriranje AMS prostora za pohranu i pokazuje kako prenijeti sadržaj za pohranu šifrirane:

- Stvaranje sadržaja ključa.
- Stvaranje sredstava. Postavite na AssetCreationOption StorageEncryption prilikom stvaranja sredstava.

     Sredstva šifriranim moraju biti povezane s tipkama sadržaja.
- Veza sadržaja ključ sredstava.  
- Postavljanje šifriranja odnosi parametre entiteti AssetFile.
 
>[AZURE.NOTE]Ako želite izlaganje resursa za šifriranu prostora za pohranu, morate konfigurirati pravila za isporuku sredstava. Prije moguće je strujanjem vaše resursa, strujanje poslužitelja uklanja šifriranje prostora za pohranu i strujanja putem pravila za isporuku navedeni sadržaj. Dodatne informacije potražite u članku [Konfiguriranje pravilnika za isporuku resursa](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Rad s Media Services REST API-JA, primjenjuju se Imajte na umu sljedeće:
>
>Prilikom pristupanja entiteti u Media Services, morate postaviti određene zaglavlje polja i vrijednosti u zahtjevima za HTTP. Dodatne informacije potražite u članku [Postavljanje Media Services REST API razvoj](media-services-rest-how-to-use.md).

>Nakon uspješnog povezivanja s https://media.windows.net, primit ćete 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI kao što je opisano u [povezuje se s Media Services pomoću REST API -JA](media-services-rest-connect-programmatically.md). 

##<a name="storage-encryption-overview"></a>Pregled prostora za pohranu šifriranje 

Prostor za pohranu šifriranje AMS **AES GRUPU** način šifriranja odnosi se na cijelu datoteku.  Način AES GRUPU je snaga bloka koji Šifrirajte proizvoljne duljine podatke bez potrebe za udaljenosti od ruba. Pristajete šifrira brojač blok s algoritma AES i zatim XOR zapis izlaz AES s podacima za šifriranje ili dešifriranje.  Blokiranje brojač koristi konstruirana kopiranjem vrijednosti u InitializationVector da biste bajtova od 0 do 7 brojač vrijednosti i bajtova 8 do 15 vrijednosti brojač postavljene na nulu. Brojač bloka 16 bajt bajtova 8 do 15 (odnosno najmanje važan bajtova) koji se koristi kao na jednostavne 64-bitni cijeli broj bez predznaka koji se povećava po jedan za svaki sljedeći blok podataka obrađuju te se drži redoslijedom bajt mreže. Imajte na umu da ako ovaj cijeli broj dosegne maksimalna vrijednost (0xFFFFFFFFFFFFFFFF) pa se povećava se vraća brojač bloka za nula (bajtova 8 do 15) bez utjecaja na druge 64 bita od brojač (odnosno bajtova 0 do 7).   Da biste zadržali sigurnost način šifriranja AES GRUPU, InitializationVector vrijednost za dani ključ identifikator za svaku sadržaja ključ moraju biti jedinstvena za svaku datoteku i datoteke moraju biti manja od 2 ^ 64 blokovi u duljine.  To je da biste bili sigurni da vrijednost brojač ponovno se nikad ne koristi s ključem navedeni. Dodatne informacije o načinu GRUPU potražite u članku [toj wiki stranici](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (u wiki članku koristi izraz "Jednokratna šifra" umjesto "InitializationVector").

Korištenje **Šifriranja prostora za pohranu** za šifriranje Očisti sadržaj lokalno koristi AES 256 bitno šifriranje, a zatim prenese Azure za pohranu gdje je pohranjena šifriraju na ostale. Resursi koji su zaštićeni šifriranjem prostora za pohranu se automatski nešifrirani smještene u sustavu šifriranu datoteku prije no što kodiranja i po želji se ponovno šifrirane prije no što prenesete natrag kao novi izlaz resursa. Slučaj prvenstveno se koristi za pohranu šifriranje je kada želite sigurne visoke kvalitete unos medijske datoteke s šifriranjem na ostale na disku.

Da bi se isporučiti resursa za šifriranu prostora za pohranu, morate konfigurirati sredstva isporuke pravila da bi Media Services znali način za isporuku sadržaja. Prije moguće je strujanjem vaše resursa, strujanje poslužitelja uklanja šifriranje prostora za pohranu i strujanja sadržaja putem pravila navedeni isporuke (Ako, na primjer, AES, uobičajeni šifriranje ili nema šifriranja).

##<a name="create-contentkeys-used-for-encryption"></a>Stvaranje ContentKeys koji se koristi za šifriranje

Sredstva šifriranim mora biti povezan s ključem za šifriranje prostora za pohranu. Morate stvoriti tipku sadržaja koja će se koristiti za šifriranje prije stvaranja datoteke resursa. U ovom se odjeljku opisuje kako stvoriti sadržaja ključ.

Slijede općenite korake za stvaranje sadržaja tipke koje ćete pridružiti resursi koji želite da se šifrirati. 

1. Za šifriranje prostora za pohranu slučajno stvaranje 32-bajtni AES ključa. 

    To će biti sadržaja ključ za resursa, što znači da morate koristiti isti sadržaja ključa tijekom dešifriranja sve datoteke koje su povezane s tog resursa. 
2.  Nazovite [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) i [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) metode da biste dobili točne X.509 koji moraju se koristiti za šifriranje ključa sadržaja.
3.  Šifriranje ključa sadržaja s javnim ključem certifikat X.509. 

    Media Services .NET SDK koristi RSA sa OAEP pri izvođenju šifriranje.  Vidjet ćete primjer .NET u [EncryptSymmetricKeyData (opis funkcije)](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Stvaranje kontrolni zbroj vrijednosti izračunava se pomoću ključne identifikator i sadržaja ključ. U sljedećem primjeru .NET izračunava kontrolni zbroj pomoću GUID dio identifikator ključa i tipku Očisti sadržaj.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Stvaranje sadržaja ključa s **EncryptedContentKey** (pretvoriti u niz kodiran base64-om), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**i **kontrolni zbroj** vrijednosti koje ste primili u prethodnim koracima.

    
    Za šifriranje prostora za pohranu, sljedeća svojstva želite uvrstiti u tijelu zahtjeva.
     
    Svojstvo tijela zahtjev   | Opis
    ---|---
    ID-a | ContentKey Id koje ćemo nas generiranje pomoću sljedećih oblika "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | To je vrsta sadržaja ključa kao cijeli tog sadržaja ključa. Ne možemo proslijedite vrijednost 1 za šifriranje prostora za pohranu.
    EncryptedContentKey | Ćemo stvoriti novu vrijednost sadržaja ključa koji je vrijednost 256-bitni (32 bajta). Ključ je šifriran pomoću certifikata za X.509 šifriranje prostora za pohranu koji ne možemo dohvatiti iz servisa Microsoft Azure Media Services izvršavanjem zahtjev HTTP GET za GetProtectionKeyId i GetProtectionKey metode. Na primjer, pogledajte sljedeći kod .NET: **EncryptSymmetricKeyData** način definiran [ovdje](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | To je id ključa zaštitu certifikat za X.509 šifriranje prostora za pohranu koji je korišten za šifriranje naš sadržaja ključ.
    ProtectionKeyType | To je vrsta šifriranja za zaštitu ključ koji je korišten za šifriranje tipku sadržaja. U ovom se vrijednost koristi StorageEncryption(1) našeg primjera.
    Kontrolni zbroj |MD5 izračunatom kontrolnom zbroju tipke sadržaja. Izračunati je šifrira sadržaja ID-a pomoću tipku sadržaja. Kod primjer pokazuje kako izračunati na kontrolni zbroj.
    

###<a name="retrieve-the-protectionkeyid"></a>Dohvaćanje na ProtectionKeyId 
 

Sljedeći primjer prikazuje način za dohvaćanje ProtectionKeyId, otisak prsta certifikata, morate koristiti prilikom šifriranja sadržaja ključ certifikata. Učinite ovaj korak da biste bili sigurni da imate odgovarajuću potvrdu na vašem računalu.


Zahtjev:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Odgovor:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Dohvaćanje na ProtectionKey za na ProtectionKeyId

Sljedeći primjer prikazuje način za dohvaćanje X.509 certifikat pomoću na ProtectionKeyId koji ste primili u prethodnom koraku.

Zahtjev:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Odgovor:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>Stvaranje sadržaja ključa 

Nakon što dohvatiti certifikat X.509 i njegov javnim ključem za šifriranje koristi ključ sadržaja, stvorite **ContentKey** entitet, a zatim sukladno tome postaviti vrijednosti svojstva.

Jedna od vrijednosti koje morate postaviti kada stvorite sadržaj ključ je vrsta. U slučaju šifriranje prostora za pohranu, je vrijednost "1". 

Sljedeći primjer pokazuje kako stvoriti **ContentKey** sa skupom **ContentKeyType** za šifriranje prostora za pohranu ("1") i **ProtectionKeyType** postavljeno na "0" da biste naznačili da je zaštitni ključ Id otisak certifikat X.509.  


Zahtjev

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Odgovor:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Stvaranje imovine

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>Na ContentKey pridružiti imovine

Nakon stvaranja na ContentKey povezati s vaše resursa pomoću postupka $links, kao što je prikazano u sljedećem primjeru:
    
Zahtjev:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Odgovor:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Stvaranje programa AssetFile

Entitet [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) predstavlja videoisječka ili zvučnog datoteku pohranjenu u spremniku blob. Datoteku resursa uvijek je povezan s sredstvo, a sredstvo možda sadrži jednu ili više datoteka resursa. Zadatak Media Encoder usluge neće uspjeti ako je objekt datoteka resursa nije povezan s digitalnih datoteka u spremniku blob.

Imajte na umu da instancu **AssetFile** i stvarne medijske datoteke dva različita objekta. AssetFile instance sadrži metapodatke o medijske datoteke dok medijske datoteke sadrži stvarni medijskog sadržaja.

Nakon vaše digitalnim medijima datoteku prenijeli u spremniku blob, koristit ćete HTTP zahtjev **SPAJANJE** da biste ažurirali na AssetFile s informacijama o medijske datoteke (ne prikazuje se u ovoj temi). 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
