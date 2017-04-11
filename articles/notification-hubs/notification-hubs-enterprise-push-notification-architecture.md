<properties
    pageTitle="Obavijest o koncentratora - Enterprise automatske arhitekture"
    description="Upute o korištenju Azure obavijesti koncentratora u okruženju tvrtke"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Upute za arhitektonski automatske Enterprise

Korporacije danas kreću se postupno pri stvaranju mobilne aplikacije za bilo koju njihove krajnjim korisnicima (vanjsko) ili za zaposlenike (interno). Imaju postojeće pozadinskog sustava na mjestu biti mainframes ili neke aplikacije LoB koje morate integriranom arhitektura za mobilnu aplikaciju. Ovaj vodič će objasniti kako trebali biste učiniti Ta integracija preporučiti dostupno rješenje za uobičajeni scenariji.

Česti preduvjet je za slanje automatske obavijesti korisnicima putem svoje mobilne aplikacije kada se pojavi događaja koje vas zanimaju u sustavima pozadinskog. Npr. klijent banke tko ima banke bankovne aplikacija na svoj iPhone želi primati obavijesti kad upišete debitnu iznad određeno svoj račun ili intranetu scenarij u kojem zaposleniku iz odjela financije tko ima aplikacije za odobrenje proračun na svom uređaju Windows Phone želi primati obavijesti kada on prima zahtjev za odobrenje.

Bankovnog računa ili obrada odobrenje je vjerojatno moguće izvršiti neke pozadinski sustav koji morate započeti automatske korisniku. Mogu postojati više takvih pozadinskog sustava koji morate sve sastavljanje istu vrstu logike za implementaciju automatske kada događaja obavijest. Ovdje složenosti leži u integriranje nekoliko pozadinskog sustava zajedno s jednom automatske sustava koju krajnji korisnici možda ste se pretplatili na različitim obavijesti i čak i možda postoji više aplikacija za mobilne uređaje – primjerice slučaju intranetu mobilne aplikacije mjesto jedan mobilne aplikacije možda želite primati obavijesti s više takvih pozadinskog sustava. Sustavi pozadinskog informacije ili morate znati semantiku tehnologije automatske tako uobičajena rješenja Najčešći je za predstavljanje komponente koje polls pozadinskog sustava za Svi događaji koje vas zanimaju i odgovoran za slanje poruka automatske klijentu.
Ovdje ćete ćemo objasniti što još bolje rješenje za korištenje Bus servisa Azure - tema/pretplate modelu kojeg će Smanjite složenost dok stvarate rješenje skalabilni.

Evo Općenito arhitekturu rješenja (generalizirano s više mobilne aplikacije, ali ravnomjerno primjenjivo kada postoji samo jedan mobilne aplikacije)

## <a name="architecture"></a>Arhitektura

![][1]

Ključni informaciju ovog arhitektonski dijagrama je Bus servisa Azure u kojemu se nalazi na teme/pretplate programiranje model (više na njemu u [programskom servisa Bus Pub/Sub]). Primatelj u tom slučaju je mobilni pozadinskog (obično [Azure mobilne usluge], koji će pokrenuti automatske mobilne aplikacije) primati poruke izravno iz sustava pozadinskog, ali umjesto imamo programa sloj Srednja apstrakcije nudi [Bus servisa Azure] koja omogućuje mobilni pozadinskog primati poruke iz jednog ili više pozadinskog sustava. Tema Bus servisa mora se stvara za svaki od pozadinskog sustava – primjerice račun HR, financije koje su zapravo "Tema" interesa koje će započeti poruke slati kao automatske obavijesti. Sustavi pozadinskog slanje poruka ove teme. Mobile pozadinskog možete se pretplatiti na jednu ili više takvih teme stvaranjem servisa Bus pretplate. To će entitle mobilne pozadinskog za primanje obavijesti iz odgovarajuće pozadinskog sustava. Mobilni pozadinskog Nastavi da biste preslušali poruka na svojim pretplate i čim dolazna poruka je uključuje natrag i šalje kao obavijest njegov koncentrator obavijesti. Obavijest o koncentratora pa naposljetku isporučuje aplikacije za mobilne uređaje. Tako da biste sažimati ključne komponente, imamo:

1. Sustavi za pozadinskog (sustavi za LoB/naslijeđeno)
    - Stvara servisa Bus teme
    - Šalje poruku
2. Mobilni pozadinskog
    - Stvara pretplatom
    - Prima poruke (iz pozadinski sustav)
    - Šalje obavijest o klijentima (putem koncentratora obavijesti za Azure)
3. Mobilna aplikacija
    - Prima i prikaz obavijesti

###<a name="benefits"></a>Prednosti:

1. Decoupling između tekstnog okvira (mobilne aplikacije/servis putem koncentratora obavijesti) i pošiljatelja (pozadinskog sustava) omogućuje dodatne pozadinskog sustava koja se integriran s minimalnim promjena.
2. Također, Ovime scenarij više mobilne aplikacije moći primati događaje iz jednog ili više pozadinskog sustava.  

## <a name="sample"></a>Primjer:

###<a name="prerequisites"></a>Preduvjeti
Trebali biste dovršite sljedeći vodiči za Upoznajte s koncepata kao i uobičajenih stvaranja i navedeni koraci za konfiguraciju:

1. [Servis Bus Pub/Sub programiranje] - objašnjava se pojedinosti o radu s temama servisa Bus/pretplate, kako stvoriti polje naziva tako da sadrže teme/pretplate, upute za slanje i primanje poruka.
2. [Obavijest o koncentratora – Windows univerzalni Praktični vodič] – Time se objašnjava postavljanje aplikacija iz Windows trgovine i korištenje koncentratora obavijesti da biste registrirali i primati obavijesti.

###<a name="sample-code"></a>Ogledni kod

Kod punu ogledni dostupna je na [Obavijesti koncentrator uzorka]. Podjela je u sljedeća tri odjeljka:

1. **EnterprisePushBackendSystem**

    na. Taj projekt koristi Nuget paket *WindowsAzure.ServiceBus* i temelji se na [servis Bus Pub/Sub programiranje].

    b. To je jednostavno C# konzole za aplikacije u u programu Publisher LoB sustav koji se pokreće poruku primatelju se isporučivati u mobilnoj aplikaciji.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`koristi se za stvaranje servisa Bus temu koju ćete poslati poruke.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`koristi se za slanje poruka u ovoj se temi Bus servisa. Ovdje možemo jednostavno šaljete skup izravnim porukama na temu povremeno radi uzorka. U normalnim pojavit će se pozadinski sustav koji će poslati poruke kada se pojavi događaj.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    na. Taj projekt koristi *WindowsAzure.ServiceBus* i *Microsoft.Web.WebJobs.Publish* Nuget paketa i temelji se na [servis Bus Pub/Sub programiranje].

    b. To je drugi C# konzole aplikacija što smo će pokrenuti kao [Azure WebJob] jer je neprestano da biste preslušali za poruke s LoB/pozadinskog sustava. To će biti dio vaš mobilni pozadinskog.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`koristi se za stvaranje servisa Bus pretplatu za temu gdje pozadinski sustav će poslati poruke. Ovisno o scenariju tvrtke komponente će stvoriti jednu ili više pretplata na odgovarajuće teme (npr neke mogu primati poruke iz sustava HR neke od sustava financije i tako dalje)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification koristi se za čitanje poruke iz tema koristite pretplatu na njegov i sastavite ako je uspješno čitanje taj slati mobilne aplikacije pomoću Azure obavijesti koncentratora zatim obavijest (u slučaju uzorka nativni skočnoj obavijesti za Windows).

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Za objavljivanje to kao **WebJob**, desnom tipkom miša kliknite rješenja u Visual Studio i odaberite **Objavi kao WebJob**

    ![][2]

    f. Odaberite profil za objavljivanje i stvaranje novog web-mjesta Azure ako ne postoji već koji će biti smješteno ovaj WebJob i nakon što dodate na web-mjesto, a zatim **Objavi**.

    ![][3]

    g. Konfiguriranje zadatka treba "Pokreni neprestano" tako da se prilikom prijave na [Portal za klasični Azure] trebali biste vidjeti otprilike ovako:

    ![][4]


3. **EnterprisePushMobileApp**

    na. Ovo je aplikacija iz Windows trgovine koji će skočnoj obavijesti primanje WebJob pokrenut kao dio vaš mobilni pozadinskog te ga prikazali. Temelji se na [Obavijesti koncentratora – Windows univerzalni vodič].  

    b. Provjerite je li vaša aplikacija omogućen primati skočnoj obavijesti.

    c. Provjerite sljedeće koncentratora obavijesti na aplikaciju koja se zove Registracija kod pokretanja (nakon zamjena *HubName* i *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Pokretanje uzorak:

1. Provjerite je li vaš WebJob pokrenut je uspješno, a zakazano "Pokreni neprestano".
2. Pokrenite **EnterprisePushMobileApp** koji će se pokrenuti aplikaciju iz Windows trgovine.
3. Pokrenite aplikaciju konzole **EnterprisePushBackendSystem** koji će kao zamjenu za pozadinski LoB te će Početak slanja poruka i trebali biste vidjeti skočnoj obavijesti koji se prikazuju kao što je sljedeća:

    ![][5]

4. Poruke poslane izvorno servisa Bus temama koje je u tijeku nadzire servisa Bus pretplate u vaš posao Web. Kada je poruka primljena obavijest stvorena i slati aplikacije za mobilne uređaje. Možete pregledati zapisnike WebJob da biste potvrdili obrada kada se povežete s vezom zapisnike u [Azure klasični Portal] za svoj posao Web:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Uzorci koncentrator obavijesti]: https://github.com/Azure/azure-notificationhubs-samples
[Azure servis za mobilne uređaje]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Servis Bus Pub/Sub programiranje]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Obavijest o koncentratora – Windows univerzalni vodiča]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure klasični Portal]: https://manage.windowsazure.com/
