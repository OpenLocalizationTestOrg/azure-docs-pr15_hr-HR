<properties
    pageTitle="Korištenje svojstava twin | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču saznati kako koristiti twin svojstva"
    services="iot-hub"
    documentationCenter=".net"
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

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Praktični vodič: Unesite željenu svojstva da biste konfigurirali uređaje (pretpregled)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na kraju ovog praktičnog vodiča, imat ćete dvije Node.js konzole aplikacije:

* **SimulateDeviceConfiguration.js**, Simulirani uređaj aplikacija na izvješćima statusu postupka ažuriranje Simulirani konfiguraciju i čeka željena konfiguracija ažuriranja.
* **SetDesiredConfigurationAndQuery.js**, Node.js aplikacije htjeli može pokrenuti pozadinskih, koji se postavlja željena konfiguracija na uređaju i upitima postupku ažuriranja konfiguracije.

> [AZURE.NOTE] U članku [IoT koncentrator SDK-ovi] [ lnk-hub-sdks] pruža informacije o raznim SDK-ovi koje možete koristiti da biste sastavili uređaja i pozadinske aplikacije.

Da biste dovršili ovaj Praktični vodič potrebno je sljedeće:

+ Verzija Node.js 0.10.x ili noviji.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

Ako ste pratili [Početak rada s uređajem twins] [ lnk-twin-tutorial] vodič, već imate koncentrator omogućiti upravljanje uređajima i uređaju identitetu naziva **myDeviceId**; i možete prijeći na [Stvaranje aplikacije Simulirani uređaj] [ lnk-how-to-configure-createapp] sekciju.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koja povezuje se s vašeg koncentrator kao **myDeviceId**, čeka željena konfiguracija ažuriranja i zatim izvješća ažuriranja o postupku ažuriranja Simulirani konfiguracije.

1. Stvorite novu praznu mapu pod nazivom **simulatedeviceconfiguration**. U mapi **simulatedeviceconfiguration** stvorite novu datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaše naredbenog retka u mapi **simulatedeviceconfiguration** , pokrenite sljedeću naredbu da biste instalirali **azure iot uređaji**i **azure-iot-uređaja-mqtt** paketa:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. U mapi **simulatedeviceconfiguration** uređivaču teksta, stvorite novu datoteku **SimulateDeviceConfiguration.js** .

4. Dodajte sljedeći kod **SimulateDeviceConfiguration.js** datoteku i zamijeniti rezervirano mjesto za **{niz za povezivanje uređaja}** s koju ste kopirali stvaranja uređaju identitetu **myDeviceId** niz za povezivanje:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    **Klijentski** objekt izlaže sve metode koje su potrebne za interakciju s twins uređaj s uređaja. Prethodni kod kada ga inicijalizira **klijentskog** objekta dohvaća twin za **myDeviceId**i i pridružuje upravljački program za ažuriranje na željeni svojstva. Rukovatelj provjerava postoji li je zahtjev za promjenom na stvarni konfiguracije usporedbom u configIds, a zatim poziva način na koji će se pokrenuti promjene konfiguracije.

    Imajte na umu da radi jednostavnosti, prethodni kod koristi zadanu programiranih za početnu konfiguraciju. Realni aplikacija vjerojatno želite učitati tu konfiguraciju iz lokalno spremište.
    
    > [AZURE.IMPORTANT] Željeni svojstvo događajima promjene uvijek jednom čuje na vezu uređaja, provjerite je li da biste provjerili ima li stvarnu promjenu u željenom svojstva prije izvođenja ništa.

5. Dodavanje načina prije na `client.open()` poziva:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    Način **initConfigChange** ažurira prijavljenim svojstva objekta lokalni twin sa zahtjevom za ažuriranje config i postavlja status na **čekanju**, a zatim ažurira twin uređaj na servis. Nakon uspješnog postavljanja na twin, simulira dugo izvodi proces kojim se prekida u izvođenja **completeConfigChange**. Ta metoda ažurira lokalne twin prijavljenim svojstva **uspješnog** postavljanja statusa i uklanjanje **pendingConfig** objekt. Zatim se ažurira twin na servis.

    Imajte na umu da da biste spremili propusnost, prijavljenih svojstva ažuriraju navođenjem svojstva za mijenjanje (imenovani **zakrpu** iznad kod), umjesto zamjene cijeli dokument.

    > [AZURE.NOTE] Pomoću ovog praktičnog vodiča zamjenu ponašanje Istodobni konfiguracije ažuriranja. Neke procesa ažuriranje konfiguracije možda moći kako bi odgovarao promjene konfiguracije cilj dok traje ažuriranja, drugima možda ćete morati red čekanja ih i drugi ih odbaciti s pogrešku. Provjerite jeste li razmotrite željeno ponašanje za proces određenu konfiguraciju, a odgovarajuće logike prije započinjanja promjene konfiguracije.

