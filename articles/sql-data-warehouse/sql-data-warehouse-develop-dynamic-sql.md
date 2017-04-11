<properties
   pageTitle="Dinamični SQL u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za korištenje dinamički SQL programa Azure SQL Data Warehouse za razvoj rješenja."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dinamični SQL u skladištu podataka za SQL
Kada razvoj aplikacija kod za SQL Data Warehouse možda ćete morati pomoću dinamičke sql izlaganje fleksibilne, generički i modularan rješenja. SQL Data Warehouse ne podržava blob vrste podataka trenutno. To može vam ograničiti veličinu nizovima kao blob vrste obuhvaćaju vrste varchar(max) i nvarchar(max). Ako ste koristili te vrste u kodu aplikacije kada sastavljanjem vrlo velike nizovi, morat ćete podijeliti kod u blokova i upotrijebite naredbu izvršavanja PRVE.

Jednostavnog primjera:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Ako je niz kratkih možete koristiti [sp_executesql][] normalno.

> [AZURE.NOTE] I dalje je da izjave izvršava kao dinamički SQL podložno sva pravila provjere valjanosti TSQL.

## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[Pregled razvoj]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
