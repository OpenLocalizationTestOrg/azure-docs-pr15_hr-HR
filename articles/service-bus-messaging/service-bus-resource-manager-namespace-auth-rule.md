<properties
    pageTitle="Stvaranje pravila autorizacije Bus servis pomoću predloška Azure Voditelj resursa | Microsoft Azure"
    description="Stvaranje pravila autorizacije Bus servisa za polje naziva i reda čekanja pomoću predloška Azure Voditelj resursa"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Stvaranje pravila autorizacije Bus servisa za polje naziva i reda čekanja pomoću predloška Azure Voditelj resursa

U ovom se članku prikazuje kako koristiti predložak Azure Voditelj resursa koji stvara [pravilo za provjeru autentičnosti](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) za servis Bus prostor naziva i red. Ćete saznati kako da biste definirali resursa koji uvode se i navedene upute za definiranje parametara koji su kada se izvršava uvođenje. Pomoću ovog predloška za vlastite implementacije ili prilagoditi ga prema svojim potrebama.

Dodatne informacije o stvaranju predložaka potražite [Voditelj resursa za Azure za izradu predložaka][].

Dovršavanje predloška potražite u članku [predložak pravila za provjeru autentičnosti Bus servisa][] na GitHub.

>[AZURE.NOTE] Dostupni za preuzimanje i implementaciju su sljedeći predlošci Azure Voditelj resursa.
>
>-    [Stvaranje programa događaja koncentratora prostor naziva s grupom koncentratora za događaj i potrošača](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Stvaranje servisa Bus prostor naziva s reda čekanja](service-bus-resource-manager-namespace-queue.md)
>-    [Stvaranje servisa Bus prostor naziva s tema i pretplate](service-bus-resource-manager-namespace-topic.md)
>-    [Stvaranje servisa Bus prostor naziva](service-bus-resource-manager-namespace.md)
>
>Da biste provjerili najnovije predlošci, posjetite Galerija [Predložaka za brzi početak rada Azure][] i traženje "Servisa Bus."

## <a name="what-will-you-deploy"></a>Što će implementacije?

Pomoću ovog predloška će implementirati pravila autorizacije Bus servisa za polje naziva i razmjenu entitet (u ovom slučaju reda).

Ovaj predložak koristi [Zajednički pristup potpis (SAS)](service-bus-sas-overview.md) za provjeru autentičnosti. SAS omogućuje aplikacije za provjeru autentičnosti Bus servis pomoću tipkovnog konfiguriran naziva i razmjenu entitet (reda čekanja ili temu) koji su povezani određena prava. Zatim možete koristiti taj ključ za generiranje SAS tokena koje klijenti mogu koristiti u nizu za provjeru autentičnosti Bus servisa.

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Naziv pravila za provjeru autentičnosti za prostor za naziv.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Naziv reda čekanja u prostor za naziv Bus servisa.

```
"serviceBusQueueName": {
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

Stvara standardne prostor naziva servisa Bus vrste **poruka**i pravilo o autorizacije Bus servisa za polje naziva i entitet.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementaciju

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure EŽA

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili i implementirati resurse pomoću upravitelja resursa Azure, Saznajte kako upravljati sljedećim resursima pregledom ovih članaka:

- [Upravljanje Bus servisa sa servisom PowerShell](service-bus-powershell-how-to-provision.md)
- [Upravljanje resursima servisa Bus pomoću programa Explorer Bus za servis](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Bus usluge provjere autentičnosti i autorizacije](service-bus-authentication-and-authorization.md)

  [Voditelj resursa Azure omogućeno]: ../resource-group-authoring-templates.md
  [Predlošci Azure brzi početak rada]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Predložak pravila za provjeru autentičnosti Bus servisa]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
