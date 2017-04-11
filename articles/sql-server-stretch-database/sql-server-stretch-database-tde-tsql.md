<properties
   pageTitle="Omogućivanje šifriranje prozirne podataka (TDE) za SQL Server rastezanje bazu podataka na Azure TSQL | Microsoft Azure"
   description="Omogućivanje šifriranje prozirne podataka (TDE) za SQL Server rastezanje bazu podataka na Azure TSQL"
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
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Omogućivanje šifriranje prozirne podataka (TDE) za rastezanje baze podataka na Azure (-SQL transakcija)
> [AZURE.SELECTOR]
- [Portal za Azure](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Šifriranje prozirne podataka (TDE) pomaže u zaštiti prijetnju zlonamjerni aktivnosti izvođenjem u stvarnom vremenu šifriranje i dešifriranje baze podataka, pridruženi sigurnosne kopije i datoteke zapisnika transakcije na ostale bez promjene u aplikaciju.

TDE šifrira prostora za pohranu cijelu bazu podataka pomoću ključa simetričnu naziva ključa za šifriranje baze podataka. Ključ za šifriranje baze podataka zaštićen certifikata ugrađene poslužitelja. Potvrda ugrađene poslužitelja nije jedinstvena za svaki Azure poslužitelj. Microsoft automatski zakreće ove potvrde barem svaka 90 dana. Opći opis TDE potražite u članku [Prozirne šifriranje podataka (TDE)].

##<a name="enabling-encryption"></a>Omogućivanje šifriranja

Da biste omogućili TDE za programa Azure bazu podataka koja je spremanje podataka migrirati iz baze podataka s omogućenim rastezanje SQL Server, učinite sljedeće:

1. Povezivanje s *glavnom* bazom podataka na Azure poslužitelj koji hostira baze podataka pomoću prijava koji je administrator ili član uloge **dbmanager** u glavnom bazom podataka
2. Pokrenite sljedeću naredbu da biste šifrirali bazu podataka.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Onemogućivanje šifriranje

Da biste onemogućili TDE za programa Azure bazu podataka koja je spremanje podataka migrirati iz baze podataka s omogućenim rastezanje SQL Server, učinite sljedeće:

1. Povezivanje s *glavnom* bazom podataka korištenjem prijava koji je administrator ili član uloge **dbmanager** u glavnom bazom podataka
2. Pokrenite sljedeću naredbu da biste šifrirali bazu podataka.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Provjera šifriranje

Da biste provjerili status šifriranje Azure bazu podataka koja je spremanje podataka migrirati iz baze podataka s omogućenim rastezanje SQL Server, učinite sljedeće:

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


<!--Anchors-->
[Šifriranje prozirne podataka (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
