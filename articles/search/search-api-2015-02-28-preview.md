<properties
   pageTitle="Azure servisa REST API verzija 2015-02-28-pretpregled pretraživanja | Microsoft Azure | Pretpregled Azure pretraživanja API-JA"
   description="Azure servisa REST API verzija 2015-02-28-pretpregled pretraživanja sadrži eksperimentalne značajke kao što su Analyzers prirodnim jezikom i moreLikeThis pretraživanja."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure pretraživanje servisa REST API-JA: Verzija 2015-02-28-pretpregled

Ovaj se članak odnosi na referencu dokumentaciji `api-version=2015-02-28-Preview`. Taj pretpregled proširuje trenutnu verziju načelu dostupan [api-verzija = 2015 02 28](https://msdn.microsoft.com/library/dn798935.aspx), unosom eksperimentalne sljedeće značajke:

- `moreLikeThis`upit parametra u [Dokumentima traži](#SearchDocs) API-JA. Pronalazi drugi dokumenti koji se odnose na drugi određeni dokument.

Nekoliko dodatnih dijelove na `2015-02-28-Preview` REST API-JA su navedenih zasebno. To obuhvaća:

- [Bilježenje rezultata profila](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexers](search-api-indexers-2015-02-28-preview.md)

Servis za pretraživanje Azure dostupan je u više verzija. Pogledajte [Verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje.

## <a name="apis-in-this-document"></a>API-ji u ovom dokumentu

API servisa Azure pretraživanja podržava dva syntaxes URL-a za API operacije: jednostavne i OData (pročitajte članak [podrška za OData (Azure pretraživanje API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) detalje). Na sljedećem popisu navedene jednostavne sintakse.

[Stvaranje indeksa](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Ažuriranje kazala](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Početak indeksa](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Unos indeksa](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Početak Statistika indeksa](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Testiranje alata za analizu](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Brisanje indeksa](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Dodavanje, brisanje i ažuriranje podataka u indeks](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Pretraživanje dokumenata](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Pretraživanje dokumenta](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Broj dokumenata](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Prijedlozi](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Operacije indeksa

Možete stvoriti i upravljati indeksa u servis za pretraživanje Azure putem jednostavne HTTP zahtjeva (objavu, a zatim DOHVATI, STAVI, brisanje) na temelju resursa za dani indeks. Da biste stvorili indeks, najprije OBJAVILI JSON dokument koji opisuje shemi indeksa. Shema definira polja indeks, njihove vrste podataka i način korištenja (na primjer, u pretraživanja cijelog teksta, filtri, sortiranja i faceting). Također definira bilježenja rezultata profila, suggesters i druge atribute da biste konfigurirali ponašanje indeksa.

U sljedećem primjeru sadrži ilustracija shema koji se koriste za pretraživanje na hotelski podatke pomoću definiranih na jezicima koji se dva polja Opis. Obratite pozornost na to kako atribute upravljanje načinom korištenja polja. Na primjer u `hotelId` se koristi kao tipku dokumenta (`"key": true`) te je izuzeti iz pretraživanja cijelog teksta (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Nakon stvaranja indeksa ćete prenose dokumente koji popunjavanje indeksa. Potražite u članku [Dodavanje ili ažuriranje dokumenata](#AddOrUpdateDocuments) za ovu sljedeći korak.

Videouvod indeksiranje pretraživanja Azure, potražite u članku [kanala 9 oblaka pokrivaju epizode na Azure pretraživanja](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Stvaranje indeksa

Indeks je primarni srednje vrijednosti organiziranje i pretraživanje dokumenata u Azure pretraživanja, slično kako tablice organizira zapisa u bazi podataka. Svaki indeks je skup dokumenata da sve u skladu sa shemom indeksa (nazive polja, vrste podataka i svojstva), ali indekse i navesti dodatne konstrukta (suggesters, bilježenja rezultata profila i mogućnosti CORS) koje definiraju druge ponašanja pretraživanja.

Možete stvoriti novi indeks unutar servisa za pretraživanje Azure koji se pomoću zahtjev HTTP POST ili STAVI. U tijelu zahtjeva za je JSON sheme koji određuje informacije o indeks i konfiguracije.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Osim toga, možete koristiti STAVI i navedite naziv indeksa na URI. Ako ne postoji indeks, će biti stvorena.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Stvaranje indeksa određuje strukturu dokumente pohranjene i koristiti u operacije pretraživanja. Popunjavanje indeks je zasebnom operacija. Ovaj korak, poslužite se [Indeksiranje](https://msdn.microsoft.com/library/azure/mt183328.aspx) (dostupno za podržani izvori podataka) ili operaciju [Dodavanje, ažuriranje ili brisanje dokumenata](https://msdn.microsoft.com/library/azure/dn798930.aspx) . Obrnuti indeks je koji su generirani prilikom knjiže dokumente.

**Napomena**: maksimalan broj indeksi dopušteno razlikuje cijene sloju. Besplatni servis omogućuje do 3 indeksi. Standardnog servisa omogućuje 50 indeksi po servisa za pretraživanje. Potražite u članku [ograničenja i ograničenjima](http://msdn.microsoft.com/library/azure/dn798934.aspx) detalje.

**Zahtjev**

HTTPS potreban je za sve zahtjeve za uslugu. Zahtjev za **Stvaranje indeksa** možete konstruirana metodom objave ili STAVI. Prilikom korištenja objavu, morate navesti naziv indeksa u tijelu zahtjeva uz definicija sheme indeksa. STAVI, naziv indeksa je dio URL-a. Ako ne postoji indeks, stvaranja. Ako već postoji, ažuriran je u novu definiciju.

Naziv indeksa mora biti mala slova, započnite slovo ili broj, bez kose crte ili točke, a biti manje od 128 znakova. Nakon naziv indeksa počevši slovo ili broj, ostatak naziv možete uključiti slovo, broj i crtice, pod uvjetom da crtice nisu uzastopni.

Na `api-version` potreban je. Popis dostupnih verzija potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `Content-Type`: Potreban. To postavljena na`application/json`
- `api-key`: Potreban. Na `api-key` se koristi za
- autentičnost zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Zahtjev za **Stvaranje indeksa** mora sadržavati do `api-key` zaglavlje postavljeno na ključ administrator (umjesto ključa upita).

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

<a name="RequestData"></a>
**Sintaksa tijelu zahtjeva**

U tijelu zahtjeva za sadrži definicija sheme koje sadrži popis polja podataka u dokumente koji će se umeće ovaj indeks, vrste podataka, atribute, kao i neobavezan popis bilježenje rezultata profila koji se koriste za rezultatu dokumente koji se podudaraju u trenutku upita.

Imajte na umu da se za objavu zahtjev, morate navesti naziv indeksa u tijelu zahtjeva.

Može postojati samo jedno polje ključa u indeks. Ona mora biti polje niza. Ovo polje predstavlja jedinstveni identifikator za svaki dokument pohranjen u indeks.

Glavna dijela indeksa obuhvaćaju sljedeće:

- `name`
- `fields`koje će se umeće ovaj indeks, uključujući naziv, vrstu podataka i svojstva koja određuju dozvoljenu akcije na tom polju.
- `suggesters`koristi se za automatsko dovršavanje ili dovršena prije vrsta upita.
- `scoringProfiles`koristi se za rezultat prilagođenog pretraživanja rangiranje. Detalje potražite u članku [Dodavanje bilježenja rezultata profilima](https://msdn.microsoft.com/library/azure/dn798928.aspx) .
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` koristiti da biste odredili način na koji su dokumenti/upiti podijeljene u vrstu/pretraživanja tokena. Detalje potražite u članku [Analiza u pretraživanju Azure](https://aka.ms//azsanalysis) .
- `defaultScoringProfile`koristi da biste prebrisali zadana ponašanja za bilježenje rezultata.
- `corsOptions`Da biste omogućili unakrsno polazište upite odabiranja indeks.

Vidjet ćete da sintaksa za strukturiranje opseg zahtjev je na sljedeći način. Zahtjev za uzorak navedeni su dodatno na u ovoj temi.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Atributi indeksa**

Sljedećim atributima moguće je postaviti pri stvaranju indeksa. Detalje o profilima bilježenja rezultata i bilježenja rezultata potražite u članku [Dodavanje bilježenja rezultata profila](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Postavlja naziv polja.

`type`-Postavlja vrstu podataka za polje. Popis podržanih vrsta potražite u članku [Podržane vrste podataka](#DataTypes) .

`searchable`-Označava polje kao cijelog teksta može pretraživanja. To znači da je će undergo analizu kao što su word važne tijekom indeksiranja. Ako postavite na `searchable` polja na vrijednost kao što su "lijepo dan", interno ga će se podijeliti pojedinačne tokeni "lijepo" i "dan". Time se omogućuje pretraživanja cijelog teksta te uvjete. Polja vrste `Edm.String` ili `Collection(Edm.String)` su `searchable` prema zadanim postavkama. Druge vrste polja ne može biti `searchable`.

  - **Napomena**: `searchable` polja zauzeti dodatni prostor indeksa jer pretraživanje Azure će sadržavati dodatnu tokenized verziju vrijednost polja pretraživanja cijelog teksta. Ako želite da biste uštedjeli prostor indeksa, a ne morate polja budu obuhvaćeni pretraživanja, postavite `searchable` da biste `false`.

`filterable`-Omogućuje polja na koji postavljate referencu u `$filter` upita. `filterable`razlikuje se od `searchable` u kako se upravlja nizova. Polja vrste `Edm.String` ili `Collection(Edm.String)` koje su `filterable` undergo prepoznavanje riječi, pa su usporedbe za samo točno odgovara. Na primjer, ako ste postavili takvo polje `f` "lijepo dan", `$filter=f eq 'sunny'` tražit će nema rezultata, ali `$filter=f eq 'sunny day'` će. Sva polja su `filterable` prema zadanim postavkama.

`sortable`– Po zadanom sustav sortira rezultata prema rezultatu, ali više iskustva korisnicima će će da biste sortirali po polja u dokumentima. Polja vrste `Collection(Edm.String)` ne može biti `sortable`. Sva druga polja su `sortable` prema zadanim postavkama.

`facetable`– Obično se koristi u prezentaciji rezultati koji uključuje uspješnosti count po kategoriji (na primjer, traženje fotoaparata i pristupa potražite u članku po marke, tako da megapixels, cijena, itd.). Ta mogućnost nije moguće koristiti uz polja vrste `Edm.GeographyPoint`. Sva druga polja su `facetable` prema zadanim postavkama.

  - **Napomena**: polja vrste `Edm.String` koje su `filterable`, `sortable`, ili `facetable` može biti najviše 32 KB duljina. To je zato takvih polja tretiraju se kao jedan pojam i maksimalna duljina termina u pretraživanju Azure je 32KB. Ako vam je potrebna spremanje više teksta nego to u polje s jednog niza, morat ćete eksplicitno postavljene `filterable`, `sortable`, a `facetable` da biste `false` u definicije indeksa.

  - **Napomena**: Ako polje sadrži nijedan od gore atributa postavljena na `true` (`searchable`, `filterable`, `sortable`, ili`facetable`) polje učinkovito izuzeti iz indeksa obrnuti. Ta je mogućnost korisna za polja koja se koriste u upitima, ali su vam potrebne u rezultatima pretraživanja. Isključivanje takvih polja iz indeksa poboljšavaju performanse.

`key`-Označava polje kao da sadrže jedinstvene identifikatore dokumenata u indeks. Točno jedno polje morate odabrati kao u `key` polja i vrsta mora biti `Edm.String`. Polja ključa koja se može koristiti za traženje dokumenata izravno putem [Pretraživanja API -JA](#LookupAPI).

`retrievable`-Određuje hoće li se polja mogu vratiti u rezultatu pretraživanja.  To je korisno kada želite koristiti polja (na primjer, margine) kao filtar, sortiranja i bilježenje rezultata mehanizam, ali ne želite da se polje da bi biti vidljivi krajnjeg korisnika. Taj atribut mora biti `true` za `key` polja.

`analyzer`-Postavlja naziv alata za analizu da biste koristili polje na vrijeme pretraživanja i indeksiranja vrijeme. Dopuštene skup vrijednosti potražite u članku [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Ta mogućnost može se koristiti samo s `searchable` polja te ga nije moguće postaviti zajedno s ili `searchAnalyzer` ili `indexAnalyzer`.  Kada je odabran alata za analizu, nije moguće promijeniti za to polje.

`searchAnalyzer`-Postavlja naziv alata za analizu koristi se u trenutku pretraživanja za to polje. Dopuštene skup vrijednosti potražite u članku [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Ta mogućnost može se koristiti samo s `searchable` polja. Mora biti postavljeno together s `indexAnalyzer` i ne može se postaviti together s na `analyzer` mogućnost. Ovaj alat za analizu može se ažurirati na postojeće polje.

`indexAnalyzer`-Postavlja naziv alata za analizu koristi indeksiranja vrijeme za to polje. Dopuštene skup vrijednosti potražite u članku [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Ta mogućnost može se koristiti samo s `searchable` polja. Mora biti postavljeno together s `searchAnalyzer` i ne može se postaviti together s na `analyzer` mogućnost. Kada je odabran alata za analizu, nije moguće promijeniti za to polje.

`suggesters`-Postavlja način pretraživanja i polja koja su izvora sadržaja za prijedloge. Detalje potražite u članku [Suggesters](#Suggesters) .

`scoringProfiles`-Definira prilagođene bilježenja rezultata ponašanja koje omogućuju utjecati stavke koje se pojavljuju u noviji u rezultatima pretraživanja. Bilježenja rezultata profili se sastoje od polja težina i funkcije. Potražite u članku [Dodavanje bilježenje rezultata profili](https://msdn.microsoft.com/library/azure/dn798928.aspx) dodatne informacije o atributi koji se koriste u profilu bilježenja rezultata.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Podrška za jezike**

Polja pretraživanja undergo analizu te Većina često obuhvaća prepoznavanje riječi, normalizacija tekst i filtrira uvjeta. Prema zadanim postavkama pretraživanja polja u pretraživanju Azure se analizirati i s [Apache Lucene standardne alata za analizu](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) koji se prelama tekst u elemente pratiti pravila["Unicode tekst segmente"](http://unicode.org/reports/tr29/) . Uz to, standardne alata za analizu svi znakovi obrascu malim slovima. Indeksirani dokumenti i pojmova za pretraživanje proći kroz analizu tijekom indeksiranja i obradu upita.

Azure pretraživanja podržava na različitim jezicima. Svaki jezik zahtijeva nestandardne teksta za analizu koji se moraju imati račun karakteristike danom jeziku. Azure pretraživanje nudi dvije vrste analyzers:

- 35 analyzers sigurnosno po Lucene.
- 50 analyzers sigurnosno po vlasničkih Microsoft prirodnim jezikom obrade tehnologija koja se koristi u sustavima Office i Bing.

Neke razvojnim inženjerima možda radije poznatiji, jednostavno, Otvori izvor rješenje Lucene. Lucene analyzers su brže, ali Microsoft analyzers ste naprednih mogućnosti, kao što su Lematizacija, word decompounding (u jezike poput njemački, danski, nizozemski, švedski, norveškom, estonski, završi, mađarskom, slovačkom) i entitet prepoznavanje (URL-ovi, poruke e-pošte, datumi, brojevi). Ako je to moguće, pokrećite usporedbe Microsoft i Lucene analyzers da biste odlučili koji je bolje odgovara.

***Po čemu se razlikuje od***

Alat za analizu Lucene za engleski proširuje standardne alata za analizu. Ga uklanja Posvojni (završne korisnika) iz riječi primjenjuje korijen riječi u skladu sa [sustava korijen riječi algoritam](http://tartarus.org/~martin/PorterStemmer/), a uklanja engleski [Zaustavi riječi](http://en.wikipedia.org/wiki/Stop_words).

U usporedbi, alatu Microsoftov analizator izvodi Lematizacija umjesto korijen riječi. To znači da je možete učiniti oblike inflected i nepravilne riječi mnogo bolje koje rezultira relevantnije rezultate pretraživanja (pogledajte modul 7 [Azure pretraživanja MVA](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) prezentacije više pojedinosti).

Indeksiranje s Microsoft analyzers je prosjek dva do tri puta manja od njihove Ekvivalenti Lucene, ovisno o jeziku. Performanse pretraživanja trebali biste znatno neizmijenjen za upite Prosječna veličina.

***Konfiguracija***

Za svako polje u definiciji indeks možete postaviti na `analyzer` svojstvo naziva programa alata za analizu koji određuje koje jezika i dobavljača. Isti alata za analizu primijenit će se prilikom indeksiranja i pretraživanja za to polje.
Na primjer, imate zasebnim poljima za engleski, francuski i španjolski opise hotelski koji postoji jedno po drugo u istoj indeks. Koristite [parametar 'searchFields' upita](#SearchQueryParameters) da biste naveli koje polje jezično specifične za pretraživanje na temelju u upitima programa. Možete pregledati Primjeri upit koji sadrže na `analyzer` svojstvo u [Pretraživanje dokumenata](#SearchDocs). 

***Popis alata za analizu***

U nastavku je popis podržanim jezicima zajedno s Lucene i Microsoft nazive alata za analizu.

<table style="font-size:12">
    <tr>
        <th>Jezik</th>
        <th>Microsoft naziv alata za analizu</th>
        <th>Naziv Lucene alata za analizu</th>
    </tr>
    <tr>
        <td>arapski</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>armenski</td>
        <td></td>
        <td>hy.lucene</td>
    </tr>
    <tr>
        <td>Bengalski</td>
        <td>bn.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>baskijski</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>bugarski</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>katalonski</td>
        <td>ca.Microsoft</td>
        <td>ca.lucene</td>          
    </tr>
    <tr>
        <td>pojednostavljeni kineski</td>
        <td>ZH Hans.microsoft</td>
        <td>ZH Hans.lucene</td>     
    </tr>
    <tr>
        <td>tradicionalni kineski</td>
        <td>ZH Hant.microsoft</td>
        <td>ZH Hant.lucene</td>     
    <tr>
    <tr>
        <td>hrvatski</td>
        <td>hr.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>češki</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>      
    </tr>    
    <tr>
        <td>danski</td>
        <td>Da.Microsoft</td>
        <td>Da.lucene</td>      
    </tr>    
    <tr>
        <td>nizozemski</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>  
    </tr>    
    <tr>
        <td>engleski</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>      
    </tr>
    <tr>
        <td>estonski</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>finski</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>      
    </tr>    
    <tr>
        <td>francuski</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>galicijski</td>
        <td></td>
        <td>Gl.lucene</td>      
    </tr>
    <tr>
        <td>njemački</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>grčki</td>
        <td>El.Microsoft</td>
        <td>El.lucene</td>      
    </tr>
    <tr>
        <td>gudžaratski</td>
        <td>gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>hebrejski</td>
        <td>he.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>hindi</td>
        <td>hi.Microsoft</td>
        <td>hi.lucene</td>      
    </tr>
    <tr>
        <td>mađarski</td>      
        <td>hu.Microsoft</td>
        <td>hu.lucene</td>
    </tr>
    <tr>
        <td>islandski</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonezijski (Bahaski)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>irski</td>
        <td></td>
        <td>ga.lucene</td>
    </tr>
    <tr>
        <td>talijanski</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>japanski</td>
        <td>ja.Microsoft</td>
        <td>ja.lucene</td>
        
    </tr>
    <tr>
        <td>kannada</td>
        <td>ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>korejski</td>
        <td>ko.Microsoft</td>
        <td>ko.lucene</td>
    </tr>
    <tr>
        <td>latvijski</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>litvanski</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>malajamski</td>
        <td>ML.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajski (latinica)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>maratski</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>norveški</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>perzijski</td>
        <td></td>
        <td>fa.lucene</td>      
    </tr>
    <tr>
        <td>poljski</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>      
    </tr>
    <tr>
        <td>Portugalski (Brazil)</td>
        <td>pt Br.microsoft</td>
        <td>pt Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugalski (Portugal)</td>
        <td>pt Pt.microsoft</td>        
        <td>pt Pt.lucene</td>
    </tr>
    <tr>
        <td>pandžabski</td>
        <td>pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>rumunjski</td>
        <td>RO.Microsoft</td>
        <td>RO.lucene</td>
    </tr>
    <tr>
        <td>ruski</td>
        <td>ru.Microsoft</td>
        <td>ru.lucene</td>  
    </tr>
    <tr>
        <td>srpski (ćirilica)</td>
        <td>Sr cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Srpski (latinica)</td>
        <td>Sr latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>slovački</td>
        <td>Sk.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>slovenski</td>
        <td>sl.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>španjolski</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>švedski</td>
        <td>Sv.Microsoft</td>
        <td>Sv.lucene</td>
    </tr>

    <tr>
        <td>tamilski</td>
        <td>Ta.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>tajlandski</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>turski</td>
        <td>tr.Microsoft</td>
        <td>tr.lucene</td>      
    </tr>
    <tr>
        <td>ukrajinski</td>
        <td>Uk.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>vijetnamski</td>
        <td>vi.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Uz to Azure pretraživanje nudi konfiguracije jezik agnostic alata za analizu</td>
    <tr>
        <td>Standardni ASCII savijanje</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode tekst segmente (standardni izrađivača tokena)</li>
            <li>ASCII preklapanje filtar - pretvara Unicode znakova koji ne pripadaju skup najprije 127 ASCII znakova na ekvivalentima njihove ASCII. To je korisno za uklanjanje dijakritičkih znakova.</li>
        </ul>
        </td>
    </tr>
</table>

Sve analyzers s nazivima dodavati napomene s <i>lucene</i> se pokreće [analyzers Apache Lucene jezik](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Dodatne informacije o ASCII preklapanje filtra moguće je pronaći [u nastavku](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` određuje koja polja u indeks koriste se za podršku samodovršetka u pretraživanje. Obično nizovi djelomično pretraživanje šalju [API prijedloge](#Suggestions) dok korisnik upisuje upit za pretraživanje, a na API vraća skup predložene izraze. Suggester koje sami definirate u indeksu određuje polja koja se koriste za stvaranje vrsta dovršena prije pojmovi. Konfiguriranje detalje potražite u članku [Suggesters](#Suggesters) .

**Bilježenje rezultata profila**

A `scoringProfile` definira prilagođene bilježenja rezultata ponašanja koje omogućuju utjecati stavke koje se pojavljuju u noviji u rezultatima pretraživanja. Bilježenja rezultata profili se sastoje od polja težina i funkcije. Da biste ih koristili, navedite profil prema nazivu u nizu upita.

Zadani bilježenje rezultata profila radi u pozadini za izračunavanje rezultata pretraživanja za sve stavke u skupu rezultata. Možete koristiti interne, bez naziva bilježenja rezultata profila. Umjesto toga možete postaviti `defaultScoringProfile` da biste koristili prilagođeni profil kao zadani, pozvati kad god korisnički profil nije naveden u nizu upita.

Detalje potražite u članku [Dodavanje bilježenja rezultata profilima u indeks pretraživanja (Azure pretraživanje servisa REST API -JA)](search-api-scoring-profiles-2015-02-28-preview.md) .

**Mogućnosti CORS**

Klijentsko Javascript ne možete pozvati sve API-ji po zadanom jer web-pregledniku onemogućuje sve zahtjeve za unakrsno polazište. Omogućivanje CORS (zajedničko korištenje više polazište resursa) tako da postavite na `corsOptions` atribut dopustili unakrsno polazište upite za indeks. Napomena taj upit za samo API-ji podršku CORS sigurnosnih vam razloga. Za CORS moguće je postaviti sljedeće mogućnosti:

- `allowedOrigins`(obavezno): Ovo je popis drugačijeg izvora koji ćete dobiti pristup indeks. To znači da se sve Javascript kod poslužena iz tih drugačijeg izvora će moći upit indeksa (pod pretpostavkom daje ispravan ključ za API-JA). Svaki potječe obično obrasca `protocol://fully-qualified-domain-name:port` iako često izostavi priključak. [U ovom se članku](http://go.microsoft.com/fwlink/?LinkId=330822) dodatne informacije potražite u članku.
 - Ako želite dopustiti pristup svim drugačijeg izvora, uključite `*` kao jednom stavkom u na `allowedOrigins` polja. Imajte na umu da **to ne preporučuje se postupci za usluge pretraživanja radnog.** Međutim, može biti korisno za razvoj ili potrebe za ispravljanje pogrešaka.
- `maxAgeInSeconds`(neobavezno): preglednici pomoću te vrijednosti da biste odredili trajanje (u sekundama) predmemorije CORS prije isporuke odgovore. To mora biti cijeli broj koji nije negativan. Veća je ta vrijednost, bit će bolje performanse, ali na dulje će trebati za CORS pravila promjene stupile na snagu. Ako ne postavite, koristit će se zadano trajanje od pet minuta.

<a name="CreateUpdateIndexExample"></a>
**Primjer tijelu zahtjeva**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Odgovor**

Za uspješan zahtjev: "201 stvoreno".

Prema zadanim postavkama tijelo odgovor će sadržavati JSON definicija indeks koji je stvoren. Ako u `Prefer` zahtjev zaglavlje postavljeno na `return=minimal`, tijelo odgovor bit će prazan i bit će Šifra stanja uspjeh "204 nema sadržaja" umjesto "201 stvoreno". To vrijedi bez obzira na to jesu li STAVI ili objavu je korišten za stvaranje indeksa.

**Napomene**

Trenutno postoji ograničena podrška za indeks sheme ažuriranja. Ažuriranja sheme koje je potrebno ponovno indeksiranje kao što su Promjena vrste polja trenutno nisu podržani. Iako postojećih polja ne možete promijeniti ili izbrisati, nova polja možete dodati u postojeći indeks u bilo kojem trenutku. Prilikom dodavanja novog polja, sve postojeće dokumente u indeks će automatski imati null vrijednost za to polje. Nema dodatni prostor za pohranu će se utrošiti dok se novi dokumenti dodaju se u indeks.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Značajka prijedloge u pretraživanju Azure je mogućnost vrsta dovršena prije ili samodovršetka upit, pruža popis potencijalnih pojmova za pretraživanje u odgovoru djelomične niz unosa unesen u okvir za pretraživanje. Vjerojatno ste primijetili prijedloga za upite prilikom korištenja komercijalne web-preglednici: ".NET" u upišete Bing daje popis uvjete za ".NET 4.5", ".NET Framework 3.5", pa naprijed. Prilikom korištenja servisa za pretraživanje REST API-JA, implementacijom prijedloge u prilagođene aplikacije za pretraživanje Azure potrebno je sljedeće:

- Omogućivanje prijedloge dodavanjem **suggester** izgradnju u indeksu, Davanje naziva, način pretraživanja i popis polja za koju vrstu dovršena prije se poziva. Na primjer, ako odredite "cityName" kao izvorno polje pisati djelomičnog pretraživanja niz "Sea" rezultirat će "Seattle", "Seaside" i "Seatac" (sve su tri nazive gradova stvarni) koje se nude kao prijedloga za upite korisniku.

- Pozivanje prijedloge tako da nazovete [API prijedloge](#Suggestions) u kodu aplikacije. Obično nizovi djelomično pretraživanje šalju se sa servisom dok korisnik upisuje upit za pretraživanje, a taj API vraća skup predložene izraze.

U ovom se članku objašnjava kako konfigurirati **suggester**. Trebali biste provjeriti i [Prijedloge API](#Suggestions) pojedinosti o načina na koji se koristi u suggester.

**Korištenje**

`Suggesters`stvaraju se u indeks i raditi najbolje kad se koristi za prijedlog određene dokumente umjesto su uvjeti ili izraza. Najbolji kandidata polja su naslovi, imena i druge relativno kratke izraze koja mogu identificirati stavke. Manje učinkovita su koji se ponavljaju polja, kao što su kategorije i oznake ili vrlo dugo kao što su polja opise ili komentare.

Kao dio definicija indeksa, možete dodati jedan suggester da biste na `suggesters` zbirke. Svojstva koja određuju na suggester obuhvaćaju sljedeće:

- `name`: Naziv u suggester. Korištenje naziva u suggester prilikom pozivanja na `suggest` API-JA.
- `searchMode`: Strategija da biste potražili izraze kandidata. Način samo trenutno podržava je `analyzingInfixMatching`, koji se izvodi fleksibilne podudaranje izraza na početku ili u sredini rečenica.
- `sourceFields`: Popis jedan ili više polja koja su izvora sadržaja za prijedloge. Samo polja vrste `Edm.String` i `Collection(Edm.String)` možda izvori za prijedloge. Može se koristiti samo polja koja ne morate postaviti prilagođeni jezik za analizu.

**Primjer suggester**

U suggester je dio indeksa. Samo jedan suggester mogu nalaziti u na `suggesters` zbirke u trenutnoj verziji, uz zbirke polja i `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Ako ste koristili verzije javno pretpregleda pretraživanja Azure `suggesters` zamjenjuje stariji logičko svojstvo (`"suggestions": false`) koji podržano samo prefiksa prijedloge kratki nizova (3 25 znakova). Njegov zamjena `suggesters`, podržava infix koje pronađe na početku ili u sredini sadržaj polja uvjete koji se podudaraju s bolje odstupanje pogrešaka u nizove za traženje podudarnih. Počevši od načelu dostupan izdanje, to je sada samo implementaciju prijedloga API-JA. U starijim `suggestions` svojstvo koje je uvedena u `api-version=2014-07-31-Preview` funkcioniraju u toj verziji, ali nije u radu u `2015-02-28` ili novije verzije Azure pretraživanja.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Ažuriranje kazala

Možete ažurirati postojeći indeks unutar Azure pretraživanje pomoću zahtjev HTTP STAVITI. Ažuriranja obuhvaćaju dodavanje novih polja postojeću shemu, izmjena mogućnosti CORS i izmjena bilježenja rezultata profila. Dodatne informacije potražite [Dodavanje bilježenje rezultata profila](https://msdn.microsoft.com/library/azure/dn798928.aspx) . Navedite naziv funkcije index da biste ažurirali na zahtjev za URI:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Važne:** Podrška za indeks sheme ažuriranja ograničeno je na operacije koje zahtijevaju novog indeks pretraživanja. Ažuriranja sheme koje je potrebno ponovno indeksiranje, kao što su Promjena vrste polja trenutno nisu podržani. Nova polja možete dodati u bilo kojem trenutku iako postojećih polja ne možete promijeniti ili izbrisati. Isto vrijedi za `suggesters`. Nova polja mogu biti dodane u suggester u trenutku polja dodaju se, ali nije moguće ukloniti polja iz `suggesters` i ne mogu dodati postojećih polja `suggesters`.

Prilikom dodavanja novog polja u indeks, sve postojeće dokumente u indeks će automatski imati null vrijednost za to polje. Nema dodatni prostor za pohranu će se utrošiti dok se novi dokumenti dodaju se u indeks.

**Zahtjev**

HTTPS potreban je za sve zahtjeve za uslugu. Zahtjev za **Ažuriranje indeksa** konstruirana pomoću HTTP STAVITI. STAVI, naziv indeksa je dio URL-a. Ako ne postoji indeks, stvaranja. Ako već postoji indeks, ažuriran je u novu definiciju.

Naziv indeksa mora biti mala slova, započnite slovo ili broj, bez kose crte ili točke, a biti manje od 128 znakova. Nakon naziv indeksa počevši slovo ili broj, ostatak naziv možete uključiti slovo, broj i crtice, pod uvjetom da crtice nisu uzastopni.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `Content-Type`: Potreban. To postavljena na`application/json`
- `api-key`: Potreban. Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Zahtjev za **Ažuriranje indeksa** mora sadržavati do `api-key` zaglavlje postavljeno na ključ administrator (umjesto ključa upita).

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Sintaksa tijelu zahtjeva**

Kada se ažurira postojeći indeks, tijelo mora sadržavati izvorne definicije sheme plus nova polja koje dodajete, kao i izmijenjenu bilježenja rezultata profile, suggesters CORS mogućnosti i, ako postoje. Ako ne mijenjate bilježenja rezultata profili i mogućnosti CORS, morate uključiti izvornike iz stvaranja indeksa. Općenito govoreći najbolje uzorka za ažuriranja za dohvaćanje definicija indeksa s VIDJELI, izmijenite ga, a zatim je ažurirati STAVI.

Sintaksa sheme koji se koriste za stvaranje indeksa reproducirati ovdje pogodnost. Dodatne informacije potražite u članku [Stvaranje indeksa](#CreateIndex) .

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Odgovor**

Za uspješan zahtjev: "204 nema sadržaja".

Prema zadanim postavkama tijelo odgovor bit će prazan. Međutim, ako u `Prefer` zahtjev zaglavlje postavljeno na `return=representation`, tijelo odgovor će sadržavati JSON definicija indeks koji je ažuriran. U ovom slučaju Šifra stanja uspjeh bit će "200 u redu".

**Ažuriranje definicija indeksa s prilagođene analyzers**

Kada je definiran je alat za analizu, na izrađivača tokena, tokena filtra ili filtra znak, nije moguće mijenjati. Postojeći indeks moguće dodati nove samo ako se `allowIndexDowntime` zastavice postavljen na true u zahtjevu za ažuriranje indeksa: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Imajte na umu da ovaj postupak staviti indeks izvan mreže za barem nekoliko sekundi, uzrok vaše indeksiranja i zahtjevi upita nije uspjela. Dostupnost performanse i unos indeksa može biti oštećena vida za nekoliko minuta nakon ažuriranje indeksa ili više vrlo velike indeksi.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Indeksi popisa

Operacija **Indeksi popis** trenutno vraća popis indeksi na servisu Azure pretraživanja.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Zahtjev**

HTTPS potreban je za sve zahtjeve za uslugu. Zahtjev za **Indeksi popisa** možete konstruirana metodom DOHVATI.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `api-key`: Potreban. Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Zahtjev za **Popis indeksi** mora sadržavati programa `api-key` postavljena na ključa administrator (umjesto ključa upita).

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Ništa.

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

Ovo su u tijelu odgovor primjer:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Imajte na umu da možete filtrirati i odgovor na svojstva koja vas zanima. Na primjer, ako želite da se samo popis imena indeks, koristiti OData `$select` upita mogućnost:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

U ovom slučaju odgovor gornjem primjeru pojavit će se na sljedeći način:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Ovo je korisno postupak da biste spremili propusnosti ako imate mnogo indeksa u servis za pretraživanje.

<a name="GetIndex"></a>
## <a name="get-index"></a>Početak indeksa

Postupak **Dohvati indeksa** može vidjeti definicija indeksa iz pretraživanja Azure.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Zahtjev**

HTTPS potreban je za zahtjeve za uslugu. Zahtjev za **Početak indeks** možete konstruirana metodom DOHVATI.

[Naziv indeksa] u zahtjevu za URI određuje koje indeks da biste se vratili iz zbirke indeksa.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `api-key`: Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Zahtjev za **Početak indeksa** mora sadržavati programa `api-key` postavljena na ključa administrator (umjesto ključa upita).

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Ništa.

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

Pogledajte primjer JSON u [Stvaranje i ažuriranje indeksa](#CreateUpdateIndexExample) primjer opseg odgovor.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Brisanje indeksa

**Brisanje indeksa** postupak uklanja indeksa i povezane dokumente iz servisa Azure pretraživanja. Naziv indeksa možete dobiti na nadzornoj ploči servisa na portalu za Azure ili iz API Sučelja. Detalje potražite u članku [Popisa indeksa](#ListIndexes) .

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Zahtjev**

HTTPS potreban je za zahtjeve za uslugu. Zahtjev za **Brisanje indeksa** možete konstruirana pomoću metode brisanja.

[Naziv indeksa] u zahtjevu za URI određuje koje indeks da biste izbrisali iz zbirke indeksa.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `api-key`: Potreban. Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za servis URL-a. Zahtjev za **Brisanje indeksa** mora sadržavati do `api-key` zaglavlje postavljeno na ključ administrator (umjesto ključa upita).

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Ništa.

**Odgovor**

Šifra stanja: 204 bez sadržaja, vraća se uspješno odgovora.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Početak Statistika indeksa

Operacija **Dobiti Statistika indeks** pretraživanja Azure vraća broj dokumenata za trenutni indeks, uz korištenje spremišta.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Statistike o count i pohranu veličina dokumenta prikuplja svakih nekoliko minuta, ne i u stvarnom vremenu. Stoga statistike vratio ovaj API možda odražavaju promjene uzrokovanih nedavne operacija indeksiranja.

**Zahtjev**

HTTPS potreban je za sve zahtjeva za uslugu. Zahtjev za **Početak Statistika indeks** možete konstruirana metodom DOHVATI.

[Naziv indeksa] u zahtjevu za URI govori servisa da biste se vratili Statistika indeks za navedeni indeks.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.


**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `api-key`: Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Zahtjev za **Početak Statistika indeksa** mora sadržavati programa `api-key` postavljena na ključa administrator (umjesto ključa upita).

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Ništa.

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

Tijelo odgovor je u sljedećem obliku:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Testiranje alata za analizu

**Analiza API** prikazuje kako se alata za analizu dijeli teksta na tokena.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Zahtjev**

HTTPS potreban je za sve zahtjeva za uslugu. Zahtjev za **Analizu API -JA** možete konstruirana metodom objavu.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.


**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `api-key`: Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Zahtjev za **Analizu API** mora sadržavati do `api-key` postavljena na ključa administrator (umjesto ključa upita).

Već, morat ćete naziv indeksa i servis za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

ili

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

U `analyzer_name`, `tokenizer_name`, `token_filter_name` i `char_filter_name` moraju biti valjani nazivi unaprijed stvoreni ili prilagođeni analyzers, tokenizers, tokena filtri i filtri znak za indeks. Da biste saznali više o postupku Leksički analize potražite u članku [Analiza u pretraživanju Azure](https://aka.ms/azsanalysis).

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

Tijelo odgovor je u sljedećem obliku:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Analiza primjer API-JA**

**Zahtjev**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Odgovor**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Operacije dokumenta

U pretraživanju Azure indeksa je pohranjen u oblaku i ispunjavaju pomoću dokumente JSON koje prenesete na servis. Sve dokumente koje prenesete čine corpus pretraživanje podataka. Dokumenti sadrže polja, neke od kojih su tokenized u pojmova za pretraživanje kao što će se prenijeti. Na `/docs` segment URL-a u API pretraživanja Azure predstavlja skup dokumenata u indeks. Sve operacije obavljene u zbirci kao što su prijenos, spajanje, brisanje ili slanje upita preuzimanje dokumenata postavite u kontekstu jedne indeks, pa URL-ove za te operacije uvijek započinje s `/indexes/[index name]/docs` ime Dani indeks.

Kodu aplikacije mora generirati JSON dokumenata da biste prenijeli na Azure pretraživanje ili [Indeksiranje](https://msdn.microsoft.com/library/dn946891.aspx) možete koristiti da biste učitali dokumente, ako je izvor podataka baze podataka SQL Azure ili DocumentDB. Obično se unose indeksa iz jedne skup podataka koje unesete.

Trebali biste planirati na pojavljuju jedan dokument za svaku stavku koju želite potražiti. Aplikacija za najam film može imati jedan dokument po film, storefront aplikacija može imati jedan dokument po SKU, aplikaciju online courseware možda imaju jedan dokument po tečaja, istraživanje potvrđeni možda imaju jedan dokument za svaku akademski papira u svoje spremište i tako dalje.

Dokumenti se sastoje od jednog ili više polja. Polja mogu sadržavati tekst koji je tokenized Azure pretraživanja u pojmova za pretraživanje, kao i koje nisu tokenized ili vrijednosti koje nisu tekst koji se mogu koristiti u filtrima i bilježenja rezultata profila. Nazivi, vrste podataka i pretraživanje podržane značajke za svako polje ovise o shemi indeksa. Jedno od polja u svakoj shemi indeksa morate označen kao ID, a svaki dokument mora imati vrijednost za ID polje koje služi kao jedinstvena identifikacija taj dokument u indeks. Sva druga polja dokument nisu obavezni i će prema zadanim postavkama null vrijednost ako neodređeni lijevo. Imajte na umu da se vrijednosti null bi zauzimali prostor u indeks pretraživanja.

Prije nego što možete prenijeti dokumente, morate ste već stvorili indeks na servis. Detalje o ovom prvi korak potražite u članku [Stvaranje indeksa](#CreateIndex) .

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Dodavanje, ažuriranje i brisanje dokumenata

Možete prenijeti, pisama, pisma ili prijenos ili Izbriši dokumente iz Navedeni indeks pomoću HTTP POST. Grupno slanje promjena dokumenata najviše (1000 dokumenti po seriju) ili otprilike 16 MB po seriju se preporučuje za velikog broja ažuriranja.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Zahtjev**

HTTPS potreban je za sve zahtjeve za uslugu. Možete prenijeti, pisama, pisma ili prijenos ili Izbriši dokumente iz Navedeni indeks pomoću HTTP POST.

Zahtjev za URI sadrži [Naziv indeksa], koji određuje koje indeksa na kojoj ćete objaviti dokumente. Samo možete objaviti dokumente indeks jedan po jedan.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `Content-Type`: Potreban. To postavljena na`application/json`
- `api-key`: Potreban. Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Mora sadržavati zahtjev za **Dodavanje dokumenata** u `api-key` zaglavlje postavljeno na ključ administrator (umjesto ključa upita).

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

U tijelu zahtjeva za sadrži jedan ili više dokumenata indeksirati. Dokumenti se prepoznaju pomoću jedinstvenog ključa. Svakom dokumentu je pridružen akciju: prijenos, pisma, mergeOrUpload ili Izbriši. Zahtjevi za prijenos mora obuhvaćati podatke dokument kao skup parove ključa vrijednosti.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Tipke dokumenata može sadržavati samo slova, brojeve, crtice ("-"), podvlake "" i znak jednakosti ("="). Dodatne informacije potražite u članku [imenovanje pravila](https://msdn.microsoft.com/library/azure/dn857353.aspx).

**Akcije dokumenta**

- `upload`: Prijenos akciju slično je "upsert" gdje će se u dokument umetnuti ako je novi i ažurirati/zamijeniti, ako postoji. Napomena u slučaju ažuriranje zamjenjuju sva polja.
- `merge`: Spajanje ažurira postojeći dokument određena polja. Ako dokument ne postoji, spajanja neće uspjeti. Bilo koje polje u spajanja navedete zamijenit će postojeće polje u dokumentu. To obuhvaća polja vrste `Collection(Edm.String)`. Na primjer, ako dokument sadrži polje "oznake" s vrijednošću `["budget"]` i izvršavanje spajanja s vrijednošću `["economy", "pool"]` "oznaka" Završna vrijednost polja "oznake" bit će `["economy", "pool"]`. To će **neće** biti `["budget", "economy", "pool"]`.
- `mergeOrUpload`: ponaša se kao `merge` Ako dokument s određenom ključ već postoji u indeks. Ako dokument ne postoji ponaša se kao `upload` novi dokument.
- `delete`: Brisanje uklanja navedeni dokument iz indeksa. Imajte na umu da sva polja u navedete u `delete` operacija osim polja ključa će je zanemariti. Ako želite ukloniti pojedinačnih polja iz dokumenta pomoću `merge` umjesto i jednostavno polje postavljeno na izričito `null`.

**Odgovor**

Šifra stanja 200 (u redu), vraća se za uspješan odgovor, što znači da sve stavke su uspješno indeksirani. To je označen u `status` svojstvo postavljen je true za sve stavke, kao i kao u `statusCode` svojstvo postavljanje 201 (za prenesenu dokumente) ili 200 (za dokumente spojene ili izbrisani):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Kada najmanje jednu stavku nije uspješno indeksirana, vraća se Šifra stanja 207 (više statusa). Stavke koje nisu indeksirana imaju na `status` polje postavljeno na false. Na `errorMessage` i `statusCode` svojstva će vas upozoriti razlog indeksiranja pogreške:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

U sljedećoj su tablici objašnjava različite šifre stanja po dokumentu koju može vratiti u odgovoru. Imajte na umu neke naznačili probleme sa zahtjevom za sam dok drugima označavanje stanja privremene pogreške. Drugu mogućnost potrebno ponovno nakon odgode.

<table style="font-size:12">
    <tr>
        <th>Šifra stanja</th>
        <th>Značenje</th>
        <th>Retryable</th>
        <th>Bilješke</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument je uspješno promijenili ili izbrisali.</td>
        <td>n/d</td>
        <td>Operacija brisanja su <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. To jest, čak i ako se dokument ključa ne postoji u indeksu, pokušaja operacija brisanja s tim ključem rezultirat će kod 200 stanja.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument stvoren uspješno.</td>
        <td>n/d</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Došlo je do pogreške u dokumentu koji ga je spriječio indeksiraju.</td>
        <td>ne</td>
        <td>Poruka o pogrešci u odgovoru označava što nije u redu s dokumentom.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Dokument nije moguće spojiti jer navedeni ključ ne postoji u indeks.</td>
        <td>ne</td>
        <td>Ta se pogreška se pojavljuje prilikom prijenosa jer mogu stvarati nove dokumente, a ne ne pojavljuje za briše, jer su <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Verzija sukoba otkriven prilikom pokušaja indeksa u dokument.</td>
        <td>Da</td>
        <td>To se može dogoditi kada pokušavate više puta istovremeno indeksa u isti dokument.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Indeks je privremeno nedostupan jer je ažuriran 'allowIndexDowntime' postavljenom na "true".</td>
        <td>Da</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Servis za pretraživanje privremeno nije dostupan, vjerojatno zbog velikim opterećenjem.</td>
        <td>Da</td>
        <td>Kod treba čekati u tom slučaju provesti ili rizikom prolonging nedostupnosti servisa.</td>
    </tr>
</table> 

**Napomena**: Ako vaš klijent kod često naiđe 207 odgovor, jedan od mogućih razloga da je sustav opterećenju. To možete potvrditi tako da u `statusCode` svojstvo 503. Ako je to slučaj, preporučujemo da ***Ograničavanje indeksiranja zahtjeva***. U suprotnom, ako promet za indeksiranje ne subside sustav se može pokrenuti odbijanje sve zahtjeve za 503 pogrešaka.

Šifra stanja 429 označava da ste premašili kvotu na broj dokumenata po indeksa. Stvaranje novog indeksa ili nadogradnje za veća ograničenja kapaciteta.

**Primjer:**

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
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Pretraživanje dokumenata

Operacija **pretraživanja** izdaje kao zahtjev za DOHVAĆANJE ili objavu, a određuje parametre koje daju kriterije za odabir odgovarajući dokumente.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Korištenje objavu umjesto GET**

Kada koristite HTTP GET da biste nazvali **pretraživanje** API, morate imati na umu da duljinu URL-a zahtjev ne može biti dulji od 8 KB. To je obično dovoljno za većinu aplikacija. Međutim, neke aplikacije proizvesti vrlo velike upita ili OData izraza filtra. Za sljedeće aplikacije pomoću HTTP POST bolji je odabir jer dopušta veće filtri i upiti od GET. OBJAVA, broj uvjeta ili izraza u upitu je ograničavanje faktor ne veličinu neobrađenog upita jer je ograničenje veličine zahtjeva za objavu približno 16 MB.

> [AZURE.NOTE] Čak i ako je ograničenje veličine zahtjeva za objavu vrlo velike, upita za pretraživanje i izrazi filtra ne mogu biti arbitrarily složene. Dodatne informacije o pretraživanje upita i filtriranje složenosti ograničenja potražite u [Lucene sintaksu upita](https://msdn.microsoft.com/library/mt589323.aspx) i [OData sintaksu izraza](https://msdn.microsoft.com/library/dn798921.aspx) .

**Zahtjev**

HTTPS potreban je za zahtjeve za uslugu. Zahtjev za **pretraživanjem** možete konstruirana pomoću metode GET ili objavu.

Zahtjev za URI određuje koje indeks upita za sve dokumente koje odgovaraju parametre. Određeni su klauzulom parametara u nizu upita u slučaju zahtjevi za DOHVAĆANJE, a u zahtjevu za tijelo slučaju objavu zahtjeve za razgovore.

Kao praksa pri stvaranju zahtjevi za DOHVAĆANJE, ne zaboravite [URL kodiranje](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametara određene upita prilikom pozivanja REST API-JA izravno. Za **pretraživanje** operacije, to uključuje:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

Kodiranje URL preporučuje se samo na gornje parametre upita. Ako ste slučajno URL-kodiranje niz cijeli upita (sve nakon na?), zahtjevi za će prekinuti.

Osim toga, URL kodiranje potrebno je samo prilikom pozivanja REST API-JA izravno pomoću GET. Nema kodiranje URL-a potrebno je prilikom pozivanja **pretraživanje** pomoću objavu ili pri korištenju [.NET klijentska biblioteka](https://msdn.microsoft.com/library/dn951165.aspx)koji upravlja kodiranja URL-a.

<a name="SearchQueryParameters"></a>
**Parametri upita**

**Pretraživanje** prihvaća nekoliko parametre koji omogućuje kriterija upita i odrediti ponašanje pretraživanja. Navedite parametara u URL niz u upitu prilikom pozivanja **pretraživanja** putem GET i kao JSON svojstva u tijelu zahtjeva prilikom pozivanja **pretraživanja** putem objavu. Vidjet ćete da sintaksa za neke parametre su malo razlike između GET i objavu. Te razlike su zabilježene kao mjerodavni ispod:

`search=[string]`(neobavezno) – tekst za pretraživanje. Sve `searchable` polja se pretražuju po zadanom ako `searchFields` nije naveden. Pri pretraživanju `searchable` polja, a zatim tekst koji želite potražiti sam je tokenized, tako da više uvjeta mogu biti podijeljeni praznina (na primjer: `search=hello world`). Da biste potražili bilo koji izraz, koristite `*` (to može biti korisno za upite logički filtar). Ispuštanje taj parametar ima isti učinak kao i postavite na `*`. Specifičnosti na sintaksa za pretraživanje potražite u članku [Jednostavne sintaksu upita](https://msdn.microsoft.com/library/dn798920.aspx) .

  - **Napomena**: rezultate ponekad može surprising kada upita putem `searchable` polja. Na izrađivača tokena sadrži logike za rukovanje slučajeva zajedničke engleski tekst kao što su apostrofe, zareze u brojeve, itd. Na primjer, `search=123,456` odgovarat će izraz 123,456 umjesto pojedinačne uvjete 123 i 456, jer zareze se koriste kao tisuće razdjelnici za velike brojeva u engleskom jeziku. Zbog toga preporučujemo korištenje praznina umjesto interpunkcijske znakove za razdvajanje uvjete u `search` parametar.

`searchMode=any|all`(nije obavezno, zadanih vrijednosti za `any`) – li nešto ili sve od pojmova za pretraživanje mora biti uparuje za brojanje dokument kao rezultat.

`searchFields=[string]`(neobavezno) – na popisu Nazivi odvojenih zarezom polja da biste potražili određeni tekst. Ciljna polja moraju biti označen kao `searchable`.

`queryType=simple|full`(nije obavezno, zadanih vrijednosti za `simple`) – kada je postavljeno na "jednostavne" traženi tekst tumači pomoću jezika za jednostavne upite koji omogućuje simbole kao što su +, * i "". Upiti se procjenjuju preko sva polja pretraživanja (ili polja označen `searchFields`) u svakom dokumentu prema zadanim postavkama. Kada se vrsta upita postavljena na `full` tekst koji želite potražiti tumači pomoću jezika Lucene upita koji omogućuje specifične za polja i ponderiranog pretraživanja. Pročitajte članak [Jednostavne sintaksu upita](https://msdn.microsoft.com/library/dn798920.aspx) i [Lucene sintaksu upita](https://msdn.microsoft.com/library/mt589323.aspx) za specifičnosti na syntaxes pretraživanja. 
 
> [AZURE.NOTE] Pretraživanje u Lucene jezik upita nije podržan korist $filter koja sadrži sličnu funkciju u rasponu.

`moreLikeThis=[key]`(neobavezno) **Važne:** Ova je značajka dostupna samo u `2015-02-28-Preview`. Ta mogućnost nije moguće koristiti u upit koji sadrži parametar pretraživanje teksta `search=[string]`. Na `moreLikeThis` parametar pronalazi dokumente koje su slične dokumenta određen tipku dokumenta. Kada se zahtjev za pretraživanjem izvršena s `moreLikeThis`, popis pojmova za pretraživanje generira se ovisno o učestalosti i rarity pojmova u izvorišnom dokumentu. Da biste zahtjev pa koriste izraze. Prema zadanim postavkama, sadržaj svih `searchable` polja smatraju osim ako `searchFields` koristi se za ograničavanje polja koja se pretražuju.  

`$skip=#`(neobavezno) – broja rezultata pretraživanja da biste preskočili; Ne može biti veći od 100,000. Ako morate omogućiti skeniranje dokumenata u nizu, ali ne možete koristiti `$skip` zbog ograničenja, preporučuje se korištenje `$orderby` na ključu da naručili i `$filter` s rasponom upita umjesto toga.

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `skip` umjesto `$skip`.

`$top=#`(neobavezno) – broja rezultata pretraživanja za dohvaćanje. To se može koristiti u kombinaciji s `$skip` implementaciju klijentsko broja stranica rezultata pretraživanja.

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `top` umjesto `$top`.

`$count=true|false`(nije obavezno, zadanih vrijednosti za `false`) – određuje hoće li se dohvaćati ukupan broj rezultata. To je broj sve dokumente koji se podudaraju s `search` i `$filter` parametre, zanemarujući `$top` i `$skip`. Tu vrijednost postavite na `true` može imati utjecaj na performanse. Imajte na umu da je broj vraćenih usklađivanja.

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `count` umjesto `$count`.

`$orderby=[string]`(neobavezno) – popis odvojenih zarezom izraza da biste sortirali rezultate po. Svaki izraz može biti naziv polja ili poziv na `geo.distance()` (opis funkcije). Svaki izraz možete slijedi `asc` da biste naznačili uzlazno, a `desc` da biste naznačili silazno. Zadani je uzlaznim redoslijedom. TIES će do oštećenja tako da rezultati podudaraju dokumenata. Ako nema `$orderby` nije naveden, zadani redoslijed sortiranja je Silazno po dokumentu podudaranje rezultat. Postoji ograničenje od 32 uvjeta za `$orderby`.

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `orderby` umjesto `$orderby`.

`$select=[string]`(neobavezno) – odvojenih zarezom uzorak dohvatiti. Ako neodređeni, uključene su sva polja označena kao veličina u shemi. Sva polja možete zatražiti i izričito postavljanjem taj parametar `*`.

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `select` umjesto `$select`.

`facet=[string]`(nula ili više) – polje fasete po. Po želji niz može sadržavati parametara da biste prilagodili faceting izražen kao odvojenih zarezom `name:value` parove. Valjani su:

- `count`(maksimalni broj uvjeta fasete; zadana je 10). Postoji nema maksimuma, ali velikih vrijednosti plaćati odgovarajuće kazna performanse, osobito ako je polje slojevito sadrži veliki broj jedinstvenih uvjeta.
  - Na primjer: `facet=category,count:5` dobiva najboljih pet kategorija u rezultatima fasete.  
  - **Napomena**: Ako se `count` parametar je manji broj jedinstvenih uvjeta, rezultati nisu nužno točne. To je način upita faceting su raspodijeliti shards. Povećanje `count` obično povećava točnost broji termina, ali uz mjesečnu naknadu performansi.
- `sort`(jedan od `count` da biste sortirali *Silazno* po count, `-count` da biste sortirali *Uzlazno* po count, `value` da biste sortirali *Uzlazno* po vrijednosti, ili `-value` da biste sortirali *Silazno* po vrijednost)
  - Na primjer: `facet=category,count:3,sort:count` dobiva vrha tri kategorije u rezultatima fasete silaznim redoslijedom prema broju dokumenata s svaki grad. Ako, na primjer, ako gornje tri kategorije su proračun, Motel i Luxury, i proračun sadrži 5 pristupa, Motel sadrži 6 Luxury sadrži, 4, zatim polja bit će redoslijedom Motel, proračun, Luxury.
  - Na primjer: `facet=rating,sort:-value` daje memorijski blokovi za sve moguće ocjene silaznim redoslijedom prema vrijednosti. Na primjer, ako ocjene su od 1 do 5, polja će naručiti 5, 4, 3, 2, 1, bez obzira koliko dokumenata odgovaraju svakom ocjena.
- `values`(razgraničen kanala numeričke ili `Edm.DateTimeOffset` vrijednosti koji određuje dinamički skup vrijednosti fasete)
  - Na primjer: `facet=baseRate,values:10|20` daje tri memorijski blokovi: jednu za osnovnu stopa 0 do, ali ne uključujući 10, jedan za 10 do ali ne i 20 i jednu za 20 ili noviji.
  - Na primjer: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` daje dva memorijski blokovi: jedan za Hoteli renovated prije veljača 2010, a jedan za Hoteli renovated veljača izračun 1st, 2010 ili noviji.
- `interval`(interval cijeli broj veći od 0 za brojeve, ili `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` za vrijednosti datuma vremena)
  - Na primjer: `facet=baseRate,interval:100` daje memorijski blokovi koji se temelji na rasponima osnovni stopa veličine 100. Ako, na primjer, ako su osnovne stope između 60 $ a $600, će postojati memorijski blokovi za 0-100, 100 200, 200 300, 300 400, 400-500 i 500 600.
  - Na primjer: `facet=lastRenovationDate,interval:year` daje jednu grupe za svaku godinu kada su renovated hoteli.
- `timeoffset`([+-] hh, [+-] put spremljen, ili [+-] hh) `timeoffset` nije obavezno. Samo se kombinirati i s na `interval` mogućnost, a samo kad je primijenjena na polje vrste `Edm.DateTimeOffset`. Vrijednost određuje UTC pomak vremenske račun za postavljanje ograničenja vremena.
  - Na primjer: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` koristi granicu dan koji se pokreće pri 01:00:00 UTC-a (ponoći u vremenskoj zoni cilj)
- **Napomena**: `count` i `sort` možete kombinirati u istom specifikacija fasete, ali ne može se kombinirati s `interval` ili `values`, a `interval` i `values` ne može se kombinirati zajedno.
- **Napomena**: Interval pozornici na datum vrijeme su izračunati na temelju UTC vremena ako `timeoffset` nije naveden. Na primjer: za `facet=lastRenovationDate,interval:day`, dan granicu započinje 00:00:00 UTC-a. 

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `facets` umjesto `facet`. Osim toga, navedite ga kao JSON polja nizova gdje je svaki niz zasebnom fasete izraz.

`$filter=[string]`(neobavezno) – izraz za traženje strukturirane u standardni OData sintaksi. Detalje o podskup gramatike OData izraz koji podržava Azure pretraživanja potražite u članku [OData sintaksu izraza](#ODataExpressionSyntax) .

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `filter` umjesto `$filter`.

`highlight=[string]`(neobavezno) – ističe skup polja odvojenih zarezom nazive koji se koriste za rezultat. Samo `searchable` polja koje se mogu koristiti za isticanje uspješnosti.

`highlightPreTag=[string]`(nije obavezno, zadanih vrijednosti za `<em>`) – U niz oznaku koja označava pogoditi iz prvog isticanja. Mora biti postavljeno s `highlightPostTag`.

> [AZURE.NOTE] Kada nazovete podršku za **pretraživanje** pomoću GET, rezerviranih znakova u URL-u moraju biti postotak-kodirani (na primjer, % 23 umjesto #).

`highlightPostTag=[string]`(nije obavezno, zadanih vrijednosti za `</em>`) – oznaku niz koji dodaje pogoditi iz prvog isticanja. Mora biti postavljeno s `highlightPreTag`.

> [AZURE.NOTE] Kada nazovete podršku za **pretraživanje** pomoću GET, rezerviranih znakova u URL-u moraju biti postotak-kodirani (na primjer, % 23 umjesto #).

`scoringProfile=[string]`(neobavezno) – naziv bilježenja rezultata profil da biste procijenili odgovara brojčane pokazatelje za dokumente koji se podudaraju da biste sortirali rezultate.

`scoringParameter=[string]`(nula ili više) – označava vrijednosti za svaki parametar definirano u funkciji bilježenja rezultata (na primjer, `referencePointParameter`) pomoću oblika `name-value1,value2,...`.

- Ako, na primjer, ako bilježenja rezultata profila definira funkcije parametra pod nazivom "mylocation" mogućnost niz upit bio `&scoringParameter=mylocation--122.2,44.8`. Prvi crtice odvaja ime s popisa vrijednosti dok je drugi crtice dio prva vrijednost (dužinu u ovom primjeru).
- Za bilježenja rezultata parametre kao što su za oznaku povećanje koji mogu sadržavati zareze, možete escape takve vrijednosti na popisu pomoću jednostruke navodnike. Ako ne sadrže vrijednosti same jednostrukim navodnicima, možete ih escape udvostručavanjem.
  - Ako, na primjer, ako imate oznake povećanje parametar pod nazivom "mytag", a želite povećati na oznaci vrijednosti "Pozdrav, O'Brien" i "Kopač", upit bio bi mogućnost niz `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Imajte na umu da ponude samo potrebni za vrijednosti koje sadrže zarezima.

> [AZURE.NOTE] Pri pozivanju **pretraživanje** pomoću objavu, taj parametar pod nazivom `scoringParameters` umjesto `scoringParameter`. Osim toga, odredite je kao JSON polja nizova kojima je svaki niz posebnu `name-values` par.

`minimumCoverage`(nije obavezno, po zadanom je 100) – broj između 0 i 100 u kojoj stoji da se postotak indeks koji morate prekriveni upit za pretraživanje da bi upit da biste se prijavili kao vjerojatnost_u. Prema zadanim postavkama, cijeli indeks moraju biti dostupne ili `Search` će vratiti Šifra stanja HTTP 503. Ako ste postavili `minimumCoverage` i `Search` uspješnog bit će vratiti HTTP 200 i uključiti u `@search.coverage` vrijednost u odgovor koji označava postotak indeksa uvrštene u upit.

> [AZURE.NOTE] Postavljanje taj parametar na vrijednost manju od 100 mogu poslužiti za osiguravanje dostupnost pretraživanja čak i za usluge s ugovorima o samo jedan replike. Međutim, nije sve dokumente koji se podudaraju se zajamčeno koje su prisutne u rezultatima pretraživanja. Ako je važnija u aplikaciji od dostupnost opoziv pretraživanja, a zatim preporučuje se da biste napustili `minimumCoverage` na zadanu vrijednost od 100.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

Napomena: za ovaj postupak u `api-version` je naveden kao parametra upita u URL-u bez obzira na to jesu li poziva **pretraživanja** s GET ili objavu.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `api-key`: Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za servis URL-a. Zahtjev za **pretraživanjem** možete navesti ključ za administratore ili upita ključ za `api-key`.

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Za početak: ništa.

Za objavu:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Nastavka odgovora djelomičnog pretraživanja**

Ponekad Azure pretraživanje ne može vratiti sve tražene rezultate u jednom odgovor pretraživanja. To se može dogoditi razloga, kao što su kada upit traži previše dokumente navođenjem ne `$top` ili određuje vrijednost za `$top` koji je prevelik. U tim slučajevima neće sadržavati stavku Azure pretraživanja na `@odata.nextLink` opaske u tijelu odgovor i `@search.nextPageParameters` ako je zahtjev za objavu. Da biste formulate drugi zahtjev za pretraživanje da biste dobili sljedeće dio pretraživanje odgovor, poslužite se vrijednosti te primjedbe. To se naziva ***nastavka*** izvorni zahtjev za pretraživanje, a primjedbe obično nazivaju se ***tokeni nastavka***. Pogledajte [primjer u nastavku](#SearchResponse) za informacije o sintaksi te primjedbe i tamo gdje se prikazuju u tijelu odgovor. 

Razloga zašto Azure pretraživanje može vratiti nastavka tokeni su specifične za implementaciju i mogu se promijeniti. Robusne klijenti uvijek mora biti spreman za rukovanje slučajevima gdje se vraćaju dokumenata manje od očekivanih i nastavka token uključen da biste nastavili dohvaćanja dokumenata. Imajte na umu da morate koristiti istu metodu HTTP izvorni zahtjev za nastavak. Na primjer, ako ste poslali zahtjevom GET, sve zahtjeve za nastavka šaljete morate koristiti GET (i isto tako za objavu).

<a name="SearchResponse"></a>
**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Primjeri:**

Dodatne primjere možete pronaći na stranici [OData sintaksu izraza za pretraživanje Azure](https://msdn.microsoft.com/library/azure/dn798921.aspx) .

1)  Indeks pretraživanja sortirane Silazno po datumu.


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = * & $orderby = lastRenovationDate desc & api-verzija = 2015-02-28-pretpregled

    /Indexes/hotels/docs/search objavu? api-verzija 2015-02-28-Pretpregled = {"pretraživanje": "*", "orderby": "lastRenovationDate desc"}

2)  Slojevito pretraživanja u indeksu i dohvaćanje pozornici za kategorije, ocjena, oznake, kao i stavke s baseRate određene rasponi:


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = test & fasete = kategorija & fasete = ocjena & fasete = oznake i fasete = baseRate, vrijednosti: 80 | 150 | 220 & api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "testiranje", "pozornici": ["Kategorija", "ocjena", "oznake", "baseRate, vrijednosti: 80 | 150 | 220"]}

3)  Pomoću filtra, suzili rezultate prethodne slojevito upita nakon klika na ocjena 3 "i" Kategorija "Motel":


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = test & fasete = oznake i fasete = baseRate, vrijednosti: 80 | 150 | 220 & $filter = eq ocjena 3 i kategorija eq "Motel" & api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "testiranje", "pozornici": ["oznake", "baseRate, vrijednosti: 80 | 150 | 220"], "Filtar": "ocjena eq 3 i kategorija eq 'Motel'"}

4) U slojevito pretraživanja, postaviti gornju granicu jedinstveni uvjete koji se vraćaju u upitu. Zadana vrijednost je 10, ali možete povećati ili smanjiti pomoću vrijednosti u `count` parametar na na `facet` atribut:


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = test & fasete = Grad, count: 5 i api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "testiranje", "pozornici": ["Grad, count: 5"]}

5)  U indeksu unutar određenih polja; Na primjer, polje jezično specifične:


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = hôtel & searchFields = description_fr & api-verzija = 2015-02-28-pretpregled

    /Indexes/hotels/docs/search objavu? api-verzija 2015-02-28-Pretpregled = {"pretraživanje": "hôtel", "searchFields": "description_fr"}

6) Indeks pretražiti više polja. Na primjer, možete pohraniti i upit koji se može pretraživati polja na više jezika, sve unutar iste indeksa.  Ako na engleskom i francuski opise Suradnja postoji u istom dokumentu, možete se vratiti nešto ili sve u rezultatima upita:


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = hotelski & searchFields = opis, description_fr & api-verzija = 2015-02-28-pretpregled

    /Indexes/hotels/docs/search objavu? api-verzija 2015-02-28-Pretpregled = {"pretraživanje": "hotelski", "searchFields": "opis, description_fr"}

Imajte na umu da jedan indeks možete upite samo po jedan. Stvaranje višestrukih indeksa za svaki jezik osim ako ne namjeravate upit jedan po jedan.

7)  Broja stranica - Get stranici 1st stavki (veličine stranice je 10):


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = * & $skip = 0 & $top = 10 & api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "*", "preskočiti": 0, "gornji": 10}

8)  Broja stranica - Get 2 stranice stavki (veličine stranice je 10):


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = * & $skip = 10 & $top = 10 & api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "*", "preskočiti": "gornji" 10: 10}

9)  Dohvaćanje određeni skup polja:


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = * & $select = hotelName, opis i api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "*", "odaberite": "hotelName opis"}

10)  Dohvaćanje dokumente koji se podudaraju izraza određene filtra:


    DOHVAĆANJE /indexes/hotels/docs? $filter = (baseRate Spoji 60 i baseRate lt 300) ili hotelName eq "Atraktivnog Ostanite" i api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"Filtar": "(baseRate Spoji 60 i baseRate lt 300) ili hotelName eq 'Atraktivnog Ostanite'"}

11) Pretraživanje fragmentirane indeksa i vrati se s alatima za isticanje uspješnosti


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje nešto = & isticanje = opis & api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "nešto", "Isticanje": "Opis"}

12) U indeksu i vratili se dokumenti sortirane od mjesto bliže ono izvan reference


    DOHVAĆANJE /indexes/hotels/docs? pretraživanja nešto = & $orderby=geo.distance (mjesto, geography'POINT(-122.12315 47.88121)') & api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "nešto", "orderby": "geo.distance (mjesto, geography'POINT(-122.12315 47.88121)')"}

13) Pretraživanje indeksa uz pretpostavku da je bilježenja rezultata profila pod nazivom "zemlj." s dvije udaljenost bilježenja rezultata funkcije, jedan koji definira parametar pod nazivom "currentLocation" i jedan koji definira parametar pod nazivom "lastLocation"


    DOHVATI /indexes/hotels/docs? pretraživanje nešto = & scoringProfile = zemlj & scoringParameter = currentLocation – 122.123,44.77233 & scoringParameter = lastLocation – 121.499,44.2113 & api-verzija = 2015-02-28-pretpregled

    /Indexes/hotels/docs/search objavu? api-verzija 2015-02-28-Pretpregled = {"pretraživanje": "nešto", "scoringProfile": "zemlj.", "scoringParameters": ["currentLocation – 122.123,44.77233", "lastLocation – 121.499,44.2113"]}

14) Da biste pronašli dokumente u indeksu koji koriste [sintaksu jednostavnog upita](https://msdn.microsoft.com/library/dn798920.aspx). Ovaj upit vraća Hoteli koje sadrže polja pretraživanja uvjeta "udobnosti" i "mjesto" ali ne "motel":


    DOHVAĆANJE /indexes/hotels/docs? pretraživanje = udobnosti + mjesto-motel & searchMode = all & api-verzija = 2015-02-28-pretpregled

    OBJAVA /indexes/hotels/docs/search? api-verzija = 2015-02-28-Pretpregled {"pretraživanje": "udobnosti + mjesto-motel", "searchMode": "sve"}

Imajte na umu korištenje `searchMode=all` iznad. Uključujući taj parametar nadjačava zadano od `searchMode=any`, osiguravanje koji `-motel` znači "I nije" umjesto "Ili ne". Bez `searchMode=all`, dobit "Ili nije" koja se proširuje umjesto ograničava rezultata pretraživanja, a to može biti counter-intuitive nekim korisnicima.

15) Da biste pronašli dokumente u indeksu koji koriste [sintaksu upita lucene](https://msdn.microsoft.com/library/mt589323.aspx). Ovaj upit vraća Hoteli gdje polje kategorija određenim pojmom "okvira proračuna" i sva polja pretraživanja koje sadrže izraz "nedavno renovated". Dokumente koji sadrže točan izraz "nedavno renovated" rangiranja višu uslijed pojačavanje vrijednost termina (3)

    DOHVAĆANJE /indexes/hotels/docs?search = kategorije: proračun i \"nedavno renovated\"^ 3 i searchMode = all & api-verzija = 2015-02-28-Pretpregled & querytype = potpuni

    OBJAVA /indexes/hotels/docs/search?api-version = 2015-02-28-Pretpregled {"pretraživanje": "kategorija: proračun i \"nedavno renovated\"^ 3", "queryType": "puno", "searchMode": "sve"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Pretraživanje dokumenta

Operacija **Pretraživanja dokumenta** dohvaća dokumenta iz pretraživanja Azure. To je korisno kada korisnik klikne na određene rezultata, a želite da biste potražili određene detalje o tom dokumentu.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Zahtjev**

HTTPS potreban je za zahtjeve za uslugu. Zahtjev za **Pretraživanje dokumenta** možete sastavljena na sljedeći način.

    GET /indexes/[index name]/docs/key?[query parameters]

Osim toga, možete koristiti sintaksa tradicionalni OData ključa pretraživanja:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Zahtjev za URI sadrži [Naziv indeksa] i [ključ], koji određuje koji će se dokument dohvatiti iz koje indeksa. Istovremeno možete dobiti samo jedan dokument. Pomoću **pretraživanja** da biste dobili više dokumenata u jedan zahtjev.

**Parametri upita**

`$select=[string]`(neobavezno) – odvojenih zarezom uzorak dohvatiti. Ako neodređeni ili postavljena na `*`, sva polja označena kao veličina u shemi obuhvaćeni projekciju.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

Napomena: za ovaj postupak u `api-version` je naveden kao parametra upita.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `api-key`: Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za servis URL-a. Zahtjev za **Pretraživanje dokumenta** možete navesti ključ za administratore ili upita ključ za `api-key`.

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Ništa.

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Primjer**

Pretraživanje dokumenta koji ima ključ "2"

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Pretraživanje dokumenta koji ima ključ '3' pomoću OData sintakse:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Broj dokumenata

Operacija **Broj dokumenata** dohvaća ukupan broj dokumenata u indeks pretraživanja. Na `$count` sintaksa je dio protokola OData.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Zahtjev**

HTTPS potreban je za zahtjeve za uslugu. Zahtjev za **Brojanje dokumente** možete konstruirana metodom DOHVATI.

[Naziv indeksa] u zahtjevu za URI govori servisa da biste se vratili ukupan broj svih stavki u skupu dokumenata na navedenog indeksa.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev.

- `Accept`: Ta vrijednost mora biti postavljeno na `text/plain`.
- `api-key`: Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za servis URL-a. Zahtjev za **Brojanje dokumente** možete navesti ključ za administratore ili upita ključ za `api-key`.

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Ništa.

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

Odgovor tijelo sadrži vrijednost broja kao cijeli broj oblikovan kao običan tekst.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Prijedlozi

Operacija **prijedloge** dohvaća prijedloge na temelju unos djelomičnog pretraživanja. Obično koristi se u okvire pretraživanja možete unijeti vrsta dovršena prije prijedloge kao korisnici unosa pojmova za pretraživanje.

Zahtjeva za prijedlog usmjerite pri predlaganja cilj dokumenti, pa predloženog teksta može se ponoviti ako većeg broja dokumenata kandidata odgovaraju istog pretraživanja za unos. Možete koristiti `$select` za dohvaćanje ostala polja dokumenta (uključujući tipku dokumenta) tako da možete prepoznati koji dokument je izvor za svaki prijedlog.

**Prijedlozi** operacija izdaje kao zahtjev za DOHVAĆANJE ili objavu.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Korištenje objavu umjesto GET**

Kada koristite HTTP GET da biste nazvali **prijedloge** API, morate imati na umu da duljinu URL-a zahtjev ne može biti dulji od 8 KB. To je obično dovoljno za većinu aplikacija. Međutim, neke aplikacije proizvesti vrlo velike upita, posebno OData izraza filtra. Za sljedeće aplikacije pomoću HTTP POST bolji je odabir jer dopušta filtri veće od GET. OBJAVA, broj uvjeta u filtru je ograničavanje faktor ne veličinu niz neobrađenog filtra jer je ograničenje veličine zahtjeva za objavu približno 16 MB.

> [AZURE.NOTE] Čak i ako je ograničenje veličine zahtjeva za objavu vrlo velike, ne može biti arbitrarily složene izraze filtra. Dodatne informacije o ograničenju složenosti filtra potražite u članku [OData sintaksu izraza](https://msdn.microsoft.com/library/dn798921.aspx) .

**Zahtjev**

HTTPS potreban je za zahtjeve za uslugu. Zahtjev za **prijedloge** možete konstruirana pomoću metode GET ili objavu.

Zahtjev za URI određuje naziv indeksa u upit. Parametri, kao što su djelomično unos traženi pojam, određeni su klauzulom u nizu upita u slučaju zahtjevi za DOHVAĆANJE, a u zahtjevu za tijelo slučaju objavu zahtjeve za razgovore.

Kao praksa pri stvaranju zahtjevi za DOHVAĆANJE, ne zaboravite [URL kodiranje](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametara određene upita prilikom pozivanja REST API-JA izravno. Za **prijedloge** operacije, to uključuje:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

Kodiranje URL preporučuje se samo na gornje parametre upita. Ako ste slučajno URL-kodiranje niz cijeli upita (sve nakon na?), zahtjevi za će prekinuti.

Osim toga, URL kodiranje potrebno je samo prilikom pozivanja REST API-JA izravno pomoću GET. Nema kodiranje URL-a potrebno je prilikom pozivanja **prijedloge** pomoću objavu ili pri korištenju [.NET klijentska biblioteka](https://msdn.microsoft.com/library/dn951165.aspx)koji upravlja kodiranja URL-a.

**Parametri upita**

**Prijedlozi** prihvaća nekoliko parametre koji omogućuje kriterija upita i odrediti ponašanje pretraživanja. Navedite parametara u URL niz u upitu prilikom pozivanja **prijedloge** putem GET i kao JSON svojstva u tijelu zahtjeva prilikom pozivanja **prijedloge** putem objavu. Vidjet ćete da sintaksa za neke parametre su malo razlike između GET i objavu. Te razlike su zabilježene kao mjerodavni ispod:

`search=[string]`-pretraživanje tekst koji želite koristiti za prijedlog upita. Mora biti najmanje 1 znak i više od 100 znakova.

`highlightPreTag=[string]`(neobavezno) – niza oznaku koja označava da biste pronašli pristupa. Mora biti postavljeno s `highlightPostTag`.

> [AZURE.NOTE] Kada nazovete **prijedloge** pomoću GET, rezerviranih znakova u URL-u moraju biti postotak-kodirani (na primjer, % 23 umjesto #).

`highlightPostTag=[string]`(neobavezno) – niza za označavanje koji dodaje da biste pronašli pristupa. Mora biti postavljeno s `highlightPreTag`.

> [AZURE.NOTE] Kada nazovete **prijedloge** pomoću GET, rezerviranih znakova u URL-u moraju biti postotak-kodirani (na primjer, % 23 umjesto #).

`suggesterName=[string]`-Naziv suggester kao što je navedeno u na `suggesters` zbirke koja je dio definicija indeksa. A `suggester` određuje da su polja koja traže predložene izraze. Detalje potražite u članku [Suggesters](#Suggesters) .

`fuzzy=[boolean]`(nije obavezno, zadani = false) – kada postavite TRUE taj API tražit će prijedloge čak i ako je tekst koji želite potražiti zamijenjenom ili onu koja nedostaje znak. Dok je to omogućuje bolje iskustvo u nekim slučajevima dođe na performanse cijena mutno prijedlog pretraživanja su sporije i trošiti dodatne resurse.

`searchFields=[string]`(neobavezno) – na popisu naziva odvojenih zarezom polje za pretraživanje teksta pretraživanja. Ciljna polja mora biti omogućen za prijedloge.

`$top=#`(nije obavezno, zadani = 5) – broj prijedloga za dohvaćanje. Mora biti broj između 1 i 100.

> [AZURE.NOTE] Pri pozivanju **prijedloge** pomoću objavu, taj parametar pod nazivom `top` umjesto `$top`.

`$filter=[string]`(neobavezno) – izraz koji filtrira dokumente koje se smatraju prijedloge.

> [AZURE.NOTE] Pri pozivanju **prijedloge** pomoću objavu, taj parametar pod nazivom `filter` umjesto `$filter`.

`$orderby=[string]`(neobavezno) – popis odvojenih zarezom izraza da biste sortirali rezultate po. Svaki izraz može biti naziv polja ili poziv na `geo.distance()` (opis funkcije). Svaki izraz možete slijedi `asc` da biste naznačili uzlazno, a `desc` da biste naznačili silazno. Zadani je uzlaznim redoslijedom. Postoji ograničenje od 32 uvjeta za `$orderby`.

> [AZURE.NOTE] Pri pozivanju **prijedloge** pomoću objavu, taj parametar pod nazivom `orderby` umjesto `$orderby`.

`$select=[string]`(neobavezno) – odvojenih zarezom uzorak dohvatiti. Ako neodređeni, vraća se samo ključ i prijedlog tekst dokumenta. Sva polja možete zatražiti izričito postavljanjem taj parametar `*`.

> [AZURE.NOTE] Pri pozivanju **prijedloge** pomoću objavu, taj parametar pod nazivom `select` umjesto `$select`.

`minimumCoverage`(nije obavezno, po zadanom je 80) – broj između 0 i 100 u kojoj stoji da se postotak indeks koji morate prekriveni prijedlozi za upit da bi upit da biste se prijavili kao vjerojatnost_u. Prema zadanim postavkama, najmanje 80% indeksa moraju biti dostupne ili `Suggest` će vratiti Šifra stanja HTTP 503. Ako ste postavili `minimumCoverage` i `Suggest` uspješnog bit će vratiti HTTP 200 i uključiti u `@search.coverage` vrijednost u odgovor koji označava postotak indeksa uvrštene u upit.

> [AZURE.NOTE] Postavljanje taj parametar na vrijednost manju od 100 mogu poslužiti za osiguravanje dostupnost pretraživanja čak i za usluge s ugovorima o samo jedan replike. Međutim, sve odgovarajuće prijedloge su zajamčeno koje su prisutne u rezultate. Ako je opoziv važnija u aplikaciji od dostupnosti, a zatim preporučuje se da ne donji `minimumCoverage` ispod zadanu vrijednost od 80.

`api-version=[string]`(obavezno). Pretpregled verzija `api-version=2015-02-28-Preview`. Potražite u članku [Rad s verzijama servisa za pretraživanje](http://msdn.microsoft.com/library/azure/dn864560.aspx) detalje i druge verzije.

Napomena: za ovaj postupak u `api-version` je naveden kao parametra upita u URL-u bez obzira na to je li poziva **prijedloge** GET ili objavu.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjeva

- `api-key`: Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za servis URL-a. Zahtjev za **prijedloge** možete navesti ključ za administratore ili ključ upita kao na `api-key`.

Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz servisa nadzorne ploče na portalu za Azure. Potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

**Zahtjev za tijelo**

Za početak: ništa.

Za objavu:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Ako se mogućnost projekcija koristi za dohvaćanje polja uključenima u svaki element polja:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Primjer**

Dohvaćanje 5 prijedloge gdje je mogućnost djelomičnog pretraživanja unos 'lux'

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
