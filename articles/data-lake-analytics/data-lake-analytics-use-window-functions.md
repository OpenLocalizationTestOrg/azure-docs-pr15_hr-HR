<properties 
   pageTitle="Korištenje funkcija prozora U SQL Azure podataka Lake Aanlytics poslovi | Azure" 
   description="Saznajte kako pomoću funkcije prozora U SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="using-u-sql-window-functions-for-azure-data-lake-analytics-jobs"></a>Korištenje funkcija prozora U SQL Azure podataka Lake analize posla  

Prozor funkcije uvedene u ISO/ANSI SQL standardne 2003. U SQL adopts podskup prozor funkcije kako je definirano parametrom ANSI SQL Standard.

Prozor funkcije koriste se za izračunavanje unutar skupa redaka pod nazivom *windows*. Windows se definiraju tako da postavite POKAZIVAČ uvjet. Prozor funkcije riješiti neki ključni scenariji iznimno učinkovit način.

Vodič za učenje koristi dva uzorka skupove podataka da bi vas voditi kroz neke uzorak scenarija gdje možete primijeniti funkcije prozor. Dodatne informacije potražite u članku [Referenca U SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

Funkcija prozor kategorizirane u: 

- [Izvješćivanje o pogreškama funkcija zbrajanja](#reporting-aggregation-functions), kao što je zbroj ili AVG
- [Rangiranje funkcija](#ranking-functions), kao što su DENSE_RANK, broj_retka, NTILE i POLOŽAJ
- [Analitički funkcija](#analytic-functions), kao što su kumulativne distribucije, percentili ili pristupa podataka iz prethodnog retka u isti skup bez upotrebe programa samostalno uključivanje rezultata

**Preduvjeti:**

- Proći kroz sljedeća dva vodiči za:

    - [Prvi koraci pri korištenju Azure Data Lake Tools za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
    - [Prvi koraci pri korištenju U SQL Azure podataka Lake analize posla](data-lake-analytics-u-sql-get-started.md).
- Stvaranje podataka Lake analitički računa prema odredbama u [Prvi koraci pri korištenju Azure Data Lake Tools za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Stvaranje projekta za Visual Studio U-SQL prema odredbama u [Prvi koraci pri korištenju U SQL Azure podataka Lake analize posla](data-lake-analytics-u-sql-get-started.md).

## <a name="sample-datasets"></a>Ogledna skupova podataka

Pomoću ovog praktičnog vodiča koristi dvaju skupova podataka:

- QueryLog 

    QueryLog predstavlja popis što osobe tražili u tražilica. Svaki upit zapisnika obuhvaća:
    
        - Query - What the user was searching for.
        - Latency - How fast the query came back to the user in milliseconds.
        - Vertical - What kind of content the user was interested in (Web links, Images, Videos).
    
    Kopirajte i zalijepite sljedeće scrip u projektu U SQL za izgradnje skup redaka QueryLog:
    
        @querylog = 
            SELECT * FROM ( VALUES
                ("Banana"  , 300, "Image" ),
                ("Cherry"  , 300, "Image" ),
                ("Durian"  , 500, "Image" ),
                ("Apple"   , 100, "Web"   ),
                ("Fig"     , 200, "Web"   ),
                ("Papaya"  , 200, "Web"   ),
                ("Avocado" , 300, "Web"   ),
                ("Cherry"  , 400, "Web"   ),
                ("Durian"  , 500, "Web"   ) )
            AS T(Query,Latency,Vertical);
    
    U praksi, najvjerojatnije pohrane podataka u podatkovnoj datoteci. Želite pristupiti tih podataka unutar tabulatorom razgraničen datoteke pomoću koda za sljedeće: 
    
        @querylog = 
        EXTRACT 
            Query    string, 
            Latency  int, 
            Vertical string
        FROM "/Samples/QueryLog.tsv"
        USING Extractors.Tsv();

- Zaposlenici

    Skup podataka zaposlenika obuhvaća sljedeća polja:
   
        - EmpID - Employee ID.
        - EmpName  Employee name.
        - DeptName - Department name. 
        - DeptID - Deparment ID.
        - Salary - Employee salary.

    Kopirajte i zalijepite sljedeću skriptu u projektu U SQL za construcint skup redaka Zaposlenici:

        @employees = 
            SELECT * FROM ( VALUES
                (1, "Noah",   "Engineering", 100, 10000),
                (2, "Sophia", "Engineering", 100, 20000),
                (3, "Liam",   "Engineering", 100, 30000),
                (4, "Emma",   "HR",          200, 10000),
                (5, "Jacob",  "HR",          200, 10000),
                (6, "Olivia", "HR",          200, 10000),
                (7, "Mason",  "Executive",   300, 50000),
                (8, "Ava",    "Marketing",   400, 15000),
                (9, "Ethan",  "Marketing",   400, 10000) )
            AS T(EmpID, EmpName, DeptName, DeptID, Salary);
    
    Sljedeća naredba prikazuje stvaranje na skup redaka prema izdvajanje iz podatkovne datoteke.
    
        @employees = 
        EXTRACT 
            EmpID    int, 
            EmpName  string, 
            DeptName string, 
            DeptID   int, 
            Salary   int
        FROM "/Samples/Employees.tsv"
        USING Extractors.Tsv();

Kada testiranje primjere u praktičnom vodiču, morate uključiti definicije skup redaka. U SQL potrebno je definirati rowsets koji se koriste. Nekoliko primjera potreban vam je samo jedan skup redaka.

Morate dodati i sljedeća naredba za slanje skup redaka rezultat u podatkovnu datoteku:

    OUTPUT @result TO "/wfresult.csv" 
        USING Outputters.Csv();
 
 Većina uzorcima koristite varijablu pod nazivom **@result** za rezultate.

## <a name="compare-window-functions-to-grouping"></a>Usporedba funkcija prozor za grupiranje

Prikaz u prozorima i grupiranje pojmovno povezani su i drugi. Ovo je svladati odnos.

### <a name="use-aggregation-and-grouping"></a>Korištenje zbrajanja i grupiranje

Sljedeći upit za izračun zbroja plaća za sve zaposlenike koristi prikupljanja:

    @result = 
        SELECT 
            SUM(Salary) AS TotalSalary
        FROM @employees;
    
>[AZURE.NOTE] Upute za testiranje i provjeru izlaz potražite u članku [Prvi koraci pri korištenju U SQL Azure podataka Lake analize posla](data-lake-analytics-u-sql-get-started.md).

Rezultat je jedan redak s jednim stupcem. 165000 $ je zbroj vrijednosti plaća iz cijelu tablicu. 

|TotalSalary
|-----------
|165000

>[AZURE.NOTE] Ako ste novi korisnik sustava windows funkcije, korisno je zapamtiti brojeva u na izlaza.  

Sljedeća naredba koristi uvjet GROUP BY da biste izračunali ukupni salery za svaki odjel:

    @result=
        SELECT DeptName, SUM(Salary) AS SalaryByDept
        FROM @employees
        GROUP BY DeptName;

Rezultati su:

|DeptName|SalaryByDept
|--------|------------
|Inženjering|60000
|LJUDSKIH RESURSA|30000
|Izvršni|50000
|Marketing|25000

Zbroj stupca SalaryByDept je $165000, koji odgovara iznos u zadnji skripte.
 
U oba slučaja broj tamo su manje od unos redaka izlaz redaka:
 
- Bez GROUP BY skupljanja u sažima sve retke u jedan redak. 
- Korištenjem GROUP BY postoje N izlaz redaka pri čemu je N broj različitih vrijednosti koje se pojavljuju u podacima, u ovom slučaju, dobit ćete 4 retka u izlaz.

###  <a name="use-a-window-function"></a>Korištenje funkcije prozora

Uvjet postavite POKAZIVAČ u sljedeći primjer je prazno. Ovim se definira "prozor" da biste obuhvatili sve retke. ZBROJ u ovom primjeru primjenjuje se na nad uvjetu koji je prethodi.

Nije moguće čitanje upita u obliku: "Zbroji plaća iznad prozora sve retke".

    @result=
        SELECT
            EmpName,
            SUM(Salary) OVER( ) AS SalaryAllDepts
        FROM @employees;

Za razliku od GROUP BY postoje kao mnoge izlaz retke kao unos retke: 

|EmpName|TotalAllDepts
|-------|--------------------
|Noah|165000
|Sophia|165000
|Liam|165000
|Ema|165000
|Jacob|165000
|Olivia|165000
|Mason|165000
|Ava|165000
|Ethan|165000


Vrijednost 165000 (ukupno sve plaće) nalazi se u svakom retku izlaz. Taj Ukupno dolazi iz "prozor" svi reci tako da obuhvaća sve plaće. 

U sljedećem primjeru pokazuje kako suzite "prozor" da biste dobili popis svih zaposlenika, odjela i ukupan plaća za odjel. PARTICIJA tako da se dodaje uvjet postavite POKAZIVAČ.

    @result=
    SELECT
        EmpName, DeptName,
        SUM(Salary) OVER( PARTITION BY DeptName ) AS SalaryByDept
    FROM @employees;

Rezultati su:

|EmpName|DeptName|SalaryByDep
|-------|--------|-------------------
|Noah|Inženjering|60000
|Sophia|Inženjering|60000
|Liam|Inženjering|60000
|Mason|Izvršni|50000
|Ema|LJUDSKIH RESURSA|30000
|Jacob|LJUDSKIH RESURSA|30000
|Olivia|LJUDSKIH RESURSA|30000
|Ava|Marketing|25000
|Ethan|Marketing|25000

Ponovno postoje isti broj redaka za unos kao izlaz redaka. No svaki redak ima Ukupna plaća odgovarajuće odjela.




## <a name="reporting-aggregation-functions"></a>Izvješćivanje o pogreškama funkcije zbrajanja

Prozor funkcije podržavaju i sljedeće zbrajanja:

- COUNT
- ZBROJ
- MIN
- MAX
- AVG
- STDEV
- VAR

Sintaksa:

    <AggregateFunction>( [DISTINCT] <expression>) [<OVER_clause>]

Napomena: 

- Prema zadanim postavkama, funkcija zbrajanja, osim broj, zanemaruju null vrijednosti.
- Kada se uz uvjet nad određeni su klauzulom funkcija zbrajanja, uvjet ORDER BY nije dopuštena u uvjetu postavite POKAZIVAČ.

### <a name="use-sum"></a>Korištenje SUM

Sljedeći primjer dodaje ukupnu plaća odjel za svaki unos redak:
 
    @result=
        SELECT 
            *,
            SUM(Salary) OVER( PARTITION BY DeptName ) AS TotalByDept
        FROM @employees;

Evo izlaz:

|EmpID|EmpName|DeptName|DeptID|Plaća|TotalByDept
|-----|-------|--------|------|------|-----------
|1|Noah|Inženjering|100|10000|60000
|2|Sophia|Inženjering|100|20000|60000
|3|Liam|Inženjering|100|30000|60000
|7|Mason|Izvršni|300|50000|50000
|4|Ema|LJUDSKIH RESURSA|200|10000|30000
|5|Jacob|LJUDSKIH RESURSA|200|10000|30000
|6|Olivia|LJUDSKIH RESURSA|200|10000|30000
|8|Ava|Marketing|400|15000|25000
|9|Ethan|Marketing|400|10000|25000

### <a name="use-count"></a>Koristite funkcije COUNT

Sljedeći primjer dodaje dodatna polja za svaki redak da biste prikazali ukupan broj zaposlenika u svakom odjelu.

    @result =
        SELECT *, 
            COUNT(*) OVER(PARTITION BY DeptName) AS CountByDept 
        FROM @employees;

Rezultat:

|EmpID|EmpName|DeptName|DeptID|Plaća|CountByDept
|-----|-------|--------|------|------|-----------
|1|Noah|Inženjering|100|10000|3
|2|Sophia|Inženjering|100|20000|3
|3|Liam|Inženjering|100|30000|3
|7|Mason|Izvršni|300|50000|1
|4|Ema|LJUDSKIH RESURSA|200|10000|3
|5|Jacob|LJUDSKIH RESURSA|200|10000|3
|6|Olivia|LJUDSKIH RESURSA|200|10000|3
|8|Ava|Marketing|400|15000|2
|9|Ethan|Marketing|400|10000|2


### <a name="use-min-and-max"></a>Korištenje MIN i MAX

Sljedeći primjer dodaje dodatna polja za svaki redak da biste prikazali najniže plaće svaki odjel:

    @result =
        SELECT 
            *,
            MIN(Salary) OVER( PARTITION BY DeptName ) AS MinSalary
        FROM @employees;

Rezultati:

|EmpID|EmpName|DeptName|DeptID|Plaća|MinSalary
|-----|-------|--------|------|-------------|----------------
|1|Noah|Inženjering|100|10000|10000
|2|Sophia|Inženjering|100|20000|10000
|3|Liam|Inženjering|100|30000|10000
|7|Mason|Izvršni|300|50000|50000
|4|Ema|LJUDSKIH RESURSA|200|10000|10000
|5|Jacob|LJUDSKIH RESURSA|200|10000|10000
|6|Olivia|LJUDSKIH RESURSA|200|10000|10000
|8|Ava|Marketing|400|15000|10000
|9|Ethan|Marketing|400|10000|10000

MIN zamijenite MAX, a zatim isprobajte sami.


## <a name="ranking-functions"></a>Rangiranje funkcije

Rangiranje vraćaju vrijednost ranga (dugi) za svaki redak u svakom particija onako kako su definirana na PARTICIJE tako i PUTEM rečenice. Redoslijed rang upravlja u ORDER BY u uvjetu postavite POKAZIVAČ.

Sljedeće podržani su rangiranje funkcije:

- RANK
- DENSE_RANK 
- NTILE
- BROJ_RETKA

**Sintaksa:**

    [ RANK() | DENSE_RANK() | ROW_NUMBER() | NTILE(<numgroups>) ]
        OVER (
            [PARTITION BY <identifier, > …[n]]
            [ORDER BY <identifier, > …[n] [ASC|DESC]] 
    ) AS <alias>

- Uvjet ORDER BY nije obavezno za rangiranje funkcije. Ako je naveden ORDER BY je određuje redoslijed rang. Ako ORDER BY nije naveden, a zatim U SQL dodjeljuje vrijednosti na temelju redoslijed čitanja zapis. Stoga je rezultat u koje nisu deterministic vrijednost broja redaka, rank ili dense rang u slučaju da su redoslijedom prema vremenu uvjet nije naveden.
- NTILE potreban izraz koji se procjenjuje kao pozitivni cijeli broj. Taj broj određuje broj grupa u kojima mora biti podijeljen svaki particije. Ovaj identifikator koristi se samo s NTILE rangiranje (opis funkcije). 

Dodatne informacije o uvjet nad potražite u članku [Referenca U SQL]().

Broj_retka, RANGIRANJE i DENSE_RANK dodjelu brojeva redaka u prozoru. Umjesto zasebno pokrivaju ih je više intuitivno da biste vidjeli kako odgovori na isti ulaz.

    @result =
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank, 
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank 
    FROM @querylog;
        
Imajte na umu rečenice nad jednake. Rezultat:

|Upit|Latencija: int|Okomito|RowNumber|RANK|DenseRank
|-----|-----------|--------|--------------|---------|--------------
|Banana|300|Slika|1|1|1
|Tamnocrveni|300|Slika|2|1|1
|Durian|500|Slika|3|3|2
|Apple|100|Web|1|1|1
|Fig|200|Web|2|2|2
|Pastelna|200|Web|3|2|2
|Fig|300|Web|4|4|3
|Tamnocrveni|400|Web|5|5|4
|Durian|500|Web|6|6|5

### <a name="rownumber"></a>BROJ_RETKA

U svakom prozoru (okomito, slika ili web-), povećava broj retka 1 poredane po Latencija.  

![Funkcija prozora U SQL broj_retka](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-row-number-result.png)

### <a name="rank"></a>RANK

Razlikuje od ROW_NUMBER(), RANK() uzima u obzir vrijednost Latencija koja je navedena u uvjetu ORDER BY prozora.

POLOŽAJ počinje (1,1,3) jer su prve dvije vrijednosti za kašnjenje jednake. Zatim na sljedeću vrijednost je 3 jer Latencija vrijednost se premješta 500. Tipku pokažite da se čak i ako se isti rang ponudit će duplicirane vrijednosti, broj RANGA će "preskočiti" na sljedeću vrijednost broj_retka. Vidjet ćete ovaj uzorak ponovite s niza (2,2,4) u okomitom Web.

![U SQL prozor funkcija RANK](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-rank-result.png)

### <a name="denserank"></a>DENSE_RANK
    
DENSE_RANK je jednako kao rangirati PARAMETAR osim ga ne "preskočiti" prema sljedećem broj_retka umjesto koji odlazi sljedeći broj u nizu. Obratite pozornost na to nizove (1,1,2) i (2,2,3) u uzorku.

![Funkcija prozora U SQL DENSE_RANK](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-dense-rank-result.png)

### <a name="remarks"></a>Napomene

- Ako ORDER BY nije naveden od rangiranje funkcija će se primijeniti na skup redaka bez sve redoslijed. Posljedica će biti u koje nisu deterministic ponašanje na rangiranje funkcija primjene
- Nema jamstva da redaka koji je vratio upit pomoću broj_retka će naručiti točno ovaj postupak ponovite za svaku izvođenja osim ako su ispunjeni sljedeći uvjeti.

    - Jedinstvenih vrijednosti u stupcu particioniranom.
    - Jedinstvenih vrijednosti stupaca ORDER BY.
    - Jedinstvena su kombinacija vrijednosti particija stupaca i stupaca za sortiranje.

### <a name="ntile"></a>NTILE

NTILE distribuira reci u uređeni particije na određeni broj grupe. Grupa numeriranja, počevši od jedan. 


U sljedećem primjeru dijeli skup redaka u svakom particija (okomito) 4 grupe obliku Latencija upita i vraća broj grupe za svaki redak. 

Slika okomite je 3 retka, stoga je 3 grupe. 

Okomita Web ima 6 redaka, dva retka dodatni su distributed grupama prva dva. Koji je zašto postoje 2 retka u grupe 1 i 2 grupe i samo 1 redak u grupirati 3 i grupa 4.  

    @result =
        SELECT 
            *,
            NTILE(4) OVER(PARTITION BY Vertical ORDER BY Latency) AS Quartile   
        FROM @querylog;
        
Rezultati:

|Upit|Latencija|Okomito|QUARTILE
|-----|-----------|--------|-------------
|Banana|300|Slika|1
|Tamnocrveni|300|Slika|2
|Durian|500|Slika|3
|Apple|100|Web|1
|Fig|200|Web|1
|Pastelna|200|Web|2
|Fig|300|Web|2
|Tamnocrveni|400|Web|3
|Durian|500|Web|4

NTILE vodi parametar ("numgroups"). Numgroups je pozitivni cijeli broj ili dugo nepromjenjivim izraz koji određuje broj grupa u kojima mora biti podijeljen svaki particije. 

- Ako je broj redaka u particije djeljiv sa numgroups grupe će imati jednak veličina. 
- Ako je broj redaka u particije djeljiv numgroups, to će uzrokovati grupe dva veličina koje se razlikuju jednog člana. Prije manje grupe u redoslijedu koji je naveden tako da postavite POKAZIVAČ uvjet isporučuju se velikim grupama. 

Ako, na primjer:

- 100 redaka podijeljen 4 grupe: [25, 25, 25, 25]
- 102 redaka devided 4 u grupe: [26, 26, 25, 25]


### <a name="top-n-records-per-partition-via-rank-denserank-or-rownumber"></a>N najgornjih zapisa po particija putem POLOŽAJ, DENSE_RANK ili broj_retka

Mnogi korisnici želite odaberite samo GORNJIH n redaka po grupi. Nije moguće s na tradicionalni GRUPIRAJ po. 

U sljedećem primjeru na početku odjeljka funkcije rang ste vidjeti. Ne prikazuje N najgornjih zapisi za svaku particije:

    @result =
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank,
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;

Rezultati:

|Upit|Latencija|Okomito|RANK|DenseRank|RowNumber
|-----|-----------|--------|---------|--------------|--------------
|Banana|300|Slika|1|1|1
|Tamnocrveni|300|Slika|1|1|2
|Durian|500|Slika|3|2|3
|Apple|100|Web|1|1|1
|Fig|200|Web|2|2|2
|Pastelna|200|Web|2|2|3
|Fig|300|Web|4|3|4
|Tamnocrveni|400|Web|5|4|5
|Durian|500|Web|6|5|6

### <a name="top-n-with-dense-rank"></a>GORNJI N sa DENSE POLOŽAJ

Sljedeći primjer vraća gornje 3 zapise iz svake grupe s bez praznina u nizu rank numeriranje redaka u svakom particija prikaz u prozorima.

    @result =
    SELECT 
        *,
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;
    
    @result = 
        SELECT *
        FROM @result
        WHERE DenseRank <= 3;

Rezultati:

|Upit|Latencija|Okomito|DenseRank
|-----|-----------|--------|--------------
|Banana|300|Slika|1
|Tamnocrveni|300|Slika|1
|Durian|500|Slika|2
|Apple|100|Web|1
|Fig|200|Web|2
|Pastelna|200|Web|2
|Fig|300|Web|3

### <a name="top-n-with-rank"></a>GORNJI N sa POLOŽAJ

    @result =
        SELECT 
            *,
            RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank
        FROM @querylog;
    
    @result = 
        SELECT *
        FROM @result
        WHERE Rank <= 3;

Rezultati:    

|Upit|Latencija|Okomito|RANK
|-----|-----------|--------|---------
|Banana|300|Slika|1
|Tamnocrveni|300|Slika|1
|Durian|500|Slika|3
|Apple|100|Web|1
|Fig|200|Web|2
|Pastelna|200|Web|2


### <a name="top-n-with-rownumber"></a>GORNJI N sa broj_retka

    @result =
        SELECT 
            *,
            ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber
        FROM @querylog;
    
    @result = 
        SELECT *
        FROM @result
        WHERE RowNumber <= 3;

Rezultati:   
    
|Upit|Latencija|Okomito|RowNumber
|-----|-----------|--------|--------------
|Banana|300|Slika|1
|Tamnocrveni|300|Slika|2
|Durian|500|Slika|3
|Apple|100|Web|1
|Fig|200|Web|2
|Pastelna|200|Web|3

### <a name="assign-globally-unique-row-number"></a>Dodjeljivanje broj globalno jedinstveni retka

Često je korisno da biste dodijelili globalno jedinstvenog broja za svaki redak. Ovo je lako (i učinkovitiji od korištenja programa reducer) s funkcijama rangiranja.

    @result =
        SELECT 
            *,
            ROW_NUMBER() OVER () AS RowNumber
        FROM @querylog;

<!-- ################################################### -->
## <a name="analytic-functions"></a>Analitički funkcije

Analitički funkcije koriste se za razumijevanje distribucija vrijednosti u sustavu windows. Najčešći scenarij za korištenje funkcije analitički je izračunavanje percentili.

**Podržani funkcije analitički prozora**

- CUME_DIST 
- PERCENT_RANK
- PERCENTILE_CONT
- PERCENTILE_DISC

### <a name="cumedist"></a>CUME_DIST  

CUME_DIST formula izračunava relativni položaj određenu vrijednost u grupi vrijednosti. Izračunava postotak upita koji imaju na Latencija manje od ili jednaka da biste trenutnu Latencija upita u istom okomitoj osi. Za redak R, uz pretpostavku da uzlaznim redoslijedom, CUME_DIST R je broj redaka s vrijednostima manja od ili jednaka vrijednosti R, podijeljen brojem redaka koji se izračunava u skupu rezultata particija ili upita. CUME_DIST vraća brojeva u rasponu od 0 < x < = 1.

**Sintaksa**

    CUME_DIST() 
        OVER (
            [PARTITION BY <identifier, > …[n]]
            ORDER BY <identifier, > …[n] [ASC|DESC] 
    ) AS <alias>

U sljedećem primjeru koristi funkciju CUME_DIST za izračunavanje percentil Latencija za svaki upit u okomitu. 

    @result=
        SELECT 
            *,
            CUME_DIST() OVER(PARTITION BY Vertical ORDER BY Latency) AS CumeDist
        FROM @querylog;

Rezultati:
    
|Upit|Latencija|Okomito|CumeDist
|-----|-----------|--------|---------------
|Durian|500|Slika|1
|Banana|300|Slika|0.666666666666667
|Tamnocrveni|300|Slika|0.666666666666667
|Durian|500|Web|1
|Tamnocrveni|400|Web|0.833333333333333
|Fig|300|Web|0.666666666666667
|Fig|200|Web|0,5
|Pastelna|200|Web|0,5
|Apple|100|Web|0.166666666666667

Postoje 6 reci u particije gdje se nalazi ključ particija "Web" (4th redak, a zatim dolje):

- Postoje reci 6 vrijednost jednaka ili manja od 500, tako da na CUME_DIST jednaka 6/6 = 1
- Postoje 5 reci s vrijednošću jednaka ili manja od 400, tako da na CUME_DIST jednaka 5/6 = 0,83
- Postoje 4 retka s vrijednošću jednaka ili manja od 300, tako da na CUME_DIST jednaka 4/6 = 0.66
- Postoje 3 retka s vrijednošću jednaka ili manja od 200, tako da na CUME_DIST jednak 3/6 = 0,5. Postoje dva retka s istom vrijednošću Latencija.
- Postoji li 1 redak s vrijednošću jednaka ili manja od 100, tako da na CUME_DIST jednak 1/6 = 0,16. 


**Korištenje bilješke:**

- Izjednačenih vrijednosti računaju se uvijek jednaku vrijednost kumulativne distribucije.
- NULL vrijednosti se smatraju najniže mogućih vrijednosti.
- Morate navesti uvjet ORDER BY da biste izračunali CUME_DIST.
- CUME_DIST sličnu funkciju PERCENT_RANK

Napomena: Uvjet ORDER BY nije dopušteno ako iskaza SELECT ne slijedi IZLAZ. Stoga uvjet ORDER BY u izjavi IZLAZ određuje redoslijed prikaza Rezultantni skup redaka.


### <a name="percentrank"></a>PERCENT_RANK

PERCENT_RANK izračunava relativni položaj retka unutar grupe redaka. PERCENT_RANK koristi se za procjenu relativni položaj vrijednost u skup redaka ili particije. Raspon vrijednosti koje su rezultat PERCENT_RANK je veći od 0 i manji od 1. Za razliku od CUME_DIST, PERCENT_RANK uvijek je 0 za prvi redak.
    
**Sintaksa**

    PERCENT_RANK() 
        OVER (
            [PARTITION BY <identifier, > …[n]]
            ORDER BY <identifier, > …[n] [ASC|DESC] 
        ) AS <alias>

**Bilješke**

- Prvi redak u bilo kojem skupu ima PERCENT_RANK 0.
- NULL vrijednosti se smatraju najniže mogućih vrijednosti.
- Morate navesti uvjet ORDER BY da biste izračunali PERCENT_RANK.
- CUME_DIST sličnu funkciju PERCENT_RANK 


U sljedećem primjeru koristi funkciju PERCENT_RANK za izračunavanje percentil Latencija za svaki upit u okomitu. 

Uvjet PARTICIJE tako da nije naveden na particije redaka u rezultatu postavio na okomitoj osi. Uvjet ORDER BY u uvjetu nad narudžbe redaka u svakom particije. 

Vrijednost koju vraća funkcija PERCENT_RANK predstavlja rang kašnjenje u upitima u okomitu kao postotak. 


    @result=
        SELECT 
            *,
            PERCENT_RANK() OVER(PARTITION BY Vertical ORDER BY Latency) AS PercentRank
        FROM @querylog;

Rezultati:

|Upit|Latencija: int|Okomito|PercentRank
|-----|-----------|--------|------------------
|Banana|300|Slika|0
|Tamnocrveni|300|Slika|0
|Durian|500|Slika|1
|Apple|100|Web|0
|Fig|200|Web|0,2
|Pastelna|200|Web|0,2
|Fig|300|Web|0,6
|Tamnocrveni|400|Web|0,8
|Durian|500|Web|1

### <a name="percentilecont--percentiledisc"></a>PERCENTILE_CONT & PERCENTILE_DISC

Ove dvije funkcije izračunava percentil na temelju neprekinuti ili pojedinačnog distribucije skupa vrijednosti u stupcu.

**Sintaksa**

    [PERCENTILE_CONT | PERCENTILE_DISC] ( numeric_literal ) 
        WITHIN GROUP ( ORDER BY <identifier> [ ASC | DESC ] )
        OVER ( [ PARTITION BY <identifier,>…[n] ] ) AS <alias>

**numeric_literal** - percentil za izračun. Vrijednost mora biti u rasponu između 0.0 i 1.0.

UNUTAR GRUPE (ORDER BY <identifier> [ASC | DESC]) - određuje popis numeričke vrijednosti da biste sortirali i izračunati percentil iznad. Dopušten je identifikator samo jedan stupac. Izraz moraju biti vrednovani numeričke vrste. Druge vrste podataka nije dopušteno. Uzlazno je zadani redoslijed sortiranja.

IZNAD ([PARTICIJA po < identifikator >... [n]]) - dijeli unos skup redaka u particije po tipku particije na koju je primijenjena percentile (funkcija). Dodatne informacije potražite u članku RANGIRANJE odjeljak ovog dokumenta.
Napomena: Sve vrijednosti null u skupu podataka zanemaruju se.

**PERCENTILE_CONT** izračunava percentila temelju neprekinuti raspodjela vrijednosti stupca. Rezultat je nulama i možda neće biti jednaki u bilo koji od određene vrijednosti u stupcu. 

**PERCENTILE_DISC** izračunava percentil na temelju samostalni raspodjela vrijednosti u stupcu. Rezultat je jednako određenu vrijednost u stupcu. Drugim riječima, PERCENTILE_DISC, u suprotnom da biste PERCENTILE_CONT, uvijek vraća stvarni (izvorne ulazni) vrijednost.

Možete pogledati i rad u primjeru niže koja pokušava pronaći medijan (percentil = 0.50) vrijednost za kašnjenje u svakom okomito

    @result = 
        SELECT 
            Vertical, 
            Query,
            PERCENTILE_CONT(0.5) 
                WITHIN GROUP (ORDER BY Latency)
                OVER ( PARTITION BY Vertical ) AS PercentileCont50,
            PERCENTILE_DISC(0.5) 
                WITHIN GROUP (ORDER BY Latency) 
                OVER ( PARTITION BY Vertical ) AS PercentileDisc50 
        
        FROM @querylog;

Rezultati:

|Upit|Latencija: int|Okomito|PercentileCont50|PercentilDisc50
|-----|-----------|--------|-------------------|----------------
|Banana|300|Slika|300|300
|Tamnocrveni|300|Slika|300|300
|Durian|500|Slika|300|300
|Apple|100|Web|250|200
|Fig|200|Web|250|200
|Pastelna|200|Web|250|200
|Fig|300|Web|250|200
|Tamnocrveni|400|Web|250|200
|Durian|500|Web|250|200


Za PERCENTILE_CONT jer se može biti nulama vrijednosti, medijan za web-mjesto je 250 čak i ako nema upita na web-mjestu okomiti imali latenciju od 250. 

PERCENTILE_DISC procijeni vrijednosti, pa je medijana za web-mjesto 200 – što je stvarna vrijednost pronađena u okvir ulazni reci.











## <a name="see-also"></a>Vidi također

- [Pregled analize podataka Lake za Microsoft Azure](data-lake-analytics-overview.md)
- [Početak rada s podacima Lake analize pomoću portala za Azure](data-lake-analytics-get-started-portal.md)
- [Početak rada s podacima Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Interaktivni vodiči za analize korištenja Azure podataka Lake](data-lake-analytics-use-interactive-tutorials.md)
- [Analiza zapisnika web-mjesta pomoću Lake analize podataka za Azure](data-lake-analytics-analyze-weblogs.md)
- [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md)
- [Upravljanje analizom Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-manage-use-portal.md)
- [Upravljanje Azure podataka Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
- [Praćenje i rješavanje problema s poslove Lake analize podataka za Azure pomoću portala za Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
