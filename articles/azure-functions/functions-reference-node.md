<properties
    pageTitle="Referenca za razvojne inženjere Azure funkcije NodeJS | Microsoft Azure"
    description="Razumijevanje funkcije Azure pomoću NodeJS razvoju."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcije, obrada događaj, webhooks, dinamični računalnim, funkcije serverless arhitekture"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure NodeJS funkcije reference za razvojne inženjere

> [AZURE.SELECTOR]
- [C# skripte](../articles/azure-functions/functions-reference-csharp.md)
- [F # skripte](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Čvor/JavaScript sučelje za Azure funkcije olakšava izvoz funkciju koja se prenosi u `context` objekt za komunikaciju s vremena izvođenja i primanja i slanja podataka putem povezivanja.

U ovom se članku pretpostavlja da ste već pročitali [Azure funkcije reference za razvojne inženjere](functions-reference.md).

## <a name="exporting-a-function"></a>Izvoz funkcija

Sve funkcije JavaScript morate izvesti jedan `function` putem `module.exports` za vrijeme izvođenja da biste pronašli funkciju i pokrenite je. Ova funkcija moraju uvijek uvrstite na `context` objekt.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Povezivanja s `direction === "in"` prosljeđuju duž kao argumenti funkcija, što znači da koristite [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) dinamički učiniti novog unosa (Ako, na primjer, pomoću `arguments.length` za ponavljanje sve unose). Ta je funkcija je vrlo praktično ako imate samo okidača s bez dodatnih unosa predvidljivije možete pristupiti podacima okidača bez upućuju na vaš `context` objekt.

Argumenti se uvijek prosljeđuju duž funkciju redom se pojavljuju u *function.json*, čak i ako ih ne navedete u izvješćem izvozi. Na primjer, ako imate `function(context, a, b)` i promijenite ga u `function(context, a)`, i dalje možete dobiti vrijednost `b` u kod funkcije tako da upućuju na `arguments[3]`.

Sve povezivanja, bez obzira na smjer i prosljeđuju duž na `context` objekta (pogledajte informacije u nastavku). 

## <a name="context-object"></a>Kontekstni objekt

Koristi vremena izvođenja u `context` prenesite podatke iz funkcija i da biste omogućili komunikaciju s vremena izvođenja objekta.

Kontekstni objekt uvijek je prvi parametar funkciji i uvijek potrebno obuhvatiti jer kao što je metoda `context.done` i `context.log` koje su potrebne za pravilno korištenje vremena izvođenja. Možete nazvati objekt što god želite (odnosno `ctx` ili `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>context.Bindings

Na `context.bindings` objekt prikuplja svih ulazni i izlazni podataka. Podaci se dodaju na na `context.bindings` objekt putem na `name` svojstvo povezivanje. Ako, primjerice, uz sljedeću definiciju povezivanja u *function.json*, možete pristupiti sadržaj reda čekanja putem `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

Na `context.done` funkcija govori vremena izvođenja ne završite izvodi. To je važno da biste nazvali kada završite s funkcijom; Ako ih ne vremena izvođenja će i dalje nikad znati dovršiti funkcija. 

Na `context.done` funkcija omogućuje prosljeđivanje natrag korisnički definiranu pogrešku vremena izvođenja, kao i svojstvo Torba svojstva koja će prebrisati svojstva na na `context.bindings` objekt.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>context.log(Message)

Na `context.log` metode omogućuju izlaz zapisnika izraza koji se povezuju zajedno u svrhu zapisivanje. Ako koristite `console.log`, poruke će prikazati samo za razine zapisivanje procesa koji nije koristan.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

Na `context.log` način podržava isti oblik parametar koji podržava čvor [util.format način](https://nodejs.org/api/util.html#util_util_format_format) . Tako, na primjer, kod ovako:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

može se zapisati ovako:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP okidača: context.req i context.res

U slučaju okidača HTTP jer je takve uobičajeni obrazac da biste koristili `req` i `res` za HTTP zahtjeva i odgovora objekata, ne možemo odlučili da biste lakše pristupiti one na objekt kontekst, umjesto da koristite potpunu prisilno `context.bindings.name` uzorka.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Verzija čvor i upravljanje paketa

Verzija čvor trenutno zaključano pri `5.9.1`. Ne možemo trenutno istražuje dodavanjem podršku za više verzija i upućivanje konfigurirati.

Paketi možete obuhvatiti funkcija tako da prenesete *package.json* datoteke u mapu sustava funkcija u aplikaciju funkcija datotečnom sustavu. Datoteke prenesite upute, potražite u odjeljku **Ažuriranje datoteka aplikacije funkcija** [tema referenca za razvojne inženjere za Azure funkcije](functions-reference.md#fileupdate). 

Možete koristiti `npm install` u aplikaciju funkcija IO (Kudu) naredbeni redak sučelja:

1. Dođite do: `https://<function_app_name>.scm.azurewebsites.net`.

2. Kliknite **konzole za ispravljanje pogrešaka > CMD**.

3. Dođite do `D:\home\site\wwwroot\<function_name>`.

4. Pokrenite `npm install`.

Nakon što instalirate se paketa morate, uvozu na funkcija na uobičajeni način (odnosno putem `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Varijable okruženja

Da biste varijabla okruženja ili aplikacije za postavljanje vrijednosti, koristite `process.env`, kao što je prikazano u sljedećem primjeru kod:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Podrška za typeScript/CoffeeScript

Ne postoji još sve Izravni podrška za automatsko-Kompiliranje TypeScript/CoffeeScript putem izvođenja, tako da se svi koji želite morate riješiti izvan izvođenja, trenutku implementacije. 

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u sljedećim resursima:

* [Azure funkcije reference za razvojne inženjere](functions-reference.md)
* [Azure funkcije C# reference za razvojne inženjere](functions-reference-csharp.md)
* [Azure funkcije F # reference za razvojne inženjere](functions-reference-fsharp.md)
* [Azure okidača funkcije i poveznici](functions-triggers-bindings.md)
