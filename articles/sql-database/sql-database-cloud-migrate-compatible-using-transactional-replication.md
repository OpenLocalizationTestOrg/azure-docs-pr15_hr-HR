<properties
   pageTitle="Migriranje u SQL baze podataka pomoću transakcijskih replikacije | Microsoft Azure"
   description="Microsoft Azure SQL baze podataka, Migracija baze podataka, Uvoz baze podataka, transakcijskih replikacije"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Migracija baze podataka SQL Server s bazom podataka SQL Azure pomoću transakcijskih replikacije

> [AZURE.SELECTOR]
- [Čarobnjak za migraciju SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Izvoz u datoteku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Uvezi iz datoteke BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakcijskih replikacije](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

U ovom se članku saznat ćete će se migrirati kompatibilne baze podataka SQL Server s bazom podataka SQL Azure s minimalnim nedostupnost pomoću transakcijskih replikacije sustava SQL Server.

## <a name="understanding-the-transactional-replication-architecture"></a>Objašnjenje arhitektura transakcijskih replikacije

Kada Nipošto ne smijete da biste uklonili bazom podataka sustava SQL Server s radnom tijekom migracije se pojavljuje, možete koristiti SQL Server transakcijskih replikacije kao vašeg rješenja za migraciju. Da biste koristili rješenja, konfiguriranje baze podataka SQL Azure kao pretplatnika na lokalni instancu sustava SQL Server koju želite migrirati. Na lokalni transakcijskih replikacije distributer sinkronizira podatke iz lokalne baze podataka koja će se sinkronizirati (publisher) dok nove transakcije i dalje pojavljuju. 

Da biste migrirali podskup lokalne baze podataka možete koristiti i transakcijskih replikacije. Publikacija koje je replicirati s bazom podataka SQL Azure može biti ograničeno na podskup tablica u bazi podataka koja se replicirati. Za svaku tablicu koja se replicirati možete ograničiti podatke koje želite podskup redaka i/ili podskup stupce.

S transakcijskih replikacijom sve promjene podataka ili sheme prikazuju se u bazi podataka SQL Azure. Nakon dovršetka sinkronizacije i spremni ste za migriranje, promijenite niz za povezivanje aplikacija na bazu podataka SQL Azure. Kada transakcijskih replikacije drains promjene ulijevo na lokalne baze podataka i sve aplikacije pokažite na Azure DB, možete deinstalirati transakcijskih replikacije. Baze podataka SQL Azure sada je operativnom sustavu.

 ![Dijagram SeedCloudTR](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Preduvjeti za transakcijskih replikacije

Transakcijskih replikacije je tehnologija ugrađene i integrirane SQL Server od SQL Server 6.5. Je starijih i dokazana tehnologija Većina DBAs znati koji imaju iskustvo. U programu [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)sada je moguće da biste konfigurirali baze podataka SQL Azure kao [transakcijskih replikacije pretplatnika](https://msdn.microsoft.com/library/mt589530.aspx) publikaciji lokalnog. Na način na koji se postavljate ga iz Management Studio je isti kao da je argument postavljanje pretplatnik transakcijskih replikacije na lokalni poslužitelj. Podrška za ovaj scenarij je podržano kada izdavača i distributer barem jedan od sljedećih verzija sustava SQL Server:

 - SQL Server 2016 i noviji 
 - SQL Server 2014 SP1 CU3 i noviji
 - SQL Server 2014 RTM CU10 i noviji
 - SQL Server 2012 SP2 CU8 i noviji
 - SQL Server 2012 SP3 i noviji


> [AZURE.IMPORTANT] Pomoću najnovije verzije sustava SQL Server Management Studio ostati sinkronizirani s ažuriranja za Microsoft Azure i SQL baze podataka. Starijih verzija sustava SQL Server Management Studio ne može postaviti SQL bazu podataka kao pretplatnika. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Daljnji koraci

- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Dodatni resursi

- [Transakcijskih replikacije](https://msdn.microsoft.com/library/mt589530.aspx)
- [V12 baze podataka SQL](sql-database-v12-whats-new.md)
- [SQL transakcija djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
