<properties
 pageTitle="Za razvojne inženjere vodič – Upoznavanje twins uređaja | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – korištenje uređaja twins sinkronizacija stanje i konfiguracija podataka između IoT koncentratora i uređajima"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Razumijevanje uređaj twins – pregled

## <a name="overview"></a>Pregled

*Uređaj twins* su dokumenti JSON kojima se pohranjuju uređaj (meta-podataka, konfiguracije i uvjeta) informacije o stanju. Koncentrator IoT nastavi twin uređaju za svaki uređaj koji se povezuju sa IoT koncentratora. U ovom članku opisane:

* Struktura twin uređaj: *oznake*, *željeni* i *potrebnom svojstva*, i
* operacije koje aplikacija za uređaj i natrag završava može izvoditi na twins uređaja.

> [AZURE.NOTE] U isto vrijeme uređaja twins su dostupne samo s uređaja povezanih s IoT koncentrator pomoću protokola MQTT. Pogledajte [MQTT podržava] [ lnk-devguide-mqtt] članak upute za pretvaranje postojeće aplikacije uređaj da biste koristili MQTT.

### <a name="when-to-use"></a>Kada se koristi

Koristite twins uređaj da biste:

* Pohranite uređaj određenih meta-podataka u oblaku, npr implementacije mjesto vending računala.
* Informacije o trenutnom stanju kao što su dostupne mogućnosti i uvjeta iz aplikacije uređaj, primjerice uređaj povezujete putem mobilnog ili Wi-Fi veze za izvješće.
* Sinkronizacija stanja tijekova rada dugoročnih između uređaja aplikacije i natrag završi, npr pozadinskih navodeći nova verzija opreme da biste instalirali i aplikaciju uređaj izvješćivanja različite faze postupku ažuriranja.
* Upit vaš uređaj meta-podataka, konfiguriranje ili stanje.

Pomoću [uređaja u oblak poruka] [ lnk-d2c] za nizove vremenski označen događaje kao što su vremenski niz podataka senzor ili alarmi. [Oblak uređaj] načina[ lnk-methods] za interaktivnu kontrolu uređajima, kao što su uključivanjem na Lepeza.

## <a name="device-twins"></a>Twins uređaj

Uređaj twins pohranite informacije vezane uz uređaj koji:

- Uređaj i natrag završava možete koristiti da biste sinkronizirali uvjeta uređaja i konfiguracije.
- Pozadinska aplikacija možete koristiti za upit i ciljne dugoročnih operacije.

Životni ciklus uređaj twin je povezana s odgovarajući [uređaj identiteta][lnk-identity]. Twins implicitno stvara i izbrisati kada novi uređaj identitet stvara se ili briše iz koncentratora IoT.

Uređaj twin je JSON dokument koji sadrži:

* **Oznake**. Dokument JSON čitati i napisao pozadinska. Oznake nisu vidljive aplikacije uređaja.
* **Svojstva Desired**. Koristi zajedno s prijavljenim svojstva da biste sinkronizirali Konfiguracija uređaja ili uvjeta. Željeni svojstva može se postaviti samo natrag u aplikaciji završetka i mogu čitati aplikaciju uređaja. Aplikaciju uređaja možete primati obavijesti i u stvarnom vremenu promjena na željeni svojstva.
* **Svojstva Izviješteno**. Koristi zajedno s željenim svojstvima da biste sinkronizirali Konfiguracija uređaja ili uvjeta. Prijavljenim svojstva mogu postaviti samo ako aplikaciju uređaja možete biti pročitajte i mu tako da pozadinska aplikacija.

Uz to, korijenu twin uređaja sadrži samo za čitanje svojstva odgovarajuće identitet kao sadržanih u [uređaj identiteta registra][lnk-identity].

![][img-twin]

Evo primjera uređaj twin JSON dokumenta:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

