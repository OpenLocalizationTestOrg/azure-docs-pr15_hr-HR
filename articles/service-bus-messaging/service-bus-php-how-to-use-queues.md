<properties 
    pageTitle="Kako pomoću servisa Bus redova pomoću PHP | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Bus redova u Azure. Primjere koda napisan i." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Upute za korištenje servisa Bus redovi

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ovaj vodič pokazuje kako pomoću servisa Bus redova. Primjere zapisuju u PHP i koristiti [SDK Azure i](../php-download-sdk.md). Scenariji u kojima je moguće uvrstiti **Stvaranje reda čekanja**, **Slanje i primanje poruka**i **Brisanje reda čekanja**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Stvaranje i aplikacije

Samo preduvjeta za stvaranje i aplikacije koji pristupa servisa Azure Blob je pozivanju klase u [SDK Azure i](../php-download-sdk.md) iz unutar. Da biste stvorili aplikacije ili blok za pisanje možete koristiti bilo koji alat za razvoj.

> [AZURE.NOTE] Vaša instalacija i mora imati [nastavak OpenSSL](http://php.net/openssl) instalirana ni omogućena.

U ovom vodiču će koristiti značajke servisa koje možete pozvati iz aplikacije i lokalno ili kod koji se izvodi unutar Azure web uloge, ulogu suradnika ili web-mjesta.

## <a name="get-the-azure-client-libraries"></a>Početak Azure klijent biblioteke

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Da biste koristili servis Bus reda čekanja API-ji, učinite sljedeće:

1. Referenca autoloader datoteku pomoću [require_once] [ require_once] izjava.
2. Reference sve klase koje koristite.

Sljedeći primjer pokazuje kako uključiti autoloader datoteku i referencu **ServicesBuilder** predmete.

> [AZURE.NOTE] U ovom se primjeru (i još primjera u ovom članku) pretpostavlja da ste instalirali biblioteke klijenta i za Azure putem Skladatelj. Ako ste instalirali biblioteke ručno ili kao paket KRUŠAKA, morate se referencirati **WindowsAzure.php** autoloader datoteku.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

U primjerima koji slijede, u `require_once` izjava uvijek će se prikazivati, ali samo klasa potrebne navedeni primjer ispravno izvršiti referencirani.

## <a name="set-up-a-service-bus-connection"></a>Postavljanje servisa Bus veze

Stvoriti instancu servisa Bus klijent, najprije morate imati valjani niz za povezivanje u ovom obliku:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Gdje **krajnjoj točki** obično je u obliku `[yourNamespace].servicebus.windows.net`.

Da biste stvorili bilo kojeg klijenta servisa Azure morate koristiti **ServicesBuilder** predmete. možeš:

* Niz za povezivanje prenesite izravno na njega.
* Korištenje **CloudConfigurationManager (CCM)** da biste provjerili više vanjskih izvora niz za povezivanje:
    * Prema zadanim postavkama dolazi s podrškom za jedan vanjski izvor - okolini varijable
    * Možete dodati nove izvore produženjem klase **ConnectionStringSource**

Primjer navedene u nastavku, niz za povezivanje će se proslijediti izravno.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Kako: Stvaranje reda čekanja

Možete izvesti upravljanje radnje za servis Bus redove putem **ServiceBusRestProxy** predmete. Objekt programa **ServiceBusRestProxy** konstruirana putem metodu tvorničke **ServicesBuilder::createServiceBusService** nizom odgovarajuću vezu koja encapsulates tokena dozvola za upravljanje ga.

Sljedeći primjer pokazuje kako stvoriti instancu **ServiceBusRestProxy** i poziva stvaranje reda pod nazivom **ServiceBusRestProxy -> createQueue** `myqueue` unutar na `MySBNamespace` polje naziva servisa:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Možete koristiti u `listQueues` način na `ServiceBusRestProxy` objekata da biste provjerili je li reda čekanja s navedenim nazivom već postoji u prostor naziva.

## <a name="how-to-send-messages-to-a-queue"></a>Kako: slanje poruka reda čekanja

Da biste poslali poruku reda Bus servisa, aplikacija poziva metodu **ServiceBusRestProxy -> sendQueueMessage** . Sljedeći kod prikazuje kako poslati poruku u `myqueue` reda čekanja prethodno stvorene u `MySBNamespace` polje naziva servisa.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Da biste (primljenim i iz) servisa Bus redova su instance klase **BrokeredMessage** poruke. Objekti **BrokeredMessage** sadrže skup standardne metode (kao što su **getLabel**, **getTimeToLive**, **setLabel**i **setTimeToLive**) i svojstva koje se koriste za držite prilagođena svojstva specifičnim aplikacijama i tijelo podataka proizvoljne aplikacije.

Servis Bus redova podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u redu čekanja, ali postoji u kapaciteta ukupnu veličinu poruke zaključale reda. Ovaj gornju granicu veličine reda čekanja je 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Kako se poruka iz reda čekanja

Da biste primili poruku iz reda, najbolje je metoda **ServiceBusRestProxy -> receiveQueueMessage** . Poruka je moguće primiti u dva različita načina rada: **ReceiveAndDelete** (zadano) i **PeekLock**.

Korištenje načina **ReceiveAndDelete** primite je operacija jednim snimka - odnosno kada servis Bus primi zahtjev za poruku u redu čekanja, označava poruku kao se potrošena i vraća aplikaciji. Način **ReceiveAndDelete** je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

U načinu **PeekLock** primanje poruke postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da biste spriječili da drugi korisnici ga prima i vraća aplikacije. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje prosljeđivanjem primljene poruke **ServiceBusRestProxy -> deleteMessage**. Kada je servis Bus vidi **deleteMessage** poziva, označavanje poruke kao što je u tijeku potrošena će se i ukloniti iz reda.

Sljedeći primjer prikazuje način na koji poruke mogu primati i obrađuju **PeekLock** načinu rada (ne zadani način).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako: ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako je primatelj aplikacija nije moguće obraditi poruku zbog nekog razloga, pa je možete nazvati metodu **unlockMessage** na primljene poruke (umjesto metode **deleteMessage** ). Time će Bus servisa da biste otključali poruku u redu čekanja i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan u redu čekanja, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (na primjer, ako ruši se aplikacija), a zatim servis Bus će automatski otključati poruku i učiniti je dostupnom koji se dobiva ponovno.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se izda zahtjev za **deleteMessage** , zatim poruku će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obradu**; to jest, svaku poruku obrađuje barem jednom, ali u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, preporučuje se dodavanje dodatnih logike aplikacije za rukovanje isporuka duplicirane poruke. To je često postići pomoću metode **getMessageId** poruke ostaje nepromijenjen preko pokušao.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus redovi, potražite u članku [redova, teme i pretplate][] dodatne informacije.

Dodatne informacije i potražite u [Centru za razvojne inženjere i](/develop/php/).

[Redova, teme i pretplate]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
