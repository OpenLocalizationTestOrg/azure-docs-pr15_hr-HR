<properties
    pageTitle="Početak rada s blob prostora za pohranu i Visual Studio povezani services (Servisi u oblaku) | Microsoft Azure"
    description="Upute za početak rada s spremište blobova platforme Azure u oblak servisa projekta u Visual Studio nakon povezivanja s računom za pohranu pomoću Visual Studio povezani servisi"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Početak rada s spremište blobova platforme Azure i Visual Studio povezani services (oblaka services projekata)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako započeti s spremište blobova platforme Azure nakon koje ste stvorili ili poziva račun za pohranu Azure pomoću dijaloški okvir za Visual Studio **Dodavanje povezani servisi** u programu project Visual Studio oblaka services. Pokazat ćemo vam kako pristupiti i stvaranje spremnika blob te kako izvoditi uobičajene zadatke kao što su prijenos, unos i preuzimanje blob-ova. Primjere zapisuju u C\# i koristite [Microsoft Azure prostora za pohranu klijenta biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure blobova je servis za pohranu velike količine nestrukturirane podatke koje je moguće pristupiti s bilo kojeg mjesta na svijetu putem HTTP ili HTTPS. Jedan blob može biti bilo koje veličine. Blob-ova može biti elemente kao što su slike, audio i videodatoteka, sirovim podacima i datoteke dokumenata.

Baš kao i datoteke live u mapama, blob polja za pohranu žive spremnika. Nakon što ste stvorili u prostor za pohranu, stvorite jedan ili više spremnika u prostora za pohranu. Na primjer, u spremište pod nazivom "Slika", možete stvoriti spremnika u spremište pod nazivom "slike" za pohranu slika i drugog naziva "audio" da biste pohranili audiodatoteke. Nakon stvaranja spremnike blob pojedinačne datoteke možete prenijeti na njih.

