<properties
    pageTitle="Kako pomoću servisa Bus teme pomoću Java | Microsoft Azure"
    description="Saznajte kako pomoću servisa Bus teme i pretplata u Azure. Primjere koda zapisuju za Java aplikacije."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Upute za korištenje servisa Bus teme i pretplate

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ovaj vodič opisuje način korištenja servisa Bus teme i pretplate. Primjere zapisuju u Java i korištenje [Azure SDK Java][]. Scenariji u kojima je moguće uvrstiti **Stvaranje teme i pretplate**, **Stvaranje filtara za pretplatu**, **slanje poruka na temu**, **dobivate poruku iz pretplate**i **Brisanje teme i pretplate**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Što su teme Bus servisa i pretplate?

Servis Bus teme i pretplata podržava na *objavljivanja/pretplate* poruka komunikacije modela. Prilikom korištenja teme i pretplate, komponente distribuirane aplikacije komunikaciju izravno međusobno; Umjesto toga mogu razmjenjivati poruke putem teme, koja služi kao posrednik.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

In contrast with Bus servis reda čekanja obrađuje svaku poruku tako da jedan korisnik, teme i pretplate pružaju oblik "jedan-prema-više" komunikaciju, pomoću uzorak Objavi/pretplata. Nije moguće registrirati višestruke pretplate na temu. Prilikom slanja poruke na temu ga zatim postane dostupna za svaku pretplatu ručicu/postupak neovisno.

Pretplata na temu nalikuje virtualne reda čekanja koja prima kopije poruka koje su poslane na temu. Po želji možete registrirati filtar pravila za temu na temelju po pretplatu, što vam omogućuje da filtar/ograničiti primiti koje se poruke na temu koja tema pretplate.

Servis Bus teme i pretplata omogućuju mjerilo za obradu velik broj poruka preko velikog broja korisnicima i aplikacijama.

## <a name="create-a-service-namespace"></a>Stvorite polje naziva servisa

Da biste počeli koristiti servis Bus teme i pretplata u Azure, prvo morate stvoriti polje naziva servisa. Prostor naziva nudi scoping spremnik adresiranja resursima servisa Bus unutar vaše aplikacije.

Da biste stvorili prostor naziva:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Provjerite jeste li instalirali [Azure SDK Java][] prije stvaranje ovaj uzorak. Ako koristite Eclipse, možete instalirati [Azure komplet alata za Eclipse][] koja obuhvaća Azure SDK Java. Zatim možete dodati **Microsoft Azure biblioteke za Java** projekta:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Dodajte sljedeće naredbe Uvezi na vrh datoteka jezika Java:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Dodavanje biblioteka Azure za Java vašem putu Sastavi i uključiti u sklopu za implementaciju sustava project.

## <a name="create-a-topic"></a>Stvaranje teme

Upravljanje operacije servisa Bus teme može izvoditi putem **ServiceBusContract** predmete. **ServiceBusContract** objekt konstruirana s odgovarajućim konfiguracijom koji encapsulates token SAS s dozvolama za upravljanje ga, a klase **ServiceBusContract** je jedini točka komunikaciju s Azure.

