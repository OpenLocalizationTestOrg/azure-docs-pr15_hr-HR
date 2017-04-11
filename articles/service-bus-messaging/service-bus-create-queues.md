<properties 
    pageTitle="Pisanje aplikacije koje koriste servis Bus redova | Microsoft Azure"
    description="Kako se napišite jednostavne utemeljen na red aplikaciju koja koristi Bus servisa."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Stvaranje aplikacije koje koriste redova Bus servisa

U ovoj se temi opisuje servisa Bus redovima i prikazuje kako napišite jednostavne utemeljen na red aplikaciju koja koristi Bus servisa.

Zamislite iz svijeta Maloprodaja u kojem morate moguće usmjeriti podatke o prodaji iz pojedinačne WBT Point-of-Sale (Položaj) u sustav upravljanja zalihama koja koristi podatke da biste odredili kada burzovni mora biti popuniti. Rješenje koristi servis Bus poruka za komunikaciju između na WBT i sustav upravljanja zalihama, kao što je prikazano na sljedećoj slici:

![Servis Bus redova slika 1](./media/service-bus-create-queues/IC657161.gif)

Svaki terminal Položaj izvješća njegove podatke o prodaji slanjem poruke **DataCollectionQueue**. Te poruke ostaju u ovom slijedu dok se ne mogu se dohvaćaju sustav upravljanja zalihama. Ovaj uzorak je često termed *asinkronog poruka*, jer terminal Položaj mora proći odgovori iz sustava upravljanja zalihama da biste nastavili obrada.

## <a name="why-queuing"></a>Zašto čekanja?

Prije nego što smo vidjeli koda potrebnog za ovu aplikaciju za postavljanje, razmotrite prednosti korištenja reda u tom slučaju umjesto da WBT Položaj izravno razgovarati (sinkronizirano) da biste sustav upravljanja zalihama.

### <a name="temporal-decoupling"></a>Vremenski decoupling

S asinkronog razmjenu uzorak proizvođača i korisnici ne moraju biti u mreži u isto vrijeme. Infrastruktura za razmjenu pouzdano sprema poruke dok dosta strana spreman primati. To znači da komponente distribuirane aplikacije mogu biti povezani, ili po želji; Ako, na primjer, za održavanje ili zbog neočekivanog komponente bez utjecaja na cijeli sustav. Osim toga, dosta aplikacija može imati samo biti u mreži tijekom vremena određene dana. Ako, na primjer, u ovom scenariju maloprodaja sustav upravljanja zalihama samo možda dolazi online nakon dana tvrtke.

### <a name="load-leveling"></a>Učitavanje uravnoteživanju

U mnogim aplikacijama sustavu opterećenje mijenja se s vremenom, dok je obrada vrijeme potrebno za svaku jedinicu poslovne obično konstante. Intermediating proizvođača poruke i korisnici s reda, to znači da dosta aplikacije (tempiranja) samo mora biti dodijeljena da servisa average opterećenja umjesto Vršna Učitaj. Dubina reda čekanja će rasti i ugovora kao mijenja se dolazne učitavanja. Time izravno novac pogledu količinu infrastrukture potrebne za učitavanje aplikacije.

