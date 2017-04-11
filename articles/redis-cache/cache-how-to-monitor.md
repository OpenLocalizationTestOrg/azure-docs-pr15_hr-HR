<properties 
    pageTitle="Upute za praćenje Azure Redis predmemorije | Microsoft Azure" 
    description="Upute za praćenje stanja i performanse vaše instance predmemorije Redis Azure" 
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
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Upute za praćenje predmemorije Redis Azure

Azure Redis predmemorije nudi nekoliko mogućnosti za svoje instance predmemorije za nadzor. Možete prikaz metriku, prikvačite metriku grafikoni da biste na Startboard, Prilagodba datum i vrijeme raspon nadzor grafikoni, dodavanje i uklanjanje metriku s grafikonima, i postavljanje upozorenja kada se ispuni određeni uvjet. Ti alati omogućuju vam praćenje stanja instanci Azure Redis predmemorije i olakšavaju upravljanje predmemoriranja aplikacija.

Kada su omogućene Dijagnostika predmemorije, metriku instance predmemorije Redis Azure su koji se prikupljaju otprilike svakih 30 sekundi i spremljene tako da se prikazuje na popisu metriku grafikona i procijeniti upozorenja pravila.

Predmemoriju metriku prikuplja pomoću na Redis naredba [informacije](http://redis.io/commands/info) . Dodatne informacije o različitim informacije vrijednosti koje se koriste za svaku predmemoriju metriku potražite [dostupna metriku i izvješćivanje o vremenskim razmacima](#available-metrics-and-reporting-intervals).

Da biste prikazali metrika predmemorije, [Pronađite](cache-configure.md#configure-redis-cache-settings) vašoj instanci predmemorije [Azure portal](https://portal.azure.com). Metriku instance predmemorije Redis Azure je moguće pristupiti na plohu **Redis metriku** .

![Redis mjerenja][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Ako se sljedeća poruka se prikazuje u plohu **Redis metriku** , slijedite korake u odjeljku [Omogućivanje predmemorije dijagnostiku](#enable-cache-diagnostics) da biste omogućili dijagnostika predmemoriju.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

Plohu **Redis metriku** ima **praćenja** grafikone koji prikazivati metriku predmemoriju. Svakom grafikonu moguće je prilagoditi dodavanjem ili uklanjanjem metriku i promjena interval za izvješćivanje. Za prikaz i konfiguriranje operacije i upozorenja, plohu **Redis predmemorije** sadrži odjeljak **operacije** koja se prikazuje predmemorije **događaja** i **upozorenja pravila**.

## <a name="enable-cache-diagnostics"></a>Omogućivanje Dijagnostika predmemorije

Azure Redis predmemorije pruža mogućnost Dijagnostika podatke pohranjene u prostor za pohranu računa da biste mogli koristiti bilo koji alat koji želite pristupiti i izravno obrade podataka. U redoslijedu za dijagnostiku predmemorije prikupljeni pohranjuju i prikazuju na portalu Azure s računom za pohranu mora biti konfigurirana. Predmemorije u istom području i pretplatu zajednički koristiti isti prostor za pohranu račun za dijagnostiku i kada se mijenja konfiguraciju se primjenjuje na sve predmemorije u pretplate koje su u tom području.

Omogućivanje i konfiguriranje Dijagnostika predmemorije, pomaknite se do plohu **Redis predmemoriju** za vaše instancu predmemoriju. Ako Dijagnostika još nije omogućena, pojavljuje se poruka umjesto Dijagnostika grafikona.

![Omogućivanje Dijagnostika predmemorije][redis-cache-enable-diagnostics]

Kliknite poruku da biste prikazali plohu **mjerenje** , a zatim kliknite **dijagnostičkih postavki** za omogućivanje i konfiguriranje dijagnostičkih postavki predmemorije instanca servisa.

![Postavki dijagnostike][redis-cache-diagnostic-settings]

![Konfiguriranje dijagnostičkog][redis-cache-configure-diagnostics]

Kliknite gumb **na** da biste omogućili dijagnostika predmemorije i prikaz dijagnostike konfiguracije.

Kliknite strelicu s desne strane **Prostora za pohranu računa** odaberite račun za pohranu na čuvanje dijagnostičkih podataka. Ostvarili najbolje performanse, odaberite račun za pohranu u području isti kao predmemoriju.

Kada niste konfigurirali dijagnostičkih postavki, kliknite **Spremi** da biste spremili konfiguracije. Imajte na umu da može potrajati nekoliko sekundi dok se da bi promjene stupile na snagu.

>[AZURE.IMPORTANT] Predmemorije u istom području i pretplatu zajednički koristiti iste postavke za pohranu za dijagnostiku i kada konfiguracija je promijenjena (Dijagnostika omogućeno/onemogućeno ili promjena računa za pohranu) se primjenjuje na sve predmemorije u pretplate koje su u tom području.

Da biste pogledali pohranjene metriku, pregledajte tablice u vašem računu za pohranu s nazivima koji počinju `WADMetrics`. Dodatne informacije o pristupu pohranjene metriku izvan Azure portal potražite u članku oglednih [podataka programa Access Redis predmemoriju za nadzor](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .

>[AZURE.NOTE] Samo spremaju se u račun za odabrani prostora za pohranu metrika prikazuju se na portalu za Azure. Ako promijenite račune za pohranu, podaci u prethodno konfiguriran za pohranu računa ostaju dostupna za preuzimanje, ali ne prikazuju na portalu za Azure.  

## <a name="available-metrics-and-reporting-intervals"></a>Dostupne metriku i izvješćivanje o vremenskim razmacima

Predmemorija metriku prijavljene pomoću nekoliko izvješćivanja intervale, uključujući **Proteklog hour**, **danas**, **proteklog tjedna**i **Prilagođeno**. Plohu **metriku** za svaki metriku grafikon prikazuje average, minimalne i maksimalne vrijednosti za svaki metriku na grafikonu, a neke metriku prikaz ukupnog zbroja za izvješćivanje interval. 

Svaki mjerenja obuhvaća dvije verzije. Jedan metriku mjeri performansi za cijelu predmemoriju i predmemorije koje koriste [Klasteriranje](cache-how-to-premium-clustering.md), druga verzija mjerenja koja obuhvaća `(Shard 0-9)` performanse mjere naziv jednog shard u predmemoriju. Na primjer, ako je predmemoriju 4 shards `Cache Hits` je ukupni iznos pristupa za cijelu predmemoriju i `Cache Hits (Shard 3)` je samo pristupa za tu shard predmemoriju.

>[AZURE.NOTE] Čak i kada predmemoriju je u stanju mirovanja s aplikacijama nema povezanog aktivni klijent, možda ćete vidjeti neke predmemorije aktivnosti, kao što su povezani klijenata, memorije i postupaka koji se izvode. Aktivnost je normalno tijekom postupka instance predmemorije Redis Azure.

| Mjerenja            | Opis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pronalaženje objekata u predmemoriji        | Broj uspješno ključa pretraživanja navedeni interval za izvješćivanje. To karte `keyspace_hits` iz Redis [informacije](http://redis.io/commands/info) naredbu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Neuspjele akcije u predmemoriji      | Broj neuspjelih ključa pretraživanja navedeni interval za izvješćivanje. To karte `keyspace_misses` od naredbe Redis informacije. Neuspjele akcije u predmemoriji ne znači nužno je došlo do problema s predmemoriju. Ako, na primjer, prilikom korištenja programskom predmemoriji aside uzorak aplikacije sustava izgleda prvi u predmemoriji za pojedinu stavku. Ako stavku ne postoji (neuspješno pronalaženje u predmemoriji), stavka se dohvaća iz baze podataka i dodali u predmemoriju za buduću upotrebu. Neuspjele akcije u predmemoriji su normalno ponašanje za programskom predmemoriji aside uzorak. Ako je broj neuspjele akcije u predmemoriji veći od očekivanog, ispitivanje logike aplikacije koja popunjava i čita iz predmemorije. Ako stavke su se izbaciti iz predmemorije zbog memorije, a zatim možda postoje neka neuspjele akcije u predmemoriji, ali bi bolje metriku praćenje za memorije `Used Memory` ili `Evicted Keys`. |
| Povezani klijenti | Broj klijentske veze u predmemoriju za određeni interval za izvješćivanje. To karte `connected_clients` od naredbe Redis informacije. Kada se dosegne [ograničenje](cache-configure.md#default-redis-server-configuration) prilikom pokušaja uspostavljanja veze u predmemoriju neće uspjeti. Imajte na umu da čak i ako su još nije aktivna klijentska aplikacija, i dalje možda postoji nekoliko slučajeva povezanih klijenata zbog interne procesa i veze.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Izbaciti tipke      | Broj stavki izbaciti iz predmemorije za navedeni izvješćivanja interval zbog na `maxmemory` ograničenje. To karte `evicted_keys` od naredbe Redis informacije.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Istekle tipke      | Broj stavki istekla iz predmemorije za određeni interval za izvješćivanje. Ta vrijednost karte `expired_keys` od naredbe Redis informacije.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Dobiti              | Broj operacije get iz predmemorije za određeni interval za izvješćivanje. Ta vrijednost je zbroj sljedeće vrijednosti iz informacija Redis naredbe sve: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, i `cmdstat_getrange`, i jednako zbroj pronalaženje objekata u predmemoriji i neuspješnih pronalaženja objekata izvješćivanja interval.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Redis poslužitelja | Postotak ciklusa u kojem je poslužitelj Redis zauzet obrade i ne čeka neaktivno poruke. Ako brojač dosegne 100 znači poslužitelja Redis sadrži kliknite performanse ceiling i Procesor ne može obraditi radite bilo brže. Ako vam se prikazuje učitavanja visoke Redis poslužitelj će vidjeti vremenskog ograničenja iznimke u klijentu. U ovom slučaju razmotrite skaliranje prema gore ili particija podataka u većem broju predmemorije.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Skupovi              | Broj skup operacije u predmemoriju za određeni interval za izvješćivanje. Ta vrijednost je zbroj sljedeće vrijednosti iz informacija Redis naredbe sve: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, i `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Ukupna operacije  | Ukupan broj naredbi obraditi predmemoriju za određeni interval za izvješćivanje. Ta vrijednost karte `total_commands_processed` od naredbe Redis informacije. Imajte na umu da kad Azure Redis predmemorije koristi isključivo za pub/sub će se bez metriku `Cache Hits`, `Cache Misses`, `Gets`, ili `Sets`, ali neće biti `Total Operations` metriku koja odražava korištenje predmemorije za pub/sub operacije.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Korištene memorije       | Iznos predmemorije memorije koristi za parove ključa vrijednosti u predmemoriji u MB navedeni interval za izvješćivanje. Ta vrijednost karte `used_memory` od naredbe Redis informacije. To ne uključuje metapodataka ili Fragmentacija.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Korištene memorije RSS   | Iznos predmemorije memorije koristi u MB navedeni izvješćivanja interval, uključujući Fragmentacija i metapodacima. Ta vrijednost karte `used_memory_rss` od naredbe Redis informacije.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| PROCESOR               | Na procesora poslužitelja Azure Redis predmemorije kao postotak navedeni interval za izvješćivanje. Ta vrijednost karata operacijski sustav `\Processor(_Total)\% Processor Time` brojač performansi.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Čitanje predmemorije        | Pročitajte količine podataka iz predmemorije u megabajtima u sekundi (MB/s) navedeni interval za izvješćivanje. Ta vrijednost izvodi se iz kartice mrežnog sučelja koji podržavaju virtualnog računala koje hostira predmemorije i nije Redis određene. **Ta vrijednost odgovara propusnost mreže koristi ovaj predmemoriju. Ako želite postaviti upozorenja za poslužitelj strani mreže propusnosti ograničenja, pa ga stvorite pomoću to `Cache Read` brojač. Pogledajte [u ovoj su tablici](cache-faq.md#cache-performance) ograničenja opaženih propusnosti za različite predmemorije cijene razine i veličine.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Pisanje predmemorije       | Količinu podataka zapisati u predmemoriju u megabajtima u sekundi (MB/s) tijekom određenu izvješćivanje interval. Ta vrijednost izvodi se iz kartice mrežnog sučelja koji podržavaju virtualnog računala koje hostira predmemorije i nije Redis određene. Ta vrijednost odgovara propusnost mreže podataka koji se šalju u predmemoriju iz klijenta.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Kako pogledati metriku i Prilagodba grafikona

Možete pogledati pregled metriku predmemoriju na plohu **Redis metriku** . Da biste pristupili **Redis metriku** plohu odaberite **sve postavke** > **Redis metriku**.

![Redis mjerenja][redis-cache-redis-metrics]


Plohu **Redis metriku** sadrži sljedeći grafikoni.

| Redis metriku grafikona | Prikazane mjerenja     |
|------------------|-------------------|
| Čitanje predmemorije i predmemorije pisanje | Čitanje predmemorije |
|                            | Pisanje predmemorije |
| Povezani klijenti      | Povezani klijenti |
| Pristupa i neuspješnih pronalaženja objekata  | Pronalaženje objekata u predmemoriji        |
|                  | Neuspjele akcije u predmemoriji      |
| Ukupna naredbe   | Ukupna operacije  |
| Uzima i skupove    | Dobiti              |
|                  | Skupovi              |
| Korištenje središnjih procesora         | PROCESOR           |
| Korištenje memorije      | Korištene memorije   |
|                   | Korištene memorije RSS |
| Redis poslužitelja | Poslužitelja   |
| Count ključ | Ukupna tipke |
|           | Izbaciti tipke |
|           | Istekle tipke |


Za detaljnije prikaz metriku na određeni grafikon, a da biste prilagodili grafikon, kliknite željeni grafikon iz plohu **Redis metriku** da biste prikazali plohu **metriku** za taj grafikon.

![Metričkim plohu][redis-cache-metric-blade]

Pri dnu plohu **metriku** za taj grafikon navedene su sve upozorenja koja su postavljena na metriku prikazati na grafikonu.

Da biste dodali ili uklonili metriku ili promijenili izvješćivanja razmak, odaberite **Uređivanje grafikona**.

Da biste dodali ili uklonili metriku iz grafikona, potvrdite okvir pokraj naziva mjerenja. Da biste promijenili interval izvješćivanja, kliknite željeni interval. Da biste promijenili **vrstu grafikona**, kliknite željeni stil. Kada su željene promjene, kliknite **Spremi**. 

![Uređivanje grafikona][redis-cache-edit-chart]

Kada kliknete **Spremi** promjene će i dalje pojavljuje dok ne napustite plohu **metriku** . Kada vam se vratite kasnije, vidjet ćete izvornu metričke i vremenski raspon ponovno. Dodatne informacije o prilagođavanju grafikonima potražite u članku [metriku servisa Monitor](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Da biste pogledali metriku određeno vremensko razdoblje na grafikonu, držite pokazivač miša iznad jedne od određene stupce ili točke na grafikonu koji odgovara željenom vrijeme, a prikazuju se metriku za taj interval.

![Prikaz detalja iz grafikona][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Upute za praćenje predmemorije premium s Klasteriranje

Premium predmemorije koji imaju [Klasteriranje](cache-how-to-premium-clustering.md) omogućeno može sadržavati do 10 shards. Svaki shard ima zasebnom metrika, a te metriku se pridružuje nudi metriku za predmemoriju kao cjelinu. Svaki mjerenja obuhvaća dvije verzije. Jedan metriku mjeri performanse za cijelu predmemoriju i druga verzija mjerenja koja obuhvaća `(Shard 0-9)` performanse mjere naziv jednog shard u predmemoriju. Na primjer, ako je predmemoriju 3 shards `Cache Hits` je ukupni iznos pristupa za cijelu predmemoriju i `Cache Hits (Shard 2)` je samo pristupa za tu shard predmemoriju.

Svaki nadzora grafikon prikazuje metriku najviše razine predmemoriju uz metriku za svaku shard predmemoriju.

![Monitora][redis-cache-premium-monitor]

Pokazivač miša iznad točke podataka prikazuje pojedinosti za tog trenutka u vremenu. 

![Monitora][redis-cache-premium-point-summary]

Veće vrijednosti obično su vrijednosti zbroja za predmemoriju dok manje vrijednosti su pojedinačne metriku na shard. Imajte na umu da u ovom primjeru postoje tri shards i pronalaženje objekata u predmemoriji su raspodijeliti ravnomjerno na shards.

![Monitora][redis-cache-premium-point-shard]

Da biste vidjeli više detalja kliknite grafikon da biste pogledali prošireni prikaz na plohu **mjerenje** .

![Monitora][redis-cache-premium-chart-detail]

Svakom grafikonu se po zadanome sadrži performanse brojač predmemorije najviše razine, kao i mjerača performansi za pojedinačne shards. Možete prilagoditi te na plohu **Uređivanje grafikona** .

![Monitora][redis-cache-premium-edit]

Dodatne informacije o mjerača dostupna performansi potražite u članku [dostupne metriku i izvješćivanje o vremenskim razmacima](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Operacije i upozorenja

U odjeljku **operacija** na plohu **Redis predmemorije** ima **događaja** i **upozorenja pravila** sekcije.

![Oeprations][redis-cache-operations-events]

Da biste vidjeli popis nedavnih operacije predmemorije, kliknite grafikon **događaja** da biste prikazali plohu **događaja** . Operacija Primjeri dohvaćanje i ponovno stvaranje tipke za pristup i aktivacija i razlučivost upozorenja pravila. Dodatne informacije o svaki događaj, kliknite događaj u plohu **događaja** .

Dodatne informacije o događajima potražite u članku [Prikaz događaja i zapisnika nadzora](../monitoring-and-diagnostics/insights-debugging-with-events.md).

U odjeljku **pravila za upozorenja** prikazuje broj upozorenja za instancu predmemoriju. Upozorenja pravila omogućuje vam praćenje vašoj instanci predmemorije i primati poruke e-pošte kad god se određene vrijednosti metričkim dosegne prag definirano u pravilo. 

Upozorenja pravila vrednuju otprilike svakih pet minuta, a kada se aktivira pravilo upozorenja, konfigurirani obavijesti šalju se. Upozorenja pravila aktivacije i obavijesti se ne obrađuju po; mogu postojati Odgoda od nekoliko minuta prije nego što se aktivira pravilo upozorenja i obavijesti koje se šalju.

Upozorenja pravila možete prikazati i postavljanje plohu **metriku** za određene nadzora grafikon ili iz plohu **upozorenja pravila** .

Upozorenja pravila imaju sljedeća svojstva.

| Svojstvo upozorenja pravilo                 | Opis                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Resurs                            | Resurs koji se procjenjuje pravilom upozorenja. Kada stvarate pravilo upozorenja iz Redis predmemorije, predmemoriju je resurs.                                                                                                                                                                                                                                                                                                                                                  |
| Ime                                | Naziv koji služi kao jedinstvena identifikacija upozorenja pravila u trenutnoj instanci predmemoriju.                                                                                                                                                                                                                                                                                                                                                                                       |
| Opis                         | Neobavezan opis upozorenja pravila.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Mjerenja                              | Metrika moguće nadzirati pravilom upozorenja. Popis metriku predmemorije, potražite u članku dostupne metriku i izvješćivanje o vremenskim razmacima.                                                                                                                                                                                                                                                                                                                                             |
| Uvjet                           | Operator uvjeta upozorenja pravila. Moguće odabrati: veći od, veće od ili jednako, manje od manje od ili jednako                                                                                                                                                                                                                                                                                                                             |
| Prag                           | Vrijednost koja se koristi za usporedbu s metriku pomoću operatora određen svojstvo uvjet. Ovisno o metriku, tu vrijednost možda bajtova/sekundi bajtova, % ili broj.                                                                                                                                                                                                                                                                                     |
| Razdoblje.                              | Određuje razdoblje prosječnu vrijednost metriku s kojima se koristi za usporedbu upozorenja pravilo. Ako, na primjer, ako je razdoblje tijekom posljednjeg sat, prosječnu vrijednost metriku u intervalu prethodne sat koristi se za usporedbe. Ako želite biti obaviješteni kada praga zadovolji zbog u šiljak u aktivnosti, zatim kraći razdoblje je odgovarajući. Da biste primili obavijest kada osigurale trajne aktivnosti iznad praga, koristite dulje razdoblje. |
| Servis za e-pošte i dodatnih administratora | Kada je true, administrator servisa i dodatnih administratora su je slali prilikom aktiviranja upozorenje.                                                                                                                                                                                                                                                                                                                                                                    |
| Dodatni administrator e-pošte      | Adresu e-pošte neobavezno administrator dodatne biti obaviješten prilikom aktiviranja upozorenje.                                                                                                                                                                                                                                                                                                                                                                    |

Samo jedan obavijest šalje po aktivaciju upozorenja pravilo. Kada se prekorači praga za pravilo, a zatim se šalje obavijest, pravilo nije ponovno procjenjuje dok metriku pada ispod praga. Ako metriku naknadno premašuje prag, upozorenje je aktivirati i se šalje obavijest o novoj.

Da biste vidjeli sva upozorenja pravila za vaše instancu predmemorije, kliknite dio **upozorenja pravila** u plohu **Redis predmemoriju** . Da biste vidjeli samo upozorenja pravila koja koristi određenu metriku, pomaknite se do plohu **metriku** za grafikon koji sadrži taj metriku.

![Upozorenja pravila][redis-cache-alert-rules]

Da biste dodali pravilo upozorenja, kliknite **Dodaj upozorenja** iz plohu **metrički** ili plohu **upozorenja pravila** . 

Unesite željeno pravilo kriterije u pravilo plohu **Dodaj upozorenja** , a zatim kliknite **u redu**. 

![Dodavanje pravila za upozorenja][redis-cache-add-alert]

>[AZURE.NOTE] Upozorenja pravilo stvaranja tako da kliknete **Dodaj upozorenja** iz plohu **metriku** samo metriku prikazati na grafikonu u tom plohu će se pojaviti u **metriku** padajući popis. Upozorenja pravilo stvaranja tako da kliknete **Dodaj upozorenja** iz **upozorenja pravila** plohu sva mjerenja predmemorije dostupne su u **metriku** padajući popis.

Kada upozorenja pravilo se sprema će se prikazati **upozorenje pravila** plohu, kao i na plohu **metriku** za sve grafikone koji prikazuju metriku koristi u pravilo za upozorenja. Da biste uredili upozorenja pravilo, kliknite naziv upozorenja pravila da biste prikazali plohu **Uređivanje pravila** . Iz plohu **Uređivanje pravila** možete urediti svojstva pravilo, brisanje ili onemogućite pravilo za upozorenja ili ponovno omogućite pravilo prethodno onemogućena.

>[AZURE.NOTE] Sve promjene koje napravite sa svojstvima pravilo može potrajati koji trenutak prije nego što ih odražavaju se na plohu **upozorenja pravila** ili plohu **mjerenje** .

Kada se aktivira pravilo upozorenja, poruke e-pošte koja se šalje ovisno o konfiguraciji upozorenja pravilo i prikazuje se ikona upozorenja u dijelu **upozorenja pravila** na plohu **Redis predmemorije** .

Upozorenja pravilo smatra se riješiti kada upozorenja uvjet više vrednuje kao true. Kada uvjet upozorenja pravila ne riješi, ikonu upozorenja je zamijenjena funkcijom kvačica. Pojedinosti o upozorenja aktivacije i rješenja, kliknite dio **događaja** na plohu **Redis predmemorije** da biste pogledali događaje na plohu **događaja** .

Dodatne informacije o upozorenja u Azure potražite u članku [primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Metriku na plohu Redis predmemorije

Plohu **Redis predmemorije** prikazuje sljedeće kategorije metriku.

-   [Nadzor grafikoni](#monitoring-charts)
-   [Korištenje grafikona](#usage-charts)

### <a name="monitoring-charts"></a>Nadzor grafikoni

U odjeljku **nadzor** sadrži **dodirne i pronalaženja u predmemoriji**, **dobit će i postavlja**, **veze**i grafikoni **Ukupni naredbe** .

![Nadzor grafikoni][redis-cache-monitoring-part]

**Nadzor** grafikoni prikazuju sljedeće metriku.

| Nadzor grafikona | Predmemorija mjerenja     |
|------------------|-------------------|
| Pristupa i neuspješnih pronalaženja objekata  | Pronalaženje objekata u predmemoriji        |
|                  | Neuspjele akcije u predmemoriji      |
| Uzima i skupove    | Dobiti              |
|                  | Skupovi              |
| Veze      | Povezani klijenti |
| Ukupna naredbe   | Ukupna operacije  |

Informacije o prikaz metriku i prilagodbi pojedinačne grafikone u ovom odjeljku u odjeljku sljedeće [upute za prikaz metriku i Prilagodba grafikona metriku](#how-to-view-metrics-and-customize-charts) .

### <a name="usage-charts"></a>Korištenje grafikona

Odjeljak **Korištenje** ima **Redis učitavanja poslužitelja**, **Memorije**, **Bandwith mreže**i grafikoni **Procesora** i prikazuje i **određivanje cijena sloju** za instancu predmemorije.

![Korištenje grafikona][redis-cache-usage-part]

**Određivanje cijena sloju** prikazuje cijene predmemorije sloju, a može biti koristi [Skaliranje](cache-how-to-scale.md) predmemoriju za različite cijene sloju.

**Korištenje** grafikona prikazuje sljedeće metriku.

| Korištenje grafikona       | Predmemorija mjerenja |
|-------------------|---------------|
| Redis poslužitelja | Poslužitelja   |
| Korištenje memorije      | Korištene memorije   |
| Propusnost mreže | Pisanje predmemorije   |
| Korištenje središnjih procesora         | PROCESOR           |

Informacije o prikaz metriku i prilagodbi pojedinačne grafikone u ovom odjeljku u odjeljku sljedeće [upute za prikaz metriku i Prilagodba grafikona metriku](#how-to-view-metrics-and-customize-charts) .
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



