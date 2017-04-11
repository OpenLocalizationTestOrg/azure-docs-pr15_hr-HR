<properties
    pageTitle="Aplikaciju za Xamarin.Android dodati automatske obavijesti | Aplikacije servisa za Azure"
    description="Saznajte kako koristiti aplikacije servisa za Azure i koncentratora Azure obavijesti da biste poslali automatske obavijesti Xamarin.Android aplikacije"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Aplikaciju za Xamarin.Android dodati automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Pregled


U ovom ćete praktičnom vodiču dodate automatske obavijesti projekt [Xamarin.Android Brzi start](app-service-mobile-windows-store-dotnet-get-started.md) tako da se automatske obavijesti poslati na uređaju svaki put kada se umetne zapis.

Ako ne koristite project server preuzete brzi početak rada, morate paket za nastavak automatske obavijesti. Dodatne informacije potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .


##<a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ Aktivni Google račun. Možete se prijaviti za Google račun pri [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ [Google Cloud poruka klijentske komponente](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Konfiguriranje obavijesti koncentratora

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Omogućivanje Firebase Cloud razmjene poruka

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Konfiguriranje Azure slanje zahtjeva za slanje

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Ažuriranje project server da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Konfiguriranje klijenta projekta za slanje obavijesti

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Dodavanje koda za automatsko prosljeđivanje obavijesti aplikacije

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Testiranje automatske obavijesti u aplikaciji

Aplikaciju možete testirati i pomoću virtualnog uređaja u na emulator. Postoje dodatni koraci konfiguriranja potrebna kada se pokrene s programa emulator.

1. Provjerite je li se za implementaciju u ili ispravljanje pogrešaka na virtualnog uređaja koji je Google API-ji Postavi kao cilj, kao što je prikazano u nastavku u upravitelju Android virtualne uređaja (AVD).

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Dodajte Google račun Android uređaj tako da kliknete **aplikacije** > **Postavke** > **Dodavanje računa**, slijedite upute.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Pokrenite aplikaciju todolist kao prije i umetnite novu stavku obveze. Ovaj put u području obavijesti prikazuje se ikona obavijesti. Možete otvoriti ladica obavijesti da biste prikazali cijeli tekst obavijesti.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
