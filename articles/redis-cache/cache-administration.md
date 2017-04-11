<properties 
    pageTitle="Kako upravljati Azure Redis predmemorije | Microsoft Azure"
    description="Upute za izvođenje zadataka administracije kao što je ponovno pokrenite računalo i raspored ažuriranja za Azure Redis predmemoriju"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Kako upravljati predmemorije Redis Azure

U ovoj se temi opisuje izvođenje zadataka administracije kao što su ponovnog pokretanja i zakazivanje ažuriranja za vaše Azure Redis predmemorije instance.

>[AZURE.IMPORTANT] Postavki i značajki opisanih u ovom članku dostupne su samo za Premium sloju predmemorije.


## <a name="administration-settings"></a>Postavke administracije

Postavke predmemorije Redis Azure **Administracija** omogućuju izvođenje sljedećih administrativnih zadataka za predmemoriju premium. Da biste pristupili postavke administracije, kliknite **Postavke** ili **sve postavke** iz predmemorije Redis plohu i pomaknite se na odjeljak **Administracija** u plohu **Postavke** .

![Administracija](./media/cache-administration/redis-cache-administration.png)

-   [Ponovno pokrenite računalo](#reboot)
-   [Raspored ažuriranja](#schedule-updates)

## <a name="reboot"></a>Ponovno pokrenite računalo

**Ponovno** plohu omogućuje vam da ponovno čvorovi predmemoriju. To omogućuje vam da biste testirali aplikacija za otpornost u slučaju pogreške.

![Ponovno pokrenite računalo](./media/cache-administration/redis-cache-reboot.png)

Ako imate predmemoriju premium s Klasteriranje omogućena, možete odabrati koji shards predmemoriju da biste ponovno.

![Ponovno pokrenite računalo](./media/cache-administration/redis-cache-reboot-cluster.png)

Da biste ponovno čvorovi predmemorije, odaberite željeni čvorove, a zatim kliknite **ponovno pokrenuti računalo**. Ako imate predmemoriju premium s Klasteriranje omogućena, odaberite shard(s) ponovnog pokretanja računala, a zatim **ponovno pokrenuti računalo**. Nakon nekoliko minuta, ponovno pokretanje odabrane node(s) i su sigurnosno Internetu kasnije nekoliko minuta.

Učinak na klijentske aplikacije ovisi o node(s) koji ponovnog pokretanja.

-   **Matrica** – kada je sustava čvor osnovne Azure Redis predmemorije ne uspijeva putem čvor replike te promiče matrice. Tijekom ovog prebacivanje može biti kratki intervala u kojem veze možda neće funkcionirati u predmemoriju.
-   **Podređeni** – kada je čvor podređeni sustava, postoji obično bez utjecaja na klijente predmemorije.
-   **Oba matrice i podređeni** – kada su oba čvorove predmemoriju sustava, sve podatke u nestaje predmemorije i veze s Neuspjelo predmemorije dok primarni čvor isporučuje se vratite u mrežni. Ako ste konfigurirali [postojanost podataka](cache-how-to-premium-persistence.md), najnovija sigurnosna kopija vratit će se kada predmemoriju isporučuje se vratite u mrežni način. Imajte na umu da su sve pisanja predmemoriju do kojih je došlo nakon zadnje sigurnosne kopije izgubiti.
-   **Node(s) predmemorije premium s Klasteriranje omogućeno** - prilikom ponovnog pokretanja node(s) predmemorije premium s Klasteriranje omogućena, ponašanje jednak prilikom ponovnog pokretanja node(s) predmemorije koje nisu grupirani.


>[AZURE.IMPORTANT] Ponovno pokrenite računalo dostupna je samo za Premium sloju predmemorije.

## <a name="reboot-faq"></a>Najčešća pitanja vezana uz ponovnog pokretanja računala

-   [Koje čvor ponovno za da biste testirali Moje aplikacije?](#which-node-should-i-reboot-to-test-my-application)
-   [Može li se ponovno predmemoriju da biste očistili klijentske veze?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Hoću li izgubiti podatke iz moje predmemorije li ponovno pokretanje?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Možete I ponovno Moje predmemoriju pomoću komponente PowerShell, EŽA ili druge alate za upravljanje?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Što cijene razine možete koristiti funkciju ponovno pokrenite računalo?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Koje čvor ponovno za da biste testirali Moje aplikacije?

Da biste testirali otpornost aplikacije protiv neuspjeh primarni čvora predmemorije, ponovno čvor **matrice** . Da biste testirali otpornost aplikacije protiv neuspjeh sekundarne čvora, ponovno čvor **podređeni** . Da biste testirali otpornost aplikacije protiv ukupni neuspjeh predmemorije, ponovno **oba** čvorove.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Može li se ponovno predmemoriju da biste očistili klijentske veze?

Da, ako ponovno sve veze za klijent su očistite predmemoriju. Možete korisna u slučaju gdje sve veze za klijenta koriste prema gore, na primjer zbog logike pogrešku ili pogrešku u klijentskoj aplikaciji. Svaki cijene sloju ima drugi [klijent veze ograničenja](cache-configure.md#default-redis-server-configuration) za različitih veličina, a kada se ta ograničenja dosegne više klijentske veze prihvaćaju. Ponovnog pokretanja predmemoriju omogućuje da biste očistili sve veze za klijenta.

>[AZURE.IMPORTANT] Ako klijentske veze koriste zbog pogreške logike ili pogrešku u kodu klijent, imajte na umu da StackExchange.Redis će automatski ponovno povezati kada se vratite u mrežni čvor Redis. Ako podlozi problem ne riješi, klijentske veze i dalje će biti korištena.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Hoću li izgubiti podatke iz moje predmemorije li ponovno pokretanje?

Ako ponovno **matrice** i **podređeni** čvorovi gubi se svi podaci u predmemoriji (ili u toj shard ako koristite predmemoriju premium s Klasteriranje omogućeno). Ako ste konfigurirali [postojanost podataka](cache-how-to-premium-persistence.md), vratit će se najnovija sigurnosna kopija kada se vratite u mrežni predmemoriju. Imajte na umu da su sve pisanja predmemoriju koji su se pojavili kad je izvršena sigurnosne kopije izgubiti.

Ako ponovno samo jedan od čvorove podaci nisu obično gubi, ali i dalje može biti. Primjer osnovne čvor je sustava i pisanje predmemorije je u tijeku, podaci iz predmemorije pisanje je izgubiti. Drugi scenarij gubitka podataka bit će ako ponovno jedan čvor i druge čvor će se dogoditi da biste se prema dolje zbog pogreške u isto vrijeme. Dodatne informacije o mogućim uzrocima gubitka podataka potražite u članku [što se dogodilo s mojim podacima u Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Možete I ponovno Moje predmemoriju pomoću komponente PowerShell, EŽA ili druge alate za upravljanje?

Da, za PowerShell upute potražite u članku [ponovno Redis predmemoriju](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Što cijene razine možete koristiti funkciju ponovno pokrenite računalo?

Ponovno pokrenite računalo dostupna je samo u premium cijene sloju.

## <a name="schedule-updates"></a>Raspored ažuriranja

**Raspored ažuriranja** plohu omogućuje vam da biste odredili održavanje prozor za predmemoriju. Kada prozora Održavanje nije naveden, sva ažuriranja poslužitelja Redis su bili tijekom ovaj prozor. Imajte na umu da prozora Održavanje odnosi se samo na Redis ažuriranja poslužitelja i ne želite sve Azure ažurira ili ažurira operacijskom sustavu VMs koji hostira predmemoriju.

![Raspored ažuriranja](./media/cache-administration/redis-schedule-updates.png)

Da biste odredili prozora Održavanje, potvrdite željene dana navesti održavanja prozor Pokreni sat za svaki dan i kliknite **u redu**. Imajte na umu da u prozoru vrijeme održavanja u UTC-u. 

>[AZURE.NOTE] Prozor zadani održavanje ima li ažuriranja je 5 sati. Ta vrijednost nije moguće konfigurirati na portalu Azure, ali možete je konfigurirati pomoću komponente PowerShell sustava `MaintenanceWindow` parametar cmdleta [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Dodatne informacije potražite u članku [se upravlja zakazana ažuriranja pomoću programa PowerShell, EŽA ili druge alate za upravljanje?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Zakazivanje ažuriranja najčešća Pitanja

-   [Kada se ažuriranja ne izvršiti ako ne koristim značajku raspored ažuriranja?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Koje vrste ažuriranja vrše tijekom prozora zakazano održavanje?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Mogu li upravlja zakazana ažuriranja pomoću programa PowerShell, EŽA ili druge alate za upravljanje?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Što cijene razine možete koristiti funkciju raspored ažuriranja?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Kada se ažuriranja ne izvršiti ako ne koristim značajku raspored ažuriranja?

Ako ne navedete prozora Održavanje ažuriranja mogu se postaviti u bilo kojem trenutku.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Koje vrste ažuriranja vrše tijekom prozora zakazano održavanje?

Samo Redis poslužitelja ažuriranja vrše tijekom prozora zakazano održavanje. Prozor održavanje ne primjenjuju se na Azure ažuriranja i ažuriranja VM operacijski sustav.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Mogu li upravlja zakazana ažuriranja pomoću programa PowerShell, EŽA ili druge alate za upravljanje?

Da, možete upravljati zakazanog ažuriranja pomoću sljedeće Cmdlete ljuske PowerShell.

-   [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Novi AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Novi AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Uklanjanje AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Što cijene razine možete koristiti funkciju raspored ažuriranja?

Raspored ažuriranja dostupna je samo u premium cijene sloju.

## <a name="next-steps"></a>Daljnji koraci

-   Upoznavanje s dodatnim značajkama [Azure Redis predmemorije premium sloju](cache-premium-tier-intro.md) .





