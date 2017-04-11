<properties 
   pageTitle="Najčešća pitanja vezana uz baza podataka Azure SQL" 
   description="Odgovori na uobičajena pitanja kupaca o oblaka baze podataka i baze podataka SQL Azure, sustav upravljanja relacijske baze podataka tvrtke Microsoft (RDBMS) i baze podataka zatražite od kao servis u oblaku." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>Najčešća pitanja vezana uz baze podataka SQL

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Način korištenja baze podataka SQL prikazat će se na računa? 
Računi bazom podataka sustava SQL na predvidljivi satu na temelju sloju servisa + razina performansi za jednu baze podataka ili eDTUs po skup elastic baze podataka. Stvarno korištenje je izračunati i raspodijeliti zaračunava, tako da se računa može prikazati razlomke od jednog sata. Ako, na primjer, ako postoji baze podataka za 12 sati u mjesecu, računa prikazuje korištenje 0,5 dana. Uz to, servis razine + razinu performanse i eDTUs po skup su neispravno u da biste olakšali da biste vidjeli broj dana bazu podataka koju ste koristili za svaki unos u jedan mjesec.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Što se događa ako baze podataka za jedinstveno aktivan za manje od jednog sata ili koristi veći sloju servisa za manje od jednog sata?
Se naplatiti za svaki sat postoji baze podataka pomoću najveće sloju servisa + razina performansi koji primjenjuje tijekom tog sata, bez obzira na korištenje ili li baza podataka bio aktivan za manje od jednog sata. Ako, na primjer, ako stvorite jednu bazu podataka i brisanje pet minuta kasnije računa odražava naknadu h jedna baza podataka. 

Primjeri
    
- Ako stvorite osnovni bazu podataka, a zatim ga odmah nadograditi na standardni S1, vam se naplatiti brzinom standardne S1 za prvi sat.

- Ako ste nadogradili baze podataka Basic na Premium na 10:00 prijepodne i nadogradnja Završi na 1:35 AM na sljedeći dan, vam se naplatiti brzinom Premium počevši od 1 12.00 podne 

