<properties 
    pageTitle="Kako koristiti predmemorije Azure Redis | Microsoft Azure" 
    description="Saznajte kako unaprijediti performanse Azure aplikacija s predmemorije Redis Azure" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Kako koristiti Azure Redis predmemorije

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PLATFORME ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Ovaj vodič pokazuje kako započeti rad sa sustavom **Predmemorije Redis Azure**. Microsoft Azure Redis predmemorije temelji se na popularne Otvori izvor Redis predmemoriju. Je sigurna, namjenski Redis predmemoriju, upravlja Microsoft omogućuje vam pristup. Predmemoriju stvoren pomoću Azure Redis predmemorije se može pristupiti iz bilo koju aplikaciju unutar Microsoft Azure.

Microsoft Azure Redis predmemorije je dostupan na sljedeće razine:

-   **Osnovni** – jedan čvor. Višestruki veličina do 53 GB.
-   **Standardni** – primarni replike dva čvor. Višestruki veličina do 53 GB. 99.9% SLA.
-   **Premium** – dva čvor primarni/replike s do 10 shards. Veličina više od 6 GB 530 GB (obratite nam se više). Sve značajke standardne sloju i dodatne uključujući podršku za [Redis klaster](cache-how-to-premium-clustering.md), [Redis postojanost](cache-how-to-premium-persistence.md)i [Azure virtualne mreže](cache-how-to-premium-vnet.md). 99.9% SLA.

Svaki sloju razlikuje se značajke i cijene. Informacije o cijenama, potražite u članku [Predmemorije cijene pojedinosti][].

U ovom vodiču saznati kako koristiti klijent [StackExchange.Redis][] pomoću C\# kod. Scenariji u kojima je moguće uvrstiti **Stvaranje i konfiguriranje predmemoriju**, **Konfiguriranje predmemorije klijenata**i **Dodavanje i uklanjanje objekata iz predmemorije**. Dodatne informacije o korištenju Azure Redis predmemorije, pogledajte odjeljak [Daljnji koraci][] . Detaljne vodič izgradnje programa MVC ASP.NET web app s Redis predmemorije, potražite u članku [Stvaranje web-aplikacijama s Redis predmemorije](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Početak rada s Azure Redis predmemorije

Uvod u predmemoriji Redis Azure je jednostavno. Da bismo započeli, stvaranje i konfiguriranje predmemoriju. Nakon toga možete konfigurirati klijenti predmemoriju tako da mogu pristupati predmemoriju. Kada niste konfigurirali klijenti predmemorije, možete početi raditi s njima.