Klase **ServiceBusService** navedene metode možete stvarati, numeriranje i brisati teme. Sljedeći primjer pokazuje kako se **ServiceBusService** objekta možete koristiti da biste stvorili temi pod nazivom `TestTopic`, s poljem naziva pod nazivom `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Načina na **TopicInfo** koja omogućuju svojstva temu želite postaviti (na primjer: da biste postavili zadanu vrijeme važenja (TTL) vrijednost za primjenu na poruke poslane na temu). Sljedeći primjer pokazuje kako stvarati određenoj temi pod nazivom `TestTopic` s maksimalne veličine 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Imajte na umu da možete koristiti metodu **listTopics** na objekte **ServiceBusContract** da biste provjerili je li temu s navedenim nazivom već postoji u polje naziva servisa.

## <a name="create-subscriptions"></a>Stvaranje pretplate

Pretplata na temu i stvaraju se pomoću klase **ServiceBusService** . Pretplate se nazivaju i može imati neobavezno filtar koji ograničava skup poruka proslijeđena virtualne red svoje pretplate.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Stvaranje pretplate s filtrom zadani (MatchAll)

Filtar **MatchAll** je zadani filtar koji se koristi ako nema filtra nije naveden stvaranja novu pretplatu. Kada se koristi filtar **MatchAll** , sve poruke koje se objavljuju u temi spremaju se u svoje pretplate virtualne red. Sljedeći primjer stvara pretplatu pod nazivom "AllMessages" i koristi zadani **MatchAll** filtar.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Stvaranje pretplate s filtrima

Možete stvoriti i filtri koje omogućuju doseg koji treba poruke poslane na temu prikazuju se u određenoj temi pretplate.

Većina fleksibilne vrstu filtra podržava pretplate je u [SqlFilter][], koji implementira podskup SQL92. Filtri SQL rade na svojstva poruke koje su objavljene na temu. Dodatne informacije o izrazima koje je moguće koristiti s filtrom SQL pregledajte sintaksa [SqlFilter.SqlExpression][] .

Sljedeći primjer stvara pretplatu pod nazivom `HighMessages` s objektom [SqlFilter][] koji samo odabire poruke koje sadrže prilagođeno svojstvo **MessageNumber** veći od 3:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Isto tako, sljedeći primjer stvara pretplatu pod nazivom `LowMessages` s objektom [SqlFilter][] koji samo odabire poruke koje sadrže **MessageNumber** svojstvo manje od ili jednako 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Kada je sada za slanje poruka `TestTopic`, će uvijek isporuku za primatelje pretplaćeni na `AllMessages` pretplate, i selektivno isporučeno primatelje pretplaćeni na na `HighMessages` i `LowMessages` pretplate (ovisno o tome sadržaj poruke).

## <a name="send-messages-to-a-topic"></a>Slanje poruka teme

Da biste poslali poruku teme Bus servisa, aplikacija dohvaća **ServiceBusContract** objekt. Sljedeći kod pokazuje kako slanje poruke u `TestTopic` temu stvoreno unutar na `HowToSample` prostor naziva:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Poruke poslane na servis Bus teme su instance klase [BrokeredMessage][] . [BrokeredMessage][] *objekti sadrže skup standardne metode (kao što su * *setLabel* * i * *TimeToLive**), rječnika koji koristi se za čuvanje prilagođena svojstva specifične za aplikacije i u tijelu podataka proizvoljne aplikacije. Aplikaciju možete postaviti tijelo poruke prosljeđivanjem serializable objekata u Graditelj [BrokeredMessage][]i odgovarajuće * *DataContractSerializer* * koristit će se zatim serijalizirati objekt. Osim toga, u * *java.io.InputStream** se može pružati.

Sljedeći primjer pokazuje kako poslati pet testiranje poruke u `TestTopic` **MessageSender** ćemo dobiti u isječak koda iznad.
Imajte na umu kako mijenja se svojstva vrijednost **MessageNumber** svake poruke na iteracije u petlji (Time ćete odrediti koje dobio):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Servis Bus teme podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u temi, ali postoji u kapaciteta ukupnu veličinu poruke zaključale temu. Ovu temu veličinu definirana je u trenutku stvaranja s gornju granicu 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Kako se poruka iz pretplate

Da biste primili poruku iz pretplate, koristite **ServiceBusContract** objekt. Primljene poruke možete raditi u dva različita načina rada: **ReceiveAndDelete** i **PeekLock**.

Korištenje načina **ReceiveAndDelete** primite je operacija jednim snimka - odnosno kada servis Bus primi zahtjev za poruku, označava poruku kao se potrošena i vraća aplikaciji. Način **ReceiveAndDelete** je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Budući da Bus servisa će ste označili poruku kao potrošena, a zatim kada aplikacija pokreće i započinje ponovno troše poruke, ga će imati Propušteni poruku koja je potrošena prije no što se srušiti.

U načinu **PeekLock** primanje postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da drugi korisnici ga prima sprječavanje i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete **Brisanje** primljene poruke. Kada je servis Bus vidi poziva za **Brisanje** , označavanje poruke kao što je u tijeku potrošena će se i uklonite ga s temu.

U primjeru pokazuje kako se poruke mogu primati i obrađuju **PeekLock** načinu rada (ne zadani način). Izvodi petlje primjeru u nastavku obrađuje poruke u pretplatu "HighMessages" i zatim zatvara kada nema više poruka (umjesto toga ga nije moguće postaviti čekanja za nove poruke).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako je primatelj aplikacija nije moguće obraditi poruku zbog nekog razloga, pa je možete nazvati metodu **unlockMessage** na primljene poruke (umjesto metode **deleteMessage** ). Time će Bus servisa da biste otključali poruku unutar teme i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i vremenskog ograničenja povezan s porukom zaključan unutar teme, a ne uspijete aplikacija za obradu poruku prije nego što vremensko ograničenje zaključavanja istekne (na primjer, ako ruši se aplikacija), a zatim servis Bus će automatski otključati poruku i učiniti je dostupnom koji se dobiva ponovno.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se izda zahtjev za **deleteMessage** , zatim poruku će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da **Barem jednom obrade**, odnosno svake poruke obradit će se barem jednom no u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. To je često postići pomoću metode **getMessageId** poruke, koju će ostaju iste preko pokušao.

## <a name="delete-topics-and-subscriptions"></a>Brisanje teme i pretplate

U primarni da biste izbrisali teme i pretplate tako da koriste **ServiceBusContract** objekt. Brisanje teme i izbrisati sve pretplate registrirane s temom. Pretplate se brišu i zasebno.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus redovi, potražite u članku [servis Bus redova, teme i pretplate][] dodatne informacije.

  [Azure SDK Java]: http://azure.microsoft.com/develop/java/
  [Azure komplet alata za Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Servis Bus redovi, tema i pretplate]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
