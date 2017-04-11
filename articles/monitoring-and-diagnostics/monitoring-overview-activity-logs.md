<properties
    pageTitle="Pregled zapisnika Azure aktivnosti | Microsoft Azure"
    description="Saznajte što je zapisnik aktivnosti Azure te kako ga možete koristiti da biste shvatili događaja unutar Azure pretplatu."
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
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Pregled zapisnika Azure aktivnosti
**Zapisnik aktivnosti Azure** je zapisnik koji daje uvid u operacije koje su izvršene na resursa u svoju pretplatu. Zapisnik aktivnosti je pod starim nazivom "Zapisnike nadzora" ili "Radu zapisnike", jer izvješća kontrola ravnini događaje za pretplate. Korištenje zapisnik aktivnosti, možete odrediti na "što, tko i kada ' za neki pisanje operacija (broj, objavu, DELETE) snimljene resursa u svoju pretplatu. Mogu razumjeti i status postupak i drugih relevantnih svojstava. Zapisnik aktivnosti obuhvaćaju operacija čitanja (DOHVATI).

Zapisnik aktivnosti razlikuje se od [Dijagnostičke zapisnike](./monitoring-overview-of-diagnostic-logs.md), koji su sve zapisnike čuje po resurs. Ti zapisnici daju podatke o na operacija resursa, a ne na resursa.

Možete dohvatiti događaje iz vaše zapisnik aktivnosti pomoću portala Azure, EŽA, PowerShell cmdleti i Azure Monitor REST API-JA.

