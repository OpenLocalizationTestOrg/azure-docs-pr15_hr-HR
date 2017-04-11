<properties 
    pageTitle="Servis Bus Uparena prostora naziva | Microsoft Azure"
    description="Detalji o implementaciji upareni prostor naziva i trošak"
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Uparena Detalji o implementaciji prostor naziva i troškova posljedice

Metoda [PairNamespaceAsync][] pomoću instancu komponente [SendAvailabilityPairedNamespaceOptions][] izvodi vidljivi zadatke u vaše ime. Jer postoji su trošak pitanja vezana uz prilikom korištenja značajke, korisno je razumjeti tih zadataka tako da se ponašanje očekivati kada se to događa. U API engages sljedeće automatsko ponašanje u vaše ime:

-   Stvaranje reda čekanja zaostale.
-   Stvaranje objekta [MessageSender][] koji razgovara redova ili teme.
-   Kada SMS entitet postane dostupan, šalje pomoću naredbe ping poruke entitet za prepoznavanje situacije u taj entitet ponovo postane dostupna.
-   Po želji stvara skupa "pumps poruka" koje poruke premjestiti s redova zaostale primarni redova.
-   Koordinate zatvaranja/s kvarom [MessagingFactory][] instanci primarnih i sekundarnih.

Ta značajka funkcionira na visoku razinu na sljedeći način: kada je dobar primarni entitet, bez promjene u ponašanju pojavljuju. Kada trajanje [FailoverInterval][] minutama i primarni entitet vidi nije uspjelo šalje nakon koje nisu prolaznim [MessagingException][] ili [TimeoutException][]pojavljuje se na sljedeći način:

1.  Pošaljite onemogućene su operacije da biste primarni entitet, a zatim sustav pings primarni entitet dok Pingovi možete uspješno isporučene.

2.  Red čekanja slučajni zaostale odabran.

3.  Objekti [BrokeredMessage][] usmjeruje u odabranom zaostale red.

1.  Postupak slanja čekanja za odabrani zaostale ne uspije, taj red se povlače iz zakretanje, a potvrđen je u novi red. Saznajte sve pošiljatelja u instancu [MessagingFactory][] pogreške.

Sljedeća slika opisuju niz. Najprije pošiljatelj šalje poruke.

![Upareni prostora naziva][0]

Nakon pogreška da biste poslali primarni red pošiljatelj započinje slanja poruka slučajno odabranom zaostale red. Istodobno započinje ping zadatka.

![Upareni prostora naziva][1]

Sada na i dalje u redu čekanja za sekundarne i poruka nije isporučena primarni reda čekanja. Kada primarni reda čekanja dobar ponovno, barem jedan mora biti pokrenut proces u syphon. Na syphon nudi poruke iz svih različite redova zaostale entiteti proper odredište (redovima i teme).

![Upareni prostora naziva][2]

Ostatak u ovoj se temi opisuje određene detalje funkcioniranja ove komada.

## <a name="creation-of-backlog-queues"></a>Stvaranje reda čekanja zaostale

Objekt [SendAvailabilityPairedNamespaceOptions][] proslijeđena metodi [PairNamespaceAsync][] označava broj zaostale redova koji želite koristiti. Svaki red zaostale zatim stvara se pomoću sljedećih svojstava izričito postavljanje (sve druge vrijednosti se postavljaju na zadane postavke [QueueDescription][] ):

