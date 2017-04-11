<properties
    pageTitle="Stvaranje indeksa pretraživanja Azure pomoću .NET SDK | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Stvaranje indeksa u kodu korištenjem SDK za .NET Azure pretraživanja."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Stvaranje indeksa pretraživanja Azure pomoću .NET SDK
> [AZURE.SELECTOR]
- [Pregled](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [OSTALE](search-create-index-rest-api.md)


U ovom se članku će vas voditi kroz postupak stvaranja Azure pretraživanje [indeksa](https://msdn.microsoft.com/library/azure/dn798941.aspx) koristeći [SDK za .NET Azure pretraživanja](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Prije nego što slijedi ovaj vodič i stvaranje indeksa, imat ćete već [stvorene na servis za pretraživanje Azure](search-create-service-portal.md).

Imajte na umu da sve uzorak koda u ovom članku napisan C#. Možete pronaći izvor cijeli kod [na GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Li. Prepoznavanje api-ključ administrator servisa Azure pretraživanja
Sad kad ste dodjeli servis za pretraživanje Azure, spremni ste gotovo izdavanje zahtjeva protiv servisa krajnjoj točki pomoću .NET SDK-a. Najprije morate nabaviti administrator api-tipki koja je stvorena servisa za pretraživanje koji dodjeli. .NET SDK će poslati api-ključ na svaki zahtjev za uslugu. Imate valjani ključ uspostavlja pouzdanost, na temelju po zahtjev, između aplikacija slanja zahtjeva i servis na koji se rukuje.

1. Da biste pronašli na servisu api-tipki morate se prijaviti na [Portal za Azure](https://portal.azure.com/)
2. Idite na plohu servisa Azure pretraživanja
3. Kliknite ikonu "Tipke"

Na servisu imat će administrator te *tipki* *upita*.

  - Primarnih i sekundarnih *ključeva za administratore* dodijeliti prava sve operacije, uključujući mogućnost za upravljanje servisom, stvaranje i brisanje indeksa, indexers i izvora podataka. Postoje dvije tipke tako da možete nastaviti koristiti tipku sekundarne Ako odlučite Obnovi primarni ključ i obratno.
  - *Strelicama upita* dopustiti pristup samo za čitanje indekse i dokumentima i obično distribuirati s klijentskim aplikacijama taj problem zahtjeva za pretraživanje.

U svrhu stvaranje indeksa, možete koristiti ključ primarni ili sekundarni administrator.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Stvaranje instanca klase SearchServiceClient
Da biste počeli koristiti SDK za .NET Azure pretraživanja, morat ćete stvoriti instancu na `SearchServiceClient` predmete. Klase ima nekoliko constructors. Onaj koji želite uzima naziv za servis za pretraživanje, a `SearchCredentials` objekt kao parametara. `SearchCredentials`prelama ključ API-ja.

Stvara novu kodu niže `SearchServiceClient` pomoću vrijednosti za naziv servisa za pretraživanje i api-ključ pohranjenima u konfiguracijskoj datoteci aplikacije (`app.config` ili `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`sadrži programa `Indexes` svojstvo. Ovo svojstvo sadrži sve načine potrebnih za stvaranje popisa, ažuriranje i brisanje indeksa pretraživanja Azure.

> [AZURE.NOTE] Na `SearchServiceClient` predmete upravlja veza sa servisa za pretraživanje. Da biste izbjegli otvaranja previše veza, trebali biste pokušate zajednički koristiti jednu instancu `SearchServiceClient` u aplikaciji ako je to moguće. Postupke su niti sigurnih za omogućivanje takve zajedničkog korištenja.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Definiranje pomoću pretraživanja Azure indeksa u `Index` klase
Jedan poziv na `Indexes.Create` način će stvoriti indeks. Ta metoda uzima kao parametar programa `Index` objekt koji definira indeksu pretraživanja Azure. Morate stvoriti programa `Index` objekta te inicijalizaciju na sljedeći način:

1. Postavljanje na `Name` svojstvo na `Index` objekt na naziv indeksa.
2. Postavljanje na `Fields` svojstvo na `Index` objekt da biste polje `Field` objekte. Svaki od na `Field` objekata definira ponašanje polja u indeks. Možete unijeti naziv polja za Graditelj uz vrsta podataka (ili alata za analizu za niz polja). Možete postaviti i druga svojstva kao što su `IsSearchable`, `IsFilterable`, itd.

Važno da zadržite potrebama pretraživanje korisničko sučelje i business na umu pri dizajniranju indeks kao svaki je `Field` mora biti dodijeljena [odgovarajuća svojstva](https://msdn.microsoft.com/library/azure/dn798941.aspx). Ove svojstva upravljanje prikazanim pretraživanje značajke (filtriranje, faceting, sortiranje pretraživanje po cijelom tekstu, itd.) primjenjuju se na koja se polja. Za neko svojstvo izričito ne postavite, na `Field` klase po zadanom je onemogućivanje odgovarajuće značajke pretraživanja, osim ako izričito Omogući.

Za našeg primjera ćemo ste pod nazivom naš indeks "Hoteli" i definira naš polja na sljedeći način:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Ne možemo pažljivo odlučili vrijednosti svojstava za svaku `Field` na temelju kako ćemo mislite da će se koristiti u aplikaciji. Na primjer, vjerojatno je osobe koje pretražuju za Hoteli će biti zainteresirani podudaranja ključnih riječi na na `description` polje, pa ne možemo Omogućivanje pretraživanja cijelog teksta za to polje postavljanjem `IsSearchable` da biste `true`.

Uzmite u obzir te točno glasnoću indeksa vrste `DataType.String` mora biti na onu koja je označena kao polje _ključa_ postavljanjem `IsKey` da biste `true` (potražite u članku `hotelId` u gornjem primjeru).

Definicija indeksa iznad koristi alat za analizu prilagođeni jezik za na `description_fr` jer je namijenjen za pohranu teksta na francuskom jeziku. U odjeljku [jezik koji podržava temu na MSDN-](https://msdn.microsoft.com/library/azure/dn879793.aspx) kao i u odgovarajuće [bloga](https://azure.microsoft.com/blog/language-support-in-azure-search/) za dodatne informacije o analyzers jezik.

> [AZURE.NOTE]  Imajte na umu da prosljeđivanjem `AnalyzerName.FrLucene` u Graditelj, u `Field` bit će automatski vrste `DataType.String` a bit će vam `IsSearchable` postavljena na `true`.

## <a name="iv-create-the-index"></a>IV. Stvaranje indeksa
Sad kad ste je inicijalizirana `Index` objekt, možete stvoriti indeks jednostavno tako da nazovete `Indexes.Create` na vašem `SearchServiceClient` objekta:

```csharp
serviceClient.Indexes.Create(definition);
```

Za zahtjev za uspješan način na koji će vratiti normalno. Ako postoji problem sa zahtjevom za kao što su nevaljani parametar, će vratiti metodu `CloudException`.

Kada završite s indeks, a želite izbrisati, samo poziva na `Indexes.Delete` način na vašem `SearchServiceClient`. Na primjer, to je kako želite izbrisati indeks "Hoteli":

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] Primjer kod u ovom se članku koristi sinkrono metode SDK za .NET Azure pretraživanje jednostavnosti. Preporučujemo da koristite asinkronog metode u vlastiti aplikacije da biste ih zadržali skalabilni i odredište. Na primjer, u primjerima iznad nije moguće koristiti `CreateAsync` i `DeleteAsync` umjesto `Create` i `Delete`.

## <a name="next"></a>Sljedeći
Kada stvorite indeks pretraživanja Azure, vi ćete biti spremni [prenijeti sadržaj u indeks](search-what-is-data-import.md) da bi se pokrenuti pretraživanje podataka.
