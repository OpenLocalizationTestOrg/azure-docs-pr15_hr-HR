## <a name="view-device-telemetry-in-the-dashboard"></a>Telemetry prikaz uređaja na kontrolnoj tabli

Nadzorne ploče u udaljenom nadzor rješenja omogućuje prikaz telemetry koji svoje uređaje poslati IoT koncentrator.

1. U pregledniku, vratite udaljene nadzor rješenje nadzorne ploče, pritisnite **uređaji** na lijevoj ploči za navigaciju do **popisa uređaja**.

2. **Popis uređaja**, trebali biste vidjeti stanje uređaja je sada **izvodi**.

    ![][18]

3. Kliknite **nadzornu ploču** vratili nadzornu ploču, odaberite uređaj u **uređaj za prikaz** padajućeg popisa da biste vidjeli njegove telemetry. Telemetry iz aplikacije uzorak je 50 jedinica za interne temperatura, 55 jedinica za vanjska temperatura i 50 jedinica za humidity. Imajte na umu da po zadanom nadzorne ploče prikazuje samo temperature i humidity vrijednosti.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Pošalji naredbu na uređaj

Nadzorne ploče u udaljenom nadzor rješenja omogućuje slanje naredbi uređaji kroz koncentrator IoT. Na primjer, u udaljenom nadzor rješenje možete poslati naredbu za postavljanje Interna temperatura uređaj.

1. U udaljenom nadzor rješenje nadzornu ploču, pritisnite **uređaji** na lijevoj ploči za navigaciju do **popisa uređaja**.

2. Kliknite **ID uređaja** za uređaj **popisu uređaja**.

3. Na ploči **uređaj pojedinosti** pritisnite **naredbe**.

    ![][13]

4. U **naredbu** padajući popis, odaberite **SetTemperature**i zatim **temperatura** unesite novu vrijednost temperatura. Pritisnite **naredbu Pošalji** slanje naredbu na uređaj.

    ![][14]

    > [AZURE.NOTE] Povijest naredbi inicijalno prikazuje status naredbu kao **Neriješeno**. Kada uređaj priznaje naredbu, status se mijenja **uspjeh**.

5. Na nadzornu ploču, provjerite da uređaj je sada slanje 75 kao novu vrijednost temperatura.

## <a name="next-steps"></a>Sljedeći koraci

[Prilagodba prethodno konfigurirali rješenja] članak[ lnk-customize] opisuje neke načine možete proširiti ovaj uzorak. Moguća proširenja uključuju pomoću realni senzori i implementiranju dodatnih naredbi.

Saznajte više o [dozvolama na web-mjestu azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
