<properties 
    pageTitle="Upute za stvaranje web-aplikacijama Redis predmemorije | Microsoft Azure" 
    description="Saznajte kako stvoriti web-aplikacijama s Redis predmemorije" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Upute za stvaranje web-aplikacijama Redis predmemorije

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PLATFORME ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti i implementirati ASP.NET web-aplikacije web-aplikaciju u aplikacije servisa za Azure pomoću Visual Studio 2015. Primjer aplikacije prikazuje popis tima statističkih podataka iz baze podataka i prikazuje sve načine možete koristiti Azure Redis predmemoriju za pohranu i dohvaćanje podataka iz predmemorije. Kada dovršite vodič imat ćete pokrenuti web-aplikacije koje se čita i zapisuje u bazu podataka, optimizirane s Azure Redis predmemorije i smješten u Azure.

Ćete saznati:

-   Kako stvoriti ASP.NET MVC 5 web-aplikacije u Visual Studio.
-   Upute za pristup podacima iz baze podataka pomoću Framework entitet.
-   Kako poboljšati propusnost podataka i smanjiti učitavanja baze podataka za pohranu i dohvaćanje podataka pomoću Azure Redis predmemorije.
-   Upute za korištenje na Redis Sortira skup za dohvaćanje gornji 5 timovima.
-   Upute za dodjeljivanje Azure resursi za aplikacije pomoću predloška Voditelj resursa.
-   Kako objaviti aplikacije Azure pomoću Visual Studio.

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili vodič, morate imati sljedeće preduvjete.

