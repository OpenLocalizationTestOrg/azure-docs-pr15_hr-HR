> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

Ovaj vodič [simuliranog oblak uređaj Prenesi uzorak] prikazuje kako koristiti [Azure IoT pristupnik SDK] [ lnk-sdk] za slanje telemetry uređaj oblak koncentrator IoT iz simuliranog uređaja.

Prikaz pokriva:

1. **Arhitektura**: važne arhitektonski informacije o uzorak Simulirani Prenesi oblak uređaj.

2. **Sastavi i pokrenite**: korake potrebne za izgradite i pokrenite uzorka.

## <a name="architecture"></a>Arhitektura

Uzorak simuliranog oblak uređaj prijenos prikazuje kako koristiti SDK za stvaranje pristupnik koji šalje telemetry iz simuliranog uređaji IoT koncentrator. Simulirani uređaji ne može izravno povezati koncentrator IoT jer:

- Uređaji koristiti komunikacijski protokol razumjeti IoT koncentrator.
- Uređaji nisu dovoljno pametni da zapamti identitet dodijeljene po IoT koncentrator.

Pristupnika rješava te probleme za simulirane uređaje na sljedeće načine:

- Pristupnika razumije protokol koristi simuliranog uređaji prima telemetry uređaj oblak iz uređaja i prosljeđuje te poruke IoT koncentrator pomoću protokola razumjeti koncentrator.
- Pristupnika sprema koncentratorom IoT identitete u ime simuliranog uređaje i djeluje kao proxy simuliranog uređaji slanje poruka IoT koncentrator.

Sljedeći dijagram prikazuje glavne komponente uzorka, uključujući module pristupnik:

![][1]


> [AZURE.NOTE] Module proslijediti poruke izravno međusobno. Module objaviti poruke Interna broker koji se isporučuje u poruke drugim modulima pomoću pretplate mehanizam kao što je prikazano u dijagramu ispod. Za dodatne informacije pogledajte [ovladavanju pristupnik SDK IoT][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Modul ingestion protokol

Ovom modulu je polazište za dobivanje podataka iz uređaja, kroz pristupnika i u oblaka. U uzorku, modul izvodi četiri zadatke:

1.  Stvara simuliranog temperatura podataka.
    
    Napomena: Ako ste koristili realni uređaji, modul želite čitati podatke iz tih fizičkih uređaja.

2.  Smješta podataka simuliranog temperatura u sadržaj poruke.

3.  Svojstvo s lažne MAC adresu dodaje u poruku koja sadrži simuliranog temperatura podataka.

4.  Čini poruku dostupni sljedeći modul u lancu.

> [AZURE.NOTE] Modul naziva **ingestion X protokol** u dijagramu iznad naziva **Simulirani uređaj** u šifru izvora.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; IoT ID koncentrator modul

Ovom modulu skenira za poruke koje uključuju svojstva koja sadrži MAC adresu dodao ingestion modul protokol simuliranog uređaja. Ako modul pronađe takvog svojstva, dodaje drugo svojstvo s ključem uređaj u koncentrator IoT poruku i zatim postaje poruka dostupna sljedeći modul u lancu. Ovo je kako uzorka pridružuje koncentratorom IoT identitete uređaj simuliranog uređaji. Programer postavlja mapiranje između MAC adrese i koncentratorom IoT identitete ručno kao dio konfiguracije modul. 

> [AZURE.NOTE]  Ovaj uzorak koristi MAC adresu kao uređaj Jedinstveni identifikator i korelaciji s identitetom koncentrator IoT uređaja. Međutim, možete napisati vlastite modul koji koristi različite Jedinstveni identifikator. Ako, na primjer, možda imate uređaji s jedinstveni serijske brojeve ili telemetry podataka koji ima naziv jedinstveni uređaj ugrađen u ga može koristiti za utvrđivanje identiteta koncentrator IoT uređaja.

### <a name="iot-hub-communication-module"></a>Modul IoT koncentrator komunikacije

Ovom modulu uzima poruke s IoT koncentrator dodijelio prethodne modul identiteta uređaj i šalje sadržaj poruke koristeći HTTP koncentrator za IoT. HTTP je jedan od tri protokola razumjeti IoT koncentrator.

Umjesto otvaranja vezu IoT koncentrator za svaki uređaj simuliranog ovom modulu otvara jedne HTTP veze iz pristupnik koncentrator za IoT i multiplexes veze iz simuliranog uređaja putem te veze. To omogućuje jedan pristupnik za povezivanje mnogo više uređaja, simuliranog ili drukčije, nego biti moguće ako otvoriti jedinstveni veze za svaki uređaj.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Simulirani uređaj oblak Prenesi uzorka]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md