U Korijenski objekt su svojstva sustava i objektima spremnik za `tags` i obje `reported` i `desired properties`. Na `properties` spremnik sadrži neke elemente samo za čitanje (`$metadata`, `$etag`, i `$version`) koje su odnosno opisane u [Twin metapodataka] [ lnk-twin-metadata] i [optimistična Procjena istodobnosti] [ lnk-concurrency] sekcije.

### <a name="reported-property-example"></a>Primjer prijavljenim svojstvo

U primjeru iznad sadrži twin uređaj s `batteryLevel` svojstvo koje je dojavi aplikaciju uređaja. Ovo svojstvo zahvaljujući upita, a zatim raditi na uređajima koji se temelji na zadnjoj razini prijavljenim baterija. Drugi primjer promijenile mogućnosti uređaja za uređaj aplikacije izvješća ili mogućnosti povezivanja.

Imajte na umu kako prijavljenim svojstva Pojednostavnite scenariji kojima pozadinska zainteresirani zadnje poznate vrijednosti svojstva. Pomoću [uređaja u oblak poruka] [ lnk-d2c] ako pozadinska nije potrebno obraditi uređaju telemetriju u obliku nizove vremenski označen događaja, kao što su vremenski niz.

### <a name="desired-configuration-example"></a>Primjer željena konfiguracija

U primjeru iznad u `telemetryConfig` željeni i poznatih svojstva koriste natrag završetka i uređaja aplikaciju za sinkronizaciju telemetrijskih konfiguraciju za ovaj uređaj. Ako, na primjer:

1. Pozadinska aplikacija postavlja željene svojstvo s vrijednošću željena konfiguracija. Evo dio dokumenta s željeni svojstvo:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. Aplikaciju uređaj prima obavijest o promjenama odmah ako povezani ili pri prvom ponovnog povezivanja. Aplikaciju uređaj pa izvješća ažurirane konfiguraciju (ili pomoću programa pogreške uvjet u `status` svojstvo). Evo dio prijavljenim svojstva:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. Pozadinska aplikacija možete zadržati praćenje rezultata postupak konfiguracije na brojnim uređajima postavljanjem [upita] [ lnk-query] twins.

> [AZURE.NOTE] Iznad isječci su primjeri, optimizirana čitljivost mogući način kodiranje Konfiguracija uređaja i njezin status. Koncentrator IoT nametnuti određene sheme za željenu i poznatih svojstva twins uređaja.

U mnogim slučajevima twins se koriste za sinkronizaciju dugoročnih operacija kao što su ažuriranja opreme. Pogledajte korištenje [željeni svojstava koja možete konfigurirati uređaje] [ lnk-twin-properties] dodatne informacije o korištenju svojstva za sinkronizaciju i praćenje dugo izvodi postupke svim uređajima.

## <a name="back-end-operations"></a>Pozadinske operacije

Pozadinska pristajete na twin pomoću sljedećih atomske operacija, putem HTTP-a izložen:

1. **Dohvaćanje twin po ID-a**. Ovaj postupak vraća prijavljena sadržaja twin u dokumentu, uključujući oznake i želji i svojstva sustava.
2. **Djelomično ažuriranje twin**. Ovaj postupak omogućuje pozadinska djelomično ažuriranja u twin oznake ili željenim svojstvima. Djelomično ažurirati izražen je kao dokument JSON koji dodaje ili ažurira neko svojstvo spomenuti. Svojstva postavljena na `null` uklanjaju. Ako, na primjer, sljedeće stvara novo svojstvo željeni vrijednošću `{"newProperty": "newValue"}`, prebrisuje postojeću vrijednost `existingProperty` s `"otherNewValue"`, i potpuno ukloniti `otherOldProperty`. Bez promjena dogodit će se na ostale postojeće željenim svojstvima i oznaka:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Zamijenite željenim svojstvima**. Ovaj postupak omogućuje pozadinska potpuno prebrisati sve postojeće željenim svojstvima i zamijeniti novi dokument JSON za `properties/desired`.
4. **Zamjena oznake**. Analogously da biste zamijenili željenim svojstvima, ova operacije omogućuje pozadinska potpuno prebrisati sve postojeće oznake i zamijeniti novi dokument JSON za `tags`.

