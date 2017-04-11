<properties
    pageTitle="Stvaranje indeksa pretraživanja Azure pomoću REST API-JA | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Stvaranje indeksa u kodu korištenjem Azure pretraživanje HTTP REST API-JA."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Stvaranje indeksa pretraživanja Azure pomoću REST API-JA
> [AZURE.SELECTOR]
- [Pregled](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [OSTALE](search-create-index-rest-api.md)


U ovom se članku će vas voditi kroz postupak stvaranja Azure pretraživanje [indeksa](https://msdn.microsoft.com/library/azure/dn798941.aspx) koristeći Azure pretraživanje REST API-JA.

Prije nego što slijedi ovaj vodič i stvaranje indeksa, imat ćete već [stvorene na servis za pretraživanje Azure](search-create-service-portal.md).

Da biste stvorili indeks pretraživanja Azure pomoću REST API-JA, će izdati jedan zahtjev HTTP POST na krajnjoj točki URL-a servisa Azure pretraživanja. Definicije indeks će se nalaziti u tijelu zahtjeva kao ispravnog JSON sadržaj.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Li. Prepoznavanje api-ključ administrator servisa Azure pretraživanja
Sad kad ste dodjeli servis za pretraživanje Azure, možete izdati HTTP zahtjeva protiv krajnjoj točki URL-a na servisu pomoću REST API-JA. Međutim, *sve* API zahtjeve mora sadržavati api-tipku koja je stvorena servisa za pretraživanje koji dodjeli. Imate valjani ključ uspostavlja pouzdanost, na temelju po zahtjev, između aplikacija slanja zahtjeva i servis na koji se rukuje.

1. Da biste pronašli na servisu api-tipki morate se prijaviti na [Portal za Azure](https://portal.azure.com/)
2. Idite na plohu servisa Azure pretraživanja
3. Kliknite ikonu "Tipke"

Na servisu imat će administrator te *tipki* *upita*.

 - Primarnih i sekundarnih *ključeva za administratore* dodijeliti prava sve operacije, uključujući mogućnost za upravljanje servisom, stvaranje i brisanje indeksa, indexers i izvora podataka. Postoje dvije tipke tako da možete nastaviti koristiti tipku sekundarne Ako odlučite Obnovi primarni ključ i obratno.
 - *Strelicama upita* dopustiti pristup samo za čitanje indekse i dokumentima i obično distribuirati s klijentskim aplikacijama taj problem zahtjeva za pretraživanje.

U svrhu stvaranje indeksa, možete koristiti ključ primarni ili sekundarni administrator.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Definiranje indeksu pretraživanja Azure pomoću ispravnog JSON
Jedan objavu HTTP zahtjev za uslugu će stvoriti indeks. Tijelu zahtjeva za HTTP POST će sadržavati pojedinačni JSON objekt koji se definira indeksu pretraživanja Azure.

1. Prvo svojstvo taj objekt JSON je naziv indeksa.
2. Svojstvo drugog objekta JSON je JSON polja pod nazivom `fields` koja sadrži zasebni objekt JSON za svako polje indeksa. Svaki od ovih objekata JSON sadrže više parova naziv/vrijednost za svaku atributa polja uključujući "naziv", "Vrsta", itd.

Važno da zadržite potrebama pretraživanje korisničko sučelje i business na umu pri dizajniranju indeks, kao što je svako polje mora biti dodijeljena [proper atribute](https://msdn.microsoft.com/library/azure/dn798941.aspx)je. Polja koja primijeniti te atribute upravljanje prikazanim pretraživanje značajke (filtriranje, faceting, sortiranje pretraživanje po cijelom tekstu, itd.). Za bilo koji ne navedete atribut, zadana će se da biste omogućili odgovarajuće značajke pretraživanja, osim ako izričito Onemogući.

Za našeg primjera ćemo ste pod nazivom naš indeks "Hoteli" i definiranih naš polja na sljedeći način:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Ne možemo pažljivo ste odabrali atribute indeks za svako polje na temelju kako ćemo mislite da će se koristiti u aplikaciji. Ako, na primjer, `hotelId` je jedinstveni ključ koja je korisnicima traženjem Hoteli vjerojatno neće znati, pa onemogućit ćemo pretraživanja cijelog teksta za to polje postavljanjem `searchable` da biste `false`, čime se štedi prostor u indeks.

Uzmite u obzir te točno glasnoću indeksa vrste `Edm.String` mora biti na onu koja je označena kao polje "tipke".

Definicija indeksa iznad koristi alat za analizu prilagođeni jezik za na `description_fr` jer je namijenjen za pohranu teksta na francuskom jeziku. U odjeljku [jezik koji podržava temu na MSDN-](https://msdn.microsoft.com/library/azure/dn879793.aspx) kao i u odgovarajuće [bloga](https://azure.microsoft.com/blog/language-support-in-azure-search/) za dodatne informacije o analyzers jezik.

## <a name="iii-issue-the-http-request"></a>III. Poslao zahtjev za HTTP
1. Pomoću definicije indeks kao tijelo zahtjev, problema zahtjev HTTP POST na krajnjoj točki URL servisa Azure pretraživanja. U URL-u, provjerite jeste li servis naziv koristiti kao naziv glavnog računala i postavili na vlastito `api-version` kao parametra niza upita (najnoviju verziju API `2015-02-28` u trenutku ovaj dokument za objavljivanje).
2. U zaglavljima zahtjev za određivanje u `Content-Type` kao `application/json`. Također morat ćete unijeti ključ za administratore na servisu koji ste naveli u koraku kojeg u na `api-key` zaglavlje.


Će morati unijeti vlastiti servisa ime i api ključ za poslao zahtjev za ispod:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Za zahtjev za uspješan, trebali biste vidjeti Šifra stanja 201 (stvorena). Dodatne informacije o stvaranju indeksa putem REST API-JA, posjetite API reference na [MSDN-u](https://msdn.microsoft.com/library/azure/dn798941.aspx). Dodatne informacije o drugim HTTP kodovima stanja koju nije moguće vratiti u slučaju potražite u članku [kodovi stanja HTTP (Azure pretraživanje)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Kada završite s indeks, a želite izbrisati, samo poslao zahtjev za brisanje HTTP-a. Na primjer, to je kako želite izbrisati indeks "Hoteli":

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Sljedeći
Kada stvorite indeks pretraživanja Azure, vi ćete biti spremni [prenijeti sadržaj u indeks](search-what-is-data-import.md) da bi se pokrenuti pretraživanje podataka.
