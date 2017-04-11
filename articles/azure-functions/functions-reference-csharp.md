<properties
    pageTitle="Referenca za razvojne inženjere Azure funkcije | Microsoft Azure"
    description="Razumijevanje razvoju funkcije Azure pomoću C#."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcije, obrada događaj, webhooks, dinamični računalnim, funkcije serverless arhitekture"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure funkcije C# reference za razvojne inženjere

> [AZURE.SELECTOR]
- [C# skripte](../articles/azure-functions/functions-reference-csharp.md)
- [F # skripte](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Sučelje C# za Azure funkcije temelji se na Azure WebJobs SDK. Tokova podataka u funkcija C# putem način argumente. Nazive argumenata navedenih u `function.json`, i unaprijed definiranim nazivima za pristup elemente kao što su funkcije tokeni zapisivaču i otkazivanje.

U ovom se članku pretpostavlja da ste već pročitali [Azure funkcije reference za razvojne inženjere](functions-reference.md).

## <a name="how-csx-works"></a>Kako funkcionira .csx

Na `.csx` oblikovanje omogućuje pisanje manje "predloženog" i usredotočite se na samo C# funkcije za pisanje. Funkcije Azure samo biti sve skupa reference i prostora naziva potrebno gore vrh, kao i obično, a umjesto omatanja sve u polje naziva i predmet, možete jednostavno definirati vaše `Run` način. Ako morate unijeti sve klase, primjerice da biste definirali POCO objekte, možete upisati predmete unutar iste datoteke.

## <a name="binding-to-arguments"></a>Povezivanje s argumentima

Različite povezivanja su povezane s funkcije C# putem na `name` svojstvo u konfiguraciji *function.json* . Svaki povezivanje sadrži vlastitu podržane vrste koji bezvrijedne po povezivanje; Ako, primjerice, okidač blob podržavaju niza, u POCO ili nekoliko vrsta. Možete koristiti vrstu koja najbolje odgovara vašim potrebama. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Bilježenje u zapisnik

Da biste se prijavili Izlaz na strujanje zapisnika u C#, možete uključiti u `TraceWriter` upisali argument. Preporučujemo da nazovite ga `log`. Preporučujemo da to izbjeći `Console.Write` u funkcijama Azure.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asinkrone

Da bi se funkcije asinkronog, pomoću na `async` ključne riječi i vratite se na `Task` objekt.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Otkazivanje tokena

U određenim slučajevima može imati operacije koje su osjetljive isključuje. Dok je uvijek je najbolje kod koji se može rukovati ruši, u slučajevima gdje želite učiniti graceful zatvaranja zahtjeve, koje ste definirali u [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) upisali argument.  A `CancellationToken` objavit ćemo zajedno glavno računalo zatvaranja aktivira. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Uvoz prostora naziva

Ako morate uvesti prostora naziva, to možete učiniti tako što su uobičajeni, s na `using` uvjet.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Sljedeće prostori automatski uvoze i zbog toga su Neobavezno:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Pozivanju vanjskog skupine

Skupovi framework dodavanje reference pomoću na `#r "AssemblyName"` uputa.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Ako vam je potrebna referentni privatne skupa, možete prenijeti datoteke skupa u na `bin` mapu u odnosu na – opis funkcije i reference tako da koristi naziv (npr.  `#r "MyAssembly.dll"`). Informacije o tome da biste prenijeli datoteke u mapu (funkcija), u odjeljku sljedeće upravljanje paketa.

## <a name="package-management"></a>Upravljanje paketa

Da biste koristili NuGet paketa u funkciji C#, prenesite datoteku *project.json* na funkcije mapi u datotečnom sustavu aplikaciju (opis funkcije). Evo primjera datoteke *project.json* koji dodaje referenca Microsoft.ProjectOxford.Face verzija 1.1.0:

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

Podržana je samo .NET 4.6 Framework, pa provjerite je li određuje datoteku *project.json* `net46` kao što je prikazano ovdje.

Prilikom učitavanja datoteke *project.json* vremena izvođenja dobiva pakete i automatski dodaje reference na sklopova paketa. Morate dodati `#r "AssemblyName"` upute poslužitelja. Jednostavno dodajte potrebne `using` izjave *run.csx* datoteku da biste koristili vrstama definiranim u paketima NuGet.


### <a name="how-to-upload-a-projectjson-file"></a>Kako prenijeti datoteke project.json

1. Početak provjeravanjem je li funkcija aplikacije se izvodi koje možete učiniti tako da otvorite funkcija na portalu za Azure. 

    To također omogućuje pristup strujanje zapisnika prikazuje izlaz instalacijski paket. 

2. Da biste prenijeli datoteke project.json, koristite jedan od načina opisane u odjeljku **Ažuriranje datoteka aplikacije funkcija** [tema referenca za razvojne inženjere za Azure funkcije](functions-reference.md#fileupdate). 

3. Nakon prijenosa datoteka *project.json* potražite izlaz kao u sljedećem primjeru u funkcija je strujanje zapisnika:

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

Da biste varijabla okruženja ili aplikacije za postavljanje vrijednosti, koristite `System.Environment.GetEnvironmentVariable`, kao što je prikazano u sljedećem primjeru kod:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Ponovno korištenje .csx kod

Možete koristiti klase i metode definirano u druge datoteke *.csx* u datoteci *run.csx* . Da biste to učinili, koristite `#load` upute poslužitelja u datoteci *run.csx* , kao što je prikazano u sljedećem primjeru.

Primjer *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Primjer *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Možete koristiti relativni put s na `#load` uputa:

* `#load "mylogger.csx"`učitava datoteke koja se nalazi u mapi (opis funkcije).

* `#load "loadedfiles\mylogger.csx"`učitava datoteke koja se nalazi u mapi u mapu (opis funkcije).

* `#load "..\shared\mylogger.csx"`učitava datoteke koja se nalazi u mapi na istoj razini, kao što je funkcija mapa, odnosno neposredno ispod *wwwroot*.
 
Na `#load` uputa funkcionira samo s datotekama *.csx* (C# skripte), ne s *.cs* datoteke. 

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u sljedećim resursima:

* [Azure funkcije reference za razvojne inženjere](functions-reference.md)
* [Azure funkcije F # reference za razvojne inženjere](functions-reference-fsharp.md)
* [Azure NodeJS funkcije reference za razvojne inženjere](functions-reference-node.md)
* [Azure okidača funkcije i poveznici](functions-triggers-bindings.md)

