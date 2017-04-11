<properties
    pageTitle="Početak rada s reda čekanja za pohranu i Visual Studio povezani services (Servisi u oblaku) | Microsoft Azure"
    description="Upute za početak rada s reda čekanja Azure prostora za pohranu u oblak servisa projekta u Visual Studio nakon povezivanja s računom za pohranu pomoću Visual Studio povezani servisi"
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

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Prvi koraci s Azure red prostora za pohranu i Visual Studio povezani services (oblaka services projekata)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako započeti rad sa sustavom reda čekanja Azure prostora za pohranu u Visual Studio nakon što ste stvorili ili pozivaju pomoću dijaloškog okvira Visual Studio **Dodavanje povezani servisi** računa Azure prostora za pohranu u oblak services projekta.

Pokazat ćemo vam kako stvoriti reda u kod. Također Pokazat ćemo vam upute za izvođenje operacija osnovni reda čekanja, kao što su dodavanje, izmjena, za čitanje i uklanjanje reda čekanja poruka. Primjere zapisuju u C# kod, a koristite [Microsoft Azure prostora za pohranu klijenta biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Operacija **Dodavanje povezani servisi** instalira odgovarajuće NuGet paketa da biste pristupili Azure prostora za pohranu u projektu i dodaje niz za povezivanje za račun za pohranu datoteka konfiguracije projekta.

 - Potražite u članku [Početak rada s pohranu reda čekanja Azure pomoću .NET](storage-dotnet-how-to-use-queues.md) dodatne informacije o rukovanje redova u kodu.
 - Opće informacije o Azure prostora za pohranu potražite u članku [dokumentaciju za pohranu](https://azure.microsoft.com/documentation/services/storage/) .
 - Opće informacije o servisima za Azure oblaka potražite u članku [dokumentaciju za servise u Oblaku](https://azure.microsoft.com/documentation/services/cloud-services/) .
 - Dodatne informacije o programskom ASP.NET aplikacija potražite u članku [ASP.NET](http://www.asp.net) .


Azure reda čekanja za pohranu je servis za pohranu velikog broja poruka koje je moguće pristupiti s bilo kojeg mjesta na svijetu putem čija je autentičnost provjerena poziva pomoću HTTP ili HTTPS. Jedan red poruka može biti najviše od 64 KB i reda čekanja mogu sadržavati milijune poruke do ukupni kapacitet ograničenje prostora za pohranu računa.


## <a name="access-queues-in-code"></a>Pristup redova u kodu

Da biste pristupili redova u Visual Studio servise u Oblaku projektima, morate uključuju sljedeće stavke sve C# izvornu datoteku pristup reda čekanja Azure prostora za pohranu.

1. Provjerite je li polje naziva deklaracija pri vrhu datoteke C# uključiti te **pomoću** naredbe.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Pronađite objekt **CloudStorageAccount** koji predstavlja prostor za pohranu podataka o računu. Koristite sljedeći kod da biste u nizu za povezivanje za pohranu i podataka o računu za pohranu konfiguraciju servisa Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Pronađite objekt **CloudQueueClient** referentni reda čekanja objekata na vašem računu za pohranu.  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Pronađite objekt **CloudQueue** referentni određeni red.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**BILJEŠKE:** Koristiti sve iznad kod ispred koda u sljedeća primjera.

## <a name="create-a-queue-in-code"></a>Stvaranje reda u kodu

Da biste stvorili reda čekanja u kodu, jednostavno dodajte poziv **CreateIfNotExists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Dodavanje poruke u red čekanja

Da biste umetnuli postojeću reda čekanja poruke, stvorite novi **CloudQueueMessage** objekt, a zatim pozvati metodu **AddMessage** .

Objekt programa **CloudQueueMessage** mogu se kreirati niza (u obliku UTF-8) ili raspon bajtova.

Slijedi primjer koji umeće poruka "Zdravo, svijete".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Čitanje poruke u redu čekanja

Možete pogled na poruku prednjoj strani reda bez uklanjanja iz reda čekanja tako da nazovete metodu **PeekMessage** .

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Čitanje i uklonili poruku u redu čekanja

Možete ukloniti kod (deaktivirali red čekanja) poruka iz reda čekanja u dva koraka.

1. Nazovite **GetMessage** dobiti sljedeću poruku u redu. Ostale kodove čitanju poruka iz ovog reda čekanja nevidljivim vratio **GetMessage** poruke. Prema zadanim postavkama, ova poruka ostaje nevidljivi 30 sekundi.
2.  Da biste dovršili uklanjanje poruka iz reda čekanja, nazovite **DeleteMessage**.

Dva koraka na sljedeći uklanjanja poruke pokazuje da ne uspijete koda za obradu poruku zbog kvara hardver ili softver, drugoj instanci koda možete se ista poruka i pokušajte ponovno. Sljedeći kod poziva **DeleteMessage** desno kada poruka bude obrađen.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Koristite dodatne mogućnosti za obradu i uklanjanje reda čekanja poruka

Moguće je prilagoditi dohvaćanja poruke iz reda čekanja na dva načina.

 - Možete dobiti skupine poruke (najviše 32).
 - Invisibility dulje ili kraće vremensko ograničenje, možete postaviti dopuštanja kod više ili manje vremena za potpuno obradu svaku poruku. Sljedeći primjer koda koristi metodu **GetMessages** do 20 poruke u jedan poziv. Zatim obrađuje svaku poruku pomoću **foreach** petlje. Također postavlja ograničenja invisibility na pet minuta za svaku poruku. Imajte na umu da se pokreće pet minuta za sve poruke u isto vrijeme pa nakon pet minuta ste proslijeđeni jer poziv na **GetMessages**, sve poruke koje su izbrisane više neće biti vidljiv ponovno.

Evo jednog primjera:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Početak Duljina reda čekanja

U redu čekanja možete dobiti procjena se broj poruka. Način **FetchAttributes** pita usluga reda čekanja za dohvaćanje atribute reda čekanja, uključujući broj poruka. Svojstvo **ApproximateMethodCount** vraća zadnju vrijednost dohvatiti metodom **FetchAttributes** bez pozivanja usluga reda čekanja.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Korištenje uzorka asinkrone Await s uobičajenih Azure red API-ji

U ovom se primjeru pokazuje kako koristiti uzorak asinkrone Await s uobičajenih Azure red API-ji. Uzorak poziva asinkrone verziju svaki od navedene metode, to mogu vidjeti **asinkrone** popravak nakon svake metode. Kada se koristi način asinkrone na asinkrone-await uzorak obustavlja lokalne izvođenja dok se završi poziv. Takvo ponašanje omogućuje trenutni niti obavljati druge poslove koje će se izbjeći grla performanse i poboljšava cjelokupan odziv aplikacije. Dodatne informacije o korištenju uzorak asinkrone Await u .NET potražite u članku [asinkrone i Await (C# i Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Brisanje reda čekanja

Da biste izbrisali red i sve poruke koje se nalaze u njoj, nazovite način za **Brisanje** reda čekanja objekta.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
