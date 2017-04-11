<properties 
    pageTitle="Azure Redis predmemorije uzoraka | Microsoft Azure" 
    description="Saznajte kako koristiti predmemorije Redis Azure" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Uzorci Azure Redis predmemorije 

Ova tema sadrži popis uzorcima Azure Redis predmemorije, prekrivajući scenariji kao što su povezujete predmemorije, čitanje i pisanje podatke i iz predmemoriju i koristite davatelji ASP.NET Redis predmemoriju. Neke od primjera su koje je moguće preuzeti projektima, a neke navedene su detaljne upute i uključiti koda, ali veza koje je moguće preuzeti projekt.

## <a name="hello-world-samples"></a>Uzorci pozdrav svijeta

Uzorci u ovom odjeljku Osnove povezivanja s instancom Azure Redis predmemorije za čitanje i zapisivanje podataka u predmemoriju na različitim jezicima i Redis klijenti.

Ogledna [Pozdrav svjetske](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) prikazuje kako izvršiti razne predmemorije operacije u klijentu [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET.

Ovaj primjer prikazuje način:

-   Korištenje različitih mogućnosti povezivanja
-   Čitanje i pisanje objekte i iz predmemorije pomoću sinkrono i asinkronih operacija
-   Koristite naredbe Redis MGET/MSET za vraćanje vrijednosti navedeni tipki
-   Izvođenje Redis transakcijskih operacija
-   Rad s popisima Redis i sortirani skupova
-   Spremište .NET objekte serijalizatora JsonConvert
-   Korištenje Redis postavlja implementaciju označavanje
-   Rad s Redis klaster

Dodatne informacije potražite u dokumentaciji [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) na github, a dodatne scenariji za korištenje potražite u članku [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) testova jedinica.

[Kako koristiti Azure Redis predmemorije s Python](cache-python-get-started.md) pokazuje kako započeti s Azure Redis predmemorije pomoću Python i klijent [redis Kopiraj](https://github.com/andymccurdy/redis-py) .

[Rad s .NET objekata u predmemoriji](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) prikazuje jedan od načina serijalizirati .NET objekata da biste ih u pišete i čitate iz predmemorije Redis Azure instance. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Korištenje Redis predmemorije kao skalu izvan Backplane za SignalR platforme ASP.NET

[Koristite Redis predmemorije kao skalu izvan Backplane za ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) uzorka pokazuje kako koristiti Azure Redis predmemorije kao SignalR backplane. Dodatne informacije o backplane potražite u članku [SignalR Scaleout s Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis predmemorije kupca upita uzorka

Ovaj primjer pokazuje uspoređuje performanse između pristupa podacima iz predmemoriju i pristup podacima iz postojanost prostora za pohranu. Ovaj primjer ima dvije projekata.

-   [Ogledni prikaz kako Redis predmemorije možete poboljšati performanse predmemoriranja podataka](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Početni broj bazu podataka i predmemoriju za videozapis](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>Stanje ASP.NET sesije i izlaznog predmemoriranja

[Koristite Azure predmemorije Redis za pohranu ASP.NET SessionState i OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) uzorka pokazuje kako da koristite Azure Redis predmemoriju za pohranu ASP.NET sesije i izlazne predmemorije pomoću davatelja SessionState i OutputCache za Redis.

## <a name="manage-azure-redis-cache-with-maml"></a>Upravljanje predmemorije Azure Redis s MAML

Ogledna [Upravljanje predmemorije Redis Azure pomoću biblioteke upravljanja Azure](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) pokazuje kako možete koristiti biblioteke upravljanja Azure upravljanja - (Stvaranje / Ažuriraj / brisanje) predmemoriju. 

## <a name="custom-monitoring-sample"></a>Prilagođeno nadzor uzorka

Uzorak [podataka programa Access Redis predmemorije nadzor](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) pokazuje kako pristupiti nadzora podataka za Azure Redis predmemoriju izvan Portal za Azure.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Twitter stil Kloniraj zapisan pomoću PHP i Redis

Ogledna [Retwis](https://github.com/SyntaxC4-MSFT/retwis) je Redis pozdrav svijeta. Preporučuje se s minimalnim Twitter stil društvene mreže Kloniraj pišu pomoću Redis i i pomoću klijentskog programa [Predis](https://github.com/nrk/predis) . Izvorni kod namijenjen je biti vrlo jednostavne, a u isto vrijeme da biste prikazali različite Redis strukture podataka.

## <a name="bandwidth-monitor"></a>Monitor propusnosti

Ogledni [propusnosti monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) omogućuje praćenje propusnost na klijentskom računalu. Za mjerenje propusnosti uzorak će se izvoditi na klijentskom računalu predmemorije, upućivanje poziva u predmemoriju i pridržavajte propusnosti dojavi monitoru uzorka propusnosti.
