<properties 
    pageTitle="Napomene za uvid aplikacija za Windows" 
    description="Najnovija ažuriranja za Windows SDK trgovine." 
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
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Napomene za aplikaciju uvida SDK za Windows Phone i spremiti

SDK uvida aplikacije šalje telemetrijskih o aplikaciji za uživo [Uvida aplikacije](https://azure.microsoft.com/services/application-insights/), gdje možete analizirati njegova korištenja i performanse.


## <a name="version-111"></a>Verzija 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Riješite je Zastoj tijekom pad sustava prilikom korištenja SDK Silverlight za Windows Phone. Nakon ta promjena sve rušenje koji se događa kasnije ~ 2 sekunde nakon poziv WindowsAppInitialier.InitializeAsync(...) će biti dosljedan disk, pa će biti poslana sljedeći put pokrene aplikaciju. Slučaju neočekivanog prije ~ 2 sekunde nakon poziv će zanemariti.  
- Postavite na NuGet ovisnosti na određenu verziju Core i Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Temeljni SDK

- Temeljni se upravlja iz github. Buduća napomene SDK Core pronaći ćete [u github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Verzije 1.1

### <a name="core-sdk"></a>Temeljni SDK

- SDK sada predstavlja nove vrste telemetrijskih ```DependencyTelemetry``` koji sadrži podatke o ovisnosti poziva iz aplikacije
- Nova metoda ```TelemetryClient.TrackDependency``` omogućuje da biste poslali informacije o ovisnosti pozive iz aplikacije

## <a name="version-100"></a>Verzija 1.0.0

### <a name="windows-app-sdk"></a>Windows SDK aplikacije

- Novi Inicijalizacija za aplikacije za Windows. Novi `WindowsAppInitializer` klase s `InitializeAsync()` metodom za pokretački Inicijalizacija SDK zbirke. Ta promjena omogućuju preciznije kontrole i poboljšanja performansi za inicijalizaciju značajan aplikacije na prethodni postupak ApplicationInsights.config.
- DeveloperMode više neće automatski postaviti. Da biste promijenili ponašanje DeveloperMode morate navesti kod.
- Paket NuGet više umetati ga ApplicationInsights.config. Preporučujemo da koristite novi WindowsAppInitializer kada ručno dodavanje NuGet paketa.
- ApplicationInsights.config samo čita `<InstrumentationKey>`, sve ostale postavke ignorira ukupne preferenca WindowsAppInitializer postavke.
- Tržište pohrane bit će automatski prikupljene putem SDK.
- Veći broj popravci programskih pogrešaka, poboljšanja stabilnosti i poboljšanja performansi.

### <a name="core-sdk"></a>Temeljni SDK

- Datoteka ApplicationInsights.config više nije requiered. I nije dodana u paketu NuGet. Konfiguriranje možete potpuno naveden u kod.
- Paket NuGet više dodat će datoteke ciljnih web-mjesta u rješenje. Time se uklanja postavka automatskog DeveloperMode tijekom Sastavi za ispravljanje pogrešaka. U kodu treba ručno postaviti DeveloperMode.

## <a name="version-017"></a>Verzija od 0,17

### <a name="windows-app-sdk"></a>Windows SDK aplikacije

- Windows SDK aplikacije sada podržava univerzalni Windows aplikacija stvorila protiv Tehnički pretpregled sustava Windows 10 i s Dodavanje veze za VANJSKIH 2015 RC.

### <a name="core-sdk"></a>Temeljni SDK

- TelemetryClient po zadanom je pokrenuti s na InMemoryChannel.
- Novi API dodali, `TelemetryClient.Flush()`. Ta metoda pražnjenje će pokrenuti programa odmah blokiranja prijenos svih telemetrijskih prijavljen tog klijenta. Time se omogućuje ručno pokretanje prijenos prije zatvaranja proces.
- Paket NuGet dodaje .net 4,5 cilj. Ovaj cilj je bez vanjske ovisnosti (uklonjene BCL i EventSource ovisnosti).

## <a name="version-016"></a>Verzija 0,16 

Pretpregled 2015 04 28

- SDK sada podržava DNX ciljne platforme da biste omogućili nadzor [.NET Core framework](http://www.dotnetfoundation.org/NETCore5) aplikacija.
- Instance ```TelemetryClient``` više predmemoriju Instrumentation ključ. Ako nije instrumentation ključ postavite u ```TelemetryClient``` izričito ```InstrumentationKey``` će vratiti vrijednost null. Rješava problem prilikom postavljanja ```TelemetryConfiguration.Active.InstrumentationKey``` nakon neke telemetrijskih je već koji se prikupljaju, module za telemetriju kao ovisnost prikupljanje, web zahtjeva za prikupljanje i performanse mjerača prikupljanje podataka će koristiti novi ključ instrumentation.

## <a name="version-015"></a>Verzija 0,15

- Novo svojstvo ```Operation.SyntheticSource``` sad je dostupna na ```TelemetryContext```. Sada možete označiti telemetrijskih stavke "ne pravi korisnik promet" i navedite koliko je generirana tog prometa. Kao primjer postavljanjem ovo svojstvo promet može razlikovati od vaše Automatizacija test iz učitavanja test promet.
- Logika kanal je premještena u zasebnom NuGet pod nazivom Microsoft.ApplicationInsights.PersistenceChannel. Zadani kanal sada se zove InMemoryChannel
- Nova metoda ```TelemetryClient.Flush``` omogućuje sinkronizirano pražnjenje telemetrijskih stavki iz međuspremnika

## <a name="version-013"></a>Verzija 0,13

Nema napomene uz izdanje dostupna starije verzije. 
