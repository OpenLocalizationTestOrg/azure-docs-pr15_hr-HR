<properties 
    pageTitle="Rad s geoprostornim podacima u Azure DocumentDB | Microsoft Azure" 
    description="Objašnjenje kako stvoriti, indeks i prostorno objekte s Azure DocumentDB za upite." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Rad s geoprostornim podacima u Azure DocumentDB

Ovaj je članak Uvod u funkcije geoprostornim u [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Kad pročitate, neće se moći odgovaraju na sljedeća pitanja:

- Kako spremati prostorno podataka u Azure DocumentDB?
- Kako upit geoprostornim podacima u DocumentDB Azure SQL i LINQ?
- Kako omogućiti ili onemogućiti prostorno indeksiranje u DocumentDB?

Pogledajte ovaj [Github projekta](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) za primjere koda.

## <a name="introduction-to-spatial-data"></a>Uvod u prostorno podatke

Prostorno podataka opisuje položaj i oblika objekata u prostoru. U većini aplikacija te odgovaraju objekata na zemlji, odnosno geoprostornih podataka. Prostorno podataka može se koristiti za predstavljanje lokaciju osobe, mjesta koja vas zanima ili granicu grada ili na lake. Uobičajena slučaja koristi često obuhvaćaju upite Blizina, za – primjerice, "pronaći sve kafićima blizu Moje trenutnog mjesta". 

### <a name="geojson"></a>GeoJSON
DocumentDB podržava upita geoprostornih točke podataka koji predstavlja pomoću [GeoJSON specifikacija](http://geojson.org/geojson-spec.html)i indeksiranja. Strukture podataka GeoJSON uvijek su valjanih objekata JSON, da bi se može spremiti i mu pomoću DocumentDB bez specijalizirane alate i biblioteke. DocumentDB SDK-ovi sadrže pomoćne klase i metoda koje olakšavaju rad s podacima prostorno. 

### <a name="points-linestrings-and-polygons"></a>Točke, linestrings i višekutnike
**Točka** označava jedan položaj prostor. U geoprostornih podataka točku predstavlja točno mjesto, koja može biti adresu na izračunatog polja, kiosku, programa automobili ili grada.  Točku predstavlja u GeoJSON (i DocumentDB) pomoću njegove koordinate par ili dužinu i širinu. Slijedi primjer JSON za točku.

**Točke u DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] Specifikacija GeoJSON određuje dužinu prvi i zemljopisnu širinu drugog. Kao i u drugim aplikacijama za mapiranje dužinu i širinu su kutova i predstavljene pomoću stupnjeva. Dužina vrijednosti su mjeri od značajke Prime meridijan i između-180 i 180.0 stupnjeva i zemljopisnu širinu vrijednosti su mjeri od na equator i između-90.0 i 90.0 stupnjeva. 
>
> DocumentDB tumači koordinate kao što je prikazano po WGS 84 referentni sustav. Pogledajte ispod više pojedinosti o sustavima koordinata referencu.

To može biti uložen u dokumentu DocumentDB kao što je prikazano u ovom primjeru korisnički profil koji sadrži podatke o lokaciji:

**Korištenje profila s mjesta pohranjena u DocumentDB**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Osim točke GeoJSON podržava LineStrings i višekutnike. **LineStrings** zamjenu za niz dva ili više točaka u prostor i segmenata crta koje povezuju ih. U geoprostornih podataka linestrings najčešće koriste se za predstavljanje autoceste ili rivers. **Poligona** je granicu povezanog točke zatvoreni LineString za obrasce. Višekutnike najčešće koriste se za predstavljanje prirodnim formations kao što su jezera smrznu ili političkih porezne uprave kao što su gradovi i stanja. Evo primjera poligona u DocumentDB. 

**Višekutnike u DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] Specifikacija GeoJSON zahtijeva da valjani višekutnike posljednje koordinata par dobili mora biti isti kao prvi, da biste stvorili zatvoreni oblik.
>
>Točke unutar poligona mora biti naveden u smjeru suprotnom od kazaljke na satu redoslijed. Poligona naveden u smjeru kazaljke na satu redoslijed predstavlja inverznu područja unutar njega.

Osim točke, LineString i poligona GeoJSON određuje prikaz način grupiranja više mjesta geoprostornim, kao i kako se pridružiti proizvoljne svojstva geolokacija-a kao **značajka**. Budući da su ti objekti valjani JSON, mogu sve se pohranjuju i obuhvaćene DocumentDB. No DocumentDB podržava samo automatsko indeksiranje točaka.

### <a name="coordinate-reference-systems"></a>Koordiniranje sustavi reference

Budući da je nepravilnog oblika zemlji, koordinate geoprostornih podataka predstavlja u mnogim koordinata referenca sustavima (CRS), svaka s vlastite okvira vrsta reference i mjerne jedinice. Ako, na primjer, u "nacionalna rešetke od Britanije" je referentni sustav vrlo točne Velika Britanija, ali ne izvan nje. 

Najpopularniji CRS koristi danas je svijetu Geodetic sustava [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS uređajima i mapiranje servisima te Google karte servisa Bing karte API-ji pomoću WGS 84. DocumentDB podržava upita geoprostornih podataka pomoću CRS WGS 84 na samo i indeksiranja. 

## <a name="creating-documents-with-spatial-data"></a>Stvaranje dokumenata s prostorno podacima
Kada stvorite dokumente koji sadrže GeoJSON vrijednosti, oni se automatski indeksirati s prostorno indeksa u općeprihvaćenim indeksiranja pravila zbirke web. Ako radite s DocumentDB SDK na jeziku koji se dinamički upisanog kao što su Python ili Node.js, morate stvoriti valjani GeoJSON.

**Stvaranje dokumenta s geoprostornim podacima u Node.js**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Ako radite s .NET (ili Java) SDK-ovi, možete koristiti novu klase točke i poligona unutar prostora za naziv Microsoft.Azure.Documents.Spatial da biste ugradili podaci o mjestu unutar vaše objekti aplikacije. Ove klase pojednostavniti serijalizacije i deserijalizacija prostorno podataka u GeoJSON.

**Stvaranje dokumenta s geoprostornim podacima u .NET**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Ako nemate zemljopisnu širinu i dužinu informacije, ali ste fizička adresa ili naziv lokacije, kao što su grad i država, možete potražiti stvarni koordinate pomoću geokodiranju servis kao što je Bing karte OSTALE servise. Dodatne informacije o geokodiranju Bing karte [u nastavku](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Slanje upita prostorno vrste

Nakon što smo ste snimili pogledati kako umetnuti geoprostornih podataka, pogledajmo u načinu upita te podatke pomoću DocumentDB pomoću SQL i LINQ.

### <a name="spatial-sql-built-in-functions"></a>Prostorno SQL ugrađene funkcije
DocumentDB podržava sljedeće ugrađene funkcije Otvori geoprostornim Consortium (OGC) za ispitivanje geoprostornim. Dodatne informacije o potpunog skupa ugrađene funkcije SQL jeziku, pogledajte [DocumentDB upita](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Korištenje</strong></td>
  <td><strong>Opis</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Vraća udaljenost između dva izraza točke GeoJSON.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Vraća Booleov izraz koja označava je li točke GeoJSON naveden u prvom argumentu unutar GeoJSON poligona u drugom argumentu.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Vraća Booleova vrijednost koja označava je li navedena GeoJSON točka ili poligona izraz valjana.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Vraća vrijednost JSON koja sadrži na Booleove vrijednosti vrijednost ako navedeni GeoJSON točka ili poligona izraz nije valjan, a koji nisu valjani, k tome razloga kao vrijednost niza.</td>
</tr>
</table>

Prostorno funkcije se mogu koristiti za izvođenje Blizina upite odabiranja prostorno podataka. Na primjer, Evo upit koji vraća sve obitelji dokumente koje se nalaze u 30 km na određeno mjesto pomoću ST_DISTANCE ugrađene funkcije. 

**Upit**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Rezultati**

    [{
      "id": "WakefieldFamily"
    }]

Ako uvrstite prostorno indeksiranje u indeksiranja pravilnika, zatim "udaljenost upite" će posluživanje učinkovito pomoću indeksa. Dodatne informacije o prostorno indeksiranje, pogledajte odjeljak u nastavku. Ako još nemate prostorno indeks za navedeni putovi prostorno upite možete izvršiti i dalje navođenjem `x-ms-documentdb-query-enable-scan` zahtjev zaglavlja s Postavi vrijednost na "true". U .NET, to možete učiniti prosljeđivanjem neobavezni argument **FeedOptions** upita [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) postavite na true. 

Da biste provjerili je li točku leži unutar poligona mogu se ST_WITHIN. Najčešće višekutnike koriste se za predstavljanje granice kao poštanskim brojevima, stanje granica ili prirodnim formations. Ponovno uključite li prostorno indeksiranje u indeksiranja pravilnika, zatim "u" upita će posluživanje učinkovito kroz indeksa. 

Argumenti poligona u ST_WITHIN može sadržavati samo jedan Nazovi, odnosno na višekutnike ne smije sadržavati rupe u njima. 

**Upit**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Rezultati**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Sličan način nepodudaran vrste funkcionira u upitu DocumentDB ako mjesto vrijednost koja je navedena u neki argument nije oblikovan ili nije valjana, a zatim će rezultirati **Nedefinirano** i provjeriti u odnosu dokument da biste se preskočiti u rezultatima upita. Ako je upit ne vraća rezultate, pokrenite ST_ISVALIDDETAILED za ispravljanje pogrešaka Zašto vrsta spatail nije valjana.     

DocumentDB podržava izvođenje inverzni upita, odnosno možete indeksirati višekutnike ili recima u DocumentDB, a zatim upit za područja koja sadrže određene točke. Ovaj uzorak obično se koristi logistika za prepoznavanje npr kada je kamion unosi ili ostavlja određeno područje. 

**Upit**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Rezultati**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID i ST_ISVALIDDETAILED možete koristiti da biste provjerili prostorno objekt vrijedi. Ako, na primjer, sljedeći upit provjere valjanosti točku s za vrijeme odsutnosti raspon zemljopisnu širinu vrijednost (-132.8). ST_ISVALID vraća samo Booleova vrijednost, a ST_ISVALIDDETAILED vraća na Booleove vrijednosti i niz koji sadrži razloga zašto smatra koji nisu valjani.

**Upit**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Rezultati**

    [{
      "$1": false
    }]

Ove funkcije mogu se koristiti i da biste provjerili valjanost višekutnike. Na primjer, ovdje koristimo ST_ISVALIDDETAILED da biste provjerili valjanost poligona koji je zatvoren. 

**Upit**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Rezultati**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ upita u .NET SDK

DocumentDB .NET SDK i davatelje usluga graničnik metode `Distance()` i `Within()` za korištenje unutar LINQ izraza. Davatelj DocumentDB LINQ prevodi te način pozive u ekvivalentne SQL ugrađene funkcije poziva (ST_DISTANCE i ST_WITHIN redom). 

Evo primjera LINQ upit koji pronalazi sve dokumente u zbirci DocumentDB čija se vrijednost "mjesto" je unutar polumjer 30km od određenu pokažite pomoću LINQ.

**LINQ upit za udaljenost**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Isto tako, Evo upita za pronalaženje svi dokumenti čije "mjesto" je u navedeni okvir/poligona. 

**LINQ upit za unutar**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Nakon što smo ste snimili pogledati kako postaviti upit dokumenata pomoću LINQ i SQL, recimo pogledajte kako konfigurirati DocumentDB prostorno indeksiranje.

## <a name="indexing"></a>Indeksiranje

Opisan smo u [Sheme Agnostic indeksiranje s Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papira, ne možemo namijenjene DocumentDB korisnika: Modul baze podataka se uistinu agnostic sheme i prve klase za podržavaju JSON. Modul baze podataka za pisanje Optimizirano od DocumentDB nativno razumije prostorno (točke, višekutnike i crte) predstavljeni podaci u standardu GeoJSON.

Da ne duljimo, predviđena iz geodetic koordinate na 2D ravnini zatim progresivno podijeliti ćelije na temelju **quadtree**geometriju. Te ćelije mapiraju se 1D na temelju lokacije ćelije unutar **Hilbertov prostora ispunjavanja krivulje**, koji čuva kraj točaka. Uz to kada se indeksirati podatke o lokaciji, vodi kroz postupak naziva **teselaciju**, odnosno sve ćelije koje se ne sijeku mjesto prepoznati i pohranjenih kao tipke u indeksu DocumentDB. Upit trenutku argumente kao što su točke i višekutnike su i teseliranu da biste izdvojili ID raspona odgovarajuću ćeliju, a zatim koristiti za dohvaćanje podataka iz indeksa.

Ako navedete indeksiranja pravila koja obuhvaća prostorno indeks za / * (Svi putovi), a zatim sve točke pronaći unutar zbirke indeksiraju učinkovitog prostorno upita (ST_WITHIN i ST_DISTANCE). Prostorno indeksi neće imati vrijednost preciznosti, a uvijek koristi zadanu vrijednost preciznosti.

>[AZURE.NOTE] DocumentDB podržava automatsko indeksiranje točke, višekutnike i LineStrings

Sljedeći isječak JSON prikazuje indeksiranja pravila s prostorno indeksiranja omogućena, odnosno indeksirati bilo kojem mjestu GeoJSON prisutne u dokumente za prostorno upita. Ako mijenjate indeksiranja pravila pomoću portala za Azure, možete odrediti sljedeće JSON za indeksiranje pravila da biste omogućili prostorno indeksiranja na vaše zbirke.

**Zbirka indeksiranje JSON pravila s Spatial omogućen za točke i višekutnike**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Evo isječak koda u .NET koji pokazuje kako stvoriti zbirka s prostorno indeksiranje uključen za sve putove koji sadrži točke. 

**Stvaranje zbirke s prostorno indeksiranja**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

I Evo kako možete izmijeniti postojeću zbirku da iskoristite prednost prostorno indeksiranje iznad bilo koje točke koji su pohranjeni u dokumentima.

**Izmjena postojeću zbirku s prostorno indeksiranja**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Ako je mjesto GeoJSON vrijednost unutar dokumenta oblikovan ili nije valjan, pa ga će dobiti neindeksiranih za prostorno upita. Možete provjeriti mjesto vrijednosti pomoću ST_ISVALID i ST_ISVALIDDETAILED.
>
> Ako definicije zbirke obuhvaća particija ključ, nije prijavljen indeksiranje transformaciju napredak. 

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste learnt upute za početak rada s podrškom za geoprostornim DocumentDB, možete učiniti sljedeće:

- Pokretanje kodiranje [geoprostornim .NET primjere koda na Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
- Uhvatite ruke geoprostornim upita pri [Playground DocumentDB upita](http://www.documentdb.com/sql/demo#geospatial)
- Dodatne informacije o [DocumentDB upita](documentdb-sql-query.md)
- Dodatne informacije o [Pravilima za indeksiranje DocumentDB](documentdb-indexing-policies.md)
