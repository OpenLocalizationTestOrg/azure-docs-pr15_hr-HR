> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Uvod

Uređaj twins su dokumenti JSON kojima se pohranjuju uređaj (meta-podataka, konfiguracije i uvjeta) informacije o stanju. Koncentrator IoT nastavi twin uređaju za svaki uređaj koji se povezuju sa IoT koncentratora.

Koristite twins uređaj da biste:

* Spremanje uređaj meta-podataka iz vaše pozadinskih.
* Informacije o trenutnom stanju kao što su dostupne mogućnosti i uvjeta (na primjer, povezivanje način) izvješća iz aplikacije za uređaj.
* Sinkronizacija stanja tijekova rada dugoročnih (kao što su ažuriranja opreme i konfiguracija) između aplikacija za uređaj i pozadinskih.
* Upit vaš uređaj meta-podataka, konfiguriranje ili stanje.

> [AZURE.NOTE] Uređaj twins osmišljeni su za sinkronizaciju te za ispitivanje Konfiguracija uređaja i odredbe. Pomoću [uređaja u oblak poruka] [ lnk-d2c] za nizove vremenski označen događaja (kao što su strujanja telemetrijskih podataka koji se temelji na vrijeme – senzor) i [metode oblaka uređaj] [ lnk-methods] za interaktivnu kontrolu uređajima, kao što su uključivanjem Lepeza iz aplikacije za korisnički kontrolirano.

Uređaj twins se pohranjuju u koncentratora za IoT i sadrže:

* *oznake*, uređaj meta-podataka dostupni su samo pozadinska;
* *željeni svojstva*, JSON objekata mijenjati pozadinskih i observable aplikacija uređaj; i
* završili *prijavljenim svojstva*, JSON objekti mijenjati aplikacija uređaja i pročitati u pozadinu. Oznake i svojstva ne može sadržavati polja, no može se ugnijezditi objekte.

![][img-twin]

Uz to, pozadinska aplikacija upit možete poslati uređaj twins iznad podataka na temelju.
Pogledajte [objašnjenje uređaj twins] [ lnk-twins] dodatne informacije o uređaju twins i [Jezik upita koncentrator IoT] [ lnk-query] referenca za postavljanje upita.

> [AZURE.NOTE] U isto vrijeme uređaja twins su dostupne samo s uređaja povezanih s IoT koncentrator pomoću protokola MQTT. Pogledajte [MQTT podržava] [ lnk-devguide-mqtt] članak upute za pretvaranje postojeće aplikacije uređaj da biste koristili MQTT.

Pomoću ovog praktičnog vodiča prikazuje kako da biste:

- Stvaranje pozadinske aplikacije koji dodaje *oznake* uređaj twin i Simulirani uređaj koji izvješća njegov kanala povezivanje kao u *potrebnom svojstvo* na twin uređaja.
- Upit uređaja iz aplikacije pozadinskih pomoću filtara na svojstva ranije stvorili i oznake.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md