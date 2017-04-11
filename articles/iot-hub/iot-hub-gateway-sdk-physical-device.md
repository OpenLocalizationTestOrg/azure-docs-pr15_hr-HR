<properties
    pageTitle="Koristi realni uređaj s pristupnika SDK IoT | Microsoft Azure"
    description="Azure IoT pristupnika SDK vodič pomoću uređaja Texas instrumenti SensorTag slanje podataka u koncentratoru IoT putem pristupnika radi na programa Intel Edison izračunati modul"
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT pristupnika SDK (beta) – slanje poruka oblaka uređaj s realni uređaja koji koristi Linux

Ovaj vodič [Bluetooth niskog energiju uzorka] [ lnk-ble-samplecode] pokazuje kako koristiti [Azure IoT pristupnika SDK] [ lnk-sdk] za prosljeđivanje telemetrijskih uređaja u oblak IoT koncentrator fizički uređaj i kako usmjeravanje naredbe iz koncentratora IoT fizički uređaj.

Prikaz pokriva:

* **Arhitektura**: važne arhitektonski informacije o Bluetooth niskog energiju uzorka.

* **Stvaranje i pokretanje**: korake potrebne za stvaranje i pokretanje uzorka.

## <a name="architecture"></a>Arhitektura

U prikazu pokazuje kako izraditi i pokrenuti pristupnik za IoT programa Intel Edison izračunajte modul koji pokreće Linux. Pristupnik je ugrađena pomoću IoT pristupnika SDK-a. Uzorak koristi uređaj Texas instrumenti SensorTag Bluetooth najniža energiju (Tablice) da biste prikupili podaci o temperaturi.

Kada pokrenete pristupnika je:

- Povezuje se s uređajem SensorTag pomoću protokola Bluetooth najniža energiju (Tablice).
- Povezuje IoT koncentrator pomoću protokola HTTP.
- Prosljeđuje telemetrijskih s uređaja SensorTag IoT koncentrator.
- Usmjerava naredbe iz koncentratora IoT SensorTag uređaj.

Pristupnik sadrži sljedeće modula:

- *Modul za Tablice* koji sučelja s uređajem Tablice da biste primali podaci o temperaturi s uređaja i slanje naredbi na uređaju.
- Prikazom *Oblaka Tablice uređaj modul* koja se prevodi poruke JSON dolaze iz oblaka Tablice upute za *Modul Tablice*.
- *Modul zapisivaču* koji zapisuje sve poruke pristupnika.
- Programa *Mapiranje identiteta modul* koji prevodi između Tablice uređaju MAC adrese i identiteta Azure IoT koncentrator uređaja.
- U *Modul IoT koncentratora* koji prenosi telemetrijskih podataka na koncentrator IoT i prima uređaj naredbe iz koncentratora za IoT.
- *Modul Tablice pisača* koji tumači telemetrijskih s uređaja Tablice i ispisuje oblikovani podaci na konzolu da biste omogućili otklanjanje poteškoća i ispravljanje pogrešaka.

### <a name="how-data-flows-through-the-gateway"></a>Tok podataka putem pristupnika

Sljedeći dijagram bloka ilustrira kanal za tijek telemetrijskih prijenos podataka:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Koraci koji stavku telemetrijskih uzima dolazi iz Tablice uređaj IoT koncentrator su:

1. Uređaj Tablice stvara uzorka temperature i šalje putem Bluetooth modul Tablice u pristupnika.
2. Modul za Tablice prima uzorka i objavljuje na broker uz MAC adresa uređaja.
3. Mapiranje modul identiteta uzima ovu poruku, a koristi interni tablice za MAC adresa uređaja Prevedite uređaj identitet koncentrator IoT (ID uređaja i ključ uređaja). Zatim se objavljuje novu poruku koja sadrži ogledne podatke temperatura, MAC adresa uređaja, ID uređaja i ključ uređaja.
4. Modul IoT koncentrator prima ovu novu poruku (generira mapiranje modul identiteta) i objavljuje IoT koncentrator.
5. Modul zapisivaču prijavi svih poruka s brokera datoteka na disku.

