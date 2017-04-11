<properties 
    pageTitle="Kako koristiti AMQP 1.0 s API Bus usluge za Java | Microsoft Azure" 
    description="Kako koristiti Java poruka servisa (JMS) s Bus servisa Azure i napredne poruke stavljanje Protodol (AMQP) 1.0." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Upute za korištenje na Java poruka servisa (JMS) API-JA s Bus servisa i AMQP 1.0

Na dodatna poruke stavljanje Protocol (AMQP) 1.0 je programa učinkovitog pouzdan, žičani razinom razmjenu protokol koje možete koristiti da biste izraditi robusne, više platformi razmjenu aplikacije.

Podrška za 1,0 AMQP u servis Bus znači da koristite na stavljanje i objavljivanja/pretplate brokered razmjenu značajke iz raspona platforme učinkovitog binarni protokolom. Osim toga, možete sastaviti aplikacije koji se sastoji od komponente izgrađene pomoću kombinacije jezika, okviri i operacijske sustave.

U ovom se članku objašnjava kako pomoću servisa Bus poruka značajke (redovima i objavljivanja/pretplate teme) iz Java aplikacije koje se koriste u popularne Java poruka servisa (JMS) API standard. Postoji [članak suradnika](service-bus-dotnet-advanced-message-queuing.md) koji objašnjava kako isto učinite i pomoću .NET API-JA Bus servisa. Dodatne informacije o različite platforme porukama pomoću AMQP 1.0 možete koristiti vodiče za dva zajedno.

## <a name="get-started-with-service-bus"></a>Početak rada s Bus servisa

