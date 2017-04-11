<properties
 pageTitle="Remote praćenja unaprijed konfigurirane rješenje vodič | Microsoft Azure"
 description="Opis Azure IoT unaprijed konfigurirane rješenje udaljene nadzor i njegova arhitektura."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Udaljena nadzor unaprijed konfigurirane vodič rješenja

## <a name="introduction"></a>Uvod

Paket IoT daljinskog [unaprijed konfigurirane rješenja] za nadzor[ lnk-preconfigured-solutions] implementacija do kraja do kraja nadzire rješenje za više računala koji se izvode na udaljenom mjestima. Rješenje objedinjuje ključa Azure servisi omogućuju generički implementacija scenarija tvrtke te ga možete koristiti kao početnu točku za vlastite implementacije. Možete [prilagoditi] [ lnk-customize] rješenja da bi odgovarao određenim poslovnim potrebama.

U ovom se članku vodit će vas kroz neke ključne elemenata udaljenom rješenja za nadzor koji omogućuje shvatiti način funkcioniranja. U ovom znanja pomaže vam:

- Otklanjanje poteškoća s rješenja.
- Planiranje prilagodbi rješenje vašim vlastitim potrebama. 
- Dizajnirajte vlastiti IoT rješenja koja ih koriste Azure services.

## <a name="logical-architecture"></a>Logička arhitekture

Na sljedećem su dijagramu navode logičke komponente konfiguriranog rješenja:

