<properties
    pageTitle="Strujanje Azure dijagnostičke zapisnike događaja koncentratora | Microsoft Azure"
    description="Saznajte kako strujanje Azure dijagnostičke zapisnike događaja koncentratora."
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
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Strujanje Azure dijagnostičke zapisnike s koncentratorima događaja

**[Azure dijagnostičke zapisnike](monitoring-overview-of-diagnostic-logs.md)** moguće je strujanjem u stvarnom vremenu u najbliži na bilo koji program pomoću ugrađenih mogućnost "Izvoz da biste događaj koncentratora" na portalu ili tako da omogućite servis za Id pravilo Bus dijagnostičkih postavkom pomoću cmdleta ljuske PowerShell Azure ili Azure EŽA.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Što učiniti s dijagnostičkog zapisnika i koncentratora događaja
Evo tek nekoliko načina mogu koristiti strujanje mogućnost dijagnostičkih zapisnika:

- **Zapisnici strujanje 3 strana zapisivanje i sustavi za telemetriju** – s vremenom događaj koncentratora strujanje će postati mehanizam pipe dijagnostičke zapisnike u SIEMs trećih strana i prijava analitiku rješenja.

- **Prikaz stanja servisa putem strujanja podataka PowerBI "tipkovni put"** – pomoću koncentratora događaj, strujanje analize i PowerBI, možete jednostavno transformirati Dijagnostika podatke u blizini uvida u stvarnom vremenu na servisa Azure. [U ovom se članku dokumentacija daje sjajan pregled uputa za postavljanje programa koncentratora događaj, obradu podataka pomoću analize strujanje i koristiti PowerBI kao na Izlaz](../stream-analytics/stream-analytics-power-bi-dashboard.md). Evo nekoliko savjeta za početak postaviti s dijagnostičkih zapisnika:
    - Događaj koncentratora za kategoriju dijagnostičke zapisnike automatski stvara kada potvrdite mogućnost na portalu ili omogućivanje putem komponente PowerShell, tako da biste odabrali koncentratora događaja u naziva servisa Bus čiji naziv započinje rečenicom "uvida-"
    - Slijedi primjer strujanje analize upita možete koristiti samo raščlaniti svih zapisnika podataka u tablici PowerBI:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Stvaranja prilagođenih telemetrijskih i zapisivanje platformu** – ako već imate custom-built telemetrijskih platformu ili su samo mislim o izradi jednu, Visoko skalabilni objavljivanja-pretplate prirode događaj koncentratora omogućuje vam da flexibly ingest dijagnostičke zapisnike. [Vodič za potražite u članku Dan Rosanova korisnika pomoću koncentratora događaja u platforma globalni skaliranje telemetrijskih ovdje](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Omogući strujanje dijagnostičkih zapisnika
Možete omogućiti programski streaming dijagnostičkih zapisnika putem portala, ili pomoću [Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/dn931943.aspx). U svakom slučaju, odaberite polje naziva za Bus servisa i programa događaja koncentratora će se stvoriti u naziva za svaku kategoriju zapisnika omogućite. Vrsta zapisnika koji ostvaruju resurs je dijagnostičkih **Zapisnika kategorije** . Možete odabrati želite li prikupiti za određeni resurs na portalu Azure u odjeljku plohu Dijagnostika kategorija zapisnika.

![Zapisnik kategorija na portalu](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Omogućivanje i strujanje dijagnostičke zapisnike s računalnim resursa (na primjer, VMs ili servis tkanina) [potreban je drugi skup koraka](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Pomoću cmdleta ljuske PowerShell
Da biste omogućili strujanje putem [Cmdleta ljuske PowerShell Azure](insights-powershell-samples.md), možete koristiti u `Set-AzureRmDiagnosticSetting` cmdlet s ovim parametrima:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Servis Bus pravilo ID je niz u ovom obliku: `{service bus resource ID}/authorizationrules/{key name}`, na primjer, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Putem Azure EŽA
Da biste omogućili strujanje putem [Azure EŽA](insights-cli-samples.md), možete koristiti u `insights diagnostic set` naredbe ovako:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Koristiti isti oblik za servis Bus pravilo ID-a prema uputama za Cmdlet komponente PowerShell.

###<a name="via-azure-portal"></a>Putem portala za Azure
Da biste omogućili strujanje putem portala za Azure, dođite do postavki dijagnostike resursa i odaberite "Izvoz događaja koncentrator."

![Izvoz s koncentratorima događaja na portalu](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Da biste konfigurirali, odaberite postojeći servis Bus prostor naziva. Prostor za naziv odabrana bit će koji koncentratora događaja (Ako je ovo vaš prvi put strujanje dijagnostičke zapisnike) ili strujanjem da biste (Ako su već resurse koji su strujanje te kategorije zapisnika za ovo polje naziva), i pravila definira dozvole koje ima mehanizam za strujanje. Danas, strujanje sustava koncentratora događaj potrebna je dozvola za upravljanje, čitanje i slanje. Možete stvoriti ili izmijeniti polje naziva Bus servisa zajednički pristup pravila na portalu klasični na kartici "Konfiguriraj" za vaše polje naziva Bus servisa. Da biste ažurirali jednu od tih postavki dijagnostike, klijent morate imati dozvolu za ListKey pravila za provjeru autentičnosti Bus servisa.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Kako se koristi podatke iz koncentratora za događaj?
Evo oglednih podataka za izlaz iz koncentratora za događaj:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Naziv elementa | Opis                                            |
|--------------|--------------------------------------------------------|
|zapisa       | Polje svih zapisnika događaja u ovom tereta.            |
|vrijeme          | Vrijeme pojavila događaj.                      |
|Kategorija      | Zapisnik kategorija za ovaj događaj.                           |
|resourceId    | ID resursa resursa koje generira ovaj događaj. |
|operationName | Naziv operacije.                                 |
|Razina         | Neobavezno. Pokazuje razinu zapisnika događaja.               |
|Svojstva    | Svojstva događaja.                               |


Možete pogledati popis svih proizvođača resursa koji podržavaju strujanje događaja koncentrator [ovdje](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Daljnji koraci
- [Saznajte više o Azure dijagnostičkih zapisnika](monitoring-overview-of-diagnostic-logs.md)
- [Početak rada s koncentratorima događaja](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
