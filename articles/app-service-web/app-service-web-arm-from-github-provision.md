<properties 
    pageTitle="Implementacija web-aplikacije koje je povezana s GitHub spremište" 
    description="Implementacija web-aplikacije koje projektom iz spremišta GitHub pomoću predloška Azure Voditelj resursa." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Implementacija web-aplikacijama povezane s GitHub spremište

U ovoj se temi će Saznajte kako stvoriti predložak Azure Voditelj resursa koji implementira web-aplikacije koje je povezana s projektom u spremište GitHub. Ćete saznati kako da biste definirali resursa koji uvode se i navedene upute za definiranje parametara koji su kada se izvršava uvođenje. Pomoću ovog predloška za vlastite implementacije ili prilagoditi ga prema svojim potrebama.

Dodatne informacije o stvaranju predložaka potražite u članku [Za izradu predložaka Azure Voditelj resursa](../resource-group-authoring-templates.md).

Dovršavanje predloška potražite u članku [Web App povezane GitHub predložak](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Što će implementacije

Pomoću ovog predloška će implementacija web-aplikacije koje sadrži kod iz projekta u GitHub.

Automatsko pokretanje implementaciju, kliknite gumb sljedeće:

[![Implementacija Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parametri

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

URL za spremište GitHub koja sadrži projekta za implementaciju. Ovaj parametar sadrži zadane vrijednosti, ali tu vrijednost namijenjen samo da bi se prikazala kako unijeti URL za spremište. Možete koristiti tu vrijednost kada testiranje predloška, ali želite li omogućiti URL vlastite spremište radu s predloškom.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>podružnice

Podružnice spremište za korištenje prilikom implementacija aplikacije. Zadana vrijednost je matrice, ali možete unijeti naziv bilo koje podružnice u spremištu koje želite uvesti.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Resursi za implementaciju

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Web-aplikacije

Stvaranje web-aplikaciji koja je povezana s projektom u GitHub. 

Navedite naziv web-aplikaciji putem parametar **NazivWebMjesta** i mjesto na web-aplikacije pomoću **siteLocation** parametra. Element **dependsOn** predložak određuje web-aplikaciji kao zavisne servis za tarifu. Jer je ovisna o hostinga plan web-aplikaciji nije stvoren dok hostinga plan Završi stvoren. **DependsOn** element koristi se samo da biste odredili redoslijed implementacije. Ako ne označili web-aplikaciji kao ovisi o hostinga tarifa, upravitelj resursa Azure će pokušati da biste stvorili i resursima u isto vrijeme i primiti obavijest o pogrešci ako web-aplikaciji je stvorena prije hostinga plan.

Web-aplikaciji ima podređene resursa koja je definirana u odjeljak **resursa** u nastavku. Ovaj resurs podređeni definira izvora kontrole za projekt u uveden s web-aplikaciji. U ovom predlošku kontrola izvora je povezana s određenom GitHub spremište. Spremište GitHub definiran šifrom **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** možda ćete teško kod spremište URL-a kada želite stvoriti predložak koji pritišćite tipku TAB dok koji zahtijevaju najmanji broj parametara uvodi jedan projekt.
Umjesto kodiranje spremište URL-a, dodajte parametar za spremište URL-a i koristiti tu vrijednost za svojstvo **RepoUrl** .

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Naredbe za pokretanje implementacije

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure EŽA

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
