<properties 
    pageTitle="Servis Bus i Python s AMQP 1.0 | Microsoft Azure"
    description="Pomoću servisa Bus iz Python AMQP."
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

# <a name="using-service-bus-from-python-with-amqp-10"></a>Pomoću servisa Bus iz Python AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton Python je povezivanje Python jezik za Proton-C; To je implementira li se Proton Python kao omot oko mehanizam implementirana u C.

## <a name="download-the-proton-client-library"></a>Preuzimanje klijentska biblioteka Proton

Proton C i njegove pridružene povezivanja (uključujući Python) možete preuzeti iz [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Preuzimanje je u obliku izvornog koda. Da biste sastavili kod, pomoću uputa nalaze preuzete paketa.

Imajte na umu da u vrijeme pisanja ovog SSL podrška u Proton C dostupna je samo za operacijske sustave Linux. Bus servisa Azure zahtijeva korištenje SSL, Proton C (i povezivanja jezik) možete se koristiti samo pristup Bus servisa s Linux trenutno. Da biste omogućili Proton-C s SSL u sustavu Windows je sugovornike pa potvrdite natrag redovito provjeravajte ima li ažuriranja.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Rad s redovima, teme i pretplate s Python Bus servisa

Sljedeći kod služi za slanje i primanje poruka iz servisa Bus poruka entitet.

### <a name="send-messages-using-proton-python"></a>Slanje poruke s Proton Python

Sljedeći kod pokazuje kako se poruke slati na servis Bus poruka entitet.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Primanje poruka pomoću Proton Python

Sljedeći kod prikazuje kako poruka iz servisa Bus poruka entitet.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Poruka između .NET i Proton Python

### <a name="application-properties"></a>Svojstva aplikacije

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python Bus .NET API-ji za servis

Poruka proton Python podržava aplikacija svojstva od sljedećih vrsta: **int**, **dugo**, **plutati**, **uuid**, **booleovom**, **niz**. Sljedeći kod Python pokazuje kako postaviti svojstva na poruci korištenjem svake od sljedećih vrsta svojstva.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

U .NET API servisa Bus svojstva aplikacije poruke prenose se u zbirku **Svojstva** [BrokeredMessage][]. Sljedeći kod prikazuje način za čitanje svojstava aplikacije poruku poslao Python klijenta.

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

U sljedećoj su tablici mapira svojstvo vrste Python svojstvo vrste .NET.

| Vrsta svojstva Python | Vrsta svojstva .NET |
|----------------------|--------------------|
| INT                  | INT                |
| vrijednost s pomičnim zarezom                | Dvostruki             |
| Dugi                 | Int64              |
| UUID                 | GUID               |
| booleovom                 | booleovom               |
| niz               | niz             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Servis Bus .NET API-ji za Proton Python

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
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Sljedeći kod Python prikazuje kako čitati svojstva aplikacije poruke primljene putem servisa Bus .NET klijenta.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

U sljedećoj su tablici mapira svojstvo vrste .NET svojstvo vrste Python.

| Vrsta svojstva .NET | Vrsta svojstva Python | Bilješke                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | INT                  | -                                                                                                                                                                     |
| sbyte              | INT                  | -                                                                                                                                                                     |
| CHAR               | CHAR                 | Proton Python klase                                                                                                                                                 |
| Kratki              | INT                  | -                                                                                                                                                                     |
| ushort             | INT                  | -                                                                                                                                                                     |
| INT                | INT                  | -                                                                                                                                                                     |
| uint               | INT                  | -                                                                                                                                                                     |
| Dugi               | INT                  | -                                                                                                                                                                     |
| ulong              | Dugi                 | Proton Python klase                                                                                                                                                 |
| vrijednost s pomičnim zarezom              | vrijednost s pomičnim zarezom                | -                                                                                                                                                                     |
| Dvostruki             | vrijednost s pomičnim zarezom                | -                                                                                                                                                                     |
| broja decimalnih mjesta            | Niz               | Decimalni trenutno ne podržava Proton.                                                                                                                     |
| booleovom               | booleovom                 | -                                                                                                                                                                     |
| GUID               | UUID                 | Proton Python klase                                                                                                                                                 |
| niz             | niz               | -                                                                                                                                                                     |
| Datum i vrijeme           | Vremenska oznaka            | Proton Python klase                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | DateTimeOffset.UtcTicks mapirati vrstu AMQP:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Vremenski raspon           | DescribedType        | Timespan.Ticks mapirati vrstu AMQP:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI-ja                | DescribedType        | Uri.AbsoluteUri mapirati vrstu AMQP:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Standardna svojstva

U sljedećoj tablici prikazane mapiranja između svojstva standardne poruke Proton Python i svojstva [BrokeredMessage][] standardne poruke.

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python Bus .NET API-ji za servis

| Proton Python        | Servis Bus .NET         | Bilješke                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| durable              | n/d                      | Servis Bus podržava samo durable poruke.               |
| Prioritet             | n/d                      | Servis Bus podržava samo jednu poruku prioritet.      |
| TTL                  | Message.TimeToLive       | Pretvorba, Proton Python TTL definirana je u milisekundama. |
| prvi\_acquirer      | n/d                      | -                                                           |
| Isporuka\_count      | n/d                      | -                                                           |
| ID-a                   | Message.MessageID        | -                                                           |
| Korisnik\_ID-a             | n/d                      | -                                                           |
| adresa              | Message.To               | -                                                           |
| Predmet              | Message.Label            | -                                                           |
| Odgovori\_da biste            | Message.ReplyTo          | -                                                           |
| korelacije\_ID-a      | Message.CorrelationID    | -                                                           |
| sadržaj\_vrsta        | Message.ContentType      | -                                                           |
| sadržaj\_kodiranje    | n/d                      | -                                                           |
| isteka\_vremena         | n/d                      | -                                                           |
| Stvaranje\_vremena       | n/d                      | -                                                           |
| grupa\_ID-a            | Message.SessionId        | -                                                           |
| grupa\_niza      | n/d                      | -                                                           |
| Odgovori\_da biste\_grupe\_ID-a | Message.ReplyToSessionId | -                                                           |
| Oblikovanje               | n/d                      | -                                                           |

| Servis Bus .NET        | Proton                       | Bilješke                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.Content\_vrsta        | -                                                           |
| CorrelationId           | Message.correlation\_ID-a      | -                                                           |
| EnqueuedTimeUtc         | n/d                          | -                                                           |
| Oznaka                   | Message.SUBJECT              | -                                                           |
| MessageId               | Message.ID                   | -                                                           |
| OutboundSMTPServer                 | Message.Reply\_da biste            | -                                                           |
| ReplyToSessionId        | Message.Reply\_da biste\_grupe\_ID-a | -                                                           |
| ScheduledEnqueueTimeUtc | n/d                          | -                                                           |
| ID sesije               | Message.Group\_ID-a            | -                                                           |
| TimeToLive              | Message.TTL                  | Pretvorba, Proton Python TTL definirana je u milisekundama. |
| Da biste                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Daljnji koraci

Jeste li spremni za dodatne informacije? Posjetite sljedeće veze:

- [Pregled servisa Bus AMQP]
- [AMQP u Bus servisa za Windows Server]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP u Bus servisa za Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

[Pregled servisa Bus AMQP]: service-bus-amqp-overview.md
