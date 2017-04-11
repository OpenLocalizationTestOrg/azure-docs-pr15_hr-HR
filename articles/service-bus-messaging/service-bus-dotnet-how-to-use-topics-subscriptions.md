<properties
    pageTitle="Korištenje servisa Bus teme s .NET | Microsoft Azure"
    description="Saznajte kako pomoću servisa Bus teme i pretplata pomoću .NET u Azure. Primjere koda zapisuju za .NET aplikacije."
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Upute za korištenje servisa Bus teme i pretplate

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

U ovom se članku opisuje način korištenja servisa Bus teme i pretplate. Primjere zapisuju u C# i koristiti API-ji .NET. Scenariji prekriveno obuhvaćaju stvaranje teme i pretplata i stvaranje filtara za pretplatu, slanje poruka na temu, dobivate poruku iz pretplate i brisanje teme i pretplate. Dodatne informacije o temama i pretplate u odjeljku [sljedeće korake](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Konfiguriranje aplikacija za korištenje servisa Bus

Prilikom stvaranja aplikacije koja koristi Bus servisa, morate dodati referencu u sklopu servisa Bus i uključiti odgovarajući prostori. Najjednostavniji način za to je da biste preuzeli odgovarajuće [NuGet](https://www.nuget.org) paketa.

## <a name="get-the-service-bus-nuget-package"></a>Dohvaćanje paketa NuGet Bus servisa

[Paket NuGet Bus servis](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je Najlakši način da biste dobili Bus API servisa, a da biste konfigurirali aplikaciju s sve potrebne ovisnosti Bus servisa. Da biste instalirali paket servisa Bus NuGet u projektu, učinite sljedeće:

1.  U pregledniku rješenja, desnom tipkom miša kliknite **reference**, a zatim kliknite **Upravljanje NuGet paketa**.
2.  Traženje "Servisa Bus", a zatim odaberite stavku **Bus usluge za Microsoft Azure** . Kliknite da biste dovršili instalaciju **instalirati** , a zatim zatvorite dijaloški okvir sljedeće:

    ![][7]

Sada ste spremni za pisanje koda za Bus servisa.

## <a name="create-a-service-bus-connection-string"></a>Stvaranje niza za povezivanje servisa Bus

Servis Bus koristi niz za povezivanje za pohranu krajnje točke i vjerodajnice. Niz za povezivanje možete pohraniti na konfiguracijska datoteka, a ne kodiranje je:

- Kada pomoću servisa Azure, preporučuje se spremate pomoću servisa Azure konfiguracija sustava (.csdef i .cscfg datoteke) niz za povezivanje.
- Kada koristite Azure web-mjesta ili virtualnim računalima sustava Azure, preporučuje se spremate pomoću konfiguracije sustava .NET (na primjer, datoteka Web.config) niz za povezivanje.

U oba slučaja, možete dohvatiti pomoću veze niza na `CloudConfigurationManager.GetSetting` način, kao što je prikazano u nastavku ovog članka.

### <a name="configure-your-connection-string"></a>Konfiguriranje niz za povezivanje

Mehanizam za konfiguraciju servisa omogućuje dinamički Promjena postavki konfiguracije s [portala za Azure][] bez redeploying aplikacije. Na primjer, dodati na `Setting` natpis datoteku definicije (**.csdef**) servisa, kao što je prikazano u sljedećem primjeru.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Nakon toga navedite vrijednosti u datoteci konfiguracije (.cscfg) servisa.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Koristite naziv ključa zajednički pristup potpis (SAS) i ključne vrijednosti dohvaćene s portala sustava kao što je prethodno opisano.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfiguriranje niz za povezivanje pri korištenju Azure web-mjesta ili virtualnim računalima sustava Azure

Prilikom korištenja web-mjesta ili virtualnim strojevima, preporučuje se da koristite konfiguracija sustava .NET (na primjer, Web.config). Spremanje pomoću niza veze na `<appSettings>` element.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Koristite SAS naziv i ključne vrijednosti dohvaćene s [portala za Azure][], kao što je prethodno opisano.

## <a name="create-a-topic"></a>Stvaranje teme

Možete izvesti upravljanje radnje za servis Bus teme i pretplata pomoću klase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Klase navedene metode možete stvarati, numeriranje i brisati teme.

U sljedećem primjeru konstrukata na `NamespaceManager` objekt pomoću na Azure `CloudConfigurationManager` klase pomoću niza za povezivanje koji se sastoji od osnovne adrese servisa Bus prostora za naziv i odgovarajući SAS vjerodajnice s dozvolom za upravljanje ga. Niz za povezivanje je obrasca za sljedeće:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Koristite sljedeći primjer dali konfiguracijske postavke u prethodnom odjeljku.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Postoje overloads metodu [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) koji omogućuju vam da biste postavili svojstva teme; na primjer, da biste postavili zadani vrijeme važenja (TTL) vrijednost da se primjenjuje na poruke poslane na temu. Te se postavke primjenjuju se pomoću klase [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . Sljedeći primjer pokazuje kako stvoriti temi pod nazivom **TestTopic** s maksimalne veličine 5 GB i zadane poruke TTL 1 minute.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Da biste provjerili temu s navedenim nazivom već postoji li unutar prostor naziva možete koristiti metodu [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) na [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) objektima.

## <a name="create-a-subscription"></a>Stvaranje pretplate

Možete stvoriti i tema pretplata pomoću klase [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Pretplate se nazivaju i može imati neobavezno filtar koji ograničava skup poruka proslijeđena virtualne red svoje pretplate.

> [AZURE.IMPORTANT] Redoslijedom poruka koji se dobiva po pretplatu, morate stvoriti tu pretplatu prije slanja poruke na temu. Ako nema pretplate na temu, u temi odbacuje te poruke.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Stvaranje pretplate s filtrom zadani (MatchAll)

Ako nema filtra nije naveden stvaranja novu pretplatu, filtar **MatchAll** je zadani filtar koji se koristi. Kada koristite filtar **MatchAll** , sve poruke koje se objavljuju u temi spremaju se u redu čekanja za virtualne svoje pretplate. Sljedeći primjer stvara pretplatu pod nazivom "AllMessages" i koristi zadani **MatchAll** filtar.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Stvaranje pretplate s filtrima

Možete postaviti i tako filtre koje vam omogućuju da odredite koje poruke poslane na temu moraju se pojavljuju u određenoj temi pretplate.

Većina fleksibilne vrstu filtra podržava pretplate je klasa [SqlFilter][] koji implementira podskup SQL92. Filtri SQL rade na svojstva poruke koje su objavljene na temu. Dodatne informacije o izrazima koje je moguće koristiti s filtrom SQL potražite u članku sintaksa [SqlFilter.SqlExpression][] .

Sljedeći primjer stvara pretplatu pod nazivom **HighMessages** s objektom [SqlFilter][] koji samo odabire poruke koje sadrže prilagođeno svojstvo **MessageNumber** veći od 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Isto tako, sljedeći primjer stvara pretplatu pod nazivom **LowMessages** s [SqlFilter][] koji samo odabire poruke koje sadrže **MessageNumber** svojstvo manje od ili jednako 3.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Sada kada poruka je poslana `TestTopic`, uvijek isporučuju primatelje pretplaćeni na temu pretplate **AllMessages** i selektivno isporučuju primatelje pretplaćeni na temu pretplatama **HighMessages** i **LowMessages** (ovisno o sadržaj poruke).

## <a name="send-messages-to-a-topic"></a>Slanje poruka teme

Da biste poslali poruku teme Bus servisa, aplikacija stvara objekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) pomoću niza za povezivanje.

Sljedeći kod pokazuje kako stvoriti [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objekt za temu **TestTopic** stvorena ranije u [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API poziva.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Poruke poslane na servis Bus teme su instance klase [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Objekti [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) imati skup standardna svojstva (kao što su [oznake](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) rječnika koji koristi se za čuvanje prilagođena svojstva specifičnim aplikacijama i tijelo podataka proizvoljne aplikacije. Aplikaciju možete postaviti tijelo poruke prosljeđivanjem serializable objekata Graditelj [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objekt, a odgovarajuće **DataContractSerializer** pa se serijalizirati objekt u. Umjesto toga se može pružati **System.IO.Stream** .

Sljedeći primjer pokazuje kako poslati pet poruka test objekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) **TestTopic** nabavili u prethodnom primjeru kod. Imajte na umu da vrijednost svojstva [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) svake poruke ovisi o iteracije petlje (Time se određuje koje dobio).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Servis Bus teme podržava maksimalnu veličinu poruke od 256 KB u [standardni sloju](service-bus-premium-messaging.md) i 1 MB u [sloju Premium](service-bus-premium-messaging.md). Zaglavlja, koje obuhvaćaju svojstva standardnih ili prilagođenih aplikacija, mogu sadržavati maksimalnu veličinu od 64 KB. Nema ograničenja na broj poruka koji se održava u temi, ali postoji u kapaciteta ukupnu veličinu poruke zaključale temu. Ovu temu veličinu definirana je u trenutku stvaranja s gornju granicu 5 GB. Ako je omogućeno particija, gornju granicu veća je. Dodatne informacije potražite u članku [Partitioned porukama entiteti](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Kako se poruka iz pretplate

Preporučeni način poruka iz pretplate je koristiti [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objekt. Objekti [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) možete raditi u dva različita načina rada: [ *ReceiveAndDelete* i *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Korištenje načina **ReceiveAndDelete** primite je operacija jednim snimka; to jest, kada servis Bus primi zahtjev za poruku u pretplatu, vama označava da je poruka kao što je u tijeku potrošena i vraća aplikaciji. Način **ReceiveAndDelete** je najjednostavnije modela i najbolje odgovara scenariji u kojima možete aplikaciju tolerate ne obrađuje poruke u slučaju pogreške. Da biste shvatili, razmislite o scenarij u kojem se korisnik problema zahtjev za primanje i ruši se prije obradu. Jer je servis Bus sadrži Označi poruke kao potrošena, kada aplikacija pokreće i započinje ponovno troše poruke, imate će ga Propušteni poruku koja je potrošena prije no što se srušiti.

U načinu **PeekLock** (što je zadani način), postupka primanje postaje dvije faze postupak koji omogućuje za podršku aplikacije koje se ne može tolerate nedostaje poruke. Kada servis Bus primi zahtjev, ga Traži sljedeće poruke se utrošiti, zaključati da drugi korisnici ga prima sprječavanje i vraća je aplikacija. Kada aplikacija završi obradu poruka (ili pohranjuje pouzdano za buduće obrada), dovršava drugog stupnja postupka primanje tako da nazovete [Dovršeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) na primljene poruke. Kada je servis Bus vidi **Dovršeno** poziva, označava da je poruka kao što je u tijeku potrošena i uklanja iz pretplate.

Sljedeći primjer pokazuje kako se poruke mogu primati i obrađuju pomoću zadani način **PeekLock** . Da biste odredili neku drugu vrijednost [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , možete koristiti neki drugi preopterećenja za [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). Ovaj primjer koristi povratnog [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) postupak porukama koje stižu u **HighMessages** pretplatu.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

U ovom se primjeru konfigurira povratnog [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) pomoću objekta [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) . [Samodovršetak](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) postavljen na **false** da biste omogućili ručni nadzor nad kada se [dovrši](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) poziva na primljene poruke. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) je postavljeno na 1 minute, čime će se klijent proći do jedne minute prije prekidanje značajka automatskog obnavljanja i klijent čini novi poziv da biste potražili poruke. Vrijednost svojstva smanjuje broj puta klijent čini fakturiranje pozive koji dohvaćaju poruke.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kako rukovati ruši aplikacije i pronašao poruke

Servis Bus nudi funkcijama koje će vam pomoći da bez poteškoća vratite na aplikaciju ili poteškoća obrade poruke. Ako primanju aplikacija nije uspio obraditi poruku zbog nekog razloga, pa je možete nazvati metodu [ćete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) na primljene poruke (umjesto metode [Dovršeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ). Zbog toga Bus servisa da biste otključali poruku unutar pretplate i dostupnim moguće primiti ponovno iste trajati, aplikacije ili neku drugu aplikaciju dosta.

Postoji i isteka koji je povezan s porukom zaključan unutar pretplate, a ne uspijete aplikacija za obradu poruke prije isteka lock istekne (na primjer, ako ruši se aplikacija), servis Bus automatski ćete otključati poruku, a on postaje dostupan za ponovno primili.

Za slučaj da aplikacije zatvara se nakon obrade poruku, ali prije nego što se izda zahtjev za [dovršetak](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , poruka će biti redelivered aplikaciji prilikom ponovnog pokretanja. To se često naziva da *barem jednom obradu*; to jest, svaku poruku obrađuje barem jednom, ali u određenim situacijama možda redelivered ista poruka. Ako scenarij ne tolerate duplicirane obrada, zatim razvojnim inženjerima treba logike dodatne njihovih aplikacija za rukovanje isporuka duplicirane poruke. To je često postići pomoću svojstvo [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) poruka ostaje nepromijenjen preko pokušao.

## <a name="delete-topics-and-subscriptions"></a>Brisanje teme i pretplate

Sljedeći primjer pokazuje kako izbrisati temi **TestTopic** iz polje naziva servisa **HowToSample** .

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Brisanje teme briše i sve pretplate registrirane s temom. Pretplate se brišu i zasebno. Sljedeći kod pokazuje kako izbrisati pretplatu pod nazivom **HighMessages** iz **TestTopic** temu.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Bus teme i pretplate, slijedite ove veze da biste saznali više.

-   [Redova, teme i pretplate][].
-   [Ogledni teme filtara][]
-   Pregled API-JA za [SqlFilter][].
-   Sastavljanje rad aplikacije koja se šalje i prima poruke i iz servisa Bus reda čekanja: [servis Bus brokered razmjenu .NET vodič][].
-   Servis Bus uzorke: preuzmite iz trgovine [Azure uzoraka][] ili potražite u članku [Pregled](service-bus-samples.md).

  [Portal za Azure]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Redova, teme i pretplate]: service-bus-queues-topics-subscriptions.md
  [Ogledni teme filtara]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Praktični vodič za .NET razmjenu poruka servisa Bus brokered]: service-bus-brokered-tutorial-dotnet.md
  [Azure uzorka]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
