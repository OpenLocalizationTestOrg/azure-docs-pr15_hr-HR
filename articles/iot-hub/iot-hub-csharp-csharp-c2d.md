<properties
    pageTitle="Slanje poruka oblak na uređaj s IoT koncentrator | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste saznali kako slanje poruka oblaka uređaj pomoću Azure IoT koncentrator C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Praktični vodič: Način slanja poruka oblak na uređaj s IoT koncentratora i .net

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Uvod

Azure IoT koncentrator je servis za potpuno upravljane koji olakšava omogućiti pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i završavaju aplikaciju natrag. Praktični vodič za [Početak rada s IoT koncentrator] pokazuje kako stvoriti koncentratora za IoT, Dodjela uređaju identitetu u njoj i kod Simulirani uređaj koji šalje poruke uređaj u oblak.

Pomoću ovog praktičnog vodiča sastavlja na [Početak rada s IoT koncentratora]. Kako pokazuje da biste:

- S vašeg računala oblaka pozadinskih, slanje poruka oblaka uređaj jedan uređaj putem koncentratora IoT.
- Oblak uređaj poruka na uređaju.
- S vašeg računala oblaka sigurnosno završi, zahtjev potvrđivanje isporuke (*povratne informacije*) za poruke poslane na uređaj s IoT koncentratora.

