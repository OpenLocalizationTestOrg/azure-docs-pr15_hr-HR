<properties
    pageTitle="Azure IoT koncentrator za Node.js Uvod | Microsoft Azure"
    description="Azure IoT koncentrator pomoću Node.js početak rada vodič. Pomoću Azure IoT koncentratora i Node.js sustava Microsoft Azure IoT SDK-ovi za implementaciju rješenja programa Internet stvari."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Početak rada s Azure IoT koncentrator za Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na kraju ovog praktičnog vodiča postoje tri Node.js konzole aplikacije:

* **CreateDeviceIdentity.js**, čime se uređaju identitetu i pridruženi sigurnosni ključ povezati Simulirani uređaj.
* **ReadDeviceToCloudMessages.js**, koja se prikazuje telemetrijskih poslao Simulirani uređaj.
* **SimulatedDevice.js**, koja povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili i šalje poruku telemetrijskih svaki drugi pomoću protokola AMQP.

> [AZURE.NOTE] U članku [IoT koncentrator SDK-ovi] [ lnk-hub-sdks] pruža informacije o raznim SDK-ovi koje možete koristiti da biste sastavili obje aplikacije da biste pokrenuli na uređaje i pozadinskih vašeg rješenja.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ Verzija Node.js 0.10.x ili noviji.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sada ste stvorili koncentratora za IoT. Imate IoT koncentrator naziv glavnog računala i niz za povezivanje IoT koncentratora koje su vam potrebne da biste dovršili kraja ovog praktičnog vodiča.

## <a name="create-a-device-identity"></a>Stvaranje uređaja identiteta

U ovom se odjeljku stvorite Node.js konzole aplikacija na uređaju identitetu stvara u registru identiteta koncentratora za IoT. Uređaj ne može povezati s IoT koncentrator osim ako ima stavku u registru identiteta uređaja. Pogledajte odjeljak **Uređaj identiteta registra** [IoT koncentrator za razvojne inženjere vodič] [ lnk-devguide-identity] dodatne informacije. Kada pokrenete ovu aplikaciju konzole, generira ID jedinstveni uređaja i ključ koji uređaju možete koristiti da biste odredili sam kada šalje poruke uređaj u oblak IoT koncentrator.

1. Stvorite novu praznu mapu pod nazivom **createdeviceidentity**. U mapi **createdeviceidentity** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **createdeviceidentity** , pokrenite sljedeću naredbu da biste instalirali paket SDK servisa **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. Korištenje uređivača teksta, stvoriti datoteku **CreateDeviceIdentity.js** u mapi **createdeviceidentity** .

4. Dodajte sljedeće `require` izjava na početku **CreateDeviceIdentity.js** datoteke:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Dodajte sljedeći kod **CreateDeviceIdentity.js** datoteku, a vrijednost rezerviranog mjesta zamijenite niz za povezivanje za središtu IoT koji ste stvorili u prethodnom odjeljku: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Dodajte sljedeći kod da biste stvorili definiciju uređaj u registru identiteta uređaj koncentratora za IoT. Kod stvara uređaja ako id uređaja ne postoji u registru, u suprotnom vraća ključ postojeću datoteku:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Spremite i zatvorite datoteku **CreateDeviceIdentity.js** .

8. Da biste pokrenuli aplikaciju **createdeviceidentity** , pokrenite sljedeću naredbu u naredbenom retku u mapi createdeviceidentity:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Zabilježite **id uređaja** i **ključ uređaja**. Morate te vrijednosti kasnije prilikom stvaranja aplikacije koja se povezuje s IoT koncentrator kao uređaj.

> [AZURE.NOTE] Identitet registra IoT koncentrator samo pohranjuje identiteta uređaj za omogućivanje sigurnog pristupa središtu. Sprema uređaju ID-a i tipke da biste koristili kao sigurnosne vjerodajnice i omogućeno/onemogućeno Označi zastavicom koje možete koristiti da biste onemogućili pristup za pojedinačne uređaj. Ako aplikacija nije potrebno pohranjivati metapodatke druge specifične za uređaj, trebali biste koristiti u spremištu specifičnim aplikacijama. Pogledajte vodič za [razvojne inženjere koncentrator IoT] [ lnk-devguide-identity] dodatne informacije.

## <a name="receive-device-to-cloud-messages"></a>Primanje poruka uređaj u oblak

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koji se čita uređaj u oblak poruke iz centra za IoT. Koncentratora za IoT izlaže [Koncentratora događaj][lnk-event-hubs-overview]-kompatibilne krajnju točku da biste mogli čitati poruke uređaj u oblak. Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča stvara čitača osnovni koja nije prikladna za implementaciju visoke propusnost. [Postupak uređaj u oblak poruke] [ lnk-process-d2c-tutorial] vodič prikazuje kako obraditi uređaju u oblak poruke na razini. [Početak rada s koncentratorima događaj] [ lnk-eventhubs-tutorial] Praktični vodič pruža dodatno informacije o porukama postupak iz koncentratora za događaj i se primjenjuju na krajnje točke kompatibilan s preglednikom događaja koncentrator IoT koncentratora.

