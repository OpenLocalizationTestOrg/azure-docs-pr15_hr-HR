<properties
    pageTitle="Odaberite retke za migriranje pomoću funkcije filtra (rastegnuto baza podataka) | Microsoft Azure"
    description="Saznajte kako odabrati retke za migriranje pomoću funkcije Filtar."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Odaberite retke za migriranje pomoću funkcije filtra (rastegnuto baza podataka)

Ako u zasebnu tablicu pohranili Hladna podatke, možete konfigurirati rastezanje baze podataka da biste migrirali cijelu tablicu. Ako tablica sadrži najnovije i Hladna podataka, s druge strane, možete odrediti funkcija filter da biste odabrali retke koje želite migrirati. Filtar predikata je tablica u istoj razini\-vrijednosti (opis funkcije). U ovoj se temi opisuje kako napisati u retku tablice\-vrijednosti funkcije da biste odabrali retke će se migrirati.

>   [AZURE.NOTE] Ako unesete filtar funkcija koja se izvodi loše migraciju podataka i izvodi loše. Razvlačenje baze podataka odnosi funkcija filter u tablicu pomoću operatora UNAKRSNA PRIMIJENITI.

Ako ne navedete funkcija filter, migrira se na cijelu tablicu.

Kada pokrenete bazu podataka za omogućivanje za korištenje čarobnjaka za rastezanje, možete migrirati cijelu tablicu ili funkciju Jednostavni filtar možete odrediti u čarobnjaku. Ako želite koristiti neku drugu vrstu funkcija filter da biste odabrali retke za migriranje, učinite nešto od sljedećeg.

-   Zatvorite čarobnjak i pokrenite iskaz ALTER TABLE da biste omogućili rastezanje tablice i da biste odredili funkcija filter.

-   Iskaz ALTER TABLE da biste odredili funkcija filter nakon što izađete iz čarobnjaka za pokretanje.

Sintaksa ALTER TABLE za dodavanje funkcije opisan u nastavku ovog članka.

## <a name="basic-requirements-for-the-filter-function"></a>Osnovni sistemski preduvjeti za funkcija filter
U tablici u istoj razini\-vrijednosti funkcije potrebne za filtar predikata bazu podataka rastezanje izgleda kao u sljedećem primjeru.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Parametri funkcije mora biti identifikatore stupaca iz tablice.

Da biste spriječili stupce koji se koristi funkcija filter prekine ili izmijeniti potreban je poveznica sa shemom.

### <a name="return-value"></a>Povratna vrijednost
Ako funkcija vraća na koje nisu\-prazan rezultat redak ispunjava uvjete za migrirati. U suprotnom \- to jest, ako se funkcija ne vraća rezultat \- u retku ne ispunjava uvjete za migrirati.

### <a name="conditions"></a>Uvjeta
Na &lt; *predikata* &gt; može se sastojati od jednog uvjeta ili više uvjeta pridružen logičkih operatora AND.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Svaki uvjet shodno može se sastojati od jednog uvjeta jednostavne ili više uvjeta za jednostavne pridružen logičkih operatora ili.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Jednostavne uvjeta
Jednostavne uvjet možete učiniti nešto od sljedećeg usporedbe.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Usporedba funkcija parametar konstante izraz. Na primjer, `@column1 < 1000`.

    Slijedi primjer provjerava je li vrijednost stupca s *datumima* &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Operator IS NULL ili nije NULL primijeniti parametar (opis funkcije).

-   Koristite IN operator da biste usporedili parametar funkcija popis konstante.

    Slijedi primjer provjerava je li vrijednost u *pošiljci\_status* stupac `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Operatori usporedbe
Podržani su sljedeći operatori usporedbe.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Konstanta izraza
Konstantama koje koristite u funkciji filtar može biti bilo koji deterministic izraz koji je moguće vrednovati prilikom definiranja funkciju. Konstanta izrazi mogu sadržavati sljedeće.

-   Literala. Na primjer, `N’abc’, 123`.

-   Algebarski izraza. Na primjer, `123 + 456`.

-   Deterministic funkcije. Na primjer, `SQRT(900)`.

-   Deterministic pretvorbe koje koriste GLUMCIMA ili Pretvori. Na primjer, `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Slične izraze
Ako je rezultat funkcije u skladu s pravilima koje se ovdje opisuju nakon što zamijenite između i nije između operatora s izrazima ekvivalentan AND i OR možete koristiti između, a ne između operatore.

Ne možete koristiti podupiti ili ne\-deterministic funkcije kao što su RAND() ili GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Funkcija filter dodati u tablicu
Dodavanje funkcija filter u tablicu radi iskaz **ALTER TABLE** i navođenjem postojećoj tablici u istoj razini\-vrijednosti funkcija kao vrijednost na **filtar\_PREDICATE** parametar. Ako, na primjer:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Kada povežete funkciju tablice u obliku predikata, sljedeće su true.

-   Sljedeći put migracije podataka kada se dogodi, samo onih redaka za koje funkcija vraća na koje nisu\-migriraju praznu vrijednost.

