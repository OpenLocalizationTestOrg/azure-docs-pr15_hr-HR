<properties
    pageTitle="RBAC: Ugrađeni uloge | Microsoft Azure"
    description="U ovoj se temi opisuju ugrađenoj u ulogama za kontrolu pristupa na temelju uloga (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Ugrađeni ulogama

U sklopu Azure na temelju uloga pristup kontrola (RBAC) sljedeće ugrađene uloge koje se mogu dodijeliti korisnike, grupe i usluge. Ne možete mijenjati definicije ugrađene uloge. Međutim, možete stvoriti [Prilagođeni uloga Azure RBAC](role-based-access-control-custom-roles.md) određenim potrebama vaše tvrtke ili ustanove.

## <a name="roles-in-azure"></a>Uloga u Azure

Sljedeća tablica prikazuje kratke opise ugrađene uloge. Kliknite naziv uloge da biste vidjeli detaljne popis **Akcije** i **notactions** uloge. Svojstvo **Akcija** određuje dopuštene akcije Azure resursa. Nizovi akcija možete koristiti zamjenske znakove. Svojstvo **notactions** određuje akcije koje bit će izuzeti iz dopuštene akcije.

>[AZURE.NOTE] Definicija Azure uloga stalno su razvoj. U ovom se članku je ažurni kao što je moguće, ali uvijek možete pronaći najnovije definicije uloge Azure PowerShell. Pomoću na cmdleta `(get-azurermroledefinition "<role name>").actions` ili `(get-azurermroledefinition "<role name>").notactions` kao primjenjivo.

