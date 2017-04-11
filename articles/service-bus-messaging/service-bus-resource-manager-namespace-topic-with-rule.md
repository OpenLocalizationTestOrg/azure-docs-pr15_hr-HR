<properties
    pageTitle="Stvaranje servisa Bus prostor naziva s temom, pretplatu, i pravila pomoću predloška Azure Voditelj resursa | Microsoft Azure"
    description="Stvaranje servisa Bus prostor naziva s temu, pretplate i pravilo pomoću predloška Azure Voditelj resursa"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Stvaranje servisa Bus prostor naziva s temu, pretplate i pravila pomoću predloška Azure Voditelj resursa

U ovom se članku prikazuje kako koristiti predložak Azure Voditelj resursa koji stvara servisa Bus prostor naziva s temu, pretplate i pravila (filtar). Saznat ćete kako da biste definirali resursa koji uvode se i navedene upute za definiranje parametara koji su kada se izvršava uvođenje. Pomoću ovog predloška za vlastite implementacije i prilagoditi ga prema svojim potrebama

Dodatne informacije o stvaranju predložaka potražite u članku [Upravitelj resursa za Azure za izradu predložaka][].

Dodatne informacije o vježbe i uzorci na konvencije imenovanja Azure resursima potražite u članku [Azure konvencije imenovanja resursi][].

Dovršavanje predloška potražite u članku predložak [servisa Bus prostor naziva s temu, pretplatu, i pravila][] .

>[AZURE.NOTE] Dostupni za preuzimanje i implementaciju su sljedeći predlošci Azure Voditelj resursa.
>
>-    [Stvaranje servisa Bus prostor naziva s reda čekanja i autorizacije pravila](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Stvaranje servisa Bus prostor naziva s reda čekanja](service-bus-resource-manager-namespace-queue.md)
>-    [Stvaranje servisa Bus prostor naziva](service-bus-resource-manager-namespace.md)
>-    [Stvaranje servisa Bus prostor naziva s tema i pretplate](service-bus-resource-manager-namespace-topic.md)
>
>Da biste provjerili najnovije predlošci, posjetite Galerija [Predložaka za brzi početak rada Azure][] i traženje Bus servisa.

## <a name="what-will-you-deploy"></a>Što će implementacije?

Pomoću ovog predloška, morate je implementirati servisa Bus prostor naziva s temu, pretplate i pravila (filtar).

[Servis Bus teme i pretplata](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) pružaju obrazac jedan-prema-više komunikacije u uzorku *objavljivanja/pretplate* . Prilikom korištenja teme i pretplate u komponenti distribuirane aplikacije komunikaciju izravno s drugom, umjesto toga mogu razmjenjivati poruke putem temu koja služi kao posrednik. Pretplata na temu nalikuje virtualne reda čekanja koja prima kopije poruka koje su poslane na temu. Filtar na pretplatu omogućuje vam da odredite koje poruke poslane na temu prikazivati u određenoj temi pretplate.

## <a name="what-are-rules-filters"></a>Što su pravila (filtri)?

U mnogim situacijama poruke koje sadrže određene značajke moraju obraditi na različite načine. Da biste to omogućili, možete konfigurirati pretplate da biste pronašli poruke koje ste želji svojstva, a zatim izvršite određene izmjene tih svojstava. Dok je servis Bus pretplate vidjeli sve poruke poslane na temu, možete kopirati samo podskup te poruke red virtualne pretplate. To se postiže pomoću filtara za pretplatu. Da biste saznali više o rules(filters), potražite u članku [servis Bus redova, teme i pretplate][].

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

Pomoću upravitelja Azure resursa, trebali biste definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje sekciji pod nazivom `Parameters` koja sadrži sve vrijednosti parametra. Morate definirati parametar za one vrijednosti koje se razlikuju se ovisno o projektu implementirate ili ovise o okruženju implementacije da biste. Definiranje parametara za vrijednosti koje se uvijek ostaju isti. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su raspoređeni.

Predložak definira sljedećih parametara:

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
### <a name="servicebusrulename"></a>serviceBusRuleName

Naziv rule(filter) stvorene u prostor za naziv Bus servisa.

```
   "serviceBusRuleName": {
   "type": "string",
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

Stvara standardne prostor naziva servisa Bus vrste **poruka**s tema i pretplate i pravila.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementaciju

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure EŽA

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili i implementirati resurse pomoću upravitelja resursa Azure, Saznajte kako upravljati sljedećim resursima pregledom ovih članaka:

- [Upravljanje Azure servisa Bus korištenju automatizacije Azure](service-bus-automation-manage.md)
- [Upravljanje Bus servisa sa servisom PowerShell](service-bus-powershell-how-to-provision.md)
- [Upravljanje resursima servisa Bus pomoću programa Explorer Bus za servis](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Voditelj resursa Azure omogućeno]: ../resource-group-authoring-templates.md
  [Predlošci Azure brzi početak rada]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Azure resursi konvencija imenovanja]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Servis Bus prostor naziva s temu, pretplate i pravila]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Servis Bus redovi, tema i pretplate]:service-bus-queues-topics-subscriptions.md
  