-   [Stvaranje predmemoriju][]
-   [Konfiguriranje predmemorije klijenti][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Stvoriti predmemoriju

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Da biste pristupili predmemoriju nakon stvaranja

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Dodatne informacije o konfiguriranju predmemoriju potražite [u](cache-configure.md)članku konfiguriranje predmemorije Redis Azure.

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Konfiguriranje predmemorije klijenti

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Kada je projekta klijent konfiguriran za predmemoriranje, možete koristiti tehnike što je opisano u sljedećim odjeljcima za rad s predmemoriju.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Rad s predmemorije

Koraci u ovom se odjeljku opisuju kako izvoditi uobičajene zadatke s predmemoriju.

-   [Povezivanje s predmemoriju][]
-   [Dodavanje i vraćanje objekata iz predmemorije][]
-   [Rad s .NET objekata u predmemoriji](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Povezivanje s predmemoriju

Da bi se programski radili s predmemoriju, potreban vam je referenca predmemoriju. Dodajte sljedeće na vrh bilo koje datoteke iz koje želite koristiti klijent StackExchange.Redis da biste pristupili programa Azure Redis predmemorije.

    using StackExchange.Redis;

>[AZURE.NOTE] Klijent StackExchange.Redis zahtijeva .NET Framework 4 ili noviji.

Veze u predmemoriju za Redis Azure upravlja u `ConnectionMultiplexer` predmete. Klase osmišljena je za zajedničko korištenje i ponovno iskoristiti cijeloj klijentska aplikacija, a ne morate stvoriti po operacija pregledava. 

Za povezivanje programa Azure Redis predmemorije i vratiti instance komponente s povezanim `ConnectionMultiplexer`, poziv u statični `Connect` način i lozinka u predmemoriji krajnjoj točki i ključ kao što su u sljedećem primjeru. Pomoću tipke generirao Portal Azure kao parametar lozinku.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Upozorenje: Nikad spremište vjerodajnica u izvornog koda. Da bi ovaj primjer jednostavne, mogu se omogućite njihov prikaz u izvornog koda. Potražite u članku [kako nizovi aplikacije i rad niza veze][] da biste saznali kako vjerodajnice za pohranu.

Ako ne želite koristiti SSL, ili postaviti `ssl=false` ili izostavili s `ssl` parametar.

>[AZURE.NOTE] Priključak koji nisu SSL onemogućeno je prema zadanim postavkama za nove predmemorije. Upute za omogućivanje priključka koji nisu SSL potražite u članku [Priključci pristup](cache-configure.md#access-ports)...

Jedan pristup za zajedničko korištenje na `ConnectionMultiplexer` je instanca u aplikaciji da bi statične svojstvo koje vraća povezanog instancu, slično kao u sljedećem primjeru. To omogućuje niti sigurnih za inicijalizaciju samo jedan povezani `ConnectionMultiplexer` instance. U ovim se primjerima `abortConnect` postavljen na false, što znači da će uspjeti poziv čak i ako je uspostaviti vezu predmemoriju Redis Azure. Jedna ključa značajka `ConnectionMultiplexer` je da će automatski obnovili vezu u predmemoriju jednom problem s mrežom ili druge uzroci su riješi.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Dodatne informacije o mogućnostima za konfiguriranje napredne veze, potražite u članku [StackExchange.Redis konfiguracije modela][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Uspostavljanja veze vraćanje reference na bazu podataka predmemorije redis tako da nazovete podršku za `ConnectionMultiplexer.GetDatabase` način. Objekt koji je vratio na `GetDatabase` metodu laganih prolazni objekt i ne moraju biti pohranjen.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Sad kad znate kako povezati instancu Azure Redis predmemorije i vraćanje reference na bazu podataka predmemorije, pogledajmo u radu s predmemoriju.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Dodavanje i vraćanje objekata iz predmemorije

Stavke biti pohranjene u i dohvaćeni iz predmemoriju pomoću na `StringSet` i `StringGet` metode.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis trgovine Većina podatke kao Redis nizove, ali ti nizovi može sadržavati razne vrste podataka, uključujući serijaliziranog binarne podatke koji se mogu koristiti pri pohrani .NET objekata u predmemoriji.

Prilikom pozivanja `StringGet`, ako postoji objekt, vratit će se, a ako ga ne, `null` , vraća se. U ovom slučaju dohvaćanje vrijednosti iz izvora željene podatke i spremite ga u predmemoriji za kasnije korištenje. To se naziva predmemorije aside uzorka.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Da biste odredili isteka stavke u predmemoriji, koristite na `TimeSpan` parametar `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Rad s .NET objekata u predmemoriji

Azure Redis predmemorije možete predmemoriju .NET objekata, kao i vrste podataka jednostavne, ali prije nego što mogu biti predmemorirane .NET objekt je moraju se serijalizirati. To je odgovornosti razvojni inženjer i pruža fleksibilnost za razvojne inženjere u odabir serijalizatora.

Jednostavan način serijalizirati objekata je koristiti u `JsonConvert` načine serijalizacije u [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) i serijalizirati i iz JSON. Sljedeći primjer prikazuje get i skupa pomoću programa `Employee` instancu objekta.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove, slijedite ove veze da biste saznali više o Azure Redis predmemoriju.

-   Pogledajte davatelji ASP.NET Azure predmemoriju Redis.
    -   [Stanje sesije Redis Azure davatelja](cache-aspnet-session-state-provider.md)
    -   [Azure Redis predmemoriju ASP.NET izlazne predmemorije davatelja](cache-aspnet-output-cache-provider.md)
-   [Omogućivanje Dijagnostika predmemoriju](cache-how-to-monitor.md#enable-cache-diagnostics) tako da možete [monitor](cache-how-to-monitor.md) stanja predmemoriju. Možete pregledavati metriku na portalu za Azure i također možete [preuzeti i pregledajte](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) ih pomoću alata za po izboru.
-   Pogledajte [dokumentacija za klijent StackExchange.Redis predmemoriju][].
    -   Azure Redis predmemorije možete pristupiti iz mnogo Redis klijenti i razvojnih jezika. Dodatne informacije potražite u članku [http://redis.io/clients][].
-   Azure Redis predmemorije može se koristiti s alatima i drugih proizvođača servise kao što su Redsmin i Redis Upravitelj radne površine.
    -   Dodatne informacije o Redsmin potražite u članku [kako dohvaćanje niza veze Azure Redis i koristiti ga Redsmin][].
    -   Pristup i Provjera podataka u predmemoriji Redis Azure s GUI pomoću [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
-   Potražite u dokumentaciji [redis][] i Saznajte više o [redis vrste podataka][] i [Uvod petnaest minuta da biste Redis vrste podataka][].



<!-- INTRA-TOPIC LINKS -->
[Daljnji koraci]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Stvaranje predmemoriju]: #create-cache
[Configure the cache]: #enable-caching
[Konfiguriranje predmemorije klijenti]: #NuGet
[Working with Caches]: #working-with-caches
[Povezivanje s predmemoriju]: #connect-to-cache
[Dodavanje i vraćanje objekata iz predmemorije]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Dohvaćanje niza veze Azure Redis i koristiti s Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis konfiguracije modela]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Informacije o cijenama u predmemoriju]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[Dokumentacija za klijent StackExchange.Redis predmemorije]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis vrste podataka]: http://redis.io/topics/data-types
[petnaest minuta Uvod u vrste podataka Redis]: http://redis.io/topics/data-types-intro

[Kako funkcioniraju nizovi aplikacije i nizove veze]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


