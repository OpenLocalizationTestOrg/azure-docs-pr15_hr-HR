<properties 
    pageTitle="Analytics – alat za napredno pretraživanje aplikacije uvida | Microsoft Azure" 
    description="Pregled analize, alat za napredno pretraživanje dijagnostičkih uvida aplikacije. " 
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
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Analitički u aplikaciji uvida


[Analitički](app-insights-analytics.md) je napredna pretraživanja značajka [Uvida aplikacije](app-insights-overview.md). Ove stranice opišite upita lanquage analize. 

* **[Pogledajte uvodni videozapis](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Probnu vožnju analize na našem Simulirani podataka](https://analytics.applicationinsights.io/demo)** ako aplikacija nije slanje podataka do uvida aplikacije još.

## <a name="queries-in-analytics"></a>Upiti u Analytics
 
Uobičajeni upit je *izvorišnu* tablicu nakon čega slijedi niz *operatori* odvojene `|`. 

Ako, na primjer, pretpostavimo Saznajte koje doba dana građana Hyderabad isprobajte naše web-aplikacije. A dok Ispričavamo se tamo, pogledajmo koje kodovi rezultat vraćaju njihove HTTP zahtjeva. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Ne možemo Brojanje IP adrese distinct klijenta, njihovo grupiranje po satu dana zadnjih 7 dana. 

Pogledajmo prikazati rezultate s prezentacijom trakasti grafikon, odabiru između posložiti rezultate iz različitih odgovor kodovi:

![Odaberite trakasti grafikon, x i y osi, a zatim segmente](./media/app-insights-analytics/020.png)

Izgleda kao da je naša aplikacija najpopularnijih lunchtime i Uloži vrijeme u Hyderabad. (A ne možemo treba istražiti te 500 kodovi.)


Postoje i naprednih statističkih postupaka:

![](./media/app-insights-analytics/025.png)


Jezik sadrži mnogo privlačne značajki:

* [Filtar](app-insights-analytics-reference.md#where-operator) vaše telemetrijskih neobrađenog aplikacija tako da sva polja, uključujući prilagođena svojstva i metrike.
* [Uključivanje](app-insights-analytics-reference.md#join-operator) više tablica – correlate zahtjeva s prikaza stranice, ovisnosti pozive, iznimke i zapisnika prati.
* Napredna statističke [zbrajanja](app-insights-analytics-reference.md#aggregations).
* Baš kao Napredna kao SQL, ali jednostavniji za složene upite: umjesto gniježđenja izjave pipe podatke iz jedne osnovne operacije u drugi.
* Trenutna i napredne vizualizacije.







## <a name="connect-to-your-application-insights-data"></a>Povezivanje s vašim podacima uvida aplikacije


Otvaranje analize iz aplikacije programa [plohu pregled](app-insights-dashboards.md) u aplikaciji uvida: 

![Otvaranje portal.azure.com, otvorite svoje resurs uvida aplikaciju i kliknite Analitika.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Ograničenja

Trenutno, rezultati upita ograničeni su na samo putem tjedan proteklih podataka.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Daljnji koraci


* Preporučujemo da započnete s [proizvodom jezik](app-insights-analytics-tour.md).