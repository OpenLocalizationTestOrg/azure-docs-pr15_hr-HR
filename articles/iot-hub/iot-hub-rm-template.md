<properties
    pageTitle="Stvaranje koncentratora za IoT pomoću programa ARM predloška i C# | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič za početak rada s predlošcima Voditelj resursa Azure da biste stvorili koncentratora za IoT s programom C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Stvaranje koncentratora za IoT pomoću programa C# pomoću predloška Azure Voditelj resursa

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Uvod

Voditelj resursa Azure možete koristiti za stvaranje i upravljanje Azure IoT koncentratora programski. Pomoću ovog praktičnog vodiča pokazuje kako koristiti predložak Azure Voditelj resursa za stvaranje koncentratora za IoT iz programa C#.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa Azure i classic](../resource-manager-deployment-model.md).  U ovom se članku opisuje pomoću modela implementaciju upravljanja resursima Azure.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

- Microsoft Visual Studio 2015.
- Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.
- [Račun za Azure pohranu] [ lnk-storage-account] gdje možete spremati datoteke predloška Azure Voditelj resursa.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] ili noviji.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Priprema projekt za Visual Studio

1. U Visual Studio Stvaranje projekta Visual C# Windows pomoću ovog predloška **Aplikacije konzole za** projekt. Naziv projekta **CreateIoTHub**.

2. U pregledniku rješenja, desnom tipkom miša kliknite na projektu, a zatim kliknite **Upravljanje NuGet paketa**.

3. U upravitelju Nuget paketa, provjerite **sadrže predizdanja**i potražite **Microsoft.Azure.Management.ResourceManager**. Kliknite **Instaliraj**, u **Pregledaj promjene** kliknite **u redu**, a zatim kliknite **prihvaćam** da biste prihvatili licence.

4. U upravitelju Nuget paketa, potražite **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Kliknite **Instaliraj**, u **Pregledaj promjene** kliknite **u redu**, a zatim kliknite **prihvaćam** da biste prihvatili licencu.

5. U Program.cs, zamijenite postojeće **pomoću** naredbe sljedeće:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. U Program.cs, dodajte sljedeće statične varijable zamjenu vrijednosti rezervirano mjesto. Ranije u ovom praktičnom vodiču ste bilješku o **ApplicationId**, **SubscriptionId**, **TenantId**i **lozinku** . **Naziv računa za pohranu za vašu Azure** je naziv računa spremišta Azure gdje spremati datoteke predloška Azure Voditelj resursa. **Naziv grupe resursa** je naziv grupe resursa koje koristite prilikom stvaranja središtu IoT, može biti unaprijed postojeću grupu resursa ili novi. **Uvođenje naziv** je naziv za implementaciju, kao što su **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Slanje predložak Voditelj resursa Azure da biste stvorili koncentratora za IoT

Korištenje JSON predloška i parametar datoteke da biste stvorili koncentratora za IoT u grupu resursa. Predložak programa Azure Voditelj resursa možete koristiti i da biste uredili postojeću koncentrator za IoT.

1. U pregledniku rješenja, desnom tipkom miša kliknite na projektu, kliknite **Dodaj**i zatim kliknite **Nova stavka**. Dodavanje datoteke JSON pod nazivom **template.json** u projekt.

2. Zamijenite sadržaj **template.json** sljedeću definiciju resursa da biste dodali standardni IoT koncentrator s područjem **Istočni SAD -a** (trenutni popis regije koje podržavaju IoT koncentrator potražite u članku [Azure Status][lnk-status]):

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

3. U pregledniku rješenja, desnom tipkom miša kliknite na projektu, kliknite **Dodaj**i zatim kliknite **Nova stavka**. Dodavanje datoteke JSON pod nazivom **parameters.json** u projekt.

4. Zamijenite sadržaj **parameters.json** sljedeće informacije parametar koji postavlja kao što su naziv za novi koncentrator IoT **{svoje inicijale} mynewiothub**. Imajte na umu da naziv koncentrator IoT mora biti globalno jedinstveni da bi trebali sadržavati svoje ime ili inicijale):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. U **Programu Explorer poslužitelja**povezati Azure pretplatu, a na vašem računu za pohranu Azure stvaranje spremnika koji se naziva **predložaka**. Na ploči **svojstava** postavite dozvole **javno čitanje** kontejner s **predlošcima** u **Blob**.

6. U programu **Explorer poslužitelj**, desnom tipkom miša kliknite spremnik **Predlošci** , a zatim kliknite **Prikaz Blob spremnik**. Ikona **Prijenos Blob** , odaberite dvije datoteke, **parameters.json** i **templates.json**, a zatim **Otvori** da biste prenijeli datoteke JSON spremniku **predložaka** . URL-ove blob-ova koji sadrže podatke JSON su:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. Program.cs dodajte na sljedeći način:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. Dodajte sljedeći kod metodu **CreateIoTHub** za slanje datoteka predloška i parametar za Voditelj resursa Azure:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. Dodajte sljedeći kôd **CreateIoTHub** način na koji se prikazuje status i tipki za novi koncentrator IoT:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Dovršavanje i pokrenuti aplikaciju

Sada možete dovršiti aplikacija tako da nazovete metodu **CreateIoTHub** prije stvaranja i ga pokrenuti.

1. Dodajte sljedeći kôd do kraja **Glavna** načina:

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Kliknite **Stvaranje** , a zatim **Stvaranje rješenja**. Ispravljanje pogreške.

3. Kliknite **ispravljanje pogrešaka** , a zatim **Pokrenite ispravljanje pogrešaka** da biste pokrenuli aplikaciju. Može potrajati nekoliko minuta za implementaciju da biste pokrenuli.

4. Možete provjeriti vaše aplikacije dodali novi koncentrator IoT tako što ste posjetili [Azure portal] [ lnk-azure-portal] i prikaz popisa resursa ili pomoću cmdleta komponente PowerShell **Get-AzureRmResource** .

> [AZURE.NOTE] U ovom primjeru aplikacije dodaje se S1 standardni IoT koncentrator za koje se naplaćuju. Možete izbrisati središtu IoT putem [portala za Azure] [ lnk-azure-portal] ili pomoću cmdleta ljuske PowerShell **Ukloni AzureRmResource** kada završite.

## <a name="next-steps"></a>Daljnji koraci

Sada ste implementiran koncentratora za IoT pomoću predloška Azure Voditelj resursa s programom C#, preporučujemo vam da biste istražili dodatne:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
