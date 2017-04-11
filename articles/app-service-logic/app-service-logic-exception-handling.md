<properties
   pageTitle="Logika aplikacije iznimku rukovanje | Microsoft Azure"
   description="Naučite uzoraka pogreške i iznimke rukovanja s aplikacijama logike Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Logika aplikacije pogreške i rukovanja iznimke

Logika aplikacije nudi bogatog skupa alata i uzorcima da bi se osigurala vaše integracije su robusne i prebacuju protiv pogreške.  Jedan od izazove s bilo kojeg arhitektura Integracija je jamči da nedostupnost ili probleme zavisne sustavi rukuje se pravilno.  Logika aplikacije čini rukovanje pogreške prve klase doživljaj, što vam omogućuje alata koji su potrebni funkcioniralo na iznimke i pogrešaka u tijekove rada.

## <a name="retry-policies"></a>Ponovite pravila

Osnovni vrsta iznimke i pogreškama je pravilo pokušajte ponovno.  Ovo pravilo definira akciju treba ponovite ako zahtjev za početne isteklo ili nije uspjela (svaki zahtjev koja je rezultirala na 429 5xx odgovora ili).  Po zadanom sve akcije ponovno 4 još nekoliko puta putem intervalima 20 sekundi.  Stoga ako zahtjev za prvu primili na `500 Internal Server Error` odgovor, engine tijeka rada zaustavlja za 20 sekundi i pokušaj zahtjev ponovno.  Ako nakon nekoliko pokušaja za sve odgovor još iznimke ili pogreške, tijek rada će i dalje i označavanje stanje akcije kao `Failed`.

