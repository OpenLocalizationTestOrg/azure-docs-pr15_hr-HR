<properties 
   pageTitle="Dizajniranje rješenja oblaka za oporavak Izrada korištenju zemlj. – replikacije baze podataka SQL | Microsoft Azure"
   description="Saznajte kako dizajnirati rješenje oblaka za oporavak Izrada tako da odaberete uzorak desnom prebacivanje."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Izrada oporavak Strategije za aplikacije koje koriste Elastic grupe aplikacija za baze podataka SQL 

Tijekom godina smo ste naučili servise u oblaku nisu kako i do teškog oštećenja kupljenih možete te će se dogoditi. Baze podataka SQL nudi brojne mogućnosti nudi za poslovanje aplikacije kada se dogodi te kupljenih. Baza podataka [elastic grupe](sql-database-elastic-pool.md) i samostalni podržava istu vrstu Izrada mogućnosti za oporavak. U ovom se članku opisuje nekoliko strategije DR elastic pools koji korištenje ove značajke programa SQL baze podataka tvrtke continuity.

Za potrebe ovog članka koristit ćemo Kanonski uzorak SaaS Neovisni aplikacije:

<i>Moderna oblaka temelji web-aplikacije dodjeljuje resurse jedan SQL baza podataka za svaki krajnjeg korisnika. Na Neovisni sadrži veliki broj korisnika i stoga koristi mnoge baze podataka, naziva klijentske baze podataka. Jer je baza podataka za klijent obično uzoraka nepredvidljive aktivnosti, na Neovisni koristi elastic skup da bi baze podataka cijena vrlo predvidljivi putem prošireni vremenskog razdoblja. Elastic skup pojednostavnjuje upravljanje performansama kada krivina aktivnosti korisnika. Osim baze podataka u klijentu aplikacije koristi nekoliko baza podataka za upravljanje korisničkim profilima, sigurnost, prikupljanje uzoraka korištenja itd. Raspoloživost klijenata za pojedinačne utjecati na dostupnost aplikacije cjelini. Međutim, dostupnosti i performanse upravljanje bazama podataka je od ključne važnosti za funkciju aplikacije i ako baza podataka za upravljanje van cijelu aplikaciju je izvan mreže.</i>  

Ne možemo obrađuje strategije DR prekrivajući raspon scenarije iz osjetljive prilikom pokretanja aplikacije trošak onima potrebe stroga dostupnost u ostatku papira.  

## <a name="scenario-1-cost-sensitive-startup"></a>Scenarij 1. Cijena osjetljive prilikom pokretanja

<i>Ja sam pokretanja tvrtke i sam iznimno cijena i mala slova.  Želim da biste pojednostavnili implementacije i upravljanja aplikacije i sam naznačiti imaju ograničeni SLA za pojedinačne korisnike. Ali želim da biste bili sigurni aplikacije kao cjelinu nikad je izvan mreže.</i>

Zadovoljili obavezne jednostavnosti implementacija sve klijentske baze podataka u jednom elastic skup u području Azure po izboru i implementacija upravljanja podataka kao samostalni zemlj replicirati baze podataka. Koristite zemlj.-vraćanja koja se nalazi pri bez dodatnih troškova za oporavak Izrada klijenata. Da biste osiguraj baze podataka u upravljanju trebali bi zemlj replicirati na drugoj regiji (korak 1). Tijeku cijena konfiguracije oporavak Izrada u ovom scenariju jednak je ukupni trošak sekundarni baze podataka. Tu konfiguraciju pokazuju na sljedećem dijagramu.

