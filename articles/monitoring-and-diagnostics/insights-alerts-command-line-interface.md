<properties
    pageTitle="Stvaranje upozorenja za Azure servise pomoću sučelje naredbenog retka spremno različite platforme (EŽA) | Microsoft Azure"
    description="Koristite sučelje naredbenog retka spremno za stvaranje Azure upozorenja koja se može pokrenuti obavijesti ili Automatizacija kada se zadovolje uvjet koji navedete."
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Stvaranje upozorenja za Azure servise pomoću sučelje naredbenog retka spremno različite platforme (EŽA)

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [EŽA](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Pregled

U ovom se članku objašnjava postavljanje Azure upozorenja pomoću sučelja naredbeni redak (EŽA).

>[AZURE.NOTE] Azure monitora novi je naziv za što je pod nazivom "Azure uvida" do rujan 25, 2016. Međutim, prostori a time i ispod naredbi i dalje sadržavati "uvida".

Možete primati upozorenja na temelju nadzora metriku ili događaji na servisa Azure.

- **Metričkim vrijednostima** - okidača upozorenje kada se vrijednost navedena metrika siječe prag dodijelite u bilo kojem smjeru. To je ga i pokreće kada najprije se ispuni uvjet, a zatim naknadno kada koji uvjet se više ne zadovoljavaju.    
- **Aktivnosti zapisnika događaja** - upozorenje koje možete pokrenuti *svaki* događaj ili, samo kad se pojave određeni broj događaja.

Možete konfigurirati upozorenje kada se pokrene, učinite sljedeće:

- slanje obavijesti e-poštom administrator servisa i dodatnih administratora
- Slanje e-pošte dodatna poruke e-pošte koju navedete.
- poziv na webhook
- pokretanje izvođenja Azure kompilacije (samo na Azure portalu trenutno)

Možete konfigurirati i informacije o upozorenja pravila pomoću

- [Portal za Azure](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [sučelje naredbenog retka (EŽA)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-JA](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Uvijek možete primiti pomoć za naredbe upisivanjem naredbe i stavljanje - pomoć na kraju. Ako, na primjer:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Stvaranje upozorenja pravila pomoću na EŽA

1. Izvođenje preduvjeti i prijavite se na Azure. Potražite u članku [EŽA Azure monitoru uzorka](insights-cli-samples.md). Ukratko, instalirajte na EŽA i pokrenuli te naredbe. Početak prijavljeni, prikaz vrsta pretplate koriste i Priprema pokretanje naredbe Azure Monitor.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Da biste popis postojeća pravila na grupu resursa, koristite sljedeće obrasca **azure uvida upozorenja pravilo popis** *[mogućnosti] &lt;resourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Da biste stvorili pravilo, morate najprije koristiti nekoliko važne informacije.
    - **ID resursa** za resurs koji želite postaviti upozorenja za
    - **Metričkim definicije** dostupne za resursa

    Da biste dobili ID resursa je pomoću portala za Azure. Pod pretpostavkom da resursa već stvorili, odaberite ga na portalu. Zatim u sljedeći plohu odaberite *Svojstva* u odjeljku *Postavke* . *ID RESURSA* je polje u sljedećem plohu. Drugi način je da biste pomoću [Programa Explorer Azure resursa](https://resources.azure.com/).

    Primjer id resursa za web-aplikacije je

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Da biste dobili popis dostupnih metriku i jedinice za te metriku u prethodnom primjeru resursa, koristite sljedeću naredbu EŽA:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ je preciznosti dostupna mjera (1-minutni intervala). Korištenje različitih granularnosti vam različite metričkim mogućnosti.


4. Da biste stvorili utemeljen na metriku upozorenja pravilo, naredbom sljedećem obliku:

    **Postavljanje upozorenja pravilo metriku Azure uvida** *[mogućnosti] &lt;ruleName&gt; &lt;mjesto&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operator&gt; &lt;prag&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    Sljedeći primjer postavlja upozorenja na resursa web-mjesta. Upozorenja okidača kad god dosljedno primi sve promet za pet minuta i ponovno kad primi nema promet za pet minuta.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Da biste stvorili webhook ili slanje e-pošte kada aktivira upozorenja, najprije stvorite e-pošte i/ili webhooks. Odmah Stvori pravilo naknadno. Webhook ili poruke e-pošte s ne može povezati već stvorili pravila pomoću na EŽA.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Da biste stvorili upozorenje aktivira na određeni uvjet u zapisnik aktivnosti, pomoću obrasca:

    **pravilo upozorenja uvida zapisati skup podataka** *[mogućnosti] &lt;ruleName&gt; &lt;mjesto&gt; &lt;resourceGroup&gt; &lt;operationName&gt; *

    Na primjer

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    Na operationName odgovara događaj za unos u zapisnik aktivnosti. Primjeri *Microsoft.Compute/virtualMachines/delete* i *microsoft.insights/diagnosticSettings/write*.

    Koristite naredbu komponente PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) da biste dobili popis mogućih operationNames. Umjesto toga koristite Azure portal za slanje upita zapisnik aktivnosti i pronalaženje konkretnih prošle operacije koje želite stvoriti upozorenje. Operacije u prikazu grafika zapisnika neslužbeni imena. Traženje JSON za unos i izvući OperationName vrijednost.   

7. Možete provjeriti da upozorenja je stvorio pravilno pogled na pojedinačne pravilo.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Da biste izbrisali pravila, koristite naredbu obrasca:

    **Brisanje pravila uvida upozorenja** [mogućnosti] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Te naredbe izbrišite pravila koja je prethodno stvorena u ovom članku.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Daljnji koraci

* [Pregled Azure nadzor](monitoring-overview.md) uključujući vrste informacija mogu prikupljanje i praćenje.
* Saznajte više o [Konfiguriranje webhooks u upozorenja](insights-webhooks-alerts.md).
* Dodatne informacije o [Runbooks Automatizacija Azure](..\automation\automation-starting-a-runbook.md).
* Da biste dobili [Pregled prikupljanje dijagnostičkih zapisnika](monitoring-overview-of-diagnostic-logs.md) da biste prikupili detaljne velika učestalost mjernih podataka o vašoj usluzi.
* Pogledajte [Pregled metriku zbirke](insights-how-to-customize-monitoring.md) da biste bili sigurni na servisu dostupan je i odredište.
