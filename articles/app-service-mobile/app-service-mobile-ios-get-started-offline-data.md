<properties
    pageTitle="Omogućivanje izvanmrežne sinkronizacije za Azure mobilnu aplikaciju (iOS)"
    description="Saznajte kako pomoću servisa mobilna aplikacija predmemorije i sinkronizaciju izvanmrežnih podataka u aplikaciji za iOS"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Omogućivanje izvanmrežne sinkronizacije za mobilne aplikacije iOS

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča pokriva značajka izvanmrežnu sinkronizaciju Azure mobilne aplikacije za iOS. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze. Promjene spremaju se u lokalnu bazu podataka; Kada se vratite u mrežni način je uređaj, te promjene se sinkroniziraju s udaljenog pozadinskom.

Ako je ovo prvi doživljaj Azure mobilne aplikacije, trebali biste najprije dovršite vodič [Stvaranje iOS aplikacija]. Ako ne koristite project server preuzete brzi početak rada, morate dodati paketa za nastavak podataka programa access u projekt. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Dodatne informacije o značajci izvanmrežnu sinkronizaciju potražite u temi [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

## <a name="review-sync"></a>Pregledajte sinkronizaciju kod klijenta

Projekt klijenta koji ste preuzeli za vodič [Stvaranje aplikaciji iOS] već sadrži kod podrške Izvanmrežna sinkronizacija pomoću lokalnoj temeljni podaci utemeljen bazi podataka. U ovom se odjeljku je sažetak što već sadrže programski kod vodiča. Konceptualni pregled značajke potražite u članku [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije].

Značajku sinkronizacija izvanmrežne podatke sinkronizaciju Azure mobilna aplikacija omogućuje krajnjim korisnicima omogućuje interakciju s lokalnom bazom podataka kada je mreža nije dostupna. Da biste koristili te značajke u aplikaciji, pokrenuti sinkronizaciju kontekstu `MSClient` i pozvati lokalno spremište. Referenca tablice putem na `MSSyncTable` sučelja.

1. U **QSTodoService.m** (cilj-C) ili **ToDoTableViewController.swift** (Swift), obratite pozornost na vrstu člana `syncTable` je `MSSyncTable`. Izvanmrežna sinkronizacija koristi ovaj sučelje za sinkronizaciju tablice umjesto `MSTable`. Kada se koristi sinkronizaciju tablice, sve operacije u trgovini lokalne i se sinkronizirati samo s udaljenog pozadinskom s eksplicitno automatsko prosljeđivanje i povlačite operacija.

    Da biste dobili referenca tablice sinkronizaciju, upotrijebite metodu `syncTableWithName` na `MSClient`. Da biste uklonili funkcionalnost izvanmrežnu sinkronizaciju, koristite `tableWithName` umjesto toga.

2. Prije nego što se sve tablice operacije može izvoditi, lokalno spremište mora biti pokrenut. Evo odgovarajuću šifru. 
    
    **Cilj C**:
    
    U na `QSTodoService.init` metoda:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    U na `ToDoTableViewController.viewDidLoad` metoda:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Time ste stvorili lokalno spremište pomoću sučelja `MSCoreDataStore`, koji su dani u SDK za mobilne aplikacije. Možete umjesto toga osiguraj različite lokalno spremište implementacijom na `MSSyncContextDataSource` protokol. 
    
    Osim toga, prvi parametar `MSSyncContext` se koristi za određivanje sukoba događajima. Budući da ne možemo prošlo `nil`, da će se koristiti zadani rukovatelj sukoba, koji se ne uspijeva u bilo kojem sukoba.
    
3. Sada ćemo operaciju stvarni sinkronizaciju i dohvaćanje podataka iz udaljene pozadine.

    **Cilj C**:
    
    `syncData`najprije ih gura novih promjena, zatim poziva `pullData` za dohvaćanje podataka iz udaljene pozadine. U uključivanje metodu `pullData` dobiva nove podatke koji odgovaraju upita:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    U verziji cilj C u `syncData`, ne možemo najprije poziva `pushWithCompletion` za sinkronizaciju kontekst. Ta je metoda član `MSSyncContext` (umjesto sinkroniziranje tablice sam) jer će automatske promjene na sve tablice. Samo zapise koji su izmijenjeni na mobitel lokalno (putem CUD operacije) poslat će se na poslužitelj. Zatim helper `pullData` zove, koja se poziva `MSSyncTable.pullWithQuery` za dohvaćanje udaljeni podaci i pohraniti u lokalnu bazu podataka.
    
    U verziji Swift je bez poziv `pushWithCompletion`. To je zato postupak automatske nije isključivo potrebne. Ako u kontekstu sinkronizacije za tablicu koja je način operacije automatske promjene na čekanju, povlačite uvijek problemi s automatske prvi put. Međutim, ako imate više od jedne tablice sinkronizaciju, je najbolje izričito poziva automatske da biste bili sigurni da je sve dosljedan preko povezanih tablica.
    
    U cilj C i Swift verzijama, metodu `pullWithQuery` omogućuje vam da navedete upita da biste filtrirali zapise koje želite dohvatiti. U ovom primjeru upit samo dohvaća sve zapise u alat za analizu daljinske `TodoItem` tablice.
    
    Drugi parametar `pullWithQuery` je ID za upit koji se koristi za *rastuća sinkronizaciju*. Rastuća sinkronizaciju dohvaća samo zapise promijenjene nakon zadnje sinkronizacije pomoću zapisa `UpdatedAt` vremenske oznake (pod nazivom `updatedAt` u lokalnom spremištu.) ID upita mora biti opisni niz koji je jedinstvena za svaki logičke upit u aplikaciji. Da biste onemogućivanju rastuća sinkronizacije, prenesite `nil` kao ID upita Imajte na umu da to može biti potencijalno Neučinkovit jer ga dohvaća sve zapise na svaki istaknuti postupak.

5. Aplikacija sinkronizira cilj C kada ne možemo izmijeniti ili dodavanja podataka korisnik izvodi gesta osvježavanja i vrpce. Aplikaciju Swift sinkronizira kada korisnik izvodi gesta osvježavanja i vrpce. 

Jer sinkronizira aplikacije kad god se podaci izmijenio (cilj-C) ili pri svakom pokretanju aplikacije (cilj C i Swift), aplikaciju pretpostavlja da je korisnik online. U drugom dijelu ćemo ažurirati aplikaciju da bi korisnici mogli uređivati čak i kad su izvan mreže.

## <a name="review-core-data"></a>Pregled Core podatkovnog modela

Prilikom korištenja temeljni podaci izvanmrežno spremište potrebno definirati određeni tablica i polja u podatkovnom modelu. Aplikaciju uzorka već sadrži podatkovni model s odgovarajući oblik. U ovom odjeljku ne možemo će voditi kroz ove tablice i kako se koriste.

- Otvorite **QSDataModel.xcdatamodeld**. Postoje četiri tablice definirani – tri koje se koriste u SDK i jednu tablicu za na popis obveza stavki same:     *MS_TableOperations: za praćenje stavki koje su potrebne za sinkronizaciju s poslužiteljem    * MS_TableOperationErrors: radi evidencije pogrešaka koje će se dogoditi prilikom izvanmrežne sinkronizacije     *MS_TableConfig: za praćenje zadnje ažuriranje vremena posljednje operacije sinkronizacije za sve operacije istaknuti    * TodoItem : Za pohranu stavki obveze. Sustav stupaca **createdAt**, **updatedAt**i **verzija** su svojstva sustava nije obavezno.

>[AZURE.NOTE] Azure mobilne aplikacije SDK rezervira nazivi stupaca se s "**``**". Nemojte koristiti ovaj prefiks na bilo koji nije sistemski stupaca, u suprotnom će izmjene naziva stupaca prilikom korištenja udaljene pozadinskog.

- Prilikom korištenja značajke izvanmrežne sinkronizacije, morate definirati tablica sustava kao što je prikazano u nastavku.

    ### <a name="system-tables"></a>Tablice sustava

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Atribut  |    Vrsta     |
  	|----------- |   ------    |
  	| ID-a         | Cijeli broj 64  |
  	| ID stavke     | Niz      |
  	| Svojstva | Binarni podaci |
  	| Tablica      | Niz      |
  	| tableKind  | Cijeli broj 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Atribut  |    Vrsta     |
  	|----------- |   ------    |
  	| ID-a         | Niz      |
  	| operationId | Cijeli broj 64 |
  	| Svojstva | Binarni podaci |
  	| tableKind  | Cijeli broj 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Atribut  |    Vrsta     |
  	|----------- |   ------    |
  	| ID-a         | Niz      |
  	| ključ        | Niz      |
  	| keyType    | Cijeli broj 64  |
  	| Tablica      | Niz      |
  	| vrijednost      | Niz      |

    ### <a name="data-table"></a>Podatkovna tablica

    **TodoItem**

  	| Atribut    |  Vrsta   | Napomena                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| ID-a           | Niz, označen kao obavezan  | primarni ključ u udaljenog spremišta                            |
  	| Dovršavanje     | Booleove vrijednosti | polje stavke na popis obveza                                        |
  	| tekst         | Niz  | polje stavke na popis obveza                                        |
  	| createdAt | Datum    | (neobavezno) karte da biste svojstvo createdAt sustava         |
  	| updatedAt | Datum    | (neobavezno) karte da biste svojstvo updatedAt sustava         |
  	| verzija   | Niz  | (neobavezno) koristiti da biste otkrili sukobe, karte verziju |


## <a name="setup-sync"></a>Promjena ponašanja sinkronizaciju aplikacije

U ovom ćete odjeljku će promijeniti aplikaciju da bi sinkronizacija pri otvaranju aplikacije ili prilikom umetanja i ažuriranje stavki, ali samo kada se izvodi gesta gumb Osvježi.

**Cilj C**:

1. U **QSTodoListViewController.m**, promijeniti način **viewDidLoad** da biste uklonili poziv `[self refresh]` na kraju način. Sada će se sinkronizirati s poslužiteljem pri otvaranju aplikacije podatke, ali umjesto toga će se sadržaj lokalno spremište.

2. U **QSTodoService.m**, izmijenite definiciju `addItem` tako da se ne sinkronizira nakon što umetnete stavku. Uklanjanje na `self syncData` blokirati, a zatim zamijenite na sljedeći način:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Izmjena definicija `completeItem` kao prethodna; Uklanjanje bloka za `self syncData` i zamijeniti na sljedeći način:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. U `viewDidLoad` u **ToDoTableViewController.swift**komentar izgleda te dva retka, a da biste prekinuli sinkronizaciju pri otvaranju aplikacije. U vrijeme pisanja ovaj članak aplikaciju Swift obveze ne ažuriraju servis kada netko doda ili dovršava stavku, samo na Pokreni aplikaciju.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Testiranje aplikacije

U ovom ćete odjeljku će se povezati koji nisu valjani URL-a u programu Publisher izvanmrežni scenarij. Kada dodate stavke podataka, oni će biti sadrži lokalno spremište temeljni podaci, ali nisu sinkronizirani s mobilnim pozadinskog.

1. Promijenite URL mobilne aplikacije u **QSTodoService.m** koji nisu valjani URL pa ponovno pokrenite aplikaciju:

    **Cilj C** u QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** u ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Dodavanje neke stavke obveze. Zatvorite simulator (ili Prisilno zatvaranje aplikacija) i ponovno ga pokrenite. Provjerite da su promjene dosljedan.

3. Prikaz sadržaja udaljene TodoItem tablice:

    + Za s pozadinskom Node.js idite na [portal za Azure](https://portal.azure.com/)i u mobilnu aplikaciju pozadinskog kliknite **Jednostavno tablice** > **TodoItem** da biste pogledali sadržaj na `TodoItem` tablice.
    + S pozadinskom .NET pregledati sadržaj tablice pomoću alata za SQL kao što je SQL Server Management Studio ili OSTALE klijent kao što su Fiddler ili Postman.

    Provjerite nove stavke *nisu* sinkronizirane s poslužiteljem:

4. Promijeniti URL natrag ispravno na u **QSTodoService.m** i ponovno pokrenite aplikaciju. Izvođenje gesta osvježavanja po izvlačenja prema dolje po popisu stavki. Prikazat će se Okretni gumb napredak.

5. Ponovno pogledati TodoItem podataka. Nove i promijenjene TodoItems sad trebao izgledati.

## <a name="summary"></a>Sažetak

Da bi se podržava značajku izvanmrežnu sinkronizaciju, koristi u `MSSyncTable` sučelje i pokrenuti `MSClient.syncContext` s lokalnom spremištu. U ovom slučaju lokalno spremište je baza podataka utemeljen na temeljni podaci.

Kada se koristi lokalno spremište temeljni podaci, morate definirati nekoliko tablica sa [svojstvima točan sustava](#review-core-data).

Normalni CRUD postupci za Azure mobilne aplikacije funkcioniraju kao aplikaciju i dalje povezani, ali sve operacije pojaviti protiv lokalne pohrane.

Kada željeli ne možemo sinkronizirati lokalno spremište s poslužiteljem, koristi u `MSSyncTable.pullWithQuery`način.


## <a name="additional-resources"></a>Dodatni resursi

* [Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]

* [Naslovnice u oblaku: izvanmrežnu sinkronizaciju u Azure mobilne usluge] \(Napomena: videozapis je mobilne usluge, ali Izvanmrežna sinkronizacija funkcionira na isti način u Azure mobilne aplikacije\)

<!-- URLs. -->


[Stvaranje iOS aplikacija]: app-service-mobile-ios-get-started.md
[Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Oblak naslovnice: Izvanmrežnu sinkronizaciju u Azure mobilne usluge]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
