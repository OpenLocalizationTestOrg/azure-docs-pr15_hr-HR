<properties 
    pageTitle="Konfiguriranje aplikacije uvida SDK ApplicationInsights.config ili .xml" 
    description="Omogućiti ili onemogućiti moduli zbirke podataka, i dodajte mjerača performansi i drugi parametri" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Konfiguriranje aplikacije uvida SDK ApplicationInsights.config ili .xml

SDK za .NET uvida aplikacije sastoji se od brojnih NuGet paketa. [Jezgra paket](http://www.nuget.org/packages/Microsoft.ApplicationInsights) sadrži na API-JA za slanje telemetrijskih aplikacije uvid u. [Dodatni paketa](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) omogućavaju telemetrijskih _moduli_ i _initializers_ automatsko praćenje telemetrijskih iz aplikacije i njegovog konteksta. Prilagodbom konfiguracijska datoteka možete omogućiti ili onemogućiti module za telemetriju i initializers i postavljanje parametara za neke od njih.

Konfiguracijska datoteka zove `ApplicationInsights.config` ili `ApplicationInsights.xml`, zavisno o vrsti aplikacije. Automatski dodaje u projekt prilikom [instalacije Većina verzija SDK][start]. Ona se dodaje i web-aplikacijama po [Nadzornik stanja na poslužitelj za IIS][redfield], ili kada odaberete Appplication uvida [proširenja za Azure web-mjesta ili VM](app-insights-azure-web-apps.md).

Ne postoji ekvivalentan datoteku da biste odredili [SDK na web-stranici][client].

U ovom dokumentu opisuje sekcije potražite u konfiguraciji datoteke, kako mogu kontrolirati komponente u SDK i koje paketa NuGet učitavanje te komponente.

## <a name="telemetry-modules-aspnet"></a>Moduli za telemetriju (ASP.NET)

Svakom modulu telemetrijskih prikuplja određene vrste podataka i koristi Jezgra API-JA za slanje podataka. Module instaliraju različite pakete NuGet koji dodati potrebnih redaka .config datoteci.

Postoji čvor u datoteci konfiguracije za svakom modulu. Da biste onemogućili modulu, izbrišite čvor ili ga komentar.



### <a name="dependency-tracking"></a>Ovisnost praćenja

[Ovisnost praćenja](app-insights-dependencies.md) prikuplja telemetrijskih o pozivima aplikacije unese baze podataka i vanjske servise i baza podataka. Da biste omogućili ovu modula za rad u IIS poslužitelj, morate [instalirati Nadzornik stanja][redfield]. Da biste koristili u Azure web-aplikacijama ili VMs, a zatim [Odaberite proširenje uvida aplikacije](app-insights-azure-web-apps.md).

Možete upisati i vlastitu ovisnost kod korištenjem [TrackDependency API -JA](app-insights-api-custom-events-metrics.md#track-dependency)za praćenje.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet paketa.

### <a name="performance-collector"></a>Prikupljanje performansi

[Prikuplja mjerača performansi sustava](app-insights-performance-counters.md) kao što su opterećenje procesora i memorije mreže iz IIS instalacija. Možete odrediti koje mjerača da biste prikupili, uključujući mjerača performansi ste postavili sami.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet paketa.


### <a name="application-insights-diagnostics-telemetry"></a>Aplikacija uvida Dijagnostika Telemetrijskih

Na `DiagnosticsTelemetryModule` izvješća pogrešaka u kodu instrumentation aplikacije uvida sam. Na primjer, ako je kod ne može pristupiti mjerača performansi ili programa `ITelemetryInitializer` throws iznimku. Praćenje telemetrijskih pratiti ovu modul pojavljuje u [Dijagnostike pretraživanja][diagnostic]. Šalje dijagnostičkih podataka dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketa. Ako instalirate samo taj paket, ApplicationInsights.config datoteka ne automatski stvara. 

### <a name="developer-mode"></a>Način rada za razvojne inženjere

`DeveloperModeWithDebuggerAttachedTelemetryModule`Navodi uvida aplikacije `TelemetryChannel` slanje podataka odmah, jedan telemetrijskih stavke u trenutku kada program za ispravljanje pogrešaka priložiti postupka aplikacije. Time se smanjuje vrijeme između trenutka kada aplikacija prati telemetrijskih, a kada se pojavi na portal za aplikacije uvide. On uzrokuje značajan indirektni u procesora i mrežne propusnosti.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Aplikacija uvida Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Paket NuGet

### <a name="web-request-tracking"></a>Web-zahtjev za praćenje

Izvješća [odgovor vrijeme i rezultat kod](app-insights-asp-net.md) zahtjeva za HTTP. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketa

### <a name="exception-tracking"></a>Iznimke praćenja

`ExceptionTrackingTelemetryModule`prati neobrađenu iznimke u web-aplikaciji. Potražite u članku [pogreške i iznimke][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketa


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-prati [iznimke unobserved zadatka](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-prati neobrađenu iznimke za uloge suradnika, windows services i konzole za aplikacije.
* [Aplikacija uvida Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Paket NuGet.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Paket Microsoft.ApplicationInsights nudi [core API](https://msdn.microsoft.com/library/mt420197.aspx) SDK-a. Druge module telemetrijskih tom se mogućnošću poslužite, a možete [koristiti da biste definirali vlastite telemetrijskih](app-insights-api-custom-events-metrics.md).

* Nema unosa u ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketa. Ako instalirate samo ovaj NuGet, generirat će se datoteka ne .config.

## <a name="telemetry-channel"></a>Telemetrijskih kanala

Kanal za telemetriju upravlja međuspremnik i prijenos telemetrijskih sa servisom uvida aplikacije. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`je zadani kanal za servise. Ga međuspremnika podataka u memoriji.
* `Microsoft.ApplicationInsights.PersistenceChannel`je alternative za konzole za aplikacije. Kada aplikacije zatvara prema dolje, a poslat će se kada se ponovno pokreće aplikaciju ga možete spremiti unflushed podatke stalnih prostor za pohranu.


## <a name="telemetry-initializers-aspnet"></a>Initializers telemetriju (ASP.NET)

Telemetrijskih initializers postavite svojstva kontekstu koji su poslani uz svaku stavku telemetrijskih. 

Možete [napisati vlastitu initializers](app-insights-api-filtering-sampling.md#add-properties) da biste postavili svojstva kontekst.

Standardni initializers sve postavljene akcijom paketa weba ili WindowsServer NuGet:


* `AccountIdTelemetryInitializer`postavlja svojstva AccountId.
* `AuthenticatedUserIdTelemetryInitializer`postavlja svojstva AuthenticatedUserId postavljen tako da JavaScript SDK.
* `AzureRoleEnvironmentTelemetryInitializer`ažuriranja u `RoleName` i `RoleInstance` svojstva na `Device` kontekst za sve stavke telemetrijskih s podacima koji se izdvajati iz okruženja Azure runtime.
* `BuildInfoConfigComponentVersionTelemetryInitializer`ažuriranja u `Version` svojstvo na `Component` kontekst za sve stavke telemetrijskih vrijednošću dobivenih iz na `BuildInfo.config` datoteke koje je stvorio MS Build.
* `ClientIpHeaderTelemetryInitializer`ažuriranja `Ip` svojstvo na `Location` kontekst svih stavki za telemetriju na temelju na `X-Forwarded-For` zaglavlje HTTP zahtjev.
* `DeviceTelemetryInitializer`ažurira sljedeća svojstva na `Device` kontekst za sve stavke telemetrijskih.
 - `Type`postavljeno na "PC"
 - `Id`postavljen je na naziv domene računala na kojem se pokreće web-aplikaciju.
 - `OemName`postavljeno na vrijednost dobivenih iz na `Win32_ComputerSystem.Manufacturer` polja pomoću WMI.
 - `Model`postavljeno na vrijednost dobivenih iz na `Win32_ComputerSystem.Model` polja pomoću WMI.
 - `NetworkType`postavljeno na vrijednost dobivenih iz na `NetworkInterface`.
 - `Language`postavljeno na naziv na `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`ažuriranja u `RoleInstance` svojstvo na `Device` kontekst za sve stavke telemetrijskih s nazivom domene na računalu na kojem se pokreće web-aplikaciju.
* `OperationNameTelemetryInitializer`ažuriranja u `Name` svojstvo na `RequestTelemetry` i `Name` svojstvo na `Operation` kontekst svih stavki za telemetriju na temelju HTTP metoda, kao i nazivi kontrolera ASP.NET MVC i akcija poziva za obradu zahtjeva.
* `OperationIdTelemetryInitializer`ili `OperationCorrelationTelemetryInitializer` ažuriranja u `Operation.Id` svojstvo kontekst svih stavki za telemetriju prate tijekom obrade zahtjeva s automatski generirani `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`ažuriranja u `Id` svojstvo na `Session` kontekst za sve stavke telemetrijskih vrijednošću dobivenih iz na `ai_session` kolačića generira prema kodu instrumentation ApplicationInsights JavaScript u pregledniku korisnika. 
* `SyntheticTelemetryInitializer`ili `SyntheticUserAgentTelemetryInitializer` ažuriranja u `User`, `Session` i `Operation` konteksta svojstva svih stavki za telemetriju prate kada rukovanja zahtjeva iz stilova sintetičkih izvora, kao što su raspoloživost testiranje ili robot modul za pretraživanje. Prema zadanim postavkama [Metriku Explorer](app-insights-metrics-explorer.md) prikaz stilova sintetičkih telemetrijskih. 

    Na `<Filters>` postavite svojstva zahtjeve za označavanje.
* `UserAgentTelemetryInitializer`ažuriranja u `UserAgent` svojstvo na `User` kontekst svih stavki za telemetriju na temelju na `User-Agent` zaglavlje HTTP zahtjev.
* `UserTelemetryInitializer`ažuriranja u `Id` i `AcquisitionDate` svojstva `User` kontekst za sve stavke telemetrijskih s vrijednostima dobivenih iz na `ai_user` kolačića generira prema kodu instrumentation aplikacije uvida JavaScript u pregledniku korisnika.
* `WebTestTelemetryInitializer`Postavlja korisnički id, id sesije i stilova sintetičkih izvor svojstva za HTTP zahtjeva koji dolaze iz [testira dostupnost](app-insights-monitor-web-app-availability.md).
Na `<Filters>` postavite svojstva zahtjeve za označavanje.

## <a name="telemetry-processors-aspnet"></a>Procesori telemetriju (ASP.NET)

Procesori telemetrijskih možete filtrirati i izmjena svaku stavku telemetrijskih prije slanja iz SDK portalu.

Možete [napisati vlastitu procesora telemetrijskih](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Prilagodljivo uzorkovanje telemetrijskih procesor (iz 2.0.0-beta3)

Omogućena je prema zadanim postavkama. Ako aplikaciju pošalje mnogo telemetrijskih, ovog procesora uklanja nešto od toga.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametar sadrži cilj koji algoritam pokušava ostvariti. Svaku instancu SDK radi zasebno, tako da je poslužitelj klaster nekoliko računala, će stvarni količinu telemetrijskih sukladno tome se množi.

[Dodatne informacije o uzorkovanje](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Procesor telemetrijskih uzorkovanje stopa (iz 2.0.0-beta1)

Postoji standardni [uzorkovanje telemetrijskih procesor](app-insights-api-filtering-sampling.md#sampling) (iz 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Parametri za kanal (Java)

Parametara utječu na način na koji treba Java SDK pohranu i čišćenje telemetrijskih podataka koje je prikuplja.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Broj telemetrijskih stavki koje se mogu pohraniti u SDK u memorijski prostor za pohranu. Kada je dostigne taj broj, isprazni međuspremnik telemetrijskih – odnosno stavke telemetrijskih bit će poslani poslužitelja uvida aplikacije.

-   Min: 1
-   Max: 1000
-   Zadani: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Određuje koliko često podataka pohranjenih u prostor za pohranu u memoriji moraju se isprazniti (poslane do uvida aplikacije).

-   Min: 1
-   Max: 300
-   Zadani: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Određuje Maksimalna veličina u megabajtima (MB) govornik stalni za pohranu na lokalnom disku. U ovom prostora za pohranu koristi se za održavanju telemetrijskih stavke koje se prenose krajnjoj uvida aplikacije nije uspjelo. Nakon ispunjavanja veličina prostora za pohranu odbacit će se nove stavke telemetrijskih.

-   Min: 1
-   Max: 100
-   Zadani: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Time se određuje aplikacije uvida resursa u kojem se pojavljuje podataka. Obično stvarate zasebnom resursa, s ključem zasebnom za svaku aplikacija.

Ako želite postaviti ključ dinamički – na primjer ako želite da biste poslali rezultate iz aplikacije na drugu resurse - izostaviti ključ iz konfiguracijska datoteka i postavite ga u kodu.

Da biste postavili ključ za sve instance TelemetryClient, uključujući moduli standardni telemetrijskih ključ postavite u TelemetryConfiguration.Active. U način Inicijalizacija, kao što su global.aspx.cs u servis za ASP.NET, učinite sljedeće:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Ako samo želite poslati određeni skup događaja za neki drugi resurs, možete postaviti ključ za određene TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Dodatne informacije o API Sučelja][api].

Da biste u novi ključ, [stvorite novi resurs na portalu aplikacije uvida][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

