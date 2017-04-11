<properties
    pageTitle="Konfiguriranje programa webhook u zapisnik aktivnosti Azure upozorenja | Microsoft Azure"
    description="Pogledajte kako se pomoću upozorenja zapisnik aktivnosti da biste nazvali webhooks. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Konfiguriranje programa webhook na upozorenja programa Azure zapisnik aktivnosti

Webhooks omogućuju da biste usmjerili Azure upozorenje na drugi sustavi za naknadno obradi ili prilagođene akcije. Možete koristiti na webhook na upozorenja da biste usmjerili na servise koje slanje SMS, prijavu programskih pogrešaka, obavijestiti tim putem servisa razgovora i poruka ili bilo koji broj druge akcije. U ovom se članku opisuje kako postaviti na webhook na upozorenju zapisnik aktivnosti Azure i izgleda opseg za HTTP POST da biste na webhook. Dodatne informacije o postavljanje i sheme za Azure metričkim upozorenja, [umjesto toga pogledajte ovu stranicu](insights-webhooks-alerts.md). Možete postaviti upozorenje koje zapisnik aktivnosti slanje e-pošte kada aktiviran.

>[AZURE.NOTE] Ta je značajka trenutno u pretpregledu, pa očekivati varijable kvalitete i performansi.

Možete postaviti upozorenja zapisnik aktivnosti pomoću [Cmdleta ljuske PowerShell Azure](insights-powershell-samples.md#create-alert-rules), [Više platformi EŽA](insights-cli-samples.md#work-with-alerts)ili [Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Provjera autentičnosti na webhook
Mobilna webhook možete provjeriti autentičnost pomoću neku od ovih metoda:

1. **Autorizacija utemeljen na token** - webhook URI sprema tokena ID-om, npr. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Osnovni autorizacije** - webhook URI sprema se pomoću korisničkog imena i lozinke, npr. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Shema tereta
Operacija objavu sadrži sljedeće JSON tereta i sheme za sva upozorenja temeljenih na zapisnik aktivnosti. U ovom shema je slično onome koji se koriste upozorenja temeljenih na metriku.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Naziv elementa       |Opis|
|---                |---|
|status             |Koristi se za metričkim upozorenja. Uvijek postavljeno na "aktivirati" za zapisnik aktivnosti upozorenja.|
|Kontekst            |Kontekst događaja.|
|resourceProviderName|Davatelja resursa impacted resursa.|
|conditionType      |Uvijek "događaja."|
|ime               |Naziv upozorenja pravila.|
|ID-a                 |ID resursa upozorenje.|
|Opis        |Opis kao što je postavljeno tijekom stvaranja upozorenja upozorenja.|
|subscriptionId     |Azure pretplate ID-a.|
|Vremenska oznaka          |Vrijeme u kojem će se događaj sastavio Azure servis koji obrade zahtjeva.|
|resourceId         |ID resursa impacted resursa.|
|resourceGroupName  |Naziv grupe resursa impacted resursa|
|Svojstva         |Skup `<Key, Value>` parove (odnosno `Dictionary<String, String>`) koji sadrži detalje o događaju.|
|događaj              |Element koji sadrži metapodatke o događaju.|
|autorizacija      |Svojstva RBAC događaja. Obično uključuju "Akcije", "uloga" i "opseg".|
|Kategorija           |Kategorija događaja. Podržani vrijednosti uključuju: administratora, upozorenja, sigurnost, ServiceHealth, preporuke.|
|pozivatelja             |Adresa e-pošte korisnika koji je izvršio operacije, UPN zahtjeva i SPN zahtjeva koji se temelji na dostupnost. Može biti null za određene sustava pozive.|
|correlationId      |Obično GUID u obliku niza. Događaji s correlationId pripadate istu veće akciju i obično zajednički koristite s correlationId.|
|eventDescription   |Statički tekst opis događaja.|
|eventDataId        |Jedinstveni identifikator za događaj.|
|eventSource        |Naziv servisa Azure ili infrastrukture koji je generirao događaj.|
|httpRequest        |Obično obuhvaća "clientRequestId", "clientIpAddress" i "metode" (HTTP metoda npr STAVITI).|
|Razina              |Jedna od sljedećih vrijednosti: "Ključnih", "Pogreška", "Upozorenje", "Informational" i "Tekstni."|
|operationId        |Obično GUID razdijeliti događaje koje odgovaraju jedne operacije.|
|operationName      |Naziv operacije.|
|Svojstva         |Svojstva događaja.|
|status             |Niz. Status operacije. Uobičajeni vrijednosti uključuju: "Pokrenuto", "U tijeku", "Nije uspjela", "Nije uspjela", "Aktivno", "Riješiti".|
|subStatus          |Obično obuhvaća HTTP kod stanja odgovarajuće OSTALE poziva. Mogu sadržavati i ostalih nizova s opisom na substatus. Uobičajenih vrijednosti substatus uključuju: u redu (HTTP kod stanja: 200), stvorene (Šifra stanja HTTP: 201), prihvaćena (HTTP kod stanja: 202), ne sadržaj (Šifra stanja HTTP: 204), Neispravan zahtjev (HTTP kod stanja: 400), nije pronađen (Šifra stanja HTTP: 404), sukoba (Šifra stanja HTTP: 409), Interna pogreška poslužitelja (HTTP kod stanja: 500), servis nije dostupan (Šifra stanja HTTP: 503), Prekoračenje vremena za pristupnik (HTTP kod stanja: 504)|

## <a name="next-steps"></a>Daljnji koraci
- [Dodatne informacije o zapisnik aktivnosti](monitoring-overview-activity-logs.md)
- [Izvršavanje Azure Automatizacija skripte (Runbooks) na Azure upozorenja](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Koristite aplikaciju logike slanje SMS poruka PUTEM putem Twilio iz Azure upozorenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). U ovom se primjeru je metričkim upozorenja, ali nije moguće izmijeniti da biste radili s upozorenje zapisnik aktivnosti.
- [Koristite logike aplikaciju da biste poslali zalihe poruku iz Azure upozorenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). U ovom se primjeru je metričkim upozorenja, ali nije moguće izmijeniti da biste radili s upozorenje zapisnik aktivnosti.
- [Koristite logiku aplikaciju da biste poslali poruku Azure red čekanja iz Azure upozorenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). U ovom se primjeru je metričkim upozorenja, ali nije moguće izmijeniti da biste radili s upozorenje zapisnik aktivnosti.
