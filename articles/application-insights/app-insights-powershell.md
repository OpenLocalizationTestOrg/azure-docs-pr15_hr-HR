<properties 
    pageTitle="Stvaranje aplikacije uvida resursima pomoću komponente PowerShell" 
    description="Programski stvorite aplikacije uvida resurse kao dio sustava Sastavi." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/02/2016" 
    ms.author="awills"/>
 
# <a name="create-application-insights-resources-using-powershell"></a>Stvaranje aplikacije uvida resursima pomoću komponente PowerShell

U ovom se članku objašnjava kako biste automatski stvorili do [Uvida aplikacije](app-insights-overview.md) resursa Azure. Možda, na primjer, to u sklopu postupka Sastavi. Uz osnovne aplikacije uvida resursa, možete stvarate [testira web dostupnost](app-insights-monitor-web-app-availability.md), [postavite upozorenja](app-insights-alerts.md), te ostale resurse za Azure.

Pri stvaranju ove resurse ključno je JSON predloške za [Azure Voditelj resursa](../powershell-azure-resource-manager.md). Da ne duljimo, postupak je: preuzimanje definicije JSON postojećih resursa parameterize određene vrijednosti kao što su ime. a zatim pokrenite predložak kad god želite stvoriti novi resurs. Nekoliko resursa možete pakiranje zajedno, njihovo stvaranje sve u jednom otvorite -, na primjer, programa monitor aplikacije s testira dostupnost, upozorenja i prostora za pohranu za neprekinuti izvoz. Postoje neka subtleties nekim parameterizations ćemo ćete u članku se objašnjava ovdje.

## <a name="one-time-setup"></a>Postavljanje jednokratne

Ako još niste koristili PowerShell s pretplatom Azure prije:

Instalirajte modul Azure Powershell na računalu na kojem želite pokrenuti skripte:

