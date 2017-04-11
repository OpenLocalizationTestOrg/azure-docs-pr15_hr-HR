<properties
 pageTitle="Početak upravljanja uređajima pomoću | Microsoft Azure"
 description="Pomoću ovog praktičnog vodiča pokazuje kako započeti s upravljanja uređajima na središte IoT Azure"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Praktični vodič: Početak rada s upravljanja uređajima (pretpregled)

## <a name="introduction"></a>Uvod
IoT oblaka aplikacije mogu koristiti primitives u središte IoT Azure, odnosno twin uređaja i izravno metode za daljinsko pokretanje i praćenje uređaj upravljanje akcije na uređajima.  Ovaj članak sadrži upute i kod za aplikaciju oblaka IoT i uređaja funkcioniranje zajedno da biste započeli i nadzirati pomoću IoT koncentrator ponovno pokretanje udaljenog uređaja.

Za daljinsko pokretanje i praćenje uređaj upravljanje akcije na uređajima iz aplikacije u oblaku, pozadinske koristite Azure IoT koncentrator primitives kao što su [twin uređaj] [ lnk-devtwin] i [metode oblaka uređaja (C2D)][lnk-c2dmethod]. Pomoću ovog praktičnog vodiča prikazano kako pozadinske aplikacije i uređaja možete surađivati koji omogućuju pokretanje i praćenje ponovno pokretanje udaljenog uređaja iz centra za IoT.

Pomoću metode C2D da biste započeli akcije upravljanje uređaj (kao što je ponovno pokrenite računalo, ponovno postavljanje tvorničke i ažuriranje opreme) iz aplikacije pozadinskih u oblaku. Uređaj je zadužen za sljedeće:

- Rukovanje zahtjev za metodu šalje IoT koncentratora.
- Pokretanje odgovarajuće uređaj određene akcije na uređaju.
- Pružanje ažuriranja statusa putem uređaja twin prijavio svojstva IoT koncentrator.

Koristite pozadinske aplikacije u oblaku za pokretanje upita twin uređaj da biste prijavili napretka akcija upravljanje uređaja.

Pomoću ovog praktičnog vodiča prikazuje kako da biste:

- Stvorite koncentratora za IoT i stvorite identitet uređaj u koncentratora za IoT pomoću portala za Azure.
- Stvaranje Simulirani uređaj koji ima Izravni način koji omogućuje ponovno pokrenite računalo na kojem možete pozvati tako da u oblak.
- Stvaranje aplikacije konzole za metodu izravno nakon ponovnog pokretanja poziva na uređaju Simulirani putem koncentratora za IoT.

Na kraju ovog praktičnog vodiča, imate dvije Node.js konzole aplikacije:

**dmpatterns_getstarted_device.js**, koji se povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili, prima Izravni način ponovno pokrenite računalo, simulira fizičke ponovno pokrenite računalo i izvješća vrijeme zadnjeg ponovno pokrenite računalo.

**dmpatterns_getstarted_service.js**, koji poziva izravno metode na Simulirani uređaj, prikazuje odgovor te prikazuje twin ažuriranog uređaja prijavio svojstva.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

