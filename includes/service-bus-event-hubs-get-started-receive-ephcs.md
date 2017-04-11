## <a name="receive-messages-with-eventprocessorhost"></a>Primanje poruka uz EventProcessorHost

[EventProcessorHost][] je klasu .NET koja pojednostavljuje primanju događaje iz koncentratora događaja po upravljanje stalni checkpoints i paralelno prima iz koncentratora te događaj. Pomoću [EventProcessorHost][], možete razdvojiti događaje preko većem broju primatelja, čak i kad je smješten u različitim čvorove. U ovom se primjeru pokazuje kako koristiti [EventProcessorHost][] za jednog tekstnog okvira. Uzorak [Scaled out događaj obrade][] pokazuje kako koristiti [EventProcessorHost][] s više primatelja.

Da biste koristili [EventProcessorHost][], morate imati [račun za Azure pohranu][]:

1. Prijavite se na [portal za Azure][]i kliknite **Novo** u gornjem lijevom kutu zaslona.

2. Kliknite **Podaci + prostor za pohranu**, a zatim kliknite **račun za pohranu**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. U plohu **Stvaranje računa za pohranu** unesite naziv računa za pohranu. Odaberite poruku Azure pretplatu, grupa resursa i mjesto na kojem želite stvoriti resurs. Zatim kliknite **Stvori**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Na popisu prostora za pohranu računa kliknite račun novostvorenu prostora za pohranu.

5. U računu plohu prostora za pohranu kliknite **pristupnih tipki**. Kopirajte vrijednost **ključ1** da biste koristili u nastavku ovog praktičnog vodiča.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. U Visual Studio, stvorite novi projekt Visual C# radnu površinu aplikacije pomoću ovog predloška **Aplikacije konzole za** projekt. Naziv projekta **tekstnog okvira**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. U pregledniku rješenja, desnom tipkom miša kliknite rješenje, a zatim **Upravljanje NuGet paketa rješenja**.

6. Kliknite karticu **Pregled** , a zatim potražite `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Provjerite je li navedena naziv projekta (**tekstnog okvira**) u okviru **verzije** . Kliknite **Instaliraj**i prihvatite uvjete korištenja.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio preuzimanja, instalira i dodaje referenca [Azure servisa Bus događaj koncentrator - EventProcessorHost NuGet paketa](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), s njezine ovisnosti.

7. Desnom tipkom miša kliknite projekt **tekstnog okvira** , kliknite **Dodaj**, a zatim kliknite **Predmet**. Naziv nove klase **SimpleEventProcessor**, a zatim kliknite **Dodaj** da biste stvorili klasu.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. Pri vrhu datoteke SimpleEventProcessor.cs dodajte sljedeće naredbe:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Zatim, zamijenite sljedeći kod za tijelo klase:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Klase će pozove **EventProcessorHost** na događaje postupak primili iz centra za događaj. Imajte na umu da se `SimpleEventProcessor` predmete koristi u štoperice nazvati povremeno metodu kontrolne točke na **EventProcessorHost** kontekstu. Time se osigurava da, ako ponovnog pokretanja primatelja nestat će više od pet minuta obrade rad.

9. U razredu **Program** dodajte sljedeće `using` izjava pri vrhu datoteke:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Nakon toga zamjena na `Main` način u na `Program` predmete s sljedeći kod zamjenjujući naziv događaja koncentratora i niz za povezivanje prostor naziva razini koju ste spremili u prethodno, i račun za pohranu i ključ koji ste kopirali u prethodnom odjeljku. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi instancu [EventProcessorHost][]. Da biste povećali propusnost, preporučuje se da pokrenete više instanci [EventProcessorHost][], kao što je prikazano u uzorku [Scaled out obrada događaj][] . U tim slučajevima različite instance automatski koordiniranje međusobno da biste učitali saldo primljene događaja. Ako želite više primatelja svakog procesa *sve* događaje, morate koristiti **ConsumerGroup** pojam. Prilikom primanja događaje s različitim računalima, može biti korisno da biste odredili imena instanci [EventProcessorHost][] koji se temelji na računalima (ili uloga) u koje se primjenjuju. Dodatne informacije o ovim temama potražite u članku [Pregled koncentratora za događaj][] i [Vodiču za programiranje događaja koncentratora][] temama.

<!-- Links -->
[Pregled događaja koncentratora]: ../articles/event-hubs/event-hubs-overview.md
[Programiranje vodič koncentratora za događaj]: ../articles/event-hubs/event-hubs-programming-guide.md
[Skalirana out obrada događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Račun za Azure prostora za pohranu]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Portal za Azure]: https://portal.azure.com