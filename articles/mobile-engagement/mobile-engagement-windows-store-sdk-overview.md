<properties
    pageTitle="Integracija univerzalni SDK za Windows"
    description="Windows univerzalni integracije SDK za Azure mobilne radnje"                                     
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Integracija s programom Windows univerzalni SDK za Azure mobilne radnje

Ovaj dokument opisuju sve integracije i konfiguracija mogućnosti dostupne za Azure Mobile radnje Windows univerzalni SDK.

## <a name="prerequisites"></a>Preduvjeti

Prije pokretanja ovog praktičnog vodiča, najprije morate dovršiti naš [15-minutni vodič](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Napredne značajke

### <a name="reporting-features"></a>Značajke izvješćivanja
Možete dodati te značajke:

1. [Dodatne mogućnosti izvješćivanja](mobile-engagement-windows-store-advanced-reporting.md)

2. [Konfiguriranje Napredne mogućnosti](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Obavijesti

[U aplikaciji Windows univerzalni integraciji razgovor (obavijesti)](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Oznaka Planiranje implementacije:

[Kako koristiti dodatne radnje Mobile označavanje API-JA u aplikaciji Windows univerzalni](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Napomene

###<a name="340-04192016"></a>3.4.0 (19/04/2016)

-   Dosegne prekriveno poboljšanja.
-   Zapisnici konzole dodane "TestLogLevel" API-JA za omogućivanje i onemogućivanje/filtar čuje po SDK-a.
-   Pokrenite FIXED u aktivnosti obavijesti ciljanja prvi aktivnosti ne prikazuju u aplikaciju.

Starijim verzijama potražite u članku [Dovršeno napomene](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Nadogradnja postupaka

Ako već imate integrirali stariju verziju radnje u aplikaciji, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Propušteni nekoliko verzija SDK, možda ćete morati slijedite nekoliko postupaka. Potražite u članku dovršavanje [Nadogradnje postupke](mobile-engagement-windows-store-upgrade-procedure.md). Ako, primjerice migriranja iz 0.10.1 u 0.11.0 morate najprije slijedite postupak "iz 0.9.0 za 0.10.1" pa postupak "iz 0.10.1 za 0.11.0".

###<a name="from-330-to-340"></a>Iz 3.3.0 za 3.4.0

####<a name="test-logs"></a>Testiranje zapisnika

Zapisnici konzole za koje je stvorio SDK sada može biti omogućeno/onemogućeno/filtriranja. Da biste prilagodili, ažurirali svojstvo `EngagementAgent.Instance.TestLogEnabled` u neku od dostupnih iz vrijednosti u `EngagementTestLogLevel` Enumeracije, na primjer:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Resursi

Poboljšano je prekriveno razgovor. Nije dio paketa resursa SDK NuGet.

Prilikom nadogradnje na novu verziju SDK-a, možete odabrati želite li zadržati postojeće datoteke iz mape prekriveno resursa ili ne:

* Ako prethodna prekriveno radi za vas ili su integriranje u `WebView` elemenata ručno, pa možete odlučiti želite zadržati svoje izađite iz datoteke, on će i dalje funkcionirati.
* Da biste ažurirali u novi preklapanje, zamijenite cjeline `overlay` mape iz resursa s novom iz paketa SDK (UWP aplikacije: nakon nadogradnje, možete dobiti u novu mapu prekriveno % korisničkog PROFILA\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Pomoću nove prekriveno prebrisat će sve prilagodbe na prethodne verzije.

### <a name="upgrade-from-older-versions"></a>Nadogradnja iz starije verzije

Potražite u članku [Nadogradnja postupaka](mobile-engagement-windows-store-upgrade-procedure.md)
