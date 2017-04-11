<properties
    pageTitle="Dodavanje provjere autentičnosti na iOS s aplikacijama Mobile Azure"
    description="Saznajte kako koristiti Azure mobilne aplikacije za provjeru autentičnosti korisnika aplikaciju za iOS kroz mnoštvo davatelji identiteta, uključujući AAD, Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Dodavanje provjere autentičnosti za aplikaciju za iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

U ovom ćete praktičnom vodiču dodajte provjere autentičnosti [iOS Brzi start] projektu pomoću davatelja identiteta podržane. Pomoću ovog praktičnog vodiča temelji se na vodič [iOS Brzi start] , koji se najprije morate dovršiti.

##<a name="register"></a>Registracija aplikacije za provjeru autentičnosti i konfiguriranje servisa za aplikaciju

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograničavanje dozvola korisnicima čija je autentičnost provjerena

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

U Xcode, pritisnite **Pokreni** da biste pokrenuli aplikaciju. Iznimke se zaokružuje jer aplikacija pokušava pristupiti pozadinskog kao Neprovjereni korisnika, ali _TodoItem_ tablica sada zahtijeva provjeru autentičnosti.

##<a name="add-authentication"></a>Dodavanje provjere autentičnosti za aplikaciju

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[brzi početak rada sa sustavom iOS]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
