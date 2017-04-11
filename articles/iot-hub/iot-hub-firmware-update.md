<properties
 pageTitle="Kako ažuriranje opreme | Microsoft Azure"
 description="Pomoću ovog praktičnog vodiča prikazuje kako ažuriranje opreme"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Praktični vodič: Kako je oprema ažuriranja (pretpregled)

## <a name="introduction"></a>Uvod
Na [Početak upravljanja uređajima pomoću] [ lnk-dm-getstarted] vodiča, vidjeli kako koristiti [uređaj twin] [ lnk-devtwin] i [metode oblaka uređaja (C2D)] [ lnk-c2dmethod] primitives za daljinsko ponovnog pokretanja uređaja. Pomoću ovog praktičnog vodiča koristi iste primitives IoT koncentratora i sadrži smjernice i prikazuje kako ažuriranje završetka do kraja Simulirani firmver.  Ovaj uzorak se koristi za implementaciju ažuriranje opreme ogledne Intel Edison uređaja.

Pomoću ovog praktičnog vodiča prikazuje kako da biste:

- Stvaranje aplikacije konzole za poziva metodu firmwareUpdate izravno na uređaju Simulirani putem koncentratora za IoT.
- Stvaranje Simulirani uređaj koji implementira firmwareUpdate Izravni metode koje prolazi kroz više spremišta procesa koji se pričekati da biste preuzeli slike firmver preuzimanja slika firmver i na kraju primjenjuje ti firmver slike.  U svakoj fazi izvršavanja koristi uređaj na twin prijavljenih svojstva da biste ažurirali tijek.

Na kraju ovog praktičnog vodiča, imate dvije Node.js konzole aplikacije:

**dmpatterns_fwupdate_service.js**, koja se poziva izravno metode na Simulirani uređaj, prikazuje odgovor, a povremeno (svaki 500ms) prikazuje twin ažuriranog uređaja prijavljenih svojstva.

**dmpatterns_fwupdate_device.js**, koji se povezuje koncentratora za IoT s identitetom uređaj ste ranije stvorili, prima Izravni metode firmwareUpdate, pokreće se kroz postupak više stanja u programu Publisher uključujući na ažuriranje opreme: čeka se preuzimanje slike, preuzimanje novu sliku i na kraju primjene slike.


Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

Verzija Node.js 0.12.x ili noviji <br/>  [Priprema razvojno okruženje] [ lnk-dev-setup] u članku se opisuje kako instalirati Node.js za ovog praktičnog vodiča u sustavu Windows ili Linux.

Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

Slijedite u članku [Početak rada s upravljanja uređajima](iot-hub-device-management-get-started.md) da biste stvorili koncentratora za IoT i dohvaćanje niza za povezivanje.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Stvaranje aplikacije Simulirani uređaja

U ovom ćete odjeljku stvaranja aplikacije konzole Node.js koji odgovara Izravni metode pozove oblaka, pokreće ažuriranje opreme Simulirani uređaja i koristi twin uređaj prijavljenih svojstva da biste omogućili uređaj twin upita pronađite uređaje i kada su zadnje sustava.

