> [AZURE.SELECTOR]
- [C na Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C na Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C na mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Pregled scenarija

U ovom scenariju, stvorite uređaj koji šalje sljedeće telemetry daljinskog nadzor [konfiguriranih rješenje][lnk-what-are-preconfig-solutions]:

- Vanjska temperatura
- Interna temperatura
- Humidity

Zbog jednostavnosti, kod na uređaju generira ogledne vrijednosti, ali Pozivamo vas da proširiti uzorka povezivanja stvarnih senzori uređaj i slanje pravi telemetry.

Da biste dovršili ovaj vodič, morate aktivni Azure računa. Ako nemate račun, možete stvoriti besplatna probna račun u samo nekoliko minuta. Za detalje pogledajte [Besplatna probna Azure][lnk-free-trial].

## <a name="before-you-start"></a>Prije početka

Prije pisanja koda za vaš uređaj, morate Dodjela udaljene nadzor konfiguriranih rješenje i dodjela novog prilagođenog uređaja u to rješenje.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Dodjela resursa udaljenog nadzor prethodno konfigurirali rješenja

Uređaj kreirate ovaj vodič šalje podatke instancu [udaljene nadzor] [ lnk-remote-monitoring] prethodno konfigurirali rješenje. Ako niste već dodijeljeni resursi udaljene nadzor konfiguriranih rješenja u Azure račun, slijedite dolje navedene korake:

1. Na stranici <https://www.azureiotsuite.com/> , pritisnite **+** da biste stvorili novo rješenje.

2. Pritisnite **Odabir** na ploči **udaljene nadzor** stvoriti novo rješenje.

3. Na stranici **Stvaranje udaljene nadzor rješenje** unesite **naziv rješenja** vašem izboru, odaberite **područje** koje želite uvesti i odaberite Azure pretplate želite koristiti. Zatim kliknite **Stvori rješenja**.

4. Pričekajte dok se ne dovrši postupka dodjeljivanja resursa.

> [AZURE.WARNING] Konfiguriranih rješenja koristite naplativo Azure usluge. Svakako uklonite konfiguriranih rješenja iz pretplate kada završite s njim da biste izbjegli sve nepotrebne troškove. Možete u potpunosti ukloniti konfiguriranih rješenja iz pretplate posjećivanju <https://www.azureiotsuite.com/> stranice.

Kada završi postupka dodjeljivanja resursa za udaljene nadzor rješenje, kliknite **pokretanje** otvorite rješenje nadzornu ploču u pregledniku.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Dodjela uređaj u udaljenom nadzor rješenja

> [AZURE.NOTE] Ako ste već dodijeljeni resursi uređaj u svoje rješenje, preskočite ovaj korak. Morate znati vjerodajnice uređaj kada kreirate klijentske aplikacije.

Za uređaj za povezivanje rešenje konfiguriranih ga morate identificiralo IoT koncentrator pomoću valjane vjerodajnice. Možete dohvatiti vjerodajnice uređaj iz rješenja nadzorne ploče. Uključiti uređaj vjerodajnice u klijentskoj aplikaciji u nastavku ovog praktičnog vodiča. 

Da biste dodali novi uređaj udaljene nadzor rješenje, dovršite sljedeće korake nadzorne ploče rješenje:

1.  U donjem lijevom kutu nadzorne ploče kliknite **Dodaj uređaj**.

    ![][1]

2.  Na ploči **Prilagođeno uređaja** kliknite **Dodaj novu**.

    ![][2]

3.  Odaberite **Omogući mi definiranje vlastiti ID uređaja**, unesite ID uređaja kao što su **mydevice**, kliknite **Provjeri ID** da biste provjerili taj naziv već nije u upotrebi i zatim kliknite **Stvori** Dodjela uređaj.

    ![][3]

5. Zabilježite uređaj vjerodajnice (ID uređaja, IoT koncentrator naziva glavnog računala i uređaja ključ), klijentska aplikacija treba ih povezati s udaljenom nadzor rješenje. Zatim kliknite **Obavljeno**.

    ![][4]

6. Provjerite je li vaš uređaj prikazuje u odjeljku Uređaji. Stanje uređaja je na **čekanju** dok uređaj uspostavlja vezu s udaljenom nadzor rješenje.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/