6. Pokrenite aplikaciju uređaj:

        node SimulateDeviceConfiguration.js

    Trebali biste vidjeti poruku `retrieved device twin`. Ažurirati aplikaciju pokrenut.

## <a name="create-the-service-app"></a>Stvaranje aplikacije servisa

U ovom ćete odjeljku će stvaranje aplikacije za Node.js konzole koji ažurira *željenim svojstvima* na twin pridružene **myDeviceId** s novom objektu konfiguracije telemetrijskih. Zatim upiti twins uređaja koji je pohranjen u središtu i prikazuje razliku između željeni i poznatih konfiguracija uređaja.

1. Stvorite novu praznu mapu pod nazivom **setdesiredandqueryapp**. U mapi **setdesiredandqueryapp** stvorite novu datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **setdesiredandqueryapp** , pokrenite sljedeću naredbu da biste instalirali paket **azure iothub** :

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. U mapi **addtagsandqueryapp** uređivaču teksta, stvorite novu datoteku **SetDesiredAndQuery.js** .

4. Dodajte sljedeći kod **SetDesiredAndQuery.js** datoteku i zamijeniti rezervirano mjesto za **{niz za povezivanje servisa}** s nizom za povezivanje koju ste kopirali kada stvarate vaše središte:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    Objekt **registra** izlaže sve metode koje su potrebne za interakciju s uređaja twins iz servisa. Prethodni kod kada ga inicijalizira objekt **registra** dohvaća twin za **myDeviceId**, a ažurira svojstvima željeni novi objekt konfiguracije za telemetriju. Nakon toga ga poziva funkcija događaj **queryTwins** 10 sekundi.

    > [AZURE.IMPORTANT] Ovu aplikaciju upiti koncentrator IoT svakih 10 sekundi opisne svrhe. Korištenje upita za generiranje izvješća korisnika dostupnog na brojnim uređajima, ali ne otkriva promjene. Ako rješenje zahtijeva u stvarnom vremenu obavijesti o događajima uređaj pomoću [uređaja u oblak poruka][lnk-d2c].

5. Dodati sljedeći kod desnom prije na `registry.getDeviceTwin()` poziva za implementaciju funkciju **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Prethodni kod upita u twins pohranjene u središtu i ispisali na željeni i poznatih telemetrijskih konfiguracije. Pogledajte [Jezik upita IoT koncentrator] [ lnk-query] da biste saznali kako generiranje bogata izvješća putem svih svojih uređaja.


8. **SimulateDeviceConfiguration.js** je pokrenut, pokrenite aplikaciju s:

        node SetDesiredAndQuery.js 5m

    Trebali biste vidjeti na prijavljenim promjenom u konfiguraciji iz **uspjeh** na **čekanju** za **uspjeh** ponovno s novi aktivnog slanje učestalost od pet minuta umjesto 24 sata.

    > [AZURE.IMPORTANT] Postoji Odgoda od najviše minuta između operacija izvješća uređaja i rezultata upita. To je da biste omogućili infrastruktura za upit da biste radili na razini vrlo visoka. Dohvaćanje dosljedan prikaza jedan twin upotrijebite metodu **getDeviceTwin** u razredu **registra** .

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču postavite željena konfiguracija kao *željeni svojstva* iz aplikacije pozadinskih, a napisali aplikacije Simulirani uređaj da biste otkrili promjena i kao zamjenu za ažuriranje više koraka procesa izvješćivanje njezin status kao *prijavljenih svojstva* na twin.

Pomoću sljedećih resursa da biste saznali kako:

- Slanje telemetrijskih s uređaja s [Početak rada s IoT koncentrator] [ lnk-iothub-getstarted] ćete praktičnom vodiču
- Zakazivanje i izvođenje operacija na velikih skupova uređajima potražite u članku [Korištenje poslovi omogućuje zakazivanje i emitiranje uređaj operacije] [ lnk-schedule-jobs] vodič.
- Upravljanje uređajima interaktivno (kao što su uključivanjem Lepeza iz aplikacije za korisnički kontrolirano), s [Izravni načina korištenja] [ lnk-methods-tutorial] vodič.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
