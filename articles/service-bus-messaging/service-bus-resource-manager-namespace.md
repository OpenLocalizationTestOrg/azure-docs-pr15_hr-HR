<properties
    pageTitle="Stvorite pomoću predloška resursima servisa Bus prostor naziva | Microsoft Azure"
    description="Stvaranje prostor naziva Bus servis pomoću predloška Azure Voditelj resursa"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Stvaranje polja naziva Bus servis pomoću predloška Azure Voditelj resursa

U ovom se članku opisuje kako koristiti predložak Azure Voditelj resursa koji stvara prostor naziva servisa Bus vrste **poruka** sa standardna/Basic SKU-om. U članku definira i parametre koje su navedene za izvršavanje implementacije. Pomoću ovog predloška za vlastite implementacije ili prilagoditi ga prema svojim potrebama.

Dodatne informacije o stvaranju predložaka potražite [Voditelj resursa za Azure za izradu predložaka][].

Dovršavanje predloška potražite u članku [servis Bus prostora za naziv predloška][] na GitHub.

>[AZURE.NOTE] Dostupni za preuzimanje i implementaciju su sljedeći predlošci Azure Voditelj resursa. 
>
>-    [Stvaranje programa događaja koncentratora prostor naziva s grupom koncentratora za događaj i potrošača](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Stvaranje servisa Bus prostor naziva s reda čekanja](service-bus-resource-manager-namespace-queue.md)
>-    [Stvaranje servisa Bus prostor naziva s tema i pretplate](service-bus-resource-manager-namespace-topic.md)
>-    [Stvaranje servisa Bus prostor naziva s reda čekanja i autorizacije pravila](service-bus-resource-manager-namespace-auth-rule.md)
>
>Da biste provjerili najnovije predlošci, posjetite Galerija [Predložaka za brzi početak rada Azure][] i traženje Bus servisa.

## <a name="what-will-you-deploy"></a>Što će implementacije?

Pomoću ovog predloška će implementirati Bus servisa prostor naziva s [Basic, Standardno, ili Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU-om.

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

Pomoću upravitelja Azure resursa, definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje sekciji pod nazivom `Parameters` koji sadrži sve vrijednosti parametra. Morate definirati parametar za te vrijednosti koje će se razlikovati ovisno o projektu implementirate ili ovise o okruženju implementacije da biste. Definiranje parametara za vrijednosti koje se uvijek će ostati isto. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su raspoređeni.

Ovaj predložak određuje sljedećih parametara.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Naziv naziva Bus servisa da biste stvorili.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU

Naziv servisa Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) da biste stvorili.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Predložak definira vrijednosti koje su dopuštene taj parametar (Basic, Standard ili Premium) i dodjeljuje zadana vrijednost (standardni) ako je naveden nijedna vrijednost.

Dodatne informacije o cijene servisa Bus potražite u članku [servis Bus cijene i naplata][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

API usluge Bus verziju predloška.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Resursi za implementaciju

### <a name="service-bus-namespace"></a>Prostor naziva Bus servisa

Stvara standardne prostor naziva servisa Bus vrste **razmjena poruka**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementacije

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure EŽA

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili i implementirati resurse pomoću upravitelja resursa Azure, Saznajte kako upravljati sljedećim resursima tako da pročitate ovih članaka:

- [Upravljanje Bus servisa sa servisom PowerShell](service-bus-powershell-how-to-provision.md)
- [Upravljanje resursima servisa Bus pomoću programa Explorer Bus za servis](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Voditelj resursa Azure omogućeno]: ../resource-group-authoring-templates.md
  [Servis Bus prostora za naziv predloška]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Predlošci Azure brzi početak rada]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Servis Bus cijene i naplata]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
