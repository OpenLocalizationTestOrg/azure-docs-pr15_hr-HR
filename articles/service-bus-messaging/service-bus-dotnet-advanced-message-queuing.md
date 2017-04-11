<properties 
    pageTitle="Kako koristiti AMQP 1.0 s API Bus usluge za .NET | Microsoft Azure" 
    description="Saznajte kako koristiti napredne poruke stavljanje Protodol (AMQP) 1.0 s Azure .NET servisa Bus API-jem." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Kako koristiti AMQP 1.0 s API za .NET Bus usluge

Na dodatna poruke stavljanje Protocol (AMQP) 1.0 je programa učinkovitog pouzdan, žičani razinom razmjenu protokol koje možete koristiti da biste sastavili robusne, različite platforme, razmjenu aplikacije.

Podrška za 1,0 AMQP u servis Bus znači da koristite na stavljanje i objavljivanja/pretplate brokered razmjenu značajke iz raspona platforme učinkovitog binarni protokolom. Osim toga, možete sastaviti aplikacije koji se sastoji od komponente izgrađene pomoću kombinacije jezika, okviri i operacijske sustave.

U ovom se članku objašnjava kako pomoću servisa Bus brokered razmjenu značajke (redovima i objavljivanja/pretplate teme) iz aplikacije .NET koje koriste .NET API-JA Bus servisa. Postoji [članak pomoćnom](service-bus-java-how-to-use-jms-api-amqp.md) kojima se objašnjava kako biste isti pomoću standardnih API servisa Java poruke (JMS). Dodatne informacije o različite platforme porukama pomoću AMQP 1.0 možete koristiti vodiče za dva zajedno.

## <a name="get-started-with-service-bus"></a>Početak rada s Bus servisa

