<properties
 pageTitle="Nabavite alate za Azure (Mac OS 10.10) | Microsoft Azure"
 description="Instalirajte Python i sučelje Azure naredbenog retka (Azure EŽA) na Mac OS."
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

# <a name="21-get-azure-tools-macos-1010"></a>2.1 dobili Azure alate (Mac OS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 što ćete učiniti

Instalacija sustava Azure sučelje naredbenog retka (Azure EŽA). Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 što ćete saznati

- Kako se instalira Azure EŽA.
- Kako dodati IoT podgrupa Azure EŽA.

## <a name="213-what-you-need"></a>2.1.3 što vam je potrebno

- Mac s internetskom vezom
- Aktivnu pretplatu Azure. Ako nemate račun, možete stvoriti [pomoću računa](https://azure.microsoft.com/free/) u samo nekoliko minuta.

## <a name="214-install-python"></a>2.1.4 instalacija Python

Iako je Mac OS u sklopu Python 2.7 iz okvir, preporučuje se da biste instalirali Python kroz Homebrew. U odjeljku [Python instalacije na Mac OS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Instalirajte Python i točaka ponovnim pokretanjem sljedeće naredbe:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 instalacija Azure EŽA

Azure EŽA nudi multiplatform sučelje naredbenog retka za Azure, što vam omogućuje da funkcionira izravno iz naredbenog retka za dodjelu resursa i upravljanje resursima. 

Da biste instalirali najnoviju EŽA Azure, slijedite ove korake:

1. Pokrenite sljedeće naredbe u prozoru Terminal. Ponekad je potrebno pet minuta da biste instalirali Azure EŽA.

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Provjera instalacije ponovnim pokretanjem sljedeće naredbe:

    ```bash
    az iot -h
    ```
  
Trebali biste vidjeti sljedeće izlaz ako instalacija ne uspije.

![az iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 sažetak

Instalirate Azure EŽA. Nastavite sa sljedećim odjeljkom da biste stvorili pomoću EŽA Azure Azure IoT koncentratora i uređaja identitet.

## <a name="next-steps"></a>Daljnji koraci

[2.2 Stvaranje koncentratora za IoT i registrirati svoje Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
