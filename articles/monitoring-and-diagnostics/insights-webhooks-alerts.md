<properties
    pageTitle="Konfiguriranje webhooks na Azure metričkim upozorenja | Microsoft Azure"
    description="Preusmjeri Azure upozorenja da biste drugim sustavima koji nisu Azure."
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
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfiguriranje programa webhook na Azure metričkim upozorenja

Webhooks omogućuju da biste usmjerili Azure upozorenje na drugi sustavi za naknadno obradi ili prilagođene akcije. Možete koristiti na webhook na upozorenja da biste usmjerili na servise koje slanje SMS, prijavu programskih pogrešaka, obavijestiti tim putem servisa razgovora i poruka ili bilo koji broj druge akcije. U ovom se članku opisuje kako postaviti na webhook Azure metričkim upozorenja i izgleda opseg za HTTP POST da biste na webhook. Informacije o postavljanje i sheme za zapisnik aktivnosti Azure upozorenje (upozorenje o događajima), [umjesto toga pogledajte ovu stranicu](./insights-auditlog-to-webhook-email.md).

Azure upozorenja HTTP POST upozorenja sadržaj JSON oblikovanje, sheme definiraju ispod, tako da je webhook URI koje ste omogućili prilikom stvaranja upozorenja. U ovom URI mora biti valjane HTTP ili HTTPS krajnje. Azure objava jednim zapisom po zahtjev kad je aktiviran upozorenja.

## <a name="configuring-webhooks-via-the-portal"></a>Konfiguriranje webhooks putem portala

Možete dodati ili ažurirati webhook URI na zaslonu za stvaranje/ažuriranje upozorenja na [portal](https://portal.azure.com/).

![Dodajte pravilo upozorenja](./media/insights-webhooks-alerts/Alertwebhook.png)

Možete konfigurirati i upozorenja da biste objavili webhook URI pomoću [Cmdleta ljuske PowerShell Azure](./insights-powershell-samples.md#create-alert-rules), [Više platformi EŽA](./insights-cli-samples.md#work-with-alerts)ili [Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Provjera autentičnosti na webhook

Na webhook možete provjeriti autentičnost pomoću neku od ovih metoda:

1. **Autorizacija utemeljen na token** - webhook URI sprema tokena ID-om, npr. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Osnovni autorizacije** - webhook URI sprema se pomoću korisničkog imena i lozinke, npr. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Shema tereta

Operacija objavu sadrži sljedeće JSON tereta i sheme za sva upozorenja temeljenih na metriku.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Polje | Obavezna | Fiksni skup vrijednosti | Bilješke |
| :-------------| :-------------   | :-------------   | :-------------   |
|status|Y|"Aktivirana", "Riješiti"|Stanje upozorenja temeljiti uvjeta koje ste postavili.|
|Kontekst| Y | | Upozorenja kontekst.|
|Vremenska oznaka| Y | | Vrijeme u kojem se pokreće upozorenje.|
|ID-a | Y | | Svaki upozorenja pravilo ima jedinstveni ID-a.|
|ime               |Y                  |                   | Naziv upozorenja.|
|Opis        |Y                  |                           |Opis upozorenja.|
|conditionType      |Y                  |"Metričkim", "Događaj"          |Podržane su dvije vrste upozorenja. Temelji na metrike uvjet, a drugi na temelju događaja u zapisnik aktivnosti. Koristite ovu vrijednost za provjeru ako upozorenje temelji se na metrički ili događaj.|
|uvjet          |Y                  |                           | Na temelju određenih polja da biste provjerili na conditionType.|
|metricName         |za primanje obavijesti o mjerenja  |                           |Naziv metriku koja definira što nadzire pravilo.|
|metricUnit         |za primanje obavijesti o mjerenja  |"Bajtova", "BytesPerSecond", "Broj", "CountPerSecond", "Marža", "Sekundi"|     Jedinica dopuštena za mjerenje. [Dopuštene vrijednosti su navedene ovdje](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |za primanje obavijesti o mjerenja  |                           |Stvarna vrijednost metriku koje su uzrokovale upozorenje.|
|Prag          |za primanje obavijesti o mjerenja  |                           |Vrijednost praga aktivira upozorenje.|
|windowSize         |za primanje obavijesti o mjerenja  |                           |Vremensko razdoblje koja se koristi za praćenje upozorenja aktivnosti koje se temelje na praga. Mora biti između pet minuta i 1 dana. ISO 8601 trajanje oblik.|
|timeAggregation    |za primanje obavijesti o mjerenja  |"Average," "Prezime", "Maksimalna", "Najmanja", "Nema", "Ukupna se" | Kako podatke koji se prikupljaju mora se kombinirati s vremenom. Zadana vrijednost je prosjek. [Dopuštene vrijednosti su navedene ovdje](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|operator           |za primanje obavijesti o mjerenja  |                           |Operator koji se koriste za usporedbu trenutni metričkim podataka praga skupa.|
|subscriptionId     |Y                  |                           |Azure pretplate ID-a.|
|resourceGroupName  |Y                  |                           |Naziv grupe resursa za impacted resurs.|
|resourceName       |Y                  |                           |Naziv resursa impacted resursa.|
|resourceType       |Y                  |                           |Vrsta resursa impacted resursa.|
|resourceId         |Y                  |                           |ID resursa impacted resursa.|
|resourceRegion     |Y                  |                           |Regija ili lokaciju impacted resursa.|
|portalLink         |Y                  |                           |Izravnu vezu na stranicu sažetak portala resursa.|
|Svojstva         |N                  |Neobavezno                   |Skup `<Key, Value>` parove (odnosno `Dictionary<String, String>`) koji sadrži detalje o događaju. Svojstva polja nije obavezno. U prilagođeni korisničkog Sučelja ili logike aplikacija temelji tijek rada, korisnici mogu unijeti ključ/vrijednosti koje se mogu proslijediti putem opseg. Drugi način za prosljeđivanje prilagođena svojstva natrag na webhook je putem uri webhook sam (kao što je parametara upita)|


>[AZURE.NOTE] Svojstva polja moguće je postaviti samo pomoću [Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o Azure upozorenja i webhooks u video [Integrirati upozorenja Azure s PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Izvršavanje Azure Automatizacija skripte (Runbooks) na Azure upozorenja](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Korištenje aplikacije logike za slanje do SMS poruka putem Twilio s Azure upozorenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Korištenje aplikacije logike za slanje zalihe poruke u Azure upozorenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Korištenje aplikacije logike za slanje poruka čekanja za Azure s Azure upozorenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
