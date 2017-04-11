<properties 
    pageTitle="Servis Bus i PHP s AMQP 1.0 | Microsoft Azure"
    description="Pomoću servisa Bus iz i AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Pomoću servisa Bus iz i AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton PHP je povezivanje i jezik na Proton-C; To je implementira li se Proton i kao omot oko mehanizam implementirana u C.

## <a name="downloading-the-proton-client-library"></a>Preuzimanje biblioteke Proton klijenta

Proton C i njegove pridružene povezivanja (uključujući i) možete preuzeti iz [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Preuzimanje je u obliku izvornog koda. Da biste sastavili kod, pomoću uputa koje se nalaze u paketu preuzeti.

> [AZURE.IMPORTANT] Prilikom pisanja ovog SSL podrška u Proton C dostupna je samo za operacijske sustave Linux. Bus servisa Azure zahtijeva korištenje SSL, Proton C (i povezivanja jezik) možete se koristiti samo pristup Bus servisa s Linux trenutno. Da biste omogućili Proton-C s SSL u sustavu Windows je u tijeku, pa potvrdite natrag redovito provjeravajte ima li ažuriranja.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Rad s redovima, teme i pretplate s PHP Bus servisa

Sljedeći kod služi za slanje i primanje poruka iz servisa Bus poruka entitet.

### <a name="sending-messages-using-proton-php"></a>Slanje poruka pomoću Proton PHP

Sljedeći kod pokazuje kako se poruke slati na servis Bus poruka entitet.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Primanje poruka pomoću Proton PHP

Sljedeći kod prikazuje kako poruka iz servisa Bus poruka entitet.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Poruka između .NET i Proton PHP

### <a name="application-properties"></a>Svojstva aplikacije

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP da biste Bus .NET API-ji za servis

Proton i poruke podržava aplikacija svojstva od sljedećih vrsta: **cijeli broj**, **Dvostruki**, **Booleova**, **niza**i **objekt**. Sljedeći kôd i prikazuje kako postaviti svojstva na poruci pomoću svake od sljedećih vrsta svojstva.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

U .NET API servisa Bus svojstva aplikacije poruke prenose se u zbirku **Svojstva** [BrokeredMessage][]. Sljedeći kod prikazuje kako čitati svojstva aplikacije poruku poslao i klijenta.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

U sljedećoj su tablici mapira svojstvo vrste i svojstvo vrste .NET.

| Vrsta i svojstva | Vrsta svojstva .NET |
|-------------------|--------------------|
| cijeli broj           | INT                |
| Dvostruki            | Dvostruki             |
| Booleove vrijednosti           | booleovom               |
| niz            | niz             |
| objekt            | Objekt             |

#### <a name="service-bus-net-apis-to-php"></a>Servis Bus .NET API-ji za PHP

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

Sljedeći kôd i prikazuje kako čitati svojstva aplikacije poruku poslao klijent .NET Bus servisa.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

U sljedećoj su tablici mapira svojstvo vrste .NET svojstvo vrste PHP.

| Vrsta svojstva .NET | Vrsta i svojstva | Bilješke                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | cijeli broj           | -                                                                                                                                                                     |
| sbyte              | cijeli broj           | -                                                                                                                                                                     |
| CHAR               | CHAR              | Proton i predmet                                                                                                                                                    |
| Kratki              | cijeli broj           | -                                                                                                                                                                     |
| ushort             | cijeli broj           | -                                                                                                                                                                     |
| INT                | cijeli broj           | -                                                                                                                                                                     |
| uint               | Cijeli broj           | -                                                                                                                                                                     |
| Dugi               | cijeli broj           | -                                                                                                                                                                     |
| ulong              | cijeli broj           | -                                                                                                                                                                     |
| vrijednost s pomičnim zarezom              | Dvostruki            | -                                                                                                                                                                     |
| Dvostruki             | Dvostruki            | -                                                                                                                                                                     |
| broja decimalnih mjesta            | niz            | Decimalni trenutno ne podržava Proton.                                                                                                                     |
| booleovom               | Booleove vrijednosti           | -                                                                                                                                                                     |
| GUID               | UUID              | Proton i predmet                                                                                                                                                    |
| niz             | niz            | -                                                                                                                                                                     |
| Datum i vrijeme           | cijeli broj           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks mapirati vrstu AMQP:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Vremenski raspon           | DescribedType     | Timespan.Ticks mapirati vrstu AMQP:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI-ja                | DescribedType     | Uri.AbsoluteUri mapirati vrstu AMQP:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Standardna svojstva

U sljedećoj tablici prikazane mapiranja između svojstava Proton i standardne poruke i svojstva [BrokeredMessage][] standardne poruke.

| Proton PHP           | Servis Bus .NET         | Bilješke                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Durable              | n/d                      | Servis Bus podržava samo durable poruke.          |
| Prioritet             | n/d                      | Servis Bus podržava samo jednu poruku prioritet. |
| TTL                  | Message.TimeToLive       | Pretvorba, TTL Proton i definirana je u milisekundama.   |
| prvi\_acquirer      | -                          | -                                                          |
| Isporuka\_count      | -                          | -                                                          |
| ID-a                   | Message.Id               | -                                                          |
| Korisnik\_ID-a             | -                          | -                                                          |
| Adresa              | Message.To               | -                                                          |
| Predmet              | Message.Label            | -                                                          |
| Odgovori\_da biste            | Message.ReplyTo          | -                                                          |
| korelacije\_ID-a      | Message.CorrelationId    | -                                                          |
| sadržaj\_vrsta        | Message.ContentType      | -                                                          |
| sadržaj\_kodiranje    | n/d                      | -                                                          |
| isteka\_vremena         | Message.ExpiresAtUTC     | -                                                          |
| Stvaranje\_vremena       | n/d                      | -                                                          |
| grupa\_ID-a            | Message.SessionId        | -                                                          |
| grupa\_niza      | -                          | -                                                          |
| Odgovori\_da biste\_grupe\_ID-a | Message.ReplyToSessionId | -                                                          |
| Oblikovanje               | n/d                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Servis Bus .NET API-ji za Proton PHP

| Servis Bus .NET        | Proton PHP                                             | Bilješke                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Poruka -\>sadržaj\_vrsta                                | -                                                        |
| CorrelationId           | Poruka -\>korelacije\_ID-a                              | -                                                        |
| EnqueuedTimeUtc         | Poruka -\>primjedbe [x-uključivanje-enqueued-vrijeme]             | -                                                        |
| Oznaka                   | Poruka -\>predmet                                      | -                                                        |
| MessageId               | Poruka -\>ID-a                                           | -                                                        |
| OutboundSMTPServer                 | Poruka -\>odgovor\_da biste                                    | -                                                        |
| ReplyToSessionId        | Poruka -\>odgovor\_da biste\_grupe\_ID-a                         | -                                                        |
| ScheduledEnqueueTimeUtc | Poruka -\>primjedbe ["x-uključivanje-zakazano-enqueue-vrijeme"] | -                                                        |
| ID sesije               | Poruka -\>grupe\_ID-a                                    | -                                                        |
| TimeToLive              | Poruka -\>ttl                                          | Pretvorba, TTL Proton i definirana je u milisekundama. |
| Da biste                      | Poruka -\>adresa                                      | -                                                        |

## <a name="next-steps"></a>Daljnji koraci

Jeste li spremni za dodatne informacije? Posjetite sljedeće veze:

- [Pregled servisa Bus AMQP]
- [AMQP u Bus servisa za Windows Server]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP u Bus servisa za Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[Pregled servisa Bus AMQP]: service-bus-amqp-overview.md
