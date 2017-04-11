<properties
    pageTitle="Postavljanje upozorenja u aplikaciju uvide pomoću komponente Powershell"
    description="Automatiziranje konfiguracije uvida aplikacije da biste pristupili e-pošte o promjenama metrike."
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
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Postavljanje upozorenja u aplikaciju uvide pomoću komponente PowerShell

Konfiguriranje [upozorenja](app-insights-alerts.md) u [Visual Studio aplikacije uvida](app-insights-overview.md)možete automatizirati.

Osim toga, možete [postaviti webhooks da biste automatizirali odgovor na upozorenje](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Postavljanje jednokratne

Ako još niste koristili PowerShell s pretplatom Azure prije:

Instalirajte modul Azure Powershell na računalu na kojem želite pokrenuti skripte.

 * Instalacija [Microsoft Web platformu Installer (v5 ili noviji)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Koristite da biste instalirali Microsoft Azure Powershell


## <a name="connect-to-azure"></a>Povezivanje s Azure

Pokrenite Azure PowerShell i [povezati pretplate](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Primati upozorenja

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Dodavanje upozorenja


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Primjer 1

E-pošte me ako je odgovor na poslužitelj zahtjeva za HTTP računa prosječna vrijednost od pet minuta, manja od 1 sekunde. Moje aplikacije uvida resursa zove IceCreamWebApp pa je u grupu resursa Fabrikam. Ja sam vlasnik Azure pretplate.

GUID je ID pretplate (ne ključ instrumentation aplikacije).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Primjer 2

Imam aplikacije u kojima se [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) koristiti da biste prijavili metrike pod nazivom "salesPerHour." Pošaljite poruku e-pošte da biste Moji suradnici ako "salesPerHour" ispod 100, odrediti prosjek dulje od 24 sata.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Isto pravilo može se koristiti za mjerenje prijavili pomoću [mjera parametar](app-insights-api-custom-events-metrics.md#properties) poziv na drugi praćenje kao što su TrackEvent ili trackPageView.

## <a name="metric-names"></a>Nazivi mjerenja

Naziv mjerenja | Zaslonski naziv | Opis
---|---|---
`basicExceptionBrowser.count`|Iznimke u pregledniku|Broj je neuhvaćenu iznimke izbačena u pregledniku.
`basicExceptionServer.count`|Iznimke poslužitelja|Broj neobrađenu iznimke izbačena aplikacija
`clientPerformance.clientProcess.value`|Vrijeme obrade klijenta|Vrijeme između primanje posljednje bajtni dokumenta dok se ne učitava se DOM. Zahtjevi za asinkrone možda se obrađuju.
`clientPerformance.networkConnection.value`|Vrijeme za povezivanje mreže učitavanja stranice| Vrijeme u pregledniku potrebno za povezivanje s mrežom. Može biti 0 Ako predmemorirani.
`clientPerformance.receiveRequest.value`|Vrijeme slanja odgovora| Vrijeme između preglednika slanje zahtjeva za pokretanje da biste primali odgovor.
`clientPerformance.sendRequest.value`|Slanje zahtjeva za vrijeme| Vrijeme u pregledniku da biste poslali zahtjev.
`clientPerformance.total.value`|Vrijeme učitavanja stranice preglednika|Vrijeme iz zahtjeva za korisnika dok se ne učitavaju se DOM, listovi stilova, skripte i slike.
`performanceCounter.available_bytes.value`|Dostupnom memorijom|Fizička memorija odmah dostupna proces ili za sustav.
`performanceCounter.io_data_bytes_per_sec.value`|Brzina IO|Ukupno bajtova sekundi čitati i zapisivanje datoteke, mreže i uređaje.
`performanceCounter.number_of_exceps_thrown_per_sec`|iznimke stopa|Iznimke izbačena sekundi.
`performanceCounter.percentage_processor_time.value`|Postupak CPU-a|Postotak Proteklog vremena usporednih sve postupak koristi procesor radi izvršavanja upute za proces aplikacije.
`performanceCounter.percentage_processor_total.value`|Procesor vremena|Postotak vrijeme koje procesor provodi niti ne Neaktivno.
`performanceCounter.process_private_bytes.value`|Postupak privatnih bajtova|Memorije isključivo dodijeljena procesa nadziranim aplikacije.
`performanceCounter.request_execution_time.value`|Vrijeme izvođenja zahtjeva za platforme ASP.NET|Vrijeme izvođenja najnovije zahtjev.
`performanceCounter.requests_in_application_queue.value`|Zahtjevi za ASP.NET u redu čekanja za izvršavanje|Duljina red čekanja zahtjeva za aplikaciju.
`performanceCounter.requests_per_sec`|Učestalost zahtjeva platforme ASP.NET|Brzina svih zahtjeva za aplikaciju u sekundi iz ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Ovisnost neuspjeha|Broj neuspjelih pozive aplikacija server da biste vanjskim izvorima.
`request.duration`|Vrijeme odaziva poslužitelja|Vrijeme između primanje HTTP zahtjev i završavanje slanja odgovora.
`request.rate`|Učestalost zahtjeva|Brzina svih zahtjeva za aplikaciju u sekundi.
`requestFailed.count`|Nije uspjelo zahtjeva|Zahtjevi za brojanje HTTP koja je rezultirala odgovor kod > = 400
`view.count`|Prikaza stranice|Broj zahtjeva za korisnika klijenta za web-stranicu. Stilova sintetičkih promet se filtrira.
{prilagođene metričkim naziva}|{Metričkim name}|Metričkim vrijednosti prijavio [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) ili u [mjere parametar praćenje poziva](app-insights-api-custom-events-metrics.md#properties).

Metriku pošalje različite telemetrijskih modula:

Metričkim grupe | Modul za prikupljanje
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>Prikaz | [JavaScript u pregledniku](app-insights-javascript.md)
performanceCounter | [Performanse](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Ovisnost](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
zahtjev<br/>requestFailed|[Zahtjev za poslužitelj](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Možete [automatizirati odgovor na upozorenje](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure podići web-adresu po izboru kada se potencira upozorenja.

## <a name="see-also"></a>Vidi također


* [Skripta za konfiguriranje aplikacije uvida](app-insights-powershell-script-create-resource.md)
* [Stvaranje uvida računala i web-test resurse iz predložaka](app-insights-powershell.md)
* [Automatiziranje coupling Microsoft Azure Dijagnostika do uvida aplikacije](app-insights-powershell-azure-diagnostics.md)
* [Automatiziranje odgovor upozorenja](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