Možete konfigurirati pravila Ponovi **unosa** određenu akciju.  Pravila Ponovi moguće je konfigurirati da biste isprobali koliko god 4 puta veće od 1 sat vremenskim razmacima.  Sve pojedinosti unos svojstava mogu [pronaći na MSDN-][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Ako ste željeli radnju HTTP ponovite 4 vremena i pričekajte 10 minuta između svakog pokušaj promijenile definiciju sljedeće:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Dodatne informacije o podržanim sintaksa u prikazu [Pokušaj pravila sekcije na MSDN-][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>Svojstvo RunAfter da biste privukli neuspjeha

Svaku akciju aplikacije logike deklarira akcije koje je potrebno dovršiti prije nego što će se pokrenuti akciju.  Možete smatrati ovog redoslijed koraka u tijeku rada.  U ovom redoslijed naziva u `runAfter` svojstvo u definiciji akcija.  Je objekt koji opisuje koji su akcije i akcija statusi želite izvršiti akciju.  Po zadanom se sve akcije dodati pomoću dizajnera postavljene na `runAfter` u prethodnom koraku ako je u prethodnom koraku `Succeeded`.  Međutim, možete prilagoditi tu vrijednost tako da pokreću akcije nakon prethodne radnje `Failed`, `Skipped`, ili mogući skup te vrijednosti.  Ako ste željeli da biste dodali stavku određenu temu Bus servisa nakon određenu akciju `Insert_Row` ne uspije, koristite sljedeće `runAfter` konfiguracije:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Obavijest na `runAfter` svojstvo je postavljeno na fire ako na `Insert_Row` je akcija `Failed`.  Da biste pokrenuli akciju, ako je status akcija `Succeeded`, `Failed`, ili `Skipped` vidjet ćete da sintaksa bio bi:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Radnje koje pokrenuti nakon prethodne akcija nije uspjela, a uspjela, označit će se kao `Succeeded`.  Takvo ponašanje znači ako ste uspješno zamka sve pogreške u tijeku rada Pokreni sam označen kao `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Opsega i rezultata da biste procijenili akcije

Sličan način možete pokrenuti nakon pojedinačne Akcije, možete vidjeti i akcije grupu zajedno unutar [opseg](app-service-logic-loops-and-scopes.md) - koji služe kao logička grupiranja od akcija.  Dosezi su korisne za organiziranje akcija logike aplikacije, i za izvođenje aggregate procjene statusa opseg.  Opseg sam primit će status nakon što ste dovršili sve akcije unutar opseg.  Status opseg određuje s istim kriterijima kao Pokreni – ako je konačni akcija u programa grani izvođenja `Failed` ili `Aborted` je status `Failed`.

Možete `runAfter` označio opseg `Failed` početka određene akcije za sve pogreške koja se pojavila se unutar raspona.  Pokretanje nakon prekida se opseg omogućuje stvaranje jednu akciju da biste privukli pogreške ako *radnje unutar opsega* neće uspjeti.

### <a name="getting-the-context-of-failures-with-results"></a>Početak kontekstu neuspjeha s rezultatima

Toga neuspjeha iz opseg vrlo je praktična, no preporučujemo vam i kontekst za razumijevanje točno akcija koje nije uspjela, te pogreške ili šifre statusa vraćenih.  Na `@result()` funkcija tijek rada omogućuje kontekst u rezultat sve akcije unutar opseg.

`@result()`traje jedan parametar, naziv dosega, a vraća polje rezultata sve akcije iz unutar taj doseg.  Ove akcije objekti obuhvaćaju atribute isti kao u `@actions()` proizvodi objekt, uključujući akcija vrijeme početka, vrijeme završetka akciju, stanje akcije, akcije unosa, akcija korelacije ID-a i akcija.  Možete jednostavno uparivanje programa `@result()` funkcionirati na `runAfter` da biste poslali kontekstu radnje koje nije uspjela unutar opseg.

Ako želite izvršiti akciju *za svaku* akciju u doseg koji `Failed`, možete uparivanje `@result()` s **[Polja filtra](../connectors/connectors-native-query.md)** akcije i petlje **[ForEach](app-service-logic-loops-and-scopes.md)** .  Time da biste filtrirali raspon rezultata za akcije koje nije uspjelo.  Možete koristiti filtrirani rezultat polja, a izvođenje akcije za svaku pogrešku pomoću **ForEach** petlje.  Evo jednog primjera slijede, nakon čega slijedi Detaljno objašnjenje.  U ovom se primjeru će poslati zahtjev HTTP POST s tijelo odgovor radnje koje nije uspjela unutar opsega `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Ovo su detaljno objašnjenje te što se događa:

1. Akcija **Filtra polja** da biste filtrirali na `@result('My_Scope')` da bi se rezultat sve akcije unutar`My_Scope`
1. Uvjet **Filtra polje** može biti bilo koja `@result()` stavke čiji je status jednako `Failed`.  To će filtrirati polje rezultata sve akcije iz `My_Scope` da biste samo polje rezultata nije uspjelo akcija.
1. Izvođenje akcije za **za svaku** na izlaze **Filtrirane polja** .  To će pokrenuti i akcija *za svaki* nije uspjela akcija rezultat ćemo filtrirane iznad.
    - Ako je jedan akciju u doseg koji nije uspjela akcije u na `foreach` želite pokrenuti samo jedanput.  Mnoge nije uspjelo akcije može prouzročiti jednu akciju po nije uspjelo.
1. Slanje HTTP POST na na `foreach` stavke tijelo odgovora ili `@item()['outputs']['body']`.  Na `@result()` oblik stavke nije isti kao u `@actions()` oblika, a mogu raščlaniti na isti način.
1. Uključeni dva prilagođena zaglavlja s nazivom nije uspjelo akcija `@item()['name']` i nije uspio pokretanje klijenta ID praćenja `@item()['clientTrackingId']`.

Za referencu, Evo primjera jedan `@result()` stavke.  Možete vidjeti na `name`, `body`, a `clientTrackingId` svojstva raščlaniti u primjeru iznad.  Mora biti navedeno izvan od onog na `foreach`, `@result()` vraća polja tih objekata.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Gornje izraze možete koristiti za izvođenje obrade uzoraka različite iznimki.  Možete izvršiti obrade akcija izlaze prihvaća cijelu filtrirani raspon pogreške "i" Ukloni jedan iznimki na `foreach`.  Možete uključiti i druge korisne svojstva na `@result()` odgovor gore navedenoj sintaksi.

## <a name="azure-diagnostics-and-telemetry"></a>Azure Dijagnostika i telemetrijskih

Uzorci iznad su odličan način za obradu pogreške i iznimke unutar niz, ali možete prepoznati i odgovaranje na pogreške neovisno o Pokreni sam.  [Dijagnostika Azure](app-service-logic-monitor-your-logic-apps.md) omogućuje jednostavan način da biste poslali sve događaje tijeka rada (uključujući sve Pokreni i akcija statusi) račun za Azure prostora za pohranu ili programa Azure koncentrator za događaj.  Praćenje zapisnika i metrike ili ih objaviti u praćenju alat za radije, da biste procijenili izvođenja statusi.  Jedan potencijalne je strujanjem Svi događaji putem koncentratora Azure događaja u [Strujanje analize](https://azure.microsoft.com/services/stream-analytics/).  U strujanje analize možete napisati uživo upiti s bilo kojeg anomalies, prosjeke ili neuspješna iz dijagnostičke zapisnike.  Analitički strujanje možete jednostavno izlaz s drugim izvorima podataka kao što su redova, teme, SQL, DocumentDB i Power BI.

## <a name="next-steps"></a>Daljnji koraci
- [Pogledajte kako jednog klijenta ugrađeni robusne pogreškom s aplikacijama logike](app-service-logic-scenario-error-and-exception-handling.md)
- [Pronađite dodatne primjere logike aplikacije i scenariji](app-service-logic-examples-and-scenarios.md)
- [Saznajte kako stvoriti automatiziranog implementacijama aplikacije logike](app-service-logic-create-deploy-template.md)
- [Dizajniranje i implementacija logike aplikacije Visual Studio](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9