<properties 
    pageTitle="Biblioteka klijentski elastic baze podataka pomoću Dapper | Microsoft Azure" 
    description="Pomoću Dapper biblioteka klijentski elastic baze podataka." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Biblioteka klijentski elastic baze podataka pomoću Dapper 

Ovaj je dokument za razvojne inženjere koje ovise o Dapper da biste sastavili aplikacije, ali možete obuhvaćaju [tooling elastic baze podataka](sql-database-elastic-scale-introduction.md) da biste stvorili aplikacija koje sharding mjerilo istek sloju svoje podatke.  Ovaj dokument prikazuje promjene u aplikacije utemeljene na Dapper koji su potrebni za integraciju s alatima elastic baze podataka. Naš fokus na elastic upravljanje bazom podataka shard i podataka o njima ovisne za sastavljanje poruke usmjerava s Dapper. 

**Ogledni kod**: [Elastic Alati baze podataka za baze podataka Azure SQL - Dapper Integracija](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Jednostavno je integriranje **Dapper** i **DapperExtensions** s bibliotekom klijent elastic baze podataka za baze podataka SQL Azure. Aplikacija možete koristiti podatke o njima ovisne usmjeravanja tako da promijenite stvaranja i otvaranje nove [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekata poziv [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) iz [biblioteke za klijenta](http://msdn.microsoft.com/library/azure/dn765902.aspx). Na taj način promjene u aplikaciji samo pri čemu se stvara i otvaranja nove veze. 

## <a name="dapper-overview"></a>Pregled dapper
**Dapper** je objekt relacijski mapiranje. Ga karata .NET objekte iz aplikacije za relacijske baze podataka (i obrnuto). Prvi dio ogledni kod prikazuje kako možete integrirati klijentska biblioteka elastic baze podataka pomoću aplikacije utemeljene na Dapper. Drugi dio uzorak koda ilustrira integraciji prilikom korištenja Dapper i DapperExtensions.  

Mapiranje funkcionalnost Dapper daju načina nastavka veze baze podataka koje Pojednostavnite slanja T-SQL naredbe za izvršavanje te slanje upita baze podataka. Ako, primjerice, Dapper olakšava za mapiranje između .NET objekte i parametre SQL naredbe za **izvršavanje** pozive ili zauzeti rezultate SQL upitima u .NET objekte pomoću **upita** pozive iz Dapper. 

Prilikom korištenja DapperExtensions, više ne trebate unijeti SQL naredbe. Metode proširenja, kao što su **GetList** ili **Umetanje** putem veza s bazom podataka stvaraju SQL naredbe u pozadini.
 
Još jedna prednost Dapper i DapperExtensions je aplikacija kontrole je stvaranje veza s bazom podataka. Omogućuje interakciju s bibliotekom klijent elastic baze podataka koji Brokerske djelatnosti veze na temelju mapiranje shardlets s bazama podataka u bazu podataka.

Da biste dobili Dapper sklopova, potražite u članku [Dapper točka neto](http://www.nuget.org/packages/Dapper/). Dapper proširenja potražite u članku [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Kratak pregled klijentska biblioteka elastic baze podataka

S bibliotekom klijent elastic baze podataka, definirati particije podataka aplikacije pod nazivom *shardlets* , mapirajte ih na baze podataka, a prepoznavanja ih *sharding tipke*. Možete odrediti koliko baze podataka kao što je potrebno, a vaše shardlets raspodijelite ove baze podataka. Mapiranje vrijednosti ključa sharding s bazama podataka pohranjuju kartu shard nudi API-ji u biblioteku. Tu mogućnost zove **shard mapiranje upravljanje**. Karta shard služi i kao broker veze baze podataka zahtjeva koji mogu sharding ključ za. Ova mogućnost je se nazivaju **podataka o njima ovisne usmjeravanja**.

![Shard karte i podataka o njima ovisne usmjeravanja][1]

Upravitelj karte shard štiti korisnika iz koje nisu usklađene prikaza u shardlet podatke koji se mogu pojaviti kada su operacija upravljanja Istodobni shardlet događa na baze podataka. Da biste to učinili, karte shard vi posrednik veze baze podataka za aplikaciju u komponenti s bibliotekom. Kad operacija upravljanja shard mogli bi utjecati na shardlet, time funkcija karte shard automatski uklanjanje veza s bazom podataka. 

Umjesto korištenja način za stvaranje veza za Dapper moramo [OpenConnectionForKey način](http://msdn.microsoft.com/library/azure/dn824099.aspx). Time se osigurava da dođe do sve provjere valjanosti, a veze upravlja se ispravno podatke prelazi sa shards.

### <a name="requirements-for-dapper-integration"></a>Preduvjeti za integraciju Dapper

Kada radite s klijentska biblioteka elastic baze podataka i Dapper API-ji, ne možemo želite zadržati sljedeća svojstva:

* **Scaleout**: želimo Dodavanje ili uklanjanje baze podataka iz podataka sloju sharded aplikacije za potrebe zahtjeva kapaciteta aplikacije. 

-    **Dosljednost**: budući da je naš aplikacije neproporcionalno odgovor pomoću sharding, ne možemo morate izvesti podatke o njima ovisne usmjeravanja. Želimo korištenje podataka o njima ovisne usmjeravanje mogućnosti biblioteke da biste to učinili. Posebno želimo da biste zadržali provjere valjanosti i dosljednost jamčiti nudi veze koje brokered putem upravitelja shard karte da biste izbjegli oštećenja ili rezultate upita u redu. Na taj način povezivanja s određenom shardlet su odbili ili zaustaviti ako se (na primjer) na shardlet trenutno Premjesti različite shard korištenja API-ji Podijeli/spajanja.

-    **Objekt mapiranja**: želimo da biste zadržali praktičnost mapiranja nudi Dapper da biste preveli između klase u aplikaciji i osnovne strukture baze podataka. 

Sljedeći odjeljak sadrži smjernice za tim preduvjetima za aplikacije koje se temelji na **Dapper** i **DapperExtensions**.

## <a name="technical-guidance"></a>Tehnički vodič
### <a name="data-dependent-routing-with-dapper"></a>Podaci o njima ovisne usmjeravanje s Dapper 

Aplikacija je Dapper, obično odgovorna za stvaranje i otvaranje veze u podlozi bazu podataka. Zadana vrsta T u aplikaciji, Dapper vraća rezultate upita kao .NET zbirke vrste T. Dapper izvodi mapiranje iz redaka rezultat T SQL objekata vrste T. Isto tako, Dapper karata .NET objekata u SQL vrijednosti ili parametara za naredbe jezik (DML) za rukovanje podataka. Dapper nudi ta je funkcija putem proširenje metode običnog [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekta iz biblioteka ADO .NET SQL klijenta. SQL vezom vratio Elastic mjerilo API-ji za DDR su i obične [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekte. Time se omogućuje nam izravno koristiti Dapper proširenja iznad vrste vratio klijentska biblioteka DDR API-jem, kao što je i jednostavno klijent za SQL vezu.

Ove opažanja provjerite jednostavne za korištenje veza posreduju klijentska biblioteka elastic baze podataka za Dapper.

U ovom se primjeru kod (iz popratnu uzorku) prikazuje pristup gdje tipku sharding nudi aplikacije u biblioteku vi posrednik veza s desne shard.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Poziv na [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API zamjenjuje zadanom stvaranje i otvaranje veze klijent za SQL. Poziv [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) uzima argumenti koji su potrebni za podatke o njima ovisne usmjeravanja: 

-    Karta shard za pristup podacima o njima ovisne za usmjeravanje sučelja
-    Ključ sharding da biste odredili na shardlet
-    Vjerodajnica (korisničko ime i lozinka) da biste se povezali s shard

Karta objekt shard stvara vezu shard koja sadrži shardlet za dani sharding ključ. Klijent elastic baze podataka API-ji oznaku i veze za implementaciju njegov jamstva dosljednost. Budući da se poziv [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) vraća uobičajeni SQL Client connection objekt, kasnije poziv **metoda proširenje** iz Dapper slijedi standardne Dapper vježbe.

Upiti rade vrlo slično na isti način – prvi put otvorite vezu pomoću [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) putem klijentskog programa API-JA. A zatim pomoću metode običnog Dapper proširenje kartu rezultate SQL upita u .NET objekata:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Imajte na umu da se **pomoću** blokirati s vezom za DDR scopes sve operacije baze podataka unutar bloka jedan shard gdje se drži tenantId1. Upit vraća samo blogovi pohranjene na trenutnom shard, ali ne one spremljene na druge shards. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Podaci o njima ovisne usmjeravanje s Dapper i DapperExtensions

U sklopu Dapper u zajednici dodatne proširenja koji vam može ponuditi dodatne praktičnosti i apstrakcije iz baze podataka pri razvoju aplikacija za baze podataka. Primjer je DapperExtensions. 

Korištenje DapperExtensions u aplikaciji neće se promijeniti kako stvara i njima upravlja veze baze podataka. Je i dalje odgovornosti aplikacije da biste otvorili veze i obične objekti povezivanja SQL Client se očekuje pomoću metoda kućni broj. Ne možemo možete koristiti [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kao što je vidljivo iznad. Kao što je sljedeća kod primjera pokazuju, samo se promjena smo više ne morate pisati T SQL naredbe:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

A sada ti uzorak koda za upit: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Rukovanje tranzitne kvarove

Tim za Microsoft Patterns & prakse objaviti [Tranzitne kvara zadužen za blokiranje aplikacije](http://msdn.microsoft.com/library/hh680934.aspx) da biste lakše razvojnim inženjerima prevladavanje uobičajenih uvjeta tranzitne isječci koji su otkrivena kada se pokrene u oblaku. Dodatne informacije potražite u članku [Perseverance, tajna sve uspjehe: pomoću tranzitne blokiranja kvara rukovanje aplikacije](http://msdn.microsoft.com/library/dn440719.aspx).

Uzorak koda ovisi o biblioteci tranzitne kvara da biste zaštitili tranzitne kvarove. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** kod iznad se definira kao **SqlDatabaseTransientErrorDetectionStrategy** s pokušaj broja 10 i vrijeme čekanja 5 sekundi između nekoliko pokušaja. Ako koristite transakcija, pripazite da raspon pokušaj povratak na početak transakcija slučaju tranzitne kvara.

## <a name="limitations"></a>Ograničenja

Pristupa opisani u ovom dokumentu entail nekoliko ograničenja:

* Uzorak koda za taj dokument sadrži upute za upravljanje shemom shards.
* Uz zahtjev, smo pretpostavlja da sve njegove obrada baze podataka sadržana je u jednom shard kao otkrije sharding ključa koji ste dobili zahtjev. Međutim, tu pretpostavku nije uvijek držite, na primjer, kada nije moguće dostupnim sharding ključ. Da biste to riješili, klijentska biblioteka elastic baza podataka sadrži [MultiShardQuery predmete](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Klasa implementira apstrakcije veze za slanje upita putem nekoliko shards. Korištenje MultiShardQuery u kombinaciji s Dapper izlazi iz ovog dokumenta.

## <a name="conclusion"></a>Zaključak

Aplikacije koje koriste Dapper i DapperExtensions jednostavno možete im Alati elastic baze podataka za baze podataka SQL Azure. Kroz korake navedene u ovom dokumentu, tim aplikacijama možete koristiti mogućnost alata za podatke o njima ovisne usmjeravanja tako da promijenite stvaranja i otvaranje nove [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekata poziv [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) elastic baza podataka biblioteke klijenta. Na taj način promjene aplikacije potreban za te mjesta gdje stvara i otvara nove veze. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 