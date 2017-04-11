<properties 
    pageTitle="Servis Bus s .NET i AMQP 1.0 | Microsoft Azure"
    description="Pomoću servisa Bus iz .NET AMQP"
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

# <a name="using-service-bus-from-net-with-amqp-10"></a>Pomoću servisa Bus iz .NET AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Preuzimanje Bus servisa SDK

Podrška za AMQP 1.0 dostupan je u Bus SDK servisa verzije 2.1 ili noviji. Možete omogućiti tako da preuzmete bitova Bus servisa iz [NuGet][]imate najnoviju verziju.

## <a name="configuring-net-applications-to-use-amqp-10"></a>Konfiguriranje .NET aplikacije da biste koristili AMQP 1.0

Prema zadanim postavkama klijentska biblioteka servisa Bus .NET komunicira sa servisom Bus servis pomoću namjenski protokola SOAP-poštu. Da biste koristili AMQP 1.0 umjesto zadanog protokol potrebna eksplicitnih konfiguracija na niz za povezivanje servisa Bus kao što je opisano u sljedećem odjeljku. Osim tu promjenu aplikacije kod ostaje nepromijenjen kada se koristi AMQP 1.0.

U trenutnom izdanju, postoji nekoliko značajki API koje nisu podržane prilikom korištenja AMQP. Ove značajke koje nisu podržane kasnije su navedene u odjeljku [značajke koje nisu podržane, ograničenja i behavioral razlike](#unsupported-features-restrictions-and-behavioral-differences). Neke napredne konfiguracijske postavke i imaju različite značenje prilikom korištenja AMQP.

### <a name="configuration-using-appconfig"></a>Konfiguriranje pomoću App.config

Dobro za korištenje App.config konfiguracijska datoteka za spremanje postavki aplikacijama je. Za aplikacije servisa Bus App.config možete koristiti za spremanje postavki za servis Bus **ConnectionString** vrijednost. Na primjer App.config datoteka je na sljedeći način:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Vrijednost na `Microsoft.ServiceBus.ConnectionString` je postavka niz za povezivanje servisa Bus koja se koristi za konfiguriranje veza Bus servisa. Oblik je na sljedeći način:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Gdje `[namespace]` i `SharedAccessKey` dobivaju se s [portala za Azure][]. Dodatne informacije potražite u članku [Početak rada sa servisa Bus redovima][].

Prilikom korištenja AMQP, dodavanje niz za povezivanje s `;TransportType=Amqp`. U ovom notacije obavještava klijentska biblioteka da biste vezu servisa Bus pomoću AMQP 1.0.

## <a name="message-serialization"></a>Serijalizacije poruke

Kada koristite zadani protokol, zadano ponašanje serijalizacije klijentska biblioteka .NET jest korištenje vrstu [DataContractSerializer][] serijalizirati [BrokeredMessage][] instancu za prijenos između biblioteke klijenta i servis za servis Bus. Kada koristite način prijenosa AMQP, klijentska biblioteka koristi vrsta sustava AMQP serijalizacije [brokered poruke][BrokeredMessage] u AMQP poruku. U ovom serijalizacije omogućuje poruku da biste dobili i interpretiraju primanju aplikacija sa sustavom potencijalno na različite platforme, na primjer, u aplikaciji Java koja koristi JMS API da biste pristupili Bus servisa.

Kada se izgraditi [BrokeredMessage][] instancu, možete unijeti .NET objekt kao parametar Graditelj da bi služio kao tijelo poruke. Za objekte koji se mogu mapirati AMQP jednostavne vrste tijelo Serijalizirano je u AMQP vrste podataka. Ako objekt nije moguće izravno mapirati u AMQP jednostavne vrstu; to jest, prilagođene vrste definira aplikacije, a zatim objekt serijalizirani pomoću [DataContractSerializer][]i serijaliziranog bajtova šalju se u poruci AMQP podataka.

Da biste olakšali interoperabilnost s klijentima koji nisu .NET, koristite samo .NET vrste koje se serijalizirati izravno u AMQP vrste za tijelo poruke. U sljedećoj su tablici detaljno te vrste i odgovarajuće mapiranje vrsta sustava AMQP.

| Vrsta objekta tijelo .NET          | Vrste mapiranih AMQP                          | AMQP tijelo sekcije vrsta                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| booleovom                           | Booleove vrijednosti                                   | Vrijednost AMQP                                                                                                                                                |
| bajt                           | ubyte                                     | Vrijednost AMQP                                                                                                                                                |
| ushort                         | ushort                                    | Vrijednost AMQP                                                                                                                                                |
| uint                           | uint                                      | Vrijednost AMQP                                                                                                                                                |
| ulong                          | ulong                                     | Vrijednost AMQP                                                                                                                                                |
| sbyte                          | bajt                                      | Vrijednost AMQP                                                                                                                                                |
| Kratki                          | Kratki                                     | Vrijednost AMQP                                                                                                                                                |
| INT                            | INT                                       | Vrijednost AMQP                                                                                                                                                |
| Dugi                           | Dugi                                      | Vrijednost AMQP                                                                                                                                                |
| vrijednost s pomičnim zarezom                          | vrijednost s pomičnim zarezom                                     | Vrijednost AMQP                                                                                                                                                |
| Dvostruki                         | Dvostruki                                    | Vrijednost AMQP                                                                                                                                                |
| broja decimalnih mjesta                        | decimal128                                | Vrijednost AMQP                                                                                                                                                |
| CHAR                           | CHAR                                      | Vrijednost AMQP                                                                                                                                                |
| Datum i vrijeme                       | Vremenska oznaka                                 | Vrijednost AMQP                                                                                                                                                |
| GUID                           | UUID                                      | Vrijednost AMQP                                                                                                                                                |
| bajt]                         | Binarni.                                    | Vrijednost AMQP                                                                                                                                                |
| niz                         | niz                                    | Vrijednost AMQP                                                                                                                                                |
| System.Collections.IList       | Popis                                      | Vrijednost AMQP: stavke koje se nalaze u zbirci može biti samo one koji su definirani u ovoj tablici.                                                             |
| System.Array                   | polja                                     | Vrijednost AMQP: stavke koje se nalaze u zbirci može biti samo one koji su definirani u ovoj tablici.                                                             |
| System.Collections.IDictionary | karte                                       | Vrijednost AMQP: stavke koje se nalaze u zbirci može biti samo one koji su definirani u ovoj tablici. Napomena: podržani su samo tipke niz.                        |
| URI-ja                            | Što je opisano niza (pogledajte tablici u nastavku) | Vrijednost AMQP                                                                                                                                                |
| DateTimeOffset                 | Što je opisano dugo (pogledajte tablici u nastavku)   | Vrijednost AMQP                                                                                                                                                |
| Vremenski raspon                       | Što je opisano dugo (pogledajte sljedeće)         | Vrijednost AMQP                                                                                                                                                |
| Strujanje                         | Binarni.                                    | Podaci o AMQP (može biti više). U odjeljcima podataka sadrže neobrađenog bajtova čitanje iz objekta strujanje.                                                           |
| Neki drugi objekt                   | Binarni.                                    | Podaci o AMQP (može biti više). Sadrži serijaliziranog binarni objekta koji koristi u DataContractSerializer ili serijalizatora koji vam je dao aplikacije. |

