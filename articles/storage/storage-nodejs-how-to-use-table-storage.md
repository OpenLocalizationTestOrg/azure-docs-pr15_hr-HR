<properties
    pageTitle="Upute za korištenje spremišta tablica Azure s Node.js | Microsoft Azure"
    description="Pohranite strukturiranih podataka u oblak pomoću tablice Azure prostor za pohranu, NoSQL izvor podataka."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Upute za korištenje spremišta tablica Azure s Node.js

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Pregled

U ovoj se temi objašnjava izvođenje uobičajeni scenariji pomoću servisa Azure tablica u aplikaciji Node.js.

Primjeri kod u ovoj temi pretpostavlja da već imate Node.js aplikacije. Informacije o stvaranju Node.js aplikacije u Azure potražite u članku o ovim temama:

- [Stvaranje web-aplikacijama Node.js u aplikacije servisa za Azure](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Stvaranje i implementacija web-aplikacijama Node.js Azure pomoću WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (pomoću komponente Windows PowerShell)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Konfiguriranje aplikacija za pristup Azure prostora za pohranu

Da biste koristili Azure prostora za pohranu, morate SDK za pohranu Azure Node.js koje sadrži skup praktičnost biblioteke koje komunikaciju OSTALE servise za pohranu.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Korištenje upravitelja čvor paket (NPM) da biste instalirali paket

1.  Korištenje naredbenog retka sučelja kao što je **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix), a dođite do mape u koju ste stvorili aplikacije.

2.  U prozoru naredba vrstu **npm instalacija azure prostora za pohranu** . Izlaz iz naredbe je slično kao u sljedećem primjeru.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Možete ručno pokrenuti naredbu **ls** da biste potvrdili da je **čvor\_modula** mapu stvorena. U toj mapi tražit će paket **azure prostora za pohranu** koji sadrži biblioteke morate pristupiti prostora za pohranu.

### <a name="import-the-package"></a>Uvoz paketa

Na vrh **server.js** datoteke u aplikaciji, dodajte sljedeći kod:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Postavljanje veze sustava Azure prostora za pohranu

Modul azure će čitati varijable okruženja AZURE\_prostora za POHRANU\_RAČUN i AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ ili AZURE\_prostora za POHRANU\_veze\_NIZ za podatke potrebne za povezivanje s računom Azure prostora za pohranu. Ako te varijable okruženja su postavljena, morate navesti informacije o računu prilikom pozivanja **TableService**.