- Ako vam vraćanje na stariju verziju baze podataka od Premium osnovni pri 11 12.00 podne i dovršava pri 2:15 poslije podne., a zatim baze podataka se naplaćuje brzinom Premium do 3 navečer, nakon čega naplatiti pri osnovni stope.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Način korištenja skup elastic baze podataka prikazat će se na računa i što se događa kada promijenim eDTUs po skup?
Naknade za skup elastic baze podataka prikazivati na računu kao Elastic DTUs (eDTUs) u koracima prikazan u odjeljku eDTUs po skup na [stranici cijene](https://azure.microsoft.com/pricing/details/sql-database/). Postoji bez troškova po bazi podataka za grupe elastic baze podataka. Se naplatiti za svaki sat zajedničko područje postoji na najvišu eDTU, bez obzira na korištenje ili li na resurse bio aktivan za manje od jednog sata. 

Primjeri

- Ako stvorite grupe aplikacija standardni elastic baze podataka s 200 eDTUs u 11:18 sati, dodavanje pet baze podataka na resurse vam se naplatiti za 200 eDTUs za cijeli sat, počevši od 11 sati kroz ostatak dana.
- 2 dana, na 5:05 AM, 1 baze podataka počinje troše 50 eDTUs i stavlja stižu do dana. Baza podataka 2-5 mijenjaju između 0 i 80 eDTUs. Tijekom dana, dodajte pet drugih baza podataka koje različitim eDTUs tijekom dana. 2 dana je cijeli dan naplatiti pri 200 eDTU. 
- Na dan 3, 5 sati Dodajte drugi 15 baze podataka. Korištenje baze podataka povećava se tijekom dana točki gdje odlučite eDTUs za skup od 200 do 400 povećavaju 8:05 poslije podne. Troškove na razini 200 eDTU na snazi su do 8 pm te daju veći 400 eDTUs preostale četiri sata. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Kako pomoću sustava Active zemlj. – replikacije u programa elastic baze podataka prikazat će se na računa?
Za razliku od jedne baze podataka, pomoću [Aktivni replikacije zemlj](sql-database-geo-replication-overview.md) elastic baze podataka ne sadrži Izravni utjecaj za naplatu.  Samo se naplatiti za eDTUs resursi za sve grupe (primarni skup i sekundarne skup)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Kako koristiti značajku nadzora utječe računa? 
Nadzor ugrađen u SQL baze podataka usluga na nema extra cijena te je dostupan na Basic, standardna i Premium baze podataka. Međutim, da biste spremili zapisnike nadzora, nadzor koristi značajka račun za Azure prostora za pohranu i tečajeve za tablice i redova u spremište Azure primjenjuju se na temelju veličine izvješća zapisnika.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Kako pronaći razina sloju i performanse desnom usluge za jednu baze podataka i grupe elastic baze podataka? 
Dostupne su nekoliko Alati. 

- Za lokalne baze podataka, pomoću [Savjetnik za promjenu veličine DTU](http://dtucalculator.azurewebsites.net/) preporučujemo da baze podataka i DTUs obavezno, a analiza više baza podataka za grupe elastic baze podataka.
- Ako baze podataka za jedinstveno bi pogodnost grčkog zajedničko područje, Inteligentna modul Azure, preporučuje se skup elastic baze podataka ako ga ne vidi povijesne korištenje uzorka koji je preuzimanje. U odjeljku [nadzor i upravljanje njima u skup elastic baze podataka pomoću portala za Azure](sql-database-elastic-pool-manage-portal.md). Detalje o tome kako učiniti matematičke operacije potražite u članku [cijena i performanse zahtjevi za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-guidance.md)
- Da biste vidjeli je li kada morate birati baze podataka za jedinstveno prema gore ili prema dolje, potražite u članku [performanse smjernice za jednu baze podataka](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Koliko često se sloju ili performanse razinu servisa jedne baze podataka možete promijeniti? 
S V12 bazama podataka, možete promijeniti sloju servisa (između Basic, standardna i Premium) ili razinu performanse unutar servisa sloju (na primjer, S1 za S2) koliko god želite. Za starijih verzija baza podataka, možete promijeniti razinu servisa sloju ili performanse Ukupno četiri puta u 24-satni razdoblju.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Koliko se često prilagoditi eDTUs po skup? 
Koliko god želite.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Koliko dugo traje da biste promijenili sloju ili performanse razina usluge jedne baze podataka ili premještanje baze podataka i na skup elastic baze podataka? 
Promjena sloju servis baze podataka i premještanje i na skup zahtijeva baze podataka koju želite kopirati na platformi kao pozadinska operacija. Promjena sloja u sustavu može potrajati od nekoliko minuta do nekoliko sati ovisno o veličini baze podataka. U oba slučaja baze podataka ostaju Internetu i dostupan tijekom premještanje. Detalje o promjeni jedne baze podataka, potražite u članku [Promjena sloju servis baze podataka](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Kada koristiti bazu podataka jedan nasuprot elastic baze podataka? 
Općenito govoreći, dizajnirani grupe elastic baze podataka za uobičajene [softver-kao-na-service (SaaS) aplikacije uzorak](sql-database-design-patterns-multi-tenancy-saas-applications.md)gdje se nalazi jedna baza podataka po klijenta ili klijent. Kupnja pojedinačne baze podataka i overprovisioning zadovoljavaju varijable i Vršna zahtjev za svaku bazu podataka je često ne cijena učinkovitog. Grupe, upravljanje zajednički rad na resurse i baza podataka skaliranje gore i dolje automatski. 

Inteligentna modul Azure, preporučuje se grupe aplikacija za baze podataka prilikom korištenja uzorak preuzimanje je. Detalje potražite u članku [baze podataka SQL cijene sloju preporuke](sql-database-service-tier-advisor.md). Detaljne upute o odabiru između baza podataka jedan i elastic potražite u članku [cijena i performanse zahtjevi za grupe elastic baze podataka](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Što znači da bi do 200% prostora za pohranu Maksimalna dodijeljenu baze podataka za sigurnosne kopije za pohranu? 
Spremanje sigurnosne kopije je prostora za pohranu pridružene sigurnosne kopije automatiziranog baze podataka koji se koriste za [točke-u – vrijeme-Vrati](sql-database-recovery-using-backups.md#-point-in-time-restore) i [Zemlj vraćanja](sql-database-recovery-using-backups.md#geo-restore). Baza podataka Microsoft Azure SQL daje do 200% od maksimalno dodijeljenu baze podataka za pohranu sigurnosne kopije prostora za pohranu na bez dodatnih troškova. Na primjer, ako imate standardni DB instance s veličinom dodijeljenu DB 250 GB, ponudit će vam se s 500 GB prostora za pohranu na sigurnosnog kopiranja bez dodatnih troškova. Ako bazu podataka prelazi navedeni sigurnosne kopije prostora za pohranu, možete odabrati da biste smanjili razdoblje zadržavanja tako da se obratite podršci za Azure ili platiti vrlo sigurnosne kopije pohranu naplatiti standardne brzinom pristup za čitanje geografski suvišne prostora za pohranu (RA GRS). Dodatne informacije o naplati RA GRS potražite u članku pojedinosti cijene za pohranu.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Mogu se premještanje s Web-tvrtke nove razine servisa što treba znati?
Sada će biti povučena Azure SQL Web i poslovnih baza podataka. Razine Basic, Standardno, Premium i Elastic zamijeniti retiring Web i poslovnih baza podataka. Ne možemo ste dodatne najčešća pitanja vezana uz koji treba vam pomoći u razdoblja prijelaza. [Web- a Business Edition ruža najčešća pitanja vezana uz](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Što je kašnjenja očekivani replikacije kada zemlj replikaciju baze podataka između dva područja unutar iste Zemljopis Azure?  
Ćemo su trenutno podrške za RPO pet sekundi i kašnjenja replikacije je manji od kada se zemlj sekundarni smješten u na Azure preporučuje upareni regija i na istom sloju servisa.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Što je u galeriji očekivani replikacije stvaranja zemlj sekundarni u području isti kao primarni baza podataka?  
Na temelju Empirijska podataka nema previše razlikuju intra regija i galeriji među regija replikacijom kada na Azure preporučeno koristi upareni regija. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Postoji li mrežni problem između dviju regija, pokušajte ponovno logike rad s kada je postavljen zemlj replikacije?  
Ako postoji u Prekini vezu, ne možemo Ponovi svakih 10 sekundi da biste ponovno uspostavljanje veze.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Što učiniti da biste bili sigurni da je replicirati ključnih promjena na primarnom bazom podataka?
Sekundarni zemlj je programa replike asinkrone i smo pokušajte da bi bili punu sinkronizaciju primarni. No dajemo način da biste nametnuli sinkronizacije da biste bili sigurni replikacijom ključnih promjene (na primjer, ažuriranja lozinku). Prisilne sinkronizacije utječe performanse jer blokira pozivanja niti dok su replicirati sve izvršenja transakcije. Detalje potražite u članku [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Alati za koje su dostupne praćenje kašnjenja replikacije između primarnom bazom podataka i zemlj sekundarni?
Ne možemo izložiti kašnjenja u stvarnom vremenu replikacije između primarnom bazom podataka i zemlj sekundarni putem programa DMV. Detalje potražite u članku [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).
