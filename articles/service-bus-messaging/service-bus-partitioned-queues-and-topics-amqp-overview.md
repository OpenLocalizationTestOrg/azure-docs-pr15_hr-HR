<properties 
    pageTitle="AMQP 1.0 podrška za servis Bus particije redova i teme | Microsoft Azure" 
    description="Dodatne informacije o korištenju napredne poruke stavljanje Protocol (AMQP) 1.0 sa servisa Bus particije redova i teme." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>AMQP 1.0 podrška za servis Bus particije redova i teme 

Azure servisa Bus sada podržava napredne poruke stavljanje protokol (**AMQP**) 1.0 za servis Bus **particije redovima i temama.**

**AMQP** je protokol stavljanja se otvori standardnu poruku koja omogućuje razvoj različite platforme aplikacije koje se koriste različite jezike programiranje. Opće informacije o AMQP podršci u servis Bus potražite [AMQP 1.0 podržava u Bus servisa](service-bus-amqp-overview.md).

**Partitioned redovima i teme**, poznata i kao *particioniranom entiteti*nude veća dostupnost, pouzdanosti i propusnost od običan koje nisu particije redovima i teme. Dodatne informacije o particioniranom entiteti potražite u članku [Particioniranom entiteti razmjene poruka](service-bus-partitioning.md).

Dodatak AMQP 1.0 kao protokol za komunikaciju s particioniranom redovima i teme vam omogućuje sastavljanje Uskladi aplikacije koje možete iskoristiti veća dostupnost pouzdanosti, i cijeloj nudi entiteti servisa Bus particije.

Detaljne žičani razinom AMQP 1.0 protokol vodič, koja objašnjava kako Bus servisa implementira i sastavlja na specifikacija tehničke AMQP ORGANIZACIJA, potražite u članku [AMQP 1.0 u Bus servisa Azure i događaja koncentratora protokol vodič](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>Korištenje AMQP s particioniranom reda čekanja

Redovi korisne su za scenarija koji zahtijevaju vremenski decoupling, ujednačavanje opterećenja, opterećenja i su coupling. Red čekanja izdavači slati poruke u red i korisnici poruka iz reda čekanja u slučajevima gdje poruke samo mogu primati jedanput. Dobar primjer to je u sustavu zaliha u kojem point-of-sale WBT objavljivanje podataka reda čekanja umjesto izravno u sustavu upravljanja zalihama. Sustav upravljanja zalihama zatim troši podatke u bilo kojem trenutku da biste upravljali burzovni popunjavanja. Ima nekoliko prednosti, uključujući sustav upravljanja zalihama ne morate biti na mreži cijelo vrijeme. Dodatne informacije o redovima Bus servisa potražite u članku [Stvaranje aplikacije koje koriste redova Bus servisa](service-bus-create-queues.md). 

Daljnje particioniranom reda povećava dostupnost, pouzdanosti i propusnost za aplikacije, kao što je redove imaju preko više poruka Brokerske djelatnosti i razmjenu trgovine.     

### <a name="create-partitioned-queues"></a>Stvaranje particioniranom reda čekanja

Možete stvoriti particioniranom reda čekanja pomoću i SDK Bus servisa [Azure klasični portal][] . Da biste stvorili particioniranom reda čekanja, postavite svojstvo [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) na **true** u [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) instanci. Sljedeći kod prikazuje kako stvoriti particioniranom reda čekanja pomoću servisa Bus SDK-a. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Slanje i primanje poruka pomoću AMQP

Slanje poruke i primanje poruka iz particioniranom reda pomoću AMQP kao protokol, tako da postavite svojstvo [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) na [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) , kao što je prikazano u sljedećem kodu.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>Korištenje AMQP s particioniranom teme

Tema pojmovno slični su redovi, no teme mogu usmjeravati kopiju ista poruka višestruke *pretplate*. U temi izdavači slanje poruka na temu, a koje korisnici poruka iz pretplate. U point-of-sale scenarij sustava zalihama WBT objaviti podataka na temu. Sustav upravljanja zalihama zatim Prima poruke iz pretplate. Osim toga, nadzor sustava možete primati ista poruka od neku drugu pretplatu. Dodatne informacije o temama Bus servisa i pretplate potražite u članku [Stvaranje aplikacije koje koriste servis Bus teme i pretplate](service-bus-create-topics-subscriptions.md). 

Kao i kod redove, dodatno particioniranom teme povećanje dostupnosti, pouzdanosti i propusnost za aplikacije, kao što je ove teme i njihove pretplate particije preko više poruka Brokerske djelatnosti i razmjenu trgovine. 

### <a name="create-partitioned-topics"></a>Stvaranje particioniranom teme

Možete stvoriti particioniranom teme pomoću i SDK Bus servisa [Azure klasični portal][] . Da biste stvorili particioniranom tema, postavite svojstvo [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) na **true** u [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) instanci. Sljedeći kod prikazuje kako stvoriti particioniranom teme pomoću servisa Bus SDK-a.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Slanje i primanje poruka pomoću AMQP

Slanje poruke i primanje poruka iz pretplate particioniranom teme pomoću AMQP kao protokol, tako da postavite svojstvo [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) na [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), kao što je prikazano u sljedećem kodu.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o particioniranom razmjenu entiteti, kao i AMQP sljedeće dodatne informacije potražite u članku.

*    [Particioniranom entiteti za razmjenu poruka](service-bus-partitioning.md)
*    [ORGANIZACIJA napredne poruke stavljanje Protocol (AMQP) verzije 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Podrška za AMQP 1.0 u Bus servisa](service-bus-amqp-overview.md)
*    [AMQP 1.0 u vodiču za protokol Bus servisa Azure i koncentratora događaja](service-bus-amqp-protocol-guide.md)
*    [Upute za korištenje na Java poruka servisa (JMS) API-JA s Bus servisa i AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Kako koristiti AMQP 1.0 s API za .NET Bus usluge](service-bus-dotnet-advanced-message-queuing.md)

[Azure klasični portal]: http://manage.windowsazure.com
