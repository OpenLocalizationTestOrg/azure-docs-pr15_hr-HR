<properties
   pageTitle="Priključite uređaj pomoću C na mbed | Microsoft Azure"
   description="U članku se opisuje priključite uređaj Azure IoT paket unaprijed konfigurirane udaljene nadzora rješenje pomoću aplikacije pisane C koji se izvode na uređaju sa sustavom mbed."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Povezivanje uređaja udaljene nadzora konfiguriranog rješenje (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Stvaranje i pokretanje uzorak rješenja C

Sljedeće upute opisuju korake za povezivanje [s omogućenim mbed FRDM-K64F Freescale] [ lnk-mbed-home] uređaj udaljene nadzora rješenje.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Priključite uređaj mbed na računalu mreža i stolna računala

1. Mbed povežete s mrežom pomoću Ethernet kabela. Ovaj korak nije potrebna jer primjer aplikacije potreban je pristup Internetu.

2. Potražite u članku [Uvod u mbed] [ lnk-mbed-getstarted] povezati mbed uređaj na računalo za stolna računala.

3. Ako stolno računalo koristi operacijski sustav Windows, pročitajte članak [Konfiguracije računala] [ lnk-mbed-pcconnect] da biste konfigurirali serijski priključak pristup mbed uređaj.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Stvaranje projekta programa mbed i uvoz uzorak koda

1. U web-pregledniku idite na mbed.org [web-mjesta za razvojne inženjere](https://developer.mbed.org/). Ako još niste prijavili, vidjet ćete mogućnost da biste stvorili račun (to je besplatno). U suprotnom, prijavite se pomoću vjerodajnica za račun. Zatim **prevoditelj (Compiler)** u gornjem desnom kutu stranice. Ova akcija poziva sučelja *radnog prostora* .

2. Provjerite je li hardverska platforma koristite pojavljuje se u gornjem desnom kutu prozora programa ili kliknite ikonu u desnom kutu da biste odabrali hardverska platforma.

3. Kliknite **Uvezi** na glavni izbornik. Zatim **kliknite ovdje** da biste uvezli iz URL vezu pokraj odjeljka globusa logotip mbed.

    ![][6]

4. U skočnom prozoru unesite vezu za https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ kod uzorka, a zatim kliknite **Uvezi**.

    ![][7]

5. Možete pogledati u prozoru alata za Kompiliranje mbed da uvoz taj projekt također uvozi različite biblioteke. Neke su navedeni i održava tima Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), a neke treće strane biblioteke dostupne u katalogu mbed biblioteke.

    ![][8]

6. Otvorite datoteku remote_monitoring\remote_monitoring.c, a zatim pronađite sljedeći kod u datoteci:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Zamijeni [Id uređaja] i [ključ uređaja] s podacima uređaj da biste omogućili program uzorka za povezivanje s koncentratora za IoT. Korištenje IoT koncentrator naziv glavnog računala da biste zamijenili [naziv IoTHub] i [sufiks IoTHub, odnosno azure devices.net] rezerviranih mjesta. Ako, na primjer, ako je IoT koncentrator naziv glavnog računala **contoso.azure devices.net**, **contoso** je **hubName** i sve kada je **hubSuffix**:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Prođite kroz kod

Ako vas zanima funkcioniranje program, ovom se odjeljku opisuju neka glavne dijelove uzorak koda. Ako samo želite pokrenuti kod, prijeđite omogućuje [Stvaranje i pokretanje programa](#buildandrun).

#### <a name="defining-the-model"></a>Definiranje modela

Ovaj primjer koristi [serijalizatora] [ lnk-serializer] biblioteke da biste definirali modelu koji određuje poruke uređaj mogli IoT koncentrator primati i slati iz centra za IoT. U ovom primjeru **Contoso** naziva definira **Thermostat** modelu koji određuje telemetrijskih podataka **Temperature**, **ExternalTemperature**i **Humidity** zajedno s metapodacima kao što je id uređaja, svojstva uređaja i naredbe Odgovori na uređaju:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Vezane uz definiciju modela su definicije za **SetTemperature** i **SetHumidity** naredbe Odgovori na uređaju:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Povezivanje modela u biblioteku

Funkcija **Otprema** i **IoTHubMessage** su predloženog kod za slanje telemetrijskih s uređaja i povezati poruke iz centra za IoT rukovatelja naredbe.

#### <a name="the-remotemonitoringrun-function"></a>Funkcija remote_monitoring_run

**Glavna** funkcija programa poziva funkciju **remote_monitoring_run** prilikom pokretanja aplikacije za izvršavanje ponašanje uređaja kao klijent za IoT koncentrator uređaja. Ova funkcija **remote_monitoring_run** uglavnom sastoji se od parove ugniježđene funkcije:

- **platforme\_init** i **platformu\_deinit** izvođenje operacija ovisne pokretanje i zatvaranje.
- **serijalizatora\_init** i **serijalizatora\_deinit** inicijalizaciju i poništite Inicijalizacija serijalizatora biblioteke.
- **IoTHubClient\_stvaranje** i **IoTHubClient\_uništenje** stvaranje ručicu za klijenta, **iotHubClientHandle**, pomoću vjerodajnica za uređaj za povezivanje koncentratora za IoT.

U odjeljku glavni funkcije **remote_monitoring_run** program izvršava sljedeće postupke Upotreba ručice za **iotHubClientHandle** :

- Stvara instancu thermostat modela Contoso i postavlja pozive poruke za dvije naredbe.
- Šalje informacije o uređaju, uključujući naredbe podržava, vaše IoT koncentrator pomoću serijalizatora biblioteke. Kada središtu primi ovu poruku, mijenja status uređaja na nadzornoj ploči iz na **čekanju** za **pokretanje**.
- Pokreće Ponavljaj **dok** koje šalje temperature, vanjski temperature i vrijednosti humidity IoT koncentrator sekundi.

Za referencu, Evo ogledne **DeviceInfo** poruke poslane koncentrator IoT prilikom pokretanja:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Za referencu, Evo ogledne **Telemetrijskih** poruke poslane koncentrator IoT:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Za referencu, Evo **naredbe** poslao koncentrator IoT:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Stvaranje i pokretanje programa

1. Kliknite **Prevedi** da biste sastavili program. Možete sigurno zanemariti sva upozorenja, no ako sastavljanje generira pogreške, njihovo rješavanje prije nastavka.

2. Ako sastavljanje ne uspije, web-mjesto alata za Kompiliranje mbed generira .bin datoteku pod nazivom projekta i preuzimanja na lokalno računalo. Kopirajte datoteku .bin na uređaju. Spremanje datoteke .bin na uređaj uzrokuje uređaj te ponovno pokrenite program koji se nalazi u datoteci .bin. Ručno ponovno pokrenuti program u bilo kojem trenutku pritiskom na gumb Vrati izvorno na uređaju mbed.

3. Povežite se s uređaja koji koristi se SSH klijentska aplikacija, kao što su PuTTY. Možete odrediti serijski priključak uređaju koristi potvrđivanjem Upravitelj uređaja za Windows.

    ![][11]

4. U PuTTY, kliknite **serijski** vrstu veze. Uređaj obično se povezuje pri 9600 brzina, pa unesite 9600 u okviru **brzinu** . Zatim kliknite **Otvori**.

5. Prilikom izvršavanja pokretanja programa. Možda ćete morati ponovno postavljanje ploče (pritisnite CTRL + prijelom ili pritisnite gumb za ponovno na panel) ako program ne pokreće automatski kada povežete.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
