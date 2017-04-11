<properties 
    pageTitle="Stvaranje ili premještanje baze podataka Azure SQL elastic grupe aplikacija pomoću T SQL | Microsoft Azure" 
    description="T-SQL možete koristiti za stvaranje baze podataka Azure SQL u elastic grupe aplikacija. Ili koristite T SQL da biste premjestili na datbase i grupe." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Nadzor i upravljanje elastic baze podataka grupe aplikacija s Transact-SQL  

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Pomoću naredbe za [Stvaranje baze podataka (baza podataka SQL Azure)](https://msdn.microsoft.com/library/dn268335.aspx) i [Mijenjati Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) možete stvarati i premještanje baze podataka u i Odjava iz njega elastic grupe. Elastic skup mora postojati prije korištenja ove naredbe. Te naredbe utječe samo baze podataka. Stvaranje nove grupe, a postavka svojstva skup (primjerice min i max eDTUs) nije moguće promijeniti T SQL naredbe.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Stvaranje nove baze podataka u elastic grupe aplikacija
Pomoću naredbe za stvaranje baze podataka s mogućnošću SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Sve baze podataka u elastic skup nasljeđuju sloj servisa elastic skup (Basic, Standardno, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Prebacivanje baze podataka elastic grupe
Korištenje naredbe ALTER DATABASE s na IZMIJENI i postavljanje SERVISA\_CILJNA mogućnost kao ELASTIC\_grupe APLIKACIJA; Postavite naziv na naziv ciljne grupe.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Premještanje baze podataka u elastic grupe aplikacija 
Korištenje naredbe ALTER DATABASE s na IZMIJENI i postavljanje SERVISA\_CILJNA mogućnost kao ELASTIC_POOL; Postavite naziv na naziv ciljne grupe.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Premještanje baze podataka iz elastic grupe aplikacija
Pomoću naredbe ALTER DATABASE i postavite na SERVICE_OBJECTIVE na jednu od razine performansi (S0, S1 itd.).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Baza podataka popisa u elastic grupe aplikacija
Korištenje na [sys.database\_servisa \_prikaz ciljevi](https://msdn.microsoft.com/library/mt712619) popis sve baze podataka u elastic grupe aplikacija. Prijavite se na matricu baze podataka za upit u prikazu.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Dohvaćanje podataka o korištenju resursa za zajedničko područje

Korištenje na [sys.elastic\_skup \_resursa \_stat prikaz](https://msdn.microsoft.com/library/mt280062.aspx) da biste pregledali statistiku korištenja resursa elastic grupe aplikacija na poslužitelju, logičke. Prijavite se na matricu baze podataka za upit u prikazu.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Početak korištenja resursa za elastic bazu podataka

Korištenje na [sys.dm\_ db\_ resursa\_stat prikaz](https://msdn.microsoft.com/library/dn800981.aspx) ili [sys.resource \_stat prikaz](https://msdn.microsoft.com/library/dn269979.aspx) da biste pregledali statistika korištenja resursa baze podataka u elastic grupe aplikacija. Ovaj postupak je sličan ispitivanje korištenje resursa za bilo koju jednu bazu podataka.

## <a name="next-steps"></a>Daljnji koraci

Nakon stvaranja grupe aplikacija programa elastic bazu podataka, možete upravljati elastic baze podataka u stvaranjem elastic zadatke. Elastic poslove olakšati pokretanje skripti T SQL na temelju bilo koji broj baza podataka u. Dodatne informacije potražite u članku [Pregled zadataka Elastic baze podataka](sql-database-elastic-jobs-overview.md). 

U odjeljku [Skaliranje odgovor s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md): pomoću Alati baze podataka za elastic mjerilo Izlaz, premještanje podataka, upit ili stvorite transakcije.
