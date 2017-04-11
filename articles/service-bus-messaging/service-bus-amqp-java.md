<properties 
    pageTitle="Servis Bus i Java s AMQP 1.0 | Microsoft Azure"
    description="Pomoću servisa Bus iz Java AMQP"
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

# <a name="use-service-bus-from-java-with-amqp-10"></a>Korištenje servisa Bus iz Java s AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Java poruka servisa (JMS) je standardni API-JA za rad s proizvod koji je poruka usmjerena na platformi Java. Microsoft Azure servisa Bus testirano s AMQP 1.0 temelji JMS klijentska biblioteka razvio Apache Qpid projekta. Biblioteke podržava punu API 1.1 JMS i može se koristiti s bilo kojeg AMQP 1.0 usklađen uslugom. Ovaj scenarij podržano je i u [Bus servisa za Windows Server](https://msdn.microsoft.com/library/dn282144.aspx) (lokalnog servisa Bus). Dodatne informacije potražite u članku [AMQP Bus servisa za Windows Server][].

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>Preuzimanje klijentska biblioteka Apache Qpid AMQP 1.0 JMS

Informacije o preuzimanju najnoviju verziju klijentska biblioteka Apache Qpid JMS AMQP 1.0 potražite u članku [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html).

Morate dodati sljedeće četiri POSUDU datoteke iz arhive raspodjele Apache Qpid JMS AMQP 1.0 Java CLASSPATH kada stvaranje i pokretanje aplikacija JMS s Bus servisa:

-   geronimo jms\_1.1\_.jar specifikacija-[verzija]

-   qpid-amqp-1-0-Client-[Version].jar

-   qpid-amqp-1-0-Client-jms-[Version].jar

-   qpid-amqp-1-0-Common-[Version].jar

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Rad s redovima, teme i pretplate s JMS Bus servisa

### <a name="java-naming-and-directory-interface-jndi"></a>Imenovanje Java i imenik sučelja (JNDI)

Da biste stvorili odvojenosti između logičke nazive i nazive fizičke JMS koristi Java imenovanju i imenik sučelja (JNDI). Dvije vrste objekata JMS se riješiti pomoću JNDI: **ConnectionFactory** i **odredište**. JNDI koristi davatelja modela u koje možete uključiti različite imeničkim servisima za rukovanje naziv razlučivost obavezama koje imate. Biblioteka Apache Qpid JMS AMQP 1.0 u sklopu jednostavne svojstva utemeljenih na datotekama JNDI davatelja koji je konfiguriran pomoću tekstne datoteke.

Davatelj usluge JNDI Qpid svojstva datoteke je konfiguriran pomoću svojstva datoteke sljedećih oblika:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>Konfiguriranje tvorničke veze

Unos koristi za definiranje **ConnectionFactory** u JNDI davatelju Qpid svojstva datoteke je sljedećih oblika:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Gdje `[jndi\_name]` i `[ConnectionURL]` imati sljedeće značenja:

| Ime            | Značenje                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | Logički naziv tvorničke veze. U aplikaciji Java rješava taj naziv korištenjem na JNDI `IntialContext.lookup()` način. |   |   |   |   |
| `[ConnectionURL]` | URL koji omogućuje biblioteku JMS potrebne informacije da biste AMQP broker.                                                      |   |   |   |   |

Oblikovanje veza URL je na sljedeći način:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Gdje `[namespace]`, `[username]`, a `[password]` imati sljedeće značenja:

| Ime          | Značenje                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | Servis Bus polje naziva dobivenog [Azure portal][].                      |   |   |   |   |
| `[username]`  | Servis Bus SAS ključa naziv dobivenog [Azure portal][].                    |   |   |   |   |
| `[password]`  | Kodiran URL-om obrazac ključa servisa Bus SAS dobivenog [Azure portal][]. |   |   |   |   |

