<properties
    pageTitle="Povezivanje DocumentDB pomoću pretraživanja Azure pomoću indexers | Microsoft Azure"
    description="U ovom se članku objašnjava pomoću komponente za indeksiranje pretraživanja Azure s DocumentDB kao izvor podataka."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Povezivanje DocumentDB pomoću pretraživanja Azure pomoću indexers

Ako tražite implementaciju sjajno pretraživanje sučelja DocumentDB podataka, koristite indeksiranje pretraživanja Azure za DocumentDB! U ovom se članku Pokazat ćemo vam kako integrirati Azure DocumentDB pomoću pretraživanja Azure bez potrebe za pisanja koda da biste zadržali indeksiranja infrastrukture!

Da biste to postavili ste [Postavljanje računa za pretraživanje Azure](../search/search-create-service-portal.md) (ne morate nadograditi pretraživanje), a poziva [Azure pretraživanje REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) za stvaranje DocumentDB **izvora podataka** i **Indeksiranje** za taj izvor podataka.

U redoslijed Pošalji zahtjeve za interakciju s REST API-ji, možete koristiti [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)ili bilo koji alat za vaše osobne postavke.


##<a id="Concepts"></a>Azure koncepata indeksiranje pretraživanja

Azure pretraživanja podržava stvaranje i upravljanje podataka izvora (uključujući DocumentDB) i indexers koji rade s tim izvorima podataka.

**Izvor podataka** određuje podataka koje je potrebno indeksirati, vjerodajnice za pristup podacima i pravila da biste omogućili pretraživanje Azure učinkovito prepoznavanje promjene u podacima (kao što su promijenili ili izbrisali dokumente unutar vaše zbirke). Izvor podataka se definira kao neovisno resursa tako da ga može koristiti više indexers.

**Indeksiranje** opisuje kako teče podatke iz izvora podataka u indeks za traženje cilja. Trebali biste planirati o stvaranju indeksiranje jedan za svaki cilj indeks i podataka kombinaciju izvora. Dok možete imati više indexers pisanja u isti indeks, indeksiranje možete pisati samo u jednom indeks. Indeksiranje se koristi za:

- Izvođenje jednokratnu kopiju podataka za popunjavanje indeksa.
- Sinkronizirajte indeksa promjenama u izvoru podataka na raspored. Raspored je dio definiciju indeksiranje.
- Pozivanje na zahtjev ažuriranja indeksa prema potrebi.

##<a id="CreateDataSource"></a>Korak 1: Stvaranje izvora podataka

Poslao zahtjev HTTP POST da biste stvorili novi izvor podataka u vašem Azure servisa za pretraživanje, uključujući sljedeće zaglavlja zahtjeva.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Na `api-version` potreban je. Valjane vrijednosti obuhvaćaju `2015-02-28` ili noviju verziju. Posjetite [API verzija u Azure pretraživanja](../search/search-api-versions.md) da biste vidjeli sve podržane verzije API pretraživanja.

U tijelu zahtjeva za sadrži definiciju izvora podataka koji moraju sadržavati sljedeća polja:

- **naziv**: Odaberite bilo koji naziv za predstavljanje DocumentDB baze podataka.

- **Vrsta**: korištenje `documentdb`.

- **vjerodajnice**:

    - **connectionString**: potrebna. Odredite informacije vezu s bazom podataka Azure DocumentDB u sljedećem obliku:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **spremnik**:

    - **naziv**: potrebna. Navedite id zbirke DocumentDB indeksirati.

    - **upit**: nije obavezno. Možete navesti upit stopi proizvoljne dokumenta za JSON u paušalni sheme pretraživanja Azure možete indeksirati.

