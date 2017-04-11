<properties
   pageTitle="Kako koristiti Azure pretraživanja iz aplikacije za .NET | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
   description="Kako koristiti Azure pretraživanja iz aplikacije za .NET"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="10/06/2016"
   ms.author="brjohnst"/>

# <a name="how-to-use-azure-search-from-a-net-application"></a>Kako koristiti Azure pretraživanja iz aplikacije za .NET

Ovaj je članak vodič za raditi s [Azure pretraživanje .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx). .NET SDK možete koristiti za implementaciju obogaćenog pretraživanju u aplikaciji pomoću pretraživanja Azure.

## <a name="whats-in-the-azure-search-sdk"></a>Što je na Azure pretraživanje SDK

SDK sastoji se od biblioteci klijent `Microsoft.Azure.Search`. Omogućuje vam upravljanje indeksi, izvora podataka i indexers, kao i prijenos i upravljanje dokumentima i izvršavanje upita, bez potrebe za Postupanje s pojedinostima o HTTP- a JSON.

Klijentska biblioteka definira klase kao što su `Index`, `Field`, i `Document`, kao i kao što su operacije `Indexes.Create` i `Documents.Search` na na `SearchServiceClient` i `SearchIndexClient` klase. Ove klase organizirano u sljedeća prostori:

