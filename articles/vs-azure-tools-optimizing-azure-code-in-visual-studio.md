<properties
   pageTitle="Optimiziranje Azure kod u Visual Studio | Microsoft Azure"
   description="Informirajte se o kako Azure kod alata za optimizaciju u Visual Studio bi kod robusne i omogućuje lakše predstavljanja."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Optimiziranje Azure kod

Kada programiranje aplikacije koje koristi Microsoft Azure postoje načini kodiranje morate slijediti da biste izbjegli probleme s skalabilnost aplikacije, ponašanje i performanse u okruženje oblaka. Microsoft pruža alata za analizu Azure kod koji prepoznaje i označava nekoliko te probleme obično prikaže pomaže vam riješiti ih. Možete preuzeti alat u Visual Studio putem NuGet.

## <a name="azure-code-analysis-rules"></a>Azure kod analize pravila

Alat za analizu kod Azure koristi sljedeća pravila za automatsko označavanje zastavicom Azure kod prilikom pronađe Poznati problemi utječu na performanse. Otkriveni problemi prikazuju se kao na upozorenja ili pogreške prevoditelj (Compiler). Kod rješenja ili prijedloga riješiti upozorenja ili pogreške često se odvija pomoću ikonom pronađen u sadržaju.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Izbjegavanje zadani način za stanje sesije (u-proces)

### <a name="id"></a>ID-A

AP0000

### <a name="description"></a>Opis

Ako koristite zadani način stanje sesije (u-proces) za aplikacije u oblak, možete izgubiti stanje sesije.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Prema zadanim postavkama navedeni u datoteci web.config način stanje sesije je u tijeku. Osim toga, ako nema definiranog unosa u u konfiguracijskoj datoteci, način stanje sesije po zadanom odabrana u tijeku. Način rada u tijeku pohranjuje stanje sesije u memoriji na web-poslužitelj. Prilikom ponovnog pokretanja instance ili novu instancu služi za prebacivanje podršku ili za ujednačavanje opterećenja, neće biti spremljena stanje sesije pohranjenim u memoriji na web-poslužitelj. U ovom slučaju onemogućuje aplikacija onemogućuje skalabilni oblaka.

Stanje ASP.NET sesije podržava nekoliko različitih prostora za pohranu mogućnosti za podatke o stanju sesije: InProc, StateServer, SQLServer, Prilagođeno i isključiti. Preporučuje se korištenje prilagođeni način da biste podatke glavnog računala na vanjskim pohrana stanje sesije, kao što je [davatelj Azure stanje sesije za Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Rješenja

Jedan preporučeno rješenje je za pohranu stanje sesije servis za upravljane predmemoriju. Saznajte kako koristiti [stanje sesije Azure davatelj usluga za Redis](http://go.microsoft.com/fwlink/?LinkId=401521) za pohranu stanje sesije. Stanje sesije možete spremiti i na drugim mjestima da biste bili sigurni aplikacije skalabilni na oblaka. Dodatne informacije o zamjenskom rješenja pročitajte [Načini stanje sesije](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Pokretanje način ne smije biti asinkrone

### <a name="id"></a>ID-A

AP1000

### <a name="description"></a>Opis

Stvaranje asinkronog metode (primjerice [await](https://msdn.microsoft.com/library/hh156528.aspx)) izvan metodu [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , a zatim pozivanje metode asinkrone iz [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Deklariranje metodi [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) kao asinkrone uzrokuje ulogu suradnika da biste unijeli petlje ponovno pokretanje.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Pozivanje metode asinkrone unutar metodu [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) uzrokuje izvođenje servisa za oblak u koš za ulogu suradnika. Kada se pokrene u ulogu suradnika, sva izvršavanja program odvija unutar metodu [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Izlazak iz načina [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) uzrokuje ulogu suradnika da biste ponovno pokrenuli. Prilikom izvođenja ulogu suradnika dodirne metodu asinkrone, dispatches sve operacije nakon metodu asinkrone i zatim vraća. Zbog toga ulogu suradnika da biste izašli iz načina [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) i ponovno ga pokrenite. U sljedećem iteracijom izvođenja ulogu suradnika ponovno dodirne metodu asinkrone i pokreće, uzrok ulogu suradnika u koš za ponovno kao i.

### <a name="solution"></a>Rješenja

Postavite sve operacije asinkrone izvan metodu [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Nakon toga pozvati metodu refactored asinkrone iz unutar metoda [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , kao što su .wait RunAsync (). Alat za analizu kod Azure omogućuju riješili taj problem.

Sljedeći isječak koda pokazuje kod popravak za taj problem:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Korištenje servisa Bus zajednički pristup potpis provjere autentičnosti

### <a name="id"></a>ID-A

AP2000

### <a name="description"></a>Opis

Koristi zajednički pristup potpis (SAS) za provjeru autentičnosti. Pristup kontrola servisa (ACS) ukinuta je u tijeku za provjeru autentičnosti bus servisa.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Radi bolje sigurnosti Azure Active Directory zamjenjuje provjere autentičnosti ACS s provjerom autentičnosti SAS. U odjeljku [Azure Active Directory je budućnost ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) informacije u planu prijelaz.

### <a name="solution"></a>Rješenja

Pomoću provjere autentičnosti SAS u aplikacijama. Sljedeći primjer pokazuje kako koristiti postojeće token za SAS da biste pristupili polje naziva bus servisa ili entitet.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Potražite u sljedećim temama dodatne informacije.

- Pregled, potražite u članku [Zajedničko korištenje programa Access potpis Authentication s Bus servisa](https://msdn.microsoft.com/library/dn170477.aspx)

- [Kako koristiti zajednički se koristi Access Authentication potpis s Bus servisa](https://msdn.microsoft.com/library/dn205161.aspx)

- Ogledna projekt, potražite u članku [Provjera autentičnosti pomoću zajednički pristup potpis (SAS) uz servis Bus pretplate](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Razmislite o korištenju OnMessage način da biste izbjegli "primanje petlja"

### <a name="id"></a>ID-A

AP2002

### <a name="description"></a>Opis

Da biste izbjegli premještaju se u na "primanje petlji", nazovete podršku za metodu **OnMessage** je bolje rješenje za primanje poruka od metoda **primanje** poziva. Međutim, ako morate koristiti metodu **primanje** , a zatim odredite vrijeme čekanja koji nisu zadani poslužitelj, provjerite je li poslužitelj vrijeme čekanja je više od jedne minute.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Prilikom pozivanja **OnMessage**klijent pokreće se interna poruka pump koji stalno polls reda čekanja ili pretplatu. Ova poruka pump sadrži beskonačno Ponavljaj koji izdaje poziv primili poruku. Ako je vrijeme poziva, šalje se novi poziv. Interval vremenskog ograničenja određuje vrijednost svojstvo [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)koji se koristi.

Prednost korištenja **OnMessage** u usporedbi s **primanje** je korisnici ne moraju ručno ankete za poruke, rukovati iznimke, obradu više poruka paralelno i dovršavanje poruke.

Ako je **primanje** poziva bez korištenja njezina je zadana vrijednost, obavezno je vrijednost *ServerWaitTime* više od jedne minute. Postavljanje *ServerWaitTime* na više od jedne minute onemogućuje poslužitelj isteklo prije potpuno primio poruku.

### <a name="solution"></a>Rješenja

Pogledajte sljedeće primjere koda za preporučeni načini korištenja. Dodatne informacije potražite u članku [QueueClient.OnMessage postupak (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)i [QueueClient.Receive (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Da biste poboljšali performanse Azure infrastruktura za razmjenu poruka, potražite u članku uzorak dizajn [Asinkronog Primer razmjene poruka](https://msdn.microsoft.com/library/dn589781.aspx).

Slijedi primjer korištenja **OnMessage** primati poruke.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Slijedi primjer **primanje** pomoću zadano vrijeme čekanja poslužitelja.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Slijedi primjer korištenja **primanje** s vremenom čekanja koji nisu zadani poslužitelj.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Razmislite o korištenju asinkronog metode Bus servisa

### <a name="id"></a>ID-A

AP2003

### <a name="description"></a>Opis

Pomoću metode asinkronog Bus servisa da biste poboljšali performanse brokered poruke.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Asinkrona načine omogućuje istodobnosti program aplikacije jer izvršavanja svakog poziva ne blokira glavnom niti. Prilikom korištenja servisa Bus načina za razmjenu poruka, operaciju (slanje primanje, brisanje, itd.) traje. Ovaj put sadrži obradu operacije servis servisa Bus osim Latencija zahtjeva i odgovora. Da biste povećali broj operacija po vremenu, operacije morate izvršiti istovremeno. Dodatne informacije pogledajte [Najbolje prakse performanse poboljšanja pomoću servisa Bus Brokered razmjene poruka](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Rješenja

Informacije o korištenju preporučeni način asinkronog potražite u članku [QueueClient klase (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Da biste poboljšali performanse Azure infrastruktura za razmjenu poruka, potražite u članku uzorak dizajn [Asinkronog Primer razmjene poruka](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Preporučuje se stvaranje particija redova Bus servisa i teme

### <a name="id"></a>ID-A

AP2004

### <a name="description"></a>Opis

Particija servisa Bus redova i teme za bolje performanse servisa Bus poruke.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Performanse propusnost i servis dostupnosti particija servisa Bus redovima i teme povećava jer cjelokupan propusnost particioniranom reda čekanja ili temu više ograničen performanse broker jednu poruku ili SMS trgovine. Osim toga, privremene nedostupnosti spremišta za razmjenu ne provjerite particioniranom reda čekanja ili temu nije dostupan. Dodatne informacije potražite u članku [Particija entiteti razmjene poruka](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Rješenja

Sljedeći isječak koda pokazuje kako particija razmjenu entiteti.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Dodatne informacije potražite u članku [particije servisa Bus redova i teme | Blog o programu Microsoft Azure](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) i potražite u članku [Microsoft Azure Bus particije red čekanja servisu](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) uzorka.

## <a name="do-not-set-sharedaccessstarttime"></a>Postavljanje SharedAccessStartTime

### <a name="id"></a>ID-A

AP3001

### <a name="description"></a>Opis

Izbjegavajte korištenje SharedAccessStartTimeset za trenutno vrijeme da biste odmah započeti pravila za zajedničko korištenje programa Access. Samo morate postaviti to svojstvo, ako želite započeti pravila zajednički pristup kasnije.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Sinkronizacija sat uzrokuje napraviti manje vremena razlika između podatkovnim centrima. Na primjer, želite logično mislite da postavljanje vremena početka prostora za pohranu SAS pravila kao i trenutno vrijeme pomoću DateTime.Now ili sličan način dovesti do SAS pravilo koje će se odmah su vidljive. Međutim, napraviti manje vremena razlike između podatkovnim centrima može uzrokovati probleme s ovom nakon nekoliko puta podatkovnog centra može se malo kasnije od vremena početka, vrijeme drugi ispred ga. Zbog toga SAS pravila isteka brzo (ili čak i odmah) ako je postavljen premalen vijek pravila.

Dodatne upute o korištenju zajednički pristup potpis na Azure prostora za pohranu potražite u članku [Uvod u tablice SAS (zajednički pristup potpisa) SAS reda čekanja i ažuriranje SAS Blob - Blog tima za pohranu za Microsoft Azure – Početna stranica web-mjesta – MSDN blogove](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Rješenja

Uklanjanje naredbi koja se postavlja vremena početka pravila zajednički pristup. Alat za analizu kod Azure nudi rješenje za taj problem. Dodatne informacije o sigurnosti upravljanja, pročitajte članak dizajn uzorak [Uzorkom Valet ključ](https://msdn.microsoft.com/library/dn568102.aspx).

Sljedeći isječak koda pokazuje kod popravak za taj problem.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Zajedničko korištenje pravilnik o pristupu vrijeme isteka mora biti dulje od pet minuta

### <a name="id"></a>ID-A

AP3002

### <a name="description"></a>Opis

Može postojati kao razlika pet minuta u satovi među podatkovnim centrima na različitim mjestima zbog uvjeta naziva "sat skew." Da biste spriječili u SAS pravila tokena iz istječe starije od planirano, postavite vrijeme isteka biti dulje od pet minuta.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Podatkovnim centrima na različitim mjestima diljem svijeta sinkronizirati signal sat. Budući da je potrebno vrijeme za sat signal putujete različitih mjesta, može biti odstupanje vrijeme između podatkovnim centrima na različitim zemljopisnim područjima iako sve supposedly sinkronizirati. Ovaj put razlika može utjecati na zajednički pristup pravila start vrijeme i istek vremensko razdoblje. Dakle, da biste osigurali pravilnik o pristupu zajedničko stupa na snagu odmah, ne odredite vrijeme početka. Osim toga, provjerite je li vrijeme isteka više od pet minuta da biste spriječili Prijevremeni vremenskog ograničenja.

Dodatne informacije o korištenju zajednički pristup potpis na Azure prostora za pohranu potražite u članku [Uvod u tablice SAS (zajednički pristup potpisa) SAS reda čekanja i ažuriranje SAS Blob - Blog tima za pohranu za Microsoft Azure – Početna stranica web-mjesta – MSDN blogove](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Rješenja

Dodatne informacije o sigurnosti upravljanje potražite u članku dizajn uzorak [Uzorkom Valet ključ](https://msdn.microsoft.com/library/dn568102.aspx).

Slijedi primjer ne navodeći vrijeme početka zajednički pristup pravila.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Slijedi primjer navodeći vrijeme početka zajednički pristup pravila koji traju pravila isteka veće od pet minuta.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Dodatne informacije potražite u članku [Stvaranje i korištenje potpis programa Access za zajedničko korištenje](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Korištenje CloudConfigurationManager

### <a name="id"></a>ID-A

AP4000

### <a name="description"></a>Opis

Pomoću klase [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) za projekte kao što su Azure web-mjesta i Azure mobilne usluge neće predstavljanje runtime problema. Kao preporučenim načinom rada, međutim, nije dobro je koristiti oblaka[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) kao objedinjenih način upravljanja konfiguracije za sve aplikacije za Azure oblaka.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

CloudConfigurationManager čita odgovarajuću aplikaciju okruženje konfiguracijska datoteka.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Rješenja

Refactor kod da biste koristili [CloudConfigurationManager predmete](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Alat za analizu Azure kod je nudi kod rješenje za taj problem.

Sljedeći isječak koda pokazuje kod popravak za taj problem. Zamjena

`var settings = ConfigurationManager.AppSettings["mySettings"];`

s

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Evo primjera kako spremiti postavke konfiguracije u datoteku App.config ili Web.config. Dodavanje postavke u odjeljku appSettings konfiguracijske datoteke. Slijedi Web.config datoteke na prethodni primjer kod.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Izbjegavanje nizove programiranih veze

### <a name="id"></a>ID-A

AP4001

### <a name="description"></a>Opis

Ako koristite nizove programiranih veze i morate ih kasnije ažurirati, morat ćete promjene izvornog koda i prevoditi aplikacije. Međutim, ako spremate nizu za povezivanje u konfiguracijskoj datoteci, možete promijeniti ih kasnije ažuriranjem na konfiguracijskoj datoteci.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Kodiranje nizove veze je neispravna praksa jer predstavlja probleme kada nizove veze morate brzo promijeniti. Osim toga, ako projekt mora biti prijavljena kontrola izvora, nizove programiranih veze predstavljanje slabe jer nizovi moguće je prikazati u izvornog koda.

### <a name="solution"></a>Rješenja

Pohranite nizove veze u konfiguraciji datoteke ili Azure okruženja.

- Za aplikacije samostalni, koristite app.config za spremanje postavki niz veze.

- Za IIS hostira web-aplikacije, koristite web.config za pohranu nizove veze.

- Za aplikacije vNext ASP.NET, koristite configuration.json za pohranu nizove veze.

Informacije o korištenju datoteka konfiguracije kao što su web.config ili app.config, potražite u članku [Smjernice za konfiguraciju ASP.NET Web](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Informacije o kako Azure okruženje varijable rade, potražite u članku [Azure web-mjesta: kako nizova aplikacija i veza nizova radi](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Informacije o pohrani niz za povezivanje u kontroli izvorišnog, potražite u članku [Izbjegavajte smještanje povjerljive podatke kao što su nizovi veze u datotekama koje se pohranjuju u spremištu kod izvora](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Korištenje Dijagnostika konfiguracijska datoteka

### <a name="id"></a>ID-A

AP5000

### <a name="description"></a>Opis

Umjesto Konfiguriranje postavki dijagnostike u kodu kao što su pomoću Microsoft.WindowsAzure.Diagnostics programiranje API-JA, trebali biste konfigurirati postavki dijagnostike u datoteci diagnostics.wadcfg. (Ili diagnostics.wadcfgx ako koristite 2,5 Azure SDK). Tako da to učinite, možete promijeniti postavki dijagnostike bez potrebe za prevoditi kod.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Prije nego što Azure SDK 2,5 (koji koriti Azure Dijagnostika 1.3), Azure Dijagnostika (WAD) nije moguće konfigurirati pomoću nekoliko različitih načina: dodavanje blob konfiguraciju u prostor za pohranu, pomoću izuzetne kod, deklarativno konfiguraciju ili zadanu konfiguraciju. Međutim, željeni način konfiguriranja Dijagnostika je koristiti XML datoteku konfiguracije (diagnostics.wadcfg ili diagnositcs.wadcfgx SDK 2,5 i novijim verzijama) u programu project aplikacije. Na taj se način datoteke diagnostics.wadcfg potpuno definira konfiguraciju i možete se ažurirati i ponovno implementirati po želji. Miješanje korištenje konfiguracijska datoteka diagnostics.wadcfg s programske metode konfiguracija postavki pomoću klase [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)ili [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)može dovesti do zbrku. Dodatne informacije potražite [inicijalizaciju ili promjena Azure Dijagnostika konfiguracije](https://msdn.microsoft.com/library/azure/hh411537.aspx) .

Počevši od WAD 1.3 (sklopu Azure SDK 2,5), više nije moguće koristiti kod za konfiguriranje Dijagnostika. Zbog toga samo unijeti konfiguraciju prilikom primjene ili ažuriranje proširenje Dijagnostika.

### <a name="solution"></a>Rješenja

Pomoću dizajnera konfiguracije dijagnostiku da biste premjestili dijagnostičkih postavki dijagnostike konfiguracijska datoteka (diagnositcs.wadcfg ili diagnositcs.wadcfgx SDK 2,5 i novijim verzijama). Također preporučuje se da ste instalirali [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) i koristiti najnovije značajke Dijagnostika.

1. Na izborniku prečaca za ulogu koju želite konfigurirati, odaberite Svojstva, a zatim odaberite karticu konfiguracija.

1. U odjeljku **Dijagnostika** provjerite je li potvrđen okvir **Omogući Dijagnostika** .

1. Odaberite gumb **Konfiguracija** .

  ![Pristup mogućnost Omogući dijagnostiku](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Dodatne informacije potražite u članku [Konfiguriranje Dijagnostika za servise u Oblaku Azure i virtualnih računala](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Izbjegavanje deklariranje DbContext objekte kao što je statički

### <a name="id"></a>ID-A

AP6000

### <a name="description"></a>Opis

Da biste spremili memorije, nemojte deklariranje DBContext objekte kao što je statične.

Ponovno zajednički koristiti ideje i povratne informacije pri [Azure kod analize povratne informacije](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Razlog

Objekti DBContext držite rezultate upita s svaki poziv. Statične objekte DBContext su Rashodovano dok je domena aplikacije učitan. Stoga statički DBContext objekt može raditi velike količine memorije.

### <a name="solution"></a>Rješenja

Deklariranje DBContext kao lokalnu varijablu ili polja koji nisu statički instancu, koristite za zadatak i pa pričekajte Rashodovano nakon korištenja.

Sljedeći primjer MVC kontroler predmete prikazuje kako koristiti DBContext objekt.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o optimzing Azure aplikacije za otklanjanje poteškoća potražite u članku [Otklanjanje poteškoća s web-aplikacijama u aplikacije servisa za Azure pomoću Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
