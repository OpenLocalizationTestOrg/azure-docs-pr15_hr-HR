<properties
   pageTitle="Migriranje SQL koda u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za migriranje SQL koda za Azure SQL Data Warehouse za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Migriranje SQL koda u SQL Data Warehouse

Prilikom migracije koda iz druge baze podataka za SQL Data Warehouse, najvjerojatnije morat ćete mijenjati vaša osnovna kod. Neke značajke SQL Data Warehouse možete znatno poboljšati performanse kao osmišljeni su za rad na raspodijeljeno način. Međutim, da biste zadržali performanse i promjena veličine, neke značajke nisu i dostupne.

## <a name="common-t-sql-limitations"></a>Zajednička ograničenja T SQL

Na sljedećem su popisu navedene najčešće značajke koje nisu podržane u skladištu podataka za SQL Azure. Veza saznat ćete zaobilazna rješenja za nepodržane značajke:

- [ANSI spojeva ažuriranja][]
- [ANSI spojevi u briše][]
- [Naredba za spajanje][]
- spojevi izdvojiti bazu podataka
- [pokazivači][]
- [ODABERITE... U][]
- [UMETANJE... IZVRŠAVANJA PRVE][]
- uvjet Izlaz
- korisnički definirane funkcije u istoj razini
- više izjava funkcije
- [Uobičajeni tabličnih izraza](#Common-table-expressions)
- [rekurzivne uobičajenih tabličnih izraza (CTE)] (#Recursive-common-table-expressions-(CTE)
- Funkcija CLR i postupcima
- Funkcija $partition
- varijable tablica
- Tablica vrijednosti parametara
- raspodijeljeno transakcije
- Potvrđivanje / vraćanje rad
- Spremanje transakcije
- izvršavanje konteksta (IZVRŠAVANJE AS)
- [Grupiranje prema uvjet s funkcijom rollup / kocka / skupove mogućnosti grupiranja][]
- [razina gniježđenja izvan 8][]
- [Ažuriranje putem prikaza][]
- [Korištenje odaberite za dodjelu varijable][]
- [Nema podataka o MAX upišite za dinamičku SQL nizova][]

Srećom većinu tih ograničenja možete radili oko. Objašnjenja isporučuju se u člancima odgovarajući razvoj poziva iznad.

## <a name="supported-cte-features"></a>Podržane značajke CTE

Uobičajeni tabličnih izraza (CTEs) su djelomično podržani u SQL Data Warehouse.  Sljedeće značajke CTE trenutno podržava:

- Možete navesti na CTE u naredbi SELECT.
- Na CTE može biti naveden u Naredba CREATE VIEW.
- Možete navesti na CTE u naredbi za stvaranje TABLICE kao odaberite (CTAS).
- Možete navesti na CTE u naredbi stvaranje UDALJENE TABLICE kao odaberite (CRTAS).
- Možete navesti na CTE u naredbi za stvaranje VANJSKOG TABLICE kao odaberite (CETAS).
- Udaljenoj tablici možete se pozivati iz programa CTE.
- Vanjska tablica možete se pozivati iz programa CTE.
- Višestruke definicije upita CTE može se definirati u na CTE.

## <a name="cte-limitations"></a>Ograničenja CTE

Uobičajeni tabličnih izraza postoje određena ograničenja u sustavu SQL Data Warehouse, uključujući:

- Na CTE morate slijedi jedan iskaza SELECT. Umetanje, ažuriranje, brisanje i SPAJANJE izjave nisu podržani.
- Uobičajeni izraz tablice koji sadrži reference na samom (rekurzivne Uobičajeni izraz tablice) nije podržano (pogledajte ispod sekcija).
- Određivanje više uz uvjet u na CTE nije dopušteno. Na primjer, ako je CTE_query_definition sadrži podupita, taj podupita ne smije sadržavati s ugniježđenim uz uvjet WHERE koji određuje drugi CTE.
- Uvjet ORDER BY nije moguće koristiti u CTE_query_definition, osim ako je naveden uvjetu TOP.
- Kada je CTE se koristi u naredbi koja je dio seriji, Izjava prije mora biti prikazane točkom sa zarezom.
- Kada se koristi u izvješćima pripremljeni prema sp_prepare, CTEs će se ponašati na isti način kao drugi izjava ODABIRA u PDW. Međutim, ako CTEs koriste kao dio CETAS pripremljeni prema sp_prepare, ponašanje možete odgoditi iz sustava SQL Server i druge naredbe PDW zbog način povezivanja je implementirana za sp_prepare. Ako je odabir u sp_prepare proći kroz bez otkrivanje pogreške reference CTE koristi pogrešnom stupcu koji ne postoji u CTE, ali pogreška će se tijekom izbačena sp_execute umjesto toga.

## <a name="recursive-ctes"></a>Rekurzivne CTEs

SQL Data Warehouse ne podržava rekurzivne CTEs.  Migraion rekurzivne CTE može biti Pomalo dovršavanje i postupak najbolje je da biste prekinuli prema dolje u u više korake. Obično možete koristiti petlje i popunjavanje privremena kao ponavljanje rekurzivne privremeni upiti. Kada se popunjava privremene tablice pa možete vratiti podatke u skupu jedan rezultat. Sličan pristup korišten za rješavanje `GROUP BY WITH CUBE` u članku [Grupiranje prema uvjet s funkcijom rollup / kocka / skupove mogućnosti grupiranja][] .

## <a name="unsupported-system-functions"></a>Funkcija sustav nije podržan

Postoje i neke sustava funkcije koje nisu podržane. Neke od glavni onih koji se obično pronađenom koriste u podacima skladištenje su:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Neke od tih problema možete radili oko.

## <a name="rowcount-workaround"></a>@@ROWCOUNTzaobilazno rješenje

Da biste zaobišli nedostatak podrške za @@ROWCOUNT, stvaranje pohranjena procedura koji će dohvatiti zadnji broj redaka iz sys.dm_pdw_request_steps, a zatim izvršite `EXEC LastRowCount` nakon DML naredbe.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Daljnji koraci
Cjelovit popis svih podržanih T SQL naredbe potražite u [temama Transact-SQL][].

<!--Image references-->

<!--Article references-->
[ANSI spojeva ažuriranja]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI spojevi u briše]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Naredba za spajanje]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[UMETANJE... IZVRŠAVANJA PRVE]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[SQL transakcija teme]: ./sql-data-warehouse-reference-tsql-statements.md

[pokazivači]: ./sql-data-warehouse-develop-loops.md
[ODABERITE... U]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Grupiranje prema uvjet s funkcijom rollup / kocka / skupove mogućnosti grupiranja]: ./sql-data-warehouse-develop-group-by-options.md
[razina gniježđenja izvan 8]: ./sql-data-warehouse-develop-transactions.md
[Ažuriranje putem prikaza]: ./sql-data-warehouse-develop-views.md
[Korištenje odaberite za dodjelu varijable]: ./sql-data-warehouse-develop-variable-assignment.md
[Nema podataka o MAX upišite za dinamičku SQL nizova]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
