<properties 
    pageTitle="Servis Bus transakcije | Microsoft Azure" 
    description="Pregled Bus servisa Azure atomske transakcije i slanje putem" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Pregled servisa Bus transakcije obrada

U ovom se članku govori o mogućnostima transakcije Bus servisa Azure. Većina rasprave pokazuju [Atomske transakcija s servisa Bus uzorka](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). U ovom se članku ograničeno je na pregled transakcije obradi dokumenata i *slanje putem* značajci Bus servisa dok se uzorak atomske transakcije šira i složenije opseg.

## <a name="transactions-in-service-bus"></a>Transakcije u Bus servisa

[Transakcije](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) grupa dvije ili više operacija zajedno u *izvođenja opseg*. Po prirode, takve transakcije osigurati da sve operacije pripadaju određenoj grupi operacija uspjela ili neće uspjeti verzijom. U ovom odnosu transakcije funkcioniraju kao jedan objekt koji se često se nazivaju *atomicity*. 

Servis Bus je transakcijskih poruke broker i osigurava transakcijskih integritet za sve Interna operacije na temelju njegove sprema poruke. Sve prijenosi poruka unutar Bus servisa, kao što su premještanje poruka [reagira slovo redu](service-bus-dead-letter-queues.md) ili [Automatsko prosljeđivanje](service-bus-auto-forwarding.md) poruka između entiteti su transakcijskih. Kao takve, ako servis Bus prihvaća poruke, je već pohranjena i koje su označene redni broj. Od tada nadalje prijenosi bilo koju poruku unutar servisa Bus su usklađenih operacije preko entiteti, a ne vodi do gubitka (izvor uspijeva i ne uspije cilj) ili dupliciranje (ne uspije izvora i ciljne uspješnog) poruke.

Servis Bus podržava operacija grupiranja na temelju jednog razmjenu entitet (red, teme, pretplatu) unutar opsega transakcije. Ako, na primjer, nekoliko poruka možete poslati jedan red iz opsega transakcije, a poruke će biti samo izvršenja da biste se prijavili u red nakon dovršetka transakcija uspješno.

## <a name="operations-within-a-transaction-scope"></a>Operacija unutar opsega transakcije 

Operacije koje možete izvršiti unutar opsega transakcije su na sljedeći način:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: slanje, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: Dovršeno, CompleteAsync, odustati od, AbandonAsync, Deadletter, DeadletterAsync, odgoditi, DeferAsync, RenewLock, RenewLockAsync 

Primanje operacije nisu obuhvaćeni, jer se pretpostavlja da se aplikacija prikazuje poruke načinu [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) unutar neke primanje petlja ili s [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) povratnog, a zatim samo otvara opsega transakcije za obradu poruku.

Razmještaj poruke (dovršeno, odustati od reagira slova, odgoditi) pojavljuje se zatim unutar raspona, i o njima ovisne na ukupni rezultat transakcije.

## <a name="transfers-and-send-via"></a>Prijenosi i "Pošalji putem"

Da biste omogućili transakcijskih handover podataka iz reda procesor, a zatim u drugom redu, servis Bus podržava *prijenosa*. U operaciji prijenos pošiljatelja najprije šalje poruku "prijenos red" i red čekanja za prijenos odmah premješta poruku na predviđeno odredište red koristeći istu implementaciju robusne prijenos koja se mogućnost automatsko prosljeđivanje ovisi. Poruke nastoji nikad red prijenos zapisnika tako da ona postaje vidljivo kupcima red prijenos.

Power tu mogućnost transakcijskih postaje vidljivu kada reda čekanja za prijenos sam je izvor ulazne poruke pošiljatelja. Drugim riječima, servis Bus možete prenijeti poruku red odredište "putem" red prijenos prilikom izvođenja potpuni (ili odgoditi, ili reagira slovo) operaciju na ulaznu poruku na atomske odjednom. 

### <a name="see-it-in-code"></a>Pogledajte u kodu

Da biste postavili takve prijenosa, stvorite pošiljatelj poruke namijenjen red odredište putem reda čekanja za prijenos. Dodijelit će tekstnog okvira koji povlači poruke od taj isti red. Ako, na primjer:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Zatim jednostavne transakcije koristi tih elemenata, kao u sljedećem primjeru:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Daljnji koraci

Potražite u sljedećim člancima dodatne informacije o redovima Bus servisa:

- [Ulančavanje entiteti Bus servisa s automatsko prosljeđivanje](service-bus-auto-forwarding.md)
- [Automatsko prosljeđivanje uzorka](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Atomske transakcija s uzorak Bus servisa](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure redova i usluge Bus redova usporedbi](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Upute za korištenje servisa Bus redovi](service-bus-dotnet-get-started-with-queues.md)