> [AZURE.NOTE] Morate URL kodiranje lozinku ručno. Korisni uslužni kodiranja URL je dostupan na [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

Na primjer, ako podatke nabavili je portal na sljedeći način:

| Prostor naziva:   | Test.servicebus.Windows.NET                  |
|--------------|----------------------------------------------|
| Naziv izdavača: | RootManageSharedAccessKey                                        |
| Izdavač ključ:  | abcdefg |

Zatim da biste definirali **ConnectionFactory** objekt s nazivom `SBCONNECTIONFACTORY`, niz konfiguracije bio na sljedeći način:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>Konfiguriranje odredišta

Stavka koja definira odredište u JNDI davatelju Qpid svojstva datoteke je sljedećih oblika:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Gdje `[jndi\_name]` i `[physical\_name]` imati sljedeće značenja:

| Ime              | Značenje                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | Logički naziv odredište. To je naziv rješava u aplikaciji Java u JNDI `IntialContext.lookup()` način. |
| `[physical\name]` | Naziv entitet Bus servis na koji aplikacija šalje i prima poruke.                                                  |

Imajte na umu sljedeće:

- Na `[physical\name]` vrijednost može biti reda čekanja Bus servisa ili temu.
- Prilikom primanja iz pretplate tema Bus servisa, fizičke naziv koji je naveden u JNDI mora biti naziv teme. Naziv pretplate se nudi prilikom stvaranja aplikacije kod JMS durable pretplate.
- Također je moguće pretplatu tema servisa Bus Smatraj JMS red. Nekoliko je prednosti takvog: moguće je koristiti istu šifru tekstnog okvira za redova i tema pretplate, a sve podatke o adresi (tema i pretplate nazive) externalized će se u okviru svojstva datoteke.
- Unos u datoteku svojstava pretplatu tema servisa Bus tretirati kao reda JMS, mora biti obrasca: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |

Da biste definirali logičke JMS odredište pod nazivom "TEMA", koji se preslikava servisa Bus temi pod nazivom "tema1", stavka u okviru svojstva datoteke bio na sljedeći način:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>Slanje poruke s JMS

Sljedeći kod prikazuje kako se poruke slati na temu Bus servisa. Pretpostavlja se da je koji `SBCONNECTIONFACTORY` i `TOPIC` su definirani u **servicebus.properties** konfiguracijskoj datoteci, kao što je opisano u prethodnom odjeljku.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Primanje poruka pomoću JMS

Sljedeći kod prikazuje `how` primati poruke iz pretplate tema Bus servisa. Pretpostavlja se da je koji `SBCONNECTIONFACTORY` i teme su definirani u datoteci konfiguracije **servicebus.properties** opisan u prethodnom odjeljku. Također pretpostavlja se da je naziv pretplate `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Smjernice za stvaranje robusne aplikacije

Specifikacija JMS definira kako ugovora iznimku metoda API-JA i računala kod moraju biti napisani za rukovanje takve iznimke. Ovo su neke druge točke treba uzeti u obzir vezane uz iznimku rukovanje:

-   Registrirajte se **ExceptionListener** s vezom za JMS pomoću **connection.setExceptionListener**. Time se omogućuje klijent Obaviještenost problema asinkrono. Ta se obavijest je osobito važno za veze koje samo poruke, kao što je želite imaju drugi način da biste saznali nije uspjela veze. **ExceptionListener** naziva ako postoji problem s podlozi AMQP veze, sesiju ili vezu. U aplikaciji treba u tom slučaju ponovno stvorite **JMS veze**, **sesiju**, **MessageProducer** i **MessageConsumer** objekata ispočetka.

-   Da biste potvrdili da poruke uspješno poslao **MessageProducer** Bus servisa entitet, provjerite je li aplikacija sadrži je konfiguriran pomoću u **qpid.sync\_objavljivanje** skup svojstva sustava. To možete učiniti tako da pokrenete program s na **-Dqpid.sync\_objavljivanje = true** Java VM mogućnost postaviti u naredbenom retku prilikom pokretanja aplikacije. Postavljanje ove mogućnosti konfigurira biblioteke s vraća se slanje poziva dok ne potvrdu zaprimljeni koje poruke prihvatila Bus servisa. Ako se problem pojavljuje se tijekom postupka Pošalji, **JMSException** podignut. Postoje dva moguća uzroka: 
    1. Ako je problem zbog servisa Bus odbijanje određenu poruku koja se šalje, zatim **MessageRejectedException** iznimke se zaokružuje. Ta se pogreška transitory, ili je zbog poteškoća s porukom. Tečaj za preporučene akcije je da biste nekoliko pokušaja ponoviti postupak s nekim logike za isključivanje natrag. Ako se problem ne riješi, poruku treba napuštenih s pogreškom prijavljeni lokalno. Nema potrebe za ponovno stvaranje **JMS veze**, **sesiju**ili **MessageProducer** objekata u tom slučaju. 
    2. Ako je problem zbog servisa Bus zatvaranja AMQP vezu, zatim iznimku **InvalidDestinationException** se zaokružuje. To može biti zbog problema s tranzitne ili zbog entitet poruku nije moguće izbrisati. U svakom slučaju, objekti **JMS veze**, **sesije**i **MessageProducer** stvoriti. Ako je u stanju pogreške tranzitne, zatim ovaj postupak naposljetku bit će uspješno. Ako je izbrisana je entitet, neuspjeh bit će trajno.

## <a name="messaging-between-net-and-jms"></a>Poruka između .NET i JMS

### <a name="message-bodies"></a>Tijela poruke

JMS definira pet vrsta drugu poruku: **BytesMessage**, **MapMessage**, **ObjectMessage**, **StreamMessage**i **TextMessage**. .NET API-JA Bus servisa sadrži vrstu jednu poruku [BrokeredMessage][].

#### <a name="jms-to-service-bus-net-api"></a>JMS tržišta servisa .NET API-JA za

U sljedećim se odjeljcima pokazati kako zauzeti poruke svake vrste poruka JMS s .NET. Primjer **ObjectMessage** nije sadrži, kao što je tijelo **ObjectMessage** sadrži serializable objekt u Java programski jezik, što je interpretable .NET aplikacija.

##### <a name="bytesmessage"></a>BytesMessage

Sljedeći kod prikazuje kako zauzeti tijelo **BytesMessage** objekt pomoću .NET API-ji Bus servisa.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

Sljedeći kod prikazuje kako zauzeti tijelo **MapMessage** objekt pomoću .NET API-ji Bus servisa. Kod zadanu ponovno izračunava po elementima karte prikazuje naziv i vrijednost svaki element.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

Sljedeći kod prikazuje kako zauzeti tijelo **StreamMessage** objekt pomoću .NET API-ji Bus servisa. Kod popis svih stavki iz toka, zajedno s njihove vrste.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

Sljedeći kod prikazuje kako zauzeti tijelo **TextMessage** objekt pomoću .NET API-ji Bus servisa. Kod prikazuje tekstni niz koji se nalaze u tijelu poruke.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Servis Bus .NET API-ji za JMS

U sljedećim se odjeljcima pokazuju kako .NET aplikaciju možete stvoriti poruke primljene JMS u svakoj različitih vrsta JMS poruke. Primjer **ObjectMessage** nije sadrži, kao što je tijelo **ObjectMessage** sadrži serializable objekt u Java programski jezik, što je interpretable .NET aplikacija.

##### <a name="bytesmessage"></a>BytesMessage

Sljedeći kod prikazuje kako stvoriti [BrokeredMessage][] objekta u .NET primljene klijent JMS kao **BytesMessage**.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

Sljedeći kod prikazuje kako stvoriti [BrokeredMessage][] objekta u .NET primljene klijent JMS kao **StreamMessage**.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

Sljedeći kod prikazuje kako zauzeti tijelo **TextMessage** pomoću .NET API-JA Bus servisa. Kod prikazuje tekstni niz koji se nalaze u tijelu poruke.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Svojstva aplikacije

####<a name="jms-to-service-bus-net-apis"></a>JMS da biste Bus .NET API-ji za servis

Poruka JMS podržava aplikacija svojstva od sljedećih vrsta: **Booleova**, **bajta**, **Kratki**, **int**, **dugo**, **plutati**, **Dvostruki**i **niz**. Sljedeći kod programskog jezika Java pokazuje kako postaviti svojstva na poruci korištenjem svake od sljedećih vrsta svojstva.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

U .NET API servisa Bus svojstva aplikacije poruke prenose se u zbirku **Svojstva** [BrokeredMessage][]. Sljedeći kod prikazuje način za čitanje svojstava aplikacije poruku poslao JMS klijenta.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

Sljedeća tablica prikazuje kako mapiranja svojstava vrsta JMS svojstvo vrste .NET.

| Vrsta svojstva JMS | Vrsta svojstva .NET |
|-------------------|--------------------|
| Bajt              | sbyte              |
| Cijeli broj           | INT                |
| Vrijednost s pomičnim zarezom             | vrijednost s pomičnim zarezom              |
| Dvaput            | Dvostruki             |
| Booleove vrijednosti           | booleovom               |
| Niz            | niz             |

Vrsta [BrokeredMessage][] podržava aplikacija svojstva od sljedećih vrsta: **bajta**, **sbyte**, **znak**, **Kratki**, **ushort**, **int**, **uint**, **dugo**, **ulong**, **plutati**, **Dvostruki**, **decimalni**, **booleovom**, **Guid**, **niz**, **Uri**, **datuma i vremena**, **DateTimeOffset**, i **vremenski raspon**. Sljedeći kod .NET pokazuje kako da biste postavili svojstva na objektu [BrokeredMessage][] korištenjem neke od tih svojstava vrsta.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Sljedeći kod programskog jezika Java prikazuje kako čitati svojstva aplikacije poruke primljene putem servisa Bus .NET klijenta.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

Sljedeća tablica prikazuje kako mapiranja svojstava vrsta .NET svojstvo vrste JMS.

| Vrsta svojstva .NET | Vrsta svojstva JMS | Bilješke                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | UnsignedByte      | -                                                                                                                                                                      |
| sbyte              | Bajt              | -                                                                                                                                                                     |
| CHAR               | Znak         | -                                                                                                                                                                     |
| Kratki              | Kratki             | -                                                                                                                                                                     |
| ushort             | UnsignedShort     | -                                                                                                                                                                     |
| INT                | Cijeli broj           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| Dugi               | Dugi              | -                                                                                                                                                                     |
| ulong              | UnsignedLong      | -                                                                                                                                                                     |
| vrijednost s pomičnim zarezom              | Vrijednost s pomičnim zarezom             | -                                                                                                                                                                     |
| Dvostruki             | Dvaput            | -                                                                                                                                                                     |
| broja decimalnih mjesta            | BigDecimal        | -                                                                                                                                                                     |
| booleovom               | Booleove vrijednosti           | -                                                                                                                                                                     |
| GUID               | UUID              | -                                                                                                                                                                     |
| niz             | Niz            | -                                                                                                                                                                     |
| Datum i vrijeme           | Datum              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks mapirati vrstu AMQP:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Vremenski raspon           | DescribedType     | Timespan.Ticks mapirati vrstu AMQP:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI-ja                | DescribedType     | Uri.AbsoluteUri mapirati vrstu AMQP:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>Standardni zaglavlja

U sljedećoj tablici prikazane po čemu se JMS standardnih zaglavlja i [BrokeredMessage][] standardna svojstva pridružena korištenjem AMQP 1.0.

#### <a name="jms-to-service-bus-net-apis"></a>JMS da biste Bus .NET API-ji za servis

| JMS              | Servis Bus .NET               | Bilješke                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Trenutno nije dostupno        | Servis Bus podržava samo durable poruke na primjer, DeliveryMode.PERSISTENT, bez obzira na to što je navedeno.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Poruka. TimeToLive            | Pretvorba                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | Prema zadanim postavkama JMSMessageID kodira u binarnom obliku u poruci AMQP. Po primitku binarni id poruke, klijentska biblioteka .NET pretvara se u prikaz niza na temelju vrijednosti unicode bajtova. Da biste promijenili JMS biblioteke pomoću ID-a za poruke niz, Dodaj u "Binarni messageid = false" niz za parametre upita JNDI ConnectionURL. Na primjer: “amqps://[username]:[password]@[namespace].servicebus.windows.net? binarni messageid = false ". |
| JMSPriority      | Trenutno nije dostupno        | Servis Bus ne podržava prioritet poruke.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Trenutno nije dostupno        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Poruka. OutboundSMTPServer               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Pretvorba                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms – vrsta"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Servis Bus .NET API-ji za JMS

| Servis Bus .NET        | JMS              | Bilješke                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | Trenutno nije dostupno |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Pretvorba              |
| Oznaka                   | n/d              | Trenutno nije dostupno |
| MessageId               | JMSMessageID     | -                         |
| OutboundSMTPServer                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | n/d              | Trenutno nije dostupno |
| ScheduledEnqueueTimeUtc | n/d              | Trenutno nije dostupno |
| ID sesije               | n/d              | Trenutno nije dostupno |
| TimeToLive              | JMSExpiration    | Pretvorba              |
| Da biste                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Nepodržane značajke i ograničenja

Sljedeća ograničenja postoje kada JMS putem AMQP 1.0 s Bus servisa:

-   Samo jedan **MessageProducer** ili **MessageConsumer** je dopušteno po sesiji. Ako želite stvoriti više objekata **MessageProducer** ili **MessageConsumer** u aplikaciji, stvorite namjenski sesije za svaku od njih.

-   Promjenjive tema pretplate trenutno nisu podržani.

-   Objekti **MessageSelector** nisu podržani.

-   Privremeni odredišta; na primjer, **TemporaryQueue** ili **TemporaryTopic**nisu podržane, uz **QueueRequestor** i **TopicRequestor** API-ji koja ih koriste.

-   Transakcije sesije nisu podržani.

-   Raspodijeljeno transakcije nisu podržane.

## <a name="next-steps"></a>Daljnji koraci

Jeste li spremni za dodatne informacije? Posjetite sljedeće veze:

- [Pregled servisa Bus AMQP]
- [AMQP u Bus servisa za Windows Server]

[AMQP u Bus servisa za Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Pregled servisa Bus AMQP]: service-bus-amqp-overview.md
[Portal za Azure]: https://portal.azure.com
