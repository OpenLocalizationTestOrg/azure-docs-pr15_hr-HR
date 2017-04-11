<properties
 pageTitle="Čitanje poruka ista i u odjeljku pohrana Azure | Microsoft Azure"
 description="Praćenje poruka uređaja u oblaku kao što su zapisuju se vaše spremište tablica platforme Azure."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3,3 Čitaj poruke ista i u odjeljku pohrana Azure

## <a name="331-what-will-you-do"></a>3.3.1 što ćete učiniti

Praćenje uređaj u oblak poruke koje se šalju iz Pi 3-a Raspberry koncentratora za IoT kao poruke zapisuju se tablica platforme Azure prostora za pohranu. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 će Saznanja

- Kako koristiti čitanje poruka zadatak gulp čitanje poruka ista i na vašem spremište tablica platforme Azure.

## <a name="333-what-do-you-need"></a>3.3.3 što vam je potrebno

- Morate uspješno ste završili rad u prethodnom odjeljku [pokrenuti aplikaciju uzorka Azure titranja na vašem Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 s računa za pohranu čitati nove poruke

U prethodnom odjeljku ste pokrenuli probna aplikacija na vašem Pi. Aplikacije koje se šalju poruke uzorka za koncentratora za Azure IoT. Poruke poslane koncentratora za IoT spremaju se u vašem spremište tablica platforme Azure putem aplikacije Azure funkcija. Potrebno je niz za povezivanje Azure prostora za pohranu za čitanje poruka iz tablica platforme Azure prostora za pohranu.

Da biste pročitali poruke spremljene u vašem spremište tablica platforme Azure, slijedite ove korake:

1. Dohvaćanje niza za povezivanje ponovnim pokretanjem sljedeće naredbe:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Prva naredba dohvaća podatke u `storage name` koja se koristi u drugom naredbi za dohvaćanje niza veze. `iot-sample`Zadana vrijednost je `{resource group name}` ako niste promijenili vrijednost u 2.

2. Otvorite datoteku konfiguracije `config-raspberrypi.json` datoteke u Visual Studio kod ponovnim pokretanjem sljedeće naredbe:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Zamjena `[Azure Storage connection string]` s nizom za povezivanje dobivenog u koraku 1.
4. Spremanje na `config-raspberrypi.json` datoteku.
5. Ponovno slanje poruke i njihovo čitanje iz vaše spremište tablica platforme Azure ponovnim pokretanjem sljedeće naredbe:

    ```bash
    gulp run --read-storage
    ```

    Logike za čitanje iz spremišta tablica platforme Azure se na `azure-table.js` datoteke.

    ![pokretanje – gulp čitanje spremišta](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 sažetak

Uspješno ste povezani vaše Pi koncentratora za IoT u oblaku i koristi titranja primjer aplikacije za slanje poruka uređaj u oblak. Koristiti aplikaciju Azure funkcija za pohranu dolazne poruke IoT koncentrator za vaše spremište tablica platforme Azure. Možete premjestiti na na sljedeći lekciju o slanju oblaka uređaj poruke iz centra za IoT vaše Pi.

## <a name="next-steps"></a>Daljnji koraci

[Lekciju 4: Slanje poruka oblaka uređaja](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
