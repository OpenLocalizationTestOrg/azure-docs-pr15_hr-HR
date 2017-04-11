<properties
    pageTitle="Stvaranje programa događaja koncentratora prostor naziva s koncentratora za događaj i omogućivanje arhivu pomoću predloška Azure Voditelj resursa | Microsoft Azure"
    description="Stvaranje programa događaja koncentratora prostor naziva s koncentratora za događaj i omogućivanje arhivu pomoću predloška Azure Voditelj resursa"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Stvaranje programa događaja koncentratora prostor naziva s koncentratora za događaj i omogućivanje arhivu pomoću predloška Azure Voditelj resursa

U ovom se članku prikazuje kako koristiti predložak Azure Voditelj resursa koji stvara se događaj koncentratora prostor naziva s koncentratora za događaj i omogućuje arhiviranje na koncentratora za događaj. Saznat ćete kako da biste definirali resursa koji uvode se i navedene upute za definiranje parametara koji su kada se izvršava uvođenje. Pomoću ovog predloška za vlastite implementacije i prilagoditi ga prema svojim potrebama

Dodatne informacije o stvaranju predložaka potražite u članku [Upravitelj resursa za Azure za izradu predložaka][].

Dodatne informacije o vježbe i uzorci na konvencije imenovanja Azure resursa, potražite u članku [Azure konvencije imenovanja resursi][].

Dovršavanje predloška potražite u [koncentrator za događaj i omogući arhiviranje predloška][] na GitHub.

>[AZURE.NOTE]
>Da biste provjerili najnovije predlošci, posjetite Galerija [Predložaka za brzi početak rada Azure][] i potražiti koncentratora za događaj.

## <a name="what-you-deploy"></a>Implementacija?

Pomoću ovog predloška implementacija događaj koncentratora prostor naziva s koncentratora za događaj i omogućite arhiva.

[Događaj koncentratora](../event-hubs/event-hubs-what-is-event-hubs.md) je događaj obrade servis koristili radi olakšavanja događaj i telemetrijskih ingress Azure pri pretraživanje velikog skala s niske latencije i visoke pouzdanosti. Arhiviranje koncentratora događaj omogućit će vam automatski izlaganje strujanja podataka u vašem koncentratora događaja u spremište blobova platforme Azure po izboru unutar određenog vremena ili veličine interval vašem odabiru.

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

Pomoću upravitelja Azure resursa, definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje sekciju koja se zove `Parameters` koja sadrži sve vrijednosti parametra. Morate definirati parametar za one vrijednosti koje se razlikuju temelju projekta implementirate ili ovise o okruženju implementacije za. Definiranje parametara za vrijednosti koje se uvijek ostaju isti. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su raspoređeni.

Predložak definira sljedećih parametara.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Naziv događaja koncentratora naziva da biste stvorili.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Naziv događaja koncentrator stvorena u događaja prostora za naziv koncentratora.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Broj dana tijekom kojeg želite poruke ostaju u koncentratora za događaj. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Broj particije u koncentratora za događaj.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Omogućivanje arhiviranje na koncentratora za događaj.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Oblik kodiranja navedete serijalizirati podaci o događaju.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Vremenski interval na kojoj se u arhivu pokreće arhiviranje podataka u spremište blobova platforme Azure.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Veličina interval na kojoj se u arhivu pokreće arhiviranje podataka u spremište blobova platforme Azure.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

U arhivu potreban je račun za pohranu resursa id, da biste omogućili arhiva za željenu Azure prostora za pohranu.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

Spremnik blob mjesto na koje želite da podaci o događaju se arhivirati.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

API verziju predloška.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Resursi za implementaciju

Stvara prostor za naziv vrste **EventHubs**s koncentratora za događaj i omogućuje arhiviranje.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementacije

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure EŽA

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Voditelj resursa Azure omogućeno]: ../resource-group-authoring-templates.md
[Predlošci Azure brzi početak rada]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure resursi konvencija imenovanja]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Koncentrator za događaj i omogući arhiviranje predloška]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
