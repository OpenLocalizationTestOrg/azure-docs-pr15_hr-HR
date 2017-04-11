<properties 
    pageTitle="Particija i promjena veličine u Azure DocumentDB | Microsoft Azure"      
    description="Saznajte kako stvaranje particija funkcioniranja u Azure DocumentDB, kako konfigurirati particija i particija tipke i kako odabrati desnom particija ključ za svoju aplikaciju."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Particija i promjena veličine u Azure DocumentDB
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) osmišljene su da biste lakše postigli brzo, predvidljivi performanse i skaliranje jednostavno uz aplikacije kao što je poveća. Ovaj članak sadrži pregled načina za stvaranje particija funkcioniranja u DocumentDB i opisuje kako konfigurirati DocumentDB zbirke učinkovito skaliranja aplikacija.

Kad pročitate članak će se moći odgovaraju na sljedeća pitanja:   

- Kako stvaranje particija nedovršenu Azure DocumentDB?
- Kako konfigurirati particija u DocumentDB
- Što su particije tipke i kako odaberite tipku desnom particije na moje aplikacije?

Da biste započeli s kodom, preuzmite projekta s [Uzorak upravljački program za DocumentDB testiranje performansi](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

## <a name="partitioning-in-documentdb"></a>Particija u DocumentDB

U DocumentDB, možete spremiti i sheme manje JSON dokumenata s puta redoslijed-od-od milisekundi odgovor na bilo kojem mjerilo za upite. DocumentDB nudi spremnika za spremanje podataka koji se naziva **zbirke**. Zbirke logičke resursa i mogu obuhvaćati fizičke particije ili poslužiteljima. Broj particije ovisi o DocumentDB na temelju veličine prostora za pohranu i dodijeljenu propusnost zbirke. Svaki particije u DocumentDB sadrži fiksni iznos SSD sigurnosno prostora za pohranu pridružen pa je replicirati za visoke dostupnosti. Upravljanje particija potpuno upravlja Azure DocumentDB, a morate složene kod ili upravljali vaše particije. DocumentDB zbirke su **gotovo neograničeno** prostora za pohranu i propusnost. 

Particija je potpuno proziran u aplikaciji. DocumentDB podržava brzo čitanja i pisanja, SQL i LINQ upita, JavaScript koji se temelji transakcijskih logike, dosljednost razine i kontrola preciznije pristupa putem pozive REST API-JA za jednu zbirku resurs. Servis rukuje distribucija podataka preko particije i usmjeravanje upita zahtjevi za desni particije. 

Kako to funkcionira? Kada stvorite zbirku u DocumentDB, primijetit ćete da postoji **particija ključa** konfiguracije vrijednost koje možete odrediti. Ovo je svojstvo JSON (ili put) unutar vaše dokumente koje mogu koristiti DocumentDB za raspodjelu podataka između više poslužitelja ili particije. DocumentDB će se vrijednost ključa particija raspršivanja i koristiti hashed rezultat da biste odredili particije u kojem će biti pohranjen dokument JSON. Sve dokumente s istim ključem particija će se spremiti u istom particije. 

Na primjer, razmotrite aplikacije koja se sprema podatke o zaposlenicima i njihovim odjelima DocumentDB. Pogledajmo odaberite `"department"` kao particija ključa svojstvo, da biste skalirali podataka i odjel. Svaki dokument u DocumentDB mora sadržavati je obavezna `"id"` svojstvo koje se mora biti jedinstvena za svaki dokument s istom particija vrijednost ključa, npr. `"Marketing`". Svaki dokument sprema u zbirku mora imati jedinstveni kombinacije particija ključ i ID-a, npr. `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, i `{ "Department": "Sales", "id": "0001" }`. Drugim riječima, složene svojstvo (particija ključ, ID-ja) je primarni ključ za vašu zbirku.

### <a name="partition-keys"></a>Strelicama partition
Odabir tipku particija je važno odluke koje morate učiniti u trenutku dizajniranja. Morate odabrati naziv svojstva JSON koje nudi širok raspon vrijednosti i vjerojatno će imati ravnomjerno distributed uzoraka programa access. Tipku particija je naveden kao put JSON, npr. `/department` predstavlja odjela za svojstvo. 

Sljedeća tablica prikazuje primjere ključne definicije particija i vrijednosti JSON koje odgovaraju svakom.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Put ključa partition</strong></p></td>
            <td valign="top"><p><strong>Opis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/department</p></td>
            <td valign="top"><p>Odgovara vrijednost JSON doc.department gdje se dokument nalazi u dokumentu.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ svojstava/naziv</p></td>
            <td valign="top"><p>Odgovara vrijednost JSON doc.properties.name gdje se nalazi dokument dokumenta (ugniježđene svojstvo).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Odgovara vrijednosti JSON doc.id (id i particija ključ su iste svojstvo).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "naziv odjela"</p></td>
            <td valign="top"><p>Odgovara vrijednosti JSON dokumenta ["naziv odjela"] gdje se dokument nalazi u dokumentu.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Vidjet ćete da sintaksa za put ključa particija je slična specifikacija puta za indeksiranje pravila putove ključne razlike koji odgovara put svojstvo umjesto vrijednost, odnosno postoji bez zamjenske na kraju. Na primjer, želite navedete/odjel /? Da biste indeksirati vrijednosti u odjeljku odjela, ali odredite /department kao definiciju ključa particije. Put ključa particija implicitno indeksirana, a ne možete izuzeti iz indeksiranja pomoću indeksiranja nadjačavanja pravila.

Pogledajmo kako odabir particija ključ utječe na performanse aplikacije.

### <a name="partitioning-and-provisioned-throughput"></a>Stvaranje particija i dodijeljenu propusnost
DocumentDB namijenjen je predvidljivi performansi. Kada stvorite zbirku, rezervirati propusnost pomoću ** [jedinice zahtjev](documentdb-request-units.md) (Pravi) u sekundi**. Svaki zahtjev je dodijeljen naknade za zahtjev za jedinicu koju je proporcionalno količinu sistemske resurse kao što su procesora i IO troše postupak. Čitanje dokumenta 1 kB s dosljednost sesije troši 1 jedinica zahtjev. Čitanje je 1 Pravi bez obzira na broj stavki koje se pohranjuju ili broj istovremeni zahtjevi izvodi istovremeno. Veće dokumenata potreban veći jedinice zahtjev ovisno o veličini. Ako znate veličine vaše entiteti i broj čitanja morate podrške za aplikaciju, možete Dodjela točan iznos propusnost potrebna za aplikaciju je potrebna za čitanje. 

Kada DocumentDB sprema dokumente, ga distribuira ih ravnomjerno među particije na temelju vrijednost ključa particije. Propusnost i ravnomjerno distributed među dostupni partitions odnosno propusnost po particija = (Ukupna propusnost po zbirci) / (broj razdjeljivanja). 

>[AZURE.NOTE] Da biste postigli punu propusnost zbirke morate odabrati particija ključ koji vam omogućuje da ravnomjerno raspodijeliti zahtjeva za jednu od brojnih vrijednosti ključa distinct particije.

## <a name="single-partition-and-partitioned-collections"></a>Jedan particija i particioniranom zbirki
DocumentDB podržava stvaranja i jednom i particioniranom zbirke. 

- **Partitioned zbirke** možete protežu na više particija i vrlo veliku količinu prostora za pohranu i protok podržava. Morate navesti ključ particija zbirke.
- **Jednom zbirke** imati donjem mogućnosti cijena i mogućnost upita, a zatim izvedite transakcije preko svih zbirke podataka. Imaju skalabilnost i pohranu ograničenja jedna particija. Ne morate navesti ključ particija za te zbirke. 

![Particioniranom zbirke u DocumentDB][2] 

Za scenarije koje je potrebno velike količine prostora za pohranu ili propusnost, jedan particija zbirke su dobro rješenje. Imajte na umu da jednom zbirke imaju skalabilnost i ograničenja prostora za pohranu od jedna particija, odnosno do 10 GB prostora za pohranu i do 10 000 jedinice zahtjev sekundi. 

Particioniranom zbirke podržavaju vrlo veliku količinu prostora za pohranu i propusnost. Ponuda za zadani no konfiguriran za pohranu veličine do 250 GB prostora za pohranu i promjena veličine do 250.000 jedinice zahtjev sekundi. Ako vam je potrebna veća prostora za pohranu ili propusnost po zbirci, obratite se [Podršci za Azure](documentdb-increase-limits.md) da bi se ti povećana za vaš račun.

U sljedećoj su tablici navedeni razlike u radu s jednom particija i particioniranom zbirke:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Jedan particija zbirke</strong></p></td>
            <td valign="top"><p><strong>Particioniranom zbirke</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Ključ partition</p></td>
            <td valign="top"><p>Ništa</p></td>
            <td valign="top"><p>Obavezno</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Primarni ključ za dokument</p></td>
            <td valign="top"><p>"id"</p></td>
            <td valign="top"><p>Složena ključ &lt;particija ključ&gt; i "id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimalna prostora za pohranu</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maksimalna veličina spremnika</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Neograničeno (250 GB po zadanom)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimalna propusnost</p></td>
            <td valign="top"><p>400 zahtjev jedinice u sekundi</p></td>
            <td valign="top"><p>10 000 zahtjev jedinice u sekundi</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maksimalna propusnost</p></td>
            <td valign="top"><p>10 000 zahtjev jedinice u sekundi</p></td>
            <td valign="top"><p>Neograničeno (sekundi prema zadanim postavkama 250.000 zahtjev jedinice)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API verzija</p></td>
            <td valign="top"><p>Sve</p></td>
            <td valign="top"><p>API 2015-12-16 i noviji</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Rad s na SDK-ovi

Azure DocumentDB dodaje podršku za automatsko particija s [REST API -JA verzija 2015-12-16](https://msdn.microsoft.com/library/azure/dn781481.aspx). Da biste stvorili particioniranom zbirke, morate preuzeti SDK verzije 1.6.0 ili novija verzija u jednom od podržane platforme SDK (.NET, Node.js, jezika Java, Python). 

### <a name="creating-partitioned-collections"></a>Stvaranje particioniranom zbirki

Sljedeći primjer prikazuje .NET isječak da biste stvorili zbirke za pohranu uređaj telemetrijskih podataka od 20 000 jedinica zahtjev sekundi propusnost. SDK postavlja vrijednost OfferThroughput (koji shodno postavlja na `x-ms-offer-throughput` zahtjev zaglavlja u REST API-JA). Ovdje možemo postaviti na `/deviceId` kao ključ particije. Odabir particija ključ je spremljena zajedno s ostalim zbirke metapodataka kao što su ime i indeksiranja pravila.

Primjer, ne možemo izdvojiti `deviceId` jer smo znati da (a) jer postoji velik broj uređaja, zapisivanja može biti raspodijeliti particije ravnomjerno i dopustite nam da biste skaliranje baze podataka radi ingest pretraživanje velikog količine podataka te (b) mnoge zahtjeva za kao što su dohvaćanje najnovijih čitanje uređaja imaju ograničen prikaz samo jedan deviceId i biti dohvaćeni iz jedna particija.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Da biste stvorili particioniranom zbirke, morate odrediti vrijednost propusnost > 10 000 zahtjev jedinica sekundi. Budući da je propusnost u višestrukim grafikonima od 100, to mora biti 10,100 ili noviji.

Ovaj postupak omogućuje REST API poziva DocumentDB i servis će Dodjela broj particije na temelju tražene propusnost. Možete promijeniti propusnost zbirkom web kao performansi potrebama poslovanja. Dodatne informacije potražite u članku [Performanse razine](documentdb-performance-levels.md) .

### <a name="reading-and-writing-documents"></a>Čitanje i pisanje dokumenata

Sada ćemo podataka umetnuli DocumentDB. Evo ogledne predmete koje sadrže uređaj za čitanje i poziv CreateDocumentAsync da biste umetnuli novi uređaj za čitanje u zbirku.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Recimo pročitajte dokument tako da je ključ particija i ID-a, ažurirati, a zatim kao završnom koraku izbrišite ključ particija i id. Bilješke na čitanja obuhvaćaju je PartitionKey vrijednost (odgovarajuće da biste na `x-ms-documentdb-partitionkey` zahtjev zaglavlja u REST API-JA).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Slanje upita particioniranom zbirki

Kada je upit podatke u particioniranom zbirke, DocumentDB automatski usmjerava upit particije odgovara particija ključa vrijednosti navedene u filtru (ako ih ima). Ovaj upit, primjerice, biti usmjeren particije koja sadrži tipku particija "XMS 0001".

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Sljedeći upit na tipku particija (DeviceId) imate filtra i fanned za sve particije na kojem se izvršava na temelju na particija indeksa. Bilješke koje ćete morati odrediti na EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` u REST API-JA) da bi SDK za izvršavanje upita preko particije.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Izvršavanje paralelno upita

DocumentDB SDK-ovi 1.9.0 i iznad paralelno upita izvođenja mogućnosti podrške, koje omogućuju izvođenje niske latencije upite odabiranja particioniranom zbirke, čak i kad je koje su im potrebne za dodir mnogo particije. Na primjer, sljedeći upit je konfiguriran za izvođenje paralelno preko particije.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Izvršavanje upita paralelno možete upravljati tako da usklađivanje sljedećih parametara:

- Postavljanjem `MaxDegreeOfParallelism`, možete kontrolirati stupanj parallelism odnosno maksimalni broj istodobno mrežne veze u zbirci particije. Ako to postavite-1, stupanj parallelism upravlja SDK-a. Ako u `MaxDegreeOfParallelism` nije navedeno ili postavljeno na 0, što je zadana vrijednost, će biti jedan mrežnu vezu da biste u zbirku particije.
- Postavljanjem `MaxBufferedItemCount`, možete isključiti upita klijent i Latencija strani korištenje memorije poslovanju. Ako izostavite taj parametar ili postaviti-1, broj stavki u međuspremniku tijekom izvršavanja upita paralelno upravlja SDK-a.

Dobili isti stanja zbirke web-paralelno upita dat će rezultate poredak isti kao serijski izvršavanja. Prilikom izvršavanja upita više particija koji uključuje sortiranje (ORDER BY i/ili VRH), DocumentDB SDK problemi upita paralelno preko particije i spaja djelomično sortirani rezultata na strani klijenta za dobivanje globalno uređeni rezultata.

### <a name="executing-stored-procedures"></a>Izvođenje pohranjene procedure

Možete i izvršavanja atomske transakcije protiv dokumenata s istom ID uređaja, npr. Ako ste održavanje zbrajanja ili najnovije stanje uređaja u jedan dokument. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

U sljedećem odjeljku, ne možemo pogledajte kako možete premjestiti particioniranom zbirke s jednom zbirke.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Migracija s jednom particioniranom zbirkama
Kada aplikacije korištenjem jednom zbirke mora veću propusnost (> 10 000 Pravi/s) ili veći pohrana podataka (> 10GB), [DocumentDB alata za migraciju podataka](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) možete koristiti da biste migrirali podatke iz zbirke jednom particioniranom zbirke. 

Da biste migrirali iz zbirke jednom particioniranom zbirku

1. Izvoz podataka iz zbirke jednom u JSON. Potražite u članku [Izvoz u datoteku JSON](documentdb-import-data.md#export-to-json-file) dodatne detalje.
2. Uvoz podataka u particioniranom zbirku stvorene pomoću definiciju ključne particija i više od 10 000 zahtjev jedinica po drugi propusnost kao što je prikazano u primjeru u nastavku. Dodatne informacije potražite u članku [Uvoz DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) .

![Prijenos podataka u zbirku Partitioned u DocumentDB][3]  

>[AZURE.TIP] Brže vremena uvoza, razmislite o povećava broj od paralelno zahtjeve 100 ili noviji da biste iskoristili veću propusnost dostupne za particioniranom zbirke. 

Nakon što smo obavljenog osnove, Pogledajmo nekoliko važnih dizajn pitanja vezana uz kada radite s tipkama particije u DocumentDB.

## <a name="designing-for-partitioning"></a>Dizajniranje za particija
Odabir tipku particija je važno odluke koje morate učiniti u trenutku dizajniranja. U ovom se odjeljku opisuju neke od tradeoffs uvrštene u odabir particija ključ za svoju zbirku.

### <a name="partition-key-as-the-transaction-boundary"></a>Ključ particije kao granicu transakcije
Odabir particija ključ trebali biste uskladiti potrebe za omogućivanje korištenja transakcije u odnosu na zahtjev za vaše entiteti raspodijelite više particija tipki da biste bili sigurni skalabilni rješenja. Na jedan ekstremne nije moguće postaviti isti particija ključ za sve dokumente, ali to može vam ograničiti skalabilnost rješenje. U, može dodijeliti jedinstveni particija ključ za svaki dokument koji će biti vrlo skalabilni, ali želite onemogućiti pomoću unakrsni dokument transakcije putem pohranjene procedure i okidača. Ključa particija idealna je jedan koji vam omogućuje korištenje učinkovitog upita i ima li dovoljno cardinality da biste bili sigurni da je rješenje skalabilni. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Izbjegavanje grla prostora za pohranu i performanse 
Također važno je da biste odabrali svojstvo koji omogućuje upisivanje se raspodijeliti broj različitih vrijednosti. Zahtjevi za isti particija ključa ne smije biti propusnost jedna particija i će ograničio vrijeme. Stoga važno je da biste odabrali particija ključ koji kao rezultat **"slikovnim"** unutar vaše aplikacije. Veličina ukupan prostor za pohranu za dokumente s istim ključem particija možete također nije veća od 10 GB u prostor za pohranu. 

### <a name="examples-of-good-partition-keys"></a>Primjeri dobar particija ključeva
Evo nekoliko primjera kako odaberite tipku particije aplikacije:

* Ako ste implementacije u pozadinskog korisničkog profila, korisnički ID je dobar izbor za ključ particije.
* Ako ste pohranili IoT podataka – primjerice stanje uređaja, ID uređaja je dobar izbor za ključ particije.
* Ako koristite DocumentDB za zapisivanje vremenski niz podataka, ID naziv glavnog računala ili postupak je dobar izbor za ključ particije.
* Ako imate više klijentu arhitektura, ID klijenta je dobar izbor za ključ particije.

Imajte na umu da u nekim slučajevima korištenje (kao što je web-mjesto IoT i u okvir za korisničke profile prethodno opisan) tipku particija možda jednaki id (dokument ključ). U drugi kao što je niz podataka vremena možda particija ključ koji se razlikuje od id.

### <a name="partitioning-and-loggingtime-series-data"></a>Stvaranje particija i vremenski/zapisivanje-niz podataka
Jedna od najčešće koristi slučajeva od DocumentDB je za bilježenje i telemetriju. Važno je da odaberite ključa dobar particija jer ćete morati čitanje/pisanje veliku količine podataka. Odabir će ovise o tome na čitanje i pisanje stope i vrste upita očekujete da biste pokrenuli. Evo nekoliko savjeta o tome da biste odabrali ključ dobro particije.

- Ako slučaj koristi obuhvaća small stopa piše acculumating dugo vremenskom razdoblju i datuma potrebe za upit raspona vremenske oznake i ostale filtre, primjerice pomoću skupne vrijednosti od vremenska oznaka ključa particija je dobro pristup. To vam omogućuje upit putem sve podatke za datum iz jedna particija. 
- Ako je vaš radno opterećenje Tamni pisanje koja je obično uobičajene, poslužite se particija ključ koji se ne temelji na vremenske oznake tako da se DocumentDB možete razmještavanje pisanja preko broj particije. Naziv glavnog računala, ID procesa, ID aktivnosti ili neki drugi svojstvo s visoke cardinality Evo dobar izbor. 
- Treći pristup je hibridnog nešto kojoj imate više zbirki jedan za svaki dan/mjesec i tipku particija je zrnastog svojstvo kao što su naziv glavnog računala. Ima prednosti koje možete postaviti različite performanse razine koji se temelji na prozor vrijeme, npr zbirke za trenutni mjesec je nudi veću propusnost jer služi čitanja i pisanja, dok prethodnih mjeseci s donje propusnost jer oni samo služe čitanja.

### <a name="partitioning-and-multi-tenancy"></a>Particija i više tenancy
Ako su implementacijom više klijentske aplikacije pomoću DocumentDB, postoje dva glavna uzoraka za implementaciju tenancy s DocumentDB – jedna particija ključ po klijentu i jednu zbirku po klijentu. Evo argumente za i protiv za svaki unos:

* Jedna particija tipka po klijentu: U ovom modelu klijenata su collocated unutar jedne zbirke. No upite i umeće za dokumente iz jednog klijenta može izvoditi na temelju jedna particija. Logika transakcijskih mogli implementirati i preko sve dokumente unutar klijentom. Jer više klijenata za zajedničko korištenje zbirke, uštedjet ćete prostor za pohranu i propusnost troškove okupljanje resursi za klijenata unutar jedne zbirke umjesto dodjeljivanje dodatni headroom za svaki klijent. U drawback je su performanse odvajanja po klijentu. Povećava performanse/propusnost odnose na cijelu zbirku Dodavanje veze vanjskih ciljano povećava za klijenata.
* Jednu zbirku po klijentu: svaki klijent ima vlastitu zbirke. U ovom modelu možete rezervirati performanse po klijentu. Ovaj model je DocumentDB na novi potrošnje temelji cijene model, više učinkovit za više klijentske aplikacije s malim brojem od drugih korisnika.

Možete koristiti i pristup kombinacije/tiered collocates klijenata za male i prenosi veće klijenata u vlastite zbirke.

## <a name="next-steps"></a>Daljnji koraci
U ovom se članku smo ste opisano kako stvaranje particija funkcioniranja u Azure DocumentDB kako stvarati particioniranom zbirke te kako možete odabrati dobar particija ključ za svoju aplikaciju. 

-   Izvođenje promjena veličine i performanse testiranjem DocumentDB. Potražite u članku [performanse i skaliranje testiranjem Azure DocumentDB](documentdb-performance-testing.md) za uzorka.
-   Početak rada kodiranje [SDK-ovi](documentdb-sdk-dotnet.md) ili [REST API -JA](https://msdn.microsoft.com/library/azure/dn781481.aspx)
-   Dodatne informacije o [dodijeljenu propusnost u DocumentDB](documentdb-performance-levels.md)
-   Ako želite da biste prilagodili način aplikacije izvodi particija, priključite vlastite klijentsko stvaranje particija implementacije. Potražite u članku [klijentsko particija podrška](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
