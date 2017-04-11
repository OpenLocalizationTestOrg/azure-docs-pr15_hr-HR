<properties
    pageTitle="Azure funkcije događaj koncentrator povezivanja | Microsoft Azure"
    description="Objašnjenje kako koristiti Azure događaj koncentrator povezivanja u funkcijama Azure."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure funkcije događaj koncentrator povezivanja

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

U ovom se članku objašnjava kako konfigurirati i kod povezivanja [Azure događaj koncentrator](../event-hubs/event-hubs-overview.md) za Azure funkcije. Azure funkcije podržava okidača i izlaz povezivanja za Azure koncentratora za događaj.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Ako ste novi korisnik Azure događaj koncentratora, potražite u članku [Pregled Azure koncentrator za događaj](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure koncentrator događaj pokretanje povezivanja

Okidača Azure događaj koncentrator može se koristiti za odgovor na događaj poslane na događaj strujanje koncentrator za događaj. Mora imati pristup čitanje koncentrator događaja da biste postavili okidača povezivanja.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Function.JSON za događaj koncentrator pokretanje povezivanja

Datoteke *function.json* za okidača Azure događaj koncentrator određuje sljedeća svojstva:

- `type`: Mora biti postavljeno na *eventHubTrigger*.
- `name`: Naziv varijable koristi u kod funkcije za poruku koncentrator za događaj. 
- `direction`: Mora biti postavljeno na *u*. 
- `path`: Naziv koncentratora za događaj.
- `consumerGroup`: To je neobavezno svojstvo koji se koristi za postavljanje [korisničke grupe](../event-hubs-overview.md#consumer-groups) koristi da biste se pretplatili na događaje u središtu. Ako se ispusti, u `$Default` koristi korisničke grupe. 
- `connection`: Naziv aplikacije postavka koja sadrži niz za povezivanje za prostor za naziv koji se nalazi središtu događaja u. Klikom na gumb **Podatke o vezi** za prostor za naziv, ne događaj središtu sam kopirajte niz za povezivanje.  Niz za povezivanje morate imati barem dozvole za čitanje aktivirati okidača.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure primjer okidača C# koncentratora za događaj
 
Pomoću function.json primjer iznad tijela poruke događaj zapisat će se pomoću C# kod funkcije ispod:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Azure primjer okidača F # koncentratora za događaj

Pomoću function.json primjer iznad tijela poruke događaj zapisat će se pomoću F # kod funkcije ispod:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure događaj koncentrator okidača Node.js primjer
 
Pomoću function.json primjer iznad tijela poruke događaj zapisat će se pomoću ispod Node.js kod funkcije:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure događaj koncentrator izlazna povezivanja

Koncentrator za događaj programa Azure izlazna povezivanja služi za zapisivanje događaja na događaj strujanje koncentrator za događaj. Morate imati dozvolu slanja u događaj koncentrator zapisivanja događaja u njega. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Function.JSON za događaj koncentrator izlazna povezivanja

U datoteci *function.json* koncentrator za događaj programa Azure izlaz povezivanje određuje sljedeća svojstva:

- `type`: Mora biti postavljeno na *eventHub*.
- `name`: Naziv varijable koristi u kod funkcije za poruku koncentrator za događaj. 
- `path`: Naziv koncentratora za događaj.
- `connection`: Naziv aplikacije postavka koja sadrži niz za povezivanje za prostor za naziv koji se nalazi središtu događaja u. Klikom na gumb **Podatke o vezi** za prostor za naziv, ne događaj središtu sam kopirajte niz za povezivanje.  Niz za povezivanje, morate imati dozvole Pošalji da biste poslali poruku strujanje koncentratora za događaj.
- `direction`: Mora biti postavljeno na *odgovor*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure događaj koncentrator C# kod primjer izlazna povezivanja
 
Sljedeći C# primjeru funkcija kod pokazuje zapisivanje događaja programa strujanje događaja koncentratora za događaj. U ovom primjeru predstavlja središtu događaj izlazna povezivanja gore navedenoj sintaksi primjenjuje se na timer okidača C#.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure događaj koncentrator F # primjer koda za izlazna povezivanja

Sljedeći F # primjeru funkcija kod pokazuje zapisivanje događaja programa strujanje događaja koncentratora za događaj. U ovom primjeru predstavlja središtu događaj izlazna povezivanja gore navedenoj sintaksi primjenjuje se na timer okidača C#.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure primjer koda Node.js događaj koncentrator za izlazna povezivanja
 
Sljedeći kod funkcija Node.js primjer pokazuje zapisivanje događaja programa strujanje događaja koncentratora za događaj. U ovom primjeru predstavlja središtu događaj izlazna povezivanja gore navedenoj sintaksi primjenjuje se na okidač Node.js mjerača vremena.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
