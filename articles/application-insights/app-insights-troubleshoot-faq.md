<properties 
    pageTitle="Otklanjanje poteškoća i pitanja o aplikaciji uvida" 
    description="Nešto u Visual Studio aplikacije uvida nejasno ili ne radi? Pokušajte ovdje." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Pitanja – uvida aplikacije za platforme ASP.NET

## <a name="configuration-problems"></a>Konfiguriranje problema

*Imam problema s postavka Moje:*

* [Aplikacija za .NET](app-insights-asp-net-troubleshoot-no-data.md)
* [Nadzor već – pokretanje aplikacije](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure dijagnostiku](app-insights-azure-diagnostics.md)
* [Java web-aplikacije](app-insights-java-troubleshoot.md)
* [Druge platforme](app-insights-platforms.md)

*Dobiti bez podataka iz Moj poslužitelj*

* [Postavljanje iznimke vatrozida](app-insights-ip-addresses.md)
* [Postavljanje poslužitelj za platforme ASP.NET](app-insights-monitor-performance-live-website-now.md)
* [Postavljanje jezika Java poslužitelja](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Mogu li koristiti aplikacije uvida s...?

[U odjeljku platforme][platforms]


## <a name="is-it-free"></a>Je li to besplatno?

* Da, ako ste odabrali besplatne [cijene sloju](app-insights-pricing.md). Dobit većina značajki i darežljivi kvote podataka. 
* Morate unijeti podatke kreditne kartice da biste registrirali s Microsoft Azure, ali bez naknade bit će osim ako koristite neki drugi platiti za Azure uslugu ili izričito nadograditi plaćanje sloju.
* Ako aplikaciju pošalje više podataka od mjesečni kvote za besplatne sloju, prestane zapisuju. Ako se to dogodi, možete odabrati da biste pokrenuli plaćanja, ili pričekajte da se kvote ponovnog postavljanja na kraju mjeseca.
* Osnovno korištenje i sesiju podataka se ne odnose ograničenja.
* Postoji besplatnu 30-dnevnu probnu verziju, tijekom kojeg se prebaciti značajke besplatna.
* Svaki resurs aplikacije sadrži zasebnom kvota i postavite njegove cijene sloju nezavisno od ostalih.

#### <a name="what-do-i-get-if-i-pay"></a>Što se ako se platiti?

* Veće [mjesečni kvote podataka](https://azure.microsoft.com/pricing/details/application-insights/).
* Mogućnost za plaćanje 'pokrivenosti"da biste nastavili prikupljanje podataka putem mjesečni kvote. Ako vaši podaci prelazi preko kvote, koristite naplaćuje po Mb.
* [Izvoz neprekinuto](app-insights-export-telemetry.md).


## <a name="q14"></a>Što aplikacije uvida izmjena s u projektu?

Pojedinosti ovise o vrsti projekta. Za web-aplikaciju:


+ Dodaje te datoteke u projekt:

 + ApplicationInsights.config. 
 + Ai.js


+ Instalira te pakete NuGet:

 -  *Aplikacija uvida API* - core API-JA

 -  *API uvida aplikaciju za web-aplikacije* - koristiti za slanje telemetrijskih s poslužitelja

 -  *API uvida aplikacije za aplikacije JavaScript* - koristiti za slanje telemetrijskih putem klijentskog programa

    Pakete Uključi ove skupine:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Umeće stavke:

 - Web.config

 - Packages.config

+ (Novi projekata samo – ako [Dodavanje aplikacije uvida u postojeći projekt][start], morate učiniti ručno.) Umeće isječci u kod postupak klijentske i poslužiteljske Inicijalizacija ih s ID aplikacije uvida resursa. Na primjer, u aplikaciji programa MVC kod umetnut je u osnovnu stranicu Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Kako se nadograditi iz starije verzije SDK?

Pročitajte [Napomene](app-insights-release-notes.md) za SDK odgovara vrsti vašeg računala. 



## <a name="update"></a>Kako promijeniti koji Azure resursa projektu šalje podatke?

U pregledniku rješenja, desnom tipkom miša kliknite `ApplicationInsights.config` , a zatim odaberite **Ažuriranje aplikacije uvide**. Podatke možete poslati postojeći ili novi resurs u Azure. Čarobnjak za ažuriranje mijenja ključ instrumentation u ApplicationInsights.config koji određuje koje poslužitelj SDK šalje podatke. Ako ne poništite odabir "Ažuriraj sve", ključ gdje će se pojaviti na web-stranica i promijeniti.


#### <a name="data"></a>Koliko dugo podaci se zadržavaju na portalu? Je li sigurno?

Pogledajte [zadržavanje podataka i privatnosti][data].

## <a name="logging"></a>Bilježenje u zapisnik

#### <a name="post"></a>Kako vidjeti POST podatke u dijagnostike pretraživanja?

Ne možemo ne zapisivanje podataka objavu automatski, ali možete koristiti TrackTrace poziv: smjestiti podatke u parametru poruke. Iako ne možete filtrirati na njemu ima više ograničenje veličine od ograničenja svojstva niza. 

## <a name="security"></a>Sigurnost

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Sigurna Moji podaci na portalu? Koliko se je zadržava?

U odjeljku [podataka zadržavanja i zaštita privatnosti][data].


## <a name="q17"></a>Imate li omogućeno sve u aplikaciji uvida

<table border="1">
<tr><th>Trebali biste vidjeti</th><th>Kupnja</th><th>Zašto je želite</th></tr>
<tr><td>Dostupnost grafikoni</td><td><a href="../app-insights-monitor-web-app-availability/">Testovi za web</a></td><td>Web-aplikaciju programa je prema gore</td></tr>
<tr><td>Mjerača performansi poslužitelj aplikacije: puta odgovor...
</td><td><a href="../app-insights-asp-net/">Dodavanje aplikacije uvid u projekt</a><br/>ili <br/><a href="../app-insights-monitor-performance-live-website-now/">Instalacija AI Status monitora na poslužitelju</a> (ili napišite vlastiti kod da biste <a href="../app-insights-api-custom-events-metrics/#track-dependency">pratili ovisnosti</a>)</td><td>Otkrivanje poteškoća mjerača performansi</td></tr>
<tr><td>Ovisnost telemetrijskih</td><td><a href="../app-insights-monitor-performance-live-website-now/">Instalacija Nadzornik stanja AI na poslužitelju</a></td><td>Dijagnosticiranje problema s bazama podataka ili drugih vanjskih komponenti</td></tr>
<tr><td>Stoga kašnjenja zatražite od iznimke</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">Umetanje TrackException pozive u kodu</a> (ali neke su prijavljene automatski)</td><td>Otkrivanje i dijagnosticiranje iznimke</td></tr>
<tr><td>Kašnjenja zapisnika pretraživanja</td><td><a href="../app-insights-search-diagnostic-logs/">Dodavanje prilagodnik za zapisivanje</a></td><td>Dijagnosticiranje iznimke problemi mjerača performansi</td></tr>
<tr><td>Osnove korištenja klijent: prikaza stranice, sesijama,...</td><td><a href="../app-insights-javascript/">Initializer JavaScript koda na web-stranicama</a></td><td>Analize korištenja</td></tr>
<tr><td>Prilagođeni metriku klijenta</td><td><a href="../app-insights-api-custom-events-metrics/">Praćenje poziva na web-stranicama</a></td><td>Poboljšanje doživljaja rada</td></tr>
<tr><td>Prilagođeni metriku poslužitelja</td><td><a href="../app-insights-api-custom-events-metrics/">Praćenje poziva u kodu poslužitelja</a></td><td>Poslovno obavještavanje</td></tr>
</table>


## <a name="automation"></a>Automatizacija

Možete [napisati skripte komponente PowerShell](app-insights-powershell.md) za stvaranje i ažuriranje aplikacije uvida resursi.

## <a name="more-answers"></a>Više odgovora

* [Forum uvida aplikacije](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 