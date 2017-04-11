<properties
    pageTitle="Omogućivanje izvanmrežne sinkronizacije za Azure mobilnu aplikaciju (Xamarin.Forms) | Microsoft Azure"
    description="Upute za korištenje aplikacije za mobilne aplikacije servisa za predmemorije i sinkronizaciju izvanmrežnih podataka u aplikaciji Xamarin.Forms"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Omogućivanje izvanmrežne sinkronizacije za mobilne aplikacije Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Pregled
Pomoću ovog praktičnog vodiča predstavlja značajku izvanmrežnu sinkronizaciju Azure mobilna aplikacija za Xamarin.Forms. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije – prikaz, dodavanje ili izmjenu podataka – čak i kad je nema mrežne veze. Promjene se spremaju u lokalnu bazu podataka. Kada se vratite u mrežni način je uređaj, te promjene se sinkroniziraju s daljinskog servisa.

Pomoću ovog praktičnog vodiča temelji se na rješenje za brzi početak rada Xamarin.Forms za mobilne aplikacije koje ste stvorili kada dovršite vodič [stvaranje aplikacije za iOS Xamarin]. Rješenje za brzi početak rada za Xamarin.Forms sadrži šifru podrške Izvanmrežna sinkronizacija koji upravo mora biti omogućen. U ovom ćete praktičnom vodiču ažurirajte rješenje za brzi početak rada da biste uključili izvanmrežni značajke Azure mobilne aplikacije. Ne možemo istaknite i izvanmrežno specifične kod u aplikaciji. Ako ne koristite rješenje preuzete brzi početak rada, morate dodati paketa za nastavak podataka programa access u projekt. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije][1].

Dodatne informacije o značajci izvanmrežnu sinkronizaciju potražite u temi [Izvanmrežnu sinkronizaciju podataka u aplikacijama za mobilne Azure][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Omogućivanje funkcije izvanmrežnu sinkronizaciju u rješenje za brzi početak rada

Izvanmrežna sinkronizacija kod uvrštava u projektu pomoću C# preprocessor upute poslužitelja. Kada u **IZVANMREŽNO\_SINKRONIZACIJU\_OMOGUĆENO** definiran simbol, te putova kod obuhvaćeni sastavljanje. Za aplikacije sustava Windows, morate instalirati i SQLite platforme.

1. U Visual Studio, desnom tipkom miša kliknite rješenje > **Upravljanje NuGet paketa rješenja...**, zatim okvir za pretraživanje i instalaciju paketa NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** za svim projektima rješenja.

2. U pregledniku rješenja otvorite datoteku TodoItemManager.cs iz projekta s **prijenosni** u nazivu, što je prijenosni Biblioteka klasa projekta, zatim uklonite preprocessor sljedećih smjernica:

        #define OFFLINE_SYNC_ENABLED

4. (Neobavezno) Da biste podržava uređaje sa sustavom Windows, instalirajte jedan od sljedećih paketa runtime SQLite:

    * **Runtime Windows 8.1:** Instalacija [SQLite Windows 8.1][3].
    * **u sustavu windows Phone 8.1:** Instalacija [SQLite za Windows Phone 8.1][4].
    * **Univerzalni Windows platforma** Instalacija [SQLite za univerzalno univerzalni Windows][5].

    Iako za brzi početak rada sadrže univerzalni Windows projekt, platformu univerzalni Windows podržan je Xamarin obrazaca.

5. (Neobavezno) U svaki projekt aplikacije Windows desnom tipkom miša kliknite **reference** > **Dodavanje referenca...**, proširite mapu **sustava Windows** > **proširenja**.
    Omogućivanje odgovarajući **SQLite za Windows** SDK uz **Visual C++ 2013 Runtime za Windows** SDK.
    Nazive SQLite SDK malo razlikovati sa svakom platformu sustava Windows.

## <a name="review-the-client-sync-code"></a>Pregledajte sinkronizaciju kod klijenta

Ovdje je Kratak pregled što već sadrže programski kod vodiča unutar na `#if OFFLINE_SYNC_ENABLED` upute poslužitelja. Funkcija Izvanmrežna sinkronizacija se TodoItemManager.cs datoteku projekta u programu project prijenosni Biblioteka klasa. Konceptualni pregled značajke potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije][2].

* Prije nego što se sve tablice operacije može izvoditi, lokalno spremište mora biti pokrenut. Lokalno spremište baze podataka se pokreće u predmete Graditelj **TodoItemManager** pomoću sljedeći kod:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Kod stvara novu lokalnu SQLite bazu podataka pomoću klase **MobileServiceSQLiteStore** .

    Način **DefineTable** stvaranje tablice u lokalnom spremištu koja odgovara polja u navedene vrste.  Da biste obuhvatili sve stupce koji se nalaze u udaljena baza podataka ne sadrži vrstu. Nije moguće spremiti podskup stupaca.

* Polje **todoTable** u **TodoItemManager** nije vrsta **IMobileServiceSyncTable** umjesto **IMobileServiceTable**. U ovom koristi predmete lokalne baze podataka za sve stvaranje, čitanje, ažuriranje i brisanje Postupci tablice (CRUD). Odlučite te promjene pritisak za mobilnu aplikaciju pozadinski tako da nazovete **PushAsync** na **IMobileServiceSyncContext**. Kontekst za sinkronizaciju olakšava sačuvati odnosa između tablica za praćenje i margina promjene u svim tablicama klijent aplikaciju izmijenio kada se zove **PushAsync** .

    Sljedeću metodu **SyncAsync** zove za sinkronizaciju s pozadinskom mobilnu aplikaciju:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Ovaj primjer koristi jednostavne pogreškom s zadani rukovatelj sinkronizaciju. Realni aplikacije želite upravljati razne pogreške kao što su uvjeti na mreži i sukoba poslužitelja pomoću prilagođenih **IMobileServiceSyncHandler** implementacije.

