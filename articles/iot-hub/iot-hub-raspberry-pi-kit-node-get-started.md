<properties
 pageTitle="Početak rada s nadjev od Pi 3 | Microsoft Azure"
 description="Početak rada s Raspberry Pi 3, stvaranje koncentratora za IoT Azure i povezivanje na Pi koncentrator IoT"
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

# <a name="get-started-with-raspberry-pi-3"></a>Početak rada s nadjev od Pi 3

U ovom ćete praktičnom vodiču počet ćete tako da Naučite osnove rad s Raspberry Pi 3 te izvodi Raspbian. Zatim saznat ćete kako se jednostavno povezivanje uređaja s oblakom s [Azure IoT koncentratora](iot-hub-what-is-iot-hub.md). Za Windows 10 IoT Core uzorka, posjetite [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Lekciju 1: Konfiguriranje uređaja

![Dijagram E2E nastave 1](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

U ovom nastave konfiguriranje Raspberry Pi 3 uređaj s operacijskim sustavom, postavite razvojno okruženje i implementaciju aplikacija za vrijednost Pi.

### <a name="configure-your-device"></a>Konfiguriranje uređaja

Konfiguriranje Raspberry Pi 3 za prvo korištenje i instalirajte Raspbian OS, besplatne operacijski sustav koji je optimiziran za hardver Raspberry Pi.

*Preostalo vrijeme da biste dovršili: 30 minuta* 

[Idi na "Konfiguriranje uređaju"](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Početak alata
Preuzimanje alata i softver omogućuje stvaranje i implementaciju prvi aplikacija za Raspberry Pi 3.

*Preostalo vrijeme da biste dovršili: 20 minuta* 

[Idite na "Dohvati alata"](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Stvaranje i implementaciju aplikacija za titranja

Kloniraj aplikacije Node.js uzorka Github, a gulp za implementaciju ovu aplikaciju panel Raspberry Pi 3. U ovom primjeru aplikacije titranja LED povezan s ploče svake dvije sekunde.

*Preostalo vrijeme da biste dovršili: pet minuta* 

[Idite na "Stvaranje i implementaciju aplikacija za titranja"](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lekciju 2: Stvaranje koncentratora za IoT

![Dijagram E2E nastave 2](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

U ovom nastave stvarate pomoću besplatnog Azure računa, Dodjela koncentratora za Azure IoT te prvi uređaj u središte Azure IoT.

Dovršili nastave 1 prije pokretanja ovog predloška.

### <a name="get-the-azure-tools"></a>Nabavite alate za Azure

Instalacija Azure sučelje naredbenog retka (Azure EŽA).

*Preostalo vrijeme da biste dovršili: 10 minuta* 

[Odaberite "Dohvati Azure Alati"](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Stvaranje koncentratora za IoT i registrirati svoje Raspberry Pi 3

Stvaranje grupe resursa, dodijelite prvi koncentratora za Azure IoT i dodavanje prvom uređaju koncentrator IoT Azure pomoću Azure EŽA. 

*Preostalo vrijeme da biste dovršili: 10 minuta* 

[Idite na "Stvaranje koncentratora za IoT i registrirati svoje Raspberry Pi 3"](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Lekciju 3: Slanje poruka uređaj u oblak

![Dijagram E2E nastave 3](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

U ovom nastave šaljete poruke iz vaše Pi koncentratora za IoT. Stvorite aplikaciju Azure funkcija koja uzima dolazne poruke iz centra za IoT i zapisuje spremište tablica platforme Azure.

Prije početka ovog predloška, dovršite za početnike 1 i 2.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Stvaranje aplikacije za Azure funkcija i račun za Azure prostora za pohranu

Stvorite aplikaciju Azure funkcija i račun za pohranu Azure pomoću predloška Azure Voditelj resursa.

*Preostalo vrijeme da biste dovršili: 10 minuta* 

[Idi na 'Stvori Azure funkcija aplikacije i račun za Azure pohranu'](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Pokretanje aplikacije uzorka za slanje poruka uređaj u oblak

Uvođenje i pokretanje aplikacije za uzorak s uređajem Raspberry Pi 3 koja šalje poruke IoT koncentrator.

*Preostalo vrijeme da biste dovršili: 10 minuta* 

[Idi na "Pokretanje uzorka aplikacije za slanje poruka uređaj u oblak"](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Čitanje poruka ista i u odjeljku pohrana Azure
Praćenje poruka uređaja u oblaku kao što su zapisuju pohranom servisa Azure.

*Preostalo vrijeme da biste dovršili: pet minuta* 

[Idi na "čitanje poruka ista i u odjeljku pohrana Azure"](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Lekciju 4: Slanje poruka oblaka uređaja

![Dijagram E2E nastave 4](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

U ovom nastave demos slanju poruke iz centra za Azure IoT Pi 3-a Raspberry. Poruke kontrolirati i isključenog ponašanja LED koji je povezan s vašeg Pi. Primjer aplikacije pripremiti je za vas da biste postigli ovaj zadatak.

Dovršite za početnike 1, 2 i 3 prije pokretanja ovog predloška.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Pokrenite aplikaciju uzorka primati poruke oblaka uređaja

Primjer aplikacije u 4 nastave izvršava na vašem Pi i nadzire dolazne poruke iz centra za IoT. Novi zadatak gulp šalje poruke vašeg Pi iz centra za IoT da biste titranja na LED.

*Preostalo vrijeme da biste dovršili: 10 minuta* 

[Idi na "Izvršavaju primjer aplikacije primaju poruke oblaka uređaj"](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Neobavezna sekcija: promjena i isključenog ponašanje u LED

Prilagođavanje poruke da biste promijenili u LED na Uključivanje i isključivanje ponašanje.

*Preostalo vrijeme da biste dovršili: 10 minuta* 

[Idi na ' neobavezne sekcije: promjena i isključenog ponašanje u LED "](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Otklanjanje poteškoća

Ako bilo koji troubles zadovoljava tijekom s predavanja, možete traženje rješenja na ovoj stranici.

[Idite na odjeljak "Otklanjanje poteškoća"](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
