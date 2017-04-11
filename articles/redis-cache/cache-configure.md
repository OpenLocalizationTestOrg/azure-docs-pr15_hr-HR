<properties 
    pageTitle="Upute za konfiguriranje predmemorije Redis Azure | Microsoft Azure"
    description="Razumijevanje zadanu konfiguraciju Redis Azure predmemoriju Redis i Saznajte kako konfigurirati svoje instance predmemorije Redis Azure"
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
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Upute za konfiguriranje predmemorije Redis Azure

U ovoj se temi opisuje kako pregledati i ažurirati konfiguraciju za vaše instance predmemorije Redis Azure i pokriva zadanu Redis konfiguraciju poslužitelja za instance predmemorije Redis Azure.

>[AZURE.NOTE] Dodatne informacije o konfiguriranju i korištenjem značajki za predmemorije premium potražite u članku [kako konfigurirati postojanost za Premium Redis predmemoriju Azure](cache-how-to-premium-persistence.md), [kako konfigurirati Klasteriranje za Premium Redis predmemoriju Azure](cache-how-to-premium-clustering.md)i [upute za konfiguriranje podrške za virtualne mreže za Premium Azure Redis predmemoriju](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Konfiguriranje postavki predmemorije Redis

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis predmemorije nudi sljedeće postavke na plohu **Postavke** .

![Redis postavke predmemorije](./media/cache-configure/redis-cache-settings.png)



-   [Podrška i rješavanje problema s postavkama](#support-amp-troubleshooting-settings)
-   [Opće postavke](#general-settings)
    -   [Svojstva](#properties)
    -   [Tipke za pristup](#access-keys)
    -   [Napredne postavke](#advanced-settings)
    -   [Redis Savjetnik za predmemoriju](#redis-cache-advisor)
-   [Postavke mjerila](#scale-settings)
    -   [Cijene sloju](#pricing-tier)
    -   [Redis klaster](#cluster-size)
-   [Postavke upravljanja podacima](#data-management-settings)
    -   [Redis postojanost podataka](#redis-data-persistence)
    -   [Uvoz/izvoz](#importexport)
-   [Postavke administracije](#administration-settings)
    -   [Ponovno pokrenite računalo](#reboot)
    -   [Raspored ažuriranja](#schedule-updates)
-   [Postavki dijagnostike](#diagnostics-settings)
-   [Postavke mreže](#network-settings)
-   [Postavke upravljanja resursa](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Podrška i rješavanje problema s postavkama

Postavke u odjeljku **podrška + otklanjanje poteškoća** pružiti mogućnosti za rješavanje problema u predmemoriju.

![Podrška za + otklanjanje poteškoća](./media/cache-configure/redis-cache-support-troubleshooting.png)

Kliknite **Dijagnosticiranje i rješavanje problema s** da biste dobili uobičajene probleme i Strategije za njihovo rješavanje.

Kliknite **zapisnik aktivnosti** da biste pogledali radnji na predmemoriju. Filtriranje možete koristiti i da biste proširili ovaj prikaz da biste dodali druge resurse. Dodatne informacije o radu s zapisnike nadzora, potražite u članku [Prikaz događaja i zapisnika nadzora](../monitoring-and-diagnostics/insights-debugging-with-events.md) i [nadzor operacije s Voditelj resursa](../resource-group-audit.md). Dodatne informacije o praćenje događaja Azure Redis predmemorije, potražite u članku [operacije i upozorenja](cache-how-to-monitor.md#operations-and-alerts).

**Stanje resursa** nadzirane vaše resursa i obavijestit će vas ako je pokrenut prema očekivanjima. Dodatne informacije o stanju servisa Azure resursa potražite u članku [Pregled stanja Azure resursa](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Stanje resursa je trenutno nije moguće izvješća o stanju instanci Azure Redis predmemorije smješten u virtualne mreže. Dodatne informacije potražite u članku [sve značajke predmemorije funkcioniraju kada hosting predmemorije u na VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Kliknite **Novi zahtjev za podršku** da biste otvorili zahtjev za podršku za predmemoriju.

## <a name="general-settings"></a>Opće postavke

U odjeljku **Općenite** postavke omogućuju pristup i konfigurirati sljedeće postavke predmemorije.

![Opće postavke](./media/cache-configure/redis-cache-general-settings.png)

-   [Svojstva](#properties)
-   [Tipke za pristup](#access-keys)
-   [Napredne postavke](#advanced-settings)
-   [Redis Savjetnik za predmemoriju](#redis-cache-advisor)

### <a name="properties"></a>Svojstva

Kliknite **Svojstva** da biste vidjeli informacije o predmemorije, uključujući predmemorije krajnjoj točki i priključaka.

![Redis predmemorije svojstva](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Tipke za pristup

Pritisnite **tipke za pristup** da biste pogledali ili Obnovi tipkovni prečaci za predmemoriju. Koriste se uz naziv glavnog računala i priključaka od plohu **Svojstva** tako da klijenti za povezivanje s predmemoriju.

![Redis predmemorije pristupnih tipki](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Napredne postavke

Su sljedeće postavke konfigurirajte na plohu **Napredne postavke** .

-   [Pristup priključci](#access-ports)
-   [Pravilnik za Maxmemory i maxmemory rezervirana](#maxmemory-policy-and-maxmemory-reserved)
-   [Obavijesti o Keyspace (Napredne postavke)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Pristup priključci

Prema zadanim postavkama koje nisu SSL pristup je onemogućen za nove predmemorije. Da biste omogućili priključak koji nisu SSL, kliknite **ne** **Dopusti pristup samo putem SSL** na **plohu Napredne postavke** , a zatim kliknite **Spremi**.

![Redis predmemorije pristup priključci](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Pravilnik za Maxmemory i maxmemory rezervirana

Postavke **pravilnika Maxmemory** i **maxmemory rezervirana** na plohu **Napredne postavke** konfigurirajte pravilnike memorije predmemoriju. Postavke **pravilnika za maxmemory** konfigurira eviction pravila za predmemoriju i **maxmemory rezervirane** konfigurira memorije rezervirane za procese koji nisu predmemorije.

![Redis pravila Maxmemory predmemorije](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Pravilo Maxmemory** omogućuje vam da odaberete sljedeća pravila eviction.

-   promjenjive lru – to je zadana vrijednost.
-   allkeys lru
-   promjenjive izravnim
-   Slučajni allkeys
-   promjenjive ttl
-   noeviction

Dodatne informacije o pravilima maxmemory potražite u članku [Eviction pravila](http://redis.io/topics/lru-cache#eviction-policies).

Postavka **maxmemory rezervirane** konfigurira količinu memorije u megabajtima (MB) je rezervirana za-cache operacija kao što su replikacije tijekom prebacivanje. Može se koristiti i kad imate visoke Fragmentacija proporcija. Postavljanje tu vrijednost omogućuje vam više Redis poslužitelja dosljednost kada vaš učitavanja mijenja se. Ta vrijednost potrebno postaviti veća za radnih opterećenja koji su pisanje podebljano. Ako je memorije rezervirana za operacije nije dostupna za spremanje predmemoriranih podataka.

>[AZURE.IMPORTANT] Postavka **maxmemory rezervirane** dostupna je samo za standardnu i predmemorira Premium.

### <a name="keyspace-notifications-advanced-settings"></a>Obavijesti o Keyspace (Napredne postavke)

Redis keyspace obavijesti konfigurirane na plohu **Napredne postavke** . Keyspace obavijesti da bi klijenti da biste primali obavijesti kada određenih događaja.

![Redis predmemorije Napredne postavke](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspace obavijesti i postavke **obavijesti-keyspace-događaji** dostupne su samo za standardnu i predmemorira Premium.

Dodatne informacije potražite u članku [Redis Keyspace obavijesti](http://redis.io/topics/notifications). Ogledni kod potražite u članku [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) datoteku u uzorku [svijeta pozdrav](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) .

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Savjetnik za predmemoriju

**Preporuke** plohu prikazuje preporuke za predmemoriju. Tijekom operacije normalno, prikazuju se preporuke ne. 

![Preporuke](./media/cache-configure/redis-cache-no-recommendations.png)

Ako se pojavi sve uvjete tijekom operacije predmemoriju kao što su visoke memorije, propusnost mreže ili poslužitelja, upozorenje se prikazuje na plohu **Redis predmemoriju** .

![Preporuke](./media/cache-configure/redis-cache-recommendations-alert.png)

Dodatne informacije možete pronaći na plohu **preporuke** .

![Preporuke](./media/cache-configure/redis-cache-recommendations.png)

Možete nadzirati te metriku odjeljke za [nadzor grafikone](cache-how-to-monitor.md#monitoring-charts) i [Korištenje grafikone](cache-how-to-monitor.md#usage-charts) plohu **Redis predmemoriju** .

Svaki cijene sloju ima različite ograničenja za klijentske veze, memorije i propusnosti. Ako predmemoriju izvršenja maksimalnog kapaciteta za ove metriku osigurale trajne vremenskom razdoblju, stvorit će se i preporuke. Dodatne informacije o metriku i ograničenja pregledava alatom za **preporuke** , potražite u tablici u nastavku.

| Redis predmemorije mjerenja      | Dodatne informacije potražite u člancima                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Korištenja propusnosti mreže | [Performanse predmemorije – raspoložive propusnosti](cache-faq.md#cache-performance) |
| Povezani klijenti       | [Zadani konfiguraciju poslužitelja Redis - maxclients](#maxclients)            |
| Poslužitelja             | [Korištenje grafikona – Redis učitavanja poslužitelja](cache-how-to-monitor.md#usage-charts)  |
| Korištenje memorije            | [Performanse predmemorije – veličina](cache-faq.md#cache-performance)                |

Da biste nadogradili predmemorije, kliknite **Nadogradi sada** da biste promijenili [cijene sloju](#pricing-tier) i promjena veličine predmemoriju. Dodatne informacije o odabiru cijene sloju potražite u članku [koje nudi Redis predmemorije i veličine služe?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Postavke mjerila

Postavke u odjeljku **Skaliranje** jednostavan način možete pristupiti i konfigurirati sljedeće postavke predmemorije.

![Mreža](./media/cache-configure/redis-cache-scale.png)

-   [Cijene sloju](#pricing-tier)
-   [Redis klaster](#cluster-size)

### <a name="pricing-tier"></a>Cijene sloju

Kliknite **određivanje cijena sloju** prikaz ili promjena cijene sloju za predmemoriju. Dodatne informacije o skaliranje, potražite u članku [Skaliranje Azure Redis predmemoriju](cache-how-to-scale.md).

![Redis predmemorije cijene sloju](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Redis klaster

Kliknite **(PRETPREGLED) Redis klaster** da biste promijenili veličinu klaster za tekući predmemorije premium s Klasteriranje omogućena.

>[AZURE.NOTE] Imajte na umu da dok sloju Premium predmemorije Redis Azure Izdana je Općenito dostupan, Redis klaster značajka trenutno u pretpregledu.

![Redis klaster](./media/cache-configure/redis-cache-redis-cluster-size.png)

Da biste promijenili klaster, upotrijebite klizač ili upišite broj između 1 i 10 u tekstni okvir **Shard zbroja** , a zatim kliknite **u redu** da biste spremili.

>[AZURE.IMPORTANT] Klasteriranje redis dostupna je samo za predmemorije Premium. Dodatne informacije potražite u članku [kako konfigurirati Klasteriranje za Premium Redis predmemoriju Azure](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Postavke upravljanja podacima

Postavke u odjeljku **Upravljanje podacima** jednostavan način možete pristupiti i konfigurirati sljedeće postavke predmemorije.

![Upravljanje podacima](./media/cache-configure/redis-cache-data-management.png)

-   [Redis postojanost podataka](#redis-data-persistence)
-   [Uvoz/izvoz](#importexport)

### <a name="redis-data-persistence"></a>Redis postojanost podataka

Kliknite **Redis postojanost podataka** za omogućivanje, onemogućivanje i konfiguriranje podataka postojanost predmemoriju premium.

![Redis postojanost podataka](./media/cache-configure/redis-cache-persistence-settings.png)

Da biste omogućili postojanost Redis, kliknite **omogućiti** da biste omogućili sigurnosne kopije RDB (Redis baza podataka). Da biste onemogućili Redis postojanost, kliknite **onemogućene**.

Da biste konfigurirali interval sigurnosne kopije, odaberite **Sigurnosne kopije učestalost** s padajućeg popisa. Izbori uključuju **15 minuta**, **30 minuta**, **60 minuta**, **6 sati**, **12 sati**i **24 sata**. Interval pokreće brojeći nakon prethodne sigurnosne kopije operacija uspješno dovrši i kada je minutama pokreće novu sigurnosnu kopiju.

Kliknite **Račun za pohranu** za odabir poslovnog subjekta prostora za pohranu pa odaberite ili **primarni ključ** ili **sekundarni ključ** za korištenje iz **Ključa za pohranu** padajući popis. Morate odabrati račun za pohranu u području isti kao predmemorije, a **Pohranu Premium** računa preporučuje se jer je za pohranu premium veću propusnost. Kad god je regenerated ključa za pohranu za vaš račun postojanost, morate ponovno odaberite željenu tipku s **Ključa za pohranu** padajući popis.

Kliknite **u redu** da biste spremili postojanost konfiguracije.

>[AZURE.IMPORTANT] Postojanost redis podataka dostupna je samo za predmemorije Premium. Dodatne informacije potražite u članku [kako konfigurirati postojanost za Premium Redis predmemoriju Azure](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Uvoz/izvoz

Uvoz/izvoz je postupak upravljanja Azure Redis predmemorije podataka koji omogućuje uvoz podataka u predmemoriji Redis Azure ili izvoz podataka iz predmemorije Redis Azure po uvozu i izvozu snimku Redis predmemorije baze podataka (RDB) od premium predmemorije blob stranice u račun za Azure prostora za pohranu. To vam omogućuje da migrirati između različitih instance predmemorije Redis Azure ili popunjavanje predmemorije s podacima prije korištenja.

Uvoz može se koristiti da bi se prikazala Redis kompatibilne RDB datoteke s bilo kojeg Redis poslužitelja u oblak ni okruženju, uključujući Redis sustavom Linux, Windows ili bilo kojem davatelju usluga oblaku kao što su Amazon Web-servisima i drugim korisnicima. Uvoz podataka je jednostavan način da biste stvorili predmemoriju s unaprijed unesene podatke. Tijekom postupka uvoza Azure Redis predmemorije učitava RDB datoteke sa servisa za pohranu Azure u memorije i umeće tipki u predmemoriju.

Izvoz omogućuje vam da biste izvezli podatke pohranjene u predmemoriji Redis Azure da biste Redis kompatibilne datoteke RDB. Tu značajku možete koristiti za premještanje podataka iz jedne instance Azure Redis predmemorije u drugu ili na drugi poslužitelj Redis. Tijekom postupka izvoza privremene datoteke je stvoreno na VM koji hostira instancu server Azure Redis predmemorije, a datoteka se prenosi račun zaduženog za pohranu. Kada se dovrši operaciju izvoza status uspjeha ili pogreške, privremene datoteke će se briše.

>[AZURE.IMPORTANT] Uvoz/izvoz dostupna je samo za Premium sloju predmemorije. Dodatne informacije i upute potražite u članku [Uvoz i izvoz podataka u predmemoriji Redis Azure](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Postavke administracije

Postavke u odjeljku **Administracija** omogućuju izvođenje sljedećih administrativnih zadataka za predmemoriju premium. 

![Administracija](./media/cache-configure/redis-cache-administration.png)

-   [Ponovno pokrenite računalo](#reboot)
-   [Raspored ažuriranja](#schedule-updates)

>[AZURE.IMPORTANT] Postavke u ovom odjeljku dostupne samo za Premium sloju predmemorije.

### <a name="reboot"></a>Ponovno pokrenite računalo

**Ponovno** plohu omogućuje vam da ponovno čvorovi predmemoriju. To omogućuje vam da biste testirali aplikacija za otpornost u slučaju pogreške.

![Ponovno pokrenite računalo](./media/cache-configure/redis-cache-reboot.png)

Ako imate predmemoriju premium s Klasteriranje omogućena, možete odabrati koji shards predmemoriju da biste ponovno.

![Ponovno pokrenite računalo](./media/cache-configure/redis-cache-reboot-cluster.png)

Ponovno jednu ili više čvorove predmemorije, odaberite željeni čvorove, a zatim **ponovno pokrenuti računalo**. Ako imate predmemoriju premium s Klasteriranje omogućena, odaberite shard(s) ponovnog pokretanja računala, a zatim **ponovno pokrenuti računalo**. Nakon nekoliko minuta, ponovno pokretanje odabrane node(s) i su sigurnosno Internetu kasnije nekoliko minuta.

>[AZURE.IMPORTANT] Ponovno pokrenite računalo dostupna je samo za Premium sloju predmemorije. Dodatne informacije i upute potražite u odjeljku [Administracija Azure Redis predmemorije - ponovno](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Raspored ažuriranja

**Raspored ažuriranja** plohu omogućuje određivanje prozora Održavanje Redis poslužitelja ažuriranja za predmemoriju. 

>[AZURE.IMPORTANT] Imajte na umu da prozora Održavanje odnosi se samo na Redis ažuriranja poslužitelja i ne želite sve Azure ažurira ili ažurira operacijskom sustavu VMs koji hostira predmemoriju.

![Raspored ažuriranja](./media/cache-configure/redis-schedule-updates.png)

Da biste odredili prozora Održavanje, potvrdite željene dana navesti održavanja prozor Pokreni sat za svaki dan i kliknite **u redu**. Imajte na umu da u prozoru vrijeme održavanja u UTC-u. 

>[AZURE.IMPORTANT] Raspored ažuriranja dostupna je samo za Premium sloju predmemorije. Dodatne informacije i upute potražite u odjeljku [Administracija Azure Redis predmemorije - raspored ažuriranja](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Postavki dijagnostike

U odjeljku **Dijagnostika** omogućuje konfiguriranje Dijagnostika za predmemoriju Redis.

![Dijagnostika](./media/cache-configure/redis-cache-diagnostics.png)

Kliknite **Dijagnostika** za [Konfiguriranje računa za pohranu](cache-how-to-monitor.md#enable-cache-diagnostics) koristi za pohranu Dijagnostika predmemoriju.

![Redis Dijagnostika predmemorije](./media/cache-configure/redis-cache-diagnostics-settings.png)

Da biste [pogledali metriku](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) za predmemorije i **upozorenja pravila** da biste [postavili upozorenja pravila](cache-how-to-monitor.md#operations-and-alerts)kliknite **Redis metriku** .

Dodatne informacije o Azure Redis predmemorije Dijagnostika potražite u članku [praćenje Azure Redis predmemoriju](cache-how-to-monitor.md).


## <a name="network-settings"></a>Postavke mreže

Postavke u odjeljku **mreža** jednostavan način možete pristupiti i konfigurirati sljedeće postavke predmemorije.

![Mreža](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Virtualna mrežne postavke dostupne su samo za premium predmemorije koje ste konfigurirali s podrškom za VNET tijekom stvaranja predmemoriju. Dodatne informacije o stvaranju predmemoriju premium s VNET podržava i ažuriranje postavki potražite [u](cache-how-to-premium-vnet.md)članku konfiguriranje virtualne mreže podrške za Premium Azure Redis predmemoriju.

## <a name="resource-management-settings"></a>Postavke upravljanja resursa

![Upravljanje resursima](./media/cache-configure/redis-cache-resource-management.png)

U odjeljku **oznake** omogućuje vam da organizirate resurse. Dodatne informacije potražite u članku [Korištenje oznake da biste organizirali Azure resurse](../resource-group-using-tags.md).

U odjeljku **zaključava** omogućuje vam da biste zaključali na pretplatu, grupa resursa ili resursa da biste drugim korisnicima u tvrtki ili ustanovi iz slučajno izbrišete ili izmjena ključnih resursi. Dodatne informacije potražite u članku [Lock resursi s Azure Voditelj resursa](../resource-group-lock-resources.md).

U odjeljku **korisnici** pruža podršku za kontrolu pristupa na temelju uloga (RBAC) na portalu za Azure da biste lakše organizacije zadovoljava preduvjete za upravljanje njihove pristup jednostavno i točno. Dodatne informacije potražite u članku [Upravljanje pristupom utemeljeno na ulogama na portalu za Azure](../active-directory/role-based-access-control-configure.md).

Kliknite **Izvezi predložak** omogućuje stvaranje i izvoz predloška distribuiranih resursa za buduće implementacije. Dodatne informacije o radu s predlošcima potražite u članku [uvođenja resursi s predlošcima Azure Voditelj resursa](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Zadani Redis konfiguraciju poslužitelja

Nove instance predmemorije Redis Azure konfigurirane uz sljedeće Redis konfiguracije.

>[AZURE.NOTE] Postavke u ovom odjeljku nije moguće promijeniti pomoću na `StackExchange.Redis.IServer.ConfigSet` način. Ako ovaj postupak zove se s jednim od naredbi u ovom odjeljku, Izbačena je iznimka sličnu ovoj:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Sve vrijednosti koje se može konfigurirati, kao što su **max memorije pravila**se može konfigurirati putem Azure portal ili naredbenog retka upravljanje alate kao što su Azure EŽA ili PowerShell.

|Postavka|Zadana vrijednost|Opis|
|---|---|---|
|Baza podataka|16|Zadani broj baza podataka je 16, ali možete konfigurirati različit broj koji se temelji na cijene sloju. <sup>1</sup> zadanu bazu podataka DB 0, možete odabrati neku drugu o korištenju web-veze temelj `connection.GetDatabase(dbid)` pri čemu je dbid broj između `0` i `databases - 1`.|
|maxclients|Ovisi o cijenama sloju<sup>2</sup>|To je maksimalni broj povezanog klijenti dopušteno u isto vrijeme. Kada se dostigne ograničenje Redis zatvorit će se pogreška 'maksimalni broj klijenti dosegne' slanja nove veze.|
|maxmemory pravila|promjenjive lru|Pravila Maxmemory je postavka za kako Redis ćete odabrati što uklonite kad zbirka dostigne maxmemory (veličina predmemorije koja nudi koju ste odabrali kada ste stvorili predmemoriju). S Azure Redis predmemorije zadana je postavka promjenjive-lru, čime se uklanja tipke s do isteka skupa pomoću algoritam LRU. Ta postavka može se konfigurirati na portalu za Azure. Dodatne informacije potražite u članku [Maxmemory pravila i maxmemory rezervirane](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory uzorka|3|LRU i algoritama minimalnog TTL nisu precizno algoritama, ali aproksimirane algoritama (da bi se manje memorije), pa možete odabrati i veličina uzorka za provjeru. Na primjer zadane Redis će provjeriti tri tipke i odaberite onaj koji je korišten manje nedavno.|
|lua vremensko ograničenje|5000|Maksimalno vrijeme izvođenja Lua skripte u milisekundama. Ako zbirka dostigne maksimalno vrijeme izvođenja Redis će prijaviti skriptu je još u izvođenja nakon najveći dopušteni vrijeme i započet će da biste odgovorili na upite s pogreškom.|
|lua događaj ograničenje|500|Ovo je maksimalni veličinu reda čekanja skripte događaja.|
|pubsub normalclient--međuspremnik-ograničenje izlaza klijent-izlaz--ograničenje međuspremnika|0 0 032mb 8mb 60|Ograničenja za međuspremnik klijent izlaz može se koristiti da biste nametnuli prekidanje veze sa klijenata koji se ne čitanja podataka s poslužitelja dovoljno brzo zbog nekog razloga (uobičajeni Razlog je Pub/Sub klijent ne može zauzeti poruke jednako brzo kao u programu publisher možete ponuditi ih). Dodatne informacije potražite u članku [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a>
<sup>1</sup> Ograničenja za `databases` razlikuje za svaki Azure Redis predmemoriju cijene sloju i može se postaviti na stvaranje predmemorije. Ako nema `databases` postavka navedena tijekom stvaranja predmemorije, zadana vrijednost je 16.

-   Osnovne i standardne predmemorije
    -   C0 predmemorije (250 MB) – do 16 baze podataka
    -   C1 predmemorije (1 GB) – do 16 baze podataka
    -   C2 predmemorije (2,5 GB) – do 16 baze podataka
    -   C3 (6 GB) predmemorije - do 16 baze podataka
    -   C4 predmemorije (13 GB) – najviše 32 baze podataka
    -   C5 predmemorije (26 GB) – do 48 baze podataka
    -   C6 predmemorije (53 GB) – najviše 64 baze podataka
-   Predmemorija Premium
    -   P1 (6 GB - 60 GB) - 16 baze podataka
    -   P2 (13 GB - 130 GB) – 32 baze podataka
    -   P3 (26 GB - 260 GB) – 48 baze podataka
    -   P4 (53 GB - 530 GB) – 64 baze podataka
    -   Sve predmemorije premium s Redis klaster omogućeno - Redis klaster podržava samo korištenje baze podataka 0 tako da na `databases` ograničiti za sve predmemorije premium s Redis klaster omogućeno efektivno iznosi 1, a zatim [Odaberite](http://redis.io/commands/select) naredbu nije dopušteno. Dodatne informacije potražite u članku [treba unesite željene promjene u moj klijentsku aplikaciju da biste koristili Klasteriranje?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] Na `databases` postavka može biti konfigurirano samo tijekom stvaranja predmemorije i samo pomoću programa PowerShell, EŽA ili drugim upravljanje klijentima. Na primjer konfiguriranja `databases` tijekom stvaranja predmemorije pomoću komponente PowerShell, pročitajte članak [Novo AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` razlikuje za svaki Azure Redis predmemoriju cijene sloju.

-   Osnovne i standardne predmemorije
    -   C0 predmemorije (250 MB) – najviše 256 veze
    -   C1 predmemorije (1 GB) – do 1000 veze
    -   C2 predmemorije (2,5 GB) – do 2000 veze
    -   C3 (6 GB) predmemorije - najviše 5000 veze
    -   C4 predmemorije (13 GB) – do 10 000 veze
    -   C5 predmemorije (26 GB) – najviše 15,000 veze
    -   C6 predmemorije (53 GB) – do 20 000 veze
-   Predmemorija Premium
    -   P1 (6 GB - 60 GB) – 7,500 veze
    -   P2 (13 GB - 130 GB) – 15,000 veze
    -   P3 (26 GB - 260 GB) – 30 000 veze
    -   P4 (53 GB - 530 GB) – 40,000 veze

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis naredbi koje nisu podržane u predmemoriji Redis Azure

>[AZURE.IMPORTANT] Jer konfiguracija i upravljanje instance predmemorije Redis Azure Microsoft upravlja provjerom onemogućene su sljedeće naredbe. Ako pokušate pozovite ih primit ćete poruku o pogrešci koja je slična `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  KONFIGURACIJA
>-  ISPRAVLJANJE POGREŠAKA
>-  MIGRIRANJE
>-  SPREMI
>-  ISKLJUČIVANJE
>-  SLAVEOF
>-  SKUPINE – skupine pisanje naredbe onemogućene su, ali samo za čitanje klaster naredbe dopušteno.

Dodatne informacije o Redis naredbama potražite u članku [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis konzole

Možete i sigurno izdati naredbe na vaše instance predmemorije Redis Azure pomoću **Konzole za Redis**, koji je dostupan za standardnu i predmemorira Premium.

>[AZURE.IMPORTANT] Konzola za Redis rad s VNET, Klasteriranje, i baza podataka osim 0. 
>
>-  [VNET](cache-how-to-premium-vnet.md) – kada predmemoriju je dio na VNET, samo klijente u na VNET možete pristupiti predmemoriju. Budući da konzole za Redis koristi klijentski redis cli.exe hostirane na VMs koji nisu dio vaše VNET, ga ne može povezati s predmemoriju.
>-  [Clustering](cache-how-to-premium-clustering.md) - u Redis konzole koristi klijentski redis cli.exe koji ne podržava Klasteriranje trenutno. Uslužni redis eža u [nestabilan](http://redis.io/download) grani Redis spremište na GitHub implementira osnovni podršku prilikom rada s na `-c` prijelaz. Dodatne informacije potražite u članku [Reprodukcija s klaster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) na [http://redis.io](http://redis.io) u [Redis klaster vodič](http://redis.io/topics/cluster-tutorial).
>-  Konzola za Redis čini novu vezu baze podataka 0 svaki put kada pošaljete naredbe. Ne možete koristiti u `SELECT` naredbu da biste odabrali drugu bazu podataka jer je 0 uz svaku naredbu Vraćanje baze podataka. Informacije o pokretanju Redis naredbe, uključujući promjenu u drugu bazu podataka potražite u članku [način pokretanja naredbe Redis?](cache-faq.md#how-can-i-run-redis-commands)

Da biste pristupili konzole za Redis, kliknite **konzolu** iz plohu **Redis predmemoriju** .

![Redis konzole](./media/cache-configure/redis-console-menu.png)

Za izdavanje naredbi u odnosu na instancu predmemorije, jednostavno upišite željene naredbe u konzoli sustava.

![Redis konzole](./media/cache-configure/redis-console.png)

Popis Redis naredbi koje onemogućuju Azure predmemoriju Redis potražite u članku u prethodnom odjeljku [Redis naredbe koje nisu podržane u predmemoriji Redis Azure](#redis-commands-not-supported-in-azure-redis-cache) . Dodatne informacije o Redis naredbama potražite u članku [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Premještanje predmemoriju na novu pretplatu

Predmemoriju možete premjestiti u novu pretplatu tako da kliknete **Premjesti**.

![Premještanje Redis predmemorije](./media/cache-configure/redis-cache-move.png)

Informacije o premještanje resursa u jednu grupu resursa u drugu i iz jedne pretplate na drugo, potražite u članku [Premještanje resursa ili pretplatu za novu grupu resursa](../resource-group-move-resources.md).

## <a name="next-steps"></a>Daljnji koraci
-   Dodatne informacije o radu s naredbama Redis potražite u članku [način pokretanja naredbe Redis?](cache-faq.md#how-can-i-run-redis-commands).