## <a name="offline-sync-considerations"></a>Izvanmrežna sinkronizacija pitanja

U uzorku, metodu **SyncAsync** samo zove pokretanja i primitku sinkronizaciju.  Da biste pokrenuli sinkronizaciju u aplikaciji za Android ili iOS, padajući popis stavki; za Windows, koristite gumb **Sinkroniziraj** . U aplikaciji za stvarne nije moguće i provjerite okidača sinkronizaciju promjenama stanje mreže.

Kada je istaknuti se izvršava na temelju tablicu koja sadrži na čekanju lokalne ažuriranja kontekstu koji istaknuti postupak automatski okidača pratiti prethodnog konteksta pribadače. Prilikom osvježavanja, dodavanje i završavanje stavke u ovom primjeru, izostavite eksplicitnih **PushAsync** poziv.

U kodu navedeni svi zapisi u tablici udaljene TodoItem su mu, ali i moguće je da biste filtrirali zapise prosljeđivanjem id upita i upit **PushAsync**. Dodatne informacije potražite u odjeljku *Rastuća sinkronizacije* u [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije][2].

## <a name="run-the-client-app"></a>Pokrenite aplikaciju klijenta

S izvanmrežnu sinkronizaciju onemogućen, pokrenite klijentska aplikacija barem jednom na svakom platformu za ispunjavanje lokalno spremište baze podataka. Kasnije kao zamjenu za izvanmrežni scenarij i mijenjati podatke u lokalnom spremištu dok je aplikaciju izvan mreže.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Ažuriranje funkcioniranja sinkronizacije aplikacije klijenta

U ovom ćete odjeljku izmjena klijent projekta u programu Publisher izvanmrežni scenarij pomoću aplikacije nije valjan URL za svoje pozadinskog. Osim toga, možete isključiti mrežne veze pomicanjem uređaju "Način aviona."  Kada dodali ili promijenili stavke podataka, te promjene sadrži lokalno spremište, ali ne sinkronizira sa spremišta podataka pozadinskog dok je ponovno uspostaviti vezu.

1. U pregledniku rješenja otvorite datoteku Constants.cs projekta iz **prijenosni** projekta i promijenite vrijednost `ApplicationURL` tako da pokazuje na koji nisu valjani URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. Otvaranje datoteke TodoItemManager.cs **prijenosni** projekta, a zatim dodajte na **Uhvatite** osnovni klase **iznimku** blok **pokušajte... zamka** u **SyncAsync**. Ovaj blok **Uhvatite** zapisuje poruka iznimke na konzoli sustava na sljedeći način:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Stvaranje i pokretanje aplikacije klijenta.  Dodajte neki nove stavke. Obratite pozornost na to da iznimke prijavljen je na konzoli za svaki pokušaj sinkronizirati s pozadinski. Nove stavke postoje samo u lokalnom spremištu dok se ne završi mobilne pozadinskog. Aplikacije klijenta ponaša se kao da je povezano s pozadinskom, podrške sve stvaranje, čitanje, ažuriranje, operacija brisanja (CRUD).

4. Zatvorite aplikaciju i ponovno ga da biste potvrdili da se nove stavke koje ste stvorili dosljedan u lokalnom spremištu.

5. (Neobavezno) Koristite Visual Studio da biste prikazali baze podataka SQL Azure tablice da biste vidjeli se nije promijenio podatke u pozadinskom bazom podataka.

    U Visual Studio, otvorite **Eksplorer za poslužitelj**. Otvorite bazu podataka u **Azure**->**Baze podataka SQL**. Desnom tipkom miša kliknite bazu podataka, a zatim odaberite **Otvori u programu Explorer sustava SQL Server objekt**. Sada možete pregledavati tablice baze podataka SQL i njezin sadržaj.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Ažurira aplikaciju za klijent povezati vaš mobilni pozadinskog

U ovom ćete odjeljku ponovno povezati aplikacije za mobilne pozadinskog koji simulira aplikacija uskoro vratite u mrežni rad. Prilikom izvršavanja gesta osvježavanja podataka sinkronizira se vaš mobilni pozadinskog.

1. Ponovno otvorite Constants.cs. Ispravljanje u `applicationURL` na točan URL.

2. Ponovno stvaranje i pokretanje aplikacija klijenta. Aplikacija pokušava sinkronizirati s pozadinskom mobilne aplikacije nakon pokretanja. Provjerite je li izuzetaka ste prijavljeni na konzoli za ispravljanje pogrešaka.

3. (Neobavezno) Prikaz ažurirane podatke pomoću programa Explorer objekt za SQL Server ili alat za OSTALE kao što su Fiddler ili [Postman][6]. Obratite pozornost na to je sinkronizirana podataka između pozadinskom bazom podataka i u lokalnom spremištu.

    Obratite pozornost na to podaci sinkronizirane između baza podataka i u lokalnom spremištu i sadrži stavke koje ste dodali dok je prekinuta aplikacije.

## <a name="additional-resources"></a>Dodatni resursi

* [Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije][2]
* [Azure mobilne aplikacije .NET SDK NEMODERIRANA][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md