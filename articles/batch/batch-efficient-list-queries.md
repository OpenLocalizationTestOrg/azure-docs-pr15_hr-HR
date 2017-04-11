<properties
    pageTitle="Učinkovito upite u seriji Azure | Microsoft Azure"
    description="Poboljšati performanse filtriranjem upitima prilikom traženja informacije o obradu resurse kao što su grupe, zadatke, zadatke i izračunati čvorove."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="query-the-azure-batch-service-efficiently"></a>Učinkovito upita servisa Azure grupe

Ovdje ćete saznati kako poboljšati performanse aplikacije Azure grupe tako da smanjite količinu podataka koje se vraćaju servis kada upit zadataka, zadaci i izračunati čvorove s [.NET obradu] [ api_net] biblioteke.

Gotovo sve aplikacije za obradu moraju izvršiti neke vrste nadzor ili nekog drugog postupak koji često upiti servis za obradu u pravilnim vremenskim razmacima. Ako, na primjer, da biste utvrdili postoje li sve u redu čekanja zadatke u posao, morate dobiti podataka na svakom zadatku posla. Da biste provjerili status čvorove u svoje grupe aplikacija, morate dobiti podataka na svakoj čvor u. U ovom se članku objašnjava kako izvršiti takvih upita u najučinkovitiji način.

## <a name="meet-the-detaillevel"></a>Upoznavanje s DetailLevel

U radne grupe aplikacija entiteti kao što su zadacima, zadatke i računalnim čvorove možete numerirati tisućica. Kada zatražite informacije o sljedećim resursima potencijalno veliku količinu podataka morate "siječe na žičani" iz servisa za obradu u aplikaciji na svaki upit. Ograničavanjem broja stavki i vrste podataka koje je vratio upit možete povećati brzinu svoje upite i stoga performanse aplikacije.

[Grupe za .NET] [ api_net] isječak koda API popisi *svaki* zadatak koji je povezan s posla, uz *sve* svojstva svakog zadatka:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Na raspolaganju vam mnogo učinkovitiji upit za popis, međutim, primjenom "razina pojedinosti" u upit. To možete učiniti tako unošenju podataka [ODATADetailLevel] [ odata] objekt [JobOperations.ListTasks] [ net_list_tasks] način. Ovaj isječak vraća samo ID, naredbenog retka i računalnim čvor informacije svojstva dovršene zadatke:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

U ovom primjeru scenariju, ako postoje tisuće zadatke u sklopu posla rezultate iz drugog upita obično vratit će se većina brže od prvoga. Dodatne informacije o korištenju ODATADetailLevel kada popis stavki s API-jem .NET obradu je uključiti [ispod](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Preporučujemo te vam *uvijek* navesti objekta ODATADetailLevel pozivima .NET API popis da biste bili sigurni učinkovitiji i performanse aplikacije. Navođenjem razine detalja može pridonijeti donje grupe servisa odgovor vremena, poboljšanje Upotreba mreže i minimiziranje memorije po klijentske aplikacije.

## <a name="filter-select-and-expand"></a>Filtriranje, odaberite i proširivanje

[Grupe za .NET] [ api_net] i [POSTAVITE obradu] [ api_rest] API-ji pružaju mogućnost da biste smanjili broj stavki koje se vraćaju se na popisu, kao i količinu informacija koje se vraćaju za svaki unos. To navođenjem **Filtriranje**, **Odaberite**i **proširite nizovi** prilikom izvršavanja upita popisa.

### <a name="filter"></a>Filtar
Niz filtra je izraz koji se smanjuje broj stavki koje se vraćaju. Ako, na primjer, popis samo izvodi zadatke za posao ili popis samo računalnim čvorove spremni za izvođenje zadataka.

- Niz filtra sastoji se od jedan ili više izraza izraze koji se sastoji od naziv svojstva, operator i vrijednost. Možete navesti svojstva vrijede za svaku vrstu entitet za upit, kao što su operatori podržani za svako svojstvo.
- Više izraza možete kombinirati pomoću logički operatori `and` i `or`.
- U ovom se primjeru filtrirati popise niz samo tekućeg "prikaz" Zadaci: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Odaberite
Odaberite niz ograničenjima vrijednosti nekretnina vraćene za svaku stavku. Navedite popis naziva svojstva, a samo one vrijednosti nekretnina vraćaju stavki u rezultatima upita.

- Odaberite niz sastoji se od popisa odvojenih zarezom svojstvo imena. Možete odrediti bilo koje od svojstava za entitet vrste su upita.
- U ovom primjeru odaberite niz određuje samo tri vrijednosti nekretnina bi trebala biti vraćena za svaki zadatak: `id, state, stateTransitionTime`.

### <a name="expand"></a>Proširivanje
Proširivanje niz smanjuje broj API poziva koje su potrebne da biste dobili određene podatke. Kada koristite niz za proširivanje, moguće dobiti dodatne informacije o svakoj stavci s jednom API poziva. Umjesto stjecanje popis entiteti traži podatke za svaku stavku na popisu pomoću niz za proširivanje da biste dobili iste podatke u jednom API poziva. Manje API poziva znači bolje performanse.

- Slično odaberite niz, proširivanje niz određuje hoće li određenih podataka obuhvaćeni popisa rezultata upita.
- Proširivanje niz podržano je samo kada se koristi u popis zadataka, raspored posla, zadatke i grupe. Trenutno samo podržava Statistika informacije.
- Kada su potrebni sva svojstva, a nije naveden bez odaberite niz, na Proširi niz *moraju* se koristiti da biste saznali Statistika. Ako niz odaberite koristi se da biste dobili podskup svojstva, zatim `stats` bude navedena u nizu za odabir i proširivanje niz moraju biti određeni.
- U ovom se primjeru proširite niz navode informacije Statistika bi trebala biti vraćena za svaku stavku na popisu: `stats`.

> [AZURE.NOTE] Tijekom izgradnje vrste niz tri upita (filtriranje, odaberite i proširite), koje morate naziva svojstava i predmet provjerite podudaraju li se od verzija za element njihove REST API-JA. Ako, na primjer, prilikom rada s klasu.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) , morate navesti **stanju** umjesto **Stanje**, čak i ako je svojstvo .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Tablice ispod za mapiranja svojstava između .NET i REST API-ji potražite u članku.

### <a name="rules-for-filter-select-and-expand-strings"></a>Pravila za filtriranje, odaberite a proširite nizove

- Svojstva imena u filtru, odaberite pa proširite nizove prikazivati kao [Obrade ostalih] [ api_rest] API – čak i kada koristite [Obradu .NET] [ api_net] ili nešto na druge grupe SDK-ovi.
- Sve svojstvo nazivima razlikuju se velika i mala slova, ali su vrijednosti nekretnina slova.
- Datumu/vremenu nizove može biti jedna od dva oblika, a mora biti ispred `DateTime`.

  - Primjer oblikovanje W3C DTF:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Primjer oblikovanje RFC 1123:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Booleova nizovi su `true` ili `false`.
- Ako svojstva koje nisu valjane ili operator nije naveden, u `400 (Bad Request)` će uzrokovati pogrešku.

## <a name="efficient-querying-in-batch-net"></a>Učinkovito pretraživanje na obradu .NET

Unutar [Grupe za .NET] [ api_net] API-JA, [ODATADetailLevel] [ odata] klasa se koristi za unošenju podataka filtar odaberite, a proširenje nizove na popisu operacija. Klase ODataDetailLevel ima tri svojstva javnog niz koji mogu biti naveden u na Graditelj ili postavljanje izravno na objekt. Zatim prenesite ODataDetailLevel objekt kao parametar za razne operacije popisa kao što su [ListPools][net_list_pools], [ListJobs][net_list_jobs], a [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Ograničiti broj stavki koje se vraćaju.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Odredite koje vrijednosti nekretnina vraćaju se sa svakom stavkom.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Dohvaćanje podataka za sve stavke u jednom API poziva umjesto zasebnom pozive za svaku stavku.

Sljedeći isječak koda koristi .NET API obradu učinkovito upita za obradu servisa za statistiku određeni skup grupe. U ovom scenariju grupe korisnika ima test i radne grupe. Test skup ID-ovi su mjestu sa "testom.", a zatim radni skup ID-ovi su mjestu s "radni". U isječak, *myBatchClient* je pravilno inicijalizirana instancu klase [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Instance komponente [ODATADetailLevel] [ odata] koji je konfiguriran za korištenje odaberite i proširivanje rečenice možete također se proslijediti odgovarajuće Pronađite načine, kao što su [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), da biste ograničili količinu podataka koje se vraćaju.

## <a name="batch-rest-to-net-api-mappings"></a>Obradu ostalih za .NET API mapiranja

Svojstvo imena u filtru, odaberite, a proširite nizovi *morate* odražavaju njihove verzija REST API-JA, u web-mjesto naziv i u okvir za slučaj. U tablicama u nastavku omogućuju mapiranja između verzija za .NET i REST API-JA.

### <a name="mappings-for-filter-strings"></a>Mapiranja nizova filtra

- **.NET popis metoda**: svaki od metoda .NET API-JA u ovom stupcu prihvaća [ODATADetailLevel] [ odata] objekt kao parametar.
- **Zahtjevi za OSTATAK popis**: svakog REST API-JA stranica povezana u ovom stupcu sadrži tablicu koja navodi svojstva i operacije koje su dopuštene u nizovima *Filtar* . Ove postupke i nazivi svojstava će koristiti kada sastavljanje [ODATADetailLevel.FilterClause] [ odata_filter] niz.

| Načini popis .NET | Zahtjevi za OSTATAK popis |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Popis certifikati u račun][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Popis datoteka pridružene zadatak][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Popis statusa pripremu za posao i zadataka izdanje posla za posao][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Popis zadataka u račun][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Popis datoteka na čvor][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Popis zadataka povezan s posla][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Popis rasporedima zadataka u račun][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Popis zadataka pridružene raspored posla][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Popis čvorove računalnim u zajedničko područje][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Popis grupe putem računa][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Mapiranja odaberite nizova

- **Vrsta grupe za .NET**: API .NET obradu vrste.
- **Entiteti REST API -JA**: svaka stranica u ovom stupcu sadrži jednu ili više tablica s popisima nazive svojstvo REST API-JA za tu vrstu. Nazivi svojstvo se koristi prilikom slažete *Odaberite* nizove. Ove istim nazivima svojstvo će koristiti kada sastavljanje [ODATADetailLevel.SelectClause] [ odata_select] niz.

| Obradu vrste .NET | Entiteti REST API-JA |
|---|---|
| [Certifikat][net_cert] | [Informacije o potvrdi][rest_get_cert] |
| [CloudJob][net_job] | [Informacije o posao][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Informacije o raspored posla][rest_get_schedule] |
| [ComputeNode][net_node] | [Informacije o čvor][rest_get_node] |
| [CloudPool][net_pool] | [Informacije o zajedničko područje][rest_get_pool] |
| [CloudTask][net_task] | [Informacije o zadatku][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Primjer: sastavljanje niz filtra

Kada stvorite niz filtra za [ODATADetailLevel.FilterClause][odata_filter], pogledajte gornje tablice u odjeljku "Mapiranje filtar nizova" pronalaženje REST API-JA dokumentaciju stranicu koja odgovara postupka popis koji želite izvesti. Koje je moguće filtrirati svojstava i njihovih podržani operatora pronaći ćete u prvoj tablici multirow na toj stranici. Ako želite dohvatiti Svi zadaci čije izlazni kod je osim nule, na primjer, redak na [popisu zadataka povezan s posla] [ rest_list_tasks] određuje odgovarajuće svojstvo niza i dozvoljenu operatora:

| Svojstvo | Operacije dopuštene | Vrsta |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Dakle, niz filtra za popis sve zadatke kod osim nule izlaz bio je sljedeći:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Primjer: Stvaranje niza za odabir

Sastavljanje [ODATADetailLevel.SelectClause][odata_select], pogledajte gornje tablice u odjeljku "Mapiranje odaberite nizova" i idite na stranicu REST API-JA koji odgovara vrsti entitet koji su stavku. Moguće odabrati svojstava i njihovih podržani operatora pronaći ćete u prvoj tablici multirow na toj stranici. Ako želite dohvatiti samo ID i naredbenog retka za svaki zadatak na popisu, na primjer, pronaći te retke u tablici primjenjivo na [informacije o zadatku][rest_get_task]:

| Svojstvo | Vrsta | Bilješke |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Odaberite niz za uključivanje samo ID i naredbenog retka uz svaki navedenih zadatak pa bio sljedeći:

`id, commandLine`

## <a name="code-samples"></a>Primjere koda

### <a name="efficient-list-queries-code-sample"></a>Uzorak koda upita učinkovitog popisa

Pogledajte [EfficientListQueries] [ efficient_query_sample] uzorka projekt na GitHub da biste vidjeli kako učinkovito upita popisa može utjecati na performanse u aplikaciji. Ove aplikacije konzole za C# stvara i dodaje velik broj zadataka posla. Nakon toga će se više poziva [JobOperations.ListTasks] [ net_list_tasks] metodu i prolaze [ODATADetailLevel] [ odata] objekata konfiguriranih pomoću vrijednosti različitih svojstava za promjenu količine podataka koja se vraća. Ona će dati izlaz sličnu ovoj:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Kao što je prikazano u Proteklog vremena, možete znatno smanjite upita odgovor vremena ograničavanjem svojstava i broj stavki koje se vraćaju. To i druge projekte uzorka možete pronaći u [azure, grupe i uzorke] [ github_samples] spremište na GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>Ogledna biblioteka i kod BatchMetrics

Osim gornje kod uzorka EfficientListQueries, možete pronaći [BatchMetrics] [ batch_metrics] projekta u [azure, grupe i uzorke] [ github_samples] GitHub spremište. Ogledni projekta BatchMetrics pokazuje kako učinkovito praćenje napretka posla grupe za Azure pomoću obrade API-JA.

[BatchMetrics] [ batch_metrics] uzorka sadrži biblioteku projekt predmete .NET koje možete ugraditi u vlastiti projekata i jednostavan program naredbenog retka Nekorištenje i ilustrira korištenje biblioteke.

Primjer aplikacije u projektu prikazuje sljedeće postupke:

1. Odabir određene atribute da bi se samo morate svojstva preuzimanja
2. Filtriranje na stanje prijelaza vremena da bi se Preuzmi samo promjene nakon zadnjeg upita

Na primjer, sa sljedećim korakom pojavljuje se u BatchMetrics biblioteke. Vratit će se ODATADetailLevel koji određuje koje samo na `id` i `state` za entiteti koji su mu treba moguće dohvatiti svojstva. Navodi koje samo entiteti od određenu promijenjena čije stanje `DateTime` parametar bi trebala biti vraćena.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Daljnji koraci

### <a name="parallel-node-tasks"></a>Paralelni čvor zadataka

[Maksimiziranje grupe za Azure izračunati korištenje resursa sa zadacima Istodobni čvor](batch-parallel-node-tasks.md) je još je jedan članak vezane uz performanse aplikacije grupe. Neke vrste radnih opterećenja su prednosti izvršavanja paralelno zadaci na veću –, ali manje – izračun čvorove. Pogledajte [primjer scenarija](batch-parallel-node-tasks.md#example-scenario) u članku informacije o takve scenarij.

### <a name="batch-forum"></a>Forum za obradu

[Forum za obradu Azure] [ forum] na MSDN-sjajno su početno mjesto raspravljati grupe i postavljati pitanja o servisu. Glavni na putem za korisno "ljepljive" objave, i pitanja kao što su biste tijekom stvaranja vašeg rješenja za obradu.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
