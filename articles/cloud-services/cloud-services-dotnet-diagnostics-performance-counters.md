<properties
   pageTitle="Korištenje mjerača performansi u Azure Dijagnostika | Microsoft Azure"
   description="Da biste pronašli grla i ugađanje performansi pomoću mjerača performansi web-mjesto servisa Azure oblaka ili u okvir za virtualnog računala."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Stvaranje i korištenje mjerača performansi u aplikaciji za Azure

U ovom se članku opisuju prednosti i kako da biste postavili mjerača performansi u Azure aplikacije. Prikupljanje podataka, pronađite grla i precizno podesite performanse sustava i aplikacija možete ih koristiti.

Dostupne za Windows Server, IIS i ASP.NET mjerača performansi možete koji se prikupljaju i upotrijebljena za određivanje stanju Azure web uloge, uloge suradnika i virtualnih računala. Stvaranje i korištenje mjerača prilagođene performansi.  

Možete provjeriti brojač podataka o performansama
1. Izravno na glavnom računalu aplikacije pomoću alata za praćenje performansi pristupiti putem udaljene radne površine
2. Sa sustava centar za komponentu Operations Manager pomoću paket za upravljanje Azure
3. Pomoću drugih nadzora alata za pristup dijagnostičkih podataka prenijeti Azure prostora za pohranu. Dodatne informacije potražite u [spremište i prikaz dijagnostičkih podataka u Azure prostora za pohranu](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

Dodatne informacije o performansama programa [Azure klasični portal](http://manage.azure.com/)za nadzor, potražite [u](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/)članku servisi u Oblaku Monitor.

Dodatne detaljnije upute o stvaranju na bilježenje i praćenje strategije i korištenje dijagnostiku i drugih tehnika za otklanjanje poteškoća i optimizirati Azure aplikacije, potražite u članku [Otklanjanje poteškoća najbolje prakse za razvoj aplikacija za Azure](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Omogućivanje brojač performanse

Prema zadanim postavkama nisu omogućeni mjerača performansi. Aplikacije i pokretanje zadatka izmijeniti zadanu Dijagnostika agent za konfiguraciju da biste uključili mjerača performansi određene koje želite nadzirati za svaku instancu uloge.

### <a name="performance-counters-available-for-microsoft-azure"></a>Dostupno za Microsoft Azure mjerača performansi

Azure nudi podskup mjerača performansi dostupne za Windows Server, IIS i ASP.NET stogu. Sljedeća tablica prikazuje dio mjerača performansi u određenom kamata za Azure aplikacije.

|Kategorija brojač: Objekta (Instance)|Naziv brojač      |Referenca|
|---|---|---|
|.NET CLR iznimke (_globalni_)|# Exceps izbačena sekundi   |Iznimke mjerača performansi|
|.NET CLR memorije (_globalni_)    |Jedan % GC            |Mjerača performansi memorije|
|PLATFORME ASP.NET                      |Ponovnog pokretanja računala    |Mjerača performansi za platforme ASP.NET|
|PLATFORME ASP.NET                      |Zahtjev za vrijeme izvođenja  |Mjerača performansi za platforme ASP.NET|
|PLATFORME ASP.NET                      |Zahtjevi za prekinuta veza   |Mjerača performansi za platforme ASP.NET|
|PLATFORME ASP.NET                      |Ponovno pokreće postupak tempiranja |Mjerača performansi za platforme ASP.NET|
|ASP.NET aplikacije (__Ukupno__)|Zahtjevi za ukupni zbroj        |Mjerača performansi za platforme ASP.NET|
|ASP.NET aplikacije (__Ukupno__)|Zahtjevi s          |Mjerača performansi za platforme ASP.NET|
|V4.0.30319 platforme ASP.NET           |Zahtjev za vrijeme izvođenja  |Mjerača performansi za platforme ASP.NET|
|V4.0.30319 platforme ASP.NET           |Vrijeme čekanja zahtjeva       |Mjerača performansi za platforme ASP.NET|
|V4.0.30319 platforme ASP.NET           |Zahtjevi za trenutni        |Mjerača performansi za platforme ASP.NET|
|V4.0.30319 platforme ASP.NET           |Zahtjevi u redu čekanja         |Mjerača performansi za platforme ASP.NET|
|V4.0.30319 platforme ASP.NET           |Zahtjevi za odbijena       |Mjerača performansi za platforme ASP.NET|
|Memorije                       |Dostupne MBytes        |Mjerača performansi memorije|
|Memorije                       |Odluka o izvršenom bajtova         |Mjerača performansi memorije|
|Processor(_Total)            |Vrijeme procesor %        |Mjerača performansi za platforme ASP.NET|
|TCPv4                        |Do neuspjele veze     |TCP objekta|
|TCPv4                        |Veza uspostavljena |TCP objekta|
|TCPv4                        |Poništavanje veze       |TCP objekta|
|TCPv4                        |Šalje segmenata/sec       |TCP objekta|
|Mrežni Interface(*)         |Bajtova/sec      |Objekt sučelja mreže|
|Mrežni Interface(*)         |Šalje bajtova/sec          |Objekt sučelja mreže|
|Mrežni sučelja (Microsoft virtualnog računala Bus mrežnog prilagodnika _2)|Bajtova/sec|Objekt sučelja mreže|
|Mrežni sučelja (Microsoft virtualnog računala Bus mrežnog prilagodnika _2)|Šalje bajtova/sec|Objekt sučelja mreže|
|Mrežni sučelja (prilagodnik _2 mreže Microsoft virtualnog računala Bus)|Ukupno sec bajtova|Objekt sučelja mreže|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Stvaranje i dodavanje mjerača performansi prilagođene aplikacije

Azure ima podršku za stvaranje i mijenjanje mjerača prilagođene performansi web uloge i uloge suradnika. Na mjerača možda se koristiti za praćenje i nadzor ponašanje specifičnim aplikacijama. Možete stvarati i brisati prilagođene performanse brojač kategorije i specifikator iz pokretanje zadatka, uloga web ili uloga suradnika s dodatnim dozvolama.

>[AZURE.NOTE] Kod koji se mijenja mjerača prilagođene performansi morate razinom dozvole za pokretanje. Ako je kod u ulogu web ili ulogu suradnika, uloge mora obuhvaćati i oznaku <Runtime executionContext="elevated" /> u datoteci ServiceDefinition.csdef uloge Inicijalizacija pravilno.

Prilagođeni performanse podataka možete poslati pomoću agent za dijagnostiku Azure prostora za pohranu.

Brojač podataka standardne performanse je generirao Azure procesa. Prilagođeni performanse podataka moraju se stvoriti tako da web-ulogu ili tempiranja uloga aplikacije. Informacije o vrstama podataka koje se mogu pohraniti u mjerača prilagođene performansi potražite u članku [Vrste brojač performansi](https://msdn.microsoft.com/library/z573042h.aspx) . Primjer u kojem se stvara i skupova podataka prilagođene performanse u ulozi web potražite u članku [PerformanceCounters uzorka](http://code.msdn.microsoft.com/azure/) .

## <a name="store-and-view-performance-counter-data"></a>Pohrana i prikaz brojača podataka o performansama

Azure predmemorira podataka o performansama brojač s drugim dijagnostičke informacije. Ove podatke dostupna je za udaljene nadzor pokrenutom instancu uloga pomoću udaljene radne površine programa access da biste vidjeli alate kao što su praćenje performansi. Održati podataka izvan instancu uloga agent za dijagnostiku morate prenijeti podatke Azure prostora za pohranu. Ograničenje veličine na predmemoriranih podataka o performansama brojač moguće je konfigurirati u agent za dijagnostiku i može se konfigurirati kao dio zajedničke ograničenje za dijagnostičkih podataka. Dodatne informacije o postavljanju Veličina međuspremnika potražite u članku [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) i [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Pregled postavljanje agent za dijagnostiku za prijenos podataka na račun za pohranu potražite u [spremište i prikaz dijagnostičkih podataka u Azure prostora za pohranu](https://msdn.microsoft.com/library/azure/hh411534.aspx) .

Svaku instancu brojač konfigurirani performanse naveden na navedeni uzorkovanja i sampled podaci se prenose na račun za pohranu zahtjeva za zakazano prijenos ili zahtjev za prijenos na zahtjev. Automatsko prijenosi može se zakazati često kao jednom u minuti. Performanse podataka prenijeti po agent za dijagnostiku pohranjuju se u tablici, WADPerformanceCountersTable, na računu za pohranu. U ovoj su tablici možete pristupiti i mu s načinima standardne Azure prostora za pohranu API-JA. Potražite u članku [Microsoft Azure PerformanceCounters uzorka](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) za slanje upita i prikaz podataka o performansama brojač iz tablice WADPerformanceCountersTable primjer.

>[AZURE.NOTE] Ovisno o učestalost prijenos agent za dijagnostiku i Latencija reda čekanja najnovije performanse brojač podatke na računu za pohranu možda zastarjeli nekoliko minuta.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Omogućivanje mjerača performansi korištenjem dijagnostike konfiguracijska datoteka

Da biste omogućili mjerača performansi u aplikaciji za Azure pomoću sljedećeg postupka.

## <a name="prerequisites"></a>Preduvjeti

U ovom se odjeljku pretpostavlja da ste da uvezeni monitor Dijagnostika u aplikaciji i dodati konfiguracijska datoteka Dijagnostika Visual Studio rješenje (diagnostics.wadcfg SDK 2.4, a ispod ili diagnostics.wadcfgx u SDK 2,5 i noviji). Pogledajte korake 1 i 2 u [Omogućivanje Dijagnostika Azure servise u Oblaku i virtualnim strojevima](./cloud-services-dotnet-diagnostics.md)) dodatne informacije.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Korak 1: Prikupljanje i spremanje podataka iz mjerača performansi

Nakon što dodate datoteke Dijagnostika Visual Studio rješenje možete konfigurirati prikupljanje i prostora za pohranu podataka o performansama brojač u aplikaciji za Azure. To možete učiniti tako da dodate mjerača performansi Dijagnostika datoteku. Dijagnostika podataka, uključujući mjerača performansi, prikupljaju se najprije na instancu. Podatke pa dosljedan WADPerformanceCountersTable tablicu u servisu Azure tablice tako da se i morat ćete navesti račun za pohranu u aplikaciji. Ako aplikacija se testirati lokalno u Emulator izračunati, možete i spremiti Dijagnostika podataka lokalno u Emulator prostora za pohranu. Prije nego što spremanje podataka za dijagnostiku morate otvorite [Azure klasični portal](http://manage.windowsazure.com/) i stvaranje računa za pohranu. Najbolja praksa je da biste pronašli svoj račun za pohranu isto mjesto zemlj kao Azure aplikacije da biste izbjegli plaćanja troškove vanjskih propusnosti i da biste smanjili Latencija.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Dodajte mjerača performansi Dijagnostika datoteku

Postoji mnogo mjerača možete koristiti. Sljedeći primjer prikazuje nekoliko mjerača performansi koji se preporučuje za web i nadzor ulogu suradnika.

Otvorite datoteku Dijagnostika (diagnostics.wadcfg SDK 2.4, a ispod ili diagnostics.wadcfgx u SDK 2,5 i iznad) i dodajte sljedeće na element DiagnosticMonitorConfiguration:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Atribut bufferQuotaInMB, koji određuje maksimalnu količinu prostora za sistemske datoteke koja je dostupna za vrstu zbirke podataka (Azure zapisnike, IIS zapisnika itd.). Zadana je vrijednost 0. Kada je dostigne kvote, najstarije podaci se brišu prilikom dodavanja nove podatke. Zbroj sva svojstva bufferQuotaInMB mora biti veći od vrijednosti atribut OverallQuotaInMB. Detaljnije rasprave utvrđivanja koliko prostora za pohranu potrebno za skup podataka Dijagnostika, potražite u članku u odjeljku Postavljanje WAD [Otklanjanje poteškoća najbolje prakse za razvoj aplikacija za Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Atribut scheduledTransferPeriod koji određuje razmak između zakazani prijenosi podataka, zaokružuje najbliži minutu. U sljedećim primjerima je postavljen na PT30M (30 minuta). Postavljanje razdoblje prijenos small vrijednosti, kao što je 1 minutu će negativno utjecati na performanse vaše aplikacije u radni, ali mogu poslužiti za prikazuju Dijagnostika brzo radi kada želite testirati. Razdoblje zakazani prijenos mora biti dovoljno male da biste bili sigurni da dijagnostičkih podataka neće se prebrisati na instancu, ali dovoljna da ona neće utjecati na performanse aplikacije.

Atribut counterSpecifier određuje brojač performanse da biste prikupili. Atribut sampleRate određuje Brzina kojom brojač performanse treba biti uzorkovanja, u tom slučaju 30 sekundi.

Kada dodate mjerača performansi u koje želite prikupiti, promjene se spremaju Dijagnostika datoteku. Ćete morati Navedite račun za pohranu koji će biti dosljedan Dijagnostika podataka.

### <a name="specify-the-storage-account"></a>Navedite račun za pohranu

Da biste svoje podatke za dijagnostiku na račun servisa Azure prostora za pohranu i dalje pojavljuje, morate navesti niza za povezivanje u datoteci konfiguracije (ServiceConfiguration.cscfg) servisa.

Za Azure SDK 2,5 prostora za pohranu račun možete navesti u datoteci diagnostics.wadcfgx.

>[AZURE.NOTE] Ove se upute odnose samo Azure SDK 2.4 i ispod. Za Azure SDK 2,5 prostora za pohranu račun možete navesti u datoteci diagnostics.wadcfgx.

Da biste postavili nizu za povezivanje:

1. Otvorite datoteku ServiceConfiguration.Cloud.cscfg pomoću omiljene uređivač i postavite niz za povezivanje za pohranu. Vrijednosti *AccountName* i *AccountKey* nalaze se na portalu Azure klasični u prostor za pohranu računa nadzorne ploče, u odjeljku Upravljanje tipke.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Spremite datoteku ServiceConfiguration.Cloud.cscfg.

3. Otvorite datoteku ServiceConfiguration.Local.cscfg i provjerite je li UseDevelopmentStorage postavljen na true.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Sad kad se postavljaju u nizu za povezivanje, aplikacija će zadržava Dijagnostika podataka na račun za pohranu kada je implementiran aplikacije.
4. Spremanje i stvaranje projekta, a zatim implementacija aplikacije.

## <a name="step-2-optional-create-custom-performance-counters"></a>Korak 2: Mjerača prilagođene performansi (neobavezno) stvaranje

Osim mjerača unaprijed definiranih performansi možete dodati vlastitu mjerača prilagođene performansi praćenje weba ili tempiranja uloge. Mjerača prilagođene performansi može se koristiti za praćenje i nadzire ponašanje specifičnim aplikacijama i mogu stvoriti i brisati u pokretanje zadatka, uloga web ili uloga suradnika s dodatnim dozvolama.

Agent za Azure Dijagnostika osvježava performanse brojač konfiguraciju iz datoteke .wadcfg jedne minute nakon pokretanja.  Ako stvarate mjerača prilagođene performansi u način OnStart i traje dulje nego jedne minute za izvršavanje zadataka pokretanja, mjerača svoje prilagođene performansi će nije stvorena kada agent za dijagnostiku Azure pokuša učitati ih.  U ovom scenariju vidjet ćete Azure Dijagnostika pravilno snima sve podatke za dijagnostiku osim mjerača svoje prilagođene performansi.  Da biste riješili taj problem, stvorite mjerača performansi u pokretanje zadatka ili premještanje neke pokretanje zadatka raditi na način OnStart nakon stvaranja mjerača performansi.

Poduzmite sljedeće korake da biste stvorili jednostavan prilagođene performanse counter pod nazivom "\MyCustomCounterCategory\MyButton1Counter":

1. Otvorite datoteku definicije servisa (CSDEF) za svoju aplikaciju.
2. Dodajte Runtime element u WebRole ili WorkerRole element za izvršavanje s dodatnim ovlastima:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Spremite datoteku.
4. Otvorite datoteku Dijagnostika (diagnostics.wadcfg SDK 2.4, a ispod ili diagnostics.wadcfgx u SDK 2,5 i iznad) i dodajte sljedeće na DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Spremite datoteku.
6. Stvaranje kategoriju mjerača prilagođene performansi u način OnStart vaša uloga prije pozivanje osnovni. OnStart. Sljedeći C# primjer stvara prilagođena kategorija, ako se još ne postoji:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Ažurirajte mjerača unutar vaše aplikacije. U sljedećem primjeru obnavlja brojač prilagođene performanse na Button1_Click događaja:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Spremite datoteku.  

Prilagođeni performanse podataka sada prikupljaju se Azure Dijagnostika monitor.

## <a name="step-3-query-performance-counter-data"></a>Korak 3: Brojač podataka o performansama upita

Kad je implementiran aplikacije i pokrenut monitor Dijagnostika će početi prikupljanje mjerača performansi i persisting tih podataka Azure za pohranu. Pomoću alata kao što je poslužitelj Explorer u Visual Studio, [Explorer Azure prostora za pohranu](http://azurestorageexplorer.codeplex.com/)ili [Upravitelj Dijagnostika Azure](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) po Cerebrata prikaz podataka mjerača o performansama u tablici WADPerformanceCountersTable. Možete i programatically upita servisa tablice pomoću [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md)ili [PHP](../storage/storage-php-how-to-use-table-storage.md).

Sljedeći C# primjer prikazuje jednostavnog upita na temelju tablice WADPerformanceCountersTable i sprema podatke Dijagnostika u CSV datoteku. Kada mjerača performansi spremaju se u CSV datoteku pomoću graphing mogućnosti u programu Microsoft Excel ili neki drugi alat vizualni prikaz podataka. Obavezno dodajte referenca Microsoft.WindowsAzure.Storage.dll, odnosno uvršteno na Azure SDK za .NET listopad 2012 i noviji. U sklopu je instaliran Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ direktorij %.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Mapirajte entiteti C# objekata pomoću prilagođenih predmete izvedene iz **TableEntity**. Sljedeći kod definira je entitet predmete koji predstavlja brojač performanse u tablici **WADPerformanceCountersTable** .


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Daljnji koraci
[Pregled dodatnih članaka na Azure Dijagnostika] (.. / azure diagnostics.md)
