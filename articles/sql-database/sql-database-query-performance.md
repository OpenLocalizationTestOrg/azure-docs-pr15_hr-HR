<properties 
   pageTitle="Azure SQL baza podataka upita performanse uvida" 
   description="Praćenje performansi upita služi za identifikaciju većine upita procesora koristi za baze podataka SQL Azure." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL baza podataka upita performanse uvida


Upravljanje i ugađanje performansi relacijske baze podataka je zahtjevne zadatak koji zahtijeva značajan stručna znanja i vrijeme ulaganja. Upit performanse uvid omogućuje vam da manje vremena performanse baze podataka za otklanjanje poteškoća unosom sljedeće:

- Detaljnije uvid u svoje potrošnje baze podataka resursa (DTU). 
- Najčešći upiti prema broj procesora/trajanje/izvođenja koje možete potencijalno moguće je počeo gledati za bolje performanse.
- Mogućnost dubinski analizirati prema dolje detalje upita, pogledajte njegov tekst i povijest Upotreba resursa. 
- Napomene koje pokazuju radnji [Savjetnik za baze podataka SQL Azure](sql-database-advisor.md) ugađanju performansi  



## <a name="prerequisites"></a>Preduvjeti

- Uvid performansama upita dostupna samo s V12 za baze podataka SQL Azure.
- Upit performanse uvid potreban je [Spremište upita](https://msdn.microsoft.com/library/dn817826.aspx) aktivan na bazu podataka. Ako je spremište upita nije pokrenut, na portalu od vas traži da biste ga uključili.

 
## <a name="permissions"></a>Dozvole

Sljedeće dozvole [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-configure.md) su obvezni za korištenje uvid performanse upita: 

- **Čitač**, **vlasnika**, **suradnika**, **Suradničke baze podataka SQL**ili **SQL Server suradničke** dozvole su potrebne da biste prikazali gornju resursa dosta upita i grafikone. 
- Za prikaz tekst upita, potrebne su dozvole **vlasnika**, **suradnika**, **Suradničke baze podataka SQL**ili **SQL Server suradnika** .



## <a name="using-query-performance-insight"></a>Korištenje upita performanse uvida

Upit performanse uvid je jednostavno je za korištenje:

- Otvorite [portal za Azure](https://portal.azure.com/) i pronađite bazu podataka koju želite pregledati. 
  - S lijeve strane izbornika, u odjeljku podršci i otklanjanju poteškoća, odaberite "Upit performanse uvid".
- Na kartici prvi pregledajte popis Najčešći upiti troše resursa.
- Odaberite pojedinačne upit za prikaz detalja.
- Otvorite [Savjetnik za baze podataka SQL Azure](sql-database-advisor.md) i provjerite jesu li preporukama dostupni.
- Pomoću klizača ili zumiranje ikone da biste promijenili opaženih interval.

    ![Nadzorna ploča](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Nekoliko sati podataka mora se prikazuju spremište upita za SQL baze podataka možete unijeti uvida performanse upita. Ako baza podataka ima bez aktivnosti ili spremišta upita nije bio aktivan tijekom određenog vremenskog razdoblja, grafikona bit će prazan prilikom prikaza to vremensko razdoblje. Ako nije pokrenut, u bilo kojem trenutku mogu omogućiti spremišta upita.   



## <a name="review-top-cpu-consuming-queries"></a>Pregledajte gornji procesora koristi upita

[Portal](http://portal.azure.com) učinite sljedeće:

1. Dođite do baze podataka SQL, a zatim kliknite **sve postavke** > **podršku + otklanjanje poteškoća** > **uvid performanse upita**. 

    ![Uvid performanse upita][1]

    Najčešći upiti prikaz otvorit će se i navedene su Najčešći upiti dosta procesora.

1. Kliknite oko grafikona detalje.<br>U gornjem retku prikazuje cjelokupni % DTU za bazu podataka, dok trake prikaz procesora % troše odabranih upita za odabrani interval (na primjer, ako je odabran **proteklog tjedna** svaka traka predstavlja jedan dan).

    ![Najčešći upiti][2]

    Rešetka dna predstavlja Zbrojeno informacije vidljive upite.

  - ID upita – Jedinstveni identifikator upita u bazi podataka.
  - Procesor po upitu observable interval (ovisi o funkcija zbrajanja).
  - Trajanje po upita (ovisi o funkcija zbrajanja).
  - Ukupan broj izvršavanja za određeni upit.

    Potvrdite ili poništite pojedinačne upite za uključivanje ili isključivanje ih iz grafikona pomoću potvrdne okvire.

1. Ako zastarjele podatke, kliknite gumb **Osvježi** .
1. Možete koristiti pomicat će se i zumiranja gumbe da biste promijenili interval opažanje i istražiti krivina:  ![postavke](./media/sql-database-query-performance/zoom.png)
1. Po želji, ako želite da drugi prikaz, možete odabrati **prilagođene** kartice i postaviti:
  
  - Metrika (CPU, trajanje, zbroj izvršavanja)
  - Vremenski interval (posljednji 24 sata, prošli tjedan, prošli mjesec). 
  - Broj upita.
  - Funkcija zbrajanja.

    ![postavke](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Prikaz pojedinosti pojedinačne upita

Da biste prikazali pojedinosti upita:

1. Kliknite bilo koji upit u popis Najčešći upiti.

    ![pojedinosti](./media/sql-database-query-performance/details.png)

1. Prikaz detalja o otvorit će se i zbroj za potrošnju/trajanje/izvršavanja upita procesora podijeljene tijekom vremena.
1. Kliknite oko grafikona detalje.
  - Gornji grafikon prikazuje redak s cjelokupan % DTU baze podataka, a trake su procesora % troše odabranog upita.
  - Drugi grafikon prikazuje ukupno trajanje po odabranog upita.
  - Dno grafikon prikazuje ukupni broj izvršavanja po odabranog upita.
    
    ![Detalji o upitu][3]

1. Po želji, pomoću klizača, gumbi za zumiranje ili kliknite **Postavke** da biste prilagodili prikaz upita za podatke ili da biste odabrali drugu vremensko razdoblje.

## <a name="review-top-queries-per-duration"></a>Najčešći upiti Pregled po trajanje

Nedavne ažuriranjem uvid performanse upita, ne možemo uvedene dva nove metrike koji mogu vam olakšati prepoznavanje potencijalne grla: broj trajanje i izvršavanja.<br>

Dugoročnih upiti mogu sadržavati maksimalnog potencijal za zaključavanje resursi dulje, blokiranje drugim korisnicima i ograničavanje skalabilnost. Oni su i najbolje kandidata za optimizaciju.<br>

Da biste odredili dugo izvodi upita:

1. Otvorite karticu **Prilagođeno** u uvid performanse upita za odabrane baze podataka
1. Promjena metriku se **trajanje**
1. Odaberite broj upita i opažanje interval
1. Odaberite funkciju zbrajanja
  - **SUM** zbraja sve vrijeme izvršavanja upita tijekom cijele opažanje intervala.
  - **Max** pronalazi upiti koji vrijeme izvođenja je najveći cijeli opažanje interval.
  - **Avg** pronalazi Prosječno vrijeme izvođenja sva izvršavanja upita i prikaz prvih iz ove prosjeka. 

    ![Trajanje upita][4]

## <a name="review-top-queries-per-execution-count"></a>Najčešći upiti Pregled po zbroj izvršavanja

Velik broj izvršavanja možda neće biti utjecaja same baze podataka i korištenje resursa može biti manje, ali cjelokupan aplikacije, mogla bi se sporo.

U nekim slučajevima zbroj vrlo visoka izvršavanja mogu dovesti do povećati od mreže round trips. ROUND trips znatno utjecati na performanse. Su predmet latenciju mreže i Latencija do poslužitelja. 

Ako, na primjer, mnogo utemeljenih na podacima web-mjesta intenzivnog pristup bazi podataka za svaki zahtjev za korisnika. Dok je veza okupljanje pomaže, povećana mrežnog prometa i Učitaj na poslužitelju baze podataka za obradu mogu negativno utjecati na performanse.  Općenitih savjeta je da biste zadržali round trips apsolutne najmanje.

Da biste odredili često izvršenja upita ("chatty") upita:

1. Otvorite karticu **Prilagođeno** u uvid performanse upita za odabrane baze podataka
1. Promjena metriku se **zbroj izvršavanja**
1. Odaberite broj upita i opažanje interval

    ![zbroj izvršavanja upita][5]

## <a name="understanding-performance-tuning-annotations"></a>Objašnjenje primjedbe za ugađanje performansi 

Tijekom Istraživanje svoje radno opterećenje u uvid performanse upita, mogli biste primijetiti ikone s okomitu crtu pri vrhu grafikona.<br>

Te ikone su primjedbe; predstavljaju performanse utjecaja radnji [Savjetnik za baze podataka SQL Azure](sql-database-advisor.md). Tako da omogućuje opaske se osnovne informacije o akcija:

![opaske upita][6]

Ako želite dodatne ili primijenite Savjetnik za preporuke, kliknite ikonu. Otvorit će se pojedinosti o akcija. Ako je aktivan preporuka možete primijeniti ga izravno Odsutan pomoću naredbe.

![Detalji o upitu primjedbe][7]

### <a name="multiple-annotations"></a>Više primjedbe. ###

Nije moguće da zbog uvećanja, primjedbe blizu međusobno će se sažeti u jednu. To će predstavljati posebno ikona, da ga kliknete će Otvori novi plohu kojima popis grupirane primjedbe će se prikazati.
Correlating upite i akcije ugađanju performansi omogućuju da biste bolje razumjeli svoje radno opterećenje. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Optimiziranje konfiguracija spremišta upita za uvid performanse upita

Tijekom korištenje uvid performanse upita, možda ćete naići sljedeće poruke spremišta upita:

- "Upita spremište nije ispravno konfigurirano na tu bazu podataka. Kliknite ovdje da biste saznali više."
- "Upita spremište nije ispravno konfigurirano na tu bazu podataka. Kliknite ovdje da biste promijenili postavke." 

Te se poruke obično se spremište upita nije uspio prikupiti nove podatke. 

Prvog slova se događa kada je upit spremište je u stanju samo za čitanje i optimalnog postavljaju parametre. To možete popraviti povećanje veličine spremišta upita ili poništavanjem spremišta upita.

![gumb qds][8]

Drugom slučaju se događa kada spremišta upita isključeno ili parametri ne postavite optimalnog. <br>Možete promijeniti pravilnika zadržavanja i snimanje i omogućiti spremišta upita izvršavanjem naredbe ispod ili izravno s portala:

![gumb qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Preporučena pravilnika zadržavanja i snimanje

Postoje dvije vrste pravila zadržavanja:

- Veličina temelji – ako je postavljen na automatski je će Čišćenje podataka automatski kada se dosegne blizu maksimalnog ograničenja veličine.
- Vrijeme temelji – po zadanom smo će postavljena na 30 dana, što znači da, ako nema dovoljno prostora pokrenut će upit trgovine, će izbrisati informacije o upitu starije od 30 dana

Snimanje nije moguće postaviti pravila:

- **Sve** – pohranjuju svi upiti.
- **Automatsko** – zanemaruje rijetko i upiti s nevažne Kompiliranje i izvođenja trajanje. Pragovi za brojanje, Kompiliranje i izvođenja trajanje izvršavanja interno određuju. Ovo je zadana mogućnost.
- **Ništa** – spremište upita zaustavlja snimanje nove se upite Međutim runtime stat za upite koji se već snimljenu se i dalje prikupljene.
    
Preporučujemo da postavka sva pravila za čistog pravila do 30 dana i automatski:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Povećavanje veličine spremišta upita. To se može izvesti povezivanje s bazom podataka i izdavanja sljedeći upit:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Primjene te postavke će naposljetku provjerite spremišta upita prikupljanje nove se upite, no ako želite pričekati možete očistiti spremišta upita. 
> [AZURE.NOTE] Izvođenje sljedeći upit će izbrisati sve podatke za trenutni u spremištu upita. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Sažetak

Uvid performanse upita olakšava razumijevanje utjecaj svoje radno opterećenje upita i njegovom potrošnje resursa za bazu podataka. S tom značajkom će dodatne informacije o vrhu troše upita i lakše prepoznali one da biste riješili problem prije nego što postanu problem.




## <a name="next-steps"></a>Daljnji koraci

Dodatne preporuke o unaprjeđenje SQL baze podataka kliknite [preporuke](sql-database-advisor.md) plohu **Uvid performanse upita** .

![Advisor performansi](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

