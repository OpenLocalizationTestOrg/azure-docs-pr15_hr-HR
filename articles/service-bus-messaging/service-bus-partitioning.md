<properties 
    pageTitle="Particije redova i teme | Microsoft Azure"
    description="U članku se opisuje particija servisa Bus redovima i teme pomoću više Brokerske djelatnosti poruke."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Particioniranom redovima i teme

Azure servisa Bus uključuje više poruka Brokerske djelatnosti za obradu poruke i više poruka trgovine za spremanje poruka. Običan redu ili tema je obrađuje jednu poruku broker i pohranjene u spremištu za razmjenu na jedan. Servis Bus omogućuje redova ili teme koje se particije preko više poruka Brokerske djelatnosti i razmjenu trgovine. To znači da cjelokupan propusnost particioniranom reda čekanja ili temu više ograničen performanse broker jednu poruku ili SMS trgovine. Uz to, privremene nedostupnosti razmjenu spremišta uopće se ne prikazuje particioniranom reda čekanja ili temu nije dostupan. Particioniranom redovima i teme mogu sadržavati sve napredne značajke Bus servisa, kao što je podrška za transakcije i sesije.

Dodatne informacije o internals Bus servisa u sintaksi [arhitektura Bus servisa][] .

## <a name="how-it-works"></a>Kako funkcionira

Svaki particioniranom reda čekanja ili temu se sastoji od više fragmentirane. Svaki dio se pohranjuju u različitim razmjenu store, a obrađuje drugu poruku broker. Prilikom slanja poruke da bi se particioniranom reda čekanja ili temu servisa Bus nešto na fragmentirane dodjeljuje poruku. Odabir obavlja se nasumično Bus servisa ili pomoću ključa za particija koje možete odrediti pošiljatelja.

Ako klijent želi dobiti poruku iz particioniranom reda čekanja ili iz pretplate particioniranom teme, upiti Bus servisa sve fragmentirane za poruke, vraća prvu poruku dohvaćene s bilo kojeg razmjenu trgovine primatelju. Servis Bus predmemorije ostale poruke i vraća ih kad primi dodatne prima zahtjeve. Primanje klijent nije svjesni particija; ponašanje klijent dostupnog particioniranom reda čekanja ili teme (, na primjer, čitanje, dovršite, odgoditi deadletter, prefetching) jednak ponašanje običnog entitet.

Postoji bez dodatnih troškova prilikom slanja poruke da biste ili primanje poruka s particioniranom reda čekanja ili temu.

## <a name="enable-partitioning"></a>Omogućivanje particija

Da biste koristili particioniranom redovima i teme s Bus servisa Azure, koristite SDK Azure 2.2 ili novija verzija ili navedite `api-version=2013-10` u vašem HTTP zahtjeva.

Možete stvoriti servisa Bus redovima i teme u veličina 1, 2, 3, 4 i 5 GB (zadano je 1 GB). S particija omogućena, servis Bus stvara 16 particije za svaki GB koju navedete. Kao takve, ako stvorite red čekanja koji je 5 GB po veličini, sa 16 particije veličinu Maksimalna reda čekanja postane (5 \* 16) = 80 GB. Maksimalna veličina particioniranom reda čekanja ili temu možete vidjeti tako da pogledate na stavku [Azure portal][].

Da biste stvorili particioniranom reda čekanja ili temu na nekoliko načina. Kada stvorite red ili temu iz aplikacije, možete omogućiti particija za reda čekanja ili temu tako da odnosno svojstvo [QueueDescription.EnablePartitioning][] ili [TopicDescription.EnablePartitioning][] na **true**. Tih svojstava moraju biti postavljene u trenutku reda čekanja ili stvaranja temu. Nije moguće promijeniti tih svojstava na postojeće reda čekanja ili temu. Ako, na primjer:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Osim toga, možete stvoriti particioniranom reda čekanja ili teme u Visual Studio ili [Azure portal][]. Kada stvorite novi red ili temu na portalu, postavite mogućnost **Omogući particija** u plohu **opće postavke** reda čekanja ili tema prozor **Postavke** na **true**. U Visual Studio, kliknite **Omogući particija** potvrdni okvir u dijaloškom okviru **Novi red** ili **Novu temu** .

## <a name="use-of-partition-keys"></a>Korištenje ključeva partition

Kada je poruka enqueued u particioniranom reda čekanja ili temu, servis Bus provjerava prisutnosti ključa particije. Ako se pronađe jednu, odabire fragment na temelju te tipke. Ako ne pronađete ključ particije, odabire dijelom koji se temelji na interni algoritam.

### <a name="using-a-partition-key"></a>Pomoću tipke partition

Neke scenarije, kao što su sesije ili transakcije, potreban je poruke za pohranu u određeni dio. Sve scenarija zahtijevaju korištenje ključ particije. Sve poruke koje koriste isti ključa particija su dodijeljene iste fragment. Ako je privremeno nedostupan fragment, servis Bus vraća pogrešku.

