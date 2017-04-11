<properties
 pageTitle="Izravni načina | Microsoft Azure"
 description="Ovom ćete praktičnom vodiču saznati kako koristiti Izravni metode"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Praktični vodič: Korištenje izravne metode

## <a name="introduction"></a>Uvod

Azure IoT koncentrator je potpuno Upravljani servis koji omogućuje pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i aplikacije sigurnosno završetka. Prethodni vodiče ([Početak rada s IoT koncentratora] i [slanje poruka oblak na uređaj s IoT koncentrator]) prikazuju osnovni uređaj u oblak i oblaka uređaj razmjenu funkcionalnost IoT koncentratora. Koncentrator IoT i daje mogućnost za pozivanje metode koje nisu durable na uređajima iz oblaka. Načini predstavljaju zahtjev za odgovor interakcije s uređajem koji je slična poziv HTTP iz tog njihovu uspjeti ili neće uspjeti (čim korisnik navedena ograničenja) da bi korisnik znao status poziva. [Pozivanje metode izravno na uređaju] [ lnk-devguide-methods] metode detaljno opisuje i nudi upute o tome kada koristiti metode nasuprot poruke oblak na uređaj.

Pomoću ovog praktičnog vodiča prikazuje kako da biste:

- Stvorite koncentratora za IoT i stvorite identitet uređaj u koncentratora za IoT pomoću portala za Azure.
- Stvaranje Simulirani uređaj koji ima Izravni metode koje možete pozvati tako da u oblak.
- Stvaranje aplikacije konzole koja se poziva izravno metode na uređaju Simulirani putem koncentratora za IoT.

> [AZURE.NOTE] Trenutačno Izravni metode su dostupne samo s uređaja povezanih s IoT koncentrator pomoću protokola MQTT. Pogledajte [MQTT podržava] [ lnk-devguide-mqtt] članak upute za pretvaranje postojeće aplikacije uređaj da biste koristili MQTT.

Na kraju ovog praktičnog vodiča, imate dvije Node.js konzole aplikacije:

* **CallMethodOnDevice.js**, koja se poziva metode na Simulirani uređaja i prikazuje odgovor.
* **SimulatedDevice.js**, koja povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili i odgovori na način pozove oblaka.

> [AZURE.NOTE] U članku [IoT koncentrator SDK-ovi] [ lnk-hub-sdks] pruža informacije o raznim SDK-ovi koje možete koristiti da biste sastavili obje aplikacije da biste pokrenuli na uređaje i pozadinskih vašeg rješenja.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Verzija Node.js 0.10.x ili noviji.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koji odgovara metode pozove oblaka.

1. Stvorite novu praznu mapu pod nazivom **simulateddevice**. U mapi **simulateddevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **simulateddevice** , pokrenite sljedeću naredbu da biste instalirali **azure iot uređaji** uređaj SDK paketa i paketa **azure-iot-uređaja-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. U mapi **simulateddevice** uređivaču teksta, stvorite novu datoteku **SimulatedDevice.js** .

4. Dodajte sljedeće `require` naredbe na početku **SimulatedDevice.js** datoteke:

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Dodati varijable **connectionString** i koristiti da biste stvorili uređaj klijenta. **{Niz za povezivanje uređaja}** zamijenite generira u odjeljku *Stvaranje uređaju identitetu* niz za povezivanje:

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Dodajte sljedeća funkcija implementaciju metodu na uređaju:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Otvori vezu IoT koncentratora i početak Inicijalizacija ga slušatelj metoda:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Spremite i zatvorite datoteku **SimulatedDevice.js** .

> [AZURE.NOTE] Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog provodite pravila Ponovi (kao što su veze Ponovi), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara][lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Pozivanje metode na uređaj

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koji poziva metode na Simulirani uređaja i prikazuje odgovor.

1. Stvorite novu praznu mapu pod nazivom **callmethodondevice**. U mapi **callmethodondevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **callmethodondevice** , pokrenite sljedeću naredbu da biste instalirali paket **azure iothub** :

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Korištenje uređivača teksta, stvoriti datoteku **CallMethodOnDevice.js** u mapi **callmethodondevice** .

4. Dodajte sljedeće `require` naredbe na početku **CallMethodOnDevice.js** datoteke:

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Dodavanje sljedeće varijable izvješća i vrijednost rezerviranog mjesta zamijenite niz za povezivanje za koncentratora za IoT:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Stvaranje klijentskog programa da biste otvorili veza koncentratora za IoT.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Dodajte sljedeća funkcija pozivanje metode uređaja i ispišite uređaj odgovor na konzoli sustava:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Spremite i zatvorite datoteku **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbenom retku u mapi **simulateddevice** , pokrenite sljedeću naredbu da biste pokrenuli slušanje za metodu pozive iz centra za IoT:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. U naredbenom retku u mapi **callmethodondevice** , pokrenite sljedeću naredbu da biste započeli nadzor koncentratora za IoT:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Prikazat će se uređaj Upoznajte na način ispisa poruke i aplikacije koja se naziva način prikaza odgovor s uređaja:

    ![][9]
    
## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču konfiguriran novi koncentrator IoT na portalu za Azure, a zatim stvoriti uređaju identitetu u odjeljku središte identiteta registra. Koristi ovaj identitet uređaj da biste omogućili aplikaciju Simulirani uređaj da biste brzo načinima pozvati tako da u oblak. Koji ste stvorili i aplikaciju koja se poziva metode na uređaju i prikazuje odgovor s uređaja. 

Da biste nastavili Uvod u koncentratoru IoT i drugim situacijama IoT za istraživanje potražite u članku:

- [Početak rada s IoT koncentratora]
- [Zakazivanje zadataka na više uređaja][lnk-devguide-jobs]

Da biste saznali kako proširiti vaše IoT rješenja i raspored način poziva na više uređaja, vidjeti [Raspored i emitiranje poslove] [ lnk-tutorial-jobs] vodič.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Slanje poruka oblak na uređaj s IoT koncentratora]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Početak rada s IoT koncentratora]: iot-hub-node-node-getstarted.md
