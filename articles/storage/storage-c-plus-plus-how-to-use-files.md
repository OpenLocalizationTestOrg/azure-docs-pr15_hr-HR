<properties
    pageTitle="Kako koristiti za pohranu datoteka iz C++ | Microsoft Azure"
    description="Spremanje podataka iz datoteke u oblaku s Azure pohrani."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Kako koristiti za pohranu datoteka iz C++

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>O ovog praktičnog vodiča

U ovom ćete praktičnom vodiču ćete Saznajte upute za izvođenje osnovnih operacija servis za pohranu sustava Microsoft Azure datoteku. Putem uzorcima pisane C++, ćete saznati kako stvoriti dionice i direktorija, prijenos, popisa i brisanje datoteka. Ako ste novi korisnik servisa Microsoft Azure za pohranu datoteka, prolaze kroz koncepte u odjeljcima će biti vrlo koristan za razumijevanje primjere.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Stvaranje aplikacije C++

Da biste sastavili uzorcima, morat ćete instalirati biblioteka za pohranu klijenta 2.4.0 Azure za C++. Trebalo i stvoriti račun za Azure prostora za pohranu.

Da biste instalirali prostora za pohranu klijenta Azure 2.4.0 za C++, možete koristiti jednu od sljedećih načina:

-   **Linux:** Slijedite upute navedene na stranici [Azure prostora za pohranu klijenta biblioteku C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .

-   **Windows:** U Visual Studio, kliknite **Alati &gt; Upravitelj paketa NuGet &gt; konzole za Upravitelj paketa**. Na [konzoli upravitelja NuGet paketa](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) upišite sljedeću naredbu i pritisnite **ENTER**.

    Wastorage instalaciju paketa

## <a name="set-up-your-application-to-use-file-storage"></a>Postavljanje aplikacije da biste koristili pohrani

Dodajte sljedeće naredbe na vrh C++ datoteku koju želite koristiti Azure API-ji za pohranu da biste pristupili datotekama obuhvaćaju:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Postavljanje niz za povezivanje programa Azure prostora za pohranu

Da biste koristili za pohranu datoteka, morate povezati s računom Azure prostora za pohranu. Prvi korak bi da biste konfigurirali niza za povezivanje koji koristimo povezati s računom za pohranu. Pogledajmo definirati statične varijabla da to učinite.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Povezivanje s poslovnim subjektom Azure prostora za pohranu

Klase **cloud_storage_account** možete koristiti za prikaz podataka o računu za pohranu. Za dohvaćanje podataka o računu za pohranu iz niza za povezivanje za pohranu, možete koristiti metodu **raščlaniti** .

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Kako: Stvaranje zajedničke mape

Sve datoteke i mape u pohrani koji se nalaze u spremnika koji se naziva **zajedničko korištenje**. Račun za pohranu može imati proizvoljan broj dionice, kao što je vaš račun kapacitet omogućuje. Da biste dobili pristup zajedničkom i njegov sadržaj, morate koristiti klijent za pohranu datoteka.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Pomoću klijenta za pohranu datoteka, možete zatim dobiti referenca na zajedničko korištenje.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Da biste stvorili zajednički koristi, pomoću metode **create_if_not_exists** objekta **cloud_file_share** .

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

**Zajedničko korištenje** sada sadrži referencu dijeljenje pod nazivom **Moje-uzorka – zajedničko korištenje**.

## <a name="how-to-upload-a-file"></a>Kako: prijenos datoteke

Pri na vrlo najmanju programa Azure prostora za pohranu za zajedničko korištenje datoteka sadrži korijenski direktorij gdje se mogu nalaziti datoteke. U ovom odjeljku ćete saznati kako prenijeti datoteke s lokalno spremište na korijenski direktorij na zajedničko korištenje.

Prvi korak pri prijenosu datoteke je da biste dobili referenca imeniku gdje treba nalaziti. To se tako da nazovete metode **get_root_directory_reference** objekta zajedničko korištenje.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Sad kad ste reference na korijenskom direktoriju koji se zajednički koristi, možete prenijeti datoteke na ga. U ovom se primjeru prenosi iz datoteke, tekst i strujanje.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Kako: Stvorite direktorij

Prostor za pohranu možete organizirati i postavljanjem datoteke unutar direktorijima umjesto da ih u korijenskom direktoriju. Servis za pohranu datoteka Azure omogućuje stvaranje suvišni direktorija kao da bi vaš račun. Ispod kod će stvoriti direktorij pod nazivom **Moje-uzorka-direktorija** u korijenskom direktoriju, kao i poddirektorij pod nazivom **Moje-uzorka-poddirektorij**.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Kako: popis datoteke i mape u zajedničke mape

Nabavljanje popis datoteke i mape unutar zajednički jednostavno učiniti tako da nazovete **list_files_and_directories** **cloud_file_directory** referencu na. Da biste pristupili bogatog skupa svojstva i postupci za vraćene **list_file_and_directory_item**, mora se pozvati metodu **list_file_and_directory_item.as_file** da biste dobili **cloud_file** objekt ili metode **list_file_and_directory_item.as_directory** da biste dobili **cloud_file_directory** objekt.

Sljedeći kod pokazuje kako dohvatiti i izlaz URI svake stavke u korijenskom direktoriju koji se zajednički koristi.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Kako: preuzimanje datoteke

Da biste preuzeli datoteke, najprije dohvaćanje referenca datoteke i poziva **download_to_stream** način na koji želite prenijeti sadržaj datoteke strujanje objekt koji se zatim može zadržava u lokalnu datoteku. Osim toga, možete koristiti metodu **download_to_file** preuzeti sadržaj datoteke u lokalnu datoteku. Možete koristiti metodu **download_text** preuzeti sadržaj datoteke u obliku tekstnog niza.

U sljedećem primjeru pomoću metode **download_to_stream** i **download_text** da bismo pokazali preuzimanje datoteke koje su stvorene u ranijim odlomcima.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Kako: brisanje datoteke

Drugi uobičajeni postupak za pohranu datoteka je brisanje datoteke. Sljedeći kod briše datoteku pod nazivom Moje-uzorka-datoteke-3 pohranjene u korijenskom direktoriju.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Kako: brisanje imenik

Brisanje direktorij je jednostavno zadatka, premda je Primijetit ćete da ne možete izbrisati direktorija koji i dalje sadrži datoteke ili druge direktorija.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Kako: brisanje zajedničke mape

Brisanje zajednički obavlja tako da nazovete metodu **delete_if_exists** na cloud_file_share objektu. Evo ogledni kod koji ne.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Postavljanje maksimalnu veličinu za zajedničko korištenje datoteka

Kvota (ili Maksimalna veličina) možete postaviti za zajedničko korištenje datoteka, u gigabajta. Možete provjeriti i da biste vidjeli kojom količinom podataka pohranjena na zajedničko korištenje.

Postavljanjem kvote za zajedničke mape možete ograničiti ukupnu veličinu datoteka pohranjenih na zajedničko korištenje. Ako ukupnu veličinu datoteke na zajedničko korištenje prelazi kvote postavljena na zajedničko korištenje, zatim klijente neće biti moguće povećajte postojeće datoteke ili stvoriti nove datoteke, osim ako ih bit će prazni.

U sljedećem primjeru pokazuje kako provjeriti koje se trenutno koristi za dijeljenje i upute za postavljanje kvote za zajedničko korištenje.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Stvaranje potpisa zajednički pristup za datoteku ili zajedničko korištenje datoteka

Možete generirati zajednički pristup potpis (SAS) za zajedničko korištenje datoteka ili za pojedinačne datoteke. Možete stvoriti i pravila zajednički pristup na zajedničko korištenje datoteka da biste upravljali zajednički pristup potpisa. Stvaranje pravila za zajednički pristup se preporučuje, kao što je pruža srednje vrijednosti opozivanje na SAS ako moraju biti ugrožena.

U sljedećem primjeru stvara pravilo zajednički pristup na zajedničkom mjestu, a zatim koristi to pravilo omogućuje ograničenja za SAS na datoteci na zajedničko korištenje.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Dodatne informacije o stvaranju i korištenju potpisa zajednički pristup potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o Azure prostora za pohranu, proučite ove resurse:

-   [Prostor za pohranu klijenta biblioteka za C++](https://github.com/Azure/azure-storage-cpp)

-   [Explorer Azure prostora za pohranu](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)
