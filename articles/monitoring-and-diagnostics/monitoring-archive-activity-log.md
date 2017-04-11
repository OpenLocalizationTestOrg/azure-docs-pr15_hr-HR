<properties
    pageTitle="Arhiviranje zapisnika Azure aktivnosti | Microsoft Azure"
    description="Saznajte kako arhivirati zapisnik aktivnosti Azure na Dugoročne zadržavanja na računu za pohranu."
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
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Arhiviranje zapisnika Azure aktivnosti
U ovom se članku Pokazat ćemo kako možete koristiti Azure portalu cmdleta ljuske PowerShell ili EŽA različite platforme za arhiviranje o [**Zapisnik aktivnosti Azure**](monitoring-overview-activity-logs.md) na računu za pohranu. Ta je mogućnost korisna ako želite zadržati aktivnosti zapisnika dulje od 90 dana (s potpunu kontrolu nad pravila zadržavanja) za nadzora, statične analizu ili sigurnosnu kopiju. Ako trebate samo zadržati svoje događaje 90 dana ili manje ne morate postaviti arhiviranje s računom za pohranu jer događaje zapisnika aktivnosti se zadržavaju u Azure platforme za 90 dana bez omogućivanja arhiviranje.

## <a name="prerequisites"></a>Preduvjeti
Prije nego počnete, morate [stvoriti račun za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) na koji možete arhivirati vaše zapisnik aktivnosti. Preporučujemo korištenje postojeće prostora za pohranu računa koji ima slovo druge, koji nisu nadzor podataka pohranjenih u ga tako da se bolje možete upravljati pristupom za praćenje podataka. Međutim, ako su arhiviranje i dijagnostičke zapisnike i metriku s računom za pohranu, je možda smisla da koristi taj račun za pohranu za vaše aktivnosti evidenciju da biste zadržali sve podatke o praćenju na središnjem mjestu. Računa za pohranu koji koristite mora biti račun za pohranu glavnu svrhu, ne s računom spremište blobova platforme.

## <a name="log-profile"></a>Zapisnik profila
Ako želite arhivirati zapisnik aktivnosti na bilo koji od metoda navedenih u nastavku, postavite **Zapisnika profila** za pretplatu. Profil zapisnika definira vrstu događaja koji su spremljeni ili strujanjem i na izlaze – koncentrator za pohranu računa i/ili događaj. Također definira pravila zadržavanja (broj dana koliko će se zadržavati) za događaje spremljeni na račun za pohranu. Ako pravila zadržavanja postavljen na nulu, događaji spremaju beskonačno. U suprotnom, to možete postaviti na bilo koju vrijednost između 1 i 2147483647. [Potražite dodatne informacije o zapisniku profili ovdje](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Zapisnik aktivnosti pomoću portala za arhiviranje
1. Na portalu kliknite vezu **Zapisnik aktivnosti** lijeve strane navigacije. Ako ne vidite vezu za zapisnik aktivnosti, najprije kliknite vezu **Više usluga** .

    ![Dođite do plohu zapisnik aktivnosti](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Pri vrhu na plohu, kliknite **Izvoz**.

    ![Kliknite gumb Izvezi](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. U plohu koji će se pojaviti, potvrdite okvir za **Izvoz s računom za pohranu** i odaberite račun za pohranu.

    ![Postavljanje računa za pohranu](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Pomoću klizača ili tekstni okvir, definirati nekoliko dana za koji će biti zadržane aktivnosti zapisnik događaja na vašem računu za pohranu. Ako biste radije da bi vaši podaci ista i na računu za pohranu beskonačno, postavite taj broj na nulu.
5. Kliknite **Spremi**.

## <a name="archive-the-activity-log-via-powershell"></a>Arhiviranje zapisnik aktivnosti PowerShell
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Svojstvo         | Obavezno | Opis                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | ne       | ID resursa za pohranu računa na koju želite spremiti zapisnika aktivnosti.                                                                                                                                                                                                                        |
| Mjesta        | Da      | Popis odvojenih zarezom područja za koju želite prikupiti aktivnosti zapisnika događaja. Možete prikazati popis svih područja [tako što ste posjetili ovu stranicu](https://azure.microsoft.com/en-us/regions) ili pomoću [Azure upravljanja REST API -JA](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| RetentionInDays  | Da      | Broj dana koji događaji moraju biti zadržani, između 1 i 2147483647. Vrijednost nula pohranjuje zapisnike beskonačno (zauvijek).                                                                                                                                                                                             |
| Kategorije       | Da      | Odvojenih zarezom popis kategorija događaja koje treba prikupiti. Moguće vrijednosti su pisanje, brisanje i akcije.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Arhiviranje zapisnik aktivnosti putem EŽA
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Svojstvo        | Obavezno | Opis                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ime            | Da      | Naziv profila zapisnika.                                                                                                                                                                                                                                                                         |
| storageId       | ne       | ID resursa za pohranu računa na koju želite spremiti zapisnika aktivnosti.                                                                                                                                                                                                                        |
| mjesta       | Da      | Popis odvojenih zarezom područja za koju želite prikupiti aktivnosti zapisnika događaja. Možete prikazati popis svih područja [tako što ste posjetili ovu stranicu](https://azure.microsoft.com/en-us/regions) ili pomoću [Azure upravljanja REST API -JA](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays | Da      | Broj dana koji događaji moraju biti zadržani, između 1 i 2147483647. Vrijednost nula će sadržavati zapisnike beskonačno (zauvijek).                                                                                                                                                                                             |
| kategorije      | Da      | Odvojenih zarezom popis kategorija događaja koje treba prikupiti. Moguće vrijednosti su pisanje, brisanje i akcije.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Prostor za pohranu sheme zapisnik aktivnosti
Kada ste postavili arhiviranje, spremnik za pohranu stvorit će se u račun za pohranu čim se pojavi događaj zapisnik aktivnosti. Blob polja unutar spremnik slijedite isti oblik preko zapisnik aktivnosti i dijagnostičke zapisnike. Struktura ove blob-Ova je:

> uvida upotrebljiva – zapisnici/naziv = = zadani/resourceId/PRETPLATE / {ID pretplate} / y = {četveroznamenkastom godinom numerički} / m = {dvoznamenkasti mjeseca} / d = {dvoznamenkasti numerički dan} / h hour}/m=00/PT1H.json dvoznamenkasti 24-satni prikaz {=

Na primjer, naziv blob možda je:

> insights-Operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Svaki PT1H.json blob sadrži blob JSON događaja koji se nastale tijekom sat navedene u blob URL (npr. h = 12). Tijekom prikaz sata, događaji dodaju datoteke PT1H.json kako nastaju. Vrijednost minute (m = 00) uvijek 00, jer događaje zapisnika aktivnosti podijeljene su u pojedinačne blob-ova po satu.

U datoteci PT1H.json svaki događaj je pohranjena u polju "zapisa", slijedite ovaj oblik:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
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
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Naziv elementa    | Opis                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| vrijeme            | Vremenska oznaka kada događaj sastavio servisa Azure obrade zahtjeva odgovara događaj.    |
| resourceId      | ID resursa impacted resursa.                                                                          |
| operationName   | Naziv operacije.                                                                                         |
| Kategorija        | Kategorija Akcije, npr. Pisanje, čitanje, akcije.                                                               |
| resultType      | Vrste rezultata, npr. Početak uspješno, pogreška,                                                            |
| resultSignature | Ovisi o vrsti resursa.                                                                                  |
| durationMs      | Trajanje operacije u milisekundama                                                                      |
| callerIpAddress | IP adresa korisnika koji je izvršila operacije, UPN zahtjeva i SPN zahtjeva koji se temelji na dostupnost.         |
| correlationId   | Obično GUID u obliku niza. Događaji koje zajednički koristite s correlationId pripada istu uber akciju.         |
| identitet        | Blob JSON s opisom autorizacije i zahtjevima.                                                             |
| autorizacija   | Blob RBAC svojstva događaja. Obično obuhvaćaju svojstva "Akcije", "uloga" i "opseg".            |
| Razina           | Razina događaja. Jedna od sljedećih vrijednosti: "Ključnih", "Pogreška", "Upozorenje", "Informational" i "Tekstni" |
| mjesto        | Područje u kojem mjesto došlo je do (ili globalne).                                                             |
| Svojstva      | Skup `<Key, Value>` parove (odnosno rječnika) s opisom detalje događaja.                             |

> [AZURE.NOTE] Svojstva i korištenje tih svojstava možete ovisi o resurs.

## <a name="next-steps"></a>Daljnji koraci
- [Preuzimanje blob-ova za analizu](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Strujanje zapisnik aktivnosti s koncentratorima događaja](monitoring-stream-activity-logs-event-hubs.md)
- [Saznajte više o zapisnik aktivnosti](monitoring-overview-activity-logs.md)
