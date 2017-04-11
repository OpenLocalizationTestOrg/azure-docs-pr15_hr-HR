<properties
    pageTitle="Početak rada s SDK pristupnika koncentrator za IoT | Microsoft Azure"
    description="Azure IoT pristupnika SDK prođite kroz sustavu Windows da biste ilustrirali koncepti upoznati prilikom korištenja pristupnika SDK za Azure IoT."
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
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT pristupnika SDK (beta) – započeti rad sa sustavom Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Sastavljanje uzorka

Prije nego što počnete, morate [postaviti razvojno okruženje] [ lnk-setupdevbox] za rad s SDK-a u sustavu Windows.

1. Otvorite **naredbeni redak za razvojne inženjere za VS2015** naredbeni redak.
2. Dođite do korijenske mape u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu.
3. Pokretanje sustava **Alati\\build.cmd** skripte. Ova skripta stvara datoteku rješenja za Visual Studio, sastavlja rješenje i pokreće testova. Rješenja za Visual Studio možete pronaći u mapi **Sastavljanje** u lokalnoj kopiji **azure-iot-pristupnika-sdk** spremištu.

## <a name="how-to-run-the-sample"></a>Pokretanje uzorka

1. Skripta **build.cmd** stvara mapu pod nazivom **Sastavljanje** u lokalnoj kopiji u spremištu. Ova mapa sadrži dvije module koji se koristi u ovom primjeru.

    Skripta Sastavi smješta **logger_hl.dll** u na **Sastavljanje\\modula\\zapisivaču\\ispravljanje pogrešaka** mapu i **hello_world_hl.dll** u na **Sastavljanje\\module\\hello_world\\ispravljanje pogrešaka** mapu. Pomoću ovih za vrijednost **Modul put** kao što je prikazano u nastavku JSON postavke datoteci.

2. Datoteka **hello_world_win.json** u na **uzoraka\\hello_world\\src** mapa je datoteka postavki JSON primjer za Windows koje možete koristiti da biste pokrenuli uzorka. Postavke JSON primjeru prikazano u nastavku pretpostavlja da klonirana spremište IoT pristupnika SDK korijenski pogon **C:** . Ako je preuzeti na drugo mjesto, morate prilagoditi **Modul put** vrijednosti u na **uzoraka\\hello_world\\src\\hello_world_win.json** datoteke sukladno tome.

3. Modul **logger_hl** u odjeljku **argumenata** **filename** vrijednost postavite na naziv i put datoteke u kojima se nalaze podaci zapisnika.

    Ovo je primjer datoteke JSON postavke za Windows koje ćete pisati u datoteku **log.txt** u korijenskoj mapi na pogon **C:** .

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. U naredbenom retku, dođite do korijenske mape lokalnu kopiju spremište **azure-iot-pristupnika-sdk** .
4. Pokrenite sljedeću naredbu:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md