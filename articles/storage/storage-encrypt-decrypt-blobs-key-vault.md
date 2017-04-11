<properties
    pageTitle="Praktični vodič: Šifriranje i dešifriranje blob-ova u Azure spremište na platformi Microsoft Azure ključ sigurnog korištenja | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz šifriranje i dešifriranje blob šifriranjem klijentsko za spremište na platformi Microsoft Azure s sigurnog ključ Azure."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Praktični vodič: Šifriranje i dešifriranje blob-ova u Azure spremište na platformi Microsoft Azure ključ sigurnog korištenja

## <a name="introduction"></a>Uvod

Pomoću ovog praktičnog vodiča opisuje način na koji koristite za pohranu klijentsko šifriranja s sigurnog ključ Azure. Ga vodit će vas kroz šifriranje i dešifriranje blob u konzole aplikacije pomoću tih tehnologija.

**Procijenjena vrijeme da biste dovršili:** 20 minuta

Pregled informacija o sigurnog ključ Azure, potražite u članku [što je sigurnog ključ Azure?](../key-vault/key-vault-whatis.md).

Pregled informacija o klijentsko šifriranje za pohranu Azure, potražite u članku [klijentsko šifriranja i Azure sigurnog ključ za spremište na platformi Microsoft Azure](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, morate imati sljedeće:

- Račun za Azure prostora za pohranu
- Visual Studio 2013 ili noviji
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Pregled klijentsko šifriranja

Da biste saznali klijentsko šifriranje za Azure prostora za pohranu potražite u članku [klijentsko šifriranja i Azure sigurnog ključ za spremište na platformi Microsoft Azure](storage-client-side-encryption.md)

Ovdje je kratak opis funkcioniranja šifriranja na strani klijenta:

1. Azure prostora za pohranu klijenta SDK generira ključ šifriranja sadržaja (CEK), što je ključ simetričnu one-time se koristi.
2. Podatke o klijentu je šifriran pomoću ovog CEK.
3. Na CEK pa je omotan (šifrirane i) pomoću ključa za šifriranje ključ (KEK). Na KEK označena ključa identifikator i mogu biti asimetričnim ključa par ili simetričnu ključ i možete se upravlja lokalno ili spremljene u Azure ključ sigurnog. Prostor za pohranu klijenta sam nikad ima pristup na KEK. Samo se poziva algoritam ključa prelamanje koju je dodijelio sigurnog ključ. Da biste koristili prilagođeni davatelja usluga za ključ prelamanje/unwrapping po želji možete odabrati klijentima.
4. Šifrirani podaci pa prenesene sa servisom Azure prostora za pohranu.


## <a name="set-up-your-azure-key-vault"></a>Postavljanje sustava sigurnog ključ Azure
Da biste nastavili s ovog praktičnog vodiča, trebali biste učiniti sljedeće korake koje su navedene u ovom praktičnom vodiču [Početak rada s sigurnog Azure ključ](../key-vault/key-vault-get-started.md):

- Stvaranje ključa zbirke ključeva.
- Dodajte ključ ili tajna ključa zbirke ključeva.
- Registracija aplikacije pomoću servisa Azure Active Directory.
- Autorizirajte aplikaciju koju želite koristiti ključ ili tajna.

Zabilježite ClientID i ClientSecret koji su generirani prilikom registriranja aplikacije pomoću servisa Azure Active Directory.

Stvaranje i tipke u ključa zbirke ključeva. Pretpostavimo da za ostale vodiča za koju ste koristili sljedeće nazive: ContosoKeyVault i TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Stvaranje aplikacije konzole za pakete i AppSettings

U Visual Studio, stvorite novu aplikaciju konzolu.

Dodajte potrebne nuget paketa na konzoli za Upravitelj paketa.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Dodajte AppSettings u App.Config.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Dodajte sljedeće `using` izvješća i provjerite je li dodati referenca System.Configuration u projekt.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Dodavanje načina da biste dobili token u aplikaciji konzole

Klase sigurnog ključ koji su potrebni za provjeru autentičnosti za pristup vašem ključa sigurnog koristi sa sljedećim korakom.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Pristup i sigurnog ključ prostor za pohranu u programu

U funkciji glavne dodati sljedeći kod.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Modeli ključa sigurnog objekta
>
>Je važno je znati da postoje zapravo dva ključ sigurnog modeli objekta Imajte na umu: jedno se temelji na REST API-JA (KeyVault prostor naziva) i drugi je datotečni nastavak za šifriranje klijentsko.

> Klijent sigurnog ključ stupi u interakciju s REST API-JA, a možete koristiti tipke za JSON Web i tajne za dvije vrste stvari koje se nalaze u sigurnog ključ.

> Proširenja sigurnog ključ su klase koje čini posebno stvorene za šifriranje klijentsko u Azure prostora za pohranu. Sadrže sučelje za tipke (IKey) i klase na temelju pojam ključ prevoditelja. Postoje dvije implementacije IKey koje su vam potrebne informacije: RSAKey i SymmetricKey. Sada se dogoditi da bi odgovarali stvari koje se nalaze u ključ sigurnog, ali sada su klase nezavisnih (tako da ključ i tajna dohvate sigurnog klijent ključ implementirati IKey).


## <a name="encrypt-blob-and-upload"></a>Šifriranje blob i prijenos
Dodajte sljedeći kod šifriranje blob i prenijeti na račun za Azure prostora za pohranu. Način **ResolveKeyAsync** koji se koristi vraća programa IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Slijedi snimka zaslona s [Azure klasični Portal](https://manage.windowsazure.com) za blob zaštićenu pomoću klijentsko šifriranja s ključem pohranjene u sigurnog ključ. Svojstvo **KeyId** je URI za ključ u sigurnog ključ koji funkcionira kao u KEK. Svojstvo **EncryptedKey** sadrži šifrirane verziju na CEK.

![Snimka zaslona s prikazom Blob metapodataka koja sadrži metapodatke šifriranje][1]

> [AZURE.NOTE] Ako pogledate Graditelj BlobEncryptionPolicy, vidjet ćete da možete prihvatiti ključ i/ili na prevoditelja. Imajte na umu koji desno sada ne možete koristiti u prevoditelja za šifriranje jer ne trenutno ne podržavaju ključa zadani.



## <a name="decrypt-blob-and-download"></a>Šifriranje blob i preuzimanje
Dešifriranje ustvari prilikom korištenja klase Razrješavanje smisla. ID ključa koji se koristi za šifriranje povezan je s blob-om u njegove metapodatke tako da nema razloga za dohvaćanje tipku i Imajte na umu veza između ključ i blob. Imate da biste bili sigurni da se tipku ostaje u sigurnog ključ.   

Privatni ključ RSA ključ ostaje u sigurnog ključ tako da za dešifriranje pojavljivati tipku šifrirane s blob metapodataka koja sadrži na CEK se šalje sigurnog ključ za dešifriranje.

Dodajte na sljedeći način dešifrirali blob koju ste upravo učitali.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Postoji nekoliko druge vrste resolvers da biste olakšali upravljanje ključevima, uključujući: AggregateKeyResolver i CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Korištenje ključa sigurnog tajne
Da biste koristili u tajna šifriranjem klijentsko se putem klase SymmetricKey jer je tajna je zapravo simetričnu ključ. No, kao što je naznačeno iznad, tajna u sigurnog ključa ne odgovara točno u SymmetricKey. Objašnjenje nekoliko stvari:


- Ključ u na SymmetricKey mora biti fiksne duljine: 128, 192, 256, 384 ili 512 bitova.
- Ključ u na SymmetricKey mora biti Base64 kodiran.
- Tajna sigurnog ključ koji će se koristiti kao u SymmetricKey mora imati vrstu sadržaja "octet/aplikacije-strujanje" u sigurnog ključ.

Evo jednog primjera u ljusci PowerShell stvaranja na tajna u sigurnog ključ koji se mogu koristiti kao u SymmetricKey.
Napomena: Konačnog kodirani vrijednost $key, je samo u svrhu pokazni. U vlastiti kod ćete htjeti generiranje ovog ključa.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

U aplikaciji konzole možete koristiti isti poziv kao prije dohvatiti ovaj tajna kao u SymmetricKey.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

To je to. Uživajte!

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju spremište na platformi Microsoft Azure s C# potražite u članku [Microsoft Azure prostora za pohranu klijenta biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Dodatne informacije o Blob REST API-JA potražite u članku [Blob servisa REST API -JA](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Najnovije informacije na spremište na platformi Microsoft Azure, idite na [Blog tima za Microsoft Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
