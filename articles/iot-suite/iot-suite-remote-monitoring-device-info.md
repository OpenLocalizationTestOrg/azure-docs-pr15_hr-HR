<properties
 pageTitle="Uređaj informacije metapodataka u udaljene nadzora rješenje | Microsoft Azure"
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
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Uređaj informacije metapodataka u udaljene nadzor unaprijed konfigurirane rješenja

Remote nadzor konfiguriranog rješenja Azure IoT paket prikazuje se odlučile za upravljanje metapodataka za uređaj. U ovom se članku navode pristup rješenje potrebno da biste omogućili da biste razumjeli:

- Što metapodataka za uređaj pohranjuje rješenja.
- Kako rješenje upravlja metapodataka za uređaj.

## <a name="context"></a>Kontekst

Remote nadzora konfiguriranog rješenje koristi [Azure IoT koncentrator] [ lnk-iot-hub] da biste omogućili uređajima slanje podataka u oblaku. Koncentrator IoT obuhvaća [uređaj identiteta registra] [ lnk-identity-registry] da biste kontrolirali pristup IoT koncentrator. Registar identiteta uređaj IoT koncentrator razlikuje se od daljinskog nadzor specifične za rješenja *uređaj registra* kojoj su pohranjeni podaci metapodataka za uređaj. Udaljena nadzora rješenja koristi [DocumentDB] [ lnk-docdb] baze podataka za implementaciju njegov registra uređaj za pohranu informacija metapodataka za uređaj. [Microsoft Azure IoT referenca arhitektura] [ lnk-ref-arch] opisuje ulogu registra uređaja u standardne IoT rješenja.

> [AZURE.NOTE] Remote nadzora konfiguriranog rješenje zadržava identiteta registra uređaj sinkronizirati s uređajem registra. Oba pomoću isti id uređaja identificirati samo svaki uređaj priključen na IoT koncentrator.

[Koncentrator IoT uređaj upravljanje pretpregled] [ lnk-dm-preview] dodaje značajke IoT koncentratora koje su slične značajke upravljanja informacije na uređaju opisane u ovom članku. Trenutno udaljenom nadzora rješenje koristi samo obično dostupne značajke (GA) u središte IoT.

## <a name="device-information-metadata"></a>Informacije o metapodataka za uređaj

Uređaj informacije metapodataka JSON dokument pohranjuju u bazi podataka DocumentDB registra uređaj ima sljedeću strukturu:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: samom uređaju piše tih svojstava, a uređaj za izdavanje certifikata za te podatke. Drugi primjer svojstva uređaja obuhvaćaju proizvođač, model broj i serijski broj. 
- **DeviceID**: id jedinstveni uređaja. Ta vrijednost je ista u registru identiteta koncentrator IoT uređaja.
- **HubEnabledState**: status uređaja u središte IoT. Ta vrijednost na početku postavljena na **null** dok je uređaj najprije povezuje. Na portalu za rješenja **null** vrijednost predstavlja se kao uređaj koji se "registrirani, no ne postoji."
- **CreatedTime**: vrijeme stvaranja uređaj.
- **DeviceState**: stanje dojavi uređaj.
- **UpdatedTime**: vrijeme na uređaju zadnjeg ažuriranja putem portala za rješenja.
- **SystemProperties**: portal rješenje piše svojstva sustava, a uređaj ima poznavanje tih svojstava. Na primjer svojstvo sustava je **ICCID** Ako rješenje ovlašten s i povezani servisa upravljanje SIM omogućeno uređaja.
- **Naredbe**: popis naredbi uređaj podržava. Uređaj dohvaćaju ti podaci prikazivali rješenja.
- **CommandHistory**: popis naredbi poslao udaljene nadzora rješenje uređaj i status te naredbe.
- **IsSimulatedDevice**: zastavice koja služi za identifikaciju uređaja kao Simulirani uređaj.
- **ID**: Jedinstveni identifikator DocumentDB za taj dokument uređaja.

