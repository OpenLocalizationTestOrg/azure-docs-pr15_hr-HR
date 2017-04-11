<properties
   pageTitle="Migracija postojeće skladištu podataka SQL Azure u spremište premium | Microsoft Azure"
   description="Upute za premještanje postojeće SQL Data Warehouse premium za pohranu"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Migracija Premium prostora za pohranu detalja
SQL Data Warehouse nedavno uvedena [Premium prostor za pohranu veći predvidljivost performansi][].  Ne možemo sada ste spremni za migriranje postojeće podatke skladištima trenutno na standardni prostora za pohranu u Premium prostora za pohranu.  Nastavite čitati dodatne pojedinosti o kako i kada dođe do automatskog Migracija a koja se sama migriranju ako biste radije da biste odredili kada se pojavljuje na nedostupnost.

Ako imate više od jedne Data Warehouse, koristite [raspored automatskog premještanja][] dolje da biste odredili kada također se migrirati.

## <a name="determine-storage-type"></a>Određivanje vrste prostora za pohranu
Ako ste stvorili u DW prije datuma u nastavku, već koristite standardne prostora za pohranu.  Svaki Data Warehouse na standardni prostora za pohranu podložni automatskog premještanja sadrži obavijest pri vrhu plohu Data Warehouse [Azure Portal][] koja vas obavještava da "Nadogradnja*na nadolazeće premium prostora za pohranu potrebno se prekida.  Saznajte više ->*. "

| **Regija**          | **DW stvorena prije tog datuma**   |
| :------------------ | :-------------------------------- |
| Istok Australija      | Prostor za pohranu Premium još nije dostupno |
| Australija Jugoistok | Kolovoz 5, 2016                    |
| Južna Brazil        | Kolovoz 5, 2016                    |
| Središnja Kanada      | Možda 25, 2016                      |
| Istok Kanada         | Možda 26, 2016                      |
| Središnje SAD-a          | Možda 26, 2016                      |
| Kina Istok          | Prostor za pohranu Premium još nije dostupno |
| Sjeverna Kina         | Prostor za pohranu Premium još nije dostupno |
| Istočnoazijski           | Možda 25, 2016                      |
| Istočni SAD-a             | Možda 26, 2016                      |
| Istočni US2            | Možda 27, 2016                      |
| Središnja Indija       | Možda 27, 2016                      |
| Južna Indija         | Možda 26, 2016                      |
| Indija Zapad          | Prostor za pohranu Premium još nije dostupno |
| Istok Japan          | Kolovoz 5, 2016                    |
| Japan Zapad          | Prostor za pohranu Premium još nije dostupno |
| Sjeverna središnje SAD-a    | Prostor za pohranu Premium još nije dostupno |
| Sjeverna Europa        | Kolovoz 5, 2016                    |
| Južna središnje SAD-a    | Možda 27, 2016                      |
| Jugoistočne Azije      | Možda 24, 2016                      |
| Europa Zapad         | Možda 25, 2016                      |
| Zapad središnje SAD-a     | Kolovoz 26, 2016                   |
| Zapad SAD-a             | Možda 26, 2016                      |
| Zapad US2            | Kolovoz 26, 2016                   |

## <a name="automatic-migration-details"></a>Detalji o automatskog premještanja
Prema zadanim postavkama, ne možemo će migrirati bazu podataka za vas tijekom 6 pm i 6: 00 u vašoj regiji lokalno vrijeme tijekom [raspored automatskog premještanja][] ispod.  Postojeće Data Warehouse bit će nestabilan tijekom migracije.  Ne možemo procjenu Migracija će potrajati oko jedan sat po TB prostora za pohranu po Data Warehouse.  Ne možemo će također osigurati da ne naplaćuje tijekom bilo koji dio automatskog premještanja.

> [AZURE.NOTE] Nećete moći koristiti postojeće Data Warehouse tijekom migracije.  Nakon dovršetka migracije Data Warehouse će se vratite u mrežni način.

Detalji o nastavku su koraci Microsoft traje u vaše ime da biste dovršili migracije, a ne zahtijeva sve involvement na svoj dio.  U ovom primjeru zamislite da vaše postojeće DW na standardni prostora za pohranu trenutno pod nazivom "MyDW."

1.  Microsoft preimenuje "MyDW" u "MyDW_DO_NOT_USE_ [vremenska oznaka]"
2.  Microsoft zaustavlja "MyDW_DO_NOT_USE_ [vremenska oznaka]."  Za to vrijeme koristi se sigurnosnu kopiju.  Vidjet ćete više Pauziraj/životopise ako smo naiđe na probleme tijekom ovog postupka.
3.  Microsoft stvara novi DW pod nazivom "MyDW" na Premium prostora za pohranu iz sigurnosne kopije u korak 2.  "MyDW" prikazat će se tek nakon obnavljanja.
4.  Nakon dovršetka vraćanja "MyDW" se vraća u isti DWUs i pauzirano ili aktivnog stanje prije migracije.
5.  Nakon dovršetka migracije Microsoft briše "MyDW_DO_NOT_USE_ [vremenska oznaka]"
    
> [AZURE.NOTE] Ove postavke drži kao dio migracije:
> 
>   -  Nadzor na razini baze podataka mora biti ponovno omogućiti
>   -  Pravila vatrozida na razini **baze podataka** moraju biti readded.  Pravila vatrozida na razini **poslužitelja** se neće utjecati.

### <a name="automatic-migration-schedule"></a>Zakazivanje automatskog premještanja
Automatsko Migracija pojaviti od 18: 00 – 6: 00 (lokalno vrijeme po regijama) tijekom sljedeći raspored prekida.

| **Regija**          | **Procijenjena početnog datuma**     | **Procijenjena završni datum**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Istok Australija      | Još nije odrediti           | Još nije odrediti           |
| Australija Jugoistok | Kolovoz 10, 2016              | Kolovoz 24, 2016              |
| Južna Brazil        | Kolovoz 10, 2016              | Kolovoz 24, 2016              |
| Središnja Kanada      | Lipnja 23, 2016                | Srpanj 1, 2016                 |
| Istok Kanada         | Lipnja 23, 2016                | Srpanj 1, 2016                 |
| Središnje SAD-a          | Lipnja 23, 2016                | Srpanj 4, 2016                 |
| Kina Istok          | Još nije odrediti           | Još nije odrediti           |
| Sjeverna Kina         | Još nije odrediti           | Još nije odrediti           |
| Istočnoazijski           | Lipnja 23, 2016                | Srpanj 1, 2016                 |
| Istočni SAD-a             | Lipnja 23, 2016                | Srpanj 11, 2016                |
| Istočni US2            | Lipnja 23, 2016                | Srpanj 8, 2016                 |
| Središnja Indija       | Lipnja 23, 2016                | Srpanj 1, 2016                 |
| Južna Indija         | Lipnja 23, 2016                | Srpanj 1, 2016                 |
| Indija Zapad          | Još nije odrediti           | Još nije odrediti           |
| Istok Japan          | Kolovoz 10, 2016              | Kolovoz 24, 2016              |
| Japan Zapad          | Još nije odrediti           | Još nije odrediti           |
| Sjeverna središnje SAD-a    | Još nije odrediti           | Još nije odrediti           |
| Sjeverna Europa        | Kolovoz 10, 2016              | Kolovoz 31, 2016              |
| Južna središnje SAD-a    | Lipnja 23, 2016                | Srpanj 2, 2016                 |
| Jugoistočne Azije      | Lipnja 23, 2016                | Srpanj 1, 2016                 |
| Europa Zapad         | Lipnja 23, 2016                | Srpanj 8, 2016                 |
| Zapad središnje SAD-a     | Kolovoz 14, 2016              | Kolovoz 31, 2016              |
| Zapad SAD-a             | Lipnja 23, 2016                | Srpanj 7, 2016                 |
| Zapad US2            | Kolovoz 14, 2016              | Kolovoz 31, 2016              |

## <a name="self-migration-to-premium-storage"></a>Self-migracije na Premium prostora za pohranu
Ako želite odrediti kada će se izvršiti na nedostupnost, možete koristiti sljedeće korake da biste migrirali postojeće Data Warehouse na standardni prostora za pohranu za pohranu Premium.  Ako odlučite koja se sama migrirati self-migracije morate dovršiti prije početka automatskog premještanja u tom području da biste izbjegli opasnosti automatskog premještanja uzrokuje sukob (pogledajte [raspored automatskog premještanja][]).

