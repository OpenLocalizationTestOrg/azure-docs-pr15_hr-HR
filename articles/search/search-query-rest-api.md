<properties
    pageTitle="Indeks pretraživanja Azure pomoću REST API-JA za upite | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Sastavljanje upita za pretraživanje u odjeljku Azure pretraživanje i korištenje parametara pretraživanja za filtriranje i sortiranje rezultata pretraživanja."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Indeks pretraživanja Azure pomoću REST API-JA za upite
> [AZURE.SELECTOR]
- [Pregled](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [OSTALE](search-query-rest-api.md)

U ovom se članku će vam pokazati kako indeks koristeći [Azure pretraživanje REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx)za upite.

Prije početka ovog vodiča, trebali biste već ste [stvorili indeks pretraživanja Azure](search-what-is-an-index.md) i [popunjeno je s podacima](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Li. Prepoznavanje servisa Azure pretraživanja upita api-ključ
Ključne komponente svaki operacije pretraživanja na temelju Azure pretraživanje REST API-JA je *ključ za api* koja je stvorena za servis koji je dodijeljen. Imate valjani ključ uspostavlja pouzdanost, na temelju po zahtjev, između aplikacija slanja zahtjeva i servis na koji se rukuje.

1. Da biste pronašli na servisu api-tipki morate se prijaviti na [Portal za Azure](https://portal.azure.com/)
2. Idite na plohu servisa Azure pretraživanja
3. Kliknite ikonu "Tipke"

Na servisu imat će administrator te *tipki* *upita*.

 - Primarnih i sekundarnih *ključeva za administratore* dodijeliti prava sve operacije, uključujući mogućnost za upravljanje servisom, stvaranje i brisanje indeksa, indexers i izvora podataka. Postoje dvije tipke tako da možete nastaviti koristiti tipku sekundarne Ako odlučite Obnovi primarni ključ i obratno.
 - *Strelicama upita* dopustiti pristup samo za čitanje indekse i dokumentima i obično distribuirati s klijentskim aplikacijama taj problem zahtjeva za pretraživanje.

Za potrebe upita indeksa, možete koristiti jedan od ključeva za upit. Ključeva administrator može se koristiti za upite, ali koristite tipke upita u kodu aplikacije kao što je to bolje slijedi [načelo najmanjih ovlasti](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-formulate-your-query"></a>II. Formulate upita
Da biste [pronašli indeks pomoću REST API -JA](https://msdn.microsoft.com/library/azure/dn798927.aspx)na dva načina. Način je poslao zahtjev za HTTP POST kojoj definirana vaše parametara upita u objektu JSON u tijelu zahtjeva. Drugi način je poslao zahtjev za HTTP GET kojoj definirana vaše parametara upita u zahtjev za URL-a. Imajte na umu da objavu ima više [Opuštena ograničenja](https://msdn.microsoft.com/library/azure/dn798927.aspx) veličine parametara upita od GET. Zbog toga preporučujemo korištenje objavu, osim ako imate posebne okolnosti gdje GET bio praktičnije.

Za objavu i GET, morate unijeti *Naziv usluge*, *Naziv indeksa*i proper *API verzija* (najnoviju verziju API `2015-02-28` u trenutku ovaj dokument za objavljivanje) u zahtjev za URL-u. Za početak, *nizu upita* na kraju URL će se ovdje možete navesti parametre upita. Za oblik URL-a potražite u nastavku:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

Oblik za objavu nije isti, ali samo api-verzijom u parametrima niza upita.



#### <a name="example-queries"></a>Primjer upita

Evo nekoliko primjera upite za indeks pod nazivom "Hoteli". Tih upita prikazuju se u obliku PRONAĐITE i objavu.

Cijeli indeks potražite izraz "proračuna" i vratiti samo na `hotelName` polja:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Primjena filtra u indeks da biste pronašli Hoteli jeftinijim od 150 $ po noći i vratili se `hotelId` i `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Pretražili cijeli indeks, redoslijedom prema određenom polju (`lastRenovationDate`) silaznim redoslijedom trajati vrha dva rezultata, a Prikaži samo `hotelName` i `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Pošaljite zahtjev za HTTP
Sad kad ste formulated upita kao dio HTTP zahtjev za URL (za DOHVAĆANJE) ili tijelo (za objavu), možete definirati zahtjev za zaglavlja i poslati upit.

#### <a name="request-and-request-headers"></a>Zahtjev i zaglavlja zahtjeva
Morate definirati zaglavlja s dva zahtjeva za DOHVAĆANJE ili tri za objavu:
1. Na `api-key` zaglavlja mora biti postavljeno na tipku upita ste pronašli u drugom koraku li iznad. Imajte na umu da vam može poslužiti ključa administrator kao u `api-key` zaglavlje, ali preporučuje se korištenje ključa upita kao isključivo daje pristup samo za čitanje indekse i dokumentima.
2. Na `Accept` zaglavlja mora biti postavljeno na `application/json`.
3. Za objavu samo na `Content-Type` zaglavlja mora biti postavljena i na `application/json`.

Potražite u nastavku zahtjev HTTP GET za pretraživanje pomoću Azure pretraživanje REST API-JA, korištenje jednostavnog upita koji se traži termina "motel" indeks "Hoteli":

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Evo istog upita primjer pomoću HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Zahtjev za uspješan upit će rezultirati kod stanja `200 OK` i rezultati se vraćaju kao JSON u tijelu odgovor. Evo što rezultate iznad upita izgleda kao što su, uz pretpostavku da je indeks "Hoteli" popunjavaju s oglednim podacima u [Uvoz podataka u pretraživanju Azure pomoću REST API -JA](search-import-data-rest-api.md) (Imajte na umu da se JSON je oblikovan za prikaz).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Dodatne informacije potražite u odjeljku "Odgovor" [Pretraživanje](https://msdn.microsoft.com/library/azure/dn798927.aspx)dokumenata. Dodatne informacije o drugim HTTP kodovima stanja koju nije moguće vratiti u slučaju potražite u članku [kodovi stanja HTTP (Azure pretraživanje)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
