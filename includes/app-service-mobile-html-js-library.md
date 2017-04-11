##<a name="create-client"></a>Stvaranje veze za klijenta

Stvorite vezu klijent stvaranjem na `WindowsAzure.MobileServiceClient` objekt.  Zamjena `appUrl` URL-om za aplikacije za mobilne uređaje.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Rad s tablicama

Access ili ažuriranje podataka, stvaranje reference na tablici pozadinskog. Zamjena `tableName` s nazivom tablice

```
var table = client.getTable(tableName);
```

Nakon što dodate referenci tablice, možete raditi daljnje s tablice:

* [Upit u tablicu](#querying)
  * [Filtriranje podataka](#table-filter)
  * [Kretanje podataka stranicama](#table-paging)
  * [Sortiranje podataka](#sorting-data)
* [Umetanje podataka](#inserting)
* [Mijenjati podatke](#modifying)
* [Brisanje podataka](#deleting)

###<a name="querying"></a>Kako: upit referenci tablice

Nakon što dodate referenci tablice, možete ga koristiti za dohvaćanje podataka na poslužitelju.  Upiti vrše na jeziku "LINQ nalik".
Da biste vratili svih podataka iz tablice, koristite sljedeće:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Funkcija uspjeh naziva s rezultatima.   Nemojte koristiti `for (var i in results)` u za uspjeh funkcionirati kao koji će ponavljanje informacije koje je uvršten u rezultate kada druge funkcije upita (kao što su `.includeTotalCount()`) koriste.

Dodatne informacije o sintaksu upita, pogledajte [upita objekt dokumentaciju].

####<a name="table-filter"></a>Filtriranje podataka na poslužitelju

Možete koristiti na `where` uvjet u referenci tablice:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Možete koristiti i funkcije koji filtrira objekt.  U ovom slučaju na `this` varijabla vam je dodijeljen trenutni objekt filtrira.  Ovo je functionally jednako prethodnog primjera:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Kretanje podataka stranicama

Korištenje načina take() i skip().  Na primjer, ako želite podijeliti tablicu na zapise 100 redak:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Na `.includeTotalCount()` da biste dodali polje totalCount rezultate objekt koji se koristi način.  Polje totalCount se ispunjava ukupan broj zapisa koju želite vratiti ako se koristi bez broja stranica.

Možete koristiti varijablu stranice i neke UI gumba stranica popis; pomoću loadPage() učitavanje nove zapise za svaku stranicu.  Trebali biste provesti neke sortiranje predmemoriranje brzinu pristup zapisima koji već učitan.


####<a name="sorting-data"></a>Kako: vratiti podatke koji su sortirani

Korištenje upita metode .orderBy() ili .orderByDescending():

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Dodatne informacije o objekt upita, pogledajte [upita objekt dokumentaciju].

###<a name="inserting"></a>Kako: Umetanje radnog lista

Stvaranje objekta JavaScript s datumom odgovarajuće i asinkrono poziva table.insert():

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Na uspješno umetanja umetnute stavke, vraća se s dodatna polja koji su potrebni za sinkronizaciju operacije.  Trebali biste ažurirati vlastiti predmemorije s ove informacije o kasnija ažuriranja.

Imajte na umu SDK poslužitelj za Azure mobilne aplikacije Node.js podržava dinamičke sheme svrhe razvoj.
Slučaju dinamički sheme shemi tablice ažurira hodu, što omogućuje dodavanje stupaca u tablici samo zadavanjem ih u umetnut ili ažurirati operacija.  Preporučujemo da isključe dinamički sheme prije prelaska aplikacije da biste radni.

###<a name="modifying"></a>Kako: izmjenu podataka

Slično metodu .insert(), trebali biste stvorili objekt ažuriranja i poziva .update().  Ažuriranje objekt mora sadržavati ID zapisa za ažuriranje – to je dobiven prilikom čitanja zapis ili prilikom pozivanja .insert().

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Kako: brisanje podataka

Nazovite .del() način da biste izbrisali zapis.  Prenesite ID referenca objekta:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
