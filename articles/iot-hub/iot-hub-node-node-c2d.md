<properties
    pageTitle="Slanje poruka oblak na uređaj s IoT koncentrator | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste saznali kako slanje poruka oblaka uređaj koncentrator IoT Azure pomoću Java."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Praktični vodič: Način slanja poruka oblak na uređaj s koncentrator IoT i Node.js

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Uvod

Azure IoT koncentrator je servis za potpuno upravljane koji olakšava omogućiti pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i završavaju aplikaciju natrag. Praktični vodič za [Početak rada s IoT koncentrator] pokazuje kako stvoriti koncentratora za IoT, Dodjela uređaju identitetu u njoj i kod Simulirani uređaj koji šalje poruke uređaj u oblak.

Pomoću ovog praktičnog vodiča sastavlja na [Početak rada s IoT koncentratora]. Kako pokazuje da biste:

- S vašeg računala oblaka pozadinskih, slanje poruka oblaka uređaj jedan uređaj putem koncentratora IoT.
- Oblak uređaj poruka na uređaju.
- S vašeg računala oblaka sigurnosno završi, zahtjev potvrđivanje isporuke (*povratne informacije*) za poruke poslane na uređaj s IoT koncentratora.

Dodatne informacije o oblaka uređaj poruke možete pronaći u [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

Na kraju ovog praktičnog vodiča, pokrenite dvije Node.js konzole aplikacije:

* **SimulatedDevice**, izmijenjenu verziju aplikacije stvorene u [Početak rada s IoT koncentrator], čime se povezuje s koncentratora za IoT i prima poruke oblak na uređaju.
* **SendCloudToDeviceMessage**, koji šalje poruku oblaka uređaj Simulirani uređaja putem koncentratora IoT i prima njegov potvrđivanje isporuke.

> [AZURE.NOTE] Koncentrator IoT ima podršku SDK za mnoge uređaj platforme i jezika (uključujući C, Java i Javascript) do Azure IoT uređaj SDK-ovi. Podrobne upute o povezivanju uređaja kod ovog praktičnog vodiča i obično Azure IoT koncentrator potražite u članku [Razvojni centar za Azure IoT].

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Verzija Node.js 0.10.x ili noviji.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

## <a name="receive-messages-on-the-simulated-device"></a>Primanje poruka na uređaju Simulirani

U ovom ćete odjeljku izmjena Simulirani uređaj aplikaciju koju ste stvorili u oblak uređaj poruka iz koncentratora IoT [Početak rada s IoT koncentratora] .

1. Korištenje uređivača teksta, otvorite datoteku u SimulatedDevice.js.

2. Izmjena **connectCallback** funkcija za rukovanje poruke poslane s IoT koncentratora. U ovom primjeru uređaj uvijek poziva funkciju **dovršena** će obavijestiti IoT koncentrator ona sadrži obrađuje poruku. Novu verziju funkciju **connectCallback** izgleda ovako:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

    > [AZURE.NOTE] Ako koristite HTTP umjesto MQTT ili AMQP kao prijenos, instancu **DeviceClient** provjerava za poruke s IoT koncentrator diskovni (manje od svakih 25 minuta). Dodatne informacije o razlikama između MQTT, AMQP i HTTP podršci i ograničavanje IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Slanje poruke oblaka uređaja

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koja šalje poruke oblaka uređaj aplikaciju Simulirani uređaja. Potreban vam je uređaj Id uređaja koji ste dodali u ovom praktičnom vodiču za [Početak rada s IoT koncentratora] . Morate niz za povezivanje za vaše IoT središte koje možete pronaći na [portal za Azure].

1. Stvorite praznu mapu pod nazivom **sendcloudtodevicemessage**. U mapi **sendcloudtodevicemessage** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **sendcloudtodevicemessage** , pokrenite sljedeću naredbu da biste instalirali paket **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. U mapi **sendcloudtodevicemessage** uređivaču teksta, stvorite novu datoteku **SendCloudToDeviceMessage.js** .

4. Dodajte sljedeće `require` naredbe na početku **SendCloudToDeviceMessage.js** datoteke:

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Dodajte sljedeći kod **SendCloudToDeviceMessage.js** datoteku. Zamijenite vrijednost za rezervirano mjesto niza veze niz za povezivanje za središtu IoT koji ste stvorili u ovom praktičnom vodiču za [Početak rada s IoT koncentratora] . Rezervirano mjesto za uređaj cilj zamijenili id uređaja uređaja koji ste dodali u ovom praktičnom vodiču za [Početak rada s IoT koncentratora] :

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Dodajte sljedeće funkcije da biste ispisali rezultate operacije na konzolu:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Dodajte sljedeće funkcije da biste ispisali isporuke poruke povratnih informacija na konzoli:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Dodajte sljedeći kôd slanje poruke s uređajem i rukovanje povratne informacije poruku kada uređaj priznaje poruke oblak na uređaju:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Spremite i zatvorite datoteku **SendCloudToDeviceMessage.js** .

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbeni-u mapi **simulateddevice** , pokrenite sljedeću naredbu da biste poslali telemetrijskih IoT koncentrator i da biste preslušali za poruke u oblak na uređaju:

    ```
    node SimulatedDevice.js 
    ```

    ![Pokrenite aplikaciju Simulirani uređaja][img-simulated-device]

2. U naredbenom retku u mapi **sendcloudtodevicemessage** , pokrenite sljedeću naredbu da biste poslali poruku oblaka uređaja i pričekajte da povratne informacije za potvrdu:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Pokrenite aplikaciju da biste c2d naredba za slanje][img-send-command]

    > [AZURE.NOTE] Za sake jednostavnosti, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog trebali biste provesti pravila Ponovi (kao što su eksponencijalne backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara].

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako slanje i primanje poruka oblak na uređaj. 

Da biste vidjeli primjera dovršeno završetka do kraja rješenja koje koriste IoT koncentrator, potražite u članku [Azure IoT paket].

Dodatne informacije o razvoju rješenja s IoT koncentrator potražite u članku [Vodič za razvojne inženjere koncentrator IoT].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Početak rada s IoT koncentratora]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Vodič za IoT koncentrator za razvojne inženjere]: iot-hub-devguide.md
[Razvojni centar za Azure IoT]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Portal za Azure]: https://portal.azure.com
[Azure IoT paketa]: https://azure.microsoft.com/documentation/suites/iot-suite/