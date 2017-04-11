<properties
    pageTitle="Postavljanje aplikacije uvida u programa Azure pomoću komponente PowerShell | Microsoft Azure"
    description="Automatiziranje konfiguriranje Azure Dijagnostika za kanal aplikacije uvid u."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Da biste postavili uvida aplikacije za Azure web-aplikaciju programa pomoću komponente PowerShell

[Microsoft Azure](https://azure.com) može biti [konfiguriran za slanje Azure Dijagnostika](app-insights-azure-diagnostics.md) do [Uvida aplikacije za Visual Studio](app-insights-overview.md). Dijagnostika odnose se na servise u Oblaku Azure i Azure VMs. Oni nadopunjuju telemetrijskih koju ste poslali s aplikacije pomoću aplikacije uvida SDK-a. Automatizacija procesa stvaranja nove resurse u Azure, dio možete konfigurirati Dijagnostika pomoću komponente PowerShell.

## <a name="azure-template"></a>Predloška Azure

Ako web-aplikaciju na Azure i stvaranje resursa pomoću predloška Azure Voditelj resursa, možete konfigurirati aplikacije uvida dodavanjem to čvor resurse:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-Naziv aplikacije uvida resursa
* `myWebAppName`– ID-a web-aplikacije


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Omogućivanje Dijagnostika proširenje kao dio implementacija servis u Oblaku

Na `New-AzureDeployment` cmdlet ima parametar `ExtensionConfiguration`, koji traje polja Dijagnostika konfiguracija. To se može stvoriti pomoću na `New-AzureServiceDiagnosticsExtensionConfig` cmdlet. Ako, na primjer:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Omogućivanje Dijagnostika nastavak na postojeće Oblaku

Na postojeći servis pomoću `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Početak Trenutna konfiguracija proširenje za dijagnostiku

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Uklonite proširenje za dijagnostiku

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Ako je omogućeno proširenje Dijagnostika pomoću `Set-AzureServiceDiagnosticsExtension` ili `New-AzureServiceDiagnosticsExtensionConfig` bez parametra uloga možete ukloniti pomoću proširenje `Remove-AzureServiceDiagnosticsExtension` bez parametra uloge. Ako parametar uloga korišten prilikom omogućivanja proširenje pa ga morate koristit i prilikom uklanjanja datotečni nastavak.

Da biste uklonili ulogama za pojedinačne proširenje dijagnostiku:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Vidi također

* [Praćenje Azure oblaka servisa aplikacija s računala uvida](app-insights-cloudservices.md)
* [Slanje Azure Dijagnostika do uvida aplikacije](app-insights-azure-diagnostics.md)
* [Automatiziranje Konfiguriranje upozorenja](app-insights-powershell-alerts.md)