Ovaj vodič pretpostavlja da ste već prostor naziva Bus servisa koji sadrži reda pod nazivom **queue1**. Ako to ne učinite, pa možete [stvoriti polje naziva i reda čekanja](service-bus-create-namespace-portal.md) pomoću [portala za Azure](https://portal.azure.com). Dodatne informacije o stvaranju servisa knjižna se prostori naziva "i" Redovi potražite u članku [upute za korištenje servisa Bus redova](service-bus-dotnet-get-started-with-queues.md).

> [AZURE.NOTE] Particioniranom redovima i teme podržavaju AMQP. Dodatne informacije potražite u članku [Partitioned poruka entiteti](service-bus-partitioning.md) i [AMQP 1.0 podrška za servis Bus particije redova i teme](service-bus-partitioned-queues-and-topics-amqp-overview.md).

## <a name="downloading-the-amqp-10-jms-client-library"></a>Preuzimanje biblioteke AMQP 1.0 JMS klijenta

Informacije o tome gdje trebate preuzeti najnoviju verziju klijentskog biblioteke Apache Qpid JMS AMQP 1.0 potražite u članku [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Morate dodati sljedeće četiri POSUDU datoteke iz arhive raspodjele Apache Qpid JMS AMQP 1.0 Java CLASSPATH kada stvaranje i pokretanje aplikacija JMS s Bus servisa:

- geronimo jms\_1.1\_specifikacija 1.0.jar
- qpid-amqp-1-0-Client-[Version].jar
- qpid-amqp-1-0-Client-jms-[Version].jar
- qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Kodiranje Java aplikacije

### <a name="java-naming-and-directory-interface-jndi"></a>Imenovanje Java i imenik sučelja (JNDI)

Da biste stvorili odvojenosti između logičke nazive i nazive fizičke JMS koristi Java imenovanju i imenik sučelja (JNDI). Dvije vrste objekata JMS se riješiti pomoću JNDI: ConnectionFactory i odredište. JNDI koristi davatelja modela u koje možete uključiti različite imeničkim servisima za rukovanje naziv razlučivost obavezama koje imate. Oblikovanje za na Apache Qpid JMS AMQP 1.0 biblioteke u sklopu jednostavne svojstva utemeljenih na datotekama JNDI davatelja koji je konfiguriran pomoću svojstva datoteke od sljedećeg:

```
# servicebus.properties - sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Konfiguriranje na ConnectionFactory

Unos koristi za definiranje **ConnectionFactory** u JNDI davatelju Qpid svojstva datoteke je sljedećih oblika:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Gdje **[jndi_name]** i **[ConnectionURL]** imaju sljedeće značenja:

- **[jndi_name]**: logički naziv u ConnectionFactory. To je naziv koji će se razriješiti u aplikaciji Java metodom JNDI IntialContext.lookup().
- **[ConnectionURL]**: A URL koji omogućuje biblioteku JMS potrebne informacije da biste AMQP broker.

Oblikovanje **ConnectionURL** je na sljedeći način:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Gdje **[prostor naziva]**, **[SASPolicyName]** i **[SASPolicyKey]** imaju sljedeće značenja:

- **[polja naziva]**: polje naziva u Bus servisa.
- **[SASPolicyName]**: naziv pravila u redu čekanja zajedničko korištenje potpisa programa Access.
- **[SASPolicyKey]**: ključ za pravila u redu čekanja zajednički se koristi potpis pristup.

> [AZURE.NOTE] Morate URL kodiranje lozinku ručno. Korisni uslužni URL kodiranje je dostupan na [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>Konfiguriranje odredišta

Unos koristi za definiranje odredište u JNDI davatelju Qpid svojstva datoteke je sljedećih oblika:

```
queue.[jndi_name] = [physical_name]
```

ili

```
topic.[jndi_name] = [physical_name]
```

Gdje **[jndi\_naziv]** i **[fizičke\_naziv]** imati sljedeće značenja:

- **[jndi_name]**: logički naziv odredište. To je naziv koji će se razriješiti u aplikaciji Java metodom JNDI IntialContext.lookup().
- **[physical_name]**: naziv entitet Bus servis na koji aplikacija šalje i prima poruke.

> [AZURE.NOTE] Prilikom primanja iz pretplate tema Bus servisa, fizičke naziv koji je naveden u JNDI mora biti naziv teme. Naziv pretplate se nudi prilikom stvaranja aplikacije kod JMS durable pretplate. [Vodič za servis Bus AMQP 1.0 programere](service-bus-amqp-dotnet.md) navedeni su dodatni Detalji o radu sa servisa Bus tema pretplate s JMS.

### <a name="write-the-jms-application"></a>Pisanje JMS aplikacije

Nema posebne API-ji ili mogućnosti potrebne prilikom korištenja JMS s Bus servisa. Međutim, postoji nekoliko ograničenja koja će biti kasnije prekriveno. Kao što je s bilo kojom aplikacijom JMS prva stvar potreban je konfiguracije okruženja JNDI, da biste mogli riješiti **ConnectionFactory** i odredišta.

#### <a name="configure-the-jndi-initialcontext"></a>Konfiguriranje JNDI InitialContext

Okruženje za JNDI konfiguriran prosljeđivanjem hashtable podatke o konfiguraciji u Graditelj klase javax.naming.InitialContext. Dva potrebni elemenata u hashtable naziv klase početne tvorničke kontekst i URL davatelja. Sljedeći kod prikazuje kako konfigurirati okruženje JNDI za korištenje svojstva datoteke Qpid utemeljena JNDI davatelja sa svojstvima datoteku pod nazivom **servicebus.properties**.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Jednostavni JMS aplikacije pomoću servisa Bus reda čekanja

Sljedeći primjer program šalje JMS TextMessages Bus servisa red nazivom JNDI logičke reda ČEKANJA i prima poruke natrag.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Pokrenite aplikaciju

Pokretanje aplikacije proizvodi Izlaz iz obrasca:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Različite platforme razmjene poruka između JMS i .NET

Ovaj vodič prikazivao upute za slanje i primanje poruka iz servisa Bus pomoću JMS. Neke ključne pogodnosti AMQP 1.0 je omogućuje aplikacije sastavljanja iz komponente namijenjene poruka razmijenjenih pouzdano i na potpunoj na različitim jezicima.

Pomoću aplikacije JMS uzorka prethodno opisan i slično .NET aplikacije iz vodič za suradnika, [kako koristiti AMQP 1.0 s .NET servisa Bus .NET API -jem](service-bus-dotnet-advanced-message-queuing.md), možete razmjenjivati poruke između .NET i Java. 

Dodatne informacije o detaljima različite platforme porukama pomoću servisa Bus i AMQP 1.0 potražite u članku [Vodič za servis Bus AMQP 1.0 programere](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS za .NET

Da bismo pokazali JMS za .NET poruka:

* Pokretanje aplikacije uzorka .NET bez argumenata naredbenog retka.
* Pokretanje aplikacije Java uzorka s argumentom naredbenog retka "sendonly". U tom načinu aplikacija će primati poruke iz reda čekanja, ona će poslati samo.
* Pritisnite **Enter** nekoliko puta na konzoli za aplikaciju Java što će uzrokovati poruke slati.
* Te poruke, isporučene su u aplikaciji .NET.

#### <a name="output-from-jms-application"></a>Izlaz iz aplikacije JMS

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Izlaz iz aplikacije za .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET za JMS

Da bismo pokazali .NET da biste JMS poruka:

* Pokretanje aplikacije .NET uzorka s argumentom naredbenog retka "sendonly". U tom načinu aplikacija će primati poruke iz reda čekanja, ona će poslati samo.
* Pokretanje aplikacije uzorka Java bez argumenata naredbenog retka.
* Pritisnite **Enter** nekoliko puta na konzoli za aplikaciju .NET što će uzrokovati poruke slati.
* Te poruke, isporučene su Java aplikacija.

#### <a name="output-from-net-application"></a>Izlaz iz aplikacije za .NET

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Izlaz iz aplikacije JMS

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nepodržane značajke i ograničenja

Sljedeća ograničenja postoje kada putem JMS AMQP 1.0 s Bus servisa, odnosno:

* Samo jedan **MessageProducer** ili **MessageConsumer** je dopušteno po **sesiji**. Ako je potrebno stvoriti više **MessageProducers** ili **MessageConsumers** u aplikaciji stvaranje namjenski **sesije** za svaku od njih.
* Promjenjive tema pretplate trenutno nisu podržani.
* **MessageSelectors** trenutno nisu podržani.
* Privremeni odredišta, odnosno **TemporaryQueue**, **TemporaryTopic** trenutno nisu podržani, uz **QueueRequestor** i **TopicRequestor** API-ji koja ih koriste.
* Transakcije sesije i raspodijeljeno transakcije nisu podržane.

## <a name="summary"></a>Sažetak

Ovaj vodič s uputama prikazivao korištenje servisa Bus brokered razmjenu značajki (redovima i objavljivanja/pretplate teme) iz Java pomoću popularne JMS API-JA i AMQP 1.0.

Servis Bus AMQP 1.0 možete koristiti i iz drugih jezika, uključujući .NET, C, Python i PHP. Komponente izgrađene pomoću ove različite jezike možete pouzdano razmjenu poruka i na potpunoj pomoću AMQP 1.0 podržava Bus servisa. Dodatne informacije potražite u članku [Vodič za servis Bus AMQP 1.0 programere](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Daljnji koraci

* [Podrška za AMQP 1.0 u Bus servisa Azure](service-bus-amqp-overview.md)
* [Kako koristiti AMQP 1.0 s API za .NET Bus usluge](service-bus-dotnet-advanced-message-queuing.md)
* [Servis Bus AMQP 1.0 vodič za programere](service-bus-amqp-dotnet.md)
* [Upute za korištenje servisa Bus redovi](service-bus-dotnet-get-started-with-queues.md)
* [Razvojni centar za Java](/develop/java/).



 