- **dataChangeDetectionPolicy**: nije obavezno. Pročitajte članak [podataka promijenite Pravilnik za otkrivanje](#DataChangeDetectionPolicy) ispod.

- **dataDeletionDetectionPolicy**: nije obavezno. U odjeljku [pravila za otkrivanje brisanja podataka](#DataDeletionDetectionPolicy) ispod.

Potražite u nastavku [tijelo primjer zahtjev](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Dohvaćanje promijenjene dokumente

Svrha pravilo za otkrivanje promjena podataka je učinkovito prepoznavanje stavki promijenjene podatke. Trenutno je jedini podržani pravila u `High Water Mark` pomoću pravila u `_ts` svojstvo posljednji izmijenio vremenske oznake koje ste dobili od DocumentDB – koja je navedena na sljedeći način:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Također morat ćete dodati `_ts` u projekciju i `WHERE` uvjet upita. Ako, na primjer:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Dohvaćanje izbrisane dokumenata

Kada se reci se brišu iz izvorišne tablice, trebali biste izbrisati te retke iz indeks pretraživanja. Svrha pravilo za otkrivanje brisanja podataka je učinkovito prepoznavanje stavke izbrisane podatke. Trenutno je jedini podržani pravila u `Soft Delete` pravila (brisanja je označena zastavicom neke sortiranja), koji je naveden na sljedeći način:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Morat ćete uključiti svojstvo softDeleteColumnName u uvjetu SELECT ako koristite prilagođene projekcije.

###<a id="CreateDataSourceExample"></a>Primjer tijelu zahtjeva

Sljedeći primjer stvara izvora podataka pomoću prilagođenog Savjeti za upit i pravila:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Odgovor

Ako izvor podataka uspješno je stvorena primit će je stvorio 201 HTTP odgovor.

##<a id="CreateIndex"></a>Korak 2: Stvaranje indeksa

Ako još nemate jedno mjesto, stvorite indeks pretraživanja Azure cilj. To možete učiniti s [Korisničkog Sučelja Portal Azure](../search/search-create-index-portal.md) ili pomoću [API za stvaranje indeksa](https://msdn.microsoft.com/library/azure/dn798941.aspx).

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Provjerite je li u shemi indeksa ciljne kompatibilna s sheme izvorni dokumenti JSON ili izlaz na projekciji prilagođeni upit.

>[AZURE.NOTE] Za particioniranom zbirke tipku dokument zadani je DocumentDB na `_rid` svojstvo koje dohvaća preimenovana `rid` u pretraživanju Azure. Osim toga, DocumentDB, `_rid` vrijednosti sadrži znakove koji nisu valjani u pretraživanje Azure ključevima; Zbog toga u `_rid` Base64 kodirani su vrijednosti.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Slika A: mapiranja između JSON vrste podataka i vrste podataka Azure pretraživanja

| VRSTA PODATAKA JSON|   KOMPATIBILNI ODREDIŠNE INDEKSA POLJA VRSTE|
|---|---|
|Booleovom|Edm.Boolean, Edm.String|
|Brojevi koji izgledaju cijelim brojevima|Edm.Int32, Edm.Int64, Edm.String|
|Brojevi od tog izgleda kao pluta točke|Edm.Double, Edm.String|
|Niz|Edm.String|
|Polja prim vrste npr "a", "b"; "c" |Collection(EDM.String)|
|Nizovi koji izgleda kao i kod datuma| Edm.DateTimeOffset, Edm.String|
|GeoJSON objekti npr {"Vrsta": "Mjesto", "koordinate": [dugo, lat]} | Edm.GeographyPoint |
|Ostale objekte JSON|N/D|

###<a id="CreateIndexExample"></a>Primjer tijelu zahtjeva

Sljedeći primjer stvara indeks s poljem id i opis:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Odgovor

Primit ćete je stvorio 201 HTTP odgovor ako uspješno stvoren indeks.

##<a id="CreateIndexer"></a>Korak 3: Stvaranje indeksiranje

Možete stvoriti novi indeksiranje unutar servis za pretraživanje Azure pomoću zahtjev HTTP POST sljedeće zaglavlja.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

U tijelu zahtjeva za sadrži definiciju indeksiranje koji moraju sadržavati sljedeća polja:

- **naziv**: potrebna. Naziv indeksiranje.

- **dataSourceName**: potrebna. Naziv postojećeg izvora podataka.

- **targetIndexName**: potrebna. Naziv postojeći indeks.

- **raspored**: nije obavezno. Potražite u članku [Indeksiranje raspored](#IndexingSchedule) ispod.

###<a id="IndexingSchedule"></a>Pokretanje indexers prema rasporedu

Indeksiranje po želji možete odrediti raspored. Ako postoji raspored indeksiranje će se pokrenuti povremeno po raspored. Raspored sadrži sljedećim atributima:

- **interval**: potrebna. Pokreće se trajanje vrijednost koja određuje interval ili razdoblje, za indeksiranje. Najmanji dopušteni interval je pet minuta; na najdulje je jedan dan. Moraju biti oblikovani kao vrijednost "dayTimeDuration" XSD (ograničeni podskup vrijednost [ISO 8601 trajanje](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Obrazac za to je: `P(nD)(T(nH)(nM))`. Primjeri: `PT15M` svakih 15 minuta `PT2H` za svaku dva sata.

- **startTime**: potrebna. UTC datuma i vremena koja određuje kada bi trebao počinjati indeksiranje izvodi.

###<a id="CreateIndexerExample"></a>Primjer tijelu zahtjeva

Sljedeći primjer stvara indeksiranje koja se kopira podatke iz zbirke upućuje na `myDocDbDataSource` izvora podataka da biste na `mySearchIndex` indeksa na raspored koji započinje Sij 1, 2015 UTC-a i pokrenulo zaračunava.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Odgovor

Primit ćete je stvorio 201 HTTP odgovor ako indeksiranje uspješno je stvorena.

##<a id="RunIndexer"></a>Korak 4: Pokretanje indeksiranje

Uz pokrenut povremeno na raspored, indeksiranje mogu također se pozvati na zahtjev slanjem sljedeći HTTP POST zahtjev:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Odgovor

Primit ćete je prihvatio 202 HTTP odgovor ako indeksiranje uspješno pozvan.

##<a name="GetIndexerStatus"></a>Korak 5: Dohvatite status indeksiranje

Zahtjev HTTP GET dohvatiti trenutni povijest statusa i izvođenja indeksiranje možete problema:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Odgovor

Prikazat će se u redu 200 HTTP odgovor vraća uz tijelo odgovor koji sadrži informacije o ukupnog statusa stanja indeksiranje, zadnjeg poziva za indeksiranje, kao i povijest nedavne instanci komponente za indeksiranje (ako postoje).

Odgovor trebao bi izgledati otprilike ovako:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Izvršavanje povijest sadrži najviše na 50 najnovije dovršene izvršavanja, koje se sortiraju obrnutim kronološkim redoslijedom (da bi najnoviji izvođenja dolazi prvo u odgovoru).

##<a name="NextSteps"></a>Daljnji koraci

Čestitamo! Upravo ste naučili kako integrirati Azure DocumentDB pomoću pretraživanja Azure pomoću indeksiranje za DocumentDB.

 - Da biste saznali kako više Azure DocumentDB, posjetite [stranicu servisa DocumentDB](https://azure.microsoft.com/services/documentdb/).

 - Da biste saznali kako se informacije o pretraživanje Azure, posjetite [stranice servisa za pretraživanje](https://azure.microsoft.com/services/search/).
