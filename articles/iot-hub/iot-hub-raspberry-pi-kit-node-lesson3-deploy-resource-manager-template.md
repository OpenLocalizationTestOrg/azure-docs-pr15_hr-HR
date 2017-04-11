<properties
 pageTitle="Stvaranje aplikacije Azure funkcija i račun za Azure pohranu | Microsoft Azure"
 description="Aplikaciju Azure funkcija očekuje podatke na Azure IoT koncentrator događaje, obrađuje dolazne poruke i zapisuje spremište tablica platforme Azure."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 stvaranje Azure funkcija aplikacije i račun za pohranu za Azure

[Funkcija Azure](../../articles/azure-functions/functions-overview.md) je rješenje jednostavno radi male dijelove koda, pod nazivom "funkcije" u oblaku. Aplikaciju Azure funkcija hostira izvođenja funkcije u Azure.

## <a name="311-what-will-you-do"></a>3.1.1 što ćete učiniti

Stvorite aplikaciju Azure funkcija i račun za pohranu Azure pomoću predloška Azure Voditelj resursa. Aplikaciju Azure funkcija očekuje podatke na Azure IoT koncentrator događaje, obrađuje dolazne poruke i zapisuje spremište tablica platforme Azure. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="312-what-will-you-learn"></a>3.1.2 će Saznanja

- Kako koristiti [Azure Voditelj resursa](../../articles/azure-resource-manager/resource-group-overview.md) za implementaciju Azure resursi.
- Kako koristiti aplikaciju Azure funkcija obradu IoT koncentrator poruka i pisanje ih u tablicu u spremište tablica platforme Azure.

## <a name="313-what-do-you-need"></a>3.1.3 što vam je potrebno

- Morate uspješno dovršite prethodne predavanja: [Početak rada s Pi 3-a Raspberry](iot-hub-raspberry-pi-kit-node-get-started.md) i [Stvaranje koncentratora za Azure IoT](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 otvorite aplikaciju uzorka

Otvaranje projekta uzorak u Visual Studio kod ponovnim pokretanjem sljedeće naredbe:

```bash
cd Lesson3
code .
```

![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- Na `app.js` datoteka u na `app` podmapu je ključa izvornu datoteku. U ovom izvorna datoteka sadrži kod da biste poslali poruku 20 puta koncentratora za IoT i titranja LED za svaku poruku šalje.
- U `arm-template.json` je Voditelj resursa Azure predložak koji sadrži aplikaciju Azure funkcija i račun za Azure prostora za pohranu datoteka.
- Na `arm-template-param.json` datoteka je konfiguracijska datoteka koristi predložak Azure Voditelj resursa.
- Na `ReceiveDeviceMessages` podmapu sadrži šifru Node.js za funkciju Azure.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 konfiguriranje predložaka Azure Voditelj resursa i stvaranje resursa u Azure

Ažuriranje na `arm-template-param.json` datoteke u Visual Studio kod.

![Parametri predložaka Azure Voditelj resursa](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Zamijenite **[naziv IoT koncentrator]** **{naziva koncentrator}** koji ste naveli u [2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
- Zamijenite sve prefiks želite **[prefiks niz za nove resurse]** . Prefiks osigurava naziv resursa globalno jedinstveni da biste izbjegli sukoba. Koristite crtice ili broj početnog u prefiks.

> [AZURE.NOTE] Ne morate `azure_storage_connection_string` u ovom odjeljku. Nastavak kao što je.

Nakon što ažurirate na `arm-template-param.json` datoteka, implementacija resurse Azure ponovnim pokretanjem sljedeće naredbe:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Da biste stvorili ove resurse potrebno pet minuta. Dok je u tijeku je stvaranje resursa, možete premjestiti na sljedeću sekciju.

## <a name="316-summary"></a>3.1.6 sažetak

Stvorite Azure funkcija aplikacije za obradu IoT koncentrator poruke i račun za Azure prostora za pohranu za pohranu te poruke. Možete premjestiti na sljedeći odjeljak i pokrenite uzorka za slanje poruka uređaj u oblak na vašem Pi.

## <a name="next-steps"></a>Daljnji koraci

[3,2 pokretanje aplikacije uzorka za slanje poruka uređaj u oblak na vašem Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

