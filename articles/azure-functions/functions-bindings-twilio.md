<properties
    pageTitle="Povezivanje Azure funkcije Twilio | Microsoft Azure"
    description="Objašnjenje kako koristiti Twilio povezivanja s funkcijama Azure."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure Twilio funkcije izlazna povezivanja

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

U ovom se članku objašnjava kako konfigurirati i koristiti Twilio povezivanja s funkcijama Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure podržava funkcije Twilio izlaz povezivanja omogućiti vaše funkcije za slanje SMS tekstnih poruka s nekoliko redaka koda i [Twilio](https://www.twilio.com/) računa. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Function.JSON za Azure obavijesti koncentrator izlazna povezivanja

Datoteka function.json omogućuje sljedeća svojstva:

- `name`: Naziv varijable koristi za Twilio SMS tekstnu poruku u kod funkcije.
- `type`: mora biti postavljeno na *"twilioSms"*.
- `accountSid`: Ta vrijednost mora biti postavljeno na naziv postavka za App koja sadrži Twilio računa SID-a.
- `authToken`: Ta vrijednost mora biti postavljeno na naziv postavka za App koja sadrži vaš token Twilio provjeru autentičnosti.
- `to`: Tu vrijednost je postavljeno na telefonski broj koji se šalju SMS tekst.
- `from`: Tu vrijednost je postavljeno na telefonski broj koji je poslala tekstne SMS.
- `direction`: mora biti postavljeno na *"više"*.
- `body`: Tu vrijednost se može koristiti za konačnog kod tekstnu SMS poruku ako morate postaviti dinamički kod za funkcija. 

 
Primjer function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Primjer C# reda čekanja okidača s Twilio izlazna povezivanja

#### <a name="synchronous"></a>Slika

Kod sinkrono primjer za okidača reda čekanja za pohranu Azure pomoću izvan parametar tekstne poruke slati na klijentu smjestili reda.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asinkronog

Kod asinkronog primjer za okidača reda čekanja za pohranu Azure šalje tekstnu poruku klijentu smjestili reda.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Primjer Node.js reda čekanja okidača s Twilio izlazna povezivanja

U ovom se primjeru Node.js šalje tekstnu poruku klijentu smjestili reda.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