Pogledajte ovaj [videozapis Uvod u zapisnik aktivnosti](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Što sve možete raditi s zapisnik aktivnosti
Evo nekoliko stvari koje možete raditi s zapisnik aktivnosti:

- Slanje upita i prikaz **Azure portal**.
- Upit putem REST API-JA, Cmdlet ljuske PowerShell ili EŽA.
- [Stvaranje upozorenja e-pošte ili webhook koja pokreće isključivanje događaja zapisnik aktivnosti.](./insights-auditlog-to-webhook-email.md)
- [Spremite je na **Račun za pohranu** za arhiviranje ili ručno provjere](./monitoring-archive-activity-log.md). Možete postaviti vrijeme zadržavanja (u danima) pomoću **Zapisnika profila**.
- Analiza u PowerBI pomoću [**PowerBI sadržaja paketa**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/).
- [Strujanje je **Događaj koncentrator** ](./monitoring-stream-activity-logs-event-hubs.md) za ingestion servisa drugih proizvođača ili prilagođeni analize rješenja kao što su PowerBI.

## <a name="export-the-activity-log-with-log-profiles"></a>Izvoz zapisnik aktivnosti s profilima zapisnika
**Zapisnik profila** određuje kako se vaše zapisnik aktivnosti izvozi. Korištenje zapisnika profila, možete konfigurirati:

- Gdje treba poslati zapisnik aktivnosti (račun za pohranu ili događaj koncentratora)
- Moraju se poslati događaj kategorije (akcija za pisanje, a zatim Izbriši)
- Trebali biste moguće izvesti regije (mjesta)
- Koliko dugo zapisnik aktivnosti moraju biti zadržani na računu za pohranu – zadržavanja nula dana znači zapisnika čuvaju zauvijek. U suprotnom vrijednost može biti bilo koji broj dana između 1 i 2147483647. Ako se postavljaju pravilnika o zadržavanju, ali spremanje zapisnika u račun za pohranu onemogućen (na primjer, ako samo su odabrane mogućnosti koncentratora događaj ili OMS), pravila zadržavanja imati utjecaja.

Te postavke može konfigurirati putem mogućnosti "Izvoz" u zapisnik aktivnosti plohu na portalu. Također može biti konfigurirano programski [pomoću Azure Monitor REST API -JA](https://msdn.microsoft.com/library/azure/dn931927.aspx), cmdleta ljuske PowerShell ili EŽA. Pretplatu možete imati samo jedan profil zapisnika.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Konfiguriranje zapisnika profila pomoću portala za Azure
Možete strujanje zapisnik aktivnosti programa koncentrator događaj ili ih spremiti na računu za pohranu pomoću mogućnosti "Izvoz" na portalu za Azure.

1. Dođite do plohu **Zapisnik aktivnosti** pomoću izbornika na lijevoj strani portalu.

    ![Pomaknite se do zapisnik aktivnosti u portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kliknite gumb **Izvezi** pri vrhu na plohu.

    ![Gumb za izvoz u portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. U plohu koji će se prikazati, možete odabrati:  
    - područja za koju želite izvesti događaja
    - Račun za pohranu u koju želite spremiti događaja
    - broj dana koje želite zadržati te događaja u prostor za pohranu. Postavka od 30 dana zauvijek zadržava zapisnike.
    - Servis Bus prostor naziva u kojem želite na događaj koncentrator će biti stvoren za strujanje te događaja.

    ![Izvoz plohu zapisnik aktivnosti](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Kliknite **Spremi** da biste spremili postavke. Postavke odmah se primjenjuju se na vašu pretplatu.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Konfiguriranje zapisnika profila pomoću cmdleta ljuske PowerShell za Azure
#### <a name="get-existing-log-profile"></a>Početak postojeći profil zapisnika
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Dodavanje zapisnika profila
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Svojstvo         | Obavezno | Opis   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ime             | Da      | Naziv profila zapisnika.                                 |
| StorageAccountId | ne       | ID resursa za pohranu računa na koju želite spremiti zapisnik aktivnosti.                         |
| serviceBusRuleId | ne       | Servis Bus pravilo ID za prostor za naziv servisa Bus moraju događaja koncentratora stvorene u kojima želite. Je niz u ovom obliku: `{service bus resource ID}/authorizationrules/{key name}`. |
| Mjesta        | Da      | Popis odvojenih zarezom područja za koju želite prikupiti aktivnosti zapisnika događaja.              |
| RetentionInDays  | Da      | Broj dana koji događaji moraju biti zadržani, između 1 i 2147483647. Vrijednost nula pohranjuje zapisnike beskonačno (zauvijek). |
| Kategorije       | ne       | Odvojenih zarezom popis kategorija događaja koje treba prikupiti. Moguće vrijednosti su pisanje, brisanje i akcije.                                 |

#### <a name="remove-a-log-profile"></a>Uklanjanje profila zapisnika
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Konfiguriranje zapisnika profila koji se koriste EŽA različite platforme Azure
#### <a name="get-existing-log-profile"></a>Početak postojeći profil zapisnika
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Na `name` svojstvo mora biti naziv profila zapisnika.

#### <a name="add-a-log-profile"></a>Dodavanje zapisnika profila
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Svojstvo         | Obavezno | Opis   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ime             | Da      | Naziv profila zapisnika.                                 |
| storageId        | ne       | ID resursa za pohranu računa na koju želite spremiti zapisnik aktivnosti.                         |
| serviceBusRuleId | ne       | Servis Bus pravilo ID za prostor za naziv servisa Bus moraju događaja koncentratora stvorene u kojima želite. Je niz u ovom obliku: `{service bus resource ID}/authorizationrules/{key name}`. |
| mjesta        | Da      | Popis odvojenih zarezom područja za koju želite prikupiti aktivnosti zapisnika događaja.              |
| retentionInDays  | Da      | Broj dana koji događaji moraju biti zadržani, između 1 i 2147483647. Vrijednost nula pohranjuje zapisnike beskonačno (zauvijek).     |
| kategorije       | ne       | Odvojenih zarezom popis kategorija događaja koje treba prikupiti. Moguće vrijednosti su pisanje, brisanje i akcije.                                 |

#### <a name="remove-a-log-profile"></a>Uklanjanje profila zapisnika
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Sheme događaja
Svaki događaj u zapisnik aktivnosti sadrži blob JSON slično kao u ovom primjeru:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Naziv elementa         | Opis             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| autorizacija        | Blob RBAC svojstva događaja. Obično obuhvaća svojstva "Akcije", "uloga" i "opseg".|
| pozivatelja               | Adresa e-pošte korisnika koji je izvršila operacije, UPN zahtjeva i SPN zahtjeva koji se temelji na dostupnost.|
| kanala             | Jedna od sljedećih vrijednosti: "Administrator", "Operacija"|
| correlationId        | Obično GUID u obliku niza. Događaji koje zajednički koristite s correlationId pripada istu uber akciju.   |
| Opis          | Statički tekst opis događaja.                              |
| eventDataId          | Jedinstveni identifikator događaja.    |
| eventSource          | Naziv servisa Azure ili infrastrukture koji je generirao ovaj događaj.    |
| httpRequest          | Blob s opisom Http zahtjev. Obično obuhvaća "clientRequestId", "clientIpAddress" i "metode" (HTTP način. For example, STAVITE).                            |
| Razina                | Razina događaja. Jedna od sljedećih vrijednosti: "Ključnih", "Pogreška", "Upozorenje", "Informational" i "Tekstni"  |
| resourceGroupName    | Naziv grupe resursa za impacted resurs.               |
| resourceProviderName | Naziv davatelja resursa za impacted resursa             |
| resourceUri          | Id resursa impacted resursa.                               |
| operationId          | GUID razdijeliti događaje koje odgovaraju jedne operacije.         |
| operationName        | Naziv operacije.  |
| Svojstva           | Skup `<Key, Value>` parove (to jest, rječnika) s opisom detalje događaja.                                |
| status               | Niz koji opisuje status operacije. Neke uobičajene vrijednosti: rada u tijeku je uspio, nije uspjelo, Aktivno, Riješeno.                                |
| subStatus            | Obično HTTP kod stanja odgovarajuće REST poziva, ali možete uključiti i ostalih nizova s opisom substatus, kao što su te uobičajenih vrijednosti: u redu (HTTP kod stanja: 200), stvorene (HTTP kod stanja: 201), prihvaćenom (HTTP kod stanja: 202), ne sadržaj (HTTP kod stanja: 204), Neispravan zahtjev (HTTP kod stanja: 400), nije pronađen (HTTP kod stanja: 404), sukoba (HTTP kod stanja : 409), Interna pogreška poslužitelja (Šifra stanja HTTP: 500), servis nije dostupan (Šifra stanja HTTP: 503), Prekoračenje vremena za pristupnik (Šifra stanja HTTP: 504). |
| eventTimestamp       | Vremenska oznaka kada događaj sastavio servisa Azure obrade zahtjeva odgovara događaj.     |
| submissionTimestamp  | Vremenska oznaka kada događaj postala dostupna za slanje upita.             |
| subscriptionId       | Azure pretplate ID-a.  |
| nextLink             | Token nastavka sljedeći skup rezultata za dohvaćanje kada oni su podijeljene prema gore u više odgovora. Obično je potrebna kada postoji više od 200 zapisa.     |

## <a name="next-steps"></a>Daljnji koraci
- [Dodatne informacije o zapisnik aktivnosti (bivši zapisnike nadzora)](../resource-group-audit.md)
- [Strujanje zapisnik aktivnosti Azure s koncentratorima događaja](./monitoring-stream-activity-logs-event-hubs.md)
