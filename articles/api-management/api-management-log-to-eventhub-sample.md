<properties
    pageTitle="Praćenje API-ji s Azure API upravljanja, koncentratora za događaj i Runscope"
    description="Primjer aplikacije takvi pravila zapisnika eventhub povezne Azure API Management, Azure događaj koncentratora i Runscope za HTTP bilježenje i praćenje"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Praćenje API-ji s Azure API upravljanja, koncentratora za događaj i Runscope

[Usluga upravljanja API](api-management-key-concepts.md) nudi mnogo mogućnosti da biste poboljšali obrade zahtjeva za HTTP poslanih u HTTP API-jem. Međutim, su tranzitne postojanje parametra zahtjeve i odgovore. Zahtjev je izvršena i teče putem servisa za upravljanje API-JA za pozadinskog API-JA. Vaš API obrađuje zahtjev i odgovor teče unatrag kroz potrošača API-JA. Servis za upravljanje API zadržava neke važne statističkih podataka o API-ji za prikaz u programu Publisher portala nadzornoj ploči, ali beyond da detalje više nema.

Pomoću [zapisnika eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [pravila](api-management-howto-policies.md) u servis za upravljanje API pošaljete detalje iz zahtjeva i odgovora [Azure događaj koncentratora](../event-hubs/event-hubs-what-is-event-hubs.md). Postoje razni od razloga zašto možda želite Generiraj događaje iz HTTP poruka šalje se na vaše API-ji. Primjeri obuhvaćaju popratne ažuriranja, analitičkih, iznimku upozorenjem i 3 integracije proizvođača.   

U ovom se članku objašnjava kako snimiti cijelu HTTP zahtjeva i odgovora poruku, poslati koncentratora za događaj i preusmjeravanje poruke trećoj strani servis koji omogućuje HTTP bilježenje i praćenje servisa.

## <a name="why-send-from-api-management-service"></a>Zašto se poslati iz servisa za upravljanje API-JA?
Nije moguće pisati HTTP proizvod koji se može priključiti HTTP API okviri za snimanje HTTP zahtjeva i odgovora i sažetka sadržaja u bilježenje i praćenje sustavi. Negativna strana tog pristupa je proizvod HTTP mora biti integriranom pozadinskog API-JA i moraju se podudarati platformu na API-JA. Ako postoji više API-ji svaki od njih mora implementirati na proizvod. Često je razloga zašto nije moguće ažurirati pozadinskog API-ji.

Pomoću servisa Azure API Management za integraciju s infrastruktura za zapisivanje nudi središnje i platformu neovisno rješenja. Ujedno skalabilni djelomice zbog [zemlj replikacije](api-management-howto-deploy-multi-region.md) mogućnosti upravljanja Azure API-JA.

## <a name="why-send-to-an-azure-event-hub"></a>Zašto se poslati koncentrator za događaj programa Azure?
Ovo je pametnije je Pitaj, zašto stvoriti pravila koja se odnose na Azure događaj koncentratora? Postoje brojnih i različitih mjesta gdje se možda želite prijave Moje zahtjeva za. Zašto ne samo slati zahtjeve izravno na konačno odredište?  To je mogućnost. Međutim, pri stvaranju zapisivanje zahtjeve iz servisa za upravljanje sustava API je potrebno uzeti u obzir kako zapisivanje poruke će utjecati na performanse na API-JA. Postupne povećanja u opterećenja se može riješiti tako da joj povećate dostupna instance komponente sustava ili putem zemlj replikacije. Međutim, kratki krivina u promet mogu prouzročiti zahtjevi za se može značajno odgoditi ako usporiti opterećenju zahtjevi za zapisivanje infrastrukture.

Azure koncentratora događaj namijenjen je ingress velikog količine podataka, kapaciteta za povezanima s znatno veći broj događaja od broj HTTP zahtjeva za većinu API-ji postupak. Središtu događaj ponaša se kao vrstu sofisticirane međuspremnik između servisa za upravljanje API-JA i infrastrukture koji će spremiti i obrađivati poruke. Time se osigurava da performansi API će se zbog infrastruktura za zapisivanje.  

Kada podatke prenesen na događaj koncentrator ga je ista i i čekati na koje korisnici događaj koncentrator za obradu ga. Koncentrator događaj važnih kako će se obrađuju, ga samo vodi brigu o provjerite je li poruka će biti uspješno isporučene.     

Događaj koncentratora imati mogućnost strujanje događaja koji se više grupa korisnika. Time se omogućuje događaja koji se može obraditi potpuno različite sustave. Time se omogućuje podrške mnogo scenarija integracije ne stavlja Zbrajanje kašnjenja obrade zahtjeva za API unutar servisa za upravljanje za API kao što je samo jedan događaj mora biti generira.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Pravila za slanje poruka u aplikaciji/http
Koncentratora za događaj prihvaća podaci o događaju kao niz jednostavne. Sadržaj koji se potpuno su oko. Da biste mogli objediniti HTTP zahtjev i poslali ga događaj koncentratora moramo oblikovati niz s informacijama zahtjeva i odgovora. U slučajevima kao što je ovaj ako postoji postojeći oblik ćemo je možete koristiti, a zatim ćemo nemate pisati našim Raščlanjivanje kod. Na početku se smatra pomoću [HAR](http://www.softwareishard.com/blog/har-12-spec/) za slanje HTTP zahtjeva i odgovora. Međutim, ovaj oblik optimiziran je za pohranu niz HTTP zahtjeva u obliku JSON temelji. Sadrži broj obavezno elemente koji se dodaje nepotrebne složenosti scenarija od prosljeđivanje poruka HTTP pokazivač na žičani.  

Druga mogućnost je za korištenje na `application/http` vrsta medija, kao što je opisano u specifikacija HTTP [RFC 7230](http://tools.ietf.org/html/rfc7230). Ovu vrstu medija koristi iste oblikovanja koja se koristi za zapravo poslati HTTP poruka putem sustava žičani, ali cijelu poruku može smjestiti u tijelu zahtjeva za drugi HTTP. U našem slučaju smo samo namjeravate koristiti tijelo kao naš poruku da biste poslali koncentratora za događaj. Jednostavnijeg postoji analizator koji postoji u bibliotekama [Microsoft ASP.NET web-API 2.2 klijentu](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) koji mogu raščlaniti ovaj oblik i pretvoriti u lokalnog `HttpRequestMessage` i `HttpResponseMessage` objekte.

Da biste mogli stvoriti ovu poruku moramo da iskoristite prednost C# temelji [pravila izraza](https://msdn.microsoft.com/library/azure/dn910913.aspx) u upravljanju API Azure. Ovdje je pravilo koje šalje poruku HTTP zahtjev za Azure događaj koncentratora.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Deklariranje pravila
Postoji nekoliko stvari određene vrijednosti Spominjanje o ovaj izraz pravila. Pravila eventhub zapisnika ima atribut naziva zapisivaču id koji se odnosi na naziv zapisivaču stvorenog unutar servisa za upravljanje za API-JA. Pojedinosti o tome kako instalacija programa zapisivaču koncentrator događaja u servis za upravljanje API pronaći ćete u dokumentu [kako Zapiši Azure koncentratora događaja u upravljanju Azure API -JA](api-management-howto-log-event-hubs.md). Drugi atribut je neobavezan parametar koji upućuje koncentratora događaja koje particije za spremanje poruka u. Događaj koncentratora pomoću particije da biste omogućili scalabilty i zahtijevaju najmanje dva. Uređeni isporuku poruke samo zajamčiti unutar particije. Ako ne možemo uputite u koje particije da biste postavili poruku, koristit će kružnog algoritam za raspodjelu opterećenja koncentrator za događaj. Međutim, koji može uzrokovati neke od naše poruka obraditi izvan redoslijeda.  

### <a name="partitions"></a>Particije
Da biste bili sigurni naš poruke isporučuju se korisnici mogli i iskoristiti raspodjele mogućnost Učitaj razdjeljivanja, koju sam odabrao da biste poslali poruke sa zahtjevom za HTTP particija i HTTP odgovor poruka na drugi particije. To će osigurati s raspodjelom čak i učitavanje, a zatim ćemo jamči da sve zahtjeve za će se utrošiti redoslijedom i sve odgovore će se utrošiti redoslijedom. Moguće je da odgovor biti potrošena prije odgovarajući zahtjev, ali kao što je to nije problem kao imamo različite mehanizam za correlating zahtjeve za odgovore i bismo znali da se zahtjevi za uvijek dolaziti prije odgovore.

### <a name="http-payloads"></a>HTTP payloads
Nakon sastavnih na `requestLine` ćemo provjerite je li biti odrezana tijelu zahtjeva. U tijelu zahtjeva skraćuju se na samo 1024. To može se povećati, no pojedinačne poruke događaj koncentrator ograničeni su na 256KB, pa je vjerojatnost da neke poruke HTTP tijela će ne pristaju ni u jednu poruku. Pri izvođenju zapisivanje i analitiku veliku količinu podataka može biti izvedena iz samo HTTP zahtjev za redak i zaglavlja. Osim toga, mnogo API zahtjeva samo vratite small tijela i tako da je prilično minimalnog usporedbi smanjenje prijenos, obradu i pohranu troškove da biste zadržali sav sadržaj tijela gubitak podataka vrijednost rezanja velike tijela. Konačni OneNote o obradi tijelo je potrebna za prosljeđivanje `true` da biste u obliku<string>način () jer smo čitanja sadržaja tijela ali je i želite pozadinskog API da biste mogli pročitati tijelo. Po Prenos TRUE u ovu metodu smo prouzročiti tijelo da biste se u međuspremniku tako da ga može pročitati drugi put. To je važno je znati ako imate API-JA koji ne prijenos velikih datoteka ili koristi dugo ankete. U tim slučajevima bi trebali biste izbjegli tijelo za čitanje.   

### <a name="http-headers"></a>HTTP zaglavlja
HTTP zaglavlja možete jednostavno prenijeti putem u oblik poruke u obliku par jednostavne ključa/vrijednosti. Ne možemo ste odabrali za uklanjanje out određena sigurnost osjetljive polja, da bi se izbjeglo nepotrebno propušta vjerodajnica informacije. Je vjerojatno da bi analize svrhe može koristiti tipke API-JA i ostalih vjerodajnica. Ako želimo za analizu na korisnika i određeni proizvod koriste, a da ne može se koristiti koji od na `context` objekta te dodali poruku.     
### <a name="message-metadata"></a>Poruka metapodataka
Kada sastavljanjem cijelu poruku da biste poslali koncentrator događaja u prvom retku nije zapravo dio na `application/http` poruku. U prvom retku je Dodatni metapodaci koji se sastoji od je li poruka zahtjev ili poruka s odgovorom i id poruke koja se koristi za povezivanje zahtjevi za odgovore. Id poruke se stvara pomoću drugog pravila koja izgleda ovako:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Nije moguće smo ste stvorili poruci zahtjeva, koji pohranjene u varijablu dok odgovor je vratiti, a zatim jednostavno pošalje zahtjeva i odgovora kao jednu poruku. Međutim, neovisno o slanju zahtjeva i odgovora i korištenje id poruke za povezivanje dva, ćemo dobiti veću fleksibilnost veličine poruka, mogućnost da biste iskoristili više particija whilst zaštiti redoslijed poruke i zahtjev će se pojaviti u našem zapisivanje nadzorne ploče ranije. Također mogu postojati nekim scenarijima kojima valjani odgovor nikad se ne šalju koncentrator događaj, vjerojatno zbog pogreške kobne zahtjev u servisa za upravljanje API-JA, ali i dalje će imamo zapis zahtjeva.

Pravila da biste poslali poruku HTTP odgovor izgleda slično je zahtjev i tako da se konfiguracija dovršeno pravila izgleda ovako:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

Na `set-variable` pravila stvara vrijednost koja je dostupno i u `log-to-eventhub` pravila u `<inbound>` sekcije i `<outbound>` sekciju.  

## <a name="receiving-events-from-event-hubs"></a>Primanje događaje iz koncentratora za događaj
Događaje iz centra za događaj Azure primaju [AMQP protokol](http://www.amqp.org/). U tim Bus servisa Microsoft učinjene klijenta biblioteke dostupne da biste olakšali dosta događaja. Postoje dvije različite pristupa podržava, jedan se *Izravno potrošača* i ostale koristi u `EventProcessorHost` predmete. Primjeri sljedećih dva pristupa pronaći ćete u [Vodiču za programiranje događaja koncentratora](../event-hubs/event-hubs-programming-guide.md). Kratka verzija razlike je, `Direct Consumer` omogućuje potpunu kontrolu i `EventProcessorHost` ne dio posla plumbing za ali čini određene pretpostavke o kako će obrade te događaja.  

### <a name="eventprocessorhost"></a>EventProcessorHost
U ovom primjeru koristit ćemo u `EventProcessorHost` zbog jednostavnosti, no je možda nije najbolji odabir za određeni scenarij. `EventProcessorHost`ne napravljenoga od pazeći da ne morate brinuti o niti problemi procesor klase određeni događaj. Međutim, u našem scenariju smo su jednostavno pretvaranje poruku u drugom obliku i prosljeđivanje ga uzduž na drugi servis pomoću metode asinkrone. Nema potrebe za ažuriranje zajedničkog stanja i stoga bez rizika niti problema. Za većinu scenarije `EventProcessorHost` vjerojatno je najbolji odabir, a certainly lakše mogućnost.     

### <a name="ieventprocessor"></a>IEventProcessor
Središnje pojam prilikom korištenja `EventProcessorHost` je da biste stvorili na implementacija u `IEventProcessor` sučelje koje sadrži način `ProcessEventAsync`. Ovdje je prikazana bit taj način:

  asinkrone {IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) zadatka

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Popis objekata EventData prosljeđuju se u način, a zatim ćemo ponavljanje tog popisa. Bajtova svaki način su raščlaniti u HttpMessage objekt, a zatim taj objekt se prenosi instance komponente IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
Na `HttpMessage` instancu sadrži tri podatka:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

U `HttpMessage` sadrži instance programa `MessageId` GUID koji omogućuje nam da biste se povezali HTTP zahtjev za odgovarajuće HTTP odgovor i logička vrijednost koja određuje ako objekt sadrži instance komponente HttpRequestMessage i HttpResponseMessage. Pomoću ugrađenoj u HTTP klase iz `System.Net.Http`, sam mogao da iskoristite prednost u `application/http` Raščlanjivanje kod koji se isporučuje u `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
Na `HttpMessage` instancu zatim proslijediti implementacija `IHttpMessageProcessor` koji je sučelje stvorili decouple primanje i tumačenja događaj iz centra za događaj Azure i konkretnu obradu ga.


## <a name="forwarding-the-http-message"></a>Prosljeđivanje poruke HTTP
Ovaj primjer I odlučili bio bi zanimljive automatske HTTP zahtjev putem [Runscope](http://www.runscope.com). Runscope je servis u oblaku specializes u HTTP pogrešaka, bilježenje i praćenje. Besplatni sloj imaju tako da je jednostavno možete pokušati i zatražit da biste vidjeli HTTP zahtjeva u stvarnom vremenu slijedi kroz naših usluga upravljanja API-JA.

Na `IHttpMessageProcessor` implementaciju izgledaju ovako

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Sam mogao da iskoristite prednost u [postojećoj biblioteci klijenta za Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) koji olakšava automatske `HttpRequestMessage` i `HttpResponseMessage` instance prema gore u svoje servis. Da biste pristupili Runscope API ćete poslovnog subjekta i ključa API-JA. Upute za početak ključa API pronaći ćete u screencast [Stvaranje aplikacije programa Access Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) .

## <a name="complete-sample"></a>Gotovi ogledni
[Izvorni kod](https://github.com/darrelmiller/ApimEventProcessor) i testira uzorka koji se nalaze na Github. Morate [API servisa za upravljanje](api-management-get-started.md), [Povezanog koncentratora za događaj](api-management-howto-log-event-hubs.md)i [Račun za pohranu](../storage/storage-create-storage-account.md) da biste pokrenuli uzorka za sebe.   

Uzorak je samo jednostavne konzole aplikacija koja očekuje podatke za događaja koji dolaze iz koncentratora za događaj pretvara ih u na `HttpRequestMessage` i `HttpResponseMessage` objekata, a zatim prosljeđuje na Runscope API-JA.

Na sljedećoj slici animirane možete vidjeti na zahtjev za API na portalu za razvojne inženjere, konzole aplikacija prikazuje poruku koja se primili, obrađuju i proslijeđene, a zatim zahtjeva i odgovora prikazuju promet Runscope kontrola.

![Demonstracija zahtjeva za prosljeđivanje na Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Sažetak
Azure servisa za upravljanje API nudi prikladno mjesto da biste snimili promet HTTP Izvorište ili odredište vaše API-ji. Azure koncentratora događaj je vrlo prilagodljivi, malo troškova rješenja za snimanje te promet i umetanje u sekundarni obrada sustavi za zapisivanje, nadzor i druge jednostavnije analize. Povezivanje s 3 strana promet sustavi kao Runscope je jednostavno kao nekoliko redaka dozen koda za nadzor.

## <a name="next-steps"></a>Daljnji koraci
-   Dodatne informacije o Azure događaj koncentratora
    -   [Početak rada s koncentratorima Azure događaja](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Primanje poruka uz EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Programiranje vodič koncentratora za događaj](../event-hubs/event-hubs-programming-guide.md)
-   Dodatne informacije o Integracija upravljanje API-JA i koncentratora događaja
    -   [Kako Zapiši Azure koncentratora događaja u upravljanju API Azure](api-management-howto-log-event-hubs.md)
    -   [Referenca entitet zapisivaču](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Referenca za zapisnik eventhub pravila](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    