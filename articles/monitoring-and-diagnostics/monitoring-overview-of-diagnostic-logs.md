<properties
    pageTitle="Pregled Azure dijagnostičke zapisnike | Microsoft Azure"
    description="Saznajte što su Azure dijagnostičke zapisnike te kako ih možete koristiti da biste shvatili događaja unutar Azure resursa."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Pregled Azure dijagnostičkih zapisnika
**Dijagnostičke zapisnike Azure** su zapisnika čuje po resursa koje daju bogatih, često korištenih podatke o postupka resursa. Sadržaj ove zapisnika mijenja se po vrsti resursa (, na primjer, sustava Windows evidenciji jedna kategorija dijagnostičkog zapisnika za VMs i blob, tablice te reda čekanja zapisnika kategorije dijagnostičke zapisnike za račune za pohranu) razlikovati od u [zapisnik aktivnosti (prijašnjeg zapisnik nadzora ili radu zapisnik)](monitoring-overview-activity-logs.md), koji pruža uvid u operacije koje su izvršene na resursa u svoju pretplatu. Resursi za sve podržava novu vrstu dijagnostičke zapisnike što je opisano u nastavku. Na popisu podržava usluga ispod navedene vrste resursa koje podržavaju novi dijagnostičke zapisnike.

![Logička položaj dijagnostičkih zapisnika](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Što sve možete raditi sa zapisnicima dijagnostičkih podataka
Evo nekoliko stvari koje možete raditi s dijagnostičkih zapisnika:

- Možete ih spremiti na **Račun za pohranu** za nadzor ili ručno provjere. Možete postaviti vrijeme zadržavanja (u danima) pomoću **Dijagnostičkih postavki**.
- Za ingestion servisa drugih proizvođača ili prilagođeni analize rješenja kao što su PowerBI [strujanje ih s **Koncentratorima događaj** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) .
- Analizirati ih s [OMS zapisnika Analytics](../log-analytics/log-analytics-azure-storage-json.md)

## <a name="diagnostic-settings"></a>Dijagnostičkih postavki
Dijagnostičke zapisnike za resurse koji nisu računalnim konfigurirane su dijagnostičkih postavki. **Dijagnostičkih postavki** za kontrolu resursa:

- Gdje dijagnostičke zapisnike šalju (račun za pohranu, koncentratora za događaj i/ili OMS prijava analitiku).
- Kategorija zapisnika šalju.
- Koliko svake kategorije zapisnika mora biti zadržani na računu za pohranu – zadržavanja nula dana znači da se zapisnici čuvaju zauvijek. U suprotnom, tu vrijednost možete u rasponu od 1 do 2147483647. Ako se postavljaju pravilnika o zadržavanju, ali spremanje zapisnika u račun za pohranu onemogućen (na primjer ako samo su odabrane mogućnosti koncentratora događaj ili OMS), pravila zadržavanja imati utjecaja.

Te jednostavno konfigurirane postavke putem plohu Dijagnostika resursa na portalu za Azure, putem naredbe ljuske PowerShell Azure i EŽA ili putem [Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [AZURE.WARNING] Dijagnostičke zapisnike metriku računalnim resursa (na primjer, VMs ili servis tkanina) koristiti [odvojene mehanizam za konfiguraciju i Odabir izlaza](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Kako omogućiti skup dijagnostičkih zapisnika
Skup dijagnostičke zapisnike moguće je omogućiti kao dio stvaranja resursa ili nakon stvaranja resursa putem plohu resursa na portalu. Dijagnostičke zapisnike možete omogućiti i u bilo kojem trenutku pomoću naredbe ljuske PowerShell Azure ili EŽA ili korištenju Azure Monitor REST API-JA.

> [AZURE.TIP] Ove upute možda ne primjenjuju se izravno na svaki resurs. Pregledajte sheme veze pri dnu ove stranice da biste razumjeli posebne korake koji se odnose na određene vrste resursa.

[U ovom se članku prikazuje kako možete koristiti predložak resursa da biste omogućili dijagnostičkih postavki prilikom stvaranja resursa](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Omogućivanje dijagnostičke zapisnike na portalu
Dijagnostičke zapisnike možete omogućiti na portalu za Azure prilikom stvaranja vrste resursa računalnim omogućivanjem proširenja za Windows ili Linux Azure dijagnostiku:

1.  Idite na **Novo** , a zatim odaberite resurs koji vas zanima.
2.  Nakon konfiguriranja osnovne postavke, a zatim odaberete veličinu, u plohu **Postavke** , u odjeljku **nadzor**, odaberite **omogućeno** , a zatim na račun za pohranu gdje želite spremiti dijagnostičke zapisnike. Kada šaljete Dijagnostika s računom za pohranu vam se naplatiti stope podatke za pohranu i transakcije.

    ![Omogućivanje dijagnostičke zapisnike tijekom stvaranja resursa](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Kliknite **u redu** pa stvorite resurs.

Za resurse koji nisu računalnim možete omogućiti dijagnostičke zapisnike na portalu za Azure nakon stvaranja resursa tako da učinite sljedeće:

1.  Idite na plohu resursa i otvorite plohu **Dijagnostika** .
2.  Kliknite **na** , a zatim odaberite račun za pohranu i/ili koncentratora za događaj.

    ![Omogućivanje dijagnostičke zapisnike nakon stvaranja resursa](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  U odjeljku **zapisnike**, odaberite **Zapisnika kategorije** koju želite prikupiti ili strujanje.
4.  Kliknite **Spremi**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Omogućivanje dijagnostičke zapisnike PowerShell
Da biste omogućili dijagnostičke zapisnike putem cmdleta ljuske PowerShell Azure, koristite sljedeće naredbe.

Da biste omogućili prostora za pohranu dijagnostičke zapisnike na računu za pohranu, koristite sljedeću naredbu:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

ID računa za pohranu je identifikator resursa za pohranu račun na koju želite poslati zapisnike.

Da biste omogućili strujanje dijagnostičkih zapisnika programa koncentrator događaj, koristite sljedeću naredbu:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

Servis Bus pravilo ID je niz u ovom obliku: `{service bus resource ID}/authorizationrules/{key name}`.

Da biste omogućili slanje dijagnostičkih zapisnika s radnim prostorom prijava analitiku, koristite sljedeću naredbu:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] Parametar WorkspaceId nije dostupna u izdanju listopad. Će postati dostupna u studenom izdanju.

Možete dobiti svoj ID prijava analitiku radnog prostora na portalu za Azure.

Možete kombinirati parametara da biste omogućili više mogućnosti izlaza.

### <a name="enable-diagnostic-logs-via-cli"></a>Omogućivanje dijagnostičke zapisnike putem EŽA
Da biste omogućili dijagnostičke zapisnike putem EŽA Azure, koristite sljedeće naredbe:

Da biste omogućili prostora za pohranu dijagnostičke zapisnike na računu za pohranu, koristite sljedeću naredbu:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

ID računa za pohranu je identifikator resursa za pohranu račun na koju želite poslati zapisnike.

Da biste omogućili strujanje dijagnostičkih zapisnika programa koncentrator događaj, koristite sljedeću naredbu:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

Servis Bus pravilo ID je niz u ovom obliku: `{service bus resource ID}/authorizationrules/{key name}`.

Da biste omogućili slanje dijagnostičkih zapisnika s radnim prostorom prijava analitiku, koristite sljedeću naredbu:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] Parametar workspaceId nije dostupna u izdanju listopad. Će postati dostupna u studenom izdanju.

Možete dobiti svoj ID prijava analitiku radnog prostora na portalu za Azure.

Možete kombinirati parametara da biste omogućili više mogućnosti izlaza.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Omogućivanje dijagnostičke zapisnike putem REST API-JA
Da biste promijenili dijagnostičkih postavki pomoću Azure Monitor REST API-JA, pogledajte [ovaj dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Upravljanje postavkama dijagnostičkih na portalu

Da biste bili sigurni da svi resurse pravilno postavljeni s dijagnostičkih postavki, dođite do plohu **praćenja** na portalu i otvorite plohu **Dijagnostičke zapisnike** .

![Dijagnostički zapisnici plohu na portalu](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Možda ćete morati kliknite "Dodatne services" da biste pronašli plohu praćenja.

U ovom plohu možete pregledavati i svi resursi koji podržavaju dijagnostičke zapisnike da biste vidjeli ako imaju Dijagnostika omogućeno i koji račun za pohranu, koncentrator za događaj i/ili prijava analitiku radnog prostora se one zapisnika slijedi da biste filtrirali.

![Dijagnostičke zapisnike plohu rezultate na portalu](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Klikom na resurs prikazivat će se sve zapisnika koji su pohranjeni u račun za pohranu i dati mogućnost da biste isključili ili izmjena dijagnostičkih postavki. Kliknite ikonu za preuzimanje da biste preuzeli zapisnika za određeno vremensko razdoblje.

![Dijagnostički zapisnici plohu jedan resurs](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Dijagnostičke zapisnike samo se pojavljuju u tom prikazu i biti dostupno za preuzimanje ako ste konfigurirali dijagnostičkih postavki ih spremiti na račun za pohranu.

Klikom na vezu za **Dijagnostičkih postavki** će otvorili plohu dijagnostičkih postavki, gdje možete Omogućivanje, onemogućivanje i Izmjena postavki dijagnostike za odabrani resurs.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Podržane usluge i sheme za dijagnostički zapisnici
Shema dijagnostičke zapisnike ovisi o kategoriji resursa i zapisnika. U nastavku su podržane usluge i osobe sheme.

| Servis                       | Sheme i dokumenti                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Softver opterećenja     |    [Zapisnik analize Azure opterećenja (pretpregled)](../load-balancer/load-balancer-monitor-log.md)             |
|    Mrežni sigurnosne grupe    |    [Zapisnik analize mreže sigurnosnih grupa (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Aplikacija pristupnika       |    [Zapisivanje Dijagnostika za pristupnik za aplikaciju](../application-gateway/application-gateway-diagnostics.md)     |
|    Ključni zbirke ključeva                  |    [Azure ključa sigurnog zapisivanje](../key-vault/key-vault-logging.md)                                                 |
|    Azure pretraživanja               |    [Omogućivanje i korištenje analize promet za pretraživanje](../search/search-traffic-analytics.md)                         |
|    Spremište Lake podataka            |    [Pristupanju dijagnostičke zapisnike Azure podataka Lake spremišta](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Analitički Lake podataka        |    [Pristup dijagnostičke zapisnike Lake analitičkih podataka za Azure](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Logika aplikacije                 |    Nema sheme nije dostupna.                                                                                         |
|    Grupe za Azure                |    [Azure obradu Zapisivanje dijagnostičkih podataka](../batch/batch-diagnostics.md)                                              |
|    Automatizacija Azure           |    [Zapisnik analytics za automatizaciju Azure](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Koncentrator za događaj                  |    Nema sheme nije dostupna.                                                                                         |
|    Servis Bus                |    Nema sheme nije dostupna.                                                                                         |
|    Strujanje Analytics           |    Nema sheme nije dostupna.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Podržani zapisnika kategorije po vrsti resursa

|Vrsta resursa|Kategorija|Kategorija zaslonsko ime|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Zapisnici posla|
|Microsoft.Automation/automationAccounts|JobStreams|Strujanja posla|
|Microsoft.Batch/batchAccounts|ServiceLog|Servis zapisnika|
|Microsoft.DataLakeAnalytics/accounts|Nadzora|Zapisnika nadzora|
|Microsoft.DataLakeAnalytics/accounts|Zahtjevi za|Zahtjev za zapisnika|
|Microsoft.DataLakeStore/accounts|Nadzora|Zapisnika nadzora|
|Microsoft.DataLakeStore/accounts|Zahtjevi za|Zahtjev za zapisnika|
|Microsoft.EventHub/namespaces|ArchiveLogs|Arhiviranje zapisnika|
|Microsoft.EventHub/namespaces|OperationalLogs|Radu zapisnika|
|Microsoft.KeyVault/vaults|AuditEvent|Zapisnika nadzora|
|Microsoft.Logic/workflows|WorkflowRuntime|Dijagnostičke događaje u vrijeme izvođenja tijeka rada|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Događaj mreže sigurnosne grupe|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Brojač pravilo mreže sigurnosne grupe|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Mrežni sigurnosne grupe pravila tijek događaja|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Učitavanje opterećenja upozorenja događaja|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Učitavanje raspoređivača probni stanja stanja|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Zapisnik pristup pristupnika aplikacije|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Zapisnik performansama pristupnika aplikacije|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Zapisnik vatrozid pristupnika aplikacije|
|Microsoft.Search/searchServices|OperationLogs|Zapisnik postupka|
|Microsoft.ServerManagement/nodes|RequestLogs|Zahtjev za zapisnika|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Radu zapisnika|
|Microsoft.StreamAnalytics/streamingjobs|Izvršavanje|Izvršavanje|
|Microsoft.StreamAnalytics/streamingjobs|Prilikom stvaranja|Prilikom stvaranja|

## <a name="next-steps"></a>Daljnji koraci
- [Strujanje dijagnostičke zapisnike **događaja koncentratora**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Promjena dijagnostičkih postavki pomoću Azure Monitor REST API-JA](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Analiza zapisnike s OMS zapisnika Analytics](../log-analytics/log-analytics-azure-storage-json.md)
