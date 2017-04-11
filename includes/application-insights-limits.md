Postoje neka ograničenja broja metriku i događaje po aplikacije (to jest, po instrumentation ključa). 

Ograničenja ovise o [cijene sloju](https://azure.microsoft.com/pricing/details/application-insights/) koji odaberete.

**Resurs** | **Zadano ograničenje** | **Maksimalno ograničenje**
-------- | ------------- | -------------
Točke podataka sesiju<sup>1, 2</sup> mjesečno | neograničeno | 
Ukupna točaka po mjesecu za zahtjev za događaj, ovisnosti, praćenje, iznimke i prikaz stranice | milijuna 5 | 50 milijuna<sup>3</sup>
[Praćenje i prijaviti se](../articles/application-insights/app-insights-search-diagnostic-logs.md) stopa podataka | 200 dp/s | 500 dp/s
[Iznimke](../articles/application-insights/app-insights-asp-net-exceptions.md) podataka stopa | 50 dp/s | 50 dp/s
Ukupno stopu za zahtjev za događaj, ovisnosti te telemetrijskih prikaz stranice | 200 dp/s | 500 dp/s
Sirovim podacima zadržavanja za [pretraživanje](../articles/application-insights/app-insights-diagnostic-search.md) i [analize](../articles/application-insights/app-insights-analytics.md) | sedam dana
Skupne podatke zadržavanja za [explorer mjerenja](../articles/application-insights/app-insights-metrics-explorer.md) | 90 dana
Broj naziv [Svojstva](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Dužina naziva svojstva | 150 | 
Duljina vrijednosti svojstva | 8192 | 
Praćenje i iznimke duljine poruke | 10000 |
Count naziv [mjerenja](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Dužina naziva mjerenja |  150 | 
[Dostupnost testira](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> točku podataka je pojedinačne metričkim vrijednost ili događaj s priloženom svojstva i mjere.

<sup>2</sup> A sesiju točke podataka prijavi na početak ili kraj sesije i prijavljuje identitet korisnika.

<sup>3</sup> možete kupiti dodatni kapaciteta izvan milijuna 50.
 
[O cijene i kvota u uvide aplikacije](../articles/application-insights/app-insights-pricing.md)