| Naziv uloge | Opis |
| --------- | ----------- |
| [API upravljanje servis suradničkih](#api-management-service-contributor) | Možete upravljati API Management services |
| [Aplikacija uvida komponente suradnika](#application-insights-component-contributor) | Možete upravljati uvida aplikacije komponente |
| [Automatizacija Operator](#automation-operator) | Mogućnost da biste pokrenuli, zaustavili, privremeno obustavljanje i nastavite poslove |
| [BizTalk suradnika](#biztalk-contributor) | Možete upravljati BizTalk services |
| [Suradnik ClearDB MySQL DB](#cleardb-mysql-db-contributor) | Možete upravljati baze podataka ClearDB MySQL |
| [Suradnik](#contributor) | Možete upravljati sve osim programa access. |
| [Suradnik tvorničke podataka](#data-factory-contributor) | Možete stvoriti web-mjesta i upravljanje factories podataka i podređenih resursima u njima. |
| [DevTest Labs korisnika](#devtest-labs-user) | Možete pogledati sve i povezivanje početka, ponovno pokretanje i zatvaranje virtualnog računala |
| [Suradnik DNS Zone](#dns-zone-contributor) | Možete upravljati DNS zone i zapisa |
| [Suradnik DocumentDB računa](#documentdb-account-contributor) | Možete upravljati DocumentDB računa |
| [Inteligentna sustavi račun suradnika](#intelligent-systems-account-contributor) | Možete upravljati računima Inteligentna sustavi |
| [Mreža suradnika](#network-contributor) | Možete upravljati Svi mrežni resursi |
| [Novi Relic APM račun suradnika](#new-relic-apm-account-contributor) | Možete upravljati račune za nove Relic aplikacije tečaj upravljanje performansama i aplikacije |
| [Vlasnik](#owner) | Možete upravljati sve, uključujući programa access |
| [Čitač](#reader) | Možete pogledati sve, ali ne možete mijenjati |
| [Redis predmemorije suradnika](#redis-cache-contributor) | Možete upravljati Redis predmemorije |
| [Raspored posla zbirke suradnika](#scheduler-job-collections-contributor) | Možete upravljati zbirkama posao rasporeda |
| [Suradnik servisa za pretraživanje](#search-service-contributor) | Možete upravljati usluge pretraživanja |
| [Upravitelj sigurnosti](#security-manager) | Možete upravljati sigurnosne komponente, sigurnosne pravilnike i virtualnih računala |
| [SQL DB suradnika](#sql-db-contributor) | Možete upravljati baze podataka SQL, ali ne njihove vezane uz sigurnost pravila |
| [Upravitelj sigurnost SQL](#sql-security-manager) | Možete upravljati pravilnika vezane uz sigurnost sustava SQL Server i baza podataka |
| [SQL Server suradnika](#sql-server-contributor) | Možete upravljati SQL poslužitelja i baze podataka, ali ne njihove vezane uz sigurnost pravila |
| [Klasični prostora za pohranu računa suradnika](#classic-storage-account-contributor) | Možete upravljati računima klasični prostora za pohranu |
| [Prostor za pohranu računa suradnika](#storage-account-contributor) | Možete upravljati računima za pohranu |
| [Korisnički pristup administratora](#user-access-administrator) | Možete upravljati korisničkog pristupa resursima za Azure |
| [Klasični virtualnog računala suradnika](#classic-virtual-machine-contributor) | Možete upravljati klasični virtualnih računala, ali ne virtualne mreže ili prostora za pohranu račun s kojim su povezani |
| [Suradnik virtualnog računala](#virtual-machine-contributor) | Možete upravljati virtualnih računala, ali ne virtualne mreže ili prostora za pohranu račun s kojim su povezani |
| [Klasični mreža suradnika](#classic-network-contributor) | Možete upravljati klasični virtualne mreže i Rezervirana IP-ovi |
| [Suradnik Plan za web](#web-plan-contributor) | Možete upravljati tarife za web |
| [Suradnik web-mjesta](#website-contributor) | Upravljanje web-mjesta, ali ne tarife web na koji su povezani |

## <a name="role-permissions"></a>Dozvole za uloge
Tablice u nastavku opisuju dozvole za određene dodijeljen ulogama. To se može obuhvaćati **Akcije**koje daju dozvole i **NotActions**, što im.

### <a name="api-management-service-contributor"></a>API upravljanje servis suradničkih
Možete upravljati API Management services

| **Akcija** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Stvaranje i upravljanje uslugama za upravljanje API-JA |
| Microsoft.Authorization/*/read | Čitanje autorizacije |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Čitanje uloge i dodjele uloga |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="application-insights-component-contributor"></a>Aplikacija uvida komponente suradnika
Možete upravljati uvida aplikacije komponente

| **Akcija** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i dodjele uloga |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.Insights/components/* | Stvaranje i upravljanje komponente uvida |
| Microsoft.Insights/webtests/* | Stvaranje i upravljanje testira web |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="automation-operator"></a>Automatizacija Operator
Mogućnost da biste pokrenuli, zaustavili, privremeno obustavljanje i nastavite poslove

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i dodjele uloga |
| Microsoft.Automation/automationAccounts/jobs/read | Čitanje automatizaciju zadataka računa |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Nastavljanje posla za račun programa automatizacije |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Zaustavljanje posla za račun programa automatizacije |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Čitanje Automatizacija strujanja posao računa |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Privremeno obustavljanje posla za račun programa automatizacije |
| Microsoft.Automation/automationAccounts/jobs/write | Pisanje automatizaciju zadataka računa |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Čitanje Automatizacija financijske posla |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Čitanje Automatizacija financijske posla |
| Microsoft.Automation/automationAccounts/read | Čitanje Automatizacija računi |
| Microsoft.Automation/automationAccounts/runbooks/read | Čitanje Automatizacija runbooks |
| Microsoft.Automation/automationAccounts/schedules/read | Automatizacija Pročitaj financijske analize |
| Microsoft.Automation/automationAccounts/schedules/write | Pisanje Automatizacija financijske analize |
| Microsoft.Insights/components/* | Stvaranje i upravljanje komponente uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="biztalk-contributor"></a>BizTalk suradnika
Možete upravljati BizTalk services

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i dodjele uloga |
| Microsoft.BizTalkServices/BizTalk/* | Stvaranje i upravljanje uslugama za BizTalk |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="cleardb-mysql-db-contributor"></a>Suradnik ClearDB MySQL DB
Možete upravljati baze podataka ClearDB MySQL

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i dodjele uloga |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |
| successbricks.cleardb/Databases/* | Stvaranje i upravljanje bazama podataka ClearDB MySQL |

### <a name="contributor"></a>Suradnik
Sve osim programa access možete upravljati

| **Akcija** ||
| ------- | ------ |
| * | Stvaranje i upravljanje resursima sve vrste |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Nije moguće izbrisati ulogama i dodjele uloga |  
| Microsoft.Authorization/*/Write | Nije moguće stvoriti uloge i dodjele uloga |

### <a name="data-factory-contributor"></a>Suradnik tvorničke podataka
Stvaranje i upravljanje factories podataka i podređenih resursa u njima.

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.DataFactory/dataFactories/* | Stvaranje i upravljanje factories podataka i podređenih resursa u njima. |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="devtest-labs-user"></a>DevTest Labs korisnika
Možete pogledati sve i povezivanje početka, ponovno pokretanje i zatvaranje virtualnog računala

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Compute/availabilitySets/read | Čitanje svojstva skupa dostupnosti |
| Microsoft.Compute/virtualMachines/*/read | Čitanje svojstva virtualnog računala (VM veličine, izvođenja status, proširenja VM itd.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Deallocate virtualnim strojevima |
| Microsoft.Compute/virtualMachines/read | Čitanje svojstva virtualnog računala |
| Microsoft.Compute/virtualMachines/restart/action | Ponovno pokrenite virtualnim strojevima |
| Microsoft.Compute/virtualMachines/start/action | Pokretanje virtualnim strojevima |
| Microsoft.DevTestLab/*/read | Čitanje svojstava na Laboratorija |
| Microsoft.DevTestLab/labs/createEnvironment/action | Stvaranje okruženja Laboratorija |
| Microsoft.DevTestLab/labs/formulas/delete | Brisanje formule |
| Microsoft.DevTestLab/labs/formulas/read | Formula za čitanje |
| Microsoft.DevTestLab/labs/formulas/write | Dodavanje ili izmjena formule |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Analiza Laboratorija pravila |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Pridruživanje adrese skup pozadinskog raspoređivača učitavanja |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Uključivanje raspoređivača opterećenja ulaznog NAT pravila |
| Microsoft.Network/networkInterfaces/*/read | Čitanje svojstava sučelje mreže (Ako, na primjer, sve na učitavanja balancers koji mrežnog sučelja je dio) |
| Microsoft.Network/networkInterfaces/join/action | Uključivanje virtualnog računala na mreži sučelje |
| Microsoft.Network/networkInterfaces/read | Čitanje sučelje mreže |
| Microsoft.Network/networkInterfaces/write | Pisanje sučelje mreže |
| Microsoft.Network/publicIPAddresses/*/read | Čitanje svojstava javnu IP adresa |
| Microsoft.Network/publicIPAddresses/join/action | Uključiti javnu IP adresa |
| Microsoft.Network/publicIPAddresses/read | Čitanje mreže javnu IP adresa |
| Microsoft.Network/virtualNetworks/subnets/join/action | Uključivanje virtualne mreže |
| Microsoft.Resources/deployments/operations/read | Implementacija operacija čitanja |
| Microsoft.Resources/deployments/read | Čitanje implementacije |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Storage/storageAccounts/listKeys/action | Popis za pohranu računa tipke |

### <a name="dns-zone-contributor"></a>Suradnik DNS Zone
Možete upravljati DNS zone i zapisa.

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/\*/čitanja | Čitanje uloge i dodjele uloga |
| Microsoft.Insights/alertRules/\* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.Network/dnsZones/\* | Stvaranje i upravljanje DNS zone i zapisa |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/\* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/\* | Stvaranje i upravljanje karticama za podršku |


### <a name="documentdb-account-contributor"></a>Suradnik DocumentDB računa
Možete upravljati DocumentDB računa

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.DocumentDb/databaseAccounts/* | Stvaranje i upravljanje računima DocumentDB |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="intelligent-systems-account-contributor"></a>Inteligentna sustavi račun suradnika
Možete upravljati računima Inteligentna sustavi

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.IntelligentSystems/accounts/* | Stvaranje i upravljanje računima Inteligentna sustavi |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="network-contributor"></a>Mreža suradnika
Možete upravljati Svi mrežni resursi

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.Network/* | Stvaranje i upravljanje mreža |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="new-relic-apm-account-contributor"></a>Novi Relic APM račun suradnika
Možete upravljati račune za nove Relic aplikacije tečaj upravljanje performansama i aplikacije

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |
| NewRelic.APM/accounts/* | Stvaranje i upravljanje novi Relic aplikacije performanse upravljanje računima |

### <a name="owner"></a>Vlasnik
Možete upravljati sve, uključujući programa access

| **Akcija** ||
| ------- | ------ |
| * | Stvaranje i upravljanje resursima sve vrste |

### <a name="reader"></a>Čitač
Možete pogledati sve, ali ne možete mijenjati

| **Akcija** ||
| ------- | ------ |
| * / čitanja | Resursi za sve vrste osim tajne za čitanje. |

### <a name="redis-cache-contributor"></a>Redis predmemorije suradnika
Možete upravljati Redis predmemorije

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Cache/redis/* | Stvaranje i upravljanje Redis predmemorije |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="scheduler-job-collections-contributor"></a>Raspored posla zbirke suradnika
Možete upravljati zbirkama posao rasporeda

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Scheduler/jobcollections/* | Stvaranje i Upravljanje zbirkama posla |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku  |

### <a name="search-service-contributor"></a>Suradnik servisa za pretraživanje
Možete upravljati usluge pretraživanja

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Search/searchServices/* | Stvaranje i upravljanje uslugama za pretraživanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku  |

### <a name="security-manager"></a>Upravitelj sigurnosti
Možete upravljati sigurnosne komponente, sigurnosne pravilnike i virtualnih računala

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.ClassicCompute/*/read | Pročitajte informacije o konfiguraciji klasični izračunati virtualnim strojevima |
| Microsoft.ClassicCompute/virtualMachines/*/write | Pisanje konfiguracija virtualnim strojevima |
| Microsoft.ClassicNetwork/*/read | Pročitajte informacije o konfiguraciji o klasični mreži  |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Security/* | Stvaranje i upravljanje pravilima i sigurnosne komponente |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku  |

### <a name="sql-db-contributor"></a>SQL DB suradnika
Možete upravljati baze podataka SQL, ali ne njihove vezane uz sigurnost pravila

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje uloge i uloge dodjele |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima upozorenja |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Sql/servers/databases/* | Stvaranje i upravljanje bazama podataka za SQL |
| Microsoft.Sql/servers/read | Čitanje SQL Server |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Ne možete uređivati pravila nadzora |
| Microsoft.Sql/servers/databases/auditingSettings/* | Ne možete uređivati postavke nadzora |
| Microsoft.Sql/servers/databases/auditRecords/read  | Ne može čitati zapise nadzora |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Ne možete uređivati pravila veze |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Ne možete uređivati podatke masking pravila |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Ne možete uređivati sigurnosnih upozorenja pravila |
| Microsoft.Sql/servers/databases/securityMetrics/* | Ne možete uređivati metriku sigurnosti |

### <a name="sql-security-manager"></a>Upravitelj sigurnost SQL
Možete upravljati pravilnika vezane uz sigurnost sustava SQL Server i baza podataka

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje Microsoft autorizacije |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Sql/servers/auditingPolicies/* | Stvaranje i upravljanje pravila nadzora u programu SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Stvaranje i upravljanje nadzora postavku za SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Stvaranje i upravljanje pravila nadzora baze podataka SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Stvaranje i upravljanje postavke nadzora baze podataka SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read | Čitanje nadzora zapisa |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Stvaranje i upravljanje pravilima veze za SQL server baze podataka |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Stvaranje i upravljanje podacima baze podataka SQL server masking pravila |
| Microsoft.Sql/servers/databases/read | Baze podataka SQL za čitanje |
| Microsoft.Sql/servers/databases/schemas/read | Shema baze podataka za čitanje SQL server |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Stupci tablice za čitanje SQL poslužitelja baze podataka |
| Microsoft.Sql/servers/databases/schemas/tables/read | Tablice baze podataka za čitanje SQL server |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Stvaranje i upravljanje SQL poslužitelja baze podataka sigurnosnih upozorenja pravila |
| Microsoft.Sql/servers/databases/securityMetrics/* | Stvaranje i upravljanje SQL poslužitelja baze podataka sigurnost mjerenja |
| Microsoft.Sql/servers/read | Čitanje SQL Server |
| Microsoft.Sql/servers/securityAlertPolicies/* | Stvaranje i upravljanje pravilima upozorenja zaštite SQL server |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="sql-server-contributor"></a>SQL Server suradnika
Možete upravljati SQL poslužitelja i baze podataka, ali ne njihove vezane uz sigurnost pravila

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje autorizacije|
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Sql/servers/* | Stvaranje i upravljanje SQL Server |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Ne možete uređivati pravila nadzora u programu SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Ne možete uređivati postavke nadzora u programu SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Ne možete uređivati pravila nadzora baze podataka SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Ne možete uređivati postavke nadzora baze podataka SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read  | Ne može čitati zapise nadzora |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Ne možete uređivati pravila veze za SQL poslužitelja baze podataka |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Ne možete uređivati podatke baze podataka SQL server masking pravila |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Ne možete uređivati SQL poslužitelja baze podataka sigurnosnih upozorenja pravila |
| Microsoft.Sql/servers/databases/securityMetrics/* | Ne možete uređivati SQL poslužitelja baze podataka sigurnost mjerenja |
| Microsoft.Sql/servers/securityAlertPolicies/* | Ne možete uređivati SQL server sigurnosnih upozorenja pravila |

### <a name="classic-storage-account-contributor"></a>Klasični prostora za pohranu računa suradnika
Možete upravljati računima klasični prostora za pohranu

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje autorizacije |
| Microsoft.ClassicStorage/storageAccounts/* | Stvaranje i upravljanje računima za pohranu |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="storage-account-contributor"></a>Prostor za pohranu računa suradnika
Možete upravljati računima za pohranu, ali ne pristupaju na njih.

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje svih autorizacije |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.Insights/diagnosticSettings/* | Upravljanje dijagnostičkih postavki |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Storage/storageAccounts/* | Stvaranje i upravljanje računima za pohranu |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="user-access-administrator"></a>Korisnički pristup administratora
Možete upravljati korisničkog pristupa resursima za Azure

| **Akcija** ||
| ------- | ------ |
| * / čitanja | Resursi za sve vrste osim tajne za čitanje. |
| Microsoft.Authorization/* | Upravljanje autorizacije |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="classic-virtual-machine-contributor"></a>Klasični virtualnog računala suradnika
Možete upravljati klasični virtualnih računala, ali ne virtualne mreže ili prostora za pohranu račun s kojim su povezani

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje autorizacije |
| Microsoft.ClassicCompute/domainNames/* | Stvaranje i upravljanje nazivima domene klasični računalnim |
| Microsoft.ClassicCompute/virtualMachines/* | Stvaranje i upravljanje virtualnim strojevima |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Spajanje mreža sigurnosne grupe |
| Microsoft.ClassicNetwork/reservedIps/link/action | Veza Rezervirana IP-ovi |
| Microsoft.ClassicNetwork/reservedIps/read | Čitanje Rezervirana IP adresa |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Uključivanje virtualne mreže |
| Microsoft.ClassicNetwork/virtualNetworks/read | Čitanje virtualne mreže |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Čitanje diskova račun za pohranu |
| Microsoft.ClassicStorage/storageAccounts/images/read | Slika računa za pohranu za čitanje |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Popis za pohranu računa tipke |
| Microsoft.ClassicStorage/storageAccounts/read | Računi klasični prostora za pohranu za čitanje |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="virtual-machine-contributor"></a>Suradnik virtualnog računala
Možete upravljati virtualnih računala, ali ne virtualne mreže ili prostora za pohranu račun s kojim su povezani

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje autorizacije |
| Microsoft.Compute/availabilitySets/* | Stvaranje i organiziranje skupova računalnim dostupnosti |
| Microsoft.Compute/locations/* | Stvaranje i upravljanje računalnim mjesta |
| Microsoft.Compute/virtualMachines/* | Stvaranje i upravljanje virtualnim strojevima |
| Microsoft.Compute/virtualMachineScaleSets/* | Stvaranje i organiziranje skupova skaliranje virtualnog računala |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Spajanje mreža pristupnika pozadinskog adresu okupljanje |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Uključivanje učitavanja opterećenja pozadinskog adresu grupe |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Uključivanje opterećenja dolazni NAT grupe |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Uključivanje opterećenja ulazna pravila NAT |
| Microsoft.Network/loadBalancers/read | Čitanje učitavanja balancers |
| Microsoft.Network/locations/* | Stvaranje i upravljanje mrežna mjesta |
| Microsoft.Network/networkInterfaces/* | Stvaranje i upravljanje sučelje mreže |
| Microsoft.Network/networkSecurityGroups/join/action | Spajanje mreža sigurnosne grupe |
| Microsoft.Network/networkSecurityGroups/read | Čitanje mreže sigurnosne grupe |
| Microsoft.Network/publicIPAddresses/join/action | Spajanje mreža javnu IP adrese |
| Microsoft.Network/publicIPAddresses/read | Čitanje mreže javnu IP adresa |
| Microsoft.Network/virtualNetworks/read | Čitanje virtualne mreže |
| Microsoft.Network/virtualNetworks/subnets/join/action | Uključivanje podmreže virtualne mreže |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Storage/storageAccounts/listKeys/action | Popis za pohranu računa tipke |
| Microsoft.Storage/storageAccounts/read | Računi za pohranu za čitanje |
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="classic-network-contributor"></a>Klasični mreža suradnika
Možete upravljati klasični virtualne mreže i Rezervirana IP-ovi

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje autorizacije |
| Microsoft.ClassicNetwork/* | Stvaranje i upravljanje klasični mreža |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |

### <a name="web-plan-contributor"></a>Suradnik Plan za web
Možete upravljati tarife za web

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje autorizacije |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |
| Microsoft.Web/serverFarms/* | Stvaranje i upravljanje farme poslužitelja |

### <a name="website-contributor"></a>Suradnik web-mjesta
Upravljanje web-mjesta, ali ne tarife web na koji su povezani

| **Akcija** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Čitanje autorizacije |
| Microsoft.Insights/alertRules/* | Stvaranje i upravljanje pravilima za upozorenja uvida |
| Microsoft.Insights/components/* | Stvaranje i upravljanje komponente uvida |
| Microsoft.ResourceHealth/availabilityStatuses/read | Čitanje stanja resursa |
| Microsoft.Resources/deployments/* | Stvaranje i upravljanje implementacijama grupa resursa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupa resursa za čitanje |  
| Microsoft.Support/* | Stvaranje i upravljanje karticama za podršku |
| Microsoft.Web/certificates/* | Stvaranje i upravljanje certifikata na web-mjesta |
| Microsoft.Web/listSitesAssignedToHostName/read | Čitanje dodijeljene naziv glavnog računala web-mjesta |
| Microsoft.Web/serverFarms/join/action | Uključivanje farme poslužitelja |
| Microsoft.Web/serverFarms/read | Čitanje farme poslužitelja |
| Microsoft.Web/sites/* | Stvaranje i upravljanje web-mjesta |

## <a name="see-also"></a>Vidi također
- [Na temelju uloga kontrola pristupa](role-based-access-control-configure.md): početak rada s RBAC na portalu za Azure.
- [Prilagođeno uloga Azure RBAC](role-based-access-control-custom-roles.md): Saznajte kako stvoriti prilagođene uloge da odgovara vašim potrebama programa access.
- [Izvješće o povijesti promjena za stvaranje programa access](role-based-access-control-access-change-history-report.md): vođenja evidencije promjena dodjele uloga u RBAC.
- [Otklanjanje poteškoća na temelju uloga kontrola pristupa](role-based-access-control-troubleshooting.md): prijedlozi za rješavanje uobičajenih problema.
