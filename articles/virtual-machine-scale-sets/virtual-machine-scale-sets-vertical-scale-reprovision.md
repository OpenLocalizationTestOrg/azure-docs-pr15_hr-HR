<properties
    pageTitle="Okomito mjerilo Azure virtualnog računala skaliranje skupove | Microsoft Azure"
    description="Kako okomito skaliranje virtualnog računala kao odgovor na nadzor upozorenja s Automatizacija Azure"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Okomiti automatsko skaliranje s skupovima skaliranje virtualnog računala

U ovom se članku opisuje kako okomito skaliranja Azure [Skupove skaliranje virtualnog računala](https://azure.microsoft.com/services/virtual-machine-scale-sets/) sa ili bez reprovisioning. Okomito skaliranje VMs koji nisu u skupovima mjerilo, potražite [okomito skaliranje Azure virtualnog računala s Azure automatizaciju](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Okomito skaliranje, poznata i kao _proširenja_ i _Promjena veličine prema dolje_, znači da se povećava ili smanjuje veličina virtualnog računala (VM) kao odgovor na radno opterećenje. Usporediti s [Vodoravno skaliranje](./virtual-machine-scale-sets-autoscale-overview.md)nazivaju _Skaliranje izgleda_ i _Skaliranje u_kojem se mijenja broj VMs ovisno o tome povećavaju.

Reprovisioning znači da uklonite postojeće VM i zamjene novom. Kada ne povećate ili smanjite veličinu VMs u skupu na skaliranje VM, u nekim slučajevima želite promijeniti veličinu postojeće VMs i zadržali podatke, dok je u drugim slučajevima morate uvesti nove VMs novu veličinu. Ovaj dokument pokriva oba slučaja.

Okomito skaliranje može biti korisna kada:

- Servis utemeljena na virtualnim strojevima je u odjeljku-koristi (na primjer pri vikenda). Smanjivanje veličine VM možete smanjiti mjesečni troškove.
- Povećanje VM veličina da biste cope s većim zahtjev bez stvaranja dodatnih VMs.

Možete postaviti okomito skaliranje biti okidačima ovisno o upozorenja metričkim na temelju iz skupa VM mjerilo. Kada se aktivira upozorenje ga aktivira na webhook te okidača runbook koji se mogu mijenjati veličinu vaš skaliranje postavite prema gore ili dolje. Okomito skaliranje može konfigurirati tako da slijedite ove korake:

1. Stvorite račun za automatizaciju Azure s mogućnošću pretraživanja kroz razine izvoditi.
2. Azure Automatizacija okomito mjerilo runbooks za skupove skaliranje VM uvesti u svoju pretplatu.
3. Dodajte u webhook vaše runbook.
4. Dodajte upozorenja na VM skaliranje skupa pomoću webhook obavijest.

> [AZURE.NOTE] Okomiti autoscaling možete samo odvija unutar određenog raspona VM veličina. Usporedba specifikacije svaki veličinu prije no što odlučite da biste skalirali iz jedne u drugu (veći broj nije uvijek označava veći veličina VM). Možete odabrati da biste skalirali između Sljedeći parovi veličina:

>| Veličina VM skaliranja par |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Stvorite račun za automatizaciju Azure s mogućnošću pretraživanja kroz razine izvoditi

Prvo što biste trebali učiniti je stvorite račun za Azure Automatizacija koji će biti smješteno runbooks koji se koristi za promjenu veličine instance postavite skaliranje VM. Nedavno [Azure Automatizacija](https://azure.microsoft.com/services/automation/) uvedena značajku "Pokreni kao račun" što čini postavka gore glavnice za servis za automatsko pokretanje na runbooks u ime korisnikove vrlo lako. Dodatne informacije o tome u nastavku članka:

* [Autentičnost Runbooks s računom za Azure Pokreni kao](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Uvoz Azure Automatizacija okomito mjerilo runbooks u pretplatu

Runbooks potrebne za okomito skaliranje svoje skupove skaliranje VM su već objavljene u galeriji Azure Automatizacija Runbook. Da biste uvezli ih u svoju pretplatu slijedite korake u ovom članku:

* [Galerija Runbook i module za automatizaciju Azure](../automation/automation-runbook-gallery.md)

Odaberite mogućnost galerije Pregledaj na izborniku Runbooks:

![Runbooks za uvoz][runbooks]

Prikazane su runbooks koji su potrebni za uvoz. Odaberite runbook ovisno o tome želite okomito skaliranje sa ili bez reprovisioning:

![Galerija Runbooks][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Dodavanje u webhook vaše runbook

Kada uvezete runbooks morat ćete dodati na webhook na runbook tako da ga može aktivirati upozorenja iz skupa na skaliranje VM. Detalje o stvaranju na webhook za svoje Runbook su opisani u ovom članku:

* [Azure webhooks automatizaciju](../automation/automation-webhooks.md)

> [AZURE.NOTE] Provjerite je li kopirate webhook URI prije nego što zatvorite dijaloški okvir webhook kao što je to ćete u sljedećem odjeljku.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Dodavanje upozorenja da biste postavili VM skala

U nastavku je skriptu PowerShell koji pokazuje kako dodati upozorenja da biste postavili na skaliranje VM. Pogledajte članak Dohvaćanje naziva metrika pokreću upozorenje na: [Azure Monitor autoscaling uobičajenih metriku](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Preporučuje se da biste konfigurirali umjerene termin upozorenja da biste izbjegli pokretanje okomito skaliranje i pridružene usluge prekida, često. Preporučujemo da prozor najmanje 20 – 30 minuta ili više njih. Razmislite o vodoravnog skaliranja ako vam je potrebna da biste izbjegli prekida.

Dodatne informacije o stvaranju upozorenja potražite u sljedećim člancima:

* [Azure Monitor PowerShell uzorka za brzi početak rada](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure uzoraka Monitor različite platforme EŽA brzi početak rada](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Sažetak

U ovom se članku prikazivao jednostavan Primjeri okomitog skaliranja. Obogaćeni raznih događaje s prilagođeni skup akcija možete se povezati s te sastavne blokove - račun Automatizacija, runbooks, webhooks, upozorenja.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
