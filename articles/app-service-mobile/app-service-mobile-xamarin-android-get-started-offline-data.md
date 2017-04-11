<properties
    pageTitle="Omogućivanje izvanmrežne sinkronizacije za Azure mobilnu aplikaciju (Xamarin Android)"
    description="Upute za korištenje aplikacije za mobilne aplikacije servisa za predmemorije i sinkronizaciju izvanmrežnih podataka u aplikaciji za Xamarin Android"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Omogućivanje izvanmrežne sinkronizacije za mobilne aplikacije Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča predstavlja značajku izvanmrežnu sinkronizaciju Azure mobilna aplikacija za Xamarin.Android. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije – prikaz, dodavanje ili izmjenu podataka – čak i kad je nema mrežne veze. Promjene se spremaju u lokalnu bazu podataka.
Kada se vratite u mrežni način je uređaj, te promjene se sinkroniziraju s daljinskog servisa.

U ovom ćete praktičnom vodiču ažurirati klijent projekta iz vodiča [Stvaranje Xamarin Android aplikacije] da biste podržava izvanmrežno značajke Azure mobilne aplikacije. Ako ne koristite project server preuzete brzi početak rada, morate dodati paketa za nastavak podataka programa access u projekt. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Dodatne informacije o značajci izvanmrežnu sinkronizaciju potražite u temi [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="update-the-client-app-to-support-offline-features"></a>Ažurira aplikaciju za klijenta za podršku značajki izvanmrežno

Azure značajke izvanmrežne mobilnu aplikaciju omogućuju omogućuje interakciju s lokalnom bazom podataka kada se nalazite u izvanmrežni scenarij. Da biste koristili te značajke u aplikaciji, pokretanje [SyncContext] lokalno spremište. Zatim pozvati tablice putem sučelja [IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite koristi se kao lokalno spremište na uređaju.

1. U Visual Studio, otvorite upravitelj NuGet paketa u programu project završio u vodiču za [Stvaranje Xamarin Android aplikacije] .  Traženje i instalirajte paket NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

2. Otvorite datoteku ToDoActivity.cs i uklonite na `#define OFFLINE_SYNC_ENABLED` definicija.

3. U Visual Studio, pritisnite tipku **F5** da biste ponovno stvaranje i pokretanje aplikacija klijenta. Aplikaciju funkcionira na isti jednak onome prije omogućeno izvanmrežnu sinkronizaciju. Međutim, lokalnu bazu podataka sada unose s podacima koji se može koristiti u izvanmrežni scenarij.

## <a name="update-sync"></a>Ažuriranje aplikacije da biste prekinuli pozadinski

U ovom ćete odjeljku prekinuti vezu za mobilnu aplikaciju pozadinskog sustava u programu Publisher izvanmrežni situacija. Kada dodate stavke podataka, vaše rukovatelj iznimku obavijestit će vas aplikacija je u izvanmrežnom načinu rada. U ovom stanju nove stavke dodane u lokalnom spremištu i su sinkronizirani s pozadinskom mobilne aplikacije kada je automatske se izvršava u povezanim stanju.

1. Uređivanje ToDoActivity.cs zajedničke projektu. Promjena **applicationURL** tako da pokazuje na koji nisu valjani URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Onemogućivanje Wi-Fi veze i mobilne mreže na uređaju ili pomoću aviona način možete demonstrirati i izvanmrežnog funkcioniranja.

2. Pritisnite **F5** omogućuje stvaranje i pokretanje aplikacija. Obratite pozornost na to sinkronizacije nije uspjela prilikom osvježavanja prilikom pokretanja aplikacije.

3. Unesite nove stavke, a zatim obratite pozornost na to da automatske ne uspije čiji je status [CancelledByNetworkError] svaki put kada kliknete **Spremi**. Međutim, nove stavke obveze postoji u lokalnom spremištu dok se ne završi pozadinskog mobilne aplikacije.  U aplikaciji radnog ako izostavi te iznimke aplikaciju klijent se ponaša kao da je i dalje povezan s pozadinskom mobilne aplikacije.

4. Zatvorite aplikaciju i ponovno ga da biste potvrdili da se nove stavke koje ste stvorili dosljedan u lokalnom spremištu.

5. (Neobavezno) U Visual Studio, otvorite **Eksplorer za poslužitelj**. Otvorite bazu podataka u **Azure**->**Baze podataka SQL**. Desnom tipkom miša kliknite bazu podataka, a zatim odaberite **Otvori u programu Explorer sustava SQL Server objekt**. Sada možete pregledavati tablice baze podataka SQL i njezin sadržaj. Provjerite je li se nije promijenio podatke u pozadinskom bazom podataka.

6. (Neobavezno) Pomoću alata za OSTALE kao što su Fiddler ili Postman poslati upit mobilnog pozadinskog, korištenje upita za DOHVAĆANJE u obrascu `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Ažuriranje aplikacije povezati svoje pozadinskog mobilnu aplikaciju

U ovom ćete odjeljku ponovno povezati aplikaciju da biste pozadinskog mobilne aplikacije. Kada prvi put pokrenete aplikaciju, u `OnCreate` pozive rukovatelja događajima `OnRefreshItemsSelected`. Ta metoda poziva `SyncAsync` da biste sinkronizirali svoje lokalno spremište s pozadinskom bazom podataka.

1. Otvorite ToDoActivity.cs zajedničke projekta i vratiti promjena svojstva **applicationURL** .

2. Da biste ponovno stvaranje i pokretanje aplikacija, pritisnite tipku **F5** . Aplikaciju sinkronizira lokalne promjene s pozadinskom Azure mobilnu aplikaciju pomoću automatske i povlačite operacija kada se `OnRefreshItemsSelected` izvršava način.

3. (Neobavezno) Prikaz ažurirane podatke pomoću programa Explorer objekt za SQL Server ili alata za OSTALE kao što je Fiddler. Obavijest o podacima je sinkronizirana između Azure mobilnu aplikaciju pozadinskom bazom podataka i u lokalnom spremištu.

4. U aplikaciji, kliknite potvrdni okvir pokraj nekoliko stavki za popunjavanje u lokalnom spremištu.

  `CheckItem`pozivi `SyncAsync` dovršena je sinkronizacija svaku stavku s pozadinskom mobilnu aplikaciju. `SyncAsync`poziva automatske i povlačite. **Kad god se izvoditi povlačite prema tablicu koja klijent je promijenio, na automatske se uvijek izvršava automatski**. Na taj način sve tablice u lokalnom spremištu zajedno s odnosima ostaju isti. Taj se problem može uzrokovati neočekivane pribadače. Dodatne informacije o takvo ponašanje potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="review-the-client-sync-code"></a>Pregledajte sinkronizaciju kod klijenta

Projekt klijent Xamarin koji ste preuzeli kada već dovrše vodič [Stvaranje aplikacije za Xamarin Android] sadrži kod podrške Izvanmrežna sinkronizacija koristeći lokalne baze podataka SQLite. Evo što već sadrži vodiča c ode kratki pregled. Konceptualni pregled značajke potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

* Prije nego što se sve tablice operacije može izvoditi, lokalno spremište mora biti pokrenut. Lokalno spremište baze podataka se pokreće kada `ToDoActivity.OnCreate()` izvršava `ToDoActivity.InitLocalStoreAsync()`. Ova metoda stvara lokalni SQLite bazu podataka koristeći na `MobileServiceSQLiteStore` predmete koje ste dobili od klijentskog programa Azure mobilne aplikacije SDK.

    Na `DefineTable` način stvaranje tablice u lokalnom spremištu koja odgovara polja u vrstu navedeni `ToDoItem` u tom slučaju. Da biste obuhvatili sve stupce koji se nalaze u udaljena baza podataka ne sadrži vrstu. Moguće je spremiti samo podskupa stupaca.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* Na `toDoTable` član `ToDoActivity` je na `IMobileServiceSyncTable` upišite umjesto `IMobileServiceTable`. Na IMobileServiceSyncTable usmjerava sve stvaranje, čitanje, ažuriranje i brisanje Postupci tablice (CRUD) u lokalnom spremištu bazu podataka.

    Odlučite kada promjene pomiču se za mobilnu aplikaciju Azure pozadinski tako da nazovete `IMobileServiceSyncContext.PushAsync()`. Kontekst za sinkronizaciju olakšava sačuvati odnosa između tablica za praćenje i margina promjene u svim tablicama klijent aplikaciju izmijenio kada `PushAsync` naziva.

    Pozivi navedeni kod `ToDoActivity.SyncAsync()` za sinkronizaciju prilikom osvježavanja todoitem popisa ili na todoitem dodaje ili dovršeno. Kod sinkronizira nakon svake promjene lokalni.

    U kodu navedeni svi zapisi u na udaljenom `TodoItem` su mu tablice, ali preporučuje se i filtriranje zapisa prosljeđivanjem id upita i upit da biste `PushAsync`. Dodatne informacije potražite u odjeljku *Rastuća sinkronizacije* u [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Dodatni resursi

* [Sinkroniziranje izvanmrežne podatke u Azure mobilne aplikacije]
* [Azure mobilne aplikacije .NET SDK NEMODERIRANA][8]

<!-- URLs. -->
[Stvaranje aplikacije za Xamarin Android]: ../app-service-mobile-xamarin-android-get-started.md
[Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Stvaranje aplikacije za Xamarin Android]: app-service-mobile-xamarin-android-get-started.md
[Sinkroniziranje izvanmrežne podatke u Azure mobilne aplikacije]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