Sljedeći dijagram bloka ilustrira kanal protok podataka naredba uređaj:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. Modul IoT koncentrator povremeno polls IoT koncentrator za nove poruke naredbe.
2. Kada modul IoT koncentrator primi novu poruku naredbe, objavljuje ga da biste brokera.
3. Mapiranje modul identiteta uzima poruke naredbe i koristi interni tablice da biste preveli ID uređaja IoT koncentrator uređaju MAC adresa. Zatim se objavljuje novu poruku koja sadrži adresu MAC ciljni uređaj na karti svojstva poruke.
4. Oblak Tablice uređaj modul odabire tu poruku i prevodi u odgovarajuće Tablice upute za modul Tablice. Zatim se objavljuje novu poruku.
5. Modul Tablice odabire tu poruku i izvršava/i uputa po komunikaciju s uređaja Tablice.
6. Modul zapisivaču prijavi svih poruka s brokera datoteka na disku.

## <a name="prepare-your-hardware"></a>Priprema hardvera

Pomoću ovog praktičnog vodiča pretpostavlja da koristite [Texas instrumenti SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) uređaj povezan Intel Edison ploču.

### <a name="set-up-the-edison-board"></a>Postavljanje ploče Edison

Prije nego što počnete, provjerite je li povezivanje uređaja Edison u bežičnoj mreži. Da biste postavili Edison uređaj, morate povezati s glavnim računalom. Intel omogućuje vodiči za sljedeći operacijski sustavi za početak rada:

- [Početak rada s ploče za razvoj Intel Edison na 64-bitni Windows][lnk-setup-win64].
- [Početak rada s ploče za razvoj Intel Edison u sustavu Windows 32-bitni][lnk-setup-win32].
- [Početak rada s ploče za razvoj Edison Intel operacijskom sustavu Mac OS X][lnk-setup-osx].
- [Početak rada s ploče Edison Intel® na Linux][lnk-setup-linux].

Da biste postavili uređaj Edison i Upoznajte se s njom, trebali biste dovršili sve korake iz sljedećeg "započnite rad" Članci osim posljednji korak, "Odaberite IDE," koji je na nepotrebne za trenutni vodič. Na kraju postupka za postavljanje Edison imate:

- Flashed vaše Edison s najnoviji firmver.
- Uspostaviti vezu serijski s vašeg računala da biste na Edison.
- Pokrenite **configure_edison** skriptu da biste postavili lozinku i omogućili Wi-Fi veze na svoje Edison.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Omogući povezivanje s uređajem SensorTag iz panel za Edison

Prije pokretanja uzorka, morate provjerite panel Edison možete povezati s uređajem SensorTag.

Najprije morate provjeriti vaše Edison možete povezati SensorTag uređaja.

1. Deblokiranje bluetooth na na Edison i provjerite je li broj verzije **5.37**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. Izvršavanje naredbe **bluetoothctl** . Sada su u ljusci za interaktivno bluetooth. 

3. Unesite naredbe **power na** potenciju gore kontrolerom bluetooth. Trebali biste vidjeti izlaz slično:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Ne napuštajući ljuske interaktivne bluetooth unesite na naredbu **pretražiti na** radi pronalaženja bluetooth uređaja. Trebali biste vidjeti izlaz slično:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Vidljivost SensorTag uređaj tako da pritisnete mali gumb (zelena LED treba flash). Na Edison treba otkrijte SensorTag uređaj:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    U ovom primjeru, vidjet ćete da je MAC adresa uređaja SensorTag **A0:E6:F8:B5:F6:00**.

