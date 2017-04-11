<properties
    pageTitle="Azure HTTP funkcije i webhook povezivanja | Microsoft Azure"
    description="Objašnjenje kako pomoću okidača HTTP- a webhook i povezivanja u funkcijama Azure."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure HTTP funkcije i webhook povezivanja

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

U ovom se članku objašnjava kako konfigurirati i kod HTTP- a webhook okidača i povezivanja u funkcijama Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON za HTTP- a webhook povezivanja

Datoteka *function.json* omogućuje svojstva koji se odnose na zahtjeva i odgovora.

Svojstva za HTTP zahtjev:

- `name`: Naziv varijable koristi u kod funkcije za zahtjev za objekt (ili tijelu zahtjeva slučaju funkcije Node.js).
- `type`: Mora biti postavljeno na *httpTrigger*.
- `direction`: Mora biti postavljeno na *u*. 
- `webHookType`: Za okidača WebHook valjane su vrijednosti *github*, *Prazan hod*i *genericJson*. Za pokretanje HTTP nije u WebHook, postavite svojstvo prazan niz. Dodatne informacije o WebHooks potražite u članku u sljedećoj sekciji [WebHook okidača](#webhook-triggers) .
- `authLevel`: Ne odnosi na WebHook okidača. Postavljeno na "funkcije" zahtijeva API ključ "anonimni" ključa zahtjeva ispustite na API-JA ili "administrator" obavezne glavni ključ za API-JA. Dodatne informacije potražite u članku [API tipke](#apikeys) ispod.

Svojstva za HTTP odgovor:

- `name`: Naziv varijable koristi u kod funkcije objekta odgovor.
- `type`: Mora biti postavljeno na *HTTP-a*.
- `direction`: Mora biti postavljeno na *odgovor*. 
 
Primjer *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook okidača

Okidač WebHook je okidača HTTP koji je namijenjen WebHooks sljedeće značajke:

* Za određene WebHook davatelje usluge (trenutno GitHub i Prazan hod podržani) vašeg davatelja usluge potpis Provjeri valjanost izvođenja funkcije.
* Funkcija Node.js izvođenja funkcije predstavlja tijelu zahtjeva umjesto objekt zahtjev. Nema ne posebni postupak C# u funkcijama, jer kontrolirati navedeni navođenjem vrsta parametra. Ako navedete `HttpRequestMessage` dobijete zahtjev objekt. Ako navedete vrstu POCO, izvođenja funkcije pokušava raščlaniti JSON objekt u tijelu zahtjeva za popunjavanje svojstva objekta.
* Da biste pokrenuli u WebHook funkcija HTTP zahtjev mora sadržavati ključa API-JA. Taj zahtjev za okidača koji nisu iz programa WebHook HTTP nije obavezno.

Informacije o postavljanju GitHub WebHook potražite u članku [GitHub za razvojne inženjere – stvaranje WebHooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>URL za pokretanje funkcija

Da biste pokrenuli funkciju, pošaljite zahtjev HTTP URL koji je kombinacija funkcija aplikacije URL-a i naziva funkcije:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>Strelicama API-JA

Prema zadanim postavkama ključa API mora biti uključeno s HTTP zahtjev za pokretanje programa HTTP-a ili WebHook (opis funkcije). Tipku možete uvrstiti u varijablu niza upita pod nazivom `code`, ili možete uvrstiti u programa `x-functions-key` HTTP zaglavlje. U funkcijama koje nisu WebHook možete označiti ključa API nije obavezno tako da postavite na `authLevel` svojstvo "anonimni" u datoteci *function.json* .

Vrijednosti ključa API-JA možete pronaći u mapi *D:\home\data\Functions\secrets* u datotečnom sustavu aplikacije (opis funkcije).  Glavni ključ i funkcijskom tipkom se postavljaju u datoteci *host.json* , kao što je prikazano u ovom primjeru. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Funkcijska tipka iz *host.json* može se koristiti za pokretanje bilo koju funkciju, ali ne aktiviraju onemogućene funkcije. Glavni ključ može se koristiti za pokretanje bilo koju funkciju i će pokrenuti funkciju čak i ako je onemogućen. Funkcija obavezne glavnog ključa postavljanjem možete konfigurirati na `authLevel` svojstvo "administrator". 

Ako mapa *tajne* sadrži JSON datoteka s istim nazivom kao funkciju, na `key` svojstvo u toj datoteci može se koristiti za pokretanje funkciju i ovog ključa funkcionirat će samo s funkcijom na koju se odnosi. Ako, na primjer, ključ API-JA za funkciju pod nazivom `HttpTrigger` je naveden u *HttpTrigger.json* u mapi *tajne* . Evo jednog primjera:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Kada postavljate okidač WebHook, ne objavljuj glavni ključ kod davatelja WebHook. Pomoću tipke koji funkcioniraju samo s funkcijom obrađuje na WebHook.  Glavni ključ može se koristiti za pokretanje bilo koju funkciju čak i onemogućiti funkcije.

## <a name="example-c-code-for-an-http-trigger-function"></a>Primjer C# kod za pokretanje funkciji HTTP 

Primjer kod Traži u `name` parametara u nizu upita ili u tijelu zahtjeva za HTTP.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>Primjer F # kod za pokretanje funkciji HTTP

Primjer kod Traži u `name` parametara u nizu upita ili u tijelu zahtjeva za HTTP.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Potrebno je na `project.json` datoteku koja koristi NuGet reference na `FSharp.Interop.Dynamic` i `Dynamitey` skupine, ovako:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

To će koristiti NuGet vaše ovisnosti za dohvaćanje i će uputiti na njih u skriptu.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Primjer Node.js kod za pokretanje funkciji HTTP 

U ovom primjeru kod Traži u `name` parametara u nizu upita ili u tijelu zahtjeva za HTTP.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Primjer C# kod za GitHub WebHook (funkcija) 

U ovom primjeru kod evidentira GitHub problem komentare.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Primjer F # kod za GitHub WebHook (funkcija)

U ovom primjeru kod evidentira GitHub problem komentare.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Primjer Node.js kod za GitHub WebHook (funkcija) 

U ovom primjeru kod evidentira GitHub problem komentare.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
