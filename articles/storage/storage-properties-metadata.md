<properties
    pageTitle="Postavljanje i dohvaćanje svojstva i metapodatke za objekte za pohranu Azure | Microsoft Azure"
    description="Spremanje prilagođene metapodataka na objekata u prostor za pohranu Azure, postavljanje i dohvaćanje svojstva sustava."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Postavljanje i dohvaćanje svojstva i metapodatke #

## <a name="overview"></a>Pregled

Objekti u svojstva sustava podrške za pohranu Azure i korisnički definirana metapodatke, osim podataka koje sadrže sljedeće:

*   **Svojstva sustava.** Svojstva sustava postoji na svakom resursa za pohranu. Neke od njih može pročitati ili postavili, a neke samo za čitanje. U odjeljku naslovnice, neka svojstva sustava odgovaraju određenih standardnih HTTP zaglavlja. Biblioteka klijentski Azure spremišta zadržava te umjesto vas.  

*   **Korisnički definirane metapodataka.** Korisnički definirane metapodataka je metapodataka koje ste odredili na navedeni resursa, u obliku par vrijednost za naziv. Možete koristiti metapodataka za spremanje dodatne vrijednosti pomoću resursa za pohranu; ove vrijednosti su samo u svrhu vlastite i ne utječu na ponašanje resurs.  

Dohvaćanje vrijednosti svojstava i metapodacima resursa za pohranu je dva koraka. Prije nego što mogu čitati te vrijednosti, ne mora izričito dohvaćati ih tako da nazovete metodu **FetchAttributes** .

> [AZURE.IMPORTANT] Svojstvo i metapodacima vrijednosti resursa za pohranu ne ispunjavaju osim ako je jedan od načina **FetchAttributes** poziva. 

## <a name="setting-and-retrieving-properties"></a>Postavljanje i dohvaćanje svojstva

Za dohvaćanje vrijednosti nekretnina pozvati metodu **FetchAttributes** na blob ili spremnik za popunjavanje svojstva, a zatim pročitajte vrijednosti.

Da biste postavili svojstva na neki objekt, navedite vrijednosti svojstva, a zatim pozvati metodu **SetProperties** .

Sljedeći primjer koda stvara spremnik i neke od njezinih vrijednosti nekretnina zapisuje prozora konzole:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Postavljanje i dohvaćanje metapodataka

Možete odrediti metapodataka kao parove vrijednost za naziv na blob ili spremnik resursa. Da biste postavili metapodataka, dodati parove naziv vrijednosti u zbirku **metapodataka** na resursa, a zatim pozvati metodu **SetMetadata** da biste spremili vrijednosti na servis.

> [AZURE.NOTE] Naziv metapodatka mora biti usklađen sa konvencije imenovanja za identifikatore C#.
 
Sljedeći primjer koda postavlja metapodataka na spremnik. Jedna vrijednost postavljen pomoću metode **Add** u zbirci. Druga vrijednost postavljeno korištenjem sintakse implicitno ključa/vrijednosti. Obje su valjane.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Da biste dohvatili metapodataka, pozvati metodu **FetchAttributes** na blob ili spremnik za popunjavanje zbirke **metapodataka** , a zatim pročitajte vrijednosti, kao što je prikazano u primjeru u nastavku.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Vidi također  

- [Azure prostora za pohranu klijenta biblioteka za .NET Reference](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure biblioteka za pohranu klijenta za .NET paketa](https://www.nuget.org/packages/WindowsAzure.Storage/) 