- [Microsoft.Azure.Search](https://msdn.microsoft.com/library/azure/microsoft.azure.search.aspx)
- [Microsoft.Azure.Search.Models](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.aspx)

Trenutna verzija SDK za .NET Azure pretraživanje sada je Općenito dostupan. Ako želite da biste poslali povratne bismo ugraditi u novu verziju, posjetite naš [preusmjereni na stranicu](https://feedback.azure.com/forums/263029-azure-search/).

.NET SDK podržava verziju `2015-02-28` od Azure pretraživanje REST API-JA, navedenih na [MSDN-u](https://msdn.microsoft.com/library/azure/dn798935.aspx). Ova verzija sada obuhvaća i podršku za Lucene sintaksu upita i analyzers jezika Microsoft. Novije značajke koje su *dio ove verzije, kao što je podrška za* na `moreLikeThis` parametar za pretraživanje, u [pretpregledu](search-api-2015-02-28-preview.md) i nije još dostupnih u SDK-a. Možete provjeriti ponovno uključite [verzijama servisa za pretraživanje](https://msdn.microsoft.com/library/azure/dn864560.aspx) za ažuriranja statusa ili značajku.

Ostale značajke koje nisu podržane u ovom SDK obuhvaćaju sljedeće:

  - [Upravljanje operacije](https://msdn.microsoft.com/library/azure/dn832684.aspx). Operacija upravljanja obuhvaćaju dodjele resursa Azure pretraživanja i upravljanje tipke API-JA. Te će biti podržane u u zasebnom Azure pretraživanje .NET upravljanje SDK u budućnosti.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Nadogradnja na najnoviju verziju SDK

Ako već koristite stariju verziju SDK za .NET Azure pretraživanja i želite li nadograditi na novu verziju načelu dostupan, [u ovom se članku](search-dotnet-sdk-migration.md) objašnjava kako.

## <a name="requirements-for-the-sdk"></a>Preduvjeti za SDK-a

1. Visual Studio 2013 ili Visual Studio 2015.

2. Sami servisa za pretraživanje Azure. Da biste koristili SDK, trebat će vam naziv usluge i jedne ili više tipki API-JA. [Stvaranje servisa na portalu](search-create-service-portal.md) pomoći će vam kroz sljedeće korake.

3. Preuzmite Azure pretraživanje .NET SDK [paket NuGet](http://www.nuget.org/packages/Microsoft.Azure.Search) pomoću "Upravljanje NuGet paketa" u Visual Studio. Samo potražite naziv paketa `Microsoft.Azure.Search` na NuGet.org.

.NET SDK za pretraživanje Azure podržava aplikacija ciljanja .NET Framework 4,5, kao i aplikacije iz Windows trgovine ciljanja Windows 8.1 i Windows Phone 8.1. Silverlight nije podržana.

## <a name="core-scenarios"></a>Scenariji Core

Morat ćete učiniti u aplikaciji za pretraživanje na nekoliko stvari. U ovom ćete praktičnom vodiču ćemo objasniti core scenarija:

- Stvaranje indeksa
- Popunjavanje indeksa s dokumentima
- Traženje dokumenata pomoću pretraživanja cijelog teksta i filtri

Ogledni kod koji slijedi ilustrira svaki od njih. Slobodno koristiti u koda u vlastiti računala.

### <a name="overview"></a>Pregled

Primjer aplikacije smo ćete Istraživanje stvara novi indeks pod nazivom "Hoteli" popunjava određene dokumente, a zatim izvršava neke upita za pretraživanje. Ovo su glavni program prikazuje cjelokupni tijek:

    // This sample shows how to delete, create, upload documents and query an index
    static void Main(string[] args)
    {
        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();
    }

Ćemo vas provesti kroz ovaj korak po korak. Najprije moramo da biste stvorili novu `SearchServiceClient`. Taj objekt omogućuje upravljanje indeksima. Da bi se izgraditi jednu, morate unijeti naziv servisa Azure pretraživanja, kao i ključa administrator API-JA.

        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

> [AZURE.NOTE] Ako unesete netočan ključ (na primjer, upita ključa gdje je potrebna ključa za administratore) u `SearchServiceClient` će vratiti na `CloudException` zbog pogreške s porukom "Zabranjeno" prvi put pozovete način operacija na njoj, kao što su `Indexes.Create`. Ako se to dogodi vama, ponovno naš ključ za API-JA.

Sljedeći nekoliko redaka nazovite načini za stvaranje indeksa pod nazivom "Hoteli" izbrišete najprije ako već postoji. Ne možemo će voditi kroz ovih metoda malo kasnije.

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

Nakon toga indeksu mora biti popunjena. Da biste to učinili, ne možemo će vam na `SearchIndexClient`. Dva su načina nabaviti: tako da ga izgradnje ili tako da nazovete `Indexes.GetClient` na na `SearchServiceClient`. Koristimo drugu mogućnost pogodnost.

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

> [AZURE.NOTE] U aplikaciji za uobičajene pretraživanje, upravljanje indeksom i populaciju upravlja zasebnom komponenta iz upita za pretraživanje. `Indexes.GetClient`prikladno za popunjavanje indeksa jer ona sprema imate li problema prilikom prikazivanja drugi `SearchCredentials`. To čini prosljeđivanjem administrator ključ koji ste stvorili u `SearchServiceClient` novu `SearchIndexClient`. Međutim, u dio aplikacije koji se izvršava upita je bolje stvoriti u `SearchIndexClient` izravno tako da možete proslijediti u ključu upita umjesto ključa za administratore. Ovo je dosljedan načelo najmanjih ovlasti i ćete lakše sigurniju aplikaciju. Možete saznati više o administrator te tipki upita [u nastavku](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Imamo na `SearchIndexClient`, ne možemo možete popuniti indeksa. To možete učiniti tako da drugi način smo će voditi kroz kasnije.

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

Na kraju, izvršiti nekoliko upita za pretraživanje i prikaz rezultata i ponovno korištenje na `SearchIndexClient`:

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();

Ako pokrenete ovu aplikaciju s naziv valjani usluge i ključ za API-JA, izlaz trebao bi izgledati ovako:

    Deleting index...

    Creating index...

    Uploading documents...

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []
    Complete.  Press any key to end application...

Šifra cijelog izvora aplikacije navedeni su na kraju ovog članka.

Nakon toga će trajati pozornije razmotrite svaki od metoda pozove `Main`.

### <a name="creating-an-index"></a>Stvaranje indeksa

Nakon stvaranja na `SearchServiceClient`, na sljedeći način `Main` ne Izbriši indeks "Hoteli" Ako je već postoji. Koji obavlja tvrtka na sljedeći način:

    private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
    {
        if (serviceClient.Indexes.Exists("hotels"))
        {
            serviceClient.Indexes.Delete("hotels");
        }
    }

Ova metoda koristi u određenom `SearchServiceClient` da biste provjerili ako postoji indeks, a ako jest, izbrišite je.

> [AZURE.NOTE] Primjer kod u ovom se članku koristi sinkrono metode SDK za .NET Azure pretraživanje jednostavnosti. Preporučujemo da koristite asinkronog metode u vlastiti aplikacije da biste ih zadržali skalabilni i odredište. Na primjer, u gore navedeni način možete koristiti `ExistsAsync` i `DeleteAsync` umjesto `Exists` i `Delete`.

Sljedeći, `Main` stvara novi indeks "Hoteli" tako da nazovete metoda:

    private static void CreateHotelsIndex(SearchServiceClient serviceClient)
    {
        var definition = new Index()
        {
            Name = "hotels",
            Fields = new[]
            {
                new Field("hotelId", DataType.String)                       { IsKey = true },
                new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
            }
        };

        serviceClient.Indexes.Create(definition);
    }

Ta metoda stvara novu `Index` objekt s popisom `Field` objekata koji definira shemu novi indeks. Svako polje ima naziv, vrstu podataka i nekoliko atributa koji definiraju ponašanja pretraživanja. Uz polja, možete i dodati bilježenja rezultata profile, suggesters ili CORS mogućnosti indeksa (to su izostavljena u uzorka za ispušteno da bude kraće). Dodatne informacije o objekt indeksa i njezin dio sastavnoj možete pronaći u referenci SDK na [MSDN-u](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.index_members.aspx), kao i u [Referenca Azure pretraživanje REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx).

### <a name="populating-the-index"></a>Popunjavanje indeksa

Sljedeći korak u `Main` je za popunjavanje novostvorenu indeksa. To možete učiniti na sljedeći način:

    private static void UploadDocuments(ISearchIndexClient indexClient)
    {
        var documents =
            new Hotel[]
            {
                new Hotel()
                {
                    HotelId = "1058-441",
                    HotelName = "Fancy Stay",
                    BaseRate = 199.0,
                    Category = "Luxury",
                    Tags = new[] { "pool", "view", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                    Rating = 5,
                    Location = GeographyPoint.Create(47.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "666-437",
                    HotelName = "Roach Motel",
                    BaseRate = 79.99,
                    Category = "Budget",
                    Tags = new[] { "motel", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                    Rating = 1,
                    Location = GeographyPoint.Create(49.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "970-501",
                    HotelName = "Econo-Stay",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "pool", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(46.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "956-532",
                    HotelName = "Express Rooms",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "wifi", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(48.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "566-518",
                    HotelName = "Surprisingly Expensive Suites",
                    BaseRate = 279.99,
                    Category = "Luxury",
                    ParkingIncluded = false
                }
            };

        try
        {
            var batch = IndexBatch.Upload(documents);
            indexClient.Documents.Index(batch);
        }
        catch (IndexBatchException e)
        {
            // Sometimes when your Search service is under load, indexing will fail for some of the documents in
            // the batch. Depending on your application, you can take compensating actions like delaying and
            // retrying. For this simple demo, we just log the failed document keys and continue.
            Console.WriteLine(
                "Failed to index some of the documents: {0}",
                String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
        }

        // Wait a while for indexing to complete.
        Thread.Sleep(2000);
    }

Ta metoda ima četiri dijela. Prvi stvara polje s `Hotel` objekata koji će vam poslužiti kao naš ulaznih podataka da biste prenijeli u indeks. U ovom podaci programiranih za jednostavnosti. U vlastiti aplikacije podataka vjerojatno će potjecati iz vanjskog izvora podataka kao što su baze podataka SQL.

Drugi dio stvara sustava `IndexBatch` koja sadrži dokumente. Odredite operacije koju želite primijeniti na grupu u trenutku kad ga stvorite, u tom slučaju tako da nazovete `IndexBatch.Upload`. Skupine zatim prenijeti u indeks pretraživanja Azure tako da na `Documents.Index` način.

> [AZURE.NOTE] U ovom primjeru smo samo prenosite dokumente. Ako želite spojiti promjene u postojeće dokumente ili brisanje dokumenata, mogli biste stvoriti serije tako da nazovete `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, ili `IndexBatch.Delete` umjesto toga. Možete kombinirati i različite operacije u jednu grupu tako da nazovete `IndexBatch.New`, koji traje Zbirka `IndexAction` objekata, od kojih svaka govori Azure pretraživanja za obavljanje određenog postupka na dokumentu. Možete stvoriti svaki `IndexAction` vlastitu operacijom tako da nazovete odgovarajući način kao što su `IndexAction.Merge`, `IndexAction.Upload`i tako dalje.

Treći dio ovu metodu je zamka blok koji rukuje slučaj važne pogreške za indeksiranje. Ako na servisu Azure pretraživanje ne uspije indeksirati neke dokumenata u seriji, programa `IndexBatchException` se Expression.Error po `Documents.Index`. To se može dogoditi ako su indeksiranje dokumente dok je na servisu pod velikim opterećenjem. **Preporučujemo da izričito zadužen za taj slučaj u kodu.** Možete odgoditi i ponovno pokušajte indeksiranje dokumente koji nije uspjelo ili možete prijaviti i nastaviti ne uzorka ili to možete učiniti nešto drugo ovisno o potrebama dosljednost podataka za vaše aplikacije.

Na kraju, način kašnjenja za dvije sekunde. Indeksiranje događa asinkrono na servisu Azure pretraživanja tako da se aplikacija za uzorak treba čekati kratko vrijeme da biste bili sigurni da su dokumenti dostupni za pretraživanje. Kašnjenja ovako su obično samo u pokazni programi, testira i probne aplikacije.

#### <a name="how-the-net-sdk-handles-documents"></a>Način rukovanja .NET SDK dokumenata

Koje možda se pitate se kako se moći prenositi instance klase korisnički definirana kao što su SDK za .NET pretraživanje Azure `Hotel` u indeks. Da biste odgovorili to pitanje, pogledajmo na `Hotel` klasa:

    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }

Prvo što biste obratite pozornost na to je da svaki javno svojstvo `Hotel` odgovara polju definicija indeksa, ali s jednog presudne razlike: naziv svakog polja počinje slovom malih ("camel slučaju") uz naziv svake javno svojstvo `Hotel` počinje slovom pisani velikim slovima ("Pascal slučaju"). Ovo je uobičajeni scenarij u aplikacijama za .NET koje obavljaju povezivanje podataka gdje je shema cilj izvan kontrole razvojni inženjer. Umjesto kršiti .NET imenovanja smjernice tako da svojstvo naziva camel velikih, to možete prepoznati SDK za mapiranje naziva svojstava camel predmet automatski se `[SerializePropertyNamesAsCamelCase]` atribut.

> [AZURE.NOTE] SDK za .NET Azure pretraživanje koristi biblioteka [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) Serijalizacija i prilagođene model objekata i iz JSON ukloniti serijski broj. Ako je potrebno, prilagodite ovaj serijalizacije. Dodatne informacije potražite u članku [Prilagođeno serijalizacije s JSON.NET](#JsonDotNet).
 
Drugi važno o na `Hotel` predmete nekoliko vrsta podataka javno svojstva. .NET vrste tih svojstava mapirajte njihove vrste ekvivalentan polja u definicija indeksa. Ako, na primjer, u `Category` niz svojstava karte da biste na `category` polje koje je vrste `Edm.String`. Postoje slične Vrsta mapiranja između `bool?` i `Edm.Boolean`, `DateTimeOffset?` i `Edm.DateTimeOffset`, itd. Određena pravila za mapiranje vrsta su navedenih s na `Documents.Get` metoda na [MSDN-u](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Ovu mogućnost da biste koristili vlastiti klase kao dokumenata radi u oba smjera; Dohvaćanje rezultata pretraživanja i imaju SDK automatski ukloniti serijski broj ih s vrstom po izboru, kao što je Vidimo u sljedećem odjeljku.

> [AZURE.NOTE] .NET SDK za pretraživanje Azure podržava i dinamički upisali dokumenata pomoću na `Document` predmete, mapiranje ključa vrijednosti od naziva polja da biste vrijednosti polja. To je korisno u slučajevima gdje ne znate shemi indeksa u trenutku dizajniranja ili gdje je bio bi inconvenient povezati klase konkretan model. Sve načine u SDK koje se tiču dokumenata ste overloads kojima se koriste u `Document` predmete, kao i svakako upisali overloads koje koriste parametar generički vrste. Samo drugu mogućnost se koriste u primjeru koda ovog praktičnog vodiča. Možete saznati više o na `Document` predmete [ovdje](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Važne napomene o vrstama podataka**

Prilikom dizajniranja vlastite klase model da biste mapirali indeks pretraživanja Azure, preporučujemo da deklariranje svojstva vrsta vrijednosti kao što su `bool` i `int` koji treba (na primjer, `bool?` umjesto `bool`). Ako koristite koji nisu svojstvo, morate **jamči** da nema dokumenata u indeks sadrže null vrijednost za odgovarajuće polje. SDK ni servisa za pretraživanje za Azure pomoći će vam da biste nametnuli to.

Ovo nije samo hipotetska problem: Zamislite scenarij u kojem Dodavanje novog polja da biste postojeći indeks koji je vrste `Edm.Int32`. Nakon ažuriranja definicija indeksa, svi dokumenti će imati null vrijednost tog novog polja (jer su svi tipovi koji u pretraživanju Azure). Ako koristite razredu model s u ne-koji `int` svojstava za to polje, prikazat će vam se na `JsonSerializationException` ovako prilikom dohvaćanja dokumenata:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Zbog toga preporučujemo da koristite koji vrste na razrede modela kao preporučenim načinom rada.

<a name="JsonDotNet"></a>
#### <a name="custom-serialization-with-jsonnet"></a>Prilagođeni serijalizacije s JSON.NET

SDK koristi JSON.NET Serijalizacija i prekidanja serije dokumenata. Možete prilagoditi serijalizacije i deserialization po potrebi definiranjem vlastite `JsonConverter` ili `IContractResolver` (potražite u [dokumentaciji JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) više pojedinosti). To može biti korisna kada želite prilagoditi postojeće predmete modela iz aplikacije za pretraživanje Azure i druge naprednije scenarijima. Na primjer, uz prilagođene serijalizacije možete:

 - Uključivanje ili isključivanje određena svojstva klase modela pohranjivanje kao polja u dokument.
 - Mapirajte između naziva svojstava u kodu i naziva polja u indeks.
 - Stvorite prilagođene atribute koje je moguće koristiti za i mapiranje svojstava polja dokumenta kao i stvaranje odgovarajućih definicija indeksa.

Primjeri implementacije prilagođene serijalizacije u testova jedinica za Azure pretraživanje .NET SDK-a na GitHub možete pronaći. Dobru početnu točku je [ovu mapu](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Sadrži klase koji koriste testove prilagođene serijalizacije.

### <a name="searching-for-documents-in-the-index"></a>Traženje dokumenata u indeks

Posljednji korak u aplikaciji uzorka je da biste potražili neki dokumenti u indeks. Sa sljedećim korakom čini sljedeće:

    private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
    {
        // Execute search based on search text and optional filter
        var sp = new SearchParameters();

        if (!String.IsNullOrEmpty(filter))
        {
            sp.Filter = filter;
        }

        DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
        foreach (SearchResult<Hotel> result in response.Results)
        {
            Console.WriteLine(result.Document);
        }
    }

Najprije stvara novu ovu metodu `SearchParameters` objekt. Koristi se za navedite dodatne mogućnosti upita kao što su sortiranje, filtriranje, označavanja stranica i faceting. U ovom primjeru ćemo se postavka samo na `Filter` svojstvo.

Sljedeći je korak za zapravo izvršavanje upita za pretraživanje. To možete učiniti pomoću na `Documents.Search` način. U ovom slučaju ćemo proslijediti tekst koji želite tražiti da biste koristili kao niz, uz parametara pretraživanja ste ranije stvorili. Ne možemo odrediti `Hotel` kao parametar type za `Documents.Search`, koja pokazuje SDK za dokumente u rezultatima pretraživanja ukloniti serijski broj u objekti vrste `Hotel`.

Na kraju, ova metoda zadanu ponovno izračunava kroz sve pločice u rezultatima pretraživanja ispisuje svaki dokument da biste konzoli sustava.

Pogledajmo detaljnije Upoznavanje kako se zove metoda:

    SearchDocuments(indexClient, searchText: "fancy wifi");

    SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

U prvi poziv smo tražite sve dokumente koji sadrže upit uvjeta "atraktivnog" ili "Wi-Fi veze". Pretraživanje teksta u drugi poziv je postavljeno na "*", što znači "pronaći sve". Dodatne informacije o sintaksi izraza upita za pretraživanje možete pronaći [ovdje](https://msdn.microsoft.com/library/azure/dn798920.aspx).

Drugi poziv koristi OData `$filter` izraz, `category eq 'Luxury'`. To constrains pretraživanja vratiti samo dokumente kojima u `category` polja u potpunosti podudara s nizom "Luxury". Možete saznati više o sintaksi OData s podrškom za pretraživanje Azure [ovdje](https://msdn.microsoft.com/library/azure/dn798921.aspx).

Sad kad znate što znače te dvije pozive, mora biti lakše vidjeti Zašto njihove izlaz izgleda ovako:

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []

Prvi pretraživanja vraća dva dokumenta. Prvi je "Atraktivnog" u nazivu, dok drugi ima "Wi-Fi veze" na `tags` polja. Drugi pretraživanja vraća dva dokumente koji se pokreću se samo dokumente u indeks koji imaju na `category` polje postavljeno na "Luxury".

Ovaj korak dovršava vodič, ali se ne zaustavi ovdje. **Daljnji koraci** pruža dodatne resurse za dobivanje dodatnih informacija o Azure pretraživanja.

## <a name="next-steps"></a>Daljnji koraci

- Pronađite reference za [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) i [REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) na MSDN-u.
- Deepen svoje znanje kroz [videozapise i druge uzoraka i vodiči za](search-video-demo-tutorial-list.md).
- Saznajte više o značajkama i mogućnostima u ovoj verziji programa pretraživanje SDK Azure: [Pregled Azure pretraživanja](https://msdn.microsoft.com/library/azure/dn798933.aspx)
- Pregledajte [konvencija imenovanja](https://msdn.microsoft.com/library/azure/dn857353.aspx) da biste saznali pravila za davanje naziva različitih objekata.
- Pregledajte [podržane vrste podataka](https://msdn.microsoft.com/library/azure/dn798938.aspx) u Azure pretraživanja.


## <a name="sample-application-source-code"></a>Ogledna aplikacije izvornog koda

Evo šifru cijelog izvora aplikacije primjer koristi u ovaj vodič kroz. Imajte na umu da ćete morati zamijenite naziv servisa i API ključa rezerviranih mjesta u Program.cs vlastitih vrijednosti upute za stvaranje i pokretanje uzorka.

Program.CS:

```csharp
using System;
using System.Configuration;
using System.Linq;
using System.Threading;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    class Program
    {
        // This sample shows how to delete, create, upload documents and query an index
        static void Main(string[] args)
        {
            // Put your search service name here. This is the hostname portion of your service URL.
            // For example, if your service URL is https://myservice.search.windows.net, then your
            // service name is myservice.
            string searchServiceName = "myservice";

            string apiKey = "Put your API admin key here.";

            SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

            Console.WriteLine("{0}", "Deleting index...\n");
            DeleteHotelsIndexIfExists(serviceClient);

            Console.WriteLine("{0}", "Creating index...\n");
            CreateHotelsIndex(serviceClient);

            ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

            Console.WriteLine("{0}", "Uploading documents...\n");
            UploadDocuments(indexClient);

            Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
            SearchDocuments(indexClient, searchText: "fancy wifi");

            Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
            SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

            Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("hotels"))
            {
                serviceClient.Indexes.Delete("hotels");
            }
        }

        private static void CreateHotelsIndex(SearchServiceClient serviceClient)
        {
            var definition = new Index()
            {
                Name = "hotels",
                Fields = new[]
                {
                    new Field("hotelId", DataType.String)                       { IsKey = true },
                    new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                    new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                    new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                    new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                    new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
                }
            };

            serviceClient.Indexes.Create(definition);
        }

        private static void UploadDocuments(ISearchIndexClient indexClient)
        {
            var documents =
                new Hotel[]
                {
                    new Hotel()
                    {
                        HotelId = "1058-441",
                        HotelName = "Fancy Stay",
                        BaseRate = 199.0,
                        Category = "Luxury",
                        Tags = new[] { "pool", "view", "concierge" },
                        ParkingIncluded = false,
                        LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                        Rating = 5,
                        Location = GeographyPoint.Create(47.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "666-437",
                        HotelName = "Roach Motel",
                        BaseRate = 79.99,
                        Category = "Budget",
                        Tags = new[] { "motel", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                        Rating = 1,
                        Location = GeographyPoint.Create(49.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "970-501",
                        HotelName = "Econo-Stay",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "pool", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(46.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "956-532",
                        HotelName = "Express Rooms",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "wifi", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(48.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "566-518",
                        HotelName = "Surprisingly Expensive Suites",
                        BaseRate = 279.99,
                        Category = "Luxury",
                        ParkingIncluded = false
                    }
                };

            try
            {
                var batch = IndexBatch.Upload(documents);
                indexClient.Documents.Index(batch);
            }
            catch (IndexBatchException e)
            {
                // Sometimes when your Search service is under load, indexing will fail for some of the documents in
                // the batch. Depending on your application, you can take compensating actions like delaying and
                // retrying. For this simple demo, we just log the failed document keys and continue.
                Console.WriteLine(
                    "Failed to index some of the documents: {0}",
                    String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
            }

            // Wait a while for indexing to complete.
            Thread.Sleep(2000);
        }

        private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
        {
            // Execute search based on search text and optional filter
            var sp = new SearchParameters();

            if (!String.IsNullOrEmpty(filter))
            {
                sp.Filter = filter;
            }

            DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
            foreach (SearchResult<Hotel> result in response.Results)
            {
                Console.WriteLine(result.Document);
            }
        }
    }
}
```

Hotel.CS:

```csharp
using System;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }
}
```
