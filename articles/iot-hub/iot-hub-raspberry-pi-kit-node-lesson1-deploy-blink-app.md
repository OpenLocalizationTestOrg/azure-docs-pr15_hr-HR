<properties
 pageTitle="Stvaranje i implementaciju aplikacija za titranja | Microsoft Azure"
 description="Kloniraj aplikacije Node.js uzorka Github, a gulp za implementaciju ovu aplikaciju panel Raspberry Pi 3. U ovom primjeru aplikacije titranja LED povezan s ploče svake dvije sekunde."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 stvaranje i implementaciju aplikacija za titranja

## <a name="131-what-you-will-do"></a>1.3.1 što ćete učiniti

Kloniraj aplikacije Node.js uzorka Github, a pomoću gulp alata za implementaciju aplikacije uzorka Pi 3-a Raspberry. Primjer aplikacije titranja LED povezan s ploče svake dvije sekunde. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="132-what-you-will-learn"></a>1.3.2 što ćete saznati

- Upute za korištenje na `device-discover-cli` alat za dohvaćanje umrežavanje informacija o vašem Pi.
- Kako implementirati i izvršavaju primjer aplikacije na vašem Pi.
- Kako implementirati i ispravljanje pogrešaka aplikacije daljinski izvode na vašem Pi.

## <a name="133-what-you-need"></a>1.3.3 što vam je potrebno

Morate uspješno ste dovršili prati odjeljka nastave 1:

- [Konfiguriranje uređaja](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [Početak alata](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 dohvatiti IP adresa i glavno računalo naziv vaše Pi

Otvorite naredbeni redak u sustavu Windows ili terminal prozora u Mac OS ili Ubuntu, a zatim pokrenite sljedeću naredbu:

```bash
devdisco list --eth
```

Trebali biste vidjeti na Izlaz koja je slična sljedećoj:

![otkrivanje uređaja](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Obratite pažnju na `IP address` i `hostname` vaše pi. Morate te informacije u nastavku odjeljka.

> [AZURE.NOTE] Provjerite je li vaša Pi povezan s istom mrežom kao i računalo. Ako, na primjer, ako je vaše računalo povezano s bežičnom mrežom dok vaše Pi priključen u ožičenoj vezi, možda nećete vidjeti IP adresa u rezultatu devdisco.

## <a name="135-clone-the-sample-application"></a>1.3.5 Kloniraj primjer aplikacije

Da biste otvorili uzorak koda, slijedite ove korake:

1. Kloniraj spremište uzorak iz Github ponovnim pokretanjem sljedeće naredbe:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Otvorite aplikaciju uzorak u Visual Studio kod ponovnim pokretanjem sljedeće naredbe:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

Na `app.js` datoteka u na `app` podmapu je ključa izvornu datoteku koja sadrži kod kontrolu na LED.

### <a name="136-install-application-dependencies"></a>1.3.6 instalirati aplikaciju ovisnosti

Instalirajte biblioteke i druge module potrebne za aplikaciju uzorka pokretanjem sljedeće naredbe:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 konfigurirati vezu uređaja

Da biste konfigurirali vezu uređaja, slijedite ove korake:

1. Generiranje konfiguracijska datoteka uređaj tako da pokrenete sljedeću naredbu:

    ```bash
    gulp init
    ```

    Konfiguracijska datoteka `config-raspberrypi.json` sadrži korisničke vjerodajnice koju koristite za prijavu u vašem Pi. Da biste izbjegli osipanja korisničke vjerodajnice, generira se datoteka konfiguracije u podmapi `.iot-hub-getting-started` od polazne mape na računalu.

2. Otvorite datoteku konfiguracije uređaja u Visual Studio kod ponovnim pokretanjem sljedeće naredbe:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Zamjena rezerviranog mjesta `[device hostname or IP address]` IP adresa ili naziv glavnog računala koji se isporučuju u odjeljku 1.3.4.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Čestitamo! Uspješno ste stvorili prvi primjer aplikacije za vaše Pi.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 uvođenje i pokrenuti aplikaciju uzorka

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 Node.js i instalirati NPM na vašem Pi

Instalirajte Node.js i NPM na vašem Pi ponovnim pokretanjem sljedeće naredbe:

```bash
gulp install-tools
```

Ponekad je potrebno deset minuta da biste dovršili prilikom prvog pokretanja ovog zadatka.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 implementacija i pokrenite aplikaciju uzorka

Implementacija i pokrenuti aplikaciju uzorka ponovnim pokretanjem sljedeće naredbe:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 provjerite funkcionira aplikacije

Sada prikazat će se LED na vašem Pi titranje svake dvije sekunde.  Ako ne vidite LED titranje, potražite u članku [Vodič za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md) za rješenja za najčešće probleme.
![Titranje LED](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Korištenje `Ctrl + C` da biste prekinuli aplikaciju.

## <a name="139-summary"></a>1.3.9 sažetak

Potrebni alati za rad s vašeg Pi instaliranja i implementiran uzorka aplikacije za vaše Pi da biste titranja na LED. Možete odmah premjestiti sljedeću nastave možete stvarati, uvođenje i pokrenite neku drugu aplikaciju uzorka koja se povezuje vaše Pi Azure IoT koncentrator za slanje i primanje poruka.

## <a name="next-steps"></a>Daljnji koraci

Sada ste spremni za pokretanje nastave 2 koji počinje s [Nabavite alate za Azure](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
