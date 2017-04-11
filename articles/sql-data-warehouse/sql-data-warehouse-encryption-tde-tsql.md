<properties
   pageTitle="Šifriranje prozirne podataka u skladištu SQL podataka (T SQL) | Microsoft Azure"
   description="Šifriranje prozirne podataka (TDE) u SQL Data Warehouse (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Početak rada s prozirnim podataka šifriranja (TDE)


> [AZURE.SELECTOR]
- [Pregled sigurnosti](sql-data-warehouse-overview-manage-security.md)
- [Provjera autentičnosti](sql-data-warehouse-authentication.md)
- [Šifriranje (Portal)](sql-data-warehouse-encryption-tde.md)
- [Šifriranje (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Potrebne ovlasti

Da biste omogućili prozirne šifriranje podataka (TDE), morate biti administrator ili član uloge dbmanager.

## <a name="enabling-encryption"></a>Omogućivanje šifriranja

Slijedite ove korake da biste omogućili TDE za SQL Data Warehouse:

1. Povezivanje s *glavnom* bazom podataka na poslužitelj koji hostira baze podataka pomoću prijava koji je administrator ili član uloge **dbmanager** u glavnom bazom podataka
2. Pokrenite sljedeću naredbu da biste šifrirali bazu podataka.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Onemogućivanje šifriranje

Slijedite ove korake da biste onemogućili TDE za SQL Data Warehouse:

1. Povezivanje s *glavnom* bazom podataka korištenjem prijava koji je administrator ili član uloge **dbmanager** u glavnom bazom podataka
2. Pokrenite sljedeću naredbu da biste šifrirali bazu podataka.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Pauzirano SQL Data Warehouse mora se nastaviti prije promjene postavki TDE.

## <a name="verifying-encryption"></a>Provjera šifriranje

Da biste provjerili stanje šifriranja za SQL Data Warehouse, slijedite ove korake:

1. Povezivanje s *osnovne* ili instanca baze podataka pomoću prijava koji je administrator ili član uloge **dbmanager** u glavnom bazom podataka
2. Pokrenite sljedeću naredbu da biste šifrirali bazu podataka.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Rezultat ```1``` označava šifriranu bazu podataka, ```0``` označava koje nisu šifrirane baze podataka.

## <a name="encryption-dmvs"></a>Šifriranje DMVs  

- [sys.Databases][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
