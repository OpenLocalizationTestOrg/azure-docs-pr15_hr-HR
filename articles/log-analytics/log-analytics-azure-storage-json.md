<properties
    pageTitle="Analiza Azure dijagnostičke zapisnike pomoću zapisnika analize | Microsoft Azure"
    description="Prijava analitiku može čitati zapisnike s Azure servisa za pisanje Azure dijagnostičke zapisnike bloba prostor za pohranu u JSON OSNOVNI oblik."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Analiza Azure dijagnostičke zapisnike pomoću zapisnika Analytics

Prijava analitiku može prikupiti zapisnike za sljedeće Azure servise za pisanje [Azure dijagnostičke zapisnike](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) blobova u JSON OSNOVNI oblik:

+ Automatizacija (pretpregled)
+ Ključni sigurnog (pretpregled)
+ Pristupnik za računala (pretpregled)
+ Mrežni sigurnosne grupe (pretpregled)

U sljedećim se odjeljcima će vas voditi kroz pomoću ljuske PowerShell za:

+ Konfiguriranje prijava analitiku u prikupiti zapisnike iz spremišta za svaki resurs  
+ Omogućivanje rješenje prijava analitiku servisa za Azure

Prije nego što prijava analitiku možete prikupiti podatke za ove resurse, Azure Dijagnostika mora biti omogućen. Korištenje na `Set-AzureRmDiagnosticSetting` cmdlet uključite zapisivanje podataka.

Pogledajte sljedeće članke dodatne informacije o omogućivanju dijagnostičkih zapisnika:

+ [Ključni zbirke ključeva](../key-vault/key-vault-logging.md)
+ [Pristupnik za aplikaciju](../application-gateway/application-gateway-diagnostics.md)
+ [Mrežni sigurnosne grupe](../virtual-network/virtual-network-nsg-manage-log.md)

Ovo je dokumentacija sadrži detalje na:

+ Prikupljanje podataka za otklanjanje poteškoća
+ Zaustavljanje zbirke podataka

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfiguriranje zapisnika analize prikupljanje Azure dijagnostičkih zapisnika

Prikupljanje zapisnika za te servise i omogućavanja rješenje vizualizacija zapisnike je izvršiti pomoću skripte komponente PowerShell.

U primjeru u nastavku omogućuje bilježenje svih podržanih resursa

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Za neke resurse je moguće izvršiti prethodni konfiguracije na portalu Azure na način opisan u [Pregled Azure dijagnostičke zapisnike](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs).

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfiguriranje zapisnika analize prikupljanje Azure dijagnostičkih zapisnika

Ne možemo ste naveli modul skriptu PowerShell koji izvozi dva cmdleta radi jednostavnijeg konfiguriranje analize zapisnika:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`od vas traži unos i može se postaviti jednostavne konfiguracija
2. `Add-AzureDiagnosticsToLogAnalytics`uzima resursa za praćenje kao ulaz i konfigurira zapisnika Analytics  

### <a name="pre-requisites"></a>Prije requisites

1. Azure PowerShell s verzijom 1.0.8 ili novija verzija od cmdleta radu uvide.
  - [Kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md)
  - Provjerite je li vaša verzija Cmdlete:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Zapisivanje dijagnostičkih podataka je konfiguriran za Azure resursa koje želite nadzirati. Korištenje `Set-AzureRmDiagnosticSetting` ili se odnositi na [Korištenje zapisnika Analytics za prikupljanje podataka s računa za Azure prostora za pohranu](log-analytics-azure-storage.md) za omogućivanje Dijagnostika.
3. Radni prostor [Zapisnika Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS)  
4. Modul AzureDiagnosticsAndLogAnalytics PowerShell
  - Preuzimanje modula [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) iz galerije PowerShell

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Mogućnost 1: Pokreću skripte interaktivne konfiguracija

Otvorite ljusku PowerShell i pokretanje:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Prikazuje popis dostupnih stavki i dobili upit za odaberite odgovarajuću mogućnost.
Što trebate napraviti odabire za svaku od sljedećeg:

+ Vrste resursa (Azure Services) prikupljanje zapisnika iz
+ Prikupljanje zapisnika iz instanci resursa
+ Prijava analitiku radnog prostora da biste prikupili podatke

Nakon izvođenja ovu skriptu, trebali biste vidjeti zapise u prijava analitiku otprilike 30 minuta nakon nove dijagnostičkih podataka napisan za pohranu. Ako se zapisi nisu dostupne kada se ovaj put odnose se na odjeljak otklanjanje poteškoća ispod.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Mogućnost 2: Napraviti popis resursa i prenesite cmdlet konfiguracija

Možete sastaviti popis resursa Azure Dijagnostika omogućeno i prenesite resurse cmdlet konfiguracije.

Dodatne informacije o cmdletu možete vidjeti tako da pokrenete `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Da biste pronašli dodatne detalje na OMS [Prijava analitiku PowerShell cmdleti](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Ako su u različite pretplate Azure resursa i radnog prostora, prebacivati iz jedne po potrebi pomoću`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Nakon izvođenja ovu skriptu, trebali biste vidjeti zapisa u prijava analitiku otprilike 30 minuta nakon nove dijagnostičkih podataka napisan za pohranu. Ako se zapisi nisu dostupne kada se ovaj put odnose se na odjeljak otklanjanje poteškoća ispod.  

>[AZURE.NOTE] Konfiguracija nije vidljiv na portalu za Azure. Možete provjeriti pomoću konfiguracije na `Get-AzureRmOperationalInsightsStorageInsight` cmdlet.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Zaustavljanje prijava analitiku iz prikupljanje Azure dijagnostičkih zapisnika

Da biste izbrisali konfiguracije prijava analitiku resursa, koristite na `Remove-AzureRmOperationalInsightsStorageInsight` cmdlet.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Konfiguracija za Azure dijagnostičke zapisnike za otklanjanje poteškoća

*Problem*

Prilikom konfiguriranja resurs koji je zapisivanje klasični za pohranu, prikazat će se sljedeća pogreška:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Razlučivost*

Prijavite se u sustav API za uvođenje klasičnog model s`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Prikupljanje podataka za Azure dijagnostičke zapisnike za otklanjanje poteškoća

Ako se ne prikazuju podatke za vaše Azure resursa u zapisnik analize, možete koristiti sljedeće korake za otklanjanje poteškoća:

+ Provjera podataka slijedi račun za pohranu
+ Provjerite je li omogućen prijava analitiku rješenja za servis
+ Provjerite je li prijava analitiku konfiguriran za čitanje iz spremišta
+ Ažuriranje za pohranu ključ za račun

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Provjerite je li podataka slijedi je račun za pohranu

Možete provjeriti ako su podaci slijedi račun za pohranu pomoću alata koji omogućuje pregledavanje Azure prostora za pohranu (na primjer Visual Studio) ili pomoću komponente PowerShell.

Da biste pronašli račun za pohranu resurs je konfiguriran da biste se prijavili pomoću sljedeće komponente PowerShell:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Spremnik za pohranu koriste Azure Dijagnostika ima naziv s u obliku:  

`insights-logs-<Category>`

Odnose se na [korištenje prostora za pohranu Cmdlete da biste provjerili spremnik za podacima](../storage/storage-powershell-guide-full.md) da biste saznali više o prikazivanju sadržaj na račun za pohranu.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Provjerite je li omogućen prijava analitiku rješenja za servis

Ako koristite `Add-AzureDiagnosticsToLogAnalyticsUI`, ispravan prijava analitiku rješenje je automatski omogućena za vas.

Da biste provjerili je li omogućena rješenje, pokrenite sljedeće komponente PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Ako rješenje nije omogućena, možete je pomoću sljedeće komponente PowerShell omogućiti:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Da biste pronašli naziv rješenja da biste omogućili za svaku vrstu resursa, koristite sljedeće komponente PowerShell (Ova varijabla nije dostupna kada ste uvezli modul):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Provjerite je li prijava analitiku konfiguriran za čitanje iz spremišta

Ako dodate dodatne resurse za Azure, morate omogućiti dijagnostičkog zapisnika ih i konfigurirati zapisnika Analytics za njih.
Da biste provjerili koje resurse i račune za pohranu prijava analitiku je konfiguriran za prikupljanje zapisnika za korištenje ljuske PowerShell za sljedeće:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Ažuriranje ključa za pohranu

Ako promijenite ključ za račun za pohranu, konfiguracije prijava analitiku i mora se ažurirati na novi ključ.
Konfiguriranje prijava analitiku mogu ažurirati novog ključa pomoću sljedeće komponente PowerShell:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Da biste saznali naziv prostora za pohranu uvid u `Get-AzureRmOperationalInsightsStorageInsight` cmdlet, kao što je prikazano u starijim primjerima.

## <a name="next-steps"></a>Daljnji koraci

- [Korištenje spremište blobova platforme za IIS i spremište tablica za događaje](log-analytics-azure-storage-iis-table.md) da biste pročitali zapisnike za Azure services taj unos Dijagnostika spremište tablica ili IIS zapisnici mogli unijeti bloba prostora za pohranu.
- [Omogućivanje rješenja](log-analytics-add-solutions.md) za davanje uvid u podatke.
- [Korištenje upita za pretraživanje](log-analytics-log-searches.md) da biste analizirali podatke.
