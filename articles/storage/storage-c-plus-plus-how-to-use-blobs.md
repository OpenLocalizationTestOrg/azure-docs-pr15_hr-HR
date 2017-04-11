<properties
    pageTitle="Upute za korištenje spremišta blobova (spremište objekta) iz C++ | Microsoft Azure"
    description="Pohranite nestrukturirane podatke u oblaku s Azure blobova (spremište objekta)."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-blob-storage-from-c"></a>Upute za korištenje spremišta blobova iz C++  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

Azure blobova je servis koji pohranjuje nestrukturirane podatke u oblaku kao objekata/blob-ova. Spremište blobova platforme možete pohraniti sve vrste teksta ili binarne podatke, kao što su dokument, medijske datoteke ili program za instalaciju aplikacije. Spremište blobova platforme naziva se i spremište objekata.

Ovaj vodič će demonstrirati kako izvoditi uobičajene scenarije putem servisa za spremište blobova platforme Azure. Primjere zapisuju u C++ i koristite [Azure prostora za pohranu klijenta biblioteku C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scenariji u kojima je moguće uvrstiti **Prijenos**, **popis**, **Preuzimanje**i **Brisanje** blob-ova.  

>[AZURE.NOTE] Ovaj vodič pronalaze klijentska biblioteka za pohranu Azure C++ verziju 1.0.0 i iznad. Preporučena verzija je biblioteka za pohranu klijenta 2.2.0, koji je dostupan putem [NuGet](http://www.nuget.org/packages/wastorage) ili [GitHub](https://github.com/Azure/azure-storage-cpp).

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Stvaranje aplikacije C++
U ovom vodiču će se koristiti za pohranu značajke koje se može pokrenuti unutar C++ aplikacije.  

Da biste to učinili, morat ćete instalirati biblioteku Azure prostora za pohranu klijenta za C++ i stvaranje računa Azure prostora za pohranu u pretplatu za Azure.   

Da biste instalirali biblioteku Azure prostora za pohranu klijenta za C++, možete koristiti na sljedeće načine:

-   **Linux:** Slijedite upute navedene na stranici [Azure prostora za pohranu klijenta biblioteku C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** U Visual Studio, kliknite **Alati > Upravitelj paketa NuGet > Upravitelj paketa konzole**. Na [konzoli upravitelja NuGet paketa](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) upišite sljedeću naredbu i pritisnite **ENTER**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Konfiguriranje aplikacija za pristup spremište blobova platforme  
Dodavanje obuhvaćaju sljedeće naredbe na vrh C++ datoteku koju želite koristiti Azure prostora za pohranu API-ji za pristup blob-ova:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Postavljanje niz za povezivanje programa Azure prostora za pohranu
Klijent za Azure prostora za pohranu koristi niza za povezivanje za pohranu za pohranu krajnje točke i vjerodajnice za pristup servisa za upravljanje podacima. Kada se pokrene u klijentskoj aplikaciji, morate unijeti niz za povezivanje za pohranu u sljedećem obliku pomoću naziv računa za pohranu i tipkovni prečac za pohranu za račun za pohranu koji je naveden u [Azure Portal](https://portal.azure.com) za *AccountName* i *AccountKey* vrijednosti. Informacije o računima za pohranu i pristupnih tipki, potražite u članku [O računi servisa Azure prostora za pohranu](storage-create-storage-account.md). Ovaj primjer pokazuje kako deklarirati statične polja držite niz za povezivanje:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Da biste testirali vaše aplikacije u lokalnom računalu sa sustavom Windows, možete koristiti Microsoft Azure [emulator prostora za pohranu](storage-use-emulator.md) koje se instaliraju pomoću [Azure SDK](https://azure.microsoft.com/downloads/). Prostor za pohranu emulator je utility koji simulira Blob, reda čekanja i tablice usluga dostupni servisu Azure na vašem računalu lokalne razvoj. Sljedeći primjer prikazuje način deklarirati statične polja držite niz za povezivanje za svoje lokalno spremište emulator:

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Da biste započeli emulator Azure prostora za pohranu, odaberite gumb **Start** ili pritisnite tipku s logotipom **sustava Windows** . Počnite pisati **Emulator Azure prostora za pohranu**i odaberite **Microsoft Azure prostora za pohranu Emulator** s popisa aplikacija sustava.  

Sljedeća primjera pretpostavlja da koristite jedan od ta dva načina za dohvaćanje niza veze za pohranu.  

## <a name="retrieve-your-connection-string"></a>Dohvaćanje niza za povezivanje
Klase **cloud_storage_account** možete koristiti za prikaz podataka o računu za pohranu. Za dohvaćanje podataka o računu za pohranu iz niza za povezivanje za pohranu, možete koristiti metodu **raščlaniti** .  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Nakon toga se referenca **cloud_blob_client** predmete kao što je omogućuje vam da dohvatite objekata koji predstavljaju spremnika i pohranjeni u servis za pohranu Blob blob-ova. Sljedeći kod stvara objekt **cloud_blob_client** pomoću objekta račun za pohranu smo dohvatiti iznad:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Kako: Stvaranje spremnika

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Ovaj primjer pokazuje kako stvoriti spremnik ako se još ne postoji:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Prema zadanim postavkama, novi spremnik je privatna i potrebno je navesti prostora za pohranu tipkovnog da biste preuzeli blob polja s ovom spremniku. Ako želite da bi datoteke (blob-ova) unutar spremnik dostupni svima, možete postaviti spremnik biti javno pomoću koda za sljedeće:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Svi na internetu mogu vidjeti blob-ova u spremniku javno, ali možete izmijeniti ili ih izbrisati samo ako imate odgovarajuće tipkovni prečac.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Kako: prenijeti blob u spremniku
Azure blobova podržava bloka blob-ova i blob-Ova stranica. U većini slučajeva, blok blob je preporučena vrsta da biste koristili.  

Da biste prenijeli datoteke na blob blok, upućivanje spremnik te ga koristiti upućivanje blob blok. Nakon što dodate blob referenca, možete prenijeti bilo koji niz podataka da biste ga tako da nazovete metodu **upload_from_stream** . Ovaj postupak će stvoriti blob-om ako niste već postoji, ili prebrisati ako postoji. Sljedeći primjer pokazuje kako prenijeti blob u spremniku i pretpostavlja da spremnik već stvorili.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Osim toga, možete koristiti metodu **upload_from_file** da biste prenijeli datoteke na blob blok.

## <a name="how-to-list-the-blobs-in-a-container"></a>Kako: popis blob-ova u spremniku
Da biste dobili popis blob-ova u spremniku se referenca kontejner. Kontejner s **list_blobs** način možete koristiti za dohvaćanje blob-ova i/ili direktorija u njoj. Da biste pristupili bogatog skupa svojstva i postupci za vraćene **list_blob_item**, mora se pozvati metodu **list_blob_item.as_blob** da biste dobili **cloud_blob** objekt ili metode **list_blob.as_directory** da biste dobili cloud_blob_directory objekt. Sljedeći kod pokazuje kako dohvatiti i izlaz URI svake stavke u spremniku **Moje-uzorka – spremnika** :

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Dodatne informacije o popisu operacija, potražite u članku [Popis resursa za pohranu Azure u C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Kako: preuzimanje blob-ova
Da biste preuzeli blob-ova, najprije dohvaćanje blob reference i poziva metodu **download_to_stream** . Sljedeći primjer koristi metodu **download_to_stream** prenijeti sadržaj blob strujanje objekt koji se zatim može zadržava u lokalnu datoteku.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Osim toga, možete koristiti metodu **download_to_file** preuzeti sadržaj blob u datoteku.
Osim toga, **download_text** način možete koristiti da biste preuzeli sadržaj blob kao tekstni niz.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Kako: brisanje blob-ova
Da biste izbrisali blob, najprije se referenca blob, a zatim poziva metodu **delete_blob** na njemu.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste naučili osnove blobova, slijedite ove veze da biste saznali više o Azure prostora za pohranu.  

-   [Kako koristiti reda čekanja za pohranu iz C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Upute za korištenje spremišta tablica iz C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Popis resursa Azure prostora za pohranu u C++](storage-c-plus-plus-enumeration.md)
-   [Prostor za pohranu klijenta biblioteke C++ reference](http://azure.github.io/azure-storage-cpp)
-   [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)
- [Prijenos podataka s pomoćnog programa naredbenog retka AzCopy](storage-use-azcopy.md)