> [AZURE.NOTE] Krajnje kompatibilan s preglednikom događaja koncentrator za čitanje poruka uređaj u oblak uvijek koristi protokol AMQP.

1. Stvorite novu praznu mapu pod nazivom **readdevicetocloudmessages**. U mapi **readdevicetocloudmessages** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **readdevicetocloudmessages** , pokrenite sljedeću naredbu da biste instalirali paket **azure događaj koncentratora** :

    ```
    npm install azure-event-hubs --save
    ```

3. Korištenje uređivača teksta, stvoriti datoteku **ReadDeviceToCloudMessages.js** u mapi **readdevicetocloudmessages** .

4. Dodajte sljedeće `require` naredbe na početku **ReadDeviceToCloudMessages.js** datoteke:

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Dodavanje sljedeće varijable izvješća i vrijednost rezerviranog mjesta zamijenite niz za povezivanje za koncentratora za IoT:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Dodajte sljedeće dvije funkcije Izlaz na konzoli:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Dodajte sljedeći kod stvaranje **EventHubClient**, otvorite vezu da biste koncentratora za IoT i stvaranje tekstnog okvira za svaki particije. Ovu aplikaciju koristi filtar kada se stvara u tako da se primatelja prikazuje samo poruke poslane koncentrator IoT nakon pokretanja primatelja pokrenut. Ovaj je filtar korisna u okruženje za testiranje tako da vidite samo trenutni skup poruke. U okruženju proizvodnje koda provjerite je li procijenit sve poruke – pogledajte [upute za obradu IoT koncentrator uređaj u oblak poruka] [ lnk-process-d2c-tutorial] vodiča dodatne informacije:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Spremite i zatvorite datoteku **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koji simulira uređaj koji šalje poruke uređaj u oblak koncentratora za IoT.

1. Stvorite novu praznu mapu pod nazivom **simulateddevice**. U mapi **simulateddevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **simulateddevice** , pokrenite sljedeću naredbu da biste instalirali **azure iot uređaji** uređaj SDK paketa i paketa **azure-iot-uređaja-amqp** :

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. U mapi **simulateddevice** uređivaču teksta, stvorite novu datoteku **SimulatedDevice.js** .

4. Dodajte sljedeće `require` naredbe na početku **SimulatedDevice.js** datoteke:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Dodati varijable **connectionString** i koristiti da biste stvorili uređaj klijenta. Zamjena **{youriothostname}** pod nazivom središtu IoT koji ste stvorili u odjeljku *Stvaranje koncentratora za IoT* . **{Yourdevicekey}** zamijenite vrijednost ključa uređaj generira u odjeljku *Stvaranje uređaju identitetu* :

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Dodajte sljedeće funkcije da biste prikazali Izlaz iz aplikacije:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Stvaranje na povratnog i korištenje funkcije **setInterval** da biste poslali novu poruku koncentratora za IoT svaki drugi:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

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

8. Otvaranje veze s koncentratora za IoT i početak slanja poruka:

    ```
    client.open(connectCallback);
    ```

9. Spremite i zatvorite datoteku **SimulatedDevice.js** .

> [AZURE.NOTE] Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog provodite pravila Ponovi (kao što je eksponencijalnom backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara][lnk-transient-faults].


## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbenom retku u mapi **readdevicetocloudmessages** , pokrenite sljedeću naredbu da biste započeli nadzor koncentratora za IoT:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js IoT koncentrator servisa klijentska aplikacija za praćenje poruka uređaj u oblak][7]

2. U naredbenom retku u mapi **simulateddevice** , pokrenite sljedeću naredbu da biste započeli slanja telemetrijskih podataka koncentratora za IoT:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js IoT koncentrator uređaj klijentske aplikacije za slanje poruka uređaj u oblak][8]

3. **Korištenje** pločice [Azure portal] [ lnk-portal] prikazuje se broj poruka poslana središtu:

    ![Azure portala korištenje pločica s prikazom broj poruke poslane IoT koncentratora][43]

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču konfiguriran novi koncentrator IoT na portalu za Azure, a zatim stvoriti uređaju identitetu u odjeljku središte identiteta registra. Koristi ovaj identitet uređaj da biste omogućili aplikaciju Simulirani uređaj da biste poslali poruke uređaj u oblak središtu. Koji ste stvorili i aplikaciju koja se prikazuje u poruke koje su stigle prema središtu. 

Da biste nastavili Uvod u koncentratoru IoT i drugim situacijama IoT za istraživanje potražite u članku:

- [Povezivanje uređaja][lnk-connect-device]
- [Uvod u upravljanje uređajima][lnk-device-management]
- [Uvod u pristupnik SDK IoT][lnk-gateway-SDK]

Da biste saznali kako proširiti rješenje IoT i obraditi uređaju u oblak poruke na razini, prikazati [poruka uređaj u oblak postupak] [ lnk-process-d2c-tutorial] vodič.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
