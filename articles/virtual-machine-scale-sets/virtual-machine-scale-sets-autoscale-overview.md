<properties
    pageTitle="Automatska promjena veličine i virtualnog računala skaliranje skupove | Microsoft Azure"
    description="Dodatne informacije o korištenju za dijagnostiku i resursima automatsko skaliranje automatski promijeniti omjer virtualnim strojevima u skupu mjerilo."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automatska promjena veličine i virtualnog računala skaliranje skupova

Automatsko skaliranje virtualnim strojevima u skupu skaliranje je stvaranje ili brisanje strojeva u skupu prema potrebi u skladu potrebama performansi. Kao što je rastom glasnoće poslovne aplikacije možda ćete morati dodatne resurse da biste omogućili da biste učinkovito izvršavanje zadataka.

Automatska promjena veličine je automatizirani proces pomaže olakšalo njihovo indirektni upravljanje. Smanjivanjem indirektni ne morate neprestano praćenje performansi sustava ili odlučite kako upravljati resursi. Promjena veličine je elastic postupak. Dodatni resursi moguće dodavati kao povećava opterećenja, ali kao zahtjev smanjenja resursi moguće ukloniti za minimiziranje troškove i održavanje razine performansi.

Postavite automatsko skaliranje na skali postavljeno korištenjem predloška programa Azure Voditelj resursa, Azure PowerShell, Azure EŽA ili Azure portal.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Postavite skaliranje pomoću predložaka Voditelj resursa

Umjesto implementacija i zasebno, upravljanje svaki resurs aplikacije koristili predložak koji implementira svih resursa u jednom, usklađenih postupak. U predlošku definiraju resursima aplikacija i implementaciju parametara određeni su klauzulom za različite okruženja. Predložak se sastoji od JSON i izraza koje možete koristiti za sastavljanje vrijednosti za implementaciju sustava. Da biste saznali više, pogledajte [Voditelj resursa za Azure za izradu predložaka](../resource-group-authoring-templates.md).

U predlošku, navedite element kapaciteta:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Kapacitet označava broj virtualnim strojevima u skupu. Možete ručno promijeniti kapacitet uvođenjem predloška s drukčijom vrijednošću. Ako su implementacije predloška da biste samo promijenili kapacitet, možete uključiti samo element SKU ažurirane kapaciteta.

Automatska promjena kapacitet vaš skaliranje zakazivati pomoću kombinacije resursa autoscaleSettings i nastavak Dijagnostika.

### <a name="configure-the-azure-diagnostics-extension"></a>Konfiguriranje proširenje Dijagnostika Azure

Automatska promjena veličine možete učiniti samo ako je uspješno na svakom virtualnog računala u skupu skaliranje metriku zbirke. Proširenje Dijagnostika Azure pruža mogućnosti za nadzor i dijagnostici koji odgovara potrebama zbirke metriku automatsko skaliranje resursa. Proširenje možete instalirati kao dio predloška Voditelj resursa.

U ovom se primjeru prikazuje varijable koje se koriste u predlošku da biste konfigurirali proširenje dijagnostiku:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parametri se daju kada je implementiran u predložak. U ovom primjeru navedeni su naziv računa spremišta gdje se pohranjuju podaci i skup skala s kojeg prikupljanja podataka. I u ovom primjeru Windows Server samo broja niti brojač performanse prikupljaju se. Sve mjerača performansi dostupne u sustavu Windows ili Linux može se koristiti za prikupljanje informacija za dijagnostiku i moguće ih je uvrstiti u konfiguraciji kućni broj.

U ovom se primjeru prikazuje definicije proširenje u predložak:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Kada se pokrene proširenje Dijagnostika, u tablici koja se nalazi na računu za pohranu koji navedete prikupljanja podataka. U tablici WADPerformanceCounters možete pronaći prikupljene podatke:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Konfiguriranje autoScaleSettings resursa

Resurs autoscaleSettings koristi podatke iz proširenje Dijagnostika odlučite želite li da biste povećali ili smanjili broj virtualnim strojevima u skupu mjerilo.

U ovom se primjeru prikazuje konfiguraciju resursa u predlošku:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

U gornjem primjeru dva pravila se stvaraju za definiranje automatskih postupaka skaliranja. Prvo pravilo definira akciju skaliranje Izlaz i drugo pravilo definira akcije u mjerilo. Pravila isporučuju se ove vrijednosti:

- **metricName** - tu vrijednost je ista kao brojač performanse koji ste definirali u varijablu wadperfcounter za proširenje Dijagnostika. U gornjem primjeru brojač broja niti se koristi.  
- **metricResourceUri** - tu vrijednost je identifikator resursa skupa skaliranje virtualnog računala. Ovaj identifikator sadrži naziv grupe resursa, naziv davatelja resursa i naziv skale postavljen na skaliranje.
- **timeGrain** – u ovom je vrijednost preciznosti metrike koji se prikupljaju. U prethodnom primjeru prikupljanja podataka na intervala od jedne minute. Ta se vrijednost koristi s timeWindow.
- **statistike** – tu vrijednost određuje kako se spajaju metriku kako bi odgovarao Automatska akcija skaliranja. Moguće vrijednosti su: prosjek, minimum, maksimum.
- **timeWindow** – tu vrijednost je raspon vrijeme u kojem prikupljaju se podaci instance. Mora biti između pet minuta i 12 sati.
- **timeAggregation** – tu vrijednost određuje kako treba podatke koji se prikupljaju se kombinirati s vremenom. Zadana vrijednost je prosjek. Moguće vrijednosti su: prosjek, Minimum, maksimum, prezime, ukupni zbroj, broj.
- **operator** – tu vrijednost je operator koji se koristi za usporedbu metričke podatke i praga. Moguće vrijednosti su: jednako NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **prag** – u ovom je vrijednost koja se pokreće akcija mjerilo. Obavezno pružaju dovoljno razlikuju praga za akciju skaliranje iz i praga akcije u mjerilo. Ako ste postavili vrijednosti koje će biti jednaki, sustav anticipates konstante promjena koje onemogućuje implementacijom skaliranja akcija. Na primjer, postavljanje i za 600 niti u prethodnom primjeru ne funkcionira.
- **smjer** – tu vrijednost određuje akciju koja se koristi se kada se postiže vrijednosti praga. Moguće vrijednosti su povećajte ili smanjite.
- **Vrsta** – tu vrijednost je vrsta akcije koje bi se trebale provoditi i mora biti postavljeno na ChangeCount.
- **vrijednost** – tu vrijednost je broj virtualnim strojevima koji su dodati ili ukloniti iz skupa mjerilo. Ta vrijednost mora biti 1 ili noviji.
- **cooldown** – tu vrijednost je vrijeme čekanja nakon zadnje akcije skaliranja prije no što se pojavljuje na sljedeću akciju. Ta vrijednost mora biti između jednu minutu i jedan tjedan.

Ovisno o performansama brojač koristite neki elementi u konfiguraciji predložak koji se koristi drugačije. U prethodnom primjeru, brojač performanse broja niti, vrijednosti praga je 650 za akciju skaliranje iz i je vrijednost praga 550 akcije u mjerilo. Ako koristite brojač kao što je % procesor vrijeme, vrijednosti praga postavljen je na željeni postotak središnjeg procesora koristi koji određuje skaliranja akcije.

Opterećenjem stvaranja na virtualnim strojevima koji pokreće skaliranja akcija kapacitet skup povećava se ovisno o vrijednost u predlošku. Na primjer, u rasponu skup ondje gdje je postavljeno kapacitet 3 i vrijednost akcije skaliranje postavljena na 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Kada se stvara opterećenja koji uzrokuje count average niti Go iznad prag 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Skaliranje iz akcija aktivira koji uzrokuje kapacitet postavljanje da biste se povećati za jedan:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

A virtualnog računala dodaje se skup mjerilo:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Nakon cooldown razdoblja od pet minuta, ako Prosječan broj niti na strojeva ostaje iznad 600 nekom drugom računalu dodaje se postavljanje. Ako broj average niti ostaje ispod 550, kapacitet skup skaliranje se smanjuje za jednu i stroj je uklonjena iz skupa.

## <a name="set-up-scaling-using-azure-powershell"></a>Postavite skaliranje pomoću komponente PowerShell Azure

Da biste vidjeli Primjeri pomoću komponente PowerShell da biste postavili autoscaling, pogledajte [Azure Monitor PowerShell Brzi start uzorka](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Postavite skaliranje pomoću EŽA Azure

Da biste vidjeli Primjeri korištenja Azure EŽA da biste postavili autoscaling, pogledajte [EŽA različite platforme Azure Monitor Brzi start uzorka](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Postavite skaliranje pomoću portala za Azure

Da biste vidjeli primjer pomoću portala za Azure da biste postavili autoscaling, pogledajte [Stvaranje postavljanje na skaliranje virtualnog računala pomoću portala za Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Istražite skaliranja akcije

- [Portal za azure]() – trenutno možete dobiti tijekom ograničenog vremenskog podataka pomoću portala za.
- [Azure resursa Explorer]() – ovaj alat je najbolje za istraživanje trenutno stanje skupa mjerilo. Slijedite ovaj put i trebali biste vidjeti prikaz instancu skupa skaliranje koji ste stvorili: pretplate > {pretplate} > resourceGroups > {grupu resursa} > davatelji > Microsoft.Compute > virtualMachineScaleSets > {vaš skaliranje skup} > virtualMachines
- Azure PowerShell – koristite sljedeću naredbu da biste dobili neke informacije:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Povežite se s jumpbox virtualnog računala kao što bi bilo kojeg uređaja, a zatim možete pristupiti daljinski virtualnim strojevima u skale postavljanje praćenje pojedinačnih procesa.

## <a name="next-steps"></a>Daljnji koraci

- Pogledajte [automatski promijeniti omjer strojeva u skupu na skaliranje virtualnog računala](virtual-machine-scale-sets-windows-autoscale.md) da biste vidjeli primjera kako stvoriti skalu postaviti automatsko skaliranje konfiguriran.
- Pronalaženje Primjeri Azure Monitor nadzor značajke u [Azure Monitor PowerShell Brzi start uzorka](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Informirajte se o značajkama obavijesti u [Korištenje automatsko skaliranje akcija za slanje e-pošte i webhook upozorenja u Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Upute za [korištenje nadzora zapisnike za slanje e-pošte i webhook upozorenja u Azure monitora](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Saznajte više o [Napredne automatsko skaliranje scenarijima](./virtual-machine-scale-sets-advanced-autoscale.md).