| Vrsta .NET      | Mapirani AMQP opisane vrsta                                                                                              | Bilješke                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI-ja            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Vremenski raspon       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nepodržane značajke i ograničenja behavioral razlike

Sljedeće značajke API usluge .NET za Bus trenutno nisu podržane prilikom korištenja AMQP:

-   Transakcije

-   Slanje putem odredište prijenos

Postoje i neke small razlike u ponašanja API usluge .NET za Bus prilikom korištenja AMQP u usporedbi s zadani protokol:

-   Svojstvo [OperationTimeout][] bit će zanemarene.

-   `MessageReceiver.Receive(TimeSpan.Zero)`implementira se kao `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Dovršavanje poruke tako da lock tokeni možete samo poduzeti primatelje poruke koju prethodno primili poruke.

## <a name="controlling-amqp-protocol-settings"></a>Upravljanje postavkama AMQP protokola

API-ji .NET izložiti nekoliko postavki nadzire ponašanje AMQP protokol:

-   **MessageReceiver.PrefetchCount**: određuje početne odobrenja primjenjuje se na vezu. Zadana je vrijednost 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: kontrole maksimalnu veličinu okvira AMQP nudi tijekom Pregovori pri veze otvorite vremena. Zadano je 65 536 bajtova.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Ako su prijenosi batchable, tu vrijednost određuje najveći odgode za slanje dispositions. Po zadanom nasljeđuju pošiljatelja i primatelja. Pojedinačne pošiljatelja i primatelja možete nadjačati po zadanom, što je 20 milisekundi.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: određuje hoće li se veze AMQP uspostaviti putem SSL veze. Zadana je vrijednost **true**.

## <a name="next-steps"></a>Daljnji koraci

Jeste li spremni za dodatne informacije? Posjetite sljedeće veze:

- [Pregled servisa Bus AMQP]
- [AMQP 1.0 podrška za servis Bus particije redova i teme]
- [AMQP u Bus servisa za Windows Server]

  [Početak rada sa servisa Bus redovima]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Portal za Azure]: https://portal.azure.com
[Pregled servisa Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0 podrška za servis Bus particije redova i teme]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP u Bus servisa za Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
