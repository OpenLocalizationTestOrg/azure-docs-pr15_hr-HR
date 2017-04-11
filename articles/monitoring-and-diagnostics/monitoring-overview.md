<properties
    pageTitle="Pregled nadzora u Microsoft Azure | Microsoft Azure"
    description="Gornje razine pregled nadzor i dijagnostici u Microsoft Azure uključujući upozorenja, webhooks, automatsko skaliranje itd."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Pregled nadzora u Microsoft Azure

Ovaj članak sadrži konceptualni pregled Azure resursi za nadzor. Pruža pokazivača na informacije na određene vrste resursa.  Više razine o nadzor vaše aplikacije koje nisu Azure točku prikaza potražite u članku [smjernice za nadzor i dijagnostici](../best-practices-monitoring.md).

Aplikacija oblaka su složeni s mnogo premještanje dijelovima. Nadzor nudi podataka da biste bili sigurni da se aplikacija ostaje prema gore i pokretanje u dobar stanju. Također pomaže stave isključivanje mogućih problema ili otklanjanje poteškoća s prethodnih one. Osim toga, možete koristiti nadzora podataka da bi se dobio precizno uvid o aplikaciji. Taj znanja možete olakšavaju da biste poboljšali performanse računala ili maintainability ili automatizirati akcije koje bi inače bio potreban ručno intervencije.

Sljedeći dijagram prikazuje konceptualni prikaz Azure nadzor, uključujući vrste zapisnika možete prikupiti i što sve možete raditi s tim podacima.   

