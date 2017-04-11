<properties
   pageTitle="Rješavanje problema za kompatibilnost baze podataka sustava SQL Server prije migracije s bazom podataka SQL | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, Migracija baze podataka, kompatibilnost, čarobnjak za migraciju SQL Azure"
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

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Korištenje čarobnjaka za migraciju SQL Azure SQL Server riješiti probleme s kompatibilnošću baze podataka prije migracije s bazom podataka SQL Azure

> [AZURE.SELECTOR]
- Koristite [Čarobnjak za migraciju Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Korištenje [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Korištenje [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

U ovom se članku Naučite prepoznavanje i rješavanje problema kompatibilnosti baze podataka za SQL Server pomoću čarobnjaka za migraciju SQL Azure prije migracije s bazom podataka SQL Azure.

## <a name="using-sql-azure-migration-wizard"></a>Pomoću čarobnjaka za migraciju Azure SQL

Pomoću alata za CodePlex [Čarobnjak za SQL Azure migraciju](http://sqlazuremw.codeplex.com/) da biste generirali T SQL skripte iz baze podataka kompatibilan izvora. Ova skripta pa transformacije tako da bi bio kompatibilan s bazom podataka sustava SQL čarobnjak. Zatim povežite s bazom podataka SQL Azure izvršiti skriptu. Ovaj alat analizira i praćenje datoteke da biste odredili problemi s kompatibilnošću. Skripta možete generirao sheme samo ili mogu sadržavati podatke u obliku BCP. Dodatna dokumentacija, uključujući detaljne upute dostupna je na CodePlex pri [Čarobnjak za migraciju SQL Azure](http://sqlazuremw.codeplex.com/).  

 ![Dijagram SAMW migracije](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Svi kompatibilan sheme koji su pronađeni u čarobnjaku za se mogu popraviti pomoću njegov ugrađene transformacije. Nekompatibilni klijentski skriptu koja ne može biti adresirane su prijavljena kao pogreške, s komentarima umetnutog u generirana skripta. Ako se otkriju mnoge pogreške, Visual Studio ili SQL Server Management Studio počnete slijediti i riješite pomoću svaku pogrešku koju ne može biti popravi pomoću čarobnjaka za migraciju SQL Server.

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migriranje kompatibilne baze podataka SQL Server u SQL baze podataka](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
