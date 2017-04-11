<properties 
    pageTitle="Skriptu PowerShell da biste stvorili do uvida aplikacije resursa" 
    description="Moguće automatizirati stvaranje aplikacija uvida resursa." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>Skriptu PowerShell da biste stvorili do uvida aplikacije resursa

*Aplikacija uvida je u pretpregledu.*

Kada želite nadzirati nove aplikacije - ili nove verzije aplikacije - s [Uvida aplikacije za Visual Studio](https://azure.microsoft.com/services/application-insights/), postaviti novi resurs u Microsoft Azure. Ovaj resurs je gdje analizirati i prikazati telemetrijskih podataka iz aplikacije. 

Stvaranje novog resursa možete automatizirati pomoću komponente PowerShell.

Ako, na primjer, ako su razvoj aplikacija za mobilni uređaj, je vjerojatno da, u bilo kojem trenutku će se nekoliko objavljene verzije aplikacije koristi klijentima. Ne želite da biste dobili telemetrijskih rezultate iz različitih verzija miješana prema gore. Tako ćete dobiti proces Sastavi da biste stvorili novi resurs za svaki Sastavi.

## <a name="script-to-create-an-application-insights-resource"></a>Skripta za stvaranje do uvida aplikacije resursa

Potražite u članku specifikacija odgovarajući cmdlet:

* [Novi AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Novi AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*Skriptu komponente PowerShell*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Postupanje s na iKey

Svaki resurs je označena instrumentation tipke (iKey). U iKey je na Izlaz skripte za stvaranje resursa. Sastavljanje skriptu mora sadržavati iKey da biste uvid SDK aplikacije ugrađen u svojoj aplikaciji.

Da biste učinili dostupnim s iKey SDK na dva načina:
  
* U [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* [Pokretanje kodu](app-insights-api-custom-events-metrics.md)i: 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Vidi također

* [Stvaranje uvida računala i web-test resurse iz predložaka](app-insights-powershell.md)
* [Postavljanje nadzor Azure dijagnostike sa servisom PowerShell sustava](app-insights-powershell-azure-diagnostics.md) 
* [Postavljanje upozorenja pomoću komponente PowerShell](app-insights-powershell-alerts.md)

 