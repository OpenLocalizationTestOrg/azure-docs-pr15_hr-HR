<properties
    pageTitle="Omogućivanje izvanmrežne sinkronizacije za Azure mobilnu aplikaciju (Cordova) | Microsoft Azure"
    description="Upute za korištenje aplikacije za mobilne aplikacije servisa za predmemorije i sinkronizaciju izvanmrežnih podataka u aplikaciji Cordova"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Omogućivanje izvanmrežne sinkronizacije za mobilne aplikacije Cordova

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Pomoću ovog praktičnog vodiča predstavlja značajku izvanmrežnu sinkronizaciju Azure mobilna aplikacija za Cordova. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze. Promjene spremaju se u lokalnu bazu podataka; Kada se vratite u mrežni način je uređaj, te promjene se sinkroniziraju s daljinskog servisa.

Pomoću ovog praktičnog vodiča temelji se na rješenje za brzi početak rada Cordova za mobilne aplikacije koje ste stvorili kada dovršite vodič [Apache Cordova brzo pokretanje]. U ovom ćete praktičnom vodiču će se ažurirati rješenje za brzi početak rada da biste dodali izvanmrežne značajke Azure mobilne aplikacije. Ne možemo će istaknuti kod specifičnih za izvanmrežno u aplikaciji.

Dodatne informacije o značajci izvanmrežnu sinkronizaciju potražite u temi [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije]. Detalje o korištenju API potražite u članku datoteke [README] u dodatak.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Dodavanje izvanmrežnu sinkronizaciju rješenje za brzi početak rada

Izvanmrežna sinkronizacija kod mora biti dodan u aplikaciju. Izvanmrežna sinkronizacija potreban je dodatak za cordova sqlite pohranu koji se automatski se dodaju aplikacije kada dodatak za Azure mobilne aplikacije uvrštava u projektu. Brzi početak rada projekta sadrži oba Ti dodaci.

1. U pregledniku rješenja za Visual Studio, otvorite index.js i zamijenite sljedeći kod

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    kod:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Nakon toga zamijenite sljedeći kod:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    kod:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Prethodni kod dodavanja Inicijalizacija lokalno spremište i definiranje lokalne tablice koja odgovara stupcu vrijednosti koje se koriste u vašem Azure sigurnosno Završi. (Nije potrebno da biste obuhvatili sve vrijednosti u stupcu kod.)

    Referenca na kontekst sinkronizaciju se tako da nazovete **getSyncContext**. Kontekst za sinkronizaciju olakšava sačuvati odnosa između tablica za praćenje i margina promjene u svim tablicama klijent aplikaciju izmijenio kada se zove **automatske** .

3. Ažurirajte URL aplikacije za mobilnu aplikaciju aplikacije URL-a.

4. Nakon toga zamijenite kod:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    kod:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Prethodni kod inicijalizira kontekst sinkronizaciju, a zatim poziva getSyncTable (umjesto getTable) da biste dobili reference na lokalne tablice.

    U ovom koristi kod lokalne baze podataka za sve stvaranje, čitanje, ažuriranje i brisanje Postupci tablice (CRUD).

    Ovaj primjer izvodi jednostavne pogreškom na sukoba. Realni aplikaciju želite rukovati razne pogreške kao što su uvjeti na mreži, sukoba poslužitelja i drugim korisnicima. Primjeri kod potražite u članku [uzorka izvanmrežnu sinkronizaciju].

5. Zatim dodajte ove funkcije za izvođenje stvarni sinkronizaciju.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Odlučite kada automatske promjene za mobilnu aplikaciju pozadinski tako da nazovete **automatske** na **syncContext** objekt koji se koristi klijenta. Ako, na primjer, poziv **syncBackend** nije moguće dodati gumb događajima u aplikaciji kao što je novi gumb Sinkroniziraj ili možete dodati poziv funkciji **addItemHandler** da biste sinkronizirali kad god se doda nova stavka.

##<a name="offline-sync-considerations"></a>Izvanmrežna sinkronizacija pitanja

U uzorku, način **automatske** **syncContext** zove samo prilikom pokretanja aplikacije u funkciji povratnog za prijavu.  U aplikaciji za stvarne nije moguće i provjerite ta je funkcija sinkronizacije pokreće ručno ili kada se mijenja stanje mreže.

Kada je istaknuti se izvršava na temelju tablicu koja sadrži na čekanju lokalne ažuriranja prate prema kontekstu, povlačite postupak automatski pokrenuti prethodnog konteksta automatske. Prilikom osvježavanja, dodavanje i završavanje stavke u ovom primjeru možete izostaviti eksplicitno **Automatsko prosljeđivanje** poziva, budući da je možda suvišne (prvi Provjera [datoteka PROČITAJME] za trenutni status značajke).

