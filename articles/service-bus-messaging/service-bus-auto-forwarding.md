<properties 
    pageTitle="Automatsko prosljeđivanje Bus usluge razmjene poruka entiteti | Microsoft Azure"
    description="Kako se lanac reda ili pretplatu na drugom redu čekanja ili temu."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Ulančavanje entiteti Bus servisa s automatsko prosljeđivanje

*Automatsko prosljeđivanje* značajka omogućuje lanac redu ili pretplatu na drugom redu čekanja ili temu koja je dio isti prostor naziva. Kada je omogućen automatsko prosljeđivanje, servis Bus automatski uklanja poruke koje se smještaju u prvi red ili pretplate (izvor) i stavlja ih u drugom redu ili teme (odredište). Imajte na umu da je moguće je poslati poruku odredišni entitet izravno. Osim toga, nije moguće lanac subqueue, kao što je red čekanja deadletter drugom redu ili temu.

## <a name="using-auto-forwarding"></a>Korištenje automatskog prosljeđivanja

Automatsko prosljeđivanje možete omogućiti tako da postavite [QueueDescription.ForwardTo][] ili [SubscriptionDescription.ForwardTo][] svojstva za objekte [QueueDescription][] ili [SubscriptionDescription][] izvora, kao u sljedećem primjeru.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Odredišni entitet moraju se nalaziti u trenutku stvara se izvorišni entitet. Ako ne postoji odredišni entitet, servis Bus vraća iznimku kada se od vas zatraži da biste stvorili izvorišni entitet.

Automatsko prosljeđivanje možete koristiti da biste skalirali out pojedinačnu temu. Servis Bus ograničava [broj pretplate na temu za dani](service-bus-quotas.md) 2000. Omogućuje postavljanje dodatnih pretplate stvaranjem teme druge razine. Imajte na umu da čak i ako nije povezan putem servisa Bus ograničenje broja pretplate, dodavanje druge razine teme možete poboljšati cjelokupan propusnost teme.

![Automatsko prosljeđivanje scenarija][0]

Automatsko prosljeđivanje možete koristiti i da biste decouple poruke pošiljatelja primatelja. Na primjer, razmotrite ERP sustava koji se sastoji od tri modula: Obrada narudžbe, upravljanje inventarom i upravljanje klijentima odnosa. Svaki od tih moduli generira poruka koje nisu enqueued u odgovarajuće temu. Alice i Teo nisu prodajni predstavnici koji žele u svim porukama koje se odnose na svojim klijentima. Za primanje tih poruka, Alice i Teo svaki stvaranje osobnog reda čekanja i pretplatu za svaku temu ERP koja automatsko prosljeđivanje svih poruka na svojim reda čekanja.

![Automatsko prosljeđivanje scenarija][1]

Ako Alice vodi odmor, svoj osobni reda čekanja, a ne u temi ERP, unosi prema gore. U ovom scenariju jer je prodajni predstavnik sadrži primljene poruke, nema teme ERP ikad dosegne kvote.

## <a name="auto-forwarding-considerations"></a>Automatsko prosljeđivanje pitanja

Ako odredišni entitet akumulirao velik broj poruka i premašuje kvotu ili je onemogućen odredišni entitet, izvorišni entitet dodaje poruke [reda čekanja reagira slovo](service-bus-dead-letter-queues.md) dok je prostor na odredištu (ili je entitet ponovno omogućiti). Ta poruka će i dalje uživo u redu čekanja reagira slovo te morate izričito primati i ih obrađivati iz reda reagira pismo.

Kada Ulančavanje zajedno pojedinačnim temama da biste dobili složeni temu s mnogo pretplate, preporučuje se imate li moderiranje broj pretplate na temu prve razine i mnogo pretplate na teme druge razine. Na primjer, temu prve razine s 20 pretplatama svaku od njih povezane druge razine temu s 200 pretplatama omogućuje veću propusnost od prve razine temu s 200 pretplatama svaki povezane druge razine temu s 20 pretplate.

Računi servisa Bus jedan postupak za svaki proslijeđene poruke. Ako, na primjer, slanja poruke na temu s 20 pretplatama svaku od njih konfiguriran za automatsko prosljeđivanje poruka na drugom redu čekanja ili temi naplate kao 21 operacije ako sve pretplate prve razine primaju kopiju poruke.

Da biste stvorili na pretplatu koja je povezane drugom redu ili temu, Stvaratelj pretplate na morate imati dozvole za **Upravljanje** izvorišni i odredišni entitet. Slanje poruka na temu izvor potreban je samo dozvole za **Slanje** o temi izvora.

## <a name="next-steps"></a>Daljnji koraci

Detaljne informacije o automatsko prosljeđivanje potražite u sljedećim temama reference:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Da biste saznali više o poboljšanja performansi Bus servisa, potražite u članku [Partitioned porukama entiteti][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Particioniranom entiteti za razmjenu poruka]: service-bus-partitioning.md