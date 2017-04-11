<properties
    pageTitle="Korištenje dinamičke telemetrijskih | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste saznali kako koristiti dinamičke telemetrijskih s udaljenog nadzora konfiguriranog rješenja."
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
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Korištenje dinamičke telemetrijskih s udaljenog nadzor unaprijed konfigurirane rješenja

## <a name="introduction"></a>Uvod

Dinamični telemetrijskih omogućuje vizualizacija sve telemetrijskih poslane na udaljenom nadzora konfiguriranog rješenja. Simulirani uređaje koje implementacija konfiguriranog rješenjem pošaljite temperature i humidity telemetrijskih koji se mogu vizualizirati na nadzornoj ploči. Ako prilagoditi postojeće Simulirani uređaja, stvaranje novih Simulirani uređaja ili povezivanje fizičkih uređaja prethodno rješenje možete poslati drugim vrijednostima telemetrijskih kao što su vanjski temperatura, RPM ili windspeed. Zatim možete vizualizirati ovaj dodatne telemetrijskih na nadzornoj ploči.

Pomoću ovog praktičnog vodiča koristi jednostavne Node.js Simulirani uređaj koji možete jednostavno izmijeniti Eksperimentirajte s dinamičke telemetrijskih.

Da biste dovršili ovaj Praktični vodič, morat ćete:

- Aktivnu pretplatu Azure. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Besplatnu probnu verziju Azure][lnk_free_trial].
- [Node.js] [ lnk-node] verziju 0.12.x ili noviji.

Pomoću ovog praktičnog vodiča u operacijskom sustavu, kao što je Windows ili Linux, gdje možete instalirati Node.js mogu provoditi.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Konfiguriranje uređaja Simulirani Node.js

1. Na udaljenom nadzornoj ploči nadzor, kliknite **+ Dodaj uređaj** , a zatim dodajte prilagođeni uređaja. Zabilježite središtu IoT naziv glavnog računala, id uređaja i ključ uređaja. Morate ih kasnije u ovom ćete praktičnom vodiču prilikom pripreme remote_monitoring.js uređaj klijentska aplikacija.

2. Provjerite je li tu verziju Node.js 0.12.x ili kasnije je instaliran na vašem računalu razvoj. Pokrenite `node --version` naredbeni redak ili u ljusci da biste provjerili verziju. Informacije o korištenju upravitelja paketima da biste instalirali Node.js na Linux potražite u članku [Instaliranje Node.js putem upravitelja paketima][node-linux].

3. Nakon instalacije Node.js Kloniraj najnoviju verziju programa [azure-iot-SDK-ovi] [ lnk-github-repo] spremište na vaše računalo razvoj. Uvijek koristi **osnovnu** podružnice za najnoviju verziju biblioteke i uzorka.

4. U lokalnoj kopiji [azure-iot-SDK-ovi] [ lnk-github-repo] spremište, a zatim Kopiraj sljedeće dvije datoteke iz mape čvor/uređaj/uzoraka Isprazni mapu na računalu razvoj:

  - Packages.JSON
  - remote_monitoring.js

5. Otvorite datoteku remote_monitoring.js i potražite definiciju varijable sljedeće:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Zamijenite **[IoT koncentrator uređaj niz za povezivanje]** niz za povezivanje uređaja. Pomoću vrijednosti za naziv glavnog računala IoT koncentrator, id uređaja i ključ uređaja koje ste napravili obratite pozornost na korak 1. Niz za povezivanje uređaja sastoji se od sljedećih oblika:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Ako vaš naziv glavnog računala IoT koncentrator **contoso** a identifikacijskog broja za uređaj **mydevice**, niz za povezivanje izgleda ovako:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Spremite datoteku. Pokrenite sljedeće naredbe u ljusci ili naredbenog retka u mapu koja sadrži ove datoteke da biste instalirali potrebne paketa i izvršavaju primjer aplikacije:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Pridržavajte se dinamički telemetriju u akciji

Na nadzornoj ploči prikazuje telemetrijskih temperature i humidity iz postojeće Simulirani uređaja:

![Zadane nadzorne ploče][image1]

Ako ste odabrali ste pokrenuli u prethodnom odjeljku Node.js Simulirani uređaj, potražite temperature, humidity i vanjske temperatura telemetrijskih:

![Dodavanje vanjskog temperatura nadzorne ploče][image2]

Remote nadzora rješenje automatski otkriva vrstu telemetrijskih dodatne vanjskih temperatura i dodaje grafikon na nadzornoj ploči.

## <a name="add-a-telemetry-type"></a>Dodavanje vrste telemetrijskih

Sljedeći je korak da biste zamijenili telemetrijskih generira Node.js Simulirani uređaj pomoću novog skupa vrijednosti:

