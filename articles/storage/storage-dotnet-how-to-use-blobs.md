<properties
    pageTitle="Početak rada s Azure blobova (objekt spremište) pomoću .NET | Microsoft Azure"
    description="Pohranite nestrukturirane podatke u oblaku s Azure blobova (spremište objekta)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Početak rada s Azure blobova pomoću .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

Azure blobova je servis koji pohranjuje nestrukturirane podatke u oblaku kao objekata/blob-ova. Spremište blobova platforme možete pohraniti sve vrste teksta ili binarne podatke, kao što su dokument, medijske datoteke ili program za instalaciju aplikacije. Spremište blobova platforme naziva se i spremište objekata.

### <a name="about-this-tutorial"></a>O ovog praktičnog vodiča

Pomoću ovog praktičnog vodiča pokazuje kako .NET kod za neke uobičajeni scenariji pomoću spremište blobova platforme Azure. Scenariji u kojima je moguće uvrstiti prijenos, popis, preuzimanje i brisanje blob-ova.

**Procijenjena vrijeme da biste dovršili:** 45 minuta

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure prostora za pohranu klijenta biblioteka za .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Upravitelj konfiguracije za .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Račun za Azure prostora za pohranu](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Dodatne primjere

Dodatni Primjeri uporabe blobova, potražite u članku [Uvod u spremište blobova platforme Azure u .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Možete preuzeti aplikaciju ogledne i pokrenite je ili potražite kod na GitHub.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Dodavanje prostora za naziv deklaracija

Dodajte sljedeće `using` naredbe na vrh na `program.cs` datoteke:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Raščlaniti niz za povezivanje

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Stvaranje klijentskog programa Blob servisa

Klase **CloudBlobClient** omogućuje vam dohvaćanje spremnika i pohranjene u blobova blob-ova. Ovdje je jedan od načina za stvaranje klijentskog programa servisa:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Sada ste spremni za pisanje koda koji se čita podatke iz i zapisuje podatke sa spremištem blobova.

## <a name="create-a-container"></a>Stvaranje spremnika

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Ovaj primjer pokazuje kako stvoriti spremnika ako se još ne postoji:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Prema zadanim postavkama, novi spremnik je privatno, što znači da odredite prostora za pohranu tipkovnog da biste preuzeli blob-ova iz ovaj spremnik za. Ako želite da bi datoteka u spremniku dostupni svima, možete postaviti spremnik biti javno pomoću koda za sljedeće:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Svi na internetu mogu vidjeti blob-ova u spremniku javno, ali možete izmijeniti ili ih izbrisati samo ako imate pristup tipku odgovarajući račun ili zajednički pristup potpis.

## <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Azure blobova podržava bloka blob-ova i blob-Ova stranica.  U većini slučajeva, blok blob je preporučena vrsta da biste koristili.

Da biste prenijeli datoteke na blob blok, upućivanje spremnik te ga koristiti upućivanje blob blok. Nakon što dodate blob referenca, možete prenijeti bilo koji niz podataka da biste ga tako da nazovete metodu **UploadFromStream** . Ovaj postupak će stvoriti blob-om ako niste već postoji, ili prebrisati ako postoji.

Sljedeći primjer pokazuje kako prenijeti blob u spremniku i pretpostavlja da spremnik već stvorili.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku

Da biste dobili popis blob-ova u spremniku se referenca kontejner. Kontejner s **ListBlobs** način možete koristiti za dohvaćanje blob-ova i/ili direktorija u njoj. Da biste pristupili bogatog skupa svojstva i postupci za vraćeni **IListBlobItem**, morate je oblikuje **CloudBlockBlob**, **CloudPageBlob**ili **CloudBlobDirectory** objekt.  Ako je vrsta Nepoznato, vrsta provjere možete koristiti da biste utvrdili koju želite da biste na anketu.  Sljedeći kod pokazuje kako dohvatiti i izlaz URI svaku stavku u `photos` spremnik:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

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

Kao što je prikazano gore, možete nazvati blob polja s informacije o putu njihova imena. Time se stvara strukturu virtualnog direktorija koje možete organizirati i prolaziti kao tradicionalni datotečni sustav. Imajte na umu da strukturu direktorija je virtualne samo - su samo resursi dostupni u spremište blobova platforme spremnika i blob-ova. Međutim, klijentska biblioteka za pohranu nudi **CloudBlobDirectory** objekta da bi prikazao virtualnog direktorija i pojednostavili postupak rad s blob-ova koji su organizirane na taj način.

Na primjer, razmotrite sljedeće skup bloka blob-ova u spremniku pod nazivom `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Kada poziv **ListBlobs** na spremnik "fotografije" (kao što je iznad uzorku), vraća se hijerarhijski popis. Sadrži **CloudBlobDirectory** i **CloudBlockBlob** objekte predstavlja direktorija i blob-ova u željeni spremnik. Rezultat izlaz izgleda ovako:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Po želji možete postaviti **UseFlatBlobListing** parametar metode **ListBlobs** na **true**. U ovom slučaju svaki blob u spremniku, vraća se kao **CloudBlockBlob** objekt. Poziv na **ListBlobs** da biste se vratili na plošnu unos izgleda ovako:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

a rezultat izgleda ovako:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Preuzimanje blob-ova

Da biste preuzeli blob-ova, najprije dohvaćanje blob reference i poziva metodu **DownloadToStream** . Sljedeći primjer koristi metodu **DownloadToStream** prenijeti sadržaj blob strujanje objekt koji se zatim može se zadržavaju u lokalnu datoteku.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Način **DownloadToStream** možete koristiti i da biste preuzeli sadržaj blob kao tekstni niz.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Brisanje blob-ova

Da biste izbrisali blob, najprije se referenca blob, a zatim poziva metodu **Brisanje** na njemu.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asinkrono popis blob polja na stranicama

Ako se popis velik broj blob-ova ili ako želite upravljati broj rezultata koji se vraćate stavku odjednom, može prikazati blob polja na stranicama s rezultatima. U ovom se primjeru pokazuje kako se vratiti rezultate na stranicama asinkrono, tako da se izvršavanje je blokirana tijekom čekanja da biste se vratili na velike skup rezultata.

U ovom se primjeru prikazuju paušalni blob stavku, ali hijerarhijski popis možete izvesti i tako da postavite na `useFlatBlobListing` parametar metode **ListBlobsSegmentedAsync** `false`.

Jer je način uzorak poziva asinkronog način, ga mora biti prefaced s na `async` ključnoj riječi i ona mora vratiti objekt **zadatka** . Ključna riječ await za metodu **ListBlobsSegmentedAsync** određen obustavlja izvršavanje metodu uzorka dok se dovršava unos zadatka.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
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

## <a name="writing-to-an-append-blob"></a>Pisanje programa blob s dodavanjem

Programa blob s dodavanjem nije novu vrstu blob, uvodi verzije 5.x biblioteke Azure prostora za pohranu klijenta za .NET. U upit s dodavanjem blob optimiziran je za dodavanjem operacije, kao što su zapisivanja. Kao što je blok blob, u upit s dodavanjem blob sastoji se od blokova, no prilikom dodavanja novog bloka programa dodavanjem blob je uvijek dodaje na kraj blob-om. Ne možete ažurirati ili izbrisati postojeće blok u programa dodavanjem blob. Blokiranje ID-a za programa dodavanjem blob su vidljiva kao što je blok blob se nalazili.

Svaki blok u programa dodavanjem blob može biti različitih veličina, maksimalno 4 MB do i programa dodavanjem blob može sadržavati najviše 50.000 blokova. Maksimalna veličina programa dodavanjem blob stoga je malo više od 195 GB (4 MB X 50.000 blokove).

U primjeru u nastavku stvara novi upit s dodavanjem blob i dodaje neke podatke, simulaciju operacija jednostavne zapisivanja.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Potražite u članku [objašnjenje bloka blob-ova, stranice blob-ova i dodavanje blob-ova](https://msdn.microsoft.com/library/azure/ee691964.aspx) za dodatne informacije o razlikama između tri vrste blob-ova.

## <a name="managing-security-for-blobs"></a>Upravljanje sigurnošću za blob-ova

Prema zadanim postavkama Azure prostora za pohranu čuva podatke sigurne ograničavanjem pristupa vlasnika računa koja se nalazi na ima tipke za pristup računu. Kada morate omogućiti zajedničko korištenje blob podatke na računu za pohranu, važno je da biste to učinili bez ugrožavanja sigurnosti tipke za pristup računu. Osim toga, možete šifrirati blob podataka da biste bili sigurni da je sigurna prelaska putem na žičani te u odjeljku pohrana Azure.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Upravljanje pristupom podacima blobova platforme

Blob podatke na računu za pohranu po zadanom je dostupan samo vlasnik računa za pohranu. Provjera autentičnosti zahtjeva protiv blobova potreban ključ za račun programa access prema zadanim postavkama. Međutim, možda želite omogućiti određenih podataka blob drugim korisnicima. Imate dvije mogućnosti:

- **Anonimni pristup:** Možete učiniti spremnik ili njegov blob-ova javno dostupnim za anonimni pristup. Dodatne informacije potražite u članku [Upravljanje anonimni pristup za čitanje spremnika i blob-ova](storage-manage-access-to-resources.md) .
- **Zajednički se koristi access potpisi:** Klijenti može dati zajednički pristup potpis (SAS), koji pruža ovlaštenog pristup resursu na vašem računu za pohranu s dozvolama koje ste odredili i intervalu koji navedete. Dodatne informacije potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Šifriranje blob podataka

Azure prostora za pohranu podržava šifriranje blob podataka na klijentskom računalu i na poslužitelju:

- **Klijentsko šifriranja:** U biblioteci za pohranu klijenta za .NET podržava šifriranje podataka u klijentskim aplikacijama prije prijenosa Azure prostora za pohranu i dešifriranje podataka tijekom preuzimanja klijentu. Biblioteku podržava i integracija s sigurnog Azure ključ za upravljanje ključevima za račun za pohranu. Dodatne informacije potražite u članku [Klijentsko šifriranja s .NET za spremište na platformi Microsoft Azure](storage-client-side-encryption.md) . Vidjeti [Praktični vodič: šifriranje i dešifriranje blob-ova u Azure spremište na platformi Microsoft Azure ključ sigurnog korištenja](storage-encrypt-decrypt-blobs-key-vault.md).
- **Poslužiteljsko šifriranja**: Azure prostora za pohranu sada podržava šifriranje na strani poslužitelja. U odjeljku [šifriranje servisa Azure prostora za pohranu za podatke na ostale (pretpregled)](storage-service-encryption.md).

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove blobova, slijedite ove veze da biste saznali više.

### <a name="microsoft-azure-storage-explorer"></a>Explorer prostora za pohranu za Microsoft Azure
- [Microsoft Azure prostora za pohranu Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) je besplatne, samostalne aplikacije od Microsofta koji omogućuje rad vizualno s podacima Azure prostora za pohranu u sustavu Windows, OS X i Linux.

### <a name="blob-storage-samples"></a>Uzorci spremišta blobova platforme

- [Uvod u spremište blobova platforme Azure u .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Referenca spremište blobova platforme

- [Prostor za pohranu klijenta biblioteka za .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [Referenca REST API-JA](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Konceptualni vodilice

- [Prijenos podataka s pomoćnog programa naredbenog retka AzCopy](storage-use-azcopy.md)
- [Početak rada s pohrani za .NET](storage-dotnet-how-to-use-files.md)
- [Kako koristiti blobova platforme Azure s WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
