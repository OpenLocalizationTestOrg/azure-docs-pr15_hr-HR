<properties
 pageTitle="Pokretanje aplikacije uzorka za slanje poruka uređaj u oblak | Microsoft Azure"
 description="Uvođenje i pokretanje aplikacije za uzorak 3 Pi vaše Raspberry koja šalje poruke IoT koncentrator i titranja na LED."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3,2 pokretanje aplikacije uzorka za slanje poruka uređaj u oblak

## <a name="321-what-you-will-do"></a>3.2.1 što ćete učiniti

Implementacija i pokrenite probna aplikacija na vašem Raspberry Pi 3 koje šalje poruke koncentratora za IoT. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2 što ćete saznati

- Kako koristiti alat za gulp implementacije i pokrenuti aplikaciju Node.js uzorka na vašem Pi.

## <a name="323-what-you-need"></a>3.2.3 što vam je potrebno

- Morate uspješno ste završili rad u prethodnom odjeljku u ovom nastave: [Stvaranje aplikacije Azure funkcija i račun Azure prostora za pohranu za obradu i spremanje IoT koncentrator poruka](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 se IoT koncentratora i uređaja veze nizovima

Niz za povezivanje uređaja koristi se za vrijednost Pi povezati koncentratora za IoT. Niz za povezivanje središtu IoT se koristi za povezivanje koncentratora za IoT identitetu uređaja koji predstavlja vaš Pi u središtu IoT.

- Dohvaćanje niza veze za koncentrator IoT ponovnim pokretanjem sljedeće naredbe Azure EŽA:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`je naziv koji ste naveli u 2. Korištenje `iot-sample` kao vrijednost `{resource group name}` ako niste promijenili vrijednost u 2.

- Dohvaćanje niza za povezivanje uređaja ponovnim pokretanjem sljedeće naredbe:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`vodi iste vrijednosti kao onaj koji se koristi s prethodne naredbe. Korištenje `iot-sample` kao vrijednost `{resource group name}` i korištenje `myraspberrypi` kao vrijednost `{device id}` ako niste promijenili vrijednost u 2.

## <a name="325-configure-the-device-connection"></a>3.2.5 konfigurirati vezu uređaja

1. Pokretanje konfiguracijska datoteka ponovnim pokretanjem sljedeće naredbe:

    ```bash
    npm install
    gulp init
    ```

2. Otvorite datoteku konfiguracije uređaj `config-raspberrypi.json` u Visual Studio kod ponovnim pokretanjem sljedeće naredbe:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Provjerite sljedeće zamjena u na `config-raspberrypi.json` datoteke:

  - Zamijenite **[naziv glavnog računala uređaja ili IP adresa]** uređaj IP adresa ili naziv glavnog računala ste dobili od `device-discovery-cli` ili s vrijednošću nasljeđuju od konfiguriran obrađene u 1.
  - Zamjena **[IoT uređaj niz za povezivanje]** s na `device connection string` ste nabavili.
  - Zamjena **[IoT koncentrator niz za povezivanje]** s na `iot hub connection string` ste nabavili.

Ažuriranja u `config-raspberrypi.json` datoteke tako da možete implementirati aplikaciju uzorka s računala.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 uvođenje i pokrenuti aplikaciju uzorka

Implementacija i pokrenuti aplikaciju uzorka vaše Pi ponovnim pokretanjem sljedeće naredbe:

```bash
gulp
```

> [AZURE.NOTE] Zadatak gulp zadani pokreće `install-tools`, `deploy`, a `run` sekvencijalno zadaci. U [1 nastave](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)ste pokrenuli zadaci nakon međusobno zasebno.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 provjerite funkcionira aplikacije uzorka

Trebali biste vidjeti LED koji je povezan s vašeg Pi titranje svake dvije sekunde. Svaki put kada se LED titranja, primjer aplikacije šalje poruku koncentratora za IoT i potvrđuje da je je uspješno poslati poruku koncentratora za IoT. Uz to, za svaku poruku primio središtu IoT poruku ispisa u prozoru konzole. Primjer aplikacije automatski prekida se nakon slanja poruke 20.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 sažetak

Ste implementiran i pokreće novu aplikaciju uzorka titranja vaše Pi slanje poruka uređaj u oblak koncentratora za IoT. Praćenje poruke kao što je pisanju na račun za Azure pohranu možete premjestiti u sljedećem odjeljku.

## <a name="next-steps"></a>Daljnji koraci

[3,3 Čitaj poruke ista i u odjeljku pohrana Azure](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)