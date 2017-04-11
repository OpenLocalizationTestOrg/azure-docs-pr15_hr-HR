<properties
    pageTitle="Kako pomoću servisa Bus redova pomoću Java | Microsoft Azure"
    description="Saznajte kako pomoću servisa Bus redova u Azure. Primjere koda pisane Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Upute za korištenje servisa Bus redovi

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

U ovom se članku opisuje način korištenja servisa Bus redova. Primjere zapisuju u Java i korištenje [Azure SDK Java][]. Scenariji u kojima je moguće uvrstiti **Stvaranje reda čekanja**, **Slanje i primanje poruka**i **Brisanje reda čekanja**.

## <a name="what-are-service-bus-queues"></a>Što su redova Bus servis?

Servis Bus redova podržava **brokered porukama** modela komunikacije. Prilikom korištenja redova komponenti distribuirane aplikacije komunikaciju izravno međusobno; Umjesto toga ih razmjene poruka putem red koji funkcionira kao posrednik (broker). Proizvođač poruke (pošiljatelj) ruke isključivanje poruke u red i zatim nastavlja njegov obrada.
Asinkrono, potrošača poruke (tekstnog okvira) povlači poruka iz reda čekanja i obrađuje ga. Producentu nema ćete morati pričekati odgovor od korisnik da bi se i dalje obraditi i poslati daljnje poruke. Redovi nude **prvog u, prvi Out (FIFO)** Isporuka poruke jedan ili više konkurentnog njezinoj. To jest, poruke se obično prima i obrađuje primatelje redoslijedom u kojem su dodane u redu čekanja, a svakoj poruci je prima i obrađuje potrošača samo jedne poruke.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Servisa Bus redova su općenite namjene tehnologija koje je moguće koristiti za razna scenarije:

- Komunikacija između web- a tempiranja ulogama u više razina Azure aplikacije.
- Komunikacije između aplikacija za lokalni i Azure hostira aplikacije hibridnog rješenja.
- Komunikacija između komponenti aplikacije raspodijeljeno radi lokalnog u različitim tvrtke ili ustanove ili odjela tvrtke ili ustanove.

Pomoću redova omogućuje skaliranje izvan vaše aplikacije i omogućivanje otpornost u arhitekturi.

## <a name="create-a-service-namespace"></a>Stvorite polje naziva servisa

Da biste počeli koristiti servis Bus redova u Azure, prvo morate stvoriti prostor naziva. Prostor naziva nudi scoping spremnik adresiranja resursima servisa Bus unutar vaše aplikacije.

Da biste stvorili prostor naziva:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Provjerite jeste li instalirali [Azure SDK Java][] prije stvaranje ovaj uzorak. Ako koristite Eclipse, možete instalirati [Azure komplet alata za Eclipse][] koja obuhvaća Azure SDK Java. Zatim možete dodati **Microsoft Azure biblioteke za Java** projekta:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Dodajte sljedeće `import` naredbe na vrh datoteka jezika Java:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Stvaranje reda čekanja

Upravljanje operacije za servis Bus redove može izvoditi putem **ServiceBusContract** predmete. **ServiceBusContract** objekt konstruirana s odgovarajućim konfiguracijom koji encapsulates token SAS s dozvolama za upravljanje ga, a klase **ServiceBusContract** je jedini točka komunikaciju s Azure.

Klase **ServiceBusService** navedene metode možete stvarati, numeriranje i redovima. U sljedećem primjeru pokazuje kako se **ServiceBusService** objekta možete koristiti za stvaranje reda pod nazivom "TestQueue" s poljem naziva pod nazivom "HowToSample":

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Načina na **QueueInfo** koje omogućuju svojstva čekanja za moguće je počeo gledati (na primjer: da biste postavili zadanu vrijeme važenja (TTL) vrijednost da se primjenjuje na poruke poslane reda). Sljedeći primjer pokazuje kako stvoriti reda pod nazivom `TestQueue` s maksimalne veličine 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Imajte na umu da možete koristiti metodu **listQueues** na objekte **ServiceBusContract** da biste provjerili je li reda čekanja s navedenim nazivom već postoji u polje naziva servisa.

## <a name="send-messages-to-a-queue"></a>Slanje poruke u red

Da biste poslali poruku reda Bus servisa, aplikacija dohvaća **ServiceBusContract** objekt. Sljedeći kod služi slanje poruke u `TestQueue` reda čekanja prethodno stvorena u na `HowToSample` prostor naziva:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Poruke poslane i dobili od servisa Bus redova su instance klase [BrokeredMessage][] . Objekti [BrokeredMessage][] imati skup standardna svojstva (kao što su [oznake](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) rječnika koji koristi se za čuvanje prilagođenu aplikaciju konkretnih svojstava i tijelo podataka proizvoljne aplikacije. Aplikaciju možete postaviti tijelo poruke prosljeđivanjem serializable objekata u Graditelj [BrokeredMessage][], a odgovarajuće serijalizatora koristit će se zatim serijalizirati objekt. Osim toga, možete unijeti **java. IO. InputStream** objekt.

Sljedeći primjer pokazuje kako poslati pet testiranje poruke u `TestQueue` **MessageSender** ćemo dobiti u prethodni isječak koda:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Servis Bus redova podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u redu čekanja, ali postoji u kapaciteta ukupnu veličinu poruke zaključale reda. Ovu veličinu reda čekanja definirana je u trenutku stvaranja s gornju granicu 5 GB.

## <a name="receive-messages-from-a-queue"></a>Primanje poruka iz reda čekanja

Glavni način poruka iz reda je koristiti **ServiceBusContract** objekt. Primljene poruke možete raditi u dva različita načina rada: **ReceiveAndDelete** i **PeekLock**.

Korištenje načina **ReceiveAndDelete** primite je operacija jednim snimka - odnosno kada servis Bus primi zahtjev za poruku u redu čekanja, označava poruku kao se potrošena i vraća aplikaciji. Način **ReceiveAndDelete** (što je zadani način) je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu.
Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

U načinu **PeekLock** primanje postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da biste spriječili drugi korisnici ga prima i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete **Brisanje** primljene poruke. Kada je servis Bus vidi poziva za **Brisanje** , označavanje poruke kao što je u tijeku potrošena će se i ukloniti iz reda.

Sljedeći primjer prikazuje način na koji poruke mogu primati i obrađuju **PeekLock** načinu rada (ne zadani način). U primjeru u nastavku ne beskonačno Ponavljaj i obrađuje poruke po primitku u našem "TestQueue":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako rukovati ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako je primatelj aplikacija nije moguće obraditi poruku zbog nekog razloga, pa je možete nazvati metodu **unlockMessage** na primljene poruke (umjesto metode **deleteMessage** ). Time će Bus servisa da biste otključali poruku u redu čekanja i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan u redu čekanja, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (npr., ako ruši se aplikacija), a zatim servis Bus će automatski otključati poruku i učiniti je dostupnom koji se dobiva ponovno.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se izda zahtjev za **deleteMessage** , zatim poruku će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obrade**, odnosno svake poruke obradit će se barem jednom no u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. To je često postići pomoću metode **getMessageId** poruke, koju će ostaju iste preko pokušao.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus redovi, potražite u članku [redova, teme i pretplate][] dodatne informacije.

Dodatne informacije potražite u [Centru za razvojne inženjere Java](/develop/java/).


  [Azure SDK Java]: http://azure.microsoft.com/develop/java/
  [Azure komplet alata za Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Redova, teme i pretplate]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

