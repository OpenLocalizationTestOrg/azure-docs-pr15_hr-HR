<properties 
    pageTitle="Filtriranje i pretprocesnih u SDK uvida aplikaciju | Microsoft Azure" 
    description="Pisanje Telemetrijskih procesora i Telemetrijskih Initializers za SDK za filtriranje ili Dodaj svojstva podataka prije slanja za telemetriju portal za aplikacije uvida." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filtriranje i pretprocesnih telemetriju u SDK uvida aplikacije

*Aplikacija uvida je u pretpregledu.*

Možete pisati i Konfiguriraj dodatke za SDK uvida aplikacije da biste prilagodili način na koji je telemetrijskih bilježe i obrađuju prije slanja uvida aplikacije servisa. 

Trenutno su te značajke dostupne za ASP.NET SDK.

* [Uzorkovanje](app-insights-sampling.md) smanjuje količinu telemetrijskih bez utjecaja statistici. Zadržava zajedno povezane točke podataka, tako da možete se kretati među njima kada dijagnosticiranje problema. Na portalu ukupni broji se množi da biste pokrivaju za stvaranje uzoraka vaše.
* [Filtriranje s procesora Telemetrijskih](#filtering) omogućuje odabir ili izmjena telemetriju u SDK prije slanja na poslužitelj. Ako, na primjer, nije moguće smanjili količinu telemetrijskih izuzimanjem zahtjeva za iz robotima. Dok je filtriranje više osnovnih pristup smanjivanje promet od uzorkovanje. Omogućuje veću kontrolu nad što se prenose, ali morate imati na umu da utječe statistici – na primjer, ako filtriraju sve zahtjeve za uspješan.
* [Dodaj svojstva i Initializers telemetrijskih](#add-properties) sve telemetrijskih poslane iz aplikacije, uključujući telemetrijskih iz standardnog module. Na primjer, nije moguće dodati izračunate vrijednosti; ili verziju brojevima za filtriranje podataka na portalu.
* [U SDK API](app-insights-api-custom-events-metrics.md) će se koristiti za slanje prilagođenih događaja i metrike.


Prije nego što započnete:

* Instalirajte [aplikaciju uvida SDK ASP.NET v2](app-insights-asp-net.md) u svojoj aplikaciji. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filtriranje: ITelemetryProcessor

Ovu tehniku daje više izravno kontrolirati što uključen ili isključen iz toka telemetrijskih. Koristite u kombinaciji s uzorkovanje, ili zasebno.

Da biste filtrirali telemetrijskih, pisanje telemetrijskih procesora i registrirati s SDK-a. Sve telemetrijskih prolazi kroz vaš procesor, a možete ga ispustite iz toka ili Dodaj svojstva. To obuhvaća telemetrijskih iz standardni moduli kao što su prikupljanje zahtjev HTTP i prikupljanje ovisnosti te telemetrijskih ste sami napisali. Možete, primjerice, filtriraju telemetrijskih o zahtjevima robotima ili uspješno ovisnost pozive. 

> [AZURE.WARNING] Filtriranje telemetrijskih šalje SDK pomoću procesora možete skew statistike koje vidite na portalu i otežavaju slijedite povezanih stavki.
> 
> Umjesto toga, preporučujemo da koristite [uzorkovanje](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Stvaranje telemetrijskih procesor

1. Provjerite je li u aplikaciji SDK uvida u projektu verziju 2.0.0 ili noviji. Desnom tipkom miša kliknite projekt u pregledniku rješenja za Visual Studio, a zatim odaberite upravljanje NuGet paketa. U upravitelju paket NuGet, provjerite Microsoft.ApplicationInsights.Web.

1. Stvaranje filtra implementirati ITelemetryProcessor. Ovo je neku drugu mogućnost proširenja točku kao što su modul za telemetriju, telemetrijskih initializer i telemetrijskih kanala. 

    Obratite pozornost na to Telemetrijskih procesora sastavljanje je lanac obrada. Kada stvoriti instancu telemetrijskih procesor prenesite vezu do sljedećeg procesora u niz. Kada točku telemetrijskih podataka se prenosi metodu procesa, ne rad i zatim poziva sljedeći procesor Telemetriju u niz.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Posebni znak u ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Ovo nije isti sekciju koju Inicijalizacija filtar za stvaranje uzoraka.)

Prenesite vrijednosti niza iz datoteke .config unosom javno imenovana svojstva u svojoj učionici. 

> [AZURE.WARNING] Pobrinuti tako da odgovara nazivu vrste i svojstva imena u datoteci .config nazivi klasa i svojstva u kod. Ako se datoteka .config odnosi vrste nepostojećeg ili svojstvo, SDK tihu možda neće uspjeti da biste poslali sve telemetrijskih.

 
**Osim toga,** možete pokrenuti filtar u kod. U odgovarajuću Inicijalizacija predmete – na primjer AppStart u Global.asax.cs - Umetanje vaš procesor lanac:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients stvorene nakon ove točke koristit će vaše procesora.

### <a name="example-filters"></a>Primjer filtara

#### <a name="synthetic-requests"></a>Stilova sintetičkih zahtjeva

Filtriranje testira robotima i na webu. Iako metriku Explorer nudi mogućnost filtriranja stilova sintetičkih izvora, tu mogućnost smanjuje promet filtriranjem ih na SDK-a.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Neuspjele provjere autentičnosti

Filtriranje zahtjeva za odgovor "401". 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Filtriranje brzo udaljene ovisnost poziva

Ako samo želite dijagnosticiranje poziva sporo, filtriraju one brzo. 

> [AZURE.NOTE] To će skew statistike koje vidite na portalu. Ovisnost grafikon će izgledati kao da se pozivi ovisnost su sve pogreške.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Dijagnosticiranje problema ovisnost

[U ovom blogu](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) opisuju projekta za dijagnosticiranje problema ovisnost slanjem automatski običnog Pingovi ovisnosti.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Dodaj svojstva: ITelemetryInitializer

Koristite telemetrijskih initializers da biste definirali globalnih svojstava koje se šalju sa svim telemetrijskih; a da biste nadjačali odabrane ponašanje standardni telemetrijskih module. 

Ako, na primjer, uvida aplikacije paketa Web prikuplja telemetrijskih o HTTP zahtjeva. Po zadanom je označava kao nije uspjela svaki zahtjev odgovor koda > = 400. No ako želite 400 Smatraj vjerojatnost_u, možete unijeti telemetrijskih initializer koji postavlja svojstva za uspjeh.

Ako unesete telemetrijskih initializer, on se zove kad god se neki od načina Track*() naziva. To obuhvaća metode pozove standardni telemetrijskih module. Po konvencije, te moduli postavljeni neko svojstvo koji je već postavljen tako da je initializer. 

**Definiranje sustava initializer**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Učitavanje vaše initializer**

U ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Osim toga,* možete stvoriti instancu initializer u kodu, na primjer u Global.aspx.cs:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Pogledajte informacije u ovom primjer.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>Initializers telemetrijskih JavaScript

*JavaScript*

Umetanje telemetrijskih initializer odmah nakon koda za inicijalizaciju koju ste dobili na portalu: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Sažetak svojstava nije prilagođeno dostupna na na telemetryItem potražite u članku [podatkovnog modela](app-insights-export-data-model.md#lttelemetrytypegt).

Možete dodati proizvoljan broj initializers po želji. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor i ITelemetryInitializer

Koja je razlika između telemetrijskih procesora i telemetrijskih initializers?

* Postoje neka preklapa u što možete raditi s njima: obje se može koristiti za dodavanje svojstva za telemetriju.
* Uvijek pokrenuti prije TelemetryProcessors TelemetryInitializers.
* TelemetryProcessors omogućuju potpuno zamijeniti ili odbaciti telemetrijskih stavke.
* TelemetryProcessors ne procesa telemetrijskih brojač performansi.



## <a name="persistence-channel"></a>Postojanost kanala 

Ako aplikaciju izvodi gdje internetska veza nije uvijek dostupna ili spor, preporučujemo da koristite kanala postojanost umjesto zadani u memoriji kanal. 

Zadani kanal u memoriji gubi sve telemetrijskih koje nisu poslane po vremenu Zatvara aplikaciju. Iako možete koristiti `Flush()` da biste poslali sve podatke u međuspremnik, ona i dalje gubi podataka ako nema internetske veze, ili ako aplikaciju zatvara prije dovršetka prijenosa.

Za razliku od toga kanala postojanost međuspremnika telemetriju u datoteci, prije no što pošaljete na portalu. `Flush()`osigurava podaci se pohranjuju u datoteci. Ako se neki podaci ne šalju po vremenu Zatvara aplikaciju, on će ostati u datoteci. Prilikom ponovnog pokretanja aplikaciju podatke pa će biti poslan li povezani s Internetom. Podaci akumulira u datoteci za dugo kao što je potrebno dok je dostupna veza. 

### <a name="to-use-the-persistence-channel"></a>Da biste koristili postojanost kanala

1. Uvoz paketa NuGet [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Uključi kod u aplikaciji programa odgovarajuću Inicijalizacija mjesto:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Korištenje `telemetryClient.Flush()` prije aplikacije zatvara, da biste bili sigurni podaci se šalje na portal sustava ili spremiti datoteku.

    Imajte na umu da Flush() sinkrono postojanost kanala, ali asinkronog za drugih kanala.

 
Kanal postojanost optimiziran je za uređaje scenarije, gdje je broj događaja koje je stvorio program relativno malen i veza se često nepouzdanih. Ovaj kanal će pisanje događaja na disk u pouzdanog prostora za pohranu najprije i pokušaju poslati. 

#### <a name="example"></a>Primjer

Recimo da želite nadzirati neobrađenu iznimke. Se pretplatite na `UnhandledException` događaj. U povratnog, uključiti poziv pražnjenje da biste bili sigurni da se telemetrijskih je ista i.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Kada aplikacija zatvara, vidjet ćete datoteke u `%LocalAppData%\Microsoft\ApplicationInsights\`, koja sadrži Komprimirana događaja. 
 
Kada sljedeći put pokrenete aplikaciju, kanal će obraditi tu datoteku i izlaganje telemetrijskih do uvida aplikacije ako je to moguće.

#### <a name="test-example"></a>Primjer test

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Kod za kanal postojanost nalazi se na [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Pregled dokumenata

* [Pregled API-JA](app-insights-api-custom-events-metrics.md)

* [Referenca platforme ASP.NET](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>Kod SDK

* [Temeljni ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Daljnji koraci


* [Zapisnici i događaje pretraživanja][diagnostic]
* [Stvaranje uzoraka](app-insights-sampling.md)
* [Otklanjanje poteškoća][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
