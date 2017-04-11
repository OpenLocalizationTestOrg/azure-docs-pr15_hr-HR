<properties 
    pageTitle="Nadzor korištenja i performanse za Windows aplikacija za stolna računala" 
    description="Analiza korištenja i performanse sustava Windows za stolna računala s HockeyApp i uvida aplikacije." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Nadzor korištenja i performanse u aplikacijama za radnu površinu sustava Windows

*Aplikacija uvida je u pretpregledu.*

[Uvid aplikacije za Visual Studio](app-insights-overview.md) i [HockeyApp](https://hockeyapp.net) omogućuju praćenje distribuiranih aplikacija za korištenje i performanse.

> [AZURE.IMPORTANT] Preporučujemo da se [HockeyApp](https://hockeyapp.net) izlaganja i praćenje aplikacija za stolna računala i uređaja. S HockeyApp, koje možete upravljati raspodjele uživo i testiranje korisničke povratne informacije, kao i praćenje izvješća o korištenju i pad sustava. Možete [izvesti, a zatim upit na telemetrijskih s analize](app-insights-hockeyapp-bridge-app.md).

> Iako telemetrijskih možete ju je poslala aplikacije uvid u aplikaciji za stolna računala, to je chiefly korisne su za ispravljanje pogrešaka i eksperimentalne svrhe.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Da biste poslali telemetrijskih do uvida aplikacije iz aplikacije za Windows

1. [Portal za Azure](https://portal.azure.com) [stvoriti do uvida aplikacije resurs](app-insights-create-new-resource.md). Za vrstu aplikacije, odaberite ASP.NET aplikacija.
2. Preuzimanje kopije tipku Instrumentation. Pronađite ključ u Essentials padajućem izborniku novi resursa koji ste upravo stvorili. 
3. U Visual Studio uređivanje NuGet paketa aplikacije projekta i dodajte Microsoft.ApplicationInsights.WindowsServer. (Ili odaberite Microsoft.ApplicationInsights ako želite samo na Gola API bez zbirke moduli standardni telemetrijskih.)
4. Postavljanje instrumentation ključa ili u kodu:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*ključ*`";` 

    ili ApplicationInsights.config (Ako ste instalirali jedan od paketa standardni telemetrijskih):
 
    `<InstrumentationKey>`*ključ*`</InstrumentationKey>` 

    Ako koristite ApplicationInsights.config, provjerite je li svojstvima u pregledniku rješenja postavljene na **akciji sastavljanje = sadržaj, a zatim Kopiraj izlaznog direktorija = Kopiraj**.
5. [Korištenje na API-JA](app-insights-api-custom-events-metrics.md) da biste poslali telemetrijskih.
6. Pokrenite aplikaciju i potražite u članku telemetriju u resursa koji ste stvorili na portalu za Azure.

## <a name="telemetry"></a>Primjer kod

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Daljnji koraci

* [Stvaranje nadzorne ploče](app-insights-dashboards.md)
* [Dijagnostičke pretraživanja](app-insights-diagnostic-search.md)
* [Istražite mjerenja](app-insights-metrics-explorer.md)
* [Pisanje analize upita](app-insights-analytics.md)
 
