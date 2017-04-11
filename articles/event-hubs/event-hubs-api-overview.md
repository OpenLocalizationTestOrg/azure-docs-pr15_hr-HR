<properties 
    pageTitle="Pregled događaja Azure koncentratora API-ji | Microsoft Azure"
    description="Sažetak neke ključne događaj koncentratora .NET klijent API-ji."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Pregled događaja koncentratorima API-JA

U ovom se članku ukratko opisane neke ključne događaj koncentratora .NET klijent API-ji. Postoje dvije kategorije: upravljanje i API-ji izvođenju. API-ji izvođenju sastoje se od sve operacije potrebne za slanje i primanje poruka. Operacija upravljanja omogućuju upravljanje stanju entitet događaj koncentratorima stvaranje, ažuriranje i brisanje entiteti.

Scenariji za nadzor obuhvaćaju upravljanje i izvođenju. Dokumentacija referentne na .NET API-ji, potražite u članku reference [.NET Bus servisa](https://msdn.microsoft.com/library/azure/mt419900.aspx) i [EventProcessorHost API -JA](https://msdn.microsoft.com/library/azure/mt445521.aspx) .

## <a name="management-apis"></a>Upravljanje API-ji

Da biste izveli sljedeće postupke za upravljanje, morate imati dozvole za **Upravljanje** na događaj koncentratorima naziva:

### <a name="create"></a>Stvaranje

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Ažuriranje

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Brisanje

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>API-ji izvođenju

### <a name="create-publisher"></a>Stvaranje programa publisher

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Objavljivanje poruke

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Stvaranje korisnika

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Korištenje poruka

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Procesor događaj glavno računalo API-ji

Ove API-ji pružaju otpornost radnih procesa koji se mogu postati nedostupne, raspodijeliti shards preko zaposlenici zaduženi za dostupna.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

Sučelje [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) se definira na sljedeći način:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o scenariji koncentratora događaj, posjetite sljedećih veza:

- [Što je koncentratora događaj Azure?](event-hubs-what-is-event-hubs.md)
- [Pregled događaja koncentratora](event-hubs-overview.md)
- [Programiranje vodič koncentratora za događaj](event-hubs-programming-guide.md)
- [Primjere koda koncentratora za događaj](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

Ovdje su .NET API reference:

- [Servis Bus i događaja koncentratora .NET API referenci](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Procesor događaj glavno računalo API reference](https://msdn.microsoft.com/library/azure/mt445521.aspx)
