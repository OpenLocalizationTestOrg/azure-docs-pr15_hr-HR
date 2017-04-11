<properties
 pageTitle="Pokrenite aplikaciju uzorka primati poruke oblak na uređaju | Microsoft Azure"
 description="Primjer aplikacije u 4 nastave izvršava na vašem Pi i nadzire dolazne poruke iz centra za IoT. Novi zadatak gulp šalje poruke vašeg Pi iz centra za IoT da biste titranja na LED."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 pokrenuti aplikaciju uzorka primati poruke oblaka uređaja

U ovom ćete odjeljku implementirati probna aplikacija na vašem Raspberry Pi 3. Primjer aplikacije nadzire dolazne poruke iz centra za IoT. Pokrenite gulp zadatka na računalo da biste mogli slati poruke u vašem Pi iz koncentratora za IoT. Nakon primanja poruka primjer aplikacije titranja na LED. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="411-what-you-will-do"></a>4.1.1 što ćete učiniti

- Povežite aplikaciju uzorka s koncentratora za IoT.
- Uvođenje i pokrenuti aplikaciju uzorka.
- Slanje poruka iz centra za IoT vaše Pi da biste titranja na LED.

## <a name="412-what-you-will-learn"></a>4.1.2 što ćete saznati

- Upute za praćenje dolazne poruke iz centra za IoT.
- Upute za slanje poruka oblaka uređaj iz centra za IoT vaše Pi. 

## <a name="413-what-do-you-need"></a>4.1.3 što vam je potrebno

- 3 Pi Raspberry koja je postavljena za korištenje. Da biste saznali kako postaviti svoje Pi, potražite u članku [nastave 1: početak rada s uređajem Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
- Koncentrator IoT koja je stvorena u pretplatu za Azure. Da biste saznali kako stvoriti koncentratora za IoT Azure, potražite u članku [nastave 2: Stvaranje koncentratora za IoT Azure](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 povezivanje aplikacije uzorka koncentratora za IoT

1. Provjerite jeste li vi u mapi repo `iot-hub-node-raspberrypi-getting-started`. Otvorite aplikaciju uzorak u Visual Studio kod ponovnim pokretanjem sljedeće naredbe:

    ```bash
    cd Lesson4
    code .
    ```

    Obavijest na `app.js` datoteka u na `app` podmape. Na `app.js` datoteka je ključa izvornu datoteku koja sadrži kod praćenje dolazne poruke iz centra za IoT. Na `blinkLED` funkcija titranja na LED.

    ![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Pokretanje konfiguracijska datoteka pomoću sljedeće naredbe:

    ```bash
    npm install
    gulp init
    ```

    Ako ste dovršili nastave 3 na ovom računalu, sve konfiguracije nasljeđuju, pa možete preskočiti korak 4.1.5. Ako dovršili nastave 3 na drugo računalo, morate zamijeniti rezerviranih mjesta u na `config-raspberrypi.json` datoteku. Na `config-raspberrypi.json` datoteka se nalazi u podmapu polazne mape.

    ![Konfiguracija](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- Zamijenite **[naziv glavnog računala uređaja ili IP adresa]** IP adresa ili naziv glavnog računala koji se isporučuju tako da pokrenete naredbu vaše Pi`devdisco list --eth`
- **[IoT uređaj niz za povezivanje]** zamijenite niz za povezivanje uređaja koji se isporučuju tako da pokrenete naredbu `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- **[IoT koncentrator niz za povezivanje]** zamijenite niz za povezivanje koncentrator IoT koji se isporučuju tako da pokrenete naredbu `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 uvođenje i pokrenuti aplikaciju uzorka

Implementacija i pokrenuti aplikaciju uzorka vaše Pi ponovnim pokretanjem sljedeće naredbe:
  
```
gulp
```

Naredba gulp prvo pokreće zadatak Alati za instalaciju. Zatim uvodi primjer aplikacije za vaše Pi. Na kraju, pokrenuti aplikaciju na vaše Pi i zasebnom zadatka na glavnom računalu za slanje poruka 20 titranja vaše Pi iz centra za IoT.

Nakon izvođenja primjer aplikacije, započinje slušanje poruke iz centra za IoT. U međuvremenu, zadatak gulp šalje nekoliko poruka "titranja" iz centra za IoT vaše Pi. Za sve primljene poruke titranja primjer aplikacije poziva funkciju blinkLED za titranja na LED.

Trebali biste vidjeti LED titranje svake dvije sekunde kao zadatka gulp šalje 20 poruke iz centra za IoT vaše Pi. Posljednje je "Zaustavi" poruku koja obavještava aplikacije zaustavljate.

![Gulp](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 sažetak

Uspješno ste poslali poruke iz centra za IoT vaše Pi da biste titranja na LED. Sljedeći odjeljak je neobavezna sekcija koji pokazuje kako promijeniti i isključenog ponašanje u LED.

## <a name="next-steps"></a>Daljnji koraci

[Neobavezna sekcija: promjena i isključenog ponašanje u LED](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)