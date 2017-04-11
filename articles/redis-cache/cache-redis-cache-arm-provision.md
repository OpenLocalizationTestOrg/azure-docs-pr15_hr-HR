<properties 
    pageTitle="Dodjela resursa Redis predmemorije | Microsoft Azure" 
    description="Pomoću predloška Azure Voditelj resursa za implementaciju sustava Azure Redis predmemoriju." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Stvaranje Redis predmemoriju pomoću predloška

U ovoj se temi saznati kako Stvaranje predloška Azure Voditelj resursa koji implementira programa Azure Redis predmemoriju. Da biste zadržali dijagnostičkih podataka mogu se predmemoriju s postojećeg računa za pohranu. I Saznajte kako da biste definirali resursa koji uvode se i navedene upute za definiranje parametara koji su kada se izvršava implementaciju. Pomoću ovog predloška za vlastite implementacije ili prilagoditi ga prema svojim potrebama.

Trenutno dijagnostičkih postavki se zajednički koriste za sve predmemorije u istom području za pretplatu. Ažuriranje jedan predmemorije u regiji utječe na sve ostale predmemorije u regiji.

Dodatne informacije o stvaranju predložaka potražite u članku [Za izradu predložaka Azure Voditelj resursa](../resource-group-authoring-templates.md).

Dovršavanje predloška potražite u članku [Redis predmemorije predložak](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Dostupni su predlošci Voditelj resursa za novog [Premium sloju](cache-premium-tier-intro.md) . 
>
>-    [Stvaranje Premium Redis predmemorije s Klasteriranje](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Stvaranje predmemorije Redis Premium s postojanost podataka](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Stvorite predmemoriju Redis Premium s VNet i neobavezna Klasteriranje](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Da biste provjerili najnovije predložaka, potražite u članku [Azure predložaka za brzi početak rada](https://azure.microsoft.com/documentation/templates/) i traženje `Redis Cache`.

## <a name="what-you-will-deploy"></a>Što će implementacije

U ovom predlošku će implementirati programa Azure Redis predmemorije koji koristi postojeći račun za pohranu za dijagnostičkih podataka.

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

Pomoću upravitelja Azure resursa, definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje nazivaju parametri sekcije koja sadrži sve vrijednosti parametra.
Morate definirati parametar za one vrijednosti koje se razlikuju se ovisno o projektu implementirate ili ovise o okruženju implementacije da biste. Definiranje parametara za vrijednosti koje se uvijek ostaju isti. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su raspoređeni. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Mjesto predmemorije Redis. Ostvarili najbolje performanse, koristite na isto mjesto aplikacija će se koristiti s predmemoriju.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Naziv postojećeg računa za pohranu za dijagnostiku. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Booleova vrijednost koja označava želite li dopustiti pristup putem priključke koji nisu SSL.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Vrijednost koja označava je li omogućen Dijagnostika. Korištenje Uključeno ili ISKLJUČENO.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Resursi za implementaciju

### <a name="redis-cache"></a>Redis predmemorije

Stvara predmemoriju Azure Redis.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementaciju

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure EŽA

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


