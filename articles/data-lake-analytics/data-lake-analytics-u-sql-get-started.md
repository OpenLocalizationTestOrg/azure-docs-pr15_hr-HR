<properties
   pageTitle="Razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio | Azure"
   description="Saznajte kako instalirati Data Lake Tools za Visual Studio, razvoju i testiranje U SQL skripte. "
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

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Praktični vodič: Početak rada s jezikom Azure podataka Lake analize U-SQL

U SQL je na jeziku koji se ujednačuje prednosti SQL s izražajna power vlastitog koda za obradu sve podatke na bilo kojem mjerilo. Mogućnost skalabilni raspodijeljeno upita U-SQL-omogućuje učinkovito analize podataka u spremištu i preko relacijski sprema kao što su baze podataka SQL Azure.  Omogućuje u tijeku nestrukturirane podatke primjenom sheme na čitanje, Umetanje prilagođene logiku i UDF- i obuhvaća mogućnost proširenja da biste omogućili precizno grained kontrolu nad kako izvoditi na razini. Da biste saznali više o filozofije dizajna iza U SQL, pogledajte ovaj [Visual Studio bloga](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Postoje neke razlike iz ANSI SQL ili T-SQL-a. Ako, na primjer, njegov ključne riječi kao što su odabir mora biti u velika slova.

To je vrsta sustava i izraz jezik unutar odaberite uvjete, gdje se u C# nalaze predikati itd.
To znači da vrste podataka nekoliko vrsta C# i vrste podataka pomoću C# NULL semantiku i operacije usporedbe unutar predikata slijedite C# sintaksa (npr., na == "odnožje").  To također znači, vrijednosti su je cijeli .NET objekte, što omogućuje jednostavno koristiti bilo koji način da biste upravljali objekta (npr "f o o o". Split(' ')).

Dodatne informacije potražite u članku [Referenca U SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Preduvjeti

Morate dovršiti [Praktični vodič: razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

U ovom praktičnom vodiču pomoću sljedeće skripte U SQL pokrenuli analize podataka Lake posao:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Ova skripta ne obuhvaća sve korake transformaciju. Ga čita iz izvorišne datoteke naziva **SearchLog.tsv**schematizes ga, a proizvodi u skup redaka u datoteku s nazivom **SearchLog – prvi-u – sql.csv**.

Obratite pozornost na upitnik pokraj vrste podataka polja za trajanje. To znači da trajanje polje može biti null.

Neki koncepti i ključne riječi u skripti:

- **Varijable skup redaka**: svaki upit izraz koja daje skup redaka koje se mogu dodijeliti tjednog prikaza kalendara. U SQL slijedi T SQL Varijable imenovanja uzorak, primjerice, **@searchlog** u skripti.
    Imajte na umu dodjeljivanjem prisilno izvršavanja. Samo nazive izraza te vam nudi mogućnost izgradnje složenije izraza.
- **Izdvajanje** vam omogućuje da biste definirali shemu čitanje. Shema navedena je naziv stupca i C# upišite naziv par po stupcu. Koristi se s so-called **program za izdvajanje**, na primjer, **Extractors.Tsv()** izdvojiti tsv datoteke. Razviti prilagođene izdvajanje.
- **IZLAZ** uzima skup redaka i serializes ga. Na Outputters.Csv() izlaz datoteke razdvojene zarezom u navedeno mjesto. Možete razviti i prilagođena Outputters.
- Obratite pozornost na to su dvije putovi relativni putovi. Možete koristiti i apsolutni putovi.  Na primjer

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Apsolutni put morate koristiti da biste pristupili datotekama u povezani računi za pohranu.  Vidjet ćete da sintaksa za datoteke spremljene u povezani poslovni subjekt za pohranu Azure je:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob kontejner s javnim blob-ova ili dozvole za pristup javnim spremnika trenutno nisu podržani.

## <a name="use-scalar-variables"></a>Skalarna varijable koristiti

Skalarna varijable možete koristiti kao i pojednostavniti održavanje web skripte. Prethodna skripta U SQL mogu biti napisani i kao sljedeće:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Pretvaranje rowsets

**Odaberite** koristite za pretvorbu rowsets:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Uvjet WHERE koristi [C# Booleov izraz](https://msdn.microsoft.com/library/6a71f45d.aspx). Izraz jezika C# možete koristiti da biste učinili vlastite izrazima i funkcijama. Čak i možete izvesti složeniju filtriranje kombiniranjem s logičke veznika (ANDs) i disjunctions (ORs).

Sljedeću skriptu koristi metodu DateTime.Parse() i veznika.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Obratite pozornost na to da drugi upit radi prikaza rezultata prvi skup redaka i stoga rezultat je sastavljanje tih dvaju filtara. Možete koristiti i naziv varijable i nazivi imaju ograničen prikaz samo lexically.

## <a name="aggregate-rowsets"></a>Zbrajanja rowsets

U SQL omogućuje s poznatih **ORDER BY**, **GRUPIRAJ po** i agregacija.

Sljedeći upit pronalazi ukupno trajanje po regijama, a zatim proizvodi vrha 5 trajanja redoslijedom.

U SQL rowsets sačuvati njihov redoslijed za sljedeći upit. Dakle, za narudžbu u izlaz, morate dodati ORDER BY izjavu IZLAZ kao što je prikazano u nastavku:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U SQL ORDER BY uvjetu mora biti u kombinaciji s DOHVAĆANJE uvjeta u izrazu SELECT.

Da biste ograničili izlaz grupe koji ispunjavaju uvjet HAVING mogu se POJAVLJUJU U SQL uvjet WHERE:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Spajanje podataka

U SQL sadrži uobičajene operatore spoj kao što su UNUTARNJI SPOJ, lijevo/desno/CIJELOG VANJSKI SPOJ, a zatim UKLJUČITE OČEKIVAN da biste se pridružili ne samo tablice, ali sve rowsets (čak i one proizvodi iz datoteka).

Sljedeću skriptu spajanja searchlog s zapisnik dojam oglašavanje i daje nam oglasa za nizu upita za navedeni datum.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U SQL podržava samo sintaksa ANSI usklađen spoja: predikata Rowset1 SPOJ Rowset2 Uključeno. Sintaksa stare iz Rowset1, gdje Rowset2 predikata nije podržana.
Predikata u SPOJU mora biti na spoj jednakosti i bez izraza. Ako želite koristiti izraz ga dodati u uvjetu select prethodni skup redaka korisnika. Ako želite učiniti drugačiju usporedbu, možete je premjestiti u uvjetu WHERE.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Stvaranje baze podataka, funkcije s tabličnim vrijednostima, prikazima i tablice

U SQL ne omogućuje korištenje podataka u kontekstu baze podataka i shemu. Tako da ne morate uvijek čitanje ili pisanje datoteke.

Svaki U SQL skripta se izvodi s zadanu bazu podataka (matrica) i zadane shema (vlasnika baze podataka) kao njegovog zadani konteksta. Možete stvoriti vlastitu bazu podataka i/ili shemu. Da biste promijenili kontekst, koristite naredbu **KORISTITE** da biste promijenili kontekst.


### <a name="create-a-table-valued-function-tvf"></a>Stvaranje funkcije s tabličnim vrijednostima (TVF)

U prethodna skripta U SQL ponavlja pomoću IZDVOJITI iz iste izvorne datoteke za čitanje. Funkcije s tabličnim vrijednostima U SQL omogućuje Enkapsulacija podataka za buduću upotrebu.   

Sljedeću skriptu stvara TVF naziva *Searchlog()* u zadanu bazu podataka i sheme:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Sljedeću skriptu pokazuje kako koristiti TVF definirano u prethodna skripta:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Stvaranje prikaza

Ako imate samo jedan izrazu upita koji želite apstraktnog i želite ga parameterize, možete stvoriti prikaz umjesto funkcije s tabličnim vrijednostima.

Sljedeću skriptu stvara prikaz naziva *SearchlogView* u zadanu bazu podataka i sheme:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Sljedeću skriptu pokazuje definirani prikazu:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Stvaranje tablica

Slično tablice relacijske baze podataka, U SQL omogućuje stvaranje tablice s unaprijed definirane shemom ili stvaranje tablice i shemu iz upita koji se nalaze u tablici (poznat i kao stvaranje odaberite kao tablicu ili CTAS).

Sljedeću skriptu stvaranje baze podataka i dvije tablice:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Tablica s upitima

Upit možete poslati tablice (stvorena u prethodna skripta) na isti način kao upit koji putem podatkovne datoteke. Umjesto stvaranja skup redaka pomoću IZDVOJITI sada se upućuje na naziv tablice.

Pretvorba skripte koje ste prethodno koristili izmjene čitati iz tablica:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Imajte na umu da trenutno ne možete pokrenuti POTVRDITE tablicu u istu skriptu kao skriptu koju stvorite tablicu.


##<a name="conclusion"></a>Zaključak

Opisana u ovom praktičnom vodiču je samo mali dio U SQL. Zbog opseg ovog praktičnog vodiča, ga ne pokriva sve, kao što su:

- Korištenje UNAKRSNE PRIMIJENITI otpakiravanje dijelove nizovi, polja i karte u retke.
- Funkcioniranje particioniranom skupa podataka (datoteke skupova i particioniranom tablica).
- Razvoj korisnički definirane operatora kao što su izdvajanje, outputters, procesora, korisnički definirane čitači u C#.
- Funkcije U SQL prikaz u prozorima.
- Upravljanje U SQL koda s prikaza, funkcije s tabličnim vrijednostima i pohranjene procedure.
- Pokrenite proizvoljne prilagođenog koda na vašem čvorove obrada.
- Povezivanje baze podataka SQL Azure i federate upiti preko njih te podatke U SQL i Lake Azure podataka.

## <a name="see-also"></a>Vidi također

- [Pregled analize podataka Lake za Microsoft Azure](data-lake-analytics-overview.md)
- [Razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Korištenje funkcija prozora U SQL Azure podataka Lake analize posla](data-lake-analytics-use-window-functions.md)
- [Praćenje i rješavanje problema s poslove Lake analize podataka za Azure pomoću portala za Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Recite nam što mislite

- [Pošaljite zahtjev za značajku](http://aka.ms/adlafeedback)
- [Zatražite pomoć na forumima](http://aka.ms/adlaforums)
- [Slanje povratnih informacija na U SQL](http://aka.ms/usqldiscuss)
