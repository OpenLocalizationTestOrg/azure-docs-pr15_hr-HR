<properties
   pageTitle="Dodjeljivanje varijable u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za dodjeljivanje Transact-SQL Varijable u Azure SQL Data Warehouse za razvoj rješenja."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Dodjeljivanje varijable u SQL Data Warehouse
Varijable u SQL Data Warehouse se postavljaju pomoću na `DECLARE` izjava ili `SET` izjava.

Sljedeće načina savršeno valjanu za postavljanje varijable vrijednosti:

## <a name="setting-variables-with-declare"></a>Postavljanje varijable s OBJAVA

Pokretanje varijable s OBJAVA je jedan od najviše fleksibilne načina za postavljanje varijable vrijednosti u SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

Da biste postavili više od jedne varijable odjednom možete koristiti i OBJAVA. Ne možete koristiti `SELECT` ili `UPDATE` da biste to učinili:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Ne možete initialise i koristite varijablu u istom izjavi OBJAVA. Da biste ilustrirali točke u primjeru u nastavku **nije** dozvoljen u @p1 je pokrenuti i koristiti u istu izjava OBJAVA. To će uzrokovati pogrešku.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Postavljanje vrijednosti sa SKUPOM
Postavljanje je vrlo uobičajeni način za postavljanje jednu varijablu.

Sve primjere u nastavku su postavljanje varijable sa SKUPOM valjani načina:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Jednom varijablom možete postaviti samo istovremeno sa SKUPOM. Međutim, kao što je vidljivo iznad složenih operatori su permissable.

## <a name="limitations"></a>Ograničenja
Odabir ili ažuriranje ne možete koristiti za dodjelu varijable.


## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[Pregled razvoj]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