![Servis Bus redova slika 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Za ujednačavanje opterećenja

Kao što je opterećenje povećava, više radnih procesa moguće dodavati čitati iz radnih reda. Samo jedan od radnih procesa obraditi svaku poruku. Osim toga, ovaj utemeljen na istaknuti za ujednačavanje opterećenja omogućuje najbolju korištenje računala radnih čak i ako se tempiranja računala razlikuju s obzirom na potenciju obrada, kao što je će istaknuti poruke vlastite Maksimalna brzina. Ovaj uzorak je često termed konkurentnog potrošača uzorka.

![Servis Bus redova slika 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Su coupling

Korištenje reda čekanja za Srednja između proizvođača poruke i korisnici poruka sadrži intrinzična su coupling između komponente. Jer proizvođača i korisnici nisu svjesni međusobno povezani, potrošača mogu biti nadograđeni bez učinka na producentu. Osim toga, možete razmjenu topologije poslovanja bez utjecaja na postojeće krajnje točke. Ne možemo ćete navode to više kada ćemo objasniti što objavljivanja/pretplate.

## <a name="show-me-the-code"></a>Pokaži mi kod

Sljedeći odjeljak pokazuje kako pomoću servisa Bus da biste sastavili ove aplikacije.

### <a name="sign-up-for-an-azure-account"></a>Registracija za račun za Azure

Potreban vam je račun za Azure da biste počeli funkcionirati s Bus servisa. Ako nije već postoji, možete se prijaviti za besplatan račun [ovdje](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Stvaranje prostor naziva

Kada imate pretplatu, možete [stvoriti novi prostor za naziv](service-bus-create-namespace-portal.md). Prostor za svaki naziv služi kao scoping spremnik za skup entiteti Bus servisa. Dajte novi prostor za naziv jedinstven naziv sve račune Bus servisa. 

### <a name="install-the-nuget-package"></a>Instalirajte paket NuGet

Da biste koristili prostora za naziv Bus servisa, aplikacija mora pozivati u sklopu servisa Bus, posebice Microsoft.ServiceBus.dll. Možete pronaći ovu skupa kao dio sustava Microsoft Azure SDK i preuzimanja dostupna je na [stranici za preuzimanje Azure SDK](https://azure.microsoft.com/downloads/). Međutim, [paket NuGet Bus servis](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je Najlakši način da biste dobili Bus API servisa, a da biste konfigurirali aplikaciju sa svim ovisnosti Bus servisa.

### <a name="create-the-queue"></a>Stvorite red čekanja

Upravljanje postupci za servis Bus poruka entiteti (redova i objavljivanja/pretplate teme) se izvode putem [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) predmete. Servis Bus koristi [Zajednički pristup potpis (SAS)](service-bus-sas-overview.md) na temelju sigurnosnih modela. Klase [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) predstavlja sigurnosni token davatelja s načinima ugrađene tvorničke vraćanje Neki davatelji poznati tokena. Ćemo pomoću metode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) držite SAS vjerodajnice. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) instance zatim konstruirana s osnovne adrese servisa Bus naziva i tokena davatelja usluga.

Klase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) navedene metode možete stvarati, numeriranje i brisanje poruka entiteti. Kod koji je prikazano ovdje prikazuje kako instancu [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) se stvara i koristi za stvaranje reda čekanja **DataCollectionQueue** .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Imajte na umu da su prisutne overloads metodu [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) koja omogućuju svojstva čekanja za moguće je počeo gledati. Ako, na primjer, možete postaviti zadano vrijeme važenja (TTL) vrijednost za poruke poslane na red.

### <a name="send-messages-to-the-queue"></a>Slanje poruka u redu čekanja

Za vrijeme izvođenja operacije na servis Bus entiteti; Ako, na primjer, slanje i primanje poruka, aplikacija stvoriti [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekt. Slično klase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , instancu [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) stvara iz osnovnu adresu polje naziva servisa i tokena davatelja usluga.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Poruke poslane i dobili od servisa Bus redova su instance klase [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Klase sastoji se od skupa standardna svojstva (kao što su [oznake](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) rječnika koji koristi se za čuvanje svojstva aplikacije pa tijelo podataka proizvoljne aplikacije. Aplikaciju možete postaviti tijelo po Prenos u serializable objekte (u sljedećem primjeru prosljeđuje u objektu **SalesData** koja predstavlja podatke o prodaji iz terminal Položaj), koji će se koristiti [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) serijalizirati objekt. Umjesto toga se može pružati [strujanje](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) objekta.

Slanje poruka na navedeni reda čekanja u našem slučaju **DataCollectionQueue**najjednostavnije da biste koristili [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) da biste stvorili objekt programa [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) izravno iz [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) instance.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Primanje poruka iz reda čekanja

Da biste primili poruke iz reda čekanja, možete koristiti [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekt koji stvarate izravno s [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) pomoću [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx). Primatelje poruke možete raditi u dva različita načina rada: **ReceiveAndDelete** i **PeekLock**. [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) postavljen stvaranja primatelja poruke, kao parametar [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) poziv.


Prilikom korištenja načina **ReceiveAndDelete** , na primanje je operacija jednim snimka; to jest, kada servis Bus primi zahtjev, vama označava da je poruka kao što je u tijeku potrošena i vraća aplikaciji. Način **ReceiveAndDelete** je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruku ako se pojaviti pogreška. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa označiti poruku kao potrošena, prilikom ponovnog pokretanja računala i pokreće ponovno troše poruke je će imati Propušteni poruku koja je potrošena prije na pad sustava.

U načinu **PeekLock** na primanje postaje dvije faze postupak koji omogućuje podržava aplikacija koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da biste spriječili drugi korisnici ga prima i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete [Dovršeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) na primljene poruke. Kada servis Bus vidi [Dovršeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) poziva, označava poruku kao se potrošena.

Mogući su dva rezultata. Najprije, ako aplikacija nije uspio obraditi poruku zbog nekog razloga, ga možete nazvati [ćete](https://msdn.microsoft.com/library/azure/hh181837.aspx) na primljene poruke (umjesto [Dovršeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Zbog toga Bus servisa da biste otključali poruku i učiniti je dostupnom koji se dobiva ponovno isti potrošača ili neki drugi korisnik dovršavanje. Drugo, postoji isteka pridružene zaključavanje i ako aplikacija nije moguće obraditi poruku prije zaključavanje isteka istekne (na primjer, ako ruši se aplikacija), a zatim servis Bus će otključavanje poruku i učiniti je dostupnom koji se dobiva ponovno (zapravo izvršavanja [zbog toga odustati od](https://msdn.microsoft.com/library/azure/hh181837.aspx) operacije po zadanom).

Imajte na umu da ako aplikaciju ruši se nakon obrađuje poruku, ali prije [Dovršeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) zahtjev izdavanja, poruka će biti redelivered aplikaciji prilikom ponovnog pokretanja. To je često termed *Barem jednom* obrada. To znači da svaka poruka obradit će se najmanje jedanput, ali u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, dodatne logike je potreban u aplikaciji da biste otkrili duplikate. To se može postići na temelju svojstvo [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) poruke. Vrijednost tog svojstva ostaje nepromijenjen preko pokušao. To je termed obrada *Samo jedanput* .

Kod koji je prikazano ovdje prima i obrađuje poruke pomoću načina **PeekLock** , koji je zadana vrijednost ako je vrijednost [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) izričito navedena.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>Koristite klijent reda čekanja

Primjeri ranije u ovom odjeljku stvara [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) i [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekti izravno iz [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) za slanje i primanje poruka iz reda čekanja, odnosno. Zamjenski pristup jest korištenje [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) predmete koje podržava i slanje i primanje operacije osim naprednije značajke, kao što su sesije.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove redovi, potražite u članku [Stvaranje aplikacije koje koriste servis Bus teme i pretplate](service-bus-create-topics-subscriptions.md) da biste nastavili ovaj rasprave pomoću mogućnosti objavljivanja/pretplate Bus servisa teme i pretplate.