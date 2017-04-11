<properties
    pageTitle="Bilježenje rezultata profili (Azure REST API verzija 2015-02-28-pretpregled pretraživanja) | Microsoft Azure | Pretpregled Azure pretraživanja API-JA"
    description="Azure pretraživanje je servis za pretraživanje glavnom računalu oblak koji podržava usklađivanje rangiranih rezultata koji se temelji na korisnički definirane bilježenja rezultata profila."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Bilježenje rezultata profili (Azure pretraživanje REST API verzija 2015-02-28-pretpregled)

> [AZURE.NOTE] U ovom se članku opisuju bilježenja rezultata profila u [2015-02-28-pretpregled](search-api-2015-02-28-preview.md). Trenutno nema razlike između na `2015-02-28` verziju navedenih na [MSDN-u](http://msdn.microsoft.com/library/azure/mt183328.aspx) i `2015-02-28-Preview` verzije koje se ovdje opisuju, ali nudimo ovaj dokument ipak omogućili dokument opseg preko cijele API-JA.

## <a name="overview"></a>Pregled

Bilježenje rezultata odnosi se na izračunavanje rezultata pretraživanja za svaku stavku u rezultatima pretraživanja. Rezultat je pokazatelj važnosti stavke u kontekstu trenutnog operacija pretraživanja. Što je rezultat viši, više odgovarajućih stavku. U rezultatima pretraživanja, stavke su rang poredane od najviše Nisko, ovisno o rezultatu pretraživanja izračunati za svaku stavku.

Azure pretraživanje koristi zadani bilježenje rezultata za izračunavanje početne rezultata, ali možete prilagoditi izračuna kroz bilježenja rezultata profila. Bilježenja rezultata profili omogućuju veću kontrolu nad rang stavki u rezultatima pretraživanja. Na primjer, možda ćete morati povećati stavke, ovisno o njihovim potencijalni prihod, Promicanje novija stavke ili možda povećajte stavke koje su u zalihama predugo traje.

Bilježenja rezultata profila je dio definicija indeksa koji se sastoji od polja, funkcija i parametre.

Da biste okvirnu koji izgleda bilježenja rezultata profila, u sljedećem primjeru prikazuje jednostavni profil pod nazivom "zemlj.". Ove pojačava stavki koje imaju pojam za pretraživanje u `hotelName` polja. Koriste se i u `distance` funkcija koje se odabrati stavke koje su u deset kilometrima na trenutno mjesto. Ako netko pretražuje na termina 'inn' i 'inn' se događa s biti dio naziva hotelski, dokumente koji sadrže Hoteli s 'inn' prikazat će se noviji u rezultatima pretraživanja.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Da biste koristi ovaj profil bilježenja rezultata upita je formulated da biste odredili profil u nizu upita. U nastavku upita obratite pozornost na to parametra upita `scoringProfile=geo` u zahtjev.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Ovaj upit pretražuje na termina 'inn', a prosljeđuje na trenutnom mjestu. Napomena da ovaj upit sadrži drugi parametri, kao što su `scoringParameter`. Parametri upita opisana su u [Pretraživanje dokumenata (Azure pretraživanje API -JA)](search-api-2015-02-28-preview.md#SearchDocs).

Kliknite [primjer](#example) da biste pregledali detaljnije primjera bilježenja rezultata profila.

## <a name="what-is-default-scoring"></a>Što je bilježenje rezultata zadane?

Bilježenje rezultata formula izračunava rezultata pretraživanja za svaku stavku u skupu rank uređeni rezultata. Sve stavke u skupu rezultata pretraživanja je dodijeljen rezultat pretraživanja, a zatim rangiranih najviša prema najmanjem. Stavke veće rezultati se vraćaju aplikaciji. Po zadanom se vraćaju gornji 50, ali ne možete koristiti u `$top` parametar da biste se vratili na manje ili veće broj stavki (do 1000 u jednom odgovor).

Prema zadanim postavkama, rezultat pretraživanja je izračunati ovisno o Statistička svojstva podaci i upit. Azure pretraživanje pronalazi dokumente koji sadrže pojmova za pretraživanje u nizu upita (neke ili sve, ovisno o `searchMode`), favoring dokumente koji sadrže mnogo instanci pojam za pretraživanje. Rezultat pretraživanja ide čak i noviji pojam se rijetko preko corpus podataka, ali uobičajeni unutar dokumenta. Temelj za taj se način računalno važnosti zove TF IDF ili (termina učestalost Inverzija dokument učestalost).

Pod pretpostavkom da postoji bez prilagođenog sortiranja, zatim rangiranja rezultata prema rezultatu pretraživanja prije nego što se vraćaju se aplikacija. Ako `$top` nije naveden, 50 stavki koje se pojavljuju najveće pretraživanje vraćaju rezultat.

Vrijednosti rezultat pretraživanja možete ponavljati u cijelom skupu rezultata. Na primjer, možda imate 10 stavki s rezultatom 1,2, 20 stavke s rezultatom 1,0 i 20 stavke s rezultatom od 0,5. Kada više pristupa imaju isti rezultat pretraživanja, redoslijed iste stavke testu dobije nije definiran, a nije stabilan. Pokreni u upit i možda će se prikazati stavke položaj shift. Uz dvije stavke uz isti rezultat, nema jamstva koja prikazuje prvi.

## <a name="when-to-use-custom-scoring"></a>Kada koristiti prilagođene bilježenje rezultata

Stvorite jedan ili više profila bilježenja rezultata kada zadano ponašanje rangiranje ne daleko dovoljno u poslovne ciljeve sastanka. Na primjer, možda odlučite važnosti pretraživanje trebali biste odabrati novododani stavke. Isto tako, mogu imati polje koje sadrži Profitna marža ili neko drugo polje koji označava potencijalni prihod. Povećanje pristupa koji daju pogodnosti pridonijeti vašem poslovanju može biti važan faktor u procjeni da biste koristili bilježenja rezultata profila.

Prema važnosti sustavom redoslijed i implementirati putem bilježenje rezultata profila. Preporučujemo da ste koristili u prošlosti kojih možete sortirati cijena, datum, ocjena ili važnosti stranicama s rezultatima pretraživanja. U odjeljku pretraživanje Azure bilježenja rezultata profila pogona mogućnost "važnosti". Definiciju relevantnosti kontrolira vam predicated na poslovne ciljeve i vrstu pretraživanju želite prikazati.

<a name="example"></a>
## <a name="example"></a>Primjer

Kao što je naznačeno, prilagođene bilježenje rezultata je implementirana kroz bilježenje rezultata profili koji su definirani u shemom indeksa.

U ovom je primjeru prikazano sheme indeksa s dva bilježenja rezultata profilima (`boostGenre`, `newAndHighlyRated`). Upit protiv ovaj indeks koji uključuje ili profila kao parametar upita će koristiti profila da bi se rezultat skup rezultata.

[Pokušajte u ovom primjeru](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Tijek rada

Možete implementirati prilagođeni ponašanje bilježenja rezultata, dodajte bilježenja rezultata profil u skladu sa shemom koji definira indeks. Možete imati više profila bilježenja rezultata u indeks, ali jedan profil možete navesti samo u trenutku u bilo kojem upita.

Početak rada s [predloška](#bkmk_template) na navedene u ovoj temi.

Navedite naziv. Profili bilježenja rezultata nije obavezno, ali ako dodate jedan, naziv je obavezan. Slijedite konvencije imenovanja polja (počinje slovom, izbjegava posebnih znakova i rezervirane riječi). Dodatne informacije potražite u članku [Pravila imenovanja](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

U tijelu bilježenja rezultata profila konstruirana ponderiranog polja i funkcije.

### <a name="weights"></a>Težina ###

Na `weights` svojstvo profila bilježenja rezultata navodi parove naziv i vrijednosti koje dodijeliti relativni Debljina polje s vrijednostima. U [primjeru](#example)albumTitle, stupca i artistName polja su boosted 1,5, 5 i 2, odnosno. Zašto se stupca je boosted mnogo veća od ostalih? Ako se pretraživanje obavljaju iznad podataka koji je Pomalo istovrstan (kao što je to slučaj s 'stupca' u na `musicstoreindex`), možda će biti potrebno veće varijanca u odnosu debljine. Na primjer, u `musicstoreindex`, "rock" prikazuje se kao oba stupca i u opise jednak način phrased stupca. Ako želite da se stupca da biste su opis stupca, polje stupca morat ćete mnogo veći relativni debljinu.

### <a name="functions"></a>Funkcija ###

Funkcije koriste kada dodatni Izračuni su potrebni za određeni konteksta. Vrste funkcija valjani su `freshness`, `magnitude`, `distance` i `tag`. Svaku funkciju ima parametara jedinstvenih na njega.

  - `freshness`treba koristiti kada želite da biste uvećali prema kako nove i stare stavke. Ova funkcija može se koristiti samo pomoću polja datuma i vremena (`Edm.DataTimeOffset`). Napomena u `boostingDuration` atribut koristi se samo s funkciju svježina.
  - `magnitude`treba koristiti kada želite da biste uvećali na temelju kako Visoko ili niske nije numerička vrijednost. Scenariji koje mogu pozivati funkcije uključiti povećanje Profitna marža, najveći cijena, najnižu cijenu ili broj preuzimanja. Možete obrnuti raspon visoke Niski, ako želite da se inverzni uzorak (, na primjer, da biste stavke pojačavanje donjem cjenovnom više stavki veći cjenovnom). Dan raspon cijene $ do 100 $1, ćete postaviti `boostingRangeStart` na 100 i `boostingRangeEnd` od 1 da biste uvećali donjem cjenovnom stavke. Ova funkcija se može koristiti samo s poljima dvaput i cijeli broj.
  - `distance`trebali biste koristiti kada želite da biste uvećali Blizina ili geografsku lokaciju. Ova funkcija može se koristiti samo s `Edm.GeographyPoint` polja.
  - `tag`trebali biste koristiti kada želite da biste uvećali po oznake zajednički između dokumenata i upita za pretraživanje. Ova funkcija može se koristiti samo s `Edm.String` i `Collection(Edm.String)` polja.
  
#### <a name="rules-for-using-functions"></a>Pravila za korištenje funkcija ####

  - Funkcija vrsta (svježina, veličina, udaljenost, oznake) mora biti mala slova.
  - Funkcija ne mogu sadržavati vrijednosti null ili prazan. Konkretno, ako uvrstite NazivPolja morate postaviti nešto.
  - Funkcije se mogu primijeniti samo na koje je moguće filtrirati polja. Dodatne informacije o koje je moguće filtrirati polja potražite u članku [Stvaranje indeksa](search-api-2015-02-28-preview.md#CreateIndex) .
  - Funkcije se mogu primijeniti samo na polja koji su definirani u skup polja indeksa.

Kada je definiran indeks, stvorite indeks tako da prenesete sheme indeks, nakon čega slijedi dokumenata. Upute za ove postupke potražite [Create Index](search-api-2015-02-28-preview.md#CreateIndex) i [Dodavanje ili ažuriranje dokumenata](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) . Kada je ugrađena indeks, imat ćete funkcionalni bilježenja rezultata profil koji funkcionira s pretraživanje podataka.

<a name="bkmk_template"></a>
## <a name="template"></a>Predložak
U ovom se odjeljku prikazuje sintaksu i predložak za bilježenje rezultata profila. Pogledajte [indeksa atributa referencu](#bkmk_indexref) u sljedećem odjeljku za opise atribute.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Bilježenje rezultata Referenca svojstva profila

**Napomena** Bilježenja rezultata funkcije se mogu primijeniti samo s poljima koja su koje je moguće filtrirati.

| Svojstvo | Opis |
|----------|-------------|
| `name`   | Obavezan. To je naziv profila bilježenja rezultata. Slijedi isti konvencije imenovanja polja. Ga morate počinju slovima, ne može sadržavati točke, dvotočke i interpunkcija ili @ simbola, a ne može pokrenuti sadrže točan izraz "azureSearch" (velika i mala slova). |
| `text` | Sadrži svojstvo debljine. |
| `weights` | Neobavezno. Vrijednost za naziv par koji određuje naziv polja i relativne debljine. Relativna Debljina moraju biti pozitivni cijeli broj ili broj s pomičnom točkom. Možete navesti naziv polja bez odgovarajućih Debljina. Težina služe za označavanje važnost jednog polja u drugo. |
| `functions` | Neobavezno. Imajte na umu da bilježenja rezultata funkcije mogu primijeniti samo s poljima koja su koje je moguće filtrirati. |
| `type` | Obavezan za bilježenje rezultata funkcije. Navodi vrstu funkcija koristiti. Valjane vrijednosti obuhvaćaju `magnitude`, `freshness`, `distance` i `tag`. U svakom bilježenja rezultata profilu možete uključiti više od jedne funkcije. Naziv funkcije mora biti mala slova. |
| `boost` | Obavezan za bilježenje rezultata funkcije. Pozitivan broj za neobrađenog rezultat koristiti kao množitelj. Ne mogu biti jednak 1. |
| `fieldName` | Obavezan za bilježenje rezultata funkcije. Bilježenja rezultata funkcije se mogu primijeniti samo na polja koja su dio zbirke polja indeksa, a koje su koje je moguće filtrirati. Nadalje, svaka vrsta funkcija predstavlja dodatna ograničenja (svježina se koristi s datumom i vremenom polja, veličina cijeli broj ili dvostruko polja, udaljenost mjesto polja i oznaka s niz ili niz zbirke polja). Možete navesti samo jedno polje po definicija (opis funkcije). Na primjer, da biste vidjeli dvaput iz istog profila, trebali biste obuhvaćaju dva veličina definicije jedan za svako polje. |
| `interpolation` | Obavezan za bilježenje rezultata funkcije. Definira nagib za koji rezultat povećanje povećava od početka raspon na kraj raspona. Valjane vrijednosti obuhvaćaju `linear` (zadano), `constant`, `quadratic`, i `logarithmic`. U odjeljku [Postavljanje interpolations](#bkmk_interpolation) detalje. |
| `magnitude` | Veličina bilježenje rezultata funkcija koristi se za rangiranje utemeljenog na rasponu vrijednosti za numeričko polje. Neke od najčešćih primjera korištenja to su:<ul><li>Ocjenjivanje zvjezdicama: Alter bilježenje rezultata na temelju vrijednosti u polju "Zvijezda ocjena". Kad su dvije stavke odgovarajući, stavke veće ocjene najprije će se prikazati.</li><li>Margina: Kada relevantne dokumente dva prodavača možda želite da biste uvećali dokumente koji imaju više margine prvi put.</li><li>Kliknite broji: aplikacije koje pratite, kliknite kroz akcije proizvodi ili više njih, možete koristiti veličina pojačavanje stavke koje su da biste na najbolji promet.</li><li>Preuzimanje broji: za aplikacije koji omogućuje funkcija veličina vam povećajte evidentiranje preuzimanja stavki koje se najčešće preuzeti.</li></ul> |
| `magnitude:boostingRangeStart` | Postavlja vrijednost početka raspona prema kojima je veličina testu dobije. Ta vrijednost mora biti cijeli broj ili broj s pomičnom točkom. Ocjena od 1 do 4, to će biti 1. Za margine veće od 50%, to bi 50. |
| `magnitude:boostingRangeEnd` | Postavlja Završna vrijednost raspona prema kojima je veličina testu dobije. Ta vrijednost mora biti cijeli broj ili broj s pomičnom točkom. Ocjena od 1 do 4, to će biti 4. |
| `magnitude:constantBoostBeyondRange` | Valjane su vrijednosti true ili false (zadano). Kada postavite na true, cijeli pojačavanje će i dalje da biste primijenili dokumente koje imaju vrijednost za polje cilj koji je veće od gornjem kraj raspona. Ako je false, pojačavanje Ova funkcija se neće primijeniti dokumente koji se pojavljuju vrijednosti za polje cilj koja se nalazi izvan raspona. |
| `freshness` | Svježina bilježenje rezultata funkcija koristi se za izmjenu rangiranja rezultata za stavke na temelju vrijednosti u poljima DateTimeOffset. Na primjer, se stavke s datumom novija možete veća od starijih stavki rangiranih. (Imajte na umu da je moguće rank stavki kao što su događaja u kalendaru pomoću datume u budućnosti tako da se stavke bliže prisutnosti može biti rangiranih veća od stavki Dodatno u budućnosti.) Jedan kraj raspona u trenutnom izdanju servisa će biti popravljeno trenutnog vremena. Drugi je vrijeme u prošlosti na temelju na `boostingDuration`. Da biste uvećali raspon u budućnosti puta koristiti negativni `boostingDuration`. Stopa koja na povećanje mijenja iz najviše i na interpolacijom određuje najmanji raspon primjenjuje se na bilježenja rezultata profila (pogledajte na slici u nastavku). Da biste Zrcalili boosting faktor primjenjuje, odaberite uz faktor pojačavanje manji od 1. |
| `freshness:boostingDuration` | Skupovi razdoblje isteka nakon povećanje koji će se zaustaviti za određeni dokument. Pogledajte [skup boostingDuration] [#bkmk_boostdur] u sljedećem odjeljku sintaksa i primjeri. |
| `distance` | Udaljenost bilježenja rezultata funkcija koristi utječe na rezultat dokumenata ovisno o tome kako zatvoriti ili krajnjem su odnosu zemljopisni reference. Mjesto referenca dobiva kao dio upita u parametru (pomoću na `scoringParameter` upita parametar) kao du lat argument. |
| `distance:referencePointParameter` | Parametar će se proslijediti u upitima da biste koristili kao referencu mjesto. scoringParameter je parametra upita. Opisi parametara upita potražite u članku [Pretraživanje dokumenata](search-api-2015-02-28-preview.md#SearchDocs) . |
| `distance:boostingDistance` | Broj koji označava udaljenost u kilometrima iz reference mjesto na kojem završava boosting raspon. |
| `tag` | Oznaka bilježenje rezultata funkcija koristi se za utječe na rezultat dokumenti na temelju oznaka u dokumentima i upita za pretraživanje. Dokumenti koje ste oznake zajedničko s upit za pretraživanje će biti boosted. Oznake za upit za pretraživanje prikazuje se kao parametar bilježenja rezultata u svaki zahtjev za pretraživanjem (pomoću na `scoringParameter` upita parametar). |
| `tag:tagsParameter` | Parametar će se proslijediti u upitima da biste odredili oznake za određeni zahtjev. `scoringParameter`je parametra upita. Opisi parametara upita potražite u članku [Pretraživanje dokumenata](search-api-2015-02-28-preview.md#SearchDocs) . |
| `functionAggregation` | Neobavezno. Odnosi se samo kada su navedeni funkcije. Valjane vrijednosti obuhvaćaju: `sum` (zadano), `average`, `minimum`, `maximum`, i `firstMatching`. Rezultat pretraživanja je jedna vrijednost koja je izračunat iz većeg broja varijabli, uključujući više funkcija. Ovaj atribute pokazuje kako se boosts sve funkcije posložene jedan zbrajanja pojačavanje koji se primjenjuje rezultatu osnovni dokument. Osnovni rezultat temelji se na vrijednost tf idf izračunati iz dokumenta i upit za pretraživanje. |
| `defaultScoringProfile` | Prilikom izvršavanja zahtjeva za pretraživanje ako nema bilježenja rezultata profila nije naveden, zatim bilježenje rezultata zadani je korištenih (tf-idf samo). Zadani naziv profila za bilježenje rezultata moguće je postaviti ovdje, uzrok Azure pretraživanje tako da koristi taj profil kada nema određenu profila izražen je u zahtjev za pretraživanjem. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Postavljanje interpolations

Interpolations omogućuju da odredite nagib za koji rezultat povećanje povećava od početka raspon na kraj raspona. Možete koristiti sljedeće interpolations:

  - `Linear`: Za stavke koje se nalaze u rasponu max i min, pojačavanje primjenjuje se na stavku će se izvršiti neprestano padajući iznosom. Linearni je zadani interpolacijom bilježenja rezultata profila.
  - `Constant`: Za stavke koje se nalaze u početne i završne raspon, konstante pojačavanje će se primijeniti na rank rezultate.
  - `Quadratic`: U comparison da biste linearni interpolacijom koja ima neprestano padajući pojačavanje Kvadratna prethodno će smanjiti manje tempom i zatim kad je izvršenja Završetak raspona, ga smanjuje puno veći interval. Ta mogućnost interpolacijom nije dopuštena u oznaci bilježenje rezultata funkcije.
  - `Logarithmic`: U comparison da biste linearni interpolacijom koja ima neprestano padajući pojačavanje Logarithmic će prethodno smanjiti veći tempom i zatim kad je izvršenja Završetak raspona, ga smanjuje na mnogo interval za manje. Ta mogućnost interpolacijom nije dopuštena u oznaci bilježenje rezultata funkcije.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>Postavljanje boostingDuration

`boostingDuration`je atribut funkcije svježina. Koristite da biste postavili programa isteka razdoblja nakon povećanje koji će se zaustaviti za određeni dokument. Na primjer, da biste uvećali proizvoda crte ili marke za razdoblje Promotivni 10 dana, želite odrediti razdoblje 10 dana kao "P10D" za te dokumente. Ili da biste uvećali navedite predstojeće događaje u sljedećem tjednu "-P7D".

`boostingDuration`moraju biti oblikovani kao vrijednost "dayTimeDuration" XSD (ograničeni podskup vrijednost trajanje ISO 8601). Obrazac za to je: `[-]P[nD][T[nH][nM][nS]]`.

Sljedeća tablica sadrži nekoliko primjera.

| Trajanje | boostingDuration |
|----------|------------------|
| jedan dan | "P1D" |
| 2 dana i 12 sati | "P2DT12H" |
| 15 minuta | "PT15M" |
| 30 dana, 5 sati, 10 minuta, a 6.334 sekundi | "P30DT5H10M6.334S" |

Dodatne primjere potražite u članku [XML sheme: vrste podataka (W3.org web-mjesta)](http://www.w3.org/TR/xmlschema11-2/).

**Vidi također**
[Azure pretraživanje servisa REST API-JA](http://msdn.microsoft.com/library/azure/dn798935.aspx) na MSDN-u <br/>
[Stvaranje indeksa (Azure pretraživanje API-JA)](http://msdn.microsoft.com/library/azure/dn798941.aspx) na MSDN-u<br/>
[Dodavanje bilježenja rezultata profila u indeks pretraživanja](http://msdn.microsoft.com/library/azure/dn798928.aspx) na MSDN-u<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
