<properties
    pageTitle="Početak rada s Azure mobilne aplikacije za Xamarin.Android aplikacije"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti Azure mobilne aplikacije za razvoj Xamarin Android"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Stvaranje Xamarin.Android aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako dodati servise u oblaku pozadinskog Xamarin.Android aplikacije. Dodatne informacije potražite u članku [koje su mobilne aplikacije](app-service-mobile-value-prop.md).

Snimka zaslona u aplikaciji dovršene je ispod:

![][0]

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve aplikacije Mobile vodiče za Xamarin.Android aplikacije.

##<a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, potrebne su vam sljedeće preduvjete:

* Aktivni Azure račun. Ako nemate račun, registrirati za probnu programa Azure te pronađite do 10 besplatne aplikacije Mobile. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio sa Xamarin. Potražite u članku [Postavljanje i instalacije za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) upute.

>[AZURE.NOTE]Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](https://tryappservice.azure.com/?appServiceName=mobile).  Odmah možete stvoriti short-lived starter mobilnu aplikaciju u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="create-an-azure-mobile-app-backend"></a>Stvaranje pozadinskog sustava Azure mobilnu aplikaciju

Slijedite ove korake da biste stvorili pozadinskog za mobilnu aplikaciju.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sada ste dodjeli Azure mobilnu aplikaciju pozadinskog sustava koje je moguće koristiti mobilnu verziju klijentskog aplikacija. Nakon toga preuzmite project server za jednostavan "obveze popis" pozadinskog sustava i objavite Azure.

## <a name="configure-the-server-project"></a>Konfiguriranje server project

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Preuzmite i pokrenite aplikaciju Xamarin.Android

1. U odjeljku **preuzimanja i pokretanja Xamarin.Android projekta**, kliknite gumb za **Preuzimanje** .

    Spremite datoteku Komprimirana projekta s vašim lokalnim računalom i zabilježite koja je spremiti.

2. Omogućuje stvaranje projekta i pokrenite aplikaciju, pritisnite tipku **F5** .

3. U aplikaciji, upišite smislen tekst, kao što je _Dovršeno vodič_ , a zatim kliknite gumb **Dodaj** .

    ![][10]

    Podaci iz zahtjeva za umetnut je u tablici TodoItem. Vraća stavke koje se pohranjuju u tablici pozadinskog mobilne aplikacije, a podatke prikazuje na popisu.

    > [AZURE.NOTE] Možete pregledati kod koji se može pristupiti vašem pozadinskog mobilne aplikacije za slanje upita i umetanje podataka koji se nalazi u datoteci ToDoActivity.cs C#.

##<a name="next-steps"></a>Daljnji koraci

* [Izvanmrežna sinkronizacija dodati aplikacije](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Dodavanje provjere autentičnosti za aplikaciju](app-service-mobile-xamarin-android-get-started-users.md)
* [Aplikaciju za Xamarin.Android dodati automatske obavijesti](app-service-mobile-xamarin-android-get-started-push.md)
* [Kako koristiti upravljane klijent za Azure mobilne aplikacije](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
