<properties 
    pageTitle="Otklanjanje poteškoća s Azure Redis predmemorije | Microsoft Azure" 
    description="Saznajte kako riješiti uobičajene probleme s Azure Redis predmemoriju." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Otklanjanje poteškoća s predmemorije Redis Azure

Ovaj članak sadrži upute za otklanjanje poteškoća s sljedećih kategorija Azure Redis predmemorije problema.

-   [Otklanjanje poteškoća klijentskog strani](#client-side-troubleshooting) - Ova sekcija sadrži upute prepoznavanje i rješavanje problema uzrokovanih aplikacije povezivanje Azure Redis predmemoriju.
-   [Otklanjanje poteškoća s poslužiteljem strani](#server-side-troubleshooting) - Ova sekcija sadrži upute koje prepoznavanje i rješavanje problema s uzrokovati na strani poslužitelja predmemorije Redis Azure.
-   [Iznimke vremenskog ograničenja StackExchange.Redis](#stackexchangeredis-timeout-exceptions) – u ovom se odjeljku nudi informacije o otklanjanju poteškoća prilikom korištenja StackExchange.Redis klijenta.


>[AZURE.NOTE] Nekoliko korake za otklanjanje poteškoća u ovom vodiču sadrže upute za izvođenje naredbe Redis i praćenje različitih metriku performanse. Dodatne informacije i upute potražite u člancima u odjeljku [Dodatne informacije](#additional-information) .

## <a name="client-side-troubleshooting"></a>Otklanjanje poteškoća strani klijenta


U ovom se odjeljku opisuje otklanjanje problema koji se pojavljuju zbog uvjet na klijentskoj aplikaciji.

-   [Memorije pritisak na klijentskom računalu](#memory-pressure-on-the-client)
-   [Neprekidnog rada prometa](#burst-of-traffic)
-   [Visoke klijent procesora](#high-client-cpu-usage)
-   [Prekoračena propusnost strani klijenta](#client-side-bandwidth-exceeded)
-   [Veličina velika/odgovor na zahtjev](#large-requestresponse-size)
-   [Što se dogodilo s mojim podacima u Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Memorije pritisak na klijentskom računalu

#### <a name="problem"></a>Problem

Memorije pritisak na klijentskom računalu potencijalnih klijenata za sve vrste probleme s performansama koji se može usporiti obrada podataka koje je poslao instancu Redis bez bilo koje vrijeme. Kada memorije dodirne, sustav obično ima podatke stranice iz fizičke memorije virtualna memorija koja se nalazi na disku. Ova *stranica s kvarom* uzrokuje da znatno usporiti sustav.

#### <a name="measurement"></a>Mjera 

1.  Praćenje memorije na računalu da biste bili sigurni da premašuju dostupnom memorijom. 
2.  Monitora na `Page Faults/Sec` brojač performansi. Većina sustavi će imati neke stranice kvarove čak i tijekom operacije normalno, pa pogledajte krivina brojač performanse kvarove stranice koje odgovaraju vremensko ograničenje.

#### <a name="resolution"></a>Razlučivost

Nadogradite klijent veće klijentu veličina VM s dodatnu memoriju ili istražujte u vašem uzoraka korištenja memorije da biste smanjili consuption memorije.


### <a name="burst-of-traffic"></a>Neprekidnog rada prometa

#### <a name="problem"></a>Problem

Bursts prometa u kombinaciji s slabo `ThreadPool` kašnjenja u obrada podataka poslužitelj Redis već poslao, ali još nije potrošena na klijentskoj strani može uzrokovati postavke.

#### <a name="measurement"></a>Mjera 

Monitor kako vaše `ThreadPool` Statistika promjena tijekom vremena pomoću koda [ovako](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Možete i pogledati na `TimeoutException` poruka od StackExchange.Redis. Evo jednog primjera:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

U poruci iznad postoje nekoliko problema koji su zanimljive:

 1. Primijetit ćete da u na `IOCP` sekcije i `WORKER` sekciju imate u `Busy` vrijednosti veće od na `Min` vrijednost. To znači da vaše `ThreadPool` postavke potrebne prilagodbe.
 2. Možete pogledati i `in: 64221`. To označava 64211 bajtova zaprimljeni pri socket layer otklanjanje, ali niste još pročitali aplikacija (npr. StackExchange.Redis). To obično znači da aplikacija nije čitanja podataka iz mreže jednako brzo kao poslužitelj šalje se na vas.

#### <a name="resolution"></a>Razlučivost

Konfiguriranje [Postavki ThreadPool](https://gist.github.com/JonCole/e65411214030f0d823cb) da biste bili sigurni da vaš prostor niti će proširenja brzo u odjeljku scenariji neprekidnog rada.


### <a name="high-client-cpu-usage"></a>Visoke klijent procesora

#### <a name="problem"></a>Problem

Visoke procesora na klijentskom računalu je znak da sustav ne može pratiti rada koji je zatraženo da izvrši. To znači da klijent možda neće funkcionirati za obradu odgovora Redis pravovremeno iako Redis vrlo brzo poslali odgovor.

#### <a name="measurement"></a>Mjera

Praćenje sustava širine procesora koristi putem portala za Azure ili brojač pridružene performansi. Pripazite da ne želite nadzirati procesora *postupak* jer jedan proces može imati manje procesora istovremeno cjelokupnog sustava procesora mogu biti visoka. Pogledajte krivina procesora koji odgovaraju vremenska ograničenja. Uslijed visoke procesora, možda ćete vidjeti visoko `in: XXX` vrijednosti u `TimeoutException` poruke pogreške, kao što je opisano u odjeljku [neprekidnog rada prometa](#burst-of-traffic) .

>[AZURE.NOTE] StackExchange.Redis 1.1.603 i kasnije sadrži na `local-cpu` metričkim u `TimeoutException` poruke o pogreškama. Provjerite koristite li najnoviju verziju [paketa StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Postoje programskih pogrešaka koje se neprestano popravlja kod da biste ga učinili Robusniji vremensko ograničenje pa je važno imate najnoviju verziju.

#### <a name="resolution"></a>Razlučivost

Nadogradnja na veću veličinu VM s više procesora kapaciteta ili istražiti što uzrokuje krivina procesora. 



### <a name="client-side-bandwidth-exceeded"></a>Prekoračena propusnost strani klijenta

#### <a name="problem"></a>Problem

Različite veličine klijentskim računalima imati ograničenja na kolika je propusnost mreže imaju dostupna. Ako klijent premašuje raspoložive propusnosti, zatim podataka ne obrađuju na klijentskoj strani jednako brzo kao poslužitelj šalje se. To može dovesti do vremenska ograničenja.

#### <a name="measurement"></a>Mjera

Praćenje kako promijeniti vašeg korištenja propusnosti tijekom vremena pomoću koda [ovako](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Imajte na umu da kod nije uspješno pokrenuti u nekim okruženjima s ograničenim dozvolama (kao što je Azure web-mjesta).

#### <a name="resolution"></a>Razlučivost 

Klijent VM povećajte ili smanjite utrošak propusnosti mreže.


### <a name="large-requestresponse-size"></a>Veličina velika/odgovor na zahtjev

#### <a name="problem"></a>Problem

Velike zahtjeva i odgovora može uzrokovati vremenska ograničenja. Na primjer, pretpostavimo da vaš vrijednost vremenskog ograničenja konfigurirali klijent je 1 sekunde. Aplikacija zahtjeve dvije tipke (npr. "A" i "B") u isto vrijeme (koristeći istu fizičke mrežne veze). Većina klijenti podržava "Pipelining" zahtjeva, tako da se oba zahtjevi za "A" i "B" poslati na žičani s poslužiteljem jedan nakon drugog bez čekanja odgovore. Poslužitelj će slati odgovore u istoj narudžbi. Ako je odgovor "A" velik dovoljno je možete pojesti gore većinu vremenskog ograničenja za daljnji zahtjevi. 

Sljedeći primjer prikazuje taj scenarij. U ovom scenariju zahtjev "A" i "B" šalju se brzo, poslužitelj pokreće brzo slanja odgovora "A" i "B", ali zbog vremena prijenos podataka, "B" se zamrzne iza na zahtjev i vremena izvan čak i ako je poslužitelj je brzo odgovorio.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Mjera

Ovo je teško jedan za mjerenje. Zapravo su instrumenata klijent kod da biste pratili velike zahtjeve i odgovore. 

#### <a name="resolution"></a>Razlučivost

1.  Redis optimiziran je za veliki broj small vrijednosti, umjesto nekoliko velikih vrijednosti. Preferirani rješenje je da biste prekinuli kopije podataka u povezanim manje vrijednosti. Pogledajte na [što je raspon veličina idealne vrijednosti za redis? 100KB je prevelika?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Objavite detalje oko Zašto se preporučuje manje vrijednosti.
2.  Povećajte vaše VM (za postupak klijentske i poslužiteljske predmemoriju za Redis) da biste dobili veće mogućnosti propusnosti smanjivanju vrijeme prijenosa podataka za veće odgovore. Imajte na umu da početak više propusnosti na samo poslužitelj ili samo na klijentu možda neće biti dovoljno. Izmjerite vašeg korištenja propusnosti, a zatim Usporedi s mogućnostima veličina VM koju trenutno imate.
3.  Povećajte broj `ConnectionMultiplexer` objekata koji koristite i zahtjeve za kružno putem različitih veza.


### <a name="what-happened-to-my-data-in-redis"></a>Što se dogodilo s mojim podacima u Redis?

#### <a name="problem"></a>Problem

Želim određenih podataka u moj instanci Azure Redis predmemorije, ali niste čini se.

##### <a name="resolution"></a>Razlučivost

U odjeljku [što se dogodilo s mojim podacima u Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) za mogući uzroci i rješenja.


## <a name="server-side-troubleshooting"></a>Otklanjanje poteškoća s poslužiteljem strani

U ovom se odjeljku opisuje otklanjanje problema koji se pojavljuju zbog uvjeta na poslužitelju predmemoriju.

-   [Tlaka memorije na poslužitelju](#memory-pressure-on-the-server)
-   [Visoke procesora / učitavanje poslužitelja](#high-cpu-usage-server-load)
-   [Prekoračena propusnost strani poslužitelja](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Tlaka memorije na poslužitelju

#### <a name="problem"></a>Problem

Za sve vrste probleme s performansama koji se može usporiti obrade zahtjeva za potencijalne klijente memorije pritisak na strani poslužitelja. Kada memorije dodirne, sustav obično ima podatke stranice iz fizičke memorije virtualna memorija koja se nalazi na disku. Ova *stranica s kvarom* uzrokuje da znatno usporiti sustav. Postoji nekoliko mogućih uzroka ovog tlaka memorije: 

1.  Ispunili predmemoriju cijelog kapacitet s podacima. 
2.  Redis vidi Fragmentacija opterećenje memorije - najčešće uzrokovati spremanje velike objekte (Redis optimiziran je za malih objekata - potražite u članku na [što je raspon veličina idealne vrijednosti za redis? 100KB je prevelika?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Objavite detalje). 

#### <a name="measurement"></a>Mjera

Redis izlaže dva metriku koji mogu vam olakšati prepoznavanje taj problem. Prvi je `used_memory` i druga `used_memory_rss`. [Ove metriku](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) dostupnih na portalu za Azure ili putem naredbe [Redis informacije](http://redis.io/commands/info) .

#### <a name="resolution"></a>Razlučivost

Postoji nekoliko mogućih promjene koje možete napraviti pojednostavnjuju dobar memorije:

1. [Konfiguriranje pravila memorije](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) i postaviti vrijeme isteka ključeva. Imajte na umu da to možda neće biti dovoljni ako imate Fragmentacija.
2. [Konfiguriranje vrijednost maxmemory rezervirane](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) koji nije dovoljna za vaše za Fragmentacija memorije.
3. Prekidanje gore velike predmemorirani objekti u manje povezane objekte.
4. [Skaliranje](cache-how-to-scale.md) radi veće veličina predmemorije.
5. Ako koristite [predmemorije premium s Redis klaster omogućeno](cache-how-to-premium-clustering.md) možete [povećati broj shards](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Visoke procesora / učitavanje poslužitelja

#### <a name="problem"></a>Problem

Visoke procesora može značiti da obradi odgovor Redis pravovremeno iako Redis vrlo brzo poslali odgovor uspijeva na strani klijenta.

#### <a name="measurement"></a>Mjera

Praćenje sustava širine procesora koristi putem portala za Azure ili brojač pridružene performansi. Pripazite da ne želite nadzirati procesora *postupak* jer jedan proces može imati manje procesora istovremeno cjelokupnog sustava procesora mogu biti visoka. Pogledajte krivina procesora koji odgovaraju vremenska ograničenja.

#### <a name="resolution"></a>Razlučivost

[Promjena veličine](cache-how-to-scale.md) veću predmemoriju sloj s više procesora kapaciteta ili istražiti što uzrokuje krivina procesora. 

### <a name="server-side-bandwidth-exceeded"></a>Prekoračena propusnost strani poslužitelja

#### <a name="problem"></a>Problem

Instance različite veličine predmemorije imaju ograničenja na kolika je propusnost mreže imaju dostupna. Ako poslužitelj premašuje raspoložive propusnosti, zatim podaci ne šalju se klijentu kao brzo. To može dovesti do vremenska ograničenja.

#### <a name="measurement"></a>Mjera

Možete nadzirati na `Cache Read` metriku koja je količinu podataka koji se čitaju iz predmemorije u megabajtima u sekundi (MB/s) navedeni interval za izvješćivanje. Ta vrijednost odgovara propusnost mreže koristi ovaj predmemoriju. Ako želite postaviti upozorenja za poslužitelj strani mreže propusnosti ograničenja, možete stvoriti pomoću to `Cache Read` brojač. Usporedite vaše čitanja s vrijednostima u [ovoj su tablici](cache-faq.md#cache-performance) ograničenja opaženih propusnosti za različite predmemorije cijene razine i veličine.

#### <a name="resolution"></a>Razlučivost

Ako niste dosljedno blizu opaženih Maksimalna propusnost za cijene veličinu sloju i predmemorije, razmislite o [Skaliranje](cache-how-to-scale.md) cijene sloju ili veličine koja ima veću propusnost mreže pomoću vrijednosti u [ovoj tablici](cache-faq.md#cache-performance) kao vodič.


## <a name="stackexchangeredis-timeout-exceptions"></a>Iznimke StackExchange.Redis vremenskog ograničenja

StackExchange.Redis koristi konfiguracije postavljanje imenovani `synctimeout` za sinkrono operacije koja ima zadanu vrijednost od 1000 ms. Ako ne sinkrono poziv dovršiti u stipulated vremenu, klijent StackExchange.Redis throws pogreške prekoračenja vremenskog ograničenja slično kao u sljedećem primjeru.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Poruka o pogrešci sadrži određene parametre koje omogućuju na uzroke i moguća rješenja problema. Sljedeća tablica sadrži detalje o metriku poruku o pogrešci.

| Metriku poruku pogreške | Pojedinosti                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Inst       | U zadnji put isječak: Izdana 0 naredbe                                                                                                                                                                                              |
| Voditelj        | Upravitelj socket izvršava `socket.select` što znači da od vas traži da biste naznačili socket koja sadrži nešto učiniti; OS-a zapravo: čitač ne aktivno čita s mreže jer ne razmislite što učiniti |
| red čekanja      | Postoje 73 ukupni operacije u tijeku                                                                                                                                                                                                        |
| qu         | 6 od operacije u tijeku je u redu čekanja za koje nisu poslane i još nisu snimljene izlaznog mrežu                                                                                                                                                           |
| qs         | 67 operacije u tijeku he su poslane na poslužitelj, ali odgovor još nije dostupna. Odgovor može biti `Not yet sent by the server` ili`sent by the server but not yet processed by the client.`                                                   |
| bez protoka         | 0 operacija u tijeku pojavljuje odgovore, ali još nije označen kao dovršen zbog čekanje dovršetka petlje                                                                                                                                      |
| wr         | Postoji aktivni writer (dakle, 6 zahtjeva za koje nisu poslane se zanemaruje) bajtova/activewriters                                                                                                                                                   |
| u         | Postoje nije aktivna čitateljima i nuli bajtova dostupnih za čitanje na NIC bajtova/activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Koraci za istraživanje

1. Kao što je najbolji način provjerite je li koristite sljedeći uzorak da biste se povezali prilikom korištenja StackExchange.Redis klijenta.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Dodatne informacije potražite u članku [Povezivanje s predmemoriju pomoću StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Azure Redis predmemorije i klijentske aplikacije provjerite jesu li u istom području u Azure. Ako, na primjer, koju možda se pojavljuje vremensko ograničenje kada predmemorije u SAD-a za istočnoazijske, ali klijent je u Zapad SAD-a i zahtjev ne dovrši unutar na `synctimeout` interval, ili možda biti dobiti vremenska ograničenja prilikom ispravljanja pogrešaka iz računala lokalne razvoj. 

    Sadrži se preporučuje da bi predmemorije i u klijentu u istom Azure regiji. Ako imate scenarij koji sadrži unakrsne regija pozive, trebali biste postaviti na `synctimeout` interval na vrijednost veću od 1000 ms interval zadani uključivanjem na `synctimeout` svojstvo u nizu za povezivanje. Sljedeći primjer prikazuje niz uzastopna StackExchange.Redis veze predmemorije s na `synctimeout` od 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Provjerite koristite li najnoviju verziju [paketa StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Postoje programskih pogrešaka koje se neprestano popravlja kod da biste ga učinili Robusniji vremensko ograničenje pa je važno imate najnoviju verziju.

4. Ako postoje zahtjeva za početak povezanu po ograničenja propusnosti na poslužitelju ili klijenta, će trajati dulje za njih da biste dovršili i time uzrokovati vremenska ograničenja. Ako je vaš vremenskog ograničenja zbog propusnost mreže na poslužitelju, potražite [Prekoračena propusnost strani poslužitelja](#server-side-bandwidth-exceeded). Ako je vaš vremenskog ograničenja zbog propusnost mreže za klijenta potražite [Prekoračena propusnost strani klijenta](#client-side-bandwidth-exceeded).

6. Koje početak procesora povezane su na poslužitelju ili na klijentskom računalu?
    -   Potvrdite ako ste početak povezane su po procesora na klijentu koji bi mogli uzrokovati zahtjev za moguća unutar na `synctimeout` intervala, što uzrokuje vremensko ograničenje. Premještanje veći klijenta ili distribucija opterećenje može pridonijeti kontrolirati. 
    -   Provjerite je li vam se prikazuju procesora vezana na poslužitelju prateći na `CPU` [metriku performanse predmemoriju](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Zahtjevi za uskoro dok je Redis procesora vezana mogu prouzročiti zahtjeve za vremenskog ograničenja. Da biste riješili taj opterećenje raspodijelite više shards u predmemoriju premium ili nadogradnju na veću veličinu ili cijene sloju. Dodatne informacije potražite u članku [Prekoračena propusnost veze strani poslužitelja](#server-side-bandwidth-exceeded).

7. Postoje li naredbe neobično dugo da obradi na poslužitelju? Dugo izvodi naredbe koje su neobično dugo traje na poslužitelju redis mogu prouzročiti vremenska ograničenja. Primjeri dugotrajne naredbe su `mget` s velikim brojem tipke, `keys *` ili slabo napisan lua skripti. Možete povezati vašoj instanci predmemorije Redis Azure pomoću klijentskog programa redis eža ili [Redis konzole](cache-configure.md#redis-console) i pokrenite naredbu [SlowLog](http://redis.io/commands/slowlog) da biste vidjeli postoje li zahtjevi za pisanje dulje nego je očekivano. Poslužitelj redis i StackExchange.Redis su optimizirani za mnoge small zahtjeve umjesto manje zahtjeva za velike. Podjele podataka u manjim blokova mogu poboljšati stvari u nastavku. 

    Informacije o povezivanje krajnjoj SSL predmemorije Redis Azure pomoću redis eža i stunnel, potražite u članku u objavu na blogu [Objavljivanje ASP.NET sesije stanje davatelj usluga za Redis pretpregled izdanja](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) . Dodatne informacije potražite u članku [SlowLog](http://redis.io/commands/slowlog).

8. Visoke učitavanja poslužitelja Redis mogu prouzročiti vremenska ograničenja. Možete nadzirati opterećenje poslužitelja prateći na `Redis Server Load` [predmemorije performanse metriku](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Poslužitelj opterećenjem od 100 (Maksimalna vrijednost) označava da redis poslužitelj je zauzeta, bez vrijeme neaktivnosti obrade zahtjeva za. Da biste vidjeli određene zahtjeve izvodite kopije svih mogućnost poslužitelja, pokrenite naredbu SlowLog, kao što je opisano u prethodni odlomak. Dodatne informacije potražite u članku [visoke procesora / učitavanje poslužitelja](#high-cpu-usage-server-load).

9. Došlo je bilo koji događaj na klijentskoj strani koji može uzrokovati mreže blip? Potražite na klijentskom računalu (web, ulogu suradnika ili je Iaas VM) ako je događaja kao što su skaliranje broj instanci klijent prema gore ili dolje ili implementacija nova verzija klijenta ili automatsko skaliranje omogućena? U našem testiranje ste naišli mogu prouzročiti automatsko skaliranje ili mijenjanje veličine gore/dolje izlaznog mrežne veze može biti izgubljene nekoliko sekundi. Kod StackExchange.Redis je prebacuju na takav događaje, a će se ponovno povezali. Za to vrijeme ponovnog povezivanja sve zahtjeve u redu čekanja mogu istek vremena.

10. Je li zahtjev za veliki ispred nekoliko small zahtjeve predmemoriju Redis istekli? Parametar `qs` u pogrešku poruka u kojoj stoji zahtjeva za koliko su poslane putem klijentskog programa na poslužitelju, ali još nije obrađen odgovor. Tu vrijednost možete zadržati rast jer StackExchange.Redis koristi jedan TCP vezu, a može samo čitati odgovor jedan po jedan. Čak i ako se prvi postupak isteklo, zaustavljanje podataka koja se šalje na/s poslužitelja, a zahtjeve blokiraju to završi, uzrok vremenskih. Jedan rješenje je da biste minimizirali izgledi vremensko ograničenje Provjera je li dovoljno velikim za svoje radno opterećenje predmemorije i podjele velikih vrijednosti u manjim blokova. Drugo rješenje moguće je koristiti skupa `ConnectionMultiplexer` objektima u klijent, a zatim odaberite najmanje učitati `ConnectionMultiplexer` prilikom slanja novi zahtjev. To treba spriječiti da jedan vremenskog ograničenja uzrokuje zahtjeve za i vremenskog ograničenja.

11. Ako koristite `RedisSessionStateprovider`, provjerite je li postavili ste vremenskog ograničenja pokušaj pravilno. `retrytimeoutInMilliseconds`mora biti veći od `operationTimeoutinMilliseonds`, u suprotnom će se izvršiti bez ponovne pokušaje. U sljedećem primjeru `retrytimeoutInMilliseconds` postavljen na 3000. Dodatne informacije potražite u članku [ASP.NET davatelj usluga stanja sesije Azure predmemoriju Redis](cache-aspnet-session-state-provider.md) i [upute za korištenje parametre konfiguracije web-mjesto davatelja stanje sesije i u okvir za izlazne predmemorije davatelja](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Provjera memorije na poslužitelju Azure Redis predmemorije [prateći](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` i `Used Memory`. Ako je pravilo eviction na mjestu, pokreće Redis evicting tipke kada `Used_Memory` dosegne veličina predmemorije. Najbolje `Used Memory RSS` mora biti samo malo iznad `Used memory`. Velike razlike znači da je memorije fragmentacija (interni ili vanjski. Kada `Used Memory RSS` je manji od `Used Memory`, to znači da dio memorije predmemorije sadrži je zamjenjuju operacijski sustav. U tom slučaju možete očekivati neke značajan latencies. Jer Redis omogućuje kontrolu nad mapiranje njegov alokacija za memorije stranica, visoko `Used Memory RSS` često je rezultat šiljka u memorije. Kada Redis oslobađa memorije, memorije dobiva natrag da biste alokator i alokator možda ili možda neće dati memorije natrag u sustav. Možda postoji nepodudaranje između na `Used Memory` vrijednost i memorije potrošnje kao potrebnom za operacijski sustav. Možda je zbog činjenica memorije je koristiti i objavio po Redis, ali nisu navedeni natrag u sustav. Da biste lakše prevladavanje probleme memorije možete poduzeti sljedeće korake.
    -   Nadogradite predmemoriju veću veličinu tako da se ne izvodi s programima memorije ograničenja u sustavu.
    -   Postavljanje vremena isteka na tipke tako da se starije vrijednosti su izbaciti doći.
    -   Monitora na na `used_memory_rss` predmemoriju metriku. Kada je ta vrijednost izvršenja veličina njihove predmemorije, vjerojatno ste pokrenuli prikazuju probleme s performansama. Raspodijelite podatke više shards ako su pomoću premium predmemorije ili nadogradnju na veću veličinu predmemorije.

    Dodatne informacije potražite u članku [Tlaka memorije na poslužitelju](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Dodatne informacije

-   [Koje nudi Redis predmemorije i veličine koristiti?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Kako usporednih i testirajte performanse Moje predmemorije?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Način pokretanja naredbe Redis?](cache-faq.md#how-can-i-run-redis-commands)
-   [Upute za praćenje predmemorije Redis Azure](cache-how-to-monitor.md)