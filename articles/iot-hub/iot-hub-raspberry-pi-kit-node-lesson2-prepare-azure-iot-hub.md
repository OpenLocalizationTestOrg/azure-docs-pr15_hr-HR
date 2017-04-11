<properties
 pageTitle="Stvaranje koncentratora za IoT i registrirati Pi 3-a Raspberry | Microsoft Azure"
 description="Stvaranje grupe resursa, stvorite koncentratora za Azure IoT i registrirati svoje Pi u središtu Azure IoT pomoću EŽA Azure."
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

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 Stvaranje koncentratora za IoT i registrirati svoje Raspberry Pi 3

## <a name="221-what-you-will-do"></a>2.2.1 što ćete učiniti

- Stvorite grupu resursa.
- Stvaranje koncentratora za Azure IoT u grupu resursa.
- Dodajte Pi 3-a Raspberry koncentrator Azure IoT pomoću EŽA Azure.

Kada koristite EŽA Azure da biste dodali svoje Pi koncentratora za IoT, servis generira ključ za vaše Pi za provjeru autentičnosti sa servisom. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="222-what-you-will-learn"></a>2.2.2 što ćete saznati

- Kako koristiti EŽA Azure da biste stvorili programa Azure IoT koncentratora.
- Upute za stvaranje uređaja identiteta za vaše Pi u koncentratora za IoT Azure.

## <a name="223-what-you-need"></a>2.2.3 što vam je potrebno

- Račun za Azure
- Mac ili na računalu sa sustavom Windows s EŽA Azure instaliran

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 stvaranje koncentratora za Azure IoT

Azure IoT koncentrator omogućuje povezivanje, praćenje i upravljanje milijune IoT resursi. Da biste stvorili koncentratora za Azure IoT, slijedite ove korake:

1. Prijavite se na račun za Azure ponovnim pokretanjem sljedeće naredbe:

    ```bash
    az login
    ```

    Nakon uspješne prijava navedene su sve dostupne Azure pretplate.

2. Postavljanje zadanog Azure pretplatu koju želite koristiti tako da pokrenete sljedeću naredbu:

    ```bash
    az account set -n {subscription id or name}
    ```

    ID pretplate ili naziv pronaći ćete u izlaz `az login`.

3. Da biste registrirali davatelj ponovnim pokretanjem sljedeće naredbe:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Davatelj morate se registrirati prije uvođenja Azure resursa koji omogućuje davatelja usluga.

    > [AZURE.NOTE] Većina davatelja usluga su registrirani automatski prema portalu Azure ili EŽA Azure koristite, ali ne svim. Dodatne informacije o davatelju potražite u članku [Otklanjanje uobičajenih Azure implementacije pogrešaka s Azure Voditelj resursa](../resource-manager-common-deployment-errors.md)

4. Stvorite grupu resursa pod nazivom iot uzorka u regiji Zapad sad ponovnim pokretanjem sljedeće naredbe:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Stvaranje koncentratora za IoT u grupi resursa iot uzorka ponovnim pokretanjem sljedeće naredbe:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    Zadani izdanje sustava središtu IoT stvorite je **F1**koji je **besplatno**. Dodatne informacije potražite u članku [Azure IoT koncentrator cijene](https://azure.microsoft.com/pricing/details/iot-hub/).

> [AZURE.NOTE] Naziv koncentratora za IoT mora biti globalno jedinstveni.
>
> U odjeljku pretplate Azure možete stvoriti samo jedan izdanje **F1** Azure IoT koncentratora.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 registrirati svoje Pi u koncentratora za IoT

Svaki uređaj koji se šalje i prima poruke iz koncentratora za Azure IoT mora biti registriran u jedinstveni ID-a.

Registrirati svoje Pi u koncentratora za Azure IoT po izvodi sljedeću naredbu:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 sažetak

Ste napravili koncentratora za Azure IoT i registriran uređaju identitetu vaše Pi u koncentratora za Azure IoT. Spremni ste prijeći na sljedeću nastave, upute za slanje poruka iz vaše Pi koncentratora za IoT.

## <a name="next-steps"></a>Daljnji koraci

Sada ste spremni da biste započeli [Stvaranje Azure funkcija aplikacije i račun Azure prostora za pohranu za obradu i spremanje poruka koncentrator IoT](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)nastave 3 koji počinje.