U kodu navedeni svi zapisi u tablici udaljene todoItem su mu, ali i moguće je da biste filtrirali zapise prosljeđivanjem id upita i upit **automatske**. Dodatne informacije potražite u odjeljku *Rastuća sinkronizacije* u [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="optional-disable-authentication"></a>(Neobavezno) Onemogućite provjeru autentičnosti

Ako niste već konfigurirali provjeru autentičnosti, a ne želite da biste postavili provjeru autentičnosti prije testiranja izvanmrežnu sinkronizaciju, komentar izvan funkciju povratnog za prijavu, ali ostavite kod unutar funkcija povratnog uncommented.

Kod trebao bi izgledati ovako nakon komentiranje više redaka za prijavu.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Sada aplikacija će se sinkronizirati s Azure pozadinskog kada pokrenete aplikaciju umjesto prilikom prijave.

## <a name="run-the-client-app"></a>Pokrenite aplikaciju klijenta

S izvanmrežnu sinkronizaciju onemogućen, možete odmah pokrenuti klijentska aplikacija barem jednom na svakom platformu za ispunjavanje lokalno spremište baze podataka. Kasnije će kao zamjenu za izvanmrežni scenarij i mijenjati podatke u lokalnom spremištu dok je aplikaciju izvan mreže.

## <a name="optional-test-the-sync-behavior"></a>(Neobavezno) Testiranje ponašanje sinkronizacije

U ovom ćete odjeljku će promijeniti klijent projekta u programu Publisher izvanmrežni scenarij pomoću aplikacije nije valjan URL za svoje pozadinskog. Prilikom svakog dodavanja ili mijenjanja stavke podataka, promjene će sadrži lokalno spremište, ali ne sinkronizira sa spremišta podataka pozadinskog dok je ponovno uspostaviti vezu.

1. U pregledniku rješenja otvoriti datoteku index.js projekta i promijeniti URL aplikacije na URL koji nisu valjani, kao što je sljedeća:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. U index.html, ažurirati CSP `<meta>` element s istim URL koji nisu valjani.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Stvaranje i pokretali aplikaciju za klijenta i obratite pozornost na to da iznimke prijavljen je na konzoli kada aplikacija pokušava sinkronizirati s pozadinski nakon prijave. Sve nove stavke, dodate će postoje samo u lokalnom spremištu dok se ne završi mobilne pozadinskog. Aplikacije klijenta ponaša se kao da je povezano s pozadinskom, podrške sve stvaranje, čitanje, ažuriranje, operacija brisanja (CRUD).

4. Zatvorite aplikaciju i ponovno ga da biste potvrdili da se nove stavke koje ste stvorili dosljedan u lokalnom spremištu.

5. (Neobavezno) Koristite Visual Studio da biste prikazali baze podataka SQL Azure tablice da biste vidjeli se nije promijenio podatke u pozadinskom bazom podataka.

    U Visual Studio, otvorite **Eksplorer za poslužitelj**. Otvorite bazu podataka u **Azure**->**Baze podataka SQL**. Desnom tipkom miša kliknite bazu podataka, a zatim odaberite **Otvori u programu Explorer sustava SQL Server objekt**. Sada možete pregledavati tablice baze podataka SQL i njezin sadržaj.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Neobavezno) Testiranje ponovno povezivanje na vaš mobilni pozadinskog

U ovom odjeljku će ponovno povezati aplikacije za mobilne pozadinskog koji simulira aplikacija uskoro vratite u mrežni rad. Kada se prijavite, podaci će se sinkronizirati s vaš mobilni pozadinskog.

1. Ponovno otvorite index.js i ispravljanje URL aplikacije na točan URL.

2. Ponovno otvorite index.html i ispravljanje URL aplikacije u CSP `<meta>` element.

3. Ponovno stvaranje i pokretanje aplikacija klijenta. Aplikacija pokušava sinkronizirati s pozadinskom mobilne aplikacije nakon prijave. Provjerite je li izuzetaka ste prijavljeni na konzoli za ispravljanje pogrešaka.

4. (Neobavezno) Prikaz ažurirane podatke pomoću programa Explorer objekt za SQL Server ili alat za OSTALE kao što je Fiddler. Obratite pozornost na to je sinkronizirana podataka između pozadinskom bazom podataka i u lokalnom spremištu.

    Obratite pozornost na to podaci sinkronizirane između baza podataka i u lokalnom spremištu i sadrži stavke koje ste dodali dok je prekinuta aplikacije.

## <a name="additional-resources"></a>Dodatni resursi

* [Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]

* [Naslovnice u oblaku: izvanmrežnu sinkronizaciju u Azure mobilne usluge] \(Napomena: videozapis je mobilne usluge, ali Izvanmrežna sinkronizacija funkcionira na isti način u Azure mobilne aplikacije\)

* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Daljnji koraci

* Pogledajte dodatne značajke izvanmrežnu sinkronizaciju kao što su Razrješenje sukoba u [izvanmrežnu sinkronizaciju uzorka]
* Pogledajte izvanmrežnu sinkronizaciju API reference u [README]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Brzi početak Apache Cordova]: app-service-mobile-cordova-get-started.md
[DATOTEKA PROČITAJME]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[Izvanmrežna sinkronizacija uzorka]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]: app-service-mobile-offline-data-sync.md
[Naslovnica oblaka: Izvanmrežnu sinkronizaciju u Azure mobilne usluge]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
