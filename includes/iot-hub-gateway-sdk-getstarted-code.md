## <a name="typical-output"></a>Tipične Izlaz

Ispod je primjer izlaza uzorak Hello svijeta napisao datoteku zapisnika. Za čitljivost su dodane čuva i karticu znakova:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Komadići koda

Ovaj odjeljak opisuje neki dijelovi koda u uzorku Hello svijeta ključa.

### <a name="gateway-creation"></a>Stvaranje pristupnika

Programer mora napisati *proces pristupnika*. Ovaj program stvara Interna Infrastruktura (broker), učitava module i postavlja sve ispravno funkcioniranje. SDK pruža funkciju **Gateway_Create_From_JSON** za omogućavanje povezati pristupnik iz datoteke JSON. Koristiti funkciju **Gateway_Create_From_JSON** koje mora proći ga put JSON datoteku koja određuje module učitati. 

Pronalaženje koda za pristupnik proces u uzorku Hello svijet u [main.c] [ lnk-main-c] datoteku. Za čitljivost, isječak ispod prikazuje skraćeni verziju Šifra postupka pristupnika. Ovaj program stvara pristupnik i čeka korisnički tipku **ENTER** prije JScriptom pristupnika. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Datoteka postavki JSON sadrži popis module učitati. Svaki modul morate navesti a:

- **module_name**: jedinstveni naziv modula.
- **module_path**: put do biblioteke koja sadrži modul. Za Linux Ovo je .so datoteku, Windows to je .dll datoteke.
- **argumente**: sve konfiguracijske informacije treba modul.

Datoteka JSON sadrži i veze između moduli koji će se proslijediti brokera. Veza ima dva svojstva:
- **izvor**: naziv modula iz u `modules` sekciji, ili "\*".
- **Sito**: naziv modula iz u `modules` sekciji

Svaka veza definira Usmjeri poruku i smjer. Poruke iz modula `source` će se dostavljati u modulu `sink`. U `source` možda postavljena na "\*", koja označava da se poruke iz bilo kojeg modula primljene putem `sink`.

Sljedeći primjer prikazuje JSON postavke datoteka koja se koristi za konfiguriranje uzorak Hello svijeta na Linux. Producent svake poruke u modulu `hello_world` će potrošenih modul `logger`. Hoće li modul zahtijeva argument ovisi o dizajnu modul. U ovom primjeru modul zapisivaču uzima argument koji je put izlazne datoteke i modul Hello svijeta poduzeti sve argumente:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Pozdrav objavljivanje poruka svijeta modul

Možete pronaći kod koji koristi modul "Pozdrav svijetu" za objavljivanje poruka u ['hello_world.c'] [ lnk-helloworld-c] datoteku. Isječak ispod prikazuje izmeni verziju s dodatne komentare i neke pogreške kod ukloniti čitljivost za rukovanje:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Pozdrav obrade poruke svijeta modul

Modul Hello svijeta nikad ne treba obraditi poruke koje druge module objaviti brokera. Ovime implementaciju povratnog poziva poruku u modulu Hello svijeta funkcija ne op.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Zapisivaču poruku modul objavljivanje i obrade

Modul zapisivaču Prima poruke iz brokera i zapisuje ih u datoteku. Nikad ne objavljuje sve poruke. Stoga, kod modul zapisivaču nikad poziva funkciju **Broker_Publish** .

Funkcija **Logger_Recieve** u [logger.c] [ lnk-logger-c] datoteka je povratnog poziva brokera pozove poruke dostavljaju u modulu zapisivaču. Isječak ispod prikazuje izmeni verziju s dodatne komentare i neke pogreške kod ukloniti čitljivost za rukovanje:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Sljedeći koraci

Da biste saznali kako koristiti pristupnik SDK IoT, pogledajte sljedeće:

- [IoT pristupnik SDK – slanje poruka oblak uređaj s simuliranog uređaj pomoću Linux][lnk-gateway-simulated].
- [Pristupnik Azure IoT SDK] [ lnk-gateway-sdk] na GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md