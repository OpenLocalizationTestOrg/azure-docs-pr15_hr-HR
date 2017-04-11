<properties
    pageTitle="Dodavanje automatske obavijesti aplikacija za Android s Azure mobilne aplikacije"
    description="Saznajte kako koristiti Azure mobilne aplikacije za slanje automatskih obavijesti aplikacija za Android."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Dodavanje automatske obavijesti aplikacija za Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Pregled
U ovom ćete praktičnom vodiču dodajete automatske obavijesti projekta za [brzi početak rada sa sustavom Android] tako da se automatske obavijesti poslati na uređaju svaki put kada se umetne zapis.

Ako ne koristite project server preuzete brzi početak rada, morat ćete paket za nastavak automatske obavijesti. Dodatne informacije potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Preduvjeti

Potrebne su vam sljedeće:

* IDE ovisno o pozadinskog projekta:

    * [Android Studio](https://developer.android.com/sdk/index.html) ako je ta aplikacija Node.js pozadinskog.

    * [Visual Studio zajednice 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) ili noviji li to aplikacije sadrži .net pozadinskog.

* Android 2.3 ili noviji, Google spremište revizije 27 ili noviji i usluge Google Play 9.0.2 ili noviji za razmjenu poruka Firebase oblaka.

* Dovršite [brzi početak rada sa sustavom Android].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Stvaranje projekta koji podržava Firebase oblaka poruka

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfiguriranje obavijesti koncentratora

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfiguriranje Azure da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Omogućivanje automatske obavijesti za server project

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Automatske obavijesti dodati aplikacije

U ovom ćete odjeljku ažurirati klijent sa sustavom Android aplikacije za rukovanje automatske obavijesti.

### <a name="verify-android-sdk-version"></a>Provjerite je li verzija SDK sa sustavom Android

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Sljedeći je korak za instaliranje servisa Google Play. Google Cloud poruka sadrži neke minimalni API razine preduvjeti za razvoj i testiranje, koji se moraju slijediti svojstvo **minSdkVersion** u manifestu.

Ako želite testirati starije uređaju, pogledajte [Postavljanje gore Google reprodukcija Services SDK] da biste odredili kako najniža možete tu vrijednost postavite i postaviti je komponenta.

### <a name="add-google-play-services-to-the-project"></a>Dodajte Google Play s servisi u projekt

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Dodavanje koda

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testiranje aplikaciju protiv objavljene servis za mobilne uređaje

Aplikaciju možete testirati izravno Pridruživanjem telefon sa sustavom Android pomoću USB kabela ili pomoću virtualnog uređaja u na emulator.

## <a name="more"></a>Dodatne informacije

<!-- URLs -->
[Brzi početak rada sa sustavom android]: app-service-mobile-android-get-started.md

[Postavljanje servisa Google Play SDK]:https://developers.google.com/android/guides/setup
