<properties 
    pageTitle="Biblioteka klijentski elastic baze podataka pomoću entitet Framework | Microsoft Azure" 
    description="Korištenje biblioteka klijentski Elastic baze podataka i entitet Framework kodiranje baze podataka" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Elastic biblioteka klijentski baze podataka s entitet Framework 
 
Ovaj dokument prikazuje promjene u aplikaciji Framework entitet koji su potrebni za integraciju s [Alati Elastic baze podataka](sql-database-elastic-scale-introduction.md). Fokus je na [shard mapiranje upravljanja](sql-database-elastic-scale-shard-map-management.md) i [usmjeravanje ovisne podataka](sql-database-elastic-scale-data-dependent-routing.md) s entitet Framework **Kod prvom** pristupu za sastavljanje poruke. Praktični vodič [Kod prvog – novu bazu podataka](http://msdn.microsoft.com/data/jj193542.aspx) za EF služi kao primjeru izvodi u ovom dokumentu. Ogledni kod popratnim ovaj dokument je dio Alati baze podataka za elastic skup uzoraka u Visual Studio primjere koda.
  
## <a name="downloading-and-running-the-sample-code"></a>Preuzimanje i pokretanje uzorak koda
Da biste preuzeli kod za ovaj članak:

* Potreban je Visual Studio 2012 ili noviji. 
* Pokrenuti Visual Studio. 
* U Visual Studio, odaberite datoteku -> novi projekt. 
* U dijaloškom okviru novi projekt, dođite do **Online uzorka** za **Visual C#** i upišite "elastic db" u okvir za pretraživanje u gornjem desnom kutu.
    
    ![Entitet Framework i aplikacije uzorka elastic baze podataka][1] 

    Odaberite uzorak naziva **Elastic Alati baze podataka za SQL Azure – entitet Framework integracije**. Nakon prihvaćanja licencu, uzorka učitava. 

Da biste pokrenuli uzorka, morate stvoriti tri prazne baze podataka u bazi podataka SQL Azure:

* Baza podataka shard Upravitelj kartu
* Baza podataka shard 1
* Baza podataka shard 2

Nakon stvaranja te baze podataka, ispunite s mjesta za slike u **Program.cs** s naziv poslužitelja baze podataka SQL Azure, nazive baze podataka i vjerodajnice za povezivanje s bazama podataka. Stvaranje rješenja u Visual Studio. Visual Studio će preuzeti potreban NuGet paketi za klijentska biblioteka elastic baze podataka Framework entitet, a tranzitne kvara rukovanje tijekom procesa Sastavi. Provjerite je li vraćanje paketa NuGet omogućen za rješenje. Tu postavku možete omogućiti tako da desnom tipkom miša na datoteku rješenja u Visual Studio pregledniku rješenja. 

## <a name="entity-framework-workflows"></a>Tijekovi rada Framework entitet 

Razvojni inženjeri Framework entitet ovise o neku od sljedeće četiri tijekove rada da biste sastavili aplikacije, a da biste bili sigurni postojanost za objekti aplikacije: 

* **Kod prvog (nove baze podataka)**: U EF za razvojne inženjere stvara modela u kodu aplikacije, a zatim EF proizvodi bazu podataka iz nje. 
* **Kod prvog (postojeću bazu podataka)**: programer omogućuje EF generiranje koda aplikacije za model s postojećom bazom podataka.
* **Prvi modela**: programer Stvori modela u dizajneru EF, a zatim EF stvara bazu podataka iz modela.
* **Prvi baze podataka**: programer koristi EF tooling da biste nametnuo modela iz postojeće baze podataka. 

Ove postupke oslanjate klase DbContext proziran upravljanje veze baze podataka i shemu baze podataka za aplikaciju. Kao što smo obrađuje detaljno dalje u dokumentu, različite constructors na osnovnu klasu DbContext omogućuju različite razine kontrole nad stvaranja veze baze podataka pokretački i sheme stvaranja. Izazove dođe do prvenstveno iz činjenica da upravljanje vezu baze podataka koje ste dobili od EF prekriva s mogućnostima upravljanja veze s podacima o njima ovisne usmjeravanje sučeljima po klijentska biblioteka elastic baze podataka. 

## <a name="elastic-database-tools-assumptions"></a>Pretpostavke Alati elastic baze podataka 

Definicija termina potražite u članku [Pojmovnik Alati Elastic baze podataka](sql-database-elastic-scale-glossary.md).

S bibliotekom klijent elastic baze podataka, definirajte particije podataka aplikacije pod nazivom shardlets. Shardlets prepoznati sharding tipke i mapiraju se na određene baze podataka. Aplikacija možda ste proizvoljan broj baza podataka po potrebi i distribucija shardlets omogućuje dovoljno kapaciteta ili performanse dobili trenutnu preduvjeti za tvrtke. Mapiranje vrijednosti ključa sharding s bazama podataka pohranjuju kartu shard nudi API-ji za klijent elastic baze podataka. Ne možemo nazovite tu mogućnost **Upravljanja karte Shard**ili SMM za kratki. Shard kartu služi i kao broker veze baze podataka zahtjeva koji mogu sharding ključ za. Ne možemo pogledajte ovu mogućnost kao **podataka ovisne usmjeravanja**. 
 
Upravitelj karte shard štiti korisnika iz koje nisu usklađene prikaza u shardlet podatke koji se mogu pojaviti kada se događa operacija upravljanja Istodobni shardlet (kao što su relocating podataka iz jedne shard na drugo). Da biste to učinili, karte shard upravlja biblioteke broker klijent veze baze podataka za aplikaciju. Time se omogućuje funkciju shard karte automatski kada operacija upravljanja shard mogli bi utjecati shardlet stvorena veza za uklanjanje veza s bazom podataka. Taj se način mora integrirati s nekim EF, funkcija, kao što su stvaranje nove veze iz postojećeg da biste provjerili postojanje baze podataka. Općenito govoreći, naš opažanje je rad standardni DbContext constructors samo radi pouzdano za zatvoreni veze baze podataka koje možete sigurno klonirana za EF. Dizajn načelo elastic baze podataka umjesto toga je samo vi posrednik otvorena veze. Jedan mislite da zatvaranje veze posreduju klijentska biblioteka prije isporuke da biste EF DbContext može riješiti problem. Međutim, zatvaranjem veze i potrebe za oslanjanjem na EF da biste ponovno otvorili, jedan foregoes provjere valjanosti i dosljednost obavlja u biblioteku. Migracija funkcionalnost EF, međutim, koristi te veze za upravljanje podlozi shemu baze podataka na način koji je proziran aplikaciji. Najbolje željeli bismo zadržati i kombiniranje sve ove mogućnosti iz biblioteka klijentski elastic baze podataka i EF u aplikaciji. U sljedećoj sekciji govori o tih svojstava i preduvjeti detaljnije. 


## <a name="requirements"></a>Preduvjeti 

Kada radite s elastic baze podataka klijentska biblioteka i API Framework entitet, ne možemo želite zadržati sljedeća svojstva: 

* **Skaliranje izlaz**: dodavanje ili uklanjanje baze podataka iz podataka sloju sharded aplikacije potrebno zahtjeve kapaciteta aplikacije. To znači da kontrolu nad na stvaranje i brisanje baze podataka i korištenja shard elastic baze podataka mapiranje Upravitelj API-ji za upravljanje bazama podataka i mapiranja shardlets. 

* **Dosljednost**: aplikacija uključuje sharding i koristi podatke o njima ovisne usmjeravanje mogućnosti biblioteke klijenta. Da biste izbjegli oštećenja ili pogrešne rezultate, veze su brokered putem upravitelja shard karta. Time se i zadržava provjere valjanosti i dosljednost.
 
* **Kod prvog**: da biste zadržali praktičnost EF, kod prvi paradigm. U kod prve klase u aplikaciji mapiraju proziran podlozi strukture baze podataka. Kod aplikacije stupi u interakciju s DbSets koji maski Većina aspekte uvrštene u podlozi obrada baze podataka.
 
* **Sheme**: entitet Framework rukuje stvaranja sheme početne baze podataka i kasnije sheme proizvod putem migracije. Po zadržavaju se ove mogućnosti, adapting aplikacije jednostavno je kao razvoja podatke. 

Sljedeće upute upućuje kako zadovoljava te preduvjete za prvu kod aplikacije pomoću alata za elastic baze podataka. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Podaci o njima ovisne usmjeravanje pomoću EF DbContext 

Veza za bazu podataka sa Framework entitet obično upravlja se putem podklase **DbContext**. Stvoriti te podklase dijelovima koji potječu od **DbContext**. Ovo je gdje definiranje vaše **DbSets** koja implementira zbirke baze podataka sigurnosno CLR objekata za svoju aplikaciju. U kontekstu usmjerivanja podataka o njima ovisne utvrđujemo nekoliko korisne svojstava koji ne moraju nužno biti držite drugim EF kod prvog aplikacije situacijama: 

* Baza podataka već postoji i registrirani na karti shard elastic baze podataka. 
* Shema aplikacije već uveden u bazu podataka (opisano u nastavku). 
* Karta shard su posreduju podataka ovisne usmjeravanje veza s bazom podataka. 

Da biste integrirali **DbContexts** s podataka ovisne usmjeravanja za skaliranje izlaz:

1. Stvaranje veza fizičke baze podataka putem sučelja klijenta elastic baze podataka karte upravitelja shard 
2. Veza s podklase **DbContext** prelamanje
3. Prenesite vezu prema dolje u **DbContext** osnovni Tečajevi da biste bili sigurni obrada na strani EF će se dogoditi kao i. 

Sljedeći primjer koda prikazuje takvog. (Kod je i popratnu projekta za Visual Studio)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Glavne točke
* Novi Graditelj zamjenjuje zadani Graditelj u podklase DbContext 
* Novi Graditelj uzima argumenti koji su potrebni za podatke o njima ovisne usmjeravanje putem biblioteke klijent elastic baze podataka:
    * shard karte da biste pristupili sučelja usmjeravanje ovisne podataka
    * sharding ključ za prepoznavanje shardlet,
    * niz za povezivanje s vjerodajnicama za ovisne o podacima usmjeravanje veza na shard. 
 
* Poziv na Graditelj klase uzima u detour u statični način koji se izvodi sve korake potrebne za usmjeravanje ovisne podataka. 
   * Koristi poziv OpenConnectionForKey sučelja klijenta elastic baze podataka na karti shard uspostaviti Otvori vezu.
   * Karta shard stvara otvorenu vezu shard koja sadrži shardlet za dani sharding ključ.
   * Otvaranje povezivanje prosljeđuje natrag na Graditelj klase od DbContext da biste naznačili da je tu vezu tako da koristi EF umjesto da EF automatski Stvori novu vezu. Klijent elastic baze podataka API je označen taj način vezu tako da možete jamči dosljednost u odjeljku operacija upravljanja karte shard.
 
  
Koristite novi Graditelj za vaše DbContext podklase umjesto zadani Graditelj u kodu. Evo jednog primjera: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Novi Graditelj otvorit će se veza s shard koja sadrži podatke za shardlet otkrije vrijednost **tenantid1**. Kod u bloku **pomoću** ostaje nepromijenjen da biste pristupili **DbSet** za blogove pomoću EF na na shard za **tenantid1**. Time se mijenja semantiku koda u korištenja blokirati tako da sve operacije baze podataka imaju sada ograničen prikaz samo jedan shard gdje se drži **tenantid1** . Ako, primjerice, upitu LINQ putem blogova **DbSet** samo vratiti blogovi pohranjene na trenutnom shard, ali ne one spremljene na druge shards.  

#### <a name="transient-faults-handling"></a>Tranzitne kvarove obrade
Tim za Microsoft Patterns & prakse objaviti [U tranzitne kvara rukovanje aplikacije blok](https://msdn.microsoft.com/library/dn440719.aspx). Biblioteke se koristi s elastic mjerilo klijentska biblioteka u kombinaciji s EF. Međutim, provjerite je li da svaku tranzitne iznimku vraća na mjesto gdje ćemo možete biti sigurni da novi Graditelj koristi se nakon tranzitne kvara tako da se sve nove veze je pokušaj pomoću constructors ćemo imati tweaked. U suprotnom vezu ispravne shard se zajamčeno, a postoje bez jamstva veza se održava kako nastaju izmjene shard kartu. 

Sljedećim primjerom koda prikazuje kako koristiti pravila Ponovi SQL oko novi constructors podklase **DbContext** : 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** kod iznad se definira kao **SqlDatabaseTransientErrorDetectionStrategy** s pokušaj broja 10 i vrijeme čekanja 5 sekundi između ponovne pokušaje. Taj se način je slična smjernicama EF i korisnički pokrenut transakcije (pročitajte članak [ograničenja s ponovni strategije izvođenja (nadalje EF6)](http://msdn.microsoft.com/data/dn307226). Oba situacije zahtijevaju kontrole da program aplikacije doseg koji vraća tranzitne iznimke: ponovno otvorite transakcija ili (kao što je prikazano) ponovno stvorite kontekstu s početnim Graditelj koji koristi klijentska biblioteka elastic baze podataka.

Morate da biste odredili gdje tranzitne iznimke da bi dovršila, vratite se u opseg precludes i korištenje ugrađene **SqlAzureExecutionStrategy** koji se isporučuju s EF. **SqlAzureExecutionStrategy** želite ponovno otvorite veze, ali ne koristite **OpenConnectionForKey** i stoga zaobići sve provjere valjanosti koja se izvršava kao dio **OpenConnectionForKey** poziv. Umjesto toga, uzorak koda koristi ugrađene **DefaultExecutionStrategy** koji se isporučuje se s EF. Umjesto **SqlAzureExecutionStrategy**radi ispravno u kombinaciji s pravila Ponovi iz tranzitne rukovanje kvara. Pravilnik za izvršavanje postavlja se u predmete **ElasticScaleDbConfiguration** . Imajte na umu smo ipak ne želite koristiti **DefaultSqlExecutionStrategy** jer predlaže koristite **SqlAzureExecutionStrategy** ako tranzitne iznimke pojaviti – što bi dovesti do pogrešan ponašanje kao što je navedeno. Dodatne informacije o različitim pokušaj pravila i EF potražite u članku [Povezivanje otpornost u EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Graditelj pisanja
U kod iznad Primjeri na zadani Graditelj ponovno piše potrebne za svoju aplikaciju za korištenje podataka o njima ovisne usmjeravanje s Framework entitet. U sljedećoj su tablici generalizes odlučile za druge constructors. 


Trenutni Graditelj  | Potrebno ponovno napisati Graditelj podataka | Osnovni Graditelj | Bilješke
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection booleovom) |Veza mora biti funkciju shard karte i tipku usmjeravanje ovisne podataka. Morate by-pass automatsko povezivanje stvaranja po EF, a umjesto pomoću karte shard vi posrednik vezu. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection booleovom) |Veza se funkciju shard karte i tipku usmjeravanje ovisne podataka. Fixed baze podataka niza ime ili veze će funkcionirati kao one provjere valjanosti by-pass po shard karte. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |Veza će stvaraju za dani shard kartu i sharding ključ s modelom navedene. Kompilirana modela će se proslijediti osnovni c'tor.
MyContext (DbConnection booleovom) |ElasticScaleContext (ShardMap, TKey, bool) |DbContext (DbConnection booleovom) |Veza mora slučajna iz mape shard i tipku. To nije moguće dati kao ulaz (osim ako taj unos je već pomoću karte shard i ključa). Na Booleove vrijednosti će se prenijeti na. 
MyContext (niz znakova, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |Veza mora slučajna shard kartu i tipku. To nije moguće dati kao ulaz (osim ako taj unos je pomoću karte shard i ključa). Kompilirana modela će se prenijeti na. 
MyContext (ObjectContext booleovom) |ElasticScaleContext (ShardMap TKey, ObjectContext, booleovom) |DbContext (ObjectContext booleovom) |Novi Graditelj mora provjerite je li sve veze u ObjectContext proslijeđena kao ulaz ponovno usmjerena na vezu upravlja Elastic mjerilo. Raspravi detaljne ObjectContexts izlazi iz ovog dokumenta.
MyContext (DbConnection, DbCompiledModel, bool) |ElasticScaleContext (ShardMap TKey, DbCompiledModel, booleovom)| DbContext (DbConnection, DbCompiledModel, bool); |Veza mora slučajna iz mape shard i tipku. Veza ne može se prikazuje se kao ulaz (osim ako taj unos je već pomoću karte shard i ključa). Model i Booleove vrijednosti prenose se Graditelj klase. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Uvođenje sheme shard kroz EF migracije 

Upravljanje automatsko sheme je praktičnost nudi Framework entitet. U kontekstu aplikacije pomoću alata za elastic baze podataka, ne možemo želite zadržati tu mogućnost automatski Dodjela shemu novostvorenu shards baze podataka je netko dodao sharded aplikaciju. Primarni koristi se da biste povećali kapacitetu sloju podataka za sharded aplikacije koje se koriste EF. Aplikacijom sharded utemeljena na EF potrebe za oslanjanjem na EF, mogućnosti za upravljanje shemom smanjuje Administracija trud baze podataka. 

Shema implementaciju putem EF Migracija najbolje funkcionira na **Neotvorena veze**. To je za razliku od scenarij za podatke o njima ovisne usmjeravanja koji se zasniva na otvorena veze koje ste dobili od elastic baze podataka Klijentski API. Drugi razlika je obavezne dosljednost: prilikom poželjno osigurati dosljednost za sve veze usmjeravanje ovisne podataka da biste zaštitili rukovanje Istodobni shard kartu, nije problem s implementaciji Početna shema za novu bazu podataka koja sadrži nije još registrirani shard kartu, a nije još je dodijeljen držite shardlets. Ne možemo možete stoga oslanjate veze običnoj bazi podataka za scenarije, umjesto podataka ovisne usmjeravanja.  

To potencijalne klijente programa pristupa gdje sheme implementaciju putem EF Migracija čvrsto je povezano s Registracija novu bazu podataka kao shard aplikacije shard karta. To ovisi o sljedeće preduvjete: 

* Baza podataka već stvorena. 
* Prazna baza podataka – sadrži shema korisnika i korisničkih podataka.
* Baza podataka nije još moguće pristupiti putem klijentskog programa elastic baze podataka API-ji za usmjeravanje ovisne podataka. 

Pomoću ove preduvjeti na mjestu možemo stvoriti na pravilnim nepotvrđen otvorena **SqlConnection** da biste izbaciti EF Migracija sheme implementacije. Sljedećim primjerom koda ilustrira takvog. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

Ovaj primjer prikazuje način na koji registrira u shard na karti shard, uvodi sheme kroz EF migracije i pohranjuje mapiranje sharding ključ da biste na shard **RegisterNewShard** . To ovisi o Graditelj od podklase **DbContext** (**ElasticScaleContext** u uzorku) koja vas vodi niza za povezivanje SQL kao ulaz. Kod za ovaj Graditelj je klikom prosljeđivanje, kao u sljedećem primjeru: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Jedan možda ste koristili verziju Graditelj nasljeđuju od osnovnu klasu. No kod treba osigurati da se zadana initializer za EF koristi prilikom povezivanja. Dakle u kratki detour u statični metodu prije zovete u Graditelj klase s niz za povezivanje. Imajte na umu da je registracija shards trebale bi funkcionirati u drugu aplikaciju domene ili postupak da biste bili sigurni da initializer postavki EF nisu u sukobu. 


## <a name="limitations"></a>Ograničenja 

Pristupa opisani u ovom dokumentu entail nekoliko ograničenja: 

* EF aplikacije koje koriste **LocalDb** najprije morate migriranje u običnoj bazi podataka sustava SQL Server prije korištenja biblioteke klijent elastic baze podataka. Skaliranje out aplikaciju putem sharding s Elastic skalom nije moguće s **LocalDb**. Imajte na umu da razvoj i dalje možete koristiti **LocalDb**. 

* Promjene na aplikaciju koje ukazuju na promjene sheme baze podataka morate proći kroz EF migracije na sve shards. Uzorak koda za taj dokument prikazuju kako to učiniti. Razmislite o korištenju ažuriranje bazu podataka s parametrom ConnectionString iteracija preko svih shards; ili izdvojiti T SQL skripte na čekanju migracije pomoću ažuriranje bazu podataka s – skriptu mogućnost i primijenite T SQL skripta vaše shards.  

* Uz zahtjev, pretpostavlja se da sve njegove obrada baze podataka sadržana je u jednom shard kao otkrije sharding ključa koji ste dobili zahtjev. No tu pretpostavku nije uvijek držite true. Na primjer, kada nije moguće da biste oslobodili sharding ključ. Da biste to riješili, klijentska biblioteka nudi klase **MultiShardQuery** koji implementira apstrakcije veze za slanje upita putem nekoliko shards. Osnove korištenja **MultiShardQuery** u kombinaciji s EF izlazi iz ovog dokumenta

## <a name="conclusion"></a>Zaključak

Kroz korake navedene u ovom dokumentu, EF aplikacije mogu koristiti mogućnost elastic baza podataka biblioteke klijenta za podatke o njima ovisne usmjeravanje prema restrukturiranje constructors podklase **DbContext** koristili u aplikaciji EF. Na taj način promjene potrebne za te mjesta gdje se klase **DbContext** već postoji. Osim toga, EF aplikacije možete nastaviti prednosti automatskog sheme implementacije kombiniranjem pozivanje potrebne EF Migracija s Registracija novi shards i mapiranja u njoj shard korake. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 