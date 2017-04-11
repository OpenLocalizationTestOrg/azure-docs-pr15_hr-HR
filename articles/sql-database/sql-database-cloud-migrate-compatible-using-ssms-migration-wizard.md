<properties
   pageTitle="Migracija baze podataka SQL Server u SQL baze podataka pomoću implementacija baze podataka radi Čarobnjak za baze podataka Microsoft Azure | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, Migracija baze podataka, čarobnjak za baze podataka Microsoft Azure"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>Migracija baze podataka SQL Server u SQL baze podataka pomoću implementacija baze podataka radi Čarobnjak za baze podataka Microsoft Azure


> [AZURE.SELECTOR]
- [Čarobnjak za migraciju SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Izvoz u datoteku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Uvezi iz datoteke BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakcijskih replikacije](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Implementacija baze podataka čarobnjaka bazom podataka Microsoft Azure SQL Server Management Studio prenosi bazom [kompatibilne baze podataka SQL Server](sql-database-cloud-migrate.md) izravno u vaš poslužitelj baze podataka SQL Azure.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Korištenje baze podataka uvođenja Čarobnjak za baze podataka Microsoft Azure

> [AZURE.NOTE] Sljedeći koraci pretpostavimo da imate u [dodjeli baze podataka SQL server](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Provjerite imate li najnoviju verziju sustava SQL Server Management Studio. Nove verzije Management Studio ažuriraju mjesečni ostaju sinkronizacija ažuriranja Azure portalu.

    > [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otvorite Management Studio i povezivanje s bazom podataka sustava SQL Server migrirati u programu Explorer objekta.
3. Desnom tipkom miša kliknite bazu podataka u programu Explorer objekata, pokažite na **mogućnost zadaci**i kliknite **Implementacija baza podataka Microsoft Azure SQL bazu podataka...**

    ![Implementacija Azure s izbornika zadataka](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  U čarobnjaku za implementaciju kliknite **Dalje**, a zatim **Poveži** s konfigurirati vezu s poslužiteljem baze podataka SQL.

    ![Implementacija Azure s izbornika zadataka](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. U odjeljku povezivanje s poslužiteljem dijaloški okvir unesite podatke o vezi za povezivanje s poslužiteljem baze podataka SQL.

    ![Implementacija Azure s izbornika zadataka](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Navedite sljedeće podatke o [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) datoteku koju ovaj čarobnjak stvara tijekom postupka migracije:

 - **Novi naziv baze podataka** 
 - **Izdanje sustava Microsoft Azure SQL baze podataka** ([servis sloju](sql-database-service-tiers.md))
 - **Maksimalna veličina baze podataka**
 - **Cilj servisa** (performanse razina)
 - **Privremeni naziv datoteke**  

    ![Izvoz postavki](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Dovršite čarobnjak. Ovisno o veličini i složenosti baze podataka, implementacije može potrajati od nekoliko minuta na mnogo sati. Ako taj čarobnjak otkrije probleme s kompatibilnošću, pogreške prikazuju se na zaslon i Migracija ne može nastaviti. Upute o tome kako riješiti probleme s kompatibilnošću baze podataka, otvorite da biste [riješili probleme s kompatibilnošću baze podataka](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.  Pomoću programa Explorer objekta, povezivanje s migriranim bazu podataka poslužitelja baze podataka SQL Azure.
8.  Pomoću portala za Azure prikaz baze podataka i njezina svojstva.

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
