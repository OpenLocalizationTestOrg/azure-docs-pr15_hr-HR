<properties
    pageTitle="Azure SQL baze podataka i performanse za jednu baze podataka | Microsoft Azure"
    description="U ovom se članku pojednostavljuju određivanje koji sloju servisa da biste odabrali za svoju aplikaciju. Preporučuje se i načina za ugađanje aplikacije da biste na najbolji s bazom podataka SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL baze podataka i performanse za jednu baze podataka

Baze podataka SQL Azure nudi tri [servisa razine](sql-database-service-tiers.md): Basic, standardna i Premium. Svaki servis sloju isključivo izolira resursi SQL baze podataka možete koristiti i jamčiti predvidljivi performansi za taj servis razinu. U ovom se članku nudimo upute koje omogućuju vam da odaberete sloju usluge za svoju aplikaciju. Ne možemo navode i načina na koje možete ugađanje aplikacije da biste izvukli iz baze podataka SQL Azure.

>[AZURE.NOTE] U ovom se članku usredotočuje se na performanse smjernice za jednu baze podataka u bazi podataka SQL Azure. Performanse uputama vezane uz grupe elastic baze podataka, potražite u članku [cijena i performanse zahtjevi za grupe elastic baze podataka](sql-database-elastic-pool-guidance.md). No imajte na umu, koji možete mnoge ugađanje preporuke u ovom članku odnosi na baze podataka u programa elastic baze podataka i dobiti slične pogodnosti performansi.

