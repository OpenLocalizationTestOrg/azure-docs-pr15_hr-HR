<properties
    pageTitle="Stvorite pomoću predloška Azure Voditelj resursa i PowerShell koncentratora za IoT | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič za početak rada s predlošcima Voditelj resursa Azure da biste stvorili koncentratora za IoT sa servisom PowerShell."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Stvaranje koncentratora za IoT pomoću komponente PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Uvod

Voditelj resursa Azure možete koristiti za stvaranje i upravljanje Azure IoT koncentratora programski. Pomoću ovog praktičnog vodiča pokazuje kako koristiti predložak Azure Voditelj resursa za stvaranje koncentratora za IoT sa servisom PowerShell.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa Azure i classic](../resource-manager-deployment-model.md).  U ovom se članku opisuje pomoću modela implementaciju upravljanja resursima Azure.

Da biste dovršili ovaj Praktični vodič potrebno je sljedeće:

- Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] ili noviji.

> [AZURE.TIP] U članku [Korištenje Azure PowerShell s Azure Voditelj resursa] [ lnk-powershell-arm] navedene su dodatne informacije o načinu korištenja skripte komponente PowerShell i Voditelj resursa Azure predloške da biste stvorili Azure resursi. 

## <a name="connect-to-your-azure-subscription"></a>Povezivanje s pretplate Azure

U naredbeni redak PowerShell, unesite sljedeću naredbu za prijavu u pretplatu za Azure:

```
Login-AzureRmAccount
```

Da biste otkrili gdje možete implementirati koncentratora za IoT i trenutno podržanih verzija API-JA možete koristiti sljedeće naredbe:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Stvorite grupu resursa koji sadrže vaše središte IoT pomoću sljedeće naredbe u podržani mjesta za IoT koncentratora. U ovom se primjeru stvara grupu resursa **MyIoTRG1**:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Slanje predložak Voditelj resursa Azure da biste stvorili koncentratora za IoT

Pomoću predloška JSON da biste stvorili koncentratora za IoT u grupu resursa. Predložak programa Azure Voditelj resursa možete koristiti i da biste uredili postojeću koncentrator za IoT.

1. Da biste stvorili predložak Voditelj resursa Azure naziva **template.json** pomoću sljedećih resursa definicije da biste stvorili novi standardni IoT koncentrator koristite uređivač teksta. U ovom se primjeru dodaje središtu IoT u regiji **Istočni SAD -a** , stvara dva korisničke grupe (**cg1** i **cg2**) na krajnjoj točki kompatibilan s preglednikom događaja koncentratora i koristi verzije **2016 02 03** API-JA. Ovaj predložak očekuje i prosljeđivanje u koncentratoru nazivu IoT kao parametar pod nazivom **hubName**. Trenutni popis mjesta koja podržavaju IoT koncentrator potražite u članku [Azure Status][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Spremite datoteku predloška Azure Voditelj resursa na lokalno računalo. U ovom se primjeru pretpostavlja da spremite u mapu s nazivom **c:\templates**.

3. Pokrenite sljedeću naredbu za implementaciju novi koncentrator IoT, prosljeđivanje naziv koncentratora za IoT kao parametar. U ovom primjeru je ime središtu IoT **abcmyiothub** (Imajte na umu da taj naziv mora biti globalno jedinstveni da bi trebali sadržavati svoje ime ili inicijale):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Rezultat prikazuje tipke za središtu IoT koju ste stvorili.

5. Možete provjeriti vaše aplikacije dodali novi koncentrator IoT tako što ste posjetili [Azure portal] [ lnk-azure-portal] i prikaz popisa resursa ili pomoću cmdleta komponente PowerShell **Get-AzureRmResource** .

> [AZURE.NOTE] U ovom primjeru aplikacije dodaje se S1 standardni IoT koncentrator za koje se naplaćuju. Možete izbrisati središtu IoT putem [portala za Azure] [ lnk-azure-portal] ili pomoću cmdleta ljuske PowerShell **Ukloni AzureRmResource** kada završite.

## <a name="next-steps"></a>Daljnji koraci

Sada ste implementiran koncentratora za IoT pomoću predloška Azure Voditelj resursa sa servisom PowerShell, preporučujemo vam da biste istražili dodatne:

- Saznajte više o mogućnostima [davatelja resursa IoT koncentrator REST API -JA][lnk-rest-api].
- Pročitajte [Pregled upravitelja resursa Azure] [ lnk-azure-rm-overview] da biste saznali više o mogućnostima Azure Voditelj resursa.

Dodatne informacije o razvoju za IoT koncentrator potražite u sljedećim člancima:

- [Uvod u C SDK][lnk-c-sdk]
- [Koncentrator IoT SDK-ovi][lnk-sdks]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
