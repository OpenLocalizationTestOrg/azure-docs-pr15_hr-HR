<properties
    pageTitle="Početak rada s blob prostora za pohranu i Visual Studio povezani servisi (ASP.NET 5) | Microsoft Azure"
    description="Kako započeti rad sa sustavom spremište blobova platforme Azure u programu project za Visual Studio ASP.NET 5 nakon stvaranja prostora za pohranu računa pomoću Visual Studio povezani servisi"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Početak rada s blobova platforme Azure prostora za pohranu i Visual Studio povezani servisi (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Pregled

U ovom se članku opisuje kako započeti rad sa sustavom spremište blobova platforme Azure u Visual Studio nakon što ste stvorili ili račun za Azure prostora za pohranu u projektu ASP.NET 5 pozivaju pomoću dijaloškog okvira Visual Studio dodavanje povezani servisi.

Azure blobova je servis za pohranu velike količine nestrukturirane podatke koje je moguće pristupiti s bilo kojeg mjesta na svijetu putem HTTP ili HTTPS. Jedan blob može biti bilo koje veličine. Blob-ova može biti elemente kao što su slike, audio i videodatoteka, sirovim podacima i datoteke dokumenata. U ovom se članku opisuje način za početak rada s blobova nakon što stvorite račun za Azure prostora za pohranu pomoću dijaloški okvir za Visual Studio **Dodavanje povezani servisi** u projektu ASP.NET 5.

Baš kao i datoteke live u mapama, blob polja za pohranu žive spremnika. Nakon što ste stvorili u prostor za pohranu, stvorite jedan ili više spremnika u prostora za pohranu. Na primjer, u spremište pod nazivom "Slika", možete stvoriti spremnika u spremište pod nazivom "slike" za pohranu slika i drugog naziva "audio" da biste pohranili audiodatoteke. Nakon stvaranja spremnike blob pojedinačne datoteke možete prenijeti na njih. [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md) potražite dodatne informacije o programski rukovanje blob-ova.

##<a name="access-blob-containers-in-code"></a>Pristup blob spremnika u kodu

Programatski pristup blob-ova u ASP.NET 5 projektima, morate dodati sljedeće stavke ako nisu već postoji.

1. Dodajte sljedeće deklaracija prostor naziva kod na vrh C# datoteke u kojima želite li programatski pristup Azure prostora za pohranu.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Pronađite objekt **CloudStorageAccount** koji predstavlja prostor za pohranu podataka o računu. Koristite sljedeći kod da biste u nizu za povezivanje za pohranu i podataka o računu za pohranu konfiguraciju servisa Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **BILJEŠKE:** Koristiti sve iznad kod ispred koda u sljedećim odjeljcima.


3. Pomoću **CloudBlobClient** objekt referenca **CloudBlobContainer** postojeće spremnik na vašem računu za pohranu.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Stvaranje spremnika u kodu

**CloudBlobClient** možete koristiti i za stvaranje spremnika u vašem računu za pohranu. Sve što trebate učiniti tako da dodate poziv **CreateIfNotExistsAsync** kao sljedeći kod:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**BILJEŠKE:** API-ji izvršiti pozive Azure prostora za pohranu u ASP.NET 5 su asinkronog. Dodatne informacije potražite [Asynchronous programiranje asinkrone i Await](http://msdn.microsoft.com/library/hh191443.aspx) . Kod ispod pretpostavlja da koriste asinkrone programiranja metode.

Da biste unijeli datoteka u spremniku dostupni svima, možete postaviti spremnik biti javno pomoću sljedeći kod.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Da biste prenijeli datoteke blob u spremniku, upućivanje spremnik te ga koristiti da biste dobili blob referencu. Nakon što ste blob reference, možete prenijeti sve strujanje podataka da biste ga tako da nazovete metodu **UploadFromStreamAsync** . Ovaj postupak stvara blob-om ako već nije ondje ili prebrisuje ako postoji. Sljedeći primjer pokazuje kako prenijeti blob u spremniku i pretpostavlja da spremnik već stvorili.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku
Da biste dobili popis blob-ova u spremniku se referenca kontejner. Zatim možete nazvati kontejner s **ListBlobsSegmentedAsync** način za dohvaćanje blob-ova i/ili direktorija u njoj. Da biste pristupili bogatog skupa svojstva i postupci za vraćeni **IListBlobItem**, morate je oblikuje **CloudBlockBlob**, **CloudPageBlob**ili **CloudBlobDirectory** objekt. Ako ne znate vrstu blob provjere vrsta možete koristiti da biste utvrdili koju želite da biste na anketu. Sljedeći kod pokazuje kako dohvatiti i izlaz URI svake stavke u spremniku.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
            }

            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob pageBlob = (CloudPageBlob)item;

                Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
            }

            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory directory = (CloudBlobDirectory)item;

                Console.WriteLine("Directory: {0}", directory.Uri);
            }
        }
    } while (token != null);

Načina drugima da biste dobili popis sadržaja spremnika blob. Dodatne informacije potražite [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Preuzimanje blob
Da biste preuzeli blob, najprije se referenca blob-om, a poziva metodu **DownloadToStreamAsync** . Sljedeći primjer koristi **DownloadToStreamAsync** način na koji želite prenijeti sadržaj blob strujanje objekt koji možete spremiti kao lokalne datoteke.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Postoje drugi načini spremanje blob-ova u obliku datoteke. Dodatne informacije potražite [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Brisanje blob
Da biste izbrisali blob, najprije se referenca blob-om, a zatim poziva metodu **DeleteAsync** na njemu.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
