<properties
    pageTitle="Praćenje performansi za mobilne web-aplikacije s Analytics za razvojne inženjere | Microsoft Azure"
    description="Performanse računala i nadzor korištenja za razvojne inženjere za mobilnu aplikaciju. , radne površine, web-servisa i pozadinska aplikacija s HockeyApp i uvida aplikacije."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobilni analize s HockeyApp i aplikacije uvida

Praćenje performansi i korištenje beta-test i distribuiranih prijenosnih i stolnih aplikacija s [HockeyApp](https://hockeyapp.net/). Praćenje podrške web-servisa i pozadinskog aplikacije s [Uvida aplikacije za Visual Studio](app-insights-overview.md). Analiza podataka iz postupak klijentske i poslužiteljske aplikacija u analize i prikaz grafikona uz međusobno Azure nadzornoj ploči.

Microsoft Developer Analytics nudi dva rješenja za praćenje performansi aplikacije:

* Aplikacije **HockeyApp za mobilne uređaje** i radna površina.
 * Distribucija verzija test za odabrane korisnike.
 * Neočekivano zatvoriti analize.
 * Prilagođeni telemetrijskih za analize korištenja.
* **Uvid aplikaciju za web-mjesta** i servisa i pozadinska aplikacija.
 * Performanse i korištenje metriku i upozorenja.
 * Iznimke izvješćivanje i praćenje dijagnostičkih.
 * Dijagnostičke pretraživanje pomoću filtriranja i povezane telemetrijskih veze.

Imaju obiju situacija:

 * Napredna **[jezik analitički upita](app-insights-analytics.md)** za dijagnostiku i analiza.
 * **[Izvoz podataka](app-insights-export-telemetry.md)** u vlastiti prostor za pohranu.
 * **Integrirano nadzorne ploče** prikaza analitičkih grafikona i tablica.

## <a name="monitor-your-app-components"></a>Praćenje aplikacija komponente

Slijedite upute u ove stranice da biste instalirali SDK u kodu i pokrenuti aplikaciju za praćenje.

### <a name="web-apps---application-insights"></a>Aplikacija za web - aplikacije uvida

* [ASP.NET web-aplikacije](app-insights-asp-net.md) 
* [Java web-aplikacije](app-insights-java-get-started.md)
* [Node.js web-aplikacije](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Servisi u Oblaku za Azure](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobilne aplikacije - HockeyApp

* [aplikaciju za iOS](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X aplikacije](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Aplikacija za android](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Aplikacija za univerzalni Windows](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Aplikacija za Windows Phone 8 i 8.1](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Prezentacija Windows Foundation aplikacije](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Windows aplikacija za stolna računala preporučujemo HockeyApp. No možete [poslati telemetrijskih iz aplikacija u sustavu Windows aplikacije uvid u](app-insights-windows-desktop.md). Možda ćete to Eksperimentirajte s uvida aplikacije.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Izvoz za telemetriju HockeyApp i analize

Možete istražiti HockeyApp Prilagođeno i prijavu telemetrijskih pomoću analize i neprekinuti izvoz značajke aplikacije uvida postavljanjem [most](app-insights-hockeyapp-bridge-app.md).




