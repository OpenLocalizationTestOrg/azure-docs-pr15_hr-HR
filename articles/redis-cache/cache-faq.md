<properties 
    pageTitle="Azure Redis predmemorije najčešća pitanja vezana uz | Microsoft Azure" 
    description="Saznajte odgovore na najčešća pitanja, obrazaca i najbolje prakse za Azure Redis predmemoriju" 
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
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Azure Redis predmemorije najčešća Pitanja

Saznajte odgovore na najčešća pitanja, uzoraka i najbolje prakse za Azure Redis predmemoriju. 


## <a name="what-if-my-question-isnt-answered-here"></a>Što ako moje pitanje niste pronašli odgovor ovdje?

Ako svoje pitanje nije ovdje navedeno, Javite nam i pomoći ćemo vam pronašli odgovor.

-   Možete postavite pitanje u [Disqus niti](#comments) na kraju u ovim najčešćim Pitanjima i sudjelovati s tim Azure predmemorije i ostali članovi zajednice o ovog članka.
-   Da biste doseći širu publiku, postavite pitanje na [Forumu MSDN predmemoriju za Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) i sudjelovati s tim Azure predmemorije i ostali članovi zajednice.
-   Ako želite poslati zahtjev za značajku možete poslati zahtjeve i ideje [Redis govorne predmemorije korisnika Azure](https://feedback.azure.com/forums/169382-cache).
-   Poruke e-pošte možete poslati i nam na [Vanjskim povratnih informacija predmemorije Azure](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Osnove Azure Redis predmemorije

Najčešća pitanja u ovom odjeljku pokriva dio osnove Azure Redis predmemoriju.

-    [Što je predmemorije Redis Azure?](#what-is-azure-redis-cache)
-    [Kako mogu početi s predmemorije Redis Azure?](#how-can-i-get-started-with-azure-redis-cache)

Sljedeća najčešća pitanja Popratno osnovni koncepti i pitanja o Azure Redis predmemorije i su odgovoriti na jedan od druge najčešća pitanja vezana uz sekcije.

-   [Koje nudi Redis predmemorije i veličine koristiti?](#what-redis-cache-offering-and-size-should-i-use)
-   [Što Redis predmemorije klijenti mogu koristiti?](#what-redis-cache-clients-can-i-use)
-   [Je li lokalne emulator Azure predmemoriju Redis?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Kako praćenje stanja i performanse Moje predmemorije?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Planiranje najčešća pitanja

-   [Koje nudi Redis predmemorije i veličine koristiti?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure Redis predmemorije performansi](#azure-redis-cache-performance)
-   [U regiju treba li pronađite Moje predmemorije?](#in-what-region-should-i-locate-my-cache)
-   [Kako se naplatiti za predmemoriju Redis Azure?](#how-am-i-billed-for-azure-redis-cache)
-   [Mogu li koristiti predmemorije Redis Azure s Azure Government Cloud ili oblaka Kina Azure?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Najčešća pitanja za razvoj

-   [Što učiniti StackExchange.Redis mogućnosti konfiguracije?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Što Redis predmemorije klijenti mogu koristiti?](#what-redis-cache-clients-can-i-use)
-   [Je li lokalne emulator Azure predmemoriju Redis?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Način pokretanja naredbe Redis?](#how-can-i-run-redis-commands)
-   [Zašto se Azure Redis predmemorije ne postoji referenca Biblioteka klasa MSDN kao što su neke od drugih servisa Azure?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Mogu li koristiti Azure Redis predmemorije kao predmemoriju za sesije i?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Najčešća pitanja sigurnosti

-   [Ako omogućite koje nisu SSL priključak za povezivanje s Redis?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Najčešća pitanja proizvodnje

-   [Što su radni najbolje postupke?](#what-are-some-production-best-practices)
-   [Što su na pitanja vezana uz prilikom korištenja uobičajenim naredbama Redis?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Kako usporednih i testirajte performanse Moje predmemorije?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Važne pojedinosti o ThreadPool growth](#important-details-about-threadpool-growth)
-   [Omogućivanje GC server da biste dobili dodatne propusnost na klijentskom računalu prilikom korištenja StackExchange.Redis](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Nadzor i najčešća pitanja za otklanjanje poteškoća

Najčešća pitanja u ovom odjeljku pokrivaju uobičajene nadzor i otklanjanje poteškoća pitanja. Dodatne informacije o nadzor i vaše instance Azure Redis predmemoriju za otklanjanje poteškoća potražite u članku [kako nadzirati Azure Redis predmemorije](cache-how-to-monitor.md) i [Otklanjanje poteškoća s predmemorije Redis Azure](cache-how-to-troubleshoot.md).

-   [Kako praćenje stanja i performanse Moje predmemorije?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Moje predmemorije za pohranu računa postavki dijagnostike mijenjati, što se dogodilo?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Zašto je omogućeno Dijagnostika za neke nove predmemorije, ali ne i ostalima?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Zašto se prikazuju vremensko ograničenje?](#why-am-i-seeing-timeouts)
-   [Zašto moj klijent prekinuta iz predmemorije?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Najčešća pitanja prethodnog nuditi predmemorije

-   [Koje nudi Azure predmemorije je za mene?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Što je predmemorije Redis Azure?

Azure Redis predmemorije temelji se na popularne Otvori-izvor [Redis predmemoriju](http://redis.io). To omogućuje vam pristup sigurne, namjenski Redis predmemoriju, upravljanim upravlja Microsoft i može pristupiti s bilo koju aplikaciju unutar Azure. Detaljnije pregled, potražite u članku [Azure Redis predmemorije](https://azure.microsoft.com/services/cache/) stranice proizvoda na Azure.com.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Kako mogu početi s predmemorije Redis Azure?

Početak rada s Azure Redis predmemorije na nekoliko načina.

-    Možete provjeriti nešto naš vodiči za dostupne za [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)i [Python](cache-python-get-started.md). 
-    Pogledajte [Kako izraditi visoke performanse aplikacije pomoću Microsoft Azure Redis predmemoriju](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
-    Preporučujemo da pročitate klijenti koja odgovara jeziku razvoj projekta da biste saznali kako koristiti Redis potražite u dokumentaciji klijenta. Postoji mnogo Redis klijenti koje mogu koristiti u sklopu Azure Redis predmemoriju. Popis Redis klijenata, potražite u članku [http://redis.io/clients](http://redis.io/clients).


Ako već nemate račun za Azure, možete učiniti sljedeće:

-    [Otvorite račun za Azure besplatno](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Dobit ekipa koje je moguće koristiti da biste isprobali plaćenu servisa Azure. Čak i kada koriste na kredita, možete zadržati račun i pomoću besplatnog servisa Azure i značajke.
-    [Aktiviranje Visual Studio pretplatnika prednosti](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Pretplate na MSDN vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Koje nudi Redis predmemorije i veličine koristiti?
Svaki Azure Redis predmemorije nudi različite razine **veličine**, **propusnosti**, **visoke dostupnosti**i **SLA** mogućnosti.

Slijede pitanja vezana uz za odabir predmemorije nuditi.

-   **Memorija**: U osnovne i standardne razine nude 250 MB – 53 GB. Razina Premium nudi do 530 GB s više dostupne [na zahtjev](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Dodatne informacije potražite u članku [Azure Redis predmemorije cijene](https://azure.microsoft.com/pricing/details/cache/).
-   **Performanse mreže**: Ako imate radno opterećenje koje je potrebno visoke propusnost sloju Premium nudi više propusnosti u usporedbi s standardni prikaz ili Basic. U svakom sloju veće veličina predmemorije imati više propusnosti zbog podlozi VM koji hostira predmemoriju. Potražite dodatne informacije [u tablici u nastavku](#cache-performance) .
-   **Propusnost**: Premium u sloju nudi Maksimalna dostupna propusnost. Ako poslužitelj predmemorije ili klijenta dostigne ograničenja propusnosti, primit ćete vremenska ograničenja na klijentskoj strani. Dodatne informacije potražite u sljedećoj tablici.
-   **Visoke dostupnosti/SLA**: Azure Redis predmemorije jamčiti da standardna/Premium predmemorije će biti dostupna najmanje 99.9% od vremena. Dodatne informacije o oglednim SLA potražite u članku [Azure Redis predmemorije cijene](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Na SLA pokriva samo veza s krajnje točke predmemoriju. Na SLA ne obuhvaća Zaštita od gubitka podataka. Preporučujemo korištenje značajke za postojanost Redis podataka u sloju Premium da biste povećali otpornost od gubitka podataka.
-   **Redis postojanost podataka**: Premium u sloju omogućuje zadržava podataka predmemorije u račun za Azure prostora za pohranu. U predmemoriju Basic/standardna svi podaci se pohranjuju samo u memoriji. U slučaju podlozi infrastrukture postoje problemi može biti mogućem gubitku podataka. Preporučujemo korištenje značajke za postojanost Redis podataka u sloju Premium da biste povećali otpornost od gubitka podataka. Azure Redis predmemorije RDB i AOF (uskoro dostupno) mogućnosti nudi u Redis postojanost. Dodatne informacije potražite u članku [kako konfigurirati postojanost za Premium Redis predmemoriju Azure](cache-how-to-premium-persistence.md).
-   **Redis klaster**: da biste stvorili predmemorira veća od 53 GB ili shard podacima preko više Redis čvorove, možete koristiti Redis Klasteriranje, koji je dostupan u sloju Premium. Svaki čvor sastoji se od par primarni/replike predmemoriju za visoke dostupnosti. Dodatne informacije potražite u članku [kako konfigurirati Klasteriranje za Premium Redis predmemoriju Azure](cache-how-to-premium-clustering.md).
-   **Napredna Zaštita i odvajanja mreže**: implementacija Azure virtualne mreže (VNET) nudi bolja sigurnost i odvajanja za vaše Azure Redis predmemorije podmreže, pravilnici kontrole programa access, i ostalih značajki da biste dodatno ograničiti pristup. Dodatne informacije potražite [u](cache-how-to-premium-vnet.md)članku konfiguriranje podrške za virtualne mreže za Premium Azure Redis predmemoriju.
-   **Konfiguriranje Redis**: U standardnu i Premium razine, možete konfigurirati Redis Keyspace obavijesti.
-   **Maksimalni broj klijentske veze**: Premium u sloju nudi maksimalni broj klijenti koje je moguće je povezati s Redis s većim brojem veza za veće veličine predmemorije. [Rješenje odnose se na stranicu cijene detalje](https://azure.microsoft.com/pricing/details/cache/).
-   **Namjenski Core Redis poslužitelja**: U sloju Premium svih veličina predmemorije su namjenski core za Redis. Osnovni/standarda za razine u ćeliju C1 veličina i noviji ste namjenski core Redis poslužitelja.
-   **Redis je jednom niti** tako da imate više od dva jezgri ne nudi dodatnu pogodnost putem pojavljuju samo dva jezgri, ali veće VM obično imaju više propusnosti od manje veličine. Ako poslužitelj predmemorije ili klijenta dostigne ograničenja propusnosti, dobit ćete vremenska ograničenja na klijentskoj strani.
-   **Poboljšanja performansi**: predmemorije u sloju Premium primjenjuju se na hardveru koja ima procesora koji se brže i daje bolje performanse u usporedbi s sloju Basic ili Standard. Predmemorija sloju Premium imati veću propusnost i donjem latencies.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure Redis predmemorije performansi

U sljedećoj su tablici prikazane vrijednosti Maksimalna propusnost opaženih tijekom testiranja raznih veličina standardnu i Premium predmemorira pomoću `redis-benchmark.exe` iz programa Iaas VM protiv krajnju točku Azure Redis predmemoriju. Imajte na umu da ne jamči te vrijednosti i postoji bez SLA o tim brojevi, ali mora biti uobičajeni. Trebali biste učitali vlastite aplikacija za određivanje veličine desnom predmemoriju za svoju aplikaciju za testiranje.

Iz ove tablice ne možemo možete crtati sljedeće zaključaka.

-   Propusnost za predmemorije koji su iste veličine veća je u sloju Premium to standardni sloju. Na primjer, 6 GB predmemorije, propusnost P1 je 140K RPS to 49 K za C3.
-   S Redis Klasteriranje, propusnost povećava Linearno kako povećavate broj shards (čvorove) u klasteru. Na primjer, ako stvorite P4 Klaster od 10 shards, zatim dostupna propusnost je 250K * 10 = 2,5 milijuna RPS.
-   Propusnost za povećavanje veličine ključeva veća je u sloju Premium to standardni sloju.

| Cijene sloju             | Veličina   | Jezgri procesora | Raspoložive propusnosti                                    | Veličina 1 KB ključ                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Veličina predmemorije Standard** |        |           | **Megabita u sekundi (Mb/s) / megabajta u sekundi (MB/s)** | **Za u sekundi (RPS)**            |
| C0                       | 250 MB | Zajedničko korištenje    | 5 / 0.625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12.5                                             | 12200                                    |
| C2                       | 2,5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62.5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **Veličina predmemorije Premium**  |        | **Jezgri procesora po shard**  |                                         | **Za u sekundi (RPS) po shard** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Upute za preuzimanje alata za Redis kao što su `redis-benchmark.exe`, potražite u članku na [način pokretanja naredbe Redis?](#cache-commands) sekciju.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>U regiju treba li pronađite Moje predmemorije?

Za najbolje performanse i najniže Latencija, pronađite Azure Redis predmemorije u na istom području predmemorije klijentsku aplikaciju.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Kako se naplatiti za predmemoriju Redis Azure?

Azure cijene Redis predmemorije je [ovdje](https://azure.microsoft.com/pricing/details/cache/). Cijene stranica sadrži popis cijene kao na satu. Predmemorija se naplatiti na temelju-minutni od vremena predmemorije stvorenih do vremena koji se brišu predmemoriju. Ne postoji mogućnost za zaustavljanje ili pauziranje naplata predmemoriju.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Mogu li koristiti predmemorije Redis Azure s Azure Government Cloud ili oblaka Kina Azure?

Da, Azure Redis predmemorije dostupna je u oblak državne Azure i Azure Kina oblaka. Imajte na umu da URL-ovi za pristup i upravljanje Azure Redis predmemorije različite u oblak državne Azure i Azure Kina oblaka usporedbi sa Azure javno oblaka. Dodatne informacije u obzir prilikom korištenja predmemorije Redis Azure s Azure Government Cloud i Azure Kina oblaka potražite u članku [Azure državne baze podataka – Azure Redis predmemorije](../azure-government/documentation-government-services-database.md#azure-redis-cache) i [Azure Kina oblak – Azure Redis predmemoriju](https://www.azure.cn/documentation/services/redis-cache/).

Informacije o korištenju Azure Redis predmemorije sa servisom PowerShell u oblak državne Azure i Azure Kina oblaka, potražite u članku [kako se povezati sa Azure Government Cloud ili Azure Kina oblaka](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Što učiniti StackExchange.Redis mogućnosti konfiguracije?

StackExchange.Redis ima brojne mogućnosti. U ovom se odjeljku govori o nekim uobičajenim postavkama. Detaljnije informacije o mogućnostima StackExchange.Redis potražite u članku [Konfiguracija StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Opis|Preporuka
---|---|---
AbortOnConnectFail|Kada postavljen na true, veza će ne ponovno povezati nakon neuspješne mreže.|Postavite na false, a omogućuju StackExchange.Redis ponovno povezati automatski.
ConnectRetry|Broj ponavljanja pokušaja povezivanja tijekom početni povezati.| Pogledajte sljedeće bilješke smjernicama. |
ConnectTimeout|Prekoračenje vremena u milisekundama za povezivanje operacije.| Pogledajte sljedeće bilješke smjernicama. |

U većini slučajeva potrebne su zadane vrijednosti klijent. Možete dodatno prilagoditi mogućnosti na temelju svoje radno opterećenje.

-   **Ponovne pokušaje**
    -   Za ConnectRetry i ConnectTimeout opće smjernice je neće uspjeti brzo i pokušajte ponovno. Temelji se na svoje radno opterećenje i koliko je vremena na izračunati prosjek ga traje za vaš klijent za izdavanje Redis naredbe i dobivate odgovor.
    -   Obavijestite StackExchange.Redis automatski se ponovno povezali umjesto Provjera stanja veze i povezati sami. **Izbjegavajte korištenje svojstvo ConnectionMultiplexer.IsConnected**.
    -   Snowballing - ponekad može naiđete na problem kada su ponovni, a to snowballs i nikad ne oporavlja. U ovom slučaju razmotrite korištenje pokušaj algoritam eksponencijalne backoff kao što je opisano u [ponovite opće smjernice](best-practices-retry-general.md) objavljuje grupi Microsoft Patterns i postupci.
-   **Vremensko ograničenje vrijednosti**
    -   Razmislite o svoje radno opterećenje, a zatim sukladno tome postaviti vrijednosti. Ako su spremanje velike vrijednosti, postavite vremensko ograničenje na veću vrijednost.
    -   Postavljanje `AbortOnConnectFail` na false i omogućite StackExchange.Redis ponovno povezali umjesto vas.
    -   Koristite instancu ConnectionMultiplexer za aplikaciju. Na LazyConnection možete koristiti da biste stvorili instancu koji je vratio svojstva veze, kao što je prikazano u [predmemoriju pomoću klase ConnectionMultiplexer za povezivanje](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
    -   Postavljanje na `ConnectionMultiplexer.ClientName` svojstvo programa instancu jedinstveni naziv aplikacije dijagnostičkih svrhe.
    -   Korištenje više `ConnectionMultiplexer` instance za prilagođene radnih opterećenja.
    -   Ako imate različitim Učitaj u aplikaciji, slijedite ovaj model. Ako, na primjer:
    -   Imate jedan multiplexer za velike tipke.
    -   Imate jedan multiplexer za small tipke.
    -   Možete postaviti različite vrijednosti za vremensko ograničenje veze i ponovnog pokušaja za svaku ConnectionMultiplexer koju koristite.
    -   Postavljanje na `ClientName` svojstvo na svakom multiplexer će vam pomoći u Dijagnostika.
    -   To će dovesti do više pojednostavnjen Latencija po `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Što Redis predmemorije klijenti mogu koristiti?

Jedna od sjajno stvari o Redis su koje je moguće više klijenata mnogo različitih razvojnih jezika za podršku. Trenutni popis klijenata potražite u članku [Redis klijente](http://redis.io/clients). Vodiči za koji pokrivaju nekoliko jezicima i klijenti, informirajte [se o korištenju Azure Redis predmemorije](cache-dotnet-how-to-use-azure-redis-cache.md) i kliknite željeni jezik iz alat za jezik prebacivanje na vrhu članka.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Je li lokalne emulator Azure predmemoriju Redis?

Postoji bez lokalne emulator Azure predmemoriju Redis, ali možete pokrenuti MSOpenTech verziju redis server.exe iz [Redis alate naredbenog retka](https://github.com/MSOpenTech/redis/releases/) na lokalnom računalu i povezati ga da biste pristupili slično iskustvo lokalne predmemorije Emulator-a, kao što je prikazano u sljedećem primjeru.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Po želji možete konfigurirati [redis.conf](http://redis.io/topics/config) datoteka za pobliže podudaranje [zadane postavke predmemorije](cache-configure.md#default-redis-server-configuration) online Azure Redis predmemoriju po želji.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Način pokretanja naredbe Redis?

Možete koristiti bilo koju naredbu navedeni na [Redis naredbe](http://redis.io/commands#) osim naredbi na [Redis naredbe koje nisu podržane u predmemoriji Redis Azure](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). Pokretanje naredbe Redis imate nekoliko mogućnosti.

-   Ako imate standardni prikaz ili Premium predmemorije, možete pokrenuti Redis naredbi pomoću [Redis konzolu](cache-configure.md#redis-console). To omogućuje siguran način za izvođenje naredbe Redis na portalu za Azure.
-   Možete koristiti i alata Redis naredbenog retka. Da biste ih koristili, poduzeti sljedeće korake.
-   Preuzmite [Redis alate naredbenog retka](https://github.com/MSOpenTech/redis/releases/).
-   Povezivanje s korištenjem predmemorije `redis-cli.exe`. Prenesite na predmemorije krajnjoj točki pomoću prijeđite na -h i tipku pomoću-kao što je prikazano u sljedećem primjeru.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Napomena da alata naredbenog retka Redis raditi SSL priključak, ali možete koristiti uslužni program kao što su `stunnel` za sigurno povezivanje alata za priključak SSL slijedeći upute u blogu [Objavljivanje ASP.NET sesije stanje davatelj usluga za Redis pretpregled izdanja](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Zašto se Azure Redis predmemorije ne postoji referenca Biblioteka klasa MSDN kao što su neke od drugih servisa Azure?

Microsoft Azure Redis predmemorije temelje se na popularne Otvori izvor Redis predmemorije, a mogu pristupiti razna [Redis klijenti](http://redis.io/clients) koje su dostupne za mnogo programskog jezika. Svaki klijent ima vlastitu API koji vam se čini pozive na instancu predmemorije Redis pomoću [Redis naredbe](http://redis.io/commands).

Budući da razlikuje svaki klijent, nema ne jednu središnje predmete referencu na MSDN; Umjesto toga svaki klijent zadržava vlastitu referentnu dokumentaciju. Osim referentnu dokumentaciju postoji nekoliko vodiče s prikazom načina za početak rada s Redis Azure predmemoriju pomoću jezicima i klijenti predmemoriju. Da biste pristupili ovim praktičnim vodičima, informirajte [se o korištenju Azure Redis predmemorije](cache-dotnet-how-to-use-azure-redis-cache.md) , a zatim kliknite željeni jezik iz alat za jezik prebacivanje na vrhu članka.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Mogu li koristiti Azure Redis predmemorije kao predmemoriju za sesije i?

Da, da biste upotrijebili predmemoriju za sesije i Azure Redis predmemorije, navedite niz za povezivanje vašoj instanci Azure Redis predmemorije u `session.save_path`.

>[AZURE.IMPORTANT] Prilikom korištenja Azure Redis predmemorije kao predmemoriju sesiju PHP, morate URL kodiranje sigurnosni ključ koji se koristi za povezivanje s predmemorije, kao što je prikazano u sljedećem primjeru.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Ako ključ nije kodirati URL-om, možda ćete dobiti iznimku sličnu ovoj:`Failed to parse session.save_path`

Dodatne informacije o korištenju Redis predmemorije kao predmemoriju sesije i PhpRedis klijenta, potražite u članku [sesije i događajima](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Ako omogućite koje nisu SSL priključak za povezivanje s Redis?

Redis poslužitelj ne podržava SSL iz okvir, ali ne Azure Redis predmemoriju. Ako se povezujete s Azure Redis predmemorije, a vaš klijent podržava SSL, kao što su StackExchange.Redis, trebali biste koristiti SSL.

Imajte na umu priključka koji nisu SSL je onemogućio zadano za nove instance predmemorije Redis Azure. Ako vaš klijent ne podržava SSL, morate omogućiti priključka koji nisu SSL slijedeći upute u odjeljku [pristup priključke](cache-configure.md#access-ports) u članku [Konfiguriranje predmemorije u predmemoriji Redis Azure](cache-configure.md) .

Kao što su redis Alati `redis-cli` raditi priključak SSL, ali možete koristiti uslužni program kao što su `stunnel` za sigurno povezivanje alata za priključak SSL slijedeći upute u blogu [Objavljivanje ASP.NET sesije stanje davatelj usluga za Redis pretpregled izdanja](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

Upute za preuzimanje alata Redis, potražite u [način pokretanja naredbe Redis?](#cache-commands) sekciju.



### <a name="what-are-some-production-best-practices"></a>Što su radni najbolje postupke?

-   [Najbolje prakse StackExchange.Redis](#stackexchangeredis-best-practices)
-   [Konfiguracija i koncepti](#configuration-and-concepts)
-   [Testiranje performansi](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Najbolje prakse StackExchange.Redis

-   Postavljanje `AbortConnect` FALSE, zatim omogućuju ConnectionMultiplexer ponovno povezati automatski. [Pogledajte detalje](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Ponovno korištenje na ConnectionMultiplexer – stvoriti novu za svaki zahtjev za potvrdu. Na `Lazy<ConnectionMultiplexer>` uzorak [ovi](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) preporučuje se.
-   Redis funkcionira najbolje manje su vrijednosti, pa preporučujemo chopping veći podatke u više tipki. U [to Redis rasprave](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)100 kb se smatra "velike". U [ovom se članku](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) potražite na primjer problem koji može uzrokovati velikih vrijednosti.
-   Konfiguriranje [postavki ThreadPool](#important-details-about-threadpool-growth) da biste izbjegli vremenska ograničenja.
-   Koristite najmanje connectTimeout zadani 5 sekundi. To želite dati StackExchange.Redis dovoljno vremena za ponovno uspostaviti vezu, u slučaju mreže blip.
-   Imajte na umu troškove performanse vezane uz različite operacije koristite. Ako, primjerice, na `KEYS` naredba O(n) postupak i moraju se izbjegavati. [Web-mjesta redis.io](http://redis.io/commands/) sadrži detalje oko složenosti vremena za svaku operaciju koja ga podržava. Kliknite svaku naredbu da biste vidjeli složenosti za svaku operaciju.

#### <a name="configuration-and-concepts"></a>Konfiguracija i koncepti

-   Koristite standardni prikaz ili Premium sloju za sustave proizvodnje. Osnovni sloju je sustav jedan čvor s bez replikacije podataka i bez SLA. Osim toga, koristite najmanje predmemorije C1. Scenariji za jednostavne razvojni i testiranje zaista namijenjene C0 predmemorije.
-   Imajte na umu da je Redis u spremištu podataka **U memoriji** . U [ovom se članku](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) tako da budu na umu slučajevi u kojima se može dogoditi gubitka podataka.
-   Razvoj sustava tako da ga možete rukovati veze blips [zbog zakrpa i prebacivanje](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Testiranje performansi

-   Pokrenite pomoću `redis-benchmark.exe` da biste dobili na dojam moguće propusnost prije pisanja vlastite mjerača performansi testira. Imajte na umu te redis usporednih ne podržava SSL, pa će da biste [omogućili priključak koji nisu SSL putem portala za Azure](cache-configure.md#access-ports) prije nego što pokrenete test. Primjeri, potražite u članku [kako usporednih i testirajte performanse Moje predmemorije?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   Klijent VM koji se koriste za testiranje mora biti u području isti kao vašoj instanci predmemorije Redis.
-   Preporučujemo korištenje Dv2 VM niz za vaš klijent, kao što su bolje hardver i dat će najbolje rezultate.
-   Provjerite postoji li vaš klijent VM odaberete barem suvišni računalstvo propusnosti mogućnost kao predmemoriju želite testirati. 
-   Omogućivanje VRSS na klijentskom računalu ako se nalazite u sustavu Windows. [Pogledajte detalje](https://technet.microsoft.com/library/dn383582.aspx).
-   Instance Redis Premium sloju će imati bolje mreže latencije i propusnost jer su pokrenuti na bolje hardver za procesora i mrežne.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Što su na pitanja vezana uz prilikom korištenja uobičajenim naredbama Redis?

-   Ne pokrećite neke naredbe Redis koji zahtijeva puno vremena da biste dovršili bez objašnjenje učinka te naredbe.
    -   Na primjer, niste pokrenuli [tipke](http://redis.io/commands/keys) naredbu u radni kao što je može potrajati da biste se vratili ovisno o broju tipke. Redis je poslužitelj jednom niti i obrađuje naredbe jedan po jedan. Ako imate druge naredbe izdavanja nakon tipke, oni neće obraditi dok Redis obrađuje naredbu tipke. [Web-mjesta redis.io](http://redis.io/commands/) sadrži detalje oko složenosti vremena za svaku operaciju koja ga podržava. Kliknite svaku naredbu da biste vidjeli složenosti za svaku operaciju.
-   Veličine ključeva - služe small ključ/ili velike ključ/vrijednosti? Općenito govoreći ovisi o scenarija. Ako je scenariju potreban veći tipke pa možete prilagoditi u ConnectionTimeout i ponovno pokušajte vrijednosti i prilagoditi logiku pokušajte ponovno. Iz perspektive Redis poslužitelj, manje vrijednosti su opaženih da bi bolje performanse.
-   To ne znači da ne možete spremati veće vrijednosti u Redis; morate biti svjesni Imajte na umu sljedeće. Latencies bit će veće. Ako imate jedan skup podataka koji su veći i onu koja je manja, možete koristiti više instanci ConnectionMultiplexer, svaki konfiguriran pomoću drugačiji skup vremenskog ograničenja i pokušajte ponovno vrijednosti, kao što je opisano u prethodnom odjeljku [što to mogućnosti konfiguracije StackExchange.Redis učinite](#cache-configuration) .



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Kako usporednih i testirajte performanse Moje predmemorije?

-   [Omogućivanje Dijagnostika predmemoriju](cache-how-to-monitor.md#enable-cache-diagnostics) tako da možete [monitor](cache-how-to-monitor.md) stanja predmemoriju. Možete pregledavati metriku na portalu za Azure i također možete [preuzeti i pregledajte](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) ih pomoću alata za po izboru.
-   Redis benchmark.exe možete koristiti da biste učitali test poslužitelja Redis.
-   Učitavanje testirate klijenta i predmemoriju Redis provjerite jesu li u istom području.
-   Korištenje redis cli.exe i nadzirati predmemoriju pomoću naredbe za informacije.
-   Ako vaš učitavanja uzrokuje opterećenje memorije Fragmentacija zatim koje treba proširenjem veće veličina predmemorije.
-   Upute za preuzimanje alata Redis, potražite u [način pokretanja naredbe Redis?](#cache-commands) sekciju.

Slijedi primjer korištenja redis benchmark.exe. Točne rezultate, pokrenite sljedeću naredbu iz VM u području isti kao predmemoriju.

-   Test Pipelined postavljanje zahtjeva za korištenje 1 tereta k

    redis benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t postavljanje - n 1000000 -d 1024 - P 50
    
-   Test Pipelined DOBITI zahtjeve pomoću 1 tereta k. 
    Napomena: Pokretanje SKUP testiranje prikazane iznad najprije za popunjavanje predmemorije
    
    redis benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t DOBITI -n 1000000 - d 1024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Važne pojedinosti o ThreadPool growth

CLR ThreadPool sadrži dvije vrste niti - "Tempiranja" i "/ i dovršetka priključak" (ili IOCP) niti. 

-   Radnih niti se koriste za elemente kao što su obrada `Task.Run(…)` ili `ThreadPool.QueueUserWorkItem(…)` metode. Ove niti i koristi razne komponente na CLR kada posao mora dogoditi na pozadini niti.
-   IOCP niti se koriste kada se dogodi asinkronog IO (npr. čitanju mreža). 

Prostor niti pruža novi radnih niti ili/i dovršetka niti na zahtjev (bez sve ograničavanje) sve dok ne postignete postavku "Najmanja" za svaku vrstu niti. Prema zadanim postavkama najmanji broj niti postavljen je na broj procesora u sustavu. 

Kada broju postojeću (zauzet) niti pristupa "najmanji" broj niti, u ThreadPool će ograničenja Brzina kojom je umetati ga novi niti jedan niti po 500 milisekundi. To znači da ako sustav dobije neprekidnog rada poslovne potrebe za IOCP niti, je obradit će koji funkcioniraju vrlo brzo. Međutim, ako je neprekidnog rada rada više od konfigurirane postavke "Najmanja", će biti neke odgode obrade dio posla kao u ThreadPool čeka u neku od sljedeće dvije stvari da se dogodi.

1. Postojeće niti postaje besplatne za obradu rad.
1. Nema postojeće niti postaje besplatno za 500ms, pa je stvoriti novu niti.

Zapravo, to znači da kada broj zauzet niti je veći od niti Min, koje vjerojatno platili se 500ms Odgoda prije mrežni promet obrade aplikacija. Osim toga, važno je da biste Imajte na umu da postojeći niti ostaje neaktivnosti dulje od 15 sekundi (koja se temelji na što se ne zaboravite), ona će se očistiti i ponovite ovaj ciklusa growth i kalo.

Ako ne možemo pogledajte primjer poruke o pogrešci s StackExchange.Redis (sastavljanje 1.0.450 ili noviji), vidjet ćete da je sada ispisuje Statistika ThreadPool (pogledajte IOCP i TEMPIRANJA detalja o nastavku).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

U primjeru iznad vidjet ćete da se za IOCP niti nalaze 6 zauzet niti i sustav nije konfigurirano tako da omogućuje 4 minimalne niti. U ovom slučaju klijent želite imati vjerojatno vidjeti dva 500 ms kašnjenja jer 6 > 4.

Imajte na umu StackExchange.Redis mogu pogoditi vremensko ograničenje ako growth IOCP ili RADNIH niti dobiva ograničio vrijeme.

### <a name="recommendation"></a>Preporuka

Uz taj podatak, preporučujemo da kupci postavite vrijednost minimalne konfiguracije za IOCP i RADNIH niti nešto veće od zadane vrijednosti. Ne možemo ne može dati one-size-fits-all smjernice o što ta vrijednost mora biti jer je vrijednost udesno za jednu aplikaciju bit će previše visoke/niske za neku drugu aplikaciju. Tu postavku možete i utjecati na performanse druge dijelove složene aplikacije, tako da svaki klijent nije potrebno precizno tu postavku da biste svojim potrebama. Dobro početno mjesto 200 ili 300, a zatim testirajte i dotjerati prema potrebi.

Upute za konfiguriranje ove postavke:

-   U ASP.NET, koristite [postavke konfiguracije "minIoThreads"][] u odjeljku s `<processModel>` konfiguracije element u web.config. Ako su pokrenuti unutar Azure web-mjesta, ta postavka je izložen putem mogućnosti konfiguracije. Međutim, trebali biste i dalje ćete moći Postavi programski (potražite u nastavku) iz načina Application_Start u global.asax.cs.

> **Važnih Napomena:** vrijednost navedenu u taj element konfiguracije je postavka *po core* . Na primjer, ako imate 4 core računala i želite postavke minIOThreads biti 200 prilikom izvođenja, koristit ćete `<processModel minIoThreads="50"/>`.

-   Izvan ASP.NET, koristite [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API-JA.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Omogućivanje GC server da biste dobili dodatne propusnost na klijentskom računalu prilikom korištenja StackExchange.Redis

Omogućivanje GC poslužitelja možete optimizirati klijent i omogućuju bolje performanse i propusnost prilikom korištenja StackExchange.Redis. Dodatne informacije o poslužitelju GC i kako ga omogućiti potražite u sljedećim člancima.

-   [Da biste omogućili GC poslužitelja](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Osnove smeća](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Smeća i performanse](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Kako praćenje stanja i performanse Moje predmemorije?

Instance Microsoft Azure Redis predmemorije moguće nadzirati [Azure portal](https://portal.azure.com). Možete prikaz metriku, prikvačite metriku grafikoni da biste na Startboard, Prilagodba datum i vrijeme raspon nadzor grafikoni, dodavanje i uklanjanje metriku s grafikonima, i postavljanje upozorenja kada se ispuni određeni uvjet. Dodatne informacije potražite u članku [Monitor Azure Redis predmemoriju](cache-how-to-monitor.md).

U odjeljku **podrška + otklanjanje poteškoća** plohu Redis predmemorije **Postavke** sadrži i nekoliko alata za praćenje i vaše predmemorije za otklanjanje poteškoća. 

-   **Otklanjanje poteškoća** navedene informacije o najčešći problemi i Strategije za njihovo rješavanje.
-   **Zapisnike nadzora** opisi radnji na predmemoriju. Filtriranje možete koristiti i da biste proširili ovaj prikaz da biste dodali druge resurse.
-   **Stanje resursa** nadzirane vaše resursa i obavijestit će vas ako je pokrenut prema očekivanjima. Dodatne informacije o stanju servisa Azure resursa potražite u članku [Pregled stanja Azure resursa](../resource-health/resource-health-overview.md).
-   **Novi zahtjev za podršku** pruža mogućnosti da biste otvorili zahtjev za podršku za predmemoriju.

Ti alati omogućuju vam praćenje stanja instanci Azure Redis predmemorije i olakšavaju upravljanje predmemoriranja aplikacija. Dodatne informacije potražite u članku [Podrška i rješavanje problema s postavkama](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Moje predmemorije za pohranu računa postavki dijagnostike mijenjati, što se dogodilo?

Predmemorije u istom području i pretplatu zajednički koristiti iste postavke za pohranu za dijagnostiku i ako je konfiguraciju promijenjene (Dijagnostika omogućeno/onemogućeno ili promjena računa za pohranu) odnosi se na sve predmemorije u pretplate koje su u tom području. Ako ste promijenili Dijagnostika postavki predmemorije, provjerite ako ste promijenili dijagnostičkih postavki predmemoriju drugi na istoj pretplate i regija. Da biste provjerili je predmemoriju za u zapisniku nadzora u `Write DiagnosticSettings` događaj. Dodatne informacije o radu s zapisnike nadzora, potražite u članku [Prikaz događaja i zapisnika nadzora](../monitoring-and-diagnostics/insights-debugging-with-events.md) i [nadzor operacije s Voditelj resursa](../resource-group-audit.md). Dodatne informacije o praćenje događaja Azure Redis predmemorije, potražite u članku [operacije i upozorenja](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Zašto je omogućeno Dijagnostika za neke nove predmemorije, ali ne i ostalima?

Predmemorije u istom regija i pretplatu zajednički koristiti iste postavke za pohranu za dijagnostiku. Ako stvorite novu predmemoriju u istom regija i pretplate kao drugi predmemorije koja ima Dijagnostika omogućena, Dijagnostika je omogućena na novu predmemoriju pomoću istih postavki.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Zašto se prikazuju vremensko ograničenje?

Vremenska ograničenja dogoditi u klijentu koji koristite za Razgovarajte s Redis. U najvećem Redis poslužitelj ne istek vremena. Kada naredbe se šalje na poslužitelj Redis, naredba je u redu, a Redis poslužitelja naposljetku uzima naredbu i izvršava ga. No klijenta možete istek vremena tijekom tog postupka, a ako ne iznimku podignut na strani pozivanja. Dodatne informacije o otklanjanju poteškoća vremensko ograničenje, potražite u članku [Otklanjanje poteškoća strani klijenta](cache-how-to-troubleshoot.md#client-side-troubleshooting) i [StackExchange.Redis vremenskog ograničenja iznimke](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Zašto moj klijent prekinuta iz predmemorije?

Slijedi popis nekih uobičajenih razlog za prekid veze predmemorije.

-   Uzroci klijentskoj strani
    -   Ponovno je implementirati klijentske aplikacije.
    -   Klijentska aplikacija izvršiti skaliranja operacija.
        -   U slučaju servise u Oblaku ili web-aplikacije, Razlog može biti automatsko skaliranje.
    -   Umrežavanje sloj na klijentskoj strani mijenjaju.
    -   Tranzitne pojavile su se u klijent ili u mreži čvorove između klijenta i poslužitelja.
    -   Prag ograničenja propusnosti su Pristigla.
    -   Procesor vezana operacije traje predugo.
-   Uzroci poslužiteljsko
    -   U standardnom predmemoriji koja nudi servisom Azure Redis predmemorije pokrenut na nije uspjelo pokazivač primarnog čvorovi sekundarne čvor.
    -   Azure je zakrpa instancu gdje je uveden u predmemoriju
        -   To može biti Redis poslužitelj ažuriranja ili Općenito VM održavanja.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Koje nudi Azure predmemorije je za mene?

>[AZURE.IMPORTANT]Po prošle godine [Objava](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)servisa Azure Upravljani servis predmemorije i predmemorije u ulozi Azure će povučena u studenom 30, 2016. Naš preporuke tako da koristi [Predmemoriju Redis Azure](https://azure.microsoft.com/services/cache/). Informacije o migraciji potražite u članku [Migracija iz upravlja predmemorije servisa Azure Redis predmemoriju](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure Redis predmemorije
Azure Redis predmemorije Općenito dostupan je u veličina do 53 GB i ima raspoloživost SLA 99.9%. Novi [premium sloju](cache-premium-tier-intro.md) nudi veličine do 530 GB i podrška za Klasteriranje VNET i postojanost s 99.9% SLA.

Azure Redis predmemorije omogućuje klijentima da biste koristili sigurne, namjenski Redis predmemoriju, upravlja Microsoft. Pomoću ove ponude dobit odražava skup značajki obogaćenog i zajednici nudi Redis, pouzdanog hostiranje i nadzor od Microsofta.

Za razliku od tradicionalni predmemorije koji se baviti samo parovima ključnih vrijednosti, Redis je popularan za njegov vrlo performant vrste podataka. Redis i podržava atomske postupke na ove vrste, kao što su pridodati niz; povećava vrijednosti u raspršivanje; Odaberite na popisu. Izračunavanje skup presjeka, unije i razlika; ili pribavljanja člana s najvećim rang u skupu sortirani. Ostale značajke obuhvaćaju podršku za transakcije, pub/sub, Lua skriptiranje tipki na ograničeno vrijeme važenja, i konfiguracijske postavke da bi Redis više ponašaju poput tradicionalnog predmemoriju.

Drugi ključni aspekt za uspjeh Redis je zajednici dobar, žive Otvori izvor ugrađeni oko njega. Ovo prikazuje raznih skup Redis klijenti koji su dostupni u više jezika. To omogućuje se može koristiti u gotovo bilo kojeg radno opterećenje bi sastavljate unutar Azure. 

Dodatne informacije o početku rada s Azure Redis predmemorije potražite u članku [dokumentaciji Azure Redis predmemorije](https://azure.microsoft.com/documentation/services/redis-cache/)i [upute za korištenje Azure Redis predmemoriju](cache-dotnet-how-to-use-azure-redis-cache.md) .

### <a name="managed-cache-service"></a>Servis za upravljane predmemorije
[Upravlja predmemorije servis postavljen biti povučena studenom 30, 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>U ulozi predmemorije
[U ulozi predmemorije postavljen je na biti povučena studenom 30, 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





[postavke konfiguracije "minIoThreads"]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
