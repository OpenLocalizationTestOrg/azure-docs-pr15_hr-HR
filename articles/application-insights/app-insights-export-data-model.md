<properties 
    pageTitle="Aplikacija uvida podatkovnog modela" 
    description="U članku se opisuje svojstva izvezene iz neprekinuti Izvoz u JSON, a koriste kao filtri." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Aplikacija uvida izvoz podatkovnog modela

U ovoj su tablici navedeni svojstva telemetrijskih poslane iz [Aplikacije uvida](app-insights-overview.md) SDK-ovi portalu. Vidjet ćete tih svojstava za izlaz podataka iz [Neprekinuti izvoz](app-insights-export-telemetry.md).
Pojavljuju i u filtara svojstava u [Metriku Explorer](app-insights-metrics-explorer.md) i [Dijagnostike pretraživanja](app-insights-diagnostic-search.md).

Upućuje na Imajte na umu:

* `[0]`u ovim su tablicama označava točke na putu gdje ste za umetanje indeksa; ali još uvijek nije 0.
* Vrijeme trajanja su u desetine microsecond, pa 10000000 == 1 sekunde.
* Datumi i vremena su UTC-a i su dani u obliku ISO`yyyy-MM-DDThh:mm:ss.sssZ`

Postoji nekoliko [primjera](app-insights-export-telemetry.md#code-samples) koji pokazuju kako ih koristiti.



## <a name="example"></a>Primjer

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Kontekst

Sve vrste telemetrijskih su praćeni odjeljak kontekst. Neke od tih polja šalju se sa svake točke podataka.



|Put|Vrsta|Bilješke|
|---|---|---|
| context.Custom.dimensions [0]  | objekt]  | Ključ vrijednost niza parove postavite parametrom prilagođenih svojstava. Duljina ključa Maks 100, vrijednosti Maks duljina 1024. Više od 100 jedinstvene vrijednosti, svojstvo možete pretraživati ali nije moguće koristiti za segmente. Max 200 tipke po ikey.  |
| context.Custom.metrics [0]  | objekt]  | Postavljanje parametrom prilagođene mjere i tako da TrackMetrics parovima ključnih vrijednosti. Maksimalni duljinu ključa 100, vrijednosti možda numerički. |
| context.data.eventTime | niz | UTC-A |
| context.data.isSynthetic | Booleove vrijednosti | Zahtjev za naizgled dolazi iz robot ili web-testa. |
| context.data.samplingRate | broj | Postotak telemetrijskih generira SDK koja je poslana na portal. Raspon 0.0 100.0.|
| context.Device | objekt | Klijentskog uređaja |
| context.Device.Browser | niz | IE, Chrome... |
| context.device.browserVersion | niz | Chrome 48.0... |
| context.device.deviceModel | niz | |
| context.device.deviceName | niz | |
| context.Device.ID | niz | |
| context.Device.locale | niz | en GB, zapis potencijalnog klijenta koji DE... |
| context.Device.Network | niz | |
| context.device.oemName | niz | |
| context.device.osVersion | niz | Glavno računalo s operacijskim Sustavom |
| context.device.roleInstance | niz | ID glavnog računala poslužitelja |
| context.device.roleName | niz | |
| context.Device.Type | niz | PC-JA pregledniku... |
| context.location | objekt | Izvedeno iz prefiksa clientip. |
| context.location.City | niz | Izvedene iz clientip, ako je poznato  |
| context.location.clientip | niz | Zadnji oktogon anonymized je 0. |
| context.location.continent | niz | |
| context.location.Country | niz | |
| context.location.province | niz | Savezna država ili pokrajina |
| context.Operation.ID | niz | Stavke koje imaju isti postupak id prikazana su kao povezane stavke na portalu. Obično id zahtjev. |
| context.Operation.name | niz | URL ili zahtjev za naziv |
| context.operation.parentId | niz | Omogućuje ugniježđene srodnim stavkama. |
| context.Session.ID | niz | ID grupe operacija iz istog izvora. U razdoblju od 30 minuta bez operaciju signale kraj sesije. |
| context.session.isFirst | Booleove vrijednosti | |
| context.user.accountAcquisitionDate | niz | |
| context.user.anonAcquisitionDate | niz | |
| context.user.anonId | niz | |
| context.user.authAcquisitionDate | niz | [Korisnika čija je autentičnost provjerena](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | Booleove vrijednosti | |
| internal.data.documentVersion | niz | |
| Internal.Data.ID | niz | |



## <a name="events"></a>Događaji

Prilagođene događaje koje generira [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event). 


|Put|Vrsta|Bilješke|
|---|---|---|
| broj događaja [0] | cijeli broj | 100 /[(uzorkovanja)](app-insights-sampling.md) . Na primjer 4 =&gt; 25%. |
| Naziv događaja [0] | niz | Naziv događaja.  Maksimalna duljina 250. |
| url za događaj [0] | niz | |
| urlData.base događaj [0] | niz | |
| urlData.host događaj [0] | niz | |

## <a name="exceptions"></a>Iznimke

Izvješća [iznimke](app-insights-asp-net-exceptions.md) na poslužitelju i u pregledniku. 


|Put|Vrsta|Bilješke|
|---|---|---|
| Sastavljanje basicException [0] | niz | |
| count basicException [0] | cijeli broj | 100 /[(uzorkovanja)](app-insights-sampling.md) . Na primjer 4 =&gt; 25%. |
| exceptionGroup basicException [0] | niz | |
| exceptionType basicException [0] | niz | |niz | |
| failedUserCodeMethod basicException [0] | niz | |
| failedUserCodeAssembly basicException [0] | niz | |
| handledAt basicException [0] | niz | |
| hasFullStack basicException [0] | Booleove vrijednosti | |
| id basicException [0] | niz | |
| način basicException [0] | niz | |
| poruka basicException [0] | niz | Poruka iznimke. Maksimalna duljina 10k.|
| outerExceptionMessage basicException [0] | niz | |
| outerExceptionThrownAtAssembly basicException [0] | niz | |
| outerExceptionThrownAtMethod basicException [0] | niz | |
| outerExceptionType basicException [0] | niz | |
| outerId basicException [0] | niz | |
| Sastavljanje parsedStack [0] basicException [0] | niz | |
| Naziv datoteke parsedStack [0] basicException [0] | niz | |
| Razina parsedStack [0] basicException [0] | cijeli broj | |
| redak parsedStack [0] basicException [0] | cijeli broj | |
| način parsedStack [0] basicException [0] | niz | |
| Stoga basicException [0] | niz | Maksimalna duljina 10k|
| typeName basicException [0] | niz | |



## <a name="trace-messages"></a>Praćenje poruka

Šalje [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)i [zapisivanje prilagodnika](app-insights-asp-net-trace-logs.md).


|Put|Vrsta|Bilješke|
|---|---|---|
| loggerName poruku [0] | niz ||
| Parametri poruku [0] | niz ||
| poruka [0] neobrađenog | niz | Poruka pogreške, Maks duljina 10k. |
| severityLevel poruku [0] | niz | |



## <a name="remote-dependency"></a>Udaljena ovisnost

Poslao TrackDependency. Da biste izvješće performanse i korištenje [pozive ovisnosti](app-insights-asp-net-dependencies.md) na poslužitelju i koristiti AJAX poziva u pregledniku.

|Put|Vrsta|Bilješke|
|---|---|---|
| asinkroni remoteDependency [0] | Booleove vrijednosti | |
| baseName remoteDependency [0] | niz |  |
| commandName remoteDependency [0] | niz | Na primjer "Polazno/indeks" |
| count remoteDependency [0] | cijeli broj | 100 /[(uzorkovanja)](app-insights-sampling.md) . Na primjer 4 =&gt; 25%. |
| dependencyTypeName remoteDependency [0] | niz | HTTP SQL... |
| durationMetric.value remoteDependency [0] | broj | Vrijeme od poziv obavljanjem odgovor po ovisnost |
| id remoteDependency [0] | niz | |
| Naziv remoteDependency [0] | niz | URL-a. Maksimalna duljina 250.|
| resultCode remoteDependency [0] | niz | iz ovisnost HTTP |
| uspjeh remoteDependency [0] | Booleove vrijednosti | |
| Vrsta remoteDependency [0] | niz | HTTP Sql... |
| url remoteDependency [0] | niz |  Maksimalna duljina 2000 |
| urlData.base remoteDependency [0] | niz | Maksimalna duljina 2000 |
| urlData.hashTag remoteDependency [0] | niz | |
| urlData.host remoteDependency [0] | niz | Maksimalna duljina 200 |


## <a name="requests"></a>Zahtjevi za

Poslao [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). Standardni moduli koristi se za vrijeme odaziva poslužitelja za izvješća mjeri na poslužitelju. 


|Put|Vrsta|Bilješke|
|---|---|---|
| count zahtjev [0] | cijeli broj | 100 /[(uzorkovanja)](app-insights-sampling.md) . Na primjer: 4 =&gt; 25%. |
| zahtjev za [0] durationMetric.value | broj | Vrijeme iz zahtjeva za Stiže odgovor. 1e7 == 1s |
| id zahtjeva [0] | niz | Id operacije |
| Naziv zahtjev [0] | niz | GET-objavu + osnovni url.  Maksimalna duljina 250 |
| responseCode zahtjev [0] | cijeli broj | HTTP odgovor koji se šalju klijenta |
| zahtjev za uspjeh [0] | Booleove vrijednosti | Zadani == (responseCode &lt; 400) |
| zahtjev za [0] URL-a | niz | Ne uključujući glavno računalo |
| zahtjev za [0] urlData.base | niz | |
| zahtjev za [0] urlData.hashTag | niz |  |
| zahtjev za [0] urlData.host | niz | |


## <a name="page-view-performance"></a>Prikaz stranice performansi

Poslao web-pregledniku. Mjeri vrijeme za obradu stranicu korisnika pokretanje zahtjev da biste prikazali dovršeno (bez asinkrone pozivi AJAX-a).

Kontekst vrijednosti prikaz klijenta za OS i verzija preglednika. 


|Put|Vrsta|Bilješke|
|---|---|---|
| clientProcess.value clientPerformance [0] | cijeli broj | Vrijeme od završetka primati HTML na stranici za prikazivanje. |
| Naziv clientPerformance [0] | niz | |
| networkConnection.value clientPerformance [0] | cijeli broj | Vrijeme da biste uspostavili mrežnu vezu. |
| receiveRequest.value clientPerformance [0] | cijeli broj | Vrijeme od kraj slanja zahtjeva u odgovoru primanje HTML. |
| sendRequest.value clientPerformance [0] | cijeli broj | Vremena snimanja da biste poslali zahtjev HTTP-a. |
| total.value clientPerformance [0] | cijeli broj | Vrijeme pokretanje da biste poslali zahtjev za prikaz stranice. |
| url clientPerformance [0] | niz | URL ovog zahtjeva |
| urlData.base clientPerformance [0] | niz | |
| urlData.hashTag clientPerformance [0] | niz | |
| urlData.host clientPerformance [0] | niz | |
| urlData.protocol clientPerformance [0] | niz | |

## <a name="page-views"></a>Prikaza stranice

Poslao trackPageView() ili [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Put|Vrsta|Bilješke|
|---|---|---|
| Prikaz [0] count | cijeli broj | 100 /[(uzorkovanja)](app-insights-sampling.md) . Na primjer 4 =&gt; 25%. |
| Prikaz [0] durationMetric.value | cijeli broj | Vrijednost po izboru postaviti u trackPageView() ili startTrackPage() - stopTrackPage(). Nije isti kao clientPerformance vrijednosti. |
| Naziv prikaza [0] | niz | Naslov stranice.  Maksimalna duljina 250 |
| url prikaza [0] | niz | |
| Prikaz [0] urlData.base | niz | |
| Prikaz [0] urlData.hashTag | niz | |
| Prikaz [0] urlData.host | niz | |



## <a name="availability"></a>Dostupnost

Izvješća [testira web dostupnost](app-insights-monitor-web-app-availability.md).

|Put|Vrsta|Bilješke|
|---|---|---|
| Dostupnost [0] availabilityMetric.name | niz | Dostupnost |
| Dostupnost [0] availabilityMetric.value | broj |1.0 ili 0.0 |
| count dostupnost [0] | cijeli broj | 100 /[(uzorkovanja)](app-insights-sampling.md) . Na primjer 4 =&gt; 25%. |
| Dostupnost [0] dataSizeMetric.name | niz | |
| Dostupnost [0] dataSizeMetric.value | cijeli broj | |
| Dostupnost [0] durationMetric.name | niz | |
| Dostupnost [0] durationMetric.value | broj | Trajanje testa. 1e7 == 1s |
| Dostupnost [0] poruke | niz | Pogreška dijagnostičkih |
| rezultat dostupnost [0] | niz | Pristupni/Neuspjelo |
| Dostupnost [0] runLocation | niz | Izvor http Opt zemlj. |
| Dostupnost [0] testName | niz | |
| Dostupnost [0] testRunId | niz | |
| Dostupnost [0] testTimestamp | niz | |




## <a name="metrics"></a>Mjerenja

Generira TrackMetric().

Vrijednost metričke nalazi se u context.custom.metrics[0]

Ako, na primjer:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>O metričkim vrijednostima

Metričkim vrijednostima u metričkim izvješća i negdje drugdje, prijavljene s strukture standardne objekta. Ako, na primjer:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Trenutno - kroz to može promijeniti u budućnosti – sve vrijednosti prijavljenih iz standardni moduli SDK `count==1` i samo u `name` i `value` korisne su polja. Samo slova gdje ih želite biti različiti bio pišete TrackMetric pozive u koji ste postavili za drugi parametri. 

Svrha ostala polja je omogućiti metriku za se pridružuje u SDK da biste smanjili promet na portal. Ako, na primjer, nije moguće izračunati prosjek nekoliko uzastopni čitanja prije slanja svaki metričkim izvješća. Zatim želite izračunati min, max, standardne devijacije i vrijednosti zbroja (zbroj ili prosjek) i postavite count broj čitanja predstavlja izvješća. 

U tablicama iznad, ne možemo ste zaboravili count rijetko koristiti polja, min, max, stdDev i sampledValue.

Ako vam je potrebna da biste smanjili količinu telemetrijskih, umjesto unaprijed Zbrajanje metriku koristite [uzorkovanje](app-insights-sampling.md) .


### <a name="durations"></a>Trajanje

Osim gdje je drugačije, trajanje prikazane su desetine microsecond, tako da se 10000000.0 znači 1 sekunde.



## <a name="see-also"></a>Vidi također

* [Aplikacija uvida](app-insights-overview.md) 
* [Neprekinuti izvoza](app-insights-export-telemetry.md)
* [Primjere koda](app-insights-export-telemetry.md#code-samples)


