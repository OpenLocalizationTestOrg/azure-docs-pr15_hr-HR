> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Uvod

U [ovladavanju koncentrator IoT twins][lnk-twin-tutorial], naučili kako postaviti uređaj meta-podataka iz vaše rješenje pozadinska korištenjem *oznake*, uvjeti uređaj izvješće iz app uređaj pomoću *prijavljeno svojstva*i upit podatke pomoću SQL nalik jezik.

U ovaj vodič će naučiti kako koristiti u twin u *željenim svojstvima* zajedno s *prijavio svojstva*, daljinski konfigurirati uređaj apps. Točnije, ovaj vodič prikazuje kako je prijavio twin's i željenim svojstvima omogućiti konfiguracije s više koraka postavke aplikacije uređaj i pružanje vidljivost rješenje natrag kraj status ovu operaciju preko sve uređaje.

Ovaj vodič na visokoj razini slijedi *željenom stanju uzorak* za upravljanje uređajima. Temeljna ideja ovaj uzorak je imati rješenje pozadinska tako što određivanje željenom stanju upravljanim uređajima umjesto slanja određene naredbe. Ovo stavlja uređaj koji je zadužen za uspostavljanje najbolji način dosegnuti željenom stanju (vrlo važno u IoT scenarijima gdje uvjeti određeni uređaj utječu na mogućnost odmah izvedba određene naredbe), dok neprestano izvješćivanje pozadinska trenutnom stanju i uvjete potencijalne pogreške. Uzorak željenom stanju je Instrumentalni upravljanja velike skupove uređaji, kao omogućuje pozadinska imati puni vidljivost stanje postupak konfiguracije preko sve uređaje.
Dodatne informacije vezane uz ulogu uzorak željenom stanju možete pronaći u upravljanje uređajima u [Pregled od Azure IoT koncentrator uređaj upravljanja][lnk-dm-overview].

> [AZURE.NOTE] U scenarijima gdje su uređaji kontrolira više interaktivni način (Uključi Ventilator iz korisnički Kontrolirana app), razmotrite korištenje [uređaja oblak metode][lnk-methods].

U ovaj vodič promjene aplikacije pozadinskih konfiguracija telemetry ciljni uređaj i zbog koje uređaj app slijedi više koraka procesa za primjenu ažuriranje konfiguracije (npr. koji zahtijevaju ponovno pokretanje modula softver), koji simulira ovaj vodič s jednostavne odgode).

Back-end pohranjuje konfiguracije u željenim svojstvima twin uređaja na sljedeći način:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Budući da konfiguracije mogu biti složene objekte, dodjeljuje im se obično jedinstveni ID (raspršivanja ili [GUID identifikatori][lnk-guid]) da biste pojednostavili njihove usporedbe.

Uređaj app izvješća njegova trenutna konfiguracija zrcaljenja željeno svojstvo **telemetryConfig** u prijavljeno svojstva:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Imajte na umu kako prijavljeno **telemetryConfig** ima je dodatna svojstva **stanja**, koristiti izvještaj stanja proces ažuriranja konfiguracija.

Primljena, novi željenom konfiguracijom app uređaj izvješća čekanju konfiguracije promjenom informacije:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Zatim kasnije neke app uređaj će izvješće uspjeh neuspjeh ove operacije ažuriranjem iznad svojstvo.
Imajte na umu kako pozadinska može, u bilo kojem trenutku upit status postupka konfiguracije preko sve uređaje.

Ovaj vodič prikazuje kako biste:

- Stvaranje simuliranog uređaj koji prima ažuriranja konfiguracija iz pozadinska i izvješća više ažuriranja kao *prijavljeno svojstva* na proces ažuriranja konfiguracija.
- Stvaranje pozadinskim app koja obnavlja željenom konfiguracijom uređaj i upiti proces ažuriranja konfiguracija.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier