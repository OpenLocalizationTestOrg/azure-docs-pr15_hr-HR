<properties
    pageTitle="Stvaranje na univerzalni Windows platformu (UWP) koji koristi na mobilne aplikacije | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste započeli s korištenjem pozadinski sustav Azure mobilne aplikacije za razvoj aplikacija univerzalni platforme Windows (UWP) u C#, Visual Basic i JavaScript."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Stvaranje aplikacije za Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako dodati servise u oblaku pozadinskog aplikaciju za univerzalni platforme Windows (UWP). Dodatne informacije potražite u članku [koje su mobilne aplikacije](app-service-mobile-value-prop.md). Slijede snimki zaslona u aplikaciji dovršeni:

![Dovršeni aplikacija za stolna računala](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Izvodi se na radnoj površini. 

![Dovršeni mobilnoj aplikaciji](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Pokretanje na telefonu

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve mobilnu aplikaciju vodiče za UWP aplikacije. 

##<a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

* Aktivni Azure račun. Ako nemate račun, možete je registrirati za probna verzija platforme Azure i dobiti do 10 besplatne mobilne aplikacije koje možete nastaviti koristiti i nakon vaše probno razdoblje završava. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio zajednice 2015] ili noviju verziju.

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](https://tryappservice.azure.com/?appServiceName=mobile). Postoji, možete odmah stvoriti short-lived starter aplikacije za mobilne uređaje u aplikacije servisa za – bez kreditne kartice potrebna, a ne preuzete obveze.

##<a name="create-a-new-azure-mobile-app-backend"></a>Stvaranje nove pozadinske Azure mobilnu aplikaciju

Slijedite ove korake da biste stvorili novi pozadinskog za mobilnu aplikaciju.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sada ste dodjeli Azure mobilnu aplikaciju pozadinskog sustava koje je moguće koristiti mobilnu verziju klijentskog aplikacija. Nakon toga će preuzimati project server za jednostavan "obveze popis" pozadinskog sustava i objavite Azure.

## <a name="configure-the-server-project"></a>Konfiguriranje server project

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Preuzmite i pokrenite projekt klijenta

Nakon što ste konfigurirali u pozadinskom mobilnu aplikaciju, možete stvoriti novu aplikaciju klijenta ili izmjena postojeće aplikacije za povezivanje s Azure. U ovom ćete odjeljku preuzimanje predloška projekta aplikacije UWP koja je prilagođena povezati svoje pozadinskog mobilnu aplikaciju.

1. Vratite se u plohu **brzi početak rada** za vaše pozadinskog mobilnu aplikaciju, kliknite **Stvori novu aplikaciju** > **preuzeti**, a zatim izdvojiti datoteke komprimirane projekta s vašim lokalnim računalom.

    ![Preuzmite Windows brzi početak rada projekta](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Neobavezno) Dodavanje aplikacije project UWP rješenje isti kao server project. To olakšava ispravljanje pogrešaka i testiranje aplikaciju i pozadinski istim rješenjem Visual Studio, ako odaberete da biste to učinili. Da biste dodali aplikaciju projekta UWP rješenje, morate koristiti Visual Studio 2015 ili noviju verziju.

4. Uz aplikaciju UWP kao projekt pri pokretanju pritisnite tipku F5 implementacije i pokretanja aplikacije.

5. U aplikaciji upišite smislen tekst, kao što je *Dovršeno vodič*, u tekstni okvir **umetnuti u TodoItem** , a zatim kliknite **Spremi**.

    ![Dovršavanje računala brzi početak rada sa sustavom Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    To nove pozadinske mobilne aplikacije koje se nalazi u Azure šalje zahtjev za objavu.

6. (Neobavezno) Zaustavite aplikaciju i ponovno ga pokrenite na drugi uređaj ili mobilnom emulator.

    ![Cijelom telefonskom brzi početak rada za Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Obratite pozornost na to da su podaci spremljeni u prethodnom koraku učita iz Azure nakon pokretanja aplikacije UWP. 

##<a name="next-steps"></a>Daljnji koraci

* [Dodavanje provjere autentičnosti za aplikaciju](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Upute za provjeru autentičnosti korisnika aplikacije s davateljem identiteta.

* [Automatske obavijesti dodati aplikacije](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Saznajte kako dodati automatske obavijesti podržava aplikacije i konfigurirati mobilnu aplikaciju pozadinskog sustava da biste koristili koncentratora Azure obavijesti da biste poslali automatske obavijesti.

* [Omogućivanje izvanmrežne sinkronizacije za aplikaciju](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Saznajte kako dodati aplikacije pomoću programa pozadinskog mobilnu aplikaciju Izvanmrežna podrška. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio zajednice 2015.]: https://go.microsoft.com/fwLink/p/?LinkID=534203
