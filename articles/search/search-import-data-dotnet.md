<properties
    pageTitle="Prijenos podataka u pretraživanju Azure pomoću .NET SDK | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Saznajte kako prenijeti podatke indeksa u pretraživanju Azure pomoću .NET SDK-a."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Prijenos podataka Azure pretraživanje pomoću .NET SDK
> [AZURE.SELECTOR]
- [Pregled](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [OSTALE](search-import-data-rest-api.md)

U ovom se članku vidjet ćete kako koristiti [Azure pretraživanje .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) za uvoz podataka u indeks pretraživanja Azure.

Prije početka ovog vodiča, trebali biste već ste [stvorili indeks pretraživanja Azure](search-what-is-an-index.md). U ovom se članku i pretpostavlja da ste već stvorili na `SearchServiceClient` objekta, kao što je prikazano u [Stvaranje indeksa pretraživanja Azure pomoću .NET SDK-a](search-create-index-dotnet.md#CreateSearchServiceClient).

Imajte na umu da sve uzorak koda u ovom članku napisan C#. Možete pronaći izvor cijeli kod [na GitHub](http://aka.ms/search-dotnet-howto).

Da bi se automatske dokumenata u indeks pomoću .NET SDK-a, morat ćete:

  1. Stvaranje na `SearchIndexClient` objekta koji želite povezati indeksu pretraživanja.
  2. Stvaranje programa `IndexBatch` koji sadrži dokumente da biste dodali, promijenili ili izbrisali.
  3. Poziv u `Documents.Index` način vaše `SearchIndexClient` da biste poslali na `IndexBatch` u indeksu pretraživanja.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>Li. Stvaranje instanca klase SearchIndexClient
Da biste uvezli podatke u indeks pomoću SDK za .NET Azure pretraživanja, morat ćete stvoriti instancu na `SearchIndexClient` predmete. Možete stvoriti ovu instancu sami, ali je jednostavnija ako već imate u `SearchServiceClient` instancu da biste nazvali njegov `Indexes.GetClient` način. Ako, na primjer, Evo kako biste dobili na `SearchIndexClient` za indeks pod nazivom "Hoteli" iz programa `SearchServiceClient` pod nazivom `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] U aplikaciji za uobičajene pretraživanje, upravljanje indeksom i populaciju upravlja zasebnom komponenta iz upita za pretraživanje. `Indexes.GetClient`prikladno za popunjavanje indeksa jer ona sprema imate li problema prilikom prikazivanja drugi `SearchCredentials`. To čini prosljeđivanjem administrator ključ koji ste stvorili u `SearchServiceClient` novu `SearchIndexClient`. Međutim, u dio aplikacije koji se izvršava upita je bolje stvoriti u `SearchIndexClient` izravno tako da možete proslijediti u ključu upita umjesto ključa za administratore. Ovo je dosljedan [načelo najmanjih ovlasti](https://en.wikipedia.org/wiki/Principle_of_least_privilege) i ćete lakše sigurniju aplikaciju. Možete saznati više o administrator te tipki upita u [Referenca Azure pretraživanje REST API -JA na MSDN-u](https://msdn.microsoft.com/library/azure/dn798935.aspx).

`SearchIndexClient`sadrži na `Documents` svojstvo. Ovo svojstvo sadrži sve načine potrebno za dodavanje, izmjena, brisanje ili dokumenata u indeks za upite.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Odlučite akcijske indeksiranja za korištenje
Da biste uvezli podatke pomoću .NET SDK, morat ćete objediniti podatke u programa `IndexBatch` objekt. U `IndexBatch` encapsulates Zbirka `IndexAction` objekata, od kojih svaka sadrži dokument i svojstva koja govori Azure pretraživanja koje akciju na taj dokument (prijenos pisma, brisanje, itd). Ovisno o tome što se ispod akcije koje odaberete samo određenih polja mora biti uključeno za svaki dokument:

Akcija | Opis | Potrebna polja za svaki dokument | Bilješke
--- | --- | --- | ---
`Upload` | Na `Upload` akcija je slična je "upsert" gdje umetnuti ako je novo i ažurirati/zamijeniti ako postoji u dokumentu. | tipka plus sva ostala polja koje želite definirati | Kada ažuriranje/Zamjena postojećeg dokumenta, svako polje koje nije naveden u zahtjevu za će imati njegovo polje postavljeno na `null`. To se događa čak i kad je polje prethodno postavljen na vrijednost koja nije null.
`Merge` | Ažurira postojeći dokument određena polja. Ako dokument ne postoji u indeksu, spajanja neće uspjeti. | tipka plus sva ostala polja koje želite definirati | Bilo koje polje u spajanja navedete zamijenit će postojeće polje u dokumentu. To obuhvaća polja vrste `DataType.Collection(DataType.String)`. Na primjer, ako dokument sadrži polje `tags` vrijednošću `["budget"]` i izvršavanje spajanja s vrijednošću `["economy", "pool"]` za `tags`, konačne vrijednosti u `tags` polje će biti `["economy", "pool"]`. Neće biti `["budget", "economy", "pool"]`.
`MergeOrUpload` | Time se ponaša kao `Merge` Ako dokument s određenom ključ već postoji u indeks. Ako dokument ne postoji, ponaša kao `Upload` novi dokument. | tipka plus sva ostala polja koje želite definirati | -
`Delete` | Uklanja navedeni dokument iz indeksa. | samo s ključem | Sva polja navedite osim polja ključa će je zanemariti. Ako želite ukloniti pojedinačnih polja iz dokumenta pomoću `Merge` umjesto i jednostavno polje postavljeno na izričito null.

Možete odrediti koje akcije želite koristiti uz različite metode statične od na `IndexBatch` i `IndexAction` klasa iz registra, kao što je prikazano u sljedećem odjeljku.

## <a name="iii-construct-your-indexbatch"></a>III. Sastavljanje vaše IndexBatch
Sad kad znate koje će se akcije izvršiti na dokumentima, ste spremni za sastavljanje na `IndexBatch`. U primjeru u nastavku prikazuje kako stvoriti grupu s nekoliko različite akcije. Bilješke u našem primjeru koristi je prilagođene klase pod nazivom `Hotel` koje mape u dokument u indeksu "Hoteli".

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

U ovom slučaju ne možemo koristite `Upload`, `MergeOrUpload`, i `Delete` kao naš akcije pretraživanja, kao što je navedeno na načine pod nazivom na na `IndexAction` predmete.

Pretpostavimo da ovaj primjer "Hoteli" indeks već popunjen broj dokumenata. Imajte na umu kako ćemo nema da biste odredili sva polja moguće dokumenata prilikom korištenja `MergeOrUpload` i kako ćemo samo određene tipku dokumenta (`HotelId`) prilikom korištenja `Delete`.

Osim toga, imajte na umu da do 1000 dokumente možete uključiti samo u jednom zahtjev za indeksiranje.

> [AZURE.NOTE] U ovom primjeru smo su Primjena različite akcije na različite dokumente. Ako želite izvesti iste akcije u svim dokumentima u seriji umjesto zovete `IndexBatch.New`, možete koristiti na druge statične metode `IndexBatch`. Na primjer, mogli biste stvoriti serije tako da nazovete `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, ili `IndexBatch.Delete`. Načini poduzeti skup dokumenata (objekti vrste `Hotel` u ovom primjeru) umjesto `IndexAction` objekte.

## <a name="iv-import-data-to-the-index"></a>IV. Uvoz podataka u indeks
Sad kad ste je inicijalizirana `IndexBatch` objekt, možete je poslati u indeks tako da nazovete `Documents.Index` na vašem `SearchIndexClient` objekt. Sljedeći primjer prikazuje način da biste uputili poziv `Index`, kao što su neke dodatne korake morat ćete izvršiti i:

```csharp
try
{
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

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Napomena u `try` / `catch` oko poziv na `Index` način. Blokiranje zamka rukuje slučaj važne pogreške za indeksiranje. Ako na servisu Azure pretraživanje ne uspije indeksirati neke dokumenata u seriji, programa `IndexBatchException` se Expression.Error po `Documents.Index`. To se može dogoditi ako su indeksiranje dokumente dok je na servisu pod velikim opterećenjem. **Preporučujemo da izričito zadužen za taj slučaj u kodu.** Možete odgoditi i ponovno pokušajte indeksiranje dokumente koji nije uspjelo ili možete prijaviti i nastaviti ne uzorka ili to možete učiniti nešto drugo ovisno o potrebama dosljednost podataka za vaše aplikacije.

Na kraju, kod u primjeru iznad kašnjenja za dvije sekunde. Indeksiranje događa asinkrono na servisu Azure pretraživanja tako da se aplikacija za uzorak treba čekati kratko vrijeme da biste bili sigurni da su dokumenti dostupni za pretraživanje. Kašnjenja ovako su obično samo u pokazni programi, testira i probne aplikacije.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Način rukovanja .NET SDK dokumenata

Koje možda se pitate se kako se moći prenositi instance klase korisnički definirana kao što su SDK za .NET pretraživanje Azure `Hotel` u indeks. Da biste odgovorili to pitanje, pogledajmo na `Hotel` predmete, mapira u shemi indeks definirano u [Stvaranje indeksa pretraživanja Azure pomoću .NET SDK](search-create-index-dotnet.md#DefineIndex):

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Prvo što biste obratite pozornost na to je da svaki javno svojstvo `Hotel` odgovara polju definicija indeksa, ali s jednog presudne razlike: naziv svakog polja počinje slovom malih ("camel slučaju") uz naziv svake javno svojstvo `Hotel` počinje slovom pisani velikim slovima ("Pascal slučaju"). Ovo je uobičajeni scenarij u aplikacijama za .NET koje obavljaju povezivanje podataka gdje je shema cilj izvan kontrole razvojni inženjer. Umjesto kršiti .NET imenovanja smjernice tako da svojstvo naziva camel velikih, to možete prepoznati SDK za mapiranje naziva svojstava camel predmet automatski se `[SerializePropertyNamesAsCamelCase]` atribut.

> [AZURE.NOTE] SDK za .NET Azure pretraživanje koristi biblioteka [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) Serijalizacija i prilagođene model objekata i iz JSON ukloniti serijski broj. Ako je potrebno, prilagodite ovaj serijalizacije. Dodatne detalje potražite u [Upgrading za Azure pretraživanje .NET SDK verzije 1.1](search-dotnet-sdk-migration.md#WhatsNew). Primjer te je koristiti u `[JsonProperty]` atribut na na `DescriptionFr` svojstvo u primjeru iznad koda.

Drugi važno o na `Hotel` predmete nekoliko vrsta podataka javno svojstva. .NET vrste tih svojstava mapirajte njihove vrste ekvivalentan polja u definicija indeksa. Ako, na primjer, u `Category` niz svojstava karte da biste na `category` polje koje je vrste `DataType.String`. Postoje slične Vrsta mapiranja između `bool?` i `DataType.Boolean`, `DateTimeOffset?` i `DataType.DateTimeOffset`, itd. Određena pravila za mapiranje vrsta su navedenih s na `Documents.Get` metoda na [MSDN-u](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Ovu mogućnost da biste koristili vlastiti klase kao dokumenata radi u oba smjera; Dohvaćanje rezultata pretraživanja i imaju SDK automatski ukloniti serijski broj ih s vrstom po izboru, kao što je prikazano u [sljedećem članku](search-query-dotnet.md).

> [AZURE.NOTE] .NET SDK za pretraživanje Azure podržava i dinamički upisali dokumenata pomoću na `Document` predmete, mapiranje ključa vrijednosti od naziva polja da biste vrijednosti polja. To je korisno u slučajevima gdje ne znate shemi indeksa u trenutku dizajniranja ili gdje je bio bi inconvenient povezati klase konkretan model. Sve načine u SDK koje se tiču dokumenata ste overloads kojima se koriste u `Document` predmete, kao i svakako upisali overloads koje koriste parametar generički vrste. Samo drugu mogućnost se koriste u primjeru koda u ovom članku. Možete saznati više o na `Document` klase [na MSDN-u](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Važne napomene o vrstama podataka**

Prilikom dizajniranja vlastite klase model da biste mapirali indeks pretraživanja Azure, preporučujemo da deklariranje svojstva vrsta vrijednosti kao što su `bool` i `int` koji treba (na primjer, `bool?` umjesto `bool`). Ako koristite koji nisu svojstvo, morate **jamči** da nema dokumenata u indeks sadrže null vrijednost za odgovarajuće polje. SDK ni servisa za pretraživanje za Azure pomoći će vam da biste nametnuli to.

Ovo nije samo hipotetska problem: Zamislite scenarij u kojem Dodavanje novog polja da biste postojeći indeks koji je vrste `DataType.Int32`. Nakon ažuriranja definicija indeksa, svi dokumenti će imati null vrijednost tog novog polja (jer su svi tipovi koji u pretraživanju Azure). Ako koristite razredu model s u ne-koji `int` svojstava za to polje, prikazat će vam se na `JsonSerializationException` ovako prilikom dohvaćanja dokumenata:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Zbog toga preporučujemo da koristite koji vrste na razrede modela kao preporučenim načinom rada.

## <a name="next"></a>Sljedeći
Po puniti indeksu pretraživanja Azure, bit ćete spremni ste za početak izdavanja upita za pretraživanje dokumenata. Detalje potražite u članku [Upita vaše Azure indeks pretraživanja](search-query-overview.md) .
