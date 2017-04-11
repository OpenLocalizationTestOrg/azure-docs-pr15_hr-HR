<properties
    pageTitle="Stvaranje resursa Bus servis pomoću predložaka Voditelj resursa Azure | Microsoft Azure"
    description="Korištenje predložaka Voditelj resursa Azure da biste automatizirali stvaranje servisa Bus resursi"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Stvaranje resursa Bus servis pomoću predložaka Voditelj resursa Azure

U ovom se članku opisuje kako stvoriti i implementirati Bus servisa i događaja koncentratora resurse pomoću predložaka, PowerShell i davatelja resursa Bus servisa Azure Voditelj resursa.

Azure resursima predložaka olakšavaju definirajte resurse za implementaciju rješenja, a da biste odredili parametre i varijabli koje omogućuju unos vrijednosti za različite okruženja. Predložak se sastoji od JSON i izraza koje možete koristiti za sastavljanje vrijednosti za implementaciju sustava. Detaljne informacije o pisanju Voditelj resursa Azure predložaka i rasprave u obliku predloška potražite u članku [Upravitelj resursa za Azure za izradu predložaka](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] Primjeri u ovom se članku pokazalo kako se koristi Voditelj resursa Azure da biste stvorili prostor za naziv Bus servisa i razmjenu entitet (u redu čekanja). Još primjera predloška potražite u [galeriju predložaka za brzi početak rada Azure][] i potražite "Servisa Bus."

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Predloške Bus servisa i upravljanja resursima koncentratora za događaj

Ove usluge Bus i upravljanja resursima za događaj koncentratora Azure predlošci dostupni su za preuzimanje i implementacija. Kliknite na sljedećim vezama detalje o svakome od njih, s vezama na vezu Predlošci na GitHub: 

- [Stvaranje servisa Bus prostor naziva](service-bus-resource-manager-namespace.md)
- [Stvaranje servisa Bus prostor naziva s reda čekanja](service-bus-resource-manager-namespace-queue.md)
- [Stvaranje servisa Bus prostor naziva s tema i pretplate](service-bus-resource-manager-namespace-topic.md)
- [Stvaranje servisa Bus prostor naziva s reda čekanja i autorizacije pravila](service-bus-resource-manager-namespace-auth-rule.md)
- [Stvaranje programa događaja koncentratora prostor naziva s grupom koncentratora za događaj i potrošača](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Implementacija sa servisom PowerShell

Sljedeći postupak opisuje kako pomoću ljuske PowerShell za implementaciju predložak Azure Voditelj resursa koji stvara **standardne** sloju servisa Bus prostor naziva i reda čekanja unutar mjesta za taj naziv. U ovom se primjeru se temelji na predlošku [Stvaranje servisa Bus prostor naziva s reda čekanja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . Približna tijek rada je na sljedeći način:

1. Instaliranje komponente PowerShell.
2. Stvaranje predloška i (nije obavezno) datoteke parametar.
2. U ljusci PowerShell, prijavite se na račun za Azure.
3. Ako nešto ne postoji, stvorite novu grupu resursa.
4. Testirajte uvođenje.
5. Po želji, postavite u način implementacije.
6. Uvođenje predloška.

Potpune informacije o implementaciji Voditelj resursa Azure predložaka potražite u članku [uvođenja resursi s predlošcima Azure Voditelj resursa][].

### <a name="install-powershell"></a>Instaliranje komponente PowerShell

Instalirajte Azure PowerShell slijedeći upute [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

### <a name="create-a-template"></a>Stvaranje predloška

Kloniraj ili predložak [201-servicebus – stvaranje-reda čekanja](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) kopirali GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Stvaranje datoteke parametara (nije obavezno)

Da biste koristili neobavezni parametri datoteku, kopirajte [201-servicebus – stvaranje-reda čekanja](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) datoteka. Zamjena vrijednosti `serviceBusNamespaceName` pod nazivom prostora za naziv Bus servis koji želite stvoriti u ovom implementacije i zamijenite vrijednost `serviceBusQueueName` pod nazivom reda koju želite stvoriti. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Dodatne informacije u sintaksi [parametar datoteke](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Prijavite se u sustav Azure i postavite Azure pretplate

U odzivniku komponente PowerShell pokrenite sljedeću naredbu:

```
Login-AzureRmAccount
```

Se od vas zatraži prijavite se na račun za Azure. Nakon prijave, pokrenite sljedeću naredbu da biste vidjeli dostupne pretplate.

```
Get-AzureRMSubscription
```

Ta naredba Vrati popis dostupnih Azure pretplate. Odaberite pretplatu na trenutnoj sesiji ponovnim pokretanjem sljedeće naredbe. Zamjena `<YourSubscriptionId>` s GUID za Azure pretplatu koju želite koristiti.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Postavljanje grupa resursa

Ako nemate postojeću grupu resursa, stvorite novu grupu resursa pomoću naredbe **Nova AzureRmResourceGroup** . Navedite naziv grupe resursa i mjesto na koje želite koristiti. Ako, na primjer:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Ako ne uspije, prikazuje se sažetak novu grupu resursa.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Provjera uvođenja

Provjerite valjanost implementaciju sustava tako da pokrenete u `Test-AzureRmResourceGroupDeployment` cmdlet. Kada testiranje implementaciju, navesti parametra točno onako kako želite da se prilikom izvršavanja uvođenje.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Stvaranje implementaciju

Da biste stvorili novu implementaciju, pokrenite na `New-AzureRmResourceGroupDeployment` naredbu, a zatim navedite potrebne parametre kada se to od vas zatraži. Parametri sadržavati naziv za implementaciju sustava naziv grupe resursa, a put ili URL za datoteku predloška. Ako parametar **načina rada** nije naveden, koristi se zadana vrijednost **rastuća** . Dodatne informacije potražite u članku [rastuća i dovršavanje implementacije](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Sljedeća naredba Traži tri parametara u prozoru PowerShell:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Da biste odredili datoteke parametara umjesto toga, koristite sljedeću naredbu.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Možete koristiti i parametara u istoj razini kada pokrenete cmdlet implementacije. Naredba je na sljedeći način:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Da biste pokrenuli [Dovršeno](../resource-group-template-deploy.md#incremental-and-complete-deployments) implementaciju, postavite parametar **načina rada** **Dovršeno**:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Provjera uvođenja

Ako u člancima uvode se uspješno, u prozoru PowerShell prikazuje se sažetak implementacije:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Daljnji koraci

Sada ste vidjeti osnovni tijeka rada i naredbe za uvođenje predloška programa Azure Voditelj resursa. Detaljnije informacije potražite na sljedećim vezama:

- [Pregled Azure Voditelj resursa][]
- [Implementacija resursi s predlošcima Voditelj resursa za Azure][]
- [Omogućeno](../resource-group-authoring-templates.md)


[Pregled Azure Voditelj resursa]: ../resource-group-overview.md
[Implementacija resursi s predlošcima Voditelj resursa za Azure]: ../resource-group-template-deploy.md
[Azure Galerija predložaka za brzi početak rada]: https://azure.microsoft.com/documentation/templates/?term=service+bus