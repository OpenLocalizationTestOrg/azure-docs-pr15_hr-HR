<properties
    pageTitle="Azure funkcije servisa Bus okidača i poveznici | Microsoft Azure"
    description="Objašnjenje kako pomoću okidača Bus servisa Azure i povezivanja u funkcijama Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcije, Funkcije, događaja obrada dinamički računalnim serverless arhitekture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure funkcije servisa Bus okidača i poveznici za redovima i teme

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

U ovom se članku objašnjava kako konfigurirati i kod Bus servisa Azure okidača i povezivanja u funkcijama Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Servis Bus reda čekanja ili temu okidača

#### <a name="functionjson"></a>Function.JSON

Datoteka *function.json* određuje sljedeća svojstva.

- `name`: Naziv varijable koristi za reda čekanja ili temu ili poruke reda čekanja ili teme u kod funkcije. 
- `queueName`: Za reda čekanja pokrenuti samo naziv čekanja za ankete.
- `topicName`: Za temu pokrenuti samo naziv temu za ankete.
- `subscriptionName`: Za temu pokrenuti samo naziv pretplate.
- `connection`: Naziv aplikacije postavka koja sadrži niza za povezivanje servisa Bus. Niz za povezivanje mora biti za servis Bus prostor naziva, nije ograničena određeni red ili temu. Ako niz za povezivanje ne je upravljanje pravima, postavite na `accessRights` svojstvo. Ako ostavite `connection` prazna, pokretanje ili povezivanje funkcioniraju s niz za povezivanje servisa Bus zadane aplikacije funkcija koja je navedena u postavkama AzureWebJobsServiceBus aplikacije.
- `accessRights`: Određuje prava pristupa za niz za povezivanje. Zadana vrijednost je `manage`. Postavite `listen` ako koristite niz za povezivanje koji se ne omogućuje upravljanje dozvolama. U suprotnom funkcija pokušajte izvođenja i Neuspjelo operacije koje je potrebno učiniti upravljanje pravima.
- `type`: Mora biti postavljeno na *serviceBusTrigger*.
- `direction`: Mora biti postavljeno na *u*. 

Primjer *function.json* u red okidača Bus servisa:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# kod primjer u kojem se obrađuje poruku Bus servis reda čekanja

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # kod primjer u kojem se obrađuje poruku Bus servis reda čekanja

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Primjer koda Node.js koji obrađuje poruku Bus servis reda čekanja

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Podržane vrste

Red čekanja poruka Bus servisa se može se deserijalizirati na bilo koju od sljedećih vrsta:

* Objekt (JSON)
* niz
* bajtni polja 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>Ponašanje PeekLock

Vrijeme izvođenja funkcija prima poruku u `PeekLock` način i pozive `Complete` na Ako funkciju uspješno Završi poruka ili poziva `Abandon` ne uspijete funkciju. Ako funkcija izvodi dulje od na `PeekLock` vremensko ograničenje, zaključavanje automatski obnoviti.

#### <a id="sbpoison"></a>Rukovanje poison porukama

Servis Bus ne svoju poison poruku rukovanje koji ne mogu kontrolirati ni konfiguriran u konfiguraciji Azure funkcije ili kod. 

#### <a id="sbsinglethread"></a>Jednom niti

Prema zadanim postavkama funkcije runtime istovremeno obrađuje više reda čekanja poruka. Da biste izravno runtime za obradu samo jedan red ili tema poruka istodobno, postavite `serviceBus.maxConcurrrentCalls` 1 u datoteci *host.json* . Informacije o *host.json* datoteka potražite u članku [Struktura mapa](functions-reference.md#folder-structure) u članku referenca za razvojne inženjere i [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) u spremištu wiki WebJobs.Script.

## <a id="sboutput"></a>Servis Bus red ili temu izlazna povezivanja

#### <a name="functionjson"></a>Function.JSON

Datoteka *function.json* određuje sljedeća svojstva.

- `name`: Naziv varijable koristi za reda čekanja ili reda čekanja poruke u kod funkcije. 
- `queueName`: Za reda čekanja pokrenuti samo naziv čekanja za ankete.
- `topicName`: Za temu pokrenuti samo naziv temu za ankete.
- `subscriptionName`: Za temu pokrenuti samo naziv pretplate.
- `connection`: Pokretanje isti kao Bus servisa.
- `accessRights`: Određuje prava pristupa za niz za povezivanje. Zadana vrijednost je `manage`. Postavite `send` ako koristite niz za povezivanje koji se ne omogućuje upravljanje dozvolama. U suprotnom funkcija pokušajte izvođenja i Neuspjelo operacije koje je potrebno učiniti upravljanje pravima, kao što su stvaranje reda čekanja.
- `type`: Mora biti postavljeno na *serviceBus*.
- `direction`: Mora biti postavljeno na *odgovor*. 

Primjer *function.json* u za korištenje timer okidača za pisanje Bus servis reda čekanja poruka:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Podržane vrste

Azure funkcije možete stvoriti Bus servis reda čekanja poruke s bilo kojeg od sljedećih vrsta.

* Objekt (uvijek stvara poruku JSON, stvara poruku s objektom null ako je vrijednost null kada istekne funkcija)
* niz (stvara poruku ako je vrijednost koja nije null kada istekne funkcija)
* raspon bajtova (funkcionira kao što je niz) 
* `BrokeredMessage`(C#, funkcionira kao što je niz)

Za stvaranje više poruka u funkciji C#, možete koristiti `ICollector<T>` ili `IAsyncCollector<T>`. Poruke se stvara prilikom pozivanja na `Add` način.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# kod Primjeri stvaranje servisa Bus reda čekanja poruka

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # kod primjer u kojem se stvara poruku Bus servis reda čekanja

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Primjer koda Node.js koji stvara poruku Bus servis reda čekanja

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
