<properties
    pageTitle="Omogućivanje izvanmrežne sinkronizacije za Azure mobilnu aplikaciju (Xamarin iOS)"
    description="Upute za korištenje aplikacije za mobilne aplikacije servisa za predmemorije i sinkronizaciju izvanmrežnih podataka u aplikaciji za iOS Xamarin"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Omogućivanje izvanmrežne sinkronizacije za mobilne aplikacije Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča predstavlja značajku izvanmrežnu sinkronizaciju Azure mobilna aplikacija za Xamarin.iOS. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije – prikaz, dodavanje ili izmjenu podataka – čak i kad je nema mrežne veze. Promjene se spremaju u lokalnu bazu podataka. Kada se vratite u mrežni način je uređaj, te promjene se sinkroniziraju s daljinskog servisa.

U ovom ćete praktičnom vodiču ažuriranje aplikacije project Xamarin.iOS iz odjeljka [Stvaranje aplikacije za iOS Xamarin] podrške Izvanmrežna značajke Azure mobilne aplikacije. Ako ne koristite project server preuzete brzi početak rada, morate dodati paketa za nastavak podataka programa access u projekt. Dodatne informacije o paketima proširenje server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Dodatne informacije o značajci izvanmrežnu sinkronizaciju potražite u temi [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="update-the-client-app-to-support-offline-features"></a>Ažurira aplikaciju za klijenta za podršku značajki izvanmrežno

Azure značajke izvanmrežne mobilnu aplikaciju omogućuju omogućuje interakciju s lokalnom bazom podataka kada se nalazite u izvanmrežni scenarij. Da biste koristili te značajke u aplikaciji, pokretanje [SyncContext] lokalno spremište. Reference tablice putem sučelja [IMobileServiceSyncTable]. SQLite koristi se kao lokalno spremište na uređaju.

1. Otvorite Upravitelj paketa NuGet u programu project završio u ovom praktičnom vodiču [Stvaranje aplikacije za iOS Xamarin] , a zatim pronađite i instalirajte paket **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet.

2. Otvorite datoteku QSTodoService.cs i uklonite na `#define OFFLINE_SYNC_ENABLED` definicija.

3. Ponovno stvaranje i pokretanje aplikacija klijenta. Aplikaciju funkcionira na isti jednak onome prije omogućeno izvanmrežnu sinkronizaciju. Međutim, lokalnu bazu podataka sada unose s podacima koji se može koristiti u izvanmrežni scenarij.

## <a name="update-sync"></a>Ažuriranje aplikacije da biste prekinuli pozadinski

U ovom ćete odjeljku prekinuti vezu za mobilnu aplikaciju pozadinskog sustava u programu Publisher izvanmrežni situacija. Kada dodate stavke podataka, vaše rukovatelj iznimku obavijestit će vas aplikacija je u izvanmrežnom načinu rada. U ovom stanju nove stavke dodane u lokalnom spremištu i će se sinkronizirati s pozadinskom mobilne aplikacije automatske sljedeće pokrenete u povezanim stanju.

1. Uređivanje QSToDoService.cs zajedničke projekta. Promjena **applicationURL** tako da pokazuje na koji nisu valjani URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Onemogućivanje Wi-Fi veze i mobilne mreže na uređaju ili pomoću aviona način možete demonstrirati i izvanmrežnog funkcioniranja.

2. Stvaranje i pokretanje aplikacija. Obratite pozornost na to sinkronizacije nije uspjela prilikom osvježavanja prilikom pokretanja aplikacije.

3. Unesite nove stavke, a zatim obratite pozornost na to da automatske ne uspije čiji je status [CancelledByNetworkError] svaki put kada kliknete **Spremi**. Međutim, nove stavke obveze postoji u lokalnom spremištu dok se ne završi pozadinskog mobilne aplikacije.  U aplikaciji radnog ako izostavi te iznimke aplikaciju klijent se ponaša kao da je i dalje povezan s pozadinskom mobilne aplikacije.

4. Zatvorite aplikaciju i ponovno ga da biste potvrdili da se nove stavke koje ste stvorili dosljedan u lokalnom spremištu.

5. (Neobavezno) Ako imate Visual Studio instaliran na Računalu, otvorite **Eksplorer za poslužitelj**. Otvorite bazu podataka u **Azure**-> **Baze podataka SQL**. Desnom tipkom miša kliknite bazu podataka, a zatim odaberite **Otvori u programu Explorer sustava SQL Server objekt**. Sada možete pregledavati tablice baze podataka SQL i njezin sadržaj. Provjerite je li se nije promijenio podatke u pozadinskom bazom podataka.

6. (Neobavezno) Pomoću alata za OSTALE kao što su Fiddler ili Postman poslati upit mobilne pozadinskog, korištenje upita za DOHVAĆANJE u obrascu `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Ažuriranje aplikacije povezati svoje pozadinskog mobilnu aplikaciju

U ovom ćete odjeljku ponovno povezati aplikaciju da biste pozadinskog mobilne aplikacije. To simulira aplikaciju prijelaz s izvanmrežni rad na mrežni rad s pozadinskom mobilne aplikacije.   Ako Simulirani razliku mrežu tako da isključite veza s mrežom, potrebne su promjene ne kod.
Uključite na mrežu ponovno.  Kada prvi put pokrenete aplikaciju, u `RefreshDataAsync` zove se način. To naizmjence poziva `SyncAsync` da biste sinkronizirali svoje lokalno spremište s pozadinskom bazom podataka.

1. Otvorite QSToDoService.cs zajedničke projekta i vratiti promjena svojstva **applicationURL** .

2. Ponovno stvaranje i pokretanje aplikacija. Aplikaciju sinkronizira lokalne promjene s pozadinskom Azure mobilnu aplikaciju pomoću automatske i povlačite operacija kada na `OnRefreshItemsSelected` izvršava način.

3. (Neobavezno) Prikaz ažurirane podatke pomoću programa Explorer objekt za SQL Server ili alat za OSTALE kao što je Fiddler. Obavijest o podacima je sinkronizirana između Azure mobilnu aplikaciju pozadinskom bazom podataka i u lokalnom spremištu.

4. U aplikaciji, kliknite potvrdni okvir pokraj nekoliko stavki za popunjavanje u lokalnom spremištu.

  `CompleteItemAsync`pozivi `SyncAsync` dovršena je sinkronizacija svaku stavku s pozadinskom mobilnu aplikaciju. `SyncAsync`poziva automatske i povlačite.
  **Kad god se izvoditi povlačite prema tablicu koja klijent je promijenio, automatske na sinkronizaciju kontekstu klijenta se uvijek izvršava najprije automatski**. Implicitno automatske osigurava sve tablice u lokalnom spremištu zajedno s odnosima ostaju isti. Dodatne informacije o taj problem, potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="review-the-client-sync-code"></a>Pregledajte sinkronizaciju kod klijenta

Projekt klijent Xamarin koji ste preuzeli kada već obavljeni vodič [Stvaranje aplikacije za iOS Xamarin] sadrži kod podrške Izvanmrežna sinkronizacija koristeći lokalne baze podataka SQLite. Slijedi kratak pregled što već sadrže programski kod vodiča. Konceptualni pregled značajke potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

* Prije nego što se sve tablice operacije može izvoditi, lokalno spremište mora biti pokrenut. Lokalno spremište baze podataka se pokreće kada `QSTodoListViewController.ViewDidLoad()` izvršava `QSTodoService.InitializeStoreAsync()`. Ova metoda stvara novu lokalnu SQLite bazu podataka koristeći na `MobileServiceSQLiteStore` predmete koje ste dobili od klijentskog programa Azure mobilnu aplikaciju SDK.

    Na `DefineTable` način stvaranje tablice u lokalnom spremištu koja odgovara polja u vrstu navedeni `ToDoItem` u tom slučaju. Da biste obuhvatili sve stupce koji se nalaze u udaljenom bazom podataka ne sadrži vrstu. Moguće je spremiti samo podskupa stupaca.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }


* Na `todoTable` član `QSTodoService` je na `IMobileServiceSyncTable` upišite umjesto `IMobileServiceTable`. Na IMobileServiceSyncTable usmjerava sve stvaranje, čitanje, ažuriranje i brisanje Postupci tablice (CRUD) u lokalnom spremištu bazu podataka.

    Odlučite kada te promjene pomiču se za mobilnu aplikaciju Azure pozadinski tako da nazovete `IMobileServiceSyncContext.PushAsync()`. Kontekst za sinkronizaciju olakšava sačuvati odnosa između tablica za praćenje i margina promjene u svim tablicama klijent aplikaciju izmijenio kada `PushAsync` naziva.

    Pozivi navedeni kod `QSTodoService.SyncAsync()` za sinkronizaciju prilikom osvježavanja todoitem popisa ili na todoitem dodaje ili dovršeno. Aplikacija će se sinkronizira nakon svake promjene lokalni. Ako je istaknuti se izvršava na temelju tablicu koja sadrži na čekanju lokalne ažuriranja prate prema kontekstu, povlačite postupak automatski pokreće kontekst automatske najprije.

    U kodu navedeni svi zapisi u na udaljenom `TodoItem` su mu tablice, ali ujedno filtriranje zapisa prosljeđivanjem id upita i upit da biste `PushAsync`. Dodatne informacije potražite u odjeljku *Rastuće sinkronizacije* u [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }


## <a name="additional-resources"></a>Dodatni resursi

* [Sinkronizacija izvanmrežne podatke u Azure mobilne aplikacije]
* [Azure mobilne aplikacije .NET SDK NEMODERIRANA][8]

<!-- Images -->

<!-- URLs. -->
[Stvaranje aplikacije za iOS Xamarin]: app-service-mobile-xamarin-ios-get-started.md
[Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md