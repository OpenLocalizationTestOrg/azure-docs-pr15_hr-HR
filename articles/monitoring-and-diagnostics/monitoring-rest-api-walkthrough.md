<properties
    pageTitle="Azure REST API vodič za nadzor | Microsoft Azure"
    description="Upute za provjeru autentičnosti zahtjevi za i korištenje Azure nadzor REST API-JA."
    authors="mcollier, rboucher"
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
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure nadzor vodič REST API-JA
U ovom se članku objašnjava da biste izvršili provjeru autentičnosti pa kod možete koristiti [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Azure Monitor API omogućuje programski dohvatiti dostupne zadane metričkim definicije (vrsta mjerenja kao što su vrijeme procesora, zahtjeve, itd.), preciznosti i metričkim vrijednostima. Nakon što se dohvate, podatke moguće je spremiti u spremištu zasebne podatke kao što su baze podataka SQL Azure, DocumentDB ili Lake Azure podataka. Iz nje dodatne analizu može izvoditi prema potrebi.

Osim rad s različitim metričkim točkama, kao što je ovaj članak prikazuje Monitor API omogućuje popis pravila upozorenja, prikaz zapisnika aktivnosti i još mnogo toga. Potpuni popis dostupnih operacije potražite u članku [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Provjera autentičnosti zahtjeva za Azure monitora

Prvi korak je zahtjev za provjeru autentičnosti.

Sve zadatke izvršiti protiv Azure Monitor API pomoću provjere autentičnosti model upravljanja resursima Azure. Dakle, sve zahtjeve za morate moguće provjeriti autentičnost kod Azure Active Directory (Azure AD). Jedan pristup za provjeru autentičnosti klijentska aplikacija je za stvaranje Azure AD usluge i dohvaćanje token za provjeru autentičnosti (JWT). Sljedeći primjer skripte prikazuje stvaranje servis za Azure AD glavni PowerShell. Detaljnije walk-through, potražite u dokumentaciji o [korištenju Azure PowerShell da biste stvorili glavni pristupa resursima servisa](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell). Također je moguće [stvoriti načelo servisa putem portala za Azure](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Upit Azure Monitor API klijentska aplikacija trebali biste koristiti glavnicu prethodno stvorena servis za provjeru autentičnosti. Sljedeći primjer skriptu PowerShell prikazuje jednu pristup pomoću [Provjere autentičnosti biblioteke imenika Active Directory](../active-directory/active-directory-authentication-libraries.md) (ADAL) za jednostavniji početak provjere autentičnosti JWT tokena. JWT token se prenosi u sklopu programa HTTP autorizacije parametra u zahtjevima za Azure Monitor REST API-JA.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Nakon dovršetka postavljanja korak provjere autentičnosti upita pa izvršavaju na temelju Azure Monitor REST API-JA. Postoje dva korisne upita:

1. Popis metričkim definicije resursa

2. Dohvaćanje metričkim vrijednostima


## <a name="retrieve-metric-definitions"></a>Dohvaćanje definicije mjerenja
>[AZURE.NOTE] Da biste dohvatili metričkim definicije pomoću Azure Monitor REST API-JA, koristite "2016-03-01" kao verzija API-JA.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Logika aplikacije Azure metričkim definicije će izgledati slično sljedeće snimku zaslona:

![ALT "JSON prikaz metričkim definicija odgovora".](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Dodatne informacije potražite u dokumentaciji [popis metričkim definicije resursa u Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/mt743621.aspx) .

## <a name="retrieve-metric-values"></a>Dohvaćanje vrijednosti mjerenja
Kada se gube se dostupne metričke definicije, zatim moguće je dohvatiti povezane metričkim vrijednostima. Pomoću to mjerenje naziv "vrijednost" (ne u ' localizedValue') za sve zahtjeve za filtriranje (Ako, na primjer, točke metričkim podataka 'CpuTime' i 'zahtjeve za dohvaćanje). Ako su navedeni nema filtara, vraća se metriku zadani.

>[AZURE.NOTE] Da biste dohvatili metričkim vrijednosti pomoću Azure Monitor REST API-JA, koristite "2016-06-01" kao verzija API-JA.

**Metoda**: početak

**Zahtjev za URI**: https://management.azure.com/subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/_{resursa-davatelja-prostor naziva}_/_{Vrsta resursa}_//providers/microsoft.insights/metrics?$filter=_{naziv resursa}__{Filtar}_& api-verzija =_{apiVersion}_

Na primjer, da biste dohvatili točke podataka metričkim RunsSucceeded za dani vremenski raspon i vrijeme Žitne od jednog sata, zahtjev bio na sljedeći način:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Rezultat će izgledati slično kao primjer pratiti snimka zaslona:

![ALT "Prikazuje prosječnu odgovor mjerenja vremena JSON odgovora"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Da biste dohvatili više točaka podataka ili zbrajanja, dodajte imena metričkim definicija i zbrajanje vrste filtar, kao što se vidi u sljedećem primjeru:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Korištenje ARMClient
Alternative pomoću komponente PowerShell (kao što je prikazano gore) jest korištenje [ARMClient](https://github.com/projectkudu/ARMClient) na računalu Windows. ARMClient rukuje Azure AD provjere autentičnosti (i dobivene JWT token) automatski. Sljedeći koraci strukture korištenje ARMClient za dohvat metrike podataka:

1. Instalirajte [Chocolatey](https://chocolatey.org/) i [ARMClient](https://github.com/projectkudu/ARMClient).

2. U prozoru terminal unesite _armclient.exe prijava_. To od vas traži da biste se prijavili Azure.

3. Vrsta _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Vrsta _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT "Korištenje ARMClient za rad s Azure praćenja REST API-JA"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Dohvaćanje ID resursa
Korištenje REST API-JA zaista može pomoći da biste shvatili dostupne metričke definicije, preciznosti i povezane vrijednosti. Prilikom korištenja [Biblioteke za upravljanje Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx), korisno je te podatke.

Prethodni kod ID resursa koji će se koristiti je cijeli put do željenog Azure resursa. Na primjer, upit s web-aplikaciju programa Azure, ID resursa bio sljedeći:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-name}/Providers/Microsoft.web/Sites/{site-name}/*

Sljedeći popis sadrži nekoliko primjera oblici ID resursa za različite Azure resurse:

* **Koncentrator IoT** - /subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/Microsoft.Devices/IotHubs/_{iot-koncentrator-name}_

* **Skup elastic SQL** - /subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/Microsoft.Sql/servers/_{skup db}_/elasticpools/_{sql-naziv grupe aplikacija –}_

* **SQL baze podataka (v12)** – /subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/Microsoft.Sql/servers/_{naziv poslužitelja}_/databases/_{Naziv baze podataka}_

* **Servis Bus** – /subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/Microsoft.ServiceBus/_{prostora za naziv}_/_{servicebus name}_

* **Skupovi skala VM** - /subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{vm name}_

* **VMs** - /subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/Microsoft.Compute/virtualMachines/_{vm name}_

* **Događaj koncentratora** - /subscriptions/_{id pretplate}_/resourceGroups/_{- naziv grupe resursa –}_/providers/Microsoft.EventHub/namespaces/_{eventhub-prostor naziva}_


Nema zamjenski postupke za dohvaćanje Identifikator resursa, uključujući korištenje Explorer resursa Azure, prikaz željeni resursa na portalu za Azure i putem komponente PowerShell ili EŽA Azure.

### <a name="azure-resource-explorer"></a>Explorer Azure resursa
Da biste pronašli Identifikator resursa za željenu resursa, jedan pristup korisno je pomoću alata za [Azure resursa Explorer](https://resources.azure.com) . Dođite do željenog resursa, a zatim pogledajte ID prikazane kao snimku zaslona sljedeće:

![ALT "Azure resursa programa Explorer"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Portal za Azure
ID resursa je moguće dohvatiti i s portala za Azure. Da biste to učinili, pronađite željenu resursa, a zatim odaberite Svojstva. ID resursa prikazuje se u plohu svojstva u sljedeće snimku zaslona:

![ALT "resursa ID koji se prikazuje u plohu svojstva na portalu za Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
Identifikator resursa možete dohvatiti pomoću cmdleta ljuske PowerShell Azure kao i. Ako, na primjer, da biste dobili ID resursa za web-aplikaciju programa Azure, izvršavanje cmdlet Get-AzureRmWebApp kao snimku zaslona sljedeće:

![ALT "ID resursa nabavili PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure EŽA
Da biste dohvatili Identifikator resursa pomoću EŽA Azure, izvršiti naredbu 'Prikaži azure webapp' navodeći na ' – json' mogućnosti, kao što je prikazano u sljedećim snimku zaslona:

![ALT "ID resursa nabavili PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Dohvaćanje podataka za zapisnik aktivnosti
Osim rad s metričkim definicije i povezane vrijednosti i moguće je radi dohvaćanja dodatnih zanimljive uvida vezane uz Azure resursi. Na primjer, moguće je podaci iz [zapisnika aktivnosti](https://msdn.microsoft.com/library/azure/dn931934.aspx) upita. Sljedeći primjer pokazuje se korištenje Azure Monitor REST API-JA za upit podaci iz zapisnika aktivnosti unutar određeni raspon datuma za Azure pretplatu:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Daljnji koraci
* Pregledajte [Pregled nadzor](monitoring-overview.md).
* Prikaz [podržani metriku s Azure Monitor](monitoring-supported-metrics.md).
* Pregledajte [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Pregledajte [Biblioteka Azure upravljanja](https://msdn.microsoft.com/library/azure/mt417623.aspx).
