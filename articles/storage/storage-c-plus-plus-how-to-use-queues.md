<properties
    pageTitle="Kako koristiti reda čekanja prostora za pohranu (C++) | Microsoft Azure"
    description="Saznajte kako koristiti servis za pohranu reda čekanja u Azure. Uzorci zapisuju u C++."
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

# <a name="how-to-use-queue-storage-from-c"></a>Kako koristiti reda čekanja za pohranu iz C++  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Pregled
Ovaj vodič vidjet ćete kako izvršiti uobičajeni scenariji putem servisa za pohranu za Azure red. Primjere zapisuju u C++ i koristite [Azure prostora za pohranu klijenta biblioteku C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scenariji u kojima je moguće uvrstiti **Umetanje**, **peeking**, **Početak**i **Brisanje** reda čekanja poruke, kao i **Stvaranje i brisanje reda čekanja**.

>[AZURE.NOTE] Ovaj vodič pronalaze klijentska biblioteka za pohranu Azure C++ verziju 1.0.0 i iznad. Preporučena verzija je biblioteka za pohranu klijenta 2.2.0, koji je dostupan putem [NuGet](http://www.nuget.org/packages/wastorage) ili [GitHub](http://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Stvaranje aplikacije C++  
U ovom vodiču će se koristiti za pohranu značajke koje se može pokrenuti unutar C++ aplikacije.

Da biste to učinili, morat ćete instalirati biblioteku Azure prostora za pohranu klijenta za C++ i stvaranje računa Azure prostora za pohranu u pretplatu za Azure.

Da biste instalirali biblioteku Azure prostora za pohranu klijenta za C++, možete koristiti na sljedeće načine:

-   **Linux:** Slijedite upute navedene na stranici [Azure prostora za pohranu klijenta biblioteku C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .
-   **Windows:** U Visual Studio, kliknite **Alati > Upravitelj paketa NuGet > Upravitelj paketa konzole**. Na [konzoli upravitelja NuGet paketa](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) upišite sljedeću naredbu i pritisnite **ENTER**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Konfiguriranje aplikacija za pristup reda čekanja za pohranu
Dodajte sljedeće naredbe na vrh C++ datoteku koju želite koristiti Azure prostora za pohranu API-ji za pristup redovima obuhvaćaju:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Postavljanje niz za povezivanje programa Azure prostora za pohranu

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

## <a name="how-to-create-a-queue"></a>Kako: Stvaranje reda čekanja
Objekt programa **cloud_queue_client** omogućuje vam dohvaćanje objekata reference za redove. Sljedeći kod stvara **cloud_queue_client** objekt.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Pomoću objekta **cloud_queue_client** upućivanje red koji želite koristiti. Red čekanja možete stvoriti ako ne postoji.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Kako: Umetanje poruke u redu čekanja
Da biste umetnuli postojeću reda čekanja poruke, stvorite novi **cloud_queue_message**. Nakon toga pozovite metodu **add_message** . **Cloud_queue_message** mogu se kreirati niz ili raspon **bajtova** . Evo kod koji stvara red (Ako ne postoji) i umeće poruka "Zdravo, svijete":

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Kako: pogled na sljedeću poruku
Možete pogled na poruku prednjoj strani reda bez uklanjanja iz reda čekanja tako da nazovete metodu **peek_message** .

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Kako: promijeniti sadržaj u redu čekanja poruke
Možete promijeniti sadržaj u poruku na mjestu u redu čekanja. Ako poruka predstavlja zadatak posla, može koristiti ovu značajku da biste ažurirali status zadatak posla. Sljedeći kod ažurira poruke reda čekanja novi sadržaj, a postavlja vidljivost vremenskog ograničenja da biste proširili drugi 60 sekundi. Sprema stanje rad povezan s porukom i daje klijent drugi minuta da biste nastavili raditi na poruci. Ovu tehniku možete koristiti za praćenje tijekova rada više koraka u redu čekanja poruke, bez potrebe da biste započeli od početka ne uspijete koraka obrade zbog kvara hardvera ili softvera. Obično želite zadržati kao i broj ponovnih pokušaja i ako poruke se ponoviti više od n puta, želite ga izbrisati. To štiti od poruku koja se pokreće svaki put kada se ona se obrađuje pogreške aplikacije.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Kako: deaktivirali red čekanja na sljedeću poruku
Kod njezini redovi poruke iz reda čekanja u dva koraka. Prilikom pozivanja **get_message**dobiti sljedeću poruku u redu. Ostale kodove čitanju poruka iz ovog reda čekanja nevidljivim vratio **get_message** poruke. Da biste dovršili uklanjanje poruka iz reda čekanja, morate nazvati i **delete_message**. Dva koraka na sljedeći uklanjanja poruke pokazuje da ne uspijete koda za obradu poruku zbog kvara hardver ili softver, drugoj instanci koda možete se ista poruka i pokušajte ponovno. Kod poziva **delete_message** desno kada poruka bude obrađen.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Kako: korištenje dodatnih mogućnosti za deaktivirali stavljanja poruke
Moguće je prilagoditi dohvaćanja poruke iz reda čekanja na dva načina. Prvo, možete dobiti skupine poruke (najviše 32). Drugo, možete postaviti invisibility dulje ili kraće vrijeme čekanja dopuštanja kod više ili manje vremena za potpuno obradu svaku poruku. Sljedeći primjer koda koristi metodu **get_messages** do 20 poruke u jedan poziv. Zatim obrađuje svaku poruku pomoću petlje **za** . Također postavlja ograničenja invisibility na pet minuta za svaku poruku. Imajte na umu da se pokreće pet minuta za sve poruke u isto vrijeme pa nakon pet minuta ste proslijeđen jer poziv na **get_messages**, sve poruke koje su izbrisane više neće biti vidljiv ponovno.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Kako: dohvaćanje Duljina reda čekanja
U redu čekanja možete dobiti procjena se broj poruka. Način **download_attributes** pita usluga reda čekanja za dohvaćanje atribute reda čekanja, uključujući broj poruka. U redu čekanja metodu **approximate_message_count** može vidjeti djelomičnog broj poruka.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Kako: Brisanje reda čekanja
Da biste izbrisali red i sve poruke koje se nalaze u njoj, nazovite metodu **delete_queue_if_exists** na objektu red.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove reda čekanja prostora za pohranu, slijedite ove veze da biste saznali više o Azure prostora za pohranu.

-   [Upute za korištenje spremišta blobova iz C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Upute za korištenje spremišta tablica iz C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Popis resursa Azure prostora za pohranu u C++](storage-c-plus-plus-enumeration.md)
-   [Prostor za pohranu klijenta biblioteke C++ reference](http://azure.github.io/azure-storage-cpp)
-   [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)

