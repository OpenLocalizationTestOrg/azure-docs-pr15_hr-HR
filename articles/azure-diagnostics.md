<properties
    pageTitle="Pregled Azure Dijagnostika | Microsoft Azure"
    description="Korištenje Azure Dijagnostika za ispravljanje pogrešaka, mjerenje performanse, nadzor, promet analize u servise u oblaku, virtualnih računala i servisa tkanina"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Što je Microsoft Azure Dijagnostika


Azure Dijagnostika je mogućnost unutar Azure koja omogućuje prikupljanje dijagnostičkih podataka na distribuiranih aplikacije. Proširenje Dijagnostika možete koristiti jedan od nekoliko različitih izvora. Trenutno podržava su Azure oblaka servisa Web i uloge suradnika, Azure virtualnim računalima sustava Microsoft Windows i tkanina servisa. Drugih servisa za Azure imati vlastite zasebnom Dijagnostika.

## <a name="data-you-can-collect"></a>Podatke možete prikupiti

Azure Dijagnostika možete prikupiti sljedeće vrste podataka:

Izvor podataka|Opis
---|---
Mjerača performansi | Operacijski sustav i mjerača prilagođene performansi
Zapisnici aplikacije     | Praćenje poruka napisao aplikacije
Zapisnike događaja sustava Windows   | Podaci koji se šalju zapisivanje događaja sustava
.NET izvor događaja    | Kod zapisivanje događaja pomoću.NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) klase
IIS zapisnika             | Informacije o IIS web-mjesta
Manifest temelji ETW   | Događaj praćenje za Windows događaje koje generira izvođenja procesa
Ispisi rušenje          | Informacije o stanju procesa u slučaju rušenje programa aplikacije
Prilagođeni zapisivanje    | Zapisnici stvorio program ili servis
Infrastruktura za Azure dijagnostičkih zapisnika|Informacije o Dijagnostika sam

Proširenje Azure Dijagnostika možete prenijeti podatke na račun za Azure prostora za pohranu ili poslali je na servise kao što su [Aplikacije uvide](./application-insights/app-insights-cloudservices.md). Podatke možete koristiti za ispravljanje pogrešaka i otklanjanje poteškoća, mjerenje performanse, nadzor korištenja resursa, promet analizu i planiranja kapaciteta i nadzor.


## <a name="versioning"></a>Rad s verzijama
U odjeljku [Povijest verzija Azure Dijagnostika](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Daljnji koraci
Odaberite uslugu koju pokušavate dohvatite Dijagnostika na i koristite u sljedećim člancima za početak rada. Koristite veze Općenito Azure Dijagnostika za referencu za određene zadatke.

## <a name="web-apps"></a>Web-aplikacije
Imajte na umu da web-aplikacije pomoću dijagnostike Azure. Pronalaženje ekvivalentan informacije na [Web-aplikacije](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Korištenjem dijagnostike Azure servise u oblaku
- Ako koristite Visual Studio, potražite u članku [Korištenje programa Visual Studio praćenje aplikacija za servise u Oblaku](./vs-azure-tools-debug-cloud-services-virtual-machines.md) za početak rada. U suprotnom, potražite u članku
- [Upute za praćenje pomoću dijagnostike Azure servise u Oblaku](./cloud-services/cloud-services-how-to-monitor.md)
- [Postavljanje Azure Dijagnostika u aplikaciji za usluge oblaka](./cloud-services/cloud-services-dotnet-diagnostics.md)

Dodatne teme, potražite u članku

- [Pomoću aplikacije uvida Azure Dijagnostika za servise u Oblaku](./application-insights/app-insights-cloudservices.md)
- [Praćenje tijeka servise u Oblaku aplikacijom Dijagnostika Azure](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [Korištenje ljuske PowerShell za postavljanje Dijagnostika na servise u Oblaku](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Virtualnim strojevima korištenjem dijagnostike Azure
- Ako koristite Visual Studio, potražite u članku [Korištenje Visual Studio pratiti virtualnim računalima sustava Azure](./vs-azure-tools-debug-cloud-services-virtual-machines.md) za početak rada. U suprotnom, potražite u članku
- [Postavljanje Azure Dijagnostika na programa Azure virtualnog računala](./virtual-machines-dotnet-diagnostics.md)

Dodatne teme, potražite u članku

- [Da biste postavili Dijagnostika virtualnim računalima sustava na Azure pomoću komponente PowerShell](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Stvaranje Windows virtualnog računala nadzora i Dijagnostika pomoću predloška Azure Voditelj resursa](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Servis tkanina korištenjem dijagnostike Azure
Početak rada na [monitoru tkanina servisa aplikacija](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Mnoge druge članke Dijagnostika tkanina usluge dostupne su u navigacijskog stabla na lijevoj strani kada dođete do ovog članka.

## <a name="general-azure-diagnostics-articles"></a>Općenito Azure Dijagnostika članci
- [Konfiguracije sheme Dijagnostika azure](https://msdn.microsoft.com/library/azure/mt634524.aspx) – upute za promjenu datoteke sheme za prikupljanje i usmjeravanje Dijagnostika podataka. Imajte na umu da Visual Studio možete koristiti da biste promijenili datoteke sheme.
- [Podaci se pohranjuju u prostor za pohranu Azure kako Azure Dijagnostika](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) – poznati nazive tablica i blob-ova u kojima se piše dijagnostičkih podataka.
- Saznajte kako [koristiti mjerača performansi u Dijagnostika Azure](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Informirajte se o [rute Azure Dijagnostika aplikacije uvid u](./azure-diagnostics-configure-applicationinsights.md)
- Ako imate poteškoća s Dijagnostika početni ili pronalaženje podataka u tablicama Azure prostora za pohranu, potražite u članku [Otklanjanje poteškoća dijagnostike Azure](./azure-diagnostics-troubleshooting.md)