1. Instalacija [Microsoft Web platformu Installer (v5 ili noviji)](http://www.microsoft.com/web/downloads/platform.aspx).
2. Koristite da biste instalirali Microsoft Azure Powershell.

## <a name="copy-the-json-for-existing-resources"></a>Kopiranje JSON za postojećih resursa

1. Postavljanje [Aplikacije uvida](app-insights-overview.md) projekta slični onima koje želite generirati automatski. Ako želite, dodajte testira web i upozorenja.
2. Stvaranje nove datoteke .json - ćemo ga pozovite `template1.json` u ovom primjeru. Kopirajte sadržaj u je:


    ```JSON

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "appName": { "type": "string" },
            "webTestName": { "type": "string" },
            "url": { "type": "string" },
            "text": { "type" : "string" }
          },
          "variables": {
            "testName": "[concat(parameters('webTestName'), 
               '-', toLower(parameters('appName')))]"
            "alertRuleName": "[concat(parameters('webTestName'), 
               '-', toLower(parameters('appName')), 
               '-', subscription().subscriptionId)]"
          },
          "resources": [
            {
              // component JSON file contents
            },
            {
              //web test JSON file contents
            },
            {
              //alert rule JSON file contents
            }
 
            // Any other resources go here
          ]
        }
    
    ```

    Ovaj predložak postavit će jedan dostupnost test osim glavni resursa.


2. Otvorite [Upravitelj Azure resursa](https://resources.azure.com/). Kretanje prema dolje kroz `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, vaše aplikacije resurs. 

    ![Navigacija u programu Explorer Azure resursa](./media/app-insights-powershell/01.png)

    *Komponente* su osnovni aplikacije uvida resursi za prikaz aplikacije. Postoje zasebne resursa za povezane upozorenja pravila i dostupnosti web testira.

3. Kopirajte JSON komponentu u odgovarajuće mjesto u `template1.json`.
6. Brisanje tih svojstava:
  * `id`
  * `InstrumentationKey`
  * `CreationDate`
4. Otvorite webtests i alertrules sekcije, a zatim kopirajte JSON za pojedinačne stavke u predlošku. (Ne kopirali čvorove webtests ili alertrules: idite u stavke u odjeljku ih.)

    Svaki test web sadrži povezane upozorenja pravilo, da biste imali da biste kopirali oba.

    Testiranje web moraju prosljeđivati prije upozorenja pravilo.

5. Zadovoljili shemu, umetnite redak u svakog resursa:

    `"apiVersion": "2014-04-01",`

    (U shemi požali i o velikih i malih slova upišite imena resursa `Microsoft.Insights/*` – ali *nemojte* mijenjati te.)


## <a name="parameterize-the-template"></a>Parameterize predložak

Sada imate zamijenite specifični Nazivi parametara. Da biste [parameterize predloška](../resource-group-authoring-templates.md), upišite izraze pomoću na [skup Pomoćnik za funkcije](../resource-group-template-functions.md). 

Nije moguće parameterize samo dio niza, stoga koristite `concat()` da biste sastavili nizova.

Evo primjera zamjene želite učiniti. Postoji nekoliko pojavljivanja svaki zamjena. Možda ćete drugima u predlošku. Ovim se primjerima koristi parametre i varijable definirali pri vrhu predložak.

Pronalaženje | Zamijenite
---|---
`"hidden-link:/subscriptions/.../components/MyAppName"`| `"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"`
`"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` | `"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",`
`"/subscriptions/.../webtests/myTestName-myAppName",` | `"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",`
`"myWebTest-myAppName"` | `"[variables(testName)]"'`
`"myTestName-myAppName-subsId"` | `"[variables('alertRuleName')]"`
`"myAppName"` | `"[parameters('appName')]"`
`"myappname"`(mala slova) | `"[toLower(parameters('appName'))]"`
`"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"`|`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]" `



## <a name="set-dependencies-between-the-resources"></a>Postavljanje međuzavisnosti resursi

Azure trebali postaviti resursi izričite redoslijedom. Da biste provjerili je li jednu postavljanje dovršava prije početka sljedećeg, dodajte retke ovisnosti:

* Na web-mjestu testiranje resursa:

    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`

* U upozorenja resursa:

    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`

## <a name="create-application-insights-resources"></a>Stvaranje aplikacije uvida resursi

1. U ljusci PowerShell, prijavite se u Azure

    `Login-AzureRmAccount`

2. Pokrenite naredbu ovako:

    ```PS

        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -templateFile .\template1.json `
               -appName myNewApp `
               -webTestName aWebTest `
               -Url http://myapp.com `
               -text "Welcome!"
               -siteName "MyAzureSite"

    ``` 

    * -ResourceGroupName je grupa u kojoj želite stvoriti nove resurse.
    * -templateFile mora biti prije prilagođenih parametara.
    * -appName naziv resursa koje želite stvoriti.
    * -webTestName naziv testa web da biste stvorili.
    * -Url url web-aplikacije.
    * -tekst niz koji se prikazuje u web-stranicu.
    * -NazivWebMjesta - koristiti ako je Azure web-mjesta


## <a name="define-metric-alerts"></a>Definiranje metričkim upozorenja

Postoji [način PowerShell Postavljanje upozorenja](app-insights-alerts.md#set-alerts-by-using-powershell).


## <a name="an-example"></a>Primjer

Evo cjelovit komponente, testiranje web i web-test upozorenja predložak koji sam stvorio:

``` JSON

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "webTestName": { "type": "string" },
    "appName": { "type": "string" },
    "URL": { "type": "string" },
    "text": { "type" : "string" }
  },
  "variables": {
    "alertRuleName": "[concat(parameters('webTestName'), '-', toLower(parameters('appName')), '-', subscription().subscriptionId)]",
    "testName": "[concat(parameters('webTestName'), '-', toLower(parameters('appName')))]"
  },
  "resources": [
    {
      //"id": "[resourceId('Microsoft.Insights/components', parameters('appName'))]",
      "apiVersion": "2014-04-01",
      "kind": "web",
      "location": "Central US",
      "name": "[parameters('appName')]",
      "properties": {
        "TenantId": "9122605a-471fc50f8438",
        "Application_Type": "web",
        "Flow_Type": "Brownfield",
        "Request_Source": "VSIX3.3.1.0",
        "Name": "[parameters('appName')]",
        //"CreationDate": "2015-10-14T15:55:10.0917441+00:00",
        "PackageId": null,
        "ApplicationId": "[parameters('appName')]"
      },
      "tags": { },
      "type": "microsoft.insights/components"
    },
    {
      //"id": "[resourceId('Microsoft.Insights/webtests', variables('testName'))]",
      "name": "[variables('testName')]",
      "apiVersion": "2014-04-01",
      "type": "microsoft.insights/webtests",
      "location": "Central US",
      "tags": {
        "[concat('hidden-link:', resourceId('microsoft.insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "provisioningState": "Succeeded",
        "Name": "[parameters('webTestName')]",
        "Description": "",
        "Enabled": true,
        "Frequency": 900,
        "Timeout": 120,
        "Kind": "ping",
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "emea-gb-db3-azr"
          }
        ],
        "Configuration": {
          "WebTest": "[concat(
             '<WebTest   Name=\"', 
                parameters('webTestName'), 
              '\"  Id=\"32cfc791-aaad-4b50-9c8d-993c21beb218\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"         Guid=\"a6f2c90b-61bf-b28hh06gg969\"  Version=\"1.1\"  Url=\"', 
              parameters('Url'), 
              '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"', 
              parameters('text'), 
              '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('testName')]"
      }
    },
    {
      //"id": "[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",
      "name": "[variables('alertRuleName')]",
      "apiVersion": "2014-04-01",
      "type": "microsoft.insights/alertrules",
      "location": "East US",
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]",
        "[resourceId('Microsoft.Insights/webtests', variables('testName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('testName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('alertRuleName')]",
        "description": "",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('testName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M",
          "failedLocationCount": 2
        },
        "action": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
          "sendToServiceOwners": true,
          "customEmails": [ ]
        },
        "provisioningState": "Succeeded",
        "actions": [ ]
      }

    }
  ]
}

```

## <a name="see-also"></a>Vidi također

Ostali članci Automatizacija:

* [Stvaranje do uvida aplikacije resursa](app-insights-powershell-script-create-resource.md) – brzi način bez korištenja predloška.
* [Postavljanje upozorenja](app-insights-powershell-alerts.md)
* [Stvaranje testira web](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Slanje Azure Dijagnostika do uvida aplikacije](app-insights-powershell-azure-diagnostics.md)
* [Stvaranje izdanje primjedbe](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)