| Put                                   | [polja naziva primarne] / x servicebus prijenos / [indeksirati] gdje [indeks] je vrijednost u [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | Integracija MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 min                                                                                             |
| EnableDeadLetteringOnMessageExpiration | TRUE                                                                                                 |
| EnableBatchedOperations                | TRUE                                                                                                 |

Ako, na primjer, prvi red zaostale stvorili za prostor za naziv **tvrtke contoso** zove `contoso/x-servicebus-transfer/0`.

Prilikom stvaranja redova kod prvo provjerava postoji li takav reda. Ako ne postoji reda, stvara se reda. Kod ne očistiti "dodatni" zaostale redova. Konkretno, ako aplikacijom primarnog polja naziva **contoso** zatraži pet zaostale redova ali reda zaostale putom `contoso/x-servicebus-transfer/7` postoji, taj dodatni zaostale red i dalje postoji, ali se ne koristi. U sustavu izričito dopušta redova dodatni zaostale postoji koji želite koristiti. Vlasnik prostor naziva ste odgovorni za čišćenje sve Neiskorišteni/neželjene zaostale redova. Razlog za tu odluku je da Bus servisa ne znate koje svrhama postoji za redove u vašem naziva. Osim toga, ako reda postoji navedenog naziva, ali ne zadovoljava pretpostavljenom [QueueDescription][], zatim vaše razloga su vlastite za promjenu zadano ponašanje. Nema jamstva učine za izmjene redova zaostale kod. Provjerite jeste li temeljito testirali promjene.

## <a name="custom-messagesender"></a>Prilagođeni MessageSender

Prilikom slanja, sve poruke prolaze kroz Interna [MessageSender][] objekt koji se obično ponaša kada sve funkcionira i preusmjerava redova zaostale kada stvari "prekinuti." Nakon primanja pogreške koje nisu prolaznim na odbrojavanje vremena. Za prebacivanje je aktivan nakon određenog [vremenski raspon][] koji se sastoji od vrijednosti svojstva [FailoverInterval][] tijekom kojeg se šalju poruke nije uspjelo. U ovom trenutku sljedeće dogodit će se za svaki entitet:

- Zadatak ping izvršava svaki [PingPrimaryInterval][] želite provjeriti jesu li entitet dostupna. Nakon uspješnog ovaj zadatak sav kod klijenta koji koristi entitet odmah se pokreće slanje novih poruka primarni naziva.

- Na [BrokeredMessage][] šalje se mijenjati trebate sjesti u redu čekanja zaostale rezultirat će buduće zahtjeve za slanje u taj isti entitet druge pošiljatelja. Izmjena uklanja neka svojstva iz objekta [BrokeredMessage][] i sprema ih na neko drugo mjesto. Sljedeća svojstva su poništen i dodati u odjeljku novi pseudonim dopuštanja Bus servisa i SDK jednoliko obradu poruka:

| Stari naziv svojstva       | Novi naziv svojstva |
|-------------------------|-------------------|
| ID sesije               | x-ms-ID sesije    |
| TimeToLive              | x ms timetolive   |
| ScheduledEnqueueTimeUtc | x ms put         |

Izvorni put odredište se pohranjuje u porukama kao svojstvo pod nazivom x ms put. Ovaj dizajn omogućuje poruke za mnoge entiteti postojati u jednom zaostale reda. Svojstva su prevesti vratiti tako da na syphon.

Prilagođeni objekt [MessageSender][] mogu se pojaviti problemi kada poruka postići ograničenje 256 KB i prebacivanje je aktivan. Prilagođeni objekt [MessageSender][] zajedno pohranjivati poruke za sve redovima i teme u redovima zaostale. Poruka s mnogo očuvat zajedno unutar redova zaostale smjese taj objekt. Rukovanje među mnogo klijenti koje poznajete međusobno za ujednačavanje opterećenja SDK slučajno izdvajanja jednog zaostale reda čekanja za svaku [QueueClient][] ili [TopicClient][] koje stvorite u kod.

## <a name="pings"></a>Pingovi

Ping poruka da je u prazan [BrokeredMessage][] s njegovo svojstvo [ContentType][] postavite aplikaciju/vnd.ms-servicebus-ping i [TimeToLive][] vrijednost 1 sekunde. Ovaj ping karakteristikama jedan posebno u servis Bus: poslužitelj nikad ne nudi na ping kad sve pozivatelja zatraži [BrokeredMessage][]. Stoga ih ne morate da biste saznali kako dobiti i zanemariti te poruke. Svaki entitet (jedinstveni reda čekanja ili tema) u po [MessagingFactory][] instanci po klijent će biti pinged kada ih se smatraju nije dostupan. Prema zadanim postavkama, to se događa kada u minuti. Ping poruke se smatraju običnog servisa Bus poruke, a može uzrokovati naknade za propusnost i poruke. Čim klijenata otkrili sustav dostupna, poruke se zaustaviti.

## <a name="the-syphon"></a>Na syphon

Barem jedan izvršni program u aplikaciji trebao bi biti aktivno pokrenut na syphon. Na syphon izvodi dugu ankete primanje koje traje 15 minuta. Kada sve entiteti su dostupni, a imate 10 zaostale redovi, aplikacija za smještanje u syphon poziva operacija primanje 40 puta po satu 960 puta dnevno i 28800 vremena u 30 dana. Kada u syphon je aktivno premještanje poruka iz na zaostale primarni red, svaku poruku iskustvo naknade za sljedeće (standardni veličinu poruke i propusnosti naplaćuje u svim fazama):

1.  Slanje u zaostale.

2.  Primanje u zaostale.

3.  Slanje primarni.

4.  Primanje primarni.

## <a name="closefault-behavior"></a>Zatvori/kvara ponašanje

Aplikacije koje hostira syphon, jednom u primarni ili sekundarni [MessagingFactory][] kvarove ili zatvoren bez njegova partnera i koja se pojavila/zatvoriti, a na syphon otkriva to stanje činovi syphon. Ako na drugim [MessagingFactory][] nije zatvoren unutar 5 sekundi, u syphon će kvara još uvijek otvoren [MessagingFactory][].

## <a name="next-steps"></a>Daljnji koraci

Potražite [asinkronog razmjenu uzoraka i visoke dostupnosti][] detaljne rasprave servisa Bus asinkronog razmjene poruka. 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [Vremenski raspon]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asinkrona razmjenu uzoraka i visoke dostupnosti]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