Primjer [Azure Portal](https://portal.azure.com) za web-mjesto programa Azure postavljanje varijable okruženja, potražite u članku [Node.js web-aplikacije pomoću servisa Azure tablice].

## <a name="create-a-table"></a>Stvaranje tablice

Sljedeći kod stvara **TableService** objekt, a koji se koristi za stvaranje nove tablice. Dodajte sljedeće pri vrhu **server.js**.

    var tableSvc = azure.createTableService();

Poziv na **createTableIfNotExists** će stvoriti novu tablicu s navedenim nazivom ako se još ne postoji. Sljedeći primjer stvara novu tablicu pod nazivom "MojaTablica" Ako je već postoji:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

Na `result.created` bit će `true` ako je stvorili novu tablicu, a `false` ako već postoji u tablici. Na `response` će sadržavati informacije o zahtjevu.

### <a name="filters"></a>Filtri

Neobavezni operacije filtriranja primjenjuje se na razne operacije obavljene pomoću **TableService**. Filtriranje operacije možete uključiti zapisivanje, automatski ponovni itd. Filtri su objekti koji implementira metode s potpisom:

    function handle (requestOptions, next)

Nakon toga njegov pretprocesnih na izborniku Mogućnosti zahtjev, metodu mora poziva "sljedeće", prosljeđivanje povratnog potpisom sljedeće:

    function (returnObject, finalCallback, next)

U ovom povratnog i nakon obrade returnObject (odgovor na zahtjev na poslužitelj) povratnog treba pozivanje Dalje ako postoji li nastaviti obradu ostalih filtara ili jednostavno inače pozivanje finalCallback da biste završili poziv servisa.

Dva filtra implementirati pokušaj logike su Azure SDK za Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. **TableService** objekta koji koristi **ExponentialRetryPolicyFilter**stvara:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Dodavanje entitet u tablicu

Da biste dodali entitet, stvorite objekt koji definira entitet svojstva. Svi entiteti mora sadržavati **PartitionKey** i **RowKey**koje su odredišnih stilova za entitet.

* **PartitionKey** - određuje particija entitet pohranjena u

* **RowKey** - služi kao jedinstvena identifikacija entitet unutar particija

**PartitionKey** i **RowKey** mora biti vrijednosti niza. Dodatne informacije potražite u članku [Osnove podatkovnog modela servisa u tablici](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Slijedi primjer definiranje entitet. Imajte na umu **Datum krajnjeg roka** definirana je kao vrstu **Edm.DateTime**. Određivanje vrsta nije obavezan, a vrste će biti nenamjerna ako nisu navedeni.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Postoji i **vremenske oznake** polja za svaki zapis koji je po Azure kada je entitet umetnuti ili ažurirati.

Da biste stvorili entiteti možete koristiti i **entityGenerator** . Sljedeći primjer stvara isti entitet zadataka pomoću **entityGenerator**.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Da biste tablicu dodali entitet, prenesite objekt entitet metodu **insertEntity** .

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Ako postupak ne uspije, `result` će sadržavati [e-oznake](http://en.wikipedia.org/wiki/HTTP_ETag) umetnute zapisa i `response` će sadržavati informacije o postupak.

Primjer odgovor:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Prema zadanim postavkama **insertEntity** ne vraća umetnute entitet kao dio sustava `response` informacije. Ako planirate na izvođenje ostalih operacija na ovaj entitet ili želite predmemoriju podatke, može biti korisno da bi se vraćaju kao dio sustava `result`. To možete učiniti tako da omogućite **echoContent** na sljedeći način:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Ažuriranje entitet

Postoje dostupna da biste ažurirali postojeći entitet više načina:

* **replaceEntity** - ažurira postojeći entitet jer je zamjenjuju

* **mergeEntity** - ažurira postojeći entitet spajanjem nove vrijednosti nekretnina u postojeći entitet

* **insertOrReplaceEntity** - ažurira postojeći entitet zamjenjujući ga. Ako postoji bez entitet, umetnut će se novi

* **insertOrMergeEntity** - ažurira postojeći entitet spajanjem nove vrijednosti nekretnina u postojeći. Ako postoji bez entitet, umetnut će se novi

Sljedeći primjer prikazuje ažuriranje entitet **replaceEntity**:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Prema zadanim postavkama, ažuriranje entitet ne provjerite ako podataka ažuriraju prethodno izmijenjena nekim drugim procesom. Za podršku Istodobni ažuriranja:
>
> 1. Dobiti e-oznake objekta ažurira. Time se vraća kao dio sustava `response` za svaki entitet povezane operacije, a mogu se dohvaćati putem `response['.metadata'].etag`.
>
> 2. Prilikom izvršavanja operacije ažuriranja za entitet, dodajte prethodno dohvaćeni novi entitet podaci o e-oznake. Ako, na primjer:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Izvođenje operacije ažuriranja. Ako je entitet izmijenjena koju ste vratili vrijednost e-oznake podataka, kao što su druga instanca aplikacije, na `error` vratit će se koja govori da uvjet ažuriranje naveden u zahtjevu nisu zadovoljeni.

S **replaceEntity** i **mergeEntity**, ako ne postoji entitet koji ažurira, zatim postupak ažuriranja neće uspjeti. Stoga ako želite pohraniti entitet bez obzira li ga već postoji, koristite **insertOrReplaceEntity** ili **insertOrMergeEntity**.

Na `result` za uspješno ažuriranje operacije će sadržavati **e-oznake** ažurirane entitet.

## <a name="work-with-groups-of-entities"></a>Rad s grupama entiteti

Ponekad je li bolje slanje više operacije zajedno u grupe da biste bili sigurni atomske obrade na poslužitelju. Da biste izvršili koji, pomoću klase **TableBatch** da biste stvorili grupu, a pomoću način **executeBatch** **TableService** izvođenje odbacivanja operacija.

 Sljedeći primjer prikazuje dva entiteti u grupu za slanje:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Za uspješan grupe operacija, `result` sadržavat će podatke za svaku operaciju u grupu.

### <a name="work-with-batched-operations"></a>Rad s odbacivanja operacije

Operacije dodali u grupu možete potražiti tako da prikaz u `operations` svojstvo. Da biste radili s operacije možete koristiti i na sljedeće načine:

* **poništite** - čisti sve operacije iz serije

* **getOperations** - dohvaća postupak iz serije

* **hasOperations** - vraća true ako skupine sadrži operacije

* **removeOperations** - uklanja operacije

* **Veličina** - vraća broj operacije u grupu

## <a name="retrieve-an-entity-by-key"></a>Dohvaćanje entitet tipke

Da biste se vratili na određene entitet, ovisno o **PartitionKey** i **RowKey**pomoću metode **retrieveEntity** .

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Nakon dovršetka ovaj postupak, `result` će sadržavati entitet.

## <a name="query-a-set-of-entities"></a>Upit skupa entiteti

Upit tablice, koristite objekt **TableQuery** izgraditi izrazu upita pomoću sljedeće uvjete:

* **Odaberite** - polja koja se vraća iz upita

* **gdje** - gdje uvjet

    * **i** - programa `and` Uvjet gdje

    * **ili** - programa `or` Uvjet gdje

* **Vrh** – broj stavki za dohvaćanje


Sljedeći primjer stvara upit koji će vratiti gornji pet stavke s na PartitionKey 'hometasks'.

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Budući da **Odaberite** ne koristi, vratit će se sva polja. Da biste izvršili upita na temelju tablice, koristite **queryEntities**. Sljedeći primjer koristi ovaj upit da biste se vratili entiteti iz 'MojaTablica'.

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Ako ne uspije, `result.entries` će sadržavati polja entiteti koji odgovaraju upitu. Ako je upit ne mogu vratiti sve entiteti `result.continuationToken` će biti koje nisu*null* i može se koristiti kao parametar treći **queryEntities** za dohvaćanje više rezultata. Da biste postigli početni upit za parametar treći koristite *null* .

### <a name="query-a-subset-of-entity-properties"></a>Podskup entitet svojstva za upite

Upit u tablicu možete dohvatiti samo nekoliko polja iz entitet.
To se smanjuje propusnost i poboljšati performanse upita, posebice za velike entiteti. Korištenje **Odaberite** uvjet i prenesite naziva polja koja se vraća. Na primjer, sljedeći upit će vratiti samo polja **Opis** i **Datum krajnjeg roka** .

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Brisanje entitet

Možete izbrisati entitet pomoću tipki njegov particija i redaka. U ovom primjeru objekt **task1** sadrži vrijednosti **RowKey** i **PartitionKey** entitet će se izbrisati. Zatim objekt prosljeđuje **deleteEntity** korakom.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Razmislite o korištenju ETags prilikom brisanja stavki, da biste bili sigurni da stavke nije promijenio drugi proces. Potražite u članku [Ažuriranje entitet](#update-an-entity) informacije o korištenju ETags.

## <a name="delete-a-table"></a>Brisanje tablice

Sljedeći kod briše tablicu s računom za pohranu.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Ako niste sigurni je li tablica postoji, koristite **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Korištenje nastavka tokena

Kada dohvaćate tablice za velike količine rezultate, potražite tokeni nastavka. Možda su dostupni za upit možda ne znaju ako sastavljanje prepoznaje kada postoji nastavka token velike količine podataka.

Rezultati objekt vraća tijekom ispitivanje entiteti skupove na `continuationToken` svojstvo kada postoji takvo token. Možete koristiti to prilikom izvršavanja upita da biste nastavili s pomičite po entiteti particija i tablice.

Prilikom postavljanja upita, parametar continuationToken bi biti omogućen između instancu objekta upita i funkciju povratnog:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Ako pregledavate u `continuationToken` objekt, vidjet ćete svojstva kao što su `nextPartitionKey`, `nextRowKey` i `targetLocation`, koji se koristi kao iteracija kroz sve rezultate.

Postoji uzorak nastavka unutar repo Node.js Azure prostora za pohranu na GitHub. Potražite `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Rad s zajednički pristup potpisa

Zajednički pristup potpisa (SAS) su siguran način za zrnastog pristup tablice bez osiguravanja naziv računa za pohranu ili tipke. SAS često se koristi za ograničeni pristup podataka, kao što su dopuštanja mobilne aplikacije sa zapisima upita.

Pouzdani aplikacije kao što je servis u oblaku generira SAS pomoću **generateSharedAccessSignature** **TableService**te je prenosi nepouzdanih ili djelomično pouzdanih aplikacija kao što su mobilne aplikacije. Na SAS generira pomoću pravila koja opisuje tijekom kojeg je valjan na SAS datume početka i završetka, baš kao razinu pristupa dodijeljene držača SAS.

Sljedeći primjer generira novog pravilnika zajednički pristup omogućit će držača SAS upit ("r") u tablici, a istekne 100 minuta nakon vremena stvaranja.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Imajte na umu da informacije glavno računalo mora se navesti također, kao što je potreban je pri držača SAS pokušava pristupiti u tablici.

Klijentska aplikacija pa se koristi u SAS sa **TableServiceWithSAS** za izvođenje operacije na tablici. U sljedećem primjeru povezuje s tablicom i izvodi upit.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Budući da u SAS je generirao samo upita programa access, ako su pokušaj Umetanje, ažuriranje i brisanje entiteti, će se vraća pogreška.

### <a name="access-control-lists"></a>Popisi za upravljanje

Da biste postavili pravilnik pristupa za SAS možete koristiti i popis kontrole pristupa (ACL). To je korisno ako želite da bi više klijenti mogli pristupiti u tablici, no nudi drugačiji pristup pravila za svaki klijent.

ACL je implementirati pomoću polja pravila za pristup s ID-om pridružen svakom pravila. U sljedećem primjeru definira dva pravila, jednu za 'Korisnik1', a jedan za 'korisnika2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Sljedeći primjer dobiva trenutni ACL **hometasks** tablice, a zatim zbrojiti nova pravila pomoću **setTableAcl**. Taj se način omogućuje sljedeće:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Nakon postavljanja ACL-a zatim možete stvoriti SAS na temelju ID za pravilo. Sljedeći primjer stvara novi SAS za 'korisnika2':

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u sljedećim člancima.

-   [Blog tima za azure prostora za pohranu][].
-   [Azure SDK za pohranu za čvor][] spremište na GitHub.
-   [Razvojni centar za Node.js](/develop/nodejs/)

  [Azure prostora za pohranu SDK za čvor]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js web-aplikacije pomoću servisa Azure tablice]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
