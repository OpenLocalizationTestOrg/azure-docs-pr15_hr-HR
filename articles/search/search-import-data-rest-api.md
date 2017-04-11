<properties
    pageTitle="Prijenos podataka u pretraživanju Azure pomoću REST API-JA | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Saznajte kako prenijeti podatke indeksa u pretraživanju Azure pomoću REST API-JA."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Prijenos podataka Azure pretraživanje pomoću REST API-JA
> [AZURE.SELECTOR]
- [Pregled](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [OSTALE](search-import-data-rest-api.md)

U ovom se članku vidjet ćete kako koristiti [Azure pretraživanje REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) za uvoz podataka u indeks pretraživanja Azure.

Prije početka ovog vodiča, trebali biste već ste [stvorili indeks pretraživanja Azure](search-what-is-an-index.md).

Da bi se automatske dokumenata u indeks pomoću REST API-JA, će izdati zahtjev HTTP POST na krajnjoj točki URL vaš indeks. U tijelu tijelo HTTP zahtjev je JSON objekt koji sadrži dokumente koji se dodaju, promijenili ili izbrisali.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Li. Prepoznavanje api-ključ administrator servisa Azure pretraživanja
Kada izdavanja HTTP zahtjeva protiv usluge pomoću REST API-JA, *svaki* API zahtjeva mora sadržavati api-tipku koja je stvorena servisa za pretraživanje koji dodjeli. Imate valjani ključ uspostavlja pouzdanost, na temelju po zahtjev, između aplikacija slanja zahtjeva i servis na koji se rukuje.

1. Da biste pronašli na servisu api-tipki morate se prijaviti na [Portal za Azure](https://portal.azure.com/)
2. Idite na plohu servisa Azure pretraživanja
3. Kliknite ikonu "Tipke"

Na servisu imat će administrator te *tipki* *upita*.

  - Primarnih i sekundarnih *ključeva za administratore* dodijeliti prava sve operacije, uključujući mogućnost za upravljanje servisom, stvaranje i brisanje indeksa, indexers i izvora podataka. Postoje dvije tipke tako da možete nastaviti koristiti tipku sekundarne Ako odlučite Obnovi primarni ključ i obratno.
  - *Strelicama upita* dopustiti pristup samo za čitanje indekse i dokumentima i obično distribuirati s klijentskim aplikacijama taj problem zahtjeva za pretraživanje.

U svrhu uvoz podataka u indeks, možete koristiti ključ primarni ili sekundarni administrator.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Odlučite akcijske indeksiranja za korištenje
Prilikom korištenja REST API-JA, HTTP POST zahtjeve će problem s tijela JSON zahtjev za URL indeks pretraživanja Azure krajnjoj točki. Objekt JSON vaše tijelo HTTP zahtjev će sadržavati jednostruko JSON polje pod nazivom "vrijednost" koja sadrži JSON objekata koji predstavlja dokumente koje želite dodati u indeks, ažuriranje ili Izbriši.

Svaki objekt JSON u polju "vrijednost" predstavlja dokumenta indeksirati. Svaki od ovih objekata sadrži ključ dokumenta i navodi željenu radnju indeksiranja (prijenos pisma, brisanje, itd). Ovisno o tome što se ispod akcije koje odaberete samo određenih polja mora biti uključeno za svaki dokument:

@search.action | Opis | Potrebna polja za svaki dokument | Bilješke
--- | --- | --- | ---
`upload` | Na `upload` akcija je slična je "upsert" gdje umetnuti ako je novo i ažurirati/zamijeniti ako postoji u dokumentu. | tipka plus sva ostala polja koje želite definirati | Kada ažuriranje/Zamjena postojećeg dokumenta, svako polje koje nije naveden u zahtjevu za će imati njegovo polje postavljeno na `null`. To se događa čak i kad je polje prethodno postavljen na vrijednost koja nije null.
`merge` | Ažurira postojeći dokument određena polja. Ako dokument ne postoji u indeksu, spajanja neće uspjeti. | tipka plus sva ostala polja koje želite definirati | Bilo koje polje u spajanja navedete zamijenit će postojeće polje u dokumentu. To obuhvaća polja vrste `Collection(Edm.String)`. Na primjer, ako dokument sadrži polje `tags` vrijednošću `["budget"]` i izvršavanje spajanja s vrijednošću `["economy", "pool"]` za `tags`, konačne vrijednosti u `tags` polje će biti `["economy", "pool"]`. Neće biti `["budget", "economy", "pool"]`.
`mergeOrUpload` | Time se ponaša kao `merge` Ako dokument s određenom ključ već postoji u indeks. Ako dokument ne postoji, ponaša kao `upload` novi dokument. | tipka plus sva ostala polja koje želite definirati | -
`delete` | Uklanja navedeni dokument iz indeksa. | samo s ključem | Sva polja navedite osim polja ključa će je zanemariti. Ako želite ukloniti pojedinačnih polja iz dokumenta pomoću `merge` umjesto i jednostavno polje postavljeno na izričito null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. Sastavljanje HTTP zahtjev i zatražite tijelo
Sad kad ste prikupili vrijednosti polja potrebne za indeks Akcije, spremni ste za sastavljanje stvarni HTTP zahtjev i tijelo JSON zahtjev za uvoz podataka.

#### <a name="request-and-request-headers"></a>Zahtjev i zaglavlja zahtjeva
U URL-a, morat ćete unijeti naziv servisa, naziv indeksa ("Hoteli" u ovom slučaju), kao i odgovarajuće API verzija (najnoviju verziju API `2015-02-28` u trenutku ovaj dokument za objavljivanje). Morat ćete odrediti na `Content-Type` i `api-key` zahtjev zaglavlja. Za drugu mogućnost, koristite jednu od tipki na servisu administrator.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Zahtjev za tijelo

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

U ovom slučaju ne možemo koristite `upload`, `mergeOrUpload`, a `delete` kao naš akcije pretraživanja.

Pretpostavimo da ovaj primjer "Hoteli" indeks već popunjen broj dokumenata. Imajte na umu kako ćemo nema da biste odredili sva polja moguće dokumenata prilikom korištenja `mergeOrUpload` i kako ćemo samo određene tipku dokumenta (`hotelId`) prilikom korištenja `delete`.

Osim toga, imajte na umu da samo možete uključiti do 1000 dokumenti (ili 16 MB) u jedan zahtjev za indeksiranje.

## <a name="iv-understand-your-http-response-code"></a>IV. Razumijevanje HTTP kod za odgovor
#### <a name="200"></a>200
Nakon slanja zahtjeva za uspješan indeksiranja primit će HTTP odgovor s kod stanja `200 OK`. Tijelo JSON HTTP odgovor bit će na sljedeći način:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Kod stanja `207` će biti vraćena kad najmanje jednu stavku nije uspješno indeksirana. Tijelo JSON HTTP odgovor će sadržavati informacije o neuspješnih dokumenata.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] To obično znači da opterećenje servisa za pretraživanje da točku indeksiranje zahtjeve gdje će početi da biste se vratili `503` odgovora. U ovom slučaju ne možemo iznimno preporučujemo da koji klijent kod ponovno isključiti i pričekajte provesti. Imat sustav neko vrijeme da biste oporavili, povećati izgleda za buduće zahtjeve neće uspjeti. Hitro ponovni vašim zahtjevima samo produžiti situaciji.

#### <a name="429"></a>429
Kod stanja `429` će biti vraćena kad ste premašili kvotu na broj dokumenata po indeksa.

#### <a name="503"></a>503
Kod stanja `503` će se vratiti ako nema stavki u zahtjevu za uspješno indeksirani. Ta se pogreška znači da je sustav pod velikim opterećenjem i trenutno nije moguće obraditi vaš zahtjev.

> [AZURE.NOTE] U ovom slučaju ne možemo iznimno preporučujemo da koji klijent kod ponovno isključiti i pričekajte provesti. Imat sustav neko vrijeme da biste oporavili, povećati izgleda za buduće zahtjeve neće uspjeti. Hitro ponovni vašim zahtjevima samo produžiti situaciji.

Dodatne informacije o Akcije dokumenta i odgovore uspjeh/pogreške, pročitajte članak [Dodavanje, ažuriranje ili brisanje dokumenata](https://msdn.microsoft.com/library/azure/dn798930.aspx). Dodatne informacije o drugim HTTP kodovima stanja koju nije moguće vratiti u slučaju potražite u članku [kodovi stanja HTTP (Azure pretraživanje)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Sljedeći
Po puniti indeksu pretraživanja Azure, bit ćete spremni ste za početak izdavanja upita za pretraživanje dokumenata. Detalje potražite u članku [Upita vaše Azure indeks pretraživanja](search-query-overview.md) .
