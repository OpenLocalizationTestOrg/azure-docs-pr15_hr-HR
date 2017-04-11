<properties
 pageTitle="Konfiguriranje uređaja | Microsoft Azure"
 description="Konfiguriranje Raspberry Pi 3 za prvo korištenje vrijeme i instalirajte Raspbian OS, besplatne operacijski sustav koji je optimiziran za hardver Raspberry Pi."
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

# <a name="11-configure-your-device"></a>1.1 konfiguriranje uređaja

## <a name="111-what-you-will-do"></a>1.1.1 što ćete učiniti

Konfiguriranje sustava Pi za prvo korištenje vrijeme i instalirajte Raspbian operacijskom sustavu, besplatne operacijski sustav koji je optimiziran za hardver Raspberry Pi. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="112-what-you-will-learn"></a>1.1.2 što ćete saznati

U ovom ćete odjeljku saznat ćete:

- Kako instalirati Raspbian na vašem Pi
- Kako napajanja vaše Pi pomoću USB kabela
- Kako se povezati svoje Pi s mrežom pomoću Ethernet kabela ili Wi-Fi
- Kako dodati programa LED na breadboard i povežite ga s vašeg Pi

## <a name="113-what-you-need"></a>1.1.3 što vam je potrebno

Da biste dovršili ovaj odjeljak, potrebno je iz Raspberry komplet za Pi 3 Starter sljedećih dijelova:

- Ploče Raspberry Pi 3
- Na kartici MicroSD 16GB
- 5V 2A power navesti sa šest podnožje micro USB kabela
- Na breadboard
- Žice poveznika
- 560 Ohm otpornik
- Slika 10mm LED
- Ethernet kabela

![Na Starter Kit](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Morate:

- Ožičenu ili bežičnom vezom za vaše Pi povezati
- USB SD prilagodnika ili mini SD karticu sustava da biste snimili sliku s operacijskim Sustavom u karticu MicroSD.
- Računalo s operacijskim sustavom Windows, Mac i Linux. Na računalu koristi se za instaliranje Raspbian na kartici MicroSD.
- Povezani s Internetom da biste preuzeli potrebni alati i softvera

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 instalacija Raspbian na kartici MicroSD

Priprema MicroSD kartice da biste napisali Raspbian sliku da biste.

1. Preuzmite Raspbian.
  1. [Preuzimanje](https://www.raspberrypi.org/downloads/raspbian/) datoteke zip za Raspbian Jessie s piksela.
  2. Izdvajanje Raspbian sliku u mapu na računalu.
2. Na kartici MicroSD instalirajte Raspbian.
  1. [Preuzmite](https://www.etcher.io) i instalirajte uslužni Etcher SD kartica snimač.
  2. Pokrenite Etcher i odaberite Raspbian slike koju ste izdvojili u koraku 1.
  3. Odaberite karticu pogon MicroSD.
    Napomena: Etcher možda ste već odabrali ispravnu jedinicu.
  4. Kliknite Flash da biste instalirali Raspbian MicroSD kartice.
  5. Na kartici MicroSD uklonili s računala kada je dovršena.
    Napomena: Je sigurno da biste uklonili kartica MicroSD izravno jer Etcher automatski Izbacuje ili unmounts kartica MicroSD po dovršetku.
  6. Umetnite karticu MicroSD u vašem Pi.

![Umetnite karticu SD](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 power na vašem Pi

Power na vašem Pi pomoću USB kabela micro i napajanjem.

![Uključeno](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] Važno je da koristite napajanjem u komplet koji je najmanje 2A da biste bili sigurni da se vaša Raspberry ulaže s dovoljno napajanja da bi ispravno funkcionirala.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 Pi 3-a Raspberry povežite se s mrežom

Možete se povezati svoje Pi u ožičenoj vezi ili bežičnoj mreži. Provjerite je li vaša Pi povezan s istom mrežom kao i računalo. Ako, na primjer, vaš Pi možete povezati skretnica vaše računalo povezano.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 povezati ožičenoj vezi

Pomoću Ethernet kabela povezati svoje Pi ožičenoj vezi. Dva LEDs na vašem Pi uključite ako uspostavljanja veze.

![Povezivanje putem Ethernet kabela](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 povezati bežičnoj mreži

Slijedite [upute](https://www.raspberrypi.org/learning/software-guide/wifi/) iz Foundation Pi Raspberry povezati svoje Pi bežičnoj mreži. Ove upute potrebno najprije povezati sa zaslonom i pomoću tipkovnice na Pi.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 povezati s LED vaše Pi

Da biste dovršili ovaj zadatak, koristite [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), žice poveznika, na LED i na otpornik. Ih povezati s priključke [Općenite namjene ulazni i izlazni](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) na pi. 

![Breadboard, LED i otpornik](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Povezivanje kraći leg od na LED **GPIO GND (PIN-a 6)**.
2. Povezivanje dulje leg od na LED jedan leg na otpornik.
3. Povezivanje drugih leg na otpornik **GPIO 4 (PIN-a 7)**.

Imajte na umu da polaritet LED važna. Ta postavka polaritet se obično naziva aktivni nisko.

![Pinout](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Congratulation! Uspješno ste konfigurirali svoje Pi.

## <a name="118-summary"></a>1.1.8 sažetak

U ovom ćete odjeljku ste naučili kako konfigurirati svoje Pi instalacije Raspbian, povezivanje s mrežom na Pi i povezivanje programa LED vaše Pi. Imajte na umu da se LED još ne Osnovno prema gore. U sljedećem odjeljku, instalirajte potrebni alati i softvera Priprema radi probna aplikacija na vašem Pi.

![Spreman hardvera](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Daljnji koraci

[1.2 Alati za početak](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
