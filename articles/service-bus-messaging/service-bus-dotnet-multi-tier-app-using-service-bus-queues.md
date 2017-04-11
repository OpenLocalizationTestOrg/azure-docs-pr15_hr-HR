<properties
    pageTitle="Aplikacija više razina .NET | Microsoft Azure"
    description="Praktični vodič za .NET, koji olakšava razviti aplikacije s više razina u Azure koji koristi servis Bus redova komunikaciju između razine."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET više razina aplikacije pomoću redova Bus servisa Azure

## <a name="introduction"></a>Uvod

Jednostavno pomoću Visual Studio i besplatne Azure SDK za .NET je razvoj za Microsoft Azure. Pomoću ovog praktičnog vodiča vodit će vas kroz korake za stvaranje aplikacije koja koristi više Azure resursa izvodi u lokalnom okruženju. Pretpostavlja imate bez rad Azure prethodnog sa.

Saznat ćete sljedeće:

-   Kako omogućiti na računalu Azure razvoja s jednom preuzimanje i instalacija.
-   Kako se pomoću programa Visual Studio razvoju za Azure.
-   Kako stvoriti aplikaciju za više razina u Azure pomoću web- a tempiranja uloga.
-   Upute za komunikaciju između razine pomoću servisa Bus redova.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

