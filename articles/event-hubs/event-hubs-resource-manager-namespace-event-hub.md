<properties
    pageTitle="Stvaranje programa događaja koncentratora prostor naziva s grupa koncentratora za događaj i potrošača pomoću predloška Azure Voditelj resursa | Microsoft Azure"
    description="Stvaranje programa događaja koncentratora prostor naziva s koncentratora za događaj i korisničke grupe pomoću predloška Azure Voditelj resursa"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Stvaranje programa događaja koncentratora prostor naziva s grupa koncentratora za događaj i potrošača pomoću predloška Azure Voditelj resursa

U ovom se članku prikazuje kako koristiti predložak Azure Voditelj resursa koji stvara se događaj koncentratora prostor naziva s koncentratora za događaj i grupu korisnika. Ćete saznati kako da biste definirali resursa koji uvode se i navedene upute za definiranje parametara koji su kada se izvršava uvođenje. Pomoću ovog predloška za vlastite implementacije i prilagoditi ga prema svojim potrebama

Dodatne informacije o stvaranju predložaka potražite [Voditelj resursa za Azure za izradu predložaka][].

Dovršavanje predloška potražite u [koncentrator za događaj i predložak potrošača grupe][] na GitHub.

>[AZURE.NOTE]
>Da biste provjerili najnovije predlošci, posjetite Galerija [Predložaka za brzi početak rada Azure][] i potražiti koncentratora za događaj.

## <a name="what-will-you-deploy"></a>Što će implementacije?

Pomoću ovog predloška će implementirati događaj koncentratora prostor naziva s koncentratora za događaj i grupu korisnika.

[Događaj koncentratora](../event-hubs/event-hubs-what-is-event-hubs.md) je događaj obrade servis koristili radi olakšavanja događaj i telemetrijskih ingress Azure pri pretraživanje velikog skala s niske latencije i visoke pouzdanosti.

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

Pomoću upravitelja Azure resursa, definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje sekciji pod nazivom `Parameters` koji sadrži sve vrijednosti parametra. Morate definirati parametar za te vrijednosti koje će se razlikovati koji se temelji na projektu implementirate ili ovise o okruženju implementacije za. Definiranje parametara za vrijednosti koje se uvijek će ostati isto. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su raspoređeni.

Predložak definira sljedećih parametara.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Naziv događaja koncentratora naziva da biste stvorili.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Naziv događaja koncentrator stvorena u događaja prostora za naziv koncentratora.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Naziv grupe potrošača stvorene za koncentratora za događaj.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

API verziju predloška.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Resursi za implementaciju

Stvara prostor za naziv vrste **EventHubs**s koncentratora za događaj i grupu korisnika.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementacije

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure EŽA

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Voditelj resursa Azure omogućeno]: ../resource-group-authoring-templates.md
[Predlošci Azure brzi početak rada]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Događaj koncentratora i korisničke grupe predloška]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