Dodatne informacije o oblaka uređaj poruke možete pronaći u [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

Na kraju ovog praktičnog vodiča pokretati dvije aplikacije konzole za Windows:

* **SimulatedDevice**, izmijenjenu verziju aplikacije stvorene u [Početak rada s IoT koncentrator], čime se povezuje s koncentratora za IoT i prima poruke oblak na uređaju.
* **SendCloudToDevice**, koji šalje poruku oblaka uređaj Simulirani uređaja putem koncentratora IoT i prima njegov potvrđivanje isporuke.

> [AZURE.NOTE] Koncentrator IoT ima podršku SDK za mnoge uređaj platforme i jezika (uključujući C, Java i Javascript) do Azure IoT uređaj SDK-ovi. Podrobne upute o povezivanju uređaja kod ovog praktičnog vodiča i obično Azure IoT koncentrator potražite u članku [Razvojni centar za Azure IoT].

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Microsoft Visual Studio 2015.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

## <a name="receive-messages-on-the-simulated-device"></a>Primanje poruka na uređaju Simulirani

U ovom odjeljku ćete izmjena Simulirani uređaj aplikaciju koju ste stvorili u oblak uređaj poruka iz koncentratora IoT [Početak rada s IoT koncentratora] .

1. U Visual Studio u programu project **SimulatedDevice** dodati sa sljedećim korakom predmete **Program** .

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    Na `ReceiveAsync` način asinkrono vraća primljenu poruku u vrijeme koje je primio uređaj. Vraća *vrijednost null* nakon specifiable isteklo (u tom slučaju zadane jedne minute služi). Kada se to dogodi, kod nastaviti Čekanje na nove poruke. To je razlog na `if (receivedMessage == null) continue` redak.

    Poziv na `CompleteAsync()` obavještava IoT koncentrator da poruku uspješno obrađen. Poruke mogu biti sigurno uklanjaju uređaja. Ako nešto što se dogodilo koji spriječio dovršenje obrade poruke aplikaciju uređaj, IoT koncentrator nudi ga ponovno. Zatim važno je da se logiku obrade poruka u aplikaciji uređaj *idempotent*tako da prima ista poruka više puta ima isti učinak. Aplikaciju možete i privremeno ćete poruku, zbog čega IoT koncentrator zadržavanje poruku u redu čekanja za buduće potrošnje. Ili aplikacija možete odbiti poruku u kojoj se trajno uklanja poruku iz reda. Dodatne informacije o životnom ciklusu oblaka uređaj poruka potražite u članku [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Prilikom korištenja HTTP umjesto MQTT ili AMQP kao prijenosa, u `ReceiveAsync` način vraća odmah. Podržani uzorka za poruke oblak na uređaj s HTTP je povremeno povezanih uređaja provjeru za poruke diskovni (manje od svakih 25 minuta). U IoT središte ograničavanje zahtjeve izdavanja više HTTP prima rezultate. Dodatne informacije o razlikama između MQTT, AMQP i HTTP podršci i ograničavanje IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

2. Dodajte sljedeće metoda u način **glavne** desno prije nego na `Console.ReadLine()` redak:

        ReceiveC2dAsync();

> [AZURE.NOTE] Za sake jednostavnosti, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog trebali biste provesti pravila Ponovi (kao što su eksponencijalne backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara].

## <a name="send-a-cloud-to-device-message"></a>Slanje poruke oblaka uređaja

U ovom odjeljku ćete pisati aplikacije konzole za Windows koja šalje poruke oblaka uređaj aplikaciju Simulirani uređaja.

1. U trenutnom rješenju Visual Studio stvorite novi projekt Visual C# radnu površinu aplikacije pomoću predloška **Aplikacije konzole za** projekt. Naziv projekta **SendCloudToDevice**.

    ![Novi projekt u Visual Studio][20]

2. U pregledniku rješenja, desnom tipkom miša kliknite rješenje, a zatim **Upravljanje NuGet paketa rješenja...**. 

    Otvorit će se prozor za **Upravljanje NuGet paketa** .

3. Traženje `Microsoft Azure Devices`, kliknite **Instaliraj**i prihvatite uvjete korištenja. 

    To preuzimanja, instalira i dodaje referenca [Azure IoT - paket NuGet SDK servisa].

4. Dodajte sljedeće `using` izjava pri vrhu datoteke **Program.cs** :

        using Microsoft.Azure.Devices;

5. Dodajte sljedeća polja u **Program** predmet. Zamijenite vrijednost rezervirano mjesto s nizom za povezivanje koncentrator IoT iz [Početak rada s IoT koncentratora]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Dodajte na sljedeći način predmete **Program** :

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Ovu metodu šalje novu poruku oblaka uređaj na uređaju s ID-om, `myFirstDevice`. Promijenite taj parametar sukladno tome, u slučaju da izmijenio od onog koji se koriste u [Početak rada s IoT koncentratora].

7. Na kraju, dodajte sljedeće retke **Glavna** načina:

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Programa Visual Studio, desnom tipkom miša kliknite rješenje, a odaberite **Postavljanje pokretanja projekata...**. Odaberite **više projekata pokretanje**, a zatim odaberite akciju **pokretanje** **ProcessDeviceToCloudMessages**, **SimulatedDevice**i **SendCloudToDevice**.

9.  Pritisnite **F5**. Sve tri aplikacije trebala. Potvrdite okvir prozori **SendCloudToDevice** , a zatim pritisnite **Enter**. Trebali biste vidjeti poruku koja se primio Simulirani aplikaciju.

    ![Aplikacija primanju poruka][21]

## <a name="receive-delivery-feedback"></a>Prikupljanje povratnih informacija za isporuku
Moguće je na zahtjev za isporuku (ili isteka) acknowledgements iz koncentratora IoT za svaku poruku oblak na uređaj. Time se omogućuje oblaka pozadinska jednostavno obavještavate pokušaj ili plaćati logike. Dodatne informacije o oblaka uređaj povratne informacije potražite u članku [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

U ovom ćete odjeljku će promijeniti **SendCloudToDevice** aplikacije za povratne informacije o zahtjevu i primanje IoT koncentratora.

1. U Visual Studio u programu project **SendCloudToDevice** dodati sa sljedećim korakom predmete **Program** .
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Imajte na umu da uzorak primanje isti onog koji je korišten primati poruke oblaka uređaja iz aplikacije za uređaj.

2. Dodavanje sljedećih metoda u način **glavne** odmah nakon što u `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` redak:

        ReceiveFeedbackAsync();

3. Da biste zatražili povratne informacije za isporuku poruke oblaka uređaj, morate navesti svojstva ili u način **SendCloudToDeviceMessageAsync** . Dodavanje sljedeći redak odmah nakon što u `var commandMessage = new Message(...);` redak:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Pokretanje aplikacije pritiskom na tipku **F5**. Trebali biste vidjeti sve tri aplikacije start. Potvrdite okvir prozori **SendCloudToDevice** , a zatim pritisnite **Enter**. Trebali biste vidjeti poruku prima Simulirani aplikacija i nakon nekoliko sekundi, prima aplikacija **SendCloudToDevice** poruku povratne informacije.

    ![Aplikacija primanju poruka][22]

> [AZURE.NOTE] Za sake jednostavnosti, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog trebali biste provesti pravila Ponovi (kao što su eksponencijalne backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara].

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako slanje i primanje poruka oblak na uređaj. 

Da biste vidjeli primjera dovršeno završetka do kraja rješenja koje koriste IoT koncentrator, potražite u članku [Azure IoT paket].

Dodatne informacije o razvoju rješenja s IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - paket servisa SDK NuGet]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[Vodič za IoT koncentrator za razvojne inženjere]: iot-hub-devguide.md
[Početak rada s IoT koncentratora]: iot-hub-csharp-csharp-getstarted.md
[Razvojni centar za Azure IoT]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT paketa]: https://azure.microsoft.com/documentation/suites/iot-suite/