Sve iznad operacije podržava [optimistična Procjena istodobnosti] [ lnk-concurrency] i zatraži dozvole **ServiceConnect** kako je definirano u [Sigurnost] [ lnk-security] članka.

Osim te operacije pozadinska upit možete poslati twins pomoću programa SQL nalik [jezika za upite][lnk-query], i izvođenje operacija na velikih skupova twins pomoću [poslova][lnk-jobs].

## <a name="device-operations"></a>Operacije uređaja

Aplikaciju uređaja radi twin pomoću sljedećih operacija atomske:

1. **Dohvaćanje twin**. Ovaj postupak vraća sadržaja dokumenta u twin (uključujući i oznake i to želite, prijavljenih i svojstva sustava) za trenutno povezani uređaj.
2. **Djelomično je ažuriranje svojstava**. Ovaj postupak omogućuje djelomičnog ažuriranja prijavljenim svojstva trenutno povezani uređaj. Koristi se isti JSON OSNOVNI oblik ažuriranje na kao pozadinska nasuprotne djelomičnog ažuriranja željenim svojstvima.
3. **Observe želji svojstva**. Trenutno povezani uređaj možete primati obavijesti o ažuriranjima željenim svojstvima čim se zbivaju. Uređaj prima istom obrascu ažuriranja (djelomična ili potpuna zamjena) izvršio pozadinska.

Sve iznad operacije zatraži dozvole **DeviceConnect** kako je definirano u [Sigurnost] [ lnk-security] članka.

[Azure IoT uređaj SDK-ovi] [ lnk-sdks] olakšavaju korištenje iznad operacija s raznim jezicima i platformama. Dodatne informacije o detalje o primitives IoT koncentrator za sinkronizaciju željeni svojstva pronaći ćete u [tijeku ponovno povezivanje uređaja][lnk-reconnection].

> [AZURE.NOTE] Trenutno uređaj twins su dostupne samo s uređaja povezanih s IoT koncentrator pomoću protokola MQTT.

## <a name="reference-topics"></a>Pregled tema:

U sljedećim temama reference sadrže dodatne informacije o pristupom koncentratora za IoT.

## <a name="tags-and-properties-format"></a>Oznake i svojstva oblik

Oznake, a zatim željeni i poznatih svojstva su JSON objekte s sljedeća ograničenja:

* Sve tipke JSON objektima su velika i mala slova 128 char UNICODE nizove. Dopuštene znakova isključi UNICODE kontrolne znakove (segmenti C0 i C1), a `'.'`, `' '`, i `'$'`.
* Sve vrijednosti u objektu JSON može biti od sljedećih vrsta JSON: Booleove vrijednosti, broj, niz, a zatim objekt. Nije dopušteno polja.

## <a name="twin-size"></a>Veličina twin

Koncentrator IoT nameće je ograničenje veličine 8KB na vrijednosti `tags`, `properties/desired`, i `properties/reported`, isključujući elemente samo za čitanje.
Veličina je izračunati tako što broji kontrolirati sve znakove osim UNICODE znakova (segmenti C0 i C1) i razmak `' '` kada se pojavi izvan konstanta niza.
Koncentrator IoT odbacit uz poruku o pogrešci sve operacije koje želite povećati te dokumente iznad ograničenje.

## <a name="twin-metadata"></a>Twin metapodataka

