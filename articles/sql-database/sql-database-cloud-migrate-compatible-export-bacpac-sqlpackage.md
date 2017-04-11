<properties
   pageTitle="Izvoz baze podataka SQL Server BACPAC datoteku pomoću SqlPackage | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, Migracija baze podataka izvezite baze podataka, izvoz datoteke BACPAC, sqlpackage"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Izvoz baze podataka SQL Server BACPAC datoteku pomoću SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

U ovom se članku objašnjava izvoz bazom podataka sustava SQL Server u datoteku [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) korištenjem pomoćnog programa naredbenog retka [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Uslužni isporučuje s najnovijim verzijama sustava [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools za Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)ili izravno iz Microsoftova centra za preuzimanje možete preuzeti najnoviju verziju [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) .

1. Otvorite naredbeni redak i promjena direktorij koji sadrži pomoćnog programa naredbenog retka sqlpackage.exe – uslužni isporučuje se s programom Visual Studio i SQL Server. Pomoću pretraživanja na vašem računalu da biste pronašli put u svom okruženju.
2. Pokrenite sljedeću naredbu sqlpackage.exe sa sljedećim argumentima okruženju sustava:

    "sqlpackage.exe /Action:Export /ssn: /sdn < naziv_poslužitelja >: < database_name > /tf: < target_file >

  	| Argument  | Opis  |
  	|---|---|
  	| < naziv_poslužitelja >  | Naziv poslužitelja izvora  |
  	| < database_name >  | Izvorni naziv baze podataka  |
  	| < target_file >  | Naziv datoteke i mjesto za datoteku BACPAC  |

    ![Izvoz podataka sloja aplikacije na izborniku zadataka](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Uvoz u BACPAC s bazom podataka SQL Azure pomoću SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Uvoz u BACPAC SqlPackage baze podataka Azure SQL](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Uvoz u BACPAC portal Azure za baze podataka SQL Azure](sql-database-import.md)
- [Uvoz u BACPAC PowerShell baze podataka Azure SQL](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
