<properties
 pageTitle="Otklanjanje poteškoća s | Microsoft Azure"
 description="Stranica za Raspberry Pi Node.js sučelje za otklanjanje poteškoća"
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

# <a name="troubleshooting"></a>Otklanjanje poteškoća

## <a name="hardware-issues"></a>Hardverske probleme

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Aplikaciju i pokreće, ali se LED je titranje

Taj se problem uvijek povezanima s povezivanjem elektronička hardvera. Poduzmite sljedeće korake da biste otklonili poteškoće.

1. Potvrdite okvir ako se odlučite ispravni **GPIO** na panel za. Dva priključke u ovom nastave mora biti **GPIO GND (6 PIN-a)** i **GPIO 04 (PIN-a 7)**.
2. Provjera je li polaritet vaše LED točan. Više leg trebali biste naznačili **pozitivan**, anode PIN-a.
3. Korištenje **3.3V prikvačiti** i **GND PIN-a** na nadjev od Pi 3. Smatraj Power Kontroler vaše Pi. Provjerite je li se na LED dobro funkcionira.

![Specifikacija LED](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Ostali problemi hardvera

Informacije o rješavanju uobičajenih problema na Raspberry Pi 3 odnose se na [službeni stranice za otklanjanje poteškoća](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problemi s Node.js paketa

### <a name="no-response-during-gulp-tasks"></a>Nema odgovora tijekom gulp zadataka

Ako naiđete na probleme gulp zadataka koji se izvode, možete dodati u `--verbose` mogućnosti za ispravljanje pogrešaka. Pokušajte da biste prekinuli trenutni gulp zadataka s `Ctrl + C` , a zatim pokrenite sljedeću naredbu u prozoru konzole da biste vidjeli poruke za ispravljanje pogrešaka. Možda će se prikazati detaljne poruke o pogreškama ispisuju u izlaz konzolu. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Uređaj otkrivanje problema

Pomoć za otklanjanje uobičajenih poteškoća s tipkom `devdisco` naredbu, provjerite [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Ostali problemi s NPM

Pokušajte ažurirati pakiranju NPM pomoću sljedeće naredbe:

```bash
npm install -g npm
```

Ako se problem i dalje postoji, ostavite komentare na kraju ovog članka, ili stvoriti Github problem u našem [Uzorka spremište](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Daljinsko uklanjanje programskih pogrešaka

### <a name="run-the-sample-application-in-debug-mode"></a>Pokrenite aplikaciju uzorak u načinu rada za ispravljanje pogrešaka

```bash
gulp run --debug
```

Kada je spreman modul za ispravljanje pogrešaka, trebali biste vidjeti ```Debugger listening on port 5858``` izlaza konzolu.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Konfiguriranje Dodavanje veze za VANJSKIH kod za povezivanje s udaljenog uređaja

Otvorite ploču **za ispravljanje pogrešaka** s lijeve strane.

Kliknite zeleni gumb **Start ispravljanje pogrešaka** (F5). Dodavanje veze za VANJSKIH kod otvorit će se **launch.json** datoteka koje je potrebno ažurirati.

Ažuriranje **launch.json** datoteke sa sljedećim sadržajem. Zamjena `[device hostname or IP address]` stvarni uređaj IP adresa ili naziv glavnog računala.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Udaljenom konfiguracije za ispravljanje pogrešaka](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Prilaganje udaljenog računala

Kliknite zeleni gumb **Start ispravljanje pogrešaka** (F5) da biste pokrenuli ispravljanje pogrešaka. 

Pročitajte [JavaScript kod za dodavanje veze za VANJSKIH](https://code.visualstudio.com/docs/languages/javascript#_debugging) da biste saznali više o program za ispravljanje pogrešaka.

![Alat za analizu daljinske interaktivne ispravljanje pogrešaka](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure EŽA problema

Azure EŽA je pretpregled Sastavi. Može se odnositi na [Pretpregled instalacija vodiča](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) za traženje rješenja.

Ako naiđete na bilo kojem programskih pogrešaka pomoću alata, u odjeljku **problemi** GitHub repo datoteka [problem](https://github.com/Azure/azure-cli/issues) .

Pomoć za otklanjanje uobičajenih problema, provjerite [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Problemi s instalacijom Python

### <a name="legacy-installation-issues-macos"></a>Problemi s instalacijom za naslijeđene (Mac OS)

Prilikom instaliranja **točaka**, pogrešku dozvola se Expression.Error kad postoje naslijeđene paketa koje se instaliraju pomoću **su** dozvole. U ovom slučaju pojavljuje jer se prethodne instalacije sustava Python pomoću brew (Mac OS) nije potpuno deinstalirati. Neke **točaka** paketa iz prethodne instalacije je stvorila korijenski koji uzrokuje pogrešku dozvola. Rješenje je da biste uklonili te paketa instalirao korijen. Da biste izvršili taj zadatak, poduzmite sljedeće korake:

1. Idite na /usr/local/lib/python2.7/site-packages
2. Popis paketa stvorili korijenski učinite sljedeće:`ls -l | grep root`
3. Da biste deinstalirali pakete s step2:`sudo rm -rf {package name}`
4. Ponovno instalirajte Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT koncentrator problema

Ako ste uspješno dodjeli koncentrator Azure IoT pomoću `azure-cli`, i potreban vam je alat za upravljanje uređajima povezivanje vašeg IoT koncentrator, pokušajte sljedeće alate:

### <a name="device-explorer"></a>Explorer uređaja

[Uređaj Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) izvodi na lokalnom računalu Windows i povezat će vaše središte IoT u Azure. Komunikaciju s sljedeće [krajnje točke IoT koncentratora](iot-hub-devguide.md):

- *Upravljanje identitetom uređaj* za dodjelu resursa i upravljanje uređajima registriran koncentratora za IoT.
- *Primanje uređaj-u – oblak* koji omogućuje praćenje poruke poslane s uređaja koncentratora za IoT.
- *Slanje oblaka uređaj* koji omogućuje slanje poruka na svojim uređajima iz centra za IoT.

Konfiguriranje sustava `IoT hub connection string` unutar ovaj alat za korištenje njegovim mogućnostima.

### <a name="iot-hub-explorer"></a>Koncentrator IoT Explorer

[Koncentrator IoT Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) je uzorka multiplatform EŽA alat za upravljanje klijentima uređaj. Alat za omogućuje upravljanje uređajima u registru identiteta, praćenje poruke uređaj u oblak i slanje naredbe oblak na uređaj.

Da biste instalirali najnoviju verziju (predizdanje) alat iothub explorer, pokrenite sljedeću naredbu u okruženju sustava naredbenog retka:

```
npm install -g iothub-explorer@latest
```

Da biste dobili dodatnu pomoć za sve naredbe iothub explorer i njihovim parametrima možete koristiti sljedeće naredbe:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Korištenje Azure portal za upravljanje resursi

U sve te predavanja sve funkcije programa EŽA dostupni su za stvaranje i upravljanje sve Azure resurse. Možda želite koristiti [Azure portal](../azure-portal-overview.md) pomoći dodjele resursa i upravljanje njima, Azure resurse za ispravljanje pogrešaka.

## <a name="azure-storage-issues"></a>Problemi s Azure prostora za pohranu

[Microsoft Azure prostora za pohranu (pretpregled)](http://storageexplorer.com) je samostalnu aplikaciju od Microsofta koji vam omogućuje rad s podacima Azure prostora za pohranu u sustavu Windows, Mac OS ni Linux. Ovaj alat omogućuje povezivanje u tablicu i vidjeti podatke u njemu. Koristite ovaj alat poteškoća Azure prostora za pohranu.
