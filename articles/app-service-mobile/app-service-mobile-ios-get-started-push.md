<properties
    pageTitle="Dodavanje automatske obavijesti da biste aplikaciji s Azure mobilne aplikacije za iOS"
    description="Saznajte kako koristiti Azure mobilne aplikacije za slanje automatskih obavijesti na aplikaciju iOS."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Dodavanje automatske obavijesti u aplikaciji za iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Pregled
U ovom ćete praktičnom vodiču dodate automatske obavijesti projekt [iOS Brzi start] tako da se automatske obavijesti poslati na uređaju svaki put kada se umetne zapis.

Ako ne koristite project server preuzete brzi početak rada, morate paket za nastavak automatske obavijesti. Dodatne informacije potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

U [iOS simulator ne podržava slanje obavijesti](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Potreban je uređaj fizički iOS i [članstvo u Program za razvojne inženjere za Apple](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Konfiguriranje obavijesti koncentratora

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Registracija aplikacije za slanje obavijesti

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfiguriranje Azure da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Ažuriranje pozadinskog da biste poslali automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Aplikaciju dodati automatske obavijesti

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Testiranje automatske obavijesti

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Dodatne informacije

* Predlošci omogućuju fleksibilnost slanje ih gura različite platforme i lokalizirane ih gura. [Upute za korištenje iOS klijentska biblioteka za Azure mobilne aplikacije](app-service-mobile-ios-how-to-use-client-library.md#templates) prikazuje kako registrirati predložaka.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[brzi početak rada sa sustavom iOS]: app-service-mobile-ios-get-started.md
