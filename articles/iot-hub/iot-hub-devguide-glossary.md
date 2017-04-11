<properties
 pageTitle="Vodič za razvojne inženjere - Pojmovnik | Microsoft Azure"
 description="Pojmovnik uobičajenih uvjeta koji se odnose na IoT koncentratora"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="glossary-of-iot-hub-terms"></a>Pojmovnik IoT koncentratora

U ovom se članku navedene neke od uobičajenih uvjeta pridružene IoT koncentratora.

## <a name="advanced-message-queueing-protocol-amqp"></a>Napredne poruke reda čekanja Protocol (AMQP)

[AMQP](https://www.amqp.org/) jedan je od protokola za razmjenu poruka koji podržava IoT koncentrator za komunikaciju s uređaja. Dodatne informacije o protokola za razmjenu poruka koji podržava IoT koncentrator potražite u članku [Slanje i primanje poruka s IoT koncentratora](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Oblak-na-uređaja (C2D)

Obično se koristi da biste se pozvali na poruke poslane s IoT koncentrator povezani uređaj. Često te poruke su naredbe koje uputiti uređaj da biste preuzeli neke akcije. Dodatne informacije potražite u članku [Slanje i primanje poruka s IoT koncentratora](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Uvjet

Odnosi se na stanje informacije o uređaju, kao što je način povezivanja trenutno se koristi, kao što je dojavi aplikaciju za uređaj. Uređaje možete se prijaviti i svoje mogućnosti. Upit možete poslati uvjet i mogućnost korištenja twin uređaja.

## <a name="data-point-message"></a>Točke podataka poruka

Točke podataka poruka da je uređaj oblaka poruku koja sadrži telemetrijskih podataka kao što je brzina vjetra ili temperature.

## <a name="desired-properties"></a>Željeni svojstva

U kontekstu twins uređaj, željenim svojstvima se koriste u kombinaciji s *potrebnom svojstva* za sinkronizaciju Konfiguracija uređaja ili uvjeta. Željeni svojstva može se postaviti samo natrag u aplikaciji završetka i vrše aplikaciju uređaja. 

## <a name="device-to-cloud-d2c"></a>Uređaj-na-oblaka (D2C)

Obično se koristi da biste se pozvali na poruke poslane s uređaja povezanih IoT koncentratora. Te poruke mogu biti [podatak](#data-point-message) ili [interaktivnog](#interactive-message) poruke. Dodatne informacije potražite u članku [Slanje i primanje poruka s IoT koncentratora](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Uređaj identiteta registra

[Uređaj identiteta registra](iot-hub-devguide-identity-registry.md) je ugrađena komponenta koncentratora za IoT koji pohranjuje informacije o pojedinačnim uređajima dopuštenom povezati koncentratora.

## <a name="device"></a>Uređaj

U kontekstu IoT, uređaj je obično manjih razmjera, samostalne računalnih uređaj koji mogu prikupljanje podataka i kontrola drugih uređaja. Na primjer na uređaju možda, okolini nadzora uređaja ili kontrolera za sustave watering i ventilacije u na greenhouse.

## <a name="device-twin"></a>Twin uređaj

[Uređaj twin](iot-hub-devguide-device-twins.md) je kopiju u koncentratoru IoT uvjet i konfiguracija postavki fizički uređaj. Koristite uređaj twin upravljanju konfiguracijama fizički uređaj.

## <a name="direct-method"></a>Izravni način

[Izravni način](iot-hub-devguide-direct-methods.md) je način za pokretanje metode izvršiti na uređaj tako da pozivanje API-JA na koncentratora za IoT.

## <a name="event-hub-compatible-endpoint"></a>Krajnje kompatibilan s preglednikom koncentrator događaja

Da biste pročitali uređaj u oblak poruke poslane koncentratora za IoT, možete povezati krajnje točke na vaše središte i koristite neki događaj koncentrator kompatibilnog način za čitanje poruke. Metode kompatibilan s preglednikom koncentrator za događaj obuhvaćaju pomoću događaja koncentratora SDK-ovi i analize strujanje Azure.

## <a name="field-gateway"></a>Pristupnik za polje

Pristupnik za polje omogućuje povezivanje za uređaje koji se ne može povezati izravno s IoT koncentratora i obično implementirati lokalno s uređajima. Dodatne informacije potražite u članku [što je koncentrator IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Zadatak

Pozadinska vaše rješenje možete koristiti zadatke za planiranje i praćenje aktivnosti na skupu uređaji registriran koncentratora za IoT. Aktivnosti obuhvaćaju ažuriranje svojstava twin želji uređaja, ažuriranje uređaj twin oznake i pozivanje Izravni metode.

## <a name="protocol-gateway"></a>Protokol pristupnika

Protokol pristupnika obično implementiran u oblak i omogućuje protokol servisa za prevođenje za uređaje povezivanje IoT koncentrator. Dodatne informacije potražite u članku [što je koncentrator IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interaktivni poruke

Interaktivni poruka da je oblak uređaj poruku koja pokreće trenutne akcije u pozadinska aplikacija. Ako, na primjer, na uređaju možda poslati alarma o pogreške koje se moraju biti prijavljeni automatski u sustavu CRM.

## <a name="iot-hub"></a>IoT koncentratora

Koncentrator IoT je potpuno upravljanih Azure servis koji omogućuje pouzdanog i sigurne Dvosmjeran komunikaciju između milijune IoT uređaji i pozadinskih rješenja. Dodatne informacije potražite u članku [što je koncentrator IoT Azure?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>Paket IoT

Azure IoT programski paketi zajedno većem broju servisa Azure s konfiguriranog rješenja. Ova konfiguriranog rješenja omogućuju vam da biste brzo započeli završetka do kraja implementacije uobičajeni scenariji IoT. Dodatne informacije potražite u članku [što je paket IoT Azure?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Zadatak

[Zadatak](iot-hub-devguide-jobs.md) u središte IoT omogućuje izvođenje operacije kao što je oprema nadogradnje na više uređaja povezanih s vaše središte.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) jedan je od protokola za razmjenu poruka koji podržava IoT koncentrator za komunikaciju s uređaja. Dodatne informacije o protokola za razmjenu poruka koji podržava IoT koncentrator potražite u članku [Slanje i primanje poruka s IoT koncentratora](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Prijavljenim svojstva

U kontekstu twins uređaj, prijavljenim svojstva se koriste u kombinaciji s *željenim svojstvima* za sinkronizaciju Konfiguracija uređaja ili uvjeta. Prijavljenim svojstva mogu postaviti samo ako aplikaciju uređaja možete biti pročitajte i mu tako da pozadinska aplikacija.

## <a name="tags"></a>Oznaka

U kontekstu devcie twins, oznake su uređaj meta-podataka pohranjena i dohvaćaju aplikacije pozadinskih u obrascu JSON dokumenta. Oznake nisu vidljive aplikacije na uređaju.

## <a name="system-properties"></a>Svojstva sustava

U kontekstu twins uređaj, svojstva sustava su samo za čitanje i obuhvatiti informacije o korištenju uređaja kao što su zadnjeg aktivnosti vrijeme i veze stanja.