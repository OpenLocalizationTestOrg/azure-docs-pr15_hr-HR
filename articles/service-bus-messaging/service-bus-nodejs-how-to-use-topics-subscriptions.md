<properties 
    pageTitle="Kako pomoću servisa Bus teme pomoću Node.js | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Bus teme i pretplata u Azure iz Node.js aplikacije." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Upute za korištenje servisa Bus teme i pretplate

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ovaj vodič opisuje način korištenja servisa Bus teme i pretplate iz Node.js aplikacija. Scenariji u kojima je moguće uvrstiti **Stvaranje teme i pretplate**, **Stvaranje filtara za pretplatu**, **slanja poruka** i teme, **dobivate poruku iz pretplate**i **Brisanje teme i pretplate**. Dodatne informacije o temama i pretplate u odjeljku [sljedeće korake](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Stvaranje aplikacije Node.js

Stvaranje prazne Node.js aplikacije. Upute o stvaranju Node.js aplikacije potražite u članku [Stvaranje i implementaciju aplikacija za Node.js na Azure Web-mjesto], pomoću komponente Windows PowerShell ili Web-mjesta s WebMatrix [Node.js u Oblaku][] .

## <a name="configure-your-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Da biste koristili Bus servisa, preuzmite paket Node.js Azure. Paket sadrži skup bibliotekama u kojima komunicirate servisa Bus OSTALE servise.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Korištenje upravitelja čvor paket (NPM) da biste dobili pakiranje

1.  Korištenje naredbenog retka sučelja kao što su **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix), idite u mapu koju ste stvorili probna aplikacija.

2.  Upišite **npm instalacija azure** u prozoru za naredbe koje treba rezultiraju sljedeći rezultat:

    ```
        azure@0.7.5 node_modules\azure
    ├── dateformat@1.0.2-1.2.3
    ├── xmlbuilder@0.4.2
    ├── node-uuid@1.2.0
    ├── mime@1.2.9
    ├── underscore@1.4.4
    ├── validator@1.1.1
    ├── tunnel@0.0.2
    ├── wns@0.5.3
    ├── xml2js@0.2.7 (sax@0.5.2)
    └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3.  Možete ručno pokrenuti naredbu **ls** da biste potvrdili da na **čvor\_modula** mapu stvorena. U toj mapi pronađite **azure** paket koji sadrži biblioteke morate pristupiti temama Bus servisa.

### <a name="import-the-module"></a>Modul za uvoz

Koristite Blok za pisanje ili neki drugi uređivač teksta, dodajte sljedeće na vrh **server.js** datoteka aplikacije:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Postavljanje servisa Bus veze

Modul Azure čita varijable okruženja AZURE\_SERVICEBUS\_prostor naziva i AZURE\_SERVICEBUS\_PRISTUP\_KLJUČNE informacije potrebne za povezivanje s Bus servisa. Ako te varijable okruženja su postavljena, morate navesti informacije o računu prilikom pozivanja **createServiceBusService**.

Primjer postavke varijable okruženja u konfiguracijskoj datoteci programa Azure u Oblaku, potražite u članku [Node.js u Oblaku s prostora za pohranu][].

Primjer [Azure klasični portal][] za web-mjesto programa Azure postavljanje varijable okruženja, potražite u članku [Node.js web-aplikacije s prostora za pohranu][].

## <a name="create-a-topic"></a>Stvaranje teme

Objekt **ServiceBusService** omogućuje vam da biste radili s temama. Sljedeći kod stvara **ServiceBusService** objekt. Dodajte ga pri vrhu datoteke **server.js** nakon naredbe da biste uvezli modul azure:

```
var serviceBusService = azure.createServiceBusService();
```

Pozivom **createTopicIfNotExists** na objektu **ServiceBusService** određenu temu će vratiti (Ako postoji) ili stvorit će se nova tema s navedenim nazivom. Sljedeći kod koristi **createTopicIfNotExists** za stvaranje ili povezivanje na temu pod nazivom "MyTopic":

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** podržava i dodatne mogućnosti koje omogućuju vam da biste nadjačali zadane postavke tema kao što su vrijeme poruke Live ili tema Maksimalna veličina. Sljedeći primjer postavlja veličinu maksimalno tema 5 GB s vremenom Live 1 minuti:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtri

Neobavezni operacije filtriranja primjenjuje se na razne operacije obavljene pomoću **ServiceBusService**. Filtriranje operacije možete uključiti zapisivanje, automatski ponovni itd. Filtri su objekti koji implementira metode s potpisom:

```
function handle (requestOptions, next)
```

Nakon izvršavanja pretprocesnih na izborniku Mogućnosti zahtjev, poziva metodu `next` prosljeđivanje povratnog potpisom sljedeće:

```
function (returnObject, finalCallback, next)
```

U ovom povratnog i nakon obrade **returnObject** (odgovor na zahtjev na poslužitelj) povratnog treba ili pozivanje se sljedeće ako postoji da biste nastavili obrade ostalih filtara ili jednostavno inače pozivanje **finalCallback** završe poziva servisa.

Dva filtra implementirati pokušaj logike su Azure SDK za Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. **ServiceBusService** objekta koji koristi **ExponentialRetryPolicyFilter**stvara:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Stvaranje pretplate

Tema pretplate i stvaraju se s objektom **ServiceBusService** . Pretplate se nazivaju i može imati neobavezno filtar koji ograničava skup poruka isporučena virtualne red svoje pretplate.

> [AZURE.NOTE] Pretplate se trajni i će i dalje postoji do ili ih ili temi su povezani s, brišu se. Ako aplikacija sadrži logiku da biste stvorili pretplatu, je najprije provjerite postoji li već pretplatu pomoću metode amortizacije **getSubscription** .

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Stvaranje pretplate s filtrom zadani (MatchAll)

Filtar **MatchAll** je zadani filtar koji se koristi ako nema filtra nije naveden stvaranja novu pretplatu. Kada se koristi filtar **MatchAll** , sve poruke koje se objavljuju u temi spremaju se u svoje pretplate virtualne red. Sljedeći primjer stvara pretplatu pod nazivom "AllMessages" i koristi zadani **MatchAll** filtar.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Stvaranje pretplate s filtrima

Možete stvoriti i filtre koji omogućuju vam opseg koje poruke poslane na temu treba prikazuju se u određenoj temi pretplate.

Većina fleksibilne vrstu filtra podržava pretplate je u **SqlFilter**, koji implementira podskup SQL92. Filtri SQL rade na svojstva poruke koje su objavljene na temu. Za dodatne informacije o izrazima koje je moguće koristiti s filtrom SQL pregledajte [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] sintakse.

Filtre možete dodati pretplati pomoću metode **createRule** **ServiceBusService** objekta. Ovaj postupak omogućuje dodavanje nove filtre na postojeću pretplatu.

> [AZURE.NOTE] Budući da zadani filtar automatski će se primijeniti na sve nove pretplate, najprije morate ukloniti zadani filtar ili **MatchAll** nadjačat će sve filtre možete navesti. Zadano pravilo možete ukloniti metodom **deleteRule** **ServiceBusService** objekta.

Sljedeći primjer stvara pretplatu pod nazivom `HighMessages` s **SqlFilter** koji samo odabire poruke koje sadrže prilagođene **messagenumber** svojstvo veći od 3:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Isto tako, sljedeći primjer stvara pretplatu pod nazivom `LowMessages` s **SqlFilter** koji samo odabire poruke koje sadrže **messagenumber** svojstvo manje od ili jednako 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Kada je sada za slanje poruka `MyTopic`, će uvijek isporuku za primatelje pretplaćeni na na `AllMessages` pretplate teme i selektivno isporučeno primatelje pretplaćeni na na `HighMessages` i `LowMessages` temu pretplate (ovisno o tome sadržaj poruke).

## <a name="how-to-send-messages-to-a-topic"></a>Upute za slanje poruka na temu

Da biste poslali poruku teme Bus servisa, aplikacija morate koristiti metodu **sendTopicMessage** **ServiceBusService** objekta.
Poruke poslane na servis Bus teme su objekti **BrokeredMessage** .
Objekti **BrokeredMessage** imati skup standardna svojstva (kao što su **oznake** i **TimeToLive**) rječnika koji koristi se za čuvanje konkretnih svojstava prilagođenu aplikaciju pa u tijelu niz podataka. Aplikaciju možete postaviti tijelo poruke prosljeđivanjem vrijednost niza **sendTopicMessage** i potrebne standardna svojstva će biti popunjena zadane vrijednosti.

Sljedeći primjer pokazuje kako poslati pet testiranje poruka "MyTopic". Imajte na umu da se mijenja se svojstva vrijednost **messagenumber** svake poruke na iteracije petlje (Time ćete odrediti koje dobio):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Servis Bus teme podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u temi, ali postoji u kapaciteta ukupnu veličinu poruke zaključale temu. Ovu temu veličinu definirana je u trenutku stvaranja s gornju granicu 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Primanje poruka iz pretplate

Poruke, isporučene su iz pretplate pomoću metode **receiveSubscriptionMessage** na **ServiceBusService** objektu. Prema zadanim postavkama, poruke se brišu iz pretplate kao pročitano; Međutim, pročitajte (uvid) i zaključavanje poruke bez brisanja iz pretplate postavljanjem neobavezan parametar **isPeekLock** na **true**.

Zadano ponašanje za čitanje i brisanje poruka u sklopu postupka primanje je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

Ako je parametar **isPeekLock** postavljen na **true**, na primanje postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da biste spriječili drugi korisnici ga prima i vraća je aplikacija.
Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete **deleteMessage** način i omogućivanje poruka će se izbrisati kao parametar. Način **deleteMessage** će označavanje poruke kao što je u tijeku potrošena i ukloniti iz pretplate.

Sljedeći primjer pokazuje kako se poruke mogu primati i obrađuju pomoću **receiveSubscriptionMessage**. U primjeru najprije prima i briše poruke iz pretplate 'LowMessages', a zatim Prima poruke iz pretplate 'HighMessages' pomoću **isPeekLock** postavljen na true. Zatim briše poruke pomoću **deleteMessage**:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako rukovati ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako je primatelj aplikacija nije moguće obraditi poruku zbog nekog razloga, pa je možete nazvati metodu **unlockMessage** na objektu **ServiceBusService** . Time će Bus servisa da biste otključali poruku unutar pretplate i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan unutar pretplate, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (na primjer, ako aplikacija ruši), servis Bus automatski ćete otključati poruku, a on postaje dostupan za ponovno primili.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se zove metodu **deleteMessage** , zatim poruku će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obrade**, odnosno svake poruke obradit će se barem jednom no u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. To je često postići pomoću **MessageId** svojstva poruke, koju će ostaju iste preko pokušao.

## <a name="delete-topics-and-subscriptions"></a>Brisanje teme i pretplate

Teme i pretplate stalni i izričito daljnja putem [Azure klasični portal][] ili programski.
Sljedeći primjer pokazuje kako izbrisati temi pod nazivom `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Brisanje teme i izbrisati sve pretplate registrirane s temom. Pretplate se brišu i zasebno. Sljedeći primjer prikazuje način da biste izbrisali pretplatu pod nazivom `HighMessages` iz na `MyTopic` tema:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus teme, slijedite ove veze da biste saznali više.

-   Potražite u članku [redova, teme i pretplate][].
-   Pregled API-JA za [SqlFilter][].
-   Posjetite [Azure SDK za čvor][] spremište na GitHub.

  [Azure SDK za čvor]: https://github.com/Azure/azure-sdk-for-node
  [Azure klasični portal]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Redova, teme i pretplate]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js u Oblaku]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Stvaranje i implementaciju aplikacija za Node.js na Azure Web-mjesto]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Servis u Oblaku Node.js s prostora za pohranu]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js web-aplikacije s prostora za pohranu]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
