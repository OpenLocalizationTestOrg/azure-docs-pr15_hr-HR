<properties
    pageTitle="Obradu IoT koncentrator uređaj u oblak poruka (.Net) | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste saznali korisne uzorke da obradi koncentrator IoT uređaja u oblak poruke."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Praktični vodič: Način obrade koncentrator IoT uređaja u oblak poruke pomoću .net

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Uvod

Azure IoT koncentrator je potpuno upravljana servis koji omogućuje pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i aplikacije sigurnosno završetka. Druge vodiče ([Početak rada s IoT koncentratora] i [slanje poruka oblak na uređaj s koncentrator IoT][lnk-c2d]) pokazalo kako se koristi osnovni uređaj u oblak i oblaka uređaj razmjenu funkcionalnost IoT koncentratora.

Pomoću ovog praktičnog vodiča sastavlja na kôda prikazanog u ovom praktičnom vodiču za [Početak rada s IoT koncentrator] , a prikazuju se dvije skalabilni uzorke koje možete koristiti za obradu uređaj u oblak poruke:

- Pouzdan prostora za pohranu uređaj u oblak poruke u [spremište blobova platforme Azure]. Uobičajeni scenarij je analize *Hladna put* pohraniti telemetrijskih podataka u BLOB polja da biste koristili kao unos u postupke analize. Ti procesi mogu biti pokreću Alati kao što su [Tvorničke Azure podataka] ili stog [HDInsight (Hadoop)] .

- Pouzdan obrada *interaktivne* uređaj u oblak poruke. Uređaj u oblak poruke interaktivni su kada su odmah okidača za skup akcija u pozadinska aplikacija. Ako, na primjer, na uređaju možda poslati poruku alarma koji pokreće Umetanje s karata u sustavu CRM. Za razliku od toga *podatak* poruke jednostavno dolaziti analitički modul. Ako, na primjer, temperatura telemetrijskih s uređaja koji će se spremiti za kasniju analizu je poruke točke podataka.

Jer IoT koncentrator izlaže [Koncentrator događaj][lnk-event-hubs]-kompatibilne krajnjoj točki Prima poruke uređaj u oblak, ovog vodiča koristi [EventProcessorHost] instance. Ova instanca:

* Pouzdano pohranjuje *podatak* poruke u spremište blobova platforme Azure.
* Prosljeđuje *interaktivne* uređaj u oblak poruke programa Azure [Bus servis reda čekanja] za obradu odmah.

Bus servisa osigurava pouzdanog obrada interaktivne poruka, kao što je nudi checkpoints po poruke i vrijeme utemeljen na prozor de-dupliciranje.

> [AZURE.NOTE] **EventProcessorHost** instance je samo jedan način obrade interaktivne poruke. Druge mogućnosti obuhvaćaju [Tkanina servisa Azure] [ lnk-service-fabric] i [Analitiku strujanje Azure][lnk-stream-analytics].

Na kraju ovog praktičnog vodiča, pokrenite tri aplikacije konzole za Windows:

* **SimulatedDevice**, izmijenjenu verziju aplikacije stvorene u na [Početak rada s IoT koncentrator] Praktični vodič šalje podatke točke uređaj u oblak poruke svaki drugi i interaktivnih poruke uređaj u oblak svakih 10 sekundi. Ta aplikacija koristi protokol AMQP možete komunicirati s IoT koncentratora.
* **ProcessDeviceToCloudMessages** koristi klase [EventProcessorHost] za dohvaćanje poruka iz krajnje kompatibilan s preglednikom događaja koncentratora. Datoteka, a zatim pouzdano sprema poruke točke podataka u spremište blobova platforme Azure i prosljeđuje interaktivne poruke servisa Bus red.
* **ProcessD2CInteractiveMessages** deaktivirali redovi interaktivne poruke iz servisa Bus reda.

> [AZURE.NOTE] Koncentrator IoT ima SDK podršku za više uređaja platforme i jezika, uključujući C, Java i JavaScript. Da biste saznali kako zamijeniti Simulirani uređaj pomoću ovog praktičnog vodiča fizički uređaj i kako povezati uređaji koncentratora za IoT, potražite u [Centru za razvojne inženjere za Azure IoT].

