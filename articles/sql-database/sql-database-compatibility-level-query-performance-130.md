<properties
    pageTitle="Razina kompatibilnosti, kako procijenite | Microsoft Azure"
    description="Alati za određivanje razini kompatibilnosti je najbolje za bazu podataka na baze podataka SQL Azure ili Microsoft SQL Server i koraka"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Poboljšane performanse upita s kompatibilnošću 130 razina u bazi podataka SQL Azure


Azure SQL baze podataka radi proziran stotine tisuće baze podataka na mnogo različitih kompatibilnosti razina čuvanje i jamstva kompatibilnosti sa starijim verzijama odgovarajuće verziju sustava Microsoft SQL Server za svojih klijenata!

Stoga ništa ne sprječava korisnike promijeniti bilo koje postojeće baze podataka najnovije razinu kompatibilnosti s benefiting iz značajke procesor upita i alat za optimizaciju novi upit. Podsjećamo povijesti poravnanje verzije SQL se zadane razine kompatibilnosti su sljedeći:

- 100: u SQL Server 2008 i Azure SQL baze podataka V11.
- 110: u SQL Server 2012 i Azure SQL baze podataka V11.
- 120: u SQL Server 2014 i Azure SQL baze podataka V12.
- 130: u SQL Server 2016 i Azure SQL baze podataka V12.


> [AZURE.IMPORTANT] Pokretanje u **mid lipnja 2016**u bazi podataka SQL Azure, Zadana razina kompatibilnosti bit će 130 umjesto 120 za **novostvoreni** baze podataka.
> 
> Baza podataka stvorena prije mid lipnja 2016 će *ne* može utjecati i će zadržati svoje trenutne razine kompatibilnosti (100, 110 ili 120). Baze podataka koje migrirati iz verzije baze podataka SQL Azure V11 za V12 neće imati njihove razinu kompatibilnosti promijeniti nešto od sljedećeg.


U ovom članku ćemo istražiti prednosti razinu kompatibilnosti 130 te upute za korištenje tih prednosti. Ne možemo adresa moguće efekti strani na performanse upita za postojeće SQL aplikacije.


## <a name="about-compatibility-level-130"></a>O razinu kompatibilnosti 130


Najprije, ako želite saznati trenutnu razinu kompatibilnosti baze podataka, izvršiti sljedeće Transact-SQL naredbe.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Prije nego što će se dogoditi tu promjenu na razinu 130 za **upravo** stvorili baze podataka, recimo pregledajte ove promjene je sve o nekoliko primjera vrlo upita i potražite u članku kako svatko može koristiti iz nje.

Obradu upita u relacijske baze podataka može biti vrlo složen i može dovesti do puno računalnih znanosti i matematičkih izraza prema standardu da biste razumjeli mogućnosti postojećih dizajna i ponašanja. U ovom dokumentu sadržaj namjerno pojednostavnjeno je da biste bili sigurni da svatko tko ima malo pozadinskih minimalne tehničke možete posljedice Promjena razine kompatibilnosti i odrediti kako može koristiti aplikacije.

