<properties
    pageTitle="Napredna konfiguracija za radnje univerzalni aplikacije Windows SDK"
    description="Napredne mogućnosti konfiguracije radnje Mobile Azure s univerzalni aplikacijama za Windows"                    
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Napredna konfiguracija za radnje univerzalni aplikacije Windows SDK

> [AZURE.SELECTOR]
- [Univerzalni sustava Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Ovaj postupak objašnjava kako konfigurirati razne mogućnosti konfiguracije za Azure Mobile radnje sa sustavom Android aplikacije.

## <a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Napredna konfiguracija

### <a name="disable-automatic-crash-reporting"></a>Onemogućivanje automatskog rušenje izvješća

Možete onemogućiti automatsko rušenje izvješćivanja značajke radnje. Zatim, kada se pojavi neobrađenu iznimku, radnje nema nikakvog učinka.

> [AZURE.WARNING] Ako onemogućiti tu značajku, zatim kada se pojavi neupravljani rušenje u svojoj aplikaciji radnje ne šalje rušenje **i** zatvorite sesiju i zadacima.

Da biste onemogućili automatsko rušenje izvješćivanja, prilagodite konfiguraciju ovisno o način na koji je deklariran:

#### <a name="from-engagementconfigurationxml-file"></a>Iz `EngagementConfiguration.xml` datoteka

Postavite izvješće rušenje `false` između `<reportCrash>` i `</reportCrash>` oznake.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Iz `EngagementConfiguration` objekta u vrijeme izvođenja

Izvješće rušenje postavite na false pomoću EngagementConfiguration objekta.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Onemogućivanje izvještavanja o stvarnom vremenu

Prema zadanim postavkama, izvješća o usluzi radnje zapisnike u stvarnom vremenu. Ako aplikacija često izvješća zapisnika, bolje je međuspremnika zapisnike te ih odjednom izvješća na temelju pravilnim vremenskim. To se naziva "koji se Brzi niz i načinu".

Da biste to učinili, nazovite metoda:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument je vrijednost u **milisekundama**. Kad god želite ponovno aktivirati u stvarnom vremenu zapisivanje, nazovite metodu bez sve parametra ili vrijednost 0.

Način neprekidnog rada malo povećava trajanja baterija ali ima utjecaj na monitoru radnje: sve sesije i zadacima trajanje se zaokružuje praga neprekidnog rada (dakle, sesije i zadacima kraće nego praga neprekidnog rada možda neće biti vidljiva). Preporučujemo korištenje praga neprekidnog rada više od 30000 (30s). Spremljeni zapisnici ograničeni su na 300 stavki. Ako pošaljete je predugačak, može izgubiti neka zapisnika.

> [AZURE.WARNING] Prag neprekidnog rada ne može konfigurirati tijekom razdoblja manje od jedne sekunde. Ako to učinite, SDK prikazuje praćenje uz pogrešku i automatski ponovno postavlja zadanu vrijednost nula sekundi. Time se pokreće SDK da biste prijavili zapisnike u stvarnom vremenu.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
