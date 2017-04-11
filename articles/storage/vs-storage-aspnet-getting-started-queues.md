<properties
    pageTitle="Početak rada s reda čekanja za pohranu i Visual Studio povezani servisi (ASP.NET) | Microsoft Azure"
    description="Kako započeti rad sa sustavom reda čekanja Azure prostora za pohranu u projektu ASP.NET u Visual Studio nakon povezivanja s računom za pohranu pomoću Visual Studio povezani servisi"
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
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Početak rada s Azure red za pohranu i Visual Studio povezani servisi

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako započeti rad sa sustavom reda čekanja Azure prostora za pohranu u Visual Studio nakon što ste stvorili ili račun Azure prostora za pohranu u projektu ASP.NET pozivaju pomoću dijaloškog okvira Visual Studio **Dodajte povezani servisi** .

Pokazat ćemo vam kako stvoriti i pristup Azure reda čekanja na vašem računu za pohranu. Također Pokazat ćemo vam upute za izvođenje operacija osnovni reda čekanja, kao što su dodavanje, izmjena, za čitanje i uklanjanje reda čekanja poruka. Primjere zapisuju u C# kod, a koristite [Microsoft Azure prostora za pohranu klijenta biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Dodatne informacije o ASP.NET potražite u članku [ASP.NET](http://www.asp.net).

Azure reda čekanja za pohranu je servis za pohranu velikog broja poruka koje je moguće pristupiti s bilo kojeg mjesta na svijetu putem čija je autentičnost provjerena poziva pomoću HTTP ili HTTPS. Jedan red poruka može biti najviše od 64 KB i reda čekanja mogu sadržavati milijune poruke do ukupni kapacitet ograničenje prostora za pohranu računa.

## <a name="access-queues-in-code"></a>Pristup redova u kodu

Da biste pristupili redova u ASP.NET projektima, morate uključuju sljedeće stavke sve C# izvornu datoteku pristup reda čekanja Azure prostora za pohranu.

1. Provjerite je li polje naziva deklaracija pri vrhu datoteke C# uključiti te **pomoću** naredbe.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Pronađite objekt **CloudStorageAccount** koji predstavlja prostor za pohranu podataka o računu. Koristite sljedeći kod da biste u nizu za povezivanje za pohranu i podataka o računu za pohranu konfiguraciju servisa Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Pronađite objekt **CloudQueueClient** referentni reda čekanja objekata na vašem računu za pohranu.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Pronađite objekt **CloudQueue** referentni određeni red.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Napomena** Koristiti sve iznad kod ispred koda u sljedeća primjera.

## <a name="create-a-queue-in-code"></a>Stvaranje reda u kodu

Da biste stvorili Azure reda čekanja u kodu, jednostavno dodajte poziv **CreateIfNotExists** gore navedeni kod.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Dodavanje poruke u red čekanja

Da biste umetnuli postojeću reda čekanja poruke, stvorite novi **CloudQueueMessage** objekt, a zatim pozvati metodu **AddMessage** .

Objekt programa **CloudQueueMessage** mogu se kreirati niza (u obliku UTF-8) ili raspon bajtova.

Slijedi primjer koji umeće poruka "Zdravo, svijete".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Čitanje poruke u redu čekanja

Možete pogled na poruku prednjoj strani reda bez uklanjanja iz reda čekanja tako da nazovete metodu PeekMessage().

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Čitanje i uklonili poruku u redu čekanja

Možete ukloniti kod (deaktivirali red čekanja) poruka iz reda čekanja u dva koraka.
1. Nazovite GetMessage() dobiti sljedeću poruku u redu. Ostale kodove čitanju poruka iz ovog reda čekanja nevidljivim vratio GetMessage() poruke. Prema zadanim postavkama, ova poruka ostaje nevidljivi 30 sekundi.
2.  Da biste dovršili uklanjanje poruka iz reda čekanja, nazovite **DeleteMessage**.

Dva koraka na sljedeći uklanjanja poruke pokazuje da ne uspijete koda za obradu poruku zbog kvara hardver ili softver, drugoj instanci koda možete se ista poruka i pokušajte ponovno. Sljedeći kod poziva **DeleteMessage** desno kada poruka bude obrađen.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Koristite dodatne mogućnosti za deaktivirali stavljanja poruke

Moguće je prilagoditi dohvaćanja poruke iz reda čekanja na dva načina.
Prvo, možete dobiti skupine poruke (najviše 32). Drugo, možete postaviti invisibility dulje ili kraće vrijeme čekanja dopuštanja kod više ili manje vremena za potpuno obradu svaku poruku. Sljedeći primjer koda koristi metodu **GetMessages** do 20 poruke u jedan poziv. Zatim obrađuje svaku poruku pomoću petlje **foreach** . Također postavlja ograničenja invisibility na pet minuta za svaku poruku. Imajte na umu da se pokreće pet minuta za sve poruke u isto vrijeme pa nakon pet minuta ste proslijeđen jer poziv na **GetMessages**, sve poruke koje su izbrisane više neće biti vidljiv ponovno.

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

U redu čekanja možete dobiti procjena se broj poruka. Način **FetchAttributes** pita queueservice dohvatiti atribute reda čekanja, uključujući broj poruka. Svojstvo **ApproximateMethodCount** vraća zadnju vrijednost dohvate metodom **FetchAttributes** bez pozivanja u queueservice.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Koristite asinkrone Await uzorak s uobičajenih queueAPIs

U ovom se primjeru pokazuje kako da biste koristili uzorak asinkrone Await s uobičajenih queueAPIs. Uzorak poziva asinkrone verziju svaki od navedene metode, to mogu vidjeti asinkrone popravak nakon svake metode. Kada se koristi način asinkrone na asinkrone-await uzorak obustavlja lokalne izvođenja dok se završi poziv. Takvo ponašanje omogućuje trenutni niti obavljati druge poslove koje će se izbjeći grla performanse i poboljšava cjelokupan odziv aplikacije. Dodatne informacije o korištenju uzorak asinkrone Await u .NET potražite u članku [asinkrone i Await (C# i Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Brisanje reda čekanja

Da biste izbrisali red i sve poruke koje se nalaze u njoj, nazovite način za **Brisanje** reda čekanja objekta.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]