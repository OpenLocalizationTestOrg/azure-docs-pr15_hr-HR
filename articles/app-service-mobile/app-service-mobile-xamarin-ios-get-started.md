<properties
    pageTitle="Početak rada s Azure servisa mobilna aplikacija za aplikacije Xamarin.iOS | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič za početak rada s pomoću mobilne aplikacije za razvoj Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Stvaranje Xamarin.iOS aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako dodati servise u oblaku pozadinskog Xamarin.iOS mobilne aplikacije pomoću programa pozadinskog Azure mobilne aplikacije.  Stvorite novi pozadinskog mobilne aplikacije i jednostavan _popis obveza popis_ aplikacije Xamarin.iOS kojoj su pohranjeni podaci aplikacije u Azure.

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve Xamarin.iOS vodiče o korištenju značajke mobilne aplikacije u servisu Azure aplikacije.

##<a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, potrebne su vam sljedeće preduvjete:

* Aktivni Azure račun. Ako nemate račun, prijavite se za Azure probnu verziju i dobiti do 10 besplatne mobilne aplikacije koje možete nastaviti koristiti i nakon vaše probno razdoblje završava. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio sa Xamarin. Potražite u članku [Postavljanje i instalacije za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) upute.

* Mac s Xcode v7.0 ili noviji i Xamarin Studio zajednice instaliran. Potražite u članku [Postavljanje i instalacije za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) i [Postavljanje, instalacija, i verifications za korisnike Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](https://tryappservice.azure.com/?appServiceName=mobile). Odmah možete stvoriti short-lived starter aplikacije za mobilne uređaje u aplikacije servisa za – bez kreditne kartice potrebna, a ne preuzete obveze.

## <a name="create-an-azure-mobile-app-backend"></a>Stvaranje pozadinskog sustava Azure mobilnu aplikaciju

Slijedite ove korake da biste stvorili pozadinskog za mobilnu aplikaciju.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfiguriranje server project

Sada ste dodjeli Azure mobilnu aplikaciju pozadinskog sustava koje je moguće koristiti mobilnu verziju klijentskog aplikacija. Nakon toga preuzmite project server za jednostavan "obveze popis" pozadinskog sustava i objavite Azure.

Poduzmite sljedeće korake da biste konfigurirali project server da biste koristili Node.js ili .NET pozadinskog.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Preuzmite i pokrenite aplikaciju Xamarin.iOS

1. Otvorite [portal za Azure] u prozoru preglednika.

2. Na plohu postavke za mobilnu aplikaciju, kliknite **Početak rada** > **Xamarin.iOS**. U odjeljku korak 3, kliknite **Stvori novu aplikaciju** ako već nije odabrano.  Zatim kliknite gumb **Preuzmi** .

    Klijentska aplikacija koja se povezuje s vaš mobilni pozadinskog preuzimaju se. Spremite datoteku Komprimirana projekta s vašim lokalnim računalom i zabilježite koja je spremiti.

3. Izdvajanje projekta koji ste preuzeli, a zatim je otvorite u Xamarin Studio (ili Visual Studio).

    ![][9]

    ![][8]

4. Omogućuje stvaranje projekta i pokretanje aplikacije u iPhone emulator, pritisnite tipku F5.

5. U aplikaciji upišite smislen tekst, kao što su _Dodatne Xamarin_, a zatim kliknite u **+** gumb.

    ![][10]

    Podaci iz zahtjeva za umetnut je u tablici TodoItem. Vraća stavke koje se pohranjuju u tablici pozadinskog mobilne aplikacije, a podaci se prikazuju na popisu.

>[AZURE.NOTE]Možete pregledati kod koji se može pristupiti vašem pozadinskog mobilne aplikacije za slanje upita i umetanje radnog lista u datoteci QSTodoService.cs C#.

##<a name="next-steps"></a>Daljnji koraci

* [Izvanmrežna sinkronizacija dodati aplikacije](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Dodavanje provjere autentičnosti za aplikaciju](app-service-mobile-xamarin-ios-get-started-users.md)
* [Aplikaciju za Xamarin.Android dodati automatske obavijesti](app-service-mobile-xamarin-ios-get-started-push.md)
* [Kako koristiti upravljane klijent za Azure mobilne aplikacije](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Portal za Azure]: https://portal.azure.com/
