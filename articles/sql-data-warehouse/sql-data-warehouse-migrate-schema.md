<properties
   pageTitle="Migriranje sheme u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za migriranje sheme za Azure SQL Data Warehouse za razvoj rješenja."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Migriranje sheme u SQL Data Warehouse#

Sljedeće sažetaka olakšavaju razlike između sustava SQL Server i SQL Data Warehouse da biste lakše migrirati bazu podataka.

## <a name="table-migration"></a>Migracija tablice

Prilikom migracije tablice koje ćete se upoznali sa značajkama tablice SQL Data Warehouse tablica.  [Pregled tablica][] je sjajno mjesto za početak.  U ovom se članku predstavlja najvažnije pitanja vezana uz prilikom stvaranja tablice kao što su tablice statistike, distribuciju, particija i indeksiranja.  Također prekrije neke [nepodržane značajke tablica][] i zaobilazna rješenja.

SQL Data Warehouse podržava uobičajenih vrsta poslovnih podataka.  Popis u članku [vrste podataka][] podržane i [nepodržane vrste podataka][].  U članku [vrste podataka][] sadrži i upit da biste odredili [nepodržane vrste podataka][].  Pri pretvorbi na vrste podataka, svakako pogledajte [najbolje prakse za vrste podataka][].

## <a name="next-steps"></a>Daljnji koraci
Kada uspješno prenijeli na shemu baze podataka SQL Data Warehouse, prijeđite na jedan od sljedećih članaka:

- [Migracija podataka][]
- [Migriranje kod][]

Dodatne informacije o SQL Data Warehouse najbolje prakse potražite u članku [najbolje prakse][] .

<!--Image references-->

<!--Article references-->
[Migriranje kod]: ./sql-data-warehouse-migrate-code.md
[Migracija podataka]: ./sql-data-warehouse-migrate-data.md
[najbolje prakse]: ./sql-data-warehouse-best-practices.md
[Pregled tablica]: ./sql-data-warehouse-tables-overview.md
[nepodržane značajke tablica]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[vrste podataka]: ./sql-data-warehouse-tables-data-types.md
[nepodržane vrste podataka]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[najbolje prakse za vrste podataka]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