Pomoću ovog praktičnog vodiča se izravno primjenjuju na druge načine za događaj koncentrator kompatibilan s poruke, kao što su projekti [HDInsight (Hadoop)] . Dodatne informacije potražite u članku [Vodič za razvojne inženjere Azure IoT koncentrator - uređaja na cloud].

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Microsoft Visual Studio 2015.

+ Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti [pomoću računa](https://azure.microsoft.com/free/) u samo nekoliko minuta.

Imat ćete neke osnovne znanja [Azure prostora za pohranu] i [Bus servisa Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Slanje interaktivne poruka s Simulirani uređaja

U ovom ćete odjeljku izmjena Simulirani uređaj aplikaciju koju ste stvorili u vodiču [Početak rada s IoT koncentrator] za slanje poruka interaktivne uređaj u oblak središtu IoT.

1. U Visual Studio u programu project **SimulatedDevice** dodati sa sljedećim korakom predmete **Program** .

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Ta je metoda sličan način **SendDeviceToCloudMessagesAsync** u programu project **SimulatedDevice** . Samo razlike među njima da sada svojstvo **MessageId** sustav, a svojstvo korisničkog naziva **messageType**.
    Kod dodjeljuje globalno jedinstveni identifikator (GUID) svojstvo **MessageId** . Bus servisa možete koristiti ovaj identifikator za uklanjanje duplikata poruke primi. Uzorak koristi to svojstvo **messageType** za razlikovanje interaktivne s porukama točke podataka. Aplikacija prosljeđuje te podatke u svojstva poruke, umjesto u tijelu poruke tako da se događaj procesor ne morate ukloniti serijski broj poruku da biste izvršili usmjeravanje poruka.

    > [AZURE.NOTE] Nije važno da biste stvorili **MessageId** koristi da biste deaktivirali duplicirali interaktivne poruke u kodu uređaja. Povremeni mrežnu komunikaciju ili druge neuspješna može rezultirati više ponovno slanje iste poruke s uređaja. Možete koristiti i ID semantičkog poruke, kao što su raspršivanje podatkovnih polja odgovarajuće poruke, umjesto GUID.

2. Dodajte sljedeće metoda u način **glavne** desno prije nego na `Console.ReadLine()` redak:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Radi jednostavnosti, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog trebali biste provesti pravila Ponovi kao što su eksponencijalne backoff, kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara].

## <a name="process-device-to-cloud-messages"></a>Obrada poruke uređaj u oblak

U ovom ćete odjeljku stvaranja aplikacije za Windows konzole koja obrađuje uređaj u oblak poruke iz centra za IoT. Koncentrator Iot izlaže programa [koncentrator događaja]-kompatibilne krajnja točka za omogućivanje aplikacije za čitanje poruka uređaj u oblak. Pomoću ovog praktičnog vodiča koristi klase [EventProcessorHost] za obradu tih poruka u aplikaciji konzolu. Dodatne informacije o porukama postupak iz koncentratora događaja potražite u članku vodič za [Početak rada s koncentratorima događaj] .

Test kada implementirati pouzdanog prostora za pohranu poruka točke podataka ili je taj događaj obrada prosljeđivanje interaktivne poruka ovisi o potrošača poruku da biste prikazali checkpoints tijek. Nadalje, da biste postigli visoke propusnost, kada čitate iz koncentratora događaj mora sadržavati checkpoints u velikim grupama. Taj se način stvara nastanka duplikata obrade za velik broj poruka ako postoji pogreška, a zatim se vratite na prethodni kontrolne točke. U ovom ćete praktičnom vodiču pogledajte kako sinkronizirati zapisivanja Azure prostora za pohranu i windows de-dupliciranje Bus servisa s **EventProcessorHost** checkpoints.

Da biste pouzdano pisanje poruka sa spremištem Azure uzorka koristi značajka potvrdi pojedinačne bloka [bloka blob-ova][Azure Block Blobs]. Procesor događaj akumulira poruke u memoriji dok je vrijeme možete unijeti na kontrolne točke. Na primjer, nakon skupljena međuspremnik poruka je dostigne maksimalno bloka veličinu 4 MB ili pak nakon Bus servisa de-dupliciranje minutama će se doba dana. Nakon toga prije kontrolne točke, kod se obvezuje novi Blok za blob-om.

Procesor događaj koristi događaj koncentratora pomiče poruku kao blok ID-a. U ovom mehanizam omogućuje procesor događaja da biste izvršili provjeru de-dupliciranje prije nego što je pamti novi Blok za pohranu, pobrinite moguće neočekivanog između potvrđivanja blok i Kontrolna točka.

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi jedan račun za Azure prostora za pohranu za pisanje svih poruka koje dohvaća iz koncentratora IoT. Da biste odlučili ako trebate koristiti više prostora za pohranu Azure računa u rješenje, potražite u članku [Azure prostora za pohranu skalabilnost smjernice].

Aplikacija koristi značajku de-dupliciranje Bus servisa da biste izbjegli duplikate kada obrađuje interaktivne poruke. Simulirani uređaj oznaku svaku interaktivne poruku s jedinstvenim **MessageId**. Te ID-ove omogućiti Bus servisa da biste bili sigurni da u prozoru za vrijeme navedeno de-dupliciranje bez dva poruke s istom **MessageId** isporučuju u primatelja. U ovom de-dupliciranje, zajedno s semantiku dovršetka po poruke koje ste dobili od redova Bus servisa olakšava implementirati pouzdanog obrada interaktivne poruka.

Da biste bili sigurni da ne prikazuje se poruka je ponovo poslati izvan prozora de-dupliciranje, kod sinkronizira Kontrolna točka mehanizam **EventProcessorHost** s prozorom de-dupliciranje reda čekanja Bus servisa. Ova sinkronizacije obavlja tvrtka prisilno na Kontrolna točka barem jednom svaki put kada se u prozor de-dupliciranje minutama (u ovom ćete praktičnom vodiču prozora je jedan sat).

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi jedan particioniranom servisa Bus čekanja za proces interaktivne poruke dohvaćeni iz koncentratora IoT. Dodatne informacije o korištenju servisa Bus redova da biste zadovoljava preduvjete skalabilnost vašeg rješenja potražite u dokumentaciji [Bus servisa Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Dodjela resursa za račun za Azure prostora za pohranu i servisa Bus reda čekanja
Da biste koristili klase [EventProcessorHost] , imate račun za pohranu Azure da biste omogućili **EventProcessorHost** za bilježenje kontrolne točke podataka. Možete koristiti postojeći račun za Azure pohranu ili slijedite upute [O Azure prostor za pohranu] da biste stvorili novi. Zabilježite niz za povezivanje računa Azure prostora za pohranu.

> [AZURE.NOTE] Kada kopirate i zalijepite niz za povezivanje računa Azure prostora za pohranu, provjerite je li nema razmaka uključena.

Morate reda Bus servisa da biste omogućili pouzdanog obrada interaktivne poruka. Možete stvoriti reda programski s prozorom de-dupliciranje jedan sat, kao što je opisano u [kako pomoću servisa Bus redova][čekanja Bus servisa]. Osim toga, možete koristiti [Azure klasični portal][lnk-classic-portal], slijedeći ove korake:

1. U donjem lijevom kutu kliknite **Novo** . Kliknite **Aplikaciju servisa** > **Bus servisa** > **reda čekanja** > **Stvoriti prilagođene**. Unesite naziv **d2ctutorial**, odaberite područje, a pomoću postojećih polja naziva ili stvorite novi. Na sljedećoj stranici odaberite **Omogući duplikata**pa postavite **dupliciranje doba dana povijest otkrivanje** na jedan sat. Kliknite kvačicu u donjem desnom kutu da biste spremili konfiguraciju red.

    ![Stvaranje reda u portal za Azure][30]

2. Na popisu servisa Bus redova kliknite **d2ctutorial**pa kliknite **Konfiguriraj**. Stvorite dva pravila zajednički pristup, jedan pod nazivom **Slanje** s dozvolama za **Slanje** i jednom pod nazivom **preslušali** s dozvolama za **Slušanje** . Kada završite, kliknite **Spremi** pri dnu.

    ![Konfiguriranje reda Azure portalu][31]

3. Kliknite **nadzorna ploča** pri vrhu, a zatim **podatke o vezi** pri dnu. Zabilježite nizove dvije veze.

    ![Red čekanja nadzorne ploče na portalu za Azure][32]

### <a name="create-the-event-processor"></a>Stvaranje događaja procesor

1. U trenutnom rješenju Visual Studio Stvaranje projekta Visual C# Windows pomoću predloška **Aplikacije konzole za** projekt, kliknite **datoteka** > **Dodaj** > **Novi projekt**. Provjerite nalazi li se .NET Framework verzije 4.5.1 ili noviji. Dodijelite naziv projekta **ProcessDeviceToCloudMessages**pa kliknite **u redu**.

    ![Novi projekt u Visual Studio][10]

2. U pregledniku rješenja, desnom tipkom miša kliknite **ProcessDeviceToCloudMessages** projekta, a zatim **Upravljanje NuGet paketa**. Pojavit će se dijaloški okvir **Upravitelj NuGet paketa** .

3. Traženje **WindowsAzure.ServiceBus**, kliknite **Instaliraj**i prihvatite uvjete korištenja. Ovaj postupak preuzimanja, instalira i dodaje referenca [Azure servisa Bus NuGet paketa](https://www.nuget.org/packages/WindowsAzure.ServiceBus), s njezine ovisnosti.

4. Traženje **Microsoft.Azure.ServiceBus.EventProcessorHost**, kliknite **Instaliraj**i prihvatite uvjete korištenja. Ovaj postupak preuzimanja, instalira i dodaje referenca [Azure servisa Bus događaj koncentrator - EventProcessorHost NuGet paketa](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), s njezine ovisnosti.

5. Desnom tipkom miša kliknite projekt **ProcessDeviceToCloudMessages** , kliknite **Dodaj**, a zatim kliknite **Predmet**. Naziv nove klase **StoreEventProcessor**, a zatim **u redu** da biste stvorili klasu.

6. Pri vrhu datoteke StoreEventProcessor.cs dodajte sljedeće naredbe:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Zamjena sljedeći kod za tijelo klase:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    Klase **EventProcessorHost** poziva klase obraditi uređaju u oblak poruke koje su stigle iz centra za IoT. Kod u klase implementira logike pouzdano spremanje poruka u spremniku blob i prosljeđivanje poruka interaktivne red Bus servisa.

    Način **OpenAsync** inicijalizira varijable **currentBlockInitOffset** koje prati trenutni pomak prvu poruku pročitati procesor ovaj događaj. Imajte na umu da je svaki procesor dužni jedna particija.

    Način **ProcessEventsAsync** prima skupine poruke iz koncentratora IoT te ih obrađuje na sljedeći način: šalje interaktivne poruke red Bus servisa i dodaje porukama točke podataka u memorijski međuspremnik pod nazivom **toAppend**. Ako u memorijski međuspremnik dosegne ograničenje 4 MB ili vrijeme windows de-dupliciranje minutama (jedan sat nakon kontrolne točke u ovom ćete praktičnom vodiču), aplikacija pokreće na kontrolne točke.

    Način **AppendAndCheckpoint** najprije generira blockId bloka za dodavanje. Azure prostora za pohranu potrebno sve blokirali ID-a da imam iste duljine metodu pads pomak s početne nule - `currentBlockInitOffset.ToString("0000000000000000000000000")`. Zatim, ako se blok s tim ID-om već blob-om, način je prebrisati s trenutni sadržaj u međuspremnik.

    > [AZURE.NOTE] Da biste pojednostavnili kod ovog praktičnog vodiča koristi jedan blob po particija ćete spremati poruke. Realni rješenja želite implementirati datoteke vodoravnim stvaranjem dodatnih datoteka nakon određenog vremena ili kada dođu određene veličine. Imajte na umu da je blobova platforme Azure bloka može sadržavati najviše 195 GB podataka.

8. U razredu **Program** dodajte sljedeća naredba za **Korištenje** pri vrhu:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Izmjena metoda **glavne** u **Program za** predmete na sljedeći način. **{Iot koncentrator niz za povezivanje}** zamijenite **iothubowner** niza za povezivanje iz vodiča za [Početak rada s IoT koncentratora] . Niz za povezivanje za pohranu zamijenite niz za povezivanje ste zabilježili na početku ovaj odjeljak. Zamijenite niz za povezivanje servisa Bus dozvole za **Slanje** red pod nazivom **d2ctutorial** ste zabilježili na početku ovaj odjeljak:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Radi jednostavnosti, pomoću ovog praktičnog vodiča koristi instancu klase [EventProcessorHost] . Dodatne informacije potražite u [Vodiču za programiranje događaja koncentratora].

## <a name="receive-interactive-messages"></a>Interaktivni poruka
U ovom ćete odjeljku pišete aplikacije za Windows konzole koja prima interaktivne poruke iz servisa Bus reda. Dodatne informacije o mijenjanje arhitekture rješenje pomoću servisa Bus potražite u članku [izraditi više razina aplikacije s Bus servisa][].

1. U trenutnom rješenju Visual Studio Stvaranje projekta za Visual C# Windows pomoću predloška **Aplikacije konzole za** projekt. Naziv projekta **ProcessD2CInteractiveMessages**.

2. U pregledniku rješenja, desnom tipkom miša kliknite **ProcessD2CInteractiveMessages** projekta, a zatim **Upravljanje NuGet paketa**. Ovaj postupak prikazuje prozor **Upravitelj NuGet paketa** .

3. Traženje **WindowsAzure.ServiceBus**, kliknite **Instaliraj**i prihvatite uvjete korištenja. Ovaj postupak preuzimanja, instalira i dodaje referenca [Bus servisa Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus), s njezine ovisnosti.

4. Dodajte sljedeće **pomoću** naredbe pri vrhu datoteke **Program.cs** :

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Na kraju, dodajte sljedeće retke **glavne** korakom. Zamjena niz za povezivanje s dozvolama za **Slušanje** red pod nazivom **d2ctutorial**:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1.  U Visual Studio, u pregledniku rješenja, desnom tipkom miša kliknite rješenje i odaberite **Postavljanje pokretanja projekata**. Odaberite **više projekata pokretanje**, a zatim odaberite **Počni** kao akciju za projekte **ProcessDeviceToCloudMessages**, **SimulatedDevice**i **ProcessD2CInteractiveMessages** .

2.  Pritisnite **F5** da biste pokrenuli konzole za tri aplikacije. Aplikacija **ProcessD2CInteractiveMessages** obrađivati svake interaktivne poruke poslane iz aplikacije **SimulatedDevice** .

  ![Tri konzole aplikacije][50]

> [AZURE.NOTE] Da biste vidjeli ažuriranja u vašem blob, morate smanjiti konstanta **MAX_BLOCK_SIZE** u predmete **StoreEventProcessor** manje vrijednosti, kao što je **1024**. Budući da je potrebno neko vrijeme da se dosegne ograničenje veličine bloka s podataka koji se šalju Simulirani uređaj, korisno je tu promjenu. S manjom veličinom blok ne moraju čekati toliko dugo da biste vidjeli blob stvorili i ažurirati. Međutim, pomoću veću veličinu bloka postavlja aplikaciju više skalabilni.

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako pouzdano obraditi točke podataka i interaktivnih poruke uređaj u oblak pomoću klase [EventProcessorHost] .

[Upute za slanje poruka oblak na uređaj s IoT koncentrator] [ lnk-c2d] objašnjava slanje poruka s vašeg pozadinskih na uređajima.

Primjera dovršeno završetka do kraja rješenja koje koriste IoT koncentrator potražite [Azure IoT paket][lnk-suite].

Dodatne informacije o razvoju rješenja s IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Spremište blobova platforme Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Tvorničke Azure podataka]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Red čekanja Bus servisa]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT koncentrator za razvojne inženjere vodič – uređaj da biste u oblaku]: iot-hub-devguide-messaging.md

[Azure prostora za pohranu]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Vodič za IoT koncentrator za razvojne inženjere]: iot-hub-devguide.md
[Početak rada s IoT koncentratora]: iot-hub-csharp-csharp-getstarted.md
[Razvojni centar za Azure IoT]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[O Azure prostora za pohranu]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Početak rada s koncentratorima događaja]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure prostora za pohranu skalabilnost smjernice]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Programiranje vodič koncentratora za događaj]: ../event-hubs/event-hubs-programming-guide.md
[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Izraditi više razina aplikacije pomoću servisa Bus]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
