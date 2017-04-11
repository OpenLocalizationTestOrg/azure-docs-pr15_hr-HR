<properties
    pageTitle="Azure funkcije obavijesti koncentrator povezivanje | Microsoft Azure"
    description="Objašnjenje kako koristiti Azure obavijesti koncentrator povezivanja u funkcijama Azure."
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
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Koncentrator za Azure obavijesti o funkcije izlazna povezivanja

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

U ovom se članku objašnjava kako konfigurirati i kod Azure obavijesti koncentrator povezivanja u funkcijama Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Vaš funkcije možete poslati automatske obavijesti konfigurirani koncentrator obavijesti Azure pomoću nekoliko redaka koda. Međutim, obavijest o središtu Azure mora konfigurirati za na platformi obavijesti Services (PNS) koji želite koristiti. Dodatne informacije o konfiguriranju koncentrator za obavijesti programa Azure i razvoj klijentske aplikacije koje se registrirati za primanje obavijesti potražite u članku [Uvod u rad s koncentratorima obavijesti](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) , a zatim kliknite ciljne platforme za klijenta u pri vrhu.

Obavijesti koje šaljete mogu biti nativni obavijesti ili predložak obavijesti. Izvorni obavijesti ciljne platforme za određene obavijesti konfigurirano u na `platform` svojstvo izlazna povezivanja. Ciljne platforme više može se koristiti predložak obavijest.   

## <a name="notification-hub-output-binding-properties"></a>Obavijest o koncentrator izlazna povezivanja svojstva

Datoteka function.json omogućuje sljedeća svojstva:

