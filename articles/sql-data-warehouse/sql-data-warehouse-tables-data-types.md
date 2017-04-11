<properties
   pageTitle="Vrste podataka u tablicama u SQL Data Warehouse | Microsoft Azure"
   description="Uvod u vrste podataka za Azure SQL Data Warehouse tablice."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Vrste podataka u tablicama u SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled][]
- [Vrste podataka][]
- [Distribucija][]
- [Indeks][]
- [Particija][]
- [Statistika][]
- [Privremeni][]

SQL Data Warehouse podržava najčešće koriste vrste podataka.  Slijedi popis vrsta podataka koje podržava SQL Data Warehouse.  Dodatne informacije o vrsti podataka podržava potražite u članku [Stvaranje tablice][].

|**Podržane vrste podataka**|||
|---|---|---|
[bigint][]|[broja decimalnih mjesta][]|[SMALLINT][]|
[Binarni.][]|[vrijednost s pomičnim zarezom][]|[smallmoney][]|
[bitne][]|[INT][]|[sysname][]|
[CHAR][]|[novac][]|[vrijeme][]|
[Datum][]|[NCHAR][]|[tinyint][]|
[Datum i vrijeme][]|[nvarchar][]|[Jedinstveni identifikator koji][]|
[datetime2][]|[realni][]|[varbinary][]|
[datetimeoffset][]|[smalldatetime][]|[VARCHAR][]|


## <a name="data-type-best-practices"></a>Najbolje prakse za vrste podataka

 Prilikom definiranja vrste stupaca za vaše najmanju vrstu podataka koje podržava podataka će poboljšati performanse upita. To je osobito važno za znak i VARCHAR stupce. Ako je najdulje vrijednost u stupcu od 25 znakova, definirajte stupca kao VARCHAR(25). Izbjegavajte definiranju sve stupce znak za velike zadane duljine. Osim toga, definirajte stupaca kao VARCHAR kada je to sve što je potrebno umjesto korištenja [NVARCHAR][].  Umjesto NVARCHAR(MAX) ili VARCHAR(MAX) koristiti NVARCHAR(4000) ili VARCHAR(8000) kada je to moguće.

## <a name="polybase-limitation"></a>Ograničenje Polybase

Ako koristite Polybase da biste učitali tablice, definirati tablice tako da se veličinu Maksimalna retka moguće, uključujući pune dužine stupaca dužine varijable ne biti dulji od 32.767 bajtova.  Dok možete definirati redak s podacima dužine varijable koji mogu biti dulji od ovaj širinu i učitavanje redaka sa BCP, nećete moći koristiti Polybase da biste učitali podatke.  Uskoro će biti dodana Polybase podrška za široke redaka.

## <a name="unsupported-data-types"></a>Nepodržane vrste podataka

Ako premještate bazu podataka iz druge platforme SQL kao što su baze podataka SQL Azure, kao što je migracije, možda ćete naići neke vrste podataka koje nisu podržane na SQL Data Warehouse.  U nastavku su nepodržane vrste podataka, kao i neke alternative možete koristiti umjesto nepodržane vrste podataka.

|Vrsta podataka|Zaobilazno rješenje|
|---|---|
|[geometriju][]|[varbinary][]|
|[Zemljopis][]|[varbinary][]|
|[hierarchyid][]|[nvarchar][] (4000)|
|[Slika][ntext,text,image]|[varbinary][]|
|[tekst][ntext,text,image]|[VARCHAR][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Podijeli stupac u nekoliko svakako upisani stupce.|
|[Tablica][]|Pretvaranje u privremene tablice.|
|[Vremenska oznaka][]|Dorada radi korištenja kod tako da koristi [datetime2][] i `CURRENT_TIMESTAMP` (opis funkcije).  Podržani su samo konstante kao zadane, stoga current_timestamp ne može biti definiran kao zadani ograničenja. Ako trebate migrirati retka verziju vrijednosti iz stupca upisani vremenske oznake koristiti [BINARNI][](8) ili [VARBINARY][BINARNI](8) za NULL ili NULL vrijednosti verziju retka.|
|[XML][]|[VARCHAR][]|
|[korisnički definirane vrste][]|ponovno pretvoriti njihove izvorne vrste gdje je to moguće|
|zadane vrijednosti|zadane vrijednosti podržava literala i samo konstante.  Osobe koje nisu deterministic izraza i funkcije, kao što su `GETDATE()` ili `CURRENT_TIMESTAMP`, nisu podržane.|

U nastavku SQL mogu se izvoditi na vaša trenutna SQL baze podataka da biste odredili stupaca koji se ne može podržati Azure SQL Data Warehouse:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u člancima na [Pregled tablica][Pregled], [distribucija tablice][Raspodijeli], [Indeksiranje tablice][indeks], [particija tablice][particije], [Zadržavanje tablice Statistika][Statistika] i [Privremenih tablica][privremene].  Dodatne informacije o najbolje prakse potražite u članku [Najbolje prakse skladištu podataka SQL][].

<!--Image references-->

<!--Article references-->
[Pregled]: ./sql-data-warehouse-tables-overview.md
[Vrste podataka]: ./sql-data-warehouse-tables-data-types.md
[Distribucija]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Particija]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Privremeni]: ./sql-data-warehouse-tables-temporary.md
[Najbolje prakse skladištu podataka SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[Stvaranje tablice]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[Binarni.]: https://msdn.microsoft.com/library/ms188362.aspx
[bitne]: https://msdn.microsoft.com/library/ms177603.aspx
[CHAR]: https://msdn.microsoft.com/library/ms176089.aspx
[Datum]: https://msdn.microsoft.com/library/bb630352.aspx
[Datum i vrijeme]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[broja decimalnih mjesta]: https://msdn.microsoft.com/library/ms187746.aspx
[vrijednost s pomičnim zarezom]: https://msdn.microsoft.com/library/ms173773.aspx
[geometriju]: https://msdn.microsoft.com/library/cc280487.aspx
[Zemljopis]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[INT]: https://msdn.microsoft.com/library/ms187745.aspx
[novac]: https://msdn.microsoft.com/library/ms179882.aspx
[NCHAR]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[realni]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[SMALLINT]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[Tablica]: https://msdn.microsoft.com/library/ms175010.aspx
[vrijeme]: https://msdn.microsoft.com/library/bb677243.aspx
[Vremenska oznaka]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[Jedinstveni identifikator koji]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[VARCHAR]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[korisnički definirane vrste]: https://msdn.microsoft.com/library/ms131694.aspx
