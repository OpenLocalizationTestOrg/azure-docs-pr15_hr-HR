<properties
   pageTitle="Priključite uređaj pomoću C na Linux | Microsoft Azure"
   description="U članku se opisuje priključite uređaj Azure IoT paket unaprijed konfigurirane udaljene nadzora rješenje pomoću aplikacije pisane C sustavom Linux."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Povezivanje uređaja udaljene nadzora konfiguriranog rješenje (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Stvaranje i pokretanje klijenta uzorka C Linux

Sljedeći postupci objašnjavaju kako stvoriti klijentska aplikacija, pisan C i ugrađena i pokrenuti Ubuntu Linux koji komunicira s udaljenom nadzora konfiguriranog rješenja. Da biste dovršili ove korake, morate na uređaju pokrenut Ubuntu verzije 15.04 ili 15.10. Prije nego što nastavite, instalirajte pripremni paketa na uređaju Ubuntu pomoću sljedeće naredbe:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Instalacija klijentskih biblioteke na uređaju

Klijent biblioteke Azure IoT koncentrator su dostupni kao paket možete instalirati na uređaju Ubuntu pomoću **Zemaljska get** -command. Izvršite sljedeće korake da biste instalirali paket koji sadrži IoT koncentrator klijenta biblioteke i zaglavlja datoteke na vašem računalu Ubuntu:

1. Dodajte AzureIoT spremište na računalu:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Instalirajte paket azure-iot-sdk-c-razvojni

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Dodavanje koda za određivanje ponašanje uređaja

Na računalu Ubuntu stvorite mapu naziva **udaljene\_nadzor**. U na **udaljene\_nadzor** mapu stvaranje četiri datoteke **main.c** **udaljene\_monitoring.c**, **udaljene\_monitoring.h**, i **CMakeLists.txt**.

Biblioteka za klijent IoT koncentrator serijalizatora pomoću modela da biste odredili oblika poruka uređaj šalje IoT koncentratora i naredbe primi iz centra za IoT.

1. U uređivaču teksta otvorite na **udaljenom\_monitoring.c** datoteku. Dodajte sljedeće `#include` izvješća:

    ```
    #include "iothubtransportamqp.h"
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
    static const char* hubName = "IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Dodajte sljedeći kod da biste definirali modela koji omogućuje uređaja možete komunicirati s IoT koncentratora. Ovaj model određuje je li uređaj šalje temperature, vanjski temperatura, humidity i id uređaja kao telemetrijskih. Uređaj šalje i metapodataka o uređaju IoT koncentrator, uključujući popis naredbi koje podržava uređaj. Ovaj uređaj odgovori naredbama **SetTemperature** i **SetHumidity**:

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

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Dodavanje koda za implementaciju ponašanje uređaja

Dodavanje funkcija izvršiti kada uređaj prima naredbe iz koncentratora i kod da biste poslali Simulirani telemetrijskih središtu.

1. Dodajte sljedeće funkcije koje se izvršiti kada uređaj primi **SetTemperature** i **SetHumidity** naredbe određene u modelu:

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
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
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
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
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
        platform_deinit();
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

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>Dodavanje koda za pozivanje remote_monitoring_run (funkcija)

U uređivaču teksta otvorite datoteku **remote_monitoring.h** . Dodavanje koda za sljedeće:

```
void remote_monitoring_run(void);
```

U uređivaču teksta otvorite datoteku **main.c** . Dodavanje koda za sljedeće:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>Kako koristiti CMake za izradu klijentska aplikacija

Sljedeći koraci opisuju kako koristiti *CMake* da biste sastavili klijentska aplikacija.

1. U uređivaču teksta otvorite datoteku **CMakeLists.txt** u mapi **remote_monitoring** .

2. Dodajte sljedeće upute da biste odredili način stvaranja klijentska aplikacija:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. U mapi **remote_monitoring** , stvorite mapu u koju želite pohraniti datoteke *provjerite* koje generira CMake, a zatim pokrenite naredbe **cmake** i **učiniti** na sljedeći način:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Pokrenite aplikaciju za klijenta i pošaljite telemetrijskih IoT koncentrator:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

