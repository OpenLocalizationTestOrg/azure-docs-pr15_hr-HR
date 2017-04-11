<properties
    pageTitle="Azure okidača funkcije i poveznici | Microsoft Azure"
    description="Objašnjenje kako pomoću okidača i povezivanja u funkcijama Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcije, obrada događaj, webhooks, dinamični računalnim, funkcije serverless arhitekture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure funkcije okidača i poveznici reference za razvojne inženjere


Ova tema sadrži Općenito referenca za okidača i povezivanja. Obuhvaća neke značajke Napredno povezivanje i sintaksa podržava sve vrste povezivanja.  

Ako tražite detaljne informacije oko kodiranje određenu vrstu okidača ili povezivanje i konfiguriranje, trebali biste kliknite jednu od okidača ili povezivanja umjesto navedena u nastavku:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Sljedećim člancima pretpostavlja da ste pročitali [Azure funkcije reference za razvojne inženjere](functions-reference.md)i [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)ili [Node.js](functions-reference-node.md) članke referenca za razvojne inženjere.



## <a name="overview"></a>Pregled

Okidača su događaj odgovora koristiti za pokretanje prilagođeni kod. Oni omogućuju reagirati na događaje preko platforme Azure ili u lokalnu. Povezivanja predstavljaju potrebne podatke metapodataka za povezivanje kod željeni okidača ili povezane unos ili izlazne podatke.

Da biste dobili bolji uvid u različitim spajanja možete integrirati s aplikacijom funkcija Azure, potražite u sljedećoj tablici.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Da biste bolje razumjeli pokreće, a zatim povezivanja Općenito, pretpostavimo da želite izvršiti neke kod u tijeku novu stavku prekida u red čekanja za Azure prostora za pohranu. Azure funkcije nudi okidača Azure čekanja da to podržava. Koji su vam potrebni, sljedeći podaci za praćenje reda čekanja:
 
- Račun za pohranu tamo gdje postoje reda.
- Naziv reda čekanja.
- Naziv varijable koju kod koristila za upućivanje na novu stavku koja je prekida u redu čekanja.  
 
Okidač red čekanja povezivanja sadrži ove informacije o funkciji Azure. Datoteka *function.json* za svaku funkciju sadrži sve povezane povezivanja.  Slijedi primjer *function.json* koja sadrži reda pokretanje povezivanja. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Kod može poslati različite vrste izlaz ovisno o obradu nova stavka reda čekanja. Na primjer, možda ćete pisati novi zapis u tablici programa Azure prostora za pohranu.  Da biste to postigli, možete postaviti na izlazna povezivanja tablici Azure prostora za pohranu. Evo primjera *function.json* koja obuhvaća prostora za pohranu izlazna povezivanja tablice koje se mogu koristiti s okidač red. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Sljedeća C# funkcija odgovori na nove stavke koja se prekida u redu čekanja i piše novi korisnik unos u tablicu programa Azure prostora za pohranu.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Dodatne primjere koda i detaljnije informacije o vrste Azure prostora za pohranu koje su podržane potražite u članku [funkcije Azure okidača i povezivanja za Azure prostora za pohranu](functions-bindings-storage.md).


Da biste koristili naprednije značajke povezivanja na portalu za Azure, kliknite mogućnost **Napredni uređivač** na kartici **Integrate** funkcija. Napredni uređivač omogućuje uređivanje *function.json* izravno na portalu.

## <a name="dynamic-parameter-binding"></a>Dinamični parametar povezivanja 

Umjesto konfiguracije statične postavka svojstva izlazna povezivanja, možete konfigurirati postavke tako da se dinamički vezana uz podatke koji su dio vaše pokretanje ulaznog povezivanja. Zamislite gdje je novi narudžbe obrađuju pomoću red čekanja za Azure prostora za pohranu. Svaku novu stavku reda čekanja je JSON niz koji sadrži najmanje sljedeća svojstva:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Možda ćete morati poslati kupca tekstnu SMS poruku pomoću računa Twilio kao ažuriranje za redoslijed primljena.  Možete konfigurirati na `body` i `to` polja vašem Twilio izlaz povezivanje se dinamički vezana uz na `name` i `mobileNumber` koje su dio unos na sljedeći način.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Funkcija kod samo je za inicijalizaciju izlazni parametar na sljedeći način. Tijekom izvođenja svojstva izlaz će vezana uz željenu ulaznih podataka.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Slučajni GUID

Azure funkcije nudi sintaksu za generiranje slučajnog GUID s vašeg povezivanja. Sljedeću sintaksu povezivanje će pisati izlaz novi BLOB s jedinstveni naziv u spremniku programa Azure prostora za pohranu: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Vraćanje jednog Izlaz

U slučajevima gdje kod funkcija vraća jedan Izlaz, možete koristiti na izlazna povezivanja s nazivom `$return` da biste zadržali više prirodnim funkcija potpisa u kodu. To može se koristiti samo s jezika koji podržavaju povratnu vrijednost (C# Node.js, F #). Povezivanje bi slično sljedeće povezivanje izlaz blob koja se koristi s okidač red.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Sljedeći C# kod više prirodan vraća rezultat bez upotrebe programa `out` parametar u potpisu (opis funkcije).


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Taj se isti način je što je prikazano ispod s Node.js.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # primjer navedene u nastavku.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

To može se koristiti s više izlaznih parametara, označavanjem jedan izlaza pomoću `$return`.


## <a name="route-support"></a>Podrška za usmjeravanje

Prema zadanim postavkama kada stvarate funkcije za pokretanje HTTP ili WebHook, funkcija je moguće adresirati s usmjeravanje obrasca:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Moguće je prilagoditi ovu rutu pomoću neobavezna `route` svojstvo na pokretanje HTTP je unos povezivanja. Kao primjer se definira sljedeće *function.json* datoteku na `route` svojstvo okidača HTTP:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Koristite tu konfiguraciju, funkciju sada je moguće adresirati sa sljedećim usmjeravanje umjesto izvorne usmjeravanje.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Time se omogućuje kod funkcije za podršku dva parametra na adresi `category` i `id`. Možete koristiti bilo koje [Web API usmjeravanje ograničenja](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) s vaše parametara. Sljedeće C# kod funkcije čini korištenje oba parametra.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Evo Node.js funkcija kod tako da koristi iste usmjeravanje parametre.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Sve funkcije usmjerava su po zadanom mjestu s *API-jem*. Možete prilagoditi ili ukloniti pomoću prefiks na `http.routePrefix` svojstvo u datoteci *host.json* . U sljedećem primjeru uklanja usmjeravanje prefiks *api* pomoću praznog niza za prefiks u datoteci *host.json* .

    {
      "http": {
        "routePrefix": ""
      }
    }

Detaljne informacije o ažuriranju *host.json* datoteke za funkcija, potražite [kako ažurirati aplikaciju funkcija datoteke](functions-reference.md#fileupdate). 

Dodavanjem tu konfiguraciju funkciju sada je moguće adresirati sa sljedećim usmjeravanje:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Informacije o drugim svojstvima možete konfigurirati u datoteci *host.json* potražite u članku [Referenca host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u sljedećim resursima:

* [Testiranje funkcije](functions-test-a-function.md)
* [Promjena veličine funkcija](functions-scale.md)
