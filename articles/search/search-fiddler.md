<properties
    pageTitle="Kako koristiti Fiddler za procjenu i testiranje Azure pretraživanje REST API-ji | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Korištenje Fiddler za pristup kod slobodno Provjera dostupnosti Azure pretraživanja i isprobavate REST API-ji."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Korištenje Fiddler procijeniti i testiranje Azure pretraživanje REST API-ji
> [AZURE.SELECTOR]
- [Pregled](search-query-overview.md)
- [Search Explorer](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [OSTALE](search-query-rest-api.md)

U ovom se članku objašnjava kako pomoću Fiddler, [besplatno preuzeti s Telerik](http://www.telerik.com/fiddler), problem HTTP zahtjevima za i pregled odgovora pomoću Azure pretraživanje REST API-JA, bez potrebe za pisanja koda. Azure pretraživanja nije potpuno upravlja, hostira oblaka servisa za pretraživanje na Microsoft Azure, jednostavno tipkovnicu putem .NET i REST API-ji. Servis za pretraživanje Azure REST API-ji su navedenih na [MSDN-u](https://msdn.microsoft.com/library/azure/dn798935.aspx).

U sljedećim koracima ćete stvaranje indeksa, prijenos dokumenata, indeks za upite i upit sustava za informacije o usluzi.

Da biste dovršili ove korake, morate je servis za pretraživanje Azure i `api-key`. Upute za početak rada potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) .

## <a name="create-an-index"></a>Stvaranje indeksa

1. Pokrenite Fiddler. Na izborniku **datoteka** , isključite mogućnost **Snimanja promet** da biste sakrili strani HTTP aktivnosti koja je nepovezanih trenutnog zadatka.

3. Na kartici **Skladatelj** ćete formulate zahtjev koji izgleda kao na sljedećem zaslonu snimka.

    ![][1]

2. Odaberite **Postavljanje**.

3. Unesite URL koji određuje URL servisa, atribute zahtjev i api-verzija. Nekoliko uputa treba imati na umu:
   + HTTPS upotrijebili prefiks.
   + Zahtjev je atribut "/ indeksi/Hoteli". Ovo govori pretraživanja da biste stvorili indeks pod nazivom "Hoteli".
   + API-verzija nije mala slova, naveden kao "? api-verzija = 2015 02 28". API verzije su važne jer pretraživanje Azure redovito uvodi ažuriranja. Na koje se rijetko pojavljuju prilike ažuriranje servisa može uzrokovati prijelom promjena na API-JA. Zbog toga Azure pretraživanje potreban je verzija API-ja na svaki zahtjev za tako da budu u potpunu kontrolu nad koji će se koristi.

    Cijeli URL trebao bi izgledati slično kao u sljedećem primjeru.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Navedite zaglavlje zahtjev za glavno računalo i ključ za api zamjenjuje vrijednosti koje su valjane servisa.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  U tijelu zahtjeva zalijepite polja koja čine definicija indeksa.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Kliknite **izvršiti**.

U nekoliko sekundi, vidjet ćete je HTTP 201 odgovor na popisu sesiju koji označava uspješno stvoren indeks.

Ako se pojavi HTTP 504, provjerite je li URL određuje HTTPS. Ako vidite HTTP 400 ili 404, potražite u tijelu zahtjeva da biste provjerili postoje pogreške ne Kopiraj – Zalijepi. HTTP 403 obično ukazuje na problem s api-ključ (koji nisu valjani ključ ili sintaksa problem s kako je navedeno tipku API-ja).

## <a name="load-documents"></a>Učitavanje dokumenata

Na kartici **Skladatelj** vaš zahtjev objaviti dokumente izgledat će ovako. U tijelu zahtjeva za sadrži pretraživanje podataka za 4 hoteli.

   ![][2]

1. Odaberite **OBJAVI**.

2.  Unesite URL koji počinje s HTTPS, nakon čega slijedi URL servisa, nakon čega slijedi "/indexes/ <'indexname ' >/dokumenti/indeksa? api-verzija = 2015 02 28". Cijeli URL trebao bi izgledati slično kao u sljedećem primjeru.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Zahtjev za zaglavlja mora biti jednak prije. Imajte na umu da ste zamijenili glavno računalo i ključ za API-ja s vrijednostima koje su valjane servisa.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Zahtjev za tijelo sadrži četiri dokumenata koja će se dodati indeks hoteli.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Kliknite **izvršiti**.

U nekoliko sekundi, trebali biste vidjeti je HTTP 200 odgovor na popisu sesiju. To označava uspješno su stvoreni dokumenti. Ako nailazite na 207, nije uspjelo barem jedan dokument da biste prenijeli. Ako nailazite na 404, imate pogreške u sintaksi u zaglavlju ili tijelu zahtjeva.

## <a name="query-the-index"></a>Indeks za upite

Sad kad učitavaju se indeksa i dokumenata, možete izdati upite odabiranja ih.  Na kartici **Skladatelj** **DOBITI** naredbe upite na servisu će izgledati slično kao na sljedećem zaslonu snimka.

   ![][3]

1.  Odaberite **Početak**.

2.  Unesite URL koji počinje s HTTPS, nakon čega slijedi URL servisa, nakon čega slijedi "/indexes/ <'indexname ' > / dokumenti?", a zatim parametre upita. Putem, primjerice, koristite sljedeći URL, zamjenjuje onaj koji je valjan servisa Ogledni naziv glavnog računala.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Ovaj upit pretražuje na termina "motel" i dohvaća fasete kategorija za ocjena.

3.  Zahtjev za zaglavlja mora biti jednak prije. Imajte na umu da ste zamijenili glavno računalo i ključ za API-ja s vrijednostima koje su valjane servisa.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Šifra odaziva mora biti 200, a izlaz odgovor trebao bi izgledati slično kao na sljedećem zaslonu snimka.

   ![][4]

Sljedeći primjer upit je [postupak za indeks pretraživanja (Azure pretraživanje API -JA)](http://msdn.microsoft.com/library/dn798927.aspx) na MSDN-u. Mnoge upita primjer u temi sadržavati razmake nedopuštene u Fiddler. Zamijeni svaki razmak s a znakom + prije lijepljenja u upitu niza prije upita u Fiddler.

**Prije nego što razmake zamjenjuju:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Kada se zamjenjuje razmake +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Upit u sustavu

Možete upita i sustav da biste dobili broji dokument i potrošnje prostora za pohranu. Na kartici **Skladatelj** zahtjev će izgledati otprilike ovako, a odgovor vratit će ukupan broj dokumenata i prostor koji zauzimaju.

 ![][5]

1.  Odaberite **Početak**.

2.  Unesite URL koji sadrži URL servisa, nakon čega slijedi "/ Hoteli/indeksi/stat? api-verzija = 2015 02 28":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Navedite zaglavlje zahtjev za glavno računalo i ključ za api zamjenjuje vrijednosti koje su valjane servisa.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  U tijelu zahtjeva ostavite praznim.

5.  Kliknite **izvršiti**. Prikazat će se Šifra stanja HTTP 200 na popisu sesiju. Odaberite stavku za naredbu.

6.  Kliknite karticu **Provjera** , kliknite karticu **zaglavlja** , a zatim JSON OSNOVNI oblik. Trebali biste vidjeti count i pohranu veličina dokumenta (u KB).

## <a name="next-steps"></a>Daljnji koraci

Bez koda potražite u članku [Upravljanje servisa za pretraživanje na Azure](search-manage.md) postići možete upravljati i pomoću pretraživanja Azure.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