![Logička modela za nadzor i Dijagnostika za resurse koji nisu računalnim](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Slika 1: Konceptualni modela za nadzor i Dijagnostika za resurse koji nisu računalnim

<br/>

![Logička modela za nadzor i Dijagnostika za računalnim resursi](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Slika 2: Konceptualni modela za nadzor i Dijagnostika za računalnim resursi


## <a name="monitoring-sources"></a>Praćenje izvora
### <a name="activity-logs"></a>Zapisnici aktivnosti
Zapisnik aktivnosti (prije se zvao Operational i zapisnika nadzora) možete potražiti podatke o resursu kako je vide Azure infrastrukture. Zapisnik sadrži podatke kao što su vrijeme kada stvoriti ili uništava resursi.  

### <a name="host-vm"></a>Glavno računalo VM
**Samo za izračun**


Neke izračunati resurse kao što su servisi u Oblaku, virtualnih računala i servisa tkanina ste namjenski glavno računalo VM interakcije s. Glavno računalo VM je ekvivalent VM korijenski u modelu hypervisor Hyper-V. U tom slučaju možete prikupiti metriku na samo VM glavno računalo uz OS za goste.  

Za druge servise za Azure nije nužno 1:1 mapiranja između vaše resursa i određeni VM glavno računalo pa metriku VM glavnog računala nisu dostupne.


### <a name="resource---metrics-and-diagnostics-logs"></a>Resurs - dijagnostički Zapisnici i mjerenja
Collectable metriku razlikuju se ovisno o vrsti resursa. Na primjer, virtualnim strojevima nudi Statistika na disku IO i procesora posto. No one STAT ne postoje servisa Bus red koji već sadrži metriku kao što je propusnost reda čekanja veličina i poruke.

Za resurse računalnim možete dobiti metriku na module za goste OS i dijagnostici kao što su Dijagnostika Azure. Azure dijagnostika omogućuje prikupljanje i usmjeravanje prikupite dijagnostičkih podataka na druga mjesta, uključujući Azure prostora za pohranu.

Popis trenutno collectable metriku dostupna je na [podržani metriku](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Aplikacija – dijagnostički zapisnici, zapisnika aplikacije i mjerenja
**Samo za izračun**

Pri vrhu OS gosta u modelu računalnim možete pokrenuti aplikacije. Oni šalji vlastite skup zapisnika i metrike.

Vrste mjerenja

- Mjerača performansi
- Zapisnici aplikacije
- Zapisnike događaja sustava Windows
- .NET izvor događaja
- IIS zapisnika
- Manifest temelji ETW
- Ispisi rušenje
- Evidencije pogrešaka za klijenta


## <a name="uses-for-monitoring-data"></a>Koristi za praćenje podataka

### <a name="visualize"></a>Vizualizacija
Vizualizacija nadzor podataka u grafike i grafikoni pomaže vam trendova daleko da brže pronađu od pogled kroz same podatke.  

Nekoliko metoda koje vizualizacije obuhvaćaju sljedeće:

- Pomoću portala za Azure
- Usmjeravanje podataka do uvida aplikacije Azure
- Usmjeravanje podataka Microsoft PowerBI
- Usmjeravanje podataka alata drugih proizvođača vizualizacija pomoću ili uživo strujanje ili tako da vas alat za čitanje iz arhive u Azure prostora za pohranu

### <a name="archive"></a>Arhiviranje
Nadzor podataka je obično sastavljene Azure prostora za pohranu i tamo čuvati sve dok ne izbrišete.

Nekoliko načina korištenja ove podatke:

- Kada napisali, imate druge alate unutar ili izvan Azure pročitati i obrade.
- Preuzimanje podataka lokalno za lokalnu arhivu ili promijenite pravilnika zadržavanja u oblaku da se podaci za prošireni vremenskog razdoblja.  
- Podatke u ostavite Azure prostora za pohranu beskonačno, iako ćete morati platiti za Azure prostora za pohranu na temelju količinu podataka držite.
-

### <a name="query"></a>Upit
Azure Monitor REST API-JA, križić naredbe platformu sučelja naredbenog retka (EŽA), cmdleta ljuske PowerShell ili .NET SDK možete koristiti za pristup podacima u sustavu ili Azure prostora za pohranu

Primjeri:

-  Dohvaćanje podataka za prilagođenu aplikaciju u praćenju ste napisali
-  Stvaranje prilagođene upite i slanje podataka za aplikacije drugih proizvođača.

### <a name="route"></a>Usmjeravanje
Strujanja nadzora podataka na druga mjesta u stvarnom vremenu.

Primjeri:

- Da biste mogli koristiti alate za vizualizaciju postoji, pošaljite aplikacije uvid u.
- Slanje koncentratora događaj tako da se mogu usmjeravati Alati drugih proizvođača za analizu u stvarnom vremenu.

### <a name="automate"></a>Automatiziranje
Možete koristiti nadzora podataka na događaje okidača ili Primjeri parni cijeli procesa:

- Podatke za automatsko skaliranje računalnim instance prema gore ili dolje na temelju koristiti prilikom učitavanja aplikacije.
- Slanje poruke e-pošte kada metrike presjek unaprijed određenim praga.
- Pozivanje web-URL (webhook) za izvršavanje akcije u sustavu izvan Azure
- Pokretanje programa runbook u Azure Automatizacija da biste izvršili bilo koji niz zadataka

## <a name="methods-of-use"></a>Načini korištenja
Općenito govoreći, možete upravljati praćenje podataka, usmjeravanje i dohvaćanja na jedan od sljedećih načina. Sve metode dostupni su za sve akcije ili vrste podataka.

- [Portal za Azure](https://portal.azure.com)
- [PowerShell](insights-powershell-samples.md)  
- [Različite platforme naredbeni redak sučelja (EŽA)](insights-cli-samples.md)
- [REST API-JA](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Praćenje ponuda Azure korisnika
Azure ima ponude koje su dostupne za nadzor servisa iz Gola metal infrastrukture za telemetriju aplikacije. Najbolje Strategije za nadzor kombinira korištenje sva tri da bi se dobio potpun, detaljne uvid u stanje servisa.

- [Azure Monitor](http://aka.ms/azmondocs) – nudi vizualizacije, upit, usmjeravanje, upozorenjem, automatsko skaliranje i automatizaciju na podacima i Azure infrastrukture (zapisnik aktivnosti) i svakog pojedinačnog Azure resursa (dijagnostičke zapisnike). Ovaj je članak dio dokumentaciju Azure Monitor. Naziv Azure Monitor je objavljen rujan 25 pri Ignite 2016.  Prethodni naziv je "Azure uvide."  
- [Aplikacija uvida](https://azure.microsoft.com/documentation/services/application-insights/) – nudi obogaćenog otkrivanje i dijagnostici ima li problema pri aplikacijskom sloju usluge, dobro integrirane preko podataka iz Azure nadzor. To je platformu Dijagnostika zadane aplikacije servisa web-aplikacijama.  Mogu usmjeravati podatke s drugih servisa s njim.  
- [Prijava analitiku](https://azure.microsoft.com/documentation/services/log-analytics/) dio [Paketa za upravljanje operacije](https://www.microsoft.com/cloud-platform/operations-management-suite) – sadrži jednu zaokruženu IT rješenja za upravljanje za lokalni i drugih proizvođača oblaku infrastrukture (primjerice AWS) uz Azure resursi.  Podatke iz Azure Monitor moguće usmjeriti izravno prijava analitiku da biste vidjeli Zapisnici i metriku za cijelo okruženje sustava na jednom mjestu.     


## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o

- [Azure monitora u videozapis s Ignite 2016](https://myignite.microsoft.com/videos/4977)
- [Uvod u Azure monitora](monitoring-get-started.md)
- [Dijagnostika azure](../azure-diagnostics.md) ako pokušavate utvrditi probleme u aplikaciji za servis u Oblaku, virtualnog računala ili tkanina servisa.
- [Aplikacija uvide](https://azure.microsoft.com/documentation/services/application-insights/) ako pokušavate dijagnostičkih probleme u web-aplikacije servisa aplikaciju programa.
- [Otklanjanje poteškoća za pohranu Azure](../storage/storage-e2e-troubleshooting.md) prilikom korištenja blob polja za pohranu, tablice ili reda čekanja
- [Prijava analitiku](https://azure.microsoft.com/documentation/services/log-analytics/) i [Postupci upravljanja paketa](https://www.microsoft.com/cloud-platform/operations-management-suite)
