<properties
    pageTitle="Upute za korištenje spremišta blobova iz Node.js | Microsoft Azure"
    description="Pohranite nestrukturirane podatke u oblaku s Azure blobova (spremište objekta)."
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



# <a name="how-to-use-blob-storage-from-nodejs"></a>Upute za korištenje spremišta blobova iz Node.js

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

U ovom se članku objašnjava da biste izvršili uobičajeni scenariji pomoću spremište blobova platforme. Primjere zapisuju putem Node.js API-JA. Scenariji u kojima je moguće uvrstiti kako prenijeti, popisa, preuzimanje i brisanje blob-ova.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Stvaranje aplikacije Node.js

Upute za stvaranje Node.js aplikacije, potražite u članku [Stvaranje web-aplikacijama Node.js u aplikacije servisa za Azure], [Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure] – pomoću komponente Windows PowerShell ili [stvorite i implementirajte Node.js web-aplikacijama za Azure pomoću matrice Web].

## <a name="configure-your-application-to-access-storage"></a>Konfiguriranje aplikacija za pristup za pohranu

Da biste koristili Azure prostora za pohranu, morate SDK za pohranu Azure Node.js koje sadrži skup praktičnost biblioteke koje komunikaciju OSTALE servise za pohranu.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Korištenje upravitelja čvor paket (NPM) da biste dobili pakiranje

1.  Dođite do mape u kojem ste stvorili primjer aplikacije pomoću sučelja naredbenog retka kao što je **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix).

2.  U prozoru naredba vrstu **npm instalacija azure prostora za pohranu** . Izlaz iz naredbe je slično kao u sljedećem primjeru kod.

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

3.  Možete ručno pokrenuti naredbu **ls** da biste potvrdili da je **čvor\_modula** mapu stvorena. U toj mapi pronađite **spremištem azure** paket koji sadrži biblioteke koje su vam potrebne da biste pristupili prostora za pohranu.

### <a name="import-the-package"></a>Uvoz paketa

Na vrh datoteke **server.js** aplikacije koju namjeravate koristiti za pohranu koristite Blok za pisanje ili neki drugi uređivač teksta, dodajte sljedeće:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Postavljanje veze sustava Azure prostora za pohranu

Modul Azure će čitati varijable okruženja `AZURE_STORAGE_ACCOUNT` i `AZURE_STORAGE_ACCESS_KEY`, ili `AZURE_STORAGE_CONNECTION_STRING`, za podatke potrebne za povezivanje s računom Azure prostora za pohranu. Ako te varijable okruženja su postavljena, morate navesti informacije o računu prilikom pozivanja **createBlobService**.