Koncentrator IoT održava vremenske oznake zadnjeg ažuriranja za svaki objekt JSON u željenom i poznatih svojstva. Na vremenske oznake u UTC-a i kodirani u obliku [ISO8601] `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Ako, na primjer:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Ti podaci zadržavaju se na svakoj razini (ne samo ostavlja JSON struktura) da biste sačuvali ažuriranja koja je ukloniti tipke objekta.

## <a name="optimistic-concurrency"></a>Optimistična Procjena istodobnosti

Oznake, svojstva željeni i poznatih sve podržava optimistična Procjena istodobnosti.
Oznake sadrže programa e-oznake, po [RFC7232], koji predstavlja predstavljanje JSON oznake. Možete koristiti sljedeće u operacija uvjetno ažuriranja iz pozadinska da biste bili sigurni dosljednost.

Svojstva željeni i prijavljene su etags, ali ste je `$version` vrijednost koja se zajamčeno rastuće. Analogously da se e-oznake na verzije moguće je koristiti strana ažuriranja kao što su (na uređaju aplikacija za prijavljene svojstvo) ili pozadinska željeni svojstva da biste nametnuli dosljednost ažuriranja.

Verzije su koristan kada observing agent (primjerice, aplikacija na uređaju opažanja željenim svojstvima) ne može uskladiti races između rezultat operacije na dohvat i obavijest ažuriranja. U odjeljku [tijek ponovno povezivanje uređaja] [ lnk-reconnection] navedene su dodatne informacije.

## <a name="device-reconnection-flow"></a>Tijek ponovno povezivanje uređaja

Koncentrator IoT sačuvati željenim svojstvima obavijesti o ažuriranjima za uređaje prekinuta veza. Slijedi uređaj koji povezuje morate dohvatiti puno željeni svojstva dokumenta, osim pretplate za obavijesti o ažuriranjima. Uz mogućnost nastanka races između obavijesti o ažuriranjima i cijeli dohvaćanja, se mora ensured protok sljedeće:

1. Aplikacija za uređaj povezuje koncentratora za IoT.
2. Uređaj aplikacije za željenu svojstva pretplaćuje obavijesti o ažuriranjima.
3. Uređaj aplikacije dohvaća cijeli dokument za željenu svojstva.

Aplikaciju uređaja možete zanemariti sve obavijesti s `$version` manji ili jednak od verzije puno dohvaćene dokumenta. Ovo je moguće jer IoT koncentrator jamčiti da uvijek povećali verzije.

> [AZURE.NOTE] U ovom logika već implementirana u [Azure IoT uređaj SDK-ovi][lnk-sdks]. Opis korisno je ako aplikaciju uređaj ne možete koristiti bilo koji od Azure IoT uređaj SDK-ovi i morate programirati sučelja MQTT izravno.

## <a name="additional-reference-material"></a>Dodatne reference materijala

Druge teme referencu u vodiču za razvojne inženjere obuhvaćaju sljedeće:

- [Krajnje točke koncentrator IoT] [ lnk-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke.
- [Ograničavanje i kvotama] [ lnk-quotas] u članku se opisuje kvote koji se odnose na servis IoT koncentratora i regulacije ponašanje očekivati prilikom korištenja servisa.
- [Koncentrator IoT uređaja i servisa SDK-ovi] [ lnk-sdks] popis različitih jezika SDK-ovi koje koristi u prilikom razvoja uređaja i servisa aplikacije kompatibilne s IoT koncentratora.
- [Upit jezik za twins, postupcima i zadacima] [ lnk-query] u članku se opisuje jezik upita možete koristiti za dohvaćanje informacija iz centra za IoT o uređaju twins, metode i zadacima.
- [Podrška IoT koncentrator MQTT] [ lnk-devguide-mqtt] navedene su dodatne informacije o podršci za IoT koncentrator za protokol MQTT.

## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili o twins uređaj, možda ćete u sljedećim temama vodič za razvojne inženjere:

- [Pozivanje metode izravno na uređaju][lnk-methods]
- [Zakazivanje zadataka na više uređaja][lnk-jobs]

Ako želite isprobati neke koncepta opisane u ovom članku, možda ćete zanimati i sljedeći vodiči koncentrator IoT:

- [Kako koristiti twin uređaja][lnk-twin-tutorial]
- [Kako koristiti twin svojstva][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png