### <a name="self-migration-instructions"></a>Upute za Self-migracije
Ako želite da biste odredili na nedostupnost, koja se sama možete migrirati Data Warehouse pomoću sigurnosnog kopiranja i vraćanja.  Vraćanje dio migracije se očekuje da bi oko jedan sat po TB prostora za pohranu po DW.  Ako želite da ostanu s istim nazivom nakon dovršetka migracije, slijedite korake za [koraci za preimenovanje tijekom migracije][]. 

1.  [Zaustavljanje][] vaše DW koji traje automatske sigurnosnog kopiranja
2.  [Vraćanje][] iz vaše zadnje snimke
3.  Izbrišite svoje postojeće DW na standardni prostora za pohranu. **Ako u ovom koraku, koji će se naplaćivati za oba DWs.**

> [AZURE.NOTE] Ove postavke drži kao dio migracije:
> 
>   -  Nadzor na razini baze podataka mora biti ponovno omogućiti
>   -  Pravila vatrozida na razini **baze podataka** moraju biti readded.  Pravila vatrozida na razini **poslužitelja** se neće utjecati.

#### <a name="optional-steps-to-rename-during-migration"></a>Neobavezno: koraci za preimenovanje tijekom migracije 
Dvije baze podataka na istom poslužitelju logičke ne mogu imati isti naziv. SQL Data Warehouse sada podržava mogućnost da biste preimenovali na DW.

U ovom primjeru zamislite da vaše postojeće DW na standardni prostora za pohranu trenutno pod nazivom "MyDW."

1.  Preimenovanje "MyDW" pomoću naredbe ALTER baze podataka da prati u nešto što vam se sviđa "MyDW_BeforeMigration."  Ta naredba kills sve postojeće transakcije i morate učiniti u glavnom bazom podataka da.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Zaustavi][] "MyDW_BeforeMigration", koji traje automatske sigurnosnog kopiranja
3.  [Vraćanje][] iz vaše najnovije snimke novu bazu podataka s nazivom koristi da bi (ex: "MyDW")
4.  Brisanje "MyDW_BeforeMigration".  **Ako u ovom koraku, koji će se naplaćivati za oba DWs.**

> [AZURE.NOTE] Ove postavke drži kao dio migracije:
> 
>   -  Nadzor na razini baze podataka mora biti ponovno omogućiti
>   -  Pravila vatrozida na razini **baze podataka** moraju biti readded.  Pravila vatrozida na razini **poslužitelja** se neće utjecati.

## <a name="next-steps"></a>Daljnji koraci
S promjenom Premium za pohranu, ne možemo imati i povećava se broj datoteka blob baze podataka u podlozi arhitekturi Data Warehouse.  Da biste maksimizirali performanse prednosti ove promjene, preporučujemo da ponovno stvaranje vaše grupirani Columnstore indeksi pomoću sljedeće skripte.  U nastavku skriptu radi tako da nameće neke od postojećih podataka dodatne blob-ova.  Ako želite da vam ne akciju, podaci će prirodan ponovno Raspodijeli tijekom vremena kao učitavanje dodatnih podataka u tablicama Data Warehouse.

**Prije requisites:**

1.  Data Warehouse trebale bi funkcionirati s 1000 DWUs ili noviji (pogledajte [Skaliranje računalnim power][])
2.  Korisnik izvršavanja skriptu mora biti u [ulozi mediumrc][] ili noviji
    1.  Za dodavanje korisnika u grupu uloga, izvršiti sljedeće: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Ako naiđete na probleme s Data Warehouse, [stvorite zahtjev za podršku možete][] i referencu "Migracije za pohranu Premium" kao mogući uzrok.

<!--Image references-->

<!--Article references-->
[Zakazivanje automatskog premještanja]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[Stvaranje zahtjev za podršku možete]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Zaustavi]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Vraćanje]: sql-data-warehouse-restore-database-portal.md
[Koraci za preimenovanje tijekom migracije]: #optional-steps-to-rename-during-migration
[Promjena veličine računalnim power]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[mediumrc uloga]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Premium prostor za pohranu veći predvidljivost performansi]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Portal za Azure]: https://portal.azure.com
