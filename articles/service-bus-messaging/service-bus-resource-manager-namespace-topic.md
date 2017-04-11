<properties
    pageTitle="Stvaranje servisa Bus prostor naziva s tema i pretplata pomoću predloška Azure Voditelj resursa | Microsoft Azure"
    description="Stvaranje servisa Bus prostor naziva s tema i pretplata pomoću predloška Azure Voditelj resursa"
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
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Stvaranje servisa Bus prostor naziva s tema i pretplata pomoću predloška Azure Voditelj resursa

U ovom se članku prikazuje kako koristiti predložak Azure Voditelj resursa koji stvara servisa Bus prostor naziva s teme i pretplate. Ćete saznati kako da biste definirali resursa koji uvode se i navedene upute za definiranje parametara koji su kada se izvršava uvođenje. Pomoću ovog predloška za vlastite implementacije i prilagoditi ga prema svojim potrebama

Dodatne informacije o stvaranju predložaka potražite [Voditelj resursa za Azure za izradu predložaka][].

Dovršavanje predloška potražite u članku predložak [servisa Bus prostor naziva s tema i pretplate][] .

>[AZURE.NOTE] Dostupni za preuzimanje i implementaciju su sljedeći predlošci Azure Voditelj resursa.
>
>-    [Stvaranje servisa Bus prostor naziva s reda čekanja i autorizacije pravila](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Stvaranje servisa Bus prostor naziva s reda čekanja](service-bus-resource-manager-namespace-queue.md)
>-    [Stvaranje servisa Bus prostor naziva](service-bus-resource-manager-namespace.md)
>-    [Stvaranje programa događaja koncentratora prostor naziva s grupom koncentratora za događaj i potrošača](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Da biste provjerili najnovije predlošci, posjetite Galerija [Predložaka za brzi početak rada Azure][] i traženje "Servisa Bus."

## <a name="what-will-you-deploy"></a>Što će implementacije?

Pomoću ovog predloška će implementirati servis Bus prostor naziva s tema i pretplate.

[Servis Bus teme i pretplata](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) pružaju obrazac jedan-prema-više komunikacije u uzorku *objavljivanja/pretplate* .

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

Pomoću upravitelja Azure resursa, definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje sekciji pod nazivom `Parameters` koji sadrži sve vrijednosti parametra. Morate definirati parametar za te vrijednosti koje će se razlikovati ovisno o projektu implementirate ili ovise o okruženju implementacije da biste. Definiranje parametara za vrijednosti koje se uvijek će ostati isto. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su raspoređeni.

Predložak definira sljedećih parametara.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Naziv naziva Bus servisa da biste stvorili.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Naziv teme stvorene u prostor za naziv Bus servisa.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Naziv pretplate stvorene u prostor za naziv Bus servisa.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

API usluge Bus verziju predloška.

```
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Resursi za implementaciju

Stvara standardne prostor naziva servisa Bus vrste **poruka**s tema i pretplate.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementaciju

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure EŽA

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili i implementirati resurse pomoću upravitelja resursa Azure, Saznajte kako upravljati sljedećim resursima pregledom ovih članaka:

- [Upravljanje Bus servisa sa servisom PowerShell](service-bus-powershell-how-to-provision.md)
- [Upravljanje resursima servisa Bus pomoću programa Explorer Bus za servis](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Voditelj resursa Azure omogućeno]: ../resource-group-authoring-templates.md
  [Predlošci Azure brzi početak rada]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Servis Bus prostor naziva s tema i pretplate]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
