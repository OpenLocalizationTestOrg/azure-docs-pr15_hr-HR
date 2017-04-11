<properties
   pageTitle="Priključite uređaj pomoću C u sustavu Windows | Microsoft Azure"
   description="U članku se opisuje priključite uređaj Azure IoT paket unaprijed konfigurirane udaljene nadzora rješenje pomoću aplikacije pisane C sustavom Windows."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Povezivanje uređaja udaljene nadzora konfiguriranog rješenje (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Stvaranje rješenja uzorka C u sustavu Windows

Sljedeći koraci pokazati kako pomoću programa Visual Studio stvaranje klijentska aplikacija pisane C koja vas obavještava rješenjem unaprijed konfigurirane udaljene nadzor.

Stvaranje projekta starter u Visual Studio 2015 i dodajte NuGet pakete koncentrator IoT uređaj klijent:

1. U Visual Studio 2015, stvaranje aplikacije konzole C pomoću predloška za Visual C++ **Win32 aplikacija konzolu** . Naziv projekta **RMDevice**.

2. Na stranici **Postavke za aplikacije** u **Čarobnjak aplikacije za Win32**, provjerite je li potvrđen **aplikacije konzole** i poništite potvrdni okvir **Precompiled zaglavlja** i **Provjera životni ciklus razvoja na sigurnost (SDL)**.

3. U **Pregledniku rješenja**izbrišite datoteke stdafx.h, targetver.h i stdafx.cpp.

4. U **Pregledniku rješenja**preimenujte datoteku RMDevice.cpp RMDevice.c.

5. U **Pregledniku rješenja**, desnom tipkom miša kliknite na projektu **RMDevice** , a zatim kliknite **Upravljanje NuGet paketa**. Kliknite **Pregledaj**, a zatim potražite i instalirati sljedeće NuGet paketa u projekt:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. U **Pregledniku rješenja**, desnom tipkom miša kliknite na projektu **RMDevice** , a zatim kliknite **Svojstva** da biste otvorili dijaloški okvir **Svojstva stranica** za projekta. Detalje potražite u članku [Postavljanje Visual C++ projekta svojstava][lnk-c-project-properties]. 

7. Kliknite mapu **Povezivač** , a zatim kliknite stranica svojstava za **unos** .

8. Dodajte **crypt32.lib** svojstvo **Dodatne zavisnosti** . Kliknite **u redu** , a zatim **u redu** ponovno da biste spremili projekt vrijednosti svojstva.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Odredite ponašanje uređaj IoT koncentratora

Biblioteka klijentski IoT koncentrator model možete koristiti da biste odredili oblika poruka uređaj šalje IoT koncentratora i naredbe primi iz centra za IoT.

1. U Visual Studio, otvorite datoteku RMDevice.c. Zamijeniti postojeći `#include` izjave s sljedeći kod:

    ```
    #include "iothubtransporthttp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Dodajte sljedeće varijable deklaracija nakon na `#include` izvješća. Zamjena vrijednosti rezervirano mjesto [Id uređaja] i [ključ uređaja] s vrijednostima za svoj uređaj s udaljenog nadzora rješenje nadzorne ploče. Da biste zamijenili [naziv IoTHub] pomoću IoT koncentrator glavnog računala na nadzornoj ploči. Na primjer, ako je IoT koncentrator naziv glavnog računala **contoso.azure devices.net**, zamijenite [naziv IoTHub] **contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Dodajte sljedeći kod da biste definirali modela koji omogućuje uređaja možete komunicirati s IoT koncentratora. Ovaj model određuje je li uređaj šalje temperature, vanjski temperatura, humidity i id uređaja kao telemetriju. Uređaj šalje i metapodataka o uređaju IoT koncentrator, uključujući popis naredbi koje podržava uređaj. Ovaj uređaj odgovori naredbama **SetTemperature** i **SetHumidity**:

    ```
    // Define the Model
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

## <a name="implement-the-behavior-of-the-device"></a>Implementacija ponašanje uređaja

Sada dodati kod koji implementira ponašanje definirane u modelu.

1. Dodajte sljedeće funkcije koje se izvršiti kada uređaj prima **SetTemperature** i **SetHumidity** naredbe iz koncentratora IoT:

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

2. Dodajte sljedeće funkcija koja se šalje poruku IoT koncentrator:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```

3. Dodajte sljedeća funkcija koje spojnica serijalizacije biblioteke u SDK:

    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```

4. Dodajte sljedeća funkcija povezivanje IoT koncentrator, slanje i primanje poruka i prekid središtu. Obratite pozornost na to kako na uređaju šalje metapodataka o sebi, uključujući naredbe podržava, IoT koncentrator kada se povezuje. U ovom metapodataka omogućuje rješenja da biste ažurirali status uređaja **radi** na nadzornoj ploči:

    ```
    void remote_monitoring_run(void)
    {
      if (serializer_init(NULL) != SERIALIZER_OK)
      {
        printf("Failed on serializer_init\r\n");
      }
      else
      {
        IOTHUB_CLIENT_CONFIG config;
        IOTHUB_CLIENT_HANDLE iotHubClientHandle;

        config.deviceId = deviceId;
        config.deviceKey = deviceKey;
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
        iotHubClientHandle = IoTHubClient_Create(&config);
        if (iotHubClientHandle == NULL)
        {
          (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
        }
        else
        {
          Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
          if (thermostat == NULL)
          {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
          }
          else
          {
            STRING_HANDLE commandsMetadata;

            if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
            {
              printf("unable to IoTHubClient_SetMessageCallback\r\n");
            }
            else
            {

              /* send the device info upon startup so that the cloud app knows
              what commands are available and the fact that the device is up */
              thermostat->ObjectType = "DeviceInfo";
              thermostat->IsSimulatedDevice = false;
              thermostat->Version = "1.0";
              thermostat->DeviceProperties.HubEnabledState = true;
              thermostat->DeviceProperties.DeviceID = (char*)deviceId;

              commandsMetadata = STRING_new();
              if (commandsMetadata == NULL)
              {
                (void)printf("Failed on creating string for commands metadata\r\n");
              }
              else
              {
                /* Serialize the commands metadata as a JSON string before sending */
                if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                {
                  (void)printf("Failed serializing commands metadata\r\n");
                }
                else
                {
                  unsigned char* buffer;
                  size_t bufferSize;
                  thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                  /* Here is the actual send of the Device Info */
                  if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed serializing\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                }

                STRING_delete(commandsMetadata);
              }

              thermostat->Temperature = 50;
              thermostat->ExternalTemperature = 55;
              thermostat->Humidity = 50;
              thermostat->DeviceId = (char*)deviceId;

              while (1)
              {
                unsigned char*buffer;
                size_t bufferSize;

                (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                {
                  (void)printf("Failed sending sensor value\r\n");
                }
                else
                {
                  sendMessage(iotHubClientHandle, buffer, bufferSize);
                }

                ThreadAPI_Sleep(1000);
              }
            }

            DESTROY_MODEL_INSTANCE(thermostat);
          }
          IoTHubClient_Destroy(iotHubClientHandle);
        }
        serializer_deinit();
      }
    }
    ```
    
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

5. **Glavna** funkcija zamijenite sljedeći kod za pozivanje funkciju **remote_monitoring_run** :

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Kliknite **Stvaranje** , a zatim **Stvaranje rješenja** da biste sastavili aplikacije uređaja.

7. U **Pregledniku rješenja**, desnom tipkom miša kliknite **RMDevice** projekta, kliknite **ispravljanje pogrešaka**, a zatim **pokrenite novu instancu** da biste pokrenuli uzorka. Na konzoli prikazuje poruke aplikacija šalje uzorka telemetriju IoT koncentrator, a prima naredbe.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx