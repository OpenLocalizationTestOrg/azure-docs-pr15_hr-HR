<properties 
    pageTitle="Migriranje predmemorije u Redis | Microsoft Azure"
    description="Saznajte kako migrirati upravlja predmemorije servisnih aplikacija u predmemoriju Redis Azure"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migriranje iz predmemorije upravljanih servisa Azure Redis predmemoriju

Migracija vaše aplikacije koje koriste upravljani servis predmemorije Azure Azure Redis predmemoriju je moguće napraviti s minimalnim promjena u aplikaciji, ovisno o značajke Upravljani servis predmemorije koristi predmemoriranja aplikacije. Tijekom API-ji nije potpuno isti su oni su slične i približno postojeći kod koji koristi Upravljani servis predmemoriju za pristup predmemoriju se može ponovno koristiti s minimalnim promjene. U ovoj se temi objašnjava da bi potrebnih konfiguracija i promjene aplikacije za migriranje za korištenje predmemorije Redis Azure Upravljani servis predmemorije aplikacijama, a prikazuje kako neke od značajki Azure Redis predmemorije se koristiti za implementaciju funkcionalnost Upravljani servis predmemorije predmemorije.

## <a name="migration-steps"></a>Koraka migracije

Za migriranje upravlja predmemorije servisne aplikacije za korištenje predmemorije Redis Azure potrebne su sljedeće korake.

-   Mapiranje značajke upravlja predmemorije servisa Azure Redis predmemoriju
-   Odaberite predmemorije nuditi
-   Stvoriti predmemoriju
-   Konfiguriranje predmemorije klijenti
    -   Uklanjanje servisa konfiguracije upravljanih predmemorije
    -   Konfiguriranje klijenta predmemorije pomoću značajke pakiranja NuGet StackExchange.Redis
-   Migriranje kod upravlja predmemorije usluge
    -   Povezivanje s predmemoriju pomoću ConnectionMultiplexer klase
    -   Vrste jednostavne podataka programa Access u predmemoriji
    -   Rad s .NET objekata u predmemoriji
-   Migracija stanja ASP.NET sesije i izlaznog predmemoriranja Azure Redis predmemoriju 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Mapiranje značajke upravlja predmemorije servisa Azure Redis predmemoriju

Azure Upravljani servis predmemorije i Azure Redis predmemorije slični su, ali neke od njihovih značajki implementirati na različite načine. U ovom se odjeljku opisuju neke od razlika i prikazuje postupak implementacije značajke Upravljani servis predmemorije u predmemoriji Redis Azure.