U ovom se članku pretpostavlja da ste već prostor naziva Bus servisa koji sadrži reda pod nazivom "queue1." Ako to ne učinite, pa možete stvoriti polje naziva i red čekanja pomoću [portala za Azure][]. Dodatne informacije o stvaranju servisa knjižna se prostori naziva "i" Redovi potražite u članku [Početak rada sa servisa Bus redovima](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Preuzimanje Bus servisa SDK

Podrška za AMQP 1.0 dostupan je u servis Bus SDK verzije 2.1 ili noviji. Preuzmite najnovije bitova iz NuGet pri [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Kod .NET aplikacijama

Prema zadanim postavkama klijentska biblioteka servisa Bus .NET komunicira sa servisom Bus servis pomoću namjenski protokola SOAP-poštu. Da biste koristili AMQP 1.0 umjesto zadanog protokol potrebna eksplicitnih konfiguracija na niz za povezivanje servisa Bus kao što je opisano u sljedećem odjeljku. Osim ta promjena kod aplikacija ne mijenja zapravo prilikom korištenja AMQP 1.0.

U trenutnom izdanju, postoji nekoliko značajki API koje nisu podržane prilikom korištenja AMQP. Ove značajke koje nisu podržane kasnije su navedene u odjeljku [nepodržane značajke i ograničenja](#unsupported-features-and-restrictions). Neke napredne konfiguracijske postavke i imaju različite značenje prilikom korištenja AMQP. Ništa od tih postavki se koriste u ovom članku, ali više detalja dostupne u [Pregled AMQP Bus servisa](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Konfiguriranje putem App.config

Je preporučljivo aplikacije koristiti App.config konfiguracijska datoteka za spremanje postavki. Za aplikacije servisa Bus App.config možete koristiti za pohranu servisa Bus **ConnectionString**. U ovom primjeru aplikacije koristi i App.config za pohranu naziv servisa Bus entitet koja se koristi za razmjenu poruka.

Ogledna datoteka App.config je prikazano ovdje:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Konfiguriranje usluge Bus niz za povezivanje

Vrijednost postavke **Microsoft.ServiceBus.ConnectionString** je niz za povezivanje servisa Bus koja se koristi za konfiguriranje veza Bus servisa. Oblik je na sljedeći način:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Gdje `[namespace]` i `[SAS key]` dobivaju se s [portala za Azure][]. Dodatne informacije potražite u članku [kako pomoću servisa Bus redova] [].

Prilikom korištenja AMQP niz za povezivanje dodaju se s `;TransportType=Amqp`, koji govori klijentska biblioteka da biste vezu servisa Bus pomoću AMQP 1.0.

### <a name="configure-the-entity-name"></a>Konfiguriranje naziv entitet

U ovom primjeru aplikacije koristi u `EntityName` postavku u odjeljku **appSettings** App.config datoteke da biste konfigurirali naziv reda čekanja kojom aplikacije što poruke.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Jednostavan .NET aplikacije pomoću servisa Bus reda čekanja

U sljedećem primjeru šalje i prima poruka iz servisa Bus reda.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Pokrenite aplikaciju

Pokretanje aplikacije proizvodi Izlaz iz obrasca:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Različite platforme razmjene poruka između JMS i .NET

U ovoj se temi prikazivao za slanje poruka Bus servis pomoću .NET te kako primanje tih poruka pomoću .NET. Neke ključne pogodnosti AMQP 1.0 je omogućuje aplikacije sastavljanja iz komponente namijenjene poruka razmijenjenih pouzdano i na potpunoj na različitim jezicima.

Pomoću aplikacije .NET uzorka prethodno opisan i slično Java aplikacije iz vodič za suradnika, [upute za korištenje na Java poruka servisa (JMS) API servisa Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)moguće je radi razmjene poruka između .NET i Java. 

Dodatne informacije o detaljima različite platforme porukama pomoću servisa Bus i AMQP 1.0 potražite u članku [Pregled usluge Bus AMQP 1.0](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS za .NET

Da bismo pokazali JMS za .NET poruka:

* Pokretanje aplikacije uzorka .NET bez argumenata naredbenog retka.
* Pokretanje aplikacije Java uzorka s argumentom naredbenog retka "sendonly". U tom načinu aplikacija će primati poruke iz reda čekanja, ona će poslati samo.
* Pritisnite **Enter** nekoliko puta na konzoli za aplikaciju Java što će uzrokovati poruke slati.
* Te poruke, isporučene su u aplikaciji .NET.

### <a name="output-from-jms-application"></a>Izlaz iz aplikacije JMS

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Izlaz iz aplikacije za .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET za JMS

Da bismo pokazali .NET da biste JMS poruka:

* Pokretanje aplikacije .NET uzorka s argumentom naredbenog retka "sendonly". U tom načinu aplikacija će primati poruke iz reda čekanja, ona će poslati samo.
* Pokretanje aplikacije uzorka Java bez bilo koji argument naredbenog retka.
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

Sljedeće značajke Bus API usluge za .NET trenutno nisu podržane prilikom korištenja AMQP:

 * Transakcije
 * Slanje putem odredište prijenos

Dodatne informacije potražite u članku [značajke koje nisu podržane, ograničenja i behavioral razlike](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Sažetak

U ovom se članku prikazivao kako pristupati servisa Bus brokered razmjenu značajke (redovima i objavljivanja/pretplate teme) iz .NET pomoću AMQP 1.0 i .NET API-JA Bus servisa.

Servis Bus AMQP 1.0 možete koristiti i iz drugih jezika uključujući Java, C, Python i PHP. Komponente izgrađene pomoću tih jezika možete razmjenjivati poruke pouzdano i na potpunoj pomoću AMQP 1.0 u Bus servisa. Dodatne informacije potražite u članku [Pregled AMQP Bus servisa](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste pročitali pregled Bus servisa i AMQP s .NET, pogledajte sljedeće veze da biste saznali više.

* [Podrška za AMQP 1.0 u Bus servisa Azure](service-bus-amqp-overview.md)
* [Upute za korištenje na Java poruka servisa (JMS) API servisa Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Upute za korištenje servisa Bus redovi](service-bus-dotnet-get-started-with-queues.md)
 
[Portal za Azure]: https://portal.azure.com
