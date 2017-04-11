<properties
    pageTitle="Azure funkcije F # reference za razvojne inženjere | Microsoft Azure"
    description="Razumijevanje razvoju funkcije Azure pomoću F #."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure funkcije, Funkcije, a zatim događaj obradu webhooks, dinamični računalnim, serverless arhitektura, F#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Referenca za Azure funkcije F # za razvojne inženjere

> [AZURE.SELECTOR]
- [C# skripte](../articles/azure-functions/functions-reference-csharp.md)
- [F # skripte](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # za Azure funkcije je rješenje jednostavno radi male dijelove koda ili "funkcije," u oblaku. Tokova podataka u funkcija F # putem PIN-a. Nazive argumenata navedenih u `function.json`, i unaprijed definiranim nazivima za pristup elemente kao što su funkcije tokeni zapisivaču i otkazivanje.

U ovom se članku pretpostavlja da ste već pročitali [Azure funkcije reference za razvojne inženjere](functions-reference.md).

## <a name="how-fsx-works"></a>Kako funkcionira .fsx

U `.fsx` je datoteka skripte F #. To možete smatrati F # projekta programa koji je sadržan u jednoj datoteci. Datoteka sadrži oba kod programa (u ovom slučaju funkcija Azure) i upute poslužitelja za upravljanje ovisnosti.

Prilikom korištenja programa `.fsx` za funkciji Azure često potrebno sklopova automatski uključene su za koji vam omogućuje fokusiranje na kod funkcije umjesto "predloženog".

## <a name="binding-to-arguments"></a>Povezivanje s argumentima

Svaki povezivanje podržava neke skup argumente kao detaljne [okidača funkcije Azure i reference za razvojne inženjere povezivanja](functions-triggers-bindings.md). Na primjer, jedan argument spajanja okidač blob podržava je POCO koji može biti izražena pomoću zapis F #. Ako, na primjer:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Azure funkcija F # se jedan ili više argumenata. Kada ćemo objasniti što argumenata funkcije Azure, ne možemo pogledajte _unos_ argumenata i _Izlaz_ argumente. Unos je argument točno ga Zvuči kao: unos Azure funkcija F #. Argument _Izlaz_ je mutable podataka ili `byref<>` argument koje služi kao način za prosljeđivanje podataka sigurnosno _izvan_ vaše funkcije.

U primjeru iznad, `blob` je argument unosa i `output` je argument izlaz. Uočite da koristi `byref<>` za `output` (nema potrebe da biste dodali u `[<Out>]` opaske). Pomoću programa `byref<>` vrsta omogućuje funkcija da biste promijenili koji zapis ili objekt argument odnosi.

Kada se zapis F # se koristi kao vrstu unosa, definiciju zapis mora biti označene s `[<CLIMutable>]` da bi se Dopusti framework Azure funkcije da biste pravilno postaviti polja prije nego što prosljeđivanje zapis u funkcija. U odjeljku Napredne postavke, `[<CLIMutable>]` generira setters za svojstvima zapisa. Ako, na primjer:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Do F # klase može se koristiti za oba i iščezava argumente. Predmet, svojstva obično ćete getters i setters. Ako, na primjer:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Bilježenje u zapisnik

Da biste se prijavili Izlaz na [strujanje zapisnika](../app-service-web/web-sites-streaming-logs-and-console.md) u F #, funkcija morate poduzeti argument vrste `TraceWriter`. Dosljednost, preporučujemo da ovaj argument zove `log`. Ako, na primjer:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asinkrone

Na `async` tijek rada možete koristiti, ali potrebno je rezultat da biste se vratili na `Task`. To se može učiniti s `Async.StartAsTask`, na primjer:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Otkazivanje tokena

Ako je potrebno funkcija za rukovanje zatvaranja bez poteškoća, možete joj je [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument. Time se kombinirati s `async`, na primjer:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Uvoz prostora naziva

Prostori naziva je moguće otvoriti na uobičajen način:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Automatski se otvaraju prostori sljedeće:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Pozivanju vanjskog skupine

Isto tako, framework skupa reference se dodati pomoću na `#r "AssemblyName"` uputa.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Funkcija Azure postavljeno okruženje automatski dodao sljedećih skupova:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

K tome, su sljedeće sklopova posebno cased i mogu pozivati simplename (npr. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Ako vam je potrebna referentni privatne skupa, možete prenijeti datoteke skupa u na `bin` mapu u odnosu na – opis funkcije i reference tako da koristi naziv (npr.  `#r "MyAssembly.dll"`). Informacije o tome da biste prenijeli datoteke u mapu (funkcija), u odjeljku sljedeće upravljanje paketa.

## <a name="editor-prelude"></a>Uređivač Prelude

Uređivač koji podržava usluga Compiler F # neće biti svjesni prostora naziva i skupovi koji se automatski uključuje Azure funkcije. Kao takve, može biti korisno za uključivanje prelude koji olakšava uređivač pronaći sklopova koristite i izričito otvorite prostora naziva. Ako, na primjer:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Funkcija Azure izvršava kodu, obrađuje izvorišnog web-mjesta s `COMPILED` definirali, pa će se zanemariti prelude uređivač.

## <a name="package-management"></a>Upravljanje paketa

Da biste koristili NuGet paketa u funkciji F #, dodajte na `project.json` datoteke na funkcije mapi u datotečnom sustavu aplikaciju (opis funkcije). Slijedi primjer `project.json` datoteku koja se dodaje referenca paket NuGet `Microsoft.ProjectOxford.Face` verzija 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Podržana je samo .NET 4.6 Framework, pa provjerite je li vaša `project.json` datoteka određuje `net46` kao što je prikazano ovdje.

Kada prenesete na `project.json` datoteku, a zatim vremena izvođenja dobiva pakete i automatski dodaje reference na sklopova paketa. Morate dodati `#r "AssemblyName"` upute poslužitelja. Jednostavno dodajte potrebne `open` naredbi za vaše `.fsx` datoteku.

Možda želite pohraniti automatski sklopova reference na uređivač prelude, da biste poboljšali vaš uređivač interakcije s F # Kompiliranje servisima.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Kako dodati na `project.json` datoteke za Azure funkcija

1. Početak provjeravanjem je li funkcija aplikacije se izvodi koje možete učiniti tako da otvorite funkcija na portalu za Azure. To također omogućuje pristup strujanje zapisnika prikazuje izlaz instalacijski paket.

2. Da biste prenijeli na `project.json` datoteka, primijenite jednu od metoda što je opisano [kako ažurirati datoteka aplikacije (opis funkcije)](functions-reference.md#fileupdate). Ako koristite [Neprekinuti implementacije za Azure funkcije](functions-continuous-deployment.md), možete dodati na `project.json` datoteke za vaše pripremna granu da bi se Eksperimentirajte s njim prije dodavanja vaše implementacije grani.

3. Nakon što u `project.json` datoteka je dodana, vidjet ćete izlaz slično kao u sljedećem primjeru u funkcija je strujanje zapisnika:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Varijable okruženja

Da biste varijabla okruženja ili aplikacije za postavljanje vrijednosti, koristite `System.Environment.GetEnvironmentVariable`, na primjer:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Ponovno korištenje .fsx kod

Možete koristiti šifru od drugih `.fsx` datoteka pomoću programa `#load` uputa. Ako, na primjer:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Omogućuje putova u `#load` su uputa u odnosu na mjesto na `.fsx` datoteke.

* `#load "logger.fsx"`učitava datoteke koja se nalazi u mapi (opis funkcije).

* `#load "package\logger.fsx"`učitava datoteke koja se nalazi u na `package` mape u mapi (opis funkcije).

* `#load "..\shared\mylogger.fsx"`učitava datoteke koja se nalazi u na `shared` mapu na istoj razini, kao što je funkcija mapa, odnosno neposredno ispod `wwwroot`.

Na `#load` uputa funkcionira samo s `.fsx` datoteke (F # skripti), a ne s `.fs` datoteke.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u sljedećim resursima:

* [Vodič za F #](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure funkcije reference za razvojne inženjere](functions-reference.md)
* [Azure funkcije C# reference za razvojne inženjere](functions-reference-csharp.md)
* [Azure NodeJS funkcije reference za razvojne inženjere](functions-reference-node.md)
* [Azure okidača funkcije i poveznici](functions-triggers-bindings.md)
* [Azure funkcije testiranje](functions-test-a-function.md)
* [Azure funkcije skaliranja](functions-scale.md)
