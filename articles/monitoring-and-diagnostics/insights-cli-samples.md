<properties
    pageTitle="Uzorci Azure EŽA monitora brzi početak rada. | Microsoft Azure"
    description="Primjeri naredbi EŽA za značajke Azure Monitor. Azure Monitor je servis Microsoft Azure koji omogućuje slanje upozorenja, nazovite URL-ovi web na temelju vrijednosti konfigurirani telemetrijskih podataka, i automatsko skaliranje servise u Oblaku, virtualnih računala i web-aplikacije."
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure uzoraka Monitor različite platforme EŽA brzi početak rada

U ovom se članku prikazuje uzorak naredbe sučelje naredbenog retka (EŽA) da biste lakše pristupiti značajkama Azure Monitor. Azure Monitor omogućuje automatsko skaliranje servise u Oblaku, virtualnih računala i web-aplikacije i za slanje upozorenja ili poziva web URL-ovi na temelju vrijednosti konfigurirani telemetrijskih podataka.

>[AZURE.NOTE] Azure monitora novi je naziv za što je pod nazivom "Azure uvida" do rujan 25, 2016. Međutim, prostori a time i ispod naredbi i dalje sadržavati "uvida".


## <a name="prerequisites"></a>Preduvjeti

Ako već niste instalirali EŽA Azure, potražite u članku [Instalacija EŽA Azure](../xplat-cli-install.md). Ako ne znate Azure EŽA možete potražite dodatne informacije o tome pomoću [EŽA Azure za Mac i Linux, Windows s Azure Voditelj resursa](../xplat-cli-azure-resource-manager.md).


U sustavu Windows, instalirajte npm iz sustava [Node.js web-mjesta](https://nodejs.org/). Kada dovršite instalaciju, pomoću CMD.exe s ovlastima Pokreni kao Administrator, iz mape na kojem je instaliran npm izvršiti sljedeće:

```console
npm install azure-cli --global
```

Nakon toga idite na bilo koje mape/mjesto koje želite i upišite naredbenom:

```console
azure help
```

## <a name="log-in-to-azure"></a>Prijavite se na Azure

Prvi je korak za prijavu na račun za Azure.

```console
azure login
```

Nakon izvođenja tu naredbu, morate prijaviti putem upute na zaslonu. Nakon toga pogledajte vašeg računa, TenantId i zadani ID pretplate. Sve naredbe funkcioniraju u kontekstu pretplate na zadano.

Da biste dobili popis detalja trenutne pretplate, koristite sljedeću naredbu.

```console
azure account show
```

Da biste promijenili kontekst za rad u neku drugu pretplatu, koristite sljedeću naredbu.

```console
azure account set "subscription ID or subscription name"
```

Da biste koristili Azure nadzor i upravljanje resursima Azure naredbe, morate biti u načinu rada za Azure Voditelj resursa.

```console
azure config mode arm
```

Da biste pogledali popis svih podržanih naredbi Azure Monitor, učinite sljedeće.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Prikaz zapisnika nadzora za pretplatu

Da biste pogledali popis zapisnika nadzora, učinite sljedeće.

```console
azure insights logs list [options]
```

Pokušajte sljedeće da biste vidjeli sve dostupne mogućnosti.

```console
azure insights logs list -help
```

Evo primjera popisa zapisnicima tako da na resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Primjer popisa zapisnicima po pozivatelja

```console
azure insights logs list --caller "myname@company.com"
```

Primjer popisa prijavi pomoću pozivatelja na vrsti resursa, unutar početni i završni datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Rad s upozorenja
Možete koristiti informacije u odjeljku rad s upozorenja.

### <a name="get-alert-rules-in-a-resource-group"></a>Primanje upozorenja pravila u grupu resursa

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Stvaranje pravila metričkim upozorenja

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Stvaranje pravila za upozorenja zapisnika

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Stvaranje upozorenja pravila webtest

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Brisanje upozorenja pravila

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Profili zapisnika
Pomoću informacija u ovom odjeljku možete raditi s profilima zapisnika.

### <a name="get-a-log-profile"></a>Dohvaćanje zapisnika profila

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Dodavanje zapisnika profila bez zadržavanja

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Uklanjanje profila zapisnika

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Dodavanje profila zapisnika s zadržavanja

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Dodavanje zapisnika profila s zadržavanja i EventHub

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Dijagnostika
Pomoću informacija u ovom odjeljku da biste radili s dijagnostičkih postavki.

### <a name="get-a-diagnostic-setting"></a>Početak dijagnostičkih postavka

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Onemogućivanje dijagnostičkih postavka

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Omogućite diagnostic postavku bez zadržavanja

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatsko skaliranje
Rad s postavkama automatsko skaliranje pomoću informacija u ovom odjeljku. Ćete morati izmijeniti u ovim se primjerima.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Automatsko skaliranje postavke za grupu resursa

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Dohvatiti postavke automatsko skaliranje prema nazivu u grupu resursa

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Postavljanje postavki auotoscale

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
