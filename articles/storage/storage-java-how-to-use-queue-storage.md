<properties
    pageTitle="Kako koristiti reda čekanja za pohranu iz Java | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure red za stvaranje i brisanje redove, umetanje, dobiti i brisanje poruka. Uzorci pisane Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Kako koristiti reda čekanja za pohranu iz Java

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Pregled

Ovaj vodič vidjet ćete kako izvršiti uobičajeni scenariji putem servisa za pohranu za Azure red. Primjere zapisuju u Java i korištenje [SDK za pohranu Azure Java][]. Scenariji u kojima je moguće uvrstiti **Umetanje**, **peeking**, **Početak**i **Brisanje** reda čekanja poruke, kao i **Stvaranje** i **Brisanje** redova. Dodatne informacije o redovima potražite u odjeljku [sljedeće korake](#Next-Steps) .

Napomena: Programa SDK dostupan je za razvojne inženjere koji koriste Azure prostora za pohranu na uređaje sa sustavom Android. Dodatne informacije potražite u članku [Azure SDK za pohranu za Android][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Stvaranje aplikacije Java

U ovom vodiču će se koristiti za pohranu značajke koje se mogu se pokrenuti aplikacije Java lokalno ili u kod koji se izvodi unutar uloga web ili uloga suradnika u Azure.

Da biste to učinili, morat ćete instalirati Java Development Kit (JDK) i stvorite račun za Azure prostora za pohranu u pretplatu za Azure. Nakon što ste to učinili, morat ćete provjerite ispunjava li sustav razvoj minimalni preduvjeti i ovisnosti koje su navedene u spremištu [SDK za pohranu Azure Java][] na GitHub. Ako je sustav zadovoljava te preduvjete, slijedite upute za preuzimanje i instaliranje biblioteka za pohranu Azure za Java na računalu te spremištu. Nakon što ste dovršili tih zadataka, će moći stvoriti aplikaciju Java koji koristi se primjerima u ovom članku.

## <a name="configure-your-application-to-access-queue-storage"></a>Konfiguriranje aplikacija za pristup reda čekanja za pohranu

Dodajte sljedeće naredbe Uvezi na vrh Java datoteku koju želite koristiti Azure prostora za pohranu API-ji za pristup reda čekanja:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Postavljanje niz za povezivanje programa Azure prostora za pohranu

Klijent za Azure prostora za pohranu koristi niza za povezivanje za pohranu za pohranu krajnje točke i vjerodajnice za pristup servisa za upravljanje podacima. Kada se pokrene u klijentskoj aplikaciji, morate unijeti niz za povezivanje za pohranu u sljedećem obliku pomoću naziv računa za pohranu i pristup primarni ključ za račun za pohranu koji je naveden u [Azure Portal](https://portal.azure.com) za *AccountName* i *AccountKey* vrijednosti. Ovaj primjer pokazuje kako deklarirati statične polja držite niz za povezivanje:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

U aplikaciji radi unutar uloga u Microsoft Azure, taj niz moguće pohraniti u datoteci konfiguracije servisa, *ServiceConfiguration.cscfg*te im možete pristupiti s pozivom **RoleEnvironment.getConfigurationSettings** korakom. Evo primjera dohvaćanje niza za povezivanje iz **postavka** element pod nazivom *StorageConnectionString* u konfiguracijskoj datoteci servisa:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Sljedeća primjera pretpostavlja da koristite jedan od ta dva načina za dohvaćanje niza veze za pohranu.

## <a name="how-to-create-a-queue"></a>Kako: Stvaranje reda čekanja

**CloudQueueClient** objekt omogućuje vam dohvaćanje objekata reference za redove. Sljedeći kod stvara **CloudQueueClient** objekt. (Napomena: postoje dodatni Načini stvaranja objekata **CloudStorageAccount** ; dodatne informacije potražite u članku **CloudStorageAccount** [Azure prostora za pohranu klijenta SDK referenca].)

Pomoću objekta **CloudQueueClient** upućivanje red koji želite koristiti. Red čekanja možete stvoriti ako ne postoji.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Kako: dodavanje poruke u red čekanja

Da biste umetnuli postojeću reda čekanja poruke, stvorite novi **CloudQueueMessage**. Nakon toga pozovite metodu **addMessage** . **CloudQueueMessage** mogu se kreirati niza (u obliku UTF-8) ili raspon bajtova. Evo kod koji stvara red (Ako ne postoji) i umeće poruka "Zdravo, svijete".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Kako: pogled na sljedeću poruku

Bez uklanjanja iz reda čekanja tako da nazovete **peekMessage**možete se pogled na poruku prednjoj strani reda.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Kako: promijeniti sadržaj u redu čekanja poruke

Možete promijeniti sadržaj u poruku na mjestu u redu čekanja. Ako poruka predstavlja zadatak posla, može koristiti ovu značajku da biste ažurirali status zadatak posla. Sljedeći kod ažurira poruke reda čekanja novi sadržaj, a postavlja vidljivost vremenskog ograničenja da biste proširili drugi 60 sekundi. Sprema stanje rad povezan s porukom i daje klijent drugi minutu da biste nastavili raditi na poruci. Ovu tehniku možete koristiti za praćenje tijekova rada više koraka u redu čekanja poruke, bez potrebe da biste započeli od početka ne uspijete koraka obrade zbog kvara hardvera ili softvera. Obično želite zadržati kao i broj ponovnih pokušaja i ako poruke se ponoviti više puta *n* , želite ga izbrisati. To štiti od poruku koja se pokreće svaki put kada se ona se obrađuje pogreške aplikacije.

Sljedeći kod uzorka pretraživanja kroz reda čekanja poruka, pronalazi prvu poruku koja odgovara "Zdravo, svijete" za sadržaj, a zatim mijenja sadržaj poruke i zatvara.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Osim toga, sljedećim primjerom koda ažurirati samo prve vidljivi poruke na u redu čekanja.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Kako: dohvaćanje Duljina reda čekanja

U redu čekanja možete dobiti procjena se broj poruka. Način **downloadAttributes** pita usluga reda čekanja za nekoliko trenutne vrijednosti, uključujući broj koliko poruke su u redu. Broj je samo djelomičnog jer poruke možete dodati ili ukloniti nakon usluga reda čekanja odgovori na vaš zahtjev. Način **getApproximateMessageCount** vraća zadnju vrijednost dohvaćenih poziv **downloadAttributes**bez pozivanja usluga reda čekanja.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Kako: Dequeue sljedeće poruke

Kod dequeues poruke iz reda čekanja u dva koraka. Prilikom pozivanja **retrieveMessage**dobiti sljedeću poruku u redu. Ostale kodove čitanju poruka iz ovog reda čekanja nevidljivim vratio **retrieveMessage** poruke. Prema zadanim postavkama, ova poruka ostaje nevidljivi 30 sekundi. Da biste dovršili uklanjanje poruka iz reda čekanja, morate nazvati i **deleteMessage**. Dva koraka na sljedeći uklanjanja poruke pokazuje da ne uspijete koda za obradu poruku zbog kvara hardver ili softver, drugoj instanci koda možete se ista poruka i pokušajte ponovno. Kod poziva **deleteMessage** desno kada poruka bude obrađen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Dodatne mogućnosti za dequeuing poruke

Moguće je prilagoditi dohvaćanja poruke iz reda čekanja na dva načina. Prvo, možete dobiti skupine poruke (najviše 32). Drugo, možete postaviti invisibility dulje ili kraće vrijeme čekanja dopuštanja kod više ili manje vremena za potpuno obradu svaku poruku.

Sljedeći primjer koda koristi metodu **retrieveMessages** do 20 poruke u jedan poziv. Zatim obrađuje svaku poruku pomoću petlja **za** . Također postavlja ograničenja invisibility na pet minuta (300 sekundi) za svaku poruku. Imajte na umu da se pokreće pet minuta za sve poruke u isto vrijeme pa kada pet minuta nakon poziv **retrieveMessages**, sve poruke koje su izbrisane više neće biti vidljiv ponovno.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Kako: popis redova

Da biste dobili popis trenutni redovi, nazovite metodu **CloudQueueClient.listQueues()** koji će vratiti zbirku objekata **CloudQueue** .

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Kako: Brisanje reda čekanja

Da biste izbrisali red i sve poruke koje se nalaze u njoj, nazovite metodu **deleteIfExists** na **CloudQueue** objektu.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove reda čekanja prostora za pohranu, slijedite ove veze na dodatne informacije o složenije zadatke za pohranu.

- [Azure prostora za pohranu SDK Java][]
- [Azure prostora za pohranu klijenta SDK Reference][]
- [Servise za pohranu Azure REST API-JA][]
- [Blog tima za Azure prostora za pohranu][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure prostora za pohranu SDK Java]: https://github.com/azure/azure-storage-java
[Azure prostora za pohranu SDK za Android]: https://github.com/azure/azure-storage-android
[Azure prostora za pohranu klijenta SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Servise za pohranu Azure REST API-JA]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
