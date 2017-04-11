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

Na kraju ovog praktičnog vodiča, imat ćete dvije Node.js konzole aplikacije:

* **AddTagsAndQuery.js**, Node.js aplikacije htjeli može pokrenuti pozadinskih, koji se dodaje oznake i upitima twins uređaja.
* **TwinSimulatedDevice.js**, Node.js aplikacije koji simulira uređaj koji povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili i njegov uvjeta za povezivanje s izvješćima.

> [AZURE.NOTE] U članku [IoT koncentrator SDK-ovi] [ lnk-hub-sdks] pruža informacije o raznim SDK-ovi koje možete koristiti da biste sastavili uređaja i pozadinske aplikacije.

Da biste dovršili ovaj Praktični vodič potrebno je sljedeće:

+ Verzija Node.js 0.10.x ili noviji.

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Stvaranje aplikacije servisa

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole koji dodaje meta-podataka o lokaciji twin pridružene **myDeviceId**. Ga, a zatim upita twins pohranjene u središtu odabirom uređaje nalazi u Sjedinjenih DRŽAVA, a zatim onih te izvješćivanje mobilnu vezu.

1. Stvorite novu praznu mapu pod nazivom **addtagsandqueryapp**. U mapi **addtagsandqueryapp** stvorite novu datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak. Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```

2. Vaš naredbenog retka u mapi **addtagsandqueryapp** , pokrenite sljedeću naredbu da biste instalirali paket **azure iothub** :

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. U mapi **addtagsandqueryapp** uređivaču teksta, stvorite novu datoteku **AddTagsAndQuery.js** .

4. Dodajte sljedeći kod **AddTagsAndQuery.js** datoteku i zamijeniti rezervirano mjesto za **{niz za povezivanje servisa}** s nizom za povezivanje koju ste kopirali kada stvarate vaše središte:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    Objekt **registra** izlaže sve metode koje su potrebne za interakciju s uređaja twins iz servisa. Prethodni kod najprije inicijalizira objekt **registra** , a zatim dohvaća twin za **myDeviceId**i na kraju ažurira njegov oznake informacijama željeno mjesto.

    Nakon ažuriranja oznake poziva funkciju **queryTwins** .

7. Na kraju **AddTagsAndQuery.js** implementaciju funkciju **queryTwins** dodati sljedeći kod:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Prethodni kod se izvršava dva upita: prvi odabire samo uređaj twins uređaje koji se nalazi u **Redmond43** biljke i drugi refines upit da biste odabrali samo uređaji povezani putem mobilne mreže.

    Imajte na umu da prethodna kod, kada se stvara objekt **upita** Određuje maksimalan broj vraćenih dokumenata. Objekt **upita** sadrži **hasMoreResults** logičko svojstvo koje možete koristiti za pozivanje metode **nextAsTwin** više puta da biste dohvatili sve rezultate. Metode naziva **Dalje** dostupna je za rezultata koji nisu twins uređaj, na primjer, rezultati upita za zbrajanje.

8. Pokrenite aplikaciju s:

        node AddTagsAndQuery.js

    Trebali biste vidjeti jedan uređaj u rezultatima upita Traži sve uređaje koji se nalazi u **Redmond43** i ništa za upit u kojem se ograničava rezultate uređaje koji koriste mobilnu mrežu.

    ![][1]

U sljedećem odjeljku stvorite aplikaciju uređaja koji se šalju informacije za povezivanje s poslovnim i mijenja rezultat upita u prethodnom odjeljku.

## <a name="create-the-device-app"></a>Stvaranje aplikacije za uređaj

U ovom ćete odjeljku stvoriti Node.js konzole za aplikaciju koja povezuje se s vašeg koncentrator kao **myDeviceId**i ažurira njegov twin prijavljenih svojstva sadrže podatke koji je povezan putem mobilne mreže.

> [AZURE.NOTE] U isto vrijeme uređaja twins su dostupne samo s uređaja povezanih s IoT koncentrator pomoću protokola MQTT. Pogledajte [MQTT podržava] [ lnk-devguide-mqtt] članak upute za pretvaranje postojeće aplikacije uređaj da biste koristili MQTT.

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

6. Sad kad uređaj prijavio svoje podatke za povezivanje bi trebao izgledati oba upita. Vratite se u mapi **addtagsandqueryapp** i ponovno pokrenite upita:

        node AddTagsAndQuery.js

    Ovaj put **myDeviceId** prikazivati i rezultata upita.

    ![][3]

## <a name="next-steps"></a>Daljnji koraci
U ovom ćete praktičnom vodiču konfiguriran novi koncentrator IoT na portalu za Azure, a zatim stvoriti uređaju identitetu u odjeljku središte identiteta registra. Dodaje uređaj meta-podataka kao oznake iz aplikacije pozadinskih i napisali Simulirani uređaj aplikacije o izvješća uređaj s povezivanjem u twin uređaja. I saznali kako postaviti upit podatke pomoću središtu IoT jezik nalik SQL upita.

Pomoću sljedećih resursa da biste saznali kako:

- Slanje telemetriju s uređaja s [Početak rada s IoT koncentrator] [ lnk-iothub-getstarted] ćete praktičnom vodiču
- Konfiguriranje uređajima pomoću twin na željeni svojstva s [pomoću željenog svojstava koja možete konfigurirati uređaje] [ lnk-twin-how-to-configure] ćete praktičnom vodiču
- Upravljanje uređajima interaktivno (kao što su uključivanjem Lepeza iz aplikacije za korisnički kontrolirano), s [Izravni načina korištenja] [ lnk-methods-tutorial] vodič.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md