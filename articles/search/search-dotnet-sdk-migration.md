<properties
   pageTitle="Nadogradnja Azure pretraživanje .NET SDK verzije 1.1 | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
   description="Nadogradnja Azure pretraživanje .NET SDK verzije 1.1"
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
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Nadogradnja na Azure pretraživanje .NET SDK verzije 2.0 – pretpregled

Ako koristite verzije 1.1 ili stariji [Azure pretraživanje .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), u ovom se članku će vam nadogradnju aplikacije da biste koristili sljedeća glavna verzija, 2.0 pretpregled.

Općenite vodič SDK i primjere potražite [u](search-howto-dotnet-sdk.md)članku korištenje pretraživanja za Azure iz aplikacije za .NET.

Verzije 2.0 pretpregled SDK .NET Azure pretraživanje sadrži neke promjene u prethodnim verzijama. To su uglavnom manji, tako da se promjene kod zahtijevaju minimalnog truda. Upute o tome kako promijeniti kod da biste koristili novu verziju SDK potražite u članku [upute za nadogradnju](#UpgradeSteps) .

> [AZURE.NOTE] Ako koristite verziju 0,13 pretpregled ili stariji, nadogradite verzije 1.1 prvi, a zatim nadogradnje na 2.0 pretpregled. U odjeljku [dodatak: korake da biste nadogradili verzije 1.1](#UpgradeStepsV1) upute.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Novosti u verziji 2.0 – pregled

Verzije 2.0 – pregled je prva verzija SDK za .NET Azure pretraživanje ciljne verzije programa Azure pretraživanje REST API-JA, posebice 2015-02-28-pretpregled za pretpregled. Time se omogućuje brojne značajke pretpregleda Azure pretraživanja iz aplikacije .NET, uključujući sljedeće:

- [Prilagođeni analyzers](https://aka.ms/customanalyzers)
- [Spremište blobova platforme Azure](search-howto-indexing-azure-blob-storage.md) i [Spremište tablica Azure](search-howto-indexing-azure-tables.md) podrška za indeksiranje
- Prilagodba indeksiranje putem [Mapiranje polja](search-indexer-field-mappings.md)
- ETags podrška da biste omogućili sigurni Istodobni ažuriranja definicije indeks, indexers i izvora podataka
- Podrška za .NET Core i .NET prijenosni profila 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Koraci za nadogradnju

Najprije ažuriranje NuGet referencu za `Microsoft.Azure.Search` korištenju konzole Upravitelj paketa NuGet ili po desnom tipkom miša na reference projekta i odaberete "Upravljanje NuGet paketa..." u Visual Studio. Provjerite je li omogućiti predizdanje paketa odabirom "Uključi predizdanja" NuGet "Upravljanje paketa" prozor u Visual Studio ili pomoću na `-IncludePrerelease` promijeniti ako koristite Upravitelj paketa NuGet konzolu.

Kada NuGet preuzme novi paketi i njihove ovisnosti, ponovno stvaranje projekta. Ovisno o tome kako je strukturirani kodu, on može ponovno stvaranje uspješno. Ako je tako, spremni ste za slanje!

Ako vaš Sastavi ne uspije, trebali biste vidjeti Sastavi pogreške kao što je sljedeća:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Sljedeći je korak da biste ispravili tu pogrešku Sastavi. Pogledajte [najnovije promjene u verziji 2.0 – pregled](#ListOfChanges) pojedinosti o što uzrokuje pogrešku i kako je ispraviti.

Vidjet ćete dodatne Sastavi upozorenja vezane uz zastarjeli metode ili svojstva. Upute o tome što trebate koristiti umjesto značajku zastarjeli neće sadržavati stavku upozorenja. Na primjer, ako se koristi aplikacija na `SearchRequestOptions.RequestId` svojstvo, trebali biste dobiti upozorenja koja vas obavještava da`"This property is deprecated. Please use ClientRequestId instead."`

Nakon što ste riješili neke Sastavi pogreške, možete unijeti promjene u aplikaciji da iskoristite prednost novih funkcija ako želite. Nove značajke u SDK detaljno opisane u [što je novo u verzije 2.0 – pregled](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Najnovije promjene u verziji 2.0 pretpregled

Postoji samo jedna promjena prijelom verzije 2.0 – pregled koji mogu zahtijevati kod promjene osim novog aplikacije.

### <a name="indexesgetclient-return-type"></a>Vrsta povrata Indexes.GetClient

Na `Indexes.GetClient` metoda ima Nova vrsta povrata. Prethodno, vraćena `SearchIndexClient`, ali to je promijenjen `ISearchIndexClient` u verzije 2.0 – pregled. Ovo je za podršku kupaca kojima želite mock na `GetClient` način za jedinica testira vraćanjem mock implementaciji sustava `ISearchIndexClient`.

#### <a name="example"></a>Primjer

Ako je kod izgleda ovako:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Možete je promijeniti na to da biste ispravili pogreške Sastavi:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Zaključak
Ako trebate dodatne informacije o korištenju SDK za .NET Azure pretraživanja, pogledajte naše nedavno ažurirane [s uputama](search-howto-dotnet-sdk.md).

Ne možemo Dobro došli povratne informacije na SDK-a. Ako naiđete na probleme, slobodno nam zatražite pomoć na [forumu MSDN Azure pretraživanja](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Ako pronađete pogrešku, problem možete datoteka u [spremištu Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues). Provjerite jeste li prefiks naslov problem s "SDK pretraživanja:".

Zahvaljujemo za korištenje Azure pretraživanja.

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Dodatak: Korake da biste nadogradili verzije 1.1 

> [AZURE.NOTE] U ovom se odjeljku odnosi se samo na korisnike 0,13 pregled i starije verzije Azure pretraživanje .NET SDK.

Najprije ažuriranje NuGet referencu za `Microsoft.Azure.Search` korištenju konzole Upravitelj paketa NuGet ili po desnom tipkom miša na reference projekta i odaberete "Upravljanje NuGet paketa..." u Visual Studio.

Kada NuGet preuzme novi paketi i njihove ovisnosti, ponovno stvaranje projekta.

Ako ste prethodno koristili verziju 1.0.0-preview, 1.0.1-preview ili 1.0.2-preview, trebala bi uspjeti sastavljanje tako da bude spremna za slanje!

Ako ste koristili ranije verzije 0.13.0-preview ili stariji, trebali biste vidjeti sastavljanje pogreške ovako:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Sljedeći je korak da biste ispravili pogreške Sastavi jedan po jedan. Najčešće je potrebno promjena neki nazivi klasa i način koji ste preimenovana je u SDK-a. [Popis najnovije promjene u verzije 1.1](#ListOfChangesV1) sadrži popis ove promjene naziva.

Ako koristite prilagođeni klase oblikovanja dokumenata i one klase imati svojstva koje nisu koji jednostavne vrste (na primjer, `int` ili `bool` u C#), u verziji 1.1 SDK od kojih Imajte na umu da postoji popravak pogrešku. Dodatne informacije potražite u članku [popravci programskih pogrešaka u verzije 1.1](#BugFixesV1) .

Na kraju, kada ste riješili neke Sastavi pogreške, možete unijeti promjene u aplikaciji da iskoristite prednost novih funkcija ako želite.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Popis najnovije promjene u verzije 1.1

Na sljedećem su popisu poredane po vjerojatnost da će promjene utjecati na kodu aplikacije.

#### <a name="indexbatch-and-indexaction-changes"></a>Promjene IndexBatch i IndexAction

`IndexBatch.Create`preimenovana je u `IndexBatch.New` i više nema u `params` argument. Možete koristiti `IndexBatch.New` za skupina koje kombinirati različite vrste Akcije (spajanja, briše itd.). Uz to, načina novi statične za stvaranje serije gdje sve akcije jednaki: `Delete`, `Merge`, `MergeOrUpload`, i `Upload`.

`IndexAction`više nema javno constructors i njezina svojstva sada su immutable. Nova metoda statične trebali biste koristiti za stvaranje akcije za različite potrebe: `Delete`, `Merge`, `MergeOrUpload`, i `Upload`. `IndexAction.Create`uklonjena je. Ako ste koristili preopterećenja koja vas vodi samo dokument, provjerite je li za korištenje `Upload` umjesto toga.

##### <a name="example"></a>Primjer

Ako je kod izgleda ovako:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Možete je promijeniti na to da biste ispravili pogreške Sastavi:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Ako želite, za to Dodatno možete pojednostavniti je:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>Promjena IndexBatchException

Na `IndexBatchException.IndexResponse` svojstvo preimenovana je u `IndexingResults`, a sada je koja odgovara vrsti `IList<IndexingResult>`.

##### <a name="example"></a>Primjer

Ako je kod izgleda ovako:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Možete je promijeniti na to da biste ispravili pogreške Sastavi:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Promjena načina operacije

Svaku operaciju u SDK za .NET Azure pretraživanje predstavljeni kao skup overloads način za pozivatelje sinkrono i asinkronih. Potpisi i factoring od ovih metoda overloads se promijenilo u verzije 1.1.

Na primjer, "Dohvati indeks Statistika" operacija u starijim verzijama programa SDK-a izložen te potpisa:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Način potpisa za istu operaciju u verzije 1.1 izgledati ovako:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Počevši od verzije 1.1, SDK za .NET Azure pretraživanje organizira operacija metode drugačije:

 - Neobavezni parametar koji su sada katalog modeliran kao zadani parametara radije od overloads dodatne metode. Time se smanjuje broj overloads način ponekad znatno.
 - Proširenje metode sada sakriti mnogo druge detalje o HTTP od pozivatelja. Ako, na primjer, starijih verzija SDK vraća odgovor objekt s programa HTTP status kod koji često niste morate provjeriti jer operacija metode vratiti `CloudException` za sve Šifra stanja koja pokazuje pogrešku. Novi načini proširenje samo vratite objekti modela spremanje problema te Ukini prelamanje u kodu.
 - Nasuprot tome, u skladu s osnovnim sučelja sada izložiti metode koje omogućuju veću kontrolu na razini HTTP ako vam je potrebna. Sada možete proslijediti u prilagođene HTTP zaglavlja da budu obuhvaćeni zahtjeve i novi `AzureOperationResponse<T>` vrsta povrata daje izravan pristup na `HttpRequestMessage` i `HttpResponseMessage` za operaciju. `AzureOperationResponse`određen u na `Microsoft.Rest.Azure` prostor naziva i zamjenjuje `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>Promjena ScoringParameters

Novi klase pod nazivom `ScoringParameter` dodana u najnovije SDK da biste olakšali pružaju parametara za bilježenje rezultata profila u upit za pretraživanje. Prethodno u `ScoringProfiles` svojstvo na `SearchParameters` predmete upisan kao `IList<string>`; Sada se uneseni kao `IList<ScoringParameter>`.

##### <a name="example"></a>Primjer

Ako je kod izgleda ovako:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Možete je promijeniti na to da biste ispravili pogreške Sastavi: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Promjena klase modela

Zbog potpis mijenja što je opisano u [mijenja način operacija](#OperationMethodChanges), mnogo klase u na `Microsoft.Azure.Search.Models` prostor naziva preimenovati ili ukloniti. Ako, na primjer:

 - `IndexDefinitionResponse`zamijenjen je`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`preimenovana je u`DocumentSearchResult`
 - `IndexResult`preimenovana je u`IndexingResult`
 - `Documents.Count()`Sada se vraća na `long` s broj dokumenata umjesto u`DocumentCountResponse`
 - `IndexGetStatisticsResponse`preimenovana je u`IndexGetStatisticsResult`
 - `IndexListResponse`preimenovana je u`IndexListResult`

Ukratko, `OperationResponse`-izvedena klase programa samo da bi se model objekta uklonjene. Preostali klase imao njihove sufiks promijenio iz `Response` da biste `Result`.

##### <a name="example"></a>Primjer

Ako je kod izgleda ovako:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Možete je promijeniti na to da biste ispravili pogreške Sastavi:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Tečajevi za odgovor i IEnumerable

Dodatni Promijeni koji mogu utjecati na kod je implementirati da više ne klase odgovor koji sadrže zbirke `IEnumerable<T>`. Umjesto toga, svojstvo zbirke možete pristupiti izravno. Na primjer, kod izgleda ovako:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Možete je promijeniti na to da biste ispravili pogreške Sastavi:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Napomena o važnosti za web-aplikacije

Ako imate web-aplikacije koja serializes `DocumentSearchResponse` izravno da biste poslali rezultate pretraživanja web-pregledniku, morat ćete promijeniti kod ili rezultate će serijalizirati pravilno. Na primjer, kod izgleda ovako:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Možete promijeniti tako da na `.Results` svojstvo odgovor pretraživanja da biste riješili problem prikazivanje rezultata pretraživanja:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Morat ćete potražiti tim slučajevima u kodu sami. **Na compiler ne upozorit će vas** jer `JsonResult.Data` vrste `object`.

#### <a name="cloudexception-changes"></a>Promjena CloudException

Na `CloudException` predmete premještena iz na `Hyak.Common` prostora za naziv da biste na `Microsoft.Rest.Azure` prostor naziva. Osim toga, njegov `Error` svojstvo preimenovana je u `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Promjene SearchServiceClient i SearchIndexClient

Vrsta na `Credentials` svojstvo promijenio iz `SearchCredentials` za osnovnu klasu `ServiceClientCredentials`. Ako vam je potrebna za pristup na `SearchCredentials` od na `SearchIndexClient` ili `SearchServiceClient`, upotrijebite novi `SearchCredentials` svojstvo.

U starijim verzijama programa u SDK `SearchServiceClient` i `SearchIndexClient` imali constructors koji traje programa `HttpClient` parametar. Te zamijenjene constructors koje koriste se `HttpClientHandler` i polja od `DelegatingHandler` objekte. To olakšava da biste instalirali prilagođene rukovatelja unaprijed obrade zahtjeva za HTTP prema potrebi.

Na kraju, ali je constructors na `Uri` i `SearchCredentials` promijenile. Na primjer, ako imate kod koji izgleda ovako:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Možete je promijeniti na to da biste ispravili pogreške Sastavi:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Također imajte na umu da vrsti parametra vjerodajnice je promijenjeno u `ServiceClientCredentials`. To je vjerojatno neće utjecati na kod od `SearchCredentials` proizlazi iz `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Prosljeđivanje zahtjeva za ID-a

U starijim verzijama programa SDK ID zahtjeva nije postaviti na na `SearchServiceClient` ili `SearchIndexClient` i želite uvrstiti u svaki zahtjev za REST API-JA. To je korisno za otklanjanje poteškoća sa servisom za pretraživanje ako je potrebno obratiti službi za podršku. Međutim, nije još korisnijim da biste postavili zahtjev za jedinstveni ID-a za svaku operaciju umjesto da biste koristili isti ID za sve operacije. Zbog toga u `SetClientRequestId` metode `SearchServiceClient` i `SearchIndexClient` uklonjene. Umjesto toga možete proslijediti ID zahtjeva za svaki način operacija putem neobavezna `SearchRequestOptions` parametar.

> [AZURE.NOTE] U buduće izdanje SDK dodat ćemo novi mehanizam za postavljanje ID zahtjeva globalno na klijentskom računalu objekte koje je dosljedno pristup koristi drugi SDK-ovi Azure.

#### <a name="example"></a>Primjer

Ako imate kod koji izgleda ovako:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Možete je promijeniti na to da biste ispravili pogreške Sastavi:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Promjena naziva sučelja

Sučelje naziva grupa operacija sve promijenjene da budu dosljedni s odgovarajućim nazivima svojstvo:

 - Vrsta `ISearchServiceClient.Indexes` preimenovana je u skladu s `IIndexOperations` za `IIndexesOperations`.
 - Vrsta `ISearchServiceClient.Indexers` preimenovana je u skladu s `IIndexerOperations` za `IIndexersOperations`.
 - Vrsta `ISearchServiceClient.DataSources` preimenovana je u skladu s `IDataSourceOperations` za `IDataSourcesOperations`.
 - Vrsta `ISearchIndexClient.Documents` preimenovana je u skladu s `IDocumentOperations` za `IDocumentsOperations`.

Ta promjena je vjerojatno neće utjecati na kodu, osim ako ste stvorili mocks ta sučelja svrhe test.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Popravci programskih pogrešaka u verzije 1.1

Došlo je pogrešku u starijim verzijama programa Azure pretraživanje .NET SDK-a koji se odnose na serijalizacije klase prilagođene modela. Programska pogreška može pojaviti ako ste stvorili prilagođene modela predmete s svojstvo vrsta ne može postaviti kao null vrijednosti.

#### <a name="steps-to-reproduce"></a>Koraci za ponavljanje pogreške

Stvorite prilagođene modela predmete s svojstvo vrsta ne može postaviti kao null vrijednosti. Na primjer, dodati javni `UnitCount` svojstvo vrste `int` umjesto `int?`.

Ako indeksirate dokumenta sa zadanom vrijednošću te vrste (na primjer, 0 za `int`), polje će biti null u pretraživanju Azure. Ako naknadno potražite taj dokument u `Search` poziv će vratiti `JsonSerializationException` gdje koji se ne može pretvoriti `null` da biste `int`.

Osim toga, filtri možda neće funkcionirati prema očekivanjima jer null napisanu u indeks umjesto predviđene vrijednosti.

#### <a name="fix-details"></a>Rješavanje pojedinosti

Taj se problem ne možemo nepromjenjivim u verzije 1.1 SDK. Sada, ako imate model predmete ovako:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

i postavite `IntValue` 0, vrijednost je sada pravilno serijalizirani kao 0 na na žičani i pohranjenih kao 0 u indeks. ROUND tripping funkcionira i prema očekivanjima.

Postoji potencijalnih problema potrebno je obratiti pažnju na taj se način: Ako koristite vrstu modela uz koji nisu svojstvo, morate **jamči** da nema dokumenata u indeks sadrže null vrijednost za odgovarajuće polje. SDK ni Azure pretraživanje REST API-JA pomoći će vam da biste nametnuli to.

Ovo nije samo hipotetska problem: Zamislite scenarij u kojem Dodavanje novog polja da biste postojeći indeks koji je vrste `Edm.Int32`. Nakon ažuriranja definicija indeksa, svi dokumenti će imati null vrijednost tog novog polja (jer su svi tipovi koji u pretraživanju Azure). Ako koristite razredu model s u ne-koji `int` svojstava za to polje, prikazat će vam se na `JsonSerializationException` ovako prilikom dohvaćanja dokumenata:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Zbog toga još uvijek preporučujemo da koristite koji vrste na razrede modela kao preporučenim načinom rada.

Dodatne informacije o ovom pogrešku i popravak, pogledajte [ovaj problem na GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
