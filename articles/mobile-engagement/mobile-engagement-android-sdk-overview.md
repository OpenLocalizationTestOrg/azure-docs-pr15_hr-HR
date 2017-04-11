<properties
    pageTitle="Integracija sa sustavom android SDK za Azure mobilne radnje"
    description="U članku se opisuje kako integrirati Azure Mobile radnje SDK u aplikacijama za Android"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Integracija sa sustavom android SDK za Azure mobilne radnje

> [AZURE.SELECTOR]
- [Univerzalni sustava Windows](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS](mobile-engagement-ios-sdk-overview.md)
- [Android](mobile-engagement-android-sdk-overview.md)

Ovaj dokument opisuju sve integracije i konfiguracija mogućnosti dostupne za Azure Mobile radnje sa sustavom Android SDK.

## <a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Napredne značajke

### <a name="reporting-features"></a>Značajke izvješćivanja

Možete dodati te značajke:

1. [Dodatne mogućnosti izvješćivanja](mobile-engagement-android-advanced-reporting.md)
2. [Mogućnosti izvješćivanja lokacije](mobile-engagement-android-location-reporting.md)
3. [Dodatne mogućnosti za konfiguraciju](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Obavijesti:
[Kako integrirati razgovor (obavijesti) u aplikacija za Android](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud razmjenu poruka (GCM): [integraciji GCM s mobilnim radnje](mobile-engagement-android-gcm-integrate.md)

2. Amazon uređaj Messaging (Admin): [integraciji Admin s mobilnim radnje](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Oznaka Planiranje implementacije:
[Kako koristiti dodatne radnje Mobile označavanje API-JA u aplikacija za Android](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Napomene

### <a name="423-08102016"></a>4.2.3 (08/10/2016)

 - Nema više lock Wi-Fi veze.
 - Riješite je zastoja zbog prilikom pozivanja getDeviceId prije init (pogrešku u 4.2.0).

Sve verzije potražite u članku [Dovršeno napomene](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Nadogradnja postupaka

Ako već imate integrirali stariju verziju naš SDK u aplikaciji, obratite se [Postupci za nadogradnju](mobile-engagement-android-upgrade-procedure.md).
