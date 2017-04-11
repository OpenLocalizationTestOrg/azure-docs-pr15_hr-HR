<properties 
    pageTitle="Praćenje aplikacija logike u aplikacije servisa za Azure | Microsoft Azure" 
    description="Kako vidjeti što ste napravili logike aplikacije" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>Praćenje aplikacija u logike

Nakon [stvaranja logike aplikacije](app-service-logic-create-a-logic-app.md), možete vidjeti cijeli povijest njegova izvođenja na portalu za Azure.  Servise kao što su Dijagnostika Azure i Azure upozorenja možete postaviti praćenje događaja u stvarnom vremenu, a upozorenja za događaje volite "kada više od 5 pokreće uspjeti unutar čas."

## <a name="monitor-in-the-azure-portal"></a>Monitor na portalu za Azure

Da biste prikazali povijest, odaberite **Pregledaj**pa odaberite **Logike aplikacije**. Prikazuje se popis svih aplikacija logike u svoju pretplatu.  Odaberite aplikaciju logike želite nadzirati.  Vidjet ćete popis svih akcije i okidača koji su se pojavili za ovu aplikaciju logike.

![Pregled](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Postoji nekoliko sekcije na ovom plohu korisne su:

- **Sažetak** navodi **sve će se pokrenuti** i **Povijest okidača**
    - Popis **svih pokreće** najnoviju aplikaciju logike izvodi.  Kliknite bilo koji redak detalja pri pokretanju ili kliknite pločicu na popisu više se pokreće.
    - **Povijest okidača** navodi sve aktivnosti okidača za ovu aplikaciju logike.  Pokretanje aktivnosti ne može biti "Preskočene" Provjeri nove podatke (primjerice pogled da biste vidjeli nove datoteke dodan na FTP), "Uspjelo" što znači da je vratio podataka pokreću logike aplikacije ili "Nije uspjela" Pogreška u konfiguraciji koje odgovaraju.
- **Dijagnostika** omogućuje prikaz runtime pojedinosti i događaji i pretplata na [Upozorenja za Azure](#adding-azure-alerts)

>[AZURE.NOTE] Sve pojedinosti izvođenja i događaje šifrirane na ostale unutar servisa logike aplikacije. Samo dešifrirati nakon zahtjev za prikaz za korisnika. Pristup Ti događaji i moguće prilagoditi tako da je Azure Role-Based pristup kontrola (RBAC).

### <a name="view-the-run-details"></a>Prikaz detalja o za pokretanje

Ovaj popis se pokreće prikazuje **Status**, **Vrijeme početka**i **trajanje** na posebice izvođenja. Odaberite bilo koji redak da biste vidjeli detalje na koji se izvode.

Nadzor prikaz prikazuje svakog koraka Pokreni, ulaza i izlaze i poruke o pogrešci koja bi mogla imati occurre.

![Pokreni i akcije](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Ako vam je potrebna sve dodatne pojedinosti poput izvođenja **ID korelacije** (koji se mogu koristiti za REST API-JA), možete kliknuti gumb **Pokreni pojedinosti** .  To obuhvaća sve korake, status i ulaza/izlaza za izvođenje.

## <a name="azure-diagnostics-and-alerts"></a>Azure Dijagnostika i upozorenja

Osim pojedinosti koje ste dobili od Azure Portal i REST API-JA iznad, možete konfigurirati logike aplikacije za korištenje Azure Dijagnostika obogaćenog pojedinosti i ispravljanje pogrešaka.

1. Kliknite **Dijagnostika** dio aplikacije plohu logike
1. Kliknite da biste konfigurirali **Dijagnostičkih postavki**
1. Konfiguriranje za događaj koncentrator ili račun za pohranu šalji podatke

    ![Azure postavki dijagnostike](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Dodavanje Azure upozorenja

Kada niste konfigurirali Dijagnostika, možete dodati Azure upozorenja kada se određeni prag ponovnog.  U plohu **Dijagnostika** odaberite pločicu **upozorenja** i **Dodavanje upozorenja**.  To će vas voditi kroz Konfiguriranje upozorenja temeljena na broju pragovi i metrike.

![Azure upozorenja mjerenja](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

Po želji možete konfigurirati **uvjet**, **prag**i **razdoblje** .  Na kraju, konfiguriranje adresu e-pošte za slanje obavijesti za ili konfigurirati na webhook.  [Zahtjev za pokretanje](../connectors/connectors-native-reqres.md) možete koristiti u aplikaciji logike pokrenuti upozorenje kao i (obavljati zadatke kao što je [objavljivati Prazan hod](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [Slanje teksta](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)ili [dodajte poruku u red](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Postavki dijagnostike Azure

Svaki od ovih događaja sadrži detalje o aplikaciji logiku i događaja kao što je status.  Evo primjera *ActionCompleted* događaja:

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

Dva svojstva koja su posebno korisno za praćenje i nadzor su *clientTrackingId* i *trackedProperties*.  

#### <a name="client-tracking-id"></a>ID klijenta praćenja

Klijent ID praćenja je vrijednost vezane će događaje preko logike aplikaciju za pokretanje, uključujući ugniježđene tijekovima rada pod nazivom u sklopu logike aplikacije.  Taj ID će biti automatski generirani ako nisu navedeni, ali ne možete ručno navesti klijent praćenje ID od okidač prosljeđivanjem na `x-ms-client-tracking-id` zaglavlja s vrijednost ID-a u zahtjevu za pokretanje (zahtjev za pokretanje, pokretanje HTTP ili webhook okidača).

#### <a name="tracked-properties"></a>Evidentirana svojstva

Premjestite akcije u definiciju tijeka rada za praćenje unosa ili izlaze Dijagnostika podataka moguće je dodati evidentirane svojstva.  To može biti korisno ako želite pratiti podataka kao što je "ID narudžbe" u vašem telemetrijskih.  Da biste dodali evidentirane svojstvo, uvrstite u `trackedProperties` svojstvo na neku akciju.  Evidentirane svojstva mogu samo praćenje jedne akcije unosa i izlaze, ali možete koristiti u `correlation` svojstva događaja koji se povezivanje preko akcije u niz.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Proširivanje vašeg rješenja

Omogućuje korištenje telemetrijskih iz koncentratora za događaj ili prostora za pohranu u druge servise kao što su [Postupci upravljanja paket](https://www.microsoft.com/cloud-platform/operations-management-suite), [Azure strujanje analize](https://azure.microsoft.com/services/stream-analytics/)i [Power BI](https://powerbi.com) da bi stvarnom vremenu nadgledanje tijekova rada integracije.

## <a name="next-steps"></a>Daljnji koraci
- [Uobičajene primjere i scenariji za logike aplikacije](app-service-logic-examples-and-scenarios.md)
- [Stvaranje predloška za implementaciju aplikacije logike](app-service-logic-create-deploy-template.md)
- [Značajki integracije Enterprise](app-service-logic-enterprise-integration-overview.md)
