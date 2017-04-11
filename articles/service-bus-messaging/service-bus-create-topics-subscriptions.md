<properties 
    pageTitle="Stvaranje aplikacije koje koriste servis Bus teme i pretplata | Microsoft Azure"
    description="Uvod u objavljivanje-pretplata mogućnosti koje nudi servis Bus teme i pretplate."
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

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Stvaranje aplikacije koje koriste servis Bus teme i pretplate

Azure servisa Bus podržava skup u oblaku, vezanima uz poruku proizvod tehnologije uključujući red čekanja pouzdanog poruka i durable objavljivanja/pretplate razmjene poruka. U ovom se članku stvara na informacija navedenih u [Stvaranje aplikacija koje koriste redova Bus servisa](service-bus-create-queues.md) i nudi Uvod u rad s mogućnosti objavljivanja/pretplate nudi Bus servisa teme.

## <a name="evolving-retail-scenario"></a>Razvoj scenarij maloprodaja

U ovom se članku nastavlja scenarij maloprodaja koristi u [aplikacijama za stvaranje koje koriste redova Bus servisa](service-bus-create-queues.md). Opoziv te podatke o prodaji iz pojedinačnih točaka od prodaje (Položaj) WBT mora biti proslijeđene u sustav upravljanja zalihama koji koristi tih podataka da biste odredili kada burzovni mora biti popuniti. Svaki terminal Položaj izvješća njegove podatke o prodaji slanjem poruke red **DataCollectionQueue** gdje ostaju dok primitku tako da sustav upravljanja zalihama, kao što je prikazano ovdje:

![Servis Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Da biste poslovanja scenarij novi zahtjev je dodana u sustavu: vlasnik spremište želi omogućiti praćenje kako se izvršava u trgovini u stvarnom vremenu.

Da biste riješili taj zahtjev, sustav morate "dodirnite" toka podataka o prodaji. I dalje želimo svaku poruku poslao WBT Položaj slati sustava upravljanja zalihama kao prije, ali želimo kopija svake poruke koju možete koristiti kako bi se prikazao prikaz nadzorne ploče vlasniku trgovine.

U bilo kojem situaciji kao što je to potrebno svake poruke se utrošiti tako da više osoba, možete koristiti servis Bus *teme*. Teme sadrže uzorak Objavi/pretplata u kojem postane dostupna na jednu ili više pretplata na registriran u temi svaku objavljene poruku. Nasuprot tome, sa redovima svaki je primio poruku jednog korisnika.

Poruke šalju na temu na isti način kao što se šalju reda. Međutim, su ne primljene poruke od temi izravno; primitku iz pretplate. Možete shvatiti pretplate na temu kao virtualne reda čekanja koja prima kopije poruka koje se šalju tu temu. Poruke, isporučene su iz pretplate na isti način kao što su primljene reda.

Vraćanje scenarij maloprodaja reda čekanja zamjenjuje teme, a pretplatu dodate, koje možete koristiti komponenti sustava za upravljanje inventarom. Sustav sada pojavit će se na sljedeći način:

![Servis Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Konfiguracija ovdje jednak način izvodi prethodne utemeljen na red dizajn. To jest, poruke poslane na temu usmjeruje na **zaliha** pretplatu iz koje **Sustav upravljanja zalihama** troši ih.

Da bi se podržava na nadzornoj ploči upravljanja ćemo stvoriti drugu pretplatu na temu, kao što je prikazano ovdje:

![Servis Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

S ovom konfiguracijom svaku poruku iz WBT Položaj postane dostupna na **nadzornoj ploči** i **zaliha** .

## <a name="show-me-the-code"></a>Pokaži mi kod

U članku [Stvaranje aplikacije koje koriste servis Bus redova](service-bus-create-queues.md) opisuje kako registrirati za račun za Azure i stvorite polje naziva servisa. Da biste koristili prostor naziva Bus servisa, aplikacija mora pozivati u sklopu servisa Bus, posebice Microsoft.ServiceBus.dll. Referentni ovisnosti servisa Bus najjednostavnije da biste instalirali servis Bus [Nuget paketa](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Možete pronaći i u sklopu kao dio Azure SDK. Preuzimanje dostupna je na [stranici za preuzimanje Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Stvaranje tema i pretplate

Upravljanje postupci za servis Bus poruka entiteti (redova i objavljivanja/pretplate teme) se izvode putem [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) predmete. Da biste stvorili [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) instancu za određeni prostor naziva potrebne su odgovarajuće vjerodajnice. Servis Bus koristi [Zajednički pristup potpis (SAS)](service-bus-sas-overview.md) na temelju sigurnosnih modela. Klase [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) predstavlja sigurnosni token davatelja s načinima ugrađene tvorničke vraćanje Neki davatelji poznati tokena. Ćemo pomoću metode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) držite SAS vjerodajnice. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) instance zatim konstruirana s osnovne adrese servisa Bus naziva i tokena davatelja usluga.

Klase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) navedene metode možete stvarati, numeriranje i brisanje poruka entiteti. Kod koji je prikazano ovdje prikazuje kako instancu [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) se stvara i koristi za stvaranje **DataCollectionTopic** temu.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Imajte na umu da su prisutne overloads metodu [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) koji omogućuju vam da biste postavili svojstva teme. Na primjer, možete postaviti zadano vrijeme važenja (TTL) vrijednost za poruke poslane na temu. Zatim dodajte pretplate **zaliha** i **nadzorne ploče** .

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Slanje poruka na temu

Za vrijeme izvođenja operacije na servis Bus entiteti; Ako, na primjer, slanje i primanje poruka, aplikacija stvoriti [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekt. Slično klase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , instancu [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) stvara iz osnovnu adresu polje naziva servisa i tokena davatelja usluga.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Poruke poslane i dobili od servisa Bus teme, su instance klase [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Klase sastoji se od skupa standardna svojstva (kao što su [oznake](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) rječnika koji koristi se za čuvanje svojstva aplikacije pa tijelo podataka proizvoljne aplikacije. Aplikaciju možete postaviti tijelo po Prenos u serializable objekte (u sljedećem primjeru prosljeđuje u objektu **SalesData** koja predstavlja podatke o prodaji iz terminal Položaj), koji će se koristiti [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) serijalizirati objekt. Umjesto toga se može pružati [strujanje](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) objekta.

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Da biste poslali poruke na temu najjednostavnije da biste koristili [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) da biste stvorili objekt programa [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) izravno iz [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) instance.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Primanje poruka iz pretplate

Slično korištenju redove, primati poruke iz pretplate možete koristiti [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekt koji stvarate izravno s [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) pomoću [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx). Koristite neku od dvije različite primanje načini (**ReceiveAndDelete** i **PeekLock**), kao što je navedeno u [aplikacijama za stvaranje koje koriste redova Bus servisa](service-bus-create-queues.md).

Imajte na umu da kada stvarate [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) za pretplate, parametar *entityPath* obrasca `topicPath/subscriptions/subscriptionName`. Stoga, da biste stvorili [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) za pretplatu **zaliha** **DataCollectionTopic** teme, *entityPath* mora biti postavljeno na `DataCollectionTopic/subscriptions/Inventory`. Kod pojavit će se na sljedeći način:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Filtri za pretplatu

Dosad u ovom scenariju sve poruke poslane na temu učinio dostupnima za sve registrirane pretplate. Ovdje ključa izraz "postane dostupna." Dok je servis Bus pretplate vidjeli sve poruke poslane na temu, možete kopirati samo podskupa od tih poruka red virtualne pretplate. To je izvršiti pomoću pretplate *filtre*. Kada stvorite pretplatu, možete navesti izraz filtra u obliku predikata SQL92 stil koji radi iznad svojstva poruke, svojstva sustava (na primjer, [oznake](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) i svojstva aplikacija, kao što su **Nazivtrgovine** u prethodnom primjeru.

Razvoj scenarij da biste to prikazuju drugi spremište je potrebno dodati u našem maloprodaja scenarij. Podaci o prodaji iz svih WBT Položaj iz obje trgovina i dalje imate usmjeriti sustav upravljanja zalihama središnje, ali spremište Upravitelj pomoću alata za nadzornu ploču samo je zanimati performanse spremišta. Koristite pretplatu filtriranje da biste to postigli. Imajte na umu da kad WBT Položaj objavite poruke, oni svojstvo **Nazivtrgovine** aplikacije poruku. Dani dva trgovine, na primjer **Redmond** i **Seattle**WBT Položaj u na Redmond pohranite pečata svoje podatke o prodaji poruke s na **Nazivtrgovine** jednako **Redmond**, dok je u Seattle spremište Položaj WBT pomoću **Nazivtrgovine** jednako **Seattle**. Da biste vidjeli podatke iz njegov Položaj WBT samo želi upravitelja spremišta Redmond spremišta. Sustav pojavit će se na sljedeći način:

![Servis Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Da biste postavili proizvodnog postupka, možete stvoriti pretplate **nadzorne ploče** na sljedeći način:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Pomoću ovog filtra za pretplatu samo poruka s svojstvo **Nazivtrgovine** postavljeno na **Redmond** kopirat će se virtualne red za pretplatu **nadzorne ploče** . Postoji mnogo više pretplata filtriranja, no. Aplikacija može imati više pravila za filtriranje po pretplati uz mogućnost da biste izmijenili svojstva poruke, kao što je prosljeđuje virtualne čekanja za pretplatu na.

## <a name="summary"></a>Sažetak

Sve razloga za korištenje stavljanje što je opisano u [Stvaranje aplikacija koje koriste servis Bus redova](service-bus-create-queues.md) se primijeniti i na teme, posebice:

- Vremenski decoupling – proizvođača poruke i korisnici ne moraju biti online istovremeno.

- Učitavanje ujednačavanje – peaks u učitavanja izglađenim po temi Omogućivanje dosta aplikacija biti dodijeljena average opterećenja umjesto Vršna Učitaj.

- Uravnoteženje opterećenja – slično reda, možete imati više konkurentnog koje korisnici priključuje u jednom pretplate, u svaku poruku isporučeni na samo jednu od koje korisnici, čime ćete za ujednačavanje opterećenja.

- Su coupling – možete poslovanja razmjenu mreže bez utjecaja na postojeće krajnje točke; Ako, na primjer, pretplate dodavanjem ili promjenom filtrira se tema da biste dopustili novi korisnici.

## <a name="next-steps"></a>Daljnji koraci

Potražite u članku [Stvaranje aplikacija koje koriste servis Bus redova](service-bus-create-queues.md) informacije o načinu korištenja redova u maloprodajnoj scenariju Položaj.