1. Zaustavite Node.js Simulirani uređaj tako da upišete **Ctrl + C** u naredbenom retku ili ljuske.

2. U datoteci remote_monitoring.js, vidjet ćete vrijednosti baza podataka za postojeće temperature, humidity i vanjske temperatura telemetrijskih. Dodajte vrijednost baza podataka za **rpm** na sljedeći način:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Uređaj Simulirani Node.js koristi funkciju **generateRandomIncrement** u datoteci remote_monitoring.js da biste dodali slučajni povećanje vrijednosti baza podataka. Slučajni vrijednost **rpm** dodavanjem redak koda nakon postojeće randomizations na sljedeći način:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Dodajte novu vrijednost rpm opseg JSON uređaj šalje koncentrator IoT:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Pokrenite Node.js Simulirani uređaj koristi sljedeću naredbu:

    ```
    node remote_monitoring.js
    ```

6. Pridržavajte se nova vrsta telemetrijskih RPM koji se prikazuje na grafikonu na nadzornoj ploči:

![Dodavanje RPM nadzorne ploče][image3]

> [AZURE.NOTE] Možda ćete morati onemogućiti, a zatim omogućiti Node.js uređaj na stranici **uređaja** na nadzornu ploču da biste odmah vidjeli promjene.

## <a name="customize-the-dashboard-display"></a>Prilagodba prikaza nadzorne ploče

Poruka **Uređaj informacije** mogu sadržavati metapodatke o telemetrijskih IoT koncentratora možete poslati na uređaju. U ovom metapodataka možete odrediti telemetrijskih vrste uređaja šalje. Promijenite vrijednost **deviceMetaData** u datoteci remote_monitoring.js da biste dodali definiciju **Telemetrijskih** slijedite definicijom **naredbe** . Sljedeći isječak koda prikazuje definiciju **naredbe** (ne zaboravite da biste dodali na `,` nakon definiciju **naredbe** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Remote nadzora rješenje koristi velika i mala slova podudaranje za usporedbu definiciju metapodataka s podacima u telemetrijskih toka.

Dodavanje definiciju **Telemetrijskih** kao što je prikazano u prethodnom koda Promjena ponašanja na nadzornoj ploči. Međutim, metapodatke također mogu sadržavati atribut **riješiti problem** da biste prilagodili prikaz na nadzornoj ploči. Ažurirajte definiciju metapodataka za **Telemetriju** kao što je prikazano u sljedećim isječak:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Sljedeće snimku zaslona prikazuje kako ta promjena mijenja legende grafikona na nadzornoj ploči:

![Prilagodba legende grafikona][image4]

> [AZURE.NOTE] Možda ćete morati onemogućiti, a zatim omogućiti Node.js uređaj na stranici **uređaja** na nadzornu ploču da biste odmah vidjeli promjene.

## <a name="filter-the-telemetry-types"></a>Filtriranje vrste telemetrijskih

Grafikon na nadzornoj ploči se po zadanom prikazuju svaki niz podataka telemetrijskih toka. Možete koristiti metapodataka **Uređaj informacije** izostavlja prikaza telemetrijskih određene vrste na grafikonu. 

Da biste na grafikonu prikazali samo Temperature i Humidity telemetrijskih, izostavite **ExternalTemperature** iz metapodataka za **Telemetriju** **Uređaj informacije** na sljedeći način:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

U **Otvorenom temperatura** više ne prikazuje na grafikonu:

![Filtriranje telemetrijskih na nadzornoj ploči][image5]

Ta izmjena utječe samo na prikaz grafikona. Vrijednosti podataka **ExternalTemperature** i dalje su pohranjena i učinili dostupnima za sve pozadinskog obrada.

> [AZURE.NOTE] Možda ćete morati onemogućiti, a zatim omogućiti Node.js uređaj na stranici **uređaja** na nadzornu ploču da biste odmah vidjeli promjene.

## <a name="handle-errors"></a>Obradu pogrešaka

Za strujanje podataka za prikaz na grafikonu, **Vrsta** u metapodacima **Uređaj informacije** moraju podudarati vrstu podataka vrijednosti za telemetriju. Na primjer, ako metapodatke navodi da je **Vrsta** podataka humidity **int** i **Dvostruki** nalazi se u strujanje telemetrijskih zatim telemetrijskih humidity ne prikazuje na grafikonu. Međutim, **Humidity** vrijednosti i dalje pohranjene i učinili dostupnima za sve pozadinskog obrada.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste vidjeti kako koristiti dinamičke telemetrijskih, dodatne informacije o načinu na koji konfiguriranog rješenja koristi informacije o uređaju: [metapodataka za uređaj informacije u na udaljenom nadzor konfiguriranog rješenje][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
