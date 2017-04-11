<properties
 pageTitle="Kako korisnici koji zakazuju | Microsoft Azure"
 description="Pomoću ovog praktičnog vodiča pokazuje kako zakazivati zadatke"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Praktični vodič: Zakazivanje i emitiranje poslove (pretpregled)

## <a name="introduction"></a>Uvod
Azure IoT koncentrator je potpuno Upravljani servis koji omogućuje programa aplikacije pozadinskih stvaranje i praćenje zadataka koji zakazivanje i ažuriranje milijune uređaja.  Zadaci se mogu koristiti za sljedeće radnje:

- Ažuriranje svojstava twin želji uređaja
- Ažuriranje uređaj twin oznake
- Pozivanje metode oblaka uređaja

Pojmovno, posao prelama jednu od ovih radnji i prati napredak izvođenja uspoređuje sa skupom uređaja, koja je definirana twin upit.  Ako, na primjer, pomoću posao pozadinskih koji aplikacije možete pozivanje nakon ponovnog pokretanja metode 10 000 uređajima određen upit twin i planirati buduće vrijeme.  Tu aplikaciju možete pratiti napredak kao svaki od tih uređaja primati i izvršavanje način ponovno pokrenite računalo.

Dodatne informacije o svakoj od tih mogućnosti u sljedećim člancima:

- Svojstva i twin uređaj: [Početak rada s twin] [ lnk-get-started-twin] i [Praktični vodič: kako koristiti svojstva twin][lnk-twin-props]
- Oblak uređaj metode: [Vodič za razvojne inženjere – Izravni metode] [ lnk-dev-methods] i [Praktični vodič: C2D metode][lnk-c2d-methods]

Pomoću ovog praktičnog vodiča prikazuje kako da biste:

- Stvaranje Simulirani uređaj koji ima Izravni način koji omogućuje lockDoor koje možete pozvati natrag u aplikaciji Završi.
- Stvaranje aplikacije konzole poziva metodu lockDoor izravno na Simulirani uređaja koji se koristi za posao i ažurira svojstva twin želji pomoću uređaja posla.

Na kraju ovog praktičnog vodiča, imate dvije Node.js konzole aplikacije:

**simDevice.js**, koja povezuje koncentratora za IoT s uređaja identiteta i prima lockDoor Izravni metode.

**scheduleJobService.js**, koji se poziva izravno metode na uređaju Simulirani i ažurirati na twin disired svojstva pomoću posao.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

Verzija Node.js 0.12.x ili noviji <br/>  [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Node.js za ovog praktičnog vodiča u sustavu Windows ili Linux.

Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom ćete odjeljku stvaranja aplikacije konzole Node.js koji odgovara Izravni metode pozove oblaka pokrene nakon ponovnog pokretanja uređaja Simulirani i koristi twin uređaj prijavljenih svojstva da biste omogućili uređaj twin upita pronađite uređaje i kada su zadnje sustava.

1. Stvorite novu praznu mapu pod nazivom **simDevice**.  U mapi **simDevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak.  Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```
    
2. Vaš naredbenog retka u mapi **simDevice** , pokrenite sljedeću naredbu da biste instalirali **azure iot uređaji** uređaj SDK paketa i paketa **azure-iot-uređaja-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. U mapi **simDevice** uređivaču teksta, stvorite novu datoteku **simDevice.js** .

4. Dodajte sljedeće "zatraži' naredbe na početku **simDevice.js** datoteke:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Dodati varijable **connectionString** i koristiti da biste stvorili uređaj klijenta.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Dodajte sljedeća funkcija za rukovanje metodu lockDoor.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Dodajte sljedeći kod da biste registrirali rukovatelj za metodu lockDoor.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Spremite i zatvorite datoteku **simDevice.js** .

> [AZURE.NOTE] Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog provodite pravila Ponovi (kao što je eksponencijalnom backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara][lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Zakazivanje zadataka za pozivanje Izravni metode i ažuriranje svojstava u twin

U ovom ćete odjeljku stvoriti aplikaciju za Node.js konzole koja pokreće udaljene lockDoor na uređaju Izravni metodom i ažuriranje svojstava u twin.

1. Stvorite novu praznu mapu pod nazivom **scheduleJobService**.  U mapi **scheduleJobService** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak.  Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```
    
2. Vaše naredbenog retka u mapi **scheduleJobService** , pokrenite sljedeću naredbu da biste instalirali **azure iothub** uređaj SDK paketa i paketa **azure-iot-uređaja-mqtt** :

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. U mapi **scheduleJobService** uređivaču teksta, stvorite novu datoteku **scheduleJobService.js** .

4. Dodajte sljedeće "zatraži' naredbe na početku **dmpatterns_gscheduleJobServiceetstarted_service.js** datoteke:

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Dodavanje sljedeće varijable deklaracija i zamjena vrijednosti rezerviranog mjesta:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Dodajte sljedeće funkciju koja će se koristiti za praćenje izvršavanja zadatka:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Dodajte sljedeći kod da biste zakazali zadatak koji poziva metodu uređaj:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Dodajte sljedeći kod da biste zakazali zadatak da biste ažurirali na twin:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Spremite i zatvorite datoteku **scheduleJobService.js** .

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbeni-u mapi **simDevice** , pokrenite sljedeću naredbu da biste započeli slušanje za Izravni način ponovno pokrenite računalo.

    ```
    node simDevice.js
    ```

2. U naredbeni-u mapi **scheduleJobService** , pokrenite sljedeću naredbu za pokretanje udaljenog ponovno pokrenite računalo i upit za twin uređaj da biste pronašli posljednji vremena ponovnog pokretanja računala.

    ```
    node scheduleJobService.js
    ```

3. Prikazat će se rezultat s uređaja i pozadinska aplikacija.


## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču koristi posao da biste zakazali izravno metode uređaja i ažuriranje svojstava twin uređaja.

Da biste nastavili prvi koraci s IoT koncentratora i uzorke upravljanja uređajima kao što je alat za analizu daljinske putem ažuriranje opreme air, pogledajte:

[Praktični vodič: Kako ažuriranje opreme][lnk-fwupdate]

Da biste nastavili Uvod u IoT koncentrator, potražite u članku [Uvod u pristupnik SDK IoT][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx