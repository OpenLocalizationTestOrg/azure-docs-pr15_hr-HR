<properties
    pageTitle="Stvaranje Cordova aplikacija na mobilne aplikacije aplikacije servisa za Azure | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste započeli s korištenjem pozadinski sustav Azure mobilne aplikacije za razvoj Apache Cordova"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, javascript, mobilnog, klijenta" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Stvaranje programa Apache Cordova aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako dodati servise u oblaku pozadinskog Apache Cordova mobilne aplikacije pomoću programa pozadinskog Azure mobilne aplikacije.  Morate stvoriti novi pozadinskog mobilne aplikacije i jednostavan _popis obveza popis_ aplikacije Apache Cordova kojoj su pohranjeni podaci aplikacije u Azure.

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve Apache Cordova vodiče o korištenju značajke mobilne aplikacije u servisu Azure aplikacije.

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

* Na Računalu s [Visual Studio zajednice 2015] ili novija verzija.
* [Visual Studio Tools for Apache Cordova].
* [Račun za Azure active](https://azure.microsoft.com/pricing/free-trial/).

Možete zaobići Visual Studio i izravno koristiti Apache Cordova naredbenog retka.  To je korisno kada dovršavanje vodič na Mac računalu.  Pomoću ovog praktičnog vodiča ne pokriva Kompiliranje Apache Cordova klijentske aplikacije pomoću naredbenog retka.

## <a name="create-a-new-azure-mobile-app-backend"></a>Stvaranje nove pozadinske Azure aplikacije za mobilne uređaje

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Pogledajte videozapis koji prikazuje slične korake](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Konfiguriranje server project

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Preuzmite i pokrenite aplikaciju Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Daljnji koraci

Sada kada se završi ovog praktičnog vodiča za brzi početak rada, prijeđite na jedan od sljedećih vodiči za:

* [Dodavanje provjere autentičnosti] na Apache Cordova aplikacije.
* [Dodavanje automatske obavijesti] na Apache Cordova aplikacije.

Saznajte više o koncepata sa servisom Azure aplikacije.

* [Provjera autentičnosti]
* [Automatske obavijesti]

Saznajte kako koristiti u SDK-ovi.

* [Apache Cordova SDK]
* [Poslužitelj ASP.NET SDK]
* [Poslužitelj Node.js SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio zajednice 2015.]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Dodavanje provjere autentičnosti]: app-service-mobile-cordova-get-started-users.md
[Dodavanje automatskih obavijesti]: app-service-mobile-cordova-get-started-push.md
[Provjera autentičnosti]: app-service-mobile-auth.md
[Automatske obavijesti]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Poslužitelj ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Poslužitelj Node.js SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
