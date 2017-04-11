<properties 
    pageTitle="Praćenje stanja i korištenje s računala uvida pokrenite aplikaciju" 
    description="Početak rada s uvida aplikacije. Analiza korištenja, dostupnost i performanse svog lokalnog ili aplikacije Microsoft Azure." 
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
    ms.date="11/25/2015" 
    ms.author="awills"/>
 
# <a name="monitor-performance-in-web-applications"></a>Praćenje performansi u web-aplikacijama

*Aplikacija uvida je u pretpregledu.*


Provjerite je li aplikacija i izvršava i brzo Saznajte više o sve pogreške. [Aplikacija uvida] [ start] će vas obavijestiti o sve probleme s performansama i iznimke i olakšavaju pronalaženje i dijagnosticiranje korijenskog uzroka.

Aplikacija uvida možete nadzirati Java i ASP.NET web-aplikacije i servise, a zatim WCF services. Moguće ih je nalazi na lokaciji, na virtualnim strojevima ili kao web-mjesta Microsoft Azure. 

Na klijentskoj strani uvida aplikacija može potrajati telemetrijskih iz web-stranice i raznih uređaja uključujući iOS i Android i Windows trgovine aplikacija.


## <a name="setup"></a>Postavljanje praćenje performansi

Ako još niste dodali aplikacije uvid u projekt (to jest, ako nema ApplicationInsights.config), odaberite neku od sljedećih načina za početak rada:

* [ASP.NET web-aplikacije](app-insights-asp-net.md)
 * [Dodavanje iznimke nadzora](app-insights-asp-net-exceptions.md)
 * [Dodaj nadzor ovisnost](app-insights-monitor-performance-live-website-now.md)
* [J2EE web-aplikacije](app-insights-java-get-started.md)
 * [Dodaj nadzor ovisnost](app-insights-java-agent.md)


## <a name="view"></a>Istraživanje metriku performansi

[Portal za Azure](https://portal.azure.com)pronađite aplikaciju uvida resursa koji ste postavili za svoju aplikaciju. Pregled plohu prikazuje podataka o performansama osnovni:



Kliknite bilo koji od njih da biste vidjeli više detalja, a da biste vidjeli rezultate za dulje razdoblje. Ako, na primjer, kliknite pločicu zahtjeve, a zatim odaberite vremenski raspon:


![Kliknite kroz više podataka, a zatim odaberite vremenski raspon](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Kliknite grafikon da biste odabrali koje metriku ga prikazuje ili dodali novi grafikon i odaberite njezin mjernih podataka:

![Kliknite grafikon da biste odabrali mjerenja](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [AZURE.NOTE] **Poništite sve metriku** da biste vidjeli cijeli odabir koja je dostupna. Metriku podijeljene grupe; Kada je odabrana bilo koji član grupe, prikazuju se samo ostalih članova te grupe.


## <a name="metrics"></a>Što znači li to sve? Pločice uspješnosti i izvješća

Postoji raznih performanse metriku možete dobiti. Započnimo s onima koje se prikazuju po zadanom na plohu aplikacije.


### <a name="requests"></a>Zahtjevi za

Broj HTTP zahtjeva u određenom razdoblju. Usporediti s rezultatima na druga izvješća da biste vidjeli kako se aplikacije ponaša kao opterećenje mijenja se.

HTTP zahtjeva obuhvaćaju sve zahtjeve za DOHVAĆANJE ili objavu za stranice, podataka i slike.

Kliknite pločicu da biste dobili Prebrojava određenih URL-ova.

### <a name="average-response-time"></a>Prosječno vrijeme odaziva

Mjeri vrijeme između zahtjeva web unesete aplikacije i odgovor se vraćaju.

Upućivanje Prikaži na premještanje prosjek. Ako postoji mnogo zahtjeve, možda postoje neka koje proizlaze iz prosjeka bez očite Vršna ili dip u grafikonu.

Potražite neobično peaks. Općenito govoreći, očekivati reakcija na pokazivači s vodiča u zahtjevima. Ako je na vodiča disproportionate aplikacije možda odlazak ograničenje resursa kao što su procesora ili kapacitet usluge koristi.

Kliknite pločicu da biste dobili vremena za određenih URL-ova.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)


### <a name="slowest-requests"></a>Najmanju zahtjeva

![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Prikazuje zahtjeva za koje možda će biti potrebno ugađanju performansi.


### <a name="failed-requests"></a>Nije uspjelo zahtjeva

![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Broj zahtjeva za izbacio je neuhvaćenu iznimke.

Kliknite pločicu da biste vidjeli detalje o određene pogreške pa odaberite pojedinačne zahtjev da biste vidjeli njegove pojedinosti. 

Samo predstavniku uzorak neuspjeha zadržava se za pojedine provjere.

### <a name="other-metrics"></a>Ostale mjerenja

Da biste vidjeli što postavite druge mjernih podataka možete prikazati, kliknite grafikon i poništite odabir svih mjernih podataka da biste vidjeli dostupne cijelog. Kliknite (i) da biste vidjeli svaki metriku definicija.

![Poništite odabir svih mjernih podataka da biste vidjeli cijeli skup](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)


Odabirom bilo koje metriku će onemogućiti drugima koji se ne prikazuju na istom grafikonu.

## <a name="set-alerts"></a>Postavljanje upozorenja

Da biste primili obavijest putem e-pošte neobično vrijednosti sve mjerenja, dodajte upozorenja. Odaberite nešto od sljedećeg da biste poslali e-poštu administratorima računa ili adrese e-pošte određenog.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Postavite resurs prije ostalih svojstava. Ne odaberete resursi webtest upute za postavljanje upozorenja na performanse i korištenje mjernih podataka.

Pripazite da Imajte na umu jedinice u kojima se zatraži da unesete vrijednosti praga.

*Ne vidim gumb Dodaj upozorenje.* -Je to grupa računa kojima imate pristup samo za čitanje? Obratite se administratoru za račun.

## <a name="diagnosis"></a>Dijagnosticiranje problema

Evo nekoliko savjeta za traženje i dijagnosticiranje probleme s performansama:

* Postavljanje [web testira] [ availability] biti obaviješteni ako web-mjestu funkcionira ili odgovori neispravno ili sporo. 
* Usporedite broj zahtjev s drugim metriku jesu li sporo odgovora ili pogreške vezane uz učitati.
* [Umetanje i pretraživanje izjave o praćenju] [ diagnostic] u kodu da biste lakše pinpoint problema.

## <a name="next"></a>Daljnji koraci

[Web-testira] [ availability] -imaju web zahtjeva poslanih u aplikaciji u pravilnim vremenskim razmacima iz diljem svijeta.

[Snimanje i pretraživanje dijagnostičkih kašnjenja] [ diagnostic] - Umetanje pozive za praćenje i izravnog rezultate u pronalaženju uzroka problema.

[Korištenje praćenja] [ usage] – Saznajte načinom korištenja aplikacije.

[Otklanjanje poteškoća s] [ qna] - i značajka pitanja i odgovora

## <a name="video"></a>Videozapis

[AZURE.VIDEO performance-monitoring-application-insights]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md

 
