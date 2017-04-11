<properties 
    pageTitle="Stvaranje ContentKeys s .NET" 
    description="Saznajte kako stvoriti sadržaja prečaci koji se Omogućivanje sigurnog pristupa resursi." 
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


#<a name="create-contentkeys-with-net"></a>Stvaranje ContentKeys s .NET

> [AZURE.SELECTOR]
- [OSTALE](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media Services omogućuje vam stvaranje i isporuka šifrirane resursi. **ContentKey** omogućuje siguran pristup vaše **resursa**s. 

Kada stvorite novu imovinu (na primjer, prije [prijenosa datoteka](media-services-dotnet-upload-files.md)), možete odrediti sljedeće mogućnosti šifriranja: **StorageEncrypted**, **CommonEncryptionProtected**ili **EnvelopeEncryptionProtected**. 

Kada isporuka imovine klijentima, možete [konfigurirati za imovinu dinamički šifriranje](media-services-dotnet-configure-asset-delivery-policy.md) s jednim od sljedeće dvije encryptions: **DynamicEnvelopeEncryption** ili **DynamicCommonEncryption**.

Sredstva šifriranim moraju biti povezane s **ContentKey**s. U ovom se članku opisuje kako stvoriti sadržaja ključ.

>[AZURE.NOTE] Prilikom stvaranja novog **StorageEncrypted** resursa pomoću Media Services .NET SDK **ContentKey** automatski se stvara i povezane s sredstava.

##<a name="contentkeytype"></a>ContentKeyType

Jedna od vrijednosti koje morate postaviti kada stvorite sadržaja ključ je vrsta sadržaja ključa. Odaberite neku od sljedećih vrijednosti. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Stvaranje vrste omotnice ContentKey

Sljedeći isječak koda stvara sadržaja ključ šifriranja vrste omotnice. Zatim povezuje tipku s navedenom resursa.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

poziv

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Stvaranje vrstu običnih ContentKey    

Sljedeći isječak koda stvara sadržaja ključ vrstu običnih šifriranje. Zatim povezuje tipku s navedenom resursa.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
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
poziv

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