1. Stvorite novu praznu mapu pod nazivom **manageddevice**.  U mapi **manageddevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak.  Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```
    
2. Na vašem naredbenog retka u mapi **manageddevice** , pokrenite sljedeću naredbu da biste instalirali na **azure-iot-device@dtpreview** paketa SDK uređaja i **azure-iot-device-mqtt@dtpreview** paketa:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. U mapi **manageddevice** uređivaču teksta, stvorite novu datoteku **dmpatterns_fwupdate_device.js** .

4. Dodajte sljedeće "zatraži' naredbe na početku **dmpatterns_fwupdate_device.js** datoteke:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Dodati varijable **connectionString** i koristiti da biste stvorili uređaj klijenta.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Dodajte sljedeće funkciju koja će se koristiti za ažuriranje twin prijavljenih svojstva

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Dodajte sljedeće funkcije koji će se kao zamjenu za preuzimanje i primjenu firmver slike.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Dodajte sljedeće funkciju koja će se ažurirati status ažuriranja opreme kroz na twin prijavljene svojstva na čekanje da biste preuzeli.  Obično su uređaji obavijestiti o raspoložive ažuriranje i administrator definirana pravila uzroci uređaj da biste pokrenuli preuzimanje i primjenu ažuriranja.  Ovo je gdje želite pokrenuti logike da biste omogućili to pravilo.  Jednostavnosti, ne možemo se što za 4 sekunde i nastavite da biste preuzeli slike firmver. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Dodajte sljedeće funkciju koja će se ažurirati status ažuriranja opreme kroz na twin prijavljivati svojstva preuzimanja slike firmver.  Iza prema gore za simulaciju firmver preuzimanje i na kraju obnavlja status ažuriranje opreme obavještavate preuzimanje uspjeh ili nije uspjelo.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Dodajte sljedeće funkciju koja će se ažurirati status ažuriranja opreme kroz na twin prijavljivati svojstva primjene slika firmver.  Iza prema gore za simulaciju primjene firmver slike i na kraju obnavlja status ažuriranje opreme obavještavate Primijeni uspjeh ili nije uspjelo.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Dodajte sljedeće functoin koji rukovati metodu firmwareUpdate i pokretanje postupka ažuriranja više spremišta firmver.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Na kraju, dodajte sljedeći kod koji se povezuje koncentrator IoT kao na uređaju 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Da biste zadržali jednostavnih stvari, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog provodite pravila Ponovi (primjerice na eksponencijalnom backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara][lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Pokretanje ažuriranje opreme udaljene na uređaju Izravni metodom 

U ovom se odjeljku stvorite aplikaciju za Node.js konzole koja pokreće ažuriranje opreme udaljene na uređaju Izravni metodom i koristi uređaj twin upita da povremeno se status ažuriranje aktivni opreme na tom uređaju.


1. Stvorite novu praznu mapu pod nazivom **triggerfwupdateondevice**.  U mapi **triggerfwupdateondevice** stvoriti datoteku package.json pomoću sljedeće naredbe vaše naredbeni redak.  Prihvaćanje svih zadanih postavki:

    ```
    npm init
    ```
    
2. Vaše naredbenog retka u mapi **triggerfwupdateondevice** , pokrenite sljedeću naredbu da biste instalirali **azure iothub** uređaj SDK paketa i paketa **azure-iot-uređaja-mqtt** :

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. U mapi **triggerfwupdateondevice** uređivaču teksta, stvorite novu datoteku **dmpatterns_getstarted_service.js** .

4. Dodajte sljedeće "zatraži' naredbe na početku **dmpatterns_getstarted_service.js** datoteke:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Dodavanje sljedeće varijable deklaracija i zamjena vrijednosti rezerviranog mjesta:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Dodajte sljedeće funkcije da biste pronašli i prikaz vrijednosti u firmwareUpdate prijavljenih svojstvo.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Dodajte sljedeće funkcije za pozivanje metode firmwareUpdate da biste ponovno pokrenite uređaj cilj:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Na kraju, dodajte sljedeće funkcije kod da biste pokrenuli firmver ažuriranje niza te započne povremeno prikazivati na twin prijavljene svojstva:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Spremite i zatvorite datoteku **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U naredbeni-u mapi **manageddevice** , pokrenite sljedeću naredbu da biste započeli slušanje za Izravni način ponovno pokrenite računalo.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. U naredbeni-u mapi **triggerfwupdateondevice** , pokrenite sljedeću naredbu za pokretanje udaljenog ponovno pokrenite računalo i upit za twin uređaj da biste pronašli posljednji vremena ponovnog pokretanja računala.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Prikazat će se Upoznajte Izravni korakom ispisom poruku

## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču koristi Izravni način da biste pokrenuli ažuriranje opreme udaljene na uređaju i povremeno koristi u twin potrebnom svojstva da biste shvatili tijeku firmver ažuriranje postupak.  

Da biste saznali kako proširiti vaše IoT rješenja i raspored način poziva na više uređaja, vidjeti [Raspored i emitiranje poslove] [ lnk-tutorial-jobs] vodič.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
