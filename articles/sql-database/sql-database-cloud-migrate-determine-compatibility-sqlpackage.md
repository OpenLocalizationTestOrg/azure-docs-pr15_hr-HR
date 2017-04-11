<properties
   pageTitle="Određivanje kompatibilnosti SQL baze podataka pomoću SqlPackage.exe | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, web-mjesto Migracija baze podataka, u okvir za kompatibilnost SQL baze podataka, SqlPackage"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>Određivanje kompatibilnosti SQL baze podataka pomoću SqlPackage.exe

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Savjetnik za nadogradnju](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

U ovom se članku saznat ćete da bi se utvrdilo kompatibilni za migraciju u SQL baze podataka pomoću naredbenog retka uslužni [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) baze podataka SQL Server.

## <a name="using-sqlpackageexe"></a>Korištenje SqlPackage.exe

1. Otvorite naredbeni redak i promijenite direktorij koji sadrži najnovija verzija sustava sqlpackage.exe. Uslužni isporučuje s najnovijim verzijama sustava [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools za Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)ili izravno iz Microsoftova centra za preuzimanje možete preuzeti najnoviju verziju [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) .
2. Pokrenite sljedeću naredbu SqlPackage sa sljedećim argumentima okruženju sustava:

    ' sqlpackage.exe /Action:Export /ssn: /sdn < naziv_poslužitelja >: < database_name > /tf: /p:TableData < target_file > = < schema_name.table_name >>< output_file > 2 > & 1'

  	| Argument  | Opis  |
  	|---|---|
  	| < naziv_poslužitelja >  | Naziv poslužitelja izvora  |
  	| < database_name >  | Izvorni naziv baze podataka  |
  	| < target_file >  | Naziv datoteke i mjesto za datoteku BACPAC  |
  	| < schema_name.table_name >  | Tablica za koju podaci su Izlaz u ciljnu datoteku  |
  	| < output_file >  | Naziv datoteke i mjesto za izlazna datoteka s pogreškama, ako postoje  |

    Razlog za /p:TableName argument je da samo želimo provjeriti kompatibilnost baze podataka za izvoz u V12 za baze podataka SQL Azure umjesto izvoz podataka iz svih tablica. Nažalost, izvoz argument sqlpackage.exe ne podržava izdvajanje nula tablice. Morate navesti najmanje jednu tablicu, kao što su malu tablicu. < Output_file > sadrži izvješća sve pogreške. Na "> 2 > & 1" niz cijevi standardni izlazni i standardnu pogrešku koja je rezultat izvođenja naredbe za navedeni Izlazna datoteka.

    ![Izvoz podataka sloja aplikacije na izborniku zadataka](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Otvorite Izlazna datoteka, a zatim pregledajte pogreške kompatibilnosti ako ih ima. 

    ![Izvoz podataka sloja aplikacije na izborniku zadataka](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Rješavanje problema s kompatibilnošću Migracija baze podataka](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migriranje kompatibilne baze podataka SQL Server u SQL baze podataka](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
