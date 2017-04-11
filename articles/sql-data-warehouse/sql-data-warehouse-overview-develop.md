<properties
   pageTitle="Dizajniranje odluke i odbijanje tehnike za razvoj SQL Data Warehouse | Microsoft Azure"
   description="Razvoj koncepti, odluke dizajna, preporuke i odbijanje postupaka za SQL Data Warehouse."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Dizajniranje odluke i odbijanje tehnike za SQL Data Warehouse

Pogledajte pomoću ovih članaka razvoj da biste bolje razumjeli odluke ključa dizajna, preporuke i odbijanje tehnike za SQL Data Warehouse.

## <a name="key-design-decisions"></a>Odluke ključa dizajna
U sljedećim člancima istaknuli neke koncepata i morat ćete objašnjenje za razvoj skladištu raspodijeljeno podataka pomoću SQL Data Warehouse odluke dizajna:

- [veze][]
- [istodobnosti][]
- [transakcije][]
- [korisnički definirane sheme][]
- [Tablica raspodjele][]
- [Indeksi tablice][]
- [Tablica particije][]
- [CTAS][]
- [Statistika][]

## <a name="development-recommendations-and-coding-techniques"></a>Preporuke za razvoj i odbijanje tehnike
Ti članci istaknuli određene kodiranje tehnike, savjeti i preporuke za razvoj SQL Data Warehouse:

- [pohranjene procedure][]
- [Natpisi][]
- [Prikazi][]
- [privremenih tablica][]
- [dinamični SQL][]
- [Ponavljanje][]
- [Grupiraj po mogućnostima][]
- [varijable dodjele][]

## <a name="next-steps"></a>Daljnji koraci
Kada su kroz članke razvoj pogledajte putem stranice [Transact-SQL referenca][] više pojedinosti na podržani sintaksa za SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[istodobnosti]: ./sql-data-warehouse-develop-concurrency.md
[veze]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dinamični SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[Grupiraj po mogućnostima]: ./sql-data-warehouse-develop-group-by-options.md
[Natpisi]: ./sql-data-warehouse-develop-label.md
[Ponavljanje]: ./sql-data-warehouse-develop-loops.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[pohranjene procedure]: ./sql-data-warehouse-develop-stored-procedures.md
[Tablica raspodjele]: ./sql-data-warehouse-tables-distribute.md
[Indeksi tablice]: ./sql-data-warehouse-tables-index.md
[Tablica particije]: ./sql-data-warehouse-tables-partition.md
[privremenih tablica]: ./sql-data-warehouse-tables-temporary.md
[transakcije]: ./sql-data-warehouse-develop-transactions.md
[korisnički definirane sheme]: ./sql-data-warehouse-develop-user-defined-schemas.md
[varijable dodjele]: ./sql-data-warehouse-develop-variable-assignment.md
[Prikazi]: ./sql-data-warehouse-develop-views.md
[Referenca za SQL transakcija]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