|Upravljani značajka servisa predmemorije|Upravljani predmemorije službe|Podrška za Azure Redis predmemorije|
|---|---|---|
|Imenovani predmemorije|Zadani predmemorije konfigurirana, a u predmemoriji za standardnu i Premium ponuda, do devet dodatne pod nazivom predmemorije može konfigurirati po želji.|Azure Redis predmemorije imati konfigurirati broj baza podataka (zadano od 16) koje je moguće koristiti za implementaciju sličnu funkciju da biste imenovani predmemorije. Dodatne informacije potražite u članku [zadane Redis konfiguraciju poslužitelja](cache-configure.md#default-redis-server-configuration).|
|Visoke dostupnosti|Nudi visoke dostupnosti za stavke u predmemoriji u predmemoriji ponude standardnu i Premium. Ako stavke gube se zbog pogreške, i dalje dostupne su sigurnosne kopije stavke u predmemoriji. Upisivanje predmemoriju sekundarne vrše sinkronizirano.|Visoke dostupnosti dostupan je u predmemoriju ponude standardnu i Premium koji imaju dva čvor primarni/replike konfiguraciju (svaki shard u predmemoriju Premium ima par primarni/replike). Upisivanje replike vrše asinkrono. Dodatne informacije potražite u članku [Azure Redis predmemorije cijene](https://azure.microsoft.com/pricing/details/cache/).|
|Obavijesti|Omogućuje klijenti mogli primati asinkronog obavijesti kad se pojave raznih predmemorije operacije na imenovane predmemorije.|Klijentske aplikacije možete koristiti Redis pub sub ili [Keyspace obavijesti](cache-configure.md#keyspace-notifications-advanced-settings) da biste postigli slične funkcije obavijesti.|
|Lokalne predmemorije|Sprema kopiju objekata u predmemoriji lokalno na klijentskom računalu za vrlo brz pristup.|Klijentske aplikacije potrebni za implementaciju ta je funkcija pomoću rječnika ili slične struktura podataka.|
|Eviction pravila|Ništa ili LRU. Zadani pravilnik je LRU.|Azure Redis predmemorije podržava sljedeća pravila eviction: promjenjive lru, allkeys lru, promjenjive slučajnih, allkeys-slučajnih, promjenjive-ttl, noeviction. Zadani pravilnik je promjenjive lru. Dodatne informacije potražite u članku [zadane Redis konfiguraciju poslužitelja](cache-configure.md#default-redis-server-configuration).|
|Pravila isteka|Zadani Pravilnik za istek je apsolutna i zadanoj učestalosti isteka je deset minuta. Dostupne su i animiranog i nikad pravila.|Po zadanom se stavke u predmemoriji istječe, ali je isteka moguće je konfigurirati na temelju po pisanje pomoću overloads skup predmemorije. Dodatne informacije potražite u članku [Dodavanje i dohvaćanje objekata iz predmemorije](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Regija i označavanje|Područja su podgrupama za predmemorirane stavke. Područja podržava i opaske predmemorirane stavke s dodatnim opisni nizovima naziva oznake. Područja podržava mogućnost za izvođenje operacija pretraživanja na sve označene stavke u tom području. Sve stavke unutar područja nalaze se u jedan čvor klaster predmemoriju.|Redis predmemorije sastoji se od jedan čvor (osim ako je omogućeno Redis klaster) pa ne primjenjuje pojam Upravljani servis predmemoriju područja. Redis podržava pretraživanje i zamjenskih operacije prilikom preuzimanja tipke da opisne oznake mogli uložene u ključa imena i koristiti za dohvaćanje stavke kasnije. Primjer implementacijom označavanju rješenje pomoću Redis, potražite u članku [implementacijom predmemorije označavanje u kombinaciji s Redis](http://stackify.com/implementing-cache-tagging-redis/).|
|Serijalizacije|Upravljani predmemorije podržava NetDataContractSerializer, BinaryFormatter i korištenje prilagođenih serijalizatora. Zadano je NetDataContractSerializer.|To je odgovornosti klijentska aplikacija serijalizirati .NET objekte prije da ih stavite u predmemoriju s mogućnošću serijalizatora najviše razvojni inženjer klijenta. Dodatne informacije i uzorak koda potražite u članku [Rad s objektima .NET u predmemoriji](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).|
| Emulator predmemorije | Upravljani predmemorije nudi lokalne predmemorije emulator. | Azure Redis predmemorije nema programa emulator, ali možete [pokrenuti sastavljanje MSOpenTech od redis-server.exe lokalno](cache-faq.md#cache-emulator) u ponuditi emulator. |

## <a name="choose-a-cache-offering"></a>Odaberite predmemorije nuditi

Microsoft Azure Redis predmemorije je dostupan na sljedeće razine:

-   **Osnovni** – jedan čvor. Višestruki veličina do 53 GB.
-   **Standardni** – primarni replike dva čvor. Višestruki veličina do 53 GB. 99.9% SLA.
-   **Premium** – dva čvor primarni/replike s do 10 shards. Veličina više od 6 GB 530 GB (obratite nam se više). Sve značajke standardne sloju i dodatne uključujući podršku za [Redis klaster](cache-how-to-premium-clustering.md), [Redis postojanost](cache-how-to-premium-persistence.md)i [Azure virtualne mreže](cache-how-to-premium-vnet.md). 99.9% SLA.

Svaki sloju razlikuje se značajke i cijene. Značajke možete pronaći u nastavku ovog vodiča i dodatne informacije o cijenama potražite u odjeljku [Detalji cijene predmemoriju](https://azure.microsoft.com/pricing/details/cache/).

Početnu točku za migraciju je odaberite veličinu koja odgovara veličini prethodne predmemoriju Upravljani servis predmemorije i ovisno o preduvjetima aplikacije skaliranje prema gore ili dolje. Dodatne upute o odabiru desnom nuditi Azure Redis predmemorije potražite u članku [koje nudi Redis predmemorije i veličine služe?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Stvoriti predmemoriju

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Konfiguriranje predmemorije klijenti

Kada predmemoriju se stvara i konfigurirali, na sljedeći korak je ukloniti konfiguraciju upravlja servisa predmemorije i dodati Dodaj konfiguracije Azure Redis predmemorije i referenci tako da klijenti za predmemorije mogu pristupiti predmemoriju.

-   Uklanjanje servisa konfiguracije upravljanih predmemorije
-   Konfiguriranje klijenta predmemorije pomoću značajke pakiranja NuGet StackExchange.Redis

### <a name="remove-the-managed-cache-service-configuration"></a>Uklanjanje servisa konfiguracije upravljanih predmemorije

Prije nego što klijentske aplikacije koje se mogu konfigurirati za Azure Redis predmemorije, postojeću konfiguraciju Upravljani servis predmemorije i skupa reference moraju se ukloniti Deinstaliranjem paketa upravlja NuGet predmemorije servisa.

Da biste deinstalirali paketa upravlja NuGet servisa predmemorije, desnom tipkom miša kliknite projekt klijenta u **Pregledniku rješenja** , a zatim odaberite **Upravljanje NuGet paketa**. Odaberite čvor **paketi instalirani** i upišite W**indowsAzure.Caching** u okvir za pretraživanje instaliranih paketa. Odaberite **Windows** **Azure predmemorije** (ili **Windows** **Azure predmemoriranje** ovisno o verziji programa paketa NuGet), kliknite **Deinstaliraj**, a zatim kliknite **Zatvori**.

![Deinstalacija paketa NuGet Azure upravljanih predmemorije](./media/cache-migrate-to-redis/IC757666.jpg)

Deinstaliranje paketa upravlja NuGet servisa predmemorije uklanja Upravljani servis predmemoriju sklopova i stavke Upravljani servis predmemorije u app.config ili web.config klijentske aplikacije. Budući da nije moguće ukloniti neke prilagođene postavke kada deinstaliranje paketa NuGet, otvorite web.config ili app.config i bili sigurni da su sljedeće elemente potpuno ukloniti.

Pobrinite se da u `dataCacheClients` stavka uklonjena je iz na `configSections` element. Uklanjanje čitavog `configSections` element; samo uklanjanje na `dataCacheClients` stavku, ako postoje.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Pobrinite se da u `dataCacheClients` uklanja se sekcije. Na `dataCacheClients` sekcije bit će slično kao u sljedećem primjeru.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Nakon uklanjanja konfiguraciju upravlja predmemorije servisa možete konfigurirati klijenta predmemorije kao što je opisano u sljedećoj sekciji.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Konfiguriranje klijenta predmemorije pomoću značajke pakiranja NuGet StackExchange.Redis

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migriranje kod upravlja predmemorije usluge

API-JA za klijentski program predmemorije StackExchange.Redis je slična upravljani predmemorije servis. Ovo poglavlje sadrži pregled razlike.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Povezivanje s predmemoriju pomoću ConnectionMultiplexer klase

U upravljani servis predmemorije veze u predmemoriju obradila je na `DataCacheFactory` i `DataCache` klase. U Azure Redis predmemorije se upravlja te veze na `ConnectionMultiplexer` predmete.

Dodajte sljedeće pomoću izjave na vrh bilo koje datoteke iz kojih želite pristupiti predmemoriju.

    using StackExchange.Redis
                                
Ako je ovo polje naziva ne riješi, obavezno da ste dodali StackExchange.Redis NuGet paketa kao što je opisano u [Konfiguriranje klijenti predmemorije](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

>[AZURE.NOTE] Imajte na umu da klijent StackExchange.Redis zahtijeva .NET Framework 4 ili noviji.

Da biste se povezali s instancom Azure Redis predmemorije, poziv u statični `ConnectionMultiplexer.Connect` metodu i lozinka na krajnjoj točki i ključa. Jedan pristup za zajedničko korištenje na `ConnectionMultiplexer` je instanca u aplikaciji da bi statične svojstvo koje vraća povezanog instancu, slično kao u sljedećem primjeru. To omogućuje niti sigurnih za inicijalizaciju samo jedan povezani `ConnectionMultiplexer` instance. U ovom primjeru `abortConnect` postavljen na false, što znači da će uspjeti poziv čak i ako je uspostaviti vezu u predmemoriju. Jedna ključa značajka `ConnectionMultiplexer` je da će automatski obnovili vezu u predmemoriju jednom problem s mrežom ili druge uzroci su riješi.

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

Krajnja točka predmemorije, tipke i priključaka možete preuzeti s plohu **Redis predmemoriju** za vaše instancu predmemoriju. Dodatne informacije potražite u članku [Redis predmemoriju svojstva](cache-configure.md#properties).

Uspostavljanja veze vraćanje reference na bazu podataka predmemorije Redis tako da nazovete podršku za `ConnectionMultiplexer.GetDatabase` način. Objekt koji je vratio na `GetDatabase` metodu laganih prolazni objekt i ne moraju biti pohranjen.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Klijent StackExchange.Redis koristi u `RedisKey` i `RedisValue` vrste za pristup i pohranu stavki u predmemoriji. Ove vrste mapiranje na većinu jednostavne vrste jezik, uključujući niza i često se ne koriste izravno. Redis niza osnovni vrste Redis vrijednost i mogu sadržavati razne vrste podataka, uključujući serijaliziranog binarni strujanja, a dok ne možete koristiti vrstu izravno, će koristiti metode koje sadrže `String` u nazivu. Za većinu jednostavne vrste podataka pohranu i Dohvaćanje stavki iz predmemorije pomoću na `StringSet` i `StringGet` načine, osim ako su pohrana zbirke ili druge vrste podataka Redis u predmemoriju. 

`StringSet`i `StringGet` vrlo su slične predmemorije usluga upravljanih `Put` i `Get` načine, s jednim Glavna razlika se da prije nego što postavite i se .NET objekta u predmemoriji koje morate serijalizirati je najprije. 

Pri pozivanju `StringGet`, ako postoji objekt, vratit će se i ako se ne, vraća se null. U ovom slučaju dohvaćanje vrijednosti iz izvora željene podatke i spremite ga u predmemoriji za kasnije korištenje. To se naziva predmemorije aside uzorka.

Da biste odredili isteka stavke u predmemoriji, koristite na `TimeSpan` parametar `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure Redis predmemorije možete raditi s .NET objekata, kao i vrste podataka za jednostavne, ali prije nego što mogu biti predmemorirane .NET objekt je moraju se serijalizirati. Ovo je odgovornosti razvojni inženjer. To pruža fleksibilnost za razvojne inženjere u odabir serijalizatora. Dodatne informacije i uzorak koda potražite u članku [Rad s objektima .NET u predmemoriji](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>Migracija stanja ASP.NET sesije i izlaznog predmemoriranja Azure Redis predmemoriju

Azure Redis predmemorije ima davatelja usluga za stanje ASP.NET sesije i stranice izlazne predmemorije. Da biste migrirali aplikacije koja koristi verzije Upravljani servis predmemorije od tih davatelja usluga, najprije uklonite postojeće sekcije iz vaše web.config, a konfiguriranje verzije Azure Redis predmemorije davatelja usluga. Upute o korištenju davatelji Azure Redis predmemorije platforme ASP.NET potražite u članku [ASP.NET davatelj usluga stanja sesije Azure predmemoriju Redis](cache-aspnet-session-state-provider.md) i [ASP.NET izlazne predmemorije davatelja za Azure Redis predmemoriju](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Daljnji koraci

Istražite [Azure Redis predmemorije dokumentaciju](https://azure.microsoft.com/documentation/services/cache/) vodiče, uzorka, videozapisima i drugim.

