<properties
    pageTitle="Omogućivanje izvanmrežne sinkronizacije za aplikaciju univerzalni platforme Windows (UWP) pomoću mobilne aplikacije | Aplikacije servisa za Azure"
    description="Saznajte kako pomoću mobilne aplikacije Azure predmemorije i sinkroniziranje podataka izvanmrežno u svojoj aplikaciji univerzalni platforme Windows (UWP)."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Omogućivanje izvanmrežne sinkronizacije za aplikacija za Windows

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako dodati Izvanmrežna podrška univerzalni platforme Windows (UWP) aplikaciju putem programa pozadinskog Azure mobilnu aplikaciju. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije – prikaz, dodavanje ili izmjenu podataka – čak i kad je nema mrežne veze. Promjene se spremaju u lokalnu bazu podataka. Kada se vratite u mrežni način je uređaj, te promjene se sinkroniziraju s udaljenog pozadinskom.

U ovom ćete praktičnom vodiču ažurirati aplikaciju projekt UWP iz vodiča [Stvaranje aplikacija u sustavu Windows] da biste podržava izvanmrežno značajke Azure mobilne aplikacije. Ako ne koristite project server preuzete brzi početak rada, morate dodati paketa za nastavak podataka programa access u projekt. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Dodatne informacije o značajci izvanmrežnu sinkronizaciju potražite u temi [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="requirements"></a>Preduvjeti

Pomoću ovog praktičnog vodiča Traži sljedeće stara requisites učinite sljedeće:

* Visual Studio 2013 sa sustavom sustavu Windows 8.1 ili novijim.
* Nakon dovršetka [Stvaranje aplikacija u sustavu Windows][Stvaranje aplikacija u sustavu windows].
* [Azure mobilne usluge SQLite spremišta][sqlite store nuget]
* [SQLite za razvoj univerzalni platformu sustava Windows](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Ažurira aplikaciju za klijenta za podršku značajki izvanmrežno

Azure značajke izvanmrežne mobilnu aplikaciju omogućuju omogućuje interakciju s lokalnom bazom podataka kada se nalazite u izvanmrežni scenarij. Da biste koristili te značajke u aplikaciji, pokretanje [SyncContext] [ synccontext] u lokalnom spremištu. Zatim pozvati tablice putem sučelja[IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite koristi se kao lokalno spremište na uređaju.

1. Instalirajte [SQLite runtime za univerzalni platformu sustava Windows](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. U Visual Studio, otvorite upravitelj NuGet paketa za projekt aplikacije UWP završio u vodiču za [Stvaranje aplikacija u sustavu Windows] .
    Traženje i instalirajte paket NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

4. U pregledniku rješenja, desnom tipkom miša kliknite **reference** > **Dodavanje referenca...**  >  **Univerzalni Windows** > **proširenja**, zatim omogućiti **SQLite za univerzalni platformu sustava Windows** i **Visual C++ izvođenja 2015 za aplikacije univerzalni platformu sustava Windows**.

    ![Dodavanje SQLite UWP reference][1]

5. Otvorite datoteku MainPage.xaml.cs i uklonite na `#define OFFLINE_SYNC_ENABLED` definicija.

6. U Visual Studio, pritisnite tipku **F5** da biste ponovno stvaranje i pokretanje aplikacija klijenta. Aplikaciju funkcionira na isti jednak onome prije omogućeno izvanmrežnu sinkronizaciju. Međutim, lokalnu bazu podataka sada unose s podacima koji se može koristiti u izvanmrežni scenarij.

## <a name="update-sync"></a>Ažuriranje aplikacije da biste prekinuli pozadinski

U ovom ćete odjeljku prekinuti vezu za mobilnu aplikaciju pozadinskog sustava u programu Publisher izvanmrežne situacija. Kada dodate stavke podataka, vaše rukovatelj iznimku obavijestit će vas aplikacija je u izvanmrežnom načinu rada. U ovom stanju nove stavke dodane u lokalnom spremištu i će se sinkronizirati s pozadinskom mobilne aplikacije automatske sljedeće pokrenete u povezanim stanju.

1. Uređivanje App.xaml.cs zajedničke projekta. Komentar izvan inicijalizaciju **MobileServiceClient** i dodajte sljedeći redak koji koristi URL koji nisu valjani mobilne aplikacije:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Možete demonstrirati izvanmrežnog funkcioniranja onemogućivanjem Wi-Fi veze i mobilne mreže na uređaju ili korištenje aviona načina.

2. Pritisnite **F5** omogućuje stvaranje i pokretanje aplikacija. Obratite pozornost na to sinkronizacije nije uspjela prilikom osvježavanja prilikom pokretanja aplikacije.

3. Unesite nove stavke, a zatim obratite pozornost na to da automatske ne uspije čiji je [CancelledByNetworkError] status svaki put kada kliknete **Spremi**. Međutim, nove stavke obveze postoji u lokalnom spremištu dok se ne završi pozadinskog mobilne aplikacije.  U aplikaciji radnog ako izostavi te iznimke aplikaciju klijent se ponaša kao da je i dalje povezan s pozadinskom mobilne aplikacije.

4. Zatvorite aplikaciju i ponovno ga da biste potvrdili da se nove stavke koje ste stvorili dosljedan u lokalnom spremištu.

5. (Neobavezno) U Visual Studio, otvorite **Eksplorer za poslužitelj**. Otvorite bazu podataka u **Azure**->**Baze podataka SQL**. Desnom tipkom miša kliknite bazu podataka, a zatim odaberite **Otvori u programu Explorer sustava SQL Server objekt**. Sada možete pregledavati tablice baze podataka SQL i njezin sadržaj. Provjerite je li se nije promijenio podatke u pozadinskom bazom podataka.

6. (Neobavezno) Pomoću alata za OSTALE kao što su Fiddler ili Postman poslati upit mobilne pozadinskog, korištenje upita za DOHVAĆANJE u obrascu `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Ažuriranje aplikacije povezati svoje pozadinskog mobilnu aplikaciju

U ovom ćete odjeljku se ponovno povežete aplikaciju da biste pozadinskog mobilne aplikacije. Te promjene kao zamjenu za ponovno povezivanje s mrežom na aplikaciju.

Kada prvi put pokrenete aplikaciju, u `OnNavigatedTo` pozive rukovatelja događajima `InitLocalStoreAsync`. Ta metoda naizmjence poziva `SyncAsync` da biste sinkronizirali svoje lokalno spremište s pozadinskom bazom podataka. Aplikacija pokušava sinkronizirati prilikom pokretanja.

1. Otvorite App.xaml.cs zajedničke projektu, a zatim uklonite vaše prethodne Inicijalizacija `MobileServiceClient` da biste koristili ispravno URL mobilne aplikacije.

2. Da biste ponovno stvaranje i pokretanje aplikacija, pritisnite tipku **F5** . Aplikaciju sinkronizira lokalne promjene s pozadinskom Azure mobilnu aplikaciju pomoću automatske i povlačite operacija kada na `OnNavigatedTo` izvršava rukovatelja događajima.

3. (Neobavezno) Prikaz ažurirane podatke pomoću programa Explorer objekt za SQL Server ili alat za OSTALE kao što je Fiddler. Obavijest o podacima je sinkronizirana između Azure mobilnu aplikaciju pozadinskom bazom podataka i u lokalnom spremištu.

4. U aplikaciji, kliknite potvrdni okvir pokraj nekoliko stavki za popunjavanje u lokalnom spremištu.

  `UpdateCheckedTodoItem`pozivi `SyncAsync` dovršena je sinkronizacija svaku stavku s pozadinskom mobilnu aplikaciju. `SyncAsync`poziva automatske i povlačite. Međutim, **kad god se izvoditi povlačite prema tablicu koja klijent je promijenio, na automatske se uvijek izvršava automatski**. Takvo ponašanje osigurava sve tablice u lokalnom spremištu zajedno s odnosima ostaju isti. Taj se problem može uzrokovati neočekivane pribadače.  Dodatne informacije o taj problem, potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].


##<a name="api-summary"></a>Sažetak API-JA

Podržava značajke izvanmrežne mobilnih usluga ćemo koristi [IMobileServiceSyncTable] sučelja i pokrenuti [MobileServiceClient.SyncContext] [ synccontext] s lokalnom bazom podataka SQLite. Tijekom izvanmrežnog rada, normalno CRUD postupci za mobilne aplikacije funkcioniraju kao aplikaciju i dalje povezani tijekom operacije pojaviti protiv lokalne pohrane. Na sljedeće načine koriste se za sinkronizaciju u lokalnom spremištu s poslužiteljem:

*  **[PushAsync]** Budući da ova metoda član [IMobileServicesSyncContext], pozadinski pomiču se promjene na sve tablice. Samo zapise s lokalne promjene se šalju na poslužitelj.

* **[PullAsync] ** 
   na istaknuti se pokreće iz [IMobileServiceSyncTable]. Kada u tablicu evidentirane promjene, implicitno automatske pokreće se da biste bili sigurni da sve tablice u lokalnom spremištu zajedno s odnosima ostaju isti. Parametar *pushOtherTables* određuje hoće li ostalim tablicama u kontekstu pomiču se u implicitno automatske. Parametar *upita* otvara se [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   ili OData nizu upita za filtriranje podataka u vraćenom. Parametar *queryId* koristi se za definiranje rastuća sinkronizaciju. Dodatne informacije potražite u članku  [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]** Pokrenite aplikaciju povremeno nazovite ovaj postupak Pročistite zastarjele podatke iz lokalno spremište. Koristite parametar *prisilno* kada se morate Očisti sve promjene koje još nisu sinkronizirane.

Dodatne informacije o koncepte potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Dodatne informacije

Sljedeće teme sadrže dodatne informacije o značajci izvanmrežnu sinkronizaciju mobilna aplikacija:

* [Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]
* [Azure mobilne aplikacije .NET SDK NEMODERIRANA][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]: app-service-mobile-offline-data-sync.md
[Stvaranje aplikacije za windows]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
