<properties
   pageTitle="Prijava analitiku upozorenja REST API-JA"
   description="Prijava analitiku upozorenja REST API-JA omogućuje stvaranje i upravljanje upozorenjima u paketu operacije upravljanja (OMS).  Ovaj članak sadrži pojedinosti u API-JA i nekoliko primjera za izvođenje različite operacije."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Prijava analitiku upozorenja REST API-JA

Prijava analitiku upozorenja REST API-JA omogućuje stvaranje i upravljanje upozorenjima u paketu operacije upravljanja (OMS).  Ovaj članak sadrži pojedinosti u API-JA i nekoliko primjera za izvođenje različite operacije.

Prijava analitiku pretraživanje REST API-JA je RESTful te im možete pristupiti putem Azure resursima REST API-JA. U ovom dokumentu pronaći ćete primjere gdje će se na API pristupiti putem naredbenog retka PowerShell pomoću [ARMClient](https://github.com/projectkudu/ARMClient), alat naredbenog retka za Otvori izvor koji pojednostavljuje pozivanje Azure resursima API-JA. Korištenje ARMClient i PowerShell jedan je od mnogo mogućnosti za pristup API za pretraživanje zapisnika analize. Pomoću alata za te mogu koristiti RESTful API Azure resursima za upućivanje poziva na radne prostore OMS i izvršavanje naredbi pretraživanja unutar njih. U API će izlaz rezultata pretraživanja na vas u obliku JSON, što omogućuje korištenje rezultata pretraživanja na razne načine programski.

## <a name="prerequisites"></a>Preduvjeti
Trenutno upozorenja samo mogu se kreirati pomoću spremljeno pretraživanje u zapisnik analize.  Može se odnositi na [Zapisnika pretraživanja REST API -JA](log-analytics-log-search-api.md) za dodatne informacije.

## <a name="schedules"></a>Rasporedi
Spremljeno pretraživanje može imati jednu ili više rasporeda. Raspored određuje koliko često je pretraživanje Izvedi i razdoblje tijekom kojih je označena kriterij.
Rasporedi imati svojstava u tablici u nastavku.

| Svojstvo  | Opis |
|:--|:--|
| Interval | Učestalost pokretanja pretraživanja. Mjeri u minutama. |
| QueryTimeSpan | Vremenski interval prema kojima se procjenjuje kriterij. Mora biti jednaka ili veća od intervala. Mjeri u minutama. |
| Verzija | API verzija koja se koristi.  Trenutno to moraju uvijek biti postavljeno na 1. |

Na primjer, razmotrite upit događaj s intervala od 15 minuta i vremenski raspon od 30 minuta. U ovom slučaju će se izvoditi upit svakih 15 minuta, a upozorenje koje bi se pokrenuti ako kriterij nastavlja se da se raščlanjuje true postavite pokazivač raspon 30 minuta.

### <a name="retrieving-schedules"></a>Dohvaćanje rasporeda
Da biste dohvatili sve rasporede za spremljeno pretraživanje, upotrijebite metodu Get.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Upotrijebite metodu Get raspored ID-om za dohvaćanje određeni raspored za spremljeno pretraživanje.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Slijedi odgovor uzorka za raspored.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Stvaranje rasporeda
Upotrijebite metodu stavi s ID-om jedinstveni raspored da biste stvorili novi raspored.  Imajte na umu dva rasporeda ne mogu imati isti ID čak i ako su povezani s drukčijim spremljena pretraživanja.  Kada stvorite raspored na konzoli OMS, GUID stvara se za zakazivanje ID.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Uređivanje rasporeda
Upotrijebite metodu stavi postojeći raspored ID za istu spremljeno pretraživanje da biste izmijenili taj raspored.  U tijelu zahtjeva za mora sadržavati e-oznake rasporeda.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Brisanje rasporeda
Upotrijebite metodu Izbriši raspored ID da biste izbrisali raspored.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Akcija
Raspored može imati više akcija. Akciju mogu definirati jednom ili više procesa za izvođenje kao što je slanje e-poštu ili počevši s runbook ili definirati prag koja određuje kada podudaraju s rezultatima pretraživanja neke kriterijima.  Neke akcije će definirati i tako da se procese koji se izvode kada se ispuni praga.

Sve akcije imaju svojstva u tablici u nastavku.  Različite vrste upozorenja imaju različite dodatna svojstva koji su opisan u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Vrsta | Vrsta akcije.  Moguće vrijednosti trenutno su upozorenja i Webhook. |
| Ime | Zaslonsko ime upozorenja. |
| Verzija | API verzija koja se koristi.  Trenutno to moraju uvijek biti postavljeno na 1. |

### <a name="retrieving-actions"></a>Dohvaćanje akcije
Upotrijebite metodu Get dohvatiti sve akcije za raspored.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Upotrijebite metodu Get ID akcija za dohvaćanje određenu akciju za raspored.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Stvaranje ili uređivanje akcije
Upotrijebite metodu stavi s Identifikacijskim akcije koje su jedinstvene za raspored da biste stvorili novu akciju.  Kada stvorite akcije na konzoli OMS, GUID je za akciju ID.

Upotrijebite metodu stavi postojeće ID akcija za isti spremljeno pretraživanje da biste izmijenili taj raspored.  U tijelu zahtjeva za mora sadržavati e-oznake rasporeda.

Oblikovanje zahtjev za stvaranje nove akcije razlikuje vrsta akcije da bi se navode u ovim se primjerima u sljedećim odjeljcima.

### <a name="deleting-actions"></a>Brisanje akcije
Upotrijebite metodu Izbriši ID akcije da biste izbrisali neku akciju.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Akcije za upozorenje
Raspored mora imati jednu i samo jednu akciju za upozorenje.  Akcije za upozorenje imati jednu ili više sekcija u tablici u nastavku.  Svaki je opisan u dodatno ispod.

| Sekcije | Opis |
|:--|:--|
| Prag | Kriteriji za kada se izvodi akciju. |  
| EmailNotification | Slanje pošte većem broju primatelja. |
| Olakšava | Pokretanje programa runbook u Azure Automatizacija da biste ispravili problem identificirani. |

#### <a name="thresholds"></a>Pragovi
Akcija za upozorenje mora imati jednu i samo jednu praga.  Kada se rezultati parametra spremljeno pretraživanje podudaraju praga u akcije vezane uz pretraživanje, pokrenite su drugi procesi u akciju.  Akciju mogu sadržavati samo prag tako da se može se koristiti s akcijama drugih vrsta koji ne sadrže pragovi.

Pragovi imaju svojstva u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Operator | Operator usporedbe praga. <br> gt = veće od <br> lt = manje od |
| Vrijednost | Vrijednost za praga. |

Na primjer, razmotrite upit događaj s intervala od 15 minuta, vremenski raspon od 30 minuta i praga veći od 10. U ovom slučaju će se izvoditi upit svakih 15 minuta, a upozorenje koje će se pokrenuti ako ga vratiti 10 događaje koje su stvorene putem raspon 30 minuta.

Slijedi odgovor uzorka za neku akciju s samo praga.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Upotrijebite metodu stavi s ID-om jedinstvene akcije da biste stvorili novu akciju praga za raspored.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Upotrijebite metodu stavi s postojećeg ID akcije da biste izmijenili prag akcija za raspored.  U tijelu zahtjeva za mora sadržavati e-oznake akcije.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>Obavijesti e-pošte
Obavijesti e-poštom poslati poštu jednog ili više primatelja.  Sadrže svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Primatelji | Popis adresa pošte. |
| Predmet | Predmet poruke. |
| Privitak | Privici trenutno nisu podržane, tako da to uvijek će imati vrijednost "Nema". |

Slijedi odgovor uzorka za akcija obavijesti e-pošte s praga.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Upotrijebite metodu stavi s ID-om jedinstvene akcije da biste stvorili novu akciju e-pošte za raspored.  Sljedeći primjer stvara obavijest e-poštom praga tako da e-pošte šalju prilikom rezultate spremljeno pretraživanje Premašili prag.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Upotrijebite metodu stavi s postojećeg ID akcije da biste izmijenili akciju e-pošte za raspored.  U tijelu zahtjeva za mora sadržavati e-oznake akcije.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Olakšava akcije
Remediations pokrenuti na runbook u automatizaciji Azure koja pokušava da biste ispravili problem otkrije upozorenje.  Morate stvoriti webhook za runbook koji se koriste u olakšava akcije i zatim navedite URI u svojstvu WebhookUri.  Kada stvorite ovu akciju korištenju konzole za OMS, novi webhook automatski se stvara za na runbook.

Remediations obuhvaćaju svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| RunbookName | Naziv na kompilacije. Time se mora podudarati objavljene runbook na računu za automatizaciju konfiguriran u rješenje Automatizacija u radnom prostoru OMS. |
| WebhookUri | URI na webhook.
| Isteka | Datum isteka i vrijeme na webhook.  Ako na webhook nema programa isteka, zatim to može biti bilo koji valjani datum u budućnosti. |

Slijedi odgovor uzorka za olakšava započinje praga.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Upotrijebite metodu stavi s ID-om jedinstvene akcije da biste stvorili novu akciju olakšava za raspored.  Sljedeći primjer stvara na olakšava praga tako da na runbook se pokreće kada rezultate spremljeno pretraživanje Premašili prag.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Upotrijebite metodu stavi s postojećeg ID akcije da biste izmijenili olakšava akcija za raspored.  U tijelu zahtjeva za mora sadržavati e-oznake akcije.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Primjer
Sljedeći je dovršena primjer da biste stvorili novo upozorenje e-pošte.  Time se stvara novi raspored uz akcije koje sadrže praga i e-pošte.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook akcije
Akcije Webhook pokretanje procesa tako da nazovete URL-a i po želji pružanje tereta slati.  Oni su slične olakšava akcije osim namjena za webhooks koje možda pozivanje procesa osim Automatizacija Azure runbooks.  Također pružaju dodatna mogućnost prikazivanja na teret za isporuku udaljene postupak.

Akcije Webhook imati praga, ali umjesto toga treba dodati raspored koji sadrži akciju upozorenja s praga.  Možete dodati više akcija Webhook koji će sve pokrenuti kada ispunjen praga.

Akcije Webhook uključuju svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| WebhookUri | Predmet poruke. |
| CustomPayload | Prilagođeni tereta slati na webhook.  Oblik će ovise o tome što se webhook očekuje. |

Slijedi odgovor uzorka za webhook akcije i akcija pridružene upozorenja s praga.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Stvaranje ili uređivanje webhook akcije
Upotrijebite metodu stavi s ID-om jedinstvene akcije da biste stvorili novu akciju webhook za raspored.  Sljedeći primjer stvara Webhook akcije i akcija za upozorenje s praga tako da na webhook će se aktiviraju kada rezultate spremljeno pretraživanje Premašili prag.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Upotrijebite metodu stavi s postojećeg ID akcije da biste izmijenili webhook akcija za raspored.  U tijelu zahtjeva za mora sadržavati e-oznake akcije.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Daljnji koraci

- Korištenje [REST API -JA za zapisnika pretraživanja](log-analytics-log-search-api.md) u zapisnik analize.
