<properties
    pageTitle="Korištenje OSTALE za sigurnosno kopiranje i vraćanje aplikacije servisa za aplikacije"
    description="Saznajte kako koristiti RESTful API poziva za sigurnosno kopiranje i vraćanje aplikaciju u aplikacije servisa za Azure"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Korištenje OSTALE za sigurnosno kopiranje i vraćanje aplikacije servisa za aplikacije

> [AZURE.SELECTOR]
- [PowerShell](../app-service/app-service-powershell-backup.md)
- [REST API-JA](websites-csm-backup.md)

[Aplikacije servisa za aplikacije](https://azure.microsoft.com/services/app-service/web/) mogu se sigurnosno kao BLOB-ova u Azure prostora za pohranu. Stvaranje sigurnosne kopije mogu sadržavati baze podataka aplikacije. Aplikaciju ikad slučajno izbriše ili aplikaciju mora biti vraćeni u starijim verzijama, može vratiti iz bilo kojeg prethodne sigurnosne kopije. Sigurnosne kopije možete učiniti na zahtjev u bilo kojem trenutku ili sigurnosne kopije može se zakazati u odgovarajuću vremenskim razmacima.

U ovom se članku objašnjava kako sigurnosnog kopiranja i vraćanja aplikacije RESTful API zahtjevima. Ako želite stvoriti i upravljati kopija aplikacije grafički putem portala za Azure, u odjeljku [sigurnosno kopiranje web app u aplikacije servisa za Azure](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Početak rada
Slanje zahtjeva za OSTALE, morate znati **naziv**vaše aplikacije, **grupa resursa**i **id pretplate**. Ove informacije možete pronaći tako da kliknete aplikacije u plohu **Aplikacije servisa** [Azure portal](https://portal.azure.com). Primjer u ovom članku, ne možemo konfiguriranja web-mjesta **backuprestoreapiexamples.azurewebsites.net**. Pohranjena u grupi resursa zadani Web WestUS i radi na pretplatu s na ID 00001111-2222-3333-4444-555566667777.

![Informacije o uzorak web-mjesta][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Sigurnosno kopiranje i vraćanje REST API-JA
Ne možemo sada obuhvaća nekoliko primjera kako REST API-JA pomoću sigurnosnog kopiranja i vraćanja aplikacije. Svaki primjer uključuje URL i HTTP zahtjev tijelo. Ogledna URL sadrži rezervirana mjesta umotan u vitičaste zagrade, kao što su {id pretplate}. Zamjena rezerviranih mjesta odgovarajuće podatke za aplikacije. Za referencu, Evo objašnjenje svakog rezerviranog mjesta koji se pojavljuje u primjeru URL-ova.

* Pretplata id – ID-JA Azure pretplatu koja sadrži aplikaciju
* Naziv grupe resursa – – naziv grupe resursa koji sadrži aplikaciju
* Naziv – naziv aplikacije
* sigurnosno kopiranje id – ID-JA sigurnosnu kopiju aplikacije

Dovršavanje dokumentaciju API-JA, uključujući nekoliko neobavezni parametar koji se mogu uključiti u HTTP zahtjev potražite u članku [Explorer Azure resursa](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Sigurnosno kopiranje aplikacije na zahtjev
Da biste odmah sigurnosno aplikaciju, pošaljite zahtjev za **objavu** da biste **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Evo što URL izgleda slično našeg primjera web-mjesta. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/DEFAULT-web-WestUS/Providers/Microsoft.web/Sites/backuprestoreapiexamples/Backup/**

Navedite JSON objekt u tijelu zahtjeva za određivanje koji račun za pohranu da biste koristili za spremanje sigurnosne kopije. Objekt JSON mora imati svojstvo pod nazivom **storageAccountUrl**, koji sadrži [SAS URL](../storage/storage-dotnet-shared-access-signature-part-1.md) dodjelu pristup zapisivanju spremnik za pohranu Azure koja sadrži sigurnosne kopije blob-om. Ako želite da sigurnosno kopiranje baze podataka, morate navesti i popis koji sadrži nazive, vrsta i nizu za povezivanje baza podataka za sigurnosno kopiranje.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Sigurnosnu kopiju aplikacije počinje čim se primi zahtjev. Postupak sigurnosnog kopiranja traje predugo da biste dovršili. HTTP odgovor sadrži ID-a koji možete koristiti u drugi zahtjev da biste vidjeli status sigurnosnu kopiju. Evo primjera tijelo HTTP odgovor u našem zahtjev za sigurnosno kopiranje.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Poruka o pogrešci pronaći ćete u svojstvu zapisnika HTTP odgovor.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Zakazivanje automatskog sigurnosnog kopiranja
Osim sigurnosno kopiranje aplikacije na zahtjev, možete planirati i sigurnosnu kopiju da se dogodi automatski.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Postavljanje novi raspored automatske sigurnosnog kopiranja
Da biste postavili sigurnosne kopije raspored, pošaljite zahtjev za **STAVITI** na **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Evo kako URL izgleda za naše primjer web-mjesto. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/DEFAULT-web-WestUS/Providers/Microsoft.web/Sites/backuprestoreapiexamples/config/Backup**

U tijelu zahtjeva moraju imati JSON objekt koji određuje Konfiguracija sigurnosnog kopiranja. Evo primjera s obavezni parametri.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

U ovom se primjeru konfigurira aplikaciju za automatsko sigurnosno kopiranje svakih sedam dana. Parametri **frequencyInterval** i **frequencyUnit** zajedno odredite koliko često se dogoditi sigurnosnih kopija. Valjane vrijednosti za **frequencyUnit** su **sat** i **dan**. Na primjer, da biste sigurnosnu kopiju aplikacije svaki 12 sati, postavite frequencyInterval 12 i frequencyUnit na sat.

Stare sigurnosne kopije se automatski uklanjaju iz računa za pohranu. Možete odrediti kako stare sigurnosnih kopija može biti postavljanjem parametar **retentionPeriodInDays** . Ako želite da uvijek imati najmanje jednu sigurnosnu kopiju spremili, bez obzira na to koliko je, postavljen **keepAtLeastOneBackup** na true.

### <a name="get-the-automatic-backup-schedule"></a>Početak automatske raspored sigurnosnog kopiranja
Da biste dobili Konfiguracija sigurnosnog kopiranja aplikacije programa, pošaljite zahtjev za **objavu** na URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

URL za naše primjer web-mjesto je **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Dohvatite status sigurnosne kopije
Ovisno o tome kako velike je aplikaciju, sigurnosnu kopiju može potrajati neko vrijeme da biste dovršili. Sigurnosne kopije možda se neće funkcionirati, istek vremena ili djelomično uspjeti. Da biste vidjeli status sve aplikacije, kopija, pošaljite zahtjev za **DOHVAĆANJE** URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Da biste vidjeli status određene sigurnosnu kopiju, pošaljite zahtjev za DOHVAĆANJE URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Evo kako URL izgleda za naše primjer web-mjesto. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/DEFAULT-web-WestUS/Providers/Microsoft.web/Sites/backuprestoreapiexamples/backups/1**

Odgovor tijelo sadrži objekt JSON slično kao u ovom primjeru.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Status sigurnosnu kopiju nije Numerirano vrsta. Ovdje je svaki mogući stanje.

* 0 – u tijeku: Stvaranje sigurnosne kopije je pokrenut, ali još nije završio.
* 1 – nije uspjela: Sigurnosno kopiranje nije uspjelo.
* 2 – uspješno: Sigurnosno kopiranje uspješno dovršena.
* 3 – prekoračili vremensko ograničenje: Sigurnosnu kopiju nije završeno u vremenu i otkazan.
* 4 – stvoreno: Zahtjev za sigurnosne kopije je u redu čekanja, ali nije pokrenut.
* 5 – preskočeno: Sigurnosno kopiranje jeste li nastaviti zbog raspored pokretanje previše sigurnosne kopije.
* 6 – PartiallySucceeded: Sigurnosno kopiranje je uspjela, ali neke datoteke nisu sigurnosno jer ih nije moguće čitati. To se obično događa jer je zaključavanje smješta na datotekama.
* 7 – DeleteInProgress: Sigurnosno kopiranje zatražio je izbrišu, ali još nije izbrisan.
* 8 – DeleteFailed: Sigurnosno kopiranje nije moguće izbrisati. To se može dogoditi istekla SAS URL koji je korišten za stvaranje sigurnosne kopije.
* 9 – izbrisati: Sigurnosno kopiranje uspješno je izbrisana.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Vraćanje aplikacije iz sigurnosne kopije
Ako je izbrisana aplikacije ili ako želite da biste se vratili na prethodnu verziju aplikacije, aplikaciju možete vratiti iz sigurnosne kopije. Pozvati vraćanja, pošaljite zahtjev za **objavu** na URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Evo kako URL izgleda za naše primjer web-mjesto. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/DEFAULT-web-WestUS/Providers/Microsoft.web/Sites/backuprestoreapiexamples/backups/1/Restore**

U tijelu zahtjeva za slanje JSON objekt koji sadrži svojstva za postupak vraćanja. Slijedi primjer koji sadrži sva odgovarajuća svojstva:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Vraćanje na nove aplikacije
Ponekad možda želite stvoriti novu aplikaciju kada Vraćanje sigurnosne kopije, umjesto prebrisivanja već postojeće aplikacije. Da biste to učinili, promijenite zahtjev za URL-a na novu aplikaciju koju želite stvoriti, te svojstvo **prebrisati** u na JSON **False**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Brisanje sigurnosne kopije u aplikaciju
Ako želite da biste izbrisali sigurnosnu kopiju, pošaljite zahtjev za **Brisanje** na URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Evo kako URL izgleda za naše primjer web-mjesto. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/DEFAULT-web-WestUS/Providers/Microsoft.web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>Upravljanje URL SAS sigurnosne kopije
Aplikacije servisa za Azure će pokušati brisanje sigurnosne kopije iz spremišta Azure pomoću SAS URL koji ste dobili prilikom stvaranja sigurnosne kopije. Ovaj URL SAS više nije valjana, sigurnosno kopiranje nije moguće izbrisati kroz REST API-JA. Međutim, možete ažurirati URL SAS pridružene sigurnosnu kopiju slanjem zahtjeva za **objavu** na URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Evo kako URL izgleda za naše primjer web-mjesto. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/DEFAULT-web-WestUS/Providers/Microsoft.web/Sites/backuprestoreapiexamples/backups/1/list**

U tijelu zahtjeva za slanje JSON objekt koji sadrži novi URL SAS. Evo primjera.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Sigurnosnih vam razloga URL SAS pridružene sigurnosnu kopiju nisu vraćena prilikom slanja zahtjeva za PRISTUP za određenu sigurnosnu kopiju. Ako želite da biste pogledali URL SAS pridružene sigurnosnu kopiju, pošaljite zahtjev za objavu na istoj URL iznad. U tijelu zahtjeva sadržavati prazan JSON objekt. Odgovor poslužitelja sadrži sve informacije o tom sigurnosne kopije, uključujući njezin URL SAS.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
