<properties 
    pageTitle="Kako pomoću servisa Bus teme pomoću PHP | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Bus teme i servisu Azure." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Upute za korištenje servisa Bus teme i pretplate

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

U ovom se članku objašnjava korištenje servisa Bus teme i pretplate. Primjere zapisuju u PHP i koristiti [SDK Azure i](../php-download-sdk.md). Scenariji u kojima je moguće uvrstiti **Stvaranje teme i pretplate**, **Stvaranje filtara za pretplatu**, **slanje poruka na temu**, **dobivate poruku iz pretplate**i **Brisanje teme i pretplate**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Stvaranje i aplikacije

Samo preduvjeta za stvaranje i aplikacije koji pristupa servis blobova platforme Azure tako da upućuju na klase u [SDK Azure i](../php-download-sdk.md) iz unutar. Da biste stvorili aplikacije ili blok za pisanje možete koristiti bilo koji alat za razvoj.

> [AZURE.NOTE] Vaša instalacija i mora imati [nastavak OpenSSL](http://php.net/openssl) instalirana ni omogućena.

U ovom se članku opisuje način korištenja značajke usluge koje možete pozvati u aplikaciji i lokalno ili kod koji se izvodi unutar Azure web uloge, ulogu suradnika ili web-mjesta.

## <a name="get-the-azure-client-libraries"></a>Početak Azure klijent biblioteke

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Da biste koristili Bus API servisa:

1. Referenca autoloader datoteku pomoću [require_once] [ require-once] izjava.
2. Reference sve klase koje koristite.

Sljedeći primjer pokazuje kako uključiti autoloader datoteku i referencu **ServiceBusService** predmete.

> [AZURE.NOTE] U ovom se primjeru (i još primjera u ovom članku) pretpostavlja da ste instalirali biblioteke klijenta i za Azure putem Skladatelj. Ako ste instalirali biblioteke ručno ili kao paket KRUŠAKA, morate se referencirati **WindowsAzure.php** autoloader datoteku.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

U sljedećim primjerima u `require_once` izjava uvijek će se prikazivati, ali samo klasa potrebne navedeni primjer ispravno izvršiti referencirani.

## <a name="set-up-a-service-bus-connection"></a>Postavljanje servisa Bus veze

Stvoriti instancu servisa Bus klijent najprije morate imati valjani niz za povezivanje u ovom obliku:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Gdje `Endpoint` je obično u obliku `https://[yourNamespace].servicebus.windows.net`.

Da biste stvorili bilo kojeg klijenta servisa Azure morate koristiti **ServicesBuilder** predmete. možeš:

* Niz za povezivanje prenesite izravno na njega.
* Korištenje **CloudConfigurationManager (CCM)** da biste provjerili više vanjskih izvora niz za povezivanje:
    * Prema zadanim postavkama dolazi s podrškom za jedan vanjski izvor - okolini varijabli.
    * Proširivanje klase **ConnectionStringSource** možete dodati novih izvora.

Primjer navedene u nastavku, niz za povezivanje prosljeđuje izravno.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Stvaranje teme

Možete izvršiti operacija upravljanja teme Bus servisa putem **ServiceBusRestProxy** predmete. Objekt programa **ServiceBusRestProxy** konstruirana putem metodu tvorničke **ServicesBuilder::createServiceBusService** nizom odgovarajuću vezu koja encapsulates tokena dozvola za upravljanje ga.

Sljedeći primjer pokazuje kako stvoriti instancu **ServiceBusRestProxy** i poziva da biste stvorili temi pod nazivom **ServiceBusRestProxy -> createTopic** `mytopic` unutar na `MySBNamespace` prostor naziva:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] Možete koristiti u `listTopics` način na `ServiceBusRestProxy` objekata da biste provjerili je li temu s navedenim nazivom već postoji u polje naziva servisa.

## <a name="create-a-subscription"></a>Stvaranje pretplate

Tema pretplate i stvaraju se pomoću metode **ServiceBusRestProxy -> createSubscription** . Pretplate se nazivaju i može imati neobavezno filtar koji ograničava skup poruka proslijeđena virtualne red svoje pretplate.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Stvaranje pretplate s filtrom zadani (MatchAll)

Filtar **MatchAll** je zadani filtar koji se koristi ako nema filtra nije naveden stvaranja novu pretplatu. Kada se koristi filtar **MatchAll** , sve poruke koje se objavljuju u temi spremaju se u svoje pretplate virtualne red. Sljedeći primjer stvara pretplatu pod nazivom "mysubscription" i koristi zadani **MatchAll** filtar.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Stvaranje pretplate s filtrima

Možete postaviti i tako filtre koje vam omogućuju da odredite koje poruke poslane na temu moraju se pojavljuju u određenoj temi pretplate. Većina fleksibilne vrstu filtra podržava pretplate je u **SqlFilter**, koji implementira podskup SQL92. Filtri SQL rade na svojstva poruke koje su objavljene na temu. Dodatne informacije o SqlFilters potražite u članku [SqlFilter.SqlExpression svojstvo][sqlfilter].

