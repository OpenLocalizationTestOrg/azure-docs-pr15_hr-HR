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
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Povezivanje baze podataka SQL Azure Azure pretraživanje pomoću indexers

Azure servisa za pretraživanje je servis za pretraživanje glavnom računalu oblaka koja olakšava omogućuju izvrstan pretraživanju. Prije nego što možete pretraživati, morate popuniti indeks pretraživanja Azure s podacima. Ako se podaci nalaze u baze podataka Azure SQL, novi **Indeksiranje pretraživanja Azure za baze podataka SQL Azure** (ili **Azure SQL indeksiranje** za kratki) u pretraživanju Azure možete automatizirati postupak indeksiranja. To znači da imate manje kod za pisanje i manje infrastrukturu za zanimaju.

Trenutno indexers funkcioniraju samo Azure SQL baze podataka, SQL Server Azure VMs i [Azure DocumentDB](../documentdb/documentdb-search-indexer.md). U ovom se članku smo ćete fokus na indexers koji rade s bazom podataka SQL Azure. Želite li da biste vidjeli podršci za dodatne izvore podataka, unesite povratne informacije na [forum za povratne informacije Azure pretraživanja](https://feedback.azure.com/forums/263029-azure-search/).

U ovom se članku obrađuje mehanika korištenja indexers, ali ne možemo ćete i naniže u značajkama i ponašanja dostupne samo s bazama podataka SQL (na primjer, integrirano evidentiranja promjena).

## <a name="indexers-and-data-sources"></a>Indexers i izvora podataka

Da biste postavili i konfiguriranje Azure SQL indeksiranje, možete nazvati [Azure pretraživanje REST API -JA](http://go.microsoft.com/fwlink/p/?LinkID=528173) za stvaranje i upravljanje **indexers** i **izvore podataka**. 

[Indeksiranje klase](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) u [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)ili čarobnjaka za uvoz podataka možete koristiti i [Azure portal](https://portal.azure.com) za stvaranje i planiranje indeksiranje.

**Izvor podataka** određuje podatke koje želite indeksirati vjerodajnice potrebne za pristup podacima i pravila koja omogućuju pretraživanje Azure učinkovito prepoznavanje promjene u podacima (Novo, promijenili ili izbrisali retke). Je definiran kao neovisno resursa tako da ga može koristiti više indexers.

**Indeksiranje** je resurs koji povezuje se s cilj pretraživanja indeksi izvora podataka. Indeksiranje koristi se na sljedeće načine:
 
- Izvođenje jednokratnu kopiju podataka za popunjavanje indeksa.
- Ažuriranje indeksa promjenama u izvoru podataka na raspored.
- Pokrenite na zahtjev za ažuriranje indeksa prema potrebi. 

## <a name="when-to-use-azure-sql-indexer"></a>Kada se koristi za indeksiranje Azure SQL

Ovisno o nekoliko čimbenika koji se odnose na podatke, koristite Azure SQL indeksiranje možda ili nisu pogodni. Ako vaši podaci odgovara sljedeće preduvjete, možete koristiti Azure SQL indeksiranje: 

- Svi podaci dolazi iz jedne tablice ili prikazu
    - Ako se podaci raspršeni preko više tablica, možete stvoriti prikaz i koristiti taj prikaz s indeksiranje. Međutim, imajte na umu da ako koristite prikaz, koje nećete moći koristiti otkrivanje promjena SQL Server integriran. U ovom se odjeljku Dodatne informacije potražite u članku. 
- Vrste podataka koje se koriste u izvoru podataka podržava indeksiranje. Većina, ali ne i svih programa SQL vrste podržane. Detalje potražite u članku [mapiranje vrsta podataka u pretraživanju Azure](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
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

Zatim stvorite indeks pretraživanja Azure ciljne Ako još nemate jedno mjesto. To možete učiniti s [portala korisničkog Sučelja](https://portal.azure.com) ili pomoću [API za stvaranje indeksa](https://msdn.microsoft.com/library/azure/dn798941.aspx).  Provjerite je li u shemi indeksa ciljne kompatibilne sa shemom izvorišnu tablicu – pogledajte [mapiranja između SQL Azure pretraživanje podatke vrste i](#TypeMapping) detalje.

Na kraju, stvorili indeksiranje dodjeljivanja naziva i pozivate indeks izvorišno i odredišno podataka učinite sljedeće:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Indeksiranje stvorena na taj način ne obuhvaća raspored. Automatski pokrene jednom čim je stvoren. Pokrenite ga ponovno u bilo kojem trenutku pomoću zahtjeva za **pokretanje programa za indeksiranje** :

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Možete prilagoditi nekoliko aspekti komponente za indeksiranje ponašanja, kao što su veličina grupe i koliko dokumenata možete preskočiti prije izvođenja programa za indeksiranje će nije uspjelo. Dodatne informacije potražite u članku [Stvaranje API za indeksiranje](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
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

Možete urediti i komponente za indeksiranje da biste pokrenuli povremeno prema rasporedu. Da biste to učinili, jednostavno dodajte svojstvo **rasporeda** pri stvaranju ili ažuriranje indeksiranje. U sljedećem primjeru pokazuje STAVI zahtjev da biste ažurirali indeksiranje:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Potreban je parametar **interval** . Interval se odnosi na vrijeme između početka dva uzastopna indeksiranje izvršavanja. Najmanji dopušteni interval je pet minuta; na najdulje je jedan dan. Moraju biti oblikovani kao vrijednost "dayTimeDuration" XSD (ograničeni podskup vrijednost [ISO 8601 trajanje](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Obrazac za to je: `P(nD)(T(nH)(nM))`. Primjeri: `PT15M` svakih 15 minuta `PT2H` za svaku dva sata.

Neobavezni **startTime** označava kada treba početka zakazanog izvršavanja; Ako je izostavljen, koristit će se trenutno vrijeme UTC-a. Ovaj put može biti u prošlosti – u kojem slučaju prvog izvođenja se zakazati kao da se indeksiranje pokrenuta neprestano od na startTime.  

Izvršavanje samo jedan zadani program za indeksiranje mogu se izvoditi odjednom. Ako indeksiranje već se izvršavaju prilikom sljedeće bi trebao da biste započeli, izvršavanja je preskočeno te životopise u intervalu sljedeći, uz pretpostavku da nema indeksiranje zadataka izvodi.

Ćemo razmotriti primjer da bi sustav učinila više konkretnih. Pretpostavimo da ne možemo sljedeće raspored svaki sat konfigurirali: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Evo što se događa: 

1. Prvi izvođenja indeksiranje se pokreće pri ili oko ožujak 1, 2015 12.00 podne UTC-A.
1. Pretpostavimo ovaj izvođenja vodi 20 minuta (ili bilo kojem trenutku manji od 1 sat).
1. Drugi izvođenja pokreće pri ili oko ožujak 1, 2015 1 12.00 podne 
1. Sada pretpostavimo da ova izvođenja potrebno je više od jednog sata (to je potrebna veliki broj dokumenata za to zapravo pojavljivati, ali je korisno slici) – na primjer, minuta 70 – tako da se dovrši oko 2 10 podne
1. To je sada 2 12.00 podne, vrijeme za treći izvršavanje da biste pokrenuli. No Budući da drugi izvođenja iz AM 1 i dalje izvodi treći izvođenja preskače. Treći izvođenja započinje AM 3

Možete dodajte, promijenite ili izbrišite plan za postojeće komponente za indeksiranje pomoću zahtjev za **Postavljanje programa za indeksiranje** . 

## <a name="capturing-new-changed-and-deleted-rows"></a>Snimanje nove, mijenjati i izbrisati retke

Ako koristite raspored, a vaša tablica sadrži trivial koje nisu broj redaka, trebali biste koristiti pravila otkrivanja promjenu podataka, tako da se indeksiranje učinkovito možete dohvatiti samo na nove ili promijenjene retke bez potrebe za ponovno indeksiranje cijelu tablicu.

### <a name="sql-integrated-change-tracking-policy"></a>SQL integriran pravila za evidentiranje promjena

Ako bazu podataka sustava SQL podržava [Evidentiranje promjena](https://msdn.microsoft.com/library/bb933875.aspx), preporučujemo korištenje **SQL integrirani promjena praćenja pravila**. Ovo pravilo omogućuje najučinkovitiji evidentiranje promjena, a omogućuje i Azure pretraživanja da biste odredili izbrisane retke bez potrebe za dodavanje eksplicitnih "meki Izbriši" stupca u tablicu.

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

### <a name="high-water-mark-change-detection-policy"></a>Otkrivanje promjena visoke vode Označi pravila

Dok se preporučuje se pravila SQL integrirani evidentiranje promjena, nećete moći koristiti ako se podaci nalaze u prikazu ili pak koristite stariju verziju baze podataka Azure SQL. U tom slučaju, razmislite o korištenju pravila za oznaku visoke vode. Ovo pravilo se može se koristiti ako tablica sadrži stupac koji zadovoljava sljedeće kriterije:

- Umeće sve navesti vrijednost u stupcu. 
- Sva ažuriranja stavke i promijenite vrijednost stupca. 
- Povećava vrijednosti u ovom stupcu sa svim promjenama.
- Upiti koje koriste u `WHERE` uvjet slično `WHERE [High Water Mark Column] > [Current High Water Mark Value]` učinkovito izvršavaju.

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

### <a name="soft-delete-column-deletion-detection-policy"></a>Meki brisanje otkrivanje stupaca brisanje pravila

Kada se reci se brišu iz izvorišne tablice, vjerojatno koju želite izbrisati retke iz indeks pretraživanja. Ako koristite evidentiranje pravila promjena SQL integriran, to je snimanja brigu o umjesto vas. Međutim, promjena visoke vode Označi pravila za praćenje ne vam pomoći s izbrisane retke. Što učiniti? 

Ako retke fizički uklanjaju se iz tablice, da ste izvan luck – ne postoji način da biste nametnuo prisutnosti zapise koji više ne postoji.  Međutim, "mekih Izbriši" tehniku možete koristiti da biste označili retke kao logično izbrisane bez ih ukloniti iz tablice dodavanjem stupca i označavanje retke kao što je izbrisana korištenje oznaka vrijednosti u tom stupcu.

Prilikom korištenja mekih Izbriši postupak, možete odrediti na mekih brisanje pravila na sljedeći način pri stvaranju ili ažuriranje izvora podataka: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Imajte na umu da **softDeleteMarkerValue** mora biti niz – koristiti prikaz niza stvarna vrijednost. Na primjer, ako imate stupca cijeli broj u kojem su izbrisane retke označeni vrijednost 1, pomoću `"1"`; Ako imate stupac BITNE gdje su izbrisane retke označeni Booleova vrijednost true, pomoću `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapiranja između SQL vrste podataka i vrste podataka za pretraživanje Azure

|Vrsta podataka za SQL | Indeks ciljne dopuštene vrste polja |Bilješke 
|------|-----|----|
|bitne|Edm.Boolean, Edm.String| |
|INT, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| bigint | Edm.Int64, Edm.String | |
| realni, plutati |Edm.Double, Edm.String | |
| smallmoney, novac decimalni broj | Edm.String| Azure pretraživanje ne podržava vrpce decimalni vrste Edm.Double jer je to ćete izgubiti preciznosti |
| CHAR, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Pretvorba niz stupac u Collection(Edm.String) potreban je pomoću pretpregleda API verzija 2015-02-28-pretpregled. [U ovom se članku](search-api-indexers-2015-02-28-preview.md#CreateIndexer) dodatne informacije potražite u članku| 
|smalldatetime, datumom i vremenom, datetime2, datum, datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|Zemljopis | Edm.GeographyPoint | Podržani su samo Zemljopis instanci vrste TOČKU s 4326 SRID (što je zadana postavka) | | 
|ROWVERSION| N/D |Redak verziju stupaca nije moguće spremiti u indeks pretraživanja, ali se može koristiti za evidentiranje promjena | |
| vrijeme, vremenski raspon, binarni, varbinary, slike, xml, geometriju, vrste CLR | N/D |Nije podržano |


## <a name="frequently-asked-questions"></a>Najčešća pitanja

**Q:** Mogu li koristiti indeksiranje Azure SQL s bazama podataka SQL sustavom IaaS VMs u Azure?

O: da. Međutim, morate dopustiti servisa za pretraživanje da biste se povezali s bazom podataka tako da učinite sljedeće dvije stvari. Pročitajte članak [Konfiguriranje veze s indeksiranje pretraživanja Azure sa sustavom SQL Server na programa Azure VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) dodatne informacije.

1. Možda ćete morati konfigurirati bazu podataka s pouzdanom potvrdom tako da se servis za pretraživanje možete otvoriti SSL veze s bazom podataka.
2. Konfiguriranje vatrozida dopustili pristup IP adrese servisa za pretraživanje.

**Q:** Možete koristiti za indeksiranje Azure SQL s izvodi lokalne baze podataka SQL? 

A: možemo ne preporučujemo da ili podržava, kao što je to je potrebna otvaranje baze podataka za internetski promet. 

**Q:** Mogu li koristiti za indeksiranje Azure SQL s bazama podataka koji nisu SQL Server u IaaS sustavom Azure? 

A: Ne podržavamo ovaj scenarij jer nismo testirali za indeksiranje s nijedna baza podataka koji nisu SQL Server.  

**Q:** Je li moguće stvoriti više indexers koji se izvode na raspored? 

O: da. Međutim, samo jedan program za indeksiranje mogu se izvoditi na jedan čvor odjednom. Ako trebate više indexers izvodi istovremeno, razmislite o skaliranje gore servisa za pretraživanje da biste više jedinica za pretraživanje. 

**Q:** Pokretanje indeksiranje utječe na Moje radno opterećenje upit? 

O: da. Indeksiranje izvodi na jedan od čvorove u servis za pretraživanje, a resurse za taj čvor se zajednički koriste između indeksiranja i posluživanje promet upita i zahtjeve API-JA. Ako pokrenuli intenzivno radnih opterećenja indeksiranja i upita i pojaviti visoke rata 503 pogrešaka i povećavati vremena odgovor, preporučujemo da skaliranje gore servisa za pretraživanje.
