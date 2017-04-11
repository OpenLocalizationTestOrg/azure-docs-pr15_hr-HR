<properties
    pageTitle="Početak rada s pohranu reda čekanja Azure pomoću .NET | Microsoft Azure"
    description="Azure redova pružaju pouzdan, asinkronog razmjene poruka između aplikacija komponente. U oblaku omogućuje razmjenu vaše aplikacije komponente za promjenu veličine neovisno."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Početak rada s pohranu reda čekanja Azure pomoću .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Pregled

Azure reda čekanja za pohranu nudi oblaka razmjene poruka između aplikacija komponente. U dizajniranja aplikacije za skaliranje aplikacije komponente se često samostalne, tako da se mogu mijenjati veličinu neovisno. Reda čekanja za pohranu nudi asinkronog poruka za komunikaciju između aplikacija komponente, bez obzira na kojima rade u oblaku, na radnoj površini, lokalnog poslužitelja ili na mobilnom uređaju. Red čekanja za pohranu podržava i upravljanje asinkronog zadataka te izgradnju tijekove rada postupak.

### <a name="about-this-tutorial"></a>O ovog praktičnog vodiča

Pomoću ovog praktičnog vodiča pokazuje kako .NET kod za neke uobičajeni scenariji pomoću reda čekanja Azure prostora za pohranu. Scenariji u kojima je moguće uvrstiti stvaranje i brisanje redovima i dodavanje, za čitanje i brisanje reda čekanja poruke.

