
<properties
   pageTitle="Izvoz baze podataka SQL Server BACPAC datoteku pomoću SQL Server Management Studio | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, Migracija baze podataka izvezite baze podataka, izvoz datoteke BACPAC, čarobnjak za izvoz podataka aplikacije u sloju"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Izvoz baze podataka SQL Server BACPAC datoteku pomoću SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
U ovom se članku objašnjava izvoz baze podataka SQL Server [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) datoteku pomoću podataka sloju aplikacije čarobnjaka za izvoz u SQL Server Management Studio. 

1. Provjerite imate li najnoviju verziju sustava SQL Server Management Studio. Nove verzije Management Studio ažuriraju mjesečni ostaju sinkronizacija ažuriranja Azure portalu.

     > [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otvorite Management Studio i povezati izvorišne baze podataka u programu Explorer objekta.

    ![Izvoz podataka sloja aplikacije na izborniku zadataka](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Desnom tipkom miša kliknite izvorišne baze podataka u programu Explorer objekata, pokažite na **mogućnost zadaci**i kliknite **Izvoz podataka sloja aplikacije...**

    ![Izvoz podataka sloja aplikacije na izborniku zadataka](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. U čarobnjaku za izvoz konfigurirati izvoz da biste spremili datoteku BACPAC da biste na mjesto na lokalnom disku ili da biste je Azure bloba. Izvezeni BACPAC uvijek sadrži sheme cijele baze podataka i prema zadanim postavkama, podaci iz svih tablica. Upute za izdvajanje podataka iz neke ili sve tablice, koristite karticu Napredno. Na primjer, možda, odaberite da biste izvezli samo podatke za referencu tablice, a ne iz svih tablica.

***Važno*** Kada se izvoze u BACPAC za spremište blobova platforme Azure, pomoću standardnih prostora za pohranu. Nije podržan uvoz u BACPAC od premium prostora za pohranu.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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
