<properties
    pageTitle="Dodavanje provjere autentičnosti na Android s aplikacijama Mobile | Aplikacije servisa za Azure"
    description="Saznajte kako pomoću mobilne aplikacije u Azure aplikacije servisa za provjeru autentičnosti korisnika aplikacijom Android kroz mnoštvo davatelji identiteta, uključujući Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Dodavanje provjere autentičnosti aplikacija za Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Sažetak

U ovom ćete praktičnom vodiču dodajte provjere autentičnosti todolist makronaredbi brzi početak rada na Android pomoću davatelja identiteta podržane. Pomoću ovog praktičnog vodiča temelji se na vodič [Početak rada s aplikacijama Mobile] , koja se najprije morate dovršiti.

##<a name="register"></a>Registracija aplikacije za provjeru autentičnosti i konfiguriranje servisa za aplikaciju

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograničavanje dozvola korisnicima čija je autentičnost provjerena

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Android Studio otvorite predviđene što dovršili projekta s vodič [Početak rada s mobilne aplikacije]. Na izborniku za **pokretanje** kliknite **Pokreni aplikaciju** , a zatim provjerite je li se potencira neobrađenu iznimku s Šifra stanja 401 (Unauthorized) nakon pokretanja aplikacije.

     Ova se iznimka događa jer aplikacija pokušava pristupiti pozadinskog kao Neprovjereni korisnika, ali _TodoItem_ tablica sada zahtijeva provjeru autentičnosti.

Nakon toga ažurirati aplikaciju za provjeru autentičnosti korisnika prije traži resursa iz pozadine za mobilnu aplikaciju.

## <a name="add-authentication-to-the-app"></a>Dodavanje provjere autentičnosti za aplikaciju

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Tokeni za provjeru autentičnosti predmemorije na klijentskom računalu

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Daljnji koraci

Sad kad Završi ovog praktičnog vodiča osnovnu provjeru autentičnosti, razmotrite nastavka na jedan od sljedećih vodiči za:

+ [Dodavanje automatske obavijesti da biste aplikacija za Android](app-service-mobile-android-get-started-push.md) Saznajte kako konfigurirati mobilnu aplikaciju pozadinskog sustava da biste koristili koncentratora Azure obavijesti da biste poslali automatske obavijesti.

+ [Omogućivanje izvanmrežne sinkronizacije za aplikacija za Android](app-service-mobile-android-get-started-offline-data.md) Saznajte kako dodati aplikacije pomoću mobilne aplikacije pozadinskog Izvanmrežna podrška. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Početak rada s aplikacijama Mobile]: app-service-mobile-android-get-started.md
