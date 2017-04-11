<properties
    pageTitle="Kako koristiti za pohranu datoteka iz Java | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure datoteke za prijenos, preuzeli, popisa i brisanje datoteka. Uzorci pisane Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Kako koristiti za pohranu datoteka iz Java

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Pregled

U ovom vodiču za ćete Dodatne upute za izvođenje osnovnih operacija servis za pohranu sustava Microsoft Azure datoteku. Putem uzoraka pisane Java ćete saznati kako stvoriti dionice i direktorija, prijenos, popisa i brisanje datoteka. Ako ste novi korisnik servisa Microsoft Azure za pohranu datoteka, prolaze kroz koncepte u odjeljcima će biti vrlo koristan za razumijevanje primjere.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Stvaranje aplikacije Java

Da biste sastavili uzorcima, trebat će vam Java Development Kit (JDK) i [] [Azure SDK za pohranu za Java]. Trebalo i stvoriti račun za Azure prostora za pohranu.

## <a name="setup-your-application-to-use-file-storage"></a>Postavljanje aplikacije da biste koristili pohrani

Da biste koristili Azure API-ji za pohranu, dodajte sljedeće naredbe na vrh datoteka jezika Java koje namjeravate pristup za pohranu servisa.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Postavljanje niz za povezivanje programa Azure prostora za pohranu

Da biste koristili za pohranu datoteka, morate povezati s računom Azure prostora za pohranu. Prvi korak bi da biste konfigurirali niza za povezivanje koji koristimo povezati s računom za pohranu. Pogledajmo definirati statične varijabla da to učinite.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Zamijenite your_storage_account_name i your_storage_account_key stvarnih vrijednosti za vaš račun za pohranu.

## <a name="connecting-to-an-azure-storage-account"></a>Povezivanje s poslovnim subjektom Azure prostora za pohranu

Da biste se povezali s računom za pohranu, morate koristiti objekt **CloudStorageAccount** prosljeđivanje niza za povezivanje njegov **raščlaniti** korakom.

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** throws programa InvalidKeyException pa ćete morati staviti unutar bloka pokušajte/zamka.

## <a name="how-to-create-a-share"></a>Kako: Stvaranje zajedničke mape

Sve datoteke i mape u pohrani koji se nalaze u spremnika koji se naziva **zajedničko korištenje**. Račun za pohranu može imati suvišni dionice kao što je vaš račun kapacitet omogućuje. Da biste dobili pristup zajedničkom i njegov sadržaj, morate koristiti klijent za pohranu datoteka.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Pomoću klijenta za pohranu datoteka, možete zatim dobiti referenca na zajedničko korištenje.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Da biste zapravo stvorili zajednički koristi, pomoću metode **createIfNotExists** objekta CloudFileShare.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

**Zajedničko korištenje** sada sadrži referencu dijeljenje pod nazivom **sampleshare**.

## <a name="how-to-upload-a-file"></a>Kako: prijenos datoteke

Programa Azure prostora za pohranu za zajedničko korištenje datoteka sadrži pri na vrlo najmanje, korijenski direktorij gdje se mogu nalaziti datoteke. U ovom odjeljku ćete saznati kako prenijeti datoteke s lokalno spremište na korijenski direktorij na zajedničko korištenje.

Prvi korak pri prijenosu datoteke je da biste dobili referenca imeniku gdje treba nalaziti. To se tako da nazovete metodu **getRootDirectoryReference** objekta za zajedničko korištenje.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Sad kad ste referenca korijenski direktorij koji se zajednički koristi, možete prenijeti datoteke na koristeći sljedeći kod.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Kako: Stvorite direktorij

Prostor za pohranu možete organizirati i postavljanjem datoteke unutar podređenu direktorija umjesto da ih u korijenskom direktoriju. Servis za pohranu datoteka Azure omogućuje stvaranje suvišni direktorija kao da bi vaš račun. Ispod kod će stvoriti podređene direktorij pod nazivom **sampledir** u korijenskom direktoriju.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Kako: popis datoteke i mape u zajedničke mape

Nabavljanje popis datoteke i mape unutar zajednički jednostavno učiniti tako da nazovete **listFilesAndDirectories** CloudFileDirectory referencu na. Način vraća popis ListFileItem objekte koji se mogu ponavljati na. Na primjer, sljedeći kod prikazat će se popis datoteke i mape u korijenskom direktoriju.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Kako: preuzimanje datoteke

Jedna od češće operacije će izvesti protiv pohrani jest da biste preuzeli datoteke. U sljedećem primjeru kod preuzimanja SampleFile.txt i prikazuje njegov sadržaj.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Kako: brisanje datoteke

Drugi uobičajeni postupak za pohranu datoteka je brisanje datoteke. Sljedeći kod briše datoteku pod nazivom SampleFile.txt pohranjena u direktoriju **sampledir**.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Kako: brisanje imenik

Brisanje direktorij je prilično jednostavan zadatak, premda je Primijetit ćete da ne možete izbrisati direktorija koji i dalje sadrži datoteke ili druge direktorija.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Kako: brisanje zajedničke mape

Brisanje zajednički obavlja tako da nazovete metodu **deleteIfExists** na CloudFileShare objektu. Evo ogledni kod koji ne.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Daljnji koraci

Ako želite da biste saznali više o Azure pohranu API-ji, slijedite ove veze.

- [Razvojni centar za Java](http://azure.microsoft.com/develop/java/)
- [Azure prostora za pohranu SDK Java](https://github.com/azure/azure-storage-java)
- [Azure prostora za pohranu SDK za Android](https://github.com/azure/azure-storage-android)
- [Azure prostora za pohranu klijenta SDK Reference](http://dl.windowsazure.com/storage/javadoc/)
- [Servise za pohranu Azure REST API-JA](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/)
- [Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)
