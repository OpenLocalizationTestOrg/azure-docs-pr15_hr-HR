<properties
    pageTitle="Azure uzorci kratke Monitor PowerShell. | Microsoft Azure"
    description="Pristup Azure Monitor značajkama kao što su automatsko skaliranje, upozorenja, webhooks pretraživanje zapisnika aktivnosti pomoću komponente PowerShell."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure Monitor PowerShell uzorka za brzi početak rada

Ovo članak prikazuje uzorak naredbe ljuske PowerShell za pristup značajkama Azure Monitor. Azure Monitor omogućuje automatsko skaliranje servise u Oblaku, virtualnih računala i web-aplikacije i za slanje upozorenja ili poziva web URL-ovi na temelju vrijednosti konfigurirani telemetrijskih podataka.

>[AZURE.NOTE] Azure monitora novi je naziv za što je pod nazivom "Azure uvida" do rujan 25, 2016. Međutim, prostori a time i ispod naredbi i dalje sadržavati "uvida".

## <a name="set-up-powershell"></a>Postavljanje PowerShell
Ako to već niste učinili, postavite PowerShell da biste pokrenuli na vašem računalu. Dodatne informacije potražite [u](../powershell-install-configure.md) članku instalacija i konfiguriranje PowerShell.

## <a name="examples-in-this-article"></a>U ovom članku

Primjeri u članku pokazuju kako pomoću cmdleta Azure Monitor. Možete pregledati i cijeli popis Azure Monitor PowerShell cmdleti pri [cmdleta Azure monitora (uvida)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).


## <a name="sign-in-and-use-subscriptions"></a>Prijava i korištenje pretplate

Prvo, prijavite se u pretplatu za Azure.

```PowerShell
Login-AzureRmAccount
```

Morate se prijaviti. Kada to učinite, vaš račun, prikazuju se TenantId i zadani Id pretplate. Azure cmdleta raditi u kontekstu pretplate zadani. Da biste pogledali popis pretplata kojima imate pristup, koristite sljedeću naredbu.

```PowerShell
Get-AzureRmSubscription
```

Da biste promijenili kontekstu rad u neku drugu pretplatu, koristite sljedeću naredbu.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Dohvaćanje zapisnika nadzora za pretplatu
Korištenje na `Get-AzureRmLog` cmdlet.  Slijedi nekoliko uobičajenih primjera.

Da biste dobili stavke evidencije od tog vremena i datuma kako bi se prikazao:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Pojavljuje se stavke evidencije između raspon vremena i datuma:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Dohvaćanje zapisnika stavke iz određene grupe resursa:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Stavke evidencije zatražite od davatelja određene resursa između raspon vremena i datuma:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Pojavljuje se sve stavke zapisnika s određenim pozivatelja:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Sljedeća naredba dohvaća zadnji 1000 događaje iz zapisnika nadzora:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`podržava više parametara. Pogledajte na `Get-AzureRmLog` referenca za dodatne informacije.

>[AZURE.NOTE] `Get-AzureRmLog`samo pruža petnaest dana povijesti. Korištenje parametra **– MaxEvents** omogućuje upit posljednje događaje N izvan petnaest dana. Za pristup događaje starije od 15 dana, koristite REST API-JA ili SDK (C# uzorka pomoću SDK-a). Ako ne uključite **StartTime**, zadana vrijednost **EndTime** je minus jedan sat. Ako ne uključite **EndTime**, zadana vrijednost je trenutno vrijeme. Cijelo vrijeme su u UTC-u.

## <a name="retrieve-alerts-history"></a>Dohvaćanje povijest upozorenja
Da biste vidjeli sva upozorenja događaje, upit možete poslati zapisnike Voditelj resursa Azure pomoću sljedećih primjera.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Da biste prikazali povijest na određenom pravilu upozorenja, možete koristiti u `Get-AzureRmAlertHistory` cmdlet, prosljeđivanje u Identifikator resursa upozorenja pravila.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Na `Get-AzureRmAlertHistory` cmdlet podržava razne parametara. Dodatne informacije potražite u članku [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Dohvaćanje informacija na upozorenja pravila
Sve sljedeće naredbe djeluje na grupu resursa pod nazivom "montest".

Prikaz svih svojstava upozorenja pravila:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Dohvati sva upozorenja na grupu resursa:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Dohvaćanje sva upozorenja pravila za ciljnu resursa. Na primjer, sva upozorenja pravila postaviti na VM.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`podržava drugi parametri. Dodatne informacije potražite u članku [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Stvaranje pravila za upozorenja
Možete koristiti u `Add-AlertRule` cmdlet za stvaranje, ažuriranje i Onemogući pravilo upozorenja.

Možete stvoriti e-pošte i webhook svojstva pomoću `New-AzureRmAlertRuleEmail` i `New-AzureRmAlertRuleWebhook`, odnosno. U cmdlet upozorenja pravilo dodijeliti ih kao akcije svojstvo **Akcija** upozorenja pravila.

Sljedeći odjeljak sadrži ogledne koji pokazuje kako stvoriti pravilo upozorenja s različitim parametra.

### <a name="alert-rule-on-a-metric"></a>Upozorenja pravilo na metrike
U sljedećoj su tablici opisani parametara i vrijednosti koje se koriste za stvaranje upozorenja pomoću metričkih podataka.


|Parametar|vrijednost|
|---|---|
|Ime|  simpletestdiskwrite|
|Mjesto ovo pravilo za upozorenja|   Istočni SAD-a|
|ResourceGroup| montest|
|TargetResourceId|  /subscriptions/S1/resourceGroups/montest/Providers/Microsoft.Compute/virtualMachines/testconfig|
|MetricName upozorenja koja je stvorena|   Zapisivanje \PhysicalDisk (_Total) \Disk/sec. Pogledajte na `Get-MetricDefinitions` cmdlet ispod kako dohvatiti nazive točno mjerenja|
|operator|  GreaterThan|
|Prag vrijednost (broj/sec u za ovu metriku)|    1|
|WindowSize (HH format)|  00:05:00|
|Sakupljač (statistike mjerenja u tom slučaju koristi prosječni broj)|  Prosjek|
|prilagođene poruke e-pošte (niz polje)|'foo@example.com','bar@example.com'|
|Slanje e-pošte vlasnika, suradnici i čitači|    -SendToServiceOwners|

Stvorite akciju e-pošte

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Stvaranje Webhook akcije

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Stvaranje upozorenja pravila na metriku % procesora na klasični VM

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Dohvaćanje upozorenja pravila

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Upozorenja cmdlet za dodavanje ažurira pravilo ako već postoji pravilo za upozorenja za dani svojstva. Da biste onemogućili upozorenja pravilo, uključiti parametar **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Upozorenje na događaj zapisnika nadzora

>[AZURE.NOTE] Ta je značajka i dalje u pretpregledu.

U ovom scenariju ćete slanje e-pošte kada web-mjesto uspješno pokrene u svoju pretplatu na u grupi resursa *abhingrgtest123*.

Postavljanje pravilo e-pošte

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Postavljanje pravila webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Stvaranje pravila na događaj

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Dohvaćanje upozorenja pravila

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

Na `Add-AlertRule` cmdlet omogućuje različite parametara. Dodatne informacije potražite u članku [Dodavanje AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Dobit ćete popis dostupnih metriku za upozorenja
Možete koristiti u `Get-AzureRmMetricDefinition` cmdlet da biste vidjeli popis svih metriku za određeni resurs.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Sljedeći primjer generira tablicu s metriku naziv i jedinica za njega.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Potpuni popis dostupnih mogućnosti za `Get-AzureRmMetricDefinition` dostupna je na [Početak MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Stvaranje i upravljanje postavkama automatsko skaliranje
Resursa, kao što je web-aplikacijama, VM, servis u Oblaku ili postavite skaliranje VM može imati samo jedan automatsko skaliranje postavka konfiguriran za njega.
Međutim, svaka postavka automatsko skaliranje može imati više profila. Ako, na primjer, jedan utemeljen na performanse skaliranje profila i drugu za raspored na temelju profila. Svaki profil možete imati više pravila konfiguriran na njemu. Dodatne informacije o automatsko skaliranje potražite [u](../cloud-services/cloud-services-how-to-scale.md)članku automatsko skaliranje aplikacija.

Evo nekoliko koraka koristit ćemo:

1. Stvaranje pravila.
2. Stvaranje profila prethodno mapiranje pravila koja ste stvorili s profilima.
3. Neobavezno: Stvaranje obavijesti za automatsko skaliranje konfiguriranjem svojstva webhook i e-pošte.
4. Stvaranje je postavka automatsko skaliranje s nazivom na ciljnom resursa mapiranjem profila i obavijestima koje ste stvorili u prethodnim koracima.

Sljedeći primjeri pokazuju kako možete stvoriti na automatsko skaliranje postavku za skalu VM za je operacijski sustav Windows koristeći metriku Upotreba procesora.

Prvo, stvorite pravilo kojim se skaliranje odgovaranjem, s povećava broj instanci.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Zatim stvorite pravilo za skaliranje u, pomoću smanjenje broja instanci.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Zatim stvorite profil za pravila.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Stvaranje webhook svojstva.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Stvaranje svojstva obavijesti za automatsko skaliranje postavku, uključujući e-pošte i webhook koju ste prethodno stvorili.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Na kraju, stvorite postavku automatsko skaliranje da biste dodali profila koji ste stvorili iznad.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Dodatne informacije o upravljanju automatsko skaliranje postavke potražite u članku [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Automatsko skaliranje povijest
Sljedeći primjer prikazuje kako možete vidjeti nedavne automatsko skaliranje i upozorenja događaja. Pomoću okvira pretraživanje zapisnika nadzora da biste prikazali povijest automatsko skaliranje.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Možete koristiti u `Get-AzureRmAutoScaleHistory` cmdlet za dohvaćanje automatsko skaliranje povijest.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Dodatne informacije potražite u članku [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Prikaz detalja za postavljanje programa automatsko skaliranje
Možete koristiti u `Get-Autoscalesetting` cmdlet za dohvaćanje dodatne informacije o postavljanju automatsko skaliranje.

Sljedeći primjer prikazuje detalje o sve postavke automatsko skaliranje u grupu resursa 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Sljedeći primjer prikazuje detalje o sve postavke automatsko skaliranje u grupu resursa "myrg1" i isključivo automatsko skaliranje postavku pod nazivom "MyScaleVMSSSetting".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Uklanjanje programa postavka automatsko skaliranje
Možete koristiti u `Remove-Autoscalesetting` cmdlet da biste izbrisali je postavka automatsko skaliranje.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Upravljanje profilima zapisnika za zapisnika nadzora

Možete stvoriti *zapisnik profila* i izvoz podataka iz nadzornih zapisnika s računom za pohranu i zadržavanje podataka možete konfigurirati za njega. Po želji možete strujanja podataka i koncentratora za događaj. Imajte na umu da ta je značajka trenutno u pretpregledu, i možete stvoriti samo jedan profil zapisnika po pretplati. Stvaranje i Upravljanje profilima zapisnika možete koristiti sljedeće Cmdlete s trenutne pretplate. Možete odabrati i određenu pretplatu. Iako PowerShell zadanih postavki za trenutne pretplate, uvijek možete promijeniti te pomoću `Set-AzureRmContext`. Možete konfigurirati zapisnike nadzora usmjeravanje podataka u bilo koji račun za pohranu ili događaj koncentrator tu pretplatu. Podaci se upisuju kao blob datoteke u obliku JSON.

### <a name="get-a-log-profile"></a>Dohvaćanje zapisnika profila
Za dohvaćanje vaše postojeće profile zapisnika, koristite na `Get-AzureRmLogProfile` cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Dodavanje zapisnika profila bez zadržavanja podataka

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Uklanjanje profila zapisnika

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Dodavanje zapisnika profila uz zadržavanje podataka

Svojstvo **- RetentionInDays** možete navesti broj dana, kao pozitivni cijeli broj, gdje će se podaci zadržavaju.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Dodavanje zapisnika profila s zadržavanja i EventHub
Osim usmjeravanje podataka na račun za pohranu, možete i strujanja ga da biste koncentratora za događaj. Imajte na umu da u ovom izdanju pretpregled i prostora za pohranu je obavezna konfiguracija računa, ali konfiguracije koncentrator događaj nije obavezno.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfiguriranje dijagnostičkog zapisnika
Mnoge Azure servise navedite dodatne zapisnike i telemetrijskih, uključujući Azure mreže sigurnosne grupe, softver učitavanja Balancers, sigurnog ključ, Azure servisa za pretraživanje, i logike aplikacije, a oni mogu konfigurirati za spremanje podataka u račun za Azure prostora za pohranu. Taj postupak može izvršiti samo na razini resursa i račun za pohranu mora sadržavati na istom području kao resurs cilj gdje je konfiguriran postavku Dijagnostika.

### <a name="get-diagnostic-setting"></a>Početak dijagnostičkih postavka

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Onemogućivanje dijagnostičkih postavka

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Omogućite diagnostic postavku bez zadržavanja

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Omogućite diagnostic postavku s zadržavanja

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Omogućite diagnostic postavku s zadržavanja za kategoriju konkretnog zapisnika

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