> [AZURE.NOTE] Informacije o uređaju možete dodati metapodatke opisuje telemetrijskih uređaj šalje IoT koncentrator. Udaljenom nadzora rješenje koristi ovaj metapodataka za telemetriju da biste prilagodili način na koji na nadzornoj ploči prikazuje [dinamični telemetrijskih][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Životni ciklus

Kada prvi put stvorite uređaj na portalu rješenja, rješenja stvara unos u registru njegov uređaja kao što je prikazano prethodno. Većina informacija na početku stubbed, a **HubEnabledState** je postavljena na **null**. Sada rješenje i stvara unos za uređaj u registru identiteta uređaja koje generira tipki uređaj koji se koristi za provjeru s IoT koncentratora.

Kada uređaj najprije spaja rješenje, šalje poruku informacije uređaja. Ova poruka uređaj informacije obuhvaćaju svojstva uređaja proizvođača uređaja, broj modela i serijski broj. Uređaj informacijske poruke uključuje i popis naredbi podržava uređaj uključujući informacije o parametre naredbe. Kada rješenje primi ovu poruku, on se ažurira metapodataka za uređaj informacije u registru uređaja.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Prikaz i uređivanje informacije o uređaju na portalu rješenja

Na popisu uređaja na portalu rješenje prikazuje sljedeća svojstva uređaja kao stupci: **Status**, **DeviceId**, **proizvođač**, **Model broj**, **serijski broj**, **firmver**, **platforme**, **procesor**i **Instaliran RAM -a**. Svojstva uređaja **zemljopisnu širinu** i **dužinu** pogon lokaciju na karti servisa Bing na nadzornoj ploči. 

![Popis uređaja][img-device-list]

Ako kliknete **Uređivanje** u oknu s **Detaljima uređaj** na portalu rješenja, možete urediti tih svojstava. Uređivanje tih svojstava ažurira zapis za uređaj u bazi podataka DocumentDB. Međutim, ako je uređaj šalje poruku informacije ažuriranog uređaja, ga prebrisat će sve promjene na portalu rješenja. Ne možete uređivati **DeviceId**, **naziv glavnog računala**, **HubEnabledState**, **CreatedTime**, **DeviceState**i **UpdatedTime** svojstva na portalu rješenje jer je samo uređaj za izdavanje certifikata preko tih svojstava.

![Uređivanje uređaja][img-device-edit]

Portal za rješenje možete koristiti da biste uklonili uređaj s vašeg rješenja. Kada uklonite uređaj rješenje uklanja metapodataka za uređaj informacije iz registra uređaj rješenja i uklanja unos uređaj u registru identiteta koncentrator IoT uređaja. Prije uklanjanja uređaj, morate ga onemogućiti.

![Uklanjanje uređaja][img-device-remove]

## <a name="device-information-message-processing"></a>Obrada poruke informacije za uređaj

Uređaj informacije poruke koje šalje na uređaju se razlikuju od telemetrijskih poruke. Uređaj informacije poruke sadrže podatke kao što su svojstva uređaja, naredbe možete odgovoriti na uređaju i povijest sve naredbe. Koncentrator IoT sam sadrži poznavanje metapodataka koje se nalaze u poruci informacije o uređaju i obrađuje poruku na isti način kao i obradit će sve poruke koje uređaj u oblak. U udaljene nadzora rješenju [Azure strujanje analize] [ lnk-stream-analytics] posla (ASA) čita poruke iz koncentratora IoT. Analitički strujanje **DeviceInfo** posla filtre za poruke koje sadrže **"Vrstu objekta": "DeviceInfo"** i prosljeđuje instancu **EventProcessorHost** glavnog računala koji se izvodi u posao web. Logika u instanci **EventProcessorHost** koristi id uređaja da biste pronašli zapis DocumentDB za određeni uređaj i ažurirati zapis. Zapis registra uređaj sada obuhvaća podatke kao što su svojstva uređaja, naredbe i povijest naredbi.

> [AZURE.NOTE] Uređaj informacijske poruke je standardni poruka uređaj u oblak. Rješenje razlikuje uređaj informacije poruke i telemetrijskih poruke pomoću ASA upita.

## <a name="example-device-information-records"></a>Primjer uređaj informacije zapisa

Udaljena nadzora konfiguriranog rješenja koristi dvije vrste zapisima s podacima o uređaju: zapisa za Simulirani uređaje uvode se sa rješenja i zapisi za prilagođenu uređaje povezati rješenja.

### <a name="simulated-device"></a>Simulirani uređaja

Sljedeći primjer prikazuje zapisu podataka JSON uređaj za Simulirani uređaj. Ovaj se zapis ima vrijednost za **UpdatedTime**, što znači uređaj je poslao poruku **DeviceInfo** IoT koncentrator. Zapis sadrži nekoliko uobičajenih svojstava uređaja, definira na šest naredbe Simulirani uređaje podržavaju i zastavice **IsSimulatedDevice** postavljena na **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Posebni uređaj

Sljedeći primjer prikazuje zapisu podataka JSON uređaj za prilagođene uređaj i ima **IsSimulatedDevice** postavljenom na **0**. Možete pogledati prilagođene uređaj podržava je dvije naredbe i portalu rješenje je poslao **SetTemperature** naredbu na uređaju:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

U sljedećem odjeljku prikazuju poruke JSON **DeviceInfo** uređaja koji se šalju da biste ažurirali podatke metapodataka za uređaj:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Daljnji koraci

Sada ste završili s učenje kako prilagoditi konfiguriranog rješenja, možete istražiti neke od ostalih značajki i mogućnosti IoT paket unaprijed konfigurirane rješenja:

- [Pregled predvidljivu održavanja unaprijed konfigurirane rješenja][lnk-predictive-overview]
- [Najčešća pitanja o IoT paketa][lnk-faq]
- [Sigurnost IoT od dna prema gore][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
