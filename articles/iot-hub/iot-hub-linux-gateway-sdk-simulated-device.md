<properties
    pageTitle="Kao zamjenu za uređaj s pristupnika SDK IoT | Microsoft Azure"
    description="Azure IoT pristupnika SDK prođite kroz ilustracija slanje telemetrijskih iz Simulirani uređaja koji koristi pristupnik SDK IoT Azure pomoću Linux."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-linux"></a>Azure IoT pristupnika SDK (beta) – slanje poruka uređaj u oblak s Simulirani uređaja koji koristi Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Stvaranje i pokretanje uzorka

Prije nego što počnete, morate:

- [Postavljanje okruženja za razvoj] [ lnk-setupdevbox] za rad s SDK na Linux.
- [Stvaranje koncentratora za IoT] [ lnk-create-hub] u pretplatu Azure ćete naziv vaše središte da biste dovršili ovaj prikaz. Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.
- Dodavanje dva uređaja koncentratora za IoT i zabilježite svoje ID-a i ključevi uređaj. Možete koristiti na [uređaju Explorer ili iothub explorer] [ lnk-explorer-tools] alat da biste dodali uređajima središtu IoT koji ste stvorili u prethodnom koraku i dohvatiti ključ.

Da biste sastavili uzorka:

1. Otvorite u ljusci.
2. Dođite do korijenske mape u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu.
3. Pokrenuti skriptu **tools/build.sh** . Ova skripta koristi uslužni **cmake** da biste stvorili mapu s nazivom **Sastavljanje** u korijenskoj mapi lokalnu kopiju spremište **azure-iot-pristupnika-sdk** i generiranje na makefile. Skripta pa sastavlja rješenje i pokreće testova.

> [AZURE.NOTE]  Svaki put kada pokrenete skriptu **build.sh** , briše i obnavlja mapu **Sastavljanje** u korijenskoj mapi lokalnu kopiju **azure-iot-pristupnika-sdk** spremište.

Da biste pokrenuli uzorka:

U uređivaču teksta otvorite datoteku **samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json** u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu. Datoteka konfigurira module u pristupnik za uzorak:

- Modul za **IoTHub** povezuje koncentratora za IoT. Morate konfigurirati je slanje podataka u koncentratora za IoT. Konkretno, postavite vrijednost **IoTHubName** naziv koncentratora za IoT i postavite vrijednost **IoTHubSuffix** **azure devices.net**. Postavite vrijednost **prijenosa** na jedan od: "HTTP", "AMQP" ili "MQTT". Imajte na umu da trenutno samo "HTTP" će zajednički koristiti jednu TCP veza za sve poruke uređaja. Ako postavite vrijednost "AMQP" ili "MQTT" pristupnika će zadržati zasebnu TCP vezu IoT koncentrator za svaki uređaj.
- Modul za **Mapiranje** mapira MAC adrese Simulirani uređaja IoT koncentrator uređaju ID-a. Provjerite da odgovara **deviceId** vrijednosti ID-ove uređaje koje ste dodali koncentratora za IoT i sadrže li vrijednosti **deviceKey** tipke dva uređaja.
- Moduli **BLE1** i **BLE2** su Simulirani uređaji. Imajte na umu kako njihove adrese MAC podudaraju s dimenzijama u modulu **Mapiranje** .
- Modul za **zapisivaču** zapisnik aktivnosti pristupnika u datoteku.
- **Modul put** vrijednosti prikazano u nastavku pretpostavlja da će pokrenuti uzorka u korijenskom direktoriju lokalnu kopiju **azure-iot-pristupnika-sdk** spremište.
- Polja **veze** pri dnu datoteke JSON povezuje module **BLE1** i **BLE2** modul **Mapiranje** i **Mapiranje** modul **IoTHub** modul. Također provjerava sve poruke su zapisuje modul **zapisivaču** .

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "./build/modules/iothub/libiothub_hl.so",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "./build/modules/identitymap/libidentity_map_hl.so",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "./build/modules/logger/liblogger_hl.so",
            "args":
            {
                "filename":"./deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}

```

Spremite promjene koje ste napravili konfiguracijskoj datoteci.

Da biste pokrenuli uzorka:

1. U ljusci, dođite do korijenske mape u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu.
2. Pokrenite sljedeću naredbu:

    ```
    ./build/samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ./samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```

3. Možete koristiti na [uređaju Explorer ili iothub explorer] [ lnk-explorer-tools] alat za praćenje poruke kojima koncentrator IoT prima iz pristupnika.

## <a name="next-steps"></a>Daljnji koraci

Ako želite dobiti dodatne razumijevanja pristupnika SDK IoT i Eksperimentirajte s primjere koda, posjetite sljedeći vodiči za razvojne inženjere i resursima:

- [Slanje poruka uređaj u oblak s realni uređaja s pristupnika SDK IoT][lnk-physical-device]
- [SDK Azure IoT pristupnika][lnk-gateway-sdk]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Sigurne rješenje IoT na prema gore][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-create-hub]: iot-hub-create-through-portal.md