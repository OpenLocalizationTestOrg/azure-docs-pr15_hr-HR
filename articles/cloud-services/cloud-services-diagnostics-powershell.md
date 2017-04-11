<properties
    pageTitle="Omogućivanje Dijagnostika na servise u Oblaku Azure pomoću komponente PowerShell | Microsoft Azure"
    description="Saznajte kako omogućiti Dijagnostika za servise u oblaku pomoću komponente PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Omogućivanje Dijagnostika na servise u Oblaku Azure pomoću komponente PowerShell

Možete prikupiti dijagnostičkih podataka kao što je aplikacija zapisnike, performanse brojač itd iz korištenje nastavka Azure Dijagnostika servis u Oblaku. U ovom se članku opisuje kako omogućiti nastavak Azure Dijagnostika za servise u Oblaku pomoću komponente PowerShell.  Preduvjeti koja su potrebna za u ovom se članku potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Omogućivanje Dijagnostika proširenje kao dio implementacija servis u Oblaku

Taj se način od dobar neprekinuti integraciju vrste scenariji gdje se mogu omogućiti nastavak Dijagnostika kao dio implementaciji servisa u oblaku. Prilikom stvaranja novog implementacije u Oblaku možete omogućiti nastavak Dijagnostika Prenos u parametru *ExtensionConfiguration* cmdleta [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) . Parametar *ExtensionConfiguration* vodi polja konfiguracija Dijagnostika koje je moguće stvoriti pomoću cmdleta [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) .

Sljedeći primjer prikazuje kako možete omogućiti Dijagnostika za servise u oblaku s WebRole i WorkerRole svaki pojavljuju različite Dijagnostika konfiguracije.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Ako konfiguracijska datoteka Dijagnostika određuje StorageAccount element pod nazivom računa za pohranu, cmdleta New AzureServiceDiagnosticsExtensionConfig će automatski koristi taj račun za pohranu. To funkcionira, računa za pohranu mora biti iste pretplate na kao uvodi servisa u Oblaku.

Iz Azure SDK 2,6 nadalje objavljivanje datoteke konfiguracije proširenja generira na MSBuild Odredišni izlaz neće sadržavati naziv računa spremišta temelju niza konfiguracije Dijagnostika naveden u konfiguracijskoj datoteci usluge (.cscfg). Skripta u nastavku prikazuje analizirati datoteke konfiguracije proširenja iz Odredišni izlaz Objavi i konfiguriranje Dijagnostika proširenja za svaku ulogu pri implementaciji servisa u oblaku.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online koristi slične pristup automatiziranih deploymnts servise u Oblaku s nastavkom Dijagnostika. Dovršavanje primjer potražite u članku [Objavljivanje AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) .

Ako nema StorageAccount nije naveden u konfiguraciji Dijagnostika, morate proći u parametru StorageAccountName cmdletu. Ako parametar StorageAccountName nije naveden, cmdlet će uvijek koristi račun za pohranu koji je naveden u parametar, a ne onaj koji je naveden u konfiguracijskoj datoteci Dijagnostika.

Ako je račun za dijagnostiku prostora za pohranu u neku drugu pretplatu na iz servisa u Oblaku, morate izričito prenesite parametri StorageAccountName i StorageAccountKey cmdletu. Parametar StorageAccountKey nije potrebna kada je račun za dijagnostiku prostora za pohranu u okviru iste pretplate kao cmdlet možete automatski upita, a zatim postavite vrijednost ključa prilikom omogućivanja proširenja za dijagnostiku. Međutim, ako je račun za dijagnostiku prostora za pohranu u neku drugu pretplatu, zatim cmdlet možda nećete moći automatski dohvatiti tipku i morate izričito navesti ključ kroz parametar StorageAccountKey.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Omogućivanje Dijagnostika nastavak na postojeće Oblaku

Cmdlet [Skup AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) možete koristiti da biste omogućili ili ažurirati Dijagnostika konfiguraciju na neki servis u Oblaku, koji je već pokrenut.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Početak Trenutna konfiguracija proširenje za dijagnostiku
Pomoću cmdleta [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) da biste dobili trenutnu konfiguraciju Dijagnostika za servise u oblaku.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Uklonite proširenje za dijagnostiku
Da biste isključili Dijagnostika na neki servis u oblaku koristite cmdlet [Ukloni AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) .

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Ako je omogućeno proširenje Dijagnostika pomoću *Skupa AzureServiceDiagnosticsExtension* ili *Novo AzureServiceDiagnosticsExtensionConfig* bez parametra *uloga* možete ukloniti proširenje pomoću *Ukloni AzureServiceDiagnosticsExtension* bez parametra *uloge* . Ako parametar *uloga* korišten prilikom omogućivanja proširenje pa ga morate koristit i prilikom uklanjanja datotečni nastavak.

Da biste uklonili ulogama za pojedinačne proširenje dijagnostiku:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o korištenju Azure Dijagnostika i drugih tehnika za otklanjanje poteškoća potražite u članku [Omogućavanje dijagnostike Azure servise u Oblaku i virtualnih računala](cloud-services-dotnet-diagnostics.md).
- [Dijagnostika konfiguracije sheme](https://msdn.microsoft.com/library/azure/dn782207.aspx) objašnjava različite xml konfiguracija mogućnosti za nastavak Dijagnostika.
- Da biste saznali kako omogućiti nastavak Dijagnostika za virtualnim strojevima, potražite u članku [Stvaranje Windows virtualnog računala nadzora i Dijagnostika pomoću predloška Azure Voditelj resursa](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