-   [Račun za Azure](#azure-account)
-   [Visual Studio 2015 s Azure SDK za .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Račun za Azure

Potreban vam je račun za Azure da biste dovršili vodič. možeš:

* [Otvorite račun za Azure besplatno](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Dobit ekipa koje je moguće koristiti da biste isprobali plaćenu servisa Azure. Čak i kada koriste na kredita, možete zadržati račun i pomoću besplatnog servisa Azure i značajke.
* [Aktiviranje Visual Studio pretplatnika prednosti](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Pretplate na MSDN vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 s Azure SDK za .NET

Vodič je pisane za Visual Studio 2015 s [Azure SDK za .NET](../dotnet-sdk.md) 2.8.2 ili noviji. [Preuzmite najnovije Azure SDK za Visual Studio 2015 ovdje](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio automatski se instalira s SDK ako već nemate.

Ako imate Visual Studio 2013, možete ga [Preuzmite najnovije Azure SDK za Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Neke zaslonima mogu izgledati drugačije od ilustracije prikazano ovog praktičnog vodiča.

>[AZURE.NOTE] Ovisno o tome koliko ovisnosti SDK već imate na računalu, instalacije SDK može potrajati, od nekoliko minuta pola sata ili više.

## <a name="create-the-visual-studio-project"></a>Stvaranje projekta za Visual Studio

1. Otvorite Visual Studio i kliknite **datoteku**, **Novi** **projekt**.

2. Proširite čvor **Visual C#** na popisu **predložaka** , odaberite **oblak**pa kliknite **ASP.NET web-aplikacije**. Provjerite je li odabrana **.NET Framework 4.5.2** .  Upišite **ContosoTeamStats** u tekstni okvir **naziv** , a zatim kliknite **u redu**.
 
    ![Stvaranje projekta][cache-create-project]

3. Odaberite **MVC** kao vrsta projekta. Poništite potvrdni okvir **glavnog računala u oblaku** . U ovom praktičnom vodiču ćete [Dodjela resursa za Azure](#provision-the-azure-resources) i [Objavljivanje aplikacija za Azure](#publish-the-application-to-azure) u sljedećim koracima. Primjer dodjeljivanje aplikacije servisa za web-aplikaciji iz Visual Studio ostavite **glavnog računala u oblaku** potvrđen, potražite u članku [Početak rada sa Web Apps na servisu Azure aplikacije pomoću platforme ASP.NET i Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).

    ![Odaberite predložak projekta][cache-select-template]

4. Kliknite **u redu** da biste stvorili projekta.

## <a name="create-the-aspnet-mvc-application"></a>Stvaranje aplikacije MVC platforme ASP.NET

U ovom odjeljku vodič stvorit ćete osnovni aplikacije koji čita i prikazuje tima statističkih podataka iz baze podataka.

-   [Dodavanje u model](#add-the-model)
-   [Dodavanje kontrolerom](#add-the-controller)
-   [Konfiguriranje prikaza](#configure-the-views)

### <a name="add-the-model"></a>Dodavanje u model

1. Desnom tipkom miša kliknite **modela** u **Pregledniku rješenja**, a zatim odaberite **Dodaj**, **Predmet**. 

    ![Dodavanje modela][cache-model-add-class]

2. Unesite `Team` klase naziv, a zatim kliknite **Dodaj**.

    ![Dodajte predmet modela][cache-model-add-class-dialog]

3. Zamjena na `using` naredbe pri vrhu na `Team.cs` datoteke sa sljedećim pomoću naredbe.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Zamjena definicije na `Team` klase pomoću sljedeće isječak koda koji sadrži do ažurirane `Team` klase definicija, kao i neke druge entitet Framework pomoćne klase. Dodatne informacije o kod prvi pristup Framework entitet koja se koristi u ovom ćete praktičnom vodiču, potražite u članku [koda najprije za novu bazu podataka](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. U **Pregledniku rješenja**, dvokliknite **web.config** da biste ga otvorili.

    ![Web.config][cache-web-config]

3.  Dodajte sljedeće niz za povezivanje da biste na `connectionStrings` sekciju. Naziv niz za povezivanje moraju podudarati s imenom klase kontekst entitet Framework baze podataka koja je `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Nakon dodavanja, u `connectionStrings` odjeljak trebao bi izgledati kao u sljedećem primjeru.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Dodavanje kontrolerom

1. Pritisnite **F6** da biste sastavili projekta. 
2. U **Pregledniku rješenja**, desnom tipkom miša kliknite mapu **kontrolera** i odaberite **Dodaj**, **kontroler**.

    ![Dodavanje kontrolera][cache-add-controller]

3. Odaberite **MVC 5 kontroler s prikaza, putem Framework entitet**, a zatim kliknite **Dodaj**. Ako dođe do pogreške nakon što kliknete **Dodaj**, provjerite je li prvo ugrađen u projekt.

    ![Dodavanje kontrolera klase][cache-add-controller-class]

5. S padajućeg popisa **predmete modela** odaberite **tima (ContosoTeamStats.Models)** . S padajućeg popisa **predmete kontekst podataka** odaberite **TeamContext (ContosoTeamStats.Models)** . Vrsta `TeamsController` u **kontroler** tekstni okvir Naziv (Ako se neće automatski popunjavaju). Kliknite **Dodaj** da biste stvorili predmete kontroler i dodavanje zadane prikaze.

    ![Konfiguriranje kontroler][cache-configure-controller]

4. U **Pregledniku rješenja**proširite **Global.asax** , a zatim dvokliknite **Global.asax.cs** da biste ga otvorili.

    ![Global.asax.CS][cache-global-asax]

5. Dodajte sljedeća dva pomoću naredbe pri vrhu datoteke u odjeljku ostale pomoću naredbe.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Dodavanje sljedeći redak koda na kraju na `Application_Start` način.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. U **Pregledniku rješenja**, proširite `App_Start` i dvokliknite `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Zamjena `controller = "Home"` u sljedeći kod u na `RegisterRoutes` način `controller = "Teams"` kao što je prikazano u sljedećem primjeru.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Konfiguriranje prikaza

1. U **Pregledniku rješenja**, proširite mapu **prikaza** , a zatim mapu **zajednički se koristi** , a zatim dvokliknite **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Promjena sadržaja na `title` element i zamijeni `My ASP.NET Application` s `Contoso Team Stats` kao što je prikazano u sljedećem primjeru.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. U na `body` sekcije, ažurirajte prvi `Html.ActionLink` izjava i zamjena `Application name` s `Contoso Team Stats` i zamijeni `Home` s `Teams`.
    -   Prije:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Poslije:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Kod promjene][cache-layout-cshtml-code]

4. Pritisnite **Ctrl + F5** omogućuje stvaranje i pokretanje aplikacija. Ova verzija aplikacije čita rezultate izravno iz baze podataka. Imajte na umu **Stvori novo**, **Uređivanje**, **Detalji**i **Brisanje** akcije koje su automatski dodaju u aplikaciju scaffold **MVC 5 kontroler s prikaza, putem Framework entitet** . U sljedećem odjeljku vodič ćete dodati Redis predmemorije za optimiziranje pristupa podacima i navedite dodatne značajke aplikaciji.

![Aplikacija Starter][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Konfiguriranje aplikacije da biste koristili Redis predmemorije

U ovom odjeljku vodič ćete konfigurirati primjer aplikacije za pohranu i dohvaćanje Contoso tima Statistika iz predmemorije Redis Azure instance pomoću klijenta predmemorije [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .

-   [Konfiguriranje aplikacije da biste koristili StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
-   [Ažuriranje razred TeamsController tako da vraća rezultate iz predmemorije ili baze podataka](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Stvaranje, uređivanje i brisanje načina za rad s predmemoriju za ažuriranje](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Ažuriranje indeksa timovima prikaza da biste radili s predmemoriju](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Konfiguriranje aplikacije da biste koristili StackExchange.Redis

1. Da biste konfigurirali klijentske aplikacije u Visual Studio pomoću značajke pakiranja StackExchange.Redis NuGet, desnom tipkom miša kliknite projekt u **Pregledniku rješenja** , a zatim odaberite **Upravljanje NuGet paketa**. 

    ![Upravljanje NuGet paketa][redis-cache-manage-nuget-menu]

2. U tekstni okvir za pretraživanje upišite **StackExchange.Redis** , odaberite željenu verziju s rezultatima i kliknite **Instaliraj**.

    ![StackExchange.Redis NuGet paketa][redis-cache-stack-exchange-nuget]

    Paket NuGet preuzimanja i dodaje potrebne skupa reference za klijentsku aplikaciju da biste pristupili predmemorije Redis Azure s klijentom predmemorije StackExchange.Redis. Ako biste radije da biste koristili verziju istaknuti pod nazivom biblioteke **StackExchange.Redis** klijenta, odaberite **StackExchange.Redis.StrongName**; u suprotnom odaberite **StackExchange.Redis**.

3. U **Pregledniku rješenja**, proširite mapu **kontrolera** , a zatim dvokliknite **TeamsController.cs** da biste ga otvorili.

    ![Kontroler timovima][cache-teamscontroller]

4. Dodajte sljedeća dva pomoću naredbe za **TeamsController.cs**.

        using System.Configuration;
        using StackExchange.Redis;

5. Dodajte sljedeća dva svojstva da biste na `TeamsController` predmete.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Stvorite datoteku na računalu pod nazivom `WebAppPlusCacheAppSecrets.config` i stavite na mjesto kojemu se neće se provjeriti pomoću šifru izvora programa uzorka odlučite da biste provjerili na neko mjesto. U ovom primjeru u `AppSettingsSecrets.config` datoteka se nalazi u `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Uređivanje u `WebAppPlusCacheAppSecrets.config` datoteku i dodati sadržaj. Ako pokrenete aplikaciju lokalno ti podaci se koristi za povezivanje vašoj instanci Azure Redis predmemoriju. Kasnije u ovom praktičnom vodiču ćete Dodjela instancu Azure Redis predmemorije i ažuriranje predmemorije ime i lozinku. Ako namjeravate pokrenuti aplikaciju uzorka lokalno možete preskočiti stvaranje datoteka i sljedeće korake koje upućuju na datoteci, jer kada implementirati Azure aplikacija dohvaća podatke o vezi predmemorije iz postavku aplikaciju za web-aplikacije i neće tu datoteku. Budući da u `WebAppPlusCacheAppSecrets.config` je implementiran za Azure s aplikacijom, ne morate je osim ako namjeravate pokrenuti aplikaciju lokalno.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. U **Pregledniku rješenja**, dvokliknite **web.config** da biste ga otvorili.

    ![Web.config][cache-web-config]

3. Dodajte sljedeće `file` atributa da biste na `appSettings` element. Ako ste koristili drugi naziv ili mjesto, zamijenite te se vrijednosti za one koji je prikazano u primjeru.
    -   Prije:`<appSettings>`
    -   Poslije:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    Izvođenje ASP.NET spaja sadržaj vanjske datoteke s oznake u na `<appSettings>` element. Za vrijeme izvođenja zanemaruje atribut datoteke ako nije moguće pronaći navedenu datoteku. Tajne (niz za povezivanje u predmemoriju) nisu dio izvornog koda za aplikaciju. Kada implementacija web-aplikaciju programa Azure, u `WebAppPlusCacheAppSecrests.config` neće biti implementirano datoteka (koji je ono što želite). Da biste naveli te tajne Azure nekoliko načina, a u ovom ćete praktičnom vodiču su konfigurirana automatski umjesto vas kada vam sljedeći praktičnog vodiča [Dodjela resursa za Azure](#provision-the-azure-resources) korak. Dodatne informacije o radu s tajne u Azure potražite u članku [najbolje prakse pri implementacija lozinke i druge povjerljive podatke ASP.NET i aplikacije servisa za Azure](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Ažuriranje razred TeamsController tako da vraća rezultate iz predmemorije ili baze podataka

U ovom primjeru Statistika tima mogu biti dohvaćeni iz baze podataka ili iz predmemorije. Statistički podaci o tim spremaju se u predmemoriji kao na serijaliziranog `List<Team>`, a i kao skup sortirani koriste vrste podataka Redis. Prilikom preuzimanja stavki iz skupa sortirani, možete dohvatiti neke, sve ili upit za određene stavke. U ovom primjeru ćete upit sortirani skup za gornje 5 timovima prema broju ima prednost.

>[AZURE.NOTE] Nije potrebna za pohranu statistike tima u više oblika u predmemoriji za korištenje predmemorije Redis Azure. Pomoću ovog praktičnog vodiča koristi više oblika da bismo pokazali neke načine i različitih vrsta podataka možete koristiti podatke u predmemoriju.



1. Dodajte sljedeće pomoću naredbe za na `TeamsController.cs` datoteke pri vrhu međusobno pomoću naredbe.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Zamjena trenutni `public ActionResult Index()` postupak sa sljedećim implementacije.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Dodajte sljedeća tri načina za na `TeamsController` klase za implementaciju u `playGames`, `clearCache`, i `rebuildDB` vrste akcija iz promjenu naredbe dodane u prethodni isječak koda.

    Na `PlayGames` način obnavlja statistike tima, simulaciju razdoblja igara, sprema rezultate u bazu podataka i čisti sada zastarjelih podataka iz predmemorije.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    Na `RebuildDB` način reinitializes baze podataka pomoću zadani skup timovima generira Statistika za njih te čisti sada zastarjelih podataka iz predmemorije.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    Na `ClearCachedTeams` način uklanja sve predmemorirane tima Statistika iz predmemorije.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Dodajte sljedeća četiri načina za na `TeamsController` predmete implementaciju razne načine za dohvaćanje statistike tima iz predmemorije i baze podataka. Svaki od ovih metoda vraća na `List<Team>` koji će se zatim prikazati po prikazu.

    Na `GetFromDB` način čita statistike tima iz baze podataka.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    Na `GetFromList` način čita statistike tima iz predmemorije kao na serijaliziranog `List<Team>`. Ako je neuspješno pronalaženje u predmemoriji, statistike tima su pročitajte iz baze podataka i zatim pohranjenih u predmemoriji za buduću upotrebu. U ovom primjeru priznanje JSON.NET serijalizacije serijalizirati .NET objekte i iz predmemorije. Dodatne informacije potražite [u](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)članku Rad s objektima .NET u predmemoriji Redis Azure.

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    Na `GetFromSortedSet` način čita statistike tima iz predmemorije sortirani skupa. Ako je neuspješno pronalaženje u predmemoriji, statistike tima su čitanja baze podataka i pohranjenih u predmemoriji sortirani postavljenog.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    Na `GetFromSortedSetTop5` način čita timovima gornjih 5 iz predmemorije sortirani skupa. Pokreće potvrđivanjem predmemoriju postojanje na `teamsSortedSet` ključa. Ako taj ključ nema, u `GetFromSortedSet` način zove čitanje statistike tima i sprema ih u predmemoriji. Sljedeće skup predmemorirani sortirani mu je za gornje 5 timovima koje se vraćaju.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Stvaranje, uređivanje i brisanje načina za rad s predmemoriju za ažuriranje

Kod scaffolding koja je stvorena kao dio ovaj uzorak obuhvaća načina za dodavanje, uređivanje i brisanje timovima. Kad god tima dodali, uređivati ili ukloniti, podatke u predmemoriji postaje zastarjelih. U ovom odjeljku ćete izmijeniti ove tri načina za čišćenje predmemorirani timovima tako da se predmemoriju neće biti usklađen s bazom podataka.

1. Dođite na `Create(Team team)` metoda u na `TeamsController` predmete. Dodavanje poziv na `ClearCachedTeams` način, kao što je prikazano u sljedećem primjeru.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Dođite na `Edit(Team team)` metoda u na `TeamsController` predmete. Dodavanje poziv na `ClearCachedTeams` način, kao što je prikazano u sljedećem primjeru.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Dođite na `DeleteConfirmed(int id)` metoda u na `TeamsController` predmete. Dodavanje poziv na `ClearCachedTeams` način, kao što je prikazano u sljedećem primjeru.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Ažuriranje indeksa timovima prikaza da biste radili s predmemoriju

1. U **Pregledniku rješenja**, proširite mapu **prikaza** , zatim mapu **timovima** , a zatim dvokliknite **Index.cshtml**.

    ![INDEX.cshtml][cache-views-teams-index-cshtml]

2. Pri vrhu datoteke, obratite pažnju na sljedeći element odlomka.

    ![Akcija tablice][cache-teams-index-table]

    Ovo je vezu da biste stvorili novi tima. Zamijenite element odlomka u sljedećoj tablici. Ova tablica sadrži veze akcija za stvaranje novog timskog, reproducira novi razdoblja igara, čišćenje predmemorije, dohvaćanje timova iz predmemorije u nekoliko oblika, dohvaćanje timova iz baze podataka i ponovno stvaranje baze podataka pomoću Osvježi ogledne podatke.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Pomaknite se do dna **Index.cshtml** datoteku i dodajte sljedeće `tr` element tako da je zadnjeg retka u tablici zadnjeg u datoteci.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Redak prikazuje vrijednost `ViewBag.Msg` koja sadrži izvješća o statusu o trenutni postupak koji je kada kliknete neku od akcija veza u prethodnom koraku.   

    ![Poruka o stanju][cache-status-message]

4. Pritisnite **F6** da biste sastavili projekta.

## <a name="provision-the-azure-resources"></a>Dodjela resursa za Azure

Za hostiranje vaše aplikacije u Azure, najprije morate Dodjela Azure servise koje je potrebno aplikacije. Primjer aplikacije pomoću ovog praktičnog vodiča koristi sljedećih servisa Azure.

-   Azure Redis predmemorije
-   Aplikacije servisa za web-aplikacije
-   SQL baze podataka

Da biste implementirali servise za ove resursa novu ili postojeću grupu po izboru, kliknite gumb sljedeće **uvođenja za Azure** .

[! [Implementacija Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Ovaj gumb **uvođenja za Azure** koristi [Stvaranje web-aplikacijama plus Redis predmemorije plus baze podataka SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure brzi početak rada](https://github.com/Azure/azure-quickstart-templates) predloška Dodjela resursa za te servise i postaviti niz za povezivanje za SQL baze podataka i postavke aplikacije za Azure Redis predmemorije niz za povezivanje.

>[AZURE.NOTE] Ako nemate račun za Azure, možete [stvoriti račun za Azure besplatne](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) u samo nekoliko minuta.

Klikom na gumb **uvođenja za Azure** vodi vas na portalu za Azure i pokreće postupak stvaranja resursi opisane pomoću predloška.

![Implementacija Azure][cache-deploy-to-azure-step-1]

1. Na plohu za **implementaciju prilagođene** odaberite Azure pretplatu koristili, pa odaberite postojeću grupu resursa ili stvorite novi i navedite mjesto grupu resursa.
2. Na plohu **parametre** Navedite naziv računa administrator (**ADMINISTRATORLOGIN** – ne koristite **administrator**), administratorsku lozinku za prijavu (**ADMINISTRATORLOGINPASSWORD**), a naziv baze podataka (**IMEBAZEPODATAKA**). Drugi parametri konfigurirana je za besplatnu aplikaciju uslugu hosting plan i donjem mogućnosti trošak SQL baze podataka i Azure Redis predmemorije koji ne dolaze s besplatne sloju.
3. Promijeniti neke od drugih postavki po želji ili zadržati na zadane postavke i kliknite **u redu**.


![Implementacija Azure][cache-deploy-to-azure-step-2]

1. Kliknite **Pregled pravne uvjete**.
2. Pročitajte uvjete na **kupnju** plohu i kliknite **kupite**.
3. Da biste započeli resurse za dodjelu resursa, kliknite **Stvori** plohu za **implementaciju prilagođene** .

Da biste pogledali tijek implementaciju sustava kliknite ikonu obavijesti, a zatim **implementacije rada**.

![Uvođenje rada][cache-deployment-started]

Status uvođenja možete pregledati na plohu **Microsoft.Template** .

![Implementacija Azure][cache-deploy-to-azure-step-3]

Po dovršetku dodjele resursa možete objaviti vaše aplikacije za Azure Visual Studio.

>[AZURE.NOTE] Sve pogreške koje se mogu pojaviti prilikom dodjele resursa procesa prikazuju se na plohu **Microsoft.Template** . Uobičajene pogreške su previše SQL poslužitelja ili previše besplatne aplikacije servisa za hostiranje tarife za pretplatu. Razrješavanje pogreške i ponovno pokrenite postupak tako da kliknete **implementirati** na plohu **Microsoft.Template** ili gumb **uvođenja za Azure** pomoću ovog praktičnog vodiča.

## <a name="publish-the-application-to-azure"></a>Objavljivanje aplikacija za Azure

U ovom koraku vodič će se objaviti aplikacije Azure i pokretanje u oblaku.

1. Desnom tipkom miša kliknite projekt **ContosoTeamStats** u Visual Studio, a zatim odaberite **Objavi**.

    ![Objavljivanje][cache-publish-app]

2. Kliknite **Aplikacije servisa Microsoft Azure**.

    ![Objavljivanje][cache-publish-to-app-service]

3. Odaberite pretplatu koristiti pri stvaranju Azure resursa, proširivanje grupa resursa koji sadrži resurse, odaberite željeni Web App pa kliknite **u redu.** Ako ste koristili gumb **uvođenja za Azure** naziv web-aplikacije započinje s **web-mjesta** Slijedi nekoliko dodatnih znakova.

    ![Odaberite Web App][cache-select-web-app]

4. Kliknite **Provjera valjanosti vezu** da biste potvrdili postavke, a zatim kliknite **Objavi**.

    ![Objavljivanje][cache-publish]

    Nakon nekoliko sekundi dok će dovršili postupak objavljivanja i pregledniku će se pokrenuti s tekućeg poslušajte aplikacije. Ako vam se DNS pogreška pri provjeri valjanosti ili objavljivanje, a samo nedavno dovrši postupka dodjele resursa za Azure resurse za aplikaciju, Pričekajte trenutak i pokušajte ponovno.

    ![Predmemorija dodali][cache-added-to-application]

U sljedećoj tablici opisane svaku akciju vezu iz aplikacije uzorka.

| Akcija                  | Opis                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Stvori novo              | Stvorite novi tima.                                                                                                                                               |
| Razdoblja za reprodukciju             | Reprodukcija razdoblja igara, ažurirati stat tima i poništite zastarjele timu podataka iz predmemorije.                                                                          |
| Očisti predmemoriju             | Poništite stat tima iz predmemorije.                                                                                                                             |
| Popis iz predmemorije         | Dohvaćanje stat tima iz predmemorije. Ako je neuspješno pronalaženje u predmemoriji, učitavanje u stat iz baze podataka i spremanje u predmemoriju za buduću upotrebu.                                        |
| Sortirano skup iz predmemorije   | Dohvaćanje stat tima iz predmemorije pomoću sortirani skupa. Ako je neuspješno pronalaženje u predmemoriji, učitavanje u stat iz baze podataka i spremite u predmemoriju pomoću sortirani skupa.  |
| Vrh 5 timovima iz predmemorije  | Dohvaćanje gornji 5 timovima iz predmemorije pomoću sortirani skupa. Ako je neuspješno pronalaženje u predmemoriji, učitavanje u stat iz baze podataka i spremite u predmemoriju pomoću sortirani skupa. |
| Učitavanje iz baze podataka            | Dohvaćanje stat tima iz baze podataka.                                                                                                                       |
| Ponovno stvaranje baze podataka              | Ponovno stvaranje baze podataka, a zatim ponovno učitajte s oglednim podacima tima.                                                                                                        |
| Uređivanje / Detalji / brisanje | Uređivanje tima, prikaz detalja o tima, tim izbrisati.                                                                                                             |


Kliknite neke od akcija, a Eksperimentirajte s dohvaća podatke iz različitih izvora. Ne razlike u vrijeme koje je potrebno da biste dovršili razne načine za dohvaćanje podataka iz baze podataka i predmemoriju.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Brisanje resursa kada završite s aplikacijom

Kada završite s aplikacijom vodiča uzorka, možete izbrisati Azure resursa koji se koriste da biste uštedjeli trošak i resurse. Ako koristite gumb **uvođenja za Azure** u odjeljku [Dodjela resursa za Azure](#provision-the-azure-resources) i sve resurse nalaze u istoj grupi resursa, možete izbrisati ih zajedno u jednom postupku tako da izbrišete grupu resursa.

1. Prijava na [portal za Azure](https://portal.azure.com) , a zatim kliknite **grupa resursa**.
2. U tekstni okvir za **Filtriranje stavki...** , upišite naziv grupe resursa.
3. Kliknite **...** s desne strane grupu resursa.
4. Kliknite **Izbriši**.

    ![Brisanje][cache-delete-resource-group]

5. Upišite naziv grupe resursa, a zatim kliknite **Izbriši**.

    ![Potvrda brisanja][cache-delete-confirm]

Nakon nekoliko sekundi dok se briše se grupa resursa i sve njegove sadrži resurse.

>[AZURE.IMPORTANT] Imajte na umu da izbrišete grupu resursa nepovratno je, a da grupu resursa i resursima u njoj trajno izbrisati. Pripazite da ne slučajno izbrišete grupu pogrešan resursa ili resursi. Ako ste stvorili resursi za hostiranje ovaj uzorak u postojeću grupu resursa, svakog resursa možete izbrisati pojedinačno s njihovim odgovarajući blades.

## <a name="run-the-sample-application-on-your-local-machine"></a>Pokrenite aplikaciju uzorka na lokalnom računalu

Da biste pokrenuli aplikaciju lokalno na vašem računalu, potrebno vam je instanca Azure Redis predmemoriju za vaše podatke u predmemoriju. 

-   Ako ste objavili aplikaciju Azure opisan u prethodnom odjeljku, možete koristiti instancu Azure Redis predmemorije koja vam je dodijeljena tijekom tog koraka.
-   Ako imate neku drugu instancu postojeće Azure Redis predmemorije, koje možete koristiti da biste pokrenuli ovaj uzorak lokalno.
-   Ako je potrebno stvoriti instancu Azure Redis predmemorije možete slijedite korake u [Stvaranje predmemoriju](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Nakon što ste odabrali ili stvorili predmemoriju za korištenje, dođite do predmemoriju na portalu za Azure i dohvatiti [naziv glavnog računala](cache-configure.md#properties) i [pristupnih tipki](cache-configure.md#access-keys) za predmemoriju. Upute potražite u članku [Konfiguriranje Redis postavke predmemorije](cache-configure.md#configure-redis-cache-settings).

1. Otvaranje u `WebAppPlusCacheAppSecrets.config` datoteku koju ste stvorili tijekom korak [Konfiguriranje aplikaciju koju želite koristiti Redis predmemorije](#configure-the-application-to-use-redis-cache) ovog praktičnog vodiča uređivaču po izboru.

2. Uređivanje u `value` atribut i zamjena `MyCache.redis.cache.windows.net` [naziv glavnog računala](cache-configure.md#properties) predmemorije, navedite ili na [primarni ili sekundarni ključ](cache-configure.md#access-keys) predmemorije kao lozinka.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Pritisnite **Ctrl + F5** da biste pokrenuli aplikaciju.

>[AZURE.NOTE] Imajte na umu da jer aplikaciju, uključujući baze podataka, izvodi lokalno i predmemoriju Redis nalazi se u Azure, predmemoriju može prikazati u odjeljku izvođenje bazu podataka. Za najbolje performanse, na istom mjestu mora biti klijentska aplikacija i Azure Redis predmemorije instance. 

## <a name="next-steps"></a>Daljnji koraci

-   Saznajte više o [Uvod u ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) [ASP.NET](http://asp.net/) web-mjesta.
-   Još primjera stvaranja web-aplikacije programa ASP.NET u aplikacije servisa za potražite u članku [Stvaranje i implementacija ASP.NET web app u aplikacije servisa za Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) iz povezivanje [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [pokazni videozapis](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Dodatne početak rada s HealthClinic.biz videozapis, potražite u članku [Početak rada za alate za razvojne inženjere za Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Saznajte više o pristup [koda najprije za novu bazu podataka](https://msdn.microsoft.com/data/jj193542) entitet okvir koji se koristi u ovom ćete praktičnom vodiču.
-   Dodatne informacije o [web-aplikacije u servisu Azure aplikacije](../app-service-web/app-service-web-overview.md).
-   Saznajte kako [monitor](cache-how-to-monitor.md) predmemoriju na portalu za Azure.

-   Istražite Azure Redis predmemorije premium značajkama
    -   [Kako konfigurirati postojanost za Premium Redis predmemoriju Azure](cache-how-to-premium-persistence.md)
    -   [Kako konfigurirati Klasteriranje za Premium Redis predmemoriju Azure](cache-how-to-premium-clustering.md)
    -   [Upute za konfiguriranje podrške za virtualne mreže za Premium Azure Redis predmemoriju](cache-how-to-premium-vnet.md)
    -   U odjeljku [Azure Redis predmemorije najčešća pitanja vezana uz](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) dodatne pojedinosti o veličini, propusnost i propusnosti s premium predmemorije.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

