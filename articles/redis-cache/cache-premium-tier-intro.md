<properties 
    pageTitle="Uvod u sloju Premium predmemorije Redis Azure | Microsoft Azure" 
    description="Saznajte kako stvoriti i upravljanje Redis postojanost, Redis Klasteriranje i VNET podrška za vaše instance Premium sloju predmemorije Redis Azure" 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Uvod u sloju Premium predmemorije Redis Azure
Predmemorija za Azure Redis je raspodijeljeno, upravljanih predmemorije koji olakšava stvaranje iznimno skalabilni i odredište aplikacijama unosom super-brzog pristupa podacima. 

Novi sloju Premium je programa Enterprise spreman sloju, što obuhvaća sve značajke standardna sloja i drugo, kao što je bolje performanse, povećali radnih opterećenja, Izrada oporavak, uvoz/izvoz i bolja sigurnost. Nastavili čitati da biste saznali više o dodatnim značajkama programa predmemorije sloju Premium.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Bolje performanse u usporedbi s sloju standardni prikaz ili Basic
**Bolje performanse putem standardno ili osnovni sloju.** Predmemorija u sloju Premium uvode se na hardvera koji sadrži procesora koji se brže i daje bolje performanse usporedbi Basic ili standardni sloju. Predmemorija sloju Premium imati veću propusnost i donjem latencies. 

**Propusnost za predmemoriju iste veličine veća je u Premium to standardni sloju.** Na primjer, propusnost 53 GB P4 (Premium) predmemorije je za 250K u sekundi to 150 K za C6 (standardni prikaz).

