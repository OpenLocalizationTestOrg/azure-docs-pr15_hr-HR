<properties
    pageTitle="Kao zamjenu za uređaj s pristupnika SDK IoT | Microsoft Azure"
    description="Azure IoT pristupnika SDK prođite kroz sustavu Windows da biste ilustrirali slanje telemetrijskih iz Simulirani uređaja koji koristi pristupnik SDK za Azure IoT."
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT pristupnika SDK (beta) – slanje poruka uređaj u oblak pomoću Simulirani uređaja pomoću sustava Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Stvaranje i pokretanje uzorka

Prije nego što počnete, morate:

- [Postavljanje okruženja za razvoj] [ lnk-setupdevbox] za rad s SDK-a u sustavu Windows.
- [Stvaranje koncentratora za IoT] [ lnk-create-hub] u pretplatu Azure ćete naziv vaše središte da biste dovršili ovaj prikaz. Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.
- Dodavanje dva uređaja koncentratora za IoT i zabilježite svoje ID-a i ključevi uređaj. Možete koristiti na [uređaju Explorer ili iothub explorer] [ lnk-explorer-tools] alat da biste dodali uređajima središtu IoT koji ste stvorili u prethodnom koraku i dohvatiti ključ.

Da biste sastavili uzorka:

1. Otvorite **naredbeni redak za razvojne inženjere za VS2015** naredbeni redak.
2. Dođite do korijenske mape u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu.
3. Pokretanje sustava **Alati\\build.cmd** skripte. Ova skripta stvara datoteku rješenja za Visual Studio, sastavlja rješenje i pokreće testova. Rješenja za Visual Studio možete pronaći u mapi **Sastavljanje** u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu.

Da biste pokrenuli uzorka:

U uređivaču teksta otvorite datoteku **uzoraka\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu. Datoteka konfigurira module u pristupnik za uzorak:

- Modul za **IoTHub** povezuje koncentratora za IoT. Morate konfigurirati je slanje podataka u koncentratora za IoT. Konkretno, postavite vrijednost **IoTHubName** naziv koncentratora za IoT i postavite vrijednost **IoTHubSuffix** **azure devices.net**. Postavite vrijednost **prijenosa** na jedan od: "HTTP", "AMQP" ili "MQTT". Imajte na umu da trenutno samo "HTTP" će zajednički koristiti jednu TCP veza za sve poruke uređaja. Ako postavite vrijednost "AMQP" ili "MQTT" pristupnika će zadržati zasebnu TCP vezu IoT koncentrator za svaki uređaj.
- Modul za **Mapiranje** mapira MAC adrese Simulirani uređaja IoT koncentrator uređaju ID-a. Provjerite da odgovara **deviceId** vrijednosti ID-ove uređaje koje ste dodali koncentratora za IoT i sadrže li vrijednosti **deviceKey** tipke dva uređaja.
- Moduli **BLE1** i **BLE2** su Simulirani uređaji. Imajte na umu kako njihove adrese MAC podudaraju s dimenzijama u modulu **Mapiranje** .
- Modul za **zapisivaču** zapisnik aktivnosti pristupnika u datoteku.
- **Modul put** vrijednosti prikazano u nastavku pretpostavlja da klonirana spremište IoT pristupnika SDK korijenski pogon **C:** . Ako je preuzeti na drugo mjesto, morate prilagođavaju **Modul put** vrijednosti.
- Polja **veze** pri dnu datoteke JSON povezuje module **BLE1** i **BLE2** modul **Mapiranje** i **Mapiranje** modul **IoTHub** modul. Također provjerava sve poruke su zapisuje modul **zapisivaču** .

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
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
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
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

1. U naredbenom retku, dođite do korijenske mape lokalnu kopiju spremište **azure-iot-pristupnika-sdk** .
2. Pokrenite sljedeću naredbu:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Možete koristiti na [uređaju Explorer ili iothub explorer] [ lnk-explorer-tools] alat za praćenje poruke kojima koncentrator IoT prima iz pristupnika.


## <a name="next-steps"></a>Daljnji koraci

Ako želite dobiti dodatne razumijevanja pristupnika SDK IoT i Eksperimentirajte s primjere koda, posjetite sljedeći vodiči za razvojne inženjere i resursima:

- [Slanje poruka uređaj u oblak s realni uređaja s pristupnika SDK IoT][lnk-physical-device]
- [SDK Azure IoT pristupnika][lnk-gateway-sdk]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 