Pogledajmo brzo pogledati što razinu kompatibilnosti 130 objedinjuje u tablicu.  Dodatne informacije možete pronaći razini [ALTER baze podataka kompatibilnost (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), ali ovo je kratak sažetak:

- Umetanje operacije naredbe Umetni odaberite može biti Usporedni ili može imati tarifu paralelno dok prije nego što je ovaj postupak jednom niti.
- Memorije optimiziran tablicu i tablice varijable upita sada možete imati paralelno tarife dok prije ovaj postupak je i jednom niti.
- Statistika memorija optimizirana tablice možete odmah uzorkovanja i se automatski ažurirati. U odjeljku [što je novo u modul baze podataka: U memoriji OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) više pojedinosti.
- V/s način obrade retka način mijenja se s spremište stupca indeksa
  - Sortira na tablicu s spremište stupac indeksa sada su u načinu rada za obradu.
  - Prikaz u prozorima zbrajanja sada raditi u načinu za obradu kao što su KAŠNJENJA VODI TSQL izvješća.
  - Upite na spremište stupac tablice s više različitih rečenice raditi u načinu rada za obradu.
  - Upiti koji se izvršavaju DOP = 1 ili s serijski tarifom i izvoditi u načinu rada za obradu.
- Zadnje, Cardinality procjeni poboljšanja zapravo uskoro razinom kompatibilnosti 120, ali za one sa sustavom niže razine kompatibilnosti (odnosno 100 ili 110), premještanje kompatibilnosti razinu 130 će također se srušiti te poboljšanja, a to su prednosti performanse upita aplikacija.


## <a name="practicing-compatibility-level-130"></a>Practicing razinu kompatibilnosti 130


Prvi ćemo dobiti neke tablica, indekse i izravnim podataka stvorena da biste vježbali neke od tih nove mogućnosti. Primjeri skriptnog jezika TSQL možete izvršiti u odjeljku SQL Server 2016 ili u odjeljku baze podataka SQL Azure. Međutim, prilikom stvaranja baze podataka Azure SQL, provjerite jeste li odabrali najmanje P2 baze podataka jer je potrebno barem nekoliko jezgri Dopusti usporedno izvršavanje zadataka i stoga prednosti ove značajke.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Sada Pogledajmo izgleda za neke od značajki za obradu upita uskoro razinom kompatibilnosti 130.


## <a name="parallel-insert"></a>Paralelni Umetanje


Izvršavanje naredbe TSQL izvršava postupak Umetanje pod razinom kompatibilnosti 120 i 130, koji odnosno izvršava operacija INSERT u jedan obliku niti model (120), a u Usporedni model (130).


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Tako da zahtijevate stvarni tarifu za upit, pogled na njegov grafički prikaz ili njezin sadržaj XML možete odrediti koje procjeni Cardinality je funkcija pri Reproduciraj. Pogled na na tarife po usporednu na slici 1, jasno Vidimo da izvođenja stupca iz trgovine Umetanje ide iz serial u 120 paralelno u 130. Primijetite da je promjena ikone iterator u 130 tarifu s dvije paralelne strelice ilustraciju činjenica te sada izvođenja iterator uistinu paralelnih. Ako imate veliki Umetanje operacije da biste dovršili, paralelnog izvođenja povezana s brojem core koji su vam na raspolaganju za bazu podataka, će imati bolje performanse; do 100 puta brže ovisno o vašoj!


*Slika 1: Umetanje operacija mijenja iz serijski paralelno kompatibilnost sa razine 130.*


![Slika 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>Način rada SERIJSKI grupe


Isto tako, premještanje razinu kompatibilnosti 130 tijekom obrade redaka podataka omogućuje način obrade. Najprije operacije način obrade dostupne su samo kad imate indeks za spremište stupca na mjestu. Drugo, seriji obično predstavlja ~ 900 retke, a i koristi kod logike optimiziran za multicore procesora, veću propusnost memorije i izravno upravlja komprimirani podaci trgovine stupca kad god je moguće. Ispunjavate sljedeće uvjete SQL Server 2016 može se odjednom, obraditi ~ 900 redaka umjesto 1 redak u vrijeme i kao consequence, ukupni indirektni trošak operacije sada dijeli cijelu seriju smanjivanje Ukupno trošak po retku. Zajedničke količinu operacije zapravo u kombinaciji s spremište spajanje stupca smanjuje Latencija uvrštene u postupak za način rada odaberite grupe. Možete pronaći dodatne informacije o trgovini stupac i obrada način pri [Columnstore indeksi vodič](https://msdn.microsoft.com/library/gg492088.aspx).


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Kao vidljive ispod, tako da opažanja upit tarife jedno uz drugo na slici 2, možemo potražite u članku način obrade promijenio razinom kompatibilnosti i kao consequence, prilikom izvršavanja upita u oba razinu kompatibilnosti potpuno, možemo vidjeti te u većini slučajeva obrada utrošiti u načinu retka (86%) u usporedbi s način obrade (14%), gdje nakon obrade 2 serije. Povećavanje skupu podataka, pogodnost će se povećati.


*Slika 2: Odabir postupak promjene od serijski način obrade razinom kompatibilnosti 130.*


![Slika 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Način obrade na sortiranje izvođenja


Slično navedenog, ali primijeniti na operacije sortiranja Prijelaz iz načina retka (razinu kompatibilnosti 120) na način obrade (razinu kompatibilnosti 130) poboljšavaju performanse operacije SORTIRANJA isti razloga.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Vidljivi--usporedno na slici 3, Vidimo da operacije sortiranja u načinu rada za redak predstavlja 81% troška, dok je način obrade samo predstavlja 19% troška (odnosno 81% i % 56 na sortiranje sam).


*Slika 3: Redanje mijenja se iz retka u način obrade razinom kompatibilnosti 130.*


![Slika 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Pogrešno, ta uzorka sadržavati samo tens od tisuću redaka koji se ništa ne prilikom pregledavanja podaci koji su dostupni u većini SQL Server te dana. Samo one protiv milijune redaka umjesto projekta, a to možete prevesti u nekoliko minuta izvođenja spared svakodnevno na čekanju prirode svoje radno opterećenje.


## <a name="cardinality-estimation-ce-improvements"></a>Poboljšanja cardinality procjeni (će)


Uvodi 2014 SQL Server, sve baze podataka radi kompatibilnosti razini 120 ili iznad će koristio novi Cardinality procjeni funkcije. Zapravo, cardinality procjeni je logike upotrijebljena za određivanje kako SQL server će izvršavanje upita na temelju njegove Procijenjena trošak. Na procjeni izračunava se korištenjem unos od statističkih podataka povezan s objektima uvrštene u upit. Gotovo, s više razine, Cardinality procjeni funkcije su procjenjuje broj retka uz podatke o raspodjela vrijednosti, broji distinct vrijednosti i duplicirane broji koje se nalaze u tablice i objekte koji su referencirani u upitu. Početak te procjenjuje pogrešan, može uzrokovati nepotrebne disk/i zbog nedovoljno memorije daje (odnosno TempDB prelijeva) ili odabira serijski plan izvođenja putem paralelnog plan izvršavanje, nekoliko. Zaključak, netočan procjenjuje može dovesti do cjelokupnoj izvedbi smanjene performanse izvršavanja upita. Na drugoj strani bolje procjenjuje, točnije procjenjuje potencijalnih klijenata za bolje izvršavanja upita!

Kao što je rečeno prije optimizacije upita i procjenjuje su složene pitanje, ali ako želite da biste saznali više o tarifama upita i cardinality estimator, možete se referirati na dokument u [Optimiziranje vaše upita tarife s Estimator Cardinality za SQL Server 2014](https://msdn.microsoft.com/library/dn673537.aspx) za dublju dive.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Koje Cardinality procjeni trenutno koristite?


Da biste odredili u odjeljku koji su pokrenuti upitima procjeni Cardinality, recimo samo pomoću ispod uzoraka upita. Imajte na umu da je ovo prvi primjer će pokrenuti u odjeljku razinu kompatibilnosti 110, implying koristi stari Cardinality procjeni funkcije.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Nakon dovršetka izvođenja, kliknite na vezu za XML i pogledajte svojstva prvi iterator kao što je prikazano u nastavku. Imajte na umu naziv svojstva pod nazivom CardinalityEstimationModelVersion koje su trenutno postavljene na 70. To znači da je razina kompatibilnosti baze podataka postavljena na verziju sustava SQL Server 7.0 (postavlja se na 110 kao vidljive u izvješćima TSQL gore), ali vrijednost 70 jednostavno predstavlja naslijeđene Cardinality procjeni funkcije dostupne nakon SQL Server 7.0 koji imali bez glavne revizije do 2014 poslužitelja SQL (koja se isporučuje s kompatibilnosti stupanj 120).


*Slika 4: U CardinalityEstimationModelVersion postavljen je na 70 prilikom korištenja razinu kompatibilnosti 110 ili ispod.*


![Slika 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


Umjesto toga, možete promijeniti razinu kompatibilnosti za 130 i onemogućite korištenje funkcije novi Cardinality procjeni pomoću LEGACY_CARDINALITY_ESTIMATION postavite na Uključeno konfiguraciji [ALTER baze podataka iz DJELOKRUGA](https://msdn.microsoft.com/library/mt629158.aspx). To će biti potpuno isto kao i upotreba 110 iz Cardinality procjeni funkcije točke vrsta prikaza, tijekom korištenja najnovije razinu kompatibilnosti za obradu upita. To učinite, možete im nove značajke uskoro s najnovijim razinu kompatibilnosti (odnosno način obrade) za obradu upita, ali i dalje ovise o stare Cardinality procjeni funkcije prema potrebi.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Jednostavno premještanje razinu kompatibilnosti 120 ili 130 omogućuje nove Cardinality procjeni funkcije. U tom slučaju zadane CardinalityEstimationModelVersion će biti sukladno tome postaviti 120 ili 130 kao vidljive ispod.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Slika 5: U CardinalityEstimationModelVersion postavljen je na 130 prilikom korištenja razine kompatibilnosti 130.*


![Slika 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Witnessing razlike procjeni Cardinality


Sada ćemo pokrenite malo više kompleksnih upita obuhvaćaju INNER JOIN uz uvjet WHERE s nekim predikati i Pogledajmo procijenjenu vrijednost za broj redaka iz stare Cardinality procjeni funkcije prvi put.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Učinkovito izvršavanja ovaj upit vraća 200,704 retke dok Procjena retka funkcionalnošću stare Cardinality procjeni zahtjeve 194,284 redaka. Pogrešno, kao što je rečeno prije, te rezultate brojanje redaka će ovisiti i koliko često ste pokrenuli prethodnog primjera koja popunjava primjere tablica uvijek iznova pri svakom Pokreni. Pogrešno, predikati u upitu dodijelit će se utjecaja na stvarni procjeni osim oblika tablice, podataka sadržaja, a kako se ti podaci zapravo povezivanje međusobno.


*Slika 6: Procijenjenu vrijednost za broj redaka isključeno 194,284 ili 6,000 redaka iz 200,704 redaka očekivanjima.*


![Slika 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Na isti način, recimo sada izvršiti istog upita s novim funkcijama procjeni Cardinality.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Pogled na na u nastavku sada Vidimo da procijenjenu vrijednost retka je 202,877, i mnogo bliže i veće od stare procjeni Cardinality.

*Slika 7: Procijenjenu vrijednost za broj redaka sada je 202,877 umjesto 194,284.*


![Slika 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


U stvari, skup rezultata je 200,704 redaka (ali sve to ovisi o učestalosti nije pokrenut upita prethodnog primjera, ali više važno je napomenuti da, budući da u TSQL koristi naredbu RAND(), stvarnih vrijednosti vraća može se razlikovati iz jednog pokretanja na sljedeću). Dakle, u ovom primjeru određeni novi procjeni Cardinality ne bolje posla pri Procjena broj redaka jer je mnogo bliže 200,704, od 194,284 202,877! Na kraju, ako promijenite uvjet WHERE predikata da biste jednakosti (umjesto ">" na primjer), to može učiniti procjenjuje između stari i nova Cardinality funkcija još različite, ovisno o tome koliko odgovara koje možete dobiti.

Pogrešno, u ovom slučaju, koji se reci ~ 6000 isključivanje iz stvarni broj predstavlja velike količine podataka u nekim slučajevima. Sada su transpose za milijune redaka preko nekoliko tablica i složenije upita te ponekad procijenjenu vrijednost može biti isključivanje milijune redaka i zbog toga rizik od izdvajanje kopirane pogrešan izvođenja plana ili traži nedovoljno memorije daje oni mogu dovesti do TempDB prelijeva i tako da više/i mnogo veći.

Ako imate priliku vježbe ove usporedbe s Najčešći upiti i skupova podataka i potražite u članku za sebe po koliki dio procjenjuje stara i nova utječe, dok neke može samo postati više isključivanje iz u stvarnosti ili neki drugi samo jednostavno bliže stvarni redak koji broji zapravo vraćeni u skupovima rezultata. Sve to ovise oblika upite, osobina baze podataka Azure SQL, prirode i veličinu svoje skupove podataka te statistike dostupne o njima. Ako svoje baze podataka SQL Azure instancu koju ste upravo stvorili, alat za optimizaciju upit neće imati da biste sastavili njegova znanja ispočetka umjesto ponovnog korištenja Statistika koji se sastoji od prethodnog upit se pokreće. Tako, procjene su vrlo kontekstne i gotovo specifične za svaki situacija poslužitelj i aplikacije. To je važno razmjer treba imati na umu!


## <a name="some-considerations-to-take-into-account"></a>Što valja uzeti u račun


Iako se većina radnih opterećenja bi im razinu kompatibilnosti 130, prije usvojio razinu kompatibilnosti u okruženju sustava radnog zapravo su 3 mogućnosti:

1. Premještanje na razinu kompatibilnosti 130 i potražite u članku kako izvršiti stvari. U slučaju da Primijetit ćete neke nedostatke ste upravo jednostavno kompatibilnost razine ponovno postaviti njegovu izvornu razinu ili Zadrži 130 i samo obrnutim procjeni Cardinality natrag na naslijeđene način (kao što je opisano iznad, to samostalno može adresa problem).
2. Temeljito testirajte svoje postojeće aplikacije opterećenju slične radnog, precizno podešavanje i provjeru uspješnosti prije prebacivanja u radni. U slučaju problema, isti kao gore, možete uvijek se vratili na izvornu razinu kompatibilnosti ili jednostavno obrnuti procjeni Cardinality natrag za naslijeđene način rada.
3. Kao konačan mogućnost, a najnovije način za rješavanje ta pitanja, je odražava spremišta upita. To je preporučena mogućnost današnjeg! Kao pomoć analizu upitima u odjeljku kompatibilnosti razine 120 ili ispod nasuprot 130, ne preporučujemo dovoljno da koristi spremište upita. Spremište upita dostupna najnoviju verziju preglednika V12 za baze podataka SQL Azure i vam omogućuje da biste lakše s otklanjanjem poteškoća performanse upita. Smatrati spremišta upita snimač leta podataka baze podataka prikupljanje podataka i izlaganje povijesnim informacija o sve upite. To za komercijalni ispis olakšava performanse forensics smanjivanjem vremena za dijagnosticiranje i rješavanje problema. Možete pronaći dodatne informacije na [upit iz trgovine: snimač leta podataka baze podataka](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


At s više razine, ako već imate skup baze podataka radi kompatibilnosti razini 120 ili ispod, i planiranje da biste premjestili neki od njih da biste 130 ili jer svoje radno opterećenje automatski Dodjela nove baze podataka koji će biti uskoro biti postavljena po zadanom da 130, razmislite o na followings:

- Prije promjene novu razinu kompatibilnosti u proizvodnje, omogućite spremišta upita. Dodatne informacije možete se referirati da biste [promijenili način kompatibilnosti baze podataka i koristi spremište upita](https://msdn.microsoft.com/library/bb895281.aspx) .
- Nakon toga testirajte sve ključne radnih opterećenja pomoću predstavniku podataka i upita radnom okruženju i Usporedba performanse iskustvom i kao prijavljenim tako da upit trgovine. Ako imate neke nedostatke, možete prepoznati regressed upiti s trgovinom u upit i koristiti plan prisilno mogućnost iz spremišta upita (ili planiranje prikvačivanje). U tom slučaju možete definitively zadržati razinu kompatibilnosti 130 i koristiti plan bivši upita kao što je predloženo u spremištu upita.
- Ako želite omogućiti korištenje nove značajke i mogućnosti baze podataka SQL Azure (koji se izvodi SQL Server 2016), ali su osjetljive promjene ne unese po razini kompatibilnosti 130, ne uspije, nije moguće razmislite o prisilno razinu kompatibilnosti natrag na razinu koja odgovara vašem radno opterećenje pomoću na iskaz ALTER baze podataka. No, imajte na umu plan spremišta upita prikvačivanje mogućnost da je najbolja mogućnost jer ne koristite 130 zapravo zadržavanje na razini funkcionalnost stariju verziju sustava SQL Server.
- Ako imate složene aplikacije koje se protežu na više baza podataka, možda će biti potrebno da biste ažurirali dodjele resursa logičku vrijednost svojih baza podataka da biste bili sigurni razinu dosljedan kompatibilnosti preko sve baze podataka; one stare i nove Dodjela resursa. Radno opterećenje performanse računala može biti osjetljivi na fact pokrenute nekih baza podataka na drugi kompatibilnosti razinama i zbog toga kompatibilnosti razine dosljednost preko bilo koje baze podataka može biti potrebne da bi se omogućuje rad s istom klijentima sve ploču. Imajte na umu da se ne nalazi na prema, zaista ovisi kako aplikacija utječe razinu kompatibilnosti.
- Zadnje, vezane uz procjeni Cardinality i baš kao i promijenite razinu kompatibilnosti, prije nastavka u proizvodnje, preporučuje se da biste testirali radnog radno opterećenje novi uvjetima da biste odredili koristio ako program poboljšanja procjeni Cardinality.


## <a name="conclusion"></a>Zaključak


Korištenje baze podataka SQL Azure prednosti iz svih SQL Server 2016 poboljšanja jasno možete poboljšati vaše izvršavanja upita. Kao što-je! Naravno, kao što je nova značajka proper procjenu mora poduzeti da biste odredili točan uvjeta pod kojim svoje radno opterećenje baze podataka radi najbolje. Experience pokazuje da većina radno opterećenje se očekuje najmanje proziran pod razinom kompatibilnosti 130, tijekom obrade funkcije i novi Cardinality procjeni novog upita za korištenje. Je rečeno, realistically, uvijek postoje neke iznimke i način proper krajnji diligence važne procjenu da biste odredili koliko koje su prednosti te poboljšanja. I ponovno može biti spremišta upita sjajno pomoći u učinite to funkcioniralo!

Kao što je SQL Azure razvoja, razinu kompatibilnosti 140 možete očekivati u budućnosti. Kada je vrijeme odgovarajuće, ne možemo počet će razgovaraju što ta razina budućih kompatibilnosti 140 će se srušiti, kao što smo ukratko spominju ovdje odaberete razinu kompatibilnosti 130 je prebacivanja danas.

Zasad, recimo ne zaboravite, počevši lipnja 2016 baze podataka SQL Azure pretvorit će se Zadana razina kompatibilnosti iz 120 da biste 130 za novostvorenu baze podataka. Imajte na umu!


## <a name="references"></a>Reference


- [Što je novo u modul baze podataka](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blog: Upita trgovine: snimač leta podataka baze podataka, tako da Borko Novakovic, 8 2016 lipnja](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [Razinu kompatibilnosti ALTER baze podataka (transakcija-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [IZMJENA OPSEGU KONFIGURACIJU BAZE PODATAKA](https://msdn.microsoft.com/library/mt629158.aspx)

- [Razinu kompatibilnosti 130 V12 baze podataka Azure SQL](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Optimiziranje upit tarife sa sustavom SQL Server Estimator Cardinality 2014.](https://msdn.microsoft.com/library/dn673537.aspx)

- [Vodič za Columnstore indeksa](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blog: Poboljšane performanse upita s kompatibilnosti razinu 130 u bazi podataka Azure SQL, po Alain Lissoir možda 6 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
