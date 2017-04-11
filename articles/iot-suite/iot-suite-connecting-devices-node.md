<properties
   pageTitle="Priključite uređaj pomoću Node.js | Microsoft Azure"
   description="U članku se opisuje priključite uređaj Azure IoT paket unaprijed konfigurirane udaljene nadzora rješenje pomoću aplikacije pisane Node.js."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Povezivanje uređaja udaljene nadzora konfiguriranog rješenje (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Stvaranje i pokretanje node.js uzorak rješenja

1. Da biste Kloniraj GitHub spremište *Microsoft Azure IoT SDK-ovi* i instalirajte *Microsoft Azure IoT uređaj SDK Node.js* u okruženje za stolna računala, slijedite [Priprema razvojno okruženje] [ lnk-github-prepare] upute.

2. U lokalnoj kopiji [azure-iot-SDK-ovi] [ lnk-github-repo] spremište, a zatim Kopiraj sljedeće dvije datoteke iz mape čvor/uređaj/uzoraka u mapu na uređaju:

  - Packages.JSON
  - remote_monitoring.js

3. Otvorite datoteku remote_monitoring.js i potražite sljedeće varijabla:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Zamijenite **[IoT koncentrator uređaj niz za povezivanje]** niz za povezivanje uređaja. Možete pronaći vrijednosti za koncentratora za IoT naziv glavnog računala, id uređaja i ključ uređaja na udaljenom nadzora rješenje nadzornoj ploči. Niz za povezivanje uređaja sastoji se od sljedećih oblika:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Ako vaš naziv glavnog računala IoT koncentrator **contoso** a identifikacijskog broja za uređaj **mydevice**, niz za povezivanje izgleda:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Spremite datoteku. Pokrenite sljedeće naredbe u naredbenom retku u mapu koja sadrži ove datoteke da biste instalirali potrebne paketa i izvršavaju primjer aplikacije:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md