Ovisno o scenariju, svojstva različite poruke koriste kao particija ključ:

**ID sesije**: Ako poruka je svojstvo [BrokeredMessage.SessionId][] postavljeno, a zatim servis Bus koristi to svojstvo kao ključ particije. Na taj način, sve poruke koje se nalaze u istoj sesiji rješava isti broker poruke. Time se omogućuje Bus usluga da bi poruke redoslijed kao i dosljednost stanja sesije.

**PartitionKey**: Ako poruka sadrži svojstvo [BrokeredMessage.PartitionKey][] , ali ne [BrokeredMessage.SessionId][] skup svojstava, a zatim servis Bus koristi to svojstvo [PartitionKey][] kao ključ particije. Ako poruka sadrži [ID sesije][] i postavljenim svojstvima [PartitionKey][] , oba svojstva moraju biti jednake. Ako je svojstvo [PartitionKey][] postavljeno na neku drugu vrijednost od svojstva [ID sesije][] , servis Bus vraća iznimku **InvalidOperationException** . Svojstvo [PartitionKey][] treba koristiti ako pošiljatelja šalje koje nisu sesiju umu transakcijskih poruke. Tipku particija osigurava da sve poruke koje se šalju u sklopu transakcije rješava isti razmjenu broker.

**MessageId**: Ako reda čekanja ili tema je svojstvo [QueueDescription.RequiresDuplicateDetection][] postavljeno na **true** i nisu postavljena svojstva [BrokeredMessage.SessionId][] ili [BrokeredMessage.PartitionKey][] , a zatim svojstvo [BrokeredMessage.MessageId][] služi kao tipku particije. (Imajte na umu biblioteke web-mjesto Microsoft .NET i u okvir za AMQP automatski dodjelu ID poruke ako slanje aplikacija ne.) U ovom slučaju sve kopije ista poruka rješava isti broker poruke. Time se omogućuje Bus servisa za otkrivanje i uklanjanje duplikata poruka. Ako svojstvo [QueueDescription.RequiresDuplicateDetection][] ne postavite na **true**, Bus servisa ne preporučujemo svojstvo [MessageId][] kao particija ključ.

### <a name="not-using-a-partition-key"></a>Ne koristite tipke partition

U odsutnost ključa particija servisa Bus distribuira poruke kružnog način da biste sve fragmente particioniranom reda čekanja ili temu. Ako odabrani dio nije dostupna, servis Bus dodjeljuje poruku različite fragment. Na taj način postupak slanja uspješnog unatoč privremene nedostupnosti razmjenu trgovine.

Da bi se dobilo servisa Bus dovoljno vremena za enqueue poruke u drugu fragment, [MessagingFactorySettings.OperationTimeout][] vrijednost određen klijenta koji se šalje poruku mora biti veći od 15 sekundi. Preporučuje se da postavite svojstvo [OperationTimeout][] na zadanu vrijednost od 60 sekundi.

Imajte na umu da ključ particija "će" poruku da biste određeni dio. Ako razmjenu pohrane koja sadrži ovaj fragment nije dostupan, servis Bus vraća pogrešku. U odsutnost ključa particija Bus servisa možete odabrati različite fragment i operacija uspije. Dakle, preporučuje se da ne unesete ključ particije osim ako je potrebno.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Dodatne teme: korištenje transakcija s particioniranom entiteti

Poruke koje se šalju u sklopu transakcije morate navesti ključ particije. To može biti jedna od sljedećih svojstava: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]ili [BrokeredMessage.MessageId][]. Sve poruke koje se šalju kao dio iste transakcije morate navesti isti particija ključa. Ako pokušate da biste poslali poruku bez ključa particija unutar transakcije, servis Bus vraća iznimku **InvalidOperationException** . Ako pokušate poslati više poruka u istom transakcijom koji imaju različite particije tipke, servis Bus vraća iznimku **InvalidOperationException** . Ako, na primjer:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Ako bilo koje od svojstava koja služi kao particija ključ postavite servis Bus će poruku da biste određeni dio. To se događa hoće li se koristi transakcije. Preporučuje se da ne navedete ključa particije ako nije potreban.

## <a name="using-sessions-with-partitioned-entities"></a>Sesije pomoću particioniranom entiteti

Da biste poslali transakcijskih poruke sesiju umu temu ili red, poruku morate imati postavljeno svojstvo [BrokeredMessage.SessionId][] . Ako je svojstvo [BrokeredMessage.PartitionKey][] navedena kao i, mora biti jednak svojstvo [ID sesije][] . Ako se razlikuju, servis Bus vraća iznimku **InvalidOperationException** .

Za razliku od običnog redova (koji nisu-particije) ili teme, nije moguće koristiti jedna transakcija za slanje poruka više različitih sesija. Ako je pokušaj, servis Bus vraća iznimku **InvalidOperationException **. Ako, na primjer:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatskog poruka prosljeđivanje s particije entiteti

Azure servisa Bus podržava prosljeđivanje automatskog poruka šalje, Prima ili između particioniranom entiteti. Da biste omogućili automatsko poruke prosljeđivanja, postavite svojstvo [QueueDescription.ForwardTo][] na red izvora ili pretplate. Ako poruke određuje particija ključ ([ID sesije][], [PartitionKey][]ili [MessageId][]), te tipke particija koristi se za odredišni entitet.

## <a name="considerations-and-guidelines"></a>Razmatranja i smjernice

- **Visoke dosljednost značajke**: Ako entitet koristi značajke kao što su sesija, duplikata ili eksplicitne kontrolu nad particija ključ, a zatim razmjenu operacije uvijek usmjeriti određene dijelove. Ako bilo koji od na fragmentirane sučelje velikog prometa ili podlozi spremište je dobro, neće uspjeti te operacije, a smanjena je dostupnost. Ukupni, dosljednost je li i dalje mnogo veća od entiteti koji nisu particije samo podskup promet je postoje neki problemi, umjesto sve promet.
- **Upravljanje**: operacija kao što su stvaranje, ažuriranje i brisanje moraju izvršiti na fragmentirane entitet. Ako je fragmente dobro je može rezultirati pogrešaka za te operacije. Za operaciju Dohvati podatke kao što su poruke broji morate zbrojiti iz svih fragmentirane. Ako je fragmente dobro stanje dostupnosti entitet prijavljuje kao ograničena.
- **Niska glasnoća poruke scenarije**: takve scenarije, osobito kada pomoću protokola HTTP, možda ćete morati izvršiti više primanje operacije da biste dobili sve poruke. Primanje zahtjeva za podršku, sučelje na primanje izvodi na svim fragmentirane i predmemorira sve odgovore koje ste primili. Zahtjev za kasnije primanja putem iste veze bi im ovaj predmemoriranje i primanje latencies bit će niže. Međutim, ako postoji više veza ili koristite HTTP, uspostavlja nove veze za svaki zahtjev. Kao takav postoji ne jamči da ga želite krenuti na istom čvor. Ako sve postojeće poruke zaključan i spremiti u drugom sučelje, operacija primanje vraća **null**. Naposljetku isteka poruke, a zatim ih ponovno možete primati. Preporučuje se da produžite HTTP.
- **Pregled/uvid poruke**: PeekBatch uvijek vratiti se broj poruka naveden u [svojstvu MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx). Postoje dva uobičajena razloga za to. Jedan od razloga je da je agregatnu veličinu zbirka poruka veća od maksimalne veličine 256KB. Još jedan razlog je da ako redu ili tema ima [EnablePartitioning svojstvo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) postavite na **true**, particija možda nemate dovoljno poruke da biste dovršili tražene broj poruka. Općenito govoreći, ako aplikacija želi dobiti od određenog broja poruka, ga nazovite [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) pritišćite tipku TAB dok ne može vidjeti taj broj poruka ili nema više poruka da biste nakratko. Dodatne informacije, uključujući primjere koda, pročitajte članak [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) ili [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Najnoviji dodane značajke

- Dodavanje ili uklanjanje pravila sad podržan s particioniranom entiteti. Razlikuje od koje nisu particije entiteti, te operacije nisu podržane u odjeljku transakcije. 
- AMQP sad podržan za slanje i primanje poruka iz particioniranom entitet.
- AMQP sada podržava sljedeće postupke: [Grupe za slanje](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Primanje seriju](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [primanje po redni broj](https://msdn.microsoft.com/library/azure/hh330765.aspx), [nakratko](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Obnoviti Lock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Raspored poruke](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Poništi zakazano poruku](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Dodavanje pravila](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Uklanjanje pravila](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Zaključavanje obnoviti sesiju](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Stanje sesije skup](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Stanje sesije Get](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Nakratko sesiju poruke](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)i [Nabrajanje sesije](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Ograničenja particioniranom entiteti

Trenutno servisa Bus nameće sljedeća ograničenja na particioniranom redovima i temama:

-   Particioniranom redovima i teme ne podržava slanje poruka koje pripadaju različitim sesije u jednom transakcije.
-   Servis Bus trenutno omogućuje do 100 particioniranom redova ili teme po prostor naziva. Svaki particioniranom reda čekanja ili temu broji prema kvote 10 000 entiteti po prostor za naziv (ne primjenjuju se na Premium sloju).

## <a name="next-steps"></a>Daljnji koraci

U odjeljku rasprava o da biste saznali više o stvaranju particija razmjenu entiteti [AMQP 1.0 podrška za servis Bus particije redova i teme][] . 

  [Servis Bus arhitekture]: service-bus-architecture.md
  [Portal za Azure]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [ID sesije]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [AMQP 1.0 podrška za servis Bus particije redova i teme]: service-bus-partitioned-queues-and-topics-amqp-overview.md