- `name`: Naziv varijable koristi za poruke s obavijesti koncentratora u kod funkcije.
- `type`: mora biti postavljeno na *"notificationHub"*.
- `tagExpression`: Izrazi oznake omogućuju da navedete obavijesti isporuku skup uređaje koje ste registrirali za primanje obavijesti koji se podudaraju izraz oznake.  Dodatne informacije potražite u članku [usmjeravanje i oznaka izraze](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Naziv resursa koncentrator obavijesti na portalu za Azure.
- `connection`: Ovaj niz za povezivanje mora biti postavljeno na vrijednost *DefaultFullSharedAccessSignature* koncentratora za obavijesti **Postavku aplikacije** niz veze.
- `direction`: mora biti postavljeno na *"više"*. 
- `platform`: Platformu svojstvo označava platformu obavijesti na ciljeve obavijesti. Mora biti u jednom od sljedećih vrijednosti:
    - `template`: To je zadani platformu Ako svojstvo platformu izostavi izlazna povezivanja. Predložak obavijesti se može koristiti za ciljnu bilo koju platformu konfigurirali središtu Azure obavijesti. Dodatne informacije o korištenju predložaka Općenito za slanje obavijesti unakrsni platformu s koncentrator za obavijesti programa Azure, potražite u članku [Predlošci](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Servis Apple automatske obavijesti. Dodatne informacije o konfiguriranju središtu obavijesti za APN-ove i primanje obavijesti u aplikaciji klijenta potražite u članku [Slanje slanje obavijesti za iOS uz Azure obavijesti koncentratora](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Poruka na uređaju Amazon](https://developer.amazon.com/device-messaging). Dodatne informacije o konfiguriranju središtu obavijesti za Admin i primanje obavijesti u aplikaciji na uređaju Kindle potražite u članku [Uvod u obavijesti koncentratora za aplikacije na uređaju Kindle](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud poruka](https://developers.google.com/cloud-messaging/). Firebase oblaka poruke, što je nova verzija GCM podržano je i. Dodatne informacije o konfiguriranju središtu obavijesti za GCM/FCM i primanje obavijesti u aplikaciji sustava Android klijenta, potražite u članku [Slanje slanje obavijesti za Android s koncentratorima obavijesti Azure](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows automatske obavijesti Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) ciljne platforme Windows. Windows Phone 8.1 i noviji i podržava WNS. Dodatne informacije o konfiguriranju središtu obavijesti za WNS i primanje obavijesti u aplikaciji univerzalni platforme Windows (UWP) potražite u članku [Uvod u rad s obavijesti koncentratora za Windows univerzalni platformu aplikacije](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Servis Microsoft automatske obavijesti](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). U ovom platformu podržava Windows Phone 8 i starijim platforme Windows Phone. Dodatne informacije o konfiguriranju središtu obavijesti za MPNS i primanje obavijesti u aplikaciji Windows Phone potražite u članku [Slanje automatskih obavijesti s Azure koncentratora obavijesti na uređaju Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Primjer function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Postava niza veze koncentrator za obavijesti

Da biste koristili izlaz koncentrator za obavijesti povezivanje, morate konfigurirati niz za povezivanje za središtu. To možete učiniti na kartici *Integrate* odabirom koncentratora za obavijesti ili stvaranje novog. 

Niz za povezivanje za postojeće koncentratora možete dodati i ručno dodavanjem niza za povezivanje za *DefaultFullSharedAccessSignature* koncentratora za obavijesti. Niz za povezivanje sadrži funkcija pristup dozvolu za slanje obavijesti o. Vrijednost niza za povezivanje *DefaultFullSharedAccessSignature* možete pristupiti klikom na gumb **tipke** u glavnom plohu obavijesti resursa za koncentratora u na portalu za Azure. Da biste ručno dodali niza za povezivanje za vaš koncentrator, poduzmite sljedeće korake: 

1. Na plohu **funkcija aplikacije** portala za Azure, kliknite **funkcija postavki aplikacije > Idi na postavke aplikacije servisa za**.

2. U plohu **Postavke** kliknite **Postavke aplikacije**.

3. Pomaknite se prema dolje do odjeljka **nizove veze** i dodajte imenovane stavke *DefaultFullSharedAccessSignature* vrijednosti za koncentratora za obavijesti. Promjena vrste **Prilagođeno**.
4. Referenca naziva niz veze u izlazna povezivanja. Slično **MyHubConnectionString** u primjeru iznad.

## <a name="native-notification-examples"></a>Primjeri nativni obavijesti

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>Izvorni obavijesti APN-ove s okidačima reda čekanja C#

U ovom se primjeru pokazuje kako koristiti vrste koji su definirani u [Biblioteci koncentratora za Microsoft Azure obavijesti](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) da biste poslali izvorni obavijest APN-ove. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM nativni obavijesti s okidačima reda čekanja C#

U ovom se primjeru pokazuje kako koristiti vrste koji su definirani u [Biblioteci koncentratora za Microsoft Azure obavijesti](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) da biste poslali izvorni GCM obavijest. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS nativni obavijesti s okidačima reda čekanja C#

U ovom se primjeru pokazuje kako koristiti vrste koji su definirani u [Biblioteci koncentratora za Microsoft Azure obavijesti](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) da biste poslali izvorni WNS skočnoj obavijesti. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Primjeri obavijesti predloška

#### <a name="template-example-for-nodejs-timer-triggers"></a>Primjer predloška za okidača Node.js mjerača vremena 

U ovom se primjeru će poslati primatelju obavijest za [registraciju predložak](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) koji sadrži `location` i `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Primjer mjerača vremena okidača za F # predloška

U ovom se primjeru će poslati primatelju obavijest za [registraciju predložak](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) koji sadrži `location` i `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Predložak se primjeru koristi izvan parametra 

U ovom se primjeru će poslati primatelju obavijest za [registraciju predložak](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) koji sadrži na `message` rezervirano mjesto u predlošku.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Primjer predloška s asinkronog (funkcija)

Ako koristite asinkronog kod, izgleda parametara nije dopušteno. U ovom slučaju koristite `IAsyncCollector` da biste se vratili na predlošku obavijesti. Sljedeći kod je primjer asinkronog gore navedeni kod. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Predložak se primjeru koristi JSON 

U ovom se primjeru će poslati primatelju obavijest za [registraciju predložak](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) koji sadrži na `message` rezervirano mjesto u predlošku pomoću valjani niz JSON.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Predložak se primjeru koristi vrste biblioteka koncentratora obavijesti

U ovom se primjeru pokazuje kako koristiti vrste koji su definirani u [Biblioteci koncentratora za Microsoft Azure obavijesti](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
