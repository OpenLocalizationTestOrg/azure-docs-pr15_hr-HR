<properties
    pageTitle="Stvaranje i konfiguriranje radnog prostora zapisnika analize pomoću komponente PowerShell | Microsoft Azure"
    description="Zapisivanje podataka koristi analize s poslužitelja lokalnih ili infrastrukture u oblaku. Strojno podataka možete prikupiti iz spremišta Azure kada generira Azure Dijagnostika."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Upravljanje prijava analitiku pomoću komponente PowerShell

Možete koristiti [zapisnika analize PowerShell cmdleti] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) za izvršavanje raznih funkcija u prijava analitiku iz naredbenog retka ili kao dio skriptu.  Primjeri zadatke koje možete izvršiti pomoću komponente PowerShell obuhvaćaju sljedeće:

+ Stvaranje radnog prostora
+ Dodavanje ili uklanjanje rješenja
+ Uvoz i izvoz spremljena pretraživanja
+ Stvaranje grupe za računala
+ Omogućivanje zbirke IIS zapisnika s računala s instaliran agent za Windows
+ Prikupljanje mjerača performansi s računala Linux i Windows
+ Prikupite događaje iz syslog na računalima Linux 
+ Prikupite događaje iz zapisnika događaja sustava Windows
+ Prikupljanje zapisnika prilagođene događaja
+ Dodavanje analize agent zapisnika Azure virtualnog računala
+ Konfiguriranje zapisnika analize indeksirati podatke koji se prikupljaju korištenjem dijagnostike Azure


Ovaj članak sadrži dva uzorka kod koji pokazuju neke funkcije koje možete izvršiti iz PowerShell.  Može se odnositi na [zapisnika analize PowerShell cmdlet referenca] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) za druge funkcije.

> [AZURE.NOTE] Prijava analitiku je prije se zvao radu uvida koji je zbog toga je naziv koji se koristi u s cmdletima.

## <a name="prerequisites"></a>Preduvjeti

Da biste koristili PowerShell s radnim prostorom prijava analitiku, morate imati:

+ Pretplatu na Azure i 
+ Radni prostor Azure prijava analitiku povezana s Azure pretplatu.

Ako ste stvorili radnog prostora programa OMS, ali ga nije još povezana je s Azure pretplate možete stvoriti vezu:

+ Na portalu za Azure
+ Na portalu OMS ili 
+ Pomoću cmdleta Get-AzureRmOperationalInsightsLinkTargets i novo AzureRmOperationalInsightsWorkspace.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Stvaranje i konfiguriranje zapisnika analize radnog prostora

Sljedeće ogledne skripte ilustrira način:

1.  Stvaranje radnog prostora
2.  Popis dostupnih rješenja
3.  Dodajte rješenja u radni prostor
4.  Uvoz spremljena pretraživanja
5.  Izvoz spremljena pretraživanja
6.  Stvaranje grupe za računala
7.  Omogućivanje zbirke IIS zapisnika s računala s instaliran agent za Windows
8.  Prikupljanje mjerača mjerača performansi logičkog diska s računala Linux (% Inodes koristi Besplatni megabajta; % Koristi prostor; Na disku prijenosi/sec; Na disku čitanja/sec; Zapisivanje na disku/sec)
9.  Prikupite syslog događaje iz Linux računala
10. Prikupljanje pogrešaka i upozorenja događaje iz zapisnik događaja aplikacije s računala sa sustavom Windows
11. Prikupljanje memorije dostupne Mbytes performanse brojač s računala sa sustavom Windows
12. Prikupljanje prilagođene zapisnika 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Konfiguriranje zapisnika analize indeksirati Azure dijagnostiku 

Agentless nadzor Azure resursa, resursa moraju imati Azure Dijagnostika omogućen i konfiguriran za pisanje s računom za pohranu. Prijava analitiku moguće je konfigurirati da biste prikupili zapisnike s računa za pohranu. Resursa koje biste trebali učiniti prethodni konfiguraciju obuhvaća:

+ Klasični oblaka services (uloga web- a tempiranja)
+ Servis tkanina klastere
+ Mrežni sigurnosne grupe
+ Ključ sefovi i 
+ Aplikacija pristupnika

PowerShell možete koristiti i konfiguriranje radnog prostora prijava analitiku u jedan Azure pretplate za prikupljanje zapisnika s različitim Azure pretplata.

U sljedećem primjeru kako:

1.  Postojeće račune za pohranu i mjesta koja prijava analitiku će indeksirati podatke s popisa
2.  Stvaranje konfiguracije za čitanje s računa za pohranu
3.  Ažuriranje novostvorenu konfiguraciju indeksirati podatke iz dodatna mjesta
4.  Brisanje novostvorenu konfiguraciju

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Daljnji koraci

- [Pregled zapisnika analize PowerShell cmdleti](http://msdn.microsoft.com/library/mt188224.aspx) za dodatne informacije o korištenju ljuske PowerShell za konfiguraciju zapisnika analize.