Primjer postavljanje varijable okruženja [Azure Portal](https://portal.azure.com) za aplikacije Azure web, potražite u članku [Node.js web-aplikacije pomoću servisa Azure tablice].

## <a name="create-a-container"></a>Stvaranje spremnika

Objekt **BlobService** omogućuje rad s spremnika i blob-ova. Sljedeći kod stvara **BlobService** objekt. Dodajte sljedeće pri vrhu **server.js**:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Pristupite blob anonimno pomoću **createBlobServiceAnonymous** i pružanje adresa glavnog računala. Ako je, primjerice, koristite `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Da biste stvorili novi spremnik, koristite **createContainerIfNotExists**. Sljedeći primjer koda stvara novi spremnik pod nazivom "mycontainer":

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Ako spremnik upravo stvorili, `result.created` vrijedi. Ako već postoji spremnik, `result.created` je vrijednost false. `response`sadrži informacije o operacija, uključujući informacije o e-oznake za spremnik.

### <a name="container-security"></a>Spremnik sigurnosti

Prema zadanim postavkama novog spremnika privatnih i nije moguće anonimno. Da biste spremnik javno tako da anonimno pristupiti, možete postaviti razinu pristupa kontejner s **blob** ili **spremnik**.

* **blob** - omogućuje anonimni pristup za blob sadržaja i metapodataka u ovom spremniku, ali ne spremnik metapodataka kao što je popis svih blob-ova u spremniku za čitanje

* **spremnik** - omogućuje anonimni pristup čitanje blob sadržaja i metapodacima kao i spremnik metapodataka

Sljedeći primjer koda pokazuje postavljanje razina pristupa na **blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Osim toga, možete izmijeniti razinu pristupa spremnika pomoću **setContainerAcl** da biste odredili razinu pristupa. Sljedeći primjer koda Promijeni razinu pristupa spremniku:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Rezultat sadrži informacije o operacija, uključujući trenutni **e-oznake** za spremnik.

### <a name="filters"></a>Filtri

Neobavezni operacije filtriranja možete primijeniti razne operacije obavljene pomoću **BlobService**. Filtriranje operacije možete uključiti zapisivanje, automatski ponovni itd. Filtri su objekti koji implementira metode s potpisom:

    function handle (requestOptions, next)

Nakon toga njegov pretprocesnih na izborniku Mogućnosti zahtjev, metodu mora poziva "sljedeće", prosljeđivanje povratnog potpisom sljedeće:

    function (returnObject, finalCallback, next)

U ovom povratnog i nakon obrade returnObject (odgovor na zahtjev na poslužitelj) povratnog mora pozivanje Dalje ako postoji li nastaviti obradu ostalih filtara ili jednostavno pozvati finalCallback da biste završili poziv servisa.

Dva filtra implementirati pokušaj logike su Azure SDK za Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. **BlobService** objekta koji koristi **ExponentialRetryPolicyFilter**stvara:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Postoje tri vrste blob-ova: blokirati blob-ova, stranica blob-ova i dodavanje blob-ova. Blokiranje blob-ova omogućuju da biste učinkovito prenesete velike podataka. Dodavanje blob-ova su optimizirani za dodavanje operacije. Blob-Ova stranica su optimizirani za čitanje/pisanje operacije. Dodatne informacije potražite u članku [objašnjenje bloka blob-ova, dodavanje blob-ova, i blob-Ova stranica](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blokiranje blob-ova

Da biste prenijeli podatke u blok blob, koristite sljedeće:

* **createBlockBlobFromLocalFile** - stvara novi blok blob i prenosi sadržaj datoteke

* **createBlockBlobFromStream** - stvara novi blok blob i prenosi sadržaj strujanje

* **createBlockBlobFromText** - stvara novi blok blob i prenosi sadržaj niza

* **createWriteStreamToBlockBlob** – omogućuje tok zapisivanja u blok blob

Sljedeći primjer koda prenosi sadržaj datoteke **test.txt** u **myblob**.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Na `result` vratio ovih metoda sadrži informacije o operaciji, kao što su **e-oznake** blob-om.

### <a name="append-blobs"></a>Dodavanje blob-ova

Da biste prenijeli podatke u novu blob dodavanjem, koristite sljedeće:

* **createAppendBlobFromLocalFile** - stvara novi upit s dodavanjem blob i prenosi sadržaj datoteke

* **createAppendBlobFromStream** - stvara novi upit s dodavanjem blob i prenosi sadržaj strujanje

* **createAppendBlobFromText** - stvara novi upit s dodavanjem blob i prenosi sadržaj niza

* **createWriteStreamToNewAppendBlob** - stvara novi upit s dodavanjem blob i omogućuje strujanje zapisivanja u njega

Sljedeći primjer koda prenosi sadržaj datoteke **test.txt** u **myappendblob**.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Da biste dodali blok postojeće dodavanjem blobova platforme, koristite sljedeće:

* **appendFromLocalFile** – dodavanje sadržaj datoteke u postojeći upit s dodavanjem blobova platforme

* **appendFromStream** - dodavati sadržaj strujanje postojeće dodavanjem blobova platforme

* **appendFromText** - dodavati sadržaj niza postojeće dodavanjem blobova platforme

* **appendBlockFromStream** - dodavati sadržaj strujanje postojeće dodavanjem blobova platforme

* **appendBlockFromText** - dodavati sadržaj niza postojeće dodavanjem blobova platforme

> [AZURE.NOTE] appendFromXXX API-ji neće imati neke provjere valjanosti klijentsko uvoza brzo da biste izbjegli unncessary poslužitelja poziv. appendBlockFromXXX neće.

Sljedeći primjer koda prenosi sadržaj datoteke **test.txt** u **myappendblob**.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Blob-Ova stranica

Da biste prenijeli podataka na stranici blob, koristite sljedeće:

* **createPageBlob** - stvara novu stranicu blob određene duljine

* **createPageBlobFromLocalFile** - stvara novu stranicu blob i prenosi sadržaj datoteke

* **createPageBlobFromStream** - stvara novu stranicu blob i prenosi sadržaj strujanje

* **createWriteStreamToExistingPageBlob** - nudi tok zapisivanja za postojeće stranice blobova platforme

* **createWriteStreamToNewPageBlob** - stvara novu stranicu blob i omogućuje strujanje zapisivanja u njega

Sljedeći primjer koda prenosi sadržaj datoteke **test.txt** u **mypageblob**.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Blob-Ova stranica se sastojati od 512 bajta "stranice". Primit ćete pogrešku kada prijenos podataka s veličinom koja nije višekratnik od 512.

## <a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku

Da biste dobili popis blob-ova u spremniku pomoću metode **listBlobsSegmented** . Ako želite da biste se vratili blob polja s određenim prefiks, koristite **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

U `result` sadrži programa `entries` zbirke koja je polje objekata koji opisuje svaku blob. Ako nije moguće vratiti sve blob-ova, u `result` se nalaze na `continuationToken`, koje možete koristiti kao drugi parametar za dohvaćanje dodatne unose.

## <a name="download-blobs"></a>Preuzimanje blob-ova

Preuzimanje podataka s blob, koristite sljedeće:

* **getBlobToLocalFile** - zapisuje sadržaj blob u datoteku

* **getBlobToStream** - zapisuje sadržaj blob strujanje

* **getBlobToText** - zapisuje sadržaj blob niza

* **createReadStream** - nudi strujanje za čitanje s blob-om

Sljedeći primjer koda pokazuje se korištenje **getBlobToStream** da biste preuzeli sadržaj **myblob** blob i spremanje datoteka **output.txt** pomoću strujanje:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

U `result` sadrži podatke o blob-om, uključujući informacije o **e-oznake** .

## <a name="delete-a-blob"></a>Brisanje blob

Na kraju, da biste izbrisali blob, nazovite **deleteBlob**. Sljedeći primjer koda briše blob pod nazivom **myblob**.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Istodobni programa access

Da biste podrška Istodobni pristup da biste blob iz više klijenata ili više instanci procesa, možete koristiti **ETags** ili **leases**.

* **E-oznake** - omogućuje da biste otkrili koji blob-om ili spremnik izmijenjena neki drugi proces

* **Zakup** - omogućuje nabaviti isključujući te dvije vrijednosti, moguće, pisanje ili brisanje pristup blob za određeno vremensko razdoblje

### <a name="etag"></a>E-oznake

Korištenje ETags Ako morate dopustiti više klijenata ili instance pisati u blok Blob ili stranicu bloba istodobno. U e-oznake vam omogućuje da utvrdite ako spremnik ili blob izmijenjena od prethodno čitati ili je stvoren, što vam omogućuje da ne bi prebrisali promjene izvršene drugi klijent ili postupak.

Možete postaviti e-oznake uvjeta pomoću neobavezna `options.accessConditions` parametar. Sljedeći primjer koda samo prenosi **test.txt** datoteke ako već postoji blob-om, a zatim je jedna vrijednost nalaze `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Kada koristite ETags Općenito uzorak je:

1. Nabavite u e-oznake kao rezultat Stvori, popisa ili get operacija.

2. Izvođenje akcije, provjera da jedna vrijednost nisu mijenjani.

Ako je vrijednost izmjene, to znači da drugi klijent ili instancu izmijenio blob ili spremnik jer ste nabavili jedna vrijednost.

### <a name="lease"></a>Zakup

Možete dobiti novi Zakup metodom **acquireLease** , navođenje blob ili spremnika u koji želite dobiti u Zakup na. Na primjer, sljedeći kod nabaviti Zakup na **myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Morate navesti sljedeće operacije na **myblob** na `options.leaseId` parametar. Zakup ID se vraćaju u obliku `result.id` iz **acquireLease**.

> [AZURE.NOTE] Prema zadanim postavkama Zakup traje beskonačno. Možete odrediti koje nisu beskonačno trajanje (od 15 do 60 sekundi) unosom na `options.leaseDuration` parametar.

Da biste uklonili u Zakup, koristite **releaseLease**. Da biste je prekinuti u Zakup, ali biste drugima onemogućili stjecanje novi Zakup dok izvorne trajanje istekla, slijedite **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Rad s zajednički pristup potpisi

Zajednički pristup potpisa (SAS) su siguran način za zrnastog pristup blob-ova i spremnika bez osiguravanja naziv računa za pohranu ili tipke. Zajednički pristup potpisi često se koristi za ograničeni pristup podataka, kao što su dopuštanje mobilne aplikacije da biste pristupili blob-ova.

> [AZURE.NOTE] Dok možete omogućiti anonimni pristup blob-ova, zajednički pristup potpisi omogućuju vam pružaju više kontrolirati pristup, kao što je potrebno generiranje na SAS.

Pouzdani aplikacije kao što je servis u oblaku generira zajednički pristup potpisa koristeći **generateSharedAccessSignature** **BlobService**te je prenosi nepouzdanih ili djelomično pouzdanih aplikacija kao što su mobilne aplikacije. Zajedničko korištenje programa access potpisi generiraju putem pravila koja opisuje početka i završnog datuma tijekom kojeg su valjane potpise zajednički pristup, kao i razinu pristupa dodijelio držača zajednički pristup potpisi.

Sljedeći primjer koda stvara novi zajednički pristup pravila koja omogućuje držača potpisa zajednički pristup za izvođenje operacije čitanja na **myblob** blob i istekne 100 minuta nakon vremena stvaranja.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Imajte na umu da informacije glavno računalo mora se navesti također, kao što je potreban je pri držača potpisa zajednički pristup pokušava pristupiti spremnik.

Klijentska aplikacija pa koristi zajednički pristup potpisa s **BlobServiceWithSAS** za izvođenje operacija na temelju blob-om. Informacije o **myblob**dohvaća.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Budući da potpisi zajednički pristup pojavile su imaju pristup samo za čitanje ako pokušaju da biste izmijenili blob-om, će se vraća pogreška.

### <a name="access-control-lists"></a>Popisi za upravljanje

Da biste postavili pravilnik pristupa za SAS možete koristiti i popis kontrole pristupa (ACL). To je korisno ako želite da bi više klijenti mogli pristupiti kontejneru, no nudi drugačiji pristup pravila za svaki klijent.

ACL je implementirati pomoću polja pravila za pristup s ID-om pridružen svakom pravila. Sljedeći primjer koda definira dva pravila, jednu za 'Korisnik1', a jedan za 'korisnika2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Sljedeći primjer koda dobiva trenutni ACL-a za **mycontainer**, a zatim zbrojiti nova pravila pomoću **setBlobAcl**. Taj se način omogućuje sljedeće:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Nakon postavljanja ACL-a zatim možete stvoriti zajednički pristup potpisa na temelju ID za pravilo. Sljedeći primjer koda stvara novi zajednički pristup potpise za 'korisnika2':

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u sljedećim člancima.

-   [Azure prostora za pohranu SDK čvor API Reference][]
-   [Blog tima za Azure prostora za pohranu][]
-   [Azure SDK za pohranu za čvor][] spremište na GitHub
-   [Razvojni centar za Node.js](/develop/nodejs/)
-   [Prijenos podataka s pomoćnog programa naredbenog retka AzCopy](storage-use-azcopy.md)

[Azure prostora za pohranu SDK za čvor]: https://github.com/Azure/azure-storage-node

[Stvaranje web-aplikacijama Node.js u aplikacije servisa za Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js web-aplikacije pomoću servisa Azure tablice]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Stvaranje i implementacija web-aplikacijama Node.js Azure pomoću matrice Web]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure prostora za pohranu SDK čvor API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html
