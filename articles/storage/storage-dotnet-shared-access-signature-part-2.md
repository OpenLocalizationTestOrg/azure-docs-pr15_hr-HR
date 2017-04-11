<properties
    pageTitle="Stvaranje i korištenje SAS s blobova | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča pokazuje kako stvoriti potpisa zajednički pristup za korištenje s blobova te kako ih trošiti klijentske aplikacije."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Zajedničko korištenje programa Access potpise, dio 2: Stvaranje i korištenje SAS s spremište blobova platforme

## <a name="overview"></a>Pregled

[1 dio](storage-dotnet-shared-access-signature-part-1.md) ovog praktičnog vodiča istražili zajednički pristup potpisa (SAS) te objašnjeni Praktični savjeti za korištenje ih. 2.dio pokazuje kako Generiraj, a zatim pomoću potpisa zajednički pristup s spremište blobova platforme. Primjeri su pisane C# i koristiti biblioteku Azure prostora za pohranu klijenta za .NET. Scenariji u kojima je moguće uvrstiti ove aspekte rad s zajednički pristup potpisa:

- Zajednički pristup potpis u spremniku za generiranje
- Generiranje zajednički pristup potpis u blob
- Stvaranje pohranjene pristup pravila za upravljanje potpisa u spremniku resursa
- Testiranje zajednički pristup potpisa iz klijentska aplikacija

## <a name="about-this-tutorial"></a>O ovog praktičnog vodiča

U ovom ćete praktičnom vodiču smo ćete fokus na stvaranju potpisa zajednički pristup za spremnika i blob-ova stvaranjem dvije konzole za aplikacije. Aplikacija prvog konzole generira zajednički pristup potpisa na spremnik i blob. Ove aplikacije sadrži znanja račun ključeva za pohranu. Druge aplikacije konzole za koje će poslužiti kao klijentska aplikacija, pristupa kontejner i resurse blob pomoću stvorene pomoću aplikacija prvog potpisa zajednički pristup. Ovu aplikaciju koristi potpise zajednički pristup samo za provjeru autentičnosti pristup spremniku i bloba resursa – ste znanja tipki računa.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Dio 1: Stvaranje aplikacije konzole za generiranje zajednički pristup potpisa

Najprije provjerite imate li biblioteku Azure prostora za pohranu klijenta za .NET instaliran. Možete instalirati [paket NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet paket") koji sadrži najnovije skupovi za biblioteku klijenta; To je preporučeni način za jamči da imate najnovija rješenja. Možete preuzeti i klijentska biblioteka u sklopu najnovije verzije platforme [Azure SDK za .NET](https://azure.microsoft.com/downloads/).

U Visual Studio, stvorite novu aplikaciju konzole za Windows i nazovite ih **GenerateSharedAccessSignatures**. Dodavanje reference na **Microsoft.WindowsAzure.Configuration.dll** i **Microsoft.WindowsAzure.Storage.dll**, pomoću bilo koju od sljedećih načina:

-   Ako želite da biste instalirali paket NuGet, najprije instalirajte [NuGet klijenta](https://docs.nuget.org/consume/installing-nuget). U Visual Studio, odaberite **projekta | Upravljanje NuGet paketa**, potražiti **Azure prostora za pohranu**na Internetu, a zatim slijedite upute za instalaciju.
-   Umjesto toga možete pronaći u sklopova u instalaciji SDK Azure i dodajte reference na njih.

Pri vrhu datoteke Program.cs, dodajte sljedeće **pomoću** naredbe:

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Uređivanje datoteke app.config tako da sadrži postavke konfiguracije s nizom za povezivanje koja upućuje na svoj račun za pohranu. App.config datoteke trebao bi izgledati slično ovome:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Generiranje potpisa zajednički pristup URI za spremnik

Počinje sa, ćemo dodati način da biste generirali zajednički pristup potpis u novi spremnik. U ovom slučaju potpis nije povezan s pravilo pohranjene access tako da se prenosi na URI podatke koji označava njegov vrijeme isteka i dozvole koje se dodjeljuje.

Najprije dodajte kod **Main()** metoda provjere autentičnosti pristup računu za pohranu i stvoriti novi spremnik:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Zatim dodajte nov način koje generira potpis zajednički pristup za spremnik i vraća potpis URI:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Pri dnu metodu **Main()** prije poziv **Console.ReadLine()**, da biste pozvali **GetContainerSasUri()** ili prozor konzole za pisanje potpis URI dodajte sljedeće retke:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Kompiliranje i pokretanje da biste izlaz potpis zajednički pristup URI za novi spremnik. URI će biti sličan URI za sljedeće:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Kada pokrenete kod zajednički pristup potpis koji ste stvorili na spremnik će vrijediti sljedeći dvadeset i četiri sata. Potpis daje dozvolu za klijenta za blob popisa polja u spremniku i pisanje nove blob spremniku.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Generiranje potpisa zajednički pristup URI za Blob

Nakon toga ne možemo pisanje slične kod da biste stvorili novi blob unutar spremnik i Generiranje potpisa zajednički pristup za nju. Zajednički pristup potpis nije povezan s pravilo pohranjene access tako da obuhvaća vrijeme početka, vrijeme isteka i informacije o dozvolama na URI.

Dodavanje nov način koji stvara novu blob i pisati tekst, zatim generira zajednički pristup potpis i vraća potpis URI:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Pri dnu metodu **Main()** , dodajte sljedeće retke da biste nazvali **GetBlobSasUri()**prije poziv **Console.ReadLine()**i pisanje potpis zajednički pristup URI prozor konzole:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Kompiliranje i pokretanje za izlaz potpis zajednički pristup URI za nove blob. URI će biti sličan URI za sljedeće:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Stvaranje pravila pristup pohranjene na spremnik

Sada ćemo stvoriti pravilo pohranjene pristup na spremnik koji će definirati ograničenja za sve zajednički pristup potpise koji su povezani s njom.

U prijašnjih primjera smo navedeno vrijeme početka (neizravno ili izravno), vrijeme isteka i dozvole potpis zajednički pristup URI-JA sam. U sljedećim primjerima smo bit će naveden te na pravila pohranjene pristup, a ne na zajednički pristup potpis. Na taj način omogućuje nam da biste promijenili te ograničenja bez ponovo izdaju potpis zajednički pristup.

Moguće je imati jednu ili više od ograničenja zajednički pristup potpis, a ostatak na spremljene pristup pravila. Međutim, možete samo odrediti vrijeme početka, vrijeme isteka i dozvole na jednom mjestu ili nekom drugom; ne možete, na primjer, određivanje dozvola na zajednički pristup potpis i ih i navesti na spremljene pristup pravila.

Imajte na umu da dodate pravilo pristup spremniku, morate dolaznog kontejner s postojećim dozvolama, dodavanje novog pravila programa access, a zatim postavljanje dozvola za kontejner s.

Dodajte novi način koji stvara novog pravilnika pohranjene pristup spremnik i vraća naziv pravila:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

Pri dnu metodu **Main()** prije poziv **Console.ReadLine()**dodajte sljedeće retke prvi Očisti sve postojeće pravilnike za pristup i poziva **CreateSharedAccessPolicy()** metoda:    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Imajte na umu da kad očistite pravilnike za pristup spremniku, morate najprije dobivaju dozvole za postojeće kontejner s, a zatim poništite odabir dozvola, a zatim ponovno Postavi dozvole.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Stvaranje potpisa zajednički pristup URI na spremniku koji koristi pravila programa Access

Zatim ćemo stvorit ćete drugi zajednički pristup potpis na spremnik koju smo stvorili ranije, ali ovaj put ne možemo ćete pridružiti potpis s pravilima programa access koju smo stvorili u prethodnom primjeru.

Dodajte novi način za generiranje drugi zajednički pristup potpis u spremniku:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Pri dnu metodu **Main()** prije poziv **Console.ReadLine()**, dodajte sljedeće retke da biste nazvali **GetContainerSasUriWithPolicy** metoda:

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Stvaranje potpisa zajednički pristup URI na Blob u kojem se koristi pravila programa Access

Na kraju, ćemo dodati sličan način da biste stvorili drugi blob i generiranje zajednički pristup potpisa koji je povezan s pravilima programa access.

Dodajte nov način da biste stvorili blob i generiranje zajednički pristup potpis:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Pri dnu metodu **Main()** prije poziv **Console.ReadLine()**, dodajte sljedeće retke da biste nazvali **GetBlobSasUriWithPolicy** metoda:    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

Način **Main()** trebala izgledati ovako u potpunosti. Pokrenite ga da biste prozor konzole za pisanje potpis zajednički pristup ji, a zatim kopirati i zalijepiti ih u tekstnu datoteku za korištenje u drugom dijelu ovog praktičnog vodiča.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Kada pokrenete aplikaciju konzole GenerateSharedAccessSignatures, vidjet ćete slično sljedećem u prozoru konzole za izlaz. To su zajednički pristup potpisa koji će se koristiti u 2 dio vodiča.

![SAS-konzole-izlaz-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Dio 2: Stvaranje aplikacije konzole za testiranje zajednički pristup potpisa

Da biste testirali potpisa zajednički pristup stvorene u prijašnjih primjera, ne možemo stvorit ćete drugi program konzole koju koristi potpisa za izvođenje operacije na spremnik i blob.

> [AZURE.NOTE] Ako više od 24 sata prošlo jer je dovršena prvi dio vodič, potpisa koje generira više neće biti valjane. U ovom slučaju, pokrećite kod u aplikaciji prvi konzole za generiranje potpisa Osvježi zajednički pristup za korištenje u drugom dijelu vodič.

U Visual Studio, stvorite novu aplikaciju konzole za Windows i nazovite ih **ConsumeSharedAccessSignatures**. Dodajte reference na **Microsoft.WindowsAzure.Configuration.dll** i **Microsoft.WindowsAzure.Storage.dll**, kao i koje ste prethodno poduzeli.

Pri vrhu datoteke Program.cs, dodajte sljedeće **pomoću** naredbe:

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

U tijelo metodu **Main()** dodajte konstante, a Ažuriraj njihove vrijednosti u zajednički pristup potpisa koji je generirao u dijelu 1 vodiča.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Dodavanje načina pokušati operacije kontejner s potpisom zajednički pristup

Zatim ćemo dodati način na koji testira neki postupci predstavniku spremnik pomoću zajednički pristup potpis na spremnik. Imajte na umu koristi li se potpis zajednički pristup za vraćanje reference na spremnik provjere autentičnosti pristup spremniku koji se temelji na samostalno potpis.

Program.cs dodajte na sljedeći način:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Ažurirajte metodu **Main()** da biste nazvali **UseContainerSAS()** objema zajednički pristup potpisa koji ste stvorili na spremnik:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Dodavanje načina pokušati Blob operacije s potpisom zajednički pristup

Na kraju, ćemo dodati način na koji testira neki postupci predstavnik blob pomoću zajednički pristup potpis na blob-om. U ovom slučaju koristimo Graditelj **CloudBlockBlob(String)**, prosljeđivanje u potpisu zajednički pristup za vraćanje reference na blob-om. Nema autentičnosti je potrebna; temelji se na samostalno potpis.

Program.cs dodajte na sljedeći način:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Ažurirajte metodu **Main()** da biste nazvali **UseBlobSAS()** objema zajednički pristup potpisa koji ste stvorili na blob-om:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Pokretanje aplikacije konzole i pridržavajte se rezultat da biste vidjeli operacijama dopušteno za koje potpisa. Poslani u prozor konzole izgledat će otprilike ovako:

![SAS-konzole-izlaz-2][sas-console-output-2]

## <a name="next-steps"></a>Daljnji koraci

[Zajednički pristup potpise, 1.dio: Osnove SAS modela](storage-dotnet-shared-access-signature-part-1.md)

[Upravljanje anonimnim pristupom čitanje spremnika i blob-ova](storage-manage-access-to-resources.md)

[Delegiranje pristupa potpisom zajednički pristup (REST API-JA)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Uvod u tablice i reda čekanja SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
