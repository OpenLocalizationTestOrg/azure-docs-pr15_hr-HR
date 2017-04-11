<properties 
    pageTitle="Kako konfigurirati postojanost podataka za Premium Redis predmemoriju Azure" 
    description="Saznajte kako konfigurirati i upravljati postojanost podataka na instance Premium sloju predmemorije Redis Azure" 
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
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Kako konfigurirati postojanost podataka za Premium Redis predmemoriju Azure

Azure Redis predmemorije ima različite predmemorije ponude koje omogućuju fleksibilnost u izbor veličina predmemorije i značajkama, uključujući nove sloju Premium.

Premium sloju Azure Redis predmemorije sadrži značajke kao što su Klasteriranje, postojanost i podršku virtualne mreže. U ovom se članku objašnjava kako konfigurirati postojanost u predmemoriji Redis Azure instanci premium.

Informacije o drugim premium značajkama predmemorije, potražite u članku [Uvod u sloju Azure Redis predmemorije Premium](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Što je postojanost podataka?
Redis postojanost omogućuje zadržava podatke pohranjene u Redis. Iskoristite snimke i sigurnosno kopiranje podataka možete učitati u slučaju hardvera. Ovo je vrlo veliko prednosti u odnosu na Basic ili Standard sloju gdje svi podaci se pohranjuju u memoriji, a može biti mogućem gubitku podataka u slučaju gdje su čvorovi predmemorije prema dolje. 

Azure Redis predmemorije nudi Redis postojanost koristite [RDB modela](http://redis.io/topics/persistence), gdje se podaci se pohranjuju u račun za Azure prostora za pohranu. Kada postojanost konfiguriran, Azure Redis predmemorije nastavi snimke predmemoriju Redis u binarnom obliku Redis disk na temelju konfigurirati učestalost sigurnosne kopije. Ako se dogodi do teškog oštećenja događaj koje onemogućuje primarni i replike predmemorije, predmemoriju je rekonstruira pomoću najnovije snimke.

Plohu **Novu predmemoriju Redis** moguće konfigurirati postojanost prilikom stvaranja predmemorije i plohu **Postavke** za postojećeg predmemorija premium.

## <a name="create-a-premium-cache"></a>Stvaranje premium predmemorije

Da biste stvorili predmemoriju i konfiguriranje postojanost, prijavu na [portal za Azure](https://portal.azure.com) i kliknite **Novo**->**podataka + prostor za pohranu**>**Redis predmemoriju**.

![Stvaranje Redis predmemorije][redis-cache-new-cache-menu]

Da biste konfigurirali postojanost, najprije odaberite jednu od **Premium** predmemorije u plohu **Odaberite vaše cijene sloju** .

![Odaberite vaše cijene sloju][redis-cache-premium-pricing-tier]

Kada je odabrana premium cijene sloju, kliknite **Redis postojanost**.

![Redis postojanost][redis-cache-persistence]

Koraci u sljedećem odjeljku opisuju kako konfigurirati postojanost Redis na novu predmemoriju premium. Kada je konfiguriran postojanost Redis, kliknite **Stvori** da biste stvorili novu predmemoriju premium s Redis postojanost.

## <a name="configure-redis-persistence"></a>Konfiguriranje postojanost Redis

Redis postojanost je konfiguriran na plohu **Redis postojanost podataka** . Za nove predmemorije ovaj plohu pristupa tijekom procesa stvaranja predmemorije, kao što je opisano u prethodnom odjeljku. Za postojećeg predmemorija plohu **Redis podataka postojanost** je pristupiti s plohu **Postavke** za predmemoriju.

![Redis postavke][redis-cache-settings]

Da biste omogućili postojanost Redis, kliknite **omogućiti** da biste omogućili sigurnosne kopije RDB (Redis baza podataka). Da biste onemogućili Redis postojanost na prethodno omogućeno premium predmemorije, kliknite **onemogućene**.

Da biste konfigurirali interval sigurnosne kopije, odaberite **Sigurnosne kopije učestalost** s padajućeg popisa. Izbori uključuju **15 minuta**, **30 minuta**, **60 minuta**, **6 sati**, **12 sati**i **24 sata**. Interval pokreće brojeći nakon prethodne sigurnosne kopije operacija uspješno dovrši i kada je minutama pokreće novu sigurnosnu kopiju.

Kliknite **Račun za pohranu** za odabir poslovnog subjekta prostora za pohranu pa odaberite ili **primarni ključ** ili **sekundarni ključ** za korištenje iz **Ključa za pohranu** padajući popis. Morate odabrati račun za pohranu u području isti kao predmemorije, a **Pohranu Premium** računa preporučuje se jer je za pohranu premium veću propusnost. 

>[AZURE.IMPORTANT] Ako je regenerated ključa za pohranu za vaš račun postojanost, morate rechoose željenu tipku iz **Ključa za pohranu** padajući popis.

![Redis postojanost][redis-cache-persistence-selected]

Kliknite **u redu** da biste spremili postojanost konfiguracije.

Sljedeće sigurnosno kopiranje (ili prvo sigurnosno kopiranje za nove predmemorije) pokreće kada interval sigurnosne kopije učestalost u minutama.



## <a name="persistence-faq"></a>Postojanost najčešća Pitanja

Sljedeći popis sadrži odgovore na najčešća pitanja o postojanost Azure Redis predmemoriju.

-   [Možete omogućiti postojanost na prethodno stvorena predmemorije?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Mogu li promijeniti učestalost sigurnosne kopije kada je li moguće stvoriti predmemoriju?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Zašto mi se sigurnosno kopiranje učestalost 60 minuta postoji li više od 60 minuta između sigurnosne kopije?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Što se događa stare sigurnosne kopije kad upišete novu sigurnosnu kopiju?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Što se događa ako mogu imati omjera na drugu veličinu i sigurnosnu kopiju vratit će se načinjene prije skaliranja operacije?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Možete omogućiti postojanost na prethodno stvorena predmemorije?

Postojanost Redis, da, moguće je konfigurirati na stvaranje predmemorije i postojećeg predmemorija premium.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Mogu li promijeniti učestalost sigurnosne kopije kada je li moguće stvoriti predmemoriju?

Da, možete promijeniti učestalost sigurnosne kopije na plohu **Redis postojanost podataka** . Upute potražite u članku [Konfiguriranje Redis postojanost](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Zašto mi se sigurnosno kopiranje učestalost 60 minuta postoji li više od 60 minuta između sigurnosnih kopija?

Interval sigurnosne kopije učestalost ne pokreće dok prethodni postupak sigurnosnog kopiranja je uspješno dovršena. Ako je sigurnosne kopije učestalost 60 minuta, a potrebno je postupak sigurnosnog kopiranja 15 minuta za uspješan dovršetak, sljedeće sigurnosno kopiranje ne pokreće do 75 minuta nakon početno vrijeme zadnje sigurnosne kopije.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Što se događa stare sigurnosne kopije kad upišete novu sigurnosnu kopiju?

Sve sigurnosne kopije osim najnovijom se automatski brišu. Brisanja može dogoditi odmah, ali starije sigurnosne kopije su dosljedan beskonačno.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Što se događa ako mogu imati omjera na drugu veličinu i sigurnosnu kopiju vratit će se načinjene prije skaliranja operacije?

-   Ako ste skalirana s odabranom mogućnošću objavu, postoji bez utjecaja.
-   Ako ste skalirana manju veličinu i imate prilagođene [baze podataka](cache-configure.md#databases) postavka koja je veće od [ograničiti baze podataka](cache-configure.md#databases) za novu veličinu, podataka u te baze podataka nije moguće vratiti. Dodatne informacije potražite u članku [je moje prilagođene baze podataka i postavljanje problematične tijekom promjene veličine?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Ako ste skalirana manju veličinu i nema dovoljno prostora u manju veličinu tako da držite sve podatke iz zadnje sigurnosne kopije, tipke će se izbaciti tijekom postupka vraćanja obično putem pravila eviction [allkeys lru](http://redis.io/topics/lru-cache) .

## <a name="next-steps"></a>Daljnji koraci
Saznajte kako koristiti više značajki predmemorije premium.

-   [Uvod u sloju Premium predmemorije Redis Azure](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
