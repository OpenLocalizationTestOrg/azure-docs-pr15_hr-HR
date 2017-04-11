<properties
    pageTitle="Upit indeksu pretraživanja Azure pomoću .NET SDK | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Sastavljanje upita za pretraživanje u odjeljku Azure pretraživanje i korištenje parametara pretraživanja za filtriranje i sortiranje rezultata pretraživanja."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Indeks pretraživanja Azure pomoću .NET SDK za upite
> [AZURE.SELECTOR]
- [Pregled](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [OSTALE](search-query-rest-api.md)

U ovom se članku će vam pokazati kako indeks koristeći [Azure pretraživanje .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)za upite.

Prije početka ovog vodiča, trebali biste već ste [stvorili indeks pretraživanja Azure](search-what-is-an-index.md) i [popunjeno je s podacima](search-what-is-data-import.md).

Imajte na umu da sve uzorak koda u ovom članku napisan C#. Možete pronaći izvor cijeli kod [na GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Li. Prepoznavanje servisa Azure pretraživanja upita api-ključ
Sad kad ste stvorili indeks pretraživanja Azure, spremni ste gotovo izdavanje upita pomoću .NET SDK. Najprije morate nabaviti upita api-tipki koja je stvorena servisa za pretraživanje koji dodjeli. .NET SDK će poslati api-ključ na svaki zahtjev za uslugu. Imate valjani ključ uspostavlja pouzdanost, na temelju po zahtjev, između aplikacija slanja zahtjeva i servis na koji se rukuje.

1. Da biste pronašli na servisu api-tipki morate se prijaviti na [Portal za Azure](https://portal.azure.com/)
2. Idite na plohu servisa Azure pretraživanja
3. Kliknite ikonu "Tipke"

Na servisu imat će administrator te *tipki* *upita*.

  - Primarnih i sekundarnih *ključeva za administratore* dodijeliti prava sve operacije, uključujući mogućnost za upravljanje servisom, stvaranje i brisanje indeksa, indexers i izvora podataka. Postoje dvije tipke tako da možete nastaviti koristiti tipku sekundarne Ako odlučite Obnovi primarni ključ i obratno.
  - *Strelicama upita* dopustiti pristup samo za čitanje indekse i dokumentima i obično distribuirati s klijentskim aplikacijama taj problem zahtjeva za pretraživanje.

Za potrebe upita indeksa, možete koristiti jedan od ključeva za upit. Ključeva administrator može se koristiti za upite, ali koristite tipke upita u kodu aplikacije kao što je to bolje slijedi [načelo najmanjih ovlasti](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Stvaranje instanca klase SearchIndexClient
Da biste problem upiti s SDK za .NET Azure pretraživanja, morat ćete stvoriti instancu na `SearchIndexClient` predmete. Klase ima nekoliko constructors. Onu koju želite uzima naziv servisa za pretraživanje, naziv indeksa i `SearchCredentials` objekt kao parametara. `SearchCredentials`prelama ključ API-ja.

Stvara novu kodu niže `SearchIndexClient` za indeks "Hoteli" (stvorena u [Stvaranje indeksa pretraživanja Azure pomoću .NET SDK](search-create-index-dotnet.md)) pomoću vrijednosti za naziv servisa za pretraživanje i api-ključ pohranjenima u konfiguracijskoj datoteci aplikacije (`app.config` ili `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`sadrži na `Documents` svojstvo. Ovo svojstvo sadrži sve načine morate indeksa Azure pretraživanja za upite.

## <a name="iii-query-your-index"></a>III. Indeks za upite
Pretraživanje s .NET SDK je jednostavno zovete u `Documents.Search` način na vašem `SearchIndexClient`. Ovaj postupak vodi nekoliko parametrima, uključujući tekst za pretraživanje s na `SearchParameters` objekt koji se mogu koristiti da biste dodatno suzili upit.

#### <a name="types-of-queries"></a>Vrste upita
Dvije glavne [vrste upita](search-query-overview.md#types-of-queries) koristit će se `search` i `filter`. A `search` upit traži jedan ili više izraza u svim poljima _pretraživanja_ indeksa. A `filter` upita procjenjuje Booleov izraz preko svih _koje je moguće filtrirati_ polja u indeks.

Pretraživanja i filtri se izvode pomoću na `Documents.Search` način. Upit za pretraživanje možete proslijediti u na `searchText` parametar, dok izraza filtra možete proslijediti u na `Filter` svojstvo na `SearchParameters` predmete. Da biste filtrirali bez pretraživanja, samo proći `"*"` za na `searchText` parametar. Da biste pretražili bez filtriranja, samo ostavite na `Filter` svojstvo uklanjanje lozinke, ili prosljeđivanje u na `SearchParameters` instancu uopće.

#### <a name="example-queries"></a>Primjer upita

Sljedeći primjer kod prikazuje na nekoliko različitih načina upit indeks "Hoteli" definiran stvaranje [indeksa pretraživanja Azure pomoću .NET SDK-a](search-create-index-dotnet.md#DefineIndex). Imajte na umu da dokumenata vratio rezultate pretraživanja pojave na `Hotel` klase koja je definirana u [Uvoz podataka u pretraživanju Azure pomoću .NET SDK-a](search-import-data-dotnet.md#HotelClass). Ogledni kod korištenje čini u `WriteDocuments` način za slanje rezultate pretraživanja na konzoli sustava. Ovaj postupak je opisan u sljedećem odjeljku.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Držač za rezultate pretraživanja
Na `Documents.Search` način vraća na `DocumentSearchResult` objekt koji sadrži rezultate upita. U primjeru u prethodnom odjeljku koristi metode pod nazivom `WriteDocuments` za slanje rezultata pretraživanja na konzolu:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Evo što izgledati rezultat upita u prethodnom odjeljku, pod pretpostavkom "Hoteli" indeks je popunjen ogledne podatke iz [Uvoza podataka u pretraživanju Azure pomoću .NET SDK](search-import-data-dotnet.md):

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Ogledni kod iznad koristi konzolu za slanje rezultata pretraživanja. Isto tako ćete prikazati rezultate pretraživanja u vlastiti računala. Pogledajte [ovaj uzorak na GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) za primjer za prikazivanje rezultata pretraživanja u ASP.NET MVC sustavom web-aplikacije.
