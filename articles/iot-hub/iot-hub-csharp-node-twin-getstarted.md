<properties
    pageTitle="Početak rada s twins | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču saznati kako koristiti twins"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Praktični vodič: Početak rada s uređajem twins (pretpregled)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Na kraju ovog praktičnog vodiča, imat ćete je .NET i aplikacije konzole za Node.js:

* **AddTagsAndQuery.sln**, .NET aplikacije htjeli može pokrenuti pozadinskih, koji se dodaje oznake i upitima twins uređaja.
* **TwinSimulatedDevice.js**, Node.js aplikacije koji simulira uređaj koji povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili i njegov uvjeta za povezivanje s izvješćima.

> [AZURE.NOTE] U članku [IoT koncentrator SDK-ovi] [ lnk-hub-sdks] pruža informacije o raznim SDK-ovi koje možete koristiti da biste sastavili uređaja i pozadinske aplikacije.

Da biste dovršili ovaj Praktični vodič potrebno je sljedeće:

+ Microsoft Visual Studio 2015.

+ Verzija Node.js 0.10.x ili noviji.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Stvaranje aplikacije servisa

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koji dodaje meta-podataka o lokaciji twin pridružene **myDeviceId**. Ga, a zatim upita twins pohranjene u središtu odabirom uređaje nalazi u Sjedinjenih DRŽAVA, a zatim one te izvješćivanje mobilnu vezu.

1. U Visual Studio dodati projekta za Visual C# klasični radnu površinu sustava Windows trenutno rješenje pomoću predloška **Aplikacije konzole za** projekt. Naziv projekta **AddTagsAndQuery**.

    ![Novi projekt Visual C# klasični radnu površinu sustava Windows][img-createapp]

2. U pregledniku rješenja, desnom tipkom miša kliknite **AddTagsAndQuery** projekta, a zatim **Upravljanje Nuget paketa**.

3. U prozoru **Upravitelja paketima Nuget** , provjerite je li potvrđen je **Uključi predizdanja** , potražite **microsoft.azure.devices**, odaberite **Instalacija** da biste instalirali na *predizdanje **Microsoft.Azure.Devices** * pakiranje i prihvatite uvjete korištenja. Ovaj postupak preuzimanja, instalira i dodaje reference na [Microsoft Azure IoT servisa SDK] [ lnk-nuget-service-sdk] Nuget paketa i njezine ovisnosti.

    ![Prozor Upravitelj Nuget paketa][img-servicenuget]

4. Dodajte sljedeće `using` naredbe pri vrhu datoteke **Program.cs** :

        using Microsoft.Azure.Devices;

5. Dodajte sljedeća polja u **Program** predmet. Zamijenite vrijednost rezerviranog mjesta niz za povezivanje za središtu IoT koju ste stvorili u prethodnom odjeljku.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Dodajte na sljedeći način predmete **Program** :

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    Klase **RegistryManager** izlaže sve metode koje su potrebne za interakciju s uređaja twins iz servisa. Prethodni kod najprije inicijalizira **registryManager** objekt, a zatim dohvaća twin za **myDeviceId**i na kraju ažurira njegov oznake informacijama željeno mjesto.

    Nakon ažuriranja, izvršava dva upita: prvi odabire samo uređaj twins uređaje koji se nalazi u **Redmond43** biljke i drugi refines upit da biste odabrali samo uređaji povezani putem mobilne mreže.

    Imajte na umu da prethodna kod, kada se stvara objekt **upita** Određuje maksimalan broj vraćenih dokumenata. Objekt **upita** sadrži **HasMoreResults** logičko svojstvo koje možete koristiti za pozivanje metode **GetNextAsTwinAsync** više puta da biste dohvatili sve rezultate. Metode naziva **GetNextAsJson** dostupna je za rezultata koji nisu twins uređaj, na primjer, rezultati upita za zbrajanje.

7. Na kraju, dodajte sljedeće retke **Glavna** načina:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Izvršavanje te aplikacije, a trebali biste vidjeti jedan uređaj u rezultatima upita Traži sve uređaje koji se nalazi u **Redmond43** i ništa za upit u kojem se ograničava rezultate uređaje koji koriste mobilnu mrežu.

    ![Rezultati upita u prozoru][img-addtagapp]

U sljedećem odjeljku stvorite aplikaciju uređaja koji se šalju informacije za povezivanje s poslovnim i mijenja rezultat upita u prethodnom odjeljku.

## <a name="create-the-device-app"></a>Stvaranje aplikacije za uređaj

U ovom ćete odjeljku stvoriti Node.js konzole za aplikaciju koja povezuje se s vašeg koncentrator kao **myDeviceId**i ažurira njegov twin prijavljenih svojstva sadrže podatke koji je povezan putem mobilne mreže.

1. Stvorite novu praznu mapu pod nazivom **reportconnectivity**. U mapi **reportconnectivity** stvorite novu datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaše naredbenog retka u mapi **reportconnectivity** , pokrenite sljedeću naredbu da biste instalirali **azure iot uređaji**i **azure-iot-uređaja-mqtt** paketa:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. U mapi **reportconnectivity** uređivaču teksta, stvorite novu datoteku **ReportConnectivity.js** .

4. Dodajte sljedeći kod **ReportConnectivity.js** datoteku i zamijeniti rezervirano mjesto za **{niz za povezivanje uređaja}** s koju ste kopirali stvaranja uređaju identitetu **myDeviceId** niz za povezivanje:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    **Klijentski** objekt izlaže sve metode koje su vam potrebne za interakciju s twins uređaj s uređaja. Prethodni kod kada ga inicijalizira **klijentskog** objekta dohvaća twin za **myDeviceId** i prilagođava njegovo prijavljenim svojstvo povezivanje s poslovnim podacima.

5. Pokrenite aplikaciju uređaja

        node ReportConnectivity.js

    Trebali biste vidjeti poruku `twin state reported`.

6. Sad kad uređaj prijavio svoje podatke za povezivanje bi trebao izgledati oba upita. Pokrenite aplikaciju.NET **AddTagsAndQuery** da biste ponovno pokrenuli upiti. Ovaj put **myDeviceId** prikazivati i rezultata upita.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Daljnji koraci
U ovom ćete praktičnom vodiču konfiguriran novi koncentrator IoT na portalu za Azure, a zatim stvoriti uređaju identitetu u odjeljku središte identiteta registra. Dodaje uređaj meta-podataka kao oznake iz aplikacije pozadinskih i napisali Simulirani uređaj aplikacije o izvješća uređaj s povezivanjem u twin uređaja. I saznali kako postaviti upit podatke pomoću središtu IoT jezik nalik SQL upita.

Pomoću sljedećih resursa da biste saznali kako:

- Slanje telemetrijskih s uređaja s [Početak rada s IoT koncentrator] [ lnk-iothub-getstarted] ćete praktičnom vodiču
- Konfiguriranje uređajima pomoću twin na željeni svojstva s [pomoću željenog svojstava koja možete konfigurirati uređaje] [ lnk-twin-how-to-configure] ćete praktičnom vodiču
- Upravljanje uređajima interaktivno (kao što su uključivanjem Lepeza iz aplikacije za korisnički kontrolirano) s [Izravni načina korištenja] [ lnk-methods-tutorial] vodič.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