![Slika 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

U slučaju prekida u području primarni oporavak korake da biste otvorili aplikaciju na Internetu slaganje su sljedeći dijagram.

- Odmah prebacivanje upravljanje bazama podataka (2) s područjem DR. 
- Promjena u niz za povezivanje aplikacije na područje DR. Sve nove račune i klijentske baze podataka stvorit će se u području DR. Postojeće korisnike prikazat će se njegovi podaci privremeno nedostupan.
- Stvaranje elastic skup u konfiguraciji isti kao izvorni skup (3). 
- Pomoću zemlj vraćanja za stvaranje kopija klijentu baze podataka (4). Možete razmislite o pokretanje pojedinačne vraća putem veze za krajnjeg korisnika ili neke druge aplikacije sheme određene Priority (prioritet).

Sada aplikacija se vratite u mrežni u regiji DR, ali neki klijenti će se dogoditi odgode prilikom pristupanja svoje podatke.

![Slika 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Ako se prekida je privremeni, moguće je da primarni regija moguće oporaviti uz Azure prije nego što dovršite sve vraća u regiji DR. U ovom slučaju, trebali biste orkestrirali premještanje aplikacije natrag u primarni regija. Proces se koraci prikazanom na sljedećem dijagramu.
 
- Otkažite sve zahtjeve za preostala zemlj vraćanja.   
- Prebacivanje baze podataka za upravljanje primarni područje (5). Napomena: Nakon određenog područja oporavak stare očuvat su automatski postale secondaries. Sada su će se prebaciti uloge ponovno. 
- Promjena u niz za povezivanje aplikacije za pokazivanje primarni regija. Sada sve nove račune i klijentske baze podataka stvorit će se u području primarni. Neki postojeći korisnici će vidjeti svoje podatke privremeno nedostupan.   
- Postavite sve baze podataka u DR samo za čitanje da biste bili sigurni da nije moguće izmijeniti u području DR (6). 
- Za svaku bazu podataka u DR koji je promijenjen od oporavka, promijenite ili izbrišite odgovarajuće baze podataka u primarni (7). 
- Kopirajte ažurirane baze podataka iz grupe aplikacija DR primarni resurse (8). 
- Brisanje grupe aplikacija DR (9)

Sada aplikacija će biti online u području primarni s sve klijentske baze podataka dostupna u primarni.

![Slika 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Na ključne **prednosti** od ovaj strategije je niskog tijeku trošak redundanciju podataka sloju jer. Sigurnosno kopiranje uzimaju se automatski servis SQL baze podataka s bez dopuna aplikacije i u bez dodatnih troškova.  Trošak je nastale samo kada se vraćaju elastic baze podataka. **Trade-off** je potpuni oporavak sve baze podataka klijentu će značajan vremena. To ovisi o ukupni broj vraća pokrenete u regiji DR i cjelokupnog veličini klijentu baza podataka. Čak i ako se neke klijenata vraća prioritet putem drugima, će se natječu o sve na druge vraća koji su pokrenuti na istom području kao što je servis će arbitrate i throttle da biste minimizirali cjelokupan utjecaj na baze podataka za postojeće korisnike. Uz to, oporavak baze podataka u klijentu ne može započeti dok se stvara novi skup elastic u regiji DR.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scenarij 2. Starijih aplikacijom tiered servisa 

<i>Ja sam starijih aplikacije SaaS s tiered servisa ponude i drugi SLA za probno razdoblje Kupci i plaćanja klijentima. Probne korisnika mogu imati smanjiti najveću moguću resursa. Probne Kupci može potrajati nedostupnost, ali želim da biste smanjili njegov vjerojatnost. Plaćanje klijenti sve nedostupnost je leta rizik. Želim da biste provjerili te plaćanja klijenti su uvijek moći pristupati svojim podacima.</i> 

Da biste podržavaju taj scenarij, trebali biste odvojite klijenata za probno razdoblje od plaćenu klijenata tako da ih stavljanje u zasebnom elastic grupe. Probne kupci promijenile donjem eDTU po klijentu i donjem SLA s dulje vrijeme za oporavak. Plaćanje kupcima bi se u grupu s veći eDTU po klijentu i noviji SLA. Da bi najniže oporavku, baza podataka za klijent plaćanje kupcima mora biti zemlj replicirati. Tu konfiguraciju pokazuju na sljedećem dijagramu. 

![Slika 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Kao prvi scenarij upravljanja baze podataka bit će prilično aktivan pa koristite zemlj replicirati samostalni baze podataka za njega (1). To će osigurati predvidljivi performansi za nove korisnike pretplate, ažuriranja profila i ostalih operacija upravljanja. Područje u kojem se nalaze očuvat baze podataka za upravljanje će biti primarni regija i regiju u kojoj se nalazite secondaries baze podataka za upravljanje bit će regija DR.

Baza podataka klijentima za plaćanje klijentu dodijelit će aktivne baze podataka u "plaćenu" dodjeli u području primarni. Trebali biste Dodjela sekundarne skup s istim nazivom u regiji DR. Svaki klijent bi zemlj replicirati na sekundarnom skup (2). Omogućit će brzo oporavak klijentu bazu podataka pomoću prebacivanje. 

Ako se prekida se pojavljuje u području primarni, oporavak korake da biste otvorili aplikaciju na Internetu prikazane su na sljedećem dijagramu:

![Slika 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Odmah uspjeti putem upravljanja baze podataka s područjem DR (3).
- Promijenite niz za povezivanje aplikacije na područje DR. Sada sve nove račune i klijentske baze podataka stvorit će se u području DR. Postojeće probne korisnike prikazat će se njegovi podaci privremeno nedostupan.
- Prebacivanje baze podataka na klijentu plaćenu resurse u regiji DR da biste odmah vratili njihove dostupnosti (4). Budući da na prebacivanje je brzi metapodataka razine promjena smatrate za optimizaciju gdje pojedinačnih failovers aktiviraju na zahtjev veze za krajnjeg korisnika. 
- Ako je eDTU veličini sekundarne skup manja od primarnih jer sekundarne baza podataka samo potreban kapacitet za obradu Evidencija promjena dok su bile secondaries, trebali biste odmah povećanje kapaciteta skup sada kako bi odgovarao cijelog povećavaju klijenata za sve (5). 
- Stvaranje nove grupe aplikacija elastic s istim nazivom i istu konfiguraciju u regiji DR za probnu kupci baze podataka (6). 
- Nakon stvaranja skup korisnika za probno razdoblje pomoću zemlj Vrati da biste vratili baza podataka za pojedinačne probnog klijenta u novu grupu (7). Možete razmislite o pokretanje pojedinačne vraća putem veze za krajnjeg korisnika ili neke druge aplikacije sheme određene Priority (prioritet).

Sada aplikacija se vratite u mrežni regija DR. Sve plaćanje korisnici imaju pristup svoje podatke tijekom probne korisnici će se dogoditi odgode prilikom pristupanja svoje podatke.

Kada je primarni regija oporaviti tako da Azure *nakon što* ste vratili aplikacije u regiji DR možete nastaviti pokretanje aplikacije u tom području ili možete odlučiti da neće primarni regija. Ako je primarni regija oporavljene *prije* postupka prebacivanje dovrši, trebali biste failing natrag odmah. Na failback će poduzeti korake što je prikazano na sljedećem dijagramu: 
 
![Slika 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Otkažite sve zahtjeve za preostala zemlj vraćanja.   
- Prebacivanje baze podataka za upravljanje (8). Nakon određenog područja oporavak stari primarni imali automatski postaju sekundarne. Sada ponovno postane primarni.  
- Prebacivanje na plaćenu klijentu baza podataka (9). Isto tako, nakon određenog područja oporavak stare očuvat automatski postaju na secondaries. Sada na očuvat će postati ponovno. 
- Postavite vraćene probne baze podataka u kojima ste promijenili u regiji DR samo za čitanje (10).
- Za svaku bazu podataka u probne kupci DR koje se promijenile od oporavka, promijenite ili izbrišite odgovarajuće baze podataka u probne kupci primarni (11). 
- Kopirajte ažurirane baze podataka iz grupe aplikacija DR primarni resurse (12). 
- Brisanje grupe aplikacija DR (13) 

> [AZURE.NOTE] Prebacivanje operacija je asinkronog. Da biste minimizirali oporavku je važno Izvršavanje naredbe za prebacivanje klijentske baze podataka u serijama od najmanje 20 baze podataka. 

U ključne **prednosti** od ovaj strategije je pruža najveće SLA za plaćanje korisnike. Također jamčiti novi pokušaji su deblokiran čim stvaranja grupe aplikacija za probno razdoblje DR. **Trade-off** je ovaj će instalacijski program će povećati ukupni trošak baze podataka u klijentu troška sekundarni DR skup za plaćenu klijente. K tome, ako sekundarne skup je različitih veličina, plaćanje korisnici će se dogoditi donjem performanse nakon prebacivanje dok se ne skup nadogradnje u regiji DR. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scenarij 3. Geografski distribuirane aplikacije sa servisom za tiered

<i>Imam starijih SaaS aplikacijom ponuda za tiered servisa. Želim nude vrlo izrazito SLA Moje plaćenu klijentima i smanjenju rizika od utjecaj prilikom kvarove jer čak i kratak prekida mogu prouzročiti nezadovoljstva klijenta. Ključne je važnosti da plaćanje korisnici uvijek mogu pristupati svojim podacima. Jesu li u pokušaji besplatne i programa SLA ne nudi tijekom probnog razdoblja.</i> 

Da biste podržavaju taj scenarij, imat ćete tri zasebna elastic grupe. Dva jednaka veličina grupe s visoke eDTUs po bazi podataka moraju dodijeljeni resursi u dvije različite regije koja će sadržavati baza podataka za klijent plaćenu klijentima. Treći grupu koja sadrži klijenata za probno razdoblje promijenile donjem eDTUs po bazi podataka i dodijeljeni resursi na jedan od dva područja.

Da bi najniže oporavku tijekom kvarove klijentu za plaćanje kupcima baze podataka mora biti zemlj replicirati s 50% primarni baza podataka u svakom od tih dviju regija. Isto tako, svako područje promijenile 50% sekundarni baza podataka. Na taj način ako područja je izvan mreže samo 50% baza podataka plaćenu klijentima bi utjecati i bit će potrebno prebacivanje. Drugih baza podataka će ostati ostaje netaknuta. Tu konfiguraciju je što je prikazano na sljedećem su dijagramu:

![Slika 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Kao i prethodni scenariji baze podataka za upravljanje će biti prilično aktivni tako da ih trebali biste konfigurirati kao samostalni zemlj replicirati baze podataka (1). To će osigurati predvidljivi performanse novog pretplatama klijenta, ažuriranja profila i ostalih operacija upravljanja. Regija odgovora bio primarni područja za upravljanje baze podataka i regija B će se koristiti za oporavak upravljanje baze podataka.

Baza podataka klijentima za plaćanje klijentu bit će i zemlj replicirati ali očuvat, a zatim podijelite secondaries između regija odgovora i regija B (2). Na taj način baze podataka u primarni klijent utječe na nedostupnosti možete prebacivanje na područje i postaju dostupne. Druge polovice baze podataka u klijentu se uopće neće utjecati. 

Sljedeći dijagram prikazuje oporavak koraci koje morate proći ako se prekida se pojavljuje u regiji A.

![Slika 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Odmah uspjeti putem upravljanja baze podataka u području B (3).
- Promijenite niz za povezivanje aplikacije na upravljanje baze podataka u regiji B. izmjena upravljanje baze podataka da biste bili sigurni novi računi i klijentske baze podataka će se stvoriti u regiji B i postojeće baze podataka u klijentu će se pronaći postoji kao i. Postojeće probne korisnike prikazat će se njegovi podaci privremeno nedostupan.
- Prebacivanje plaćenu klijentske baze podataka da bi skup 2 u regiji B da biste odmah vratili njihove dostupnosti (4). Budući da na prebacivanje je brzi metapodataka razine promjena smatrate za optimizaciju gdje pojedinačnih failovers aktiviraju na zahtjev veze za krajnjeg korisnika. 
- Od sada skup 2 sadrži samo primarni baze podataka će se ukupno radno opterećenje u povećati da bi se trebala odmah povećati veličini eDTU (5). 
- Stvaranje nove grupe aplikacija elastic s istim nazivom i istu konfiguraciju u regiji B za probnu kupci baze podataka (6). 
- Nakon stvaranja na resurse pomoću zemlj vrati vraćanje baze podataka za pojedinačne probnog klijenta u skup (7). Možete razmislite o pokretanje pojedinačne vraća putem veze za krajnjeg korisnika ili neke druge aplikacije sheme određene Priority (prioritet).


> [AZURE.NOTE] Prebacivanje operacija je asinkronog. Da biste minimizirali oporavku je važno Izvršavanje naredbe za prebacivanje klijentske baze podataka u serijama od najmanje 20 baze podataka. 

Sada aplikacija se vratite u mrežni u regiji B. Sve plaćanje korisnici imaju pristup svoje podatke tijekom probne korisnici će se dogoditi odgode prilikom pristupanja svoje podatke.

Kada je oporaviti regija odgovora ćete morati odlučite želite koristiti područje B za probno razdoblje klijentima ili failback pomoću skup probne kupce u regiji A. Jedan kriterij može biti % baze podataka probnog klijenta Izmijenjeno oporavka. Bez obzira na tu odluku morat ćete ponovno saldo klijenata za plaćenu između dva grupe. Sljedeći dijagram prikazuje postupak kada baza podataka za probno razdoblje klijentu ne vratite regija A.  
 
![Slika 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Otkažite sve zahtjeve za preostala zemlj vraćanja probne DR resurse.   
- Prebacivanje baze podataka za upravljanje (8). Nakon oporavka u regiji stare primarni imali automatski postala sekundarne. Sada ponovno postane primarni.  
- Odaberite koje plaćenu klijentske baze podataka neće uspjeti natrag na skup 1 i započeti prebacivanje na secondaries (9). Sve baze podataka u 1 nakon određenog područja oporavak automatski postala secondaries. Sada 50% od njih će postati očuvat ponovno. 
- Smanjivanje veličine skup 2 na izvornom eDTU (10).
- Postavljanje svih vraćene probne baze podataka u regiji B samo za čitanje (11).
- Za svaku bazu podataka u probne DR koji je promijenjen od oporavka preimenovati ili izbrisati odgovarajuće baze podataka u probne primarni (12). 
- Kopirajte ažurirane baze podataka iz grupe aplikacija DR primarni resurse (13). 
- Brisanje grupe aplikacija DR (14) 

Ključne **pogodnosti** ovaj strategije su:

- Podržava većina izrazito SLA plaćanje klijenti jer osigurava se prekida ne može utjecati više od 50% baze podataka u klijentu. 
- Jamčiti novi pokušaji su deblokiran čim trag DR skup je stvoren tijekom oporavka. 
- Učinkovitije korištenje kapaciteta skup omogućuje kao 50% sekundarne baza podataka u skup 1 i 2 skup su zajamčeno manje aktivna, a zatim primarni baze podataka.

Glavni **gubitke** su:

- Operacije CRUD protiv baze podataka za upravljanje dodijelit će donjem kašnjenje za krajnje korisnike povezani područje odgovora od za krajnje korisnike povezani područje B kako će se izvršavati na temelju primarnih upravljanje baze podataka.
- Potrebna je složeniji dizajn baze podataka za upravljanje. Na primjer, svaki zapis klijenta promijenile da bi se oznake mjesto na koje je potrebno promijeniti tijekom prebacivanje i failback.  
- Plaćanje klijenti možda primijetiti donjem performansi nego inače dok se ne skup nadogradnje u regiji B. 

## <a name="summary"></a>Sažetak

U ovom se članku usredotočuje se na strategije oporavak Izrada sloju baze podataka koji se koristi aplikacija za više klijentu SaaS Neovisni. Strategije odaberete se temelji na potrebama aplikacije, kao što su tvrtke model SLA želite ponuditi klijentima, proračun od ograničenja itd... Svaki opisana strategije navode prednosti i trade-off pa nije moguće napraviti informirali odluka. Osim toga, određenu aplikaciju vjerojatno dodati druge Azure komponente. Da biste trebali biste pregledati svoje tvrtke continuity upute i orkestrirali oporavak sloju baze podataka s njima. Da biste saznali više o upravljanju oporavak aplikacija za baze podataka u Azure, pogledajte [dizajniranje oblaka rješenja za oporavak Izrada](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
