<properties 
    pageTitle="Stvaranje logike aplikacije pomoću predložaka Azure upravljanja resursima u aplikacije servisa za Azure | Microsoft Azure" 
    description="Pomoću predloška Azure Voditelj resursa implementirati aplikaciju prazan logike za definiranje tijekova rada." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Stvaranje logike aplikacije pomoću predloška

Pomoću predloška Azure Voditelj resursa stvorite aplikaciju prazan logike koje je moguće koristiti da biste definirali tijekova rada. Možete definirati resursa koji uvode i upute za definiranje parametara koje su navedene kada se izvršava uvođenje. Pomoću ovog predloška za vlastite implementacije ili prilagoditi ga prema svojim potrebama.

Dodatne informacije o svojstva aplikacije logike potražite u članku [Logike aplikacije tijeka rada za upravljanje API -JA](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Primjeri definiciju sam, potražite u članku [definicije autor logike aplikacije](app-service-logic-author-definitions.md). 

Dodatne informacije o stvaranju predložaka potražite u članku [Za izradu predložaka Azure Voditelj resursa](../resource-group-authoring-templates.md).

Dovršavanje predloška potražite u članku [predloška logike aplikacije](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Što će implementacije

Pomoću ovog predloška implementacija aplikacije logike.

Automatsko pokretanje implementaciju, odaberite gumb za sljedeće:  

[![Implementacija Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Resursi za implementaciju

### <a name="logic-app"></a>Logika aplikacije

Stvara logike aplikacije.

Predlošci za naziv aplikacije logike koristi vrijednost parametra. Postavlja mjesto logike aplikacije na isto mjesto kao grupu resursa. 

Ove određeni definicije izvodi jednom na sat i pings mjesto navedeno u parametru **testUri** . 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementacije

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure EŽA

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
