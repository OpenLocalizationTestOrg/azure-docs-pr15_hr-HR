<properties
   pageTitle="Rješavanje problema kompatibilnosti baze podataka za SQL Server pomoću SQL Server upravljanje Studio prije migracije s bazom podataka SQL | Microsoft Azure"
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

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Rješavanje problema kompatibilnosti baze podataka za SQL Server pomoću SQL Server Management Studio prije migracije u SQL baze podataka

> [AZURE.SELECTOR]
- Koristite [Čarobnjak za migraciju Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Korištenje [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Korištenje [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Napredni korisnici možete ispraviti SQL Server baze podataka probleme s kompatibilnošću pomoću SQL Server Management Studio prije migracije s bazom podataka SQL Azure.


> [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>Pomoću SQL Server Management Studio

Da biste riješili probleme s kompatibilnošću pomoću različite Transact-SQL naredbe, kao što su **Izmjena baze podataka**pomoću SQL Server Management Studio. Ta metoda prvenstveno za napredne korisnike koji su upoznati radi Transact-SQL na bazu podataka uživo. U suprotnom, preporučuje se da koristite SSDT. 



## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migriranje kompatibilne baze podataka SQL Server u SQL baze podataka](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
