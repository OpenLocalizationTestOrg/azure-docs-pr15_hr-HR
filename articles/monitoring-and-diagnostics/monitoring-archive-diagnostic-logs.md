<properties
    pageTitle="Arhiviranje Azure dijagnostičke zapisnike | Microsoft Azure"
    description="Saznajte kako arhivirati vaše Azure dijagnostičke zapisnike za Dugoročne zadržavanja na računu za pohranu."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Arhiviranje Azure dijagnostičkih zapisnika
U ovom se članku Pokazat ćemo kako možete koristiti Azure portalu cmdleta ljuske PowerShell, EŽA ili REST API-JA za arhiviranje vaše [Azure dijagnostičke zapisnike](monitoring-overview-of-diagnostic-logs.md) u račun za pohranu. Ta je mogućnost korisna ako želite zadržati svoje dijagnostičke zapisnike s pravilo neobavezno zadržavanja za nadzora, statične analizu ili sigurnosnu kopiju.

## <a name="prerequisites"></a>Preduvjeti
Prije nego počnete, morate [stvoriti račun za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) na koji možete arhivirati dijagnostičke zapisnike. Preporučujemo korištenje postojeće prostora za pohranu računa koji ima slovo druge, koji nisu nadzor podataka pohranjenih u ga tako da se bolje možete upravljati pristupom za praćenje podataka. Međutim, ako su arhiviranje i zapisnik aktivnosti i dijagnostičkih mjernih podataka na račun za pohranu, je možda smisla da koristi taj račun za pohranu za dijagnostičke zapisnike kao i želite zadržati sve podatke o praćenju na središnjem mjestu. Računa za pohranu koji koristite mora biti račun za pohranu glavnu svrhu, ne s računom spremište blobova platforme.

## <a name="diagnostic-settings"></a>Dijagnostičkih postavki
Ako želite arhivirati vaše dijagnostičke zapisnike na bilo koji od metoda navedenih u nastavku, postavite **Dijagnostičkih postavku** za određeni resurs. Dijagnostičke postavka za resurs definira kategorije zapisnike tu su koji su spremljeni ili strujanjem i na izlaze – koncentrator za pohranu računa i/ili događaj. Također definira pravila zadržavanja (broj dana koliko će se zadržavati) za događaje svake kategorije zapisnika spremljeni na račun za pohranu. Ako pravilnika o zadržavanju postavljen na nulu, događaje zapisnika kategoriju spremaju beskonačno (koji se piše, zauvijek). Pravila zadržavanja u suprotnom može biti bilo koji broj dana između 1 i 2147483647. [Potražite dodatne informacije o ovdje dijagnostičkih postavki](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>Arhiviranje zapisnicima dijagnostičkih podataka pomoću portala

1. Na portalu, kliknite u plohu resursa za resursa na koji želite omogućiti arhivsku dijagnostičkih zapisnika.
2. U odjeljku **nadzor** resursa izbornika postavke odaberite **Dijagnostika**.

    ![Nadzor dijelu izbornika resursa](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Potvrdite okvir za **Izvoz u račun za pohranu**, a zatim odaberite račun za pohranu. Po želji određivanje broja dana da biste zadržali ti zapisnici pomoću na **zadržavanja (dana)** klizača. Zadržavanje nula dana beskonačno pohranjuje zapisnike.

    ![Dijagnostički zapisnici plohu](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Kliknite **Spremi**.

Dijagnostičke zapisnike arhiviraju za taj račun za pohranu čim se generira nove podatke za događaj.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Arhiviranje dijagnostičke zapisnike putem cmdleta ljuske PowerShell

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Svojstvo         | Obavezno | Opis                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Da      | ID resursa resurs koji želite postaviti dijagnostičkih postavku.                            |
| StorageAccountId | ne       | ID resursa za pohranu računa na koju želite spremiti dijagnostičke zapisnike.                          |
| Kategorije       | ne       | Popis odvojenih zarezom zapisnika kategorija da biste omogućili.                                                     |
| Omogućeno          | Da      | Booleove vrijednosti koji označava hoće li Dijagnostika su omogućiti ili onemogućiti na ovaj resurs.                  |
| RetentionEnabled | ne       | Booleove vrijednosti koji označava pravilnika o zadržavanju omogućena na ovaj resurs.                            |
| RetentionInDays  | ne       | Broj dana za koji moraju biti zadržani događaja između 1 i 2147483647. Vrijednost nula beskonačno pohranjuje zapisnike. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Arhiviranje zapisnik aktivnosti putem EŽA različite platforme

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Svojstvo         | Obavezno | Opis                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Da      | ID resursa resurs koji želite postaviti dijagnostičkih postavku.                            |
| storageId        | ne       | ID resursa za pohranu računa na koju želite spremiti dijagnostičke zapisnike.                          |
| kategorije       | ne       | Popis odvojenih zarezom zapisnika kategorija da biste omogućili.                                                     |
| omogućeno          | Da      | Booleove vrijednosti koji označava hoće li Dijagnostika su omogućiti ili onemogućiti na ovaj resurs.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Arhiviranje dijagnostičke zapisnike putem REST API-JA
[Pogledajte ovaj dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx) da biste saznali kako možete postavljen dijagnostičkih postavka pomoću Azure Monitor REST API-JA.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Shema dijagnostičke zapisnike u račun za pohranu
Kada ste postavili arhiviranje, spremnik za pohranu stvara se u račun za pohranu čim događaja koji se pojavljuje u jednu od kategorija zapisnika koje ste omogućili. Blob polja unutar spremnik slijedite isti oblik preko dijagnostičke zapisnike i zapisnik aktivnosti. Struktura ove blob-Ova je:

> uvid – zapisnici-{naziv kategorije zapisnika} / = resourceId/PRETPLATE / {ID pretplate} /RESOURCEGROUPS/ {naziv grupe resursa} /PROVIDERS/ {naziv davatelja resursa} / {resursa upišite} / {naziv resursa} / y = {četveroznamenkastom godinom numerički} / m = {dvoznamenkasti mjeseca} / d = {dvoznamenkasti numerički dan} / h = {dvoznamenkasti 24-satni prikaz hour}/m=00/PT1H.json

Ili više jednostavno

> uvide – zapisnici-{naziv kategorije zapisnika} / resourceId = / {resursa Id} / y = {četveroznamenkastom godinom numerički} / m = {dvoznamenkasti mjeseca} / d = {dvoznamenkasti numerički dan} / h = {dvoznamenkasti 24-satni prikaz hour}/m=00/PT1H.json

Na primjer, naziv blob možda je:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Svaki PT1H.json blob sadrži blob JSON događaja koji se nastale tijekom sat naveden u blob URL-a (na primjer, h = 12). Tijekom prikaz sata, događaji dodaju datoteke PT1H.json kako nastaju. Vrijednost minute (m = 00) uvijek 00, jer dijagnostičkog zapisnika događaja podijeljene su u pojedinačne blob-ova po satu.

U datoteci PT1H.json svaki događaj je pohranjena u polju "zapisa", slijedite ovaj oblik:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Naziv elementa  | Opis                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| vrijeme          | Vremenska oznaka kada događaj sastavio servisa Azure obrade zahtjeva odgovara događaj. |
| resourceId    | ID resursa impacted resursa.                                                                       |
| operationName | Naziv operacije.                                                                                      |
| Kategorija      | Kategorija zapisnika događaja.                                                                                  |
| Svojstva    | Skup `<Key, Value>` parove (odnosno rječnika) s opisom detalje događaja.                            |

> [AZURE.NOTE] Svojstva i korištenje tih svojstava možete ovisi o resurs.

## <a name="next-steps"></a>Daljnji koraci
- [Preuzimanje blob-ova za analizu](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Strujanje dijagnostičke zapisnike događaja koncentratora](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Dodatne informacije o zapisnicima dijagnostičkih podataka](monitoring-overview-of-diagnostic-logs.md)