-   Stupci koji se koristi funkciju su sheme vezana. Ti stupci ne možete mijenjati dok god tablice pomoću funkcije kao njegov predikata filtar.

Ne može se ispustiti tablice u istoj razini\-vrijednosti (opis funkcije) sve dok se tablica koristi funkciju kao njegov predikata filtar.

>   [AZURE.NOTE] Da biste poboljšali performanse funkcija filter, stvaranje indeksa na stupce koji se koriste funkcije.

### <a name="passing-column-names-to-the-filter-function"></a>Prosljeđivanje naziva stupaca u funkcija filter
Kada dodjeljujete funkcija filter u tablicu, navedite nazive stupaca proslijediti funkciji filtar s nazivom jedan dio. Ako navedete na tri dijela nakon što prođete stupcu imena, kasnije upite odabiranja na rastezanje\-omogućeni tablicu neće uspjeti.

Na primjer, ako odredite naziv stupca tri dijela kao što je prikazano u sljedećem primjeru, uspješno pokrenuli naredbu, ali neće uspjeti sljedećim upitima na tablici.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Umjesto toga, navedite funkciju filtar s naziv stupca jedan dio kao što je prikazano u sljedećem primjeru.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Dodavanje funkcija filter nakon pokretanja čarobnjaka  

Ako želite koristiti funkciju koja se ne možete stvarati u čarobnjaku za **Omogućivanje baze podataka za rastezanje** , možete pokrenuti iskaz ALTER TABLE da biste odredili funkcije nakon što izađete iz čarobnjaka. Prije primjene funkcije, međutim, imate da biste prekinuli prijenos podataka koji je već u tijeku i Premjesti nazad premještene podatke. (Da biste vidjeli dodatne informacije o zašto je to potrebno, potražite u članku [Zamjena postojećeg funkcija filter](#replacePredicate).  

1. Promijeni smjer migracije i Premjesti nazad podaci već migrirati. Ovaj postupak ne možete poništiti nakon pokretanja. Za izlazne podatke prijenosi i plaćati troškove na Azure \(izlazne\). Dodatne informacije potražite u članku [kako Azure cijene funkcionira](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Pričekajte za migraciju da biste završili. Možete provjeriti stanje **Monitor rastezanje baze podataka** iz sustava SQL Server Management Studio ili prikaz **sys.dm_db_rda_migration_status** možete upita. Dodatne informacije potražite u članku [monitora i otklanjanje poteškoća s migracije podataka](sql-server-stretch-database-monitor.md) ili [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Stvaranje funkcija filter koji želite primijeniti na tablicu.  

4. Dodavanje funkcija u tablicu, a zatim ponovno pokrenite migraciju podataka za Azure.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Filtriranje redaka prema datumu
U sljedećem primjeru prenosi retke gdje starija od siječnja 1, 2016 stupca **Datum** sadrži vrijednost.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtriranje redaka prema vrijednosti u stupcu stanje
U sljedećem primjeru prenosi retke gdje stupac **Stanje** sadrži jednu od navedenih vrijednosti.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Filtriranje redaka pomoću klizača prozora
Da biste filtrirali redaka pomoću klizača prozora, imajte na umu sljedeće preduvjete za funkciju filtar.

-   Funkcija mora biti deterministic. Stoga nije moguće stvoriti funkcija koja se automatski ponovno izračunava klizača prozora prolaskom vremena.

-   Funkcija koristi poveznica sa shemom. Stoga možete jednostavno ažurirati funkciju "na mjestu" svakodnevno tako da nazovete **ALTER FUNKCIJA** da biste premjestili u prozor klizača.

Započnite funkcija filter kao u sljedećem primjeru, koji se prenosi retke gdje starija od siječnja 1, 2016 **systemEndTime** stupac sadrži vrijednost.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Primijenite funkciju filtra za tablicu.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Ako želite da biste ažurirali klizača prozora, učinite sljedeće.

1.  Stvaranje nove funkcije koja određuje novi prozor klizača. Sljedeći primjer odabire datume starije od siječnja 2, 2016, umjesto siječanj 1, 2016.

2.  Zamijenite prethodne funkcija filter novi pozivanja **ALTER TABLE**, kao što je prikazano u sljedećem primjeru.

3. Po želji ispustite prethodne funkcija filter koju više ne koristite pozivanja **ISPUSTITE (opis funkcije)**. (Ovaj korak nije prikazan u primjeru.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Dodatne primjere funkcije valjani filtar

-   U sljedećem primjeru spaja dva jednostavne uvjeta pomoću logičkih operatora AND.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Sljedeći primjer koristi nekoliko uvjeta i deterministic pretvorbe sa PRETVORITI.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   U sljedećem primjeru pomoću funkcije i matematičke operatore.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Sljedeći primjer koristi između, a ne između operatore. Korištenje nije valjana jer je rezultat funkcije u skladu s pravilima koje se ovdje opisuju nakon što zamijenite između i nije između operatora s izrazima ekvivalentan AND i OR.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Prethodni funkcija jest sljedeća funkcija nakon zamijenite između, a ne između operatori izraza ekvivalentan AND i OR.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Primjeri filtar funkcije koje nisu valjane

-   Sljedeće funkcije nije valjan jer sadrži na koje nisu\-deterministic pretvorbe.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Sljedeće funkcije nije valjan jer sadrži na koje nisu\-deterministic funkcija poziv.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Sljedeće funkcije nije valjan jer sadrži podupita.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Sljedeće funkcije nisu valjane jer izraza koji koriste algebarski operatora ili ugrađeni\-u funkcijama moraju biti vrednovani konstantu prilikom definiranja funkciju. Nije moguće dodati reference stupca web-mjesto algebarski izraza ili u okvir za pozive (opis funkcije).

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Sljedeće funkcije nije valjan jer je krši pravila koja se ovdje opisuje nakon operator između zamijenite ekvivalentan izraz i.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Prethodni funkcija jest sljedeća funkcija nakon operatora između zamijenite ekvivalentan izraz i. Ova funkcija nije valjan jer jednostavne uvjeta možete koristiti samo logičkih operatora ili.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Kako rastezanje baze podataka primjenjuje filtar (funkcija)
Razvlačenje baze podataka primjenjuje funkcija filter u tablicu i određuje uvjete redaka pomoću operatora UNAKRSNA PRIMIJENITI. Ako, na primjer:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Ako funkcija vraća na koje nisu\-prazan rezultat za redak u retku ispunjava uvjete za migrirati.

## <a name="replacePredicate"></a>Zamjena postojećeg funkcija filter
Funkcija prethodno navedene filter možete zamijeniti izvodi iskaz **ALTER TABLE** i navođenjem nova vrijednost za na **filtar\_PREDICATE** parametar. Ako, na primjer:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Nove tablice u istoj razini\-vrijednosti funkcija ima sljedeće preduvjete.

-   Nova funkcija mora biti manje ograničenja od prethodnog funkcija.

-   Operatori programa u funkciji stare moraju se nalaziti u novoj funkciji.

-   Nova funkcija ne smije sadržavati operatore koje ne postoje u funkciji stari.

-   Ne možete promijeniti redoslijed operatora argumenata.

-   Samo konstante vrijednosti koje su dio na `<, <=, >, >=` uspoređivanja se mogu mijenjati na način koji omogućuje funkciju manji ograničenja.

### <a name="example-of-a-valid-replacement"></a>Primjer valjani zamjena
Pretpostavimo da je sljedeća funkcija trenutni predikata filtar.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Sljedeće funkcije je valjan zamjenski jer konstantu novi datum (koji određuje kasnije umanjenu datum) će funkciju manje ograničenja.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Primjeri zamjena koje nisu valjane
Sljedeće funkcije nije valjani zamjenski jer konstantu novi datum (koji određuje ranije umanjenu datumom) ne bi funkciju manje ograničenja.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Sljedeće funkcije nije valjani zamjenski jer nešto operatori usporedbe uklonjena.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Sljedeće funkcije nije valjani zamjenski jer novog uvjeta dodana je s logičkih operatora AND.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Uklanjanje funkcija filter iz tablice
Da biste migrirali cijelu tablicu umjesto odabrane retke, uklonite postojeće funkcija postavljanjem **filtar\_PREDICATE** null. Ako, na primjer:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nakon što uklonite funkcija filter, svi reci u tablici su uvjete za migraciju. Zbog toga ne možete navesti funkcija filter za istu tablicu osim ako prenesete natrag udaljeni podaci za tablicu iz Azure prvi put. To ograničenje postoji da biste izbjegli situacija u kojima redaka koji su ne ispunjava uvjete za migraciju kada unesete novi funkcija filter ste već je migrira se na Azure.

## <a name="check-the-filter-function-applied-to-a-table"></a>Provjerite funkciju filtar primjenjuje na tablicu
Da biste provjerili funkcija filter primijenjene na tablicu, otvorite Prikaz kataloga **sys.remote\_podataka\_arhiva\_tablice** i provjerite vrijednost u **filtar\_predikata** stupca. Ako je vrijednost null cijelu tablicu je ispunjava uvjete za arhiviranje. Dodatne informacije potražite u članku [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Napomene o sigurnosti za funkcije filtra  
Compromised računa s ovlastima db_owner možete činiti sljedeće.  

-   Stvaranje i Primjena funkcije s tabličnim vrijednostima koja troši velike količine resurse poslužitelja ili čeka dulje rezultira uskraćivanje usluga.  

-   Stvaranje i Primjena funkcije s tabličnim vrijednostima koja omogućuje nametnuo sadržaj tablicu za koju korisnik izričito odbijen pristup za čitanje.  

## <a name="see-also"></a>Vidi također

[Iskaz ALTER TABLICE (-SQL transakcija)](https://msdn.microsoft.com/library/ms190273.aspx)
