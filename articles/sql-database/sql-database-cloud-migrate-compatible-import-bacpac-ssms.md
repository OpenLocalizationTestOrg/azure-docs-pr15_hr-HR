<properties
   pageTitle="Migracija baze podataka SQL Server s bazom podataka SQL Azure | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, baza podataka implementacije, Migracija baze podataka, Uvoz baze podataka, izvoz baze podataka, a zatim Čarobnjak za migraciju"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Uvoz iz BACPAC u SQL baze podataka pomoću SSMS

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Portal za Azure](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

U ovom se članku objašnjava Uvezi iz datoteke [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) s bazom podataka SQL pomoću podataka sloju aplikacije čarobnjaka za izvoz u SQL Server Management Studio.

> [AZURE.NOTE] Sljedeći koraci pretpostavlja da ste već dodjeli logičke instancu sustava Azure SQL i imati pri ruci podatke o vezi.

1. Provjerite imate li najnoviju verziju sustava SQL Server Management Studio. Nove verzije Management Studio ažuriraju mjesečni ostaju sinkronizacija ažuriranja Azure portalu.

     > [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Povezivanje s poslužiteljem baze podataka SQL Azure, desnom tipkom miša kliknite mapu **baze podataka** i kliknite **Uvoz podataka sloja aplikacije...**

    ![Stavka izbornika za uvoz podataka sloja aplikacije](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Da biste stvorili bazu podataka u bazi podataka SQL Azure, Uvoz datoteke BACPAC iz lokalni disk ili odaberite račun za Azure prostora za pohranu i spremnika u koji ste prenijeli datoteku BACPAC.

    ![Uvoz postavki](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Prilikom uvoza u BACPAC iz spremišta blobova platforme Azure, pomoću standardnih prostora za pohranu. Nije podržan uvoz u BACPAC od premium prostora za pohranu.

4.  Navedite **novi naziv baze podataka** za bazu podataka na baze podataka SQL Azure, postavite **Izdanje sustava Microsoft Azure SQL baze podataka** (servis sloju), **Maksimalna veličina baze podataka**i **Servisa cilj** (performanse razina).

    ![Postavke baze podataka](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Kliknite **Dalje** , a zatim kliknite **Završi** da biste uvezli datoteku BACPAC u novu bazu podataka na poslužitelju baze podataka SQL Azure.

6. Pomoću programa Explorer objekta, povezivanje s migriranim bazu podataka poslužitelja baze podataka SQL Azure.

6.  Pomoću portala za Azure prikaz baze podataka i njezina svojstva.

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