- Dodatne informacije o programski rukovanje blob-ova, potražite u članku [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md).
- Opće informacije o Azure prostora za pohranu potražite u [dokumentaciji za pohranu](https://azure.microsoft.com/documentation/services/storage/).
- Opće informacije o servise u Oblaku Azure potražite u [dokumentaciji za servise u Oblaku](https://azure.microsoft.com/documentation/services/cloud-services/).
- Dodatne informacije o programskom ASP.NET aplikacija potražite u članku [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Pristup blob spremnika u kodu

Programatski pristup blob-ova u oblak servisa projektima, morate dodati sljedeće stavke ako nisu već postoji.

1. Dodajte sljedeće deklaracija prostor naziva kod na vrh C# datoteke u kojima želite li programatski pristup Azure prostora za pohranu.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Pronađite objekt **CloudStorageAccount** koji predstavlja prostor za pohranu podataka o računu. Koristite sljedeći kod da biste u nizu za povezivanje za pohranu i podataka o računu za pohranu konfiguraciju servisa Azure.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Pronađite objekt **CloudBlobClient** referentni postojeće spremnik na vašem računu za pohranu.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Pronađite objekt **CloudBlobContainer** referentni spremniku određene blob.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Koristiti sve kôda prikazanog iz prethodnog postupka ispred kôda prikazanog u sljedećim odjeljcima.

## <a name="create-a-container-in-code"></a>Stvaranje spremnika u kodu

> [AZURE.NOTE] Neki API-ji koji se izvode pozive izvan Azure prostora za pohranu u ASP.NET su asinkronog. Dodatne informacije potražite [Asynchronous programiranje asinkrone i Await](http://msdn.microsoft.com/library/hh191443.aspx) . Kod u sljedećem primjeru pretpostavlja da koristite asinkrone programiranje metode.

Stvaranje spremnika u vašem računu za pohranu, sve što trebate napraviti je dodavanje poziv **CreateIfNotExistsAsync** kao sljedeći kod:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Da biste unijeli datoteka u spremniku dostupni svima, možete postaviti spremnik biti javno pomoću sljedeći kod.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Svi na internetu mogu vidjeti blob-ova u spremniku javno, ali možete izmijeniti ili ih izbrisati samo ako imate odgovarajuće tipkovni prečac.

## <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Azure prostora za pohranu podržava bloka blob-ova i blob-Ova stranica. U većini slučajeva, blok blob je preporučena vrsta da biste koristili.

Da biste prenijeli datoteke na blob blok, upućivanje spremnik te ga koristiti upućivanje blob blok. Nakon što dodate blob referenca, možete prenijeti bilo koji niz podataka da biste ga tako da nazovete metodu **UploadFromStream** . Ovaj postupak stvara blob-om ako niste već postoji ili prebrisuje ako postoji. Sljedeći primjer pokazuje kako prenijeti blob u spremniku i pretpostavlja da spremnik već stvorili.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku

Da biste dobili popis blob-ova u spremniku se referenca kontejner. Kontejner s **ListBlobs** način možete koristiti za dohvaćanje blob-ova i/ili direktorija u njoj. Da biste pristupili bogatog skupa svojstva i postupci za vraćeni **IListBlobItem**, morate je oblikuje **CloudBlockBlob**, **CloudPageBlob**ili **CloudBlobDirectory** objekt. Ako je vrsta Nepoznato, vrsta provjere možete koristiti da biste utvrdili koju želite da biste na anketu. Sljedeći kod pokazuje kako dohvatiti i izlaz URI svake stavke u spremniku za **fotografije** :

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

Kao što je prikazano u prethodnom primjeru koda, servis blob ima pojam direktorija unutar spremnika, kao i. To je tako da možete organizirati blob-ova u strukturi više mapa sviđa. Na primjer, razmotrite sljedeće skup bloka blob-ova u kontejner **fotografije**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Kada poziv **ListBlobs** na spremnik (kao što je prethodni uzorak), zbirke vraća sadrži **CloudBlobDirectory** i **CloudBlockBlob** objekata koji predstavlja direktorija i blob polja koje se nalaze na najvišoj razini. Evo dobivene izlaz:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Po želji možete postaviti **UseFlatBlobListing** parametar metode **ListBlobs** na **true**. Rezultira svaki blob se vraćaju kao u **CloudBlockBlob**, bez obzira na to direktorija. Evo poziv na **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

i Evo rezultate:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Dodatne informacije potražite u članku [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Preuzimanje blob-ova

Da biste preuzeli blob-ova, najprije dohvaćanje blob reference i poziva metodu **DownloadToStream** . Sljedeći primjer koristi metodu **DownloadToStream** prenijeti sadržaj blob strujanje objekt koji se zatim može zadržava u lokalnu datoteku.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Način **DownloadToStream** možete koristiti i da biste preuzeli sadržaj blob kao tekstni niz.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Brisanje blob-ova

Da biste izbrisali blob, najprije se referenca blob i poziva način za **Brisanje** .

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asinkrono popis blob polja na stranicama

Ako se popis velik broj blob-ova ili ako želite upravljati broj rezultata koji se vraćate stavku odjednom, može prikazati blob polja na stranicama s rezultatima. U ovom se primjeru pokazuje kako se vratiti rezultate na stranicama asinkrono, tako da se izvršavanje je blokirana tijekom čekanja da biste se vratili na velike skup rezultata.

U ovom se primjeru prikazuju paušalni blob stavku, ali hijerarhijski popis možete izvesti i tako da postavite parametar **useFlatBlobListing** metode **ListBlobsSegmentedAsync** **False**.

Jer je način uzorak poziva asinkronog način, mora biti prefaced s **asinkrone** ključnu riječ, a ona mora vratiti objekt **zadatka** . Await ključnu riječ za metodu **ListBlobsSegmentedAsync** određen obustavlja izvršavanje metodu uzorka dok se ne dovrši unos zadatka.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