6. Isključite mogućnost pregleda tako da unesete naredbu **skeniranje isključeno** .
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Povezivanje s uređajem SensorTag pomoću njegovu adresu MAC unosom **Povezivanje \<MAC adresa >**. Imajte na umu da rezultatu uzorka skraćen:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Napomena: Može prikazati osobina GATT uređaja za ponovno korištenje naredbe **atributi popisa** .

8. Možete odmah prekinuti vezu s uređaja pomoću naredbe za **Prekid veze** i izađite iz ljuske bluetooth pomoću naredbe za **Izlaz iz** :
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Sada ste spremni pokrenuti ogledne Tablice pristupnika na uređaju Edison.

## <a name="run-the-ble-gateway-sample"></a>Pokretanje ogledne Tablice pristupnika

Da biste pokrenuli ogledne Tablice na vašem Edison, morate dovršiti triju zadataka:

- Konfiguriranje dva uzorka uređaja u koncentratora za IoT.
- Stvaranje pristupnika SDK IoT na uređaju Edison.
- Konfiguriranje i pokrenite ogledne Tablice na uređaju Edison.

Prilikom pisanja pristupnika SDK IoT podržava samo pristupnika koje koriste module za Tablice na Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfiguriranje dva uzorka uređaja u koncentratora za IoT

- [Stvaranje koncentratora za IoT] [ lnk-create-hub] u pretplatu Azure ćete naziv vaše središte da biste dovršili ovaj prikaz. Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.
- Dodavanje naziva **SensorTag_01** vaše koncentrator IoT uređaja i zabilježite id i uređaja ključ. Možete koristiti na [uređaju Explorer ili iothub explorer] [ lnk-explorer-tools] alate za dodavanje ovaj uređaj u središtu IoT koji ste stvorili u prethodnom koraku i dohvatiti ključ. Ovaj uređaj će mapirati na uređaju SensorTag prilikom konfiguriranja pristupnika.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Stvaranje pristupnika SDK IoT na uređaju Edison

Verzija **brojka** na na Edsion ne podržava submodules. Da biste preuzeli punu izvora za pristupnik SDK IoT da biste na Edison imate dvije mogućnosti:

- Mogućnost #1: Kloniraj [Azure IoT pristupnika SDK] [ lnk-sdk] spremište na vašem Edison i ručno Kloniraj spremište za svaku submodule.
- Mogućnost #2: Kloniraj [Azure IoT pristupnika SDK] [ lnk-sdk] spremište na stolnom gdje **brojka** podržava submodules i zatim kopirajte dovršeno spremište s submodules na vašem Edison.

Ako odaberete mogućnost #2, pomoću sljedeće naredbe i **brojka** Kloniraj pristupnika SDK IoT i sve njegove submodules:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Zatim treba poštanski cijelu lokalnom spremištu u jednom arhivsku datoteku prije no što kopirate u Edison. Možete koristiti utility kao što su **pscp** koji se isporučuje se uz **Putty** da biste kopirali arhivsku datoteku na Edison. Ako, na primjer:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Kada imate cijelu kopiju IoT pristupnika SDK spremište na vašem Edison, možete stvoriti pomoću sljedeće naredbe iz mape koja sadrži SDK:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Konfiguriranje i pokretanje ogledne Tablice na uređaju Edison

Da biste se povezati i pokrenuti uzorka, morate konfigurirati svakom modulu koja sudjeluje u pristupnika. Tu konfiguraciju navedeni su u datoteci JSON, a morate konfigurirati pet koji sudjeluju module. Postoji JSON ogledne datoteke navedene u spremištu pod nazivom **gateway_sample.json** koje možete koristiti kao početnu točku za stvaranje konfiguracijskoj datoteci. Ta je datoteka u mapi **uzoraka/ble_gateway_hl/src** u lokalnu kopiju spremište SDK IoT pristupnika.

