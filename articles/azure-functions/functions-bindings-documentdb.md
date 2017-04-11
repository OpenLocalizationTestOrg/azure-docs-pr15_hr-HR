<properties
    pageTitle="Azure DocumentDB funkcije povezivanja | Microsoft Azure"
    description="Objašnjenje kako koristiti Azure DocumentDB povezivanja u funkcijama Azure."
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

# <a name="azure-functions-documentdb-bindings"></a>Azure DocumentDB funkcije povezivanja

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

U ovom se članku objašnjava kako konfigurirati i kod Azure DocumentDB povezivanja u funkcijama Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB unos povezivanja

Unos povezivanja možete učitati dokumenta iz zbirke DocumentDB i prenesite izravno na povezivanja. ID-a dokumenta možete odrediti koji se temelji na okidača koja se poziva funkciju. U C# funkciji, sve promjene zapisa poslat će se automatski natrag u zbirci kada funkciju zatvara uspješno.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON za DocumentDB unos povezivanja

Datoteka *function.json* omogućuje sljedeća svojstva:

- `name`: Varijable naziv koji se koristi funkcija koda za dokument.
- `type`: mora biti postavljeno na "documentdb".
- `databaseName`: Bazu podataka koja sadrži dokument.
- `collectionName`Zbirka: koja sadrži dokument.
- `id`: Id dokumenta za dohvaćanje. Ovo svojstvo podržava povezivanja slično "{queueTrigger}", koji će koristiti vrijednost niza reda čekanja poruke kao dokument ID-a.
- `connection`: Ovaj niz mora biti skupa postavku aplikacije za krajnje točke za vaš račun DocumentDB. Ako odaberete računa na kartici Integrate, Nova postavka aplikacija će biti stvorena umjesto vas pod nazivom koja vas vodi sljedeći obrazac yourAccount_DOCUMENTDB. Ako je potrebno ručno stvoriti postavka App stvarni veza_niz morate preuzeti sljedeći obrazac, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- "smjer: mora biti postavljeno na *"u"*.

Primjer *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure primjer unos koda DocumentDB reda čekanja okidača C#
 
Korištenje function.json primjer iznad, povezivanje unos DocumentDB će prenesite parametar "dokument" i dohvatiti dokument s ID-a koji se podudara s nizom reda čekanja poruka. Ako taj dokument nije moguće pronaći, parametar "dokument" će biti null. Dokument je ažurirat s novom vrijednosti tekst kada zatvara funkciju.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure primjer DocumentDB unos koda za F # reda čekanja okidača

Korištenje function.json primjer iznad, povezivanje unos DocumentDB će prenesite parametar "dokument" i dohvatiti dokument s ID-a koji se podudara s nizom reda čekanja poruka. Ako taj dokument nije moguće pronaći, parametar "dokument" će biti null. Dokument je ažurirat s novom vrijednosti tekst kada zatvara funkciju.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Potrebno je na `project.json` datoteku koja koristi NuGet da biste odredili na `FSharp.Interop.Dynamic` i `Dynamitey` paketa kao paket ovisnosti, ovako:

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

To će koristiti NuGet vaše ovisnosti za dohvaćanje i će uputiti na njih u skriptu.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure primjer unos koda DocumentDB okidača Node.js reda čekanja
 
Korištenje function.json primjeru iznad, povezivanje unos DocumentDB će dohvatiti dokument s ID-a koji se podudara s nizom reda čekanja poruka i ga da biste na `documentIn` povezivanje svojstva. U funkcijama Node.js ažuriranih dokumenata ne šalju natrag zbirke. Međutim, možete proslijediti unos povezivanje izravno u DocumentDB izlazna povezivanja imenovani `documentOut` za podršku ažuriranja. U ovom se primjeru kod ažurira svojstvo text unos dokumenta i postavlja kao dokument izlaz.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB izlazna povezivanja

Vaš funkcije možete napisati JSON dokumenata za Azure DocumentDB baze podataka pomoću **Azure DocumentDB dokument** izlazna povezivanja. Dodatne informacije o Azure DocumentDB pregledajte [Uvod u DocumentDB](../documentdb/documentdb-introduction.md) i [Vodič za početak rada](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON za DocumentDB izlaz povezivanja

Datoteka function.json omogućuje sljedeća svojstva:

- `name`: Naziv varijable koristi u kod funkcije za novi dokument.
- `type`: mora biti postavljeno na *"documentdb"*.
- `databaseName`: Bazu podataka koja sadrži zbirke u kojem će se stvoriti novi dokument.
- `collectionName`: Zbirka u kojem će se stvoriti novi dokument.
- `createIfNotExists`: To je Booleova vrijednost koji određuju hoće li se u zbirku će stvoriti ako ne postoji. Zadana je vrijednost *false*. Razlog za to je nove zbirke stvaraju se rezervirane propusnost koja ima cijene posljedice. Dodatne informacije, posjetite [cijene stranice](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Ovaj niz mora biti **Postavku aplikacije** postavljena na krajnjoj točki za vaš račun DocumentDB. Ako odaberete računa na kartici **Integrate** , Nova postavka aplikacije stvorit će se za vas s nazivom koji vodi na obrascu za sljedeće `yourAccount_DOCUMENTDB`. Ako je potrebno ručno stvoriti postavka App stvarni veza_niz morate preuzeti sljedeći obrazac `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: mora biti postavljeno na *"više"*. 
 
Primjer function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB izlaz primjer koda okidača Node.js reda čekanja

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Izlaz dokumenta:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB izlaz primjer koda za F # reda čekanja okidača

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB izlaz primjer koda reda čekanja okidača C#


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB kod primjer postavka naziv izlazne datoteke

Ako želite postaviti naziv dokumenta u funkciji, postavite na `id` vrijednost.  Na primjer, ako JSON sadržaja za zaposlenika je u tijeku prekine u redu čekanja sličnu ovoj:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Sljedeći C# kod možete koristiti u funkciji okidača reda čekanja: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Ili na ekvivalentnu F # kod:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Primjer izlaz:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
