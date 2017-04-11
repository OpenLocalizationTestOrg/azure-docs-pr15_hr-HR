<properties
    pageTitle="Kako koristiti reda čekanja za pohranu iz Node.js | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure red za stvaranje i brisanje redove, umetanje, dobiti i brisanje poruka. Uzorci pisane Node.js."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Kako koristiti reda čekanja za pohranu iz Node.js

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Pregled

Ovaj vodič prikazuje kako izvoditi uobičajene scenarije putem servisa Microsoft Azure red. Primjere se pišu pomoću Node.js API-JA. Scenariji u kojima je moguće uvrstiti **Umetanje**, **peeking**, **Početak**i **Brisanje** reda čekanja poruke, kao i **Stvaranje i brisanje reda čekanja**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Stvaranje aplikacije Node.js

Stvaranje prazne Node.js aplikacije. Upute za stvaranje Node.js aplikacije potražite u članku [Stvaranje web-aplikacijama Node.js u aplikacije servisa za Azure], [Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure] pomoću komponente Windows PowerShell ili [stvorite i implementirajte Node.js web-aplikacijama za Azure pomoću matrice Web].

## <a name="configure-your-application-to-access-storage"></a>Konfiguriranje aplikacija za pohranu programa Access

Da biste koristili Azure prostora za pohranu, morate SDK za pohranu Azure Node.js koje sadrži skup praktičnost biblioteke koje komunikaciju OSTALE servise za pohranu.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Korištenje upravitelja čvor paket (NPM) da biste dobili pakiranje

1.  Korištenje naredbenog retka sučelja kao što su **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix), idite u mapu koju ste stvorili probna aplikacija.

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

3.  Možete ručno pokrenuti naredbu **ls** da biste potvrdili da je **čvor\_modula** mapu stvoren. U toj mapi tražit će paket **azure prostora za pohranu** koji sadrži biblioteke morate pristupiti prostora za pohranu.

### <a name="import-the-package"></a>Uvoz paketa

Na vrh datoteke **server.js** aplikacije koju namjeravate koristiti za pohranu koristite Blok za pisanje ili neki drugi uređivač teksta, dodajte sljedeće:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Postavljanje veze sustava Azure prostora za pohranu

Modul azure će čitati varijable okruženja AZURE\_prostora za POHRANU\_RAČUN i AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ ili AZURE\_prostora za POHRANU\_veze\_NIZ za podatke potrebne za povezivanje s računom Azure prostora za pohranu. Ako nisu postavljene ove varijable okruženja, morate navesti podatke o računu prilikom pozivanja **createQueueService**.