![Logička arhitekture](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Simulirani uređaja

U unaprijed konfiguriranom rješenje Simulirani uređaj predstavlja nikakvo uređaju (kao što je sastavni air conditioner ili funkcijom zraka rukovanje jedinice). Ako pokrenete konfiguriranog rješenja, automatski Dodjela četiri Simulirani uređaje koji se izvode na [Azure WebJob][lnk-webjobs]. Simulirani uređaje olakšavaju Istraživanje ponašanje rješenja bez potrebe za implementaciju fizičke uređaje. Da biste implementirali real fizički uređaj, potražite u članku [Povezivanje uređaj tako da u alat za analizu daljinske nadzor konfiguriranog rješenje] [ lnk-connect-rm] vodič.

Svaki Simulirani uređaj možete poslati IoT koncentrator sljedeće poruke:

| Poruka  | Opis |
|----------|-------------|
| Pokretanje  | Kada se pokrene uređaj, šalje **uređaj informacije** poruku koja sadrži podatke o sam pozadinska. Ti podaci obuhvaćaju id uređaja, metapodataka za uređaj, a zatim popis naredbi podržava uređaja i trenutnu konfiguraciju uređaja. |
| Podaci o prisutnosti | Na uređaju povremeno šalje poruku **prisutnosti** da biste prijavili li uređaj prepoznati prisutnosti senzora. |
| Telemetrijskih | Na uređaju povremeno šalje poruku **telemetrijskih** koju javlja Simulirani vrijednosti za temperature i humidity prikupljeni s uređaja Simulirani senzori. |


Simulirani uređaji šalju sljedeća svojstva uređaja u poruci **uređaj informacije** :

| Svojstvo               |  Svrha |
|------------------------|--------- |
| ID uređaja              | ID koji je naveden ili dodijeljeni stvaranja uređaj u rješenje. |
| Proizvođača           | Proizvođača uređaja |
| Broj modela           | Broj modela uređaja |
| Serijski broj.          | Serijski broj uređaja |
| Opreme               | Trenutna verzija firmver na uređaju |
| Platforme               | Arhitektura platforme uređaja |
| Procesor              | Procesor koji se izvodi na uređaju |
| Instalirane RAM-a          | Količina RAM-a instalirana na uređaj |
| Koncentrator omogućeno stanja      | Svojstvo stanje koncentrator IoT uređaja |
| Stvoreno vremena           | Vrijeme na uređaju stvorena u rješenje |
| Ažuriranim vremenom           | Zadnji put svojstva su ažurirani za uređaj |
| Zemljopisnu širinu               | Zemljopisnu širinu mjesto uređaja |
| Dužina              | Dužina mjesto uređaja |

Simulator seeds tih svojstava u Simulirani uređaja s ogledne vrijednosti.  Svaki put kada simulator inicijalizira Simulirani uređaja, uređaj objava unaprijed definiranih metapodataka IoT koncentrator. Imajte na umu kako prebrisat će se sve metapodataka ažuriranjima na portalu za uređaj.


Simulirani uređaja možete rukovati poslane na nadzornoj ploči rješenja u koncentratoru IoT sljedeće naredbe:

| Naredba                | Opis                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Šalje _ping_ uređaj da biste provjerili je aktivnosti   |
| StartTelemetry         | Pokreće uređaj slanje telemetrijskih                 |
| StopTelemetry          | Zaustavlja uređaj šalje telemetrijskih             |
| ChangeSetPointTemp     | Mijenja vrijednost Postavi točku oko kojeg se generira slučajni podataka |
| DiagnosticTelemetry    | Pokreće simulator uređaj da biste poslali dodatne telemetrijskih vrijednost (externalTemp) |
| ChangeDeviceState      | Promijeniti svojstvo programa prošireni stanje za taj uređaj te šalje poruku informacije uređaj s uređaja |

Naredba potvrdu uređaj da biste rješenje pozadinskih navedeni su u koncentratoru IoT.

## <a name="iot-hub"></a>IoT koncentratora

[Koncentrator IoT] [ lnk-iothub] ingests podataka koji se šalju iz uređaja u oblak i on postaje dostupan za poslove Azure strujanje analize (ASA). Koncentrator IoT šalje naredbe i na uređajima ime portal za uređaj. Svaki zadatak ASA strujanje koristi zasebnoj grupi potrošača IoT koncentrator za čitanje toka poruka na uređajima.

## <a name="azure-stream-analytics"></a>Azure strujanje Analytics

U udaljene nadzora rješenju [Azure strujanje analize] [ lnk-asa] (ASA) dispatches uređaj poruke koje su stigle prema središtu IoT drugih komponenti pozadinske za obradu ili prostora za pohranu. Različite poslove ASA izvođenje specifične funkcije koje se temelje na sadržaj poruke.

**Posao 1: informacije o uređaju** filtrira uređaj informacije poruke iz ulazne poruke toka i šalje krajnje koncentratora za događaj. Na uređaju šalje uređaj informacije poruke prilikom pokretanja i kao odgovor na naredbu **SendDeviceInfo** . Ovaj zadatak koristi sljedeće definicije upita za pronalaženje poruka na **uređaju informacije** :

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Ovaj zadatak šalje rezultat koncentratora za događaj radi daljnje obrade.

**Posao 2: pravila** procjenjuje dolazne temperature i humidity telemetrijskih vrijednosti na temelju pragovi po uređaja. U uređivaču pravila koja je dostupna na nadzornoj ploči rješenje postavljeni su vrijednosti praga. Svaki par uređaj/vrijednost pohranjuju i vremenske oznake u blob koji strujanje analize čita u obliku **Referentnih podataka**. Posao uspoređuje bilo koju vrijednost koje nisu prazne u odnosu praga Postavljanje uređaja. Ako je prelazi na ' >' uvjeta, posao proizvodi **alarma** događaj koji pokazuje da je premašena praga i omogućuje uređaj, vrijednosti i vremenske oznake vrijednosti. Ovaj zadatak koristi sljedeće definicije upita za prepoznavanje telemetrijskih poruke koje pokreću alarma:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Posao radi daljnje obrade rezultat šalje koncentratora za događaj i sprema pojedinosti o svakom upozorenju blobova iz kojeg rješenje nadzornu ploču možete čita informacija o upozorenjima.

**Posao 3: Telemetrijskih** primjenjuju na dolazne strujanje telemetrijskih uređaj na dva načina. Prvi šalje sve poruke telemetrijskih iz uređaja stalni spremište blobova platforme za Dugoročne prostora za pohranu. Drugi formula izračunava humidity average, minimalne i maksimalne vrijednosti iznad prozora klizača pet minuta i šalje te podatke sa spremištem blobova. Na nadzornoj ploči rješenja čita telemetrijskih podataka iz spremišta blobova za popunjavanje grafikone. Ovaj zadatak koristi sljedeće definicije upita:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Događaj koncentratora

ASA poslove **uređaj informacije** i **pravila** izlaz svoje podatke na događaj koncentratora pouzdano proslijediti na **Događaj procesor** izvodi u na WebJob.

## <a name="azure-storage"></a>Azure prostora za pohranu

Rješenje koristi spremište blobova platforme Azure održati svih neobrađenog i sažete telemetrijskih podataka s uređaja u rješenje. Na nadzornoj ploči čita telemetrijskih podataka iz spremišta blobova za popunjavanje grafikone. Da biste prikazali upozorenja, na nadzornoj ploči čita podatke iz spremišta blobova te zapise kada vrijednosti za telemetriju prekorači konfigurirani praga. Rješenje koristi i spremište blobova platforme za snimanje vrijednosti praga postavite na nadzornoj ploči.

## <a name="webjobs"></a>WebJobs

Osim u kojem se nalazi simulators uređaj, WebJobs u rješenje hostira **Događaj procesor** izvodi u programa Azure WebJob te ručice uređaj informacije poruka te odgovaranje naredbe. Koristi:

- Poruke informacije uređaj da biste ažurirali registra uređaja (koja se pohranjuju u bazi podataka DocumentDB) trenutnim informacijama uređaja.
- Naredba odgovor poruke da biste ažurirali povijest naredbi uređaj (koja se pohranjuju u bazi podataka DocumentDB).

## <a name="documentdb"></a>DocumentDB

Rješenje koristi DocumentDB baze podataka da biste pohranili podatke o uređaja povezanih s rješenja. Te informacije obuhvaćaju metapodataka za uređaj i povijest naredbe slati na uređaje na nadzornoj ploči.

## <a name="web-apps"></a>Web-aplikacije

### <a name="remote-monitoring-dashboard"></a>Udaljena nadzora nadzorne ploče
Ova stranica u web-aplikaciji koristi PowerBI javascript kontrole (potražite u članku [repo PowerBI signale](https://www.github.com/Microsoft/PowerBI-visuals)) vizualizirati telemetrijskih podataka iz uređaja. Rješenje koristi telemetrijskih posao ASA zapisivanje telemetrijskih podataka da biste bloba prostora za pohranu.


### <a name="device-administration-portal"></a>Portal za administraciju uređaja

U ovom web app omogućuje:

- Dodjela resursa za novog uređaja. Ova akcija postavlja id jedinstveni uređaja i generira ključ za provjeru autentičnosti. Informacije o uređaju ga piše identiteta registra IoT koncentratora i rješenje specifične DocumentDB bazu podataka.
- Upravljanje svojstvima uređaja. Ovu akciju sadrži postojeća svojstva za prikaz i ažuriranje s nova svojstva.
- Slanje naredbi na uređaj.
- Prikaz povijesti naredbe za uređaj.
- Omogućivanje i onemogućivanje uređaja.

## <a name="next-steps"></a>Daljnji koraci

Sljedeće članaka za blog TechNet sadrže dodatne detalje o udaljene nadzora konfiguriranog rješenja:

- [IoT paket - Pokaži napredne postavke - udaljene nadzora](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT paket - udaljene nadzor – dodavanje uživo i Simulirani uređaja](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Možete i dalje Uvod u paketu IoT tako da pročitate u sljedećim člancima:

- [Povezivanje uređaja udaljene nadzora konfiguriranog rješenje][lnk-connect-rm]
- [Dozvole za azureiotsuite.com web-mjesta][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md