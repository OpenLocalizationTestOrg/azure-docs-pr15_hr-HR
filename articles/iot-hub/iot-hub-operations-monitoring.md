<properties
 pageTitle="Operacija IoT koncentrator nadzora"
 description="Pregled Azure IoT koncentrator operacije nadzor, što vam omogućuje da nadzirati status operacije koncentratora za IoT u stvarnom vremenu"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Uvod u operacije nadzora

Operacija IoT koncentrator za nadzor omogućuje vam praćenje stanja operacije koncentratora za IoT u stvarnom vremenu. Koncentrator IoT prati događaje nekoliko kategorijama operacije, a možete odabrati u slanje događaje iz jedne ili više kategorija krajnje od koncentratora za IoT za obradu. Možete nadzirati podataka pogrešaka ili postavljanje složenije obrada na temelju uzoraka podataka.

Koncentrator IoT nadzire pet kategorija događaja:

- Operacije identiteta uređaja
- Telemetrijskih uređaj
- Oblak uređaj naredbe
- Veze
- Datoteka

## <a name="how-to-enable-operations-monitoring"></a>Kako omogućiti operacije nadzora

1. Stvaranje koncentratora za IoT. Može pronaći upute o stvaranju koncentratora za IoT u [Početak] [ lnk-get-started] vodič.

2. Otvorite plohu koncentratora za IoT. Iz njega, kliknite **nadzor operacije**.

    ![][1]

3. Odaberite nadzora kategorije koje želite nadzirati, a zatim kliknite **Spremi**. Događaji dostupni su za čitanje na krajnjoj točki kompatibilan s preglednikom događaja koncentrator naveden u **postavkama praćenja**. Krajnja točka IoT koncentrator zove `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Kategorije događaja i kako ih koristiti

Svaki operacije nadzor kategorija prati različite vrste interakcije s IoT koncentratora i svake kategorije nadzora sadrži shemu koja definira kako su strukturirane događaje iz te kategorije.

### <a name="device-identity-operations"></a>Operacije identiteta uređaja

Kategoriju uređaj identiteta operacije Prati pogreške koje se javljaju prilikom pokušaja stvaranje, ažuriranje i Brisanje unosa u registru identiteta IoT koncentratora. Praćenje kategoriju je korisno za dodjelu resursa scenarijima.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Telemetrijskih uređaj

Kategorija za telemetriju uređaj Prati pogreške koje se pojavljuju u središtu IoT i vezani uz telemetrijskih kanal. Ova kategorija uključuje pogreške koje se javljaju prilikom slanja telemetrijskih događaja (kao što su ograničavanje) i primanje telemetrijskih događaja (kao što je neovlašteno reader). Imajte na umu da kategoriju nije moguće Uhvatite pogreške uzrokovanih kod koji se izvodi na samom uređaju.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Oblak uređaj naredbe

Kategoriju oblaka uređaj naredbe Prati pogreške koje se pojavljuju u središtu IoT i vezani uz naredbe kanala uređaja. Ova kategorija uključuje pogreške koje se pojavljuju pri slanju naredbe (kao što su neovlašteno pošiljatelja), dobivate naredbe (kao što su isporuke broja skokova), i primanju naredba povratnih informacija (kao što su povratne informacije istekao). Ova kategorija pojavljivanje pogreške s uređaja koji nepravilno rukuje naredbu Ako se naredba uspješno je isporučen.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Veze

Kategorije veze Prati pogreške koje se javljaju prilikom uređaja povezivanje ili prekid veze s koncentratora za IoT. Praćenje kategoriju je korisno za prepoznavanje pokušaje neovlaštenog povezivanja i praćenja kada je veza izgubiti za uređaje u područja nisku povezivanje.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Datoteka

Kategorija za prijenos datoteka Prati pogreške koje se pojavljuju u središtu IoT i povezane funkcije za prijenos datoteka. Ova kategorija uključuje pogrešaka koje će se pojaviti prilikom SAS URI (primjerice kada istekne prije uređaj obavještava koncentrator dovršene prijenos), neuspjelih prijenosa potrebnom za uređaj, a kada se datoteka nije pronađen u prostor za pohranu prilikom stvaranja poruke za IoT koncentrator obavijesti. Imajte na umu kategoriju nije moguće Uhvatite pogreške koje se izravno pojavljuju tijekom uređaj prijenosa datoteke za pohranu.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Daljnji koraci

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md