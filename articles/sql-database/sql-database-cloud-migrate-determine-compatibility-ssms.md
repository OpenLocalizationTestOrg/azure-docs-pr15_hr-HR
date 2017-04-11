<properties
   pageTitle="Da biste odredili baze podataka SQL kompatibilnosti prije migracije s bazom podataka SQL Azure pomoću SQL Server Management Studio | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, web-mjesto Migracija baze podataka, u okvir za kompatibilnost SQL baze podataka, podataka sloju aplikacije čarobnjaka za izvoz"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>Da biste odredili baze podataka SQL kompatibilnosti prije migracije s bazom podataka SQL Azure pomoću SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Savjetnik za nadogradnju](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
U ovom članku saznat ćete dobiti ako bazu podataka sustava SQL Server kompatibilan migriranje u SQL baze podataka pomoću podataka sloju aplikacije čarobnjaka za izvoz u SQL Server Management Studio.

## <a name="using-sql-server-management-studio"></a>Pomoću SQL Server Management Studio

1. Provjerite imate li najnoviju verziju sustava SQL Server Management Studio. Nove verzije Management Studio ažuriraju mjesečni ostaju sinkronizacija ažuriranja Azure portalu.

     > [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Otvorite Management Studio i povezati izvorišne baze podataka u programu Explorer objekta.
3. Desnom tipkom miša kliknite izvorišne baze podataka u programu Explorer objekata, pokažite na **mogućnost zadaci**i kliknite **Izvoz podataka sloja aplikacije...**

    ![Izvoz podataka sloja aplikacije na izborniku zadataka](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. U čarobnjaku za izvoz kliknite **Dalje**, a zatim na kartici **Postavke** konfigurirajte izvoz da biste spremili datoteku BACPAC na mjesto na lokalnom disku ili blobova platforme Azure. BACPAC spremanja datoteke ako imate bez problema s kompatibilnošću baze podataka. Ako postoje problemi s kompatibilnošću, oni se prikazuju na konzoli sustava.

    ![Izvoz postavki](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Da biste preskočili izvoza podataka, kliknite **karticu Napredno** , a zatim poništite potvrdni okvir **Odaberi sve** . Naš cilj sada je samo za provjeru kompatibilnosti.

    ![Izvoz postavki](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Kliknite **Dalje** , a zatim kliknite **Završi**. Probleme s kompatibilnošću baze podataka, ako postoje, kada se prikazati čarobnjaka Provjeri valjanost u shemi.

    ![Izvoz postavki](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Ako se pojavi bez pogrešaka, kompatibilan bazu podataka i spremni ste za migraciju. Ako imate pogreške, morate ih riješili. Da biste vidjeli pogreške, kliknite **pogreške** za **Validating shemu**. 
    ![Izvoz postavki](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Ako na *. Uspješno je generiran BACPAC datoteke, a zatim je kompatibilan s bazom podataka SQL baze podataka i spremni ste migrirati.

## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Rješavanje problema s kompatibilnošću Migracija baze podataka](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migriranje kompatibilne baze podataka SQL Server u SQL baze podataka](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Dodatni resursi

- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
