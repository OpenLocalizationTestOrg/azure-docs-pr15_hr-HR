<properties
    pageTitle="Rješavanje 502 neispravan pristupnik, 503 Servis nije dostupan pogreške | Microsoft Azure"
    description="Otklanjanje poteškoća s 502 neispravan pristupnik i 503 Servis nije dostupan pogrešaka u web-aplikaciju programa smješten u aplikacije servisa za Azure."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 neispravan pristupnik 503 Servis nije dostupan, pogreška 503, pogreška 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Otklanjanje poteškoća s HTTP pogreške "502 neispravan pristupnik" i "503 Servis nije dostupan" u aplikacijama za Azure web

"502 neispravan pristupnik" i "503 Servis nije dostupan" su uobičajenih pogrešaka u web-aplikaciju programa hostirane na [Servisu Azure aplikacije](http://go.microsoft.com/fwlink/?LinkId=529714). U ovom se članku pomaže pri otklanjanju te pogreške.

Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete se obratiti Azure stručnjaka na [MSDN Azure i na forumima prelijevanje stogu](https://azure.microsoft.com/support/forums/). Osim toga, možete uputiti incident Azure podršci. Idi na [web-mjesto za podršku za Azure](https://azure.microsoft.com/support/options/) i kliknite **Dohvati podrška**.

## <a name="symptom"></a>Simptoma

Kada pregledavate web App, vratit će na HTTP pogreške "502 neispravan pristupnik" ili na HTTP pogreške "503 Servis nije dostupan".

## <a name="cause"></a>Uzrok

Taj se problem često uzrokuje probleme razine aplikacije kao što su:

-   Zahtjevi za dugo traje
-   aplikacije pomoću visoke memorije/CPU-a
-   aplikacija ruši zbog iznimke.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Koraci za otklanjanje poteškoća da biste riješili "502 neispravan pristupnik" i "503 Servis nije dostupan" pogreške

Otklanjanje poteškoća s može se podijeliti u tri različita zadatka, u slijedu:

1.  [Pridržavajte se i nadzirati ponašanja aplikacije](#observe)
2.  [Prikupljanje podataka](#collect)
3.  [Smanjivanje problema](#mitigate)

[Aplikacije servisa web-aplikacije](/services/app-service/web/) nudi razne mogućnosti prilikom svakog koraka.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. pridržavajte i nadzirati ponašanja aplikacije

####    <a name="track-service-health"></a>Praćenje stanja servisa

Microsoft Azure publicizes svaki put kada postoji u prekida ili performanse smanjene performanse servisa. Možete pratiti stanje servisa na [Portalu Azure](https://portal.azure.com/). Dodatne informacije potražite u članku [praćenje stanja servisa](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Praćenje web-aplikaciju programa

Ta mogućnost omogućuje vam da biste saznali ako aplikacija ima problema. U plohu web app, kliknite pločicu **zahtjeve i pogrešaka** . Plohu **metriku** vidjet ćete sva mjerenja možete dodati.

Neke određene parametre koje želite nadzirati za web-aplikacije

-   Prosječna memorija radni skup
-   Prosječno vrijeme odaziva
-   Vrijeme procesora
-   Radni skup memorije
-   Zahtjevi za

![Praćenje web-aplikacije pri rješavanju pogrešaka HTTP 502 neispravan pristupnik i 503 Servis nije dostupan](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Dodatne informacije potražite u članku:

-   [Praćenje aplikacija za Web u aplikacije servisa za Azure](web-sites-monitor.md)
-   [Primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. prikupljanje podataka

####    <a name="use-the-azure-app-service-support-portal"></a>Pomoću portala za podršku aplikacije servisa za Azure

Web Apps nudi otklanjanje problema vezanih uz web-aplikaciju programa tako da pogledate HTTP zapisnike, zapisnika događaja, ispisi postupak i drugo. Možete pristupiti svim tim informacijama pomoću našem podršku portalu pri **http://&lt;naziv aplikacije >.scm.azurewebsites.net/Support**

Portal za podršku za Azure aplikacije servisa nudi tri zasebna kartice podržava tri koraka uobičajeni scenarij za otklanjanje poteškoća:

1.  Pridržavajte se trenutni ponašanje
2.  Analiza prikupljanje informacija za dijagnostiku i pokretanjem ugrađene analyzers
3.  Smanjivanje

Ako se problem se događa odmah, kliknite **Analiza** > **Dijagnostika** > **Dijagnosticiranje sada** da biste stvorili dijagnostičkih sesiju, koji će prikupiti HTTP zapisnike, zapisnike preglednik događaja, memorijski ispisi, evidencije pogrešaka i i i obrada izvješća.

Kada prikupljanja podataka i bit će pokrenuti analizu na podacima i pružiti HTML izvješće.

U slučaju da želite preuzeti podatke, po zadanom će se sprema u mapu D:\home\data\DaaS.

Dodatne informacije o portalu službe za aplikaciju Azure potražite u članku [Novih ažuriranja za proširenja za podršku web-mjesta za Azure web-mjesta](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Kudu konzole za ispravljanje pogrešaka

U sklopu Web Apps konzole za ispravljanje pogrešaka koje možete koristiti za ispravljanje pogrešaka, istraživanje, prijenos datoteke, kao i JSON krajnje točke za dohvaćanje podataka o okruženju. To se naziva ili _Nadzorne ploče IO_ _Kudu konzole_ za web-aplikacije.

Ovu nadzornu ploču možete pristupiti tako da veza **https://&lt;naziv aplikacije >.scm.azurewebsites.net/**.

Neke stvari koje omogućuje Kudu su:

-   postavki okruženja za aplikaciju
-   strujanje zapisnika
-   dijagnostičke ispis
-   Konzola u kojem možete pokrenuti osnovni DOS naredbe i cmdleta ljuske Powershell za ispravljanje pogrešaka.


Drugi korisna značajka Kudu je da, u slučaju aplikacije se prijavi iznimke prvu mogućnost, možete koristiti Kudu i alat za SysInternals Procdump da biste stvorili memorije ako ste. Ove memorijski ispisi su snimaka postupka i često olakšava kompliciranija poteškoća s web-aplikaciju programa.

Dodatne informacije o značajki dostupnih u Kudu potražite u članku [informacije o web-mjesta Azure internetski Alati](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

To je često najjednostavniji način za oporavak jednokratne problema. [Portal za Azure](https://portal.azure.com/)na plohu web app imate mogućnosti za zaustavljanje ili ponovno pokrenite aplikaciju.

 ![ponovno pokrenite aplikaciju za rješavanje pogrešaka HTTP 502 neispravan pristupnik i 503 Servis nije dostupan](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Možete upravljati i web-aplikaciju programa pomoću komponente Powershell Azure. Dodatne informacije potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).