U sljedećim odjeljcima opisuju kako urediti datoteku konfiguracije ogledne Tablice i pretpostavlja spremište SDK pristupnika IoT se na **/home/root/azure-iot-gateway-sdk /** mapu na vašem uređaju Edison. Ako je spremište neko drugo mjesto, koje treba prilagođavaju putove:

#### <a name="logger-configuration"></a>Konfiguriranje zapisivaču

Spremište pristupnika uz pretpostavku da se nalazi u mapi **/home/root/azure-iot-gateway-sdk /**, konfiguriranje modula zapisivaču na sljedeći način:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Konfiguriranje modula Tablice

Konfiguriranje uzorka za uređaj Tablice pretpostavlja Texas instrumenti SensorTag uređaja. Standardni uređaj Tablice koje možete upravljati kako surađivati na GATT perifernih, no morat ćete ažuriranje GATT karakteristikama ID-a i podatke (uputama za pisanje). Dodajte adresu MAC SensorTag uređaj: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>Modul IoT koncentratora

Dodajte naziv koncentratora za IoT. Vrijednost sufiks je obično **azure devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Konfiguracija modul mapiranje za identitet

Dodajte MAC adresu SensorTag uređaj, a uređaj Id i ključ **SensorTag_01** uređaja koje ste dodali koncentratora za IoT:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Konfiguriranje modula pisač Tablice

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Usmjeravanje konfiguracije

Sljedeće konfiguraciju omogućuje sljedeće:
- Modul za **zapisivaču** prima i prijavite se sve poruke.
- Modul **SensorTag** šalje poruke **Mapiranje** i moduli **Tablice pisač** .
- Modul za **Mapiranje** šalje poruke modul **IoTHub** koncentratora za IoT slanje prema gore.
- Modul za **IoTHub** šalje poruke natrag modul **Mapiranje** .
- Modul za **Mapiranje** šalje poruke natrag u modulu **SensorTag** .

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Da biste pokrenuli uzorka pokrenuti na **ble_gateway_hl** binarni prosljeđivanje put u konfiguracijskoj datoteci JSON. Ako ste koristili **gateway_sample.json** datoteku, naredbe za izvršavanje izgleda ovako:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Možda ćete morati pritisnite mali gumb na uređaju SensorTag da biste ga vidljivim prije pokretanja uzorka.

Kada pokrenete uzorka, možete koristiti na [uređaju Explorer ili iothub explorer] [ lnk-explorer-tools] alat za praćenje poruka pristupnika prosljeđuje s uređaja SensorTag.

## <a name="send-cloud-to-device-messages"></a>Slanje poruka oblaka uređaja

Modul za Tablice podržava i slanje upute u središtu IoT Azure na uređaju. Možete koristiti [Azure IoT koncentrator uređaj Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) ili [IoT koncentrator Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) pošte JSON pristupnika modul Tablice prosljeđuje uređaju Tablice. Ako, na primjer, ako koristite uređaj Texas instrumenti SensorTag pa možete poslati sljedeće poruke JSON na uređaju iz centra za IoT.

- Vraćanje svih LEDs i znak zvona (ih isključiti)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- Konfiguriranje/i kao "alat za analizu daljinske"

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Uključivanje crvena LED

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Uključivanje zeleni LED

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Uključivanje u zvona

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Zadano ponašanje za uređaj pomoću protokola HTTP povezati koncentrator IoT je da biste provjerili svakih 25 minuta za novu naredbu. Stoga ako pošaljete nekoliko zasebnom naredbi ćete morati pričekati 25 minuta da biste primili svaku naredbu.

> [AZURE.NOTE] Pristupnik za nove naredbe također provjerava pri svakom pokretanju tako da obradi naredbu zaustavljanje i pokretanje pristupnika možete nametnuti.

## <a name="next-steps"></a>Daljnji koraci

Ako želite dobiti dodatne razumijevanja pristupnika SDK IoT i Eksperimentirajte s primjere koda, posjetite sljedeći vodiči za razvojne inženjere i resursima:

- [SDK Azure IoT pristupnika][lnk-sdk]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
