<properties
 pageTitle="Nabavite alate za (Mac OS 10.10) | Microsoft Azure"
 description="Preuzmite i instalirajte potrebni alati i softver za prvi primjer aplikacije za vaše Pi na Mac OS."
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

# <a name="12-get-the-tools-macos-1010"></a>1.2 dobili alate (Mac OS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 što ćete učiniti

Preuzimanje alata za razvoj i softver za prvi primjer aplikacije za vaše Raspberry Pi 3. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 što ćete saznati
U ovom ćete odjeljku saznat ćete:

- Kako instalirati brojka i Node.js
    - [Brojka](https://git-scm.com) je sustavu kontrole verzija programa Otvori izvor distribuirati. Primjer aplikacije za ovaj nastave pohranjuju se na brojka.
    - [Node.js](https://nodejs.org/en/) je runtime JavaScript s obogaćenim paketa zajednici.
- Kako koristiti NPM da biste instalirali dodatne alate za razvoj Node.js.
  - Najmanji potreban verzija Node.js je 4.5 LTS.
  - [NPM](https://www.npmjs.com) jedan je od upravitelja paket za Node.js.

## <a name="123-what-you-need"></a>1.2.3 što vam je potrebno

- Internetsku vezu za preuzimanje alata za razvoj i softvera
- Mac sa sustavom Mac OS Yosemite (10.10) ili novije verzije

## <a name="124-install-git-and-nodejs"></a>1.2.4 instalacija brojka i Node.js

Da biste instalirali brojka i Node.js, koristite program za upravljanje [Homebrew](http://brew.sh) paket slijedeći ove korake:

1. Instalirajte Homebrew. Ako ste već instalirali Homebrew, idite na stranici korak 2.
  1. Pritisnite `Cmd + Space` , a zatim unesite `Terminal` da biste otvorili terminal prozor.
  2. Pokrenite sljedeću naredbu:

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2. Instalirajte brojka i Node.js ponovnim pokretanjem sljedeće naredbe:

    ```bash
    brew install node git
    ```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 instalirati dodatne alate za razvoj Node.js

Koristite [gulp.js](http://gulpjs.com) da biste automatizirali implementaciju aplikacije uzorka vaše Pi. Pomoću [uređaja, otkrivanje i eža](https://github.com/Azure/device-discovery-cli) dohvatiti podatke o mreži o IoT uređajima.

Instalacija `gulp` i `device-discovery-cli` tako da u prozoru Terminal pokretanjem sljedeće naredbe:

```bash
sudo npm install -g device-discovery-cli gulp
```

Ako imate problema s instalacijom te dodatne razvoj Alati i Node.js na Mac OS, potražite u članku [Vodič za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md) za rješenja za najčešće probleme.

## <a name="126-install-visual-studio-code"></a>1.2.6 instalacije s kodom Visual Studio

[Preuzmite](https://code.visualstudio.com/docs/setup/osx) i instalirajte Visual Studio kod. Visual Studio kod je izvor laganih, ali Napredni uređivač kod za Windows, Linux i Mac OS. Pomoću ovog uređivača kasnije u ovom praktičnom vodiču za uređivanje uzorak koda.

## <a name="127-summary"></a>1.2.7 sažetak

Instalirate potrebna Razvojni alati i softver za prvi primjer aplikacije. U sljedećem odjeljku Stvaranje, uvođenje i izvršavaju primjer aplikacije na vašem Pi.

## <a name="next-steps"></a>Daljnji koraci

[1.3 stvaranje i implementaciju aplikacija za uzorak titranja](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
