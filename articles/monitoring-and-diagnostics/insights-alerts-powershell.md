<properties
    pageTitle="Stvaranje upozorenja za Azure servise pomoću komponente PowerShell | Microsoft Azure"
    description="Korištenje ljuske PowerShell za stvaranje Azure upozorenja koja se može pokrenuti obavijesti ili Automatizacija kada se ispuni uvjet koji navedete."
    authors="rboucher"
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Stvaranje upozorenja za Azure servise pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [EŽA](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Pregled

U ovom se članku objašnjava postavljanje Azure upozorenja pomoću komponente PowerShell.  

Možete primati upozorenja na temelju nadzora metriku ili događaji na servisa Azure.

- **Metričkim vrijednostima** - okidača upozorenja kada se vrijednost navedena metrika presjek prag dodijelite u bilo kojem smjeru. To jest, ga i pokreće kada najprije se ispuni uvjet, a zatim naknadno kada koji uvjet se više ne zadovoljavaju.    
- **Aktivnosti zapisnika događaja** - upozorenje koje možete pokrenuti *svaki* događaj ili, samo kad se pojave određeni broj događaja.

Možete konfigurirati upozorenje kada se pokrene, učinite sljedeće:

- slanje obavijesti e-poštom administrator servisa i dodatnih administratora
- Slanje e-pošte dodatna poruke e-pošte koju navedete.
- poziv na webhook
- pokretanje izvođenja Azure kompilacije (samo na portalu Azure)

Možete konfigurirati i informacije o upozorenja pravila pomoću

- [Portal za Azure](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [sučelje naredbenog retka (EŽA)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-JA](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Dodatne informacije potražite uvijek upisujete ```get-help``` , a zatim naredbu komponente PowerShell koji tražite pomoć.

## <a name="create-alert-rules-in-powershell"></a>Stvaranje upozorenja pravila u ljusci PowerShell

1. Prijavite se u sustav Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Dobit ćete popis od pretplata koje su vam dostupne. Provjerite radite desnom pretplate. Ako nije, postavite ga na pravom pomoću Izlaz iz `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Da biste popis postojeća pravila na grupu resursa, koristite sljedeću naredbu:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Da biste stvorili pravilo, morate najprije koristiti nekoliko važne informacije. 
    - **ID resursa** za resurs koji želite postaviti upozorenja za
    - **Metričkim definicije** dostupne za resursa

    Da biste dobili ID resursa je pomoću portala za Azure. Pod pretpostavkom da resursa već stvorili, odaberite ga na portalu. Zatim u sljedeći plohu odaberite *Svojstva* u odjeljku *Postavke* . Polje u sljedećem plohu je Identifikator RESURSA. Drugi način je da biste pomoću [Programa Explorer Azure resursa](https://resources.azure.com/).

    Primjer id resursa za web-aplikacije je

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Možete koristiti `Get-AzureRmMetricDefinition` da biste vidjeli popis svih metričkim definicija za određeni resurs.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Sljedeći primjer generira tablicu s metriku naziv i jedinica za tu metriku.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Potpuni popis dostupnih mogućnosti za dohvaćanje AzureRmMetricDefinition dostupna tako da pokrenete Get-MetricDefinitions.


5. Sljedeći primjer postavlja upozorenja na resursa web-mjesta. Upozorenja okidača kad god dosljedno primi sve promet za pet minuta i ponovno kad primi nema promet za pet minuta.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Da biste stvorili webhook ili slanje e-pošte kada upozorenja, najprije stvorite e-pošte i/ili webhooks. Zatim odmah stvoriti pravilo naknadno-oznakom akcije i kao što je prikazano u sljedećem primjeru. Webhook ili poruke e-pošte s ne može povezati već stvorili pravila PowerShell.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Da biste stvorili upozorenja koja se pokreće na određeni uvjet u zapisnik aktivnosti, koristite naredbe u sljedećem obliku

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    -OperationName odgovara vrsti događaja u zapisnik aktivnosti. Primjeri *Microsoft.Compute/virtualMachines/delete* i *microsoft.insights/diagnosticSettings/write*.

    Koristite naredbu komponente PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) da biste dobili popis mogućih operationNames. Umjesto toga koristite Azure portal za slanje upita zapisnik aktivnosti i pronalaženje konkretnih prošle operacije koje želite stvoriti upozorenje. Operacije u prikazu grafika zapisnika neslužbeni imena. Traženje JSON za unos i izvući OperationName vrijednost.   

8. Provjerite da upozorenja je stvorio pravilno pogled na pojedinačne pravila.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Brisanje upozorenja. Te naredbe izbrišite pravila koja je stvoreno u ovom članku.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Daljnji koraci

* [Pregled Azure nadzor](monitoring-overview.md) uključujući vrste informacija mogu prikupljanje i praćenje.
* Saznajte više o [Konfiguriranje webhooks u upozorenja](insights-webhooks-alerts.md).
* Dodatne informacije o [Runbooks Automatizacija Azure](..\automation\automation-starting-a-runbook.md).
* Da biste dobili [Pregled prikupljanje dijagnostičkih zapisnika](monitoring-overview-of-diagnostic-logs.md) da biste prikupili detaljne velika učestalost mjernih podataka o vašoj usluzi.
* Pogledajte [Pregled metriku zbirke](insights-how-to-customize-monitoring.md) da biste bili sigurni na servisu dostupan je i odredište.
