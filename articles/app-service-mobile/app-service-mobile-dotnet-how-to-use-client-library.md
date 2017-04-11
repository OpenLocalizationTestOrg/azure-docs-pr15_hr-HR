<properties
    pageTitle="Rad s bibliotekom upravljanih klijentskih aplikacija mobilne usluge (Windows | Xamarin) | Microsoft Azure"
    description="Saznajte kako koristiti .NET klijenta za Azure servisa mobilna aplikacija s aplikacijama sustava Windows i Xamarin."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Kako koristiti upravljane klijent za Azure mobilne aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Pregled

Ovaj vodič prikazuje kako izvoditi uobičajene scenarije koristi biblioteka upravljanih klijenta za Azure aplikacija usluga mobilne aplikacije za Windows i Xamarin aplikacije. Ako ste novi korisnik aplikacije Mobile, trebali biste najprije dovršavanje [Azure mobilne aplikacije brzi početak rada] [ 1] vodič. U ovom vodiču smo fokus na klijentsko upravljanih SDK-a. Dodatne informacije o poslužiteljsko SDK-ovi za mobilne aplikacije potražite u dokumentaciji za [.NET Server SDK] [ 2] ili [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Dokumentacija reference

Klijent SDK potražite u dokumentaciji referenca nalazi se ovdje: [Referenca klijent Azure mobilne aplikacije .NET][4].
Nekoliko primjera klijenta možete pronaći i u [spremištu Azure uzoraka GitHub][5].

## <a name="supported-platforms"></a>Podržane platforme

.NET platformu podržava sljedeće platforme:

* Xamarin Android izdanja za API 19 do 24 (KitKat do Nougat)
* Xamarin iOS izdanja za iOS 8.0 i novije verzije
* Univerzalni Windows platforma
* Windows Phone 8.1
* Windows Phone 8.0 osim Silverlight aplikacija

Provjera autentičnosti "poslužitelj tijek" koristi s prikaza za izloženi korisničkog Sučelja.  Ako je uređaj neće moći prikazati korisničko Sučelje prikaza, potrebne su druge načine provjere autentičnosti.  Taj SDK nije stoga prikladan za nadzor vrsta ili na sličan način ograničena uređaje.

##<a name="setup"></a>Postavljanje i preduvjeti

Pretpostavimo da imate li već stvorili i objaviti mobilnu aplikaciju pozadinskog projekta, koji sadrži najmanje jednu tablicu.  U kod koji se koristi u ovoj temi, tablice pod nazivom `TodoItem` i ima sljedeće stupce: `Id`, `Text`, i `Complete`. Ta je tablica u istu tablicu stvorili kada završite s [Azure mobilne aplikacije brzi početak rada].

Vrsta upisani odgovarajuće klijentsko u C# je klasu sljedeće:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

[JsonPropertyAttribute] [ 6] koristi se za definiranje *PropertyName* mapiranja između vrsta klijenta i tablice.

Da biste saznali kako stvoriti tablice u vašem pozadinskog mobilne aplikacije, pogledajte informacije u [temi .NET Server SDK] [ 7] ili [Node.js Server SDK temi][8]. Ako ste stvorili u pozadinskom mobilnu aplikaciju na portalu Azure pomoću za brzi početak rada, možete koristiti i postavku **jednostavno tablice** [Azure portal].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Kako: instalirajte paket SDK upravljanih klijenta

Koristite neku od sljedećih metoda da biste instalirali paket SDK upravljanih klijenta za mobilne aplikacije iz [NuGet][9]:

+ **Visual Studio** Desnom tipkom miša kliknite projekt, kliknite **Upravljanje NuGet paketa**, potražite na `Microsoft.Azure.Mobile.Client` paket, a zatim kliknite **Instaliraj**.

+ **Xamarin Studio** Desnom tipkom miša kliknite projekt, kliknite **Dodaj** > **Dodavanje NuGet paketa**, okvir za pretraživanje u `Microsoft.Azure.Mobile.Client `paket, a zatim kliknite **Dodaj paketa**.

U glavnom aktivnosti datoteci, imajte na umu da biste dodali sljedeću naredbu **pomoću** :

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Kako: rad sa simbolima za ispravljanje pogrešaka u Visual Studio

Simboli za prostor za naziv Microsoft.Azure.Mobile dostupne su na [SymbolSource][10].  Pogledajte [upute za SymbolSource] [ 11] za integraciju SymbolSource s Visual Studio.

##<a name="create-client"></a>Stvaranje klijentskog programa mobilne aplikacije

Sljedeći kod stvara [MobileServiceClient] [ 12] objekt koji se koristi za pristup vašem pozadinskog mobilnu aplikaciju.

    var client = new MobileServiceClient("MOBILE_APP_URL");

U prethodnom kod zamijenite `MOBILE_APP_URL` u URL pozadinskog mobilnu aplikaciju koji se nalazi u plohu za mobilnu aplikaciju pozadinskog sustava [Azure portal]. Objekt MobileServiceClient mora biti u jednočlana.

## <a name="work-with-tables"></a>Rad s tablicama

U sljedećoj sekciji detaljno pretraživanje i učitavanje zapisa i izmjenu podataka unutar tablice.  Možete pronaći u sljedećim temama:

* [Stvaranje reference tablice](#instantiating)
* [Upit za podatke](#querying)
* [Filtriranje vraćenih podataka](#filtering)
* [Sortiranje vraćenih podataka](#sorting)
* [Vraćanje podataka na stranicama](#paging)
* [Odabrati određene stupce](#selecting)
* [Traženje zapisa po ID-a](#lookingup)
* [Rješavanje problema s untyped upitima](#untypedqueries)
* [Umetanje podataka](#inserting)
* [Ažuriranje podataka](#updating)
* [Brisanje podataka](#deleting)
* [Rješavanje sukoba i optimistična Procjena istodobnosti](#optimisticconcurrency)
* [Povezivanje s korisničko sučelje sustava Windows](#binding)
* [Promjena veličine stranice](#pagesize)

###<a name="instantiating"></a>Kako: Stvaranje referenci tablice

Kod koji se može pristupiti ili je pobliže podataka u tablici pozadinskog poziva funkcije na u `MobileServiceTable` objekt. Dobiti referencu za tablicu tako da nazovete metodu [GetTable] na sljedeći način:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Vraćeni objekt koristi ogledni model upisani serijalizacije. Podržano je i modelu untyped serijalizacije. Na sljedeći primjer [stvara referenca untyped tablice]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

U untyped upita, morate navesti podlozi OData niza upita.

###<a name="querying"></a>Kako: podaci iz mobilne aplikacije za upite

U ovom se odjeljku opisuje kako izdati upite za mobilnu aplikaciju pozadinski, što obuhvaća sljedeće funkcije:

- [Filtriranje vraćenih podataka](#filtering)
- [Sortiranje vraćenih podataka](#sorting)
- [Vraćanje podataka na stranicama](#paging)
- [Odabrati određene stupce](#selecting)
- [Traženje podataka po ID-a](#lookingup)

>[AZURE.NOTE]Da biste spriječili da svi reci se vraćaju se provodi veličine stranice utemeljenih na poslužitelju.  Broja stranica zadržava zadani zahtjeva za velikih skupova podataka iz negativno koje utječu na servis.  Da biste vratili više od 50 redaka, koristite na `Skip` i `Take` način, kao što je opisano u [povrata podataka na stranicama].

###<a name="filtering"></a>Kako: filtar vraćenih podataka

Sljedeći kod da biste filtrirali podatke, uključujući ilustrira na `Where` uvjet u upitu. Vraća sve stavke iz `todoTable` čije `Complete` svojstvo jednak je `false`. Funkcija [gdje] primjenjuje retka filtriranje predikata upit na tablici.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

Možete pogledati URI zahtjeva poslane pozadinski pomoću poruka provjere softvera, kao što su Alati za razvojne inženjere preglednika ili [Fiddler]. Ako pogledate URI zahtjeva, obratite pozornost na to da je izmijenjena nizu upita:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

Zahtjev za OData prevede SQL upita prema SDK poslužitelja:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

Funkcija koja se prenosi u `Where` način može imati proizvoljne broj uvjeta.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

U ovom primjeru želite prevesti u SQL upita prema SDK poslužitelja:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Ovaj upit i može se podijeliti na više uvjeta:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

Dva načina se ne razlikuju i može se koristiti naizmjence.  Mogućnost bivši&mdash;od Ulančavanje više predikati u jednom upitu&mdash;je sažetom i preporučena.

Na `Where` uvjet podržava operacije koje je moguće prevesti u OData podskup. Operacije obuhvaćaju sljedeće:

* Relacijske operatore (==,! =, <, < =, >, > =),
* Aritmetičke operatore (+, -, /, *, %),
* Broj preciznost (Math.Floor, Math.Ceiling)
* Funkcije niza (duljina, podniz, Zamijeni, IndexOf, StartsWith, EndsWith)
* Svojstva datuma (godini, mjesecu, dan, sat, minuta, sekundi),
* Pristup svojstva objekta, a
* Kombiniranje bilo koju od tih operacija izraza.

Kada odlučuje što Server SDK podržava, uzmite u obzir u [dokumentaciji za OData v3].

###<a name="sorting"></a>Kako: sortiranja vraćenih podataka

Sljedeći kod prikazuje kako sortirati podatke tako da uvrstite funkciji [OrderBy] ili [OrderByDescending] u upitu. Vraća stavke iz `todoTable` Sortirano uzlazno po na `Text` polja.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Kako: vratiti podatke na stranicama

Prema zadanim postavkama pozadinski prikazuje samo prvih 50 redaka. Možete povećati broj vraćenih redaka tako da nazovete metodu [poduzeti] . Korištenje `Take` uz [Preskoči] načina za upućivanje zahtjeva za određenu "stranica" od ukupne dataset vratio upit. Sljedeći upit pri izvođenju vraća prvih tri stavki u tablici.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Sljedeći upit promijenjenih preskače prva tri rezultata i vraća sljedeća tri rezultate. Ovaj upit proizvodi druga "stranica" podataka, pri čemu je veličina stranice tri stavke.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Način [IncludeTotalCount] zahtjeve za razgovore ukupan broj za _sve_ zapise koje želite vraćenim, zanemarujući bilo koji uvjet/ograničenje označavanja stranica navedena:

    query = query.IncludeTotalCount();

U aplikaciji stvarnog svijeta poslužite slično kao u prethodnom primjeru upiti s Dojavljivač kontrolu ili usporediti korisničkog Sučelja za kretanje među stranicama.

>[AZURE.NOTE]Da biste nadjačali ograničenje 50 redaka u mobilnu aplikaciju pozadinskog, morate primijeniti [EnableQueryAttribute] javno metodu GET i odredite ponašanje broja stranica. Kada se primjenjuje na način sljedeće postavlja maksimalni broj vraćenih redaka 1000:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Kako: odabrati određene stupce

Možete odrediti koji skup svojstva da biste dodali u rezultate dodavanjem [Odaberite] uvjet u upit. Na primjer, sljedeći kod prikazuje kako odabrati samo jedno polje te kako označiti i oblikovati više polja:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Sve funkcije dosad opisane su mogu dodavati, pa ne možemo možete zadržati Ulančavanje ih. Svaki ulančana poziv utječe na više upita. Jedan primjer:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Kako: traženje podataka po ID-a

Funkcija [LookupAsync] koje se može koristiti da biste potražili objekte iz baze podataka s određenom ID-a.

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Kako: izvršavanje untyped upita

Prilikom izvršavanja upita pomoću objekta untyped tablice izričito morate navesti nizu upita OData tako da nazovete [ReadAsync], kao u sljedećem primjeru:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Što se vratite JSON vrijednosti koje možete koristiti kao što su spremnika svojstava. Dodatne informacije o JToken i Newtonsoft Json.NET potražite u članku [Json.NET] web-mjesta.

### <a name="inserting"></a>Kako: Umetanje podataka pozadinskog za mobilnu aplikaciju

Sve vrste klijenta mora sadržavati člana pod nazivom **Id**koji je po zadanom niza. Za izvođenje operacija CRUD potreban je **Id** za izvanmrežnu sinkronizaciju. Sljedeći kod prikazuje kako koristiti metodu [InsertAsync] za umetanje novih redaka u tablicu. Parametar sadrži podatke koje želite umetnuti kao objekt .NET.

    await todoTable.InsertAsync(todoItem);

Ako prilagođene jedinstveni ID vrijednosti nije obuhvaćen u `todoItem` tijekom Umetanje, GUID generira poslužitelj.
Provjerom objekt nakon vraća poziv možete dohvatiti generirani ID-a.

Da biste umetnuli untyped podataka, možda iskoristite prednost Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Slijedi primjer pomoću adrese e-pošte kao jedinstveni niz id:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Rad s ID vrijednosti

Mobilna aplikacija podržava jedinstveni prilagođen niz vrijednosti za stupac **id** tablice. Vrijednost niza omogućuje aplikacijama za korištenje prilagođenih vrijednosti kao što su adrese e-pošte ili imena korisnika za u ID-a.  Niz ID-a sadrže sljedeće prednosti:

* ID-a generiraju bez trajanjem putovanja u bazu podataka.
* Zapisi koji su lakše spojiti iz različitih tablica ili baze podataka.
* ID-ove vrijednosti bolje možete integrirati s logike aplikacije.

Kada je vrijednost niza ID nije postavljen na umetnute zapis, pozadinski mobilnu aplikaciju generira jedinstvene vrijednosti za u ID-a. Način [Guid.NewGuid] možete koristiti za stvaranje vlastitog ID vrijednosti, na klijentskom računalu ili u pozadinski.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Kako: mijenjati podatke u mobilnu aplikaciju pozadinskog

Sljedeći kod prikazuje kako koristiti metodu [UpdateAsync] da biste ažurirali postojeći zapis iste ID-ja s novim podacima. Parametar sadrži podatke koje želite ažurirati kao objekt .NET.

    await todoTable.UpdateAsync(todoItem);

Da biste ažurirali untyped podataka, možda iskoristite prednost [Json.NET] na sljedeći način:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

Na `id` polja moraju biti navedeni pri upućivanje ažuriranje. Koristi pozadinski na `id` polja da biste odredili koji redak da biste ažurirali. Na `id` polja možete preuzeti s rezultat u `InsertAsync` poziva. Na `ArgumentException` podignut ako ste pokušali ažurirati stavke bez osiguravanja na `id` vrijednost.

###<a name="deleting"></a>Kako: brisanje podataka u mobilnu aplikaciju pozadinskog

Sljedeći kod prikazuje kako koristiti metodu [DeleteAsync] da biste izbrisali postojeću instance. Instancu otkrije na `id` skupu polja na na `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Da biste izbrisali untyped podataka, možda iskoristite prednost Json.NET na sljedeći način:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Kada odaberete željene zahtjeva za brisanje, ID mora biti navedena. Druga svojstva nije prošao usluzi ili zanemaruju se na servis. Rezultat je `DeleteAsync` poziv je obično `null`. ID za prosljeđivanje u možete preuzeti s rezultat u `InsertAsync` poziva. A `MobileServiceInvalidOperationException` izbačena kada pokušate izbrisati stavku bez navođenja na `id` polja.

###<a name="optimisticconcurrency"></a>Kako: korištenje optimistična Procjena istodobnosti za rješavanje sukoba

Dva ili više klijenata možda pisati promjene istu stavku u isto vrijeme. Bez otkrivanje sukoba posljednje pisanje će prebrisati prethodna ažuriranja. **Optimistična Procjena istodobnosti kontrola** pretpostavlja da pojedine transakcije možete izvršiti i zbog toga ne koristi sve zaključavanje resursa.  Prije potvrđivanja transakcije optimistična Procjena istodobnosti kontrola potvrđuje bez transakcije je promijenio podatke. Ako je promijenjena podatke, committing transakcije je vraćen.

Mobilne aplikacije podržava optimistična Procjena istodobnosti kontrola tako da svaku stavku pomoću evidentiranja promjena u `version` stupac svojstva sustava koji je definiran za svaku tablicu u vašem pozadinskog mobilnu aplikaciju. Prilikom svakog ažuriranja zapisa, mobilne aplikacije postavlja na `version` svojstvo za taj zapis za novu vrijednost. Prilikom svakog ažuriranja zahtjeva za `version` svojstvo zapis koji je uključen u zahtjevu za uspoređuje se na istom svojstvo za zapis na poslužitelju. Ako verzija proslijeđena s zahtjev nisu u skladu s pozadinskom, a zatim pokreće klijentska biblioteka na `MobileServicePreconditionFailedException<T>` iznimke. Vrsta uključene uz iznimku je zapis iz pozadine koja sadrži verziju poslužitelja zapisa. Aplikaciju možete koristiti taj podatak odlučite želite li izvršavanja zahtjeva za ažuriranje ponovno s točno `version` vrijednost iz pozadine da biste potvrdili promjene.

Definiranje stupac na klasa tablice za na `version` svojstvo sustava da biste omogućili optimistična Procjena istodobnosti. Ako, na primjer:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Aplikacije koje se koristi untyped tablice omogućuju optimistična Procjena istodobnosti tako da postavite na `Version` označite zastavicom prilikom na `SystemProperties` tablice na sljedeći način.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Osim omogućavanja optimistična Procjena istodobnosti, koje morate i Uhvatite na `MobileServicePreconditionFailedException<T>` iznimke u kod prilikom pozivanja [UpdateAsync].  Rješavanje sukoba primjenom ispravno `version` ažurirani zapis i poziv [UpdateAsync] u zapisu riješi. Sljedeći kod prikazuje način za rješavanje sukoba pisanje jednom otkriven:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Dodatne informacije u sintaksi [Izvanmrežne sinkronizacije podataka u Azure mobilne aplikacije] .

###<a name="binding"></a>Kako: vezanja mobilne aplikacije podatke u korisničkom sučelju sustava Windows

U ovom se odjeljku objašnjava objekte vraćenih podataka pomoću elemente korisničkog Sučelja u aplikaciji Windows prikazati.  Sljedeći primjer kod povezuje izvorišnog popisa s upitom za nepotpuna stavke. [MobileServiceCollection] stvara mobilne aplikacije-umu zbirku povezivanja.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Neke se kontrole upravljanih runtime podržava sučelje naziva [ISupportIncrementalLoading]. Ovo sučelje omogućuje kontrole da biste zatražili dodatni podaci kada korisnik pomiče. Nema ugrađenu podršku za ovo sučelje za univerzalni aplikacije Windows putem [MobileServiceIncrementalLoadingCollection], koji se automatski rukuje pozive iz kontrole. Korištenje `MobileServiceIncrementalLoadingCollection` u aplikacijama za Windows na sljedeći način:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Da biste koristili novu zbirku na Windows Phone 8 i "Silverlight" aplikacije, koristite na `ToCollection` načina nastavka na `IMobileServiceTableQuery<T>` i `IMobileServiceTable<T>`. Da biste učitali podatke, nazovite `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Kada koristite zbirke stvoriti tako da nazovete `ToCollectionAsync` ili `ToCollection`, dobijete zbirke koji mogu biti povezane kontrole korisničkog Sučelja.  Ova zbirka je umu broja stranica.  Budući da zbirka je učitavanja podataka iz mreže, učitavanje ponekad ne uspijeva. Rukovanje takvih pogrešaka nadjačati na `OnException` način na `MobileServiceIncrementalLoadingCollection` učiniti iznimke dobivenima pozive na `LoadMoreItemsAsync`.

Preporučujemo ako tablica ima mnogo polja, ali samo želite prikazati neke od njih u kontroli. Možete u prethodnom odjeljku "[odabrati određene stupce](#selecting)" koristiti smjernice za odabir određenih stupaca da biste prikazali u korisničkom Sučelju.

###<a name="pagesize"></a>Promjena veličine stranice

Azure mobilne aplikacije vraća najviše 50 stavki po zahtjev prema zadanim postavkama.  Možete promijeniti veličinu broja stranica povećavanjem maksimalnu veličinu stranice na postupak klijentske i poslužiteljske.  Da biste povećali veličinu tražene stranice, navedite `PullOptions` prilikom korištenja `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Ako ste u `PageSize` jednaka ili veća od 100 unutar poslužitelja zahtjeva vraća do 100 stavki.

##<a name="#offlinesync"></a>Rad s tablicama izvan mreže

Izvanmrežna tablica pomoću lokalnih podataka iz trgovine za spremište SQLite za upotrebu u izvanmrežnom načinu rada.  Sve tablice operacije gotovi protiv lokalnoj SQLite pohranite umjesto spremište udaljeni poslužitelj.  Da biste stvorili Izvanmrežna tablica, pripremite projekta:

1. U Visual Studio, desnom tipkom miša kliknite rješenje > **Upravljanje NuGet paketa rješenja...**, zatim okvir za pretraživanje i instalaciju paketa NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** za svim projektima rješenja.

2. (Neobavezno) Da biste podržava uređaje sa sustavom Windows, instalirajte jedan od sljedećih paketa runtime SQLite:

    * **Runtime Windows 8.1:** Instalacija [SQLite Windows 8.1][3].
    * **u sustavu windows Phone 8.1:** Instalacija [SQLite za Windows Phone 8.1][4].
    * **Univerzalni Windows platforma** Instalacija [SQLite za univerzalno univerzalni Windows][5].

3. (Neobavezno). Za uređaje sa sustavom Windows, kliknite **reference** > **Dodavanje referenca...**, proširite mapu **sustava Windows** > **proširenja**, a zatim Omogući odgovarajući **SQLite za Windows** SDK uz **Visual C++ 2013 Runtime za Windows** SDK.
    Nazive SQLite SDK malo razlikovati sa svakom platformu sustava Windows.

Prije stvaranja referenci tablice lokalno spremište potrebno pripremiti:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Pokretanje spremište obavlja obično odmah nakon stvaranja klijent.  **OfflineDbPath** mora biti naziv datoteke prikladna za upotrebu na svim platformama pružate podršku.  Ako puni put je put (to jest, započinje s kosom crtom), a zatim koristi taj put.  Ako put nije potpuno kvalificiran, datoteka se smješta na mjestu ovisne.

* Za iOS i uređaje sa sustavom Android, zadani put je u mapu "Osobne datoteke".
* Za uređaje sa sustavom Windows, zadani put je u mapu "AppData" specifičnim aplikacijama.

Referenci tablice se možete dobiti korištenjem na `GetSyncTable<>` metoda:

    var table = client.GetSyncTable<TodoItem>();

Ne morate provjeriti autentičnost koristiti izvanmrežno tablicu.  Trebate samo kada su komunikaciju s pozadinskom servis za provjeru autentičnosti.

###<a name="syncoffline"></a>Sinkroniziranje Izvanmrežna tablica

Izvanmrežna tablica neće se sinkronizirati s pozadinski prema zadanim postavkama.  Sinkronizacija je podijeljen u dva.  Preuzimanje nove stavke možete automatske promjene zasebno.  Evo način uobičajeni sinkronizacije:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
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

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
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

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Ako je prvi argument `PullAsync` je null, a zatim rastuća sinkronizaciju ne koristi.  Svaka operacija sinkronizaciju dohvaća sve zapise.

SDK izvodi implicitnog `PushAsync()` prije izvlačenja zapisa.

Sukob rukovanje događajima na na `PullAsync()` način.  Na isti način kao tablice online možete baviti sukobe.  Sukob je sastavio kada `PullAsync()` zove umjesto tijekom Umetanje, ažuriranje i brisanje. Ako slučajno više sukoba, oni se vezanoj instalaciji u jednom MobileServicePushFailedException.  Držač za svaku pogrešku zasebno.

##<a name="#customapi"></a>Rad s prilagođene API-JA

Prilagođeni API omogućuje vam da biste odredili prilagođene krajnje točke koji izlažu funkcionalnosti poslužitelja koji ne mapiranje umetnut, ažuriranje, brisanje i pročitajte operacija. Pomoću prilagođenih API-JA možete imati više kontrole nad poruke, uključujući čitanja i postavljanje HTTP zaglavlja poruka i definiranje obliku tijela poruke osim JSON.

Nazovite prilagođene API tako da nazovete jedan od načina [InvokeApiAsync] na klijentskom računalu. Ako, na primjer, sljedeći redak koda šalje zahtjev za objavu na **completeAll** API-JA na pozadinski:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Ovaj obrazac za pozive upisani način i potreban je definirana vrsta povrata **MarkAllResult** . Podržane su upisani i untyped metode.

##<a name="authentication"></a>Provjeru autentičnosti korisnika

Mobilna aplikacija podržava provjere autentičnosti i dopuštanja korisnici aplikacije pomoću davatelja usluge za različite vanjskih identiteta: Facebook, Google, Microsoftov Account, Twitter i Azure Active Directory. Postavljanje dozvola za tablice ograničavanja pristupa za određene operacije samo korisnicima čija je autentičnost provjerena. Identitet korisnika čija je autentičnost provjerena možete koristiti i za primjenu pravila za provjeru autentičnosti u skriptama poslužitelja. Dodatne informacije potražite u članku vodič [Dodaj provjere autentičnosti za aplikaciju].

Podržane su dvije tokova provjere autentičnosti: tijek _upravljanjem klijenta_ i _poslužitelja upravlja_ . Tijek poslužitelja upravlja nudi najjednostavniji sučelje za provjeru autentičnosti kao ovisi vašeg davatelja usluge web-sučelja za provjeru autentičnosti. Tijek klijent upravlja omogućuje dublju Integracija s mogućnosti specifične za uređaj kao ovisi SDK-ovi za specifične za uređaj karakteristične za davatelja.

>[AZURE.NOTE] Preporučujemo korištenje tijek za klijent upravlja u aplikacijama za proizvodnju.

Da biste postavili provjeru autentičnosti, morate se registrirati aplikacije s davatelji identiteta.  Davatelja identiteta generira ID klijenta i tajna klijenta za aplikacije.  Ove vrijednosti pa se postavljaju na pozadinskog da biste omogućili provjere autentičnosti autorizacije aplikacije servisa za Azure.  Dodatne informacije potražite u vodiču [Dodaj provjere autentičnosti za aplikaciju]slijedite detaljne upute.

U ovom odjeljku možete pronaći u sljedećim temama:

+ [Klijent upravlja provjere autentičnosti](#clientflow)
+ [Poslužitelj upravlja provjere autentičnosti](#serverflow)
+ [Predmemoriranje token za provjeru autentičnosti](#caching)

###<a name="clientflow"></a>Klijent upravlja provjere autentičnosti

Aplikacije možete nezavisno obratite davatelja identiteta i omogućuju vraćeni token tijekom prijavljivanja s vašeg pozadinskom. Ovaj tijek klijent omogućuje vam omogućuje na jednom za prijave za korisnike ili radi dohvaćanja dodatnih korisničkih podataka od davatelja identiteta. Provjera autentičnosti klijenta tijek je Preferirani pomoću poslužitelja tijek davatelja identiteta SDK pruža više nativni UX dojam i omogućuje dodatne prilagodbe.

Primjeri služe za sljedeće provjere autentičnosti uzorke klijent tijek:

+ [Provjera autentičnosti biblioteke imenika Active Directory](#adal)
+ [Web-mjesto Facebook ili u okvir za Google](#client-facebook)
+ [Live SDK](#client-livesdk)

#### <a name="adal"></a>Provjere autentičnosti korisnika završavaju na provjera autentičnosti biblioteke imenika Active Directory

U Active Directory provjera autentičnosti biblioteke (ADAL) možete koristiti za provjeru autentičnosti započeti korisnika putem klijentskog programa pomoću provjere autentičnosti Azure Active Directory.

1. Konfiguriranje mobilne aplikacije pozadinskog sustava za prijavu AAD slijedeći [upute za konfiguriranje aplikacije servisa za prijavu na servisu Active Directory] vodič. Provjerite jeste li dovršili neobavezan korak registriranja aplikacija za nativni klijent.
2. U Visual Studio ili Xamarin Studio, otvorite projekt i dodajte referenca na `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet paketa. Prilikom pretraživanja, obuhvaćaju Neslužbene verzije.
3. Dodavanje koda za sljedeće aplikacije, prema platformu koristite. U svakoj, provjerite sljedeće zamjena:

    * **Umetanje IZDAVANJE ovdje** zamijenite nazivom klijenta dodjeli aplikacije. Oblik mora biti https://login.windows.net/contoso.onmicrosoft.com. Ta vrijednost mogu kopirati na kartici domena u vašem Azure Active Directory [Azure klasični portal].
    * **Umetanje-RESURSA – ID-ovdje** zamijenite ID klijenta za vaše pozadinskog mobilne aplikacije. ID klijenta možete dobiti na kartici **Dodatno** u odjeljku **Azure Active Directory postavke** na portalu.
    * **Umetanje-KLIJENT-ID-ovdje** zamijenite ID klijenta koji ste kopirali iz aplikacije za nativni klijent.
    * **Umetanje-PREUSMJERAVANJE-URI-ovdje** zamijenite krajnje _/.auth/login/done_ web-mjesta, pomoću HTTPS shemu. Ta vrijednost mora biti sličan _https://contoso.azurewebsites.net/.auth/login/done_.

    Šifra koja su potrebna za svaki platformu slijedi:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Jedinstvenu prijavu pomoću tokena iz servisa Facebook ili Google

Tijek klijenta možete koristiti kao što je prikazano u ovom isječak za Facebook ili Google.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Jedan prijaviti pomoću Microsoftova Account Live SDK

Za provjeru autentičnosti korisnika, morate se registrirati aplikacije na Microsoftov račun centar za razvojne inženjere. Konfiguriranje detalje za registraciju na vašem pozadinskog mobilnu aplikaciju. Da biste stvorili Registracija računa za Microsoft i povežite ga s vašeg pozadinskog mobilnu aplikaciju, slijedite korake u [dnevniku aplikacije za korištenje programa Microsoft Prijava na račun]. Ako ste instalirali iz Windows trgovine i Windows Phone 8/Silverlight verzije aplikacije, najprije registrirati na verziju iz Windows trgovine.

Sljedeći kod potvrđuje pomoću Live SDK i koristi vraćeni token za prijavu u pozadinskom mobilnu aplikaciju.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Dodatne informacije potražite u dokumentaciji za [Windows Live SDK] .

###<a name="serverflow"></a>Poslužitelj upravlja provjere autentičnosti

Nakon što ste se registrirali davatelja identiteta, nazovite način [LoginAsync] na [MobileServiceClient] s vrijednošću [MobileServiceAuthenticationProvider] davatelja usluga. Na primjer, sljedeći kod pokreće na poslužitelju tijek prijavu pomoću servisa Facebook.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Ako koristite davateljem identiteta osim servisa Facebook, promijenite vrijednost [MobileServiceAuthenticationProvider] vrijednosti za davatelja usluga.

U tijeku s poslužitelja aplikacije servisa za Azure upravlja provjere autentičnosti tijek OAuth prikazom stranica za prijavu u odabranog davatelja.  Kada se vraća davatelja identiteta, aplikacije servisa za Azure stvara sustava token aplikacije servisa za provjeru autentičnosti. Način [LoginAsync] vraća na [MobileServiceUser], koji pruža i [korisnički ID] korisnika čija je autentičnost provjerena i [MobileServiceAuthenticationToken]kao JSON web token (JWT). Ovaj token možete predmemorirani i ponovno iskoristiti sve dok ne što istekne. Dodatne informacije potražite u članku [predmemoriranje token za provjeru autentičnosti](#caching).

###<a name="caching"></a>Predmemoriranje token za provjeru autentičnosti

U nekim slučajevima poziv na način prijave mogu se izbjegavati nakon prve uspjela provjera autentičnosti spremanjem token za provjeru autentičnosti davatelja.  Aplikacije iz Windows trgovine i UWP možete koristiti [PasswordVault] u predmemoriju trenutni token za provjeru autentičnosti nakon je uspješno prijavu, na sljedeći način:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

Korisnički ID vrijednost se sprema kao UserName vjerodajnicu i token je spremljena kao lozinka. Sljedeći start-ups možete provjeriti **PasswordVault** predmemorirane vjerodajnice. Sljedeći primjer koristi predmemorirane vjerodajnice kada se nalaze, a u suprotnom pokušava uspješnoj pozadinski:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Prilikom odjave korisnika, morate ukloniti pohranjenih vjerodajnica na sljedeći način:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Xamarin aplikacije pomoću [Xamarin.Auth] API-ji sigurno vjerodajnice za pohranu u objektu **računa** . Primjer korištenja tih API-ji, potražite u članku datoteke šifra [AuthStore.cs] [uzorka za razmjenu fotografija ContosoMoments].

Kada koristite klijent upravlja provjeru autentičnosti, možete predmemorije i token za pristup dobivenog od davatelja kao što su Facebook ili Twitter. Ovaj token se dodjeljuje zatražiti novi token provjeru autentičnosti iz pozadine, na sljedeći način:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Automatske obavijesti

Sljedeće teme pokrivaju automatske obavijesti:

* [Registrirajte se za slanje obavijesti](#register-for-push)
* [Nabavite paket SID iz Windows trgovine](#package-sid)
* [Registrirajte se s predlošcima za različite platforme](#register-xplat)

###<a name="register-for-push"></a>Kako: registracija za slanje obavijesti

Mobilne aplikacije klijenta omogućuje vam da biste registrirali za slanje obavijesti s koncentratorima Azure obavijesti. Prilikom registracije, dobiti bolji koje ste nabavili iz na ovisne automatske obavijesti servisa (PNS). Ta vrijednost uz sve oznake pa unijeti prilikom stvaranja registracije. Sljedeći kod registrira Windows aplikacije za slanje obavijesti s sustava Windows obavijesti servisa (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Ako su margina za WNS, zatim morate [nabaviti paket iz Windows trgovine SID](#package-sid).  Dodatne informacije o aplikacije za Windows, uključujući kako registrirati za registracije predloška potražite u članku [Dodavanje automatske obavijesti u aplikaciju].

Traženje oznaka putem klijentskog programa nije podržana.  Zahtjevi za oznaku tihu ispuštaju se iz registracije.
Ako želite da biste registrirali uređaj s oznakama, stvorite prilagođeni API-jem koji koristi API koncentratora obavijesti za registraciju na vaše ime.  [Poziv API Prilagođeno](#customapi) umjesto na `RegisterNativeAsync()` način.

###<a name="package-sid"></a>Kako: nabavite paket SID iz Windows trgovine

Paket SID potreban za omogućivanje automatske obavijesti u aplikacijama iz Windows trgovine.  Da biste dobili paket SID, registrirate aplikacija iz Windows trgovine.

Da biste dobili ovu vrijednost:

1. U programu Explorer Visual Studio u rješenje, desnom tipkom miša kliknite projekt aplikacija iz Windows trgovine, kliknite **trgovina** > **Aplikacije za povezivanje s trgovinom...**.
2. U čarobnjaku kliknite **Dalje**, prijavite se pomoću Microsoftova računa, upišite naziv aplikacije u **rezerviranje novi naziv aplikacije**, a zatim kliknite **rezerviranje**.
3. Nakon registracije za aplikaciju uspješno stvorili, odaberite naziv aplikacije, kliknite **Dalje**, a zatim **povezati**.
4. Prijavite se [Razvojni centar za Windows] pomoću Microsoftova Account. U odjeljku **Moje aplikacije**, kliknite Registracija aplikacije koji ste stvorili.
5. Kliknite **Upravljanje aplikacije** > **identiteta aplikacije**, a zatim pomaknite se prema dolje da biste pronašli **SID paketa**.

Mnoge koristi paketa SID smatra URI, u tom slučaju morate koristiti _ms aplikacije: / /_ kao shemu. Zabilježite verziju paketa SID oblikovan spajanjem tu vrijednost kao prefiks.

Xamarin aplikacije potreban je neki dodatni kod da biste mogli registrirati aplikaciju sustavom iOS ili Android platformama. Dodatne informacije potražite u temi za svoju platformu:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Kako: Register automatske predložaka za slanje obavijesti za različite platforme

Da biste registrirali predloške, koristite na `RegisterAsync()` način s predlošcima na sljedeći način:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Predloške mora biti `JObject` vrste i mogu sadržavati više predložaka u sljedećem obliku JSON:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

Način **RegisterAsync()** prihvaća i sekundarne pločice:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Sve oznake uklanjaju Odsutan tijekom registracije za vrijednosnicu. Dodavanje oznaka u instalacije ili predložaka unutar instalacije, potražite u članku [rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije].

Da biste poslali obavijesti korištenja ove registriranih predložaka, pogledajte [API koncentratora obavijesti].

##<a name="misc"></a>Ostale teme

###<a name="errors"></a>Kako: obradu pogrešaka

Kada se pojavi pogreška u pozadinski, klijent SDK podiže na `MobileServiceInvalidOperationException`.  Sljedeći primjer prikazuje način rukovanja iznimku koji je vratio pozadinski:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Drugi primjer primjene Postupanje s stanja pogreške pronaći ćete u [Mobilne aplikacije datoteke uzorka]. Primjer [LoggingHandler] nudi zapisivanje delegat rukovatelj (sljedeće) da biste se prijavili u tijeku pokušaj pozadinski zahtjeve.

###<a name="headers"></a>Kako: Prilagodba zahtjev za zaglavlja

Da biste podržali scenariju određenu aplikaciju, bilo bi dobro da biste prilagodili komunikacije s pozadinskom mobilnu aplikaciju. Na primjer, preporučujemo vam da biste dodali prilagođeno zaglavlje svaki zahtjev za odlazne ili čak i promijeniti šifre statusa odgovore. Možete koristiti u prilagođeni [DelegatingHandler], kao u sljedećem primjeru:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Dodavanje provjere autentičnosti za aplikaciju]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije]: app-service-mobile-offline-data-sync.md
[Automatske obavijesti dodati aplikacije]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Registracija aplikacije da biste koristili Prijava na račun Microsoft]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Upute za konfiguriranje aplikacije servisa za prijavu na servisu Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[stvara referenca untyped tablice]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Preuzimanje]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Odaberite]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Preskoči]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[ID korisnika]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Gdje]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Portal za Azure]: https://portal.azure.com/
[Azure klasični portal]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Razvojni centar za Windows]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[API koncentratora obavijesti]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Ogledna datoteka za mobilne aplikacije]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[Dokumentacija za OData v3]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[Zajedničko korištenje uzorka s fotografijama ContosoMoments]: https://github.com/azure-appservice-samples/ContosoMoments