Dodatne informacije o veličini propusnost i propusnosti s premium predmemorije, potražite u članku [Azure Redis predmemorije najčešća Pitanja](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis postojanost podataka
Razina Premium omogućuje zadržava podataka predmemorije u račun za Azure prostora za pohranu. U predmemoriju Basic/standardna svi podaci se pohranjuju samo u memoriji. U slučaju podlozi infrastrukture postoje problemi može biti mogućem gubitku podataka. Preporučujemo korištenje značajke za postojanost Redis podataka u sloju Premium da biste povećali otpornost od gubitka podataka. Azure Redis predmemorije nudi RDB i AOF (uskoro dostupno) mogućnosti u [Redis postojanost](http://redis.io/topics/persistence). 

Upute o konfiguriranju postojanost potražite u članku [kako konfigurirati postojanost za Premium Redis predmemoriju Azure](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Redis klaster
Ako želite da biste stvorili predmemorije veća od 53 GB ili želite shard podataka preko više Redis čvorove, možete koristiti Redis Klasteriranje koja je dostupna u sloju Premium. Svaki čvor sastoji se od par predmemorije primarni/replike upravlja Azure visoke dostupnosti. 

**Klasteriranje redis vam maksimalno skaliranje i propusnost.** Propusnost povećava Linearno kako povećavate broj shards (čvorove) u klasteru. Npr. Ako stvarate P4 Klaster od 10 shards, a zatim dostupna propusnost je 250K * 10 = 2,5 milijuna za u sekundi. Pročitajte članak [Azure Redis predmemorije najčešća pitanja vezana uz](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) dodatne pojedinosti o veličini, propusnost i propusnosti s premium predmemorije.

Početak rada s Klasteriranje, potražite u članku [kako konfigurirati Klasteriranje za Premium Redis predmemoriju Azure](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Bolja sigurnost i odvajanja

Predmemorije stvorene u sloju Basic ili Standard su dostupne na javnog Interneta. Predmemoriju ograničen je pristup na temelju tipkovni prečac. S sloju Premium možete dodatno omogućiti samo klijenti unutar navedeni mreže mogu pristupiti predmemoriju. Možete implementirati Redis predmemorije u [Azure virtualne mreže (VNet)](https://azure.microsoft.com/services/virtual-network/). Možete koristiti sve značajke VNet kao što su podmreže, pravilnici kontrole programa access, i ostalih značajki da biste dodatno ograničiti pristup Redis.

Dodatne informacije potražite [u](cache-how-to-premium-vnet.md)članku konfiguriranje podrške za virtualne mreže za Premium Azure Redis predmemoriju.

## <a name="importexport"></a>Uvoz/izvoz

Uvoz/izvoz je postupak upravljanja Azure Redis predmemorije podataka koji omogućuje uvoz podataka u predmemoriji Redis Azure ili izvoz podataka iz predmemorije Redis Azure po uvozu i izvozu snimka Redis predmemorije baze podataka (RDB) od premium predmemorije blob stranice u račun za Azure prostora za pohranu. To vam omogućuje da migrirati između različitih instance predmemorije Redis Azure ili popunjavanje predmemorije s podacima prije korištenja.

Uvoz može se koristiti da bi se prikazala Redis kompatibilne RDB datoteke s bilo kojeg Redis poslužitelja u oblak ni okruženju, uključujući Redis sustavom Linux, Windows ili bilo kojem davatelju usluga oblaku kao što su Amazon Web-servisima i drugim korisnicima. Uvoz podataka je jednostavan način da biste stvorili predmemoriju s unaprijed unesene podatke. Tijekom postupka uvoza Azure Redis predmemorije učitava RDB datoteke sa servisa za pohranu Azure u memorije i umeće tipki u predmemoriju.

Izvoz omogućuje vam da biste izvezli podatke pohranjene u predmemoriji Redis Azure da biste Redis kompatibilne datoteke RDB. Tu značajku možete koristiti za premještanje podataka iz jedne instance Azure Redis predmemorije u drugu ili na drugi poslužitelj Redis. Tijekom postupka izvoza, stvorit će se privremena datoteka na VM koji hostira instancu server Azure Redis predmemorije, a datoteka se prenosi račun zaduženog za pohranu. Kada se dovrši operaciju izvoza status uspjeha ili pogreške, privremene datoteke će se briše.

Dodatne informacije potražite [u](cache-how-to-import-export-data.md)članku uvoz podataka u i izvoz podataka iz predmemorije Redis Azure.

## <a name="reboot"></a>Ponovno pokrenite računalo

Razina premium omogućuje vam da ponovno čvorovi vaše predmemorije na zahtjev. Time da biste testirali aplikacija za otpornost u slučaju pogreške. Možete ponovno pokrenuti sljedeći čvorove.

-   Glavni čvor predmemoriju
-   Podređeni čvora predmemoriju
-   Matrica i podređeni čvorove predmemoriju
-   Kada pomoću Klasteriranje premium predmemorije, možete ponovno matricu, podređeni ili oba čvorove za pojedinačne shards u predmemoriji

Dodatne informacije potražite u članku [ponovno](cache-administration.md#reboot) i [Najčešća pitanja vezana uz ponovno pokrenuti računalo](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Raspored ažuriranja

Zakazana ažuriranja značajka omogućuje određivanje održavanja prozor za predmemoriju. Kada prozora Održavanje nije naveden, sva ažuriranja poslužitelja Redis su bili tijekom ovaj prozor. Da biste odrediti prozora Održavanje, odaberite željeni dana i odredite održavanje prozor počnite sat za svaki dan. Imajte na umu da u prozoru vrijeme održavanja u UTC-u. 

Dodatne informacije potražite u članku [raspored](cache-administration.md#schedule-updates) i [Raspored ažuriranja najčešća Pitanja](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Samo Redis poslužitelja ažuriranja vrše tijekom prozora zakazano održavanje. Prozor održavanje ne primjenjuju se na Azure ažuriranja i ažuriranja VM operacijski sustav.

## <a name="to-scale-to-the-premium-tier"></a>Da biste skalirali sloju premium

Da biste skalirali sloju premium, samo odaberite neku od premium razine u plohu **Promjena cijene sloju** . Također mogu mijenjati veličinu predmemorije u sloju premium pomoću komponente PowerShell i EŽA. Detaljne upute potražite u članku [kako ga skaliranje Azure Redis predmemorije](cache-how-to-scale.md) i [Da biste automatizirali skaliranja operacija](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Daljnji koraci

Stvorite predmemoriju i Istražite nove značajke sloju premium.

-   [Kako konfigurirati postojanost za Premium Redis predmemoriju Azure](cache-how-to-premium-persistence.md)
-   [Upute za konfiguriranje podrške za virtualne mreže za Premium Azure Redis predmemoriju](cache-how-to-premium-vnet.md)
-   [Kako konfigurirati Klasteriranje za Premium Redis predmemoriju Azure](cache-how-to-premium-clustering.md)
-   [Upute za uvoz podataka u i izvoz podataka iz predmemorije Redis Azure](cache-how-to-import-export-data.md)
-   [Kako upravljati predmemorije Redis Azure](cache-administration.md)
  