Primjer [Azure Portal](https://portal.azure.com) za web-mjesto programa Azure postavljanje varijable okruženja, potražite u članku [Node.js web-aplikacije pomoću servisa Azure tablice].

## <a name="how-to-create-a-queue"></a>Upute: Stvaranje reda čekanja

Sljedeći kod stvara **QueueService** objektom, čime se omogućuje rad sa redovima.

    var queueSvc = azure.createQueueService();

Upotrijebite metodu **createQueueIfNotExists** koja vraća navedeni reda čekanja ako već postoji ili stvara novi red s navedenim nazivom ako se još ne postoji.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Ako je stvorena reda, `result.created` je true. Ako postoji reda, `result.created` je vrijednost false.

### <a name="filters"></a>Filtri

Neobavezni operacije filtriranja primjenjuje se na razne operacije obavljene pomoću **QueueService**. Filtriranje operacije možete uključiti zapisivanje, automatski ponovni itd. Filtri su objekti koji implementira metode s potpisom:

    function handle (requestOptions, next)

Nakon toga njegov pretprocesnih na izborniku Mogućnosti zahtjev, metodu mora poziva "sljedeće" prosljeđivanje povratnog potpisom sljedeće:

    function (returnObject, finalCallback, next)

U ovom povratnog i nakon obrade returnObject (odgovor na zahtjev na poslužitelj) povratnog treba pozivanje Dalje ako postoji li nastaviti obradu ostalih filtara ili jednostavno inače pozivanje finalCallback završe poziva servisa.

Dva filtra implementirati pokušaj logike su Azure SDK za Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. **QueueService** objekta koji koristi **ExponentialRetryPolicyFilter**stvara:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Upute: Umetanje poruke reda čekanja

Da biste umetnuli reda poruke, pomoću metode **createMessage** stvorite novu poruku, a zatim ga dodati u redu čekanja.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Upute: Pogled na sljedeću poruku

Možete pogled na poruku prednjoj strani reda bez uklanjanja iz reda čekanja tako da nazovete metodu **peekMessages** . Prema zadanim postavkama **peekMessages** kratki pogledi na jednu poruku.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

U `result` sadrži poruku.

> [AZURE.NOTE] Korištenje **peekMessages** kada postoje poruke nije u redu čekanja će vratiti pogrešku, no bez poruke će vratiti.

## <a name="how-to-dequeue-the-next-message"></a>Upute: Dequeue sljedeće poruke

Obrada poruke je dvije faze:

1. Dequeue poruku.

2. Brisanje poruke.

Da biste dequeue poruke, koristite **getMessages**. Ovime poruke nisu vidljivi u redu čekanja, pa ih možete obraditi nema klijenata. Kada aplikacija je obrađuju poruke, nazovite **deleteMessage** da biste izbrisali iz reda. Sljedeći primjer dobiva poruku, a zatim briše:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Prema zadanim postavkama poruke samo je skriven 30 sekundi, nakon čega je vidljiv u drugim klijentima. Možete navesti neku drugu vrijednost pomoću `options.visibilityTimeout` s **getMessages**.

> [AZURE.NOTE]
> Korištenje **getMessages** kada postoje poruke nije u redu čekanja će vratiti pogrešku, no bez poruke će vratiti.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Upute: Promjena sadržaja u redu čekanja poruke

Možete promijeniti sadržaj u poruku na mjestu u redu čekanja pomoću **updateMessage**. U sljedećem primjeru obnavlja tekst poruke:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Upute: Dodatne mogućnosti za Dequeuing poruke

Moguće je prilagoditi dohvaćanja poruke iz reda čekanja na dva načina:

* `options.numOfMessages`– Dohvaćanje skupine poruke (najviše 32).
* `options.visibilityTimeout`– Postavljanje vremenskog ograničenja dulje ili kraće invisibility.

Sljedeći primjer koristi metodu **getMessages** da biste dobili 15 poruke u jedan poziv. Zatim obrađuje svaku poruku pomoću programa za petlje. Također postavlja vremenskog ograničenja invisibility na pet minuta za sve poruke koje vraća ova metoda.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Upute: Dohvaćanje Duljina reda čekanja

**GetQueueMetadata** vraća metapodatke o reda čekanja, uključujući djelomičnog broj poruka koje čekaju u redu čekanja.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Upute: Popis redovi

Da biste dohvatili popis redovi, koristite **listQueuesSegmented**. Da biste dohvatili popis filtriran prema određenim prefiks, koristite **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Ako nije moguće vratiti sve redovi, `result.continuationToken` može se koristiti kao prvi parametar **listQueuesSegmented** ili drugi parametar **listQueuesSegmentedWithPrefix** za dohvaćanje više rezultata.

## <a name="how-to-delete-a-queue"></a>Upute: Brisanje reda čekanja

Da biste izbrisali red i sve poruke koje se nalaze u njoj, nazovite metodu **deleteQueue** na objektu red.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Da biste očistili sve poruke iz reda čekanja prije brisanja, koristite **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Kako: rad s zajedničkog pristupa potpisa

Zajednički pristup potpisa (SAS) su siguran način za zrnastog pristup redovima bez osiguravanja naziv računa za pohranu ili tipke. SAS često se koristi za ograničeni pristup reda čekanja, kao što su dopuštanja mobilne aplikacije za slanje poruka.

Pouzdani aplikacije kao što je servis u oblaku generira SAS pomoću **generateSharedAccessSignature** **QueueService**te je prenosi nepouzdanih ili djelomično pouzdanih aplikacija. Na primjer, aplikacije za mobilne uređaje. Na SAS generira pomoću pravila koja opisuje tijekom kojeg je valjan na SAS datume početka i završetka, baš kao razinu pristupa dodijeljene držača SAS.

Sljedeći primjer stvara novog pravilnika zajednički pristup omogućit će držača SAS da biste dodali poruke u redu čekanja i istekne 100 minuta nakon vremena stvaranja.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Imajte na umu da informacije glavno računalo mora se navesti također, kao što je potreban je pri držača SAS pokušava pristupiti reda.

Klijentska aplikacija pa koristi u SAS sa **QueueServiceWithSAS** za izvođenje operacija na temelju reda. U sljedećem primjeru povezuje reda čekanja i stvara poruku.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Budući da u SAS je generirao Dodaj access, ako su pokušaj čitanje, ažuriranje i brisanje poruka, će se vraća pogreška.

### <a name="access-control-lists"></a>Popisi za upravljanje

Da biste postavili pravilnik pristupa za SAS možete koristiti i popis kontrole pristupa (ACL). To je korisno ako želite da bi više klijenti mogli pristupiti reda, no nudi drugačiji pristup pravila za svaki klijent.

ACL je implementirati pomoću polja pravila za pristup s ID-om pridružen svakom pravila. U sljedećem primjeru definira dva pravila jedan za "Korisnik1" i jedan za 'korisnika2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Sljedeći primjer dobiva trenutnog ACL-a za **myqueue**, a zatim dodaje nova pravila pomoću **setQueueAcl**. Taj se način omogućuje sljedeće:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Nakon postavljanja ACL-a zatim možete stvoriti SAS na temelju ID za pravilo. Sljedeći primjer stvara novi SAS za 'korisnika2':

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove reda čekanja prostora za pohranu, slijedite ove veze na dodatne informacije o složenije zadatke za pohranu.

-   Posjetite [Blog tima za Azure prostora za pohranu][].
-   Posjetite [Azure SDK za pohranu za čvor][] spremište na GitHub.

  [Azure prostora za pohranu SDK za čvor]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Stvaranje web-aplikacijama Node.js u aplikacije servisa za Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js web-aplikacije pomoću servisa Azure tablice]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Stvaranje i implementaciju aplikacija za Node.js sa servisom Cloud Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
  [Stvaranje i implementacija web-aplikacijama Node.js Azure pomoću matrice Web]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
