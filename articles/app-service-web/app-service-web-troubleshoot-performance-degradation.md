<properties
    pageTitle="Usporiti web app performanse aplikacije servisa za | Microsoft Azure"
    description="Ovaj članak sadrži sporo web app otkloniti poteškoće s performansama u servisu Azure aplikacije."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="web aplikacije performanse spore aplikacije aplikacije sporo"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Otklanjanje poteškoća s sporo web app performanse aplikacije servisa za Azure

Ovaj članak sadrži sporo web app otkloniti poteškoće s performansama u [Servisu Azure aplikacije](http://go.microsoft.com/fwlink/?LinkId=529714).

Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete se obratiti Azure stručnjaka na [MSDN Azure i na forumima prelijevanje stogu](https://azure.microsoft.com/support/forums/). Osim toga, možete uputiti incident Azure podršci. Idi na [web-mjesto za podršku za Azure](https://azure.microsoft.com/support/options/) i kliknite **Dohvati podrška**.

## <a name="symptom"></a>Simptoma

Kada pregledavate web-aplikaciji učitavanja stranice sporo i ponekad vremenskog ograničenja.

## <a name="cause"></a>Uzrok

Taj se problem često uzrokuje probleme razine aplikacije kao što su:

-   Zahtjevi za dugo traje
-   aplikacije pomoću visoke memorije/CPU-a
-   aplikacija ruši zbog iznimke.

## <a name="troubleshooting-steps"></a>Korake za otklanjanje poteškoća

Otklanjanje poteškoća s može se podijeliti u tri različita zadatka, u slijedu:

1.  [Pridržavajte se i nadzirati ponašanja aplikacije](#observe)
2.  [Prikupljanje podataka](#collect)
3.  [Smanjivanje problema](#mitigate)

[Aplikacije servisa web-aplikacije](/services/app-service/web/) nudi razne mogućnosti prilikom svakog koraka.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. pridržavajte i nadzirati ponašanja aplikacije

#### <a name="track-service-health"></a>Praćenje stanja servisa

Microsoft Azure publicizes svaki put kada postoji u prekida ili performanse smanjene performanse servisa. Možete pratiti stanje servisa na [Portalu Azure](https://portal.azure.com/). Dodatne informacije potražite u članku [praćenje stanja servisa](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Praćenje web-aplikaciju programa

Ta mogućnost omogućuje vam da biste saznali ako aplikacija ima problema. U plohu web app, kliknite pločicu **zahtjeve i pogrešaka** . Plohu **metriku** vidjet ćete sva mjerenja možete dodati.

Neke određene parametre koje želite nadzirati za web-aplikacije

-   Prosječna memorija radni skup
-   Prosječno vrijeme odaziva
-   Vrijeme procesora
-   Radni skup memorije
-   Zahtjevi za

![Praćenje performansi web app](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Dodatne informacije potražite u članku:

-   [Praćenje aplikacija za Web u aplikacije servisa za Azure](web-sites-monitor.md)
-   [Primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Praćenje statusa krajnja točka za web

Ako koristite web-aplikaciju programa u **standardne** cijene sloju, web-aplikacijama možete nadzirati 2 krajnje točke sa 3 zemljopisnog mjesta.

Krajnja točka za nadzor konfigurira testira web iz zemlj distributed mjesta koja testirati reakcija i vrijeme aktivnosti URL-ova web. Test izvodi operaciju HTTP GET na web-URL za određivanje reakcija i vrijeme aktivnosti na svakom mjestu. Svaki konfiguriranog mjesta pokreće test svakih pet minuta.

Vrijeme aktivnosti nadzirati pomoću kodova HTTP odgovor, a reakcija mjeri se u milisekundama. Nadzor probno neće uspjeti ako HTTP odgovor kod je veće od ili jednako 400 ili odgovor traje više od 30 sekundi. Krajnje smatra dostupno samo ako je njegov nadzora testira uspjeti iz određene lokacije.

Da biste ga postavili, pročitajte članak [Kako: praćenje stanja krajnjoj točki web](web-sites-monitor.md#webendpointstatus).

Pogledajte [čuvanja Azure web-mjesta prema gore i nadzor krajnja točka - s Schackow isto](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) videozapis o krajnjoj točki nadzor.

#### <a name="application-performance-monitoring-using-extensions"></a>Aplikacija praćenje performansi pomoću proširenja

Performanse računala možete pratiti i tako da korištenje _proširenja web-mjesta_.

Svaki aplikacije servisa za web-aplikacije pruža krajnju točku sustava extensible upravljanja koje omogućuje vam korištenje naprednih skup alata za uveden kao proširenja web-mjesta. Ti Alati u rasponu od izvorni kod uređivači kao što su [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) do alata za upravljanje povezanim resurse kao što su baze podataka MySQL povezan s web-aplikacijama.

[Azure aplikacije uvida](/services/application-insights/) i [Novi Relic](/marketplace/partners/newrelic/newrelic/) su dvije performanse proširenja web-mjesta koji su dostupni za nadzor. Da biste koristili novi Relic, instalirajte agent tijekom rada. Da biste koristili Azure aplikacije uvide, ponovno stvaranje kod pomoću programa SDK, a možete instalirati i datotečni nastavak omogućuje pristup dodatne podatke. SDK omogućuje pisanje koda za nadzor korištenja i performanse aplikacije detaljnije.

Da biste koristili aplikaciju uvide, potražite u članku [praćenje performansi u web-aplikacijama](../application-insights/app-insights-web-monitor-performance.md).

Da biste koristili novi Relic, potražite u članku [Novi Relic performanse Upravljanje aplikacijama na Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. prikupljanje podataka

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Omogućite zapisivanje Dijagnostika za web-aplikacije

Okruženje za web-aplikacije sadrži dijagnostičkih funkcija Zapisnički podaci za web-poslužitelj i web-aplikacije. Te su logički razdvojeni u Dijagnostika poslužitelja web i aplikacije dijagnostici.

##### <a name="web-server-diagnostics"></a>Dijagnostika poslužitelja za web

Možete omogućiti ili onemogućiti sljedeće vrste zapisnika:

-   **Detaljne zapisivanje pogreške** – detaljne informacije o pogrešci za HTTP kodovima stanja koje označavaju pogreške (kod stanja 400 ili noviji). To može sadržavati informacije koje možete utvrditi Zašto poslužitelj je vratio kod pogreške.
-   **Nije uspjelo zahtjev za praćenje** - detaljne informacije o neuspjelih zahtjeva, uključujući za praćenje IIS komponente koje se koriste za obradu zahtjeva i u vrijeme u svaku od njih. To može biti korisno ako pokušavate poboljšati performanse web app ili izdvajanja što uzrokuje konkretne HTTP pogreške.
-   **Zapisivanje poslužitelja u web** - informacija o HTTP transakcije W3C prošireni zapisnika datotečnom obliku. To je korisno prilikom određivanja cjelokupan metrika web app, kao što su broj zahtjeva rukuje ili zahtjeva za koliko su iz određene IP adrese.

##### <a name="application-diagnostics"></a>Aplikacije dijagnostici

Aplikacije dijagnostici omogućuje snimanje informacija koje je stvorio web-aplikacije. ASP.NET aplikacije mogu koristiti u `System.Diagnostics.Trace` klase za zapisivanje podataka za zapisnik aplikacije dijagnostici.

Detaljne upute za konfiguriranje aplikacija za zapisivanje, potražite u članku [Omogućivanje Dijagnostika bilježenje u zapisnik web-aplikacijama u servisu Azure aplikacije](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Korištenje udaljene Profiliranje

Na servisu Azure aplikacije Web Apps, aplikacije API-JA i WebJobs može biti daljinski profiled. Ako proces se izvodi sporije nego je očekivano, ili Latencija HTTP zahtjeva su veći od normalni i procesora koristi proces je visoka, možete daljinski profila proces i dobiti poziv snop procesora uzorkovanje analizirati postupak aktivnosti i kod tipkovni putovi.

Dodatne informacije o potražite u članku [udaljene Profiliranje podrška za aplikacije servisa za Azure](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Pomoću portala za podršku aplikacije servisa za Azure

Web Apps nudi otklanjanje problema vezanih uz web-aplikaciju programa tako da pogledate HTTP zapisnike, zapisnika događaja, ispisi postupak i drugo. Možete pristupiti svim tim informacijama pomoću našem podršku portalu pri **http://&lt;naziv aplikacije >.scm.azurewebsites.net/Support**

Portal za podršku za Azure aplikacije servisa nudi tri zasebna kartice podržava tri koraka uobičajeni scenarij za otklanjanje poteškoća:

1.  Pridržavajte se trenutni ponašanje
2.  Analiza prikupljanje informacija za dijagnostiku i pokretanjem ugrađene analyzers
3.  Smanjivanje

Ako se problem se događa odmah, kliknite **Analiza** > **Dijagnostika** > **Dijagnosticiranje sada** da biste stvorili dijagnostičkih sesiju, koji će prikupiti HTTP zapisnike, zapisnike preglednik događaja, memorijski ispisi, evidencije pogrešaka i i i obrada izvješća.

Kada prikupljanja podataka i bit će pokrenuti analizu na podacima i pružiti HTML izvješće.

U slučaju da želite preuzeti podatke, po zadanom će se sprema u mapu D:\home\data\DaaS.

Dodatne informacije o portalu službe za aplikaciju Azure potražite u članku [Novih ažuriranja za proširenja za podršku web-mjesta za Azure web-mjesta](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Kudu konzole za ispravljanje pogrešaka

U sklopu Web Apps konzole za ispravljanje pogrešaka koje možete koristiti za ispravljanje pogrešaka, istraživanje, prijenos datoteke, kao i JSON krajnje točke za dohvaćanje podataka o okruženju. To se naziva ili _Nadzorne ploče IO_ _Kudu konzole_ za web-aplikacije.

Ovu nadzornu ploču možete pristupiti tako da veza **https://&lt;naziv aplikacije >.scm.azurewebsites.net/**.

Neke stvari koje omogućuje Kudu su:

-   postavki okruženja za aplikaciju
-   strujanje zapisnika
-   dijagnostičke ispis
-   Konzola u kojem možete pokrenuti osnovni DOS naredbe i cmdleta ljuske Powershell za ispravljanje pogrešaka.


Drugi korisna značajka Kudu je da, u slučaju aplikacije se prijavi iznimke prvu mogućnost, možete koristiti Kudu i alat za SysInternals Procdump da biste stvorili memorije ako ste. Ove memorijski ispisi su snimaka postupka i često olakšava kompliciranija poteškoća s web-aplikaciju programa.

Dodatne informacije o značajki dostupnih u Kudu potražite u članku [Alati servisa timskog web-mjesta za Azure trebate znati o](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. prevladavanje problema

####    <a name="scale-the-web-app"></a>Promjena veličine web-aplikaciji

Na servisu Azure aplikacije radi poboljšanja performansi i propusnost, možete prilagoditi skale na kojoj su pokrenuti aplikaciju. Skaliranje gore web-aplikacijama obuhvaća dvije povezane akcije: promjena tarifa aplikacije servisa za veće sloju cijene i konfiguriranju određene postavke kada ste se premjestili na višu cijene sloju.

Dodatne informacije o skaliranje, potražite u članku [Skaliranje web app u aplikacije servisa za Azure](web-sites-scale.md).

Osim toga, možete odabrati da biste pokrenuli aplikaciju na više od jedne instance. To ne samo pruža dodatne mogućnosti obrade, ali i daje neke količinu odstupanje kvara. Ako se postupak funkcionira na jednu instancu, drugoj instanci će i dalje prijeđite posluživanje zahtjeva.

Možete postaviti skaliranje ručno ili automatski.

####    <a name="use-autoheal"></a>Korištenje AutoHeal

AutoHeal recycles procesu aplikacije na temelju postavke koje ste odabrali (kao što su promjene konfiguracije, zahtjeve, utemeljen na memorije ograničenja ili vrijeme potrebno za izvršavanje zahtjeva za). U većini slučajeva, koš za smeće postupak je najbrži način za oporavak od problema. Iako se uvijek možete ponovno pokrenuti web-aplikaciji iz izravno unutar portala za Azure, AutoHeal će stvoriti automatski umjesto vas. Sve što trebate napraviti je dodavanje nekih okidača u korijen konfiguraciju za web-aplikaciju programa. Imajte na umu da su te postavke daju na isti način čak i ako aplikacija nije .net jedan.

Dodatne informacije potražite u članku [Automatsko automatskog popravka Azure web-mjesta](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Ponovno pokrenite aplikaciju za web

To je često najjednostavniji način za oporavak jednokratni problema. [Portal za Azure](https://portal.azure.com/)na plohu web app imate mogućnosti za zaustavljanje ili ponovno pokrenite aplikaciju.

 ![ponovno pokrenite web app da biste riješili probleme s performansama](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Možete upravljati i web-aplikaciju programa pomoću komponente Powershell Azure. Dodatne informacije potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).
