<properties
   pageTitle="Uvoz iz BACPAC datoteku pomoću SqlPackage u SQL baze podataka"
   description="Microsoft Azure SQL baze podataka, Migracija baze podataka, Uvoz baze podataka, Uvoz datoteke BACPAC, sqlpackage"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>Uvoz iz BACPAC datoteku pomoću SqlPackage u SQL baze podataka

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Portal za Azure](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

U ovom se članku prikazuje kako uvesti u SQL baze podataka iz datoteke [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) korištenjem pomoćnog programa naredbenog retka [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Uslužni isporučuje s najnovijim verzijama sustava [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools za Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)ili izravno iz Microsoftova centra za preuzimanje možete preuzeti najnoviju verziju [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) .


> [AZURE.NOTE] Sljedeći koraci pretpostavlja da ste već dodjeli baze podataka SQL server, podatke o vezi imati pri ruci i ste provjerili kompatibilan izvorišne baze podataka.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Uvoz iz datoteke BACPAC u baze podataka SQL Azure pomoću SqlPackage

Poduzmite sljedeće korake da biste koristili pomoćnog programa naredbenog retka [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) da biste uvezli kompatibilne baze podataka SQL Server (ili baze podataka Azure SQL) iz datoteke BACPAC.

> [AZURE.NOTE] Sljedeći koraci pretpostavlja da ste već dodjeli s poslužiteljem baze podataka SQL Azure i imati pri ruci podatke o vezi.

1. Otvorite naredbeni redak i promjena direktorij koji sadrži pomoćnog programa naredbenog retka sqlpackage.exe – uslužni isporučuje se s programom Visual Studio i SQL Server.
2. Pokrenite sljedeću naredbu sqlpackage.exe sa sljedećim argumentima okruženju sustava:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argument  | Opis  |
  	|---|---|
  	| < naziv_poslužitelja >  | Naziv poslužitelja cilj  |
  	| < database_name >  | Naziv ciljne baze podataka  |
  	| < korisničko_ime >  | korisničko ime na poslužitelju ciljni |
  	| < lozinku >  | korisničke lozinke  |
  	| < source_file >  | Naziv datoteke i mjesto za datoteku BACPAC koja se uvozi  |

    ![Izvoz podataka sloja aplikacije na izborniku zadataka](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