Ovo su tri razine servisa Azure SQL baze podataka koje možete odabrati (performanse mjeri se u bazu podataka propusnost jedinice ili [DTUs](sql-database-what-is-a-dtu.md):

- **Osnovni**. Osnovna usluga sloju ponuda dobre performanse predvidljivost za svaku bazu podataka, sat preko sat. U bazi osnovni dovoljno resursa podržava dobre performanse u male baze podataka koja ne sadrži više istovremeni zahtjevi.
- **Standardni**. Razina standardnog servisa nudi predvidljivost bolje performanse i podiže na traci za baze podataka koji imaju više istovremeni zahtjevi, kao što su radna grupa i web-aplikacije. Kad odaberete standardnog servisa sloju baze podataka, možete veličinu u aplikaciji baze podataka koji se temelji na predvidljivi performanse minute putem minute.
- **Premium**. Servis sloju Premium nudi predvidljivi performanse, drugi na drugo, za svaku Premium bazu podataka. Kad odaberete servisa sloju Premium, veličinu učitavanja Vršna uz tu bazu podataka na temelju aplikacije za baze podataka. Plan uklanja slučajeva koje performanse varijancu mogu prouzročiti small upite za trajati dulje nego je očekivano u Latencija osjetljivi operacije. Ovaj model znatno može pojednostavniti Provjera valjanosti ciklusa razvoja i proizvoda za aplikacije koje morate donijeti istaknuti izjave o potrebama Vršna resursa, varijancu na performanse ili Latencija upita.

Pri svakom sloju servisa postavite razinu performanse da biste imali fleksibilnost samo za kapacitet potrebno je plaćanje. Radno opterećenje promjene možete [prilagoditi kapaciteta](sql-database-scale-up.md), gore ili dolje. Ako, na primjer, ako tijekom razdoblja kupovinu natrag u školu je visoke svoje radno opterećenje baze podataka, možda povećanje razine performansi za bazu podataka za određeno vrijeme srpanj kroz rujan. Možete smanjiti kada istekne vaša Vršna razdoblja. Rješenje smanjite plaćanje nakon primitka optimiziranje oblaka okruženje za sezonalnost o vašem poslovanju. Ovaj model funkcionira i dobro za softver proizvoda izdanje ciklusa. Test tima mogu dodijeliti kapaciteta dok je testiranje se pokreće, a zatim otpustite te kapaciteta kad će završiti s testiranjem. U modelu kapaciteta zahtjev, plaćate kapaciteta je potreban i izbjegli trošite na namjenski resursi koji će se rijetko koristiti.

## <a name="why-service-tiers"></a>Zašto servisa razine?

Iako se svaki radno opterećenje baze podataka mogu se razlikovati, svrha razine servis je pružanje predvidljivost performanse na različitim razinama performanse. Korisnicima putem uvjete resursa za veliki baze podataka možete raditi u više namjenski računalno okruženje.

### <a name="common-service-tier-use-cases"></a>Uobičajeni sloju servis pomoću slučajeva

#### <a name="basic"></a>Osnovni

- **Upravo započinjete s bazom podataka SQL Azure**. Aplikacijama koje su u razvoju često ne morate razine visoke performanse. Baza podataka za osnovni su idealna okruženje za razvoj baze podataka, u trenutku najniža cijena.
- **Imate bazu podataka s jednog korisnika**. Aplikacije koje se pridružiti jednom korisniku baze podataka obično ne tretiraju visoke istodobnosti i performanse. Ovo su aplikacije kandidata za osnovne sloja u sustavu.

#### <a name="standard"></a>Standardna

- **Baza podataka sadrži više istovremeni zahtjevi**. Aplikacije koje servisa više korisnika istodobno obično potrebno više razine performansi. Na primjer, web-mjesta koja se moderiranje promet ili odjela aplikacije koje je potrebno više resursa prikladna su sloju standardnog servisa.

#### <a name="premium"></a>Premium

Premium servis sloju korištenje uglavnom imaju jedan ili više te značajke:

- **Učitavanje visoke Vršna**. Aplikacije koje je potrebno mnogo procesora, memorije ili ulazni i izlazni / (i) da biste dovršili operacije zahtijeva razinu namjenski, visoke performanse. Ako, na primjer, postupak baze podataka u poznate trošiti nekoliko jezgri procesora dulje vrijeme je prijedlog za servis sloju Premium.
- **Mnoge istovremeni zahtjevi**. Neka aplikacija za baze podataka, na primjer, servisa mnogo istovremeni zahtjevi kada posluživanje web-mjesto koje sadrži velikog prometa glasnoću. Osnovne i standardne servisa razine ograničavanje broja istovremeni zahtjevi po bazi podataka. Aplikacijama koje je potrebno više veza morate odabir veličine odgovarajuće rezervacija učiniti maksimalan broj potrebnih zahtjeva.
- **Niske latencije**. Neke aplikacije moraju jamči odgovor iz baze podataka u minimalnog vremenu. Ako određeni pohranjena procedura zove se kao dio šira operacije klijenta, možda će vam se zahtjev za povrat ulaganja taj poziv prisutno više od 20 milisekundama, 99 iznimaka tome pravilu. Ovu vrstu prednosti aplikacije iz servisa sloju Premium, provjerite je li potreban računalno power dostupna.

Razina usluge koje su vam potrebne za baze podataka sustava SQL ovisi o Vršna učitavanja zahtjevima za svaku dimenzije resursa. Neke aplikacije koristite trivial količinu jedan resurs, ali vrlo tretiraju ostali resursi.

## <a name="service-tier-capabilities-and-limits"></a>Mogućnosti usluge sloju i ograničenja
Razina usluge sloju i performanse povezan je s različitim ograničenja i karakteristike performansi. U ovoj su tablici opisuju ove značajke za jednu bazu podataka.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

Sljedeći odjeljci sadrže dodatne informacije o kako prikazati pomoću koje se odnose na ta ograničenja.

### <a name="maximum-in-memory-oltp-storage"></a>Maksimalna OLTP u memorijski prostor za pohranu

Prikaz **sys.dm_db_resource_stats** možete koristiti za praćenje korištenje Azure u memoriji za pohranu. Dodatne informacije o nadzoru potražite u članku [OLTP u memoriji za nadzor prostora za pohranu](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Trenutno Azure u memoriji mrežna transakcija processing (OLTP) pretpregled podržana je samo za jedan baze podataka. Ne možete je koristiti u bazama podataka u grupe elastic baze podataka.

### <a name="maximum-concurrent-requests"></a>Maksimalna istovremeni zahtjevi

Da biste vidjeli broj istovremeni zahtjevi, pokrenite ovaj Transact-SQL upit na SQL baze podataka:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Da biste analizirali radno opterećenje lokalne baze podataka sustava SQL Server, izmijenite ovaj upit da biste filtrirali određene baze podataka koje želite analizirati. Ako, na primjer, ako imate lokalne baze podataka pod nazivom MyDatabase, ovaj Transact-SQL upit vraća broj istovremeni zahtjevi u toj bazi podataka:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

To je samo snimke na jednom mjestu u vremenu. Da biste bolje razumjeli radno opterećenje i preduvjeti Istodobni zahtjev, morat ćete prikupljanje mnogo uzoraka tijekom vremena.

### <a name="maximum-concurrent-logins"></a>Maksimalna Istodobni prijave

Možete analizirati korisnika i aplikacije uzoraka da biste dobili ideju učestalosti prijave. Možete pokrenuti opterećenje stvarnog života u okruženju test da biste bili sigurni da ste odlazak to ili druga ograničenja ćemo razmotriti u ovom članku. Ne postoji jedan upita ili prikaz dinamičkog upravljanja (DMV) koji može vam prikazati Istodobni broji prijavu ili povijest.

Ako više klijenata koriste isti niz za povezivanje, servis potvrđuje svaki prijava. Ako se 10 korisnika istodobno povezati s bazom podataka pomoću isto korisničko ime i lozinku, bio bi 10 Istodobni prijave. To ograničenje se primjenjuje samo na trajanje prijave i provjeru autentičnosti. Ako se isti 10 korisnika sekvencijalno s bazom podataka, broj Istodobni prijave nikad biti veća od 1.

>[AZURE.NOTE] To ograničenje trenutno se odnosi na baze podataka u grupe elastic baze podataka.

### <a name="maximum-sessions"></a>Maksimalan broj sesija

Da biste vidjeli broj trenutnog aktivnih sesija, pokrenite ovaj Transact-SQL upit na SQL baze podataka:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Ako ste analiza radno opterećenje programa za lokalnog sustava SQL Server, izmijenite upita radi fokusiranja na određene baze podataka. Ovaj upit će vam olakšati određivanje moguće sesiju potrebama za bazu podataka ako namjeravate da je premjestite na baze podataka SQL Azure.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Ponovno tih upita vratite točke u vrijeme count. Ako više uzoraka prikupljanje s vremenom, morat ćete najbolje razumijevanje sesije koristiti.

Za analizu SQL baze podataka, možete dobiti povijesne Statistika na sesije. Upit **sys.resource_stats**pa koristite **active_session_count** stupac. U sljedećem odjeljku Dodatne informacije o korištenju ovaj prikaz.

## <a name="monitor-resource-use"></a>Nadzor korištenja resursa
Dva prikaza možete pridonijeti utvrđivanju nadzor korištenja resursa za bazu podataka za SQL odnosu servisnog sloja sustava:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Prikaz [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) možete koristiti u svakoj SQL baze podataka. Prikaz **sys.dm_db_resource_stats** prikazuje nedavne resursa koristite podatke u sloju servisa. Prosječna postotke procesora, podataka/i, zapisivanja u zapisnik i memorije bilježe svakih 15 sekundi i održavaju za 1 sat.

Budući da se ovaj prikaz omogućuje precizniji susret korištenja resursa, koristiti **sys.dm_db_resource_stats** prvog za sve trenutnom stanju analizu ili otklanjanje poteškoća. Na primjer, ovaj upit prikazuje korištenja average i maksimalne resursa za trenutnu bazu podataka tijekom proteklog hour:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Ostale upita potražite u članku primjeri u [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats

Prikaz [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) u **glavnom** bazom podataka ima dodatne informacije koje olakšavaju praćenje performansi baze podataka sustava SQL na razini njegov određenog servisa sloju i performanse. Podaci se prikupljaju svakih 5 minuta i održava se za otprilike 35 dana. Taj je prikaz korisne su za longer-term povijesne analize kako baze podataka sustava SQL koristi resurse.

Sljedeće grafikonu prikazuju na procesora korištenja resursa za bazu podataka Premium s razinom performansi P2 svaki sat u tjednu. U ovom grafikonu počinje u ponedjeljak, prikazuje 5 radne dane i prikazuje vikenda, kada se događa puno manje u aplikaciji.

![Korištenje resursa baze podataka za SQL](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Iz podataka, ova baza podataka je trenutno opterećenje procesora Vršna za samo veće od 50 posto procesora koristi odnosu performanse razinu P2 (midday na utorak). Ako je procesora označile faktor u profilu resursa aplikacije, zatim možda odlučite da je P2 razinu desnom performanse na jamči da povećavaju uvijek najbolje odgovara. Ako ste i očekivali aplikaciju s vremenom, je dobro imati na dodatne resurse međuspremnik tako da se aplikacija ne ikad dosegne ograničenje razina performansi. Ako povećati razinu performanse, moći ćete izbjeći kupca vidljiva pogreške koje se mogu pojaviti kada bazi podataka nema dovoljno power obrade zahtjeva za učinkovito, posebice u okruženju Latencija osjetljivi. Primjer je baza podataka koje podržava aplikacija koja paints web-stranice na temelju rezultata pozive baze podataka.

Imajte na umu da druge vrste aplikacija može shvatiti na istom grafikonu drugačije. Na primjer, ako aplikacija pokušava obradu podataka plaće svakog dana, a na istom grafikonu, tu vrstu "obrada" modela možda učinite precizno na razini P1 performanse. Performanse razinu P1 sadrži 100 DTUs u usporedbi s 200 DTUs na razini P2 performansi. Performanse razinu P1 nudi pola performanse P2 razinom performansi. Tako, 50 posto korištenje procesora u P2 iznosi 100 posto procesora koristi u P1. Ako aplikacija nije vremensko ograničenje, možda nije važno ako posao traje dva sata ili 2.5 sata da biste završili, ako se danas izvršiti. Aplikacije iz te kategorije vjerojatno možete koristiti na razinu P1 performansi. Možete iskoristiti prednost činjenica da postoje razdoblja tijekom dana kada je niže, korištenja resursa tako da se sve "veliki Vršna" možda stao u jednu na troughs u nastavku dan. Performanse razinu P1 možda je dobro za tu vrstu aplikacije (i spremanje novac), pod uvjetom da poslove može završiti na vrijeme svaki dan.

Azure SQL baze podataka otkriva potrošena informacije o resursu za svaku aktivni bazu podataka u prikazu **sys.resource_stats** **glavnom** bazom podataka u svakom poslužitelju. Podaci u tablici se pridružuje razdobljima 5 minuta. Pomoću servisa razine Basic, standardna i Premium podataka može potrajati više od pet minuta da se prikazuju u tablici da bi se ti podaci korisnijim povijesne analize umjesto pri--sadržajima analize. Prikaz **sys.resource_stats** da biste vidjeli Nedavna povijest baze podataka, a da biste provjerili koristi li se rezervacija odaberete isporučena performanse želite po potrebi upita.

>[AZURE.NOTE] Morate biti povezani s **glavnom** bazom podataka logičke poslužitelja baze podataka SQL upit **sys.resource_stats** u sljedećim primjerima.

U ovom primjeru prikazano kako se prikazuju podatke u tom prikazu:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Prikaz sys.resource_stats kataloga](./media/sql-database-performance-guidance/sys_resource_stats.png)

Sljedeći primjer prikazuje različite načine na koje možete koristiti prikaz kataloga **sys.resource_stats** da biste dobili informacije o tome kako SQL baze podataka koriste resurse:

1. Da biste vidjeli proteklog tjedna resursa za userdb1 baze podataka, možete pokrenuti ovaj upit:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Da biste procijenili koliko će se dobro svoje radno opterećenje odgovara razini performanse, morate kroz razine naniže u svakom aspekte metriku resursa: procesora, čitanja, pisanja, broj zaposlenici zaduženi za i broj sesija. Evo promijenjenih upita pomoću **sys.resource_stats** da biste prijavili average i maksimalne vrijednosti te metriku resursa:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Pomoću tih informacija o average i maksimalne vrijednosti svakog mjerenja resursa procijenite koliko će se dobro svoje radno opterećenje pristaju razinu performanse koje ste odabrali. Obično prosječne vrijednosti iz **sys.resource_stats** vam dobro osnovne da biste koristili o veličini cilj. Mora biti vaš primarni mjera uređaj. Na primjer, možda koristite sloju standardnog servisa razinom S2 performansi. Prosjek namijenjen postotke procesora i/i čitanja i pisanja ispod 40 posto, Prosječan broj zaposlenici zaduženi za je ispod 50 i Prosječni broj sesija je ispod 200. Možda svoje radno opterećenje uklapaju u S1 razinom performansi. Da biste vidjeli je li bazu podataka stane u ograničenja tempiranja i sesiju jednostavno je. Da biste vidjeli odgovara li baze podataka u niže razine performansi koja se odnosi na procesora, čita i pisanja, dijeljenje DTU broj niže razine performanse prema broju DTU razinu zaštite trenutnog performanse, a rezultat pomnožite sa 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Rezultat je relativna performanse razlika između dva performanse razine postotak. Ako koristite resursa veća od iznosa, svoje radno opterećenje možda uklapaju u niže razine performansi. Međutim, morate pogledati sve raspone vrijednosti za korištenje resursa i odredite, postotak, koliko često želite svoje radno opterećenje baze podataka pristaju niže razine performansi. Sljedeći upit proizvodi postotak odstupanjem po dimenzija resursa, ovisno o prag 40 posto koje ćemo izračunato u ovom primjeru:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Na temelju svoje baze podataka usluge razine cilj (a SPORIM), možete odlučiti hoće li svoje radno opterećenje pristaju niže razine performansi. Ako radno opterećenje svoje baze podataka a SPORIM je postotak 99.9 i prethodni upit vraća vrijednosti veće od 99.9 postotak za sva tri resursa dimenzije, svoje radno opterećenje vjerojatno će se uklapaju u niže razine performansi.

    Pogled na postotak odstupanjem i daje uvid u imate premjestite se na sljedeće više razine performanse zadovoljavaju vaše a SPORIM. Na primjer, userdb1 prikazuje sljedeće procesora koristi za proteklog tjedna:

  	| Prosječna procesora posto | Maksimalna procesora posto |
  	|---|---|
  	| 24.5 | 100,00 |

    Prosječna procesora je o tromjesečje ograničenje razinu performansi koja bi uklapaju u razinom performansi baze podataka. No prikazuje najveću vrijednost u bazu podataka dođe do limita razinom performansi. Morate da biste prešli u sljedeću višu razinu performansi? Morate pogledati kako više puta svoje radno opterećenje dođe 100 posto i usporedite radno opterećenje svoje baze podataka a SPORIM.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Ako ovaj upit vraća vrijednost manje od 99.9 postotak za bilo koju od tri resursa dimenzije razmotrite ili premještanje u sljedeći višu razinu performanse ili koristiti aplikaciju usklađivanje postupke da biste smanjili opterećenje bazom podataka sustava SQL.

4. Ovu vježbu u obzir uzima vaše povećava predviđene radno opterećenje u budućnosti.

## <a name="tune-your-application"></a>Precizno podesite aplikacije

U tradicionalni lokalnog sustava SQL Server, postupak planiranja kapaciteta početne često je odvojeni od postupka pokretanja aplikacije u radnog. Licence za hardver i proizvoda su kupljenje najprije i ugađanje performansi nakon dovršetka. Kada koristite baze podataka SQL Azure, je dobro interweave postupka pokrenuti aplikaciju i ugađanje ga. S modelom plaćanja kapaciteta na zahtjev, možete ugađanje aplikacije da biste pomoću minimalne resursa potrebno sada umjesto overprovisioning na hardveru na temelju pokušaja budućeg rasta planova za aplikaciju, koji se često nisu ispravni. Neki klijenti možda ne želite ugađanje aplikacije, a umjesto toga možete overprovision Hardverski resursi. Taj se način mogu dobro ako ne želite da biste promijenili ključa aplikacije tijekom zauzet razdoblja. No usklađivanje aplikaciju možete minimizirati uvjete resursa i donjem mjesečni računi prilikom korištenja servisa razine u bazi podataka SQL Azure.

### <a name="application-characteristics"></a>Značajke aplikacije

Iako su baze podataka SQL Azure servisa razine dizajnirani da biste poboljšali performanse stabilnosti i predvidljivost za aplikaciju, najbolje postupke omogućuju ugađanje aplikacije da biste bolje iskoristili resursa na razini performansi. Iako mnoge aplikacije značajan performansi jednostavno prebacivanjem na višu razinu performanse ili sloju servisa neke aplikacije dodatne usklađivanje prednosti s više razine pružanja usluge. Poboljšane performanse, uzmite u obzir ugađanje dodatne aplikacije za aplikacije koje su te značajke:

- **Programi koji imaju slabe performanse zbog "chatty" ponašanje**. Chatty aplikacije provjerite operacija pristup viškom podataka koji su osjetljivi na latenciju mreže. Možda ćete morati izmijeniti ove vrste aplikacije da biste smanjili broj operacije podataka programa access s bazom podataka SQL. Ako, na primjer, pomoću tehnika kao što su grupnog slanja promjena ad hoc upite ili premještanje upiti pohranjene procedure mogu poboljšati performanse aplikacije. Dodatne informacije potražite u članku [obrade upita](#batch-queries).
- **Baze podataka s intenzivno radno opterećenje koji ne može podržati cijelu jednom računalu**. Baza podataka koje premašuju resursi najvišu razinu performansi Premium možda pogodnost skaliranje out povećavaju. Dodatne informacije potražite u članku [sharding izdvojiti bazu podataka](#cross-database-sharding) i [Functional particija](#functional-partitioning).
- **Programi koji imaju ili lošije upita**. Aplikacije, osobito onima u sloj pristupa podacima koji loše počeo gledati upiti možda neće pogodnost na višu razinu performansi. To obuhvaća upite koje nemaju uvjet WHERE, indekse koji nedostaju ili su zastarjele Statistika. Prednosti ove aplikacije tehnike ugađanju performansi standardne upita. Dodatne informacije potražite u članku [nedostaje indekse](#missing-indexes) i [upit usklađivanje i dodatne](#query-tuning-and-hinting).
- **Programi koji imaju ili lošije podataka pristup dizajna**. Aplikacijama koje imaju ugrađeno pristup istodobnosti problemi s podacima, na primjer deadlocking, možda pogodnost na višu razinu performansi. Razmislite o smanjivanju round trips za bazu podataka SQL Azure predmemoriranja podataka na klijentskoj strani pomoću servisa Azure predmemoriranje ili neki drugi predmemoriranja technology. U odjeljku [aplikacije sloju predmemoriranje](#application-tier-caching).

## <a name="tuning-techniques"></a>Usklađivanje tehnike
U ovom ćete odjeljku smo pogledajte neke tehnike koje možete koristiti za Ugađanje baze podataka SQL Azure da biste ostvarili najbolje performanse aplikacije i pokrenuti na razini najniže moguće performanse. Neki od sljedećih načina odgovaraju tradicionalni SQL Server usklađivanje najbolje prakse, ali druge odnose se s bazom podataka SQL Azure. U nekim slučajevima možete provjeriti potrošenih resursa za bazu podataka da biste pronašli područja za daljnje ugađanje i proširivanje tradicionalni tehnika SQL Server za rad u bazi podataka SQL Azure.

### <a name="azure-portal-tools"></a>Alati za Azure portala
Možete pronaći dva alata na portalu za Azure kojih možete analizirati i riješili probleme s performansama SQL baze podataka:

- [Uvid performanse upita](sql-database-query-performance.md)
- [Savjetnik za baze podataka SQL](sql-database-advisor.md)

Portal za Azure ima dodatne informacije o od tih alata i kako ih koristiti. Da biste učinkovito dijagnosticiranje i rješavanje problema, preporučujemo da prvo pokušajte Alati na portalu za Azure. Preporučujemo da koristite ručno usklađivanje postupke koje ćemo govoriti, nedostaju indekse i ugađanje upita u određenim slučajevima.

### <a name="missing-indexes"></a>Nedostaju indeksa
Uobičajenih problema u performanse baze podataka OLTP odnosi se na fizičke dizajna baze podataka. Shema baze podataka često su namijenjene ili otpremljene bez testiranje na razini (ili u Učitaj u opsegu podataka). Nažalost, performanse plan upita može se prihvatljiva small skalu, ali slabije značajno u odjeljku količine podataka radnog razinom. Najčešći izvor ovog problema je nedostatak odgovarajuće indeksi zadovoljili filtre i drugim ograničenjima u upitu. Često nedostaju manifesti indeksi kao tablicu pregledajte kada je index traži nije suffice.

U ovom primjeru plan odabranog upita koristi pregleda kada želite suffice na rješenja:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Plan za upit s nedostaje indeksa](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Baze podataka SQL Azure omogućuju pronalaženje i otklanjanje uobičajenih nedostaje indeksirati uvjeta. DMVs koji su ugrađeni u baze podataka SQL Azure pogledajte kompilacije upita u kojem indeks će znatno smanjiti Procijenjena resursa da biste pokrenuli upit. Tijekom izvršavanja upita baze podataka SQL prati koliko često plan svaki upit se izvršava i prati Procijenjena razmaka između tarifu za izvođenje upita i onu u imagined gdje je to kazalo postojao. Možete koristiti te DMVs brzo pogoditi koje promjene dizajna fizičke baze podataka može poboljšati ukupni trošak radno opterećenje za baze podataka i njegov stvarnih radno opterećenje.

Koristite ovaj upit za procjenu potencijalne nedostaju indeksi:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

U ovom primjeru upit rezultirala ovaj prijedlog:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Nakon stvaranja, tu istu iskaza SELECT izdvajanja neku drugu tarifu, koji se koristi u traženje umjesto pregleda i učinkovitije izvršava plan:

![Plan za upit s ispravljenim indeksa](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Ključni uvid da je/i kapacitet zajedničke, obavješćivanje sustava ograničenom od pokrenite namjenski poslužitelj. Postoji u premium na minimiziranje nepotrebne/i da biste tako Maksimalno iskoristite prednosti sustava u DTU svaku razinu performanse razine servisa Azure SQL baze podataka. Odgovarajući fizička dizajna baze podataka mogućnosti možete znatno poboljšati Latencija za pojedinačne upite poboljšati propusnost istovremeni zahtjevi rukuje po jedinici skaliranje i minimiziranja troškove potrebne za ispunjavanje upit. Dodatne informacije o nedostaje indeks DMVs potražite u članku [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Usklađivanje upita i dodatne
Alat za upita u bazi podataka SQL Azure optimizaciju je slična na tradicionalni SQL Server upita alat za optimizaciju. Većina najbolje prakse za ugađanje upita i razumijevanje na zaključivanja modela ograničenja za alat za optimizaciju upit se primijeniti i na baze podataka SQL Azure. Precizno podesite upita u bazi podataka SQL Azure, možda ćete dobiti dodatnu pogodnost postupka smanjivanja zahtjeva zbrajanja resursa. Aplikacija možda moći pokrenuti na donjem trošak od untuned jednaku vrijednost jer se može pokrenuti niže razine performansi.

Primjer koji nije zajednička SQL Server, a koji se odnosi i na baze podataka SQL Azure je kako alat za optimizaciju upita "sniffs" parametara. Tijekom sastavljanja, alat za optimizaciju upita vrednuje trenutnu vrijednost parametra da biste utvrdili je li možete generirati plan više optimalnih upita. Iako ovaj strategije često može dovesti do plan upit koji je znatno brže nego plan prevedena bez vrijednosti parametara poznati, trenutno funkcionira neispravno i u sustavu SQL Server i u bazi podataka SQL Azure. Ponekad parametar nije sniffed, i ponekad je parametar sniffed, ali je generirana plan ili lošije za potpunog skupa vrijednosti parametara u radno opterećenje. Microsoft obuhvaća savjete upita (upute poslužitelja) tako da možete odrediti cilj više namjerno i nadjačati zadano ponašanje dijagnosticirati parametar. Često koristite savjete, možete ispraviti slučajevi u kojima je zadano ponašanje SQL Server ili baze podataka SQL Azure nesavršene za radno opterećenje određenog kupca.

U sljedećem primjeru pokazuje kako procesor upita možete stvoriti plan koji je ili lošije za performanse i uvjete resursa. U ovom se primjeru prikazuje i ako koristite podsjetnik za upit, možete smanjiti preduvjeti za izvođenje upita vrijeme i resurse za SQL baze podataka:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Postavljanje kod stvara tablicu koja sadrži korelacije distribucija podataka. Planiranje optimalnih upita razlikuje se ovisno o koje parametar odabran. Nažalost, tarifa predmemoriranje ponašanje ne uvijek prevoditi upita na temelju najčešću vrijednost parametra. Tako, moguće je plan ili lošije predmemorirani i koristi za mnoge vrijednosti, čak i kad je na drugu tarifu možda je bolji odabir plan u. Zatim plan upita stvara dva pohranjene procedure koji su jednaki, osim što se jedna sadrži podsjetnik za posebne upita.

**Primjerice, dio 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Primjer 2.dio**

(Ne preporučuje pričekajte najmanje 10 minuta prije nego što počnete 2.dio primjeru tako da rezultati se razlikuju u dobivene telemetrijskih podataka.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Svaki dio u ovom se primjeru nije pokuša pokrenuti naredbi s parametrima insert 1000 puta (za generiranje dovoljno učitavanja da biste koristili kao skup podataka za testiranje). Kada se izvršava pohranjene procedure, procesor upita ispituje vrijednost parametra koja je proslijeđena postupak tijekom njegova prvi sastavljanja (parametar "dijagnosticirati"). Procesor predmemorira rezultirajućem plan, a koji se koristi za kasnije instanci, čak i ako je vrijednost parametra različite. Optimalnih tarifa možda se neće koristiti u svim slučajevima. Ponekad ćete morati voditi alat za optimizaciju da biste odabrali plan koji je bolji za kutije average umjesto na određeni slučaj iz kada je upit najprije prevesti. U ovom primjeru početni plan generira "Pregled" plan koji se čita sve retke da biste pronašli sve vrijednosti koje odgovaraju parametar:

![Usklađivanje pomoću pregleda plan za upite](./media/sql-database-performance-guidance/query_tuning_1.png)

Jer smo izvršava postupak pomoću vrijednost 1, rezultirajući plan je optimalnih za vrijednost 1, ali je ili lošije za sve druge vrijednosti u tablici. Rezultat vjerojatno nije ono što želite želite ako ste slučajno, odaberite svaki plan jer plan izvodi sporije i koristi dodatne resurse.

Ako vam ponestane Testiraj uz `SET STATISTICS IO` postavljena na `ON`, u logičke skeniranje u ovom primjeru se radi u pozadini. Vidjet ćete da postoje 1,148 čitanja poduzeti tarifu (koja nije, ako je prosječna slučaj da biste vratili samo jedan redak):

![Usklađivanje pomoću logičke pregleda za upite](./media/sql-database-performance-guidance/query_tuning_2.png)

Drugi dio primjeru koristi podsjetnik za upit možete zaključiti alat za optimizaciju da biste koristili određenu vrijednost tijekom sastavljanja. U ovom slučaju nameće procesor upita da biste zanemarili vrijednost koja je proslijeđena kao parametar, a umjesto pretpostavlja da `UNKNOWN`. To se odnosi na vrijednost koja je prosječna učestalost u tablici (zanemarujući skew). Rezultat tarifa ispunjava plan utemeljen na rješenja koja se brže i koristi manje resursa u prosjeku, od plan 1.dio u ovom primjeru:

![Usklađivanje upita pomoću podsjetnik za upit](./media/sql-database-performance-guidance/query_tuning_3.png)

Vidjet ćete efekta u tablici **sys.resource_stats** (nema kašnjenje od vremena izvršiti provjeru i kada popunjava podatke u tablici). U ovom primjeru, 1.dio izvršeno tijekom vremena prozoru 22:25:00 i 2.dio izvršiti na 22:35:00. Imajte na umu da prozor starijim vrijeme koristi dodatne resurse u tom prozoru vrijeme od kasnije (zbog poboljšanja učinkovitosti plan).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Ogledni rezultati za ugađanje za upite](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Iako je glasnoće u ovom primjeru namjerno small efekt ili lošije parametara može biti znatno, osobito na veću baze podataka. Razlike u ekstremne slučajevima može biti između sekundi za slučaj da brzo i sata za slučaj da se sporo.

Možete provjeriti **sys.resource_stats** da biste odredili koristi li resurs za testiranje resursi više ili manje od drugog test. Kada usporediti podatke, odvojite vrijeme testira tako da se ne nalaze u istom prozoru 5 minuta u prikazu **sys.resource_stats** . Cilj na vježbu je da biste minimizirali ukupni iznos resursa koji se koriste, a ne da biste minimizirali Vršna resursi. Općenito govoreći, optimiziranje dio koda za kašnjenje smanjuje potrošnju resursa. Provjerite jesu li promjene koje izvršite u aplikaciju potrebno, a da promjene ne negativno utjecati na sučelje klijenta za osobe koje koriste savjete za upit u aplikaciji.

Ako je radno opterećenje skup ponavljajućih upita, često vam odgovara da biste zabilježili i provjeriti optimizacije izbore plan jer je pogoni jedinica veličina minimalne resursa potreban za smještaj bazu podataka. Nakon što joj potvrdite valjanost, povremeno preispitati planove za pomoć pri pripazite da oni imaju ne smanjena. Dodatne informacije o [Savjeti za izradu upita (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Sharding izdvojiti bazu podataka
Jer baze podataka SQL Azure pokreće obavješćivanje hardver, kapacitet ograničenja za baze podataka za jedinstveno su manja od za instalaciju tradicionalni lokalnog sustava SQL Server. Neki klijenti koristiti sharding postupke za šire postupaka baze podataka više baza podataka prilikom operacije ne stane unutar ograničenja jedne baze podataka u bazi podataka SQL Azure. Većina korisnici koji koriste sharding tehnike u bazi podataka SQL Azure Podjela svoje podatke na jedne dimenzije preko više baza podataka. Za taj se način, potrebno je razumjeti OLTP aplikacije često izvođenje transakcije koje se primjenjuju samo jedan redak ili maloj grupi redaka u shemi.

>[AZURE.NOTE] Baze podataka SQL sada sadrži biblioteku radi jednostavnijeg sharding. Dodatne informacije potražite u članku [Pregled biblioteke za klijent Elastic baze podataka](sql-database-elastic-database-client-library.md).

Na primjer, ako je baza podataka sadrži ime klijenta, narudžbe i Detalji narudžbe (kao što je tradicionalni primjer Northwind baze podataka koji se isporučuje sa sustavom SQL Server), te podatke može podijeliti u više baza podataka tako da grupirate klijenta s povezanim redoslijed i detaljne informacije o narudžbi. Možete jamči da ostaje na korisničkih podataka u bazi podataka jedan. Aplikacija će Podjela različite kupce preko baze podataka, učinkovito šire opterećenje preko više baza podataka. S sharding, korisnici ne samo izbjegavanje ograničenje veličine Maksimalna baze podataka, ali baze podataka SQL Azure također možete obraditi radnih opterećenja koje su znatno veće od ograničenja razine različite performanse, pod uvjetom da svaki pojedinačne baze podataka pristaju njegov DTU.

Iako sharding baze podataka ne smanjili kapacitet zbrajanja resursa za rješenje, je vrlo učinkovitih pri vrlo velike rješenja koje se šire više baza podataka za podršku. Svaki baze podataka mogu se izvoditi performanse različite razine za podršku vrlo velike te "učinkovitih" baze podataka s uvjete visoke resursa.

### <a name="functional-partitioning"></a>Funkcionalno particija
Korisnici sustava SQL Server često kombinirati mnoge funkcije u jednom bazi podataka. Ako, na primjer, ako aplikacije sadrži logiku upravljanja zalihama u trgovini, tu bazu podataka možda logiku zalihe, praćenje narudžbenice, pohranjene procedure i indeksiranih ili materialized prikaza koji upravljanje izvješćivanjem Kraj mjeseca. Ovu tehniku jednostavnije administriranje baze podataka za operacije kao sigurnosnu kopiju, ali je i potrebno je veličina hardver za rukovanje opterećenje Vršna sve funkcije programa.

Ako koristite arhitekturu skala Izlaz u bazi podataka SQL Azure, preporučuje se u Podjela različite funkcije aplikacije u različite baze podataka. Pomoću ove tehnike svaku aplikaciju mijenja veličinu zasebno. Kao što je aplikacija postaje busier (i povećava opterećenje baze podataka), administrator možete odabrati neovisan učinka za svaku funkciju u aplikaciji. Pri ograničenje s ovom arhitektura aplikacije mogu biti veći od jedne obavješćivanje stroj možete rukovati jer je opterećenje podjele na više računala.

### <a name="batch-queries"></a>Grupe upita
Za aplikacije koje pristup podacima pomoću visoku glasnoću, Česti ad hoc upite znatno količinu reakcija je utrošeno na mrežnu komunikaciju između razina aplikacije i razina baze podataka SQL Azure. Čak i kad su računala i baze podataka SQL Azure u centru za iste podatke, latenciju mreže između dva možda se povećava velik broj podataka access operacije. Da biste smanjili mreže round trips operacija podataka programa access, preporučujemo da koristite mogućnost na jedan od sljedećih načina obrade ad hoc upite ili prikupiti kao pohranjene procedure. Ako obrada ad hoc upite, možete poslati većeg broja upita kao jedna velika grupa u jednom putovanja s bazom podataka SQL Azure. Kompiliranje ad hoc upite u pohranjena procedura, nije moguće postići isti rezultat kao da ih obrade. Korištenje pohranjena procedura i daje prednost povećava vjerojatnost predmemoriranje tarife upita u bazi podataka SQL Azure da biste ponovno koristiti pohranjena procedura.

Neke su aplikacije pisanje ćete morati usko. Ponekad ukupni Učitaj/i u bazi podataka možete smanjiti tako da odlučuje kako zajedno skupna zapisivanja. Često, to je jednostavno kao korištenje eksplicitnih transakcije umjesto automatsko izvršavanje transakcije u pohranjene procedure i ad-hoc serije. Procjena različite tehnike koje možete koristiti, potražite u članku [Batching tehnike za aplikacije baze podataka SQL Azure](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Eksperimentirajte s vlastitim radno opterećenje da biste pronašli desnom model za Grupno slanje promjena. Provjerite da biste shvatili modela možda imaju malo drugačije transakcijskih dosljednosti jamstva. Pronalaženje desnom minimizira korištenja resursa radno opterećenje zahtijeva pronalaženje dobre kombinacije dosljednost i performanse gubitke.

### <a name="application-tier-caching"></a>Predmemoriranje razina aplikacije
Neka aplikacija za baze podataka imaju radnih opterećenja čitanje podebljano. Predmemoriranje slojeve možda smanjivanja opterećenja na bazu podataka i možda potencijalno smanjili razinu performanse potrebne za podršku baze podataka pomoću baze podataka SQL Azure. S [Azure Redis predmemorije](https://azure.microsoft.com/services/cache/), ako imate radno opterećenje čitanje podebljano, možete čitati podatke jednom (ili jednom svako računalo razina aplikacije, ovisno o tome kako je konfigurirano), i spremiti podatke izvan SQL baze podataka. To je način da biste smanjili učitavanja baze podataka (procesora i čitanje/i), ali nema efekta na transakcijskih dosljednost jer podaci koji se čitaju iz predmemorije možda biti usklađen s podacima u bazi podataka. Iako u mnogim aplikacijama neke razine nedosljednosti nije prihvatljiva, koji nije ispunjen za sve radnih opterećenja. Neki preduvjeti za aplikaciju treba razumjeti potpuno prije implementacije stvaranje strategije predmemoriranja razina aplikacije.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o razine servisa potražite u članku [Mogućnosti SQL baze podataka i performanse](sql-database-service-tiers.md)
- Dodatne informacije o grupe elastic baze podataka potražite u članku [što je skup programa Azure elastic baze podataka?](sql-database-elastic-pool.md)
- Informacije o performansama i grupe elastic baze podataka potražite u članku [kada treba uzeti u obzir u skup elastic baze podataka](sql-database-elastic-pool-guidance.md)
