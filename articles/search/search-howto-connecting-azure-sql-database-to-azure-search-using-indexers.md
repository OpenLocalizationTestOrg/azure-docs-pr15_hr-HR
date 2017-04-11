<properties 
    pageTitle="Povezivanje baze podataka Azure SQL Azure pretraživanje pomoću Indexers | Microsoft Azure | Indexers" 
    description="Saznajte kako za izvlačenje podataka iz baze podataka SQL Azure pomoću indexers indeks pretraživanja Azure." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/27/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Povezivanje baze podataka SQL Azure Azure pretraživanje pomoću indexers

Azure servisa za pretraživanje je servis za pretraživanje glavnom računalu oblaka koja olakšava omogućuju izvrstan pretraživanju. Prije nego što možete pretraživati, morate popuniti indeks pretraživanja Azure s podacima. Ako se podaci nalaze u baze podataka Azure SQL, novi **Indeksiranje pretraživanja Azure za baze podataka SQL Azure** (ili **Azure SQL indeksiranje** za kratki) možete automatizirati postupak indeksiranja. To znači da imate manje kod za pisanje i manje infrastrukturu za zanimaju.

U ovom se članku opisuje mehanika korištenja indexers, ali se opisuju značajke koje su dostupne s bazama podataka Azure SQL (na primjer, integrirano evidentiranja promjena). Azure pretraživanja podržava i drugih izvora podataka, kao što su Azure DocumentDB, blobova i spremište tablica. Želite li da biste vidjeli podrške za dodatne izvore podataka, pošaljite povratne informacije na [forum za povratne informacije Azure pretraživanja](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexers i izvora podataka

Možete postaviti i konfiguriranje korištenja programa Azure SQL indeksiranje: 

- Čarobnjak za uvoz podataka na [portal za Azure](https://portal.azure.com)
- Azure pretraživanje [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
- Azure pretraživanja [REST API-JA](http://go.microsoft.com/fwlink/p/?LinkID=528173)

U ovom se članku ćemo pomoću REST API-JA pokazuju kako stvarati i upravljati **indexers** i **izvora podataka**. 

**Izvor podataka** određuje podatke koje želite indeksirati vjerodajnice potrebne za pristup podacima i pravila koja učinkovito prepoznavanje promjene u podacima (novi, izmijenjeni ili izbrisani redaka). Je definiran kao neovisno resursa tako da ga može koristiti više indexers.

**Indeksiranje** je resurs koji povezuje se s cilj pretraživanja indeksi izvora podataka. Indeksiranje koristi se na sljedeće načine:
 
- Izvođenje jednokratnu kopiju podataka za popunjavanje indeksa.
- Ažuriranje indeksa promjenama u izvoru podataka na raspored.
- Pokrenite na zahtjev za ažuriranje indeksa prema potrebi. 

## <a name="when-to-use-azure-sql-indexer"></a>Kada se koristi za indeksiranje Azure SQL

Ovisno o nekoliko čimbenika koji se odnose na podatke, koristite Azure SQL indeksiranje možda ili nisu pogodni. Ako vaši podaci odgovara sljedeće preduvjete, možete koristiti Azure SQL indeksiranje: 

- Svi podaci dolazi iz jedne tablice ili prikazu
    - Ako se podaci raspršeni preko više tablica, možete stvoriti prikaz i koristiti taj prikaz s indeksiranje. Međutim, ako koristite prikaz, nećete moći koristiti otkrivanje promjena SQL Server integriran. Dodatne informacije potražite u članku [Ovaj odjeljak](#CaptureChangedRows). 
- Vrste podataka koje se koriste u izvoru podataka podržava indeksiranje. Podržane su Većina, ali ne i svih SQL vrste. Detalje potražite u članku [mapiranje vrsta podataka u pretraživanju Azure](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Ne morate uz u stvarnom vremenu ažuriranja indeksa kada redak promjene. 
    - Indeksiranje možete ponovno indeksirati tablice najviše svakih 5 minuta. Ako često promjene podataka i promjene morate biti vidljiv u indeksu u sekundama ili minutama jednu, preporučujemo da izravno korištenje [Azure API za indeks pretraživanja](https://msdn.microsoft.com/library/azure/dn798930.aspx) . 
- Ako imate veliki skup podataka i planiranje Pokreni komponente za indeksiranje rasporedu sheme zatražit učinkovito prepoznavanje promijenili (i izbrisati ako je primjenjivo) redaka. Dodatne informacije potražite u članku "Hvatanje promijeniti i izbrisati retke" ispod. 
- Veličina indeksirana polja u retku veća od maksimalne veličine indeksiranje zahtjev, što je 16 MB pretraživanja za Azure. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Stvaranje i korištenje indeksiranje Azure SQL

Prvo, stvorite izvor podataka: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Možete dobiti niz za povezivanje [Azure Classic Portal](https://portal.azure.com); korištenje na `ADO.NET connection string` mogućnost.

Zatim stvorite indeks pretraživanja Azure ciljne Ako još nemate jedno mjesto. Možete stvoriti indeks pomoću [portala za korisničko Sučelje](https://portal.azure.com) ili [Stvaranje indeksa API -JA](https://msdn.microsoft.com/library/azure/dn798941.aspx). Provjerite je li u shemi indeksa ciljne kompatibilne sa shemom izvorišnu tablicu – pogledajte [mapiranja između SQL i Azure vrste podataka pretraživanja](#TypeMapping).

Na kraju, stvorili indeksiranje dodjeljivanja naziva i pozivate indeks izvorišno i odredišno podataka učinite sljedeće:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Indeksiranje stvorena na taj način ne obuhvaća raspored. Automatski pokreće kada pri stvaranju. Pokrenite ga ponovno u bilo kojem trenutku pomoću zahtjeva za **pokretanje programa za indeksiranje** :

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Možete prilagoditi nekoliko aspekti komponente za indeksiranje ponašanja, kao što su veličina grupe i koliko dokumenata možete preskočiti prije izvođenja programa za indeksiranje ne uspijeva. Dodatne informacije potražite u članku [Stvaranje API za indeksiranje](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
Možda ćete morati Dopusti Azure servisa za povezivanje s bazom podataka. Upute za to potražite u članku [Povezivanje s Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) .

Praćenje statusa indeksiranje i izvođenja povijest (broj stavki indeksirana, neuspjeha itd.), koristite zahtjev za **Indeksiranje status** : 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Odgovor trebao bi izgledati otprilike ovako: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Izvršavanje povijest sadrži do 50 nedavno dovršene izvršavanja koje se sortiraju obrnutim kronološkim redoslijedom (tako da se najnovija izvođenja dolazi prvo u odgovoru). Dodatne informacije o odgovor pronaći ćete u [Dobiti Status indeksiranje](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Pokreni indexers prema rasporedu

Možete urediti i komponente za indeksiranje da biste pokrenuli povremeno prema rasporedu. Da biste to učinili, dodajte svojstvo **rasporeda** pri stvaranju ili ažuriranje indeksiranje. U sljedećem primjeru pokazuje STAVI zahtjev da biste ažurirali indeksiranje:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Potreban je parametar **interval** . Interval se odnosi na vrijeme između početka dva uzastopna indeksiranje izvršavanja. Najmanji dopušteni interval je pet minuta; na najdulje je jedan dan. Moraju biti oblikovani kao vrijednost "dayTimeDuration" XSD (ograničeni podskup vrijednost [ISO 8601 trajanje](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Obrazac za to je: `P(nD)(T(nH)(nM))`. Primjeri: `PT15M` svakih 15 minuta `PT2H` za svaku dva sata.

Neobavezni **startTime** označava kada treba početka zakazanog izvršavanja. Ako je izostavljen, koristi se trenutno vrijeme UTC-a. Ovaj put može biti u prošlosti – u kojem slučaj prvi izvršavanja zakazanog kao da se indeksiranje pokrenuta neprestano od na startTime.  

Istovremeno možete pokrenuti samo jedan izvođenja indeksiranje. Ako se indeksiranje izvodi pri njegova izvođenja, izvršavanja je odgođena dok sljedeće zakazano vrijeme.

Ćemo razmotriti primjer da bi sustav učinila više konkretnih. Pretpostavimo da ne možemo sljedeće raspored svaki sat konfigurirali: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Evo što se događa: 

1. Prvi izvođenja indeksiranje se pokreće pri ili oko ožujak 1, 2015 12.00 podne UTC-A.
1. Pretpostavimo ovaj izvođenja vodi 20 minuta (ili bilo kojem trenutku manji od 1 sat).
1. Drugi izvođenja pokreće pri ili oko ožujak 1, 2015 1 12.00 podne 
1. Sada pretpostavimo da ova izvođenja potrebno više od jednog sata – na primjer, 70 minuta – tako da se dovrši oko 2 10 podne
1. To je sada 2 12.00 podne, vrijeme za treći izvršavanje da biste pokrenuli. No Budući da drugi izvođenja iz AM 1 i dalje izvodi treći izvođenja preskače. Treći izvođenja započinje AM 3

Možete dodajte, promijenite ili izbrišite plan za postojeće komponente za indeksiranje pomoću zahtjev za **Postavljanje programa za indeksiranje** . 

<a name="CaptureChangedRows">, /a >
## <a name="capturing-new-changed-and-deleted-rows"></a>Snimanje nove, mijenjati i izbrisati retke

Ako tablica ima više redaka, trebali biste koristiti pravilo za otkrivanje promjena podataka. Promjena otkrivanje omogućuje učinkovito dohvaćanje samo na nove ili promijenjene retke bez potrebe za ponovno indeksiranje cijelu tablicu.

### <a name="sql-integrated-change-tracking-policy"></a>SQL integriran pravila za evidentiranje promjena

Ako bazu podataka sustava SQL podržava [Evidentiranje promjena](https://msdn.microsoft.com/library/bb933875.aspx), preporučujemo korištenje **SQL integrirani promjena praćenja pravila**. Ovo je najučinkovitiji pravila. Uz to, omogućuje Azure pretraživanja da biste odredili izbrisane retke bez potrebe za dodavanje eksplicitnih "meki Izbriši" stupca u tablicu.

Integrirano evidentiranja promjena je podržano počevši od sljedeće verzije baze podataka SQL Server:
 
- SQL Server 2008 R2 ili novijim, ako koristite SQL Server Azure VMs. 
- Azure SQL baza podataka V12, ako koristite baze podataka SQL Azure.

Kada pomoću SQL integriran promjena praćenja pravila, navedite pravilo za otkrivanje brisanja zasebne podatke – to pravilo ima ugrađenu podršku za označavanje izbrisati retke.

Ovo pravilo može se koristiti samo s tablicama; nije moguće koristiti s prikazima. Morate omogućiti za tablicu koristite mogli koristiti ovaj pravilnik evidentiranje promjena. Potražite u članku [Omogućivanje i onemogućivanje evidentiranje promjena](https://msdn.microsoft.com/library/bb964713.aspx) upute. 

Da biste koristili ovo pravilo, stvaranje ili ažuriranje izvora podataka ovako:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

<a name="HighWaterMarkPolicy"></a>
### <a name="high-water-mark-change-detection-policy"></a>Otkrivanje promjena visoke vode Označi pravila

Dok se preporučuje se pravila SQL integrirani evidentiranje promjena, ga možete se koristiti samo s tablicama, prikazima ne. Ako koristite prikaz, preporučujemo da koristite oznaku visoke vode pravila. Ovo pravilo se može se koristiti ako tablicu ili prikaz sadrži stupac koji zadovoljava sljedeće kriterije:

- Umeće sve navesti vrijednost u stupcu. 
- Sva ažuriranja stavke i promijenite vrijednost stupca. 
- Povećava vrijednosti u ovom stupcu sa svim promjenama.
- Upiti s sljedeće mjesto i uvjeta SORTIRAJ po možete izvršiti učinkovito: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

Ako, na primjer, stupca indeksiranih **rowversion** je idealna prijedlog za oznaku visoke vode stupac. Da biste koristili ovo pravilo, stvaranje ili ažuriranje izvora podataka ovako: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

> [AZURE.WARNING] Ako u izvorišnu tablicu indeksa oznaku visoke vode stupac, upiti koji se koriste za indeksiranje SQL možda istek vremena. Posebno u `ORDER BY [High Water Mark Column]` uvjet zahtijeva indeksa da biste pokrenuli učinkovito kada tablica sadrži više redaka. 

Ako naiđete na pogreške vremensko ograničenje, možete koristiti u `queryTimeout` indeksiranje postavke konfiguracije da biste postavili istek upita na vrijednost veću od 5 minuta vremensko ograničenje zadani. Ako, na primjer, da biste postavili vrijeme čekanja 10 minuta, stvorite ili ažuriranje indeksiranje u konfiguraciji sljedeće: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Možete i onemogućiti u `ORDER BY [High Water Mark Column]` uvjet. No to nije preporučeno jer se izvršavanje indeksiranje prekine po pogrešku, indeksiranje postojanja da ponovno obradi sve retke ako se kasnije - izvodi čak i ako indeksiranje već obrađen gotovo svi reci prema vremenu prekinuto je. Da biste onemogućili u `ORDER BY` uvjet WHERE, koristite na `disableOrderByHighWaterMarkColumn` postavka u definiciji indeksiranje:  

    {
     ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Meki brisanje otkrivanje stupaca brisanje pravila

Kada se reci se brišu iz izvorišne tablice, vjerojatno koju želite izbrisati retke iz indeks pretraživanja. Ako koristite evidentiranje pravila promjena SQL integriran, to je snimanja brigu o umjesto vas. Međutim, promjena visoke vode Označi pravila za praćenje ne vam pomoći s izbrisane retke. Što učiniti? 

Ako retke fizički uklanjaju se iz tablice, pretraživanje Azure je nije moguće izvesti prisutnosti zapise koji više ne postoji.  Međutim, "mekih Izbriši" tehniku možete koristiti da biste logično bez ih ukloniti iz tablice izbrisali retke. Dodajte neki stupac vaše redaka tablice ili prikazu i Označi kao što je izbrisati pomoću tom stupcu.

Prilikom korištenja mekih Izbriši postupak, možete odrediti na mekih brisanje pravila na sljedeći način pri stvaranju ili ažuriranje izvora podataka: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

**SoftDeleteMarkerValue** mora biti niz – koristiti prikaz niza stvarna vrijednost. Na primjer, ako imate stupca cijeli broj u kojem su izbrisane retke označeni vrijednost 1, pomoću `"1"`. Ako imate stupac BITNE gdje su izbrisane retke označeni Booleova vrijednost true, pomoću `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapiranja između SQL vrste podataka i vrste podataka za pretraživanje Azure

|Vrsta podataka za SQL | Indeks ciljne dopuštene vrste polja |Bilješke 
|------|-----|----|
|bitne|Edm.Boolean, Edm.String| |
|INT, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| bigint | Edm.Int64, Edm.String | |
| realni, plutati |Edm.Double, Edm.String | |
| smallmoney, novac decimalni broj | Edm.String| Azure pretraživanje ne podržava vrpce decimalni vrste Edm.Double jer je to ćete izgubiti preciznosti |
| CHAR, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|SQL niz može se koristiti za popunjavanje polja Collection(Edm.String) ako niz predstavlja JSON polja nizova:`["red", "white", "blue"]` | 
|smalldatetime, datumom i vremenom, datetime2, datum, datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|Zemljopis | Edm.GeographyPoint | Podržani su samo Zemljopis instanci vrste TOČKU s 4326 SRID (što je zadana postavka) | | 
|ROWVERSION| N/D |Redak verziju stupaca nije moguće spremiti u indeks pretraživanja, ali se može koristiti za evidentiranje promjena | |
| vrijeme, vremenski raspon, binarni, varbinary, slike, xml, geometriju, vrste CLR | N/D |Nije podržano |

## <a name="configuration-settings"></a>Konfiguriranje postavki

Indeksiranje SQL izlaže nekoliko konfiguracijske postavke: 

|Postavka |Vrsta podataka | Svrha | Zadana vrijednost |
|--------|----------|---------|---------------|
| queryTimeout | niz | Postavlja Prekoračenje vremena za izvršavanje SQL upita | pet minuta ("00: 05:00") |
| disableOrderByHighWaterMarkColumn | booleovom | Uzrokuje SQL upit koji se koristi visoke vode Označi pravila za izostavljanje uvjet ORDER BY. U odjeljku [pravila za oznaku visoke vodu](#HighWaterMarkPolicy) | FALSE | 

Postavke koje se koriste u na `parameters.configuration` objekta u definiciji indeksiranje. Ako, na primjer, da biste postavili istek upita na 10 minuta, stvaranje ili ažuriranje indeksiranje u konfiguraciji sljedeće: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Najčešća pitanja

**Q:** Mogu li koristiti indeksiranje Azure SQL s bazama podataka SQL sustavom IaaS VMs u Azure?

O: da. Međutim, morate dopustiti servisa za pretraživanje da biste se povezali s bazom podataka. Dodatne informacije potražite u članku [Konfiguriranje veze s indeksiranje pretraživanja Azure sa sustavom SQL Server na programa Azure VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).


**Q:** Možete koristiti za indeksiranje Azure SQL s izvodi lokalne baze podataka SQL? 

A: možemo ne preporučujemo da ili podržava, kao što je to je potrebna otvaranje baze podataka za internetski promet. 

**Q:** Mogu li koristiti za indeksiranje Azure SQL s bazama podataka koji nisu SQL Server u IaaS sustavom Azure? 

A: Ne podržavamo ovaj scenarij jer nismo testirali za indeksiranje s nijedna baza podataka koji nisu SQL Server.  

**Q:** Je li moguće stvoriti više indexers koji se izvode na raspored? 

O: da. Međutim, samo jedan program za indeksiranje mogu se izvoditi na jedan čvor odjednom. Ako trebate više indexers izvodi istovremeno, razmislite o skaliranje gore servisa za pretraživanje da biste više jedinica za pretraživanje. 

**Q:** Pokretanje indeksiranje utječe na Moje radno opterećenje upit? 

O: da. Indeksiranje izvodi na jedan od čvorove u servis za pretraživanje, a resurse za taj čvor se zajednički koriste između indeksiranja i posluživanje promet upita i zahtjeve API-JA. Ako pokrenuli intenzivno radnih opterećenja indeksiranja i upita i pojaviti visoke rata 503 pogrešaka i povećavati vremena odgovor, preporučujemo da skaliranje gore servisa za pretraživanje.
