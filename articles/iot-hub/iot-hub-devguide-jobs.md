<properties
 pageTitle="Vodič za razvojne inženjere - poslovi | Microsoft Azure"
 description="Azure vodič za razvojne inženjere koncentrator IoT - zakazivanje zadataka da biste pokrenuli na više uređaja povezanih s vaše središte"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Zakazivanje zadataka na više uređaja (pretpregled)

## <a name="overview"></a>Pregled

Kao što je opisano u člancima iz prethodne Azure IoT koncentrator omogućuje broj sastavnih blokova ([Svojstva twin uređaja i oznake] [ lnk-twin-devguide] i [metode oblaka uređaj][lnk-dev-methods]).  Obično IoT pozadinska aplikacija omogućuju administratorima uređaja i operatora za ažuriranje i interakciju s uređaja IoT grupno i zakazano vrijeme.  Poslovi Enkapsulacija izvršavanje uređaj twin ažuriranja i C2D metode uspoređuje sa skupom uređaji istovremeno raspored.  Na primjer, operator koristila pozadinska aplikacija koji želite pokrenuti i praćenje zadatak za ponovno postavljanje uređaja za izgradnju 43 i floor 3 u vrijeme koje se ne bi disruptive operacijama building.

### <a name="when-to-use"></a>Kada se koristi

Razmislite o pomoću kada poslovi: rješenje natrag završili potrebe za planiranje i praćenje tijeka bilo koji od sljedećih aktivnosti na skupu uređaj:

- Ažuriranje svojstava twin želji uređaja
- Ažuriranje uređaj twin oznake
- Pozivanje metode C2D

## <a name="job-lifecycle"></a>Životni ciklus posla

Poslovi pokrenuo pozadinska rješenja i održava IoT koncentratora.  Možete pokrenuti zadatak putem servisa dostupnog URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) i upit za tijek izvršni zadatak putem servisa dostupnog URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Kada se pokrene posao, slanje upita za poslove omogućit će pozadinska aplikacija za osvježavanje status aktivnih zadataka.

> [AZURE.NOTE] Kada pokrenete posla, svojstvo nazive i vrijednosti može sadržavati samo US-ASCII ispisivo alfanumerički osim bilo u sljedećeg kompleta: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Pregled tema:

U sljedećim temama reference sadrže dodatne informacije o korištenju zadatke.

## <a name="jobs-to-execute-direct-methods"></a>Izravni načina za izvršavanje zadataka

Slijedi pojedinosti zahtjev HTTP 1.1 za izvršavanje [Izravni način] [ lnk-dev-methods] na skupu uređajima pomoću posao:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Zadatke da biste ažurirali svojstva twin uređaja

Ovo je pojedinosti zahtjev HTTP 1.1 za ažuriranje svojstava twin uređaja pomoću posao:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Slanje upita za napredak zadataka

Slijedi pojedinosti zahtjev HTTP 1.1 za [slanje upita za zadatke][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
Na continuationToken navedeni su iz odgovor.  

## <a name="jobs-properties"></a>Svojstva poslova

Slijedi popis svojstava i njihovi opisi koji se mogu koristiti kada slanje upita za zadatke ili posao rezultate.

| Svojstvo | Opis |
| -------------- | -----------------|
| **jobId** | Aplikacija navedeni ID-a za posao. |
| **startTime** | Aplikacija prikazuje vrijeme početka (ISO 8601) za zadatak. |
| **endTime** | Koncentrator IoT navedeni datum (ISO 8601) za dovršetka posla. Valjani tek nakon što zadatak je dostigne stanje "dovršeno". | 
| **Vrsta** | Vrste zadataka: |
| | **scheduledUpdateTwin**: posao koristiti da biste ažurirali skup twin želji svojstva ili oznake. |
| | **scheduledDeviceMethod**: koristi se za pozivanje metode uređaj na skupu twin posao. |
| **status** | Trenutno stanje zadatka. Moguće vrijednosti za status: |
| | **na čekanju** : zakazano i one koje čekaju treba izdvojiti usluga zadatka. |
| | **Planirani** : zakazano vrijeme u budućnosti. |
| | **pokretanje** : trenutno aktivan posao. |
| | **Otkazano** : zadatak je otkazan. |
| | **nije uspjelo** : posla nije uspjelo. |
| | **Dovršeno** : zadatak dovršen. |
| **deviceJobStatistics** | Statističkih podataka o izvođenja posla. |

Objekt deviceJobStatistics tijekom pretpregleda, postaje dostupan tek nakon dovršetka posla.

| Svojstvo | Opis |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Broj uređaja u posla. |
| **deviceJobStatistics.failedCount** | Broj uređaja koje nije uspjela posao. |
| **deviceJobStatistics.succeededCount** | Broj uređajima gdje je uspjelo posao. |
| **deviceJobStatistics.runningCount** | Broj uređaja koje su trenutno pokrenuti posao. |
| **deviceJobStatistics.pendingCount** | Broj uređaja koje su na čekanju u Pokreni. |


### <a name="additional-reference-material"></a>Dodatne reference materijala

Druge teme referencu u vodiču za razvojne inženjere obuhvaćaju sljedeće:

- [Krajnje točke koncentrator IoT] [ lnk-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke.
- [Ograničavanje i kvotama] [ lnk-quotas] u članku se opisuje kvote koji se odnose na servis IoT koncentratora i regulacije ponašanje očekivati prilikom korištenja servisa.
- [Koncentrator IoT uređaja i servisa SDK-ovi] [ lnk-sdks] popis različitih jezika SDK-ovi koje koristi u prilikom razvoja uređaja i servisa aplikacije kompatibilne s IoT koncentratora.
- [Upit jezik za twins, postupcima i zadacima] [ lnk-query] u članku se opisuje jezik upita možete koristiti za dohvaćanje informacija iz centra za IoT o uređaju twins, metode i zadacima.
- [Podrška IoT koncentrator MQTT] [ lnk-devguide-mqtt] navedene su dodatne informacije o podršci za IoT koncentrator za protokol MQTT.

## <a name="next-steps"></a>Daljnji koraci

Ako želite isprobati neke koncepta opisane u ovom članku, možda ćete zanimati na središte IoT Praktični vodič:

- [Raspored i emitiranje poslova][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
