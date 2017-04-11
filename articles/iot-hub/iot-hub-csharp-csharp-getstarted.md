<properties
    pageTitle="Azure IoT koncentrator za C# Uvod | Microsoft Azure"
    description="Azure IoT koncentrator pomoću C# početak rada vodič. Pomoću Azure IoT koncentratora i C# sustava Microsoft Azure IoT SDK-ovi za implementaciju rješenja programa Internet stvari."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Početak rada s Azure IoT koncentrator za .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na kraju ovog praktičnog vodiča, postoje tri aplikacije konzole za Windows:

* **CreateDeviceIdentity**, čime se uređaju identitetu i pridruženi sigurnosni ključ povezati Simulirani uređaj.
* **ReadDeviceToCloudMessages**, koja se prikazuje telemetrijskih poslao Simulirani uređaj.
* **SimulatedDevice**, koja povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili i šalje poruku telemetrijskih sekundi pomoću protokola AMQP.

> [AZURE.NOTE] Informacije o raznim SDK-ovi koje možete koristiti da biste sastavili obje aplikacije da biste pokrenuli na uređaje i pozadinskih vašeg rješenja potražite u članku [IoT koncentrator SDK-ovi][lnk-hub-sdks].

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Microsoft Visual Studio 2015.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sada ste stvorili koncentratora za IoT, a imate niz naziv glavnog računala i veza koje su vam potrebne da biste dovršili kraja ovog praktičnog vodiča.

## <a name="create-a-device-identity"></a>Stvaranje uređaja identiteta

U ovom ćete odjeljku stvaranja aplikacije za Windows konzole koji stvara uređaju identitetu u registru identiteta koncentratora za IoT. Uređaj ne može povezati s IoT koncentrator osim ako ima stavku u registru identiteta uređaja. Dodatne informacije potražite u odjeljku "Uređaj identiteta registra" [IoT koncentrator za razvojne inženjere vodič][lnk-devguide-identity]. Prilikom pokretanja aplikacije konzole generira ID jedinstveni uređaja i ključ koji uređaju možete koristiti da biste odredili sam kada šalje poruke uređaj u oblak IoT koncentrator.

1. U Visual Studio dodati projekta za Visual C# klasični radnu površinu sustava Windows trenutno rješenje pomoću predloška **Aplikacije konzole za** projekt. Provjerite nalazi li se .NET Framework verzije 4.5.1 ili noviji. Naziv projekta **CreateDeviceIdentity**.

    ![Novi projekt Visual C# klasični radnu površinu sustava Windows][10]

2. U pregledniku rješenja, desnom tipkom miša kliknite **CreateDeviceIdentity** projekta, a zatim **Upravljanje Nuget paketa**.

3. U prozoru **Upravitelja paketima Nuget** odaberite **Pregledaj**, potražite **microsoft.azure.devices**, odaberite **Instalacija** da biste instalirali paket **Microsoft.Azure.Devices** i prihvatite uvjete korištenja. Ovaj postupak preuzimanja, instalira i dodaje reference na [Microsoft Azure IoT servisa SDK] [ lnk-nuget-service-sdk] Nuget paketa i njezine ovisnosti.

    ![Prozor Upravitelj Nuget paketa][11]

4. Dodajte sljedeće `using` naredbe pri vrhu datoteke **Program.cs** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Dodajte sljedeća polja u **Program** predmet. Zamijenite vrijednost rezerviranog mjesta niz za povezivanje za središtu IoT koju ste stvorili u prethodnom odjeljku.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Dodajte na sljedeći način predmete **Program** :

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Ta metoda stvara uređaju identitetu s ID-a **myFirstDevice**. (Ako taj ID uređaja već postoji u registru, kod jednostavno dohvaća postojeće informacije o uređaju.) Aplikacija prikazuje primarni ključ za taj identitet. Koristite ovaj ključ u Simulirani uređaj za povezivanje s koncentratora za IoT.

7. Na kraju, dodajte sljedeće retke **Glavna** načina:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Izvršavanje te aplikacije i zabilježite ključ uređaja.

    ![Ključ uređaja generirala aplikacija][12]

> [AZURE.NOTE] Identitet registra IoT koncentrator samo pohranjuje identiteta uređaj za omogućivanje sigurnog pristupa središtu. Sprema uređaju ID-a i tipke da biste koristili kao sigurnosne vjerodajnice i omogućeno/onemogućeno Označi zastavicom koje možete koristiti da biste onemogućili pristup za pojedinačne uređaj. Ako aplikacija nije potrebno pohranjivati metapodatke druge specifične za uređaj, trebali biste koristiti u spremištu specifičnim aplikacijama. Dodatne informacije potražite u članku [Vodič za razvojne inženjere koncentrator IoT][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Primanje poruka uređaj u oblak

U ovom ćete odjeljku stvaranja aplikacije za Windows konzole koji se čita uređaj u oblak poruke iz centra za IoT. Koncentratora za IoT izlaže [Azure događaj koncentratora][lnk-event-hubs-overview]-kompatibilne krajnju točku da biste mogli čitati poruke uređaj u oblak. Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča stvara čitača osnovni koja nije prikladna za implementaciju visoke propusnost. Da biste saznali kako obraditi uređaju u oblak poruke na razini, potražite u članku [postupak uređaj u oblak poruke] [ lnk-process-d2c-tutorial] vodič. Dodatne informacije o porukama postupak iz koncentratora događaja potražite u članku [Početak rada s koncentratorima događaj] [ lnk-eventhubs-tutorial] vodič. (Ovaj Praktični vodič je primjenjuju na krajnje točke kompatibilan s preglednikom događaja koncentrator IoT koncentratora.)

> [AZURE.NOTE] Krajnje kompatibilan s preglednikom događaja koncentrator za čitanje poruka uređaj oblak uvijek koristi protokol AMQP.

1. U Visual Studio dodati projekta za Visual C# klasični radnu površinu sustava Windows trenutno rješenje pomoću predloška **Aplikacije konzole za** projekt. Provjerite nalazi li se .NET Framework verzije 4.5.1 ili noviji. Naziv projekta **ReadDeviceToCloudMessages**.

    ![Novi projekt Visual C# klasični radnu površinu sustava Windows][10]

2. U pregledniku rješenja, desnom tipkom miša kliknite **ReadDeviceToCloudMessages** projekta, a zatim **Upravljanje Nuget paketa**.

3. U prozoru **Upravitelja paketima Nuget** traženje **WindowsAzure.ServiceBus**, odaberite **Instalacija**i prihvatite uvjete korištenja. Ovaj postupak preuzimanja, instalira i dodaje referenca [Bus servisa Azure][lnk-servicebus-nuget], s njezine ovisnosti. Paket omogućuje aplikacije za povezivanje s kompatibilan s preglednikom događaja koncentrator krajnjoj točki koncentratora za IoT.

4. Dodajte sljedeće `using` naredbe pri vrhu datoteke **Program.cs** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Dodajte sljedeća polja u **Program** predmet. Niz za povezivanje za središtu IoT koji ste stvorili u odjeljku "Stvaranje koncentratora za IoT" zamijenite vrijednost rezervirano mjesto.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Dodajte na sljedeći način predmete **Program** :

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Ova metoda koristi instancu **EventHubReceiver** primati poruke iz svih IoT koncentrator uređaj-na-oblaka primanje particije. Obratite pozornost na to kako prenijeti na `DateTime.Now` parametar prilikom stvaranja objekta **EventHubReceiver** tako da prima samo poruke poslane nakon pokretanja. Ovaj je filtar korisna u okruženje za testiranje da biste vidjeli trenutni skup poruke. U okruženju proizvodnje koda provjerite je li procijenit sve poruke. Dodatne informacije potražite u članku [način obrade koncentrator IoT uređaj oblak poruke] [ lnk-process-d2c-tutorial] vodič.

7. Na kraju, dodajte sljedeće retke **Glavna** načina:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom se odjeljku stvorite aplikaciju konzole za Windows koji simulira uređaj koji šalje poruke uređaj u oblak koncentratora za IoT.

1. U Visual Studio dodati projekta za Visual C# klasični radnu površinu sustava Windows trenutno rješenje pomoću predloška **Aplikacije konzole za** projekt. Provjerite nalazi li se .NET Framework verzije 4.5.1 ili noviji. Naziv projekta **SimulatedDevice**.

    ![Novi projekt Visual C# klasični radnu površinu sustava Windows][10]

2. U pregledniku rješenja, desnom tipkom miša kliknite **SimulatedDevice** projekta, a zatim **Upravljanje Nuget paketa**.

3. U prozoru **Upravitelja paketima Nuget** odaberite **Pregledaj**, potražite **Microsoft.Azure.Devices.Client**, odaberite **Instalacija** da biste instalirali paket **Microsoft.Azure.Devices.Client** i prihvatite uvjete korištenja. Ovaj postupak preuzimanja, instalira i dodaje referenca [Azure IoT - paket uređaj SDK Nuget] [ lnk-device-nuget] i njegov ovisnosti.

4. Dodajte sljedeće `using` izjava pri vrhu datoteke **Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Dodajte sljedeća polja u **Program** predmet. Zamjena vrijednosti rezervirano mjesto pomoću IoT koncentrator glavnog računala koju ste vratili u odjeljku "Stvaranje koncentratora za IoT", a ključ uređaja dohvatiti u odjeljku "Stvaranje uređaju identitetu".

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Dodajte na sljedeći način predmete **Program** :

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Ovaj postupak šalje novu poruku uređaj u oblak sekundi. Poruka sadrži objekt koji se JSON serijalizirani, ID uređaja i slučajno generiranim brojem u programu Publisher senzora brzina vjetra.

7. Na kraju, dodajte sljedeće retke **Glavna** načina:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Prema zadanim postavkama načina za **Stvaranje** stvara instancu **DeviceClient** koja koristi protokol AMQP komunikaciju s IoT koncentratora. Da biste koristili HTTP protokola, koristite nadjačavanje **Stvaranje** način koji omogućuje određivanje protokol. Ako koristite HTTP protokola, trebali biste dodati i paket Nuget **Microsoft.AspNet.WebApi.Client** u projekt da biste dodali polje naziva **System.Net.Http.Formatting** .

Pomoću ovog praktičnog vodiča vodi vas kroz korake da biste stvorili klijent za IoT koncentrator uređaja. Možete koristiti i [Povezani servisa Azure IoT koncentrator] [ lnk-connected-service] proširenje za Visual Studio da biste dodali potrebne kod klijentsku aplikaciju uređaja.


> [AZURE.NOTE] Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog provodite pravila Ponovi (primjerice na eksponencijalnom backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara][lnk-transient-faults].

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1.  U Visual Studio, u pregledniku rješenja, desnom tipkom miša kliknite rješenje, a zatim **postavite početni projekata**. Odaberite **više projekata prilikom pokretanja**, a zatim **pokretanje** kao akciju za projekte **ReadDeviceToCloudMessages** i **SimulatedDevice** .

    ![Pokretanje svojstva projekta][41]

2.  Pritisnite **F5** da biste pokrenuli obje aplikacije pokrenut. Konzola za izlaz iz aplikacije **SimulatedDevice** prikazuju se poruke uređaju Simulirani šalje koncentratora za IoT. Konzola za izlaz iz aplikacije **ReadDeviceToCloudMessages** prikazuju se poruke koja prima koncentratora za IoT.

    ![Konzola za izlaz iz aplikacija][42]

3. **Korištenje** pločice [Azure portal] [ lnk-portal] prikazuje se broj poruka poslana koncentrator:

    ![Azure portala korištenje pločica][43]


## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču konfiguriran koncentratora za IoT na portalu za Azure, a zatim stvoriti uređaju identitetu u odjeljku središte identiteta registra. Koristi ovaj identitet uređaj da biste omogućili aplikaciju Simulirani uređaj da biste poslali poruke uređaj u oblak središtu. Koji ste stvorili i aplikaciju koja se prikazuje u poruke koje su stigle prema središtu. 

Da biste nastavili Uvod u koncentratoru IoT i drugim situacijama IoT za istraživanje potražite u članku:

- [Povezivanje uređaja][lnk-connect-device]
- [Uvod u upravljanje uređajima][lnk-device-management]
- [Uvod u pristupnik SDK IoT][lnk-gateway-SDK]

Da biste saznali kako proširiti rješenje IoT i obraditi uređaju u oblak poruke na razini, prikazati [poruka uređaj u oblak postupak] [ lnk-process-d2c-tutorial] vodič.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