> [AZURE.NOTE] Svako pravilo na pretplatu obrađuje dolazne poruke neovisno o dodavanju svoje poruke rezultat s pretplatom. Osim toga, svaku novu pretplatu sadrži objekt programa zadana **pravila** s filtrom dodaje sve poruke iz temu s pretplatom. Da biste primali samo poruke koje odgovaraju filtar, morate ukloniti zadano pravilo. Zadano pravilo možete ukloniti pomoću na `ServiceBusRestProxy->deleteRule` način.

Sljedeći primjer stvara pretplatu pod nazivom **HighMessages** s **SqlFilter** koji samo odabire poruke koje sadrže prilagođeno svojstvo **MessageNumber** veći od 3 (potražite u članku [slanje poruka na temu](#send-messages-to-a-topic) za informacije o dodavanju prilagođena svojstva poruke):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Napomena kod potreban je korištenje dodatni prostor za naziv: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Isto tako, sljedeći primjer stvara pretplatu pod nazivom **LowMessages** s **SqlFilter** koji samo odabire poruke koje sadrže **MessageNumber** svojstvo manje od ili jednako 3:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Sada, kada poruka je poslana na `mytopic` temi je uvijek isporučeno za primatelje da biste se pretplatili na `mysubscription` pretplate, i selektivno isporučeno primatelje da biste se pretplatili na `HighMessages` i `LowMessages` pretplate (ovisno o tome sadržaj poruke).

## <a name="send-messages-to-a-topic"></a>Slanje poruka teme

Da biste poslali poruku teme Bus servisa, aplikacija poziva metodu **ServiceBusRestProxy -> sendTopicMessage** . Sljedeći kod prikazuje kako poslati poruku u `mytopic` tema koje su prethodno stvorene u `MySBNamespace` polje naziva servisa.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Poruke poslane na servis Bus teme su instance klase **BrokeredMessage** . Objekti **BrokeredMessage** sadrže skup standardna svojstva i metode (kao što su **getLabel**, **getTimeToLive**, **setLabel**i **setTimeToLive**), kao i svojstva koja se može koristiti za držite prilagođena svojstva specifičnim aplikacijama. Sljedeći primjer prikazuje način da biste poslali 5 test poruke na `mytopic` tema koje ste prethodno stvorili. Metodu **setProperty** se dodaje prilagođeno svojstvo (`MessageNumber`) za svaku poruku. Imajte na umu da se `MessageNumber` mijenja se svojstva vrijednost za svaku poruku (možete koristiti tu vrijednost da biste utvrdili koji pretplate dobio, kao što je prikazano u odjeljku [Stvaranje pretplatu](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Servis Bus teme podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u temi, ali postoji u kapaciteta ukupnu veličinu poruke zaključale temu. Ovaj gornju granicu veličine tema je 5 GB. Dodatne informacije o kvotama potražite u članku [kvota Bus servisa][].

## <a name="receive-messages-from-a-subscription"></a>Primanje poruka iz pretplate

Da biste primili poruku iz pretplate, najbolje je metoda **ServiceBusRestProxy -> receiveSubscriptionMessage** . Primljene poruke možete raditi u dva različita načina rada: **ReceiveAndDelete** (zadano) i **PeekLock**.

Korištenje načina **ReceiveAndDelete** primite je operacija jednim snimka; to jest, kada servis Bus primi zahtjev za poruku u pretplatu, vama označava da je poruka kao što je u tijeku potrošena i vraća aplikaciji. Način **ReceiveAndDelete** je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

U načinu **PeekLock** primanje poruke postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da biste spriječili drugi korisnici ga prima i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje prosljeđivanjem primljene poruke **ServiceBusRestProxy -> deleteMessage**. Kada je servis Bus vidi **deleteMessage** poziva, označavanje poruke kao što je u tijeku potrošena će se i ukloniti iz reda.

Sljedeći primjer pokazuje kako dobiti i obrada poruke pomoću **PeekLock** način rada (ne zadani način). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako: ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako je primatelj aplikacija nije moguće obraditi poruku zbog nekog razloga, pa je možete nazvati metodu **unlockMessage** na primljene poruke (umjesto metode **deleteMessage** ). Time će Bus servisa da biste otključali poruku u redu čekanja i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan u redu čekanja, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (na primjer, ako ruši se aplikacija), a zatim servis Bus će automatski otključati poruku i učiniti je dostupnom koji se dobiva ponovno.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se izda zahtjev za **deleteMessage** , zatim poruku će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obradu**; to jest, svaku poruku obrađuje barem jednom, ali u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne aplikacije koje će se rukovati isporuka duplicirane poruke. To je često postići pomoću metode **getMessageId** poruke ostaje nepromijenjen preko pokušao.

## <a name="delete-topics-and-subscriptions"></a>Brisanje teme i pretplate

Da biste izbrisali temu ili pretplatu, odnosno pomoću **ServiceBusRestProxy -> deleteTopic** ili načine **ServiceBusRestProxy -> deleteSubscripton** . Imajte na umu da brisanje teme briše i sve pretplate registrirane s temom.

Sljedeći primjer prikazuje način da biste izbrisali temi pod nazivom `mytopic` i njegov registrirani pretplate.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Pomoću metode amortizacije **deleteSubscription** možete nezavisno izbrisati pretplatu:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus redovi, potražite u članku [redova, teme i pretplate][] dodatne informacije.

[Redova, teme i pretplate]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Servis Bus kvote]: service-bus-quotas.md