U ovom ćete praktičnom vodiču ćete Sastavljanje i pokrenuti aplikaciju s više razina u Azure oblaku. Sučelje bit će ulogu ASP.NET MVC web i pozadinska će se tempiranja-uloge koja se koristi reda Bus servisa. Možete stvoriti iste više razina aplikaciju pomoću sučelja kao projekt web koja je postavila Azure web-mjestu umjesto servis u oblaku. Upute o tome što učiniti drugačije na sučelje koji Azure web-mjesta potražite u odjeljku [sljedeće korake](#nextsteps) . Možete pokušati i članak vodič za [.NET na – lokalno/oblaka hibridnog aplikacije](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) .

Na sljedećoj je snimci zaslona prikazuje dovršene aplikacije.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Pregled scenarij: komunikacije među uloga

Da biste poslali reda za obradu, pristupne komponenta korisničkog Sučelja izvodi u ulozi web morate raditi s logike Srednji sloj izvodi u ulogu suradnika. U ovom se primjeru koristi servis Bus brokered poruka za komunikaciju između na razine.

Putem razmjene poruka brokered između web- a srednjem razine decouples dviju komponenata. Za razliku od izravne poruke (to jest, TCP i HTTP-a), web razina mogao povezati s Srednji sloj izravno; Umjesto toga ga ih gura jedinica posla, kao poruke, u servis Bus pouzdano ih zadržati dok srednji sloj bude spremna za zauzeti i obrada ih.

Servis Bus nudi dva entiteti za podršku brokered poruka: redova i teme. Sa redovima, svake poruke poslane na red koriste jednog tekstnog okvira. Tema podržava uzorak Objavi/pretplata u kojem postane dostupna na pretplatu registriran u temi svaku objavljene poruku. Pretplate logično održava vlastitu reda čekanja poruka. Pretplate mogu se konfigurirati i s filtar pravila koja ograničiti skup poruka proslijeđena čekanja za pretplatu na one koji se podudaraju s filtrom. U sljedećem primjeru pomoću servisa Bus redova.

![][1]

Mehanizam za komunikaciju sadrži nekoliko prednosti u odnosu na izravne poruke:

-   **Vremenski decoupling.** S asinkronog razmjenu uzorak proizvođača i koje korisnici moraju neće biti online istovremeno. Servis Bus pouzdano sprema poruke dok dosta strana spreman primati. Time se omogućuje komponente distribuirane aplikacije prekidanjem, ili po želji, na primjer, za održavanje ili zbog neočekivanog komponente, bez utjecaja sustava kao cjelinu. Osim toga, dosta aplikacije možda trebate samo prijavi tijekom vremena određene dana.

-   **Učitavanje ujednačavanje.** U mnogim aplikacijama sustavu opterećenje mijenja se s vremenom, dok je obrada vrijeme potrebno za svaku jedinicu poslovne obično konstante. Intermediating proizvođača poruke i korisnici s reda, to znači da dosta aplikacije (tempiranja) samo mora biti dodijeljena da bi odgovarao average opterećenja umjesto Vršna Učitaj. Dubina reda čekanja rastom i ugovori kao mijenja se dolazne opterećenje. Time izravno novac pomoću količinu infrastrukture potrebne za učitavanje aplikacije.

-   **Za ujednačavanje opterećenja.** Tijekom učitavanja povećava više radnih procesa možete dodati da biste pročitali iz reda. Samo jedan od radnih procesa obraditi svaku poruku. Osim toga, ova utemeljen na istaknuti za ujednačavanje opterećenja omogućuje optimalnog korištenja strojeva radnih čak i ako se strojeva tempiranja razlikuju pomoću power obrada kao će istaknuti poruke vlastite Maksimalna brzina. Ovaj uzorak je često termed uzorak *natječu potrošača* .

    ![][2]

U sljedećim se odjeljcima navode kod koji implementira tu arhitekturu.

## <a name="set-up-the-development-environment"></a>Postavljanje okruženja za razvoj

Prije no što započnete razvoj aplikacija za Azure, dobiti alate i postaviti razvojno okruženje.

1.  Instalirajte Azure SDK za .NET na [alate i SDK][].

2.  Kliknite **Instaliraj SDK** za verziju programa Visual Studio koristite. Koraci u ovom ćete praktičnom vodiču pomoću Visual Studio 2015.

4.  Kada se to od vas zatraži da pokrenete ili spremite instalacijski program, kliknite **Pokreni**.

5.  U **Web platformu Installer**, kliknite **Instaliraj** i nastaviti s instalacijom.

6.  Nakon dovršetka instalacije, imat ćete sve potrebne za pokretanje za razvoj aplikacija. SDK sadrži alata koji omogućuju vam da jednostavno razvoj aplikacija za Azure u Visual Studio. Ako nemate ugrađen Visual Studio, SDK instalira i na besplatnu Visual Studio Express.

## <a name="create-a-namespace"></a>Stvaranje prostor naziva

Sljedeći je korak da biste stvorili polje naziva servisa i dobiti ključ zajednički pristup potpis (SAS). Prostor naziva nudi programa granicu aplikacije za svaku aplikaciju vidljiva kroz Bus servisa. Ključ SAS generira sustav stvaranja prostor naziva. Kombinacija prostor naziva i ključ SAS nudi vjerodajnice za Bus servis za provjeru autentičnosti pristup aplikaciji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Stvaranje web uloga

U ovom ćete odjeljku međuverziju sučelja aplikacije. Prvo, stvorite stranica koja se prikazuje aplikacija.
Nakon toga dodati kod koji se šalje stavke servisa Bus red i prikazuje informacije o stanju o reda.

### <a name="create-the-project"></a>Stvaranje projekta

1.  Koristite administratorske ovlasti, pokrenite Microsoft Visual Studio. Da biste pokrenuli Visual Studio s administratorskim ovlastima, desnom tipkom miša kliknite ikonu programa **Visual Studio** , a zatim kliknite **Pokreni kao administrator**. Emulator Azure računalnim koji se spominju u nastavku ovog članka zahtijeva da se Visual Studio rada s administratorskim ovlastima.

    U Visual Studio, na izborniku **datoteka** kliknite **Novo**, a zatim kliknite **projekt**.

2.  Iz **Instalirani predlošci**, u odjeljku **Visual C#**, kliknite **oblačić** , a zatim kliknite **Azure u Oblaku**. Naziv projekta **MultiTierApp**. Zatim kliknite **u redu**.

    ![][9]

3.  Iz uloga **.NET Framework 4,5** dvokliknite **ASP.NET Web uloge**.

    ![][10]

4.  Pokazivač miša postavite iznad **WebRole1** u odjeljku **rješenja Azure u Oblaku**, kliknite ikonu olovke i preimenujte ulogu web **FrontendWebRole**. Zatim kliknite **u redu**. (Pripazite da unesete "Sučelju" s u mala "e" ne "sučelju".)

    ![][11]

5.  U dijaloškom okviru **Novi projekt ASP.NET** na popisu **Odaberite predložak** , kliknite **MVC**.

    ![][12]

6. I dalje u dijaloškom okviru **Novi projekt ASP.NET** kliknite gumb **Promijeni provjere autentičnosti** . U dijaloškom okviru **Promjena provjere autentičnosti** kliknite **Bez provjere autentičnosti**, a zatim kliknite **u redu**. Za ovaj vodič se implementacija aplikacije koji se ne mora korisničko ime za prijavu.

    ![][16]

7. U dijaloškom okviru **Novi projekt ASP.NET** kliknite **u redu** da biste stvorili projekta.

6.  U **Pregledniku rješenja**u programu project **FrontendWebRole** desnom tipkom miša kliknite **reference**, a zatim kliknite **Upravljanje NuGet paketa**.

7.  Kliknite karticu **Pregled** , a zatim potražite `Microsoft Azure Service Bus`. Kliknite **Instaliraj**i prihvatite uvjete korištenja.

    ![][13]

    Primijetite da sada referencirani sklopova potreban klijent i dodani su neke nove datoteke kod.

9.  U **Pregledniku rješenja**, desnom tipkom miša kliknite **modela** i kliknite **Dodaj**, a zatim kliknite **Predmet**. U okvir **naziv** upišite naziv **OnlineOrder.cs**. Zatim kliknite **Dodaj**.

### <a name="write-the-code-for-your-web-role"></a>Pisanje koda za vašu web ulogu

U ovom ćete odjeljku stvoriti različite stranice koja se prikazuje aplikacija.

1.  U datoteci OnlineOrder.cs u Visual Studio, zamijenite postojeću definiciju prostor naziva sljedeći kod:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  U **Pregledniku rješenja**, dvokliknite **Controllers\HomeController.cs**. Dodajte sljedeće **pomoću** naredbe pri vrhu datoteku da biste uvrstili prostori za model koji ste upravo stvorili, kao i Bus servisa.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  I u datoteci HomeController.cs u Visual Studio, zamijenite postojeću definiciju prostor naziva sljedeći kod. Kod sadrži načina za rukovanje slanje stavki u redu čekanja.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Na izborniku **Sastavljanje** kliknite **Stvaranje rješenja** da biste testirali točnost svoje poslovne dosad.

5.  Stvorite prikaz za na `Submit()` način na koji ste prethodno stvorili. Desnom tipkom miša kliknite unutar na `Submit()` način (preopterećenja od `Submit()` koji traje bez parametara), a zatim odaberite **Dodaj prikaz**.

    ![][14]

6.  Pojavit će se dijaloški okvir za stvaranje prikaza. Na popisu **predložaka** odaberite **Stvori**. Na popisu **Model predmete** kliknite **OnlineOrder** predmete.

    ![][15]

7.  Kliknite **Dodaj**.

8.  Sada, promijenite prikazani naziv aplikacije. U **Pregledniku rješenja**, dvokliknite u **Views\Shared\\_Layout.cshtml** datoteku da biste ga otvorili u uređivaču za Visual Studio.

9.  Da biste zamijenili sve pojave **Moje ASP.NET aplikacija** s **LITWARE o proizvodima**.

10. Uklanjanje veze **Početna stranica**, **o**i **kontakt** . Brisanje istaknutog kod:

    ![][28]

11. Na kraju, izmjena slanje stranice za neke informacije o reda. U **Pregledniku rješenja**, dvokliknite datoteku **Views\Home\Submit.cshtml** da biste ga otvorili u uređivaču za Visual Studio. Dodavanje sljedeći redak nakon `<h2>Submit</h2>`. Zasad u `ViewBag.MessageCount` je prazan. Će ga popunili kasnije.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Sada ste implementirali vaše korisničko Sučelje. Pritisnite **F5** da biste pokrenuli aplikaciju i provjerite izgleda li prema očekivanjima.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Pisanje koda za slanje stavke servisa Bus red

Sada dodajte kod za slanje stavke u red. Prvo, stvorite klasa koja sadrži vaše podatke o vezi reda čekanja Bus servisa. Nakon toga pokrenuti vezu iz Global.aspx.cs. Na kraju, ažurirali kod za slanje koju ste stvorili ranije u HomeController.cs zapravo slanje stavke servisa Bus red.

1.  U **Pregledniku rješenja**, desnom tipkom miša kliknite **FrontendWebRole** (desnom tipkom miša kliknite projekt, ne ulogu). Kliknite **Dodaj**, a zatim kliknite **Predmet**.

2.  Naziv klase **QueueConnector.cs**. Kliknite **Dodaj** da biste stvorili klasu.

3.  Sada dodajte kod koji encapsulates podatke o vezi i inicijalizira veze servisa Bus red. Sav sadržaj QueueConnector.cs zamijenite sljedeći kod, a zatim unesite vrijednosti za `your Service Bus namespace` (vaše ime prostor naziva) i `yourKey`, koji je **primarni ključ** prethodno dobivenog Azure portal.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Sada, provjerite je li naziva će se način **pokrenuti** . U **Pregledniku rješenja**, dvokliknite **Global.asax\Global.asax.cs**.

5.  Dodajte sljedeći redak koda na kraju metodu **Application_Start** .

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Na kraju, ažurirajte web kod koji ste prethodno stvorili, slanje stavke u red. U **Pregledniku rješenja**, dvokliknite **Controllers\HomeController.cs**.

7.  Ažuriranje na `Submit()` način (preopterećenja koja vas vodi bez parametara) na sljedeći način da biste dobili broj poruka za reda.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Ažuriranje na `Submit(OnlineOrder order)` način (preopterećenja koji traje jedan parametar) na sljedeći način da biste poslali informacije o narudžbi u red.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Sada možete pokrenuti aplikaciju računala. Svaki put kada pošaljete narudžbu, povećava broj poruka.

    ![][18]

## <a name="create-the-worker-role"></a>Stvaranje ulogu suradnika

Sada će stvoriti ulogu suradnika koji obrađuje poslanih stavki redoslijed. U ovom se primjeru koristi predložak projekta za Visual Studio **Ulogu suradnika s red čekanja Bus servisu** . Već dobili odgovarajuće vjerodajnice na portalu.

1. Provjerite je li Visual Studio ste povezali račun za Azure.

2.  U Visual Studio u **Programu Explorer rješenje** desnom tipkom miša kliknite mapu **uloge** u **MultiTierApp** projekt.

3.  Kliknite **Dodaj**, a zatim kliknite **Novi projekt ulogu suradnika**. Pojavit će se dijaloški okvir **Dodavanje novog projekta uloge** .

    ![][26]

4.  U dijaloškom okviru **Dodavanje novog projekta uloga** kliknite **Ulogu suradnika s red čekanja Bus servisu**.

    ![][23]

5.  U okvir **naziv** naziv projekta **OrderProcessingRole**. Zatim kliknite **Dodaj**.

6.  Kopirajte niz za povezivanje koji ste nabavili u koraku 9 odjeljka "Stvaranje servisa Bus prostor naziva" u međuspremnik.

7.  U **Pregledniku rješenja**, desnom tipkom miša kliknite **OrderProcessingRole** koji ste stvorili u koraku 5 (pazite da kliknete desnom tipkom miša **OrderProcessingRole** u odjeljku **uloge**, a ne klase). Zatim kliknite **Svojstva**.

8.  Na kartici **Postavke** u dijaloškom okviru **Svojstva** kliknite u okvir **vrijednost** za **Microsoft.ServiceBus.ConnectionString**, a zatim zalijepite vrijednost krajnju točku koju ste kopirali u koraku 6.

    ![][25]

9.  Stvaranje programa klase **OnlineOrder** za predstavljanje narudžbe tijekom postupka iz reda. Možete ponovno iskoristiti predmete koje ste već stvorili. U **Pregledniku rješenja**, desnom tipkom miša kliknite klase **OrderProcessingRole** (desnom tipkom miša kliknite ikonu predmet, a ne ulogu). Kliknite **Dodaj**, a zatim kliknite **Postojeću stavku**.

10. Otvorite podmapu koja se za **FrontendWebRole\Models**, a zatim dvokliknite **OnlineOrder.cs** da biste ga dodali na taj projekt.

11. U **WorkerRole.cs**, promijenite vrijednost varijable **QueueName** iz `"ProcessingQueue"` da biste `"OrdersQueue"` kao što je prikazano u sljedećem kodu.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Dodajte sljedeće korištenjem iskaza pri vrhu datoteke WorkerRole.cs.

    ```
    using FrontendWebRole.Models;
    ```

13. U na `Run()` funkcioniraju, unutar na `OnMessage()` poziva, zamijenite sadržaj u `try` uvjet s sljedeći kod.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Dovršite aplikacije. Punu aplikaciju možete testirati i tako da desnom tipkom miša kliknete MultiTierApp projekta u programu Explorer rješenja, odaberite **Postavi kao pokretanje projekta**, a zatim pritisnite F5. Imajte na umu da se broj poruka ne povećali, jer ulogu suradnika obrađuje stavke iz reda čekanja te ih označava kao dovršenu. Vidjet ćete rezultat praćenja vaša uloga suradnika pregledom Azure izračunati Emulator korisničkog Sučelja. To možete učiniti tako da desnom tipkom miša kliknete ikonu emulator u području obavijesti na programskoj traci, a zatim odaberete **Prikaži izračunate Emulator korisničkog Sučelja**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Daljnji koraci  

Da biste saznali više o Bus servisa, potražite u sljedećim resursima:  

* [Azure Service Bus][sbmsdn]  
* [Servis Bus stranice servisa][sbwacom]  
* [Upute za korištenje servisa Bus redovi][sbwacomqhowto]  

Da biste saznali više o scenariji s više razina, pogledajte:  

* [.NET više razina aplikacije pomoću tablice za pohranu, redovima i blob-ova][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Alati i SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  