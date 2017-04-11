<properties
    pageTitle="Omogućivanje metrike pohrane na portalu za Azure | Microsoft Azure"
    description="Kako omogućiti metrike pohrane za servise Blob, reda čekanja, tablice i datoteke"
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Omogućivanje metrike pohrane Azure i prikaz podataka mjerenja

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Pregled

Prema zadanim postavkama metrike pohrane nije omogućen za servise za pohranu. Možete omogućiti praćenje putem [Portala za Azure](https://portal.azure.com) ili komponente Windows PowerShell ili programski putem klijentska biblioteka za pohranu.

Kada omogućite metrike pohrane, morate odabrati razdoblje zadržavanja podataka: određuje tog razdoblja za koliko prostora za pohranu servisa zadržava metriku i naknade za prostor morat pohraniti. Obično kraći razdoblje zadržavanja trebali biste koristiti minute metriku od svaki sat metriku zbog značajan dodatni prostor potreban za minute metriku. Trebali biste odabrati razdoblje zadržavanja tako da imate li dovoljno vremena za analizu podataka i preuzimanje sve metriku koje želite zadržati izvanmrežno analizirali ili pak izvješćivanja. Imajte na umu da vam se naplaćivati za preuzimanje metriku podataka s računa za pohranu.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Kako omogućiti mjernih podataka pomoću portala za Azure

Slijedite ove korake da biste omogućili metriku [Azure Portal](https://portal.azure.com):

1. Idite na račun servisa za pohranu.
1. Otvorite plohu **Postavke** , a zatim odaberite **Dijagnostika**.
1. Provjerite je li **je Status** postavljen na **na**.
1. Odaberite metriku za servise koje želite nadzirati.
2. Navedite pravilo zadržavanja da biste naznačili koliko da biste zadržali metriku i zapisivanje podataka.

Imajte na umu da [Azure Portal](https://portal.azure.com) trenutno omogućuje konfiguriranje minute metriku na računu za pohranu morate omogućiti minute metriku pomoću komponente PowerShell ili programski.

## <a name="how-to-enable-metrics-using-powershell"></a>Kako omogućiti metriku pomoću komponente PowerShell

PowerShell možete koristiti na lokalno računalo da biste konfigurirali prostor za pohranu metrika na vašem računu za pohranu pomoću cmdleta Azure PowerShell Get-AzureStorageServiceMetricsProperty za dohvaćanje trenutne postavke, a cmdlet skup AzureStorageServiceMetricsProperty da biste promijenili trenutne postavke.

Cmdleti za koja određuju metrike pohrane koristiti sljedeće argumente:

- MetricsType: mogući su vrijednosti sat i Minute.

- ServiceType: moguće vrijednosti su Blob, reda čekanja i tablice.

- MetricsLevel: mogući su vrijednosti ništa, servis i ServiceAndApi.

Ako, na primjer, sljedeću naredbu prelazi na minute metriku za servis Blob u zadani račun za pohranu uz zadržavanje razdoblje postavljena na pet dana:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Sljedeća naredba dohvaća trenutni svaki sat metriku razine i zadržavanje dana za servis blob u zadani račun za pohranu:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Informacije o konfiguriranju Azure PowerShell cmdleti za rad s web-mjesto Azure pretplata i u okvir za odabir zadanog računa za pohranu da biste koristili, potražite u članku: [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Kako omogućiti metrike pohrane programski

U sljedećim C# isječak prikazuje kako omogućiti metriku i prijave za servis Blob pomoću klijentska biblioteka za pohranu za .NET:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days.
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);


## <a name="viewing-storage-metrics"></a>Prikaz metrike pohrane

Kada konfigurirate Analytics za pohranu metrika praćenje računa za pohranu, analitičkih podataka za pohranu bilježi metriku skup poznati tablica na vašem računu za pohranu. Možete konfigurirati grafikoni da biste pogledali svaki sat metriku [Azure Portal](https://portal.azure.com):

1. Idite na račun za pohranu za [Portal za Azure](https://portal.azure.com).
2. U odjeljku **nadzor** kliknite **Dodaj pločice** da biste dodali novi grafikon. U **Galeriji pločica**odaberite metrički želite prikazati, a povucite u odjeljku **nadzor** .
3. Da biste uredili koje metriku prikazuju se na grafikonu, kliknite **Uredi** vezu. Možete dodati ili ukloniti pojedinačne metriku odabirom ili poništenje odabira ih.
4. Kada ste gotovi s uređivanjem metriku, kliknite **Spremi** .

Ako želite da biste preuzeli metriku Dugoročne prostora za pohranu ili da biste ih analizirali lokalno, morat ćete:

- Pomoću alata koji se zna u ovim su tablicama i jednostavan način možete vidjeti i preuzeti ih.
- Napišite prilagođenu aplikaciju ili skripta za čitanje i spremanje tablice.

Mnogo prostora za pohranu pregledavanje alata trećih strana nisu svjesni postojanja u ovim su tablicama i omogućuju vam da pregledavaju izravno.
Pročitajte članak [Azure prostora za pohranu programu Software Explorer](storage-explorers.md) popis dostupni alati.

> [AZURE.NOTE] Počevši od verzije 0.8.0 [Microsoft Azure prostora za pohranu Explorer] (http://storageexplorer.com/), će sada ćete moći vidjeti i preuzeti metriku tablice analize.

Da bi se programski pristupe analize tablice, imajte na umu da analize tablice ne prikazuju se ako popis sve tablice u vašem računu za pohranu. Možete im pristupiti izravno po imenu ili pomoću [CloudAnalyticsClient API -JA](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) u biblioteci klijent .NET poslati upit nazive tablica.

### <a name="hourly-metrics"></a>Svaki sat mjerenja
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minute mjerenja
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapaciteta
- $MetricsCapacityBlob

Možete pronaći sve detalje shema za u ovim su tablicama pri [Prostora za pohranu analize metriku tablice shemu](https://msdn.microsoft.com/library/azure/hh343264.aspx). Oglednih redaka ispod Prikaži samo podskup stupci slobodni, ali ilustracija neke važne značajke način metrike pohrane sprema te metriku:

| PartitionKey  |       RowKey       |                    Vremenska oznaka | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Dostupnost | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      korisnik; Sve      | 2014.-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | korisnik; QueryEntities | 2014.-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7.8                  | 100            |
| 20140522T1100 |  korisnik; QueryEntity  | 2014.-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | korisnik; UpdateEntity  | 2014.-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

U ovim podacima minute metriku primjer tipku particija koristi vrijeme minute razlučivosti. Tipku retka označava vrstu podataka koja je spremljena u retku, a to sastoji se od dva komada informacije, vrsta pristupa i vrstu zahtjeva:

- Vrsta pristupa je korisnik ili sustavu, pri čemu je korisničko se odnosi na sve zahtjeve za korisnika na servis za pohranu, a sustava odnosi se na zahtjeve za koje je načinio Analytics za pohranu.

- Vrsta zahtjev je sve u kojem se slučaju ga je sažetak redak ili ga služi za identifikaciju određene API kao što su QueryEntity ili UpdateEntity.


Ogledni podaci iznad prikazuje sve zapise jedan min (počevši od 11:00 se), tako da se broj zahtjeva QueryEntities plus broj QueryEntity zahtjeve plus broj UpdateEntity zahtjeva za dodavanje do brošure, što je zbroj prikazuje u retku korisnika: sve. Isto tako, proizlaziti average završetka do kraja latenciju 104.4286 u retku korisnika: sve izračunom ((143.8 * 5) + 3 + 9) / 7.

Razmislite o postavljanju upozorenja [Azure Portal](https://portal.azure.com) na stranici monitora tako da se metrike pohrane možete automatski obavijestiti o važnim promjenama ponašanje servise za pohranu. Ako koristite alat za pohranu explorer da biste preuzeli metriku podatke u obliku razdvojeni, Microsoft Excel možete koristiti za analizu podataka. Potražite u članku na blogu [Microsoft Azure prostora za pohranu programu Software Explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) popis Alati explorer dostupan prostor za pohranu.



## <a name="accessing-metrics-data-programmatically"></a>Programatski pristup podacima mjerenja

Sljedeći popis prikazuje uzorak C# kod koji pristupa minute metriku raspon minuta i prikazuje rezultate na konzoli prozora. Koristi se za pohranu biblioteku Azure verzija 4 koji uključuje klase CloudAnalyticsClient koji pojednostavljuje pristupa tablice metrike pohrane u.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
    select entity;

    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }

    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;

    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Što se naplaćuje učinite koje stvaraju kada omogućite metrike pohrane?

Zahtjevi za stvaranje tablice entiteti za metriku se naplaćuju po standardne stope odnosi se na sve operacije Azure prostora za pohranu za pisanje.

Čitanje i brisanje zahtjeva za klijent metriku podataka su utrošeno na standardni tečajeve. Ako ste konfigurirali pravilo zadržavanja podataka, neće se naplaćuju kada Azure prostora za pohranu izbriše stare metriku podatke. Međutim, ako izbrišete analize podataka, vaš račun nije naplatiti operacija brisanja.

Kapacitet koristi metriku tablica je utrošeno: za procjenu iznos kapaciteta koji se koristi za pohranu metrika podataka možete koristiti na sljedeći način:

- Ako svaki sat servisa koristi svaki API-JA u svaku uslugu, zatim približno 148 baze podataka se pohranjuju svaki sat u tablicama transakcije metriku ako ste omogućili servisa i API razini sažetka.

- Ako svaki sat servisa koristi svaki API-JA u svaku uslugu, zatim približno 12 baze podataka se pohranjuju svaki sat u tablicama transakcije metriku ako ste omogućili samo razina usluge sažetka.

- U tablici kapaciteta blob-ova ima dva retka dodana svakog dana (korisnik prestane koristiti delve u za zapisnike): to podrazumijeva da svakodnevno veličinu u ovoj su tablici povećava do više od 300 bajtova.

## <a name="next-steps"></a>Sljedeći koraci:
[Omogućivanje prijave i pristupanje podaci iz zapisnika za pohranu](https://msdn.microsoft.com/library/dn782840.aspx)