**Procijenjena vrijeme da biste dovršili:** 45 minuta

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure prostora za pohranu klijenta biblioteka za .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Upravitelj konfiguracije za .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Račun za Azure prostora za pohranu](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Dodavanje prostora za naziv deklaracija

Dodajte sljedeće `using` naredbe na vrh na `program.cs` datoteke:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Raščlaniti niz za povezivanje

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Stvaranje klijentskog servisa reda čekanja

Klase **CloudQueueClient** omogućuje vam dohvaćanje redova pohranjene u redu čekanja za pohranu. Ovdje je jedan od načina za stvaranje klijentskog programa servisa:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Sada ste spremni za pisanje koda koji se čita podatke iz i zapisuje podatke u redu čekanja za pohranu.

## <a name="create-a-queue"></a>Stvaranje reda čekanja

Ovaj primjer pokazuje kako stvoriti reda ako se još ne postoji:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Umetanje poruke u redu čekanja

Da biste umetnuli postojeću reda čekanja poruke, stvorite novi **CloudQueueMessage**. Nakon toga pozovite metodu **AddMessage** . **CloudQueueMessage** mogu se kreirati niza (u obliku UTF-8) ili raspon **bajtova** . Evo kod koji stvara red (Ako ne postoji) i umeće poruka "Zdravo, svijete":

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Pogled na sljedeću poruku

Možete pogled na poruku prednjoj strani reda bez uklanjanja iz reda čekanja tako da nazovete **PeekMessage** način.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Promjena sadržaja u redu čekanja poruke

Možete promijeniti sadržaj u poruku na mjestu u redu čekanja. Ako poruka predstavlja zadatak posla, može koristiti ovu značajku da biste ažurirali status zadatak posla. Sljedeći kod ažurira poruke reda čekanja novi sadržaj, a postavlja vidljivost vremenskog ograničenja da biste proširili drugi 60 sekundi. Sprema stanje rad povezan s porukom i daje klijent drugi minuta da biste nastavili raditi na poruci. Ovu tehniku možete koristiti za praćenje tijekova rada više koraka u redu čekanja poruke, bez potrebe da biste započeli od početka ne uspijete koraka obrade zbog kvara hardvera ili softvera. Obično želite zadržati kao i broj ponovnih pokušaja i ako poruke se ponoviti više puta *n* , želite ga izbrisati. To štiti od poruku koja se pokreće svaki put kada se ona se obrađuje pogreške aplikacije.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Poništi red čekanja na sljedeću poruku

Kod njezini redovi poruke iz reda čekanja u dva koraka. Prilikom pozivanja **GetMessage**dobiti sljedeću poruku u redu. Ostale kodove čitanju poruka iz ovog reda čekanja nevidljivim vratio **GetMessage** poruke. Prema zadanim postavkama, ova poruka ostaje nevidljivi 30 sekundi. Da biste dovršili uklanjanje poruka iz reda čekanja, morate nazvati i **DeleteMessage**. Dva koraka na sljedeći uklanjanja poruke pokazuje da ne uspijete koda za obradu poruku zbog kvara hardver ili softver, drugoj instanci koda možete se ista poruka i pokušajte ponovno. Kod poziva desnom **DeleteMessage** kada poruka bude obrađen.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Koristite asinkrone Await uzorak s uobičajenih reda čekanja pohranom API-ji

U ovom se primjeru pokazuje kako koristiti uzorak asinkrone Await s uobičajenih reda čekanja prostora za pohranu API-ji. Uzorak poziva asinkronog verziju svakog navedene metode kao što je naznačeno prema *asinkrone* sufiks svaki način. Kada se koristi način asinkrone, na asinkrone-await uzorak obustavlja lokalne izvođenja dok se završi poziv. Takvo ponašanje omogućuje trenutni niti obavljati druge poslove koje će se izbjeći grla performanse i poboljšava cjelokupan odziv aplikacije. Dodatne informacije o korištenju uzorak asinkrone Await u .NET potražite u članku [asinkrone i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Korištenje dodatnih mogućnosti za deaktivirali stavljanja poruke

Moguće je prilagoditi dohvaćanja poruke iz reda čekanja na dva načina.
Prvo, možete dobiti skupine poruke (najviše 32). Drugo, možete postaviti invisibility dulje ili kraće vrijeme čekanja dopuštanja kod više ili manje vremena za potpuno obradu svaku poruku. Sljedeći primjer koda koristi metodu **GetMessages** do 20 poruke u jedan poziv. Zatim obrađuje svaku poruku pomoću petlje **foreach** . Također postavlja ograničenja invisibility na pet minuta za svaku poruku. Imajte na umu da se pokreće pet minuta za sve poruke u isto vrijeme pa nakon pet minuta ste proslijeđen jer poziv na **GetMessages**, sve poruke koje su izbrisane više neće biti vidljiv ponovno.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Početak Duljina reda čekanja

U redu čekanja možete dobiti procjena se broj poruka. Način **FetchAttributes** pita usluga reda čekanja za dohvaćanje atribute reda čekanja, uključujući broj poruka. Svojstvo **ApproximateMessageCount** vraća zadnju vrijednost dohvatiti metodom **FetchAttributes** bez pozivanja usluga reda čekanja.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Brisanje reda čekanja

Da biste izbrisali red i sve poruke koje se nalaze u njoj, nazovite način za **Brisanje** reda čekanja objekta.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove reda čekanja prostora za pohranu, slijedite ove veze na dodatne informacije o složenije zadatke za pohranu.

- Prikaz reda čekanja servisa referentnu dokumentaciju za sve pojedinosti o API-ji dostupne:
    - [Prostor za pohranu klijenta biblioteka za .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Referenca REST API-JA](http://msdn.microsoft.com/library/azure/dd179355)
- Saznajte kako da biste pojednostavnili kod u pišete za rad sa spremištem Azure pomoću [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).
- Pogledajte dodatne značajke vodilice dodatne informacije o dodatne mogućnosti za spremanje podataka u Azure.
    - [Početak rada sa spremištem tablica Azure pomoću .NET](storage-dotnet-how-to-use-tables.md) strukturirane podatke.
    - [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md) nestrukturirane podatke.
    - [Povezivanje s bazom podataka SQL pomoću .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) da biste pohranili relacijskih podataka.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
