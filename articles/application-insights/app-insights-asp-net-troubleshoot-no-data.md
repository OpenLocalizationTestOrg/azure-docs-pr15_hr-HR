<properties 
    pageTitle="Nema podataka – uvida aplikacije za .NET za otklanjanje poteškoća" 
    description="Ne prikazuju podatke u Visual Studio aplikacije uvida? Pokušajte ovdje." 
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
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Nema podataka – uvida aplikacije za .NET za otklanjanje poteškoća

## <a name="some-of-my-telemetry-is-missing"></a>Neke od Moje telemetrijskih nedostaje

*U uvide aplikacije samo vidim razlomak događaja koji se generira Moje aplikacije.*

* Ako vam se dosljedno isti razlomak, uzrok su vjerojatno prilagodljivo [uzorkovanje](app-insights-sampling.md). Da biste to potvrdili, otvorite pretraživanje (iz plohu pregled) i pogledajte instance komponente zahtjeva za ili neki drugi događaj. Pri dnu zaslona u odjeljku Svojstva kliknite "..." da biste vidjeli detalje cijelog svojstvo. Ako zahtjev za brojanje > 1, a zatim uzorkovanje se u operaciji. 
* U suprotnom, moguće je da vam se odlazak na [Ograničavanje podataka](app-insights-pricing.md#limits-summary) za cijene tarifa. Ta ograničenja primjenjuju se u minuti.

## <a name="no-data-from-my-server"></a>Nema podataka iz Moj poslužitelj

*Instalirane aplikacije na Moje web-poslužitelj, a ne vidim sve telemetrijskih iz nje. Na Moje računalo razvojni je radio u redu.*

* Vjerojatno vatrozid problem. [Postavljanje iznimke vatrozida za uvid aplikacije za slanje podataka](app-insights-ip-addresses.md).

*Li [instaliran Nadzornik stanja](app-insights-monitor-performance-live-website-now.md) na Moje web-poslužitelju praćenje postojeće aplikacije. Ne vidim sve rezultate.*

* Potražite u članku [Otklanjanje poteškoća Nadzornik stanja](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Mogućnost bez 'Dodavanje aplikacije uvida' u Visual Studio

*Prilikom stvaranja novog projekta u Visual Studio ili kada se desnom tipkom miša kliknite postojeći projekt u pregledniku rješenja, ne vidim sve mogućnosti aplikacije uvide.*

+ Sve vrste projekta .NET podržava alate. Podržane su web- a WCF projekata. Za ostale vrste projekta kao što je aplikacija za stolna računala ili servisa, i dalje možete [ručno dodati u SDK aplikacije uvid u projekt](app-insights-windows-desktop.md).
+ Provjerite je li [Visual Studio 2013 ažuriranje 3 ili noviji](http://go.microsoft.com/fwlink/?LinkId=397827). Dolazi instalirana pomoću alata za uvid aplikacije.
+ Odaberite **Alati** **proširenja i ažuriranja** , a zatim potvrdite **Alati uvida aplikacija** instalirana ni omogućena. Ako je tako, kliknite **ažuriranja** da biste vidjeli postoji li ažuriranja.
+ Otvorite dijaloški okvir novi projekt, a zatim odaberite ASP.NET web-aplikacije. Ako vidite postoji mogućnost uvida aplikacije, pa su alati instalirani. Ako nije, pokušajte deinstalirati i ponovno instaliranje alata za uvid aplikacije.


## <a name="q02"></a>Nije uspjelo dodavanje aplikacije uvida

*Prilikom stvaranja novog projekta web ili kada pokušam da biste dodali aplikaciju uvida u postojeći projekt, prikazuje se poruka o pogrešci.*

Vjerojatno uzrokuje:

* Komunikacija s portala za aplikacije uvida nije uspjelo; ili
* Postoji neki problem s računom Azure;
* Imate samo [za čitanje za pretplatu ili grupu koju ste pokušali stvoriti novi resurs](app-insights-resources-roles-access-control.md).

Rješenje:

+ Provjerite vjerodajnice za prijavu dobili za račun za desno Azure. 
+ U pregledniku, provjerite imate li pristup za [Azure portal](https://portal.azure.com). Otvorite postavke, a postoji li sve ograničenja.
+ [Dodavanje aplikacije uvida u postojeći projekt](app-insights-asp-net.md): U pregledniku rješenja, desnom tipkom miša kliknite projekt i odaberite "Dodaj aplikaciju uvide."
+ Ako je i dalje ne funkcionira, slijedite [postupak ručno](app-insights-windows-services.md) dodavanje resursa na portalu, a zatim dodajte SDK projekta. 

## <a name="emptykey"></a>Prikazuje se pogreška "Instrumentation ključa ne može biti prazna"

Čini se nešto nije u redu dok su instalacije aplikacije uvida ili možda prilagodnik zapisivanje.

U pregledniku rješenja, desnom tipkom miša kliknite `ApplicationInsights.config` , a zatim odaberite **Konfiguriranje uvida aplikacije**. Prikazat će se dijaloški okvir koji vas za prijavu na Azure poziva pa stvorite resursa do uvida aplikacije ili iskoristite postojeći.


##<a name="NuGetBuild"></a>"NuGet paketi nedostaju" na moj Sastavi poslužitelj

*Kada mogu se pogrešaka na Moje računalo razvoj, ali prikazuje se pogreška NuGet na poslužitelju Sastavi sve sastavlja u redu.*

Pročitajte članak [NuGet paket Vrati](http://docs.nuget.org/Consume/Package-Restore) i [Automatsko paketa](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Nema naredbe izbornika da biste otvorili aplikaciju uvida s Visual Studio

*Kada se desnom tipkom miša kliknite Moje project Explorer rješenja, ne vidim naredbi aplikacije uvida ili ne vidim odgovarajuću naredbu Otvori uvida aplikacije.*

Vjerojatno uzrokuje:

* Ako ste ručno stvorili resursa uvida aplikacije ili projekta je vrste koji nije podržan pomoću alata za uvid aplikacije.
* Alati za aplikaciju uvida onemogućene su u Visual Studio.
* Visual Studio je starije od 2013 ažuriranje 3.

Rješenje:

* Provjerite je li vaša verzija Visual Studio ažuriranje 2013 3 ili noviji.
* Odaberite **Alati** **proširenja i ažuriranja** , a zatim potvrdite **Alati uvida aplikacija** instalirana ni omogućena. Ako je tako, kliknite **ažuriranja** da biste vidjeli postoji li ažuriranja.
* Desnom tipkom miša kliknite projekta u programu Explorer rješenja. Ako vidite naredbu **Konfiguriranje uvida aplikacije**, pomoću njega možete povezati projekta resursa u servisu uvida aplikacije.


U suprotnom vaše vrsta projekta izravno ne podržava Alati uvida aplikacije. Da biste vidjeli telemetrijskih, prijavite se na [portal za Azure](https://portal.azure.com), na lijevoj navigacijskoj traci odaberite aplikacije uvida i odaberite aplikacije.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Pristup odbijen" na otvaranju uvida aplikacije Visual Studio

*Naredba na izborniku uvida otvorene aplikacije me vodi na portalu za Azure, ali se pogreška "pristup odbijen".*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Microsoft prijavu koje ste zadnje koristili na zadani preglednik ne obuhvaća pristup [resursu koja je stvorena kad aplikacije uvida dodana za aplikaciju](app-insights-asp-net.md). Postoje dva razloga vjerojatno: 

* Imate više od jednog Microsoftova računa – možda Službeni telefon i osobni Microsoftov račun? Prijavu koje ste zadnje koristili na zadani preglednik je za neki drugi račun od onog koji ima pristup da biste [dodali aplikaciju uvid u projekt](app-insights-asp-net.md). 

 * Rješenje: Kliknite svoje ime pri gornjeg desnog prozora preglednika i odjava. Zatim se prijavite pomoću računa koji ima pristup. Zatim na lijevoj navigacijskoj traci kliknite uvida aplikaciju i odaberite aplikacije.

* Netko drugi dodao aplikacije uvid u projekt, a zaboravili omogućuju [pristup grupi resursa](app-insights-resources-roles-access-control.md) u kojem je stvorena. 

 * Rješenje: Ako se ona koriste račun tvrtke ili ustanove, mogu dodavati vam u tim ili možete dodijeliti pojedinačnih programa access u grupu resursa.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Resursa nije pronađen" na otvaranju uvida aplikacije Visual Studio

*Naredba na izborniku uvida otvorene aplikacije me vodi na portalu za Azure, ali se pogreška "resursa nije pronađen".*

Vjerojatno uzrokuje:

* Izbrisan je aplikacija uvida resursa za svoju aplikaciju; ili
* Tipku instrumentation je postavite ili izmijenjene u ApplicationInsights.config tako da uredite izravno bez ažuriranja datoteka projekta. 

Unesite instrumentation ApplicationInsights.config kontrole koje se šalju u telemetrijskih. Redak u datoteci projekta upravlja koji se otvori kada koristite naredbe u Visual Studio. 

Rješenje:

* U pregledniku rješenja, desnom tipkom miša kliknite projekt i odaberite aplikaciju uvide, konfiguriranje uvida aplikacije. U dijaloškom okviru ili možete poslati telemetrijskih postojeći resurs ili stvorite novi. Ili:
* Izravno otvoriti resurs. Prijavite se na [portal za Azure](https://portal.azure.com), kliknite uvida aplikacije na lijevoj navigacijskoj traci, a zatim pokrenite aplikaciju.



## <a name="where-do-i-find-my-telemetry"></a>Gdje se nalazi Moje telemetrijskih?

*Li prijavljeni na [portal Microsoft Azure](https://portal.azure.com)i koju tražim na Azure kućni nadzorne ploče. Stoga gdje se nalazi Moje aplikacije uvida podatke?*

* Na lijevoj navigacijskoj traci kliknite uvida aplikacije, a zatim naziv aplikacije. Ako nemate sve projekata postoji, morate [Dodavanje i konfiguriranje aplikacije uvida u projektu web](app-insights-asp-net.md).

    Postoji vidjet ćete neke sažetak grafikona. Možete klikati ih da biste vidjeli dodatne detalje.

* U Visual Studio dok ste pogrešaka aplikacije, kliknite gumb uvida aplikacije.


## <a name="q03"></a>Nema podataka o poslužitelju (ili bez podataka uopće)

*Li pokrenuli aplikacija, a zatim servis aplikacije uvida u Microsoft Azure, ali svi grafikoni prikazuju "Saznajte kako prikupiti..." ili "Nije konfiguriran."* Ili, *samo prikaz stranice i korisničke podatke, ali nema podataka o poslužitelju.*

+ Pokretanje aplikacije u načinu rada za ispravljanje pogrešaka u Visual Studio (F5). Korištenje aplikacije da biste generirali neke telemetrijskih. Provjerite može vidjeti opisanih u izlaznom prozoru Visual Studio događaja. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Portal za aplikacije uvida otvorite [Dijagnostike pretraživanja](app-insights-diagnostic-search.md). Podaci o obično se ovdje prvi put.
+ Kliknite gumb Osvježi. Na plohu povremeno Osvježi, ali možete i stvoriti ručno. Interval osvježavanja je više vremena veći vremenski raspon.
+ Provjerite odgovara tipki instrumentation. Na glavnom plohu aplikacije na portalu aplikacije uvida u **Essentials** padajući popis, potražite **ključ Instrumentation**. Nakon toga u projektu u Visual Studio, otvorite ApplicationInsights.config i potražite na `<instrumentationkey>`. Provjerite jesu li dvije tipke jednako. Ako ne želite:
 + Na portalu kliknite uvida aplikacije i potražite resursa aplikacije s desne tipke; ili
 + U programu Explorer Visual Studio u rješenje, desnom tipkom miša kliknite projekt, a zatim odaberite aplikaciju uvide, konfiguriranje. Ponovno postavite aplikaciju da biste poslali telemetrijskih desnom resursa.
 + Ako ne možete pronaći odgovarajuće tipke, provjerite koristite iste vjerodajnice za prijavu u Visual Studio, kao na portalu.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ Pogledajte kartu stanje servisa u [Microsoft Azure kućni nadzorne ploče](https://portal.azure.com). Ako postoje neka upozorenja indications, pričekajte dok se oni su vraćene na u redu i zatim zatvorite i ponovno otvorite svoje plohu aplikacije uvida aplikacije.
+ Provjerite i [naše status bloga](http://blogs.msdn.com/b/applicationinsights-status/).
+ Jeste li pisanja koda za [poslužiteljsko SDK](app-insights-api-custom-events-metrics.md) koje se mogu promijeniti ključ instrumentation u `TelemetryClient` instanci ili `TelemetryContext`? Ili niste pisanje [Konfiguracija filtar ili uzorkovanje](app-insights-api-filtering-sampling.md) koja možda filtriranje out prevelik?
+ Ako uređujete ApplicationInsights.config, pažljivo Provjerite konfiguraciju [TelemetryInitializers](app-insights-api-filtering-sampling.md)i TelemetryProcessors. Pogrešno nazivom vrsta ili parametar može uzrokovati SDK slanje bez podataka.

## <a name="q04"></a>Nema podataka o korištenju prikaza stranice, preglednicima

*Prikazuje podatke u vrijeme odaziva poslužitelja i zahtjeve poslužitelja grafikoni, ali bez podataka u biblioteci prikaz stranice ili u pregledniku ili korištenje blades.*

Podaci dolaze iz skripte na web-stranicama. 

+ Ako ste dodali aplikacije uvida u postojeći projekt web, [morate ručno Dodavanje skripte](app-insights-javascript.md).
+ Provjerite je li Internet Explorer ne prikazuje na web-mjesto u načinu kompatibilnosti.
+ Značajka web-preglednika ispravljanje pogrešaka (F12 u nekim se preglednicima pa odaberite mreža) da biste potvrdili da se podaci koji se šalju u `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Nema podataka o ovisnosti ili iznimke

Potražite u članku [ovisnost telemetrijskih](app-insights-asp-net-dependencies.md) i [telemetrijskih iznimke](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Nema podataka o performansama

Podataka o performansama (CPU-a, IO stopa i tako dalje) dostupna je za [Java web-servisi](app-insights-java-collectd.md) [Windows aplikacija za stolna računala](app-insights-windows-desktop.md), [IIS web-aplikacija i servisa ako instalirate Nadzornik stanja](app-insights-monitor-performance-live-website-now.md)i [Azure servise u Oblaku](app-insights-azure.md). To ćete pronaći u odjeljku postavke poslužitelja.

Nije dostupna za Azure web-mjesta.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Nema podataka (poslužitelj) jer je objavljen aplikaciju Moj poslužitelj

+ Provjera zapravo kopirali sve Microsoft. DLL bibliotekama ApplicationInsights na poslužitelj, zajedno s Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ U vatrozid, možda ćete morati [otvoriti neke TCP priključci](app-insights-ip-addresses.md#data-access-api).
+ Ako imate koristite proxy poslužitelj za slanje iz s korporacijskom mrežom, postavite [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) u Web.config
+ Windows Server 2008: Provjerite jeste li instalirali sljedeće ažuriranja: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523) [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Se koristi da biste vidjeli podatke, ali je on prestao

* Provjerite [status bloga](http://blogs.msdn.com/b/applicationinsights-status/).
* Imate kliknite mjesečni kvotu točaka podataka? Otvorite postavke/kvota i određivanje cijena da biste otkrili. Ako je tako, možete nadograditi tarifu ili platiti dodatne kapaciteta. Potražite u članku [cijene shemu](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Ne vidim sve podatke koje se očekuje

Ako aplikacija šalje velike količine podataka i koristite uvide SDK aplikacije za ASP.NET verzije 2.0.0-beta3 ili noviji, značajku [prilagodljivo uzorkovanje](app-insights-sampling.md) može raditi i pošaljite samo postotak vaše telemetrijskih. 

Možete ga onemogućiti, no to se ne preporučuje. Stvaranje uzoraka osmišljene tako da se povezane telemetrijskih pravilno prenose, dijagnostičkih svrhe. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Pogrešan geografske podatke u telemetrijskih korisnika

Grad, regija i država dimenzije potječu iz IP adrese, a uvijek nisu točne.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Iznimke "metode nije pronađen" o pokretanju u Azure servise u Oblaku

Jeste li sastavljanje za .NET 4.6? 4.6 automatski nije podržan u ulogama Azure servise u Oblaku. [Instalacija 4.6 na ulogama](../cloud-services/cloud-services-dotnet-install-dotnet.md) prije pokretanja aplikacije.

## <a name="still-not-working"></a>I dalje ne funkcionira...

* [Forum uvida aplikacije](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

