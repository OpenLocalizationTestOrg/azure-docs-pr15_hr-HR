<properties
    pageTitle="Stvaranje koncentratora za IoT pomoću Azure EŽA | Microsoft Azure"
    description="Slijedite ovaj članak da biste stvorili koncentratora za IoT pomoću sučelja Azure naredbenog retka."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Stvaranje koncentratora za IoT pomoću EŽA Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Uvod

Azure sučelje naredbenog retka možete koristiti za stvaranje i upravljanje Azure IoT koncentratora programski. U ovom se članku objašnjava korištenje EŽA Azure da biste stvorili koncentratora za IoT.

Da biste dovršili ovaj Praktični vodič potrebno je sljedeće:

- Aktivni Azure račun. Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.
- [Azure EŽA 0.10.4] [ lnk-CLI-install] ili noviji. Ako već imate Azure EŽA možete provjeriti na trenutnu verziju u naredbenom retku pomoću sljedeće naredbe:
```
    azure --version
```

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa Azure i classic](../resource-manager-deployment-model.md). Azure EŽA mora biti u načinu Azure Voditelj resursa:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Postavite račun za Azure i pretplate 

1. Prilikom prijave naredbeni redak tako da upišete sljedeću naredbu
```
    azure login
```
Korištenje preporučenih web-preglednik i kod za provjeru autentičnosti.

2. Ako imate više pretplata Azure, povezivanje s Azure će dopustiti pristup pridružene vjerodajnice za sve pretplate za Azure. Možete pogledati Azure pretplate, kao i koji je po zadanom, pomoću naredbe
```
    azure account list 
```

Da biste postavili pretplate kontekst u kojoj želite pokrenuti ostatak pomoću naredbe

```
    azure account set <subscription name>
```

3. Ako nemate grupu resursa možete stvoriti jedan imenovani **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] U članku [Korištenje EŽA Azure upravljanja Azure resursa i grupa resursa] [ lnk-CLI-arm] navedene su dodatne informacije o načinu korištenja Azure EŽA da biste upravljali Azure resursi. 


## <a name="create-an-iot-hub"></a>Stvaranje koncentratora za IoT

Potrebne parametri:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Da biste vidjeli sve parametre dostupne za stvaranje koristite naredbu pomoć u naredbeni redak
```
    azure iothub create -h 
```
Brzi primjer:

 Da biste stvorili koncentratora za IoT pod nazivom **exampleIoTHubName** u resursa grupe **exampleResourceGroup** jednostavno pokrenite sljedeću naredbu
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Ta se naredba EŽA Azure stvara sustava S1 standardni IoT koncentrator za koje se naplaćuju. Možete izbrisati IoT koncentrator **exampleIoTHubName** pomoću iza naredbe 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o razvoju za IoT koncentrator potražite u sljedećim člancima:
- [Koncentrator IoT SDK-ovi][lnk-sdks]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Da biste upravljali IoT koncentrator pomoću portala za Azure][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
