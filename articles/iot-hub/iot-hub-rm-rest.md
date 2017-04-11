<properties
    pageTitle="Stvaranje pomoću REST API-JA koncentratora za IoT | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti REST API-JA da biste stvorili koncentratora za IoT."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Praktični vodič: Stvaranje koncentratora za IoT C# programa i REST API-JA

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Uvod

Možete koristiti [davatelja resursa IoT koncentrator REST API -JA] [ lnk-rest-api] za stvaranje i upravljanje Azure IoT koncentratora programski. Pomoću ovog praktičnog vodiča pokazuje kako koristiti davatelja resursa IoT koncentrator REST API-JA za stvaranje koncentratora za IoT iz programa C#.

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: [Voditelj resursa Azure i classic](../resource-manager-deployment-model.md).  U ovom se članku opisuje pomoću modela implementaciju upravljanja resursima Azure.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

- Microsoft Visual Studio 2015.
- Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] ili noviji.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Priprema projekt za Visual Studio

1. U Visual Studio Stvaranje projekta Visual C# Windows pomoću ovog predloška **Aplikacije konzole za** projekt. Naziv projekta **CreateIoTHubREST**.

2. U pregledniku rješenja, desnom tipkom miša kliknite na projektu, a zatim kliknite **Upravljanje NuGet paketa**.

3. U upravitelju Nuget paketa, provjerite **sadrže predizdanja**i potražite **Microsoft.Azure.Management.ResourceManager**. Kliknite **Instaliraj**, u **Pregledaj promjene** kliknite **u redu**, a zatim kliknite **prihvaćam** da biste prihvatili licence.

4. U upravitelju Nuget paketa, potražite **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Kliknite **Instaliraj**, u **Pregledaj promjene** kliknite **u redu**, a zatim kliknite **prihvaćam** da biste prihvatili licencu.

6. U Program.cs, zamijenite postojeće **pomoću** naredbe sljedeće:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. U Program.cs, dodajte sljedeće statične varijable zamjenu vrijednosti rezervirano mjesto. Ranije u ovom praktičnom vodiču ste bilješku o **ApplicationId**, **SubscriptionId**, **TenantId**i **lozinku** . **Naziv grupe resursa** je naziv grupe resursa koje koristite prilikom stvaranja središtu IoT, može biti unaprijed postojeću grupu resursa ili novi. **Koncentrator IoT naziv** je naziv središtu IoT stvorite, kao što su **MyIoTHub** (taj naziv mora biti globalno jedinstveni da bi trebali sadržavati svoje ime ili inicijale). **Uvođenje naziv** je naziv za implementaciju, kao što su **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Stvaranje koncentratora za IoT pomoću REST API-JA

Korištenje [IoT koncentrator REST API -JA] [ lnk-rest-api] da biste stvorili koncentratora za IoT u grupu resursa. Da biste uredili postojeću koncentrator za IoT možete koristiti i REST API-JA.

1. Program.cs dodajte na sljedeći način:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Način **CreateIoTHub** da biste stvorili objekt **HttpClient** s token za provjeru autentičnosti u zaglavljima dodati sljedeći kod:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Dodavanje koda za sljedeće metodu **CreateIoTHub** opisuje središtu IoT da biste stvorili i generiranje JSON predstavljanje (trenutni popis mjesta koja podržavaju IoT koncentrator potražite u članku [Azure Status][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Dodajte sljedeći kod metodu **CreateIoTHub** pošaljite zahtjev za OSTALE za Azure, provjerite odgovor i dohvaćanje URL možete koristiti za praćenje stanja zadatka implementacije:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Do kraja metodu **CreateIoTHub** da biste koristili adresu **asyncStatusUri** dohvatiti u prethodnom koraku čekanja za implementaciju da biste dovršili dodati sljedeći kod:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Do kraja metodu **CreateIoTHub** za dohvaćanje tipke središtu IoT ste stvorili te ih ispisati na konzoli dodati sljedeći kod:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Dovršavanje i pokrenuti aplikaciju

Sada možete dovršiti aplikacija tako da nazovete metodu **CreateIoTHub** prije stvaranja i ga pokrenuti.

1. Dodajte sljedeći kôd do kraja **Glavna** načina:

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Kliknite **Stvaranje** , a zatim **Stvaranje rješenja**. Ispravljanje pogreške.

3. Kliknite **ispravljanje pogrešaka** , a zatim **Pokrenite ispravljanje pogrešaka** da biste pokrenuli aplikaciju. Može potrajati nekoliko minuta za implementaciju da biste pokrenuli.

4. Možete provjeriti vaše aplikacije dodali novi koncentrator IoT tako što ste posjetili [Azure portal] [ lnk-azure-portal] i prikaz popisa resursa ili pomoću cmdleta komponente PowerShell **Get-AzureRmResource** .

> [AZURE.NOTE] U ovom primjeru aplikacije dodaje se S1 standardni IoT koncentrator za koje se naplaćuju. Kada završite, možete izbrisati središtu IoT putem [portala za Azure] [ lnk-azure-portal] ili pomoću cmdleta ljuske PowerShell **Ukloni AzureRmResource** kada završite.

## <a name="next-steps"></a>Daljnji koraci

Sada ste implementiran koncentratora za IoT pomoću REST API-JA, preporučujemo vam da biste istražili dodatne:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
