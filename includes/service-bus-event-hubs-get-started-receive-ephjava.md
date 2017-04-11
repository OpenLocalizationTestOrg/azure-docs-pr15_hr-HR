## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Primanje poruka uz EventProcessorHost u Java

EventProcessorHost je klase jezika Java koji pojednostavljuje primanju događaje iz koncentratora događaja po upravljanje stalni checkpoints i paralelno prima iz koncentratora te događaj. Pomoću EventProcessorHost možete razdvojiti događaje preko većem broju primatelja, čak i kad je smješten u različitim čvorove. U ovom se primjeru pokazuje kako koristiti EventProcessorHost za jednog tekstnog okvira.

###<a name="create-a-storage-account"></a>Stvaranje računa za pohranu

Da biste koristili EventProcessorHost, morate imati [račun za Azure pohranu][]:

1. Prijavite se na [portal za Azure klasični][]pa kliknite **NOVO** pri dnu zaslona.

2. Kliknite **Podatkovne usluge**, a zatim **prostora za pohranu**, a zatim **Brzo stvaranje**, a zatim upišite naziv za vaš račun za pohranu. Odaberite željeni regiju, a zatim kliknite **Stvori račun za pohranu**.

    ![][11]

3. Kliknite račun novostvorenu prostora za pohranu, a zatim **Upravljanje pristupnih tipki**:

    ![][12]

    Kopirajte primarni pristupni ključ za korištenje u nastavku ovog praktičnog vodiča.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Stvaranje projekta Java pomoću EventProcessor glavno računalo

Biblioteka klijenta Java za koncentratora događaj nije dostupan za korištenje u Maven projektima u [Središnjem spremniku Maven][Maven Package], a možete se pozivati pomoću sljedećih deklariranje ovisnost unutar Maven datoteku projekta:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Za različite vrste Sastavi okruženja, izričito možete nabaviti najnoviji objavljenu POSUDU datoteke u [Središnjem spremniku Maven] [ Maven Package] ili od [točke raspodjele izdanje na GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Sljedeći primjer prvo stvorite novi projekt Maven konzole/ljuske aplikacije u omiljene Java razvojno okruženje. Klasa će se pozivati ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Koristite sljedeći kod da biste stvorili novi klase pod nazivom ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Stvorili konačni predmete naziva ```EventProcessorSample```, pomoću koda za sljedeće.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Sljedeća polja zamijenite vrijednosti koji se koriste prilikom stvaranja koncentratora za događaj i račun za pohranu.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi instancu EventProcessorHost. Da biste povećali propusnost, preporučuje se da pokrenete više instanci EventProcessorHost. U tim slučajevima različite instance automatski koordiniranje međusobno učitavanje saldo primljene događaja. Ako želite više primatelja svakog procesa *sve* događaje, morate koristiti **ConsumerGroup** pojam. Prilikom primanja događaje s različitim računalima, može biti korisno da biste odredili imena instanci EventProcessorHost koji se temelji na računalima (ili uloga) u koje se primjenjuju.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Račun za Azure prostora za pohranu]: ../articles/storage/storage-create-storage-account.md
[Azure klasični portal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

