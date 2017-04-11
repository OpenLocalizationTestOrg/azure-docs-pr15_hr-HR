<properties 
    pageTitle="Kako koristiti Bus servisa Azure s WebJobs SDK" 
    description="Saznajte kako koristiti Bus servisa Azure redovima i teme s WebJobs SDK." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Kako koristiti Bus servisa Azure s WebJobs SDK

## <a name="overview"></a>Pregled

Ovaj vodič sadrži C# kod primjere koji pokazuju kako pokretanje procesa primitku poruke Bus servisa Azure. Primjere koda pomoću [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzije 1.x.

Vodič pretpostavlja znati [kako stvoriti WebJob projekta u Visual Studio s nizovima veze koji upućuju na račun servisa za pohranu](websites-dotnet-webjobs-sdk-get-started.md).

Na koda Prikaži samo funkcije, a ne kod koji stvara u `JobHost` objekt kao u ovom primjeru:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

[Primjer koda za dovršavanje Bus servis](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) je u spremištu azure-webjobs-sdk-uzorka na GitHub.com.

## <a id="prerequisites"></a>Preduvjeti

Da biste radili s Bus servisa ćete morati instalirati paket NuGet [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) uz druge WebJobs SDK pakete. 

Morate postaviti niz za povezivanje AzureWebJobsServiceBus osim niza veze za pohranu.  To možete učiniti na `connectionStrings` odjeljku App.config datoteke, kao što je prikazano u sljedećem primjeru:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Ogledna projekt koji uključuje postavka niz veze servisa Bus u datoteci App.config, potražite u članku [primjer Bus servisa](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

U nizu za povezivanje možete postaviti i u okruženju Azure runtime koji nadjačava postavke App.config, kada se pokrene u WebJob u Azure; Dodatne informacije potražite u članku [Početak rada s WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Kako pokrenuti funkcije primitku poruke Bus servis reda čekanja

Da biste napisali funkcija koja se poziva WebJobs SDK primitku poruke reda čekanja, koristite na `ServiceBusTrigger` atribut. Atribut Graditelj vodi na parametar koji određuje naziv čekanja za ankete.

### <a name="how-servicebustrigger-works"></a>Kako funkcionira ServiceBusTrigger

SDK prima poruku u `PeekLock` način i pozive `Complete` na Ako funkciju uspješno Završi poruka ili poziva `Abandon` ne uspijete funkciju. Ako funkcija izvodi dulje od na `PeekLock` vremensko ograničenje, zaključavanje automatski obnoviti.

Servis Bus ne vlastitu rukovanje poison red koji se ne mogu kontrolirati ni konfigurirao WebJobs SDK. 

### <a name="string-queue-message"></a>Niz reda čekanja poruka

Sljedećim primjerom koda čita reda čekanja poruku koja sadrži niz i piše niz WebJobs SDK nadzorne ploče.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Bilješke:** Ako stvarate reda čekanja poruka u aplikaciji koja ne koristi WebJobs SDK, provjerite je li postaviti [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) "tekst na običnim".

### <a name="poco-queue-message"></a>POCO reda čekanja poruka

SDK automatski će ukloniti serijski broj reda čekanja poruku koja sadrži JSON za na POCO [(običan stari objekt CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) vrsta. Sljedećim primjerom koda čita reda čekanja poruku koja sadrži na `BlobInformation` objekt koji je na `BlobName` svojstvo:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Pokazuje kako koristiti svojstva s POCO za rad s blob-ova i tablica u istoj funkciji primjere koda, potražite u članku [prostora za pohranu redovi verziju ovog članka](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Ako je kod koji stvara poruku reda čekanja ne koristi WebJobs SDK, koristite kod slično kao u sljedećem primjeru:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Vrste ServiceBusTrigger surađuje

Osim toga `string` te vrste POCO, možete koristiti u `ServiceBusTrigger` atribut s bajtni polja ili `BrokeredMessage` objekt.

## <a id="create"></a>Upute za stvaranje poruka servisa Bus reda čekanja

Da biste napisali funkcija koja se stvara novi red koristi za poruke u `ServiceBus` atribut i prenesite u nazivu reda čekanja Graditelj atribut. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Stvaranje jednog reda čekanja poruke u funkciji koje nisu asinkrone

Sljedećim primjerom koda koristi izlazni parametar za stvaranje nove poruke u redu čekanja pod nazivom "outputqueue" s istim sadržajem kao primljene u redu čekanja pod nazivom "inputqueue" poruke.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Izlazni parametar za stvaranje jednog reda čekanja poruka može biti bilo koji od sljedećih vrsta:

* `string`
* `byte[]`
* `BrokeredMessage`
* Serializable POCO vrsta koju ste definirali. Automatski serijalizirani kao JSON.

Za POCO vrstu parametara reda čekanja poruke uvijek stvara se kada istekne funkciju; Ako je parametar null SDK stvara reda čekanja poruku koja će vratiti vrijednost null kada je poruka primili i deserijalizirati. Za ostale vrste ako je parametar null stvorene je bez čekanja poruke.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Stvaranje više reda čekanja poruka ili u funkcijama asinkrone

Da biste stvorili više poruka, poslužite se `ServiceBus` atribut s `ICollector<T>` ili `IAsyncCollector<T>`, kao što je prikazano u sljedećim primjerom koda:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Svaku poruku reda čekanja stvara se odmah kada na `Add` zove se način.

## <a id="topics"></a>Rad sa servisa Bus teme

Da biste napisali funkcija koja se poziva SDK se po primitku poruke o određenoj temi Bus servisa, koristite na `ServiceBusTrigger` atribut s Graditelj koja vas vodi tema ime i naziv pretplate, kao što je prikazano u sljedećim primjerom koda:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Da biste stvorili poruku o određenoj temi, koristite na `ServiceBus` atribut s nazivom temu na isti način kao i ga koristiti uz naziv reda čekanja.

## <a name="features-added-in-release-11"></a>Značajke dodane u izdanju 1.1

Sljedeće značajke su dodani u izdanju 1.1:

* Dopusti precizno prilagodbe obrade putem poruke `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`podržava prilagodbu Bus servisa `MessagingFactory` i `NamespaceManager`.
* A `MessageProcessor` uzorkom strategije omogućuje vam da navedete procesor po reda čekanja/temu.
* Poruka obrada istodobnosti podržana je prema zadanim postavkama. 
* Jednostavno prilagodbu `OnMessageOptions` putem `ServiceBusConfiguration.MessageOptions`.
* Dopusti [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) u `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (za scenarije koju nemate upravljanje pravima). 

## <a id="queues"></a>Povezane teme prekriveni u članku s uputama za pohranu redovi

Informacije o scenarijima WebJobs SDK nisu specifične za servis Bus potražite u članku [kako koristiti za pohranu Azure reda čekanja s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Teme u tom članku obuhvaćaju sljedeće:

* Funkcija asinkrone
* Više instanci
* Graceful zatvaranja
* Korištenje atributa WebJobs SDK u tijelu funkcija
* Postavljanje SDK nizu za povezivanje u kodu
* Postavljanje vrijednosti za WebJobs SDK Graditelj parametara u kodu
* Ručno pokretanje funkcije
* Pisanje zapisnika

## <a id="nextsteps"></a>Daljnji koraci

Ovaj vodič nudi primjere koda koji pokazuju kako rukovati uobičajeni scenariji za rad s Bus servisa Azure. Dodatne informacije o korištenju Azure WebJobs i WebJobs SDK potražite u članku [Azure WebJobs preporučuje resursi](http://go.microsoft.com/fwlink/?linkid=390226).
 
