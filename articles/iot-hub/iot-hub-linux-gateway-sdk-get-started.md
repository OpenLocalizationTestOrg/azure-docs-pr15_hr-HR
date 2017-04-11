<properties
    pageTitle="Početak rada s SDK pristupnika koncentrator za IoT | Microsoft Azure"
    description="Ovaj vodič Azure IoT pristupnika SDK koristi Linux da bi ilustrirala koncepata upoznati kada koristite pristupnika SDK za Azure IoT."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT pristupnika SDK (beta) – prvi koraci pri korištenju Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Sastavljanje uzorka

Prije nego što počnete, morate [postaviti razvojno okruženje] [ lnk-setupdevbox] za rad s SDK na Linux.

1. Otvorite u ljusci.
2. Dođite do korijenske mape u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu.
3. Pokrenuti skriptu **tools/build.sh** . Ova skripta koristi uslužni **cmake** da biste stvorili mapu s nazivom **Sastavljanje** u korijenskoj mapi lokalnu kopiju spremište **azure-iot-pristupnika-sdk** i generiranje na makefile. Skripta pa sastavlja rješenje i pokreće testova.

> [AZURE.NOTE]  Svaki put kada pokrenete skriptu **build.sh** , briše i obnavlja mapu **Sastavljanje** u korijenskoj mapi lokalnu kopiju **azure-iot-pristupnika-sdk** spremište.

## <a name="how-to-run-the-sample"></a>Pokretanje uzorka

1. Skripta **build.sh** generira rezultat u mapi **Sastavljanje** u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu. To obuhvaća dvije module koji se koristi u ovom primjeru.

    Skripta Sastavi smješta **liblogger_hl.so** u na **moduli/Sastavi/zapisivaču/** mapu i **libhello_world_hl.so** u na **moduli/Sastavi/hello_world/** mapu. Pomoću ovih za vrijednost **Modul put** kao što je prikazano u nastavku JSON postavke datoteci.

2. Datoteka **hello_world_lin.json** u mapi **uzoraka/hello_world/src** je datoteka postavki JSON primjer za Linux koje možete koristiti da biste pokrenuli uzorka. Postavke JSON primjeru prikazano u nastavku pretpostavlja da će pokrenuti uzorka u korijenskom direktoriju lokalnu kopiju **azure-iot-pristupnika-sdk** spremište.

3. Modul **logger_hl** u odjeljku **argumenata** **filename** vrijednost postavite na naziv i put datoteke u kojima se nalaze podaci zapisnika.

    Ovo je primjer datoteke postavke JSON Linux koje ćete pisati **log.txt** u mapu koju pokrenete uzorka.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. U ljusci, dođite do mape **azure-iot-pristupnika-sdk** .
4. Pokrenite sljedeću naredbu:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
