<properties
    pageTitle="Azure okidača funkcije i poveznici za pohranu Azure | Microsoft Azure"
    description="Objašnjenje kako pomoću okidača Azure prostora za pohranu i povezivanja u funkcijama Azure."
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
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure okidača funkcije i poveznici za pohranu za Azure

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ovaj članak objašnjava kako konfigurirati i okidača kod Azure prostora za pohranu i povezivanja u funkcijama Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure okidača reda čekanja za pohranu

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON za pohranu reda čekanja okidača

Datoteka *function.json* određuje sljedeća svojstva.

- `name`: Naziv varijable koristiti u kodu funkcija reda čekanja ili reda čekanja poruka. 
- `queueName`: Naziv čekanja za ankete. Red čekanja pravila za imenovanje potražite u članku [imenovanja redovima i metapodacima](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Naziv aplikacije postavka koja sadrži niza za povezivanje za pohranu. Ako ostavite `connection` prazna, okidača funkcioniraju s niz za povezivanje zadani prostor za pohranu za aplikaciju funkciju, koja je navedena u postavkama AzureWebJobsStorage aplikacije.
- `type`: Mora biti postavljeno na *queueTrigger*.
- `direction`: Mora biti postavljeno na *u*. 

Primjer *function.json* u redu čekanja okidača za pohranu:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Red čekanja okidača podržane vrste

Red čekanja poruka se može se deserijalizirati na bilo koju od sljedećih vrsta:

* Objekt (JSON)
* Niz
* Raspon bajtova 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Red čekanja okidača metapodataka

Red čekanja metapodataka možete nabaviti na funkcija pomoću ove nazivima varijabli:

* expirationTime
* insertionTime
* nextVisibleTime
* ID-a
* popReceipt
* dequeueCount
* queueTrigger (drugi način za dohvaćanje reda čekanja tekst poruke kao niz znakova)

Dohvaća podatke u ovom primjeru koda za C# i zapisnika red čekanja metapodataka:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Rukovanje poison reda čekanja poruke

Poruke čiji je sadržaj uzrokuje funkcija uvoza nazivaju *poison poruke*. Ako funkcija ne uspije, poruke reda čekanja neće se izbrisati naposljetku je sakrije i ponovno uzrokuje ciklusa se ponavljaju. SDK možete automatski prekinuti ciklusu nakon ograničen broj ponavljanja ili to možete učiniti ručno.

SDK će funkcije do 5 puta pozovete da obradi reda čekanja poruke. Peti pokušajte ne uspije, poruke se premješta u poison red.

Red čekanja za poison pod nazivom *{originalqueuename}*-poison. Možete napisati potreban je funkcija proces poruke iz poison reda čekanja tako da ih zapisivanju ili slanja obavijesti te ručno pažnje. 

Ako želite ručno rukovati poison poruke, možete dobiti koliko je puta poruke izdvojeni prema gore za obradu potvrđivanjem `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure reda čekanja za pohranu izlazna povezivanja

#### <a name="functionjson-for-storage-queue-output-binding"></a>Function.JSON red za pohranu izlazna povezivanja

Datoteka *function.json* određuje sljedeća svojstva.

- `name`: Naziv varijable koristiti u kodu funkcija reda čekanja ili reda čekanja poruka. 
- `queueName`: Naziv reda. Red čekanja pravila za imenovanje potražite u članku [imenovanja redovima i metapodacima](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Naziv aplikacije postavka koja sadrži niza za povezivanje za pohranu. Ako ostavite `connection` prazna, okidača funkcioniraju s niz za povezivanje zadani prostor za pohranu za aplikaciju funkciju, koja je navedena u postavkama AzureWebJobsStorage aplikacije.
- `type`: Mora biti postavljeno na *red*.
- `direction`: Mora biti postavljeno na *odgovor*. 

Primjer *function.json* na red za pohranu izlaz povezivanje koji koristi okidač reda čekanja i piše reda čekanja poruka:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Red čekanja povezivanja izlaznu podržane vrste

Na `queue` povezivanja možete serijalizirati sljedećim vrstama reda čekanja poruku:

* Objekt (`out T` u C#, stvara poruku s objektom null ako je parametar null kada istekne funkcija)
* Niz (`out string` u C#, stvara reda čekanja poruku ako je vrijednost parametra koje nisu null kada istekne funkcija)
* Raspon bajtova (`out byte[]` u C#, način funkcioniranja niz) 
* `out CloudQueueMessage`(C#, funkcionira kao što je niz) 

U C# možete se povezati `ICollector<T>` ili `IAsyncCollector<T>` gdje `T` je jedna od podržanih vrsta.

#### <a name="queue-output-binding-code-examples"></a>Red čekanja izlazna povezivanja s kodom Primjeri

U ovom primjeru koda za C# ispisuje jedan izlazne poruke reda čekanja za svaki unos reda čekanja poruku.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

U ovom primjeru koda za C# piše više poruka pomoću `ICollector<T>` (pomoću `IAsyncCollector<T>` u funkciji asinkrone):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure spremište blobova platforme okidača

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON za spremište blobova platforme okidača

Datoteka *function.json* određuje sljedeća svojstva.

- `name`: Naziv varijable koristi u kod funkcije za blob-om. 
- `path`: Put koji određuje spremnik praćenje, a po želji uzorak blob naziv.
- `connection`: Naziv aplikacije postavka koja sadrži niza za povezivanje za pohranu. Ako ostavite `connection` prazna, okidača funkcioniraju s niz za povezivanje zadani prostor za pohranu za aplikaciju funkcija koja je navedena u postavkama AzureWebJobsStorage aplikaciju.
- `type`: Mora biti postavljeno na *blobTrigger*.
- `direction`: Mora biti postavljeno na *u*.

Primjer *function.json* u okidača blob prostora za pohranu koji nadzirane za blob-ova koji su dodani u spremnik uzoraka workitems:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Pokretanje blob podržane vrste

Blob-om moguće je deserijalizirati na bilo koju od sljedećih vrsta u funkcijama čvor ili C#:

* Objekt (JSON)
* Niz

U funkcijama C# možete povezati na bilo koju od sljedećih vrsta:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Druge vrste deserijalizirati po [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

#### <a name="blob-trigger-c-code-example"></a>Blob okidača C# kod primjer

C# kod u primjeru zapisuje sadržaj svaki blob u kojem se dodaje se nadziranim kontejner.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>Uzorci ime okidača za blob

Možete navesti naziv uzorak blob u na `path` svojstvo. Ako, na primjer:

```json
"path": "input/original-{name}",
```

Ovaj put pronaći blob pod nazivom *Izvornik Blob1.txt* u spremnik za *unos* te vrijednost u `name` varijablu u kod funkcije bio `Blob1`.

Drugi primjer:

```json
"path": "input/{blobname}.{blobextension}",
```

Ovaj put želite pronaći i blob pod nazivom *Izvornik Blob1.txt*i vrijednost u `blobname` i `blobextension` varijable u kod funkcije bio *Izvornik Blob1* i *txt*.

Možete ograničiti vrste blob-ova pokrenuti funkciju navođenjem uzorak fixed vrijednošću za datotečni nastavak. Ako ste postavili u `path` da *biste/uzoraka / {naziv} .png*, samo *.png* blob-ova u spremniku *uzoraka* će pokrenuti funkciju.

Ako vam je potrebna za određivanje uzorka naziv za blob nazive koji imaju vitičaste zagrade u nazivu, možete primijeniti dvostruki vitičaste zagrade. Na primjer, ako želite pronaći blob-ova u spremniku *slike* koji su nazivi ovako:

        {20140101}-soundfile.mp3

koristi se za na `path` svojstvo:

        images/{{20140101}}-{name}

U primjeru u `name` varijable vrijednost bio *soundfile.mp3*. 

#### <a name="blob-receipts"></a>Blob potvrde

Vrijeme izvođenja funkcija Azure jamči da ne sadrži funkciju okidača blob dobiva se zove više puta za istu blob novi ili ažurirani. To čini čuvanjem *blob potvrde* da bi se odredili Ako navedeni blob verziju obrađen.

Potvrde blob spremaju se u kontejner *azure webjobs domaćini* u račun za Azure prostora za pohranu određen AzureWebJobsStorage niz za povezivanje. Potvrdu blob sadrži sljedeće podatke:

* Funkcija koja je pod nazivom blob-om ("*{naziv funkcije aplikacije}*. Funkcije. *{naziv funkcije}*", na primjer:"functionsf74b96f7. Functions.CopyBlob")
* Naziv spremnika
* Vrsta blob ("BlockBlob" ili "PageBlob")
* Naziv blobova platforme
* U e-oznake (identifikator za verziju blob, na primjer: "0x8D1DC6E70A277EF")

Ako želite da biste nametnuli reprocessing od blob potvrdu blobova platforme za taj blob ručno možete izbrisati iz spremnik *azure webjobs domaćini* .

#### <a name="handling-poison-blobs"></a>Rukovanje poison blob-ova

Ako funkcija okidača blob ne uspije, SDK poziva ga ponovno u slučaju pogreške koji je uzrok tranzitne pogreške. Ako pogrešku uzrokuje sadržaj blob-om, funkcija neće uspjeti svaki put kada se pokuša obradi blob-om. Prema zadanim postavkama SDK poziva funkcije do 5 puta za dani blob. Peti pokušajte ne uspije, SDK dodaje poruke red pod nazivom *webjobs blobtrigger poison*.

Poruka reda čekanja za poison blob-Ova je JSON objekt koji sadrži sljedeća svojstva:

* FunctionId (u obliku *{naziv funkcije aplikacije}*. Funkcije. *{naziv funkcije}*)
* BlobType ("BlockBlob" ili "PageBlob")
* Naziv spremnika
* BlobName
* E-oznake (identifikator za verziju blob, na primjer: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>Blob provjere za velike spremnika

Ako spremnik blob koji nadzire okidača sadrži više od 10 000 blob-ova, preglede za vrijeme izvođenja funkcija prijaviti datoteke da biste pogledali za nove ili promijenjene blob-ova. Ovaj postupak nije u stvarnom vremenu; Funkcija možda neće se aktivira tek nekoliko minuta ili više nakon stvaranja blob-om. Osim toga, osnovicu [za pohranu zapisnika stvaraju "najbolje naporima"](https://msdn.microsoft.com/library/azure/hh343262.aspx) ; Nema jamstva da će zabilježene svih događaja. U nekim uvjetima možda Propušteni zapisnika. Ako brzine i pouzdanosti ograničenja blob okidača za velike spremnika nisu prihvatljivi aplikacije, preporučeni način je stvaranje reda čekanja poruke kada stvorite blob-om, a koristite okidač reda čekanja umjesto blob okidača za obradu blob-om.
 
## <a id="storageblobbindings"></a>Spremište blobova platforme Azure ulazni i izlazni povezivanja

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON za spremište blobova unese ili izlazna povezivanja

Datoteka *function.json* određuje sljedeća svojstva.

- `name`: Naziv varijable koristi u kod funkcije za blob-om. 
- `path`: Put koji određuje spremnik za čitanje blob iz ili pisanje blob-om za, a po želji uzorak blob naziv.
- `connection`: Naziv aplikacije postavka koja sadrži niza za povezivanje za pohranu. Ako ostavite `connection` prazna, povezivanje funkcioniraju s niz za povezivanje zadani prostor za pohranu za aplikaciju funkciju, koja je navedena u postavkama AzureWebJobsStorage aplikacije.
- `type`: Mora biti postavljeno na *blob*.
- `direction`: Postavite *unutra* ili prema *van*. 

Primjer *function.json* u prostor za pohranu bloba unos ili izlazna povezivanja, korištenja okidača reda čekanja da biste kopirali blob:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>Blob ulazni i izlazni podržane vrste

Na `blob` povezivanja možete serijalizirati ili ukloniti serijski broj sljedeće funkcije Node.js i C#:

* Objekt (`out T` u C# za izlaz blob: stvara blob kao objekt null ako je vrijednost parametra null kada istekne funkcija)
* Niz (`out string` u C# za izlaz blob: stvara blob samo ako je parametar niza koje nisu null kada funkcija vraća)

U C# funkcijama, možete povezati sa sljedećim vrstama:

* `TextReader`(samo za unos)
* `TextWriter`(samo za izlaz)
* `Stream`
* `CloudBlobStream`(samo za izlaz)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>Blob izlaz C# kod primjer

U ovom primjeru koda za C# kopira blob čiji naziv je primljene u redu čekanja poruke.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure tablice za pohranu za unos i izlazna povezivanja

#### <a name="functionjson-for-storage-tables"></a>Function.JSON za pohranu tablice

*Function.json* određuje sljedeća svojstva.

- `name`: Naziv varijable koristi za povezivanje tablica u kod funkcije. 
- `tableName`: Naziv tablice.
- `partitionKey`i `rowKey` : zajednički koristi za čitanje jedan entitet u funkciji C# ili čvor ili upišite jedan entitet čvor funkcije.
- `take`: Maksimalan broj redaka za čitanje za tablicu za unos u funkciji čvor.
- `filter`: OData izraz filtra za tablicu za unos u funkciji čvor.
- `connection`: Naziv aplikacije postavka koja sadrži niza za povezivanje za pohranu. Ako ostavite `connection` prazna, povezivanje funkcioniraju s niz za povezivanje zadani prostor za pohranu za aplikaciju funkciju, koja je navedena u postavkama AzureWebJobsStorage aplikacije.
- `type`: Mora biti postavljeno na *tablicu*.
- `direction`: Postavite *unutra* ili prema *van*. 

Sljedeći primjer *function.json* koristi okidač reda čekanja da biste pročitali jedan redak tablice. Na JSON nudi vrijednost ključa programiranih particija i određuje tipku redak potječe reda čekanja poruka.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Prostor za pohranu tablice ulazni i izlazni podržane vrste

Na `table` povezivanja možete serijalizirati ili ukloniti serijski broj objekata u funkcijama Node.js ili C#. Objekti će imati svojstva RowKey i PartitionKey. 

U C# funkcijama, možete povezati sa sljedećim vrstama:

* `T`gdje `T` implementira`ITableEntity`
* `IQueryable<T>`(samo za unos)
* `ICollector<T>`(samo za izlaz)
* `IAsyncCollector<T>`(samo za izlaz)

#### <a name="storage-tables-binding-scenarios"></a>Spremište tablica scenariji za povezivanje

Povezivanje tablice podržava sljedeće scenarije:

* Jedan redak u funkciji C# ili čvor za čitanje.

    Postavljanje `partitionKey` i `rowKey`. Na `filter` i `take` u tom slučaju neće koristiti svojstva.

* Pročitajte više redaka u funkciji C#.

    Vrijeme izvođenja funkcija nudi programa `IQueryable<T>` objekt povezan s tablicom. Vrsta `T` mora proizlaziti iz `TableEntity` ili implementirati `ITableEntity`. U `partitionKey`, `rowKey`, `filter`, a `take` u tom slučaju neće koristiti svojstva možete koristiti u `IQueryable` objekt da biste učinili filtriranje obavezno. 

* Pročitajte više redaka u funkciji čvor.

    Postavljanje na `filter` i `take` svojstva. Ne postavite `partitionKey` ili `rowKey`.

* Upišite jedan ili više redaka u funkciji C#.

    Funkcije runtime omogućuje programa `ICollector<T>` ili `IAsyncCollector<T>` povezan s tablicom koju `T` određuje sheme entiteti koju želite dodati. Obično upišite `T` izvedena iz `TableEntity` ili implementira `ITableEntity`, ali ne mora. U `partitionKey`, `rowKey`, `filter`, a `take` u tom slučaju neće koristiti svojstva.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Primjer tablice za pohranu: pročitajte u jednoj tablici entitet C# ili čvor

Sljedeći C# kod primjer funkcionira s prethodnom datoteke *function.json* ranije prikazane čitati u jednoj tablici entitet. Red čekanja poruka ima vrijednost ključa retka i entitet tablice je za čitanje u vrstu podataka koja je definirana u datoteci *run.csx* . Sadrži vrstu `PartitionKey` i `RowKey` svojstva, a ne slijedi iz `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Sljedeći F # kod primjer funkcionira i pomoću prethodne *function.json* datoteke da biste pročitali entitet jednu tablicu.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Sljedeći primjer koda čvor funkcionira i sa prethodni *function.json* datoteka za čitanje u jednoj tablici entitet.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Primjer tablice za pohranu: Pročitajte više tablica entiteti u C# 

Sljedeći *function.json* i C# kod primjer čitanja entiteti za particija ključ koji je naveden u poruci red.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C# kod dodaje referenca za pohranu SDK Azure tako da možete vrstu entitet proizlaziti iz `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Primjer tablice za pohranu: Stvaranje tablice entiteti u C# 

Sljedeći primjer *function.json* i *run.csx* prikazuje kako napisati entiteti tablice u C#.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Primjer tablice za pohranu: Stvaranje tablice entiteti u F#

Sljedeći primjer *function.json* i *run.fsx* prikazuje kako napisati entiteti tablice u F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Primjer tablice za pohranu: Stvaranje tablice entitet u čvor

Sljedeći primjer *function.json* i *run.csx* prikazuje kako napisati tablice entitet u čvor.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
