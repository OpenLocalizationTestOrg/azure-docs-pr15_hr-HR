<properties
   pageTitle="Koristite natpise u instrument upite u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za korištenje oznake instrument upitima u skladištu podataka za SQL Azure za razvoj rješenja."
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

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Koristi natpise u instrument upite u skladištu podataka za SQL
SQL Data Warehouse podržava pojam naziva upita naljepnice. Pogledajte primjer jedne prije prelaska u bilo kojem dubine recimo:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

U ovom zadnjem retku oznake niz oznaka Moje upita u upit. To je osobito korisni kao oznaku upit-može se kroz na DMVs. To nam omogućuje mehanizam pronaći problem upita i rješavanju napredak kroz programa ETL Pokreni.

Dobar konvencija imenovanja olakšava zaista ovdje. Primjerice na primjer "PROJEKTA: postupak: IZJAVA: KOMENTAR ' bi lakše identificirati samo upita u navedenih sav kod izvor kontrole.

Pretraživanje prema natpis koristite sljedeći upit koji koristi dinamički Upravljanje prikazima:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Preporučuje se ključna da možete prelomiti uglate zagrade ili dvostruki navodnici oko riječi oznaka prilikom postavljanja upita. Natpis je rezervirana riječ i će uzrokovati pogrešku ako je razgraničena.


## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[Pregled razvoj]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