Verzija Node.js 0.12.x ili noviji <br/>  [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Node.js za ovog praktičnog vodiča u sustavu Windows ili Linux.

Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom ćete odjeljku stvaranja aplikacije konzole Node.js koji odgovara Izravni metode pozove oblaka pokrene nakon ponovnog pokretanja uređaja Simulirani i koristi twin uređaj prijavljenih svojstva da biste omogućili uređaj twin upita pronađite uređaje i kada su zadnje sustava.

1. Stvorite novu praznu mapu pod nazivom **manageddevice**.  U mapi **manageddevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak.  Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```
    
2. Na vašem naredbenog retka u mapi **manageddevice** , pokrenite sljedeću naredbu da biste instalirali na **azure-iot-device@dtpreview** paketa SDK uređaja i **azure-iot-device-mqtt@dtpreview** paketa:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. U mapi **manageddevice** uređivaču teksta, stvorite novu datoteku **dmpatterns_getstarted_device.js** .

4. Dodajte sljedeće "zatraži' naredbe na početku **dmpatterns_getstarted_device.js** datoteke:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Dodati varijable **connectionString** i koristiti da biste stvorili uređaj klijenta.  Niz za povezivanje zamijenite niz za povezivanje uređaja.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Dodajte sljedeća funkcija implementaciju metodu izravno na uređaju

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Otvaranje veze s koncentratora za IoT i započnite ga slušatelj Izravni metoda:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Spremite i zatvorite datoteku **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog provodite pravila Ponovi (primjerice na eksponencijalnom backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara][lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Pokretanje udaljenog nakon ponovnog pokretanja na uređaju Izravni metodom 

U ovom ćete odjeljku stvaranja aplikacije za Node.js konzole pokreće udaljene nakon ponovnog pokretanja na uređaju Izravni metodom i koristi upita twin uređaj da biste pronašli zadnji put nakon ponovnog pokretanja za taj uređaj.

1. Stvorite novu praznu mapu pod nazivom **triggerrebootondevice**.  U mapi **triggerrebootondevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak.  Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```
    
2. Na vašem naredbenog retka u mapi **triggerrebootondevice** , pokrenite sljedeću naredbu da biste instalirali na **azure-iothub@dtpreview** paketa SDK uređaja i **azure-iot-device-mqtt@dtpreview** paketa:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. U mapi **triggerrebootondevice** uređivaču teksta, stvorite novu datoteku **dmpatterns_getstarted_service.js** .

4. Dodajte sljedeće "zatraži' naredbe na početku **dmpatterns_getstarted_service.js** datoteke:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Dodavanje sljedeće varijable deklaracija i zamjena vrijednosti rezerviranog mjesta:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Dodajte sljedeće funkcije za pozivanje metode uređaj da biste ponovno pokrenite uređaj cilj:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Dodavanje upita za uređaj, a zatim Dohvati zadnji put nakon ponovnog pokretanja sljedeće funkcije:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Dodajte sljedeći kod da biste uputili poziv funkcije koji će pokrenuti izravno način ponovno pokrenite računalo i upit za zadnji put nakon ponovnog pokretanja:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Spremite i zatvorite datoteku **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbeni-u mapi **manageddevice** , pokrenite sljedeću naredbu da biste započeli slušanje za Izravni način ponovno pokrenite računalo.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. U naredbeni-u mapi **triggerrebootondevice** , pokrenite sljedeću naredbu za pokretanje udaljenog ponovno pokrenite računalo i upit za twin uređaj da biste pronašli posljednji vremena ponovnog pokretanja računala.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Prikazat će se Upoznajte Izravni korakom ispisom poruku

## <a name="customize-and-extend-the-device-management-actions"></a>Prilagodba i proširivanje uređaj upravljanje akcije

Vaše IoT rješenja možete proširiti definirani skup uzoraka upravljanje uređaja i omogućivanje prilagođeni uzorci pomoću uređaja twin i C2D metode primitives. Druge akcije za upravljanje uređajima Primjeri tvorničke Vrati, Ažuriranje opreme, ažuriranje softvera, upravljanje power, upravljanje mreže i povezivanje i šifriranje podataka.

## <a name="device-maintenance-windows"></a>Windows održavanje uređaja

Obično konfigurirate uređaji za izvođenje akcija u vrijeme koje minimizira prekidi i isključiti.  Windows održavanje uređaja su uzorak najčešće korištenih za određivanje vremena kada uređaj trebali biste ažurirati svoju konfiguraciju. Vašeg rješenja pozadinske možete koristiti željenim svojstvima twin uređaj da biste definirali i aktiviranje pravila na uređaju koji omogućuje prozora Održavanje. Kada uređaj primi prozor pravilo održavanja, ga o statusu pravila pomoću poznatih svojstvo twin uređaja. Aplikaciju pozadinskih možete koristiti upite twin uređaj da biste potvrditi usklađenost uređaja i svaki pravila.

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču koristi Izravni način za pokretanje udaljenog nakon ponovnog pokretanja na uređaj, koji se koristi twin uređaj prijavljenih svojstva da biste prijavili zadnji put ponovno pokrenite računalo s uređaja i mu za twin uređaj da biste otkrili zadnji put nakon ponovnog pokretanja uređaja iz oblaka.

Da biste nastavili prvi koraci s IoT koncentratora i uzorke upravljanja uređajima kao što je alat za analizu daljinske putem ažuriranje opreme zraka, pogledajte:

[Praktični vodič: Kako ažuriranje opreme][lnk-fwupdate]

Da biste saznali kako proširiti vaše IoT rješenja i raspored način poziva na više uređaja, vidjeti [Raspored i emitiranje poslove] [ lnk-tutorial-jobs] vodič.

Da biste nastavili Uvod u IoT koncentrator, potražite u članku [Uvod u pristupnik SDK IoT][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx