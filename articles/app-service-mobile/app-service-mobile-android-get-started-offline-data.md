<properties
    pageTitle="Omogućivanje izvanmrežne sinkronizacije za Azure mobilnu aplikaciju (Android)"
    description="Saznajte kako pomoću servisa mobilna aplikacija predmemorije i sinkronizaciju izvanmrežnih podataka u aplikaciji za Android"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Omogućivanje izvanmrežne sinkronizacije za Android mobilne aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča pokriva značajku izvanmrežnu sinkronizaciju Azure mobilna aplikacija za Android. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze. Promjene se spremaju u lokalnu bazu podataka. Kada se vratite u mrežni način je uređaj, te promjene se sinkroniziraju s udaljenog pozadinskom.

Ako je ovo prvi doživljaj Azure mobilne aplikacije, trebali biste najprije dovršite vodič [Stvaranje aplikacija za Android]. Ako ne koristite project server preuzete brzi početak rada, morate dodati paketa za nastavak podataka programa access u projekt. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Dodatne informacije o značajci izvanmrežnu sinkronizaciju potražite u temi [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="update-the-app-to-support-offline-sync"></a>Ažuriranje aplikacije za podršku izvanmrežne sinkronizacije

S izvanmrežnu sinkronizaciju pročitati i pisanje iz *tablice sinkronizaciju* (pomoću sučelja *IMobileServiceSyncTable* ), koji je dio **SQLite** baze podataka na uređaju.

Da biste automatske i povući razlike između uređaja i Azure mobilne usluge, pomoću *sinkronizacije kontekstu* (*MobileServiceClient.SyncContext*), koji će se pokrenuti s lokalnom bazom podataka radi pohrane podataka lokalno.

1. U `TodoActivity.java`, komentar izvan postojeću definiciju `mToDoTable` i uklonite verziju tablice sinkronizacije:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. U na `onCreate` metoda komentar izvan postojeće inicijalizaciju `mToDoTable` i uklonite tu definiciju:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. U `refreshItemsFromTable` komentar izvan definicija `results` i uklonite tu definiciju:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Komentar izvan definicija `refreshItemsFromMobileServiceTable`.

5. Uklonite definiciju `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Uklonite definiciju `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Testiranje aplikacije

U ovom ćete odjeljku testiranje ponašanje s Wi-Fi veze na, a zatim isključite mogućnost Wi-Fi veze da biste stvorili izvanmrežni scenarij.

Kada dodate stavke podataka, oni sadrži lokalno spremište SQLite, ali ne sinkroniziraju u mobilnu uslugu prije pritisnite gumb **Osvježi** . Druge aplikacije može različito tretiraju vezane uz kada je podataka potrebno sinkronizirati, ali u svrhu pokazni videozapis ovog praktičnog vodiča je korisnik izričito zatražite.

Kada pritisnete taj gumb, pokreće se novi zadatak u pozadini. Najprije ga ih gura sve promjene u lokalnom spremištu pomoću sinkronizacije kontekst, a zatim povlači sve promjene podataka iz Azure lokalnu tablicu.

### <a name="offline-testing"></a>Testiranje izvan mreže

1. Postavite uređaj ili simulator u *Načinu aviona*. Time se stvara izvanmrežni scenarij.

2. Dodavanje neke stavke *obveze* ili ih označite neke stavke kao dovršen. Zatvorite uređaja ili simulator (ili Prisilno zatvaranje aplikacija) i ponovno ga pokrenite. Provjerite da promjene ste je ista i na uređaju jer oni drže u lokalnom spremištu SQLite.

3. Prikaz sadržaja tablice Azure *TodoItem* bilo sa SQL alata kao što je *SQL Server Management Studio*ili OSTALE klijent kao što su *Fiddler* ili *Postman*. Provjerite nove stavke _nisu_ sinkronizirane s poslužiteljem

    + Za s pozadinskom Node.js idite na [portal za Azure](https://portal.azure.com/)i u mobilnu aplikaciju pozadinskog kliknite **Jednostavno tablice** > **TodoItem** da biste pogledali sadržaj na `TodoItem` tablice.
    + S pozadinskom .NET pregledati sadržaj tablice pomoću alata za SQL kao što je *SQL Server Management Studio*ili OSTALE klijent kao što su *Fiddler* ili *Postman*.

4. Uključite Wi-Fi veze na uređaj ili simulator. Nakon toga pritisnite gumb **Osvježi** .

5. Ponovno pogledati TodoItem podataka na portalu za Azure. Nove i promijenjene TodoItems sad trebao izgledati.

## <a name="additional-resources"></a>Dodatni resursi

* [Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]

* [Naslovnice u oblaku: izvanmrežnu sinkronizaciju u Azure mobilne usluge] \(Napomena: videozapis je mobilne usluge, ali Izvanmrežna sinkronizacija funkcionira na isti način u Azure mobilne aplikacije\)


<!-- URLs. -->

[Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]: app-service-mobile-offline-data-sync.md

[Stvaranje aplikacije za Android]: app-service-mobile-android-get-started.md

[Oblak naslovnice: Izvanmrežnu sinkronizaciju u Azure